---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
title: Implementazione della concorrenza ottimistica (c#) | Microsoft Docs
author: rick-anderson
description: Per un'applicazione web che consente più agli utenti di modificare i dati, sussiste il rischio che due utenti potrebbero essere modificando gli stessi dati nello stesso momento. In questo tutori...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 56e15b33-93b8-43ad-8e19-44c6647ea05c
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
msc.type: authoredcontent
ms.openlocfilehash: 9013eab1082c27aac3b561f9507842edc77c7da3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824448"
---
<a name="implementing-optimistic-concurrency-c"></a>Implementazione della concorrenza ottimistica (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_CS.exe) o [Scarica il PDF](implementing-optimistic-concurrency-cs/_static/datatutorial21cs1.pdf)

> Per un'applicazione web che consente più agli utenti di modificare i dati, sussiste il rischio che due utenti potrebbero essere modificando gli stessi dati nello stesso momento. In questa esercitazione è implementato il controllo della concorrenza ottimistica per gestire tale rischio.


## <a name="introduction"></a>Introduzione

Per le applicazioni web che consentono solo agli utenti di visualizzare dati o per quelle che includono solo un singolo utente può modificare i dati, non vi è alcun rischio di due utenti simultanei sovrascrivere accidentalmente modifiche di altri. Per le applicazioni web che consentono a più utenti di aggiornare o eliminare dati, tuttavia, sussiste il potenziale per apportare modifiche di un utente a essere in conflitto con un altro utente simultanee. Senza criteri concorrenza posto, quando due utenti modificano contemporaneamente un singolo record, l'utente che esegue il commit delle modifiche ultimo sostituiranno le modifiche apportate dalla prima.

Ad esempio, si supponga che due utenti, Jisun e Sam, sono stati entrambi che visitano una pagina nell'applicazione che consente di aggiornare ed eliminare i prodotti tramite un controllo GridView. Entrambi fare clic sul pulsante Modifica in GridView intorno alla stessa ora. Jisun diventa il nome del prodotto "Chai tè" e fa clic sul pulsante di aggiornamento. Il risultato è un `UPDATE` istruzione che viene inviato al database, che imposta *tutto* dei campi aggiornabili del prodotto (anche se Jisun aggiornate solo a un campo, `ProductName`). In questo momento, il database contiene i valori "Chai tè", la categoria Beverages, il fornitore originali liquide, e così via per questo prodotto specifico. Tuttavia, il controllo GridView nella schermata di Sam Mostra comunque il nome del prodotto nella riga GridView modificabile come "Chai". Pochi secondi dopo che sono state salvate, le modifiche del Jisun Sam gli aggiornamenti della categoria Condiments e fa clic su Aggiorna. Il risultato è un' `UPDATE` istruzione inviata al database che consente di impostare il nome del prodotto "Chai," il `CategoryID` il corrispondente ID della categoria Beverages e così via. Le modifiche del Jisun al nome del prodotto sono stati sovrascritti. Figura 1 illustra graficamente questa serie di eventi.


[![Quando due utenti aggiornano contemporaneamente un Record sono s potenziale per un utente s cambia aspetto per sovrascrivere l'altri spazi dei nomi](implementing-optimistic-concurrency-cs/_static/image2.png)](implementing-optimistic-concurrency-cs/_static/image1.png)

**Figura 1**: quando due utenti simultaneamente aggiornare un Record sono s potenziale per un utente s le modifiche al sovrascrittura di altri spazi dei nomi ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image3.png))


Analogamente, quando due utenti sono utenti in visita una pagina e un utente potrebbe essere durante l'aggiornamento di un record quando viene eliminata da un altro utente. In alternativa, tra quando un utente carica una pagina e facendo clic sul pulsante Elimina, un altro utente potrebbe avere modificato il contenuto di tale record.

Sono disponibili tre [controllo della concorrenza](http://en.wikipedia.org/wiki/Concurrency_control) strategie disponibili:

- **Non eseguire alcuna operazione** -se lo stesso record Modifica utenti simultanei, consentire l'ultimo commit vincere (comportamento predefinito)
- [**La concorrenza ottimistica** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -presuppongono che possono essere anche la concorrenza è in conflitto ogni tanto in tanto la maggior parte dei casi non si verificheranno tali conflitti; pertanto, se si verificano un conflitto, è sufficiente informare l'utente che le modifiche non può essere salvato perché un altro utente ha modificato gli stessi dati
- **Concorrenza pessimistica** -presuppongono che i conflitti di concorrenza siano comuni che gli utenti non saranno in grado di tollerare being indicato non sono state salvate le modifiche a causa di attività simultanee a un altro utente; pertanto, quando un utente avvia l'aggiornamento di un record, bloccare. questa funzionalità , impedendo in tal modo di qualsiasi altro utente di modificare o eliminare record in questione fino a quando l'utente conferma le modifiche

Tutte le esercitazioni finora hanno usato la strategia di risoluzione di concorrenza predefinita: vale a dire, abbiamo facciamo l'ultima scrittura win. In questa esercitazione verrà esaminato come implementare il controllo della concorrenza ottimistica.

> [!NOTE]
> Non esamina alcuni esempi di concorrenza pessimistica in questa serie di esercitazioni. Concorrenza pessimistica è raramente utilizzata perché ad esempio blocchi, se non rilasciata non correttamente, può impedire ad altri utenti di aggiornare i dati. Ad esempio, se un utente blocca un record per la modifica e quindi lascia per il giorno prima averlo sbloccato, nessun altro utente sarà in grado di aggiornare il record finché l'utente originale restituisce e completa il suo aggiornamento. Pertanto, nelle situazioni in cui viene utilizzata la concorrenza pessimistica, è in genere un timeout che, se raggiunto, Annulla il blocco. Siti Web vendito di ticket, che blocca una posizione particolare posti a sedere per breve periodo di tempo mentre l'utente ha completato il processo dell'ordine, è un esempio di controllo della concorrenza pessimistica.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Passaggio 1: Esaminare come la concorrenza ottimistica viene implementata

Controllo della concorrenza ottimistica funzionamento verificando che il record da aggiornare o eliminare dispone degli stessi valori si blocca durante l'aggiornamento o eliminazione di processo avviato. Ad esempio, quando si fa clic sul pulsante Modifica in un controllo GridView modificabile, del record dal database di lettura e visualizzazione dei valori nelle caselle di testo e altri controlli di Web. Questi valori originali vengono salvati per il controllo GridView. In un secondo momento, dopo che l'utente effettua le proprie modifiche e fa clic sul pulsante di aggiornamento, i valori originali e i nuovi valori vengono inviati al livello della logica di Business e quindi fino a livello di accesso ai dati. Il livello di accesso ai dati deve eseguire un'istruzione SQL che aggiornerà il record solo se i valori originali che l'utente ha iniziato la modifica sono identici a quelli ancora nel database. Figura 2 viene illustrata questa sequenza di eventi.


[![Per Update o Delete abbia esito positivo, i valori originali devono essere uguali per i valori del Database corrente](implementing-optimistic-concurrency-cs/_static/image5.png)](implementing-optimistic-concurrency-cs/_static/image4.png)

**Figura 2**: per l'aggiornamento o eliminazione a hanno esito positivo, l'originale valori deve essere uguale per i valori del Database corrente ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image6.png))


Esistono vari approcci all'implementazione della concorrenza ottimistica (vedere [Peter A. Bromberg](http://peterbromberg.net/)del [per la logica di aggiornamento di concorrenza di Optmistic](http://www.eggheadcafe.com/articles/20050719.asp) per un rapido sguardo una serie di opzioni). Il set di dati tipizzato ADO.NET fornisce un'implementazione che può essere configurata con solo il segno di graduazione di una casella di controllo. L'abilitazione della concorrenza ottimistica per un oggetto TableAdapter del dataset tipizzato aumenta dell'oggetto TableAdapter `UPDATE` e `DELETE` istruzioni per includere un confronto di tutti i valori originali di `WHERE` clausola. Nell'esempio `UPDATE` istruzione, ad esempio, aggiorna il nome e il prezzo di un prodotto solo se sono uguali ai valori originariamente recuperati quando si aggiorna il record in GridView i valori del database corrente. Il `@ProductName` e `@UnitPrice` i parametri contengono i nuovi valori immessi dall'utente, mentre `@original_ProductName` e `@original_UnitPrice` contengono i valori che originariamente venivano caricati in GridView quando è stato fatto clic sul pulsante di modifica:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample1.sql)]

