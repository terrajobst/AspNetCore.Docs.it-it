---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: Utilizzo di TemplateFields nel controllo GridView (VB) | Documenti Microsoft
author: rick-anderson
description: "Per garantire flessibilità, GridView offre il TemplateField, che esegue il rendering di un modello. Un modello può includere una combinazione di HTML statico, i controlli Web, e..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 337765988cc6ec92384bec09a72fd00505d9a039
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-templatefields-in-the-gridview-control-vb"></a>Utilizzo di TemplateFields nel controllo GridView (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe) o [Scarica il PDF](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> Per garantire flessibilità, GridView offre il TemplateField, che esegue il rendering di un modello. Un modello può includere una combinazione di HTML statico, Web, controlli e la sintassi di associazione dati. In questa esercitazione verrà esaminato come utilizzare il TemplateField per ottenere un maggiore livello di personalizzazione con il controllo GridView.


## <a name="introduction"></a>Introduzione

GridView è costituito da un set di campi che indicano le proprietà dal `DataSource` devono essere inclusi nell'output del rendering e la modalità di visualizzazione dei dati. Il tipo di campo più semplice è il BoundField, che consente di visualizzare un valore di dati come testo. Altri tipi di campo visualizzano i dati utilizzando gli elementi HTML alternativi. CheckBoxField, ad esempio, esegue il rendering come una casella di controllo il cui stato di selezione dipende dal valore di un campo di dati specificato. il ImageField esegue il rendering di un'immagine la cui origine dell'immagine è basato su un campo di dati specificato. Collegamenti ipertestuali e i pulsanti il cui stato dipende dal valore di campo di dati sottostante possono essere visualizzati utilizzando i tipi di campo HyperLinkField e ButtonField.

Mentre i tipi di campo CheckBoxField, ImageField, HyperLinkField e ButtonField consentono la visualizzazione alternativa dei dati, ancora sono piuttosto limitati rispetto alla formattazione. Un CheckBoxField può visualizzare solo una singola casella di controllo, mentre un ImageField può visualizzare solo una singola immagine. Cosa accade se un campo specifico necessario visualizzare un testo, una casella di controllo, *e* un'immagine, tutte basata su valori dei campi dati diverso? O cosa accade se si desidera visualizzare i dati di utilizzo di un controllo Web diverso dalla casella di controllo, immagine, un collegamento ipertestuale o pulsante? Inoltre, il BoundField limita la visualizzazione per un singolo campo dati. Cosa accade se si desidera mostrare due o più valori di campo di dati in un'unica colonna GridView?

Per supportare questo livello di flessibilità GridView offre il TemplateField, che esegue il rendering usando un *modello*. Un modello può includere una combinazione di HTML statico, Web, controlli e la sintassi di associazione dati. Inoltre, il TemplateField ha una varietà di modelli che possono essere utilizzati per personalizzare il rendering per situazioni diverse. Ad esempio, il `ItemTemplate` viene utilizzato per impostazione predefinita per il rendering della cella per ogni riga, ma la `EditItemTemplate` modello può essere utilizzato per personalizzare l'interfaccia durante la modifica di dati.

In questa esercitazione verrà esaminato come utilizzare il TemplateField per ottenere un maggiore livello di personalizzazione con il controllo GridView. Nel [esercitazione precedente](custom-formatting-based-upon-data-vb.md) è stato illustrato come personalizzare la formattazione in base a dati sottostante tramite il `DataBound` e `RowDataBound` gestori eventi. È inoltre possibile personalizzare la formattazione in base ai dati sottostanti è chiamando i metodi di formattazione all'interno di un modello. Questa tecnica verrà esaminato in questa esercitazione anche.

Per questa esercitazione si utilizzerà TemplateFields per personalizzare l'aspetto di un elenco di dipendenti. In particolare, è possibile elencare tutti i dipendenti, ma verrà visualizzato il dipendente nomi e cognomi in una colonna, data di assunzione in un controllo di calendario e una colonna di stato che indica il numero di giorni è stati impiegati presso la società.


