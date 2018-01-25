---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: Aggiunta di una colonna di GridView di caselle di controllo (c#) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione verrà illustrato come aggiungere una colonna di caselle di controllo a un controllo GridView per fornire all'utente un modo molto intuitivo per selezionare più righe di G...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 6f238b8ea8dfbde67dbad7a52d6b4851d67402a8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-gridview-column-of-checkboxes-c"></a>Aggiunta di una colonna di GridView di caselle di controllo (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) o [Scarica il PDF](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> In questa esercitazione verrà illustrato come aggiungere una colonna di caselle di controllo a un controllo GridView per fornire all'utente un modo molto intuitivo per selezionare più righe di GridView.


## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente abbiamo esaminato come aggiungere una colonna di pulsanti di opzione a GridView ai fini della selezione di un record specifico. Una colonna di pulsanti di opzione è un'interfaccia utente appropriata quando l'utente è limitato alla scelta di al massimo un elemento dalla griglia. In alcuni casi, tuttavia, potrebbe voler consentire all'utente di selezionare un numero arbitrario di elementi dalla griglia. I client di posta elettronica basato sul Web, ad esempio, in genere visualizzato l'elenco di messaggi con una colonna di caselle di controllo. L'utente può selezionare un numero arbitrario di messaggi e quindi eseguire un'azione, ad esempio spostare i messaggi di posta elettronica in un'altra cartella o eliminarli.

In questa esercitazione verrà spiegato come aggiungere una colonna di caselle di controllo e come determinare quali caselle di controllo sono state controllate durante il postback. In particolare, verrà creato un esempio che simuli strettamente l'interfaccia utente client di posta elettronica basato sul web. Questo esempio includerà un GridView paging Elenca i prodotti di `Products` tabella di database con una casella di controllo in ogni riga (vedere la figura 1). Un pulsante Elimina prodotti selezionati, se selezionato, verrà eliminati i prodotti selezionati.


[![Ogni riga del prodotto include una casella di controllo](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**Figura 1**: ogni riga del prodotto include una casella di controllo ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Passaggio 1: Aggiunta di paging GridView che visualizza le informazioni di prodotto

Prima di preoccupazione aggiunta di una colonna di caselle di controllo, consentono lo stato attivo prima di s sull'elenco di prodotti in un controllo GridView che supporta il paging. Avvia aprendo il `CheckBoxField.aspx` nella pagina di `EnhancedGridView` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` per `Products`. Successivamente, scegliere di associare un nuovo oggetto ObjectDataSource denominato di GridView `ProductsDataSource`. Configurare ObjectDataSource per utilizzare il `ProductsBLL` classe, chiamata di `GetProducts()` per restituire i dati. Poiché questo GridView sarà di sola lettura, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno).


[![Creare un nuovo oggetto ObjectDataSource denominato ProductsDataSource](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**Figura 2**: creare un nuovo ObjectDataSource denominato `ProductsDataSource` ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))


[![Configurare ObjectDataSource per recuperare i dati utilizzando il metodo GetProducts()](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**Figura 3**: configurare ObjectDataSource per recuperare dati usando il `GetProducts()` metodo ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))


[![Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno)](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**Figura 4**: impostare l'elenco a discesa sono elencati nelle schede DELETE, INSERT e UPDATE su (nessuno) ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))


Dopo aver completato la configurazione guidata origine dati, Visual Studio crea automaticamente BoundColumns e una con per i campi di dati relativi al prodotto. Ad esempio eseguita nell'esercitazione precedente, rimuovere tutto tranne il `ProductName`, `CategoryName`, e `UnitPrice` BoundField e modificare il `HeaderText` le proprietà per prodotto, categoria e prezzo. Configurare il `UnitPrice` BoundField in modo che il relativo valore viene formattato come valuta. Configurare inoltre GridView per supportare il paging selezionando la casella di controllo Abilita Paging lo smart tag.

Consente di aggiungere anche l'interfaccia utente per l'eliminazione dei prodotti selezionati s. Aggiungere un controllo pulsante Web sotto GridView, impostare il relativo `ID` a `DeleteSelectedProducts` e il relativo `Text` proprietà eliminare prodotti selezionati. Anziché eliminare effettivamente i prodotti dal database, per questo esempio si visualizzeranno solo un messaggio che indica i prodotti che dovrebbero essere stati eliminati. A tale scopo, aggiungere un controllo etichetta Web sotto il pulsante. Impostare l'ID su `DeleteResults`, deselezionare le relative `Text` proprietà e impostare relativo `Visible` e `EnableViewState` proprietà `false`.

Dopo aver apportato queste modifiche, il markup dichiarativo s GridView, ObjectDataSource, pulsante ed etichetta deve simile al seguente:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

Dedicare alcuni minuti per visualizzare la pagina in un browser (vedere Figura 5). A questo punto si dovrebbe vedere il nome, categoria e prezzo dei primi dieci prodotti.


[![Sono elencati il nome, categoria e prezzi dei prodotti dieci prima](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**Figura 5**: sono elencati il nome, categoria e prezzi dei prodotti dieci prima ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Passaggio 2: Aggiunta di una colonna di caselle di controllo

Poiché ASP.NET 2.0 include un CheckBoxField, si pensa che potrebbe essere utilizzata per aggiungere una colonna di caselle di controllo per un controllo GridView. Purtroppo, non è il caso, come elemento CheckBoxField è progettato per funzionare con un campo dati. Per utilizzare l'elemento CheckBoxField, è necessario specificare il campo dei dati sottostante il cui valore viene consultato per determinare se è selezionata la casella di controllo viene eseguito il rendering. È possibile usare l'elemento CheckBoxField da includere solo una colonna di caselle di controllo deselezionata.

In alternativa, è necessario aggiungere un TemplateField e aggiungere un controllo casella di controllo Web per il relativo `ItemTemplate`. Iniziare, aggiungerne un TemplateField per il `Products` GridView e renderlo il primo campo (estremità sinistra). Da GridView s smart tag, fare clic sul collegamento Modifica modelli e quindi trascinare un controllo casella di controllo Web dalla casella degli strumenti nel `ItemTemplate`. Impostare questa casella di controllo s `ID` proprietà `ProductSelector`.


[![Aggiungere un controllo casella di controllo Web denominato ProductSelector a ItemTemplate s TemplateField](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**Figura 6**: aggiungere un controllo casella di controllo di Web denominato `ProductSelector` al s TemplateField `ItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))


Con il controllo casella di controllo Web e di TemplateField aggiunto, ogni riga include una casella di controllo. Figura 7 illustra questa pagina, quando viene visualizzato tramite un browser, dopo aver aggiunto il TemplateField e una casella di controllo.


[![Ogni riga del prodotto include ora una casella di controllo](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**Figura 7**: ogni riga del prodotto include ora una casella di controllo ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Passaggio 3: Determinare quali caselle di controllo sono state controllate durante il Postback

A questo punto si dispone di una colonna di caselle di controllo ma non è possibile determinare quali caselle di controllo sono stati selezionati durante il postback. Quando si fa clic sul pulsante Elimina prodotti selezionati, tuttavia, è necessario conoscere quali caselle di controllo sono state controllate per eliminare i prodotti.

S GridView [ `Rows` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) fornisce l'accesso alle righe di dati in GridView. È possibile scorrere le righe, il controllo casella di controllo di accesso a livello di codice e quindi fare riferimento relativo `Checked` proprietà per determinare se è stata selezionata la casella di controllo.

Creare un gestore eventi per il `DeleteSelectedProducts` controllo Web Button s `Click` eventi e aggiungere il codice seguente:


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

Il `Rows` proprietà restituisce una raccolta di `GridViewRow` istanze di tale struttura le righe di dati di GridView s. Il `foreach` ciclo qui enumera la raccolta. Per ogni `GridViewRow` dell'oggetto, la riga s casella di controllo a livello di codice si accede tramite `row.FindControl("controlID")`. Se la casella di controllo è selezionata, la riga o le righe corrispondenti `ProductID` valore viene recuperato dal `DataKeys` insieme. In questo esercizio, è sufficiente visualizzare un messaggio informativo nel `DeleteResults` etichetta, anche se in un'applicazione funzionante è d invece eseguire una chiamata al `ProductsBLL` classe s `DeleteProduct(productID)` metodo.

Con l'aggiunta di questo gestore eventi, facendo clic sul pulsante Elimina prodotti selezionati a questo punto viene visualizzato il `ProductID` s dei prodotti selezionati.


[![Quando viene scelto il pulsante di prodotti di selezionato eliminare ProductIDs di prodotti selezionati sono elencati](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**Figura 8**: quando l'eliminazione selezionata prodotti pulsante i prodotti selezionati `ProductID` sono elencati ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Passaggio 4: Aggiunta di selezionare tutte e deselezionare tutti i pulsanti

Se si desidera eliminare tutti i prodotti nella pagina corrente, è necessario controllare ogni le caselle di controllo di dieci. Possiamo aiutarti accelerare questo processo aggiungendo tutti un controllo pulsante che, quando si fa clic, selezionate tutte le caselle di controllo nella griglia. Un pulsante di deselezionare tutto sarebbe ugualmente utile.

Aggiungere due controlli Web pulsante alla pagina, inserirli sopra il controllo GridView. Impostare il primo s `ID` a `CheckAll` e il relativo `Text` proprietà per controllare tutti; impostare la seconda s `ID` a `UncheckAll` e il relativo `Text` deselezionare tutte le proprietà.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

Successivamente, creare un metodo nella classe code-behind denominata `ToggleCheckState(checkState)` che, quando richiamata, enumera il `Products` GridView s `Rows` insieme e imposta s ogni casella di controllo `Checked` sul valore dell'oggetto passato in *checkState*  parametro.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

Successivamente, creare `Click` gestori eventi per il `CheckAll` e `UncheckAll` pulsanti. In `CheckAll` gestore dell'evento s, semplicemente chiamare `ToggleCheckState(true)`; in `UncheckAll`, chiamare `ToggleCheckState(false)`.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

Con questo codice, fare clic sul pulsante Seleziona tutto provoca un postback e controlla tutte le caselle di controllo GridView. Analogamente, facendo clic su deselezionare tutto Deseleziona tutte le caselle di controllo. Figura 9 viene visualizzata la schermata dopo aver verificato il pulsante Seleziona tutto.


[![Il controllo che pulsante tutti scegliere Seleziona tutte le caselle di controllo](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**Figura 9**: facendo clic su di controllare tutti pulsante Seleziona tutte le caselle di controllo ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))


> [!NOTE]
> Quando la visualizzazione di una colonna di caselle di controllo, un approccio per la selezione o deselezione di tutte le caselle di controllo è attraverso una casella di controllo nella riga di intestazione. Inoltre, corrente selezionare tutte le / implementazione deseleziona tutto richiede un postback. Le caselle di controllo è possibile selezionarla o deselezionarla, tuttavia, completamente tramite script sul lato client, fornendo un'esperienza utente snappier. Per esplorare usando una casella di controllo di riga di intestazione per deselezionare tutti e controllare tutte in dettaglio, insieme a una discussione sull'utilizzo delle tecniche sul lato client, consultare [verifica tutte le caselle di controllo in uno Script sul lato Client utilizzando GridView e una casella di controllo tutto verificare](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Riepilogo

Nei casi in cui sia necessario per consentire agli utenti di scegliere un numero arbitrario di righe da un controllo GridView prima di procedere, aggiunta di una colonna di caselle di controllo è una delle opzioni. Come abbiamo visto in questa esercitazione, tra cui una colonna di caselle di controllo GridView comporta l'aggiunta di un TemplateField con un controllo casella di controllo Web. Tramite un controllo Web (e inserendo markup direttamente il modello, come è stato fatto nell'esercitazione precedente) ASP.NET memorizza automaticamente le caselle di controllo sono stati e non è stati selezionati durante il postback. È possibile accedere anche a livello di codice le caselle di controllo nel codice per determinare se è selezionata la casella di controllo specifico o per modificare lo stato di selezione.

In questa esercitazione e l'ultimo esaminato l'aggiunta di una colonna di selettore di riga a GridView. Nell'esercitazione successiva verrà esaminato, con un bit di lavoro, è possibile aggiungere funzionalità di inserimento a GridView.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Precedente](adding-a-gridview-column-of-radio-buttons-cs.md)
[Successivo](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