> [!NOTE]
> Ciò `UPDATE` istruzione è stata semplificata per migliorare la leggibilità. In pratica, il `UnitPrice` archivia il `WHERE` clausola sarebbe più complessa poiché `UnitPrice` può contenere `NULL` s e durante la verifica del `NULL = NULL` restituisce sempre False (è invece necessario usare `IS NULL`).


Oltre a usare un diverso sottostante `UPDATE` metodi diretti di istruzione, la configurazione di un oggetto TableAdapter utilizzare ottimistica concorrenza anche di modificare la firma del relativo database. Si ricorderà da questa prima esercitazione [ *creazione di un livello di accesso ai dati*](../introduction/creating-a-data-access-layer-cs.md), che i metodi diretti DB erano quelli che accetta un elenco di scalare i valori come parametri di input (invece di un DataRow fortemente tipizzata o Istanza DataTable). Quando si usa la concorrenza ottimistica, il database diretto `Update()` e `Delete()` metodi includono parametri di input per nonché i valori originali. Inoltre, il codice nel livello BLL per l'uso di batch di aggiornamento criterio (il `Update()` overload dei metodi che accettano DataRow e DataTable anziché a valori scalari) deve essere modificata anche.

Invece di estendere gli esistente TableAdapter DAL per usare la concorrenza ottimistica (che modifica il livello BLL per contenere necessiterebbe), è possibile invece creare un nuovo DataSet tipizzato denominato `NorthwindOptimisticConcurrency`, a cui si aggiungerà un `Products` TableAdapter che Usa la concorrenza ottimistica. In seguito, si creerà un `ProductsOptimisticConcurrencyBLL` classe Business Logic Layer contenente le modifiche appropriate per supportare la concorrenza ottimistica DAL. Una volta poste questo presupposti, saremo pronti per creare la pagina ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Passaggio 2: Creazione di un livello di accesso ai dati che supporta la concorrenza ottimistica

Per creare un nuovo set di dati tipizzati, fare clic sui `DAL` cartella all'interno di `App_Code` cartella e aggiungere un nuovo set di dati denominato `NorthwindOptimisticConcurrency`. Come abbiamo visto nella prima esercitazione, tale operazione così aggiungerà un nuovo TableAdapter al set di dati tipizzati, che vengono avviate automaticamente la configurazione guidata TableAdapter. Nella prima schermata, abbiamo stiamo richiesto di specificare il database per connettersi a: connettersi al database Northwind stesso usando il `NORTHWNDConnectionString` impostazione da `Web.config`.


[![Connettersi al Database Northwind stesso](implementing-optimistic-concurrency-cs/_static/image8.png)](implementing-optimistic-concurrency-cs/_static/image7.png)

**Figura 3**: connettersi al Northwind Database stesso ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image9.png))


Successivamente, si verrà chiesto di specificare come eseguire query sui dati: tramite un'istruzione SQL ad hoc, una nuova stored procedure o una stored procedure di un oggetto esistente. Dato che abbiamo utilizzato le query SQL ad hoc nel nostro DAL originale, usare questa opzione qui.


[![Specificare i dati da recuperare tramite un'istruzione SQL Ad Hoc](implementing-optimistic-concurrency-cs/_static/image11.png)](implementing-optimistic-concurrency-cs/_static/image10.png)

**Figura 4**: specificare i dati da recuperare tramite un'istruzione SQL Ad Hoc ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image12.png))


Nella schermata seguente, immettere la query SQL da usare per recuperare le informazioni sul prodotto. È possibile usare la query SQL stesso esatta usata per il `Products` TableAdapter dal nostro DAL originale, che restituisce tutti i `Product` colonne insieme ai nomi di categoria e fornitore del prodotto:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample2.sql)]


[![Usare la stessa Query SQL dal TableAdapter prodotti in DAL originale](implementing-optimistic-concurrency-cs/_static/image14.png)](implementing-optimistic-concurrency-cs/_static/image13.png)

**Figura 5**: utilizzare la stessa Query SQL dal `Products` TableAdapter in DAL originale ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image15.png))


Prima di passare nella successiva schermata, fare clic sul pulsante Opzioni avanzate. Per questo controllo della concorrenza ottimistica utilizzano TableAdapter, è sufficiente selezionare la casella di controllo "Usa concorrenza ottimistica".


[![Abilitare il controllo della concorrenza ottimistica per verifica la &quot;usare la concorrenza ottimistica&quot; casella di controllo](implementing-optimistic-concurrency-cs/_static/image17.png)](implementing-optimistic-concurrency-cs/_static/image16.png)

**Figura 6**: abilitare il controllo della concorrenza ottimistica selezionando la casella di controllo "Usa concorrenza ottimistica" ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image18.png))


Infine, indicare che l'oggetto TableAdapter deve usare i modelli di accesso ai dati che Riempi un DataTable e restituiscono un DataTable; indicare anche che i metodi diretti DB devono essere creati. Modificare il nome del metodo per la restituzione di un modello di oggetto DataTable da GetData a GetProducts, in modo da rispecchiare le convenzioni di denominazione viene usata nel nostro DAL originale.


