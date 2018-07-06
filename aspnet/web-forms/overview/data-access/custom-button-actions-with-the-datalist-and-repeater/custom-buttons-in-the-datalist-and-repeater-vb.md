---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
title: Pulsanti personalizzati con DataList e Repeater (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà creata un'interfaccia che utilizza un controllo Repeater per visualizzare l'elenco di categorie nel sistema, con ogni categoria che fornisce un pulsante per visualizzare il associ...
ms.author: aspnetcontent
ms.date: 11/13/2006
ms.assetid: 1afdb14d-6e49-4e1f-aead-2934730d472e
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: ab580a706b76325fc4c0eccfc130ffa7db22fbd3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812951"
---
<a name="custom-buttons-in-the-datalist-and-repeater-vb"></a>Pulsanti personalizzati con DataList e Repeater (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_VB.exe) o [Scarica il PDF](custom-buttons-in-the-datalist-and-repeater-vb/_static/datatutorial46vb1.pdf)

> In questa esercitazione creeremo un'interfaccia che utilizza un controllo Repeater per visualizzare l'elenco di categorie nel sistema, con ogni categoria che fornisce un pulsante per visualizzare i prodotti connessi con un controllo BulletedList.


## <a name="introduction"></a>Introduzione

Durante le esercitazioni precedenti diciassette DataList e Repeater, abbiamo creato sia esempi di sola lettura e modifica e l'eliminazione di esempi. Per facilitare la modifica ed eliminazione di funzionalità all'interno di un controllo DataList, i pulsanti sono aggiunte a s DataList `ItemTemplate` che, quando si fa clic, ha provocato un postback e ha generato un evento di DataList corrispondente al pulsante s `CommandName` proprietà. Ad esempio, aggiungendo un pulsante per il `ItemTemplate` con un `CommandName` causa del valore di proprietà di modifica di DataList s `EditCommand` alla quale attivare postback; uno con il `CommandName` Delete genera il `DeleteCommand`.

Per modificare ed eliminare i pulsanti, anche i controlli DataList e Repeater possono includere anche i pulsanti, i controlli LinkButton o ImageButtons che, quando si fa clic, eseguire una logica personalizzata lato server. In questa esercitazione creeremo un'interfaccia che utilizza un controllo Repeater per visualizzare l'elenco di categorie nel sistema. Per ogni categoria, il controllo Repeater includerà un pulsante per visualizzare la categoria di prodotti associati alla usando un controllo BulletedList (vedere la figura 1).


[![Facendo clic su Mostra i prodotti collegamento Visualizza i prodotti s categoria in un elenco puntato](custom-buttons-in-the-datalist-and-repeater-vb/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image1.png)

**Figura 1**: fare clic su Visualizza collegamento Mostra prodotti la categoria s prodotti in un elenco puntato ([fare clic per visualizzare l'immagine con dimensioni normali](custom-buttons-in-the-datalist-and-repeater-vb/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Passaggio 1: Aggiunta di pagine Web di esercitazione pulsante personalizzato

Prima di esaminare come aggiungere un pulsante personalizzato, consentire s prima di tutto si consiglia di creare le pagine ASP.NET nel progetto di sito Web che è necessario per questa esercitazione. Iniziare aggiungendo una nuova cartella denominata `CustomButtonsDataListRepeater`. Successivamente, aggiungere le seguenti due pagine ASP.NET per quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `CustomButtons.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative ai pulsanti personalizzate](custom-buttons-in-the-datalist-and-repeater-vb/_static/image4.png)

**Figura 2**: aggiungere le pagine ASP.NET per le esercitazioni relative ai pulsanti personalizzate


In altre cartelle, analogo a `Default.aspx` nella `CustomButtonsDataListRepeater` cartella elencherà le esercitazioni nella relativa sezione. Si tenga presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Aggiungere questo controllo utente alla `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](custom-buttons-in-the-datalist-and-repeater-vb/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image5.png)

**Figura 3**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](custom-buttons-in-the-datalist-and-repeater-vb/_static/image7.png))


Infine, aggiungere le pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo il Paging e ordinamento con DataList e Repeater `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, si consiglia di visualizzare il sito Web di esercitazioni tramite un browser. Il menu a sinistra ora include elementi per la modifica, inserimento ed eliminazione di esercitazioni.


![Mappa del sito include ora la voce per l'esercitazione di pulsanti personalizzati](custom-buttons-in-the-datalist-and-repeater-vb/_static/image8.png)

**Figura 4**: la mappa del sito include ora la voce per l'esercitazione di pulsanti personalizzati


## <a name="step-2-adding-the-list-of-categories"></a>Passaggio 2: Aggiunta nell'elenco delle categorie

Per questa esercitazione è necessario creare un controllo Repeater che elenca tutte le categorie e LinkButton prodotti mostra che, quando si fa clic, consente di visualizzare i prodotti di categoria associata s in un elenco puntato. Consentire s prima di tutto creare un semplice controllo Repeater che elenca le categorie nel sistema. Iniziare aprendo il `CustomButtons.aspx` nella pagina di `CustomButtonsDataListRepeater` cartella. Trascinare un controllo Repeater dalla casella degli strumenti nella finestra di progettazione e set relativo `ID` proprietà `Categories`. Successivamente, creare un nuovo controllo origine dati nello smart tag s Repeater. In particolare, creare un nuovo controllo ObjectDataSource denominato `CategoriesDataSource` che consente di selezionare i dati dal `CategoriesBLL` classe s `GetCategories()` (metodo).


[![Configurare ObjectDataSource per usare il metodo di classe CategoriesBLL s GetCategories()](custom-buttons-in-the-datalist-and-repeater-vb/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image9.png)

**Figura 5**: configurare ObjectDataSource per usare il `CategoriesBLL` classe s `GetCategories()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](custom-buttons-in-the-datalist-and-repeater-vb/_static/image11.png))


