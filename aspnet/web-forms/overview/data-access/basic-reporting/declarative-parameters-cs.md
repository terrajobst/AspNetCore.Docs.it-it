---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
title: Parametri dichiarativi (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione verrà illustrato come utilizzare un parametro impostato su un valore hardcoded per selezionare i dati da visualizzare in un controllo DetailsView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 603c9bd3-b895-4ec6-853b-0c81ff36d580
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
msc.type: authoredcontent
ms.openlocfilehash: 840630852d28f49f4f4387f1d2cc6b275b468fc2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875939"
---
<a name="declarative-parameters-c"></a>Parametri dichiarativi (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_5_CS.exe) o [Scarica il PDF](declarative-parameters-cs/_static/datatutorial05cs1.pdf)

> In questa esercitazione verrà illustrato come utilizzare un parametro impostato su un valore hardcoded per selezionare i dati da visualizzare in un controllo DetailsView.


## <a name="introduction"></a>Introduzione

Nel [ultima esercitazione](displaying-data-with-the-objectdatasource-cs.md) abbiamo esaminato la visualizzazione dei dati con i controlli di GridView, DetailsView e FormView associati a un controllo ObjectDataSource che ha richiamato il `GetProducts()` metodo la `ProductsBLL` classe. Il `GetProducts()` il metodo restituisce un oggetto DataTable fortemente tipizzato popolato con tutti i record dal database di Northwind `Products` tabella. Il `ProductsBLL` classe contiene metodi aggiuntivi per la restituzione solo subset dei prodotti - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, e `GetProductsBySupplierID(supplierID)`. Questi tre metodi prevedono un parametro di input che indica la modalità filtrare le informazioni sul prodotto restituito.

ObjectDataSource può essere utilizzato per richiamare i metodi che prevede parametri di input, ma per farlo è necessario specificare da dove provengono i valori per questi parametri. I valori dei parametri possono essere hard-coded o possono provenire da un'ampia gamma di origini dinamiche, ad esempio: valori stringa di query, variabili di sessione, il valore della proprietà di un controllo Web nella pagina o ad altri utenti.

Per questa esercitazione innanzitutto che illustrano come utilizzare un parametro impostato su un valore hardcoded. In particolare, vedremo durante l'aggiunta di un controllo DetailsView alla pagina che visualizza informazioni su un prodotto specifico, vale a dire ' Chef Anton s Gumbo Mix, che ha un `ProductID` pari a 5. Successivamente, vedremo come impostare il valore del parametro in base a un controllo Web. In particolare, si userà una casella di testo per consentire all'utente di tipo in un paese, dopo il quale è possibile fare clic su un pulsante per visualizzare l'elenco dei fornitori che si trovano in tale paese.

## <a name="using-a-hard-coded-parameter-value"></a>Utilizzo di un valore di parametro hardcoded

Per il primo esempio, per iniziare, aggiungere un controllo DetailsView per il `DeclarativeParams.aspx` nella pagina di `BasicReporting` cartella. Smart tag del controllo DetailsView selezionare &lt;nuova origine dati&gt; dall'elenco a discesa elenco e scegliere di aggiungere un ObjectDataSource.


[![Aggiungere un ObjectDataSource alla pagina](declarative-parameters-cs/_static/image2.png)](declarative-parameters-cs/_static/image1.png)

**Figura 1**: aggiungere un ObjectDataSource ([fare clic per visualizzare l'immagine ingrandita](declarative-parameters-cs/_static/image3.png))


Procedura guidata Scegli origine dati del controllo ObjectDataSource verrà avviata automaticamente. Selezionare la `ProductsBLL` classe dalla schermata prima della procedura guidata.


[![Selezionare la classe ProductsBLL](declarative-parameters-cs/_static/image5.png)](declarative-parameters-cs/_static/image4.png)

**Figura 2**: selezionare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine ingrandita](declarative-parameters-cs/_static/image6.png))


Poiché si desidera visualizzare informazioni su un prodotto specifico che si desidera usare il `GetProductByProductID(productID)` metodo.


[![Scegliere il metodo GetProductByProductID(productID)](declarative-parameters-cs/_static/image8.png)](declarative-parameters-cs/_static/image7.png)

**Figura 3**: scegliere il `GetProductByProductID(productID)` metodo ([fare clic per visualizzare l'immagine ingrandita](declarative-parameters-cs/_static/image9.png))


Poiché il metodo selezionati è incluso un parametro, sussiste una schermata di altre per la procedura guidata, in cui viene chiesto di definire il valore da utilizzare per il parametro. L'elenco a sinistra mostra tutti i parametri per il metodo selezionato. Per `GetProductByProductID(productID)` è presente un solo `productID`. Nella parte destra è possibile specificare il valore del parametro selezionato. L'elenco di riepilogo a discesa Origine parametro enumera le possibili origini diverse per il valore del parametro. Poiché si desidera specificare un valore hardcoded di 5 per il `productID` parametro, lasciare l'origine del parametro come nessuno e immettere 5 nella casella di testo DefaultValue.


[![Un Hard-Coded parametro valore di 5 verrà utilizzato per il parametro productID](declarative-parameters-cs/_static/image11.png)](declarative-parameters-cs/_static/image10.png)

**Figura 4**: A Hard-Coded parametro valore di 5 verrà utilizzato per il `productID` parametro ([fare clic per visualizzare l'immagine ingrandita](declarative-parameters-cs/_static/image12.png))


Dopo aver completato la configurazione guidata origine dati, markup dichiarativo del controllo ObjectDataSource include un `Parameter` oggetto di `SelectParameters` raccolta per ognuno dei parametri di input previsti dal metodo definito nel `SelectMethod` proprietà. Poiché il metodo viene usata in questo esempio prevede solo un singolo parametro di input, `parameterID`, è presente solo una voce di seguito. Il `SelectParameters` raccolta può contenere qualsiasi classe che deriva dal `Parameter` classe il `System.Web.UI.WebControls` dello spazio dei nomi. Per i valori di parametro hardcoded base `Parameter` classe viene utilizzata, ma per l'altro parametro origine opzioni di un oggetto derivato `Parameter` classe viene utilizzata, è possibile creare anche una propria [tipi di parametri personalizzati](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), se necessario.

[!code-aspx[Main](declarative-parameters-cs/samples/sample1.aspx)]

> [!NOTE]
> Se state procedendo nel proprio computer dichiarativo vedrai a questo punto possono includere valori per il `InsertMethod`, `UpdateMethod`, e `DeleteMethod` proprietà, così come `DeleteParameters`. Procedura guidata Scegli origine dati di ObjectDataSource specifica automaticamente i metodi di `ProductBLL` per l'inserimento, aggiornamento ed eliminazione, in modo da utilizzare a meno che non è deselezionata in modo esplicito i timeout, verrà incluso nel codice precedente.


Durante la visita questa pagina, i dati di controllo Web richiamerà ObjectDataSource `Select` metodo, che chiamerà la `ProductsBLL` della classe `GetProductByProductID(productID)` metodo usando il valore hardcoded di 5 per il `productID` parametro di input. Il metodo restituirà l'oggetto fortemente tipizzato `ProductDataTable` oggetto che contiene una sola riga con informazioni su Chef Anton's Gumbo Mix (il prodotto con `ProductID` 5).


[![Vengono visualizzate informazioni su Chef Anton's Gumbo Mix](declarative-parameters-cs/_static/image14.png)](declarative-parameters-cs/_static/image13.png)

**Figura 5**: vengono visualizzate informazioni su Chef Anton's Gumbo Mix ([fare clic per visualizzare l'immagine ingrandita](declarative-parameters-cs/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Impostazione del valore di parametro per il valore della proprietà di un controllo Web

Parametro di ObjectDataSource valori possono essere impostati anche in base al valore di un controllo Web nella pagina. Per illustrare questo concetto, si hanno un controllo GridView in cui sono elencati tutti i fornitori che si trovano in un paese specificato dall'utente. Per eseguire l'avvio, aggiungere una casella di testo alla pagina in cui l'utente può immettere un nome di paese. Impostare questo valore del controllo TextBox `ID` proprietà `CountryName`. Inoltre, aggiungere un controllo pulsante Web.


[![Aggiungere una casella di testo con ID CountryName](declarative-parameters-cs/_static/image17.png)](declarative-parameters-cs/_static/image16.png)

**Figura 6**: aggiungere una casella di testo alla pagina contenente `ID` `CountryName` ([fare clic per visualizzare l'immagine ingrandita](declarative-parameters-cs/_static/image18.png))


Successivamente, aggiungere un controllo GridView alla pagina e, dallo smart tag, scegliere di aggiungere un nuovo oggetto ObjectDataSource. Poiché si desidera visualizzare fornitore informazioni selezionare il `SuppliersBLL` classe dalla schermata prima della procedura guidata. Dalla seconda schermata scegliere il `GetSuppliersByCountry(country)` metodo.


[![Scegliere il metodo GetSuppliersByCountry(country)](declarative-parameters-cs/_static/image20.png)](declarative-parameters-cs/_static/image19.png)

**Figura 7**: scegliere il `GetSuppliersByCountry(country)` metodo ([fare clic per visualizzare l'immagine ingrandita](declarative-parameters-cs/_static/image21.png))


Poiché il `GetSuppliersByCountry(country)` metodo ha un parametro di input, la procedura guidata include ancora una volta una schermata finale per la scelta del valore del parametro. Questa volta, impostare l'origine del parametro di controllo. Questo verrà popolato l'elenco di riepilogo a discesa ControlID con i nomi dei controlli della pagina. Selezionare il `CountryName` controllo dall'elenco. Quando viene innanzitutto visitata la `CountryName` casella di testo sarà vuoto, viene restituito alcun risultato e viene visualizzato nulla. Se si desidera visualizzare alcuni risultati per impostazione predefinita, impostare di conseguenza la casella di testo DefaultValue.


[![Impostare il valore del parametro per il valore del controllo CountryName](declarative-parameters-cs/_static/image23.png)](declarative-parameters-cs/_static/image22.png)

**Figura 8**: impostare il valore del parametro il `CountryName` valore Control ([fare clic per visualizzare l'immagine ingrandita](declarative-parameters-cs/_static/image24.png))


Markup dichiarativo di ObjectDataSource differisce leggermente dal primo esempio, utilizzando un [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) anziché lo standard `Parameter` oggetto. Oggetto `ControlParameter` dispone di proprietà aggiuntive per specificare il `ID` del controllo Web e il valore della proprietà da utilizzare per il parametro (`PropertyName`). La configurazione guidata origine dati è stata abbastanza per determinare che, per una casella di testo è consisterà probabilmente nell'utilizzare il `Text` proprietà per il valore del parametro. Se, tuttavia, si desidera utilizzare un altro valore della proprietà dal controllo Web è possibile modificare il `PropertyName` valore qui o facendo clic sul collegamento "Mostra proprietà avanzate" della procedura guidata.

[!code-aspx[Main](declarative-parameters-cs/samples/sample2.aspx)]

Quando si visita la pagina per la prima volta il `CountryName` casella di testo è vuota. ObjectDataSource `Select` metodo ancora viene richiamato da GridView, ma un valore di `null` viene passato il `GetSuppliersByCountry(country)` metodo. Converte l'oggetto TableAdapter il `null` in un database `NULL` valore (`DBNull.Value`), ma la query utilizzata dal `GetSuppliersByCountry(country)` metodo è scritta in modo che non restituisce i valori quando un `NULL` sia stato specificato per il `@CategoryID`parametro. In breve, non vengono restituiti fornitori.

Una volta il visitatore entra in un paese, tuttavia e fa clic sul pulsante Mostra fornitori per generare un postback, ObjectDataSource `Select` viene rieseguita (metodo), passando il controllo TextBox `Text` valore come il `country` parametro.


[![Vengono visualizzati i fornitori del Canada](declarative-parameters-cs/_static/image26.png)](declarative-parameters-cs/_static/image25.png)

**Figura 9**: vengono visualizzati i fornitori del Canada ([fare clic per visualizzare l'immagine ingrandita](declarative-parameters-cs/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Visualizzazione di tutti i fornitori per impostazione predefinita

Invece di nessuno dei fornitori la prima visualizzazione della pagina potrebbe essere necessario mostrare *tutti* suppliers inizialmente, consentendo all'utente di ridurre l'elenco, specificare un nome di paese nella casella di testo. Quando la casella di testo è vuota, il `SuppliersBLL` della classe `GetSuppliersByCountry(country)` viene passato una `null` valore per il relativo *`country`* parametro di input. Questo `null` valore viene quindi passato verso il basso il campo DAL `GetSupplierByCountry(country)` (metodo), in cui viene convertita in un database `NULL` valore per il `@Country` parametro nella query seguente:

[!code-sql[Main](declarative-parameters-cs/samples/sample3.sql)]

L'espressione `Country = NULL` restituisce sempre False, anche per i record il cui `Country` colonna ha un `NULL` valore; pertanto, viene restituito alcun record.

Per restituire *tutti* suppliers quando il casella di testo paese è vuoto, è possibile aumentare la `GetSuppliersByCountry(country)` metodo BLL per richiamare il `GetSuppliers()` metodo quando il relativo parametro paese è `null` e di chiamare DAL `GetSuppliersByCountry(country)` metodo in caso contrario. Ciò avrà l'effetto di restituzione di tutti i fornitori quando non viene specificato alcun paese e il subset appropriato di fornitori quando il parametro paese è incluso.

Modifica il `GetSuppliersByCountry(country)` metodo la `SuppliersBLL` classe per le operazioni seguenti:

[!code-csharp[Main](declarative-parameters-cs/samples/sample4.cs)]

Con questa modifica il `DeclarativeParams.aspx` pagina vengono visualizzati tutti i fornitori quando prima visita (o ogni volta che il `CountryName` casella di testo è vuota).


[![Tutti i fornitori sono ora visualizzati per impostazione predefinita](declarative-parameters-cs/_static/image29.png)](declarative-parameters-cs/_static/image28.png)

**Figura 10**: tutti i fornitori sono ora visualizzati per impostazione predefinita ([fare clic per visualizzare l'immagine ingrandita](declarative-parameters-cs/_static/image30.png))


## <a name="summary"></a>Riepilogo

Per utilizzare i metodi con parametri di input, è necessario specificare i valori per i parametri in ObjectDataSource `SelectParameters` insieme. Consentono di diversi tipi di parametri per il valore del parametro deve essere ottenuta da origini diverse. Il tipo di parametro predefinito viene utilizzato un valore hardcoded, ma solo come facilmente, senza una riga di codice, i valori di parametro possono essere ottenuti da querystring, sessione variabili, i cookie e anche immesso dall'utente, i valori dai controlli Web nella pagina.

Negli esempi che è stato esaminato in questa esercitazione viene illustrato come utilizzare i valori dei parametri dichiarativa. Tuttavia, potrebbe essere è necessario utilizzare un'origine di parametro che non è disponibile, ad esempio la data e ora correnti oppure, se il sito è stato mediante l'appartenenza, l'ID utente del visitatore. Per questi scenari è possibile impostare i valori dei parametri a livello di codice prima di ObjectDataSource richiamando il metodo dell'oggetto sottostante. Vedremo come effettuare questa operazione nel [esercitazione successiva](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione Hilton Giesenow. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](displaying-data-with-the-objectdatasource-cs.md)
> [Successivo](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