[![Disporre del TableAdapter utilizzare tutti i modelli di accesso ai dati](implementing-optimistic-concurrency-cs/_static/image20.png)](implementing-optimistic-concurrency-cs/_static/image19.png)

**Figura 7**: sono il TableAdapter utilizzare tutti i dati modelli di accesso ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image21.png))


Dopo aver completato la procedura guidata, la finestra di progettazione set di dati includerà oggetto fortemente tipizzato `Products` DataTable e TableAdapter. Si consiglia di rinominare l'elemento DataTable dal `Products` a `ProductsOptimisticConcurrency`, che è possibile effettuare facendo clic sulla barra del titolo della DataTable e scegliendo Rinomina dal menu di scelta rapida.


[![Un oggetto DataTable e TableAdapter sono stati aggiunti al set di dati tipizzati](implementing-optimistic-concurrency-cs/_static/image23.png)](implementing-optimistic-concurrency-cs/_static/image22.png)

**Figura 8**: oggetto DataTable e TableAdapter sono state aggiunte al set di dati tipizzato ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image24.png))


Per visualizzare le differenze tra il `UPDATE` e `DELETE` esegue una query tra il `ProductsOptimisticConcurrency` TableAdapter (che usa la concorrenza ottimistica) e TableAdapter Products (che non), fare clic sull'oggetto TableAdapter e passare alla finestra Proprietà. Nel `DeleteCommand` e `UpdateCommand` proprietà `CommandText` sottoproprietà è possibile visualizzare la sintassi SQL effettiva che viene inviata al database quando DAL update o delete relativi metodi vengono richiamati. Per il `ProductsOptimisticConcurrency` TableAdapter di `DELETE` istruzione usata è:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample3.sql)]

Mentre il `DELETE` istruzione per i TableAdapter del prodotto nel nostro DAL originale è molto più semplice:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample4.sql)]

Come può notare, il `WHERE` clausola nel `DELETE` istruzione per l'oggetto TableAdapter che usa la concorrenza ottimistica include un confronto tra ogni il `Product` tabella esistente del valori delle colonne e i valori originali al momento il GridView (o DetailsView o FormView) dell'ultimo popolamento. Poiché tutti i campi diverso da `ProductID`, `ProductName`, e `Discontinued` può avere `NULL` valori, i parametri aggiuntivi e i controlli sono inclusi per confrontare correttamente `NULL` i valori di `WHERE` clausola.

Che non verranno aggiunti qualsiasi DataTable aggiuntive per il set di dati abilitate per la concorrenza ottimistica per questa esercitazione, come la pagina ASP.NET fornirà solo l'aggiornamento ed eliminazione di informazioni sul prodotto. Tuttavia, è comunque necessario aggiungere il `GetProductByProductID(productID)` metodo di `ProductsOptimisticConcurrency` TableAdapter.

A tale scopo, fare doppio clic sulla barra del titolo dell'oggetto TableAdapter (in alto a destra l'area di `Fill` e `GetProducts` i nomi dei metodi) e scegliere Aggiungi Query dal menu di scelta rapida. Verrà avviata la configurazione guidata Query TableAdapter. Come con la configurazione iniziale del nostro TableAdapter, scegliere di creare il `GetProductByProductID(productID)` metodo usando un'istruzione SQL ad hoc (vedere la figura 4). Poiché il `GetProductByProductID(productID)` metodo restituisce informazioni su un particolare prodotto, indicare che questa query è un `SELECT` tipo che restituisce righe di query.


[![Contrassegnare il tipo di Query come un &quot;SELECT che restituisce righe&quot;](implementing-optimistic-concurrency-cs/_static/image26.png)](implementing-optimistic-concurrency-cs/_static/image25.png)

**Figura 9**: contrassegnare il tipo di Query come un "`SELECT` che restituisce righe" ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image27.png))


Nella schermata successiva è stiamo richiesto per la query SQL da usare, con query predefinita dell'oggetto TableAdapter precaricate. Aumentare la query esistente per includere la clausola `WHERE ProductID = @ProductID`, come illustrato nella figura 10.


[![Aggiungere una clausola WHERE clausola per la Query precaricata per restituire un Record di prodotto specifico](implementing-optimistic-concurrency-cs/_static/image29.png)](implementing-optimistic-concurrency-cs/_static/image28.png)

**Figura 10**: aggiungere un' `WHERE` clausola per la Query Pre-Loaded per restituire un Record di prodotto specifico ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image30.png))


Infine, modificare i nomi di metodo generato da `FillByProductID` e `GetProductByProductID`.


[![Rinominare i metodi FillByProductID e GetProductByProductID](implementing-optimistic-concurrency-cs/_static/image32.png)](implementing-optimistic-concurrency-cs/_static/image31.png)

**Figura 11**: rinominare i metodi `FillByProductID` e `GetProductByProductID` ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image33.png))


Con questa procedura guidata completata, l'oggetto TableAdapter contiene ora due metodi per il recupero dei dati: `GetProducts()`, che restituisce *tutto* prodotti; e `GetProductByProductID(productID)`, che restituisce il prodotto specificato.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Passaggio 3: Creazione di un livello di logica di Business per il campo DAL abilitate per la concorrenza ottimistica

Nostro esistente `ProductsBLL` classe contiene esempi dell'uso di aggiornamento batch sia il pattern diretta DB. Il `AddProduct` (metodo) e `UpdateProduct` entrambi gli overload utilizzano il modello di aggiornamento batch, passando un `ProductRow` istanza al metodo Update dell'oggetto TableAdapter. Il `DeleteProduct` metodo, d'altra parte, utilizza il modello di diretta database, la chiamata dell'oggetto TableAdapter `Delete(productID)` (metodo).