A differenza del controllo DataList, per cui Visual Studio crea un valore predefinito `ItemTemplate` basato sull'origine dati, i modelli di Repeater s devono essere definiti manualmente. Inoltre, i modelli di Repeater s devono essere creati e modificati in modo dichiarativo (vale a dire, 3!s ci alcuna modifica modelli non opzione nello smart tag Repeater s).

Fare clic sulla scheda origine nell'angolo inferiore sinistro e aggiungere un `ItemTemplate` che visualizza il nome della categoria s in un `<h3>` elemento e la rispettiva descrizione in un paragrafo di tag, includere una `SeparatorTemplate` che consente di visualizzare un righello orizzontale (`<hr />`) tra ogni categoria. Aggiungere anche un elemento LinkButton con il relativo `Text` proprietà è impostata su Mostra i prodotti. Dopo aver completato questi passaggi, il markup dichiarativo s pagina dovrebbe essere simile al seguente:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample2.aspx)]

Figura 6 mostra la pagina quando viene visualizzato tramite un browser. Viene elencato ogni nome di categoria e una descrizione. Pulsante Mostra prodotti, quando si fa clic, causa un postback, ma non esegue ancora alcuna operazione.


[![Ogni nome di categoria e la descrizione viene visualizzato, insieme a mostrare LinkButton prodotti](custom-buttons-in-the-datalist-and-repeater-vb/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image12.png)

**Figura 6**: ogni categoria s nome e descrizione viene visualizzato, insieme a mostrare LinkButton prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](custom-buttons-in-the-datalist-and-repeater-vb/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Passaggio 3: Esecuzione lato Server per la logica quando il Mostra prodotti LinkButton si fa clic su

Ogni volta che viene scelto un pulsante, LinkButton o ImageButton all'interno di un modello in un controllo DataList o Repeater, si verifica un postback e il controllo DataList o Repeater s `ItemCommand` viene generato l'evento. Oltre al `ItemCommand` evento, DataList controllo può anche generare un altro evento più specifico se il pulsante s `CommandName` è impostata su una delle stringhe riservate (Elimina, modifica, Cancel, Update o Select), ma il `ItemCommand` evento è *sempre* generato.

Quando viene selezionato un pulsante all'interno di un controllo DataList o Repeater, spesso è necessario passare il pulsante scelto (nel caso che potrebbero esserci più pulsanti all'interno del controllo, ad esempio sia una modifica e sul pulsante Elimina) e magari alcune informazioni aggiuntive (ad esempio il valore di chiave primaria dell'elemento è stato fatto clic sul cui pulsante). Il pulsante LinkButton e ImageButton fornisce due proprietà i cui valori vengono passati al `ItemCommand` gestore dell'evento:

- `CommandName` una stringa in genere utilizzata per identificare ogni pulsante del modello
- `CommandArgument` in genere utilizzata per contenere il valore di un campo di dati, ad esempio il valore della chiave primaria

Per questo esempio, impostare la s LinkButton `CommandName` proprietà ShowProducts e associare il valore di chiave primaria corrente di record s `CategoryID` per il `CommandArgument` proprietà usando la sintassi di associazione dati `CategoryArgument='<%# Eval("CategoryID") %>'`. Dopo aver specificato queste due proprietà, la sintassi dichiarativa s LinkButton dovrebbe essere simile al seguente:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample3.aspx)]

