---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
title: Paging efficiente in grandi quantità di dati (c#) | Microsoft Docs
author: rick-anderson
description: L'opzione di paging predefinito di un controllo di presentazione dei dati è non adatto quando si lavora con grandi quantità di dati, come relativo retriev di controllo origine dati sottostante...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 59c01998-9326-4ecb-9392-cb9615962140
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
msc.type: authoredcontent
ms.openlocfilehash: a6d023f299d3c36e0b9f0d00f2b531d73657135c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367881"
---
<a name="efficiently-paging-through-large-amounts-of-data-c"></a>Paging efficiente in grandi quantità di dati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_CS.exe) o [Scarica il PDF](efficiently-paging-through-large-amounts-of-data-cs/_static/datatutorial25cs1.pdf)

> L'opzione di paging predefinito di un controllo di presentazione dei dati è non adatto quando si lavora con grandi quantità di dati, come il controllo origine dati sottostante recupera tutti i record, anche se viene visualizzato solo un subset di dati. In tali circostanze, è necessario attivare personalizzati a paging.


## <a name="introduction"></a>Introduzione

Come descritto nell'esercitazione precedente, il paging può essere implementato in uno dei due modi:

- **Il Paging predefinito** può essere implementata da semplicemente selezionando l'opzione attiva Paging nei dati di controllo del codice Web s smart tag; tuttavia, ogni volta che visualizzano una pagina di dati, ObjectDataSource recupera *tutto* dei record, anche solo un sottoinsieme di essi vengono visualizzati nella pagina
- **Il Paging personalizzato** migliora le prestazioni del valore predefinito di paging recuperando solo i record dal database che devono essere visualizzati per la pagina specifica di dati richiesti dall'utente; tuttavia, il paging personalizzato implica un po' più complessa da implementare rispetto del paging predefinito

Dovuta alla semplicità di implementazione solo verificare una casella di controllo e quale si è completata. il paging predefinito è un'opzione interessante. Il suo approccio ve na durante il recupero di tutti i record, tuttavia, rende una scelta improbabili quando paging in sufficientemente grandi quantità di dati o per i siti con molti utenti simultanei. In tali circostanze, è necessario attivare su personalizzato per fornire un sistema di velocità di risposta di paging.

La sfida di paging personalizzato è in grado di scrivere una query che restituisce il set di record necessari per una determinata pagina di dati preciso. Fortunatamente, Microsoft SQL Server 2005 fornisce una nuova parola chiave per i risultati di classificazione, che consente di scrivere una query che possa recuperare in modo efficiente il subset corretto di record. In questa esercitazione vedremo come usare questa nuova parola chiave SQL Server 2005 per implementare il paging personalizzato in un controllo GridView. Mentre l'interfaccia utente per il paging personalizzato è identico a quello per il paging predefinito, l'esecuzione di istruzioni da una pagina al successivo tramite il paging personalizzato può essere superiore alla velocità del paging predefinito vari ordini di grandezza.

> [!NOTE]
> Il miglioramento delle prestazioni esatta esibito dai paging personalizzato dipende dal numero totale di record in fase di paging attraverso e il carico sul server di database. Alla fine di questa esercitazione verrà esaminato alcune metriche generali che illustrano i vantaggi delle prestazioni ottenuto mediante il paging personalizzato.


## <a name="step-1-understanding-the-custom-paging-process"></a>Passaggio 1: Informazioni sul processo di Paging personalizzato

Quando il paging dei dati, i record precisi visualizzati in una pagina variano a seconda della pagina di dati richiesti e il numero di record visualizzati per pagina. Ad esempio, si supponga di voler eseguire il paging attraverso i 81 prodotti, visualizzazione dei 10 prodotti per ogni pagina. Quando si visualizzano la prima pagina, d vogliamo prodotti da 1 a 10. Quando si visualizzano la seconda pagina è il d interessarti prodotti 11 a 20 e così via.

Esistono tre variabili che determinano ciò che i record devono essere recuperate e modalità di rendering l'interfaccia di paging:

- **Indice di riga iniziale** l'indice della prima riga nella pagina di dati da visualizzare; questo può essere calcolato moltiplicando l'indice della pagina per i record da visualizzare per ogni pagina e aggiungerne uno. Ad esempio, quando il paging attraverso i 10 record alla volta, per la prima pagina (il cui indice della pagina è 0), l'indice di riga iniziale è 0 \* 10 + 1 o 1; per la seconda pagina (il cui indice della pagina è 1), l'indice di riga iniziale è 1 \* 10 + 1 , o 11.
- **Numero massimo di righe** il numero massimo di record da visualizzare per pagina. Questa variabile è detto numero massimo di righe poiché per gli ultimi presente pagina potrebbe essere un numero di record restituito rispetto alla dimensione di pagina. Ad esempio, quando lo scorrimento di record 81 prodotti 10 per ogni pagina, il nona e pagina finale avrà un solo record. Nessuna pagina, tuttavia, mostrerà record più lungo del valore massimo di righe.
- **Conteggio di Record totali** il numero totale di record in fase di paging tramite. Mentre t questa variabile non è necessario per determinare i record da recuperare per una determinata pagina, definiscono l'interfaccia di paging. Ad esempio, se sono presenti 81 prodotti Page tramite, l'interfaccia di paging SA per visualizzare i numeri di pagina nove nell'interfaccia di paging.