Con il nuovo `ProductsOptimisticConcurrency` TableAdapter, il database indirizzare metodi ora richiedono che i valori originali anche essere passato. Ad esempio, il `Delete` metodo ora prevede dieci parametri di input: originale `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, e `Discontinued`. Usa i valori in di questi altri parametri di input `WHERE` clausola del `DELETE` istruzione inviata al database, solo l'eliminazione di record specificato se il mapping di valori corrente del database fino a quelle originali.

Mentre la firma del metodo per i TableAdapter `Update` metodo usato nel criterio di aggiornamento batch non è stato modificato, ha il codice necessario per registrare i valori originali e quello nuovi. Pertanto, anziché tentare di utilizzare abilitate per la concorrenza ottimistica DAL nostro esistenti `ProductsBLL` di classi, è possibile creare una nuova classe di livello per la logica di Business per l'uso di DAL nostro nuovo.

Aggiungere una classe denominata `ProductsOptimisticConcurrencyBLL` per il `BLL` cartella all'interno di `App_Code` cartella.


![Aggiungere la classe ProductsOptimisticConcurrencyBLL nella cartella BLL](implementing-optimistic-concurrency-cs/_static/image34.png)

**Figura 12**: aggiungere il `ProductsOptimisticConcurrencyBLL` classe nella cartella BLL


Successivamente, aggiungere il codice seguente per il `ProductsOptimisticConcurrencyBLL` classe:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample5.cs)]

Si noti l'uso `NorthwindOptimisticConcurrencyTableAdapters` istruzione sopra l'inizio della dichiarazione di classe. Il `NorthwindOptimisticConcurrencyTableAdapters` spazio dei nomi contiene le `ProductsOptimisticConcurrencyTableAdapter` (classe), che fornisce i metodi del DAL. Anche prima della dichiarazione di classe sono disponibili le `System.ComponentModel.DataObject` attributo, che indica a Visual Studio per includere questa classe nell'elenco a discesa della procedura guidata ObjectDataSource.

Il `ProductsOptimisticConcurrencyBLL`del `Adapter` proprietà consente di accedere rapidamente a un'istanza del `ProductsOptimisticConcurrencyTableAdapter` classe e segue il modello usato nelle nostre classi BLL originale (`ProductsBLL`, `CategoriesBLL`e così via). Infine, il `GetProducts()` metodo chiama semplicemente verso il basso DAL `GetProducts()` metodo e restituisce un `ProductsOptimisticConcurrencyDataTable` oggetto popolato con un `ProductsOptimisticConcurrencyRow` istanza per ogni record di prodotto nel database.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>L'eliminazione di un prodotto usando il modello di Direct DB con la concorrenza ottimistica

Quando si usa il modello di direct DB contro DAL che usa la concorrenza ottimistica, è necessario passare i metodi i valori nuovi e originali. Per l'eliminazione, non sono presenti nuovi valori, pertanto solo i valori originali devono essere passati. Nel nostro livello BLL, quindi, è necessario accettare tutti i parametri originali come parametri di input. Osservare ora il `DeleteProduct` nel metodo il `ProductsOptimisticConcurrencyBLL` classe Usa il metodo diretto di database. Ciò significa che questo metodo deve eseguire tutti i campi di dati prodotto dieci come parametri di input e passarli al livello dal, come illustrato nel codice seguente:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample6.cs)]

Se i valori originali - i valori che sono stati caricati ultima nel controllo GridView (o DetailsView o FormView) - differiscono da quelli nel database quando l'utente fa clic sul pulsante Elimina il `WHERE` clausola non corrisponde a qualsiasi record di database e non record saranno interessati. Di conseguenza, dell'oggetto TableAdapter `Delete` metodo restituirà `0` e il livello BLL `DeleteProduct` metodo restituirà `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>L'aggiornamento di un prodotto usando il modello di aggiornamento Batch con la concorrenza ottimistica

Come indicato in precedenza, dell'oggetto TableAdapter `Update` metodo per il modello di aggiornamento batch ha la stessa firma del metodo indipendentemente dal fatto se viene impiegata la concorrenza ottimistica. Vale a dire, il `Update` metodo prevede un DataRow, una matrice di un set di dati tipizzato, un DataTable o DataRow. Non sono presenti parametri di input aggiuntivi per specificare i valori originali. Questo è possibile perché l'oggetto DataTable tiene traccia dei valori originali e modificati per relativo DataRow(s). Quando emette DAL relativo `UPDATE` istruzione, il `@original_ColumnName` i parametri vengono popolati con i valori originali di DataRow, mentre il `@ColumnName` parametri vengono popolati con i valori modificati di DataRow.

Nel `ProductsBLL` classe (che usa la concorrenza originale, non ottimistica DAL), quando si usa il modello di aggiornamento batch per aggiornare le informazioni sul prodotto, il codice esegue la sequenza di eventi seguente:

1. Leggere le informazioni di prodotto di database correnti in un `ProductRow` istanza dell'oggetto TableAdapter utilizzando `GetProductByProductID(productID)` (metodo)
2. Assegnare i nuovi valori per il `ProductRow` istanza dal passaggio 1
3. Chiamata dell'oggetto TableAdapter `Update` metodo, passando il `ProductRow` istanza

Questa sequenza di passaggi, tuttavia, non supporta correttamente la concorrenza ottimistica in quanto il `ProductRow` popolato nel passaggio 1 viene popolata direttamente dai database, vale a dire che i valori originali, utilizzati da DataRow sono quelli attualmente esistenti nel database e non quelli che sono state associate a GridView all'inizio del processo di modifica. Al contrario, quando tramite un ottimistica abilitate per la concorrenza DAL, è necessario modificare il `UpdateProduct` overload del metodo da usare la procedura seguente:

1. Leggere le informazioni di prodotto di database correnti in un `ProductsOptimisticConcurrencyRow` istanza dell'oggetto TableAdapter utilizzando `GetProductByProductID(productID)` (metodo)
2. Assegnare il *originali* i valori per il `ProductsOptimisticConcurrencyRow` istanza dal passaggio 1
3. Chiamare il `ProductsOptimisticConcurrencyRow` dell'istanza `AcceptChanges()` metodo, che indica il DataRow che i relativi valori correnti sono quelle "originale"
4. Assegnare il *nuove* i valori per il `ProductsOptimisticConcurrencyRow` istanza
5. Chiamata dell'oggetto TableAdapter `Update` metodo, passando il `ProductsOptimisticConcurrencyRow` istanza

Passaggio 1 letture eseguite in tutti i valori del database corrente per il record di prodotto specificato. Questo passaggio è superfluo nel `UpdateProduct` overload che consente di aggiornare *tutti* delle colonne del prodotto (come questi valori vengono sovrascritti nel passaggio 2), ma è essenziale per tali overload dove solo un subset dei valori di colonna vengono passati come parametri di input. Dopo che i valori originali sono stati assegnati al `ProductsOptimisticConcurrencyRow` istanza, il `AcceptChanges()` viene chiamato il metodo che contrassegna i valori di DataRow correnti come valori originali da utilizzare nel `@original_ColumnName` parametri in di `UPDATE` istruzione. Successivamente, i nuovi valori di parametro vengono assegnati le `ProductsOptimisticConcurrencyRow` e infine il `Update` metodo viene richiamato, passando il DataRow.

Il codice seguente illustra il `UpdateProduct` overload che accetta tutti i dati del prodotto campi come parametri di input. Sebbene non illustrato in questo caso, il `ProductsOptimisticConcurrencyBLL` classe incluso nel download per questa esercitazione contiene anche un `UpdateProduct` overload che accetta solo il nome del prodotto e prezzo come parametri di input.


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample7.cs)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Passaggio 4: Passando i valori originali e nuovi dalla pagina ASP.NET per i metodi BLL