[![Tre TemplateFields vengono utilizzati per personalizzare la visualizzazione](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**Figura 1**: TemplateFields tre vengono utilizzati per personalizzare la visualizzazione ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Passaggio 1: Associazione di dati al controllo GridView.

In scenari in cui è necessario utilizzare TemplateFields per personalizzare l'aspetto di report, più semplice inizia creando un controllo GridView che contiene solo BoundField prima e quindi aggiungere di nuovo TemplateFields o convertire il BoundField esistente a TemplateFields in base alle esigenze. Pertanto, iniziamo questa esercitazione, aggiunta di un controllo GridView alla pagina tramite la finestra di progettazione e associarla a ObjectDataSource che restituisce l'elenco di dipendenti. Questi passaggi verranno creato un controllo GridView con BoundField per ognuno dei campi employee.

Aprire il `GridViewTemplateField.aspx` pagina e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Smart tag del controllo GridView scegliere di aggiungere un nuovo controllo ObjectDataSource che richiama la `EmployeesBLL` della classe `GetEmployees()` metodo.


[![Aggiungere un nuovo controllo ObjectDataSource che richiama il metodo EmployeesDataTable](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**Figura 2**: aggiungere un nuovo controllo ObjectDataSource che Invoke il `GetEmployees()` metodo ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image6.png))


Associazione di GridView in questo modo verrà aggiunto automaticamente un BoundField per ogni proprietà dipendente: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, e `Country`. Per questo report si reca fastidio con la visualizzazione di `EmployeeID`, `ReportsTo`, o `Country` proprietà. Per rimuovere questi BoundField è possibile:

- Per visualizzare questa finestra di dialogo, utilizzare il finestra di dialogo campi fare clic sul collegamento Modifica colonne smart tag del controllo GridView. Successivamente, selezionare il BoundField dall'angolo inferiore sinistro di elenco e fare clic sulla X rossa pulsante per rimuovere il BoundField.
- Modificare manualmente la sintassi dichiarativa di GridView dalla vista origine, eliminare il `<asp:BoundField>` elemento per il BoundField che si desidera rimuovere.

Dopo aver rimosso il `EmployeeID`, `ReportsTo`, e `Country` BoundField, markup di GridView dovrebbe essere simile:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

Richiedere qualche istante per visualizzare l'avanzamento in un browser. A questo punto si dovrebbe vedere una tabella con un record per ogni dipendente e quattro colonne: uno per il dipendente cognome, uno per il nome, uno per il titolo e una data di assunzione.


[![Il cognome, FirstName, titolo e HireDate campi vengono visualizzati per ogni dipendente](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**Figura 3**: il `LastName`, `FirstName`, `Title`, e `HireDate` vengono visualizzati i campi per ogni dipendente ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Passaggio 2: Visualizzare i nomi e il cognome in una singola colonna

Attualmente, ogni dipendente del primo e ultimo nomi vengono visualizzati in una colonna separata. Potrebbe essere interessante combinarle in un'unica colonna. A tale scopo è necessario utilizzare un TemplateField. È possibile di aggiungere un nuovo TemplateField, aggiungere il markup necessari e la sintassi di associazione dati e quindi eliminare il `FirstName` e `LastName` BoundField oppure è possibile convertire il `FirstName` BoundField in un TemplateField, modificare TemplateField da includere il `LastName` valore e quindi rimuovere il `LastName` BoundField.

Entrambi gli approcci net lo stesso risultato, ma mi piace conversione BoundField in TemplateFields quando possibile, perché la conversione viene aggiunto automaticamente un `ItemTemplate` e `EditItemTemplate` con sintassi di associazione dati per simulare l'aspetto e i controlli Web e le funzionalità del BoundField. Il vantaggio è che è necessario per le operazioni meno con il TemplateField man mano che il processo di conversione verrà hanno eseguito parte del lavoro per noi.

Per convertire un BoundField esistente in un TemplateField, fare clic sul collegamento Modifica colonne smart tag del controllo GridView, visualizzare la finestra di dialogo campi. Selezionare il BoundField convertire dall'elenco in basso a sinistra e fare clic sul collegamento "Converti il campo in un TemplateField" nell'angolo inferiore destro.


[![Convertire un BoundField in un TemplateField nella finestra di dialogo campi](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**Figura 4**: convertire un BoundField in un TemplateField nella finestra di dialogo campi ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image12.png))


Vado avanti e convertire il `FirstName` BoundField in un TemplateField. Dopo questa modifica non c'è alcuna differenza perceptive nella finestra di progettazione. Questo avviene perché la conversione di BoundField in un TemplateField crea un TemplateField che gestisce l'aspetto del BoundField. Nonostante esista in alcuna differenza visual a questo punto nella finestra di progettazione, il processo di conversione ha sostituito la sintassi dichiarativa del BoundField - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` : con la sintassi TemplateField seguente:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

Come si può notare, la TemplateField è costituito da due modelli un `ItemTemplate` che dispone di un'etichetta il cui `Text` proprietà è impostata sul valore del `FirstName` campo di dati e un `EditItemTemplate` con una casella di testo controllo cui `Text` proprietà è impostata per il `FirstName` campo dati. La sintassi di associazione dati - `<%# Bind("fieldName") %>` -indica che il campo dei dati  *`fieldName`*  è associato alla proprietà del controllo Web specificata.

Per aggiungere il `LastName` questo TemplateField è necessario aggiungere un altro controllo etichetta Web nel valore del campo dati il `ItemTemplate` e associare il `Text` proprietà `LastName`. Può essere eseguita manualmente o tramite la finestra di progettazione. Per eseguire l'operazione manualmente, aggiungere semplicemente la sintassi dichiarativa appropriata per il `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

Per aggiungere tramite la finestra di progettazione, fare clic sul collegamento Modifica modelli smart tag del controllo GridView. Verrà visualizzato interfaccia modifica dei modelli del controllo GridView. In questa smart tag interfaccia è un elenco di modelli in GridView. Poiché è disponibile un TemplateField a questo punto, i modelli soli nell'elenco a discesa sono tali modelli per il `FirstName` TemplateField insieme al `EmptyDataTemplate` e `PagerTemplate`. Il `EmptyDataTemplate` modello, se specificato, viene utilizzato per il rendering dell'output di GridView se non sono presenti risultati, i dati associati a GridView; il `PagerTemplate`, se specificato, viene utilizzato per eseguire il rendering dell'interfaccia di paging per un controllo GridView che supporta il paging.


[![Tramite la finestra di progettazione, è possibile modificare i modelli del controllo GridView](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**Figura 5**: il GridView modelli può essere modificato tramite Designer ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image15.png))


Per visualizzare anche il `LastName` nel `FirstName` TemplateField trascinare il controllo etichetta dalla casella degli strumenti nel `FirstName` del TemplateField `ItemTemplate` in GridView del modello di modifica di interfaccia.


[![Aggiungere un controllo Web etichetta ItemTemplate di FirstName TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**Figura 6**: aggiungere un controllo Web etichetta per il `FirstName` ItemTemplate del TemplateField ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image18.png))


A questo punto il controllo etichetta Web aggiunto di TemplateField ha relativo `Text` proprietà è impostata su "Label". È necessario modificare questa impostazione in modo che questa proprietà è associata il valore di `LastName` campo dati. Per eseguire questa fare clic sullo smart tag del controllo etichetta e scegliere l'opzione Modifica DataBindings.


[![Scegliere l'opzione di modifica dei DataBindings dell'etichetta Smart tag](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**Figura 7**: scegliere l'opzione di modifica dei DataBindings Smart Tag dell'etichetta ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image21.png))


Verrà visualizzata la finestra di dialogo DataBindings. Da qui è possibile selezionare la proprietà per partecipare all'associazione di dati dall'elenco a sinistra e selezionare il campo per associare i dati dall'elenco a discesa a destra. Scegliere il `Text` proprietà dal lato sinistro e `LastName` campo da destra e fare clic su OK.


[![Associare la proprietà di testo per il campo dei dati LastName](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**Figura 8**: associare il `Text` proprietà per il `LastName` campo dei dati ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image24.png))


> [!NOTE]
> La finestra di dialogo DataBindings consente di indicare se eseguire l'associazione dati bidirezionale. Se si lascia questa opzione è deselezionata, la sintassi di associazione dati `<%# Eval("LastName")%>` verrà utilizzato al posto di `<%# Bind("LastName")%>`. Entrambi gli approcci sono appropriato per questa esercitazione. Associazione dati bidirezionale diventa importante durante l'inserimento e modifica dei dati. Per visualizzare semplicemente i dati, tuttavia, entrambi gli approcci verranno funzionano altrettanto bene. Associazione dati bidirezionale in dettaglio si parlerà in esercitazioni future.


Richiedere qualche istante per visualizzare questa pagina tramite un browser. Come si può notare, GridView ancora include quattro colonne. Tuttavia, il `FirstName` colonna sono ora elencati *entrambi* il `FirstName` e `LastName` i valori dei campi dati.


[![FirstName e LastName valori vengono visualizzati in una singola colonna](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**Figura 9**: sia il `FirstName` e `LastName` valori vengono visualizzati in una singola colonna ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image27.png))


Per completare il primo passaggio, rimuovere il `LastName` BoundField e rinominare il `FirstName` del TemplateField `HeaderText` proprietà "Name". Dopo aver apportato queste modifiche markup dichiarativo del controllo GridView dovrebbe essere simile al seguente:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]


[![Prima di ogni dipendente e i cognomi vengono visualizzati in una colonna](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**Figura 10**: prima di ogni dipendente e i cognomi vengono visualizzati in una colonna ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Passaggio 3: Utilizzo del controllo di calendario per visualizzare il`HiredDate`campo

Visualizzazione di un valore del campo dati come testo in un controllo GridView è semplice come utilizzare BoundField. Per alcuni scenari, tuttavia, i dati sono meglio espresso mediante un particolare controllo Web anziché solo il testo. Queste attività di personalizzazione della visualizzazione dei dati è possibile con TemplateFields. Ad esempio, invece di visualizzare la data di assunzione del dipendente come testo, è possibile visualizzare un calendario (utilizzando [il controllo di calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) con data di assunzione evidenziato.

A tale scopo, avviare convertendo il `HiredDate` BoundField in un TemplateField. È sufficiente passare a smart tag del controllo GridView e fare clic sul collegamento Modifica colonne, visualizzare la finestra di dialogo campi. Selezionare il `HiredDate` BoundField e fare clic su "Converti il campo in un TemplateField."


[![Convertire il HiredDate BoundField in un TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**Figura 11**: convertire il `HiredDate` BoundField in un TemplateField ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image33.png))


Come illustrato nel passaggio 2, il BoundField verranno sostituite con un TemplateField che contiene un `ItemTemplate` e `EditItemTemplate` con un'etichetta e una casella di testo la cui `Text` proprietà sono associate le `HiredDate` valore utilizzando la sintassi di associazione dati `<%# Bind("HiredDate")%>`.

Per sostituire il testo con un controllo di calendario, modificare il modello rimuovendo l'etichetta e l'aggiunta di un controllo calendario. Dalla finestra di progettazione, selezionare Modifica modelli smart tag del controllo GridView e scegliere il `HireDate` del TemplateField `ItemTemplate` dall'elenco a discesa. Eliminare il controllo etichetta, quindi trascinare un controllo di calendario dalla casella degli strumenti all'interfaccia di modifica del modello.


[![Aggiungere un controllo di calendario per la HireDate ItemTemplate del TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**Figura 12**: aggiungere un controllo di calendario per la `HireDate` del TemplateField `ItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image36.png))


A questo punto, ogni riga in GridView conterrà un controllo calendario nel relativo `HiredDate` TemplateField. Tuttavia, il dipendente dell'effettivo `HiredDate` valore non è impostato in un punto qualsiasi nel controllo del calendario, ogni controllo di calendario per impostazione predefinita a mostrare la data e il mese corrente. Per risolvere questo problema, è necessario assegnare ogni dipendente `HiredDate` per il controllo di calendario [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) e [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) proprietà.

Smart tag del controllo calendario, scegliere Modifica DataBindings. Successivamente, eseguire il binding sia `SelectedDate` e `VisibleDate` proprietà per il `HiredDate` campo dati.


[![Associare la proprietà VisibleDate SelectedDate al campo dati HiredDate](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**Figura 13**: associare il `SelectedDate` e `VisibleDate` proprietà per il `HiredDate` campo dei dati ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image39.png))


> [!NOTE]
> La data selezionata del controllo calendario non devono necessariamente essere visibile. Ad esempio, un calendario può avere 1 agosto<sup>st</sup>, 1999 come la data selezionata, ma visualizza il mese corrente e l'anno. La data selezionata e visibili vengono specificati per il controllo di calendario `SelectedDate` e `VisibleDate` proprietà. Poiché si desidera sia selezionare il dipendente `HiredDate` e assicurarsi che sia visualizzato è necessario associare entrambe le proprietà per il `HireDate` campo dati.


Quando si visualizza la pagina in un browser, il calendario ora mostra il mese della data di assunzione del dipendente e seleziona una data specifica.


[![HiredDate del dipendente viene visualizzato nel controllo di calendario](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**Nella figura 14**: il dipendente `HiredDate` viene visualizzato nel controllo di calendario ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image42.png))


> [!NOTE]
> Conflitto con tutti gli esempi abbiamo visto finora, per questa esercitazione è stato fatto *non* impostare `EnableViewState` proprietà `False` per questo controllo GridView. Il motivo per questa decisione è che le date del controllo calendario scegliendo provoca un postback, impostare la data del calendario selezionata per la data selezionata. Se lo stato di visualizzazione del controllo GridView viene disabilitato, tuttavia, durante i postback dati di GridView sono riassociati all'origine dati sottostante, causando la data del calendario selezionata da impostare *nuovamente* al dipendente `HireDate`, sovrascrittura data scelta dall'utente.


Per questa esercitazione si tratta di una discussione opinabile poiché l'utente non è in grado di aggiornare il dipendente `HireDate`. Potrebbe essere opportuno configurare il controllo di calendario in modo che le date non selezionabili. Indipendentemente dal fatto che, in questa esercitazione viene illustrato che in alcuni casi lo stato di visualizzazione deve essere abilitato per fornire determinate funzionalità.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Passaggio 4: Che mostra il numero di giorni per il dipendente ha lavorato per l'azienda

Finora sono state due applicazioni di TemplateFields:

- La combinazione di due o più valori di campo di dati in una colonna, e
- Esprimere il valore di un campo di dati mediante un controllo Web anziché di testo

Un terzo utilizzo di TemplateFields è la visualizzazione di metadati relativi a GridView dati sottostanti. Oltre a visualizzare le date di assunzione dei dipendenti, ad esempio, potrebbe essere inoltre opportuno disporre di una colonna che visualizza il numero di giorni totale che sono state nel processo.

Ancora un altro uso di TemplateFields si manifesta in scenari quando i dati sottostanti devono essere visualizzate in modo diverso nel report pagina web nel formato archiviato nel database. Si supponga che il `Employees` tabella aveva un `Gender` campo che è archiviato il carattere `M` o `F` per indicare il sesso del dipendente. Quando si visualizzano le informazioni in una pagina web che potrebbe essere necessario visualizzare il sesso come "Male" o "Female", anziché solo "M" o "F".

Entrambi questi scenari possono essere gestite tramite la creazione di un *metodo di formattazione* nella classe code-behind della pagina ASP.NET (o in una libreria di classi separato, implementato come un `Shared` metodo) che viene richiamato dal modello. Questo tipo è un metodo di formattazione viene richiamato dal modello utilizzando la stessa sintassi di associazione dati illustrata in precedenza. Il metodo di formattazione può richiedere un numero qualsiasi di parametri, ma deve restituire una stringa. La stringa restituita è il codice HTML viene inserito nel modello.

Per illustrare questo concetto, si aumentano l'esercitazione per mostrare una colonna che indica il numero totale di giorni che è stato impiegato dal processo. Questo metodo di formattazione verranno in un `Northwind.EmployeesRow` dell'oggetto e restituire il numero di giorni per il dipendente è stato assunto sotto forma di stringa. Questo metodo può essere aggiunto a una classe code-behind della pagina ASP.NET, ma *deve* essere contrassegnato come `Protected` o `Public` per essere accessibili dal modello.


[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

Poiché il `HiredDate` campo può contenere `NULL` database valori, è innanzitutto necessario assicurarsi che il valore non è `NULL` prima di procedere con il calcolo. Se il `HiredDate` valore `NULL`, si limitino a restituire la stringa "Unknown"; in caso contrario `NULL`, si calcola la differenza tra l'ora corrente e `HiredDate` valore e restituiscono il numero di giorni.

Per utilizzare questo metodo è necessario per richiamarlo da un TemplateField in GridView utilizzando la sintassi di associazione dati. Per iniziare, aggiungere un nuovo TemplateField a GridView facendo clic sul collegamento Modifica colonne nello smart tag del controllo GridView e aggiunta di un nuovo TemplateField.


[![Aggiungere un nuovo TemplateField al controllo GridView.](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**Figura 15**: aggiungere un nuovo TemplateField a GridView ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image45.png))


Impostare questo TemplateField nuovo `HeaderText` proprietà su "Giorni nel processo" e il relativo `ItemStyle`del `HorizontalAlign` proprietà `Center`. Per chiamare il `DisplayDaysOnJob` metodo dal modello, aggiungere un `ItemTemplate` e usare la sintassi di Data Binding seguenti:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem`Restituisce un `DataRowView` oggetto corrispondente per il `DataSource` record associato al `GridViewRow`. Relativo `Row` restituirà fortemente tipizzata `Northwind.EmployeesRow`, che viene passato per il `DisplayDaysOnJob` (metodo). Questa sintassi di associazione dati possono essere visualizzati direttamente nel `ItemTemplate` (come illustrato nella sintassi dichiarativa seguente) o può essere assegnato al `Text` proprietà di un controllo etichetta Web.

> [!NOTE]
> In alternativa, anziché passare un `EmployeesRow` istanza, è possibile solo passare nel `HireDate` valore utilizzando `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Tuttavia, il `Eval` metodo restituisce un `Object`, pertanto è necessario modificare il nostro `DisplayDaysOnJob` firma del metodo per accettare un parametro di input di tipo `Object`, anziché. È ciecamente non è possibile eseguire il cast di `Eval("HireDate")` chiamata a un `DateTime` perché il `HireDate` colonna il `Employees` tabella può contenere `NULL` valori. Pertanto, è necessario accettare un `Object` come parametro di input per il `DisplayDaysOnJob` (metodo), verificare se disponesse di un database `NULL` valore (che può essere eseguita tramite `Convert.IsDBNull(objectToCheck)`) e quindi procedere di conseguenza.


A causa di questi sfumature ho scelto per passare l'intera `EmployeesRow` istanza. Nella prossima esercitazione vedremo un esempio più di adattamento per l'utilizzo di `Eval("columnName")` sintassi per passare un parametro di input in un metodo di formattazione.

Di seguito è illustrato la sintassi dichiarativa per il controllo GridView. dopo aver aggiunto il TemplateField e `DisplayDaysOnJob` chiamato dal metodo di `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

La figura 16 mostra l'esercitazione completa, quando viene visualizzato tramite un browser.


[![Viene visualizzato il numero di giorni, che il dipendente è stato del processo](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**Figura 16**: il numero di giorni viene visualizzato il dipendente è stato del processo ([fare clic per visualizzare l'immagine ingrandita](using-templatefields-in-the-gridview-control-vb/_static/image48.png))


## <a name="summary"></a>Riepilogo

TemplateField nel controllo GridView consente un maggiore livello di flessibilità per la visualizzazione di dati non disponibili con gli altri controlli di campo. TemplateFields sono ideali per le situazioni in cui:

- Più campi dati devono essere visualizzate in una colonna di GridView
- La data è espressa meglio con un controllo Web anziché come testo normale
- L'output dipende dai dati sottostanti, ad esempio la visualizzazione dei metadati o nelle riformattando i dati

Oltre a personalizzare la visualizzazione dei dati, TemplateFields vengono inoltre utilizzati per personalizzare le interfacce utente utilizzate per la modifica e l'inserimento di dati, come vedremo in futuro esercitazioni.

Le due esercitazioni continuano a esplorare i modelli, a partire da illustrato l'utilizzo di TemplateFields in un controllo DetailsView. Successivamente, si sarà attivare al FormView, che utilizza i modelli anziché i campi per garantire una maggiore flessibilità al layout e la struttura dei dati.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Dan Jagers. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](custom-formatting-based-upon-data-vb.md)
[Successivo](using-templatefields-in-the-detailsview-control-vb.md)