Quando fa clic sul pulsante, si verifica un postback e il controllo DataList o Repeater s `ItemCommand` viene generato l'evento. Il gestore dell'evento viene passato sul pulsante s `CommandName` e `CommandArgument` valori.

Creare un gestore eventi per il controllo Repeater s `ItemCommand` eventi e notare il secondo parametro passato nel gestore dell'evento (denominato `e`). Il secondo parametro è di tipo [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) e presenta le seguenti quattro proprietà:

- `CommandArgument` il valore del pulsante selezionato s `CommandArgument` proprietà
- `CommandName` il valore del pulsante s `CommandName` proprietà
- `CommandSource` un riferimento al controllo che è stato fatto clic sul pulsante
- `Item` un riferimento per la [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) che contiene il pulsante su cui è stato fatto clic; è rappresentata da ogni record associato al controllo Repeater un `RepeaterItem`

Poiché la categoria selezionata s `CategoryID` viene passato tramite il `CommandArgument` proprietà, è possibile ottenere il set di prodotti associati alla categoria selezionata nel `ItemCommand` gestore dell'evento. Questi prodotti possono quindi essere associati a un controllo BulletedList nel `ItemTemplate` (quale abbiamo ve ancora da aggiungere). Tutto ciò che rimane, quindi, consiste nell'aggiungere BulletedList, farvi riferimento nel `ItemCommand` gestore eventi e associare ad esso il set di prodotti per la categoria selezionata, che verranno affrontati nel passaggio 4.

> [!NOTE]
> S DataList `ItemCommand` gestore dell'evento viene passato un oggetto di tipo [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), che offre le stesse quattro proprietà il `RepeaterCommandEventArgs` classe.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Passaggio 4: Visualizzare i prodotti s categoria selezionata in un elenco puntato

I prodotti della categoria selezionata s possono essere visualizzati all'interno di Repeater s `ItemTemplate` usando qualsiasi numero di controlli. È stato possibile aggiungere che un altro annidati Repeater, DataList, un controllo DropDownList, un controllo GridView e così via. Poiché si vogliono visualizzare i prodotti come un elenco puntato, tuttavia, si userà il controllo BulletedList. Restituzione per il `CustomButtons.aspx` pagina markup dichiarativo s, aggiungere un controllo BulletedList il `ItemTemplate` dopo il LinkButton Mostra i prodotti. Impostare la s BulletedLists `ID` a `ProductsInCategory`. BulletedList Visualizza il valore del campo dati specificato tramite il `DataTextField` proprietà; poiché questo controllo contiene informazioni sui prodotti a esso associati, impostare il `DataTextField` proprietà `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample4.aspx)]

