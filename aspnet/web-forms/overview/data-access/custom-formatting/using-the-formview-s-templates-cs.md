---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: Utilizzo di modelli del controllo FormView (c#) | Documenti Microsoft
author: rick-anderson
description: A differenza di DetailsView, FormView è composta non da campi. In alternativa, FormView rendering viene eseguito utilizzando i modelli. In questa esercitazione si esamineranno utilizzando il F....
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: 0e1b36f0bfc244e39bb620c1c066b3e2403722cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875825"
---
<a name="using-the-formviews-templates-c"></a>Utilizzo di modelli del controllo FormView (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) o [Scarica il PDF](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)

> A differenza di DetailsView, FormView è composta non da campi. In alternativa, FormView rendering viene eseguito utilizzando i modelli. In questa esercitazione che si esamineranno utilizzando il controllo FormView per presentare una visualizzazione di dati meno rigida.


## <a name="introduction"></a>Introduzione

Nelle ultime due esercitazioni è stato illustrato come personalizzare l'output dei controlli GridView e DetailsView con TemplateFields. Consente di TemplateFields per il contenuto di un campo specifico essere altamente personalizzati, ma alla fine GridView e DetailsView hanno un aspetto piuttosto squadrato, forma di griglia. Per molti scenari di un layout di griglia è ideale, ma in alcuni casi in cui è necessaria una visualizzazione più semplice e meno rigida. Quando si visualizza un singolo record, tale un layout semplice è possibile utilizzare il controllo FormView.

A differenza di DetailsView, FormView è composta non da campi. È possibile aggiungere un TemplateField BoundField a un controllo FormView. In alternativa, FormView rendering viene eseguito utilizzando i modelli. Considerato come un controllo DetailsView che contiene un singolo TemplateField FormView. FormView supporta i modelli seguenti:

- `ItemTemplate` usato per il rendering il record specifico visualizzato in FormView
- `HeaderTemplate` Consente di specificare una riga di intestazioni facoltative
- `FooterTemplate` Consente di specificare una riga di piè di pagina facoltativi
- `EmptyDataTemplate` Quando il controllo FormView `DataSource` non è presente alcun record, il `EmptyDataTemplate` viene usato al posto del `ItemTemplate` per il rendering di markup del controllo
- `PagerTemplate` può essere utilizzato per personalizzare l'interfaccia di paging per FormViews con paging abilitato
- `EditItemTemplate` / `InsertItemTemplate` usato per personalizzare l'interfaccia di modifica o l'interfaccia di inserimento per FormViews che supportano tale funzionalità

In questa esercitazione che si esamineranno utilizzando il controllo FormView per presentare una visualizzazione dei prodotti meno rigida. Invece di campi per il nome, categoria, fornitore e del così via, FormView `ItemTemplate` verranno visualizzati i valori utilizzando una combinazione di un elemento di intestazione e un `<table>` (vedere la figura 1).


[![FormView interruzioni del Layout di griglia simile a quello visualizzato nel controllo DetailsView.](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)

**Figura 1**: FormView interrotta il Layout Grid-Like visualizzato nel controllo DetailsView ([fare clic per visualizzare l'immagine ingrandita](using-the-formview-s-templates-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Passaggio 1: Associazione di dati per il controllo FormView

Aprire il `FormView.aspx` pagina e trascinare un controllo FormView dalla casella degli strumenti nella finestra di progettazione. Quando si aggiungono innanzitutto FormView viene visualizzato come una casella grigia, che ci indica che un `ItemTemplate` è necessaria.


[![FormView non può essere eseguito il rendering nella finestra di progettazione fino a quando non viene fornito un elemento ItemTemplate](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)

**Figura 2**: il controllo FormView non è possibile visualizzare la finestra di progettazione fino a un' `ItemTemplate` fornito ([fare clic per visualizzare l'immagine ingrandita](using-the-formview-s-templates-cs/_static/image6.png))


Il `ItemTemplate` possono essere creati manualmente (tramite la sintassi dichiarativa) o possono essere create automaticamente associando FormView a un controllo origine dati tramite la finestra di progettazione. Questo create automaticamente `ItemTemplate` contiene tag HTML che elenca il nome di ogni campo e un'etichetta controllo cui `Text` proprietà è associata al valore del campo. Questo approccio anche auto-crea un `InsertItemTemplate` e `EditItemTemplate`, entrambi i quali vengono popolati con i controlli di input per ognuno dei campi di dati restituiti dal controllo origine dati.

Se si desidera creare automaticamente il modello, smart tag del controllo FormView di aggiungere un nuovo controllo ObjectDataSource che richiama la `ProductsBLL` della classe `GetProducts()` metodo. Verrà creato un controllo FormView con un `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate`. Dalla visualizzazione origine, rimuovere il `InsertItemTemplate` e `EditItemTemplate` poiché non si è interessati la creazione di un controllo FormView che supporta la modifica o inserimento ancora. Successivamente, cancellare il markup all'interno di `ItemTemplate` in modo che si dispone di una situazione pulita da.

Se si preferisce creare di `ItemTemplate` manualmente, è possibile aggiungere e configurare ObjectDataSource trascinandolo dalla casella degli strumenti nella finestra di progettazione. Tuttavia, non impostare l'origine di dati del controllo FormView dalla finestra di progettazione. Al contrario, passare alla visualizzazione origine e impostare manualmente il controllo FormView `DataSourceID` proprietà per il `ID` valore ObjectDataSource. Successivamente, aggiungere manualmente il `ItemTemplate`.

Indipendentemente dal quale approccio deciso di richiedere, a questo punto markup dichiarativo del FormView deve aspetto:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

È opportuno selezionare la casella di controllo Abilita Paging nello smart tag del controllo FormView; verrà aggiunta la `AllowPaging="True"` attributo per la sintassi dichiarativa del controllo FormView. Inoltre, impostare il `EnableViewState` la proprietà su False.

## <a name="step-2-defining-theitemtemplates-markup"></a>Passaggio 2: Definire il`ItemTemplate`del Markup

Con il controllo FormView associata al controllo ObjectDataSource e configurato per supportare il paging si è pronti per specificare il contenuto per il `ItemTemplate`. Per questa esercitazione, si hanno il nome del prodotto visualizzato un `<h3>` titolo. In seguito, che consente di usare un elemento HTML `<table>` per visualizzare le proprietà rimanenti di prodotto in una tabella di quattro colonne in cui le colonne del prima e il terza elencano i nomi delle proprietà e il secondo e il quarto elenco i relativi valori.

Questo codice può essere inserita tramite l'interfaccia di modifica del controllo FormView modello nella finestra di progettazione o immesso manualmente tramite la sintassi dichiarativa. Quando si lavora con modelli in genere risultare più veloce lavorare direttamente con la sintassi dichiarativa, ma è possibile utilizzare qualsiasi tecnica si è più comodo.

Di seguito viene illustrato il markup dichiarativo FormView dopo il `ItemTemplate`della struttura è stata completata:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

Si noti che la sintassi di associazione dati - `<%# Eval("ProductName") %>`, ad esempio può essere inserita direttamente nell'output del modello. Ovvero non è necessario essere assegnato a un controllo di etichetta `Text` proprietà. Ad esempio, è necessario il `ProductName` valore visualizzato in un `<h3>` elemento utilizzando `<h3><%# Eval("ProductName") %></h3>`, che per il prodotto Chai verrà eseguito il rendering come `<h3>Chai</h3>`.

Il `ProductPropertyLabel` e `ProductPropertyValue` classi CSS vengono utilizzate per specificare lo stile di nomi di proprietà di prodotto e i valori nel `<table>`. Queste classi CSS sono definite `Styles.css` , pertanto i nomi delle proprietà in grassetto e allineato a destra e aggiungere un diritto di riempimento per i valori delle proprietà.

Poiché non sono disponibili con il controllo FormView, nessun CheckBoxFields per mostrare il `Discontinued` valore come una casella di controllo è necessario aggiungere un controllo casella di controllo. Il `Enabled` è impostata su False, rendendo di sola lettura e la casella di controllo `Checked` proprietà è associata al valore del `Discontinued` campo dati.

Con il `ItemTemplate` completo, vengono visualizzate le informazioni di prodotto in modo molto più semplice. Confrontare l'output di DetailsView dall'ultima esercitazione (figura 3) con l'output generato dal FormView in questa esercitazione (figura 4).


[![L'Output di DetailsView rigida](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)

**Figura 3**: l'Output di DetailsView rigida ([fare clic per visualizzare l'immagine ingrandita](using-the-formview-s-templates-cs/_static/image9.png))


[![L'Output FormView fluido](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)

**Figura 4**: l'Output di FormView fluido ([fare clic per visualizzare l'immagine ingrandita](using-the-formview-s-templates-cs/_static/image12.png))


## <a name="summary"></a>Riepilogo

Mentre i controlli GridView e DetailsView possono avere l'output personalizzato tramite TemplateFields, sia ancora presentare i dati in un formato griglia, squadrato. Tutte le volte quando è necessario un singolo record da visualizzare utilizzando un layout meno rigido, FormView è la scelta ideale. Analogamente a DetailsView, FormView esegue il rendering di un singolo record relativo `DataSource`, ma a differenza di DetailsView è composto solo da modelli e non supporta i campi.

Come abbiamo visto in questa esercitazione, FormView consente un layout più flessibile per la visualizzazione di un singolo record. In futuro esercitazioni si esamineranno i controlli DataList e Repeater, che forniscono lo stesso livello di flessibilità come il FormsView, ma sono in grado di visualizzare più record (ad esempio GridView).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata E.R. Morelli. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](using-templatefields-in-the-detailsview-control-cs.md)
> [Successivo](displaying-summary-information-in-the-gridview-s-footer-cs.md)