Con il paging predefinito, l'indice di riga iniziale viene calcolata come prodotto tra l'indice della pagina e le dimensioni della pagina più uno, mentre il numero massimo di righe è semplicemente la dimensione della pagina. Poiché il paging predefinito recupera tutti i record dal database durante il rendering di qualsiasi pagina di dati, l'indice per ogni riga è noto, in modo da ottenere lo spostamento alla riga di indice di riga avvia un'attività banale. Inoltre, il numero totale di Record è immediatamente disponibile, come s semplicemente il numero di record in DataTable (o qualsiasi oggetto utilizzato per contenere i risultati di database).

In base alle variabili indice di riga iniziale e massimo di righe, un'implementazione di paging personalizzata deve restituire solo il subset preciso di record che iniziano in corrispondenza dell'indice di riga avvia e fino al numero massimo di righe di record in seguito. Il paging personalizzato offre due sfide:

- Dobbiamo poter associare in modo efficiente di ogni riga in tutti i dati in modo che è possibile iniziare a restituire i record in corrispondenza dell'indice di riga specificato Start Page tramite un indice di riga
- È necessario fornire il numero totale di record in fase di paging tramite

I due passaggi successivi verrà esaminato lo script SQL necessario per rispondere a questi due problemi. Oltre a script SQL, è necessario implementare i metodi in DAL e BLL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Passaggio 2: Restituzione del numero totale di record in fase di paging tramite

Prima di esaminare come recuperare il subset di record per la pagina visualizzata preciso, consentire s prima di tutto esaminato come restituire il numero totale di record in fase di paging tramite. Queste informazioni sono necessarie per configurare correttamente l'interfaccia utente di paging. Il numero totale di record restituiti da una determinata query SQL può essere ottenuto utilizzando il [ `COUNT` funzione di aggregazione](https://msdn.microsoft.com/library/ms175997.aspx). Ad esempio, per determinare il numero totale di record nel `Products` tabella, è possibile usare la query seguente:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample1.sql)]

Let s aggiungere un metodo al nostro DAL che restituisce queste informazioni. In particolare, si creerà un metodo DAL chiamato `TotalNumberOfProducts()` che esegue il `SELECT` istruzione illustrato in precedenza.

Iniziare aprendo il `Northwind.xsd` file di set di dati tipizzati nelle `App_Code/DAL` cartella. Successivamente, fare clic su di `ProductsTableAdapter` nella finestra di progettazione e scegliere Aggiungi Query. Come abbiamo ve illustrata nelle esercitazioni precedenti, questo ci consentirà di aggiungere un nuovo metodo al livello dal che, quando richiamata, eseguirà una specifica istruzione SQL o stored procedure. Come con i metodi TableAdapter nelle esercitazioni precedenti, in questo caso scegliere di usare un'istruzione SQL ad hoc.


![Utilizzare un'istruzione SQL Ad Hoc](efficiently-paging-through-large-amounts-of-data-cs/_static/image1.png)

**Figura 1**: utilizzare un'istruzione SQL Ad Hoc


Nella schermata successiva è possibile specificare il tipo di query da creare. Poiché questa query restituirà un singolo valore scalare il numero totale di record nel `Products` tabella scegliere il `SELECT` che restituisce un'opzione di valore singolo.


![Configurare la Query per usare un'istruzione SELECT che restituisce un valore singolo](efficiently-paging-through-large-amounts-of-data-cs/_static/image2.png)

**Figura 2**: configurare la Query per usare un'istruzione SELECT che restituisce un valore singolo


Dopo che indica il tipo di query da usare, quindi dobbiamo specificare la query.


![Utilizzo di SELECT Count da Query dei prodotti](efficiently-paging-through-large-amounts-of-data-cs/_static/image3.png)

**Figura 3**: usare il numero di SELECT (\*) FROM Query dei prodotti


Infine, specificare il nome del metodo. Come menzionato in precedenza, "Let" s usare `TotalNumberOfProducts`.


![Denominare il TotalNumberOfProducts DAL metodo](efficiently-paging-through-large-amounts-of-data-cs/_static/image4.png)

**Figura 4**: assegnare un nome di TotalNumberOfProducts DAL metodo


Dopo aver fatto clic su Fine, la procedura guidata aggiungerà il `TotalNumberOfProducts` DAL metodo. I metodi che restituiscono scalari nel DAL restituiscono i tipi nullable, nel caso in cui il risultato della query SQL `NULL`. Nostri `COUNT` query, tuttavia, verrà sempre restituito non`NULL` valore; in ogni caso, il metodo DAL restituisce un integer nullable.

Oltre al metodo DAL, è necessario anche un metodo nel livello BLL. Aprire il `ProductsBLL` file di classe e aggiungere un `TotalNumberOfProducts` metodo che chiama semplicemente verso il basso per i dispositivi DAL `TotalNumberOfProducts` metodo:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample2.cs)]

S DAL `TotalNumberOfProducts` metodo restituisce un intero che ammette valori null; tuttavia, abbiamo ve creata le `ProductsBLL` classe s `TotalNumberOfProducts` metodo in modo che restituisca un numero intero standard. Pertanto, è necessario che il `ProductsBLL` classe s `TotalNumberOfProducts` metodo restituirà la parte valore dell'intero ammette valori null restituito da s DAL `TotalNumberOfProducts` (metodo). La chiamata a `GetValueOrDefault()` restituisce il valore dell'intero ammette valori null, se esistente; se l'integer nullable `null`, tuttavia, restituisce il valore intero predefinito, 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Passaggio 3: Restituzione preciso Subset di record

