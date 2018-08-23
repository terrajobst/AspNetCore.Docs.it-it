---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: Aggiunta di una colonna GridView di caselle di controllo (VB) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione verrà illustrato come aggiungere una colonna di caselle di controllo a un controllo GridView per fornire all'utente un modo molto intuitivo di selezione di più righe di G....
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: be7fc4fe93d15e7550f873adffa15e82384d25be
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827352"
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>Aggiunta di una colonna GridView di caselle di controllo (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) o [Scarica il PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> Questa esercitazione verrà illustrato come aggiungere una colonna di caselle di controllo a un controllo GridView per fornire all'utente un modo molto intuitivo di selezione di più righe di GridView.


## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente abbiamo esaminato come aggiungere una colonna di pulsanti di opzione a GridView allo scopo di selezione di un record specifico. Una colonna di pulsanti di opzione è un'interfaccia utente appropriata quando l'utente è limitato a scegliere al massimo un elemento nella griglia. In alcuni casi, tuttavia, potrebbe vogliamo consentire all'utente di selezionare un numero arbitrario di elementi nella griglia. I client di posta elettronica basato sul Web, ad esempio, in genere visualizzare l'elenco di messaggi con una colonna di caselle di controllo. L'utente può selezionare un numero arbitrario di messaggi e quindi eseguire un'azione, ad esempio lo spostamento di messaggi di posta elettronica a un'altra cartella o eliminandoli.

In questa esercitazione si vedrà come aggiungere una colonna di caselle di controllo e come determinare quali caselle di controllo sono state controllate durante il postback. In particolare, creeremo un esempio che riproduce fedelmente l'interfaccia utente client di posta elettronica basato sul web. Questo esempio include un controllo GridView paging Elenca i prodotti nel `Products` tabella di database con una casella di controllo in ogni riga (vedere la figura 1). Un pulsante Elimina prodotti selezionati, quando si fa clic, verrà eliminati i prodotti selezionati.


[![Ogni riga di Product include una casella di controllo](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**Figura 1**: ogni riga di Product include una casella di controllo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Passaggio 1: Aggiunta di un controllo GridView di paging che elenca le informazioni sul prodotto

Prima ci preoccupiamo aggiunta di una colonna di caselle di controllo, lasciare che lo stato attivo prima con s sull'elenco di prodotti in un controllo GridView che supporta il paging. Iniziare aprendo il `CheckBoxField.aspx` nella pagina la `EnhancedGridView` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` a `Products`. Successivamente, scegliere di associare il controllo GridView per un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per usare la `ProductsBLL` classe, chiamare il `GetProducts()` metodo per restituire i dati. Poiché questo controllo GridView sarà di sola lettura, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno).


[![Creare un nuovo oggetto ObjectDataSource denominato ProductsDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**Figura 2**: creare un nuovo oggetto ObjectDataSource denominato `ProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))


[![Configurare ObjectDataSource per recuperare i dati usando il metodo GetProducts()](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**Figura 3**: configurare ObjectDataSource per recuperare dati usando il `GetProducts()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))


[![Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno)](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**Figura 4**: impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare schede su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))


Dopo aver completato la procedura guidata Configura origine dati, Visual Studio creerà automaticamente BoundColumns e un CheckBoxColumn per i campi dati relativi al prodotto. Come abbiamo fatto nell'esercitazione precedente, rimuovere tutto tranne il `ProductName`, `CategoryName`, e `UnitPrice` BoundField e modificare il `HeaderText` le proprietà del prodotto, categoria e prezzo. Configurare il `UnitPrice` BoundField in modo che il relativo valore viene formattato come una valuta. Configurare inoltre il controllo GridView per supportare il paging selezionando la casella di controllo Attiva Paging nello smart tag.

Consentire s aggiungere anche l'interfaccia utente per l'eliminazione di prodotti selezionati. Aggiungere un controllo pulsante Web sotto il controllo GridView, l'impostazione relativa `ID` al `DeleteSelectedProducts` e il relativo `Text` proprietà da eliminare i prodotti selezionati. Anziché eliminare effettivamente i prodotti dal database, in questo esempio verrà semplicemente visualizzato un messaggio che informa i prodotti che dovrebbero essere stati eliminati. Per risolvere questo problema, aggiungere un controllo etichetta Web sotto il pulsante. Impostare l'ID `DeleteResults`, deselezionare le relative `Text` proprietà e set relativo `Visible` e `EnableViewState` le proprietà da `False`.

Dopo aver apportato queste modifiche, GridView, ObjectDataSource, pulsante ed etichetta s markup dichiarativo deve simile al seguente:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

Si consiglia di visualizzare la pagina in un browser (vedere la figura 5). A questo punto si dovrebbe vedere il nome, categoria e prezzo dei primi dieci prodotti.


[![Il nome, categoria e prezzo dei primi 10 prodotti sono elencati](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**Figura 5**: sono elencati il nome, categoria e prezzo dei primi dieci prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Passaggio 2: Aggiunta di una colonna di caselle di controllo

Poiché ASP.NET 2.0 include un CampoCasellaDiControllo, si potrebbe pensare che può essere usato per aggiungere una colonna di caselle di controllo in un controllo GridView. Sfortunatamente, che non è il caso, come il CampoCasellaDiControllo è progettato per funzionare con un campo di dati Boolean. Vale a dire, per poter utilizzare il CampoCasellaDiControllo dobbiamo specificare il campo di dati sottostante il cui valore viene consultato per determinare se è selezionata la casella di controllo viene eseguito il rendering. Usiamo il CampoCasellaDiControllo non possiamo semplicemente includere una colonna di caselle di controllo è deselezionata.

In alternativa, è necessario aggiungere un TemplateField e aggiungere un controllo casella di controllo Web a relativo `ItemTemplate`. Aggiungere un TemplateField al `Products` GridView e renderlo il primo campo (a sinistra). GridView s nello smart tag, fare clic sul collegamento di modifica modelli e quindi trascinare un controllo casella di controllo Web dalla casella degli strumenti nel `ItemTemplate`. Impostare s questa casella di controllo `ID` proprietà `ProductSelector`.


[![Aggiungere un controllo casella di controllo Web denominato ProductSelector all'ItemTemplate s TemplateField](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**Figura 6**: aggiungere un controllo Web casella di controllo denominato `ProductSelector` al s TemplateField `ItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))


Con il controllo casella di controllo Web e TemplateField aggiunto, ogni riga include ora una casella di controllo. Figura 7 Mostra questa pagina, quando viene visualizzato tramite un browser, dopo che sono stati aggiunti i TemplateField e una casella di controllo.


[![Ogni riga del prodotto include ora una casella di controllo](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**Figura 7**: ogni riga del prodotto include ora una casella di controllo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Passaggio 3: Determinare quali caselle di controllo sono state controllate durante il Postback

A questo punto si dispone di una colonna di caselle di controllo, ma un modo per determinare quali caselle di controllo sono state controllate durante il postback. Quando si fa clic sul pulsante Elimina i prodotti selezionati, tuttavia, è necessario conoscere quali caselle di controllo sono state controllate per eliminare tali prodotti.

Le s GridView [ `Rows` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) fornisce l'accesso alle righe di dati nel GridView. È possibile eseguire l'iterazione attraverso queste righe, la casella di controllo di accesso a livello di codice e quindi fare riferimento relativo `Checked` proprietà per determinare se è stata selezionata la casella di controllo.

Creare un gestore eventi per il `DeleteSelectedProducts` controllo pulsante Web s `Click` eventi e aggiungere il codice seguente:


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

Il `Rows` proprietà restituisce una raccolta di `GridViewRow` le istanze di tale struttura le righe di dati di s GridView. Il `For Each` ciclo qui enumera la raccolta. Per ognuno `GridViewRow` dell'oggetto, la riga s casella di controllo a livello di codice si accede tramite `row.FindControl("controlID")`. Se la casella di controllo è selezionata, la riga o le righe corrispondenti `ProductID` valore viene recuperato dal `DataKeys` raccolta. In questo esercizio, è sufficiente visualizzare un messaggio informativo nel `DeleteResults` assegnare un'etichetta, anche se in un'applicazione funzionante è il d invece eseguire una chiamata per il `ProductsBLL` classe s `DeleteProduct(productID)` (metodo).

Con l'aggiunta di questo gestore dell'evento, fare clic sul pulsante Elimina prodotti selezionati ora consente di visualizzare il `ProductID` s di prodotti selezionati.


[![Quando si fa clic sul pulsante prodotti eliminare selezionata sono elencate ProductIDs i prodotti selezionati](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**Figura 8**: quando la elimina selezionato prodotti viene fatto clic sul pulsante i prodotti selezionati `ProductID` sono elencati ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Passaggio 4: Aggiunta di selezionare tutte e deselezionare tutti i pulsanti

Se un utente vuole eliminare tutti i prodotti nella pagina corrente, è necessario controllare ogni le dieci caselle di controllo. Possiamo aiutarti accelerare questo processo aggiungendo tutti un controllo pulsante che, quando si fa clic, consente di selezionare tutte le caselle di controllo nella griglia. Un pulsante deselezionare tutto sarebbe ugualmente utile.

Aggiungere due controlli Web pulsante alla pagina, inserirli sopra il controllo GridView. Impostare i dispositivi prima di tutto una `ID` a `CheckAll` e il relativo `Text` controllare tutte le proprietà, impostare la seconda s `ID` a `UncheckAll` e il relativo `Text` deselezionare tutte le proprietà.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

Successivamente, creare un metodo nella classe code-behind denominata `ToggleCheckState(checkState)` che, quando richiamata, enumera le `Products` s GridView `Rows` insieme e imposta ogni s casella di controllo `Checked` sul valore dell'oggetto passato in *oggetto checkState*  parametro.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

A questo punto, creare `Click` gestori eventi per il `CheckAll` e `UncheckAll` pulsanti. Nelle `CheckAll` gestore dell'evento s, semplicemente chiamare `ToggleCheckState(True)`; nella `UncheckAll`, chiamare `ToggleCheckState(False)`.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

Con questo codice, fare clic sul pulsante Seleziona tutto determina un postback e controlla tutte le caselle di controllo GridView. Analogamente, facendo clic su deselezionare tutti Deseleziona tutte le caselle di controllo. Figura 9 mostra la schermata dopo aver verificato il pulsante Seleziona tutto.


[![Scegliere il controllo di che tutte sul pulsante Seleziona tutte le caselle di controllo](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**Figura 9**: fare clic su di controllare tutti i pulsante Seleziona tutte le caselle di controllo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))


> [!NOTE]
> Quando la visualizzazione di una colonna di caselle di controllo, uno degli approcci per la selezione o la deselezione di tutte le caselle di controllo è tramite una casella di controllo nella riga di intestazione. Inoltre, corrente selezionare tutte le / deselezionare tutti implementazione richiede un postback. Le caselle di controllo può essere selezionato o deselezionato, tuttavia, completamente tramite script lato client, offrendo così un'esperienza utente eseguiremo. Per analizzare l'uso di una casella di controllo di righe di intestazione per controllare tutti e deselezionare tutti in modo dettagliato, insieme a una discussione sull'utilizzo delle tecniche di lato client, consultare [controllo di tutte le caselle di controllo in uno Script lato Client con GridView e controllare tutte la casella di controllo](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Riepilogo

Nei casi in cui è necessario per consentire agli utenti di scegliere un numero arbitrario di righe da un controllo GridView prima di procedere, aggiunta di una colonna di caselle di controllo è un'opzione. Come abbiamo visto in questa esercitazione, tra cui una colonna di caselle di controllo GridView comporta l'aggiunta di un TemplateField con un controllo casella di controllo Web. Usando un controllo Web (rispetto a inserimento markup direttamente nel modello, come è stato fatto nell'esercitazione precedente) ASP.NET memorizza automaticamente quali erano le caselle di controllo e non sono state archiviate durante il postback. È possibile accedere anche a livello di codice le caselle di controllo nel codice per determinare se una casella di controllo specificato è selezionato o per modificare lo stato di selezione.

Questa esercitazione e il penultimo esaminato l'aggiunta di una colonna di selezione della riga a GridView. Nell'esercitazione successiva verrà esaminato come fare, con un po' di lavoro, è possibile aggiungere funzionalità di inserimento a GridView.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](adding-a-gridview-column-of-radio-buttons-vb.md)
> [Successivo](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