Con DAL e BLL completo, resta che creare una pagina ASP.NET che può utilizzare la logica di concorrenza ottimistica incorporata sistema. In particolare, i dati di controllo Web (il controllo GridView, DetailsView o FormView) devono ricordare che i valori originali e ObjectDataSource deve passare entrambi i set di valori a livello della logica di Business. Inoltre, la pagina ASP.NET deve essere configurata per gestire correttamente eventuali violazioni alla concorrenza.

Iniziare aprendo il `OptimisticConcurrency.aspx` nella pagina la `EditInsertDelete` cartella e l'aggiunta di un controllo GridView nella finestra di progettazione, l'impostazione relativa `ID` proprietà `ProductsGrid`. Dallo smart tag del controllo GridView, scegliere di creare un nuovo oggetto ObjectDataSource denominato `ProductsOptimisticConcurrencyDataSource`. Poiché si desidera che ObjectDataSource usare DAL che supporta la concorrenza ottimistica, configurarlo per usare il `ProductsOptimisticConcurrencyBLL` oggetto.


[![È possibile utilizzare ObjectDataSource oggetto ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-cs/_static/image36.png)](implementing-optimistic-concurrency-cs/_static/image35.png)

**Figura 13**: è possibile utilizzare ObjectDataSource i `ProductsOptimisticConcurrencyBLL` oggetto ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image37.png))


Scegliere il `GetProducts`, `UpdateProduct`, e `DeleteProduct` metodi dagli elenchi a discesa nella procedura guidata. Per il metodo UpdateProduct, usare l'overload che accetta tutti i campi dati del prodotto.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Configurazione delle proprietà del controllo ObjectDataSource

Dopo aver completato la procedura guidata, markup dichiarativo di ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample8.aspx)]

Come può notare, il `DeleteParameters` raccolta contiene un `Parameter` istanza per ognuno dei parametri di input dieci nella `ProductsOptimisticConcurrencyBLL` della classe `DeleteProduct` (metodo). Analogamente, il `UpdateParameters` raccolta contiene un `Parameter` istanza per ognuno dei parametri di input nella `UpdateProduct`.

Per queste esercitazioni precedenti che per la modifica dei dati, verrà rimosso di ObjectDataSource `OldValuesParameterFormatString` proprietà a questo punto, dal momento che questa proprietà indica che il metodo BLL prevede che i valori precedenti (o originali) deve essere passato, nonché i nuovi valori. Inoltre, il valore della proprietà indica i nomi di parametro di input per i valori originali. Poiché si passa i valori originali nel livello BLL, eseguire *non* rimuovere questa proprietà.

> [!NOTE]
> Il valore della `OldValuesParameterFormatString` deve eseguire il mapping di proprietà per i nomi di parametro di input nel livello BLL che prevede che i valori originali. Poiché questi parametri sono denominati `original_productName`, `original_supplierID`e così via, è possibile lasciare il `OldValuesParameterFormatString` come valore della proprietà `original_{0}`. Se, tuttavia, i parametri di input dei metodi BLL presentano nomi come `old_productName`, `old_supplierID`e così via, è necessario aggiornare il `OldValuesParameterFormatString` proprietà `old_{0}`.


È un'impostazione della proprietà finale che deve essere inviata in ordine per ObjectDataSource passare correttamente i valori originali per i metodi di livello BLL. ObjectDataSource ha un [proprietà ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) che può essere assegnato a [uno dei due valori](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -il valore predefinito. non invia i valori originali per i parametri di input originale metodi BLL
- `CompareAllValues` -inviare i valori originali per i metodi BLL; Scegliere questa opzione quando si usa la concorrenza ottimistica

Si consiglia di impostare il `ConflictDetection` proprietà `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Configurazione delle proprietà e campi di GridView

Le proprietà di ObjectDataSource è configurata correttamente, è possibile concentrare l'attenzione per la configurazione di GridView. In primo luogo, poiché si desidera che il controllo GridView per supportare la modifica ed eliminazione, selezionare le caselle di controllo Abilita modifica e Abilita eliminazione dallo smart tag del controllo GridView. Verrà aggiunto un CommandField cui `ShowEditButton` e `ShowDeleteButton` sono entrambe impostate su `true`.

Quando è associato ai `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView contiene un campo per ognuno dei campi dati del prodotto. Mentre un GridView di questo tipo può essere modificato, l'esperienza utente è tutt'altro che accettabile. Il `CategoryID` e `SupplierID` BoundField verranno sottoposti a rendering come caselle di testo che l'utente debba immettere la categoria appropriata e il fornitore sotto forma di numeri ID. Non esisterà alcuna formattazione per i campi numerici e non i controlli di convalida per garantire che il nome del prodotto è stato specificato e che il prezzo unitario unità a magazzino, le unità in ordine e i valori del livello riordino sono entrambi valori numerici appropriati e sono maggiori di o uguale a su zero.

Come illustrato nella *aggiunta di controlli di convalida per la modifica e inserimento di interfacce* e *personalizzazione dell'interfaccia di modifica dei dati* esercitazioni, può essere personalizzato dall'interfaccia utente sostituendo i BoundField con TemplateFields. Ho modificato questo GridView e la relativa interfaccia modifica procedere nel modo seguente:

- Rimossa il `ProductID`, `SupplierName`, e `CategoryName` BoundField
- Convertire il `ProductName` BoundField in un TemplateField e aggiunto un controllo RequiredFieldValidation.
- Convertire le `CategoryID` e `SupplierID` BoundField per TemplateFields e regolare l'interfaccia di modifica per usare controlli DropDownList anziché nelle caselle di testo. In questi 'TemplateFields `ItemTemplates`, il `CategoryName` e `SupplierName` vengono visualizzati i campi dati.
- Convertire le `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` BoundField TemplateFields e controlli CompareValidator aggiunti.

Dopo aver già esaminato come eseguire queste attività nelle esercitazioni precedenti, verrà semplicemente elencare qui la sintassi dichiarativa finale e lasciare l'implementazione alla procedura consigliata.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample9.aspx)]

Siamo molto vicina con un esempio completo funzionante. Tuttavia, esistono alcune sottigliezze che scivoli progressivamente backup e che ci consentirà di problemi. Inoltre, è comunque necessario un tipo di interfaccia che avvisa l'utente quando si è verificata una violazione di concorrenza.

> [!NOTE]
> Affinché un controllo Web per dati da passare correttamente i valori originali a ObjectDataSource (che vengono quindi passati per il livello BLL), è fondamentale che il controllo GridView `EnableViewState` è impostata su `true` (predefinito). Se si disabilita lo stato di visualizzazione, i valori originali vengono persi nel postback.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Passare i valori originali corretti a ObjectDataSource

Esistono un paio di problemi relativi al modo in cui che è stato configurato il controllo GridView. Se di ObjectDataSource `ConflictDetection` è impostata su `CompareAllValues` (così come sono le nostre), quando di ObjectDataSource `Update()` o `Delete()` vengono richiamati i metodi per il controllo GridView (o DetailsView o FormView), ObjectDataSource tenta di copiare il I valori originali di GridView nella relativa appropriato `Parameter` istanze. Vedere Figura 2 per una rappresentazione grafica di questo processo.