L'attività successiva consiste nel creare metodi di DAL e BLL che accettano l'indice di riga iniziale e le variabili di numero massimo di righe descritto in precedenza e restituiscono i record appropriati. Prima di procedere, ti permettono di s innanzitutto controllare lo script SQL necessario. La sfida che ci è che è necessario essere in grado di assegnare in modo efficace un indice per ogni riga in tutti i risultati in modo che possiamo restituire solo i record di avvio in corrispondenza dell'indice di riga avviare (e fino al numero di record di numero massimo di record), tramite il paging.

Questo non è un problema se esiste già una colonna nella tabella di database che funge da un indice di riga. A prima vista potrebbe essere Riteniamo che il `Products` tabella s `ProductID` campo basterebbe, come il primo prodotto ha `ProductID` pari a 1, 2, il secondo e così via. Tuttavia, l'eliminazione di un prodotto lascia un gap nella sequenza, annullando questo approccio.

Esistono due tecniche generali consente di associare in modo efficiente un indice di riga con i dati di spostarsi tra, abilitando in questo modo il subset di record da recuperare preciso:

- **Utilizzo di SQL Server 2005 s `ROW_NUMBER()` parola chiave** familiarità con SQL Server 2005, il `ROW_NUMBER()` parola chiave associa un valore di pertinenza di ogni record restituiti alcuni dati di ordinamento in base. Questa classificazione è utilizzabile come un indice di riga per ogni riga.
- **Usando una variabile di tabella e `SET ROWCOUNT`**  s SQL Server [ `SET ROWCOUNT` istruzione](https://msdn.microsoft.com/library/ms188774.aspx) può essere utilizzato per specificare il numero di record totale deve elaborare una query prima di terminare; [variabili di tabella](http://www.sqlteam.com/item.asp?ItemID=9454) sono variabili locali di T-SQL che possono contenere dati tabulari, akin a [tabelle temporanee](http://www.sqlteam.com/item.asp?ItemID=2029). Questo approccio funziona altrettanto bene con Microsoft SQL Server 2005 e SQL Server 2000 (mentre il `ROW_NUMBER()` approccio funziona solo con SQL Server 2005).  
  
  L'idea è creare una variabile di tabella con un `IDENTITY` colonna e le colonne per le chiavi primarie della tabella il paging attraverso cui i dati. Successivamente, il contenuto della tabella contenente i dati che il paging attraverso viene archiviato nella variabile di tabella, in tal modo associazione di un indice di riga sequenziale (tramite il `IDENTITY` colonna) per ogni record nella tabella. Una volta che la variabile di tabella è stata popolata, un `SELECT` istruzione nella variabile di tabella, unita alla tabella sottostante, può essere eseguito per estrarre i record specifici. Il `SET ROWCOUNT` istruzione viene utilizzata per limitare in modo intelligente il numero di record che dovranno essere sottoposti a dumping nella variabile di tabella.  
  
  Questa efficienza approccio s si basa sul numero di pagina richiesto, come il `SET ROWCOUNT` valore viene assegnato il valore di indice di riga iniziale più il numero massimo di righe. Quando il paging attraverso le pagine pari bassa, ad esempio il primo alcune pagine di dati, questo approccio è molto efficiente. Tuttavia, presenta prestazioni simile a paging predefinito durante il recupero di una pagina verso la fine.

Questa esercitazione l'implementazione personalizzata tramite paging le `ROW_NUMBER()` (parola chiave). Per altre informazioni sull'uso di variabile di tabella e `SET ROWCOUNT` tecnica, vedere [più efficiente metodo Paging attraverso grandi set di risultati](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

Il `ROW_NUMBER()` parola chiave associato un valore di pertinenza di ogni record restituito su un particolare ordinamento usando la sintassi seguente:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample3.sql)]

`ROW_NUMBER()` Restituisce un valore numerico che specifica l'ordine di priorità per ogni record per quanto riguarda l'ordine indicato. Ad esempio, per visualizzare il rango per ogni prodotto, ordinata dalla più costoso il minor, potremmo utilizziamo la query seguente:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample4.sql)]

Figura 5 viene illustrata questa query s si ottiene quando eseguito tramite la finestra di query in Visual Studio. Si noti che i prodotti vengono ordinati in base al prezzo, insieme a una classificazione di prezzo per ogni riga.


![È incluso il rango di prezzo per ogni Record restituito](efficiently-paging-through-large-amounts-of-data-cs/_static/image5.png)

**Figura 5**: è incluso il rango di prezzo per ogni Record restituito


> [!NOTE]
> `ROW_NUMBER()` è solo una delle molte nuove funzioni di rango disponibili in SQL Server 2005. Per una discussione più approfondita della `ROW_NUMBER()`, con le altre funzioni di rango, leggere [restituzione di risultati con classificazione più con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Quando i risultati di classificazione per l'oggetto specificato `ORDER BY` colonna il `OVER` clausola (`UnitPrice`, nell'esempio precedente), è necessario ordinare i risultati in SQL Server. Questa è un'operazione rapida se è presente un indice cluster tramite le colonne vengono ordinati i risultati da, o se è presente una copertura di indice, ma può essere più costoso in caso contrario. Per contribuire al miglioramento delle prestazioni per query di dimensioni sufficientemente grandi, è consigliabile aggiungere un indice non cluster per la colonna mediante il quale i risultati vengono ordinati in base. Visualizzare [funzioni di rango e le prestazioni di SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) per esaminare le considerazioni sulle prestazioni più dettagliate.

