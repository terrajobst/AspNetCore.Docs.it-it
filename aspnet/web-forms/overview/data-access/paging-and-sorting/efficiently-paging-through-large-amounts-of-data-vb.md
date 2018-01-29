---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: "In modo efficiente il Paging grandi quantità di dati (VB) | Documenti Microsoft"
author: rick-anderson
description: "L'opzione di paging predefinita di un controllo di presentazione di dati non è adatta quando si lavora con grandi quantità di dati, come il retriev di controllo origine dati sottostante..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 9a1b7fbb1e60c9f1bc6a26ccaeb7d14b4c95219d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="efficiently-paging-through-large-amounts-of-data-vb"></a>In modo efficiente il Paging grandi quantità di dati (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe) o [Scarica il PDF](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> L'opzione di paging predefinita di un controllo di presentazione di dati non è adatta quando si lavora con grandi quantità di dati, come il controllo origine dati sottostante recupera tutti i record, anche se viene visualizzato solo un subset di dati. In tali circostanze, è necessario attivare personalizzati di paging.


## <a name="introduction"></a>Introduzione

Come descritto nell'esercitazione precedente, è possibile implementare il paging in uno dei due modi:

- **Paging predefinito** può essere implementata controllando semplicemente l'opzione Abilita Paging nei dati di controllo Web s smart tag; tuttavia, ogni volta che si visualizza una pagina di dati, ObjectDataSource recupera *tutti* dei record, anche anche se solo un sottoinsieme di essi vengono visualizzati nella pagina
- **Il Paging personalizzato** migliora le prestazioni delle predefinito paging recuperando solo i record dal database che devono essere visualizzate per la particolare pagina di dati richiesto dall'utente; tuttavia, il paging personalizzato comporta un maggiore impegno per implementare del paging predefinito

Grazie alla facilità di controllo solo implementazione una casella di controllo e si stanno completata. paging predefinito è un'opzione attraente. Proprio approccio ve na durante il recupero di tutti i record, tuttavia, rende una scelta inverosimile quando il paging sufficientemente grandi quantità di dati o per i siti con molti utenti simultanei. In tali circostanze, è necessario attivare su personalizzato per fornire un sistema di risposta di paging.

La necessità di paging personalizzato è in grado di scrivere una query che restituisce il set di record necessari per una particolare pagina di dati preciso. Fortunatamente, Microsoft SQL Server 2005 fornisce una nuova parola chiave per i risultati di classificazione, che consente di scrivere una query che è possibile recuperare in modo efficiente il subset corretto di record. In questa esercitazione vedremo come utilizzare la nuova parola chiave SQL Server 2005 per implementare il paging personalizzato in un controllo GridView. Mentre l'interfaccia utente per il paging personalizzato è identico a quello per il paging predefinito, l'esecuzione di istruzioni da una pagina al successivo tramite il paging personalizzato può essere più veloce di paging predefinito diversi ordini di grandezza.

> [!NOTE]
> Il miglioramento delle prestazioni esatta esibito dal paging personalizzato dipende dal numero totale di record il paging tramite e il carico sul server di database. Alla fine di questa esercitazione verranno ora esaminate alcune metriche approssimativa che illustrano i vantaggi nelle prestazioni ottenuti mediante il paging personalizzato.


## <a name="step-1-understanding-the-custom-paging-process"></a>Passaggio 1: Comprendere il processo di Paging personalizzata

Quando il paging dei dati, i record precisi visualizzati in una pagina variano a seconda della pagina di dati richiesti e il numero di record visualizzati per pagina. Ad esempio, si supponga di voler di spostarsi tra i 81 prodotti, 10 prodotti per ogni pagina di visualizzazione. Quando si visualizza la prima pagina, d vogliamo prodotti 1 e 10. Quando si visualizza la seconda pagina è d esserti 11 20 tramite prodotti e così via.

Esistono tre variabili che indicano quali record devono essere recuperati e la modalità di rendering dell'interfaccia di paging:

- **Indice di riga iniziale** l'indice della prima riga nella pagina di dati da visualizzare; indice può essere calcolato moltiplicando l'indice della pagina per i record da visualizzare per ogni pagina e l'aggiunta di uno. Ad esempio, quando il paging i 10 record alla volta, per la prima pagina (il cui indice della pagina è 0), indice di riga iniziale è 0 \* 10 + 1 o 1, per la seconda pagina (il cui indice della pagina è 1), indice di riga iniziale è 1 \* 10 + 1 , o 11.
- **Numero massimo di righe** il numero massimo di record da visualizzare per pagina. Questa variabile viene considerata numero massimo di righe perché per l'ultima pagina può essere un numero di record restituito rispetto alle dimensioni di pagina. Ad esempio, quando lo scorrimento di record di 81 prodotti 10 per ogni pagina, la pagina nona e finale avrà un solo record. Nessuna pagina, tuttavia, verrà visualizzati più record rispetto al valore massimo di righe.
- **Totale Record** il numero totale di record il paging tramite. Mentre t questa variabile non è necessario per determinare i record da recuperare per una determinata pagina, definiscono l'interfaccia di paging. Ad esempio, se sono presenti 81 prodotti il paging tramite, l'interfaccia di paging in grado di visualizzare i numeri di pagina nove nell'interfaccia di paging.