In particolare, i valori originali di GridView vengono assegnati i valori nelle istruzioni di associazione dati bidirezionale ogni volta che l'associazione dati a GridView. Pertanto, è essenziale che i valori originali necessari tutti vengono acquisiti tramite associazione dati bidirezionale e che sono fornite in un formato convertibile.

Per visualizzare il motivo per cui questo aspetto è importante, si consiglia di visitare la pagina in un browser. Come previsto, il controllo GridView Elenca ogni prodotto con un pulsante di modifica e l'eliminazione della colonna all'estrema sinistra.


[![I prodotti sono elencati in un oggetto GridView](implementing-optimistic-concurrency-cs/_static/image39.png)](implementing-optimistic-concurrency-cs/_static/image38.png)

**Figura 14**: sono elencati i prodotti di in un controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image40.png))


Se si fa clic sul pulsante Elimina per qualsiasi prodotto, una `FormatException` viene generata un'eccezione.


[![Tentativo di eliminare eventuali risultati per i prodotti in un'eccezione FormatException](implementing-optimistic-concurrency-cs/_static/image42.png)](implementing-optimistic-concurrency-cs/_static/image41.png)

**Figura 15**: tentativo di eliminare qualsiasi prodotto risultati in un `FormatException` ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image43.png))


Il `FormatException` viene generato quando si tenta di leggere nell'originale ObjectDataSource `UnitPrice` valore. Poiché il `ItemTemplate` ha il `UnitPrice` formattati come valuta (`<%# Bind("UnitPrice", "{0:C}") %>`), include un simbolo di valuta, ad esempio 19,95 dollari con acquisto. Il `FormatException` si verifica come ObjectDataSource tenta di convertire questa stringa in un `decimal`. Per aggirare questo problema, sono disponibili diverse opzioni:

- Rimuovere il formato di valuta il `ItemTemplate`. Vale a dire, anziché usare `<%# Bind("UnitPrice", "{0:C}") %>`, è sufficiente usare `<%# Bind("UnitPrice") %>`. Lo svantaggio di questo è che il prezzo non è formattato.
- Visualizzazione di `UnitPrice` formattati come valuta nel `ItemTemplate`, ma usare il `Eval` (parola chiave) per eseguire questa operazione. Si tenga presente che `Eval` esegue l'associazione dati unidirezionale. Tuttavia è necessario fornire il `UnitPrice` valore per i valori originali, quindi sarà comunque necessario un'istruzione di associazione dati bidirezionale nel `ItemTemplate`, ma ciò può essere inserito in un controllo etichetta Web la cui proprietà `Visible` è impostata su `false`. In ItemTemplate, è possibile usare il markup seguente:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample10.aspx)]

- Rimuovere il formato di valuta la `ItemTemplate`, usando `<%# Bind("UnitPrice") %>`. In GridView `RowDataBound` gestore dell'evento, a livello di codice l'accesso ai controlli Web l'etichetta all'interno del quale il `UnitPrice` valore viene visualizzato e impostato relativo `Text` proprietà per la versione formattata.
- Lasciare il `UnitPrice` formattati come valuta. Nella finestra di GridView `RowDeleting` gestore eventi, sostituire l'originale esistente `UnitPrice` valore (19,95 dollari con acquisto) con un valore decimale effettivo tramite `Decimal.Parse`. Abbiamo visto come eseguire un'operazione simile nel `RowUpdating` gestore dell'evento nel [ *BLL - la gestione e le eccezioni di livello in una pagina ASP.NET* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) esercitazione.

Per il mio esempio ho deciso di passare con il secondo approccio, aggiunta di una Web etichetta nascosto controllo la cui `Text` proprietà è associata a non formattata ha dati bidirezionali `UnitPrice` valore.

Dopo aver risolto questo problema, provare a fare nuovamente clic sul pulsante Elimina per qualsiasi prodotto. Questa volta si otterrà un' `InvalidOperationException` ObjectDataSource quando tenta di richiamare il livello BLL `UpdateProduct` (metodo).


[![ObjectDataSource non è stato trovato un metodo con i parametri di Input che desidera trasmissione](implementing-optimistic-concurrency-cs/_static/image45.png)](implementing-optimistic-concurrency-cs/_static/image44.png)

**Figura 16**: ObjectDataSource non è stato trovato un metodo con i parametri di Input che desidera trasmissione ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image46.png))


Esaminando il messaggio dell'eccezione, è chiaro che ObjectDataSource intende richiamare un BLL `DeleteProduct` metodo che includa `original_CategoryName` e `original_SupplierName` parametri di input. Infatti il `ItemTemplate` s per il `CategoryID` e `SupplierID` TemplateFields attualmente contenere istruzioni di associazione bidirezionale con il `CategoryName` e `SupplierName` campi dati. In alternativa, è necessario includere `Bind` istruzioni con il `CategoryID` e `SupplierID` campi dati. A tale scopo, sostituire le istruzioni di associazione esistenti con `Eval` (istruzioni) e quindi aggiungere i controlli la cui etichetta nascosta `Text` sono associate ai `CategoryID` e `SupplierID` campi di dati utilizzando l'associazione dati bidirezionale, come illustrato di seguito:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample11.aspx)]

Con queste modifiche, a questo punto siamo in grado di eliminare e modificare le informazioni sul prodotto. Nel passaggio 5 esamineremo come verificare che vengano rilevate violazioni alla concorrenza. Ma per ora, richiedere alcuni minuti per provare ad aggiornare e l'eliminazione di alcuni record per assicurarsi che l'aggiornamento ed eliminazione per un singolo utente funziona come previsto.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Passaggio 5: Test del supporto della concorrenza ottimistica

Per verificare che eventuali violazioni alla concorrenza sono in corso rilevato (anziché ottenendo i dati vengano sovrascritti alla cieca), è necessario aprire due finestre del browser a questa pagina. In entrambe le istanze del browser, fare clic sul pulsante Modifica per Chai compare. Quindi, in uno solo dei browser, modificare il nome in "Chai tè" e fare clic su Aggiorna. L'aggiornamento deve avere esito positivo e restituire GridView lo stato di pre-modifica, con "Chai tè" come nuovo nome del prodotto.

A altra istanza finestra del browser, tuttavia, il nome del prodotto nella casella di testo viene ancora visualizzato "Chai". In questa seconda finestra del browser, aggiornare il `UnitPrice` a `25.00`. Senza il supporto della concorrenza ottimistica, fare clic su Aggiorna nella seconda istanza del browser modificherebbe il nome del prodotto al "Chai", in tal modo sovrascrivendo le modifiche apportate dalla prima istanza del browser. Con la concorrenza ottimistica usata, tuttavia, facendo clic sul pulsante Aggiorna nella seconda istanza del browser comporterà un [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Quando viene rilevata una violazione della concorrenza, viene generato un DBConcurrencyException](implementing-optimistic-concurrency-cs/_static/image48.png)](implementing-optimistic-concurrency-cs/_static/image47.png)