Le informazioni di classificazione restituite da `ROW_NUMBER()` non può essere utilizzata direttamente il `WHERE` clausola. Tuttavia, una tabella derivata può essere utilizzata per restituire il `ROW_NUMBER()` risultato, che può quindi essere visualizzati nei `WHERE` clausola. Ad esempio, la query seguente utilizza una tabella derivata per restituire le colonne di UnitPrice e ProductName, insieme al `ROW_NUMBER()` risultati e quindi Usa un `WHERE` clausola per restituire solo i prodotti il cui rango prezzo è compreso tra 11 e 20:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample5.sql)]

Estendere un po' ulteriormente questo concetto, che possiamo utilizzare questo approccio per recuperare una pagina specifica di dati in base ai valori di indice di riga iniziale e massimo di righe desiderati:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample6.html)]

> [!NOTE]
> Come vedremo più avanti in questa esercitazione, il *`StartRowIndex`* forniti da ObjectDataSource verrà indicizzati a partire da zero, mentre il `ROW_NUMBER()` valore restituito da SQL Server 2005 verrà indicizzati a partire da 1. Pertanto, il `WHERE` la clausola restituisce i record in cui `PriceRank` è rigorosamente maggiore *`StartRowIndex`* e minore o uguale a *`StartRowIndex`*  +  *`MaximumRows`*.


Ora che abbiamo illustrato come fornire un supporto iniziale `ROW_NUMBER()` può essere usata per recuperare una determinata pagina di dati in base ai valori di indice di riga iniziale e massimo di righe, è ora necessario implementare questa logica sotto forma di metodi di DAL e BLL.

Durante la creazione di questa query che è necessario decidere l'ordine con cui i risultati verranno classificati; consentire s ordinare i prodotti in base al nome in ordine alfabetico. Ciò significa che con l'implementazione di paging personalizzata in questa esercitazione non sarà in grado di creare un report impaginato personalizzati che possono anche essere ordinati. Nella prossima esercitazione, tuttavia, si vedrà come è possibile specificare tale funzionalità.

Nella sezione precedente abbiamo creato il metodo DAL come un'istruzione SQL ad hoc. Sfortunatamente, il parser di T-SQL in Visual Studio utilizzata dal TableAdapter guidata l t, ad esempio la `OVER` sintassi utilizzata per la `ROW_NUMBER()` (funzione). Pertanto, è necessario creare questo metodo DAL come stored procedure. Selezionare Esplora Server dal menu Visualizza (o hit Ctrl + Alt + S) ed espandere il `NORTHWND.MDF` nodo. Per aggiungere una nuova stored procedure, fare doppio clic sul nodo di Stored procedure e fare clic su Aggiungi una nuova Stored Procedure (vedere la figura 6).


![Aggiungere una nuova Stored Procedure per il Paging attraverso i prodotti](efficiently-paging-through-large-amounts-of-data-cs/_static/image6.png)

**Figura 6**: aggiungere una nuova Stored Procedure per il Paging attraverso i prodotti


Questa stored procedure deve accettare due parametri di input di tipo integer - `@startRowIndex` e `@maximumRows` e utilizzare il `ROW_NUMBER()` funzione ordinati in base il `ProductName` campo, che restituisce solo le righe maggiore rispetto al `@startRowIndex` e minore di o uguale a `@startRowIndex`  +  `@maximumRow` s. Immettere lo script seguente nella nuova stored procedure e quindi fare clic sull'icona di salvataggio per aggiungere la stored procedure nel database.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample7.sql)]

Dopo aver creato la stored procedure, è opportuno testarlo. Fare clic su di `GetProductsPaged` stored procedure assegnare un nome in Esplora Server e scegliere l'opzione Execute. Visual Studio verrà quindi chiesto di parametri di input `@startRowIndex` e `@maximumRow` s (vedere la figura 7). Provare diversi valori ed esaminare i risultati.


![Immettere un valore per il @startRowIndex e @maximumRows parametri](efficiently-paging-through-large-amounts-of-data-cs/_static/image7.png)

<strong>Figura 7</strong>: immettere un valore per il @startRowIndex e @maximumRows parametri


Dopo aver scegliendo questi valori di parametri di input, la finestra di Output visualizzerà i risultati. Figura 8 mostra i risultati quando si passano 10, sia per il `@startRowIndex` e `@maximumRows` parametri.


