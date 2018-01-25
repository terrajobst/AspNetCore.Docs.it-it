---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
title: Pulsanti personalizzati in DataList e Repeater (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione creeremo un'interfaccia che utilizza un ripetitore per elencare le categorie nel sistema, con ogni categoria di un pulsante per visualizzare il relativo associ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1afdb14d-6e49-4e1f-aead-2934730d472e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: fc6c297f08790cdcc74867df21e32258017c5a7d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="custom-buttons-in-the-datalist-and-repeater-vb"></a>Pulsanti personalizzati in DataList e Repeater (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_VB.exe) o [Scarica il PDF](custom-buttons-in-the-datalist-and-repeater-vb/_static/datatutorial46vb1.pdf)

> In questa esercitazione creeremo un'interfaccia che utilizza un ripetitore per elencare le categorie nel sistema, con ogni categoria di un pulsante per visualizzare i prodotti associati utilizzando un controllo BulletedList.


## <a name="introduction"></a>Introduzione

Durante le esercitazioni precedenti diciassette DataList e Repeater, abbiamo creato sia esempi di sola lettura e la modifica e l'eliminazione di esempi. Per facilitare la modifica ed eliminazione di funzionalità all'interno di DataList, pulsanti è stato aggiunto a DataList s `ItemTemplate` che, quando si fa clic, ha provocato un postback e ha generato un evento di DataList corrispondente al pulsante s `CommandName` proprietà. Ad esempio, aggiungendo un pulsante per la `ItemTemplate` con un `CommandName` DataList s causa del valore di proprietà di modifica `EditCommand` attiva durante il postback; uno con il `CommandName` eliminare genera il `DeleteCommand`.

Per modificare ed eliminare i pulsanti, inoltre i controlli DataList e Repeater possono includere anche i pulsanti, LinkButton o ImageButtons che, quando si fa clic, eseguire una logica sul lato server personalizzata. In questa esercitazione creeremo un'interfaccia che utilizza un ripetitore per elencare le categorie nel sistema. Per ogni categoria, Ripetitore includerà un pulsante per visualizzare la categoria di prodotti associati alla usando un controllo BulletedList (vedere la figura 1).


[![Fare clic su Mostra prodotti collegamento Visualizza i prodotti s categoria in un elenco puntato](custom-buttons-in-the-datalist-and-repeater-vb/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image1.png)

**Figura 1**: facendo clic su Visualizza collegamento Mostra prodotti la categoria s prodotti in un elenco puntato ([fare clic per visualizzare l'immagine ingrandita](custom-buttons-in-the-datalist-and-repeater-vb/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Passaggio 1: Aggiunta di pagine Web esercitazione pulsante personalizzato

Prima di esaminare come aggiungere un pulsante personalizzato, consentire s attentamente prima di creare le pagine ASP.NET nel progetto sito Web che è necessario per questa esercitazione. Per iniziare, aggiungere una nuova cartella denominata `CustomButtonsDataListRepeater`. Successivamente, aggiungere le seguenti due pagine ASP.NET a quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `CustomButtons.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative pulsanti personalizzate](custom-buttons-in-the-datalist-and-repeater-vb/_static/image4.png)

**Figura 2**: aggiungere le pagine ASP.NET per le esercitazioni relative pulsanti personalizzate


Come in altre cartelle, `Default.aspx` nel `CustomButtonsDataListRepeater` cartella elencherà le esercitazioni nella relativa sezione. Tenere presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Aggiungere il controllo utente `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](custom-buttons-in-the-datalist-and-repeater-vb/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image5.png)

**Figura 3**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente in `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](custom-buttons-in-the-datalist-and-repeater-vb/_static/image7.png))


Infine, aggiungere le pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo il Paging e l'ordinamento e DataList Ripetitore `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, dedicare alcuni minuti per visualizzare il sito Web esercitazioni tramite un browser. Il menu a sinistra include elementi per la modifica, inserimento ed eliminazione di esercitazioni.


![Mappa del sito include ora la voce per l'esercitazione di pulsanti personalizzati](custom-buttons-in-the-datalist-and-repeater-vb/_static/image8.png)

**Figura 4**: mappa del sito include ora la voce per l'esercitazione di pulsanti personalizzati


## <a name="step-2-adding-the-list-of-categories"></a>Passaggio 2: Aggiunta dell'elenco di categorie

Per questa esercitazione è necessario creare un ripetitore che elenca tutte le categorie insieme ai prodotti LinkButton mostra che, quando si fa clic, Visualizza i prodotti della categoria associata s in un elenco puntato. Consente di creare innanzitutto un ripetitore semplice che elenca le categorie nel sistema s. Aprire il `CustomButtons.aspx` nella pagina di `CustomButtonsDataListRepeater` cartella. Trascinare un controllo Repeater dalla casella degli strumenti di progettazione e il set relativo `ID` proprietà `Categories`. Successivamente, creare un nuovo controllo origine dati da smart tag s Repeater. In particolare, creare un nuovo controllo ObjectDataSource denominato `CategoriesDataSource` che consente di selezionare i dati di `CategoriesBLL` classe s `GetCategories()` metodo.


[![Configurare ObjectDataSource per utilizzare il metodo di classe CategoriesBLL s GetCategories()](custom-buttons-in-the-datalist-and-repeater-vb/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image9.png)

**Figura 5**: configurare ObjectDataSource per utilizzare il `CategoriesBLL` classe s `GetCategories()` metodo ([fare clic per visualizzare l'immagine ingrandita](custom-buttons-in-the-datalist-and-repeater-vb/_static/image11.png))


A differenza del controllo DataList, per cui Visual Studio crea un valore predefinito `ItemTemplate` in base all'origine dati, i modelli Repeater s devono essere definiti manualmente. Inoltre, i modelli Repeater s devono essere creati e modificati in modo dichiarativo (ovvero, s non esiste alcuna modifica modelli non opzione nello smart tag Ripetitore s).

Fare clic sulla scheda origine nell'angolo inferiore sinistro e aggiungere un `ItemTemplate` che visualizza il nome di categoria s in un `<h3>` elemento e la relativa descrizione in un paragrafo tag; includono un `SeparatorTemplate` che consente di visualizzare un righello orizzontale (`<hr />`) tra ogni categoria. Aggiungere anche un elemento LinkButton con il relativo `Text` proprietà è impostata su Mostra i prodotti. Dopo aver completato questi passaggi, markup dichiarativo s pagina dovrebbe essere simile al seguente:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample2.aspx)]

Figura 6 mostra la pagina quando viene visualizzato tramite un browser. Viene elencato ogni nome di categoria e una descrizione. Il pulsante Mostra prodotti, quando si fa clic, provoca un postback, ma non ancora eseguire qualsiasi azione.


[![Ogni nome di categoria e la descrizione viene visualizzata, insieme ai prodotti LinkButton Mostra](custom-buttons-in-the-datalist-and-repeater-vb/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image12.png)

**Figura 6**: s categoria ogni nome e la descrizione viene visualizzata, insieme ai prodotti Mostra LinkButton ([fare clic per visualizzare l'immagine ingrandita](custom-buttons-in-the-datalist-and-repeater-vb/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Passaggio 3: Esecuzione sul lato Server logica quando il Mostra prodotti LinkButton viene fatto clic su

Ogni volta che viene fatto clic su un pulsante, LinkButton o ImageButton all'interno di un modello in un controllo DataList o Repeater, si verifica un postback e s DataList o Repeater `ItemCommand` viene generato l'evento. Oltre al `ItemCommand` evento, il controllo DataList controllo può anche generare eventi di un altro più specifici se il pulsante s `CommandName` è impostata su una delle stringhe riservate (Elimina, modifica, Annulla, Update o Select), ma la `ItemCommand` evento è *sempre* generato.

Quando si sceglie un pulsante all'interno di un controllo DataList o Repeater, spesso è necessario passare il pulsante scelto (nel caso che possono esistere più pulsanti all'interno del controllo, ad esempio una modifica di entrambi e pulsante Elimina) e altre informazioni aggiuntive (ad esempio il valore di chiave primaria dell'elemento è stato fatto clic con pulsante). Il pulsante, LinkButton e ImageButton forniscono due proprietà i cui valori vengono passati al `ItemCommand` gestore eventi:

- `CommandName`una stringa in genere utilizzata per identificare ogni pulsante nel modello
- `CommandArgument`in genere utilizzato per contenere il valore di un campo di dati, ad esempio il valore di chiave primario

Per questo esempio, impostare il s LinkButton `CommandName` proprietà ShowProducts e associa il valore di chiave primaria corrente di record s `CategoryID` per il `CommandArgument` proprietà utilizzando la sintassi di associazione dati `CategoryArgument='<%# Eval("CategoryID") %>'`. Dopo aver specificato queste due proprietà, la sintassi dichiarativa s LinkButton dovrebbe essere simile al seguente:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample3.aspx)]

Quando viene scelto il pulsante, si verifica un postback e s DataList o Repeater `ItemCommand` viene generato l'evento. Il gestore dell'evento viene passato sul pulsante s `CommandName` e `CommandArgument` valori.

Creare un gestore eventi per il controllo Repeater s `ItemCommand` evento e notare il secondo parametro passato nel gestore eventi (denominato `e`). Il secondo parametro è di tipo [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) e presenta le seguenti quattro proprietà:

- `CommandArgument`il valore del pulsante selezionato s `CommandArgument` proprietà
- `CommandName`il valore del pulsante s `CommandName` proprietà
- `CommandSource`un riferimento al controllo che è stato fatto clic sul pulsante
- `Item`un riferimento di [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) che contiene il pulsante su cui è stato fatto clic; ogni record associato al controllo Repeater si manifesta come una`RepeaterItem`

Poiché la categoria selezionata s `CategoryID` passato tramite la `CommandArgument` proprietà, è possibile ottenere il set di prodotti associati alla categoria selezionata nel `ItemCommand` gestore dell'evento. Questi prodotti possono quindi essere associati a un controllo BulletedList nel `ItemTemplate` (che è ve aggiungere ancora). Tutto ciò che rimane, quindi, consiste nell'aggiungere BulletedList, farvi riferimento nel `ItemCommand` gestore eventi e associare il set di prodotti per la categoria selezionata, che verranno affrontati nel passaggio 4.

> [!NOTE]
> DataList s `ItemCommand` gestore eventi viene passato un oggetto di tipo [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), che offre le stesse quattro proprietà la `RepeaterCommandEventArgs` classe.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Passaggio 4: Visualizzare i prodotti s categoria selezionata in un elenco puntato

I prodotti della categoria selezionata s possono essere visualizzati in Ripetitore s `ItemTemplate` utilizzando un numero qualsiasi di controlli. È possibile aggiungere che un altro annidati Repeater, DataList, un DropDownList, un controllo GridView e così via. Poiché si desidera visualizzare i prodotti come un elenco puntato, tuttavia, si userà il controllo BulletedList. Restituire il `CustomButtons.aspx` pagina dichiarativo s, aggiungere un controllo BulletedList al `ItemTemplate` dopo LinkButton di prodotti Mostra. Impostare il s BulletedLists `ID` a `ProductsInCategory`. BulletedList Visualizza il valore del campo di dati specificato tramite il `DataTextField` proprietà; poiché il controllo dispone di informazioni sul prodotto associato, impostare il `DataTextField` proprietà `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample4.aspx)]

Nel `ItemCommand` gestore dell'evento, fare riferimento a questo controllo utilizzando `e.Item.FindControl("ProductsInCategory")` e associarlo al set di prodotti associati alla categoria selezionata.


[!code-vb[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample5.vb)]

Prima di eseguire qualsiasi azione nel `ItemCommand` gestore eventi, è s prudente verificare il valore in ingresso `CommandName`. Poiché il `ItemCommand` gestore dell'evento viene generato quando *qualsiasi* si fa clic sul pulsante, se sono presenti più pulsanti in uso il modello di `CommandName` valore distinguere l'azione da intraprendere. Verifica il `CommandName` Ecco opinabile, poiché è disponibile un solo pulsante, ma è un'operazione consigliabile modulo. Successivamente, il `CategoryID` della categoria selezionata viene recuperato dal `CommandArgument` proprietà. Il controllo BulletedList nel modello viene quindi a cui fa riferimento e associato ai risultati del `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` metodo.

Nelle esercitazioni precedenti utilizzati i pulsanti all'interno di DataList, ad esempio [una panoramica di modifica e l'eliminazione di dati in DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md), è stato determinato il valore di chiave primaria di un determinato elemento tramite il `DataKeys` insieme. Questo approccio funziona bene con DataList, Repeater non dispone di un `DataKeys` proprietà. In alternativa, è necessario utilizzare un approccio alternativo per fornire il valore di chiave primaria, ad esempio tramite il pulsante s `CommandArgument` proprietà o assegnando il valore della chiave primario a un controllo etichetta Web nascosto all'interno del modello e di leggerne il valore nel `ItemCommand`gestore eventi utilizzando `e.Item.FindControl("LabelID")`.

Dopo aver completato il `ItemCommand` gestore eventi, è opportuno testare questa pagina in un browser. Come illustrato nella figura 7, facendo clic su Mostra prodotti collegamento provoca un postback e visualizza i prodotti per la categoria selezionata in un BulletedList. Inoltre, si noti che queste informazioni sul prodotto rimane, anche se altre categorie di collegamenti di visualizzare i prodotti vengono selezionati.

> [!NOTE]
> Se si desidera modificare il comportamento di questo report, tale che sono elencati i prodotti solo una categoria s alla volta, è sufficiente impostare il controllo di BulletedList s `EnableViewState` proprietà `False`.


[![Un BulletedList viene utilizzato per visualizzare i prodotti della categoria selezionata](custom-buttons-in-the-datalist-and-repeater-vb/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image15.png)

**Figura 7**: BulletedList A viene utilizzato per visualizzare i prodotti della categoria selezionata ([fare clic per visualizzare l'immagine ingrandita](custom-buttons-in-the-datalist-and-repeater-vb/_static/image17.png))


## <a name="summary"></a>Riepilogo

I controlli DataList e Repeater possono includere un numero qualsiasi di pulsanti, LinkButton o ImageButtons all'interno dei modelli. Questi pulsanti, quando si fa clic, provocare un postback e generare il `ItemCommand` evento. Per associare un pulsante di azione sul lato server personalizzato, creare un gestore eventi per il `ItemCommand` evento. In questo gestore eventi, controllare innanzi tutto in ingresso `CommandName` valore per determinare quale pulsante è stato fatto clic. Informazioni aggiuntive, facoltativamente, possono essere fornite tramite il pulsante s `CommandArgument` proprietà.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Dennis Patterson. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](custom-buttons-in-the-datalist-and-repeater-cs.md)
