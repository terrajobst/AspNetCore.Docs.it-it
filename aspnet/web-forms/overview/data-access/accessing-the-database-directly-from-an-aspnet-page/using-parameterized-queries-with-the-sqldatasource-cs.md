---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: Uso di query con parametri con SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione, si continua nostro esaminare il controllo SqlDataSource e informazioni su come definire le query con parametri. I parametri possono essere specificati entrambi dichi...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 654c0ce5520a206e5e8e2fd20bed92ac1075bfe9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832766"
---
<a name="using-parameterized-queries-with-the-sqldatasource-c"></a>Uso di query con parametri con SqlDataSource (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe) o [Scarica il PDF](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> In questa esercitazione, si continua nostro esaminare il controllo SqlDataSource e informazioni su come definire le query con parametri. I parametri possono essere specificati in modo dichiarativo e a livello di codice e possono essere effettuato il pull da un numero di posizioni, ad esempio la stringa di query, sessione dello stato, altri controlli e altro ancora.


## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente abbiamo visto come utilizzare il controllo SqlDataSource per recuperare i dati direttamente da un database. Usando la procedura guidata Configura origine dati, tra cui scegliere il database e quindi: selezionare le colonne da restituire da una tabella o vista; Immettere un'istruzione SQL personalizzata. oppure usare una stored procedure. Se la selezione delle colonne da una tabella o una vista o l'immissione di un'istruzione SQL personalizzata, SqlDataSource controllo s `SelectCommand` proprietà viene assegnato il codice SQL ad-hoc risultante `SELECT` istruzione ed è questo `SELECT` istruzione che viene eseguita quando il S SqlDataSource `Select()` metodo viene richiamato (in modo automatico o a livello di codice da un controllo Web per dati).

Il codice SQL `SELECT` istruzioni utilizzate nelle demo esercitazione s precedente mancava `WHERE` clausole. In un `SELECT` istruzione, il `WHERE` clausola può essere utilizzata per limitare i risultati restituiti. Per visualizzare i nomi dei prodotti di costo maggiore di $50,00, ad esempio, potremmo utilizziamo la query seguente:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

In genere, i valori usati un `WHERE` clausola sono determinare da un'origine esterna, ad esempio un valore di stringa di query, una variabile di sessione o input utente da un controllo Web nella pagina. In teoria, tali input vengono specificati tramite l'uso di *parametri*. Con Microsoft SQL Server, i parametri vengono indicati utilizzando `@parameterName`, come in:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

SqlDataSource supporta le query con parametri, sia per `SELECT` istruzioni e `INSERT`, `UPDATE`, e `DELETE` istruzioni. Inoltre, i valori dei parametri possono effettuare automaticamente il pull da un'ampia gamma di origini la stringa di query, lo stato della sessione, i controlli nella pagina e così via o possono essere assegnati a livello di codice. In questa esercitazione, vedremo come definire le query con parametri e come specificare il parametro valori sia in modo dichiarativo e a livello di codice.

> [!NOTE]
> Nell'esercitazione precedente sono stati confrontati ObjectDataSource che è stata lo strumento preferito tramite le esercitazioni innanzitutto 46 con SqlDataSource, annotando le analogie concettuale. Queste analogie si estendono anche ai parametri. I parametri di ObjectDataSource s mappati ai parametri di input per i metodi nel livello di logica di Business. Con SqlDataSource, i parametri sono definiti direttamente all'interno della query SQL. Entrambi i controlli sono raccolte di parametri per la loro `Select()`, `Insert()`, `Update()`, e `Delete()` metodi ed entrambe possono avere i valori dei parametri popolati dalle origini predefinite (i valori di stringa di query, le variabili di sessione e così via ) o assegnato a livello di codice.


## <a name="creating-a-parameterized-query"></a>Creazione di una query con parametri

La procedura guidata Configura origine dati di s controllo SqlDataSource offre tre modi: per la definizione di comando da eseguire per recuperare i record del database:

- Selezionando le colonne da una tabella o vista esistente,
- Immettendo un'istruzione SQL personalizzata, o
- Scegliendo una stored procedure

Durante la selezione di colonne da una tabella esistente o vista, i parametri per il `WHERE` clausola deve essere specificata tramite il componente `WHERE` clausola finestra di dialogo. Quando si crea un'istruzione SQL personalizzata, è tuttavia possibile immettere i parametri direttamente nella `WHERE` clausola (utilizzando `@parameterName` per indicare ogni parametro). Oggetto [stored procedure di](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) è costituito da uno o più istruzioni SQL, e queste istruzioni possono essere parametrizzate. I parametri utilizzati nelle istruzioni SQL, tuttavia, devono essere passati come parametri di input alla stored procedure.

Poiché la creazione di una query con parametri dipende da come la s SqlDataSource `SelectCommand` è specificato, "Let" s dare un'occhiata a tutte e tre gli approcci. Per iniziare, aprire il `ParameterizedQueries.aspx` nella pagina la `SqlDataSource` cartella, trascinare un controllo SqlDataSource dalla casella degli strumenti nella finestra di progettazione e impostare relativo `ID` a `Products25BucksAndUnderDataSource`. Successivamente, fare clic sul collegamento Configura origine dati nello smart tag controllo s. Selezionare il database da usare (`NORTHWINDConnectionString`) e fare clic su Avanti.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Passaggio 1: Aggiunta di un`WHERE`clausola durante la selezione di colonne da una tabella o vista

Quando si selezionano i dati da restituire dal database con il controllo SqlDataSource, la procedura guidata Configura origine dati consente di selezionare semplicemente le colonne da restituire da una tabella esistente o visualizzare (vedere la figura 1). In questo modo automatico si basa su un database SQL `SELECT` istruzione, che è ciò che viene inviato al database quando le s SqlDataSource `Select()` metodo viene richiamato. Come è stato fatto nell'esercitazione precedente, selezionare la tabella di prodotti nell'elenco a discesa e verificare i `ProductID`, `ProductName`, e `UnitPrice` colonne.


[![Selezionare le colonne da restituire da una tabella o vista](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**Figura 1**: selezionare le colonne di valore restituito da una tabella o vista ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))


Per includere un `WHERE` clausola nel `SELECT` istruzione, fare clic sui `WHERE` pulsante, viene visualizzata Aggiungi `WHERE` clausola dialogo (vedere la figura 2). Per aggiungere un parametro per limitare i risultati restituiti dai `SELECT` query, in primo luogo scegliere la colonna per filtrare i dati in base. Scegliere quindi l'operatore da utilizzare per il filtro (=, &lt;, &lt;=, &gt;e così via). Infine, scegliere l'origine del valore del parametro s, ad esempio dallo stato di sessione o stringa di query. Dopo aver configurato il parametro, fare clic sul pulsante Aggiungi per includerla nella `SELECT` query.

Per questo esempio, ti permettono di s restituire solo i risultati in cui il `UnitPrice` valore è minore o uguale a $25.00. Selezionare pertanto `UnitPrice` dall'elenco a discesa di colonne e &lt;= dall'elenco a discesa operatore. Quando si usa un valore di parametro hardcoded (ad esempio $25.00) o se il valore del parametro deve essere specificato a livello di programmazione, selezionare Nessuno dall'elenco a discesa di origine. Successivamente, immettere il valore del parametro hardcoded nella casella di testo valore 25,00 e completare il processo facendo clic sul pulsante Aggiungi.


[![Limitare i risultati restituiti dagli aggiungere dove clausola finestra di dialogo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**Figura 2**: limitare i risultati restituiti da Add `WHERE` finestra di dialogo clausola ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))


Dopo aver aggiunto il parametro, fare clic su OK per tornare alla procedura guidata Configura origine dati. Il `SELECT` istruzione nella parte inferiore della procedura guidata dovrebbe ora includere una `WHERE` clausola con un parametro denominato `@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> Se si specificano più condizioni nel `WHERE` clausola da Add `WHERE` finestra di dialogo clausola, la procedura guidata crea un join con la `AND` operatore. Se è necessario includere un' `OR` nella `WHERE` clausola (, ad esempio `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`), quindi è necessario compilare il `SELECT` istruzione tramite la schermata di istruzione SQL personalizzata.


Completare la configurazione SqlDataSource (fare clic su Avanti, quindi completare) e quindi esaminare il markup dichiarativo SqlDataSource s. Il markup include ora un `<SelectParameters>` raccolta, che illustra in dettaglio le origini per i parametri nel `SelectCommand`.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

Quando la s SqlDataSource `Select()` metodo viene richiamato, il `UnitPrice` valore del parametro (25,00) viene applicato al `@UnitPrice` parametro nel `SelectCommand` prima dell'invio al database. Il risultato è che solo i prodotti dai minore o uguale a $25.00 vengono restituiti i `Products` tabella. Per verificarlo, aggiungere un controllo GridView alla pagina, associarlo a questa origine dati e quindi visualizzare la pagina tramite un browser. Dovrebbe essere visibile solo i prodotti elencati minore o uguale a $25.00, come nella figura 3 viene confermata.


[![Vengono visualizzati solo i prodotti minore di o uguale a $25.00](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**Figura 3**: vengono visualizzati solo quelli prodotti minore di o uguale a $25.00 ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Passaggio 2: Aggiunta di parametri a un'istruzione SQL personalizzata

Quando si aggiunge un'istruzione SQL personalizzata è possibile immettere il `WHERE` clausola in modo esplicito oppure specificare un valore nella cella del generatore delle Query di filtro. Per dimostrare questo concetto, consentono s visualizzare solo i prodotti in GridView i cui prezzi sono inferiori rispetto a una determinata soglia. Iniziare aggiungendo una casella di testo il `ParameterizedQueries.aspx` pagina per raccogliere il valore di soglia da parte dell'utente. Impostare la casella di testo s `ID` proprietà `MaxPrice`. Aggiungere un controllo Web pulsante e impostarne il `Text` proprietà ai prodotti di visualizzazione corrispondente.

Successivamente, trascinare un controllo GridView sulla pagina e dal suo smart tag scegliere di creare un nuovo SqlDataSource denominato `ProductsFilteredByPriceDataSource`. Dalla procedura guidata Configura origine dati, passare alla specifica di una schermata di stored procedure o un'istruzione SQL personalizzata (vedere la figura 4) e immettere la query seguente:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

Dopo aver immesso la query (manualmente o tramite il generatore delle Query), fare clic su Avanti.


[![Restituire solo i prodotti minore o uguale al valore di parametro](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**Figura 4**: restituisce solo quelli prodotti minore di o uguale al valore del parametro ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))


Poiché la query include parametri, nella schermata successiva della procedura guidata richiede per l'origine dei valori di parametri. Scegliere controllo dall'elenco a discesa di origine parametri e `MaxPrice` (il controllo TextBox s `ID` valore) dall'elenco a discesa ControlID. È anche possibile immettere un valore predefinito facoltativo da utilizzare nel caso in cui l'utente non ha immesso testo nel `MaxPrice` nella casella di testo. Per il momento, non immettere un valore predefinito.


[![Le s MaxPrice TextBox che proprietà Text viene utilizzato come origine del parametro](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**Figura 5**: il `MaxPrice` nella casella di testo s `Text` proprietà viene utilizzata come origine del parametro ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))


Completare la procedura guidata Configura origine dati facendo clic su Avanti e quindi finire. Il markup dichiarativo per il controllo GridView, TextBox, Button e SqlDataSource indicato di seguito:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

Si noti che il parametro all'interno di s SqlDataSource `<SelectParameters>` sezione è un `ControlParameter`, che include proprietà aggiuntive, ad esempio `ControlID` e `PropertyName`. Quando la s SqlDataSource `Select()` metodo viene richiamato, il `ControlParameter` estrae il valore dalla proprietà del controllo Web specificato e lo assegna al parametro corrispondente nel `SelectCommand`. In questo esempio, il `MaxPrice` s proprietà Text viene utilizzato come il `@MaxPrice` valore del parametro.

Richiedere un minuto per visualizzare questa pagina tramite un browser. Durante la prima visita la pagina o ogni volta che il `MaxPrice` nella casella di testo non è presente un valore in GridView viene visualizzato alcun record.


[![Nessun record viene che visualizzati quando il MaxPrice casella di testo è vuota](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**Figura 6**: nessun record vengono visualizzati quando il `MaxPrice` casella di testo è vuota ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))


Il motivo prodotti non vengono visualizzata è che, per impostazione predefinita, una stringa vuota per un valore del parametro viene convertita in un database `NULL` valore. Poiché il confronto di `[UnitPrice] <= NULL` restituisce sempre false, viene restituito alcun risultato.

Immettere un valore nella casella di testo, ad esempio 5.00 e fare clic sul pulsante di visualizzazione corrispondenti prodotti. Durante il postback, SqlDataSource indica che una delle relative origini parametro GridView è stato modificato. Di conseguenza, il controllo GridView riassocia a SqlDataSource, visualizzazione di tali prodotti minore o uguale a 5,00 dollari USA.


[![Vengono visualizzati i prodotti minore di o uguale a 5,00 dollari USA](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**Figura 7**: vengono visualizzati i prodotti minore di o uguale a 5,00 dollari USA ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))


## <a name="initially-displaying-all-products"></a>Inizialmente la visualizzazione di tutti i prodotti

Anziché non visualizzare prodotti quando la pagina viene caricata, potremmo voler visualizzare *tutti* prodotti. Uno dei modi per elencare tutti i prodotti ogni volta che il `MaxPrice` nella casella di testo è vuota consiste nell'impostare il valore predefinito di s parametri per un valore elevato incredibilmente, come 1000000, poiché è improbabile che Northwind Traders verrà mai hanno eseguito l'inventario con prezzo unitario s supera $1.000.000. Tuttavia, questo approccio è superficiale e potrebbe non funzionare in altre situazioni.

Nelle esercitazioni precedenti - [parametri dichiarativi](../basic-reporting/declarative-parameters-cs.md) e [Master/Dettagli filtro con un controllo DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) è stavamo affrontare un problema simile. Esiste la soluzione consisteva nel inserire questa logica nel livello della logica di Business. In particolare, il livello BLL esaminato il valore in ingresso e, se fosse `NULL` o alcuni riservati valore, la chiamata è stata indirizzata al metodo DAL che tutti i record restituiti. Se il valore in ingresso è un normale valore di filtro, è stata eseguita una chiamata al metodo che ha eseguito un'istruzione SQL utilizzata una con parametri DAL `WHERE` clausola con il valore fornito.

Sfortunatamente, l'architettura è ignorare quando si usa SqlDataSource. In alternativa, è necessario personalizzare l'istruzione SQL per recuperare in modo intelligente tutti i record se il `@MaximumPrice` parametro è `NULL` o un valore riservato. Per questo esercizio, ti permettono di s è così che, se il `@MaximumPrice` parametro è uguale al `-1.0`, quindi *tutte* dei record devono essere restituiti (`-1.0` funziona come un valore riservato perché nessun prodotto può avere un valore negativo `UnitPrice`valore). A tale scopo è possibile usare l'istruzione SQL seguente:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

Ciò `WHERE` la clausola restituisce *tutte* registra se il `@MaximumPrice` parametro è uguale a `-1.0`. Se non è il valore del parametro `-1.0`, solo i prodotti il cui `UnitPrice` è minore o uguale al `@MaximumPrice` vengono restituiti valore del parametro. Impostando il valore predefinito il `@MaximumPrice` parametro per `-1.0`, il primo caricamento della pagina (o ogni volta che il `MaxPrice` nella casella di testo è vuota), `@MaximumPrice` avrà un valore di `-1.0` e verranno visualizzati tutti i prodotti.


[![A questo punto tutti i prodotti è visualizzato quando il MaxPrice casella di testo è vuota](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**Figura 8**: ora tutti i prodotti vengono visualizzati quando il `MaxPrice` casella di testo è vuota ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))


Esistono un paio di aspetti da notare in questo modo. In primo luogo, tenere presente che il tipo di dati di s parametri viene dedotto dall'utilizzo nella query SQL. Se si modifica il `WHERE` clausola dal `@MaximumPrice = -1.0` a `@MaximumPrice = -1`, il runtime considera il parametro come un numero intero. Se quindi si tenta di assegnare il `MaxPrice` nella casella di testo in un valore decimale (ad esempio 5.00), verrà generato un errore perché non è possibile convertire 5.00 in un numero intero. Per risolvere questo problema, assicurarsi che userai `@MaximumPrice = -1.0` nella `WHERE` clausola o, meglio ancora, imposta il `ControlParameter` oggetto s `Type` proprietà Decimal.

In secondo luogo, aggiungendo il `OR @MaximumPrice = -1.0` per il `WHERE` clausola, il motore di query non è possibile utilizzare un indice per `UnitPrice` (se ne esiste uno), dando come risultato una scansione di tabella. Ciò può influire sulle prestazioni se sono presenti un numero sufficientemente elevato di record nel `Products` tabella. Un approccio migliore, è possibile spostare tale logica in una stored procedure in cui un' `IF` istruzione potrebbe eseguire una `SELECT` eseguire una query dal `Products` tabella senza un `WHERE` clausola quando tutti i record devono essere restituiti o uno cui `WHERE` clausola contiene solo il `UnitPrice` criteri, in modo che un indice può essere usato.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Passaggio 3: Creazione e utilizzo di Stored procedure con parametri

Le stored procedure possono includere un set di parametri di input che possono quindi essere usati nelle istruzioni SQL definite all'interno della stored procedure. Quando si configura il SqlDataSource per usare una stored procedure che accetta i parametri di input, questi valori dei parametri possono essere specificati utilizzando le stesse tecniche come con le istruzioni SQL ad hoc.

Per illustrare l'utilizzo delle stored procedure nel SqlDataSource, s ti permettono di creare una nuova stored procedure nel database Northwind denominato `GetProductsByCategory`, che accetta un parametro denominato `@CategoryID` e restituisce tutte le colonne dei prodotti il cui `CategoryID` corrisponde alla colonna `@CategoryID`. Per creare una stored procedure, passare a Esplora Server e il drill down di `NORTHWND.MDF` database. (Se non si t visualizzare Esplora Server, portarlo selezionando il menu di visualizzazione e selezionando l'opzione di Esplora Server.)

Dal `NORTHWND.MDF` del database, fare doppio clic sulla cartella Stored procedure, scegliere Aggiungi nuova Stored Procedure e immettere la sintassi seguente:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

Scegliere l'icona di salvataggio (o Ctrl + S) per salvare la stored procedure. È possibile testare la stored procedure facendovi dalla cartella di Stored procedure e scegliendo Esegui. Questo richiederà i parametri delle stored procedure s (`@CategoryID`, in questo caso), dopo che i risultati verranno visualizzati nella finestra di Output.


[![Il GetProductsByCategory Stored Procedure quando viene eseguito con un @CategoryID pari a 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**Figura 9**: il `GetProductsByCategory` Stored Procedure quando viene eseguito con un `@CategoryID` pari a 1 ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))


Consentire a s di utilizzare questa stored procedure per visualizzare tutti i prodotti della categoria Beverages in un controllo GridView. Aggiungere un controllo GridView di nuovo alla pagina e associarlo a un nuovo SqlDataSource denominato `BeverageProductsDataSource`. Continuare a specificare una schermata di stored procedure o un'istruzione SQL personalizzata, selezionare il pulsante di opzione di Stored procedure e selezionare il `GetProductsByCategory` stored procedure nell'elenco a discesa.


[![Selezionare il GetProductsByCategory Stored Procedure nell'elenco a discesa](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**Figura 10**: selezionare il `GetProductsByCategory` Stored Procedure nell'elenco a discesa ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))


Poiché la stored procedure accetta un parametro di input (`@CategoryID`), selezione di Avanti richiede di specificare l'origine per il valore del parametro s. Le bibite `CategoryID` è 1, pertanto lasciare l'elenco di riepilogo a discesa Origine parametro None e immettere 1 nella casella di testo DefaultValue.


[![Utilizzare un valore hardcoded pari a 1 per restituire i prodotti della categoria Beverages](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**Figura 11**: utilizzare un valore Hard-Coded pari a 1 per restituire i prodotti della categoria Beverages ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))


Come illustrato nel markup dichiarativo seguente, quando si utilizza una stored procedure, la s SqlDataSource `SelectCommand` viene impostata sul nome della stored procedure e le [ `SelectCommandType` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) è impostato su `StoredProcedure`, a indicare che il `SelectCommand` è il nome di una stored procedure anziché un'istruzione SQL ad hoc.


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

Testare la pagina in un browser. Vengono visualizzati solo i prodotti appartenenti alla categoria di bevande, sebbene *tutti i* del prodotto vengono visualizzati i campi dal `GetProductsByCategory` stored procedure restituisce tutte le colonne dai `Products` tabella. È stato possibile, naturalmente, limitare o personalizzare i campi visualizzati in GridView dalla finestra di dialogo Modifica colonne s GridView.


[![Vengono visualizzate tutte le bibite](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**Figura 12**: vengono visualizzate tutte le bibite ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Passaggio 4: Richiamare a livello di programmazione s SqlDataSource`Select()`istruzione

Gli esempi si ve descritto finora in questa esercitazione e l'esercitazione precedente sono associati i controlli SqlDataSource direttamente in un controllo GridView. I dati di controllo del codice s SqlDataSource, tuttavia, a livello di programmazione accessibili ed enumerati nel codice. Ciò può essere particolarmente utile quando è necessario eseguire query sui dati per esaminarlo, ma è necessario tenere sempre per visualizzarlo. Anziché dover scrivere tutti i boilerplate codice ADO.NET per connettersi al database, specificare il comando e recuperare i risultati, è possibile consentire la gestione di questo codice monotono SqlDataSource.

Per illustrare l'utilizzo di SqlDataSource s dati a livello di codice, si supponga che il capo ha affrontato è con una richiesta per creare una pagina web che visualizza il nome di una categoria selezionata in modo casuale e i relativi prodotti associati. Vale a dire quando un utente visita questa pagina, si vuole scegliere in modo casuale una categoria dal `Categories` di tabella, visualizzare il nome della categoria e quindi elencare i prodotti appartenenti alla categoria selezionata.

Per eseguire questa operazione, è necessario due controlli SqlDataSource uno per ottenere una categoria casuale dal `Categories` tabella e un altro per ottenere la categoria di prodotti di s. Verrà compilata SqlDataSource che recupera un record di categoria casuale in questo passaggio. Passaggio 5 esamina relativi alla creazione SqlDataSource che recupera i prodotti di categoria s.

Iniziare aggiungendo un SqlDataSource per la `ParameterizedQueries.aspx` e impostare relativi `ID` a `RandomCategoryDataSource`. Configurarlo in modo che utilizzi la query SQL seguente:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()` Restituisce i record ordinati in ordine casuale (vedere [Using `NEWID()` per ordinare in modo casuale i record](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` Restituisce il primo record dal set di risultati. Raccolto, questa query restituisce il `CategoryID` e `CategoryName` una categoria single, selezionato in modo casuale i valori della colonna.

Per visualizzare la categoria s `CategoryName` valore, aggiungere un controllo etichetta Web alla pagina, impostare relativi `ID` proprietà `CategoryNameLabel`e cancellare il `Text` proprietà. Per recuperare a livello di codice i dati da un controllo SqlDataSource, è necessario richiamare relativo `Select()` (metodo). Il [ `Select()` metodo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) prevede un singolo parametro di input di tipo [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), che consente di specificare come i dati devono essere elettronica commerciale indesiderati prima di essere restituiti. Ciò può includere istruzioni su come l'ordinamento e filtro dei dati e viene usato per i dati che durante l'ordinamento o paging dei dati da un controllo SqlDataSource, i controlli Web. Per questo esempio, tuttavia, è si necessità di t i dati da modificare prima di essere restituiti e pertanto verrà passato il `DataSourceSelectArguments.Empty` oggetto.

Il `Select()` metodo restituisce un oggetto che implementa `IEnumerable`. Il tipo preciso restituito dipende dal valore del controllo SqlDataSource 1!s [ `DataSourceMode` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Come illustrato nell'esercitazione precedente, questa proprietà può essere impostata su un valore di uno `DataSet` o `DataReader`. Se impostato su `DataSet`, il `Select()` metodo restituisce un [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) oggetto; se impostato su `DataReader`, viene restituito un oggetto che implementa [ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Poiché il `RandomCategoryDataSource` SqlDataSource è relativa `DataSourceMode` impostata su `DataSet` (impostazione predefinita), si userà un oggetto DataView.

Il codice seguente illustra come recuperare i record dal `RandomCategoryDataSource` SqlDataSource come un oggetto DataView, nonché come leggere il `CategoryName` valore di colonna della prima riga della vista dati:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]` Restituisce il primo `DataRowView` in DataView. `randomCategoryView[0]["CategoryName"]` Restituisce il valore della `CategoryName` colonna nella prima riga. Si noti che la visualizzazione di dati non fortemente tipizzato. Per fare riferimento a un valore di colonna specifica è necessario passare il nome della colonna sotto forma di stringa (in questo caso, CategoryName). Figura 13 Mostra il messaggio visualizzato nei `CategoryNameLabel` durante la visualizzazione della pagina. Naturalmente, il nome della categoria effettiva visualizzato è selezionato in modo casuale per il `RandomCategoryDataSource` SqlDataSource ogni visita una pagina (compresi i postback).


[![I dispositivi selezionati casualmente categoria che nome viene visualizzato](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**Figura 13**: s The casualmente categoria selezionata verrà visualizzato nome ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))


> [!NOTE]
> Se il controllo SqlDataSource 1!s `DataSourceMode` proprietà è stata impostata su `DataReader`, il valore restituito dalle `Select()` metodo sarebbe stati necessari per eseguire il cast a `IDataReader`. Per leggere il `CategoryName` utilizzare il valore della colonna della prima riga, è il d codice:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

Con SqlDataSource seleziona casualmente una categoria, sono pronti per aggiungere il controllo GridView in cui sono elencati i prodotti di categoria s.

> [!NOTE]
> Anziché utilizzare un controllo etichetta Web per visualizzare il nome della categoria s, avremmo potuto aggiungere un controllo FormView o DetailsView alla pagina, associarlo a SqlDataSource. Usa l'etichetta, tuttavia, siamo riusciti a esplorare come richiamare a livello di codice la s SqlDataSource `Select()` istruzione e il lavoro con i relativi dati risultanti nel codice.


## <a name="step-5-assigning-parameter-values-programmatically"></a>Passaggio 5: Assegnare i valori dei parametri a livello di codice

Tutti gli esempi si ve descritto finora in questa esercitazione Usa un valore di parametro impostati come hardcoded o a quello eseguito da una delle origini di parametro predefiniti (un valore di stringa di query, un controllo Web nella pagina e così via). Tuttavia, i parametri di s controllo SqlDataSource possono anche essere impostati a livello di codice. Per completare l'esempio corrente, è necessario un SqlDataSource che restituisce tutti i prodotti appartenenti a una categoria specificata. Questo SqlDataSource avrà un `CategoryID` parametro il cui valore deve essere impostata in base il `CategoryID` valore della colonna restituiti dal `RandomCategoryDataSource` SqlDataSource nel `Page_Load` gestore dell'evento.

Iniziare aggiungendo un controllo GridView alla pagina e associarlo a un nuovo SqlDataSource denominato `ProductsByCategoryDataSource`. Proprio come abbiamo fatto nel passaggio 3, configurare SqlDataSource, in modo che richiama la `GetProductsByCategory` stored procedure. Lasciare il parametro origine elenco a discesa elenco impostato su None, ma non si immette un valore predefinito, come verrà impostato questo valore predefinito a livello di codice.


[![Non si specifica un'origine del parametro o valore predefinito](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**Figura 14**: non si specifica un parametro di origine o il valore predefinito ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))


Dopo aver completato la procedura guidata SqlDataSource, markup dichiarativo risultante dovrebbe essere simile al seguente:


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

È possibile assegnare il `DefaultValue` del `CategoryID` parametro livello di programmazione il `Page_Load` gestore dell'evento:


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

Grazie a questa aggiunta, la pagina include un controllo GridView che mostra i prodotti associati alla categoria selezionata in modo casuale.


[![Non si specifica un'origine del parametro o valore predefinito](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**Figura 15**: non si specifica un parametro di origine o il valore predefinito ([fare clic per visualizzare l'immagine con dimensioni normali](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))


## <a name="summary"></a>Riepilogo

SqlDataSource consente agli sviluppatori di pagina definire le query con parametri i cui valori di parametro possono essere impostate come hardcoded, il pull da origini di parametro predefiniti o assegnati a livello di codice. In questa esercitazione è stato illustrato come creare una query con parametri dalla procedura guidata Configura origine dati per le query SQL ad hoc e stored procedure. Abbiamo anche esaminato tramite origini parametro hardcoded, un controllo Web come origine di parametro e a livello di codice che specifica il valore del parametro.

Ad esempio con ObjectDataSource, SqlDataSource offre inoltre funzionalità per modificare i dati sottostanti. Nell'esercitazione successiva esamineremo come definire `INSERT`, `UPDATE`, e `DELETE` istruzioni con SqlDataSource. Dopo aver aggiunto queste istruzioni, che possiamo utilizzare l'oggetto incorporato di inserimento, la modifica e l'eliminazione di funzioni intrinseche per i controlli GridView, DetailsView e FormView.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Scott Clyde Randell Schmidt e Ken Pespisa. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](querying-data-with-the-sqldatasource-control-cs.md)
> [Successivo](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