Nel `ItemCommand` gestore dell'evento, fare riferimento a questo controllo tramite `e.Item.FindControl("ProductsInCategory")` e associarlo al set di prodotti associati alla categoria selezionata.


[!code-vb[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample5.vb)]

Prima di eseguire qualsiasi azione nella `ItemCommand` gestore eventi, è s prudente prima di tutto controllare il valore della matrice in ingresso `CommandName`. Poiché il `ItemCommand` gestore dell'evento viene attivata quando si *qualsiasi* si fa clic sul pulsante, se sono presenti i pulsanti più in uso il modello di `CommandName` valore distinguere l'azione da intraprendere. Verifica la `CommandName` Ecco perde di valore, poiché sono presenti solo un singolo pulsante, ma è un'operazione consigliabile al form. Successivamente, il `CategoryID` della categoria selezionata viene recuperato dal `CommandArgument` proprietà. Il controllo BulletedList nel modello viene quindi fatto riferimento e associato ai risultati del `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (metodo).

Nelle esercitazioni precedenti che utilizzati i pulsanti all'interno di un controllo DataList, come [una panoramica di modifica e l'eliminazione dei dati in DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md), è stato determinato il valore di chiave primaria di un elemento specificato tramite la `DataKeys` raccolta. Sebbene questo approccio funziona bene con DataList, Repeater non ha un `DataKeys` proprietà. In alternativa, è necessario usare un approccio alternativo per specificare il valore di chiave primaria, ad esempio tramite il pulsante s `CommandArgument` proprietà o assegnando il valore della chiave primaria a un controllo etichetta Web nascosto all'interno del modello e lettura del valore corrispondente nella `ItemCommand`gestore di evento utilizzando `e.Item.FindControl("LabelID")`.

Dopo aver completato la `ItemCommand` gestore eventi, si consiglia di testare questa pagina in un browser. Come illustrato nella figura 7, facendo clic su Mostra prodotti collegamento determina un postback e visualizza i prodotti per la categoria selezionata in un BulletedList. Si noti inoltre che le informazioni sui prodotti rimanga, anche se altre categorie di collegamenti di visualizzare i prodotti vengono selezionati.

> [!NOTE]
> Se si desidera modificare il comportamento di questo report, in modo che i prodotti solo una categoria s sono elencati in un momento, è sufficiente impostare il controllo BulletedList 1!s `EnableViewState` proprietà `False`.


[![Un BulletedList viene usato per visualizzare i prodotti della categoria selezionata](custom-buttons-in-the-datalist-and-repeater-vb/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image15.png)

**Figura 7**: oggetto BulletedList viene usato per visualizzare i prodotti della categoria selezionata ([fare clic per visualizzare l'immagine con dimensioni normali](custom-buttons-in-the-datalist-and-repeater-vb/_static/image17.png))


## <a name="summary"></a>Riepilogo

I controlli DataList e Repeater possono includere qualsiasi numero di pulsanti, i controlli LinkButton o ImageButtons all'interno dei modelli. Questi pulsanti, quando si fa clic, possono causare un postback e generare il `ItemCommand` evento. Per associare un pulsante di selezione azione personalizzata sul lato server, creare un gestore eventi per il `ItemCommand` evento. In questo gestore eventi, controllare innanzi tutto in ingresso `CommandName` valore per determinare quale pulsante è stato fatto clic. Informazioni aggiuntive, facoltativamente, possono essere fornite tramite il pulsante s `CommandArgument` proprietà.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Dennis Patterson. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](custom-buttons-in-the-datalist-and-repeater-cs.md)
