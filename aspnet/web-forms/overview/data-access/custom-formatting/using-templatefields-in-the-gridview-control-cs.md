---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: Uso di TemplateFields nel controllo GridView (c#) | Microsoft Docs
author: rick-anderson
description: Per garantire flessibilità, il controllo GridView offre il TemplateField, che esegue il rendering usando un modello. Un modello può includere una combinazione di codice HTML statico, controlli Web, e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: fe84bd24824f4a0326a6e8d41c0d291c7ef585af
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363831"
---
<a name="using-templatefields-in-the-gridview-control-c"></a>Uso di TemplateFields nel controllo GridView (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) o [Scarica il PDF](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Per garantire flessibilità, il controllo GridView offre il TemplateField, che esegue il rendering usando un modello. Un modello può includere una combinazione di codice HTML statico, controlli Web e sintassi di associazione dati. In questa esercitazione verrà esaminato come utilizzare il TemplateField per ottenere un maggiore livello di personalizzazione con il controllo GridView.


## <a name="introduction"></a>Introduzione

Il controllo GridView è costituito da un set di campi che indicano quali proprietà dal `DataSource` devono essere inclusi nell'output sottoposto a rendering con la modalità di visualizzazione dei dati. Il tipo più semplice è il BoundField, che consente di visualizzare un valore di dati come testo. Altri tipi di campo visualizzano i dati utilizzando gli elementi HTML alternativi. CampoCasellaDiControllo, ad esempio, esegue il rendering come una casella di controllo il cui stato di selezione dipende dal valore di un campo dati specificato. ImageField esegue il rendering di un'immagine la cui origine dell'immagine si basa su un campo dati specificato. Collegamenti ipertestuali e pulsanti, il cui stato dipende dal valore di campo di dati sottostante possono essere sottoposto a rendering utilizzando i tipi di campo HyperLinkField e ButtonField.

Anche se i tipi di campo CampoCasellaDiControllo, ImageField, HyperLinkField e ButtonField consentono una visualizzazione alternativa dei dati, ancora sono piuttosto limitate rispetto alla formattazione. Un CampoCasellaDiControllo può visualizzare solo una singola casella di controllo, mentre un ImageField può visualizzare solo una singola immagine. Cosa accade se un campo specifico necessario visualizzare un testo, una casella di controllo, *e* un'immagine, tutte basata su valori dei campi dati diverso? Oppure cosa accade se si vuole visualizzare i dati usando un controllo Web per la casella di controllo, immagine, collegamento ipertestuale o sul pulsante? Inoltre, i BoundField limita la visualizzazione per un singolo campo dati. Cosa accade se si vuole mostrare due o più valori del campo dati in una singola colonna GridView?

Per supportare questo livello di flessibilità GridView offre il TemplateField, che esegue il rendering usando un *modello*. Un modello può includere una combinazione di codice HTML statico, controlli Web e sintassi di associazione dati. Inoltre, il TemplateField ha un'ampia gamma di modelli che può essere utilizzato per personalizzare il rendering per situazioni diverse. Ad esempio, il `ItemTemplate` viene utilizzata per impostazione predefinita per il rendering della cella per ogni riga, ma il `EditItemTemplate` modello può essere utilizzato per personalizzare l'interfaccia durante la modifica dei dati.

In questa esercitazione verrà esaminato come utilizzare il TemplateField per ottenere un maggiore livello di personalizzazione con il controllo GridView. Nel [esercitazione precedente](custom-formatting-based-upon-data-cs.md) è stato illustrato come personalizzare la formattazione di base di dati sottostante tramite il `DataBound` e `RowDataBound` gestori eventi. Un altro modo per personalizzare la formattazione in base ai dati sottostanti è chiamando i metodi di formattazione all'interno di un modello. Esamineremo questa tecnica anche questa esercitazione.

Per questa esercitazione si userà TemplateFields per personalizzare l'aspetto di un elenco di dipendenti. In particolare, è possibile elencare tutti i dipendenti, ma verrà visualizzato il dipendente nomi e cognomi in una colonna, data di assunzione in un controllo calendario e una colonna di stato che indica il numero di giorni è state impiegate presso la società.


[![Tre TemplateFields vengono usati per personalizzare la visualizzazione](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Figura 1**: tre TemplateFields vengono usati per personalizzare la visualizzazione ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Passaggio 1: Associazione dei dati a GridView

In reporting scenari in cui è necessario usare TemplateFields per personalizzare l'aspetto, trovo più semplice iniziare, creare un controllo GridView che contiene solo i BoundField prima e quindi aggiungere di nuovo TemplateFields o convertire i BoundField esistente a TemplateFields in base alle esigenze. Di conseguenza, iniziare questa esercitazione aggiungendo un controllo GridView alla pagina tramite la finestra di progettazione e associarlo a ObjectDataSource che restituisce l'elenco dei dipendenti. Questi passaggi verranno creato un controllo GridView con BoundField per ognuno dei campi employee.

Aprire il `GridViewTemplateField.aspx` pagina e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Dallo smart tag del controllo GridView scegliere di aggiungere un nuovo controllo ObjectDataSource che richiama il `EmployeesBLL` della classe `GetEmployees()` (metodo).


[![Aggiungere un nuovo controllo ObjectDataSource che richiama il metodo EmployeesDataTable](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**Figura 2**: aggiungere un nuovo controllo ObjectDataSource tale Invoke il `GetEmployees()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image6.png))


Associazione GridView in questo modo verrà aggiunto automaticamente un BoundField per ciascuna delle proprietà del dipendente: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, e `Country`. Per questo report è possibile non preoccuparsi di visualizzare il `EmployeeID`, `ReportsTo`, o `Country` proprietà. Per rimuovere questi BoundField è possibile:

- Utilizzare il finestra di dialogo campi fare clic sul collegamento Modifica colonne dallo smart tag del controllo GridView per visualizzare questa finestra di dialogo. Successivamente, selezionare i BoundField dall'angolo inferiore sinistro elenco e fare clic sulla X rossa sul pulsante per rimuovere i BoundField.
- Modificare la sintassi dichiarativa di GridView manualmente dalla visualizzazione origine, eliminare il `<asp:BoundField>` (elemento) per i BoundField di cui si vuole rimuovere.

Dopo aver rimosso il `EmployeeID`, `ReportsTo`, e `Country` BoundField markup di GridView simile a:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Si consiglia di visualizzare lo stato di avanzamento in un browser. A questo punto si dovrebbe essere visualizzata una tabella con un record per ogni dipendente e verranno visualizzate quattro colonne: uno per il dipendente cognome, uno per il nome di battesimo, uno per il titolo e una data di assunzione.


[![Il LastName, FirstName, titolo e HireDate campi vengono visualizzati per ogni dipendente](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Figura 3**: il `LastName`, `FirstName`, `Title`, e `HireDate` campi vengono visualizzati per ogni dipendente ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Passaggio 2: Visualizzare i nomi e il cognome in una singola colonna

Attualmente, ogni dipendente del primo e ultimo nomi vengono visualizzati in una colonna separata. Potrebbe essere interessante per combinarle in un'unica colonna. A tale scopo è necessario usare un TemplateField. È possibile sia aggiungere un nuovo TemplateField, aggiungere il markup necessario e sintassi di associazione dati e quindi eliminare il `FirstName` e `LastName` BoundField oppure è possibile convertire il `FirstName` BoundField in un TemplateField, modificare il TemplateField da includere il `LastName` valore e quindi rimuovere il `LastName` BoundField.

Entrambi gli approcci net lo stesso risultato, ma personalmente preferisco conversione BoundField in TemplateFields quando possibile, perché la conversione viene aggiunto automaticamente un `ItemTemplate` e `EditItemTemplate` con i controlli Web e la sintassi di associazione dati imitare l'aspetto le funzionalità dei BoundField e. Il vantaggio è che sarà necessario comportano un lavoro minore con il TemplateField man mano che il processo di conversione verrà sono eseguite alcune operazioni per noi.

Per convertire un BoundField esistente in un TemplateField, fare clic sul collegamento Modifica colonne dallo smart tag del controllo GridView, visualizzare la finestra di dialogo campi. Selezionare i BoundField convertire dall'elenco nell'angolo inferiore sinistro e quindi fare clic sul collegamento "Converti il campo in un TemplateField" nell'angolo inferiore destro.


[![Convertire un BoundField in un TemplateField dalla finestra di dialogo campi](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Figura 4**: converte un BoundField in un TemplateField dalla finestra di dialogo campi ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image12.png))


Proseguo e convertire il `FirstName` BoundField in un TemplateField. Dopo questa modifica è indifferente perceptive nella finestra di progettazione. Questo avviene perché la conversione i BoundField in un TemplateField crea un TemplateField che gestisce l'aspetto dei BoundField. Nonostante esista in corso alcuna differenza visual a questo punto nella finestra di progettazione, questo processo di conversione ha sostituito la sintassi dichiarativa del BoundField - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` : con la sintassi TemplateField seguente:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Come si può notare, il TemplateField è costituita da due modelli di un' `ItemTemplate` che dispone di un'etichetta il cui `Text` proprietà è impostata sul valore del `FirstName` campo di dati e un `EditItemTemplate` con una casella di testo controllo la cui `Text` viene anche impostata per il `FirstName` campo dati. La sintassi di Data Binding - `<%# Bind("fieldName") %>` -indica che il campo dati *`fieldName`* è associato alla proprietà del controllo Web specificata.

Per aggiungere il `LastName` questo TemplateField dobbiamo aggiungere un altro controllo Web etichetta nel valore del campo dati il `ItemTemplate` ed eseguire l'associazione relativa `Text` proprietà `LastName`. Questa operazione può essere eseguita manualmente o tramite la finestra di progettazione. Per eseguire l'operazione manualmente, è sufficiente aggiungere la sintassi dichiarativa appropriata per il `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Per aggiungerla tramite la finestra di progettazione, fare clic sul collegamento di modifica modelli dallo smart tag del controllo GridView. Interfaccia per la modifica modello del controllo GridView verranno visualizzate. In questo smart tag dell'interfaccia è un elenco dei modelli in GridView. Poiché è disponibile un TemplateField a questo punto, i solo i modelli presenti nell'elenco a discesa sono tali modelli per la `FirstName` TemplateField lungo con la `EmptyDataTemplate` e `PagerTemplate`. Il `EmptyDataTemplate` modello, se specificato, viene utilizzato per eseguire il rendering di output del controllo GridView se non sono presenti risultati i dati associati a GridView; il `PagerTemplate`, se specificato, viene utilizzato per il rendering di interfaccia di paging per un controllo GridView che supporta il paging.


[![I modelli del controllo GridView possono essere modificati tramite la finestra di progettazione](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Figura 5**: modelli può essere modificato tramite la progettazione di GridView di ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image15.png))


Anche visualizzare le `LastName` nella `FirstName` TemplateField trascinare il controllo etichetta nella casella degli strumenti nel `FirstName` del TemplateField `ItemTemplate` in GridView di modifica dei modelli dell'interfaccia.


[![Aggiungere un controllo Web etichetta a ItemTemplate di FirstName TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Figura 6**: aggiungere un controllo Web etichetta per il `FirstName` ItemTemplate del TemplateField ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image18.png))


A questo punto ha il controllo etichetta Web aggiunto per il TemplateField relativo `Text` proprietà è impostata su "Label". È necessario modificare questa impostazione in modo che questa proprietà è associata al valore della `LastName` invece del campo dati. Per eseguire questo, fare clic sullo smart tag del controllo etichetta e scegliere l'opzione Modifica DataBindings.


[![Scegliere l'opzione di Modifica DataBindings dallo Smart Tag dell'etichetta](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Figura 7**: scegliere l'opzione Modifica DataBindings dallo Smart Tag dell'etichetta ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image21.png))


Verrà visualizzata la finestra di dialogo DataBindings. Da qui è possibile selezionare la proprietà per partecipare al Data Binding nell'elenco a sinistra e scegliere il campo per associare i dati nell'elenco a discesa a destra. Scegliere il `Text` proprietà dal lato sinistro e `LastName` campo a destra e fare clic su OK.


[![Associare la proprietà Text al campo LastName dati](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Figura 8**: associare il `Text` proprietà per il `LastName` campo dati ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image24.png))


> [!NOTE]
> La finestra di dialogo DataBindings consente di indicare se eseguire l'associazione dati bidirezionale. Se si lascia questa opzione è deselezionata, la sintassi di Data Binding `<%# Eval("LastName")%>` verrà usato al posto di `<%# Bind("LastName")%>`. Entrambi gli approcci sono ottimale per questa esercitazione. Data Binding bidirezionale diventa importante quando vengono inseriti e la modifica dei dati. Per visualizzare semplicemente i dati, tuttavia, entrambi gli approcci funzioneranno altrettanto bene. Si parlerà dell'associazione dati bidirezionale dettagliatamente in esercitazioni future.


Richiedere qualche istante per visualizzare questa pagina tramite un browser. Come può notare, il controllo GridView ancora include quattro colonne. Tuttavia, il `FirstName` colonna sono ora elencati *entrambi* il `FirstName` e `LastName` valori del campo dati.


[![FirstName e LastName valori vengono visualizzati in una singola colonna](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Figura 9**: sia il `FirstName` e `LastName` i valori vengono visualizzati in una sola colonna ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image27.png))


Per completare il primo passaggio, rimuovere il `LastName` BoundField e rinominare il `FirstName` del TemplateField `HeaderText` proprietà "Name". Dopo tali modifiche markup dichiarativo del controllo GridView dovrebbe essere simile al seguente:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![Il primo e i cognomi di ogni dipendente vengono visualizzati in una colonna](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Figura 10**: il primo e i cognomi di ogni dipendente vengono visualizzati in una colonna ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Passaggio 3: Utilizzo del controllo di calendario alla visualizzazione di`HiredDate`campo

La visualizzazione di un valore del campo dati come testo in un controllo GridView è semplice come l'utilizzo di un BoundField. Per determinati scenari, tuttavia, la data è meglio espressa utilizzando un particolare controllo Web anziché solo con il testo. È possibile con TemplateFields tale personalizzazione della visualizzazione dei dati. Ad esempio, invece di visualizzare la data di assunzione del dipendente come testo, abbiamo potuto dimostrare un calendario (tramite [il controllo calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) con data di assunzione evidenziato.

A tale scopo, iniziare convertendo il `HiredDate` BoundField in un TemplateField. È sufficiente passare a smart tag del controllo GridView e fare clic sul collegamento Modifica colonne, visualizzare la finestra di dialogo campi. Selezionare il `HiredDate` BoundField e fare clic su "Converti il campo in un TemplateField."


[![Convertire i HiredDate BoundField in un TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Figura 11**: converte il `HiredDate` BoundField in un TemplateField ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image33.png))


Come abbiamo visto nel passaggio 2, si sostituirà i BoundField con un TemplateField che contiene un `ItemTemplate` e `EditItemTemplate` con un'etichetta e una casella di testo cui `Text` sono associate ai `HiredDate` valore utilizzando la sintassi di associazione dati `<%# Bind("HiredDate")%>`.

Per sostituire il testo con un controllo di calendario, modificare il modello rimuovendo l'etichetta e aggiunta di un controllo calendario. Dalla finestra di progettazione, selezionare Modifica modelli dallo smart tag del controllo GridView e scegliere il `HireDate` del TemplateField `ItemTemplate` nell'elenco a discesa. Successivamente, eliminare il controllo etichetta e trascinare un controllo di calendario dalla casella degli strumenti all'interfaccia di modifica del modello.


[![Aggiungere un controllo calendario per l'HireDate ItemTemplate del TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Figura 12**: aggiungere un controllo calendario per il `HireDate` del TemplateField `ItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image36.png))


A questo punto, ogni riga in GridView conterrà un controllo calendario nel relativo `HiredDate` TemplateField. Tuttavia, il dipendente dell'effettivo `HiredDate` valore non è impostato un punto qualsiasi nel controllo calendario, causando ogni controllo di calendario per l'impostazione predefinita che mostra la data e il mese corrente. Per risolvere questo problema, è necessario assegnare ogni dipendente `HiredDate` del controllo calendario [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) e [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) proprietà.

Dallo smart tag del controllo calendario, scegliere Modifica DataBindings. Successivamente, eseguire l'associazione entrambe `SelectedDate` e `VisibleDate` delle proprietà per il `HiredDate` campo dati.


[![Associare le proprietà di VisibleDate e SelectedDate al campo dati HiredDate](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Figura 13**: associare il `SelectedDate` e `VisibleDate` delle proprietà per il `HiredDate` campo dati ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image39.png))


> [!NOTE]
> Data selezionata del controllo calendario non debba necessariamente essere visibile. Ad esempio, un calendario può avere 1 ° agosto<sup>st</sup>, 1999 come la data selezionata, ma visualizza il mese corrente e l'anno. La data selezionata e visibili vengono specificati per il controllo di calendario `SelectedDate` e `VisibleDate` proprietà. Poiché si vogliono selezionare entrambe del dipendente `HiredDate` e assicurarsi che venga visualizzato dobbiamo entrambe queste proprietà per associare il `HireDate` campo dati.


Quando si visualizza la pagina in un browser, il calendario a questo punto viene illustrato il mese della data di assunzione del dipendente e seleziona data specifica.


[![HiredDate del dipendente viene visualizzato nel controllo calendario](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Figura 14**: il dipendente `HiredDate` viene visualizzato nel controllo calendario ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image42.png))


> [!NOTE]
> Contrariamente a tutti gli esempi che abbiamo visto finora, per questa esercitazione abbiamo *non* impostare `EnableViewState` proprietà `false` per questo controllo GridView. Il motivo di questa decisione è perché scegliendo le date del controllo calendario, un postback, impostando la data del calendario selezionata per la data selezionata. Se lo stato di visualizzazione del controllo GridView è disabilitato, tuttavia, in ogni postback dei dati del controllo GridView sono riassociati alla relativa origine dati sottostante, causando la data del calendario selezionata da impostare *nuovamente* al dipendente `HireDate`, sovrascrittura data scelta dall'utente.


Per questa esercitazione è una discussione opinabile poiché l'utente non è in grado di aggiornare il dipendente `HireDate`. Sarebbe probabilmente consigliabile configurare il controllo di calendario in modo che le date non sono selezionabili. Indipendentemente dal fatto che, in questa esercitazione viene illustrato che in alcuni casi lo stato di visualizzazione deve essere abilitato per offrire determinate funzionalità.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Passaggio 4: Che mostra il numero di giorni il dipendente ha lavorato per l'azienda

Finora abbiamo visto due applicazioni di TemplateFields:

- Combinazione di due o più valori del campo dati in una sola colonna, e
- Esprimere un valore del campo dati usando un controllo Web invece del testo

Un terzo uso di TemplateFields è in visualizzazione di metadati del controllo GridView i dati sottostanti. Oltre a illustrare le date di assunzione dei dipendenti, ad esempio, si potrebbe essere anche opportuno deve includere una colonna che visualizza il numero di giorni totale che sono state sul processo.

Ancora un altro uso di TemplateFields si verifica negli scenari quando devono essere visualizzato in modo diverso nel report pagina web nel formato archiviato nel database i dati sottostanti. Si supponga che il `Employees` tabella aveva una `Gender` campo in cui è archiviato il carattere `M` o `F` per indicare il sesso del dipendente. Quando si visualizzano queste informazioni in una pagina web che potrebbe essere necessario visualizzare il sesso come "Male" o "Female", anziché semplicemente "M" o "F".

Entrambi questi scenari possono essere gestiti mediante la creazione di un *metodo di formattazione* nella classe code-behind della pagina ASP.NET (o in una libreria di classi separato, implementato come un `static` (metodo)) che viene richiamato dal modello. Questo tipo è un metodo di formattazione viene richiamato dal modello usando la stessa sintassi di associazione dati illustrata in precedenza. Il metodo di formattazione può richiedere un numero qualsiasi di parametri, ma deve restituire una stringa. Questa stringa restituita è il codice HTML che viene inserito nel modello.

Per illustrare questo concetto, è possibile aumentare il corso di questa esercitazione per mostrare una colonna che elenca il numero totale di giorni di che un dipendente ha completato il processo. Questo metodo di formattazione richiederà un `Northwind.EmployeesRow` dell'oggetto e restituire il numero di giorni il dipendente è stato impiegato sotto forma di stringa. Questo metodo può essere aggiunto alla classe code-behind della pagina ASP.NET, ma *devono* contrassegnato come `protected` o `public` perché sia accessibile dal modello.


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Poiché il `HiredDate` campo può contenere `NULL` valori è innanzitutto necessario assicurarsi che il valore non di database `NULL` prima di procedere con il calcolo. Se il `HiredDate` valore è `NULL`, abbiamo semplicemente restituire la stringa "Unknown"; in caso contrario `NULL`, si calcola la differenza tra l'ora corrente e il `HiredDate` valore e restituire il numero di giorni.

Per poter usare questo metodo occorre richiamarla da un TemplateField in GridView mediante la sintassi di associazione dati. Iniziare aggiungendo un nuovo TemplateField a GridView facendo clic sul collegamento Modifica colonne nello smart tag del controllo GridView e aggiungendo un TemplateField nuovo.


[![Aggiungere un nuovo TemplateField a GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Figura 15**: aggiungere un nuovo TemplateField a GridView ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image45.png))


Impostare questo nuovo TemplateField `HeaderText` proprietà su "Giorni del processo" e la relativa `ItemStyle`del `HorizontalAlign` proprietà `Center`. Per chiamare il `DisplayDaysOnJob` metodo dal modello, aggiungere un `ItemTemplate` e usare la sintassi di Data Binding seguenti:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` Restituisce un `DataRowView` oggetto corrispondente per il `DataSource` record associato al `GridViewRow`. Relativi `Row` restituisce proprietà fortemente tipizzato `Northwind.EmployeesRow`, che viene passato al `DisplayDaysOnJob` (metodo). Questa sintassi di associazione dati possono essere visualizzati direttamente nella `ItemTemplate` (come illustrato nella sintassi dichiarativa riportato di seguito) o possono essere assegnati al `Text` proprietà di un controllo etichetta Web.

> [!NOTE]
> In alternativa, anziché passare un `EmployeesRow` istanza, è possibile solo passare nel `HireDate` valore usando `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Tuttavia, il `Eval` restituzione del metodo un' `object`, quindi dobbiamo modificare nostro `DisplayDaysOnJob` firma del metodo per accettare un parametro di input di tipo `object`, invece. Abbiamo alla cieca non possiamo eseguire il cast il `Eval("HireDate")` le chiamate a un `DateTime` perché la `HireDate` colonna il `Employees` tabella può contenere `NULL` valori. Pertanto, è necessario accettare un `object` come parametro di input per il `DisplayDaysOnJob` metodo, verificare se disponesse di un database `NULL` valore (che è possibile usare `Convert.IsDBNull(objectToCheck)`) e quindi procedere di conseguenza.


A causa di queste sfumature, ho scelto di passare all'interno dell'intero `EmployeesRow` istanza. Nella prossima esercitazione vedremo un esempio di adattamento altre per l'uso di `Eval("columnName")` sintassi per passare un parametro di input in un metodo di formattazione.

Di seguito è riportata la sintassi dichiarativa per il controllo GridView dopo aver aggiunto il TemplateField e il `DisplayDaysOnJob` chiamato dal metodo di `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

Figura 16 mostra l'esercitazione completa, quando viene visualizzato tramite un browser.


[![Viene visualizzato il numero di giorni che il dipendente ha completato il processo](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**Figura 16**: il numero di giorni viene visualizzato il dipendente è stato sul processo ([fare clic per visualizzare l'immagine con dimensioni normali](using-templatefields-in-the-gridview-control-cs/_static/image48.png))


## <a name="summary"></a>Riepilogo

TemplateField nel controllo GridView consente un grado superiore di flessibilità per la visualizzazione dei dati rispetto a quella disponibile con gli altri controlli di campo. TemplateFields sono ideali per le situazioni in cui:

- Più campi dati devono essere visualizzati in una colonna GridView
- La data è espressa meglio tramite un controllo Web anziché di testo normale
- L'output varia a seconda dei dati sottostanti, ad esempio la visualizzazione dei metadati o in riformattazione dei dati

Oltre a personalizzare la visualizzazione di dati, TemplateFields usati anche per personalizzare le interfacce utente usate per la modifica e inserimento di dati, come vedremo in futuro le esercitazioni.

Le due esercitazioni continuano a esplorare i modelli, a partire da esaminare l'uso di TemplateFields in un controllo DetailsView. In seguito, si rivolgeranno al FormView, che usa i modelli al posto dei campi per offrire maggiore flessibilità per il layout e la struttura dei dati.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Dan Jagers. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](custom-formatting-based-upon-data-cs.md)
> [Successivo](using-templatefields-in-the-detailsview-control-cs.md)