Con il paging predefinito, indice di riga iniziale viene calcolata come prodotto tra l'indice della pagina e le dimensioni della pagina più uno, mentre il numero massimo di righe è semplicemente le dimensioni della pagina. Poiché paging predefinito recupera tutti i record dal database durante il rendering di qualsiasi pagina di dati, l'indice per ogni riga è noto, rendendo lo spostamento alla riga di indice di riga iniziale un'attività banale. Inoltre, il numero totale di Record è immediatamente disponibile, come s semplicemente il numero di record di DataTable (o qualsiasi oggetto viene utilizzato per archiviare i risultati di database).

Dato le variabili di indice di riga iniziale e massimo di righe, un'implementazione personalizzata di paging deve restituire solo il subset di record a partire dall'indice di riga iniziale e fino a raggiungere il numero massimo di righe di record dopo che preciso. Il paging personalizzato sono disponibili due sfide:

- È necessario essere in grado di associare in modo efficiente un indice di riga a ogni riga in tutti i dati in modo che è possibile iniziare a restituire i record in corrispondenza dell'indice di riga specificato Start, tramite il paging
- È necessario fornire il numero totale di record il paging tramite

I due passaggi successivi esamina lo script SQL necessario per rispondere a questi due problemi. Oltre agli script SQL, è inoltre necessario implementare i metodi nel DAL e BLL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Passaggio 2: Restituzione del numero totale di record il paging tramite

Prima di esaminare come recuperare il subset di record per la pagina visualizzata preciso, consentire s descrive come restituire il numero totale di record il paging tramite. Queste informazioni sono necessarie per configurare correttamente l'interfaccia utente di paging. Il numero totale di record restituiti da una particolare query SQL può essere ottenuto utilizzando il [ `COUNT` funzione di aggregazione](https://msdn.microsoft.com/library/ms175997.aspx). Ad esempio, per determinare il numero totale di record di `Products` tabella, è possibile utilizzare la query seguente:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

Consente di aggiungere un metodo per il nostro DAL che restituisce le informazioni s. In particolare, si creerà un metodo DAL chiamato `TotalNumberOfProducts()` che esegue il `SELECT` istruzione illustrato in precedenza.

Aprire il `Northwind.xsd` file DataSet tipizzato nel `App_Code/DAL` cartella. Successivamente, fare clic su di `ProductsTableAdapter` nella finestra di progettazione e scegliere Aggiungi Query. Come si ve illustrata nelle esercitazioni precedenti, questo permetterà di aggiungere un nuovo metodo DAL che, quando richiamata, verrà eseguita una particolare istruzione SQL o stored procedure. Come con i metodi TableAdapter esercitazioni precedenti, in questo caso decidere di usare un'istruzione SQL ad hoc.


