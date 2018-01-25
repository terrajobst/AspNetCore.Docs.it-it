---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: Utilizzo di query con parametri con SqlDataSource (c#) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione, si continua il nostro esaminerà il controllo SqlDataSource e informazioni su come definire le query con parametri. I parametri possono essere specificati entrambi decla..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: b66c68b8306b905a800465ab0ed720ae6f9d16b9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-parameterized-queries-with-the-sqldatasource-c"></a>Utilizzo di query con parametri con SqlDataSource (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe) o [Scarica il PDF](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> In questa esercitazione, si continua il nostro esaminerà il controllo SqlDataSource e informazioni su come definire le query con parametri. I parametri possono essere specificati in modo dichiarativo e a livello di codice e possono essere estratto da un numero di posizioni, ad esempio la stringa di query, sessione dello stato, altri controlli e altro ancora.


## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente è stato illustrato come utilizzare il controllo SqlDataSource per recuperare i dati direttamente da un database. La procedura guidata Configura origine dati, è possibile utilizzare il database e quindi: selezionare le colonne da restituire da una tabella o visualizzazione. Immettere un'istruzione SQL personalizzata. utilizzare una stored procedure. Se la selezione delle colonne di una tabella o vista oppure immettendo un'istruzione SQL personalizzata, SqlDataSource controllo s `SelectCommand` proprietà viene assegnato al codice risultante SQL ad hoc `SELECT` istruzione ed è questo `SELECT` istruzione che viene eseguita quando il S SqlDataSource `Select()` metodo viene richiamato (a livello di codice o automaticamente da un controllo Web di dati).

L'istruzione SQL `SELECT` istruzioni utilizzate nella demo esercitazione s precedente mancava `WHERE` clausole. In un `SELECT` istruzione, il `WHERE` clausola può essere utilizzata per limitare i risultati restituiti. Per visualizzare i nomi dei prodotti di costo maggiore di $50,00, ad esempio, è possibile utilizzare la query seguente:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

In genere, i valori utilizzati un `WHERE` clausola sono determinare da qualche origine esterna, ad esempio un valore di stringa di query, una variabile di sessione o l'input dell'utente da un controllo Web nella pagina. Idealmente, quale tali input vengono specificati mediante l'utilizzo di *parametri*. Con Microsoft SQL Server, i parametri sono contrassegnati con `@parameterName`, come in:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

SqlDataSource supporta le query con parametri, entrambi per `SELECT` istruzioni e `INSERT`, `UPDATE`, e `DELETE` istruzioni. Inoltre, i valori dei parametri possono automaticamente da un'ampia gamma di origini querystring, lo stato della sessione, controlli della pagina e così via oppure possono essere assegnati a livello di codice. In questa esercitazione, vedremo come definire le query con parametri e come specificare il parametro, i valori in modo dichiarativo e a livello di codice.

> [!NOTE]
> Nell'esercitazione precedente abbiamo confrontato ObjectDataSource che è stata scelta strumenti tramite le esercitazioni prima 46 con SqlDataSource, notare le analogie concettuale. Queste analogie estenderanno anche a parametri. I parametri di s ObjectDataSource mappati ai parametri di input per i metodi nel livello di logica di Business. Con SqlDataSource, i parametri vengono definiti direttamente all'interno di query SQL. Entrambi i controlli sono raccolte di parametri per i relativi `Select()`, `Insert()`, `Update()`, e `Delete()` metodi ed entrambe possono avere valori di questi parametri popolati da origini predefinite (i valori di stringa di query, variabili di sessione e così via ) o assegnato a livello di codice.


## <a name="creating-a-parameterized-query"></a>Creazione di una query con parametri

La procedura guidata Configura origine dati di s controllo SqlDataSource offre tre modi per la definizione di comando da eseguire per recuperare i record di database:

- Selezionando le colonne da una tabella o vista esistente,
- Immettendo un'istruzione SQL personalizzata, o
- Scegliendo una stored procedure

Durante la selezione di colonne da una tabella esistente o visualizzazione, i parametri per il `WHERE` clausola deve essere specificata tramite l'aggiunta `WHERE` la finestra di dialogo clausola. Quando si crea un'istruzione SQL personalizzata, è tuttavia possibile immettere i parametri direttamente nel `WHERE` clausola (utilizzando `@parameterName` per identificare ogni parametro). Oggetto [stored procedure di](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) è costituito da uno o più istruzioni SQL, e possono essere parametrizzate queste istruzioni. I parametri utilizzati nelle istruzioni SQL, tuttavia, devono essere passati come parametri di input alla stored procedure.

Poiché la creazione di una query con parametri dipende da come s SqlDataSource `SelectCommand` è specificato, consentono s esaminare i tre approcci. Per iniziare, aprire il `ParameterizedQueries.aspx` nella pagina di `SqlDataSource` cartella, trascinare un controllo SqlDataSource dalla casella degli strumenti nella finestra di progettazione e impostare il relativo `ID` per `Products25BucksAndUnderDataSource`. Fare clic sul collegamento Configura origine dati dallo smart tag s di controllo. Selezionare il database da utilizzare (`NORTHWINDConnectionString`) e fare clic su Avanti.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Passaggio 1: Aggiunta di un`WHERE`clausola durante la selezione delle colonne di una tabella o vista

Quando si selezionano i dati da restituire dal database con il controllo SqlDataSource, la configurazione guidata origine dati consente di selezionare semplicemente le colonne da restituire da una tabella esistente o visualizzare (vedere la figura 1). In questo modo automatico crea un database SQL `SELECT` istruzione, viene inviato al database quando il s SqlDataSource `Select()` metodo viene richiamato. Come è stato fatto nell'esercitazione precedente, selezionare la tabella di prodotti nell'elenco a discesa e selezionare il `ProductID`, `ProductName`, e `UnitPrice` colonne.


[![Selezionare le colonne da restituire da una tabella o vista](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**Figura 1**: scegliere le colonne per restituito da una tabella o vista ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))


Per includere un `WHERE` clausola il `SELECT` istruzione, fare clic sul `WHERE` pulsante, viene visualizzata Aggiungi `WHERE` clausola dialogo (figura 2). Per aggiungere un parametro per limitare i risultati restituiti dal `SELECT` di query, scegliere la colonna per filtrare i dati da. Successivamente, scegliere l'operatore da utilizzare per il filtro (=, &lt;, &lt;=, &gt;e così via). Infine, scegliere l'origine del valore del parametro s, ad esempio dallo stato sessione o di stringa di query. Dopo aver configurato il parametro, fare clic sul pulsante Aggiungi per includerlo nella `SELECT` query.

Per questo esempio, s consentono di restituire solo i risultati in cui il `UnitPrice` valore è minore o uguale a $25.00. Pertanto, selezionare `UnitPrice` dall'elenco a discesa di colonne e &lt;= dall'elenco a discesa operatore. Quando si utilizza un valore di parametro a livello di codice (ad esempio $25.00) o se il valore del parametro deve essere specificato a livello di codice, selezionare Nessuno dall'elenco a discesa di origine. Successivamente, immettere il valore del parametro hardcoded nella casella di testo valore 25,00 e completare il processo facendo clic sul pulsante Aggiungi.


[![Limitare i risultati restituiti dalla posizione in cui aggiungere la finestra di dialogo clausola](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**Figura 2**: limitare i risultati restituiti da Aggiungi `WHERE` la finestra di dialogo clausola ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))


Dopo aver aggiunto il parametro, fare clic su OK per tornare alla procedura guidata Configura origine dati. Il `SELECT` istruzione nella parte inferiore della procedura guidata deve ora includere un `WHERE` clausola con un parametro denominato `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> Se si specificano più condizioni nel `WHERE` clausola da Aggiungi `WHERE` clausola della finestra di dialogo la procedura guidata li unisce con il `AND` operatore. Se è necessario includere un `OR` nel `WHERE` clausola (ad esempio `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`), quindi è necessario compilare il `SELECT` istruzione tramite la schermata di istruzione SQL personalizzata.


Completare la configurazione SqlDataSource (fare clic su Avanti, quindi completare) e quindi verificare il markup dichiarativo SqlDataSource s. Il codice include ora un `<SelectParameters>` insieme, definiscono le origini per i parametri in di `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

Quando s SqlDataSource `Select()` metodo viene richiamato, il `UnitPrice` valore del parametro (25,00) viene applicato al `@UnitPrice` parametro il `SelectCommand` prima dell'invio al database. Il risultato finale è che solo i prodotti dai minore o uguale a $25.00 vengono restituiti il `Products` tabella. Per verificarlo, aggiungere un controllo GridView alla pagina, associarlo a questa origine dati e quindi visualizzare la pagina tramite un browser. Si dovrebbero visualizzare solo i prodotti elencati sono minore o uguale a $25.00, come nella figura 3 viene confermata.


[![Vengono visualizzati solo quelli prodotti minore o uguale a $25.00](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**Figura 3**: vengono visualizzati solo quelli prodotti minore o uguale a $25.00 ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Passaggio 2: Aggiunta di parametri a un'istruzione SQL personalizzata

Quando si aggiunge un'istruzione SQL personalizzata è possibile immettere il `WHERE` clausola in modo esplicito oppure specificare un valore nella cella dei filtri del generatore di Query. Per dimostrare questo concetto, consentire s visualizzare solo i prodotti in un controllo GridView il cui prezzo è inferiore a una determinata soglia. Per iniziare, aggiungere una casella di testo di `ParameterizedQueries.aspx` pagina per raccogliere il valore di soglia da parte dell'utente. Impostare la casella di testo s `ID` proprietà `MaxPrice`. Aggiungere un controllo pulsante Web e impostare il relativo `Text` proprietà per i prodotti di visualizzazione corrispondente.

Successivamente, trascinare un controllo GridView nella pagina e dal suo smart tag scegliere di creare un nuovo SqlDataSource denominato `ProductsFilteredByPriceDataSource`. Dalla procedura guidata Configura origine dati, procedere per specificare una schermata di stored procedure o un'istruzione SQL personalizzata (vedere la figura 4) e immettere la query seguente:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

Dopo aver immesso la query (manualmente o tramite il generatore di Query), fare clic su Avanti.


[![Restituire solo i prodotti minore o uguale a un valore di parametro](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**Figura 4**: restituire solo quelli prodotti minore o uguale al valore di parametro ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))


Poiché la query include parametri, la schermata successiva della procedura guidata richiede Microsoft per l'origine dei valori di parametri. Scegliere controllo dall'elenco a discesa Origine parametro e `MaxPrice` (il controllo TextBox s `ID` valore) dall'elenco a discesa ControlID. È inoltre possibile immettere un valore predefinito facoltativo da utilizzare nel caso in cui l'utente non ha immesso il testo nel `MaxPrice` casella di testo. Per il momento, non immettere un valore predefinito.


[![Proprietà di testo viene utilizzata come origine del parametro s MaxPrice TextBox](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**Figura 5**: il `MaxPrice` TextBox s `Text` proprietà viene utilizzata come origine del parametro ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))


Completare la configurazione guidata origine dati facendo clic su Avanti, quindi Fine. Il markup dichiarativo per GridView, casella di testo il pulsante SqlDataSource seguente:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

Si noti che il parametro all'interno di s SqlDataSource `<SelectParameters>` sezione è un `ControlParameter`, che include proprietà aggiuntive, ad esempio `ControlID` e `PropertyName`. Quando i dispositivi SqlDataSource `Select()` metodo viene richiamato, il `ControlParameter` acquisisce il valore della proprietà di controllo Web specificato e lo assegna al parametro corrispondente nel `SelectCommand`. In questo esempio, il `MaxPrice` s proprietà Text viene utilizzato come il `@MaxPrice` valore del parametro.

Richiedere un minuto, per visualizzare questa pagina tramite un browser. Durante la prima visita la pagina o ogni volta che il `MaxPrice` casella di testo non è presente un valore viene visualizzato alcun record in GridView.


[![Nessun record viene che visualizzati quando il MaxPrice casella di testo è vuoto](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**Figura 6**: nessun record vengono visualizzati quando il `MaxPrice` casella di testo è vuoto ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))


Nessun prodotto viene visualizzato, infatti, in quanto, per impostazione predefinita, una stringa vuota per un valore del parametro viene convertita in un database `NULL` valore. Poiché il confronto di `[UnitPrice] <= NULL` restituisce sempre false, viene restituito alcun risultato.

Immettere un valore nella casella di testo, ad esempio 5,00 e fare clic sul pulsante di visualizzazione corrispondenti prodotti. Durante il postback, SqlDataSource informa che una delle sue origini parametro GridView è stato modificato. Di conseguenza, il controllo GridView riassocia per SqlDataSource, visualizzazione di tali prodotti minore o uguale a $5.00.


[![Vengono visualizzati i prodotti minore o uguale a $5,00](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**Figura 7**: vengono visualizzati i prodotti minore o uguale a $5,00 ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Inizialmente la visualizzazione di tutti i prodotti

Anziché non visualizzare alcun prodotti quando la pagina viene caricata, potrebbe essere necessario visualizzare *tutti* prodotti. Un modo per visualizzare l'elenco di tutti i prodotti ogni volta che il `MaxPrice` casella di testo è vuoto consiste nell'impostare il valore predefinito di s parametri su un valore elevato insanely, ad esempio 1000000, perché s improbabile che Northwind Traders mai avere inventario il cui prezzo unitario supera 1.000.000. Tuttavia, questo approccio è superficiale e potrebbe non funzionare in altre situazioni.

Nelle esercitazioni precedenti - [parametri dichiarativi](../basic-reporting/declarative-parameters-cs.md) e [Master-Details filtro con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) si stava affrontare un problema analogo. Sono la soluzione era quello di inserire la logica nel livello di logica di Business. In particolare, il livello Business LOGIC esaminato il valore in ingresso e, se si trattasse di `NULL` o alcuni riservati valore, è stata indirizzata la chiamata al metodo DAL che tutti i record restituiti. Se il valore in ingresso è un valore di filtro normale, è stata effettuata una chiamata al metodo che ha eseguito un'istruzione SQL utilizzata con un parametri DAL `WHERE` clausola con il valore fornito.

Purtroppo, l'architettura è ignorare quando si utilizza SqlDataSource. In alternativa, è necessario personalizzare l'istruzione SQL per acquisire in modo intelligente tutti i record se il `@MaximumPrice` parametro `NULL` o un valore riservato. Per questo esercizio, consentono di s è in modo che se il `@MaximumPrice` parametro è uguale a `-1.0`, quindi *tutti* i record devono essere restituiti (`-1.0` funziona come un valore riservato poiché nessun prodotto può avere un valore negativo `UnitPrice`valore). A tale scopo che è possibile utilizzare l'istruzione SQL seguente:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

Questo `WHERE` clausola restituisce *tutti* registra se il `@MaximumPrice` parametro è uguale a `-1.0`. Se il valore del parametro non è `-1.0`, solo i prodotti il cui `UnitPrice` è minore o uguale al `@MaximumPrice` viene restituito il valore di parametro. Impostando il valore predefinito il `@MaximumPrice` parametro `-1.0`, il primo caricamento della pagina (o ogni volta che il `MaxPrice` casella di testo è vuoto), `@MaximumPrice` avrà un valore di `-1.0` e tutti i prodotti verranno visualizzati.


[![Ora tutti i prodotti vengono visualizzati quando il MaxPrice casella di testo è vuoto](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**Figura 8**: ora tutti i prodotti vengono visualizzati quando il `MaxPrice` casella di testo è vuoto ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))


Esistono un paio di aspetti da notare con questo approccio. In primo luogo, tenere presente che il tipo di dati di parametro s viene dedotto dall'utilizzo di s nella query SQL. Se si modifica il `WHERE` clausola da `@MaximumPrice = -1.0` a `@MaximumPrice = -1`, il runtime utilizza il parametro come un numero intero. Se quindi si tenta di assegnare il `MaxPrice` casella di testo di un valore decimale (ad esempio 5,00), verrà generato un errore perché non è possibile convertire 5,00 in un numero intero. Per risolvere questo problema, assicurarsi di utilizzare `@MaximumPrice = -1.0` nel `WHERE` clausola o, meglio ancora, imposta il `ControlParameter` oggetto s `Type` proprietà Decimal.

In secondo luogo, aggiungendo il `OR @MaximumPrice = -1.0` per il `WHERE` clausola, il motore di query non è possibile utilizzare un indice per `UnitPrice` (se ne esiste uno), determinando in tal modo una scansione di tabella. Questo può influire sulle prestazioni sono un numero sufficiente di record di `Products` tabella. Un approccio migliore, è possibile spostare questa logica a una stored procedure in cui un `IF` istruzione potrebbe eseguire un `SELECT` query dal `Products` tabella senza un `WHERE` clausola quando tutti i record devono essere restituiti o uno cui `WHERE` clausola contiene solo il `UnitPrice` criteri, in modo che un indice può essere utilizzato.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Passaggio 3: Creazione e utilizzo di Stored procedure con parametri

Stored procedure possono includere un set di parametri di input che può quindi essere usato nelle istruzioni SQL definite all'interno della stored procedure. Quando si configura SqlDataSource per l'utilizzo di una stored procedure che accetta parametri di input, questi valori di parametro possono essere specificati con le stesse tecniche come istruzioni SQL ad hoc.

Per illustrare l'utilizzo di stored procedure in SqlDataSource, s consentono di creare una nuova stored procedure nel database Northwind denominato `GetProductsByCategory`, che accetta un parametro denominato `@CategoryID` e restituisce tutte le colonne dei prodotti il cui `CategoryID` la colonna corrisponde a `@CategoryID`. Per creare una stored procedure, accedere a Esplora Server e drill-down di `NORTHWND.MDF` database. (Se non si t Esplora Server, attivare la modalità passare al menu Visualizza e selezionando l'opzione di Esplora Server.)

Dal `NORTHWND.MDF` del database, fare doppio clic sulla cartella Stored procedure, scegliere Aggiungi nuova Stored Procedure e immettere la sintassi seguente:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

Fare clic su Salva icona (o Ctrl + S) per salvare la stored procedure. È possibile testare la stored procedure facendovi dalla cartella Stored procedure e scegliendo Esegui. Questo richiederà i parametri delle stored procedure s (`@CategoryID`, in questa istanza), dopo che i risultati, viene visualizzati nella finestra di Output.


[![Il GetProductsByCategory Stored Procedure quando viene eseguito con un @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**Figura 9**: il `GetProductsByCategory` Stored Procedure quando viene eseguito con un `@CategoryID` 1 ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))


Consente di utilizzare questa stored procedure per visualizzare tutti i prodotti della categoria Beverages in GridView s. Aggiungere un nuovo GridView alla pagina e associarlo a un nuovo SqlDataSource denominato `BeverageProductsDataSource`. Continuare per specificare una schermata di stored procedure o un'istruzione SQL personalizzata, selezionare il pulsante di opzione di Stored procedure e selezionare il `GetProductsByCategory` stored procedure dall'elenco a discesa.


[![Selezionare il GetProductsByCategory Stored Procedure dall'elenco a discesa](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**Figura 10**: selezionare il `GetProductsByCategory` Stored Procedure nell'elenco a discesa ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))


Poiché la stored procedure accetta un parametro di input (`@CategoryID`), fare clic su Avanti, viene richiesto di specificare l'origine per il valore del parametro s. Le bibite `CategoryID` è 1, pertanto lasciare l'elenco di riepilogo a discesa Origine parametro None e immettere 1 nella casella di testo DefaultValue.


[![Utilizzare un valore hardcoded pari a 1 per restituire i prodotti della categoria Beverages](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**Figura 11**: utilizzo del valore Hard-Coded 1 per restituire i prodotti della categoria Beverages ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))


Come illustrato nel codice dichiarativo seguente, quando si utilizza una stored procedure, s SqlDataSource `SelectCommand` proprietà è impostata sul nome della stored procedure e [ `SelectCommandType` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) è impostato su `StoredProcedure`, che indica che il `SelectCommand` è il nome di una stored procedure anziché un'istruzione SQL ad hoc.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

Testare la pagina in un browser. Vengono visualizzati solo i prodotti che appartengono alla categoria Beverages, anche se *tutti* del prodotto vengono visualizzati i campi, poiché il `GetProductsByCategory` stored procedure restituisce tutte le colonne dal `Products` tabella. È possibile, naturalmente, limitare o personalizzare i campi visualizzati in GridView nella finestra di dialogo Modifica colonne s GridView.


[![Verranno visualizzati tutti delle bevande](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**Figura 12**: verranno visualizzati tutti delle bevande ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Passaggio 4: Richiamare a livello di programmazione s SqlDataSource`Select()`istruzione

Negli esempi è ve descritte nell'esercitazione precedente e in questa esercitazione sono associati a controlli SqlDataSource direttamente a un controllo GridView. I dati del controllo s SqlDataSource, tuttavia, a livello di programmazione accessibili ed enumerati nel codice. Può essere particolarmente utile quando è necessario eseguire query sui dati per verificarlo, ma tenere sempre necessario per visualizzarlo. Anziché dover scrivere tutti i commenti di codice ADO.NET per connettersi al database, specificare il comando e recuperare i risultati, è possibile consentire SqlDataSource gestire questo codice monotono.

Per illustrare l'uso di s SqlDataSource dati a livello di codice, si supponga che un determinato mittente ha affrontato si con una richiesta per creare una pagina web che visualizza il nome di una categoria selezionata in modo casuale e i relativi prodotti associati. Vale a dire quando un utente visita questa pagina, si desidera scegliere casualmente una categoria di `Categories` tabella, visualizzare il nome della categoria e quindi elencati i prodotti appartenenti alla categoria selezionata.

A tale scopo è necessario avere due controlli SqlDataSource uno per selezionare una categoria casuale dal `Categories` tabella e un altro per ottenere la categoria di prodotti. Creeremo SqlDataSource che recupera un record di categoria casuali in questo passaggio. Passaggio 5 esamina crea SqlDataSource che recupera i prodotti s categoria.

Per iniziare, aggiungere un SqlDataSource per `ParameterizedQueries.aspx` e impostare il relativo `ID` a `RandomCategoryDataSource`. Configurarlo in modo che utilizzi la query SQL seguente:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()`Restituisce i record ordinati in ordine casuale (vedere [Using `NEWID()` per ordinare in modo casuale i record](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1`Restituisce il primo record dal set di risultati. Mettere insieme, la query seguente restituisce il `CategoryID` e `CategoryName` valori della colonna da una categoria singolo, selezionato in modo casuale.

Per visualizzare la categoria s `CategoryName` valore, aggiungere un controllo etichetta Web alla pagina, impostare il relativo `ID` proprietà `CategoryNameLabel`e cancellare la `Text` proprietà. Per recuperare a livello di programmazione i dati da un controllo SqlDataSource, è necessario richiamare il `Select()` metodo. Il [ `Select()` metodo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) prevede un solo parametro di input di tipo [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), che specifica come i dati devono essere messaggi prima della restituzione. Può includere istruzioni sull'ordinamento e filtro dei dati e viene utilizzato dai dati di che controlli Web durante l'ordinamento o paging dei dati da un controllo SqlDataSource. Per questo esempio, tuttavia, è si necessità di t i dati di essere modificato prima della restituzione e pertanto verrà passato il `DataSourceSelectArguments.Empty` oggetto.

Il `Select()` metodo restituisce un oggetto che implementa `IEnumerable`. Il tipo preciso restituito dipende dal valore del controllo SqlDataSource s [ `DataSourceMode` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Come descritto nell'esercitazione precedente, questa proprietà può essere impostata su un valore di `DataSet` o `DataReader`. Se impostato su `DataSet`, `Select()` metodo restituisce un [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) oggetto; se impostata su `DataReader`, restituisce un oggetto che implementa [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Poiché il `RandomCategoryDataSource` SqlDataSource è relativo `DataSourceMode` proprietà impostata su `DataSet` (impostazione predefinita), verrà utilizzato con un oggetto DataView.

Il codice seguente viene illustrato come recuperare i record di `RandomCategoryDataSource` SqlDataSource come un oggetto DataView e come leggere il `CategoryName` valore della colonna della prima riga di DataView:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]`Restituisce il primo `DataRowView` nella vista dati. `randomCategoryView[0]["CategoryName"]`Restituisce il valore della `CategoryName` colonna nella prima riga. Si noti che la vista dati fortemente tipizzato. Per fare riferimento a un valore di colonna specifica è necessario passare il nome della colonna come una stringa (CategoryName, in questo caso). Figura 13 illustra il messaggio visualizzato nel `CategoryNameLabel` la visualizzazione della pagina. Naturalmente, il nome di categoria effettiva visualizzato viene selezionato casualmente dal `RandomCategoryDataSource` SqlDataSource su ogni visita la pagina (inclusi i postback).


[![I dispositivi in modo casuale categoria selezionata che nome viene visualizzato](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**Figura 13**: s il in modo casuale categoria selezionata viene visualizzato nome ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))


> [!NOTE]
> Se il controllo SqlDataSource s `DataSourceMode` era impostata su `DataReader`, il valore restituito dal `Select()` metodo sarebbe stato necessario eseguire il cast a `IDataReader`. Per leggere il `CategoryName` valore della colonna della prima riga è d usare codice simile:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

Con SqlDataSource seleziona casualmente una categoria, è nuovamente pronto per aggiungere il controllo GridView in cui sono elencati i prodotti della categoria s.

> [!NOTE]
> Anziché utilizzare un controllo etichetta Web per visualizzare il nome di categoria s, avremmo potuto aggiungere un controllo FormView o DetailsView alla pagina, associazione a SqlDataSource. Con l'etichetta, tuttavia, consente di esplorare come richiamare a livello di programmazione s SqlDataSource `Select()` istruzione e lavorare con i dati risultanti nel codice.


## <a name="step-5-assigning-parameter-values-programmatically"></a>Passaggio 5: Assegnare i valori dei parametri a livello di codice

Tutti gli esempi si ve descritte in questa esercitazione è stato utilizzato un valore di parametro hardcoded o quello eseguito da una delle origini di parametro predefiniti (un valore di stringa di query, un controllo Web nella pagina e così via). Tuttavia, i parametri di s controllo SqlDataSource possono anche essere impostati a livello di codice. Per completare l'esempio corrente, è necessario un SqlDataSource che restituisce tutti i prodotti appartenenti a una categoria specificata. Questo SqlDataSource avrà un `CategoryID` parametro il cui valore deve essere impostata in base il `CategoryID` valore della colonna restituito dal `RandomCategoryDataSource` SqlDataSource nel `Page_Load` gestore dell'evento.

Per iniziare, aggiungere un controllo GridView alla pagina e associarlo a un nuovo SqlDataSource denominato `ProductsByCategoryDataSource`. Proprio come abbiamo visto nel passaggio 3, configurare SqlDataSource in modo che richiama la `GetProductsByCategory` stored procedure. Lasciare il parametro origine riepilogo impostato su None, ma si immette un valore predefinito, come si imposterà il valore predefinito a livello di codice.


[![Non si specifica un parametro origine o il valore predefinito](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**Nella figura 14**: non si specifica un parametro di origine o un valore predefinito ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))


Dopo aver completato la procedura guidata SqlDataSource, markup dichiarativo risultante dovrebbe essere simile al seguente:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

È possibile assegnare il `DefaultValue` del `CategoryID` parametro livello di programmazione il `Page_Load` gestore eventi:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

Con l'aggiunta, la pagina include un controllo GridView che mostra i prodotti associati alla categoria selezionata in modo casuale.


[![Non si specifica un parametro origine o il valore predefinito](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**Figura 15**: non si specifica un parametro di origine o un valore predefinito ([fare clic per visualizzare l'immagine ingrandita](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))


## <a name="summary"></a>Riepilogo

SqlDataSource consente agli sviluppatori di pagina definire le query con parametri i cui valori di parametro possono essere hardcoded, il pull da origini di parametro predefiniti o assegnati a livello di codice. In questa esercitazione è stato illustrato come creare una query con parametri dalla procedura guidata Configura origine dati per le query SQL ad hoc e stored procedure. È inoltre esaminato l'utilizzo di origini di parametro a livello di codice, un controllo Web come origine, parametro e a livello di codice che specifica il valore del parametro.

Ad esempio con ObjectDataSource, SqlDataSource offre inoltre funzionalità per modificare i dati sottostanti. Nella prossima esercitazione verrà esaminato come definire `INSERT`, `UPDATE`, e `DELETE` istruzioni con SqlDataSource. Dopo aver aggiunto queste istruzioni, che possiamo utilizzare predefiniti di inserimento, modifica ed eliminazione di funzioni intrinseche ai controlli GridView, DetailsView e FormView.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Scott Clyde Randell Schmidt e Ken Pespisa. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](querying-data-with-the-sqldatasource-control-cs.md)
[Successivo](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