[![Il record che apparirà nella seconda pagina di dati restituiti](efficiently-paging-through-large-amounts-of-data-cs/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image8.png)

**Figura 8**: il record che apparirà nella seconda pagina di dati vengono restituiti ([fare clic per visualizzare l'immagine con dimensioni normali](efficiently-paging-through-large-amounts-of-data-cs/_static/image10.png))


Con questa stored procedure creata, sono pronti per creare il `ProductsTableAdapter` (metodo). Aprire il `Northwind.xsd` DataSet tipizzato, pulsante destro del mouse nel `ProductsTableAdapter`e scegliere l'opzione Aggiungi Query. Invece di creare la query utilizzando un'istruzione SQL ad hoc, crearlo utilizzando una stored procedure esistente.


![Creare il metodo DAL utilizzando una Stored Procedure esistente](efficiently-paging-through-large-amounts-of-data-cs/_static/image11.png)

**Figura 9**: creare il metodo DAL utilizzando una Stored Procedure esistente


Successivamente, si verrà richiesto di selezionare la stored procedure per richiamare. Selezionare il `GetProductsPaged` stored procedure nell'elenco a discesa.


![Scegliere il GetProductsPaged Stored Procedure nell'elenco a discesa](efficiently-paging-through-large-amounts-of-data-cs/_static/image12.png)

**Figura 10**: scegliere la GetProductsPaged Stored Procedure nell'elenco a discesa


Nella schermata successiva viene richiesto il tipo di dati viene restituito dalla stored procedure: dati tabulari, un singolo valore o nessun valore. Poiché il `GetProductsPaged` stored procedure può restituire più record, indicano che vengono restituiti i dati tabulari.


![Indicare che la Stored Procedure restituisce dati tabulari](efficiently-paging-through-large-amounts-of-data-cs/_static/image13.png)

**Figura 11**: indica che la Stored Procedure restituisce dati tabulari


Infine, indicare i nomi dei metodi che si desidera avere creato. Come con le esercitazioni precedenti, proseguire e creare un oggetto DataTable usando il riempimento di entrambi i metodi e Restituisci un DataTable. Denominare il primo metodo `FillPaged` e il secondo `GetProductsPaged`.


![Nome FillPaged i metodi e GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image14.png)

**Figura 12**: nome FillPaged i metodi e GetProductsPaged


Inoltre creare un metodo per restituire una pagina particolare dei prodotti, è anche necessario fornire tale funzionalità nel livello BLL. Analogamente al metodo DAL, s BLL GetProductsPaged metodo deve accettare due input di integer per specificare l'indice di riga iniziale e massimo di righe e deve restituire solo i record che è compreso nell'intervallo specificato. Creare questo tipo di metodo BLL nella classe ProductsBLL che semplicemente le chiamate verso il basso in oggetti DAL metodo GetProductsPaged, come segue:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample8.cs)]

È possibile usare qualsiasi nome per i parametri di input BLL metodo s, ma, come vedremo tra breve, scegliere di usare `startRowIndex` e `maximumRows` ci risparmia di un ulteriore bit di lavoro quando si configura un ObjectDataSource per usare questo metodo.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Passaggio 4: Configurazione di ObjectDataSource per usare il Paging personalizzato

Con i metodi per l'accesso a un determinato subset di record completo, a livello BLL e DAL pronti per creare un controllo GridView controlliamo che scorre il record sottostante mediante il paging personalizzato. Iniziare aprendo il `EfficientPaging.aspx` nella pagina di `PagingAndSorting` cartella, aggiungere un controllo GridView alla pagina e configurarlo per usare un nuovo controllo ObjectDataSource. Nelle esercitazioni precedenti, abbiamo spesso ObjectDataSource configurato per usare la `ProductsBLL` classe s `GetProducts` (metodo). Questa volta, tuttavia, si vuole usare il `GetProductsPaged` (metodo), invece, poiché il `GetProducts` restituzione del metodo *tutte* dei prodotti nel database mentre `GetProductsPaged` restituisce solo un subset di record specifico.


![Configurare ObjectDataSource per usare il metodo GetProductsPaged ProductsBLL classe s](efficiently-paging-through-large-amounts-of-data-cs/_static/image15.png)

**Figura 13**: configurare ObjectDataSource per usare il metodo GetProductsPaged ProductsBLL classe s


Poiché abbiamo nuovamente la creazione di un controllo GridView di sola lettura, si consiglia di impostare l'elenco di riepilogo a discesa metodo nell'istruzione INSERT, UPDATE ed eliminare schede su (nessuno).

Successivamente, la procedura guidata ObjectDataSource richiede per le origini del `GetProductsPaged` metodo s `startRowIndex` e `maximumRows` i valori dei parametri di input. Questi parametri di input vengono effettivamente impostati da GridView automaticamente, pertanto è sufficiente lasciare il set di origine su None e fare clic su Fine.


![Lasciare le origini di parametro di Input come nessuno](efficiently-paging-through-large-amounts-of-data-cs/_static/image16.png)

**Figura 14**: lasciare le origini di parametro di Input come nessuno


Dopo aver completato la procedura guidata ObjectDataSource, GridView conterrà un BoundField o CampoCasellaDiControllo per ognuno dei campi di dati del prodotto. È possibile personalizzare l'aspetto GridView nel modo desiderato. Ho scelto di fornire un supporto iniziale per visualizzare solo le `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, e `UnitPrice` BoundField. Inoltre, configurare il controllo GridView per supportare il paging selezionando la casella di controllo Attiva Paging nel suo smart tag. Dopo tali modifiche, il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample9.aspx)]

Se si visita la pagina tramite un browser, tuttavia, il controllo GridView non è alcun dove da trovare.


![Il controllo GridView non viene visualizzato](efficiently-paging-through-large-amounts-of-data-cs/_static/image17.png)

**Figura 15**: GridView non viene visualizzato


Manca il controllo GridView ObjectDataSource è attualmente in uso 0 come valori per entrambe le `GetProductsPaged` `startRowIndex` e `maximumRows` parametri di input. Di conseguenza, la query SQL risultante non restituisce Nessun record e pertanto non viene visualizzato il controllo GridView.

Per risolvere questo problema, dobbiamo configurare ObjectDataSource per usare il paging personalizzato. A questo scopo nella procedura seguente:

1. **Impostare gli oggetti ObjectDataSource `EnablePaging` proprietà `true`**  indica a ObjectDataSource che è necessario passare il `SelectMethod` due parametri aggiuntivi: consente di specificare l'indice di riga iniziale ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) e uno per specificare il numero massimo di righe ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Impostare gli oggetti ObjectDataSource `StartRowIndexParameterName` e `MaximumRowsParameterName` proprietà conseguenza** la `StartRowIndexParameterName` e `MaximumRowsParameterName` proprietà indicano i nomi dei parametri di input passati il `SelectMethod` per scopi di paging personalizzato. Per impostazione predefinita, questi nomi di parametro vengono `startIndexRow` e `maximumRows`, per tale motivo, quando si crea il `GetProductsPaged` metodo nel livello BLL, ho utilizzato questi valori per i parametri di input. Se si sceglie di usare nomi di parametro diversi per s BLL `GetProductsPaged` metodo come `startIndex` e `maxRows`, ad esempio, è necessario imposta la s ObjectDataSource `StartRowIndexParameterName` e `MaximumRowsParameterName` proprietà conseguenza (ad esempio startIndex per `StartRowIndexParameterName` maxRows per e `MaximumRowsParameterName`).
3. **Impostare la s ObjectDataSource [ `SelectCountMethod` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) sul nome del metodo che restituisce il totale numero di record in fase di paging tramite (`TotalNumberOfProducts`)** si tenga presente che il `ProductsBLL` classe s `TotalNumberOfProducts`metodo restituisce il numero totale di record in fase di paging tramite usando un metodo DAL che esegue un `SELECT COUNT(*) FROM Products` query. Queste informazioni sono necessarie da ObjectDataSource per il rendering corretto l'interfaccia di paging.
4. **Rimuovere il `startRowIndex` e `maximumRows` `<asp:Parameter>` elementi da ObjectDataSource s Markup dichiarativo** quando si configura ObjectDataSource mediante la procedura guidata, Visual Studio ha aggiunto automaticamente due `<asp:Parameter>` elementi per il `GetProductsPaged` metodo s parametri di input. Impostando `EnablePaging` al `true`, questi parametri verranno passati automaticamente; se appaiono anche nella sintassi dichiarativa, ObjectDataSource tenterà passare *quattro* parametri per il `GetProductsPaged` (metodo) e due parametri per il `TotalNumberOfProducts` (metodo). Se si dimentica di rimuovere questi `<asp:Parameter>` elementi, quando si visita la pagina tramite un browser si riceverà un messaggio di errore, ad esempio: *ObjectDataSource 'ObjectDataSource1' non è stato possibile trovare un metodo non generico 'TotalNumberOfProducts' con i parametri: startRowIndex maximumRows*.

Dopo aver apportato queste modifiche, la sintassi dichiarativa s ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample10.aspx)]