**Figura 17**: viene rilevato quando una violazione della concorrenza, un' `DBConcurrencyException` generata ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image49.png))


Il `DBConcurrencyException` viene generata solo quando viene utilizzato il modello di aggiornamento del valore DAL batch. Il modello di direct DB non genera un'eccezione, indica semplicemente che nessuna riga è interessata. Per illustrare questo concetto, restituiscono GridView entrambe le istanze di browser al rispettivo stato di pre-editing. Successivamente, la prima istanza del browser, fare clic sul pulsante di modifica e cambiare il nome di prodotto da "Chai tè" a "Chai" e fare clic su Aggiorna. Nella seconda finestra del browser, fare clic sul pulsante Elimina per Chai compare.

Facendo clic su Elimina, eseguito il postback della pagina, il controllo GridView richiama di ObjectDataSource `Delete()` metodo e ObjectDataSource chiama verso il basso il `ProductsOptimisticConcurrencyBLL` della classe `DeleteProduct` metodo, passando i valori originali. Originale `ProductName` valore per la seconda istanza del browser è "Chai tè", che non corrisponde corrente `ProductName` valore nel database. Di conseguenza il `DELETE` istruzione eseguita nel database influisce su zero righe poiché non sono presenti record nel database che la `WHERE` clausola soddisfa. Il `DeleteProduct` restituzione del metodo `false` e i dati di ObjectDataSource sono riassociati per il controllo GridView.

Dal punto di vista dell'utente finale, facendo clic sul pulsante Elimina per tè Chai compare nella seconda finestra del browser ha causato la schermata per un attimo e, al momento proveniente, il prodotto è ancora presente, anche se a questo punto viene visualizzato come "Chai" (prodotto nome modifica apportata dal browser prima istanza). Se l'utente fa clic sul pulsante Elimina anche in questo caso, l'eliminazione avrà esito positivo, quello originale di GridView `ProductName` valore ("Chai") corrisponde a questo punto con il valore nel database.

In entrambi i casi, l'esperienza utente è tutt'altro che ideale. Ovviamente non si desidera visualizzare i dettagli specifici relativi all'utente di `DBConcurrencyException` eccezione quando si usa il modello di aggiornamento batch. E il comportamento quando si usa il modello di direct DB è una certa confusione come comando gli utenti non riuscito, ma si è verificato alcuna indicazione precisa del motivo della generazione.

Per ovviare a questi due problemi, è possibile creare controlli etichetta Web nella pagina che fornisce una spiegazione per il motivo per cui un aggiornamento o eliminazione non è riuscita. Per il modello di aggiornamento batch, è possibile determinare se un `DBConcurrencyException` eccezione nel gestore dell'evento di post-livello del controllo GridView, riporta l'etichetta di avviso in base alle esigenze. Per il metodo diretto di database, è possibile esaminare il valore restituito del metodo BLL (ovvero `true` se è stata interessata una riga, `false` in caso contrario) e visualizzare un messaggio informativo in base alle esigenze.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Passaggio 6: Aggiunta di messaggi informativi e visualizzarli in caso di una violazione della concorrenza

