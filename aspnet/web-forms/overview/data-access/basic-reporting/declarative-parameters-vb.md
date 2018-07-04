---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Parametri dichiarativi (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione viene verrà illustrato come usare un parametro impostato su un valore hardcoded per selezionare i dati da visualizzare nel controllo DetailsView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 86590faf30b41acdd8a0922f9cc33feabfded23f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397730"
---
<a name="declarative-parameters-vb"></a>Parametri dichiarativi (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) o [Scarica il PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> In questa esercitazione viene verrà illustrato come usare un parametro impostato su un valore hardcoded per selezionare i dati da visualizzare nel controllo DetailsView.


## <a name="introduction"></a>Introduzione

Nel [ultima esercitazione](displaying-data-with-the-objectdatasource-vb.md) è stata esaminata la visualizzazione dei dati con i controlli GridView, DetailsView e FormView associati a un controllo ObjectDataSource che ha richiamato la `GetProducts()` metodo dal `ProductsBLL` classe. Il `GetProducts()` metodo restituisca un DataTable fortemente tipizzate popolato con tutti i record del database di Northwind `Products` tabella. Il `ProductsBLL` classe contiene metodi aggiuntivi per la restituzione solo subset dei prodotti - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, e `GetProductsBySupplierID(supplierID)`. Questi tre metodi prevedono un parametro di input che indica la modalità filtrare le informazioni sul prodotto restituito.

ObjectDataSource è utilizzabile per richiamare i metodi che prevede parametri di input, ma per farlo è necessario indicare anche dove provengono i valori per questi parametri. I valori dei parametri possono essere impostate come hardcoded o possono provenire da svariate origini dinamiche, ad esempio: valori di stringa di query, le variabili di sessione, il valore della proprietà di un controllo Web nella pagina o un altro.

Per questa esercitazione Iniziamo che illustra come usare un parametro impostato su un valore hardcoded. In particolare, esamineremo l'aggiunta di un controllo DetailsView alla pagina che visualizza le informazioni su un prodotto specifico, ovvero combinazione ' s Gumbo Chef Anton, ha un `ProductID` pari a 5. Successivamente, si vedrà come impostare il valore del parametro basato su un controllo Web. In particolare, si userà una casella di testo per consentire all'utente di digitare in un paese, dopo il quale è possibile fare clic su un pulsante per visualizzare l'elenco di fornitori che si trovano in tale paese.

## <a name="using-a-hard-coded-parameter-value"></a>Utilizzo di un valore di parametro a livello di codice

Nel primo esempio, iniziare aggiungendo un controllo DetailsView per la `DeclarativeParams.aspx` nella pagina di `BasicReporting` cartella. Dallo smart tag di DetailsView, selezionare &lt;nuova origine dati&gt; dall'elenco a discesa elenco e scegliere di aggiungere un ObjectDataSource.


[![Aggiungere un ObjectDataSource alla pagina](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Figura 1**: aggiungere un ObjectDataSource alla pagina ([fare clic per visualizzare l'immagine con dimensioni normali](declarative-parameters-vb/_static/image3.png))


Verrà automaticamente avviata Creazione guidata Scegli origine dati del controllo ObjectDataSource. Selezionare il `ProductsBLL` classe dalla schermata prima della procedura guidata.


[![Selezionare la classe ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Figura 2**: selezionare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](declarative-parameters-vb/_static/image6.png))


Poiché si vogliono visualizzare le informazioni relative a un prodotto specifico da utilizzare il `GetProductByProductID(productID)` (metodo).


[![Scegliere il metodo GetProductByProductID(productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Figura 3**: scegliere il `GetProductByProductID(productID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](declarative-parameters-vb/_static/image9.png))


Poiché il metodo che è stato selezionato include un parametro, è una schermata di altre per la procedura guidata, in cui viene richiesto di definire il valore da utilizzare per il parametro. L'elenco a sinistra mostra tutti i parametri per il metodo selezionato. Per la `GetProductByProductID(productID)` è presente un solo `productID`. Nella parte destra è possibile specificare il valore del parametro selezionato. L'elenco di riepilogo a discesa Origine parametro enumera le possibili origini diverse per il valore del parametro. Poiché si vuole specificare un valore hardcoded pari a 5 per il `productID` parametro, lasciare l'origine del parametro None e immettere 5 nella casella di testo DefaultValue.


[![Un Hard-Coded parametro valore di 5 verrà utilizzato per il parametro productID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Figura 4**: A Hard-Coded parametro valore di 5 verrà utilizzato per il `productID` parametro ([fare clic per visualizzare l'immagine con dimensioni normali](declarative-parameters-vb/_static/image12.png))


Dopo aver completato la procedura guidata Configura origine dati, markup dichiarativo del controllo ObjectDataSource include un `Parameter` dell'oggetto nel `SelectParameters` raccolta per ognuno dei parametri di input previsti dal metodo definito nel `SelectMethod` proprietà. Poiché il metodo viene usata in questo esempio prevede solo un singolo parametro di input, `parameterID`, è presente solo una voce di seguito. Il `SelectParameters` raccolta può contenere qualsiasi classe che deriva dal `Parameter` classe la `System.Web.UI.WebControls` dello spazio dei nomi. Per la base i valori di parametro hardcoded `Parameter` classe viene utilizzata, ma per l'altro parametro opzioni un derivato di origine `Parameter` classe viene utilizzata; è anche possibile creare una propria [tipi di parametro personalizzati](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), se necessario.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Se state procedendo nel proprio computer markup dichiarativo viene visualizzato a questo punto maggio includono i valori per il `InsertMethod`, `UpdateMethod`, e `DeleteMethod` delle proprietà, nonché `DeleteParameters`. Procedura guidata Scegli origine dati di ObjectDataSource specifica automaticamente i metodi dal `ProductBLL` da utilizzare per l'inserimento, aggiornamento ed eliminazione, quindi a meno che non è selezionata in modo esplicito quelli out, verranno inclusi nel markup riportato sopra.


Quando si visita questa pagina, i dati di controllo Web richiamerà di ObjectDataSource `Select` metodo, che chiamerà il `ProductsBLL` della classe `GetProductByProductID(productID)` metodo utilizzando il valore hardcoded pari a 5 per il `productID` parametro di input. Il metodo restituirà l'oggetto fortemente tipizzato `ProductDataTable` oggetto che contiene una singola riga con informazioni su Chef Anton's Gumbo Mix (il prodotto con `ProductID` 5).


[![Vengono visualizzate informazioni su Chef Anton's Gumbo Mix](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Figura 5**: vengono visualizzate informazioni su Chef Anton's Gumbo Mix ([fare clic per visualizzare l'immagine con dimensioni normali](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Impostazione del valore di parametro per il valore della proprietà di un controllo Web

Parametri di ObjectDataSource possono essere impostati anche i valori in base al valore di un controllo Web nella pagina. Per illustrare questo concetto, è possibile disporre di un controllo GridView che elenca tutti i fornitori che si trovano in un paese specificato dall'utente. Per completare questa Guida di avvio tramite l'aggiunta di una casella di testo per la pagina in cui l'utente può immettere un nome di paese. Impostare questa casella di testo del controllo `ID` proprietà `CountryName`. Aggiungere anche un controllo pulsante Web.


[![Aggiungere una casella di testo alla pagina con ID CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Figura 6**: aggiungere una casella di testo alla pagina contenente `ID` `CountryName` ([fare clic per visualizzare l'immagine con dimensioni normali](declarative-parameters-vb/_static/image18.png))


Successivamente, aggiungere un controllo GridView alla pagina e, dallo smart tag, scegliere di aggiungere un nuovo oggetto ObjectDataSource. Poiché si vogliono visualizzare supplier informazioni selezionare il `SuppliersBLL` classe dalla schermata prima della procedura guidata. Nella seconda schermata scegliere il `GetSuppliersByCountry(country)` (metodo).


[![Scegliere il metodo GetSuppliersByCountry(country)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Figura 7**: scegliere il `GetSuppliersByCountry(country)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](declarative-parameters-vb/_static/image21.png))


Poiché il `GetSuppliersByCountry(country)` metodo ha un parametro di input, la procedura guidata include ancora una volta una schermata finale per la scelta del valore del parametro. Questa volta, impostare l'origine del parametro al controllo. Questo verrà popolato l'elenco di riepilogo a discesa ControlID con i nomi dei controlli della pagina. Selezionare il `CountryName` controllo dall'elenco. Quando la pagina viene prima visitata la `CountryName` casella sarà vuota, quindi viene restituito alcun risultato e viene visualizzato nulla. Se si desidera visualizzare alcuni risultati per impostazione predefinita, impostare di conseguenza la casella di testo DefaultValue.


[![Impostare il valore del parametro per il valore del controllo CountryName](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Figura 8**: impostare il valore del parametro il `CountryName` valore del controllo ([fare clic per visualizzare l'immagine con dimensioni normali](declarative-parameters-vb/_static/image24.png))


Markup dichiarativo di ObjectDataSource differisce leggermente nel primo esempio, usando un [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) invece dello standard `Parameter` oggetto. Oggetto `ControlParameter` sono disponibili proprietà aggiuntive per specificare il `ID` del controllo Web e il valore della proprietà da utilizzare per il parametro (`PropertyName`). La procedura guidata Configura origine dati è stata in grado di determinare che, per una casella di testo, è probabilmente opportuno usare il `Text` proprietà per il valore del parametro. Se, tuttavia, si desidera usare un altro valore della proprietà dal controllo Web è possibile modificare il `PropertyName` valore qui o facendo clic sul collegamento "Mostra proprietà avanzate" nella procedura guidata.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Quando si visita la pagina per la prima volta il `CountryName` nella casella di testo è vuota. ObjectDataSource `Select` metodo verrà ancora richiamato da GridView, ma un valore di `Nothing` viene passato il `GetSuppliersByCountry(country)` (metodo). Converte l'oggetto TableAdapter il `Nothing` in un database `NULL` valore (`DBNull.Value`), ma la query utilizzata dal `GetSuppliersByCountry(country)` metodo è scritta in modo che non restituisce eventuali valori quando un `NULL` valore è specificato per il `@CategoryID`parametro. In breve, nessun fornitore vengono restituito.

Dopo che il visitatore entra in un paese, tuttavia e fa clic sul pulsante Mostra fornitori per causare un postback, ObjectDataSource `Select` metodo viene rieseguito, passando il controllo TextBox `Text` come valore di `country` parametro.


[![Vengono visualizzati i fornitori del Canada](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Figura 9**: vengono visualizzati i fornitori del Canada ([fare clic per visualizzare l'immagine con dimensioni normali](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Visualizzazione di tutti i fornitori per impostazione predefinita

Anziché mostrare nessuno dei fornitori durante la visualizzazione prima di tutto la pagina potrebbe essere necessario mostrare *tutti* suppliers inizialmente, consentendo all'utente per ridurre l'elenco immettendo un nome del paese nella casella di testo. Quando la casella di testo è vuota, il `SuppliersBLL` della classe `GetSuppliersByCountry(country)` viene passato nel `Nothing` per il *`country`* parametro di input. Ciò `Nothing` valore viene quindi passato verso il basso DAL `GetSupplierByCountry(country)` metodo, in cui viene convertita in un database `NULL` valore per il `@Country` parametro nella query seguente:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

L'espressione `Country = NULL` restituisce sempre False, anche per i record la cui proprietà `Country` colonna contiene un `NULL` valore; pertanto, viene restituito alcun record.

Per restituire *tutti i* fornitori quando il paese nella casella di testo è vuoto, è possibile aumentare le `GetSuppliersByCountry(country)` metodo nel livello BLL per richiamare il `GetSuppliers()` metodo quando il relativo parametro paese è `Nothing` e per chiamare il livello DAL `GetSuppliersByCountry(country)` metodo in caso contrario. Questo avrà l'effetto di restituzione di tutti i fornitori quando non viene specificato alcun paese e il subset appropriato di fornitori quando il paese è incluso.

Modifica il `GetSuppliersByCountry(country)` nel metodo il `SuppliersBLL` classe al seguente:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Con questa modifica il `DeclarativeParams.aspx` pagina Mostra tutti i fornitori quando visitati prima di tutto (o ogni volta che il `CountryName` nella casella di testo è vuota).


[![Tutti i suoi fornitori sono ora visualizzate per impostazione predefinita](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Figura 10**: tutti i suoi fornitori sono ora visualizzate per impostazione predefinita ([fare clic per visualizzare l'immagine con dimensioni normali](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Riepilogo

Per usare i metodi con parametri di input, è necessario specificare i valori per i parametri di ObjectDataSource `SelectParameters` raccolta. Consentono di diversi tipi di parametri per il valore del parametro devono essere ottenuti da origini diverse. Il tipo di parametro predefinito Usa un valore hardcoded, ma altrettanto facilmente (e senza una riga di codice) i valori dei parametri possono essere ottenuti dalla stringa di query, sessione variabili, i cookie e anche immesso dall'utente i valori dai controlli Web nella pagina.

Gli esempi che è stato illustrato in questa esercitazione viene illustrato come usare i valori dei parametri dichiarativi. Tuttavia, potrebbero essere casi quando è necessario usare un'origine del parametro che non è disponibile, ad esempio la data e ora correnti, o, se il sito è stata usata l'appartenenza, l'ID utente del visitatore. Per questi scenari è possibile impostare i valori dei parametri a livello di codice prima di ObjectDataSource richiama metodo relativo dell'oggetto sottostante. Si vedrà come eseguire questa operazione nel [prossima esercitazione](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Hilton Giesenow, revisore per questa esercitazione. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](displaying-data-with-the-objectdatasource-vb.md)
> [Successivo](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