Si noti che il `EnablePaging` e `SelectCountMethod` sono state impostate le proprietà e il `<asp:Parameter>` gli elementi sono stati rimossi. Figura 16 mostra una cattura di schermata della finestra delle proprietà dopo aver apportate queste modifiche.


![Per usare il Paging personalizzato, configurare il controllo ObjectDataSource](efficiently-paging-through-large-amounts-of-data-cs/_static/image18.png)

**Figura 16**: per usare il Paging personalizzato, configurare il controllo ObjectDataSource


Dopo aver apportato queste modifiche, visita questa pagina tramite un browser. Dovrebbe essere 10 prodotti nell'elenco, ordinato in ordine alfabetico. Si consiglia di esaminare i dati una pagina alla volta. Anche se non vi è alcuna differenza visual dalla prospettiva degli utenti finali s tra il paging predefinito e il paging personalizzato, in modo più efficiente il paging personalizzato Sfoglia le pagine di grandi quantità di dati come recuperare solo i record che devono essere visualizzati per una determinata pagina.


[![I dati, ordinato in base al prodotto, nome, è di paging con Paging personalizzato](efficiently-paging-through-large-amounts-of-data-cs/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image19.png)

**Figura 17**: la Data, ordinato in base al prodotto, nome è di paging con Paging personalizzato ([fare clic per visualizzare l'immagine con dimensioni normali](efficiently-paging-through-large-amounts-of-data-cs/_static/image21.png))


> [!NOTE]
> Con paging personalizzato, la pagina contare valore restituito da ObjectDataSource s `SelectCountMethod` viene archiviato nello stato di visualizzazione GridView s. Altre variabili di GridView i `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` insieme e così via vengono archiviate nel *lo stato del controllo*, che viene mantenuto indipendentemente dal valore di istanze della classe GridView `EnableViewState` proprietà. Poiché il `PageCount` valore viene mantenuto durante i postback con stato di visualizzazione, quando si usa un'interfaccia di paging che include un collegamento che consente di accedere all'ultima pagina, è fondamentale che lo stato di visualizzazione GridView s sia attivato. (Se l'interfaccia di paging non include un collegamento diretto all'ultima pagina, quindi è possibile disabilitare lo stato di visualizzazione)


Scegliendo il collegamento alla pagina ultima causa un postback e istruisce il controllo GridView per aggiornare il `PageIndex` proprietà. Se si seleziona l'ultimo collegamento di pagina, il controllo GridView assegna relativi `PageIndex` proprietà su un valore pari a 1 minore relativo `PageCount` proprietà. Lo stato di visualizzazione disabilitato, il `PageCount` valore viene persa durante i postback e `PageIndex` viene assegnato il valore intero massimo invece. Successivamente, il controllo GridView tenta di determinare l'indice di riga iniziale moltiplicando il `PageSize` e `PageCount` proprietà. Ciò comporta un `OverflowException` poiché il prodotto supera le dimensioni del numero intero massimo consentito.

## <a name="implement-custom-paging-and-sorting"></a>Implementare il Paging personalizzato e l'ordinamento

L'implementazione di paging personalizzato corrente richiede che l'ordine con cui i dati viene eseguito il paging attraverso sia specificato in modo statico durante la creazione di `GetProductsPaged` stored procedure. Tuttavia, si potrebbe sia preso nota prima che lo smart tag s di GridView contiene una casella di controllo Abilita ordinamento oltre l'opzione attiva Paging. Sfortunatamente, aggiungendo il supporto dell'ordinamento per il controllo GridView con l'implementazione di paging personalizzato corrente solo ordinare i record nella pagina visualizzata dei dati. Ad esempio, se si configura il controllo GridView per supportare anche il paging e quindi, quando si visualizzano la prima pagina di dati, ordinare in base al nome di prodotto in ordine decrescente, invertirà l'ordine dei prodotti nella pagina 1. Come illustrato nella figura 18, ad esempio Carnarvon Leoni Mostra come il primo prodotto durante l'ordinamento in ordine alfabetico inverso, che ignora i 71 altri prodotti che seguono il termine Leoni Carnarvon, in ordine alfabetico; il tipo di ordinamento vengono considerati solo i record nella prima pagina.


[![Solo i dati visualizzati nella pagina corrente è ordinato](efficiently-paging-through-large-amounts-of-data-cs/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image22.png)

**Figura 18**: solo i dati visualizzati nella pagina corrente è ordinato ([fare clic per visualizzare l'immagine con dimensioni normali](efficiently-paging-through-large-amounts-of-data-cs/_static/image24.png))


L'ordinamento si applica solo alla pagina corrente dei dati perché l'ordinamento avviene dopo i dati sono stati recuperati da BLL s `GetProductsPaged` metodo e questo metodo restituisce solo i record per la pagina specifica. Per implementare l'ordinamento in modo corretto, è necessario passare l'espressione di ordinamento per il `GetProductsPaged` metodo in modo che i dati è possibile classificare in modo appropriato prima di restituire la pagina di dati specifica. Si vedrà come eseguire questa operazione nell'esercitazione successiva.

## <a name="implementing-custom-paging-and-deleting"></a>Implementazione personalizzata di Paging e l'eliminazione

Se l'abilitazione della funzionalità di eliminazione in GridView i cui dati viene eseguito il paging utilizzando tecniche di paging personalizzato si noterà che quando si elimina l'ultimo record dall'ultima pagina, il controllo GridView viene rimosso anziché in modo appropriato il decremento s GridView `PageIndex`. Per riprodurre il bug, abilitare l'eliminazione per l'esercitazione che semplicemente abbiamo appena creato. Passare all'ultima pagina (pagina 9), in cui verrà visualizzato un unico prodotto poiché si sta paging dei 81 prodotti, 10 prodotti alla volta. Eliminare questo prodotto.

In seguito all'eliminazione del prodotto ultimo, il controllo GridView *dovrebbero* automaticamente andare alla pagina ottava e tale funzionalità varierà con paging predefinito. Con paging personalizzato, tuttavia, dopo l'eliminazione di tale prodotto ultimo nell'ultima pagina, il controllo GridView semplicemente scomparirà dallo schermo completamente. Il motivo esatto *perché* in questo caso è di tipo bit esula dall'ambito di questa esercitazione, vedere [eliminazione l'ultimo Record nell'ultima pagina da un controllo GridView con Paging personalizzato](http://scottonwriting.net/sowblog/posts/7326.aspx) per i dettagli di basso livello per l'origine di Questo problema. In sintesi, s a causa della sequenza di passaggi che vengono eseguiti da GridView quando si fa clic sul pulsante Elimina seguente:

1. Eliminare il record
2. Ottenere il record appropriato da visualizzare per l'oggetto specificato `PageIndex` e `PageSize`
3. Verificare che il `PageIndex` non superi il numero di pagine di dati nell'origine dati; se, riduce automaticamente la s GridView `PageIndex` proprietà
4. Associare la pagina appropriata di dati a GridView mediante i record ottenuti nel passaggio 2

Il problema deriva dal fatto che nel passaggio 2 le `PageIndex` usato quando si acquisisce i record da visualizzare è ancora il `PageIndex` dell'ultima pagina il cui unico record semplicemente è stato eliminato. Pertanto, nel passaggio 2 *alcun* record restituiti dall'ultima pagina di dati non contiene alcun record. Quindi, nel passaggio 3, il controllo GridView si rende conto che relativi `PageIndex` proprietà è maggiore del numero totale di pagine nell'origine dati (poiché è ve eliminato l'ultimo record nell'ultima pagina) e pertanto decrementa relativo `PageIndex` proprietà. Nel passaggio 4 GridView tenta di associarsi ai dati recuperati nel passaggio 2. Tuttavia, nel passaggio 2 non sono stati restituiti record, generando un GridView vuoto. Con il paging predefinito, problema l t area poiché nel passaggio 2 *tutti* i record vengono recuperati dall'origine dati.

Per risolvere questo problema sono disponibili due opzioni. La prima consiste nel creare un gestore eventi per s GridView `RowDeleted` gestore dell'evento che determina il numero di record sono stato visualizzato nella pagina semplicemente eliminato. Se si è verificato un solo record, quindi avere ricevuto il record eliminato solo quello più recente ed è necessario diminuire la s GridView `PageIndex`. Naturalmente, si desidera solo aggiornare il `PageIndex` se l'operazione di eliminazione è effettivamente riuscita, che è possibile determinare, garantendo che il `e.Exception` è di proprietà `null`.

Questo approccio funziona perché aggiorna il `PageIndex` dopo il passaggio 1 ma prima del passaggio 2. Pertanto, nel passaggio 2, viene restituito il set appropriato di record. A tale scopo, usare codice simile al seguente:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample11.cs)]

Una soluzione alternativa consiste nel creare un gestore eventi per gli oggetti ObjectDataSource `RowDeleted` eventi e impostare il `AffectedRows` proprietà su un valore pari a 1. Dopo aver eliminato il record nel passaggio 1 (ma prima di recuperare nuovamente i dati nel passaggio 2), aggiorna il GridView relativo `PageIndex` proprietà se una o più righe interessate dall'operazione. Tuttavia, il `AffectedRows` proprietà non è impostata da ObjectDataSource e pertanto si omette questo passaggio. Per disporre di questo passaggio eseguito è possibile impostare manualmente il `AffectedRows` proprietà se l'operazione di eliminazione viene completata correttamente. Ciò può essere eseguita usando codice simile al seguente:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample12.cs)]

Il codice per entrambi questi gestori eventi sono reperibili nella classe code-behind del `EfficientPaging.aspx` esempio.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Confronto tra le prestazioni dell'impostazione predefinita e il Paging personalizzato

Poiché il paging personalizzato recupera solo i record necessari, mentre il paging predefinito restituisce *tutti* del record per ogni pagina che viene visualizzato, è s cancellare che il paging personalizzato è più efficiente il paging predefinito. Tuttavia, solo la modalità è molto più efficiente è il paging personalizzato? Il tipo dei miglioramenti delle prestazioni può essere osservato dal passaggio da paging predefinito per il paging personalizzato?

Sfortunatamente, 3!s ci Nessuna dimensione adatta a tutte le risposte di seguito. Il miglioramento delle prestazioni dipende da numerosi fattori, in maggiore evidenza due che corrisponde al numero di record in fase di paging attraverso e del carico posizionato sui canali database server e la comunicazione tra il server web e server di database. Per le tabelle di piccole dimensioni con poche decine record, la differenza nelle prestazioni può essere ignorabile. Per le tabelle di grandi dimensioni, con migliaia a centinaia di migliaia di righe, tuttavia, la differenza nelle prestazioni è acuta.

Un articolo, mio [Paging personalizzato in ASP.NET 2.0 con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), contiene alcuni test è stato eseguito per presentare le differenze nelle prestazioni tra queste due tecniche di spostamento quando il paging attraverso una tabella di database con 50.000 record. In questi test ho esaminato il sia il tempo per eseguire la query a livello di SQL Server (utilizzando [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) e di pagina ASP.NET utilizzando [le funzionalità di traccia di ASP.NET s](https://msdn.microsoft.com/library/y13fw6we.aspx). Tenere presente che questi test sono stati eseguiti nella mia finestra di sviluppo con un singolo utente attivo e pertanto sono poco e non possono simulare modelli di carico tipico sito Web. Indipendentemente da ciò, i risultati illustrano le differenze relative nel tempo di esecuzione per impostazione predefinita e il paging personalizzato quando si lavora con sufficientemente grandi quantità di dati.


|  | **Durata media Durata (sec)** | **Operazioni di lettura** |
| --- | --- | --- |
| **Paging Profiler SQL predefinito** | 1.411 | 383 |
| **Personalizzato Paging SQL Profiler** | 0.002 | 29 |
| **Traccia di ASP.NET il Paging predefinito** | 2.379 | *N/D* |
| **Traccia di ASP.NET il Paging personalizzato** | 0.029 | *N/D* |


Come può notare, il recupero di una determinata pagina di dati necessarie in Media 354 meno operazioni di lettura e completati in una frazione del tempo. Nella pagina ASP.NET personalizzato la pagina è stata in grado di eseguire il rendering nel prossimo a 1/100<sup>th</sup> del tempo necessario per l'utilizzo del paging predefinito. Visualizzare [il mio articolo](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) per altre informazioni su questi risultati insieme a un database e il codice è possibile scaricare per riprodurre questi test nel proprio ambiente.

## <a name="summary"></a>Riepilogo

Il paging predefinito è una passeggiata implementare controllo semplicemente la casella di controllo Attiva Paging in dati Web controllo s smart tag, ma tale semplicità comporta il costo delle prestazioni. Con il paging predefinito, quando un utente richiede una qualsiasi pagina di dati *tutti* vengono restituiti record, anche se potrebbe essere visualizzata solo una piccola frazione di essi. Per evitare questo sovraccarico delle prestazioni, ObjectDataSource offre un'alternativa paging personalizzato opzione di paging.

Anche se il paging personalizzato rappresenta un miglioramento paging i problemi di prestazioni s recuperando solo i record che devono essere visualizzati, impostazione predefinita è s più complesse da implementare il paging personalizzato. Prima di tutto necessario è possibile scrivere una query che in modo corretto (e in modo efficiente) accede il subset specifico dei record richiesti. Ciò può essere eseguita in svariati modi; quello che abbiamo esaminati in questa esercitazione consiste nell'usare SQL Server 2005 s nuovo `ROW_NUMBER()` funzione per classificare i risultati e quindi per restituire solo i risultati il cui rango è compreso nell'intervallo specificato. Inoltre, è necessario aggiungere un metodo per determinare il numero totale di record in fase di paging tramite. Dopo aver creato questi metodi DAL e BLL, è necessario anche configurare ObjectDataSource in modo che possa determinare il numero totale di record sono il paging attraverso e passare correttamente i valori di indice di riga iniziale e massimo di righe per il livello BLL.

Anche se implementare il paging personalizzato richiedono un numero di passaggi ed è non altrettanto semplice quanto il paging predefinito, il paging personalizzato è una necessità durante lo scorrimento sufficientemente grandi quantità di dati. Come esaminare i risultati delle pagine ha dimostrato, personalizzato può fare secondi all'esterno di tempo di rendering della pagina ASP.NET e possibile alleggerire il carico sul server di database per uno o più ordini di grandezza.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](paging-and-sorting-report-data-cs.md)
> [Successivo](sorting-custom-paged-data-cs.md)
