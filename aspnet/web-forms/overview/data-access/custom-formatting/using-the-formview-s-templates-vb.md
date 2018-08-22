---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Usando i modelli del controllo FormView (VB) | Microsoft Docs
author: rick-anderson
description: A differenza di DetailsView, FormView non comprende i campi. Al contrario, FormView rendering viene eseguito usando i modelli. In questa esercitazione verrà esaminato utilizzando le F....
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: 6d16ef7ef8a3d5fce10e0d0b88421be294e9fc8d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827289"
---
<a name="using-the-formviews-templates-vb"></a>Usando i modelli del controllo FormView (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) o [Scarica il PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> A differenza di DetailsView, FormView non comprende i campi. Al contrario, FormView rendering viene eseguito usando i modelli. In questa esercitazione che verrà esaminato tramite il controllo FormView per presentare una visualizzazione di dati meno rigida.


## <a name="introduction"></a>Introduzione

Nelle ultime due esercitazioni è stato illustrato come personalizzare gli output dei controlli GridView e DetailsView, uso di TemplateFields. TemplateFields consentono il contenuto per un campo specifico essere altamente personalizzato, ma alla fine GridView e DetailsView avere un aspetto simile a griglia e piuttosto squadrato. Per molti scenari è consigliabile un layout griglia simile a questo tipo, ma in alcuni casi è necessaria una visualizzazione più fluida, meno rigida. Quando si visualizza un singolo record, è possibile utilizzando il controllo FormView tali un layout fluido.

A differenza di DetailsView, FormView non comprende i campi. È possibile aggiungere un BoundField o TemplateField a un controllo FormView. Al contrario, FormView rendering viene eseguito usando i modelli. Pensi a FormView come un controllo DetailsView contenente un TemplateField singolo. FormView supporta i modelli seguenti:

- `ItemTemplate` utilizzato per il rendering il record specifico visualizzato nel controllo FormView
- `HeaderTemplate` Consente di specificare una riga di intestazioni facoltative
- `FooterTemplate` Consente di specificare una riga di piè di pagina facoltativi
- `EmptyDataTemplate` Quando il controllo FormView `DataSource` non dispone di alcun record, il `EmptyDataTemplate` viene usato al posto di `ItemTemplate` per il rendering del markup del controllo
- `PagerTemplate` può essere utilizzato per personalizzare l'interfaccia di paging per FormViews che hanno il paging è abilitato
- `EditItemTemplate` / `InsertItemTemplate` usato per personalizzare l'interfaccia per la modifica o l'interfaccia di inserimento per FormViews che supportano questa funzionalità

In questa esercitazione che verrà esaminato tramite il controllo FormView per presentare una visualizzazione di prodotti meno rigida. Evitando che i campi per il nome, categoria, fornitore e del così via, FormView `ItemTemplate` mostrerà tali valori usando una combinazione di un elemento di intestazione e un `<table>` (vedere la figura 1).


[![FormView suddivide la del Layout di tipo griglia visualizzato nel controllo DetailsView.](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Figura 1**: FormView esce il Layout Grid-Like visualizzato nel controllo DetailsView ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-formview-s-templates-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Passaggio 1: Associazione dei dati a FormView

Aprire il `FormView.aspx` pagina e trascinare un controllo FormView dalla casella degli strumenti nella finestra di progettazione. Quando si aggiunge innanzitutto FormView viene visualizzato come una casella grigia, che indicano a noi che un `ItemTemplate` è necessaria.


[![FormView non è possibile visualizzare nella finestra di progettazione fino a quando non viene fornito un ItemTemplate](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Figura 2**: il controllo FormView non è possibile visualizzare la finestra di progettazione fino a quando non un' `ItemTemplate` viene fornito ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-formview-s-templates-vb/_static/image6.png))


Il `ItemTemplate` possono essere creati manualmente (tramite la sintassi dichiarativa) o possono essere creati automaticamente mediante l'associazione FormView a un controllo origine dati tramite la finestra di progettazione. Ciò creato automaticamente `ItemTemplate` contiene codice HTML che elenca il nome di ogni campo e un'etichetta di controllo la cui `Text` è associata la proprietà per il valore del campo. Questo approccio anche auto-crea un' `InsertItemTemplate` e `EditItemTemplate`, entrambi vengono popolati con i controlli di input per ognuno dei campi dati restituiti dal controllo origine dati.

Se si desidera creare automaticamente il modello, dallo smart tag del controllo FormView aggiungere un nuovo controllo ObjectDataSource che richiama il `ProductsBLL` della classe `GetProducts()` (metodo). Verrà creato un controllo FormView con un `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate`. Dalla visualizzazione origine, rimuovere il `InsertItemTemplate` e `EditItemTemplate` poiché non sono interessati la creazione di un controllo FormView che supporta la modifica o inserimento ancora. Successivamente, cancellare il contenuto markup all'interno di `ItemTemplate` in modo che abbiamo uno slate pulito di lavorare da.

Se si preferisce creare di `ItemTemplate` manualmente, è possibile aggiungere e configurare ObjectDataSource, trascinarlo dalla casella degli strumenti nella finestra di progettazione. Tuttavia, non imposta origine dati del controllo FormView dalla finestra di progettazione. Al contrario, passare alla visualizzazione origine e impostare manualmente il controllo FormView `DataSourceID` proprietà per il `ID` pari a ObjectDataSource. Successivamente, aggiungere manualmente il `ItemTemplate`.

Indipendentemente dal quale approccio si deciso di sfruttare, a questo punto markup dichiarativo di FormView deve sono simili a:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Si consiglia di controllare la casella di controllo Attiva Paging nello smart tag del controllo FormView; verrà aggiunta la `AllowPaging="True"` sintassi dichiarativa del controllo FormView dell'attributo. Inoltre, impostare il `EnableViewState` proprietà su False.

## <a name="step-2-defining-theitemtemplates-markup"></a>Passaggio 2: Definizione di`ItemTemplate`del Markup

Con il controllo FormView associata al controllo ObjectDataSource e configurato per supportare il paging siamo pronti per specificare il contenuto per il `ItemTemplate`. Per questa esercitazione, è possibile visualizzare nome del prodotto in un `<h3>` intestazione. In seguito, è possibile usare un elemento HTML `<table>` per visualizzare le proprietà rimanenti del prodotto in una tabella di quattro colonne in cui la prima e la terza colonna elenca i nomi delle proprietà e il secondo e il quarto elenco i relativi valori.

Questo markup può essere immessi tramite l'interfaccia di modifica modello del controllo FormView nella finestra di progettazione o immettere manualmente tramite la sintassi dichiarativa. Quando si lavora con i modelli in genere risultare più veloce lavorare direttamente con la sintassi dichiarativa, ma è possibile usare qualsiasi tecnica con cui hai più dimestichezza.

Il markup seguente illustra il markup dichiarativo di FormView dopo il `ItemTemplate`della struttura è stata completata:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Si noti che la sintassi di Data Binding - `<%# Eval("ProductName") %>`, ad esempio può essere inserito direttamente nell'output del modello. Vale a dire, è necessario non assegnarlo a un controllo etichetta `Text` proprietà. Ad esempio, è necessario il `ProductName` valore visualizzato un `<h3>` elemento usando `<h3><%# Eval("ProductName") %></h3>`, che per il prodotto Chai verranno sottoposti a rendering come `<h3>Chai</h3>`.

Il `ProductPropertyLabel` e `ProductPropertyValue` classi CSS vengono utilizzate per specificare lo stile dei nomi delle proprietà di prodotto e i valori nel `<table>`. Queste classi CSS sono definite `Styles.css` , pertanto i nomi delle proprietà grassetto e allineato a destra e aggiungere il diritto di riempimento per i valori delle proprietà.

Poiché non esistono disponibili con FormView, nessun CheckBoxFields per mostrare il `Discontinued` valore come una casella di controllo è necessario aggiungere un controllo casella di controllo. Il `Enabled` è impostata su False, rendendo di sola lettura e la casella di controllo `Checked` proprietà è associata al valore della `Discontinued` campo dati.

Con la `ItemTemplate` completo, vengono visualizzate le informazioni di prodotto in modo molto più fluido. Confrontare l'output di DetailsView dall'ultima esercitazione (figura 3) con l'output generato dal FormView in questa esercitazione (figura 4).


[![L'Output di DetailsView rigida](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Figura 3**: l'Output di DetailsView rigida ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-formview-s-templates-vb/_static/image9.png))


[![L'Output di FormView fluide](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Figura 4**: l'Output di FormView fluidodinamica ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-formview-s-templates-vb/_static/image12.png))


## <a name="summary"></a>Riepilogo

Anche se i controlli GridView e DetailsView possono avere l'output delle applicazioni personalizzata utilizzando TemplateFields, entrambe ancora presentare i propri dati in un formato simile a griglia e squadrato. Nei casi quando un singolo record deve essere visualizzata utilizzando un layout meno rigido, FormView rappresenta la scelta ideale. Analogamente a DetailsView, FormView esegue il rendering di un singolo record dalla relativa `DataSource`, ma a differenza di DetailsView è composto solo da modelli e non supporta i campi.

Come abbiamo visto in questa esercitazione, FormView consente un layout più flessibile per la visualizzazione di un singolo record. Esercitazioni in futuro verranno presi in esame i controlli DataList e Repeater, che forniscono lo stesso livello di flessibilità come il FormsView, ma sono in grado di visualizzare più record (ad esempio, il controllo GridView).

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata E.R. Morelli. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](using-templatefields-in-the-detailsview-control-vb.md)
> [Successivo](displaying-summary-information-in-the-gridview-s-footer-vb.md)