Quando si verifica una violazione della concorrenza, comportamento anomalo dipende se è stata utilizzata l'aggiornamento batch o un modello diretta DB DAL. Seguire l'esercitazione Usa entrambi i modelli, con il modello di aggiornamento batch utilizzato per l'aggiornamento e il modello diretto DB usato per l'eliminazione. Per iniziare, aggiungiamo due controlli etichetta Web alla pagina di cui viene illustrato che si è verificata una violazione della concorrenza durante il tentativo di eliminare o aggiornare i dati. Impostare il controllo etichetta `Visible` e `EnableViewState` delle proprietà per `false`; in questo modo tali da risultare nascoste in ogni visita pagina con la differenza per quelli pagina particolare visits where loro `Visible` viene impostata a livello di codice su `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample12.aspx)]

Oltre all'impostazione loro `Visible`, `EnabledViewState`, e `Text` delle proprietà, ho impostato anche il `CssClass` proprietà `Warning`, in modo che l'etichetta da visualizzare in un tipo di carattere di grandi dimensioni, rosso, corsivo e grassetto. Il CSS `Warning` classe è stata definita e aggiunto a Styles. CSS nella *esaminando gli eventi associati con inserimento, aggiornamento ed eliminazione* esercitazione.

Dopo aver aggiunto queste etichette, la finestra di progettazione in Visual Studio dovrebbe essere simile alla figura 18.


[![Sono stati aggiunti due controlli Label per la pagina](implementing-optimistic-concurrency-cs/_static/image51.png)](implementing-optimistic-concurrency-cs/_static/image50.png)

**Figura 18**: due etichette controlli sono stati aggiunti alla pagina ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image52.png))


Con questi controlli etichetta Web attivati, siamo pronti per esaminare come determinare quando una violazione della concorrenza si è verificato, a cui punta l'etichetta appropriata `Visible` può essere impostata su `true`, visualizzando il messaggio informativo.

## <a name="handling-concurrency-violations-when-updating"></a>La gestione delle violazioni di concorrenza durante l'aggiornamento

Questa sezione descrive come gestire le violazioni di concorrenza quando si usa il modello di aggiornamento batch. Poiché tali violazioni con il batch di aggiornamento causa modello una `DBConcurrencyException` dell'eccezione, è necessario aggiungere codice alla nostra pagina ASP.NET per determinare se un `DBConcurrencyException` eccezione si è verificato durante il processo di aggiornamento. Se pertanto, si dovrebbe essere visualizzato un messaggio per l'utente che spiega che le modifiche non sono state salvate perché un altro utente ha modificato gli stessi dati tra quando ha iniziato a modificare il record e quando si è fatto clic sul pulsante di aggiornamento.

Come abbiamo visto nel *BLL - la gestione e le eccezioni di livello in una pagina ASP.NET* dell'esercitazione, tali eccezioni possono essere rilevate e soppressi nei gestori di evento di post-livello del controllo Web dei dati. Pertanto, è necessario creare un gestore eventi per il controllo GridView `RowUpdated` evento che controlla se un `DBConcurrencyException` è stata generata l'eccezione. Questo gestore eventi viene passato un riferimento a qualsiasi eccezione generata durante il processo di aggiornamento, come illustrato nel caso in cui gestore di codice seguente:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample13.cs)]

Nel caso di un `DBConcurrencyException` eccezione, questo gestore dell'evento Visualizza il `UpdateConflictMessage` controllo etichetta e indica che è stata gestita l'eccezione. Con questo codice, quando si verifica una violazione della concorrenza quando si aggiorna un record, le modifiche dell'utente vengono perse, dal momento che verrebbe hanno sovrascritto le modifiche a un altro utente nello stesso momento. In particolare, il controllo GridView è restituito al relativo stato di pre-modifica e associato ai dati del database corrente. La riga GridView verranno aggiornati con le altre modifiche dell'utente, che erano in precedenza non è visibile. Inoltre, il `UpdateConflictMessage` controllo etichetta verrà illustrato all'utente agli eventi. Questa sequenza di eventi è illustrata nella figura 19.


[![Un utente s gli aggiornamenti vengono perse in caso di una violazione della concorrenza](implementing-optimistic-concurrency-cs/_static/image54.png)](implementing-optimistic-concurrency-cs/_static/image53.png)

**Figura 19**: un utente s gli aggiornamenti vengono perse in caso di una violazione della concorrenza ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image55.png))


> [!NOTE]
> In alternativa, invece di restituire il controllo GridView per lo stato di pre-modifica, potremmo lasciamo GridView nello stato di modifica impostando il `KeepInEditMode` proprietà del passato aggiuntivo `GridViewUpdatedEventArgs` oggetto su true. Se si adotta questo approccio, tuttavia, essere certi che riassociare i dati a GridView (richiamando relativo `DataBind()` metodo), in modo che vengano caricati i valori di altro utente nell'interfaccia di modifica. Il codice disponibile per il download in questa esercitazione presenta queste due righe di codice nel `RowUpdated` gestore dell'evento come commento; è sufficiente rimuovere il commento queste righe di codice per avere il controllo GridView rimangono in modalità di modifica dopo una violazione della concorrenza.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Rispondere a eventuali violazioni alla concorrenza durante l'eliminazione

Con il modello diretto database, non viene creata alcuna eccezione generato in caso di una violazione della concorrenza. Al contrario, l'istruzione di database semplicemente influisce su alcun record, come la clausola WHERE non corrisponde a tutti i record. Tutti i metodi di modifica dei dati creati nel livello BLL sono stati progettati in modo che restituiscano un valore booleano che indica se è interessato esattamente di un record. Pertanto, per determinare se si è verificata una violazione della concorrenza quando si elimina un record, è possibile esaminare il valore restituito del livello BLL `DeleteProduct` (metodo).

Il valore restituito per un metodo BLL può essere esaminato nei gestori di evento di post-livello di ObjectDataSource tramite il `ReturnValue` proprietà del `ObjectDataSourceStatusEventArgs` oggetto passato nel gestore dell'evento. Poiché si è interessati a determinare il valore restituito dal `DeleteProduct` metodo, è necessario creare un gestore eventi per ObjectDataSource `Deleted` evento. Il `ReturnValue` proprietà è di tipo `object` e può essere `null` se è stata generata un'eccezione e il metodo è stato interrotto prima potrebbe restituire un valore. Pertanto, si dovrebbe verificare innanzitutto che il `ReturnValue` proprietà non `null` ed è un valore booleano. Supponendo che questo controllo ha esito positivo, viene illustrato il `DeleteConflictMessage` controllo etichetta, se il `ReturnValue` è `false`. A questo scopo tramite il codice seguente:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample14.cs)]

In caso di una violazione della concorrenza, viene annullata la richiesta di eliminazione dell'utente. Il controllo GridView viene aggiornato, che mostra le modifiche che si sono verificati per tale record tra il momento in cui l'utente è caricata la pagina e quando ha fatto clic sul pulsante Elimina. Quando accade realmente una violazione di questo tipo, il `DeleteConflictMessage` viene visualizzata l'etichetta, che spiega agli eventi (vedere Figura 20).


[![Un utente s Delete viene annullato in caso di una violazione della concorrenza](implementing-optimistic-concurrency-cs/_static/image57.png)](implementing-optimistic-concurrency-cs/_static/image56.png)

**Figura 20**: un utente s Delete viene annullato in caso di una violazione della concorrenza ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-cs/_static/image58.png))


## <a name="summary"></a>Riepilogo

Opportunità per eventuali violazioni alla concorrenza esiste in ogni applicazione che consente a più utenti simultanei per aggiornare o eliminare dati. Se per, quando due utenti aggiornano contemporaneamente gli stessi dati utente che ottiene l'ultima scrittura "WINS", la sovrascrittura di altro utente modifica le modifiche non vengono presi in considerazione tali violazioni. In alternativa, gli sviluppatori possono implementare uno dei due controlli di concorrenza ottimistica o pessimistica. Controllo della concorrenza ottimistica presuppone che eventuali violazioni alla concorrenza siano poco frequenti e semplicemente non consente un aggiornamento o eliminazione di comando che costituisce una violazione della concorrenza. Controllo della concorrenza pessimistica presuppone che la concorrenza le violazioni sono frequenti e semplicemente il rifiuto di un utente di aggiornare o eliminare i comandi non è accettabile. Con controllo della concorrenza pessimistica, un record di aggiornamento implica il blocco, impedendo in tal modo di qualsiasi altro utente di modificare o eliminare il record, mentre è bloccato.

Il set di dati tipizzati in .NET fornisce funzionalità per il supporto di controllo della concorrenza ottimistica. In particolare, il `UPDATE` e `DELETE` istruzioni eseguite al database includono tutte le colonne della tabella, in modo da garantire che l'aggiornamento o eliminazione viene eseguita solo se i dati del record corrente corrispondano con i dati originali all'utente che avevano quando esecuzione di loro update o delete. Dopo aver configurato per supportare la concorrenza ottimistica, è necessario aggiornare i metodi di livello BLL. Inoltre, la pagina ASP.NET che chiama verso il basso il livello BLL deve essere configurata in modo che ObjectDataSource recupera i valori originali dal relativo controllo Web per dati e li passa il livello BLL.

Come abbiamo visto in questa esercitazione, che implementa il controllo della concorrenza ottimistica in un'applicazione web ASP.NET implica l'aggiornamento DAL e BLL e aggiungere il supporto in una pagina ASP.NET. Indica se il lavoro aggiuntivo è un investimento a livello di tempo e fatica dipende dall'applicazione. Se hai raramente utenti simultanei, l'aggiornamento dei dati o i dati che viene aggiornata sono diversi da altra, quindi il controllo della concorrenza non è un problema principale. Se, tuttavia, normalmente sono presenti più utenti nel sito di lavorare con gli stessi dati, il controllo della concorrenza può aiutare a evitare eliminazioni o aggiornamenti di un utente sovrascrivano involontariamente degli altri.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](customizing-the-data-modification-interface-cs.md)
> [Successivo](adding-client-side-confirmation-when-deleting-cs.md)