![Utilizzare un'istruzione SQL Ad Hoc](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**Figura 1**: utilizzare un'istruzione SQL Ad Hoc


Nella schermata successiva è possibile specificare il tipo di query da creare. Poiché la query restituirà un singolo valore scalare il numero totale di record di `Products` tabella scegliere la `SELECT` che restituisce un'opzione singola.


![Configurare la Query per l'utilizzo di un'istruzione SELECT che restituisce un valore singolo](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**Figura 2**: configurare la Query per l'utilizzo di un'istruzione SELECT che restituisce un valore singolo


Dopo che indica il tipo di query da utilizzare, è quindi necessario specificare la query.


![Utilizzo di SELECT Count da Query di prodotti](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**Figura 3**: utilizza il conteggio Seleziona (\*) Query prodotti da


Infine, specificare il nome del metodo. Come s menzionati in precedenza, consentono di utilizzare `TotalNumberOfProducts`.


![Nome di TotalNumberOfProducts DAL metodo](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**Figura 4**: nome di TotalNumberOfProducts DAL metodo


Dopo aver fatto clic su Fine, la procedura guidata aggiungerà il `TotalNumberOfProducts` DAL metodo. I metodi di restituzione scalari DAL restituiscono tipi nullable, nel caso in cui il risultato della query SQL `NULL`. Il nostro `COUNT` query, tuttavia, restituirà sempre un non -`NULL` valore; in ogni caso, il metodo DAL restituisce un intero che ammette valori null.

Oltre al metodo DAL, è necessario anche un metodo in BLL. Aprire il `ProductsBLL` file di classe e aggiungere un `TotalNumberOfProducts` metodo che chiama semplicemente verso il basso per i dispositivi DAL `TotalNumberOfProducts` metodo:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

S DAL `TotalNumberOfProducts` il metodo restituisce un intero che ammette valori null; tuttavia, si viene creato il `ProductsBLL` classe s `TotalNumberOfProducts` metodo in modo che restituisca un numero intero standard. Pertanto, è necessario che il `ProductsBLL` classe s `TotalNumberOfProducts` il metodo restituisce la parte del valore dell'intero ammette valori null restituito da dispositivi DAL `TotalNumberOfProducts` metodo. La chiamata a `GetValueOrDefault()` restituisce il valore dell'intero ammette valori null, se esistente; se l'integer nullable `null`, tuttavia, restituisce il valore intero predefinito, 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Passaggio 3: Restituzione preciso Subset di record

L'attività successiva consiste nel creare metodi nel DAL e BLL che accettano l'indice di riga iniziale e variabili di numero massimo di righe descritti in precedenza e restituiscono i record appropriati. Prima di procedere è s consentono di esaminare innanzitutto lo script SQL necessario. La sfida che ci è che è necessario essere in grado di assegnare in modo efficiente un indice per ogni riga in tutti i risultati in modo che è possibile restituire solo i record a partire dall'indice di riga iniziale (e fino al numero di record massimo di record), tramite il paging.

Questo non è una richiesta di verifica se è già presente una colonna nella tabella di database che funge da un indice di riga. A prima vista si potrebbe pensare che il `Products` tabella s `ProductID` campo basterebbe, come prima del prodotto ha `ProductID` pari a 1, 2, il secondo e così via. Tuttavia, l'eliminazione di un prodotto lascia un gap nella sequenza, annullando questo approccio.

Sono disponibili due tecniche generali consente di associare in modo efficiente un indice di riga con i dati per il paging, consentendo il sottoinsieme preciso di record da recuperare:

- **Utilizzo di SQL Server 2005 s `ROW_NUMBER()` (parola chiave)** nuova per SQL Server 2005, il `ROW_NUMBER()` parola chiave associa un valore di pertinenza di ogni record restituito basato sull'ordinamento di alcuni. Questa classificazione è utilizzabile come un indice di riga per ogni riga.
- **Utilizzando una variabile di tabella e `SET ROWCOUNT`**  s di SQL Server [ `SET ROWCOUNT` istruzione](https://msdn.microsoft.com/library/ms188774.aspx) può essere usato per specificare il numero totale di record deve elaborare una query prima della chiusura; [le variabili di tabella](http://www.sqlteam.com/item.asp?ItemID=9454) sono variabili locali di T-SQL che possono contenere dati tabulari, akin a [tabelle temporanee](http://www.sqlteam.com/item.asp?ItemID=2029). Questo approccio funziona anche con Microsoft SQL Server 2005 e SQL Server 2000 (mentre il `ROW_NUMBER()` approccio funziona solo con SQL Server 2005).  
  
 L'idea è creare una variabile di tabella con un `IDENTITY` colonna e le colonne per le chiavi primarie della tabella, il paging tramite cui i dati. Successivamente, il contenuto della tabella i cui dati il paging tramite viene archiviato nella variabile di tabella, in tal modo associazione di un indice di riga sequenziali (tramite il `IDENTITY` colonna) per ogni record nella tabella. Dopo aver compilata la variabile di tabella, una `SELECT` istruzione nella variabile di tabella, unito alla tabella sottostante, è possibile eseguire per estrarre i record specifici. Il `SET ROWCOUNT` istruzione viene utilizzata per limitare in modo intelligente il numero di record che devono essere copiate in una variabile di tabella.  
  
 Questa efficienza approccio s è in base al numero di pagina richiesto, come il `SET ROWCOUNT` valore viene assegnato il valore di indice di riga iniziale più il numero massimo di righe. Durante la suddivisione tra le pagine numerate basso, ad esempio il primo alcune pagine di dati in questo approccio è molto efficiente. Tuttavia, produce prestazioni simili a paging predefinito durante il recupero di una pagina alla fine.

Questa esercitazione l'implementazione personalizzata tramite paging le `ROW_NUMBER()` (parola chiave). Per ulteriori informazioni sull'utilizzo della variabile di tabella e `SET ROWCOUNT` tecnica, vedere [più efficiente metodo per il Paging tramite grandi set di risultati](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

Il `ROW_NUMBER()` (parola chiave) associato un valore di pertinenza di ogni record restituito su un particolare ordinamento utilizzando la sintassi seguente:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()`Restituisce un valore numerico che specifica l'ordine di priorità per ogni record per quanto riguarda l'ordine indicato. Ad esempio, per visualizzare il rango per ogni prodotto, ordinata dalle più costosa minimi, è possibile usare la query seguente seguente:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

Figura 5 Mostra questa query risultati s quando eseguito tramite la finestra di query in Visual Studio. Si noti che i prodotti vengono ordinati in base al prezzo, insieme a una classificazione di prezzo per ogni riga.


![È incluso il rango di prezzo per ogni Record restituito](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**Figura 5**: è incluso il rango di prezzo per ogni Record restituito


> [!NOTE]
> `ROW_NUMBER()`è solo una delle molte nuove funzioni di rango disponibili in SQL Server 2005. Per una descrizione più dettagliata di `ROW_NUMBER()`, insieme alle altre funzioni di rango, leggere [restituzione di risultati classificati con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Quando i risultati di classificazione da specificato `ORDER BY` colonna il `OVER` clausola (`UnitPrice`, nell'esempio precedente), è necessario ordinare i risultati in SQL Server. Questa è un'operazione rapida se è un indice cluster per le colonne vengono ordinati i risultati da, o se esiste una copertura di indice, ma può essere più costosi da risolvere in caso contrario. Per migliorare le prestazioni per le query sufficientemente grande, considerare l'aggiunta di un indice non cluster per la colonna da cui i risultati vengono ordinati in base. Vedere [funzioni di rango e le prestazioni in SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) per un'analisi più approfondita delle considerazioni sulle prestazioni.

Le informazioni di classificazione restituite da `ROW_NUMBER()` non può essere utilizzata direttamente il `WHERE` clausola. Tuttavia, una tabella derivata può essere utilizzata per restituire il `ROW_NUMBER()` risultato, che può quindi essere visualizzati nel `WHERE` clausola. Ad esempio, la query seguente utilizza una tabella derivata per restituire le colonne ProductName e UnitPrice, insieme al `ROW_NUMBER()` risultato e quindi viene utilizzato un `WHERE` clausola per restituire solo i prodotti di priorità il cui prezzo è compreso tra 11 e 20:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

Estensione di un bit ulteriormente questo concetto, che possiamo utilizzare questo approccio per recuperare una pagina specifica di dati in base ai valori di indice di riga iniziale e massimo di righe desiderati:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> Come verrà illustrato più avanti in questa esercitazione, il  *`StartRowIndex`*  fornito da ObjectDataSource è indicizzato a partire da zero, mentre il `ROW_NUMBER()` valore restituito da SQL Server 2005 viene indicizzato a partire da 1. Pertanto, il `WHERE` clausola restituisce i record in cui `PriceRank` è rigorosamente maggiore  *`StartRowIndex`*  e minore o uguale a  *`StartRowIndex`*   +  *`MaximumRows`*.


Ora che vi viene descritto come `ROW_NUMBER()` può essere utilizzato per recuperare una particolare pagina di dati in base ai valori di indice di riga iniziale e massimo di righe, è ora necessario implementare la logica come metodi in DAL e BLL.

Durante la creazione di questa query che è necessario decidere l'ordine con cui i risultati verranno classificati; Consente di ordinare i prodotti in base al nome in ordine alfabetico s. Ciò significa che con l'implementazione di paging personalizzata in questa esercitazione è non sarà in grado di creare un report impaginato personalizzati che possono anche essere ordinati. Nella prossima esercitazione, tuttavia, si vedrà come tale funzionalità può essere fornita.

Nella sezione precedente abbiamo creato il metodo DAL come un'istruzione SQL ad hoc. Sfortunatamente, il parser di T-SQL in Visual Studio utilizzato da t guidata TableAdapter come il `OVER` sintassi utilizzata per il `ROW_NUMBER()` (funzione). Pertanto, è necessario creare questo metodo DAL come stored procedure. Selezionare Esplora Server dal menu Visualizza (o hit Ctrl + Alt + S) ed espandere il `NORTHWND.MDF` nodo. Per aggiungere una nuova stored procedure, fare clic sul nodo Stored procedure e scegliere Aggiungi nuova Stored Procedure (vedere Figura 6).


![Aggiungere una nuova Stored Procedure per i prodotti di Paging](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**Figura 6**: aggiungere una nuova Stored Procedure per i prodotti di Paging


Questa stored procedure deve accettare due parametri di input di interi - `@startRowIndex` e `@maximumRows` e utilizzare il `ROW_NUMBER()` funzione ordinati di `ProductName` campo, restituendo soltanto le righe maggiore rispetto al `@startRowIndex` e minore di o uguale a `@startRowIndex`  +  `@maximumRow` s. Immettere il seguente script in nuova stored procedure e quindi fare clic sull'icona Salva per aggiungere la stored procedure nel database.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

Dopo aver creato la stored procedure, richiedere qualche istante per testarlo. Fare clic su di `GetProductsPaged` stored procedure, assegnare un nome in Esplora Server e scegliere l'opzione di esecuzione. Visual Studio verrà quindi richiesto per i parametri di input, `@startRowIndex` e `@maximumRow` s (vedere la figura 7). Provare diversi valori ed esaminare i risultati.


![Immettere un valore per il @startRowIndex e @maximumRows parametri](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

**Figura 7**: immettere un valore per il @startRowIndex e @maximumRows parametri


Dopo la scelta di questi valori di parametri di input, la finestra di Output verrà visualizzati i risultati. Figura 8 mostra i risultati per il passaggio 10 per entrambi i `@startRowIndex` e `@maximumRows` parametri.


[![Il record che verrebbero visualizzati nella seconda pagina di dati vengono restituiti.](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**Figura 8**: il record che verrebbero visualizzati nella seconda pagina di dati vengono restituiti ([fare clic per visualizzare l'immagine ingrandita](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))


Con questa stored procedure creata, è nuovamente pronto per creare il `ProductsTableAdapter` metodo. Aprire il `Northwind.xsd` DataSet tipizzato, mouse, la `ProductsTableAdapter`e scegliere l'opzione Aggiungi Query. Invece di creare la query utilizzando un'istruzione SQL ad hoc, crearlo utilizzando una stored procedure esistente.


![Creare il metodo DAL utilizzando una Stored Procedure esistente](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**Figura 9**: creare il metodo DAL utilizzando una Stored Procedure esistente


Successivamente, si viene richiesto di selezionare la stored procedure da richiamare. Selezionare il `GetProductsPaged` stored procedure dall'elenco a discesa.


![Scegliere il GetProductsPaged Stored Procedure dall'elenco a discesa](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**Figura 10**: scegliere il GetProductsPaged Stored Procedure dall'elenco a discesa


Nella schermata successiva viene richiesto il tipo di dati viene restituito dalla stored procedure: dati tabulari, un singolo valore o nessun valore. Poiché il `GetProductsPaged` stored procedure può restituire più record, indicano che vengono restituiti i dati tabulari.


![Indicare che la Stored Procedure restituisce dati in formato tabulare](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**Figura 11**: indicare che la Stored Procedure restituisce dati in formato tabulare


Infine, indicare i nomi dei metodi che si desidera avere creato. Come con le esercitazioni precedenti, proseguo e crea un oggetto DataTable di metodi usando il riempimento di entrambi e Restituisci un DataTable. Denominare il primo metodo `FillPaged` e il secondo `GetProductsPaged`.


![Nome FillPaged i metodi e GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**Figura 12**: nome FillPaged i metodi e GetProductsPaged


È inoltre creare un metodo per restituire una pagina specifica di prodotti, è anche necessario fornire la stessa funzionalità in BLL. Analogamente al metodo DAL, s BLL GetProductsPaged metodo deve accettare due input di tipo integer per specificare l'indice di riga iniziale e il numero massimo di righe e deve restituire solo i record che rientrano nell'intervallo specificato. Creare un metodo di BLL nella classe ProductsBLL che semplicemente chiamate verso il basso in oggetti DAL metodo GetProductsPaged, ad esempio:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

È possibile utilizzare qualsiasi nome per i parametri di input BLL metodo s, ma, come verrà illustrato più avanti, la scelta di utilizzare `startRowIndex` e `maximumRows` ci consente di risparmiare da un'ulteriore bit di lavoro quando si configura un ObjectDataSource per utilizzare questo metodo.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Passaggio 4: Configurazione di ObjectDataSource per utilizzare il Paging personalizzato

Con i metodi BLL e per l'accesso a un determinato subset di record completo, è nuovamente pronto per creare un controllo GridView. controllare che scorre il record sottostante mediante il paging personalizzato. Aprire il `EfficientPaging.aspx` nella pagina di `PagingAndSorting` cartella, aggiungere un controllo GridView alla pagina e configurarlo per l'utilizzo di un nuovo controllo ObjectDataSource. Nelle esercitazioni precedenti, abbiamo spesso ObjectDataSource configurato per l'utilizzo di `ProductsBLL` classe s `GetProducts` metodo. Questa volta, tuttavia, si desidera utilizzare il `GetProductsPaged` (metodo), invece, dopo il `GetProducts` restituisce *tutti* dei prodotti nel database mentre `GetProductsPaged` restituisce solo un subset specifico di record.


![Configurare ObjectDataSource per utilizzare il metodo di classe ProductsBLL s GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**Figura 13**: configurare ObjectDataSource per utilizzare il metodo di classe ProductsBLL s GetProductsPaged


Poiché è nuovamente la creazione di un controllo GridView di sola lettura, dedicare alcuni minuti per impostare l'elenco di riepilogo a discesa metodo nell'istruzione INSERT, UPDATE ed eliminare le schede su (nessuno).

Successivamente, la procedura guidata ObjectDataSource richiede Microsoft per le origini del `GetProductsPaged` metodo s `startRowIndex` e `maximumRows` valori di parametri di input. Questi parametri di input vengono effettivamente impostati da GridView automaticamente, lasciare l'origine non sufficiente e fare clic su Fine.


![Lasciare le origini di parametro di Input come nessuno](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**Nella figura 14**: lasciare le origini di parametro di Input come nessuno


Dopo aver completato la procedura guidata ObjectDataSource, GridView conterrà un BoundField o CheckBoxField per ognuno dei campi dati di prodotto. È possibile personalizzare l'aspetto di GridView s secondo necessità. Si va scelto per visualizzare solo il `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, e `UnitPrice` BoundField. Inoltre, configurare GridView per supportare il paging selezionando la casella di controllo Abilita Paging smart tag. Dopo aver apportato queste modifiche, il markup dichiarativo GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

Se si visita la pagina tramite un browser, tuttavia, GridView non è alcun elemento da cui deve essere trovato.


![GridView non è visualizzato](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**Figura 15**: il GridView non è visualizzato


GridView non è disponibile perché ObjectDataSource è attualmente in uso 0 come valori per entrambe le `GetProductsPaged` `startRowIndex` e `maximumRows` i parametri di input. Di conseguenza, la query SQL risultante non restituisce Nessun record e pertanto non viene visualizzato il controllo GridView.

Per risolvere questo problema, è necessario configurare ObjectDataSource per utilizzare il paging personalizzato. Questo può essere eseguito nei passaggi seguenti:

1. **Impostare il s ObjectDataSource `EnablePaging` proprietà `true`**  indica a ObjectDataSource che è necessario passare il `SelectMethod` due parametri aggiuntivi: uno per specificare l'indice di riga iniziale ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) e uno per specificare il numero massimo di righe ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Impostare s ObjectDataSource `StartRowIndexParameterName` e `MaximumRowsParameterName` proprietà conseguenza** il `StartRowIndexParameterName` e `MaximumRowsParameterName` proprietà indicano i nomi dei parametri di input passati il `SelectMethod` per scopi di paging personalizzata . Per impostazione predefinita, questi nomi di parametro sono `startIndexRow` e `maximumRows`, per questo motivo, quando si crea il `GetProductsPaged` metodo in BLL, si usa questi valori per i parametri di input. Se si sceglie di utilizzare nomi di parametro diversi per s BLL `GetProductsPaged` metodo, ad esempio `startIndex` e `maxRows`, ad esempio, è necessario imposta il s ObjectDataSource `StartRowIndexParameterName` e `MaximumRowsParameterName` proprietà conseguenza (ad esempio startIndex per `StartRowIndexParameterName` e maxRows per `MaximumRowsParameterName`).
3. **Impostare il s ObjectDataSource [ `SelectCountMethod` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) sul nome del metodo che restituisce il totale numero di record in corso di paging tramite (`TotalNumberOfProducts`)** tenere presente che la `ProductsBLL` classe s `TotalNumberOfProducts`il metodo restituisce il numero totale di record il paging tramite un metodo DAL che viene eseguito un `SELECT COUNT(*) FROM Products` query. Queste informazioni sono necessarie da ObjectDataSource per correttamente il rendering dell'interfaccia di paging.
4. **Rimuovere il `startRowIndex` e `maximumRows` `<asp:Parameter>` elementi da ObjectDataSource s Markup dichiarativo** quando si configura ObjectDataSource mediante la procedura guidata, Visual Studio automaticamente aggiunto due `<asp:Parameter>` gli elementi per il `GetProductsPaged` metodo s parametri di input. Impostando `EnablePaging` a `true`, questi parametri verranno passati automaticamente; se sono visualizzate anche nella sintassi dichiarativa, un tentativo di passare ObjectDataSource *quattro* parametri per il `GetProductsPaged` (metodo) e due parametri per il `TotalNumberOfProducts` metodo. Se si dimentica di rimuoverli `<asp:Parameter>` elementi, durante la visita la pagina tramite un browser, si otterrà un messaggio di errore: *ObjectDataSource 'ObjectDataSource1' Impossibile trovare un metodo non generico 'TotalNumberOfProducts' con parametri: startRowIndex, maximumRows*.

Dopo aver apportato queste modifiche, la sintassi dichiarativa s ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

Si noti che il `EnablePaging` e `SelectCountMethod` sono state impostate e `<asp:Parameter>` sono stati rimossi elementi. La figura 16 mostra una cattura di schermata della finestra Proprietà dopo avere apportate queste modifiche.


![Per utilizzare il Paging personalizzato, configurare il controllo ObjectDataSource](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**Figura 16**: per utilizzare il Paging personalizzato, configurare il controllo ObjectDataSource


Dopo aver apportato queste modifiche, visitare la pagina tramite un browser. Dovrebbe essere 10 prodotti nell'elenco, ordinato in ordine alfabetico. È opportuno esaminare i dati una pagina alla volta. Anche se non vi è alcuna differenza visual dal punto di vista s dell'utente finale tra paging predefinito e il paging personalizzato, in modo più efficiente il paging personalizzato pagine grandi quantità di dati recupera solo i record che devono essere visualizzati per una determinata pagina.


[![I dati, ordinato in base al prodotto, nome, è di paging utilizzando Paging personalizzato](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**Figura 17**: dei dati, ordinato in base al prodotto, nome, è di paging utilizzando Paging personalizzato ([fare clic per visualizzare l'immagine ingrandita](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))


> [!NOTE]
> Con il paging personalizzato, la pagina conteggio valore restituito da ObjectDataSource s `SelectCountMethod` viene archiviato nello stato di visualizzazione s GridView. Altre variabili GridView il `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` insieme e così via vengono archiviati in *lo stato del controllo*, che viene mantenuto indipendentemente dal valore di GridView s `EnableViewState` proprietà. Poiché il `PageCount` valore viene mantenuto durante i postback utilizzando lo stato di visualizzazione, quando si utilizza un'interfaccia di paging che include un collegamento per accedere all'ultima pagina, è necessario che lo stato di visualizzazione GridView s essere abilitato. (Se l'interfaccia di paging non è incluso un collegamento diretto all'ultima pagina, quindi è possibile disabilitare lo stato di visualizzazione)


Facendo clic sull'ultimo collegamento di pagina provoca un postback e indica al controllo GridView. per aggiornare il relativo `PageIndex` proprietà. Se si fa clic sull'ultimo collegamento di pagina, GridView assegna relativo `PageIndex` proprietà su un valore minore di relativo `PageCount` proprietà. Lo stato di visualizzazione è disabilitato, il `PageCount` valore viene persa durante i postback e `PageIndex` viene assegnato il valore integer massimo invece. Successivamente, GridView tenta di determinare l'indice di riga iniziale moltiplicando il `PageSize` e `PageCount` proprietà. Ciò comporta un `OverflowException` poiché il prodotto supera le dimensioni di numero intero massimo consentito.

## <a name="implement-custom-paging-and-sorting"></a>Il Paging personalizzato di implementare e ordinamento

L'implementazione personalizzata di paging corrente richiede che l'ordine con cui i dati viene eseguito il paging tramite venga specificato in modo statico durante la creazione di `GetProductsPaged` stored procedure. Tuttavia, si potrebbe hanno evidenziato che lo smart tag s di GridView contiene una casella di controllo Abilita ordinamento oltre all'opzione Abilita Paging. Purtroppo, aggiungere il supporto dell'ordinamento GridView con l'implementazione personalizzata di paging corrente solo ordinare i record nella pagina visualizzata dei dati. Ad esempio, se si configura il controllo GridView per supportare anche il paging e quindi, quando si visualizzano la prima pagina di dati, ordinare in base al nome di prodotto in ordine decrescente, verrà invertire l'ordine dei prodotti nella pagina 1. Come mostrato nella figura 18, ad esempio Leoni Carnarvon Mostra come prima del prodotto durante l'ordinamento in ordine alfabetico inverso, che ignora i 71 altri prodotti che seguono in ordine alfabetico; Leoni Carnarvon, il tipo di ordinamento vengono considerati solo i record nella prima pagina.


[![Solo i dati visualizzati nella pagina corrente è ordinato.](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**Figura 18**: solo i dati visualizzati nella pagina corrente viene ordinato ([fare clic per visualizzare l'immagine ingrandita](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))


L'ordinamento si applica solo alla pagina corrente dei dati perché l'ordinamento avviene dopo i dati sono stati recuperati da s BLL `GetProductsPaged` metodo e questo metodo restituisce solo i record per la pagina specifica. Per implementare l'ordinamento in modo corretto, è necessario passare l'espressione di ordinamento per il `GetProductsPaged` metodo in modo che i dati è possibile classificare in modo appropriato prima di restituire la pagina di dati specifica. Vedremo come effettuare questa operazione nell'esercitazione successiva.

## <a name="implementing-custom-paging-and-deleting"></a>Implementazione personalizzata di Paging e l'eliminazione

Se è la funzionalità di eliminazione in un controllo GridView viene eseguito il paging i cui dati utilizzando le tecniche di paging personalizzata, si noterà che durante l'eliminazione dell'ultimo record dall'ultima pagina di attivazione, GridView scompare anziché in modo appropriato il decremento GridView s `PageIndex`. Per riprodurre il bug, consente di eliminare per l'esercitazione che appena appena creato. Passare all'ultima pagina (pagina 9), in cui dovrebbe essere un singolo prodotto poiché ci stiamo paging 81 prodotti, 10 alla volta. Eliminare questo prodotto.

In seguito all'eliminazione del prodotto ultimo, GridView *deve* automaticamente passare alla pagina ottava e tale funzionalità viene esposto con paging predefinito. Con il paging personalizzato, tuttavia, dopo aver eliminato l'ultimo prodotto nell'ultima pagina, GridView semplicemente scomparirà dalla schermata completamente. Il motivo esatto *perché* questo errore si verifica un po' esula dall'ambito di questa esercitazione, vedere [l'eliminazione dell'ultimo Record nell'ultima pagina da un controllo GridView con Paging personalizzata](http://scottonwriting.net/sowblog/posts/7326.aspx) per i dettagli di basso livello per l'origine di Questo problema. In breve, s a causa la sequenza di passaggi che vengono eseguiti dal controllo GridView. quando si fa clic sul pulsante Elimina seguente:

1. Eliminare il record
2. Ottenere i record appropriati da visualizzare per l'oggetto specificato `PageIndex` e`PageSize`
3. Verificare che il `PageIndex` non superi il numero di pagine di dati nell'origine dati; se, riduce automaticamente s GridView `PageIndex` proprietà
4. Associare la pagina appropriata di dati a GridView mediante i record ottenuti nel passaggio 2

Il problema deriva dal fatto che in 2 di passaggio di `PageIndex` utilizzato quando l'acquisizione di record da visualizzare è ancora il `PageIndex` dell'ultima pagina il cui unico record è stata eliminata. Pertanto, nel passaggio 2, *non* vengono restituiti record, poiché l'ultima pagina di dati non contiene alcun record. Quindi, nel passaggio 3, GridView è consapevole del fatto che il relativo `PageIndex` proprietà è maggiore del numero totale di pagine nell'origine dati (poiché è ve eliminato l'ultimo record nell'ultima pagina) e pertanto decrementa il `PageIndex` proprietà. Nel passaggio 4 GridView tenta di associarsi ai dati recuperati nel passaggio 2. Tuttavia, nel passaggio 2 ha restituito alcun record, generando un GridView vuoto. Con il paging predefinito, questo t problema area poiché nel passaggio 2 *tutti* vengono recuperati i record dall'origine dati.

Per risolvere questo problema sono disponibili due opzioni. La prima consiste nel creare un gestore eventi per s GridView `RowDeleted` gestore dell'evento che determina il numero di record sono stato visualizzato nella pagina che è stata eliminata. Se si è verificato un solo record, il record appena eliminato deve essere state ultimo quindi è necessario diminuire il s GridView `PageIndex`. Naturalmente, si desidera aggiornare solo il `PageIndex` se l'operazione di eliminazione è riuscita in realtà, che è possibile determinare assicurando che il `e.Exception` proprietà `null`.

Questo approccio funziona perché viene aggiornato il `PageIndex` dopo il passaggio 1 ma prima del passaggio 2. Pertanto, nel passaggio 2, viene restituito il set di record appropriato. A tale scopo, utilizzare codice simile al seguente:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

Una soluzione alternativa consiste nel creare un gestore eventi per i dispositivi ObjectDataSource `RowDeleted` evento e impostare il `AffectedRows` su un valore pari a 1. Dopo aver eliminato il record nel passaggio 1 (ma prima di recuperare nuovamente i dati nel passaggio 2), Aggiorna GridView relativo `PageIndex` proprietà se una o più righe sono state interessate dall'operazione. Tuttavia, il `AffectedRows` non è impostata da ObjectDataSource e pertanto questo passaggio viene omesso. Per disporre di questo passaggio eseguito è possibile impostare manualmente la `AffectedRows` proprietà se l'operazione di eliminazione viene completata correttamente. Questa può essere eseguita tramite codice simile al seguente:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

Il codice per entrambi questi gestori eventi, vedere la classe code-behind del `EfficientPaging.aspx` esempio.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Confronto tra le prestazioni dell'impostazione predefinita e il Paging personalizzato

Poiché il paging personalizzato recupera solo i record necessari, mentre restituisce paging predefinito *tutti* dei record per ogni pagina visualizzata, è s deselezionare che risulta più efficiente del paging predefinito il paging personalizzato. Tuttavia, solo quanto più efficiente, è il paging personalizzato? Il tipo di miglioramento delle prestazioni può essere visualizzato mediante lo spostamento da paging predefinito per il paging personalizzato?

Purtroppo, s non esiste alcuna dimensione non adatta a rispondere a tutti qui. Il miglioramento delle prestazioni dipende da numerosi fattori, più rilevante due il numero di record, il paging tramite e il carico viene posizionato sui database server e canali di comunicazione tra il server web e il server di database. Per le tabelle di piccole dimensioni con pochi decine record, la differenza di prestazioni può essere ignorabile. Per le tabelle di grandi dimensioni, migliaia a centinaia di migliaia di righe, tuttavia, la differenza di prestazioni è acuta.

Un articolo di essi, [il Paging personalizzato in ASP.NET 2.0 con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), contiene alcuni test è stata eseguita per mostrare le differenze di prestazioni tra queste due tecniche di paging quando il paging di una tabella di database con 50.000 record. In questi test è esaminato sia il tempo per eseguire la query a livello di SQL Server (utilizzando [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) e di pagina ASP.NET utilizzando [funzionalità di traccia di ASP.NET s](https://msdn.microsoft.com/library/y13fw6we.aspx). Tenere presente che questi test sono stati eseguiti sulla casella di sviluppo con un solo utente attivo, pertanto sono poco e non possono simulare i modelli di carico tipico sito Web. In ogni caso, i risultati illustrano le differenze relative tempo di esecuzione per impostazione predefinita e il paging personalizzato quando si lavora con sufficientemente grandi quantità di dati.


|  | **Avg. Durata (sec)** | **Letture** |
| --- | --- | --- |
| **Paging SQL Profiler predefinito** | 1.411 | 383 |
| **Personalizzato Paging SQL Profiler** | 0.002 | 29 |
| **Traccia di ASP.NET Paging predefinita** | 2.379 | *N/A* |
| **Traccia di ASP.NET il Paging personalizzato** | 0.029 | *N/A* |


Come si può notare, il recupero di una particolare pagina di dati necessaria in Media 354 meno operazioni di lettura e completato in una frazione del tempo. Nella pagina ASP.NET personalizzato la pagina è stata in grado di eseguire il rendering nel prossimo a 1/100<sup>th</sup> del tempo impiegato durante l'utilizzo del paging predefinito. Vedere [articolo](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) per ulteriori informazioni su questi risultati insieme al codice e un database è possibile scaricare per riprodurre i test nel proprio ambiente.

## <a name="summary"></a>Riepilogo

Paging predefinito è facile implementare controllo semplicemente la casella di controllo Abilita Paging in dati Web controllo s smart tag, ma tali semplicità comporta il costo delle prestazioni. Con il paging predefinito, quando un utente richiede una pagina di dati *tutti* vengono restituiti record, anche se potrebbe essere visualizzata solo una piccola frazione di essi. Per contrastare questo sovraccarico delle prestazioni, ObjectDataSource offre un'alternativa paging personalizzato opzione di paging.

Mentre il paging personalizzato migliora l'impostazione predefinita il paging di problemi di prestazioni s recuperando solo i record che devono essere visualizzati, è più necessario per implementare il paging personalizzato s. In primo luogo, è necessario scrivere una query che correttamente (e in modo efficiente) accede il subset specifico dei record richiesti. Questa operazione può essere eseguita in diversi modi. quella esaminata in questa esercitazione consiste nell'utilizzare SQL Server 2005 s nuovo `ROW_NUMBER()` i risultati di una funzione di classificazione e quindi per restituire solo i risultati il cui rango è compreso nell'intervallo specificato. Inoltre, è necessario aggiungere un modo per determinare il numero totale di record, il paging tramite. Dopo la creazione di questi metodi DAL e BLL, è necessario anche configurare ObjectDataSource in modo che possa determinare il numero totale di record paging tramite e correttamente passare i valori di indice di riga iniziale e massimo di righe al livello Business Logic.

Durante l'implementazione di paging personalizzato richiedono un numero di passaggi e non è così semplice come paging predefinito, il paging personalizzato è necessario quando il paging sufficientemente grandi quantità di dati. Come esaminare i risultati delle pagine personalizzati, sono stati possibile lasci secondi all'esterno di tempo di rendering della pagina ASP.NET e possibile alleggerire il carico sul server di database per uno o più ordini di grandezza.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Precedente](paging-and-sorting-report-data-vb.md)
[Successivo](sorting-custom-paged-data-vb.md)
