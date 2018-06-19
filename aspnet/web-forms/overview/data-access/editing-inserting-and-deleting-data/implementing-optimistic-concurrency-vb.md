---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implementazione della concorrenza ottimistica (VB) | Documenti Microsoft
author: rick-anderson
description: Per un'applicazione web che consente a più utenti di modificare i dati, sussiste il rischio che due utenti possono modificare gli stessi dati nello stesso momento. In questo tutori...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 056d907e80b5bdfa1848b4b31cb03702ca823583
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890944"
---
<a name="implementing-optimistic-concurrency-vb"></a>Implementazione della concorrenza ottimistica (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) o [Scarica il PDF](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Per un'applicazione web che consente a più utenti di modificare i dati, sussiste il rischio che due utenti possono modificare gli stessi dati nello stesso momento. In questa esercitazione verrà implementato il controllo della concorrenza ottimistica per gestire il rischio.


## <a name="introduction"></a>Introduzione

Per le applicazioni web che consentono solo agli utenti di visualizzare dati o per quelle che includono solo un singolo utente può modificare i dati, non sussiste alcun rischio di due utenti simultanei sovrascrivere le modifiche apportate da altri. Per le applicazioni web che consentono a più utenti di aggiornare o eliminare dati, tuttavia, è la possibilità che le modifiche dell'utente essere in conflitto con un altro simultanee dell'utente. Senza qualsiasi criterio di concorrenza sul posto, quando due utenti modificano contemporaneamente un singolo record, l'utente che esegue il commit delle proprie modifiche ultima sostituiranno le modifiche apportate dal primo.

Ad esempio, si supponga che due utenti, Jisun e Sam, sono stati entrambi visitare una pagina nell'applicazione che consente di aggiornare ed eliminare i prodotti tramite un controllo GridView. Entrambi fare clic sul pulsante di modifica in GridView intorno alla stessa ora. Jisun diventa il nome del prodotto "Chai tè" e fa clic sul pulsante di aggiornamento. Il risultato è un `UPDATE` istruzione che viene inviato al database, che imposta *tutti* dei campi aggiornabili del prodotto (anche se Jisun aggiornate solo a un campo, `ProductName`). In questo momento, il database contiene i valori "Chai tè", la categoria Beverages, esotiche liquide, fornitore e così via per il prodotto specifico. Nella schermata di Giorgio GridView ancora viene tuttavia il nome del prodotto nella riga GridView modificabile come "Chai". Pochi secondi dopo che le modifiche del Jisun sono state eseguite, Sam gli aggiornamenti della categoria Condiments e fa clic su Aggiorna. Ciò comporta un `UPDATE` istruzione inviata al database che imposta il nome del prodotto "Chai", il `CategoryID` per il corrispondente ID della categoria Beverages e così via. Modifiche del Jisun al nome del prodotto sono stati sovrascritti. Figura 1 illustra graficamente la serie di eventi.


[![Quando due utenti possono Aggiorna contemporaneamente un potenziale s non sono presenti Record per le modifiche dell'utente sovrascrivere l'altro](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Figura 1**: quando due utenti simultaneamente aggiornare un Record sono s potenziale per un utente cambia per sovrascrivere l'altro ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image3.png))


Analogamente, quando due utenti possono visitare una pagina, un utente potrebbe essere trova nel mezzo di aggiornamento di un record quando viene eliminato da un altro utente. In alternativa, tra quando un utente carica una pagina e facendo clic sul pulsante Elimina, un altro utente potrebbe avere modificato il contenuto di tale record.

Sono disponibili tre [il controllo della concorrenza](http://en.wikipedia.org/wiki/Concurrency_control) strategie disponibili:

- **Non eseguire alcuna operazione** -se utenti simultanei modifica lo stesso record, si supponga che l'ultimo commit win (comportamento predefinito)
- [**Concorrenza ottimistica** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -presuppongono che sebbene potrebbero essere presenti conflitti di concorrenza ogni ora e quindi la maggior parte dei casi non si verificheranno conflitti di questo tipo; pertanto, se si verificano un conflitto, semplicemente informare l'utente che le relative modifiche non può essere salvato perché un altro utente ha modificato gli stessi dati
- **Concorrenza pessimistica** -presuppongono che i conflitti di concorrenza sono comuni e che gli utenti non saranno in grado di tollerare being indicato non sono state salvate le modifiche apportate a causa di attività simultanee di un altro utente; pertanto, quando un utente avvia l'aggiornamento di un record, blocco , impedendo in questo modo qualsiasi altro utente di modificare o eliminare tale record fino a quando l'utente conferma le modifiche

Tutte le esercitazioni sono utilizzati finora la strategia di risoluzione predefinito concorrenza: in particolare, è stata consentono l'ultima scrittura win. In questa esercitazione si esamineranno come implementare il controllo della concorrenza ottimistica.

> [!NOTE]
> Non vengono analizzati negli esempi di concorrenza pessimistica questa serie di esercitazioni. Concorrenza pessimistica viene usata raramente poiché ad esempio, se non viene comunque rilasciata non correttamente, può impedire l'aggiornamento dei dati ad altri utenti. Ad esempio, se un utente blocca un record per la modifica e quindi lascia per il giorno precedente lo sblocco, nessun altro utente sarà in grado di aggiornare tale record fino a quando l'utente originale restituisce e completa il suo aggiornamento. Pertanto, in situazioni in cui viene utilizzata la concorrenza pessimistica, è in genere un timeout che, se raggiunta, Annulla il blocco. Ticket vendite i siti Web, che consente di bloccare un percorso specifico sedere per breve periodo mentre l'utente completa il processo dell'ordine, è riportato un esempio di controllo della concorrenza pessimistica.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Passaggio 1: Esaminando la modalità di concorrenza ottimistica viene implementato.

Controllo della concorrenza ottimistica funziona, verificare che il record da aggiornare o eliminare ha gli stessi valori che aveva all'avvio dell'aggiornamento o eliminazione di processo. Ad esempio, quando si fa clic sul pulsante di modifica in un controllo GridView. modificabile, del record di leggere dal database e visualizzazione dei valori nelle caselle di testo e altri controlli Web. Questi valori originali vengono salvati per il controllo GridView. In un secondo momento, dopo che l'utente effettua le proprie modifiche e fa clic sul pulsante di aggiornamento, i valori originali e i nuovi valori vengono inviati al livello di logica di Business e fino a livello di accesso ai dati. Il livello di accesso ai dati deve eseguire un'istruzione SQL che aggiornerà il record solo se i valori originali che l'utente ha iniziato la modifica sono identici a quelli ancora nel database. Figura 2 viene illustrata questa sequenza di eventi.


[![Per Update o Delete abbia esito positivo, i valori originali devono essere uguali per i valori del Database corrente](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Figura 2**: per Update o Delete su esito positivo, l'originale valori deve essere uguale per i valori del Database corrente ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image6.png))


Sono disponibili diversi approcci per l'implementazione della concorrenza ottimistica (vedere [Peter A. Bromberg](http://peterbromberg.net/)del [Optmistic concorrenza aggiornamento logica](http://www.eggheadcafe.com/articles/20050719.asp) per una breve panoramica una serie di opzioni). Il DataSet tipizzato ADO.NET fornisce un'implementazione che può essere configurata con solo il segno di graduazione di una casella di controllo. L'abilitazione di concorrenza ottimistica per un oggetto TableAdapter del dataset tipizzato aumenta del TableAdapter `UPDATE` e `DELETE` istruzioni da includere un confronto di tutti i valori originali di `WHERE` clausola. Le operazioni seguenti `UPDATE` istruzione, ad esempio, aggiorna il nome e il prezzo di un prodotto solo se i valori del database corrente sono uguali a quelli che sono stati recuperati inizialmente quando si aggiorna il record in GridView. Il `@ProductName` e `@UnitPrice` parametri contengono i nuovi valori immessi dall'utente, mentre `@original_ProductName` e `@original_UnitPrice` contengono i valori che sono stati originariamente caricati in GridView, quando è stato fatto clic sul pulsante di modifica:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> Questo `UPDATE` istruzione è stata semplificata per migliorare la leggibilità. In pratica, il `UnitPrice` archivia il `WHERE` clausola sarebbe più complessa poiché `UnitPrice` può contenere `NULL` s e durante la verifica del `NULL = NULL` restituisce sempre False (è necessario utilizzare `IS NULL`).


Oltre a utilizzare un diverso sottostante `UPDATE` istruzione, un oggetto TableAdapter utilizzare ottimistica concorrenza anche di modificare la firma del relativo database di configurazione diretta di metodi. Ricorda la prima esercitazione, [ *la creazione di un livello di accesso ai dati*](../introduction/creating-a-data-access-layer-cs.md), che sono stati metodi diretti DB che accetta un elenco di scalare i valori come parametri di input (anziché un DataRow fortemente tipizzato o Istanza di DataTable). Quando si utilizza la concorrenza ottimistica, il database diretto `Update()` e `Delete()` metodi includono parametri di input per i valori originali. Inoltre, il codice il livello Business LOGIC per l'utilizzo di batch di aggiornamento modello (il `Update()` che accettano DataRows e DataTable anziché valori scalari) deve essere modificata anche.

Invece di estendere il nostro esistente TableAdapter del per usare la concorrenza ottimistica (che modifica il livello Business LOGIC per contenere necessiterebbe), verrà invece creare un nuovo DataSet tipizzato denominato `NorthwindOptimisticConcurrency`, a cui verrà aggiunto un `Products` TableAdapter che utilizza la concorrenza ottimistica. Successivamente, si creerà un `ProductsOptimisticConcurrencyBLL` classe a livello di logica di Business con le modifiche appropriate per supportare la concorrenza ottimistica DAL. Una volta poste questo presupposti, saremo pronti a creare la pagina ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Passaggio 2: Creazione di un livello di accesso ai dati che supporta la concorrenza ottimistica

Per creare un nuovo set di dati tipizzato, fare clic su di `DAL` cartella all'interno di `App_Code` cartella e aggiungere un nuovo set di dati denominato `NorthwindOptimisticConcurrency`. Come abbiamo visto nella prima esercitazione, ciò pertanto verrà aggiunto un nuovo TableAdapter al set di dati tipizzato, avviare automaticamente la configurazione guidata TableAdapter. Nella prima schermata, ci stiamo richiesto per specificare il database per connettersi, connettersi al database Northwind stesso usando il `NORTHWNDConnectionString` impostazione dal `Web.config`.


[![Connettersi allo stesso Database Northwind](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Figura 3**: la connessione al Northwind Database stesso ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image9.png))


Successivamente, vengono richieste eseguire query sui dati la: tramite un'istruzione SQL ad hoc, una nuova stored procedure o una stored procedure di un oggetto esistente. Poiché è utilizzato query SQL ad hoc in DAL nostro originale, usare questa opzione qui.


[![Specificare i dati da recuperare tramite un'istruzione SQL Ad Hoc](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Figura 4**: specificare i dati da recuperare tramite un'istruzione SQL Ad Hoc ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image12.png))


Nella schermata seguente, immettere la query SQL da utilizzare per recuperare le informazioni sul prodotto. Consente di usare la query SQL stesso esatta utilizzata per la `Products` TableAdapter dal nostro DAL originale, che restituisce tutti i `Product` colonne con nomi di categoria e fornitore del prodotto:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]


[![Utilizzare la stessa Query SQL dal TableAdapter prodotti DAL originale](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Figura 5**: utilizzare la stessa Query SQL dal `Products` TableAdapter in originale DAL ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image15.png))


Prima di spostare nella schermata successiva, fare clic sul pulsante Opzioni avanzate. Per questo controllo della concorrenza ottimistica utilizzano TableAdapter, è sufficiente selezionare la casella di controllo "Usa concorrenza ottimistica".


[![Abilitare il controllo della concorrenza ottimistica dal controllo il &quot;usare la concorrenza ottimistica&quot; casella di controllo](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Figura 6**: abilitare controllo della concorrenza ottimistica selezionando la casella di controllo "Usa concorrenza ottimistica" ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image18.png))


Infine, indicare il TableAdapter di utilizzare i modelli di accesso ai dati che Riempi un DataTable e restituiscono un DataTable; indicare inoltre che i metodi diretti DB devono essere creati. Modificare il nome del metodo per la restituzione di un modello di oggetto DataTable da GetData a GetProducts, in modo che le convenzioni di denominazione che viene usata nel nostro originale DAL mirror.


[![Disporre del TableAdapter utilizzare tutti i modelli di accesso ai dati](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Figura 7**: sono gli TableAdapter utilizzare tutti i dati schemi di accesso ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image21.png))


Dopo aver completato la procedura guidata, la finestra di progettazione del set di dati includerà un fortemente tipizzato `Products` DataTable e TableAdapter. È opportuno rinominare la DataTable dal `Products` a `ProductsOptimisticConcurrency`, che è possibile effettuare facendo clic sulla barra del titolo del DataTable e scegliendo Rinomina dal menu di scelta rapida.


[![Un oggetto DataTable e TableAdapter sono stati aggiunti al DataSet tipizzato](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Figura 8**: un DataTable e TableAdapter sono stati aggiunti al set di dati tipizzato ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image24.png))


Per visualizzare le differenze tra il `UPDATE` e `DELETE` esegue una query tra il `ProductsOptimisticConcurrency` TableAdapter (che utilizza la concorrenza ottimistica) e TableAdapter di prodotti (che non), fare clic sul TableAdapter e passare alla finestra Proprietà. Nel `DeleteCommand` e `UpdateCommand` proprietà `CommandText` sottoproprietà è possibile visualizzare la sintassi SQL che viene inviata al database quando vengono richiamati DAL update o delete relativi metodi. Per il `ProductsOptimisticConcurrency` TableAdapter di `DELETE` istruzione utilizzata è:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Mentre il `DELETE` istruzione per il prodotto TableAdapter in DAL nostro originale è molto più semplice:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Come si può notare, la `WHERE` clausola il `DELETE` istruzione per il TableAdapter che utilizza la concorrenza ottimistica è incluso un confronto tra le varie il `Product` tabella esistente di valori di colonna e i valori originali al momento il GridView (o DetailsView o FormView) dell'ultimo popolamento. Poiché tutti i campi diverso `ProductID`, `ProductName`, e `Discontinued` può avere `NULL` valori, i parametri aggiuntivi e controlli sono inclusi per confrontare correttamente `NULL` i valori di `WHERE` clausola.

È non verrà aggiunta qualsiasi DataTable aggiuntive per il set di dati abilitate per la concorrenza ottimistica per questa esercitazione, la pagina ASP.NET fornirà solo aggiornamento ed eliminazione di informazioni sul prodotto. Tuttavia, è comunque necessario aggiungere il `GetProductByProductID(productID)` metodo il `ProductsOptimisticConcurrency` TableAdapter.

A tale scopo, fare doppio clic sulla barra del titolo dell'oggetto TableAdapter (destra area di sopra di `Fill` e `GetProducts` i nomi di metodo) e scegliere Aggiungi Query dal menu di scelta rapida. Verrà avviata la configurazione guidata Query TableAdapter. Come con la configurazione iniziale del TableAdapter il consenso esplicito per creare il `GetProductByProductID(productID)` metodo utilizzando un'istruzione SQL ad hoc (vedere la figura 4). Poiché il `GetProductByProductID(productID)` restituisce informazioni su un particolare prodotto, indicare che questa query è un `SELECT` tipo che restituisce righe di query.


[![Contrassegnare il tipo di Query come un &quot;SELECT che restituisce righe&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Figura 9**: contrassegnare il tipo di Query come un "`SELECT` che restituisce righe" ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image27.png))


Nella schermata successiva verrà chiesta la query SQL da utilizzare, con la query del TableAdapter predefinito precaricata. Aumentare la query esistente per includere la clausola `WHERE ProductID = @ProductID`, come illustrato nella figura 10.


[![Aggiungere una clausola WHERE clausola per la Query precaricata per restituire un Record di prodotto specifico](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Figura 10**: aggiungere un `WHERE` clausola alla Query Pre-Loaded per restituire un Record di prodotto specifico ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image30.png))


Infine, modificare i nomi di metodo generato per `FillByProductID` e `GetProductByProductID`.


[![Rinominare i metodi per FillByProductID e GetProductByProductID](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Figura 11**: i metodi da rinominare `FillByProductID` e `GetProductByProductID` ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image33.png))


Con questa procedura guidata completata, il TableAdapter contiene ora due metodi per il recupero dei dati: `GetProducts()`, che restituisce *tutti* prodotti; e `GetProductByProductID(productID)`, che restituisce il prodotto specificato.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Passaggio 3: Creazione di un livello di logica di Business per DAL abilitate per la concorrenza ottimistica

Il nostro esistente `ProductsBLL` classe dispone di esempi di utilizzo di aggiornamento batch sia modelli diretti DB. Il `AddProduct` (metodo) e `UpdateProduct` entrambi gli overload utilizzano il modello di aggiornamento batch, passando un `ProductRow` istanza al metodo di aggiornamento dell'oggetto TableAdapter. Il `DeleteProduct` metodo, invece, utilizza il modello diretto DB, la chiamata dell'oggetto TableAdapter `Delete(productID)` metodo.

Con il nuovo `ProductsOptimisticConcurrency` TableAdapter, il database diretta metodi ora richiedono che i valori originali anche essere passati in. Ad esempio, il `Delete` metodo prevede ora dieci parametri di input: originale `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, e `Discontinued`. Usa questi ulteriori input valori dei parametri in `WHERE` clausola del `DELETE` istruzione inviata al database di sola eliminazione del record specificato se eseguire il mapping di valori corrente del database fino a quelle originali.

Durante la firma del metodo per il TableAdapter `Update` metodo usato nel criterio di aggiornamento batch non è stato modificato, è il codice necessario per registrare i valori originali e quelli nuovi. Pertanto, anziché tentare di utilizzare DAL abilitate per la concorrenza ottimistica con il nostro esistente `ProductsBLL` classe, è possibile crearne una nuova classe di livello di logica di Business per l'utilizzo con il nostro DAL nuovo.

Aggiungere una classe denominata `ProductsOptimisticConcurrencyBLL` per il `BLL` cartella all'interno di `App_Code` cartella.


![Aggiungere la classe ProductsOptimisticConcurrencyBLL nella cartella BLL](implementing-optimistic-concurrency-vb/_static/image34.png)

**Figura 12**: aggiungere la `ProductsOptimisticConcurrencyBLL` classe nella cartella BLL


Successivamente, aggiungere il codice seguente per la `ProductsOptimisticConcurrencyBLL` classe:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Si noti l'utilizzo `NorthwindOptimisticConcurrencyTableAdapters` istruzione sopra l'inizio della dichiarazione di classe. Il `NorthwindOptimisticConcurrencyTableAdapters` spazio dei nomi contiene la `ProductsOptimisticConcurrencyTableAdapter` (classe), che fornisce i metodi del DAL. Anche prima della dichiarazione di classe sono disponibili le `System.ComponentModel.DataObject` attributo, che indica a Visual Studio per includere questa classe nell'elenco a discesa della procedura guidata ObjectDataSource.

Il `ProductsOptimisticConcurrencyBLL`del `Adapter` proprietà consente di accedere rapidamente a un'istanza del `ProductsOptimisticConcurrencyTableAdapter` classe e segue il modello usato con le nostre classi BLL originale (`ProductsBLL`, `CategoriesBLL`e così via). Infine, il `GetProducts()` metodo chiama semplicemente verso il basso il campo DAL `GetProducts()` (metodo) e restituisce un `ProductsOptimisticConcurrencyDataTable` oggetto popolato con un `ProductsOptimisticConcurrencyRow` istanza per ogni record di prodotto nel database.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>L'eliminazione di un prodotto utilizzando il modello diretto DB con concorrenza ottimistica

Quando si utilizza il modello diretto DB rispetto a DAL che utilizza la concorrenza ottimistica, è necessario passare i metodi dei valori nuovi e originali. Per l'eliminazione, non sono presenti nuovi valori, pertanto è necessario essere passati solo i valori originali. Nel nostro BLL, quindi, è necessario accettare tutti i parametri originali come parametri di input. Ora verrà il `DeleteProduct` metodo la `ProductsOptimisticConcurrencyBLL` classe utilizzare il metodo diretto DB. Ciò significa che questo metodo deve eseguire in tutti i campi di dati prodotto dieci come parametri di input e passare al DAL, come illustrato nel codice seguente:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Se i valori originali - i valori che sono stati caricati ultima nel GridView (o DetailsView o FormView) - sono diversi dai valori nel database quando l'utente fa clic sul pulsante Elimina il `WHERE` clausola non corrisponde a nessun record e qualsiasi record di database saranno interessate. Di conseguenza, dell'oggetto TableAdapter `Delete` metodo restituirà `0` e il livello Business LOGIC `DeleteProduct` metodo restituirà `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>L'aggiornamento di un prodotto utilizzando il modello di aggiornamento Batch con concorrenza ottimistica

Come accennato in precedenza, dell'oggetto TableAdapter `Update` metodo per il modello di aggiornamento batch ha la stessa firma del metodo indipendentemente dal fatto è utilizzata la concorrenza ottimistica. In particolare, il `Update` metodo prevede un DataRow, una matrice di DataRow, un oggetto DataTable o DataSet tipizzato. Non sono presenti parametri di input aggiuntivi per specificare i valori originali. Questo è possibile perché il DataTable tiene traccia dei valori originali o modificati per il relativo DataRow(s). Quando di esegue DAL relativo `UPDATE` istruzione, il `@original_ColumnName` i parametri vengono popolati con i valori originali di DataRow, mentre il `@ColumnName` i parametri vengono popolati con i valori modificati di DataRow.

Nel `ProductsBLL` classe (che utilizza la concorrenza originale, non ottimistica DAL), quando si utilizza il modello di aggiornamento batch per aggiornare informazioni sul prodotto, il codice esegue la sequenza di eventi seguente:

1. Leggere le informazioni sul prodotto di database corrente in un `ProductRow` istanza dell'oggetto TableAdapter utilizzando `GetProductByProductID(productID)` (metodo)
2. Assegnare i nuovi valori di `ProductRow` istanza dal passaggio 1
3. Chiamare il TableAdapter `Update` metodo passando la `ProductRow` istanza

Questa sequenza di passaggi, tuttavia, non supporta la concorrenza ottimistica correttamente perché il `ProductRow` popolato nel passaggio 1 viene popolato direttamente dal database, ovvero i valori originali per il DataRow sono quelli attualmente esistenti nel database e non quelli che sono state associate a GridView all'inizio del processo di modifica. Al contrario, quando tramite un ottimistica abilitate per la concorrenza DAL, è necessario modificare il `UpdateProduct` overload del metodo da utilizzare la procedura seguente:

1. Leggere le informazioni sul prodotto di database corrente in un `ProductsOptimisticConcurrencyRow` istanza dell'oggetto TableAdapter utilizzando `GetProductByProductID(productID)` (metodo)
2. Assegnare il *originale* i valori per il `ProductsOptimisticConcurrencyRow` istanza dal passaggio 1
3. Chiamare il `ProductsOptimisticConcurrencyRow` dell'istanza `AcceptChanges()` metodo, che indica il DataRow che i valori correnti sono quelle "originale"
4. Assegnare il *nuova* i valori per il `ProductsOptimisticConcurrencyRow` istanza
5. Chiamare il TableAdapter `Update` metodo passando la `ProductsOptimisticConcurrencyRow` istanza

Passaggio 1 letture in tutti i valori del database corrente per il record di prodotto specificato. Questo passaggio è superfluo nel `UpdateProduct` overload che aggiorna *tutti* delle colonne di prodotto (come questi valori vengono sovrascritte nel passaggio 2), ma è essenziale per questi overload in cui solo un sottoinsieme dei valori di colonna vengono passati come parametri di input. Dopo che i valori originali sono stati assegnati al `ProductsOptimisticConcurrencyRow` istanza, il `AcceptChanges()` metodo viene chiamato, che indica che i valori di DataRow correnti come i valori originali da usare nel `@original_ColumnName` parametri il `UPDATE` istruzione. Successivamente, i nuovi valori di parametro vengono assegnati le `ProductsOptimisticConcurrencyRow` e infine il `Update` metodo viene richiamato passando il DataRow.

Il codice seguente illustra il `UpdateProduct` overload che accetta tutti i dati di prodotto campi come parametri di input. Mentre non riportati di seguito, il `ProductsOptimisticConcurrencyBLL` classe incluso nel download per questa esercitazione contiene anche un `UpdateProduct` overload che accetta solo il nome del prodotto e prezzo come parametri di input.


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Passaggio 4: Passando i valori originali e quelli nuovi dalla pagina ASP.NET per i metodi BLL

Con DAL e BLL completa, è comunque creare una pagina ASP.NET che può utilizzare la logica di concorrenza ottimistica incorporata sistema. In particolare, i dati di controllo Web (GridView, DetailsView o FormView) è necessario ricordare che i relativi valori originali e ObjectDataSource deve passare entrambi i set di valori per il livello di logica di Business. La pagina ASP.NET deve inoltre essere configurata per gestire correttamente eventuali violazioni alla concorrenza.

Avvia aprendo il `OptimisticConcurrency.aspx` nella pagina di `EditInsertDelete` cartella e l'aggiunta di un controllo GridView nella finestra di progettazione, l'impostazione relativa `ID` proprietà `ProductsGrid`. Smart tag del controllo GridView, scegliere di creare un nuovo oggetto ObjectDataSource denominato `ProductsOptimisticConcurrencyDataSource`. Poiché si desidera utilizzare DAL che supporta la concorrenza ottimistica ObjectDataSource, configurarlo per utilizzare il `ProductsOptimisticConcurrencyBLL` oggetto.


[![È possibile utilizzare ObjectDataSource l'oggetto ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Figura 13**: è possibile utilizzare ObjectDataSource il `ProductsOptimisticConcurrencyBLL` oggetto ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image37.png))


Scegliere il `GetProducts`, `UpdateProduct`, e `DeleteProduct` metodi dagli elenchi a discesa nella procedura guidata. Per il metodo UpdateProduct, utilizzare l'overload che accetta tutti i campi di dati del prodotto.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Configurazione delle proprietà del controllo ObjectDataSource

Dopo aver completato la procedura guidata, markup dichiarativo di ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Come si può notare, la `DeleteParameters` raccolta contiene un `Parameter` istanza per ognuno dei parametri di input dieci nella `ProductsOptimisticConcurrencyBLL` della classe `DeleteProduct` metodo. Analogamente, il `UpdateParameters` raccolta contiene un `Parameter` istanza per ogni parametro di input in `UpdateProduct`.

Per le esercitazioni precedenti che per la modifica dei dati, verrà rimosso il ObjectDataSource `OldValuesParameterFormatString` proprietà a questo punto, poiché questa proprietà indica che il metodo BLL prevede i valori precedenti (o originali) deve essere passato, nonché i nuovi valori. Inoltre, il valore della proprietà indica i nomi di parametro di input per i valori originali. Poiché i valori originali viene passato in BLL, *non* rimuovere tale proprietà.

> [!NOTE]
> Il valore di `OldValuesParameterFormatString` proprietà necessario eseguire il mapping ai nomi di parametro di input di BLL che prevede che i valori originali. Poiché questi parametri è denominata `original_productName`, `original_supplierID`e così via, è possibile lasciare il `OldValuesParameterFormatString` come valore della proprietà `original_{0}`. Se, tuttavia, i parametri di input dei metodi BLL avevano nomi come `old_productName`, `old_supplierID`e così via, è necessario aggiornare il `OldValuesParameterFormatString` proprietà `old_{0}`.


È un'impostazione della proprietà finale che deve essere impostato affinché ObjectDataSource correttamente passare i valori originali per i metodi BLL. ObjectDataSource ha un [proprietà ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) che può essere assegnato a [uno dei due valori](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -il valore predefinito. non invia i valori originali per i parametri di input originale dei metodi BLL
- `CompareAllValues` -Invia i valori originali per i metodi BLL; Scegliere questa opzione quando si utilizza la concorrenza ottimistica

È opportuno impostare il `ConflictDetection` proprietà `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Configurazione delle proprietà e campi di GridView

Con le proprietà di ObjectDataSource configurate correttamente, è possibile attivare l'attenzione per l'impostazione di GridView. In primo luogo, poiché si desidera GridView per supportare la modifica ed eliminazione, fare clic sulle caselle di controllo Abilita modifica e Abilita eliminazione smart tag del controllo GridView. Verrà aggiunta una CommandField cui `ShowEditButton` e `ShowDeleteButton` sono impostati entrambi su `true`.

Quando è associato al `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView contiene un campo per ognuno dei campi dati del prodotto. Mentre un controllo GridView. questo tipo può essere modificato, l'esperienza utente è qualsiasi valore diverso accettabile. Il `CategoryID` e `SupplierID` BoundField vengono visualizzati come caselle di testo, richiedere all'utente di immettere la categoria appropriata e fornitore come numeri ID. Non ci sarà alcuna formattazione per i campi numerici e non i controlli di convalida per verificare che il nome del prodotto è stato specificato e che il prezzo unitario unità in magazzino, unità, ordine e riordinare i valori del livello sono entrambi valori numerici appropriati e sono maggiori di o uguale a a zero.

Come accennato nel *aggiunta di controlli di convalida per le interfacce di inserimento e la modalità di modifica* e *personalizzazione dell'interfaccia di modifica di dati* esercitazioni, è possibile personalizzare l'interfaccia utente da sostituendo il BoundField con TemplateFields. Ho modificato questo GridView e la relativa modifica interfaccia nei modi seguenti:

- Rimuovere il `ProductID`, `SupplierName`, e `CategoryName` BoundField
- Convertire il `ProductName` BoundField in un TemplateField e aggiungere un controllo RequiredFieldValidation.
- Convertire il `CategoryID` e `SupplierID` BoundField per TemplateFields e regolare l'interfaccia di modifica per l'utilizzo di controlli DropDownList anziché nelle caselle di testo. In questi TemplateFields' `ItemTemplates`, `CategoryName` e `SupplierName` vengono visualizzati i campi dati.
- Convertire il `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` BoundField TemplateFields e controlli CompareValidator aggiunti.

Poiché sono già state esaminate come eseguire queste attività nelle esercitazioni precedenti, verrà semplicemente elencare qui la sintassi dichiarativa finale e lasciare l'implementazione come procedure consigliate.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Si è molto simile con un esempio funzionante completo. Tuttavia, esistono alcuni sfumature che saranno un ampliamento di e causare problemi a Microsoft. Inoltre, è comunque necessario un tipo di interfaccia che avvisa l'utente quando si è verificata una violazione di concorrenza.

> [!NOTE]
> Affinché un controllo Web di dati da passare correttamente i valori originali per ObjectDataSource (che vengono quindi passati al livello Business Logic), è fondamentale che il controllo GridView `EnableViewState` è impostata su `true` (impostazione predefinita). Se si disabilita lo stato di visualizzazione, i valori originali vengono persi nel postback.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Passare i valori originali corretti ObjectDataSource

Esistono un paio di problemi con la modalità in cui che è stato configurato il controllo GridView. Se di ObjectDataSource `ConflictDetection` è impostata su `CompareAllValues` (come avviene interna), quando il ObjectDataSource `Update()` o `Delete()` metodi vengono richiamati dal GridView (o DetailsView o FormView), ObjectDataSource tenta di copiare il I valori originali di GridView in relativi appropriato `Parameter` istanze. Fare riferimento alla figura 2 per una rappresentazione grafica del processo.

In particolare, i valori originali di GridView vengono assegnati i valori nelle istruzioni di associazione dati bidirezionale ogni volta che l'associazione dati a GridView. Pertanto, è essenziale che i valori originali necessari tutti vengono acquisiti mediante l'associazione dati bidirezionale e che sono fornite in un formato convertibile.

Per visualizzare perché questo è importante, richiedere qualche istante per visitare la pagina in un browser. Come previsto, GridView Elenca ogni prodotto con un pulsante di modifica e l'eliminazione della colonna più a sinistra.


[![Sono elencati i prodotti in un controllo GridView.](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Figura 14**: sono elencati i prodotti di in un controllo GridView ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image40.png))


Se si fa clic sul pulsante Elimina per qualsiasi prodotto, un `FormatException` viene generata un'eccezione.


[![Tentativo di eliminare i risultati di prodotto in FormatException](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Figura 15**: tentativo di eliminare qualsiasi prodotto risultati in un `FormatException` ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image43.png))


Il `FormatException` viene generato quando ObjectDataSource tenta di leggere nell'originale `UnitPrice` valore. Poiché il `ItemTemplate` ha il `UnitPrice` formattati come valuta (`<%# Bind("UnitPrice", "{0:C}") %>`), include un simbolo di valuta, ad esempio $19,95. Il `FormatException` si verifica come ObjectDataSource tenta di convertire la stringa in un `decimal`. Per aggirare questo problema, è disponibile una serie di opzioni:

- Rimuovere il formato di valuta il `ItemTemplate`. Vale a dire, anziché utilizzare `<%# Bind("UnitPrice", "{0:C}") %>`, è sufficiente usare `<%# Bind("UnitPrice") %>`. Lo svantaggio di questo è il prezzo non è formattato.
- Visualizzazione di `UnitPrice` formattati come valuta nel `ItemTemplate`, ma utilizzare il `Eval` (parola chiave) a questo scopo. Tenere presente che `Eval` esegue l'associazione unidirezionale. È necessario fornire il `UnitPrice` valore per i valori originali, quindi un'istruzione di associazione dati bidirezionale in sarà comunque necessario il `ItemTemplate`, ma ciò può essere inserito in un controllo etichetta Web il cui `Visible` è impostata su `false`. In ItemTemplate, è possibile usare il markup seguente:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Rimuovere il formato di valuta il `ItemTemplate`tramite `<%# Bind("UnitPrice") %>`. In GridView `RowDataBound` gestore dell'evento, a livello di codice di accesso Web etichetta controllo all'interno del quale il `UnitPrice` valore e impostare il relativo `Text` proprietà per la versione formattata.
- Lasciare il `UnitPrice` formattati come valuta. In GridView `RowDeleting` gestore dell'evento, sostituire l'originale esistente `UnitPrice` valore ($19,95) con un valore decimale effettivo `Decimal.Parse`. È stato illustrato come ottenere funzionalità simili di `RowUpdating` gestore dell'evento nel [ *BLL - gestione e le eccezioni DAL livello in una pagina ASP.NET* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) esercitazione.

Per questo esempio si è scelto di utilizzare con il secondo approccio, aggiunta di un Web etichetta nascosto controllo cui `Text` proprietà è bidirezionale dei dati associati per il non formattato `UnitPrice` valore.

Dopo aver risolto questo problema, provare a fare nuovamente clic sul pulsante Elimina per qualsiasi prodotto. Questa volta si otterrà un `InvalidOperationException` quando ObjectDataSource tenta di richiamare il livello Business LOGIC `UpdateProduct` metodo.


[![ObjectDataSource non è possibile trovare un metodo con i parametri di Input che si desidera inviare](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Figura 16**: ObjectDataSource non è possibile trovare un metodo con i parametri di Input che si desidera trasmissione ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image46.png))


Esaminando il messaggio dell'eccezione, risulta chiaro che desidera richiamare un BLL ObjectDataSource `DeleteProduct` metodo che include `original_CategoryName` e `original_SupplierName` i parametri di input. In questo modo il `ItemTemplate` s per il `CategoryID` e `SupplierID` TemplateFields attualmente contenere istruzioni di associazione bidirezionale con il `CategoryName` e `SupplierName` campi dati. In alternativa, è necessario includere `Bind` istruzioni con il `CategoryID` e `SupplierID` campi dati. A tale scopo, sostituire le istruzioni di associazione esistenti con `Eval` istruzioni, quindi aggiungere i controlli etichetta nascosta la cui `Text` proprietà sono associate le `CategoryID` e `SupplierID` campi di dati utilizzando l'associazione dati bidirezionale, come illustrato di seguito:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Con queste modifiche, ci sono in grado di eliminare e modificare le informazioni di prodotto. Nel passaggio 5 verrà esaminato come verificare che siano rilevate violazioni alla concorrenza. Ma per ora, alcuni minuti di tentativi di aggiornamento e l'eliminazione di alcuni record per verificare che l'aggiornamento ed eliminazione per un singolo utente funziona come previsto.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Passaggio 5: Test il supporto di concorrenza ottimistica

Per verificare eventuali violazioni alla concorrenza vengono rilevati (anziché risultante dati vengano sovrascritti ciecamente), è necessario aprire due finestre del browser a questa pagina. In entrambe le istanze del browser, fare clic sul pulsante Modifica per Chai. Uno soltanto dei browser, quindi modificare il nome "Chai tè" e fare clic su Aggiorna. L'aggiornamento deve avere esito positivo e ripristinare lo stato pre-modifica, con "Chai tè" come nuovo nome del prodotto GridView.

A altra istanza finestra del browser, tuttavia, il nome del prodotto casella di testo viene comunque "Chai". In questa seconda finestra del browser, aggiornare il `UnitPrice` a `25.00`. Senza il supporto della concorrenza ottimistica, facendo clic su Aggiorna nella seconda istanza del browser modificherebbe il nome del prodotto al "Chai", in tal modo sovrascrivendo le modifiche apportate dalla prima istanza del browser. Concorrenza ottimistica utilizzata, tuttavia, fare clic sul pulsante Aggiorna nella seconda istanza del browser di un [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Quando viene rilevata una violazione della concorrenza, viene generato un DBConcurrencyException](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Figura 17**: quando una violazione della concorrenza viene rilevato, un `DBConcurrencyException` viene generata un'eccezione ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image49.png))


Il `DBConcurrencyException` viene generata solo quando viene utilizzato il modello di aggiornamento del DAL batch. Il modello diretto DB non genera un'eccezione, indica semplicemente che nessuna riga è interessata. Per illustrare questo concetto, restituire GridView entrambe le istanze di browser allo stato di pre-modifica. Quindi, nella prima istanza del browser, fare clic sul pulsante di modifica e modificare il nome di prodotto da "Chai tè" a "Chai" e fare clic su Aggiorna. Nella seconda finestra del browser, fare clic sul pulsante Elimina per Chai.

Facendo clic su Elimina postback della pagina, GridView richiama ObjectDataSource `Delete()` metodo e ObjectDataSource chiama verso il basso il `ProductsOptimisticConcurrencyBLL` della classe `DeleteProduct` (metodo), passando i valori originali. Originale `ProductName` valore per la seconda istanza del browser è "Chai tè", che non corrisponde all'oggetto corrente `ProductName` valore nel database. Pertanto il `DELETE` zero righe influiscono su istruzione per il database, poiché non sono presenti record nel database che il `WHERE` soddisfa clausola. Il `DeleteProduct` restituisce `false` e i dati di ObjectDataSource sono riassociati a GridView.

Dal punto di vista dell'utente finale, facendo clic sul pulsante Elimina per tè Chai nella seconda finestra del browser ha causato la schermata flash e, al momento confermata, il prodotto è ancora presente, anche se a questo punto viene elencato come "Chai" (il prodotto nome modifica apportata da parte del browser prima istanza). Se l'utente fa clic sul pulsante Elimina nuovamente, l'eliminazione avrà esito positivo, il controllo GridView originale del `ProductName` valore ("Chai") corrisponde ora a con il valore nel database.

In entrambi i casi, l'esperienza utente è lontano dal ideale. Chiaramente non si desidera visualizzare i dettagli specifici relativi il `DBConcurrencyException` eccezione quando si usa il modello di aggiornamento batch. E il comportamento quando si usa il modello diretto DB è una certa confusione come comando gli utenti non riuscito, ma non vi sono indicazioni precise del motivo.

Per ovviare a questi due problemi, è possibile creare controlli etichetta Web della pagina che fornisce una spiegazione per il motivo per cui un aggiornamento o eliminazione non riuscita. Per il modello di aggiornamento batch, è possibile determinare o meno un `DBConcurrencyException` eccezione nel gestore dell'evento di post-livello di GridView, visualizzare l'etichetta di avviso in base alle esigenze. Per il metodo diretto database, è possibile esaminare il valore restituito del metodo BLL (ovvero `true` se una riga è stata interessata, `false` in caso contrario) e visualizzare un messaggio informativo in base alle esigenze.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Passaggio 6: Aggiunta di messaggi informativi e visualizzarle in caso di una violazione di concorrenza

Quando si verifica una violazione di concorrenza, comportamento anomalo varia a seconda se è stata utilizzata l'aggiornamento batch o un modello di diretto DB il campo DAL. L'esercitazione vengono utilizzati entrambi i modelli, con il modello di aggiornamento batch utilizzato per l'aggiornamento e il modello diretto DB utilizzato per l'eliminazione. Per iniziare, aggiungere due controlli etichetta Web alla pagina di cui viene illustrato che si è verificata una violazione di concorrenza durante il tentativo di eliminare o aggiornare i dati. Impostare il controllo di etichetta `Visible` e `EnableViewState` proprietà `false`; in questo modo tali da risultare nascoste su ogni visita pagina ad eccezione per quelli pagina particolare visita where loro `Visible` è impostata a livello di codice su `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

Oltre all'impostazione loro `Visible`, `EnabledViewState`, e `Text` proprietà, ho impostato anche il `CssClass` proprietà `Warning`, che comporta l'etichetta da visualizzare in un tipo di carattere di grandi dimensioni, rosso, corsivo e grassetto. Il CSS `Warning` classe è stata definita e aggiunto a Styles. CSS nuovamente il *esaminando gli eventi associati con l'inserimento, aggiornamento ed eliminazione* esercitazione.

Dopo aver aggiunto queste etichette, la finestra di progettazione in Visual Studio dovrebbe essere simile alla figura 18.


[![Due controlli etichetta sono stati aggiunti alla pagina](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Figura 18**: due etichetta controlli sono stati aggiunti alla pagina ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image52.png))


Con questi controlli etichetta Web sul posto, si è pronti per esaminare come determinare quando è verificata una violazione di concorrenza, in corrispondenza del quale l'etichetta appropriata del punto `Visible` può essere impostata su `true`, il messaggio informativo.

## <a name="handling-concurrency-violations-when-updating"></a>La gestione delle violazioni di concorrenza durante l'aggiornamento

Questa sezione descrive come gestire le violazioni di concorrenza quando si usa il modello di aggiornamento batch. Poiché tali violazioni con batch di aggiornamento causa modello un `DBConcurrencyException` eccezione, è necessario aggiungere codice alla pagina di ASP.NET per determinare se un `DBConcurrencyException` eccezione durante il processo di aggiornamento. Se in tal caso, si dovrebbe visualizzare un messaggio per l'utente che spiega che le modifiche non salvate perché un altro utente ha modificato gli stessi dati tra al momento dell'avvio modificando il record e quando si fa clic sul pulsante Aggiorna.

Come illustrato nel *BLL - gestione e le eccezioni DAL livello in una pagina ASP.NET* tutorial, tali eccezioni possono essere rilevati e soppressi nei gestori di eventi di post-livello del controllo dati Web. Pertanto, è necessario creare un gestore eventi per il controllo GridView `RowUpdated` evento che controlla se un `DBConcurrencyException` è stata generata l'eccezione. Questo gestore eventi viene passato un riferimento a qualsiasi eccezione generata durante il processo di aggiornamento, come illustrato nel caso in cui gestore code di seguito:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

Fronte di un `DBConcurrencyException` eccezione, questo gestore eventi visualizza le `UpdateConflictMessage` controllo etichetta e indica che è stata gestita l'eccezione. Con questo codice, quando si verifica una violazione della concorrenza quando si aggiorna un record, le modifiche dell'utente vengono perse, dal momento che verrebbe hanno sovrascritto le modifiche a un altro utente nello stesso momento. In particolare, GridView viene ripristinato lo stato di pre-modifica e associato ai dati del database corrente. La riga GridView verranno aggiornate con le altre modifiche dell'utente, che erano in precedenza non è visibile. Inoltre, il `UpdateConflictMessage` controllo etichetta verrà segnalare all'utente agli eventi. Questa sequenza di eventi è illustrata nella figura 19.


[![Un utente s gli aggiornamenti vengano persi durante la faccia di una violazione della concorrenza](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Figura 19**: utenti s gli aggiornamenti vengano persi durante la faccia di una violazione della concorrenza ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image55.png))


> [!NOTE]
> In alternativa, anziché restituire GridView per lo stato di pre-modifica, è possibile lasciare GridView nello stato di modificando impostando il `KeepInEditMode` proprietà passato `GridViewUpdatedEventArgs` oggetto su true. Se si adotta questo approccio, tuttavia, assicurarsi di riassociare i dati a GridView (richiamando il `DataBind()` (metodo)) in modo che i valori dell'utente vengono caricati nell'interfaccia di modifica. Il codice disponibile per il download in questa esercitazione dispone di queste due righe di codice il `RowUpdated` gestore dell'evento come commento; sufficiente rimuovere il commento le righe di codice di GridView rimangono in modalità di modifica dopo una violazione della concorrenza.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Risposta a eventuali violazioni alla concorrenza durante l'eliminazione

Con il modello diretto DB, non c'è alcuna eccezione generato in caso di una violazione della concorrenza. Al contrario, l'istruzione di database semplicemente influisce su alcun record, come la clausola WHERE non corrisponde a qualsiasi record. Tutti i metodi di modifica di dati creati con il livello Business LOGIC sono stati progettati in modo che restituiscano un valore booleano che indica se sono interessate precisamente un record. Pertanto, per determinare se si è verificata una violazione di concorrenza quando si elimina un record, è possibile esaminare il valore restituito del livello Business LOGIC `DeleteProduct` metodo.

Il valore restituito per un metodo BLL può essere esaminato in gestori di eventi post-livello di ObjectDataSource tramite il `ReturnValue` proprietà del `ObjectDataSourceStatusEventArgs` oggetto passato nel gestore eventi. Poiché si intende per determinare il valore restituito dal `DeleteProduct` (metodo), è necessario creare un gestore eventi per ObjectDataSource `Deleted` evento. Il `ReturnValue` proprietà è di tipo `object` e può essere `null` se è stata generata un'eccezione e il metodo è stata interrotta prima che potrebbe restituire un valore. Pertanto, è necessario verificare innanzitutto che il `ReturnValue` proprietà non è `null` e un valore booleano. Presupponendo passato il controllo, vengono dimostrati i `DeleteConflictMessage` controllo etichetta, se il `ReturnValue` è `false`. Questo può essere eseguito utilizzando il codice seguente:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

In caso di una violazione di concorrenza, viene annullata la richiesta di eliminazione dell'utente. GridView viene aggiornato, che mostra le modifiche che si sono verificati per tale record tra il momento in cui viene caricata la pagina e quando si fa clic sul pulsante Elimina. Quando tale violazione seguito, il `DeleteConflictMessage` viene visualizzata l'etichetta, che spiega agli eventi (vedere Figura 20).


[![Un utente s Delete viene annullato in caso di una violazione della concorrenza](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Figura 20**: utenti s Delete viene annullato in caso di una violazione della concorrenza ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-vb/_static/image58.png))


## <a name="summary"></a>Riepilogo

Opportunità per eventuali violazioni alla concorrenza esiste in ogni applicazione che consente a più utenti simultanei per aggiornare o eliminare dati. Se per, se due utenti ad aggiornare contemporaneamente gli stessi dati quando ci si accinge Ottiene l'ultima scrittura "WINS", sovrascrivendo l'altro utente modifica le modifiche non vengono presi in considerazione tali violazioni. In alternativa, gli sviluppatori possono implementare il controllo della concorrenza ottimistica o pessimistica. Controllo della concorrenza ottimistica presuppone che eventuali violazioni alla concorrenza sono frequenti semplicemente non consente un aggiornamento o eliminazione di comando che costituisce una violazione di concorrenza. Controllo della concorrenza pessimistica, si presuppone che la concorrenza violazioni sono frequenti e semplicemente il rifiuto di un utente di aggiornamento o eliminazione di comando non è accettabile. Con il controllo della concorrenza pessimistica, aggiornamento di un record implica il blocco, impedendo in tal modo qualsiasi altro utente di modificare o eliminare il record è bloccato.

Il set di dati tipizzati in .NET fornisce funzionalità per supportare il controllo della concorrenza ottimistica. In particolare, il `UPDATE` e `DELETE` istruzioni eseguite in database includono tutte le colonne della tabella, in modo da assicurare che l'aggiornamento o eliminazione viene eseguita solo se i dati del record corrente corrispondano con i dati originali utente avevano quando esecuzione delle loro aggiornamento o eliminazione. Una volta DAL è stato configurato per supportare la concorrenza ottimistica, i metodi BLL dovranno essere aggiornati. Inoltre, la pagina ASP.NET che richiamano il livello Business LOGIC configurare tale ObjectDataSource recupera i valori originali dal relativo controllo Web di dati e li passa verso il basso il livello Business LOGIC.

Come abbiamo visto in questa esercitazione, l'implementazione di controllo della concorrenza ottimistica in un'applicazione web ASP.NET implica l'aggiornamento DAL e BLL e aggiunta del supporto in una pagina ASP.NET. Indica se il lavoro aggiuntivo è un ottimo investimento del tempo e impegno varia a seconda dell'applicazione. Se hai raramente utenti simultanei, l'aggiornamento dei dati o i dati che viene aggiornata sono diversi uno da altro, quindi il controllo della concorrenza non è un aspetto fondamentale. Se, tuttavia, normalmente sono presenti più utenti del sito durante l'utilizzo con gli stessi dati, controllo della concorrenza può aiutare a evitare eliminazioni o aggiornamenti di un utente sovrascrivano involontariamente di altro.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](customizing-the-data-modification-interface-vb.md)
> [Successivo](adding-client-side-confirmation-when-deleting-vb.md)
