---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
title: Controllare la denominazione di ID nelle pagine di contenuto (VB) | Documenti Microsoft
author: rick-anderson
description: Viene illustrato come controlli ContentPlaceHolder fungono da un contenitore di denominazione e pertanto a livello di codice utilizza un controllo difficile (tramite FindConrol)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: dbb024a6-f043-4fc5-ad66-56556711875b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9523fe5b241b6ff45927f142eb844a716822336b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="control-id-naming-in-content-pages-vb"></a>ID di controllo di denominazione nelle pagine di contenuto (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_VB.pdf)

> Viene illustrato come controlli ContentPlaceHolder fungono da un contenitore di denominazione e pertanto a livello di codice utilizza un controllo difficile (tramite FindConrol). Esamina il problema e soluzioni alternative. Spiega inoltre come accedere a livello di codice il valore ClientID risultante.


## <a name="introduction"></a>Introduzione

Tutti i controlli server ASP.NET includono un `ID` proprietà che identifica il controllo in modo univoco e costituisce il mezzo mediante il quale il controllo si accede a livello di codice nella classe code-behind. Analogamente, gli elementi in un documento HTML è possono includere un `id` attributo che identifica in modo univoco l'elemento; questi `id` valori vengono spesso utilizzati in script sul lato client per fare riferimento a un particolare elemento HTML a livello di codice. Detto questo, si potrebbe presumere che quando un controllo server ASP.NET viene eseguito il rendering in HTML, il relativo `ID` viene utilizzato come valore di `id` valore dell'elemento HTML di cui è stato eseguito rendering. Questo non avviene necessariamente poiché in alcune circostanze un singolo controllo con una singola `ID` valore può essere utilizzato più volte nel markup sottoposto a rendering. Si consideri un controllo GridView che include un TemplateField con un controllo etichetta Web con un `ID` valore `ProductName`. Quando il controllo GridView viene associato all'origine dati in fase di esecuzione, questa etichetta viene ripetuta una volta per ogni riga GridView. Viene eseguito esigenze etichetta univoca `id` valore.

Per gestire tali scenari, ASP.NET consente determinati controlli essere indicata come contenitori di denominazione. Un contenitore di denominazione viene utilizzato come un nuovo `ID` dello spazio dei nomi. Qualsiasi controllo server che vengono visualizzati all'interno del contenitore di denominazione è loro visualizzabile `id` valore preceduti il `ID` del controllo del contenitore di denominazione. Ad esempio, il `GridView` e `GridViewRow` classi sono entrambe contenitori di denominazione. Di conseguenza, un controllo etichetta definito in un GridView TemplateField con `ID` `ProductName` viene sottoposto a rendering `id` valore `GridViewID_GridViewRowID_ProductName`. Poiché *GridViewRowID* è univoco per ogni riga GridView, il valore risultante `id` valori siano univoci.

> [!NOTE]
> Il [ `INamingContainer` interfaccia](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) viene utilizzato per indicare che un determinato controllo server ASP.NET deve funzionare come un contenitore di denominazione. Il `INamingContainer` interfaccia ortografico non i metodi di qualsiasi tipo che deve implementare il controllo del server; invece, viene utilizzato un marcatore. Per generare il markup sottoposto a rendering, se un controllo implementa questa interfaccia quindi il motore ASP.NET automaticamente i prefissi relativi `ID` valore per i relativi discendenti sottoposto a rendering `id` i valori di attributo. Questo processo è descritto in dettaglio nel passaggio 2.


Denominazione dei contenitori non solo modificare il rendering `id` valore dell'attributo, ma anche influiscono su come il controllo può fare riferimento a livello di codice dalla classe code-behind della pagina ASP.NET. Il `FindControl("controlID")` metodo viene in genere utilizzato per fare riferimento a livello di codice a un controllo Web. Tuttavia, `FindControl` non penetrare tramite contenitori di denominazione. Di conseguenza, non è possibile utilizzare direttamente la `Page.FindControl` metodo per fare riferimento a controlli all'interno di un controllo GridView o altro contenitore di denominazione.

Come può conclusione, pagine master e gli elementi ContentPlaceHolder vengono entrambi implementati come contenitori di denominazione. In questa esercitazione verrà esaminato come master pagine effetto HTML elemento `id` valori e metodi per fare riferimento a livello di programmazione di controlli Web all'interno di una pagina di contenuto con `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Passaggio 1: Aggiunta di una nuova pagina ASP.NET

Per dimostrare i concetti illustrati in questa esercitazione, aggiungere una nuova pagina ASP.NET per il sito Web. Creare una nuova pagina contenuta denominata `IDIssues.aspx` nella cartella radice di associazione per il `Site.master` pagina master.


![Aggiungere il contenuto di pagina IDIssues.aspx nella cartella radice](control-id-naming-in-content-pages-vb/_static/image1.png)

**Figura 01**: aggiungere la pagina contenuta `IDIssues.aspx` nella cartella radice


Visual Studio crea automaticamente un controllo contenuto per ognuno dei quattro ContentPlaceHolder della pagina master. Come indicato nel [ *diversi ContentPlaceHolder e contenuto predefinito* ](multiple-contentplaceholders-and-default-content-vb.md) dell'esercitazione, se un controllo contenuto non è presente il contenuto della pagina master predefinito ContentPlaceHolder viene generato invece. Poiché il `QuickLoginUI` e `LeftColumnContent` gli elementi ContentPlaceHolder contengono il markup predefinito appropriato per questa pagina, vado avanti e rimuovere i corrispondenti controlli del contenuto da `IDIssues.aspx`. Markup dichiarativo della pagina di contenuto a questo punto, dovrebbe essere simile al seguente:


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample1.aspx)]

Nel [ *che specifica il titolo, tag Meta e altre intestazioni HTML della pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) esercitazione è stata creata una classe di base di pagine personalizzati (`BasePage`) che configura automaticamente il titolo della pagina, se è impostare in modo non esplicito. Per il `IDIssues.aspx` pagina per poter usare questa funzionalità, classe code-behind della pagina deve derivare dal `BasePage` classe (anziché `System.Web.UI.Page`). Modificare la definizione della classe code-behind in modo che risulti simile al seguente:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample2.vb)]

Infine, aggiornare il `Web.sitemap` file da includere una voce per questa lezione nuovo. Aggiungere un `<siteMapNode>` elemento e impostare il relativo `title` e `url` gli attributi "Problemi di denominazione ID di controllo" e `~/IDIssues.aspx`, rispettivamente. Dopo aver apportato questa aggiunta il `Web.sitemap` markup del file dovrebbe essere simile al seguente:


[!code-xml[Main](control-id-naming-in-content-pages-vb/samples/sample3.xml)]

Come illustrato nella figura 2, la nuova voce della mappa del sito in `Web.sitemap` verrà immediatamente aggiornata nella sezione lezioni nella colonna sinistra.


![La sezione lezioni ora include un collegamento a &quot;problemi di denominazione ID controllo&quot;](control-id-naming-in-content-pages-vb/_static/image2.png)

**Figura 02**: la sezione lezioni ora include un collegamento a "Problemi di denominazione degli ID di controllo"


## <a name="step-2-examining-the-renderedidchanges"></a>Passaggio 2: Esaminare il rendering`ID`modifiche

Per comprendere meglio le modifiche di ASP.NET motore verso il rendering `id` controlla i valori del server, aggiungere alcuni controlli Web per il `IDIssues.aspx` pagina e quindi visualizzare il markup sottoposto a rendering inviato al browser. In particolare, digitare il testo "inserire la tua età:" seguita da un controllo casella di testo Web. Ulteriormente verso il basso nella pagina aggiungere un controllo Web Button e un controllo etichetta Web. Impostare la casella di testo `ID` e `Columns` proprietà `Age` e 3, rispettivamente. Impostare il pulsante `Text` e `ID` proprietà su "Invia" e `SubmitButton`. Cancellare il contenuto dell'etichetta `Text` proprietà e impostare il relativo `ID` a `Results`.

Markup dichiarativo del controllo contenuto a questo punto dovrebbe essere simile al seguente:


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample4.aspx)]

Figura 3 mostra la pagina visualizzata mediante Progettazione di Visual Studio.


[![La pagina include i tre controlli Web: una casella di testo, pulsante ed etichetta](control-id-naming-in-content-pages-vb/_static/image4.png)](control-id-naming-in-content-pages-vb/_static/image3.png)

**Figura 03**: la pagina include tre controlli Web: una casella di testo, pulsante e l'etichetta ([fare clic per visualizzare l'immagine ingrandita](control-id-naming-in-content-pages-vb/_static/image5.png))


Visitare la pagina tramite un browser e visualizzare l'origine HTML. Come il markup riportata di seguito, il `id` i valori degli elementi HTML per i controlli Web di etichetta, pulsante e casella di testo sono una combinazione del `ID` valori dei controlli Web e `ID` i valori di denominazione dei contenitori nella pagina.


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample5.html)]

Come notato in precedenza in questa esercitazione, la pagina master sia relativo ContentPlaceHolder fungono da contenitori di denominazione. Di conseguenza, entrambe contribuiscono sottoposto a rendering `ID` i valori dei controlli annidati. Richiedere la casella di testo `id` attributo, ad esempio: `ctl00_MainContent_Age`. Tenere presente che il controllo TextBox `ID` valore `Age`. Questo è il prefisso al relativo controllo di ContentPlaceHolder `ID` valore `MainContent`. Inoltre, questo valore è il prefisso con la pagina master `ID` valore `ctl00`. L'effetto finale è un `id` il valore di attributo costituito il `ID` i valori della pagina master, il controllo ContentPlaceHolder e la casella di testo.

Figura 4 illustra questo comportamento. Per determinare il rendering `id` del `Age` casella di testo, iniziare con il `ID` valore del controllo casella di testo, `Age`. Successivamente, il funzionamento la gerarchia dei controlli. In ogni contenitore di denominazione (i nodi con un colore rosa chiaro) prefisso corrente sottoposto a rendering `id` con il contenitore di denominazione `id`.


![Gli attributi di id di rendering sono valori basati sull'ID dei contenitori di denominazione](control-id-naming-in-content-pages-vb/_static/image6.png)

**Figura 04**: il rendering `id` gli attributi sono basate sul `ID` i valori dei contenitori di denominazione


> [!NOTE]
> Come accennato in precedenza, il `ctl00` parte dell'oggetto sottoposto a rendering `id` attributo costituisce il `ID` valore della pagina master, ma si potrebbe chiedere come questo `ID` coniato valore. Non è stato specificato, in un punto qualsiasi nella nostra pagina master o contenuta. La maggior parte dei controlli del server in una pagina ASP.NET vengono aggiunti in modo esplicito tramite il markup della pagina dichiarativo. Il `MainContent` controllo ContentPlaceHolder è stato specificato in modo esplicito nel markup di `Site.master`; `Age` casella di testo è stato definito `IDIssues.aspx`del markup. È possibile specificare il `ID` valori per questi tipi di controlli tramite la finestra proprietà o la sintassi dichiarativa. Altri controlli, come la pagina master, non sono definiti nel markup dichiarativo. Di conseguenza, i relativi `ID` i valori devono essere generati automaticamente per noi. I set di motore ASP.NET il `ID` valori in fase di esecuzione di tali controlli i cui ID non sono state impostate in modo esplicito. Usa il modello di denominazione `ctlXX`, dove *XX* è un valore intero in sequenza crescente.


Poiché il master per la pagina stessa serve come un contenitore di denominazione, i controlli Web definiti nella pagina master anche essere modificato visualizzabile `id` i valori di attributo. Ad esempio, il `DisplayDate` etichetta è aggiunto alla pagina master il [ *creazione di un Layout a livello di sito con pagine Master* ](creating-a-site-wide-layout-using-master-pages-vb.md) esercitazione ha eseguito il rendering di markup seguente:


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample6.html)]

Si noti che il `id` attributo include entrambi della pagina master `ID` valore (`ctl00`) e `ID` valore del controllo etichetta Web (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Passaggio 3: A livello di codice che fa riferimento tramite i controlli Web`FindControl`

Ogni controllo server ASP.NET include un `FindControl("controlID")` metodo che cerca i discendenti del controllo per un controllo denominato *controlID*. Se ad esempio un controllo viene trovato, viene restituito; Se non viene trovato alcun controllo corrispondente, `FindControl` restituisce `Nothing`.

`FindControl`è utile negli scenari in cui è necessario un controllo di accesso ma non è un riferimento diretto a esso. Quando si lavora con dati Web controlli quali GridView, ad esempio, una volta definiti i controlli all'interno dei campi del controllo GridView nella sintassi dichiarativa, ma in fase di esecuzione viene creata un'istanza del controllo per ogni riga GridView. Di conseguenza, i controlli generati in fase di esecuzione esistono, ma non è un riferimento diretto disponibile dalla classe code-behind. Di conseguenza è necessario utilizzare `FindControl` a livello di codice con un controllo specifico all'interno dei campi del controllo GridView. (Per ulteriori informazioni sull'utilizzo `FindControl` per accedere ai controlli all'interno di modelli del controllo Web di dati, vedere [formattazione basato su dati personalizzati](../../data-access/custom-formatting/custom-formatting-based-upon-data-vb.md).) Questo stesso scenario si verifica quando l'aggiunta di controlli Web in modo dinamico a un Web Form, un argomento trattato in [la creazione di interfacce utente di immissione dati dinamico](https://msdn.microsoft.com/library/aa479330.aspx).

Per illustrare l'utilizzo di `FindControl` metodo per la ricerca per i controlli all'interno di una pagina di contenuto, creare un gestore eventi per il `SubmitButton`del `Click` evento. Nel gestore eventi, aggiungere il codice seguente, che a livello di codice fa riferimento il `Age` casella di testo e `Results` etichetta utilizzando il `FindControl` (metodo) e quindi viene visualizzato un messaggio in `Results` in base all'input dell'utente.

> [!NOTE]
> Ovviamente, non è necessario utilizzare `FindControl` per fare riferimento ai controlli Label e TextBox per questo esempio. È possibile fare riferimento direttamente tramite i relativi `ID` i valori delle proprietà. Usare `FindControl` fare clic qui per illustrare cosa accade quando si utilizza `FindControl` da una pagina contenuto.


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample7.vb)]

Mentre la sintassi utilizzata per chiamare il `FindControl` metodo differisce leggermente nelle prime due righe di `SubmitButton_Click`, sono semanticamente equivalenti. Tenere presente che tutti i controlli server ASP.NET includono un `FindControl` metodo. Ciò include la `Page` (classe), da ASP.NET tutte le classi code-behind devono derivare da. Pertanto, la chiamata `FindControl("controlID")` è equivalente alla chiamata `Page.FindControl("controlID")`, supponendo che non è ancora stato eseguito l'override di `FindControl` metodo nella classe del codice o in una classe di base personalizzata.

Dopo aver immesso il codice, visitare il `IDIssues.aspx` pagina tramite un browser, immettere la tua età e fare clic sul pulsante "Invia". Fare clic sul pulsante "Invia" su un `NullReferenceException` viene generato (vedere Figura 5).


[![Viene generata un'eccezione NullReferenceException](control-id-naming-in-content-pages-vb/_static/image8.png)](control-id-naming-in-content-pages-vb/_static/image7.png)

**Figura 05**: A `NullReferenceException` viene generato ([fare clic per visualizzare l'immagine ingrandita](control-id-naming-in-content-pages-vb/_static/image9.png))


Se si imposta un punto di interruzione `SubmitButton_Click` si noterà che entrambe le chiamate al gestore dell'evento `FindControl` restituire `Nothing`. Il `NullReferenceException` viene generato quando si tenta di accedere il `Age` della casella di testo `Text` proprietà.

Il problema è che `Control.FindControl` Cerca solo *controllo*del discendenti presenti nello stesso contenitore di denominazione. Poiché la pagina master costituisce un nuovo contenitore di denominazione, una chiamata a `Page.FindControl("controlID")` mai interessano l'oggetto della pagina master `ctl00`. (Vedere Figura 4 per visualizzare la gerarchia di controllo, che mostra il `Page` oggetto come elemento padre dell'oggetto pagina master `ctl00`.) Pertanto, il `Results` etichetta e `Age` casella di testo non vengono trovati e `ResultsLabel` e `AgeTextBox` vengono assegnati i valori di `Nothing`.

Esistono due possibili soluzioni a questo problema: è possibile eseguire il drill down, un contenitore di denominazione contemporaneamente, il controllo appropriato. oppure è possibile creare proprie `FindControl` metodo che interessano i contenitori di denominazione. Esaminare ognuna di queste opzioni.

### <a name="drilling-into-the-appropriate-naming-container"></a>Drill-down del contenitore di denominazione appropriata

Per utilizzare `FindControl` al riferimento di `Results` etichetta o `Age` casella di testo, è necessario chiamare `FindControl` da un controllo predecessore nello stesso contenitore di denominazione. Come illustrato nella figura 4, il `MainContent` controllo ContentPlaceHolder rappresenta il predecessore solo `Results` o `Age` che si trova all'interno dello stesso contenitore di denominazione. In altre parole, la chiamata di `FindControl` metodo il `MainContent` (controllo), come illustrato nel frammento di codice riportato di seguito, restituisce correttamente un riferimento al `Results` o `Age` controlli.


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample8.vb)]

Tuttavia, è possibile utilizzare il `MainContent` ContentPlaceHolder dalla classe code-behind della pagina il contenuto utilizzando la sintassi precedente perché è definito il ContentPlaceHolder nella pagina master. In alternativa, è necessario utilizzare `FindControl` per ottenere un riferimento a `MainContent`. Sostituire il codice di `SubmitButton_Click` gestore eventi con le seguenti modifiche:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample9.vb)]

Se si visita la pagina tramite un browser, immettere la tua età e fare clic sul pulsante "Invia" un `NullReferenceException` viene generato. Se si imposta un punto di interruzione `SubmitButton_Click` gestore eventi, si noterà che questa eccezione si verifica durante il tentativo di chiamare il `MainContent` dell'oggetto `FindControl` metodo. Il `MainContent` è uguale all'oggetto `Nothing` perché il `FindControl` metodo non è possibile individuare un oggetto denominato "MainContent". La causa principale è la stessa utilizzata con la `Results` etichetta e `Age` controlli TextBox: `FindControl` inizia la ricerca dalla parte superiore della gerarchia del controllo e non penetrare in contenitori di denominazione, ma la `MainContent` ContentPlaceHolder è all'interno della pagina master, ovvero un contenitore di denominazione.

Prima di utilizzare `FindControl` per ottenere un riferimento a `MainContent`, occorre innanzitutto un riferimento al controllo pagina master. Dopo aver ottenuto un riferimento alla pagina master, è possibile ottenere un riferimento al `MainContent` ContentPlaceHolder tramite `FindControl` e, da qui, i riferimenti al `Results` etichetta e `Age` casella di testo (nuovamente mediante `FindControl`). Ma, come si ottengono un riferimento alla pagina master? Controllando il `id` attributi nel markup di rendering è evidente che la pagina master `ID` valore `ctl00`. Pertanto, è possibile usare `Page.FindControl("ctl00")` per ottenere un riferimento alla pagina master, quindi utilizzare tale oggetto per ottenere un riferimento a `MainContent`e così via. Il frammento di codice seguente viene illustrata questa logica:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample10.vb)]

Mentre il codice funziona certamente, si presuppone che genera automaticamente pagina `ID` sarà sempre `ctl00`. Non è consigliabile basarsi su presupposti relativi valori generati automaticamente.

Fortunatamente, un riferimento alla pagina master è possibile accedere tramite il `Page` della classe `Master` proprietà. Pertanto, anziché dover usare `FindControl("ctl00")` per ottenere un riferimento della pagina master per poter accedere il `MainContent` ContentPlaceHolder, è possibile invece utilizzare `Page.Master.FindControl("MainContent")`. Aggiornamento di `SubmitButton_Click` gestore dell'evento con il codice seguente:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample11.vb)]

Questa volta, visitare la pagina tramite un browser, immettendo l'età e fare clic sul pulsante "Invia" Visualizza il messaggio nel `Results` assegnare un'etichetta, come previsto.


[![Età dell'utente viene visualizzato nell'etichetta](control-id-naming-in-content-pages-vb/_static/image11.png)](control-id-naming-in-content-pages-vb/_static/image10.png)

**Figura 06**: età dell'utente viene visualizzato nell'etichetta ([fare clic per visualizzare l'immagine ingrandita](control-id-naming-in-content-pages-vb/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>In modo ricorsivo la ricerca dei nomi di contenitori

Il motivo per il precedente esempio di codice a cui fa riferimento il `MainContent` controllo ContentPlaceHolder dalla pagina master e quindi la `Results` etichetta e `Age` dei controlli casella di testo `MainContent`, perché il `Control.FindControl` metodo cerca solo all'interno di *controllo*contenitore di denominazione. Con `FindControl` rimane all'interno del contenitore di denominazione è utile nella maggior parte degli scenari, perché due controlli in due contenitori di denominazione diversi possono avere lo stesso `ID` valori. Si consideri il caso di un controllo GridView che definisce un controllo etichetta Web denominato `ProductName` all'interno di uno dei relativi TemplateFields. Quando l'associazione dati a GridView in fase di esecuzione, un `ProductName` verrà creata l'etichetta per ogni riga GridView. Se `FindControl` effettuata denominazione tutti i contenitori e abbiamo chiamato `Page.FindControl("ProductName")`, quale istanza di etichetta deve il `FindControl` restituire? Il `ProductName` etichetta nella prima riga GridView? Quello nell'ultima riga?

Quando si prende in `Control.FindControl` ricerca appena *controllo*dei nomi del contenitore è significativo nella maggior parte dei casi. Ma esistono altri casi, ad esempio quello affiancate, in cui è presente un univoco `ID` in tutti i contenitori di denominazione e si desidera evitare di dover riproducono fanno riferimento a ogni contenitore di denominazione nella gerarchia di controllo in un controllo di accesso. Con un `FindControl` variant troppo ricerche in modo ricorsivo tutti i contenitori di denominazione consente di rilevare. Purtroppo, .NET Framework non include tale metodo.

Buone notizie sono che possiamo creare nostro `FindControl` metodo tale in modo ricorsivo cerca tutti i contenitori di denominazione. In realtà, utilizzando *metodi di estensione* è possibile aggiungere un `FindControlRecursive` metodo il `Control` classe complemento esistenti `FindControl` metodo.

> [!NOTE]
> Metodi di estensione sono una funzionalità nuove per c# 3.0 e Visual Basic 9, sono le lingue disponibili in .NET Framework versione 3.5 e Visual Studio 2008. In breve, metodi di estensione consentono agli sviluppatori di creare un nuovo metodo per un tipo di classe esistente tramite una sintassi speciale. Per ulteriori informazioni su questa funzionalità utile, vedere l'articolo, [estensione delle funzionalità di tipo di Base con i metodi di estensione](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Per creare il metodo di estensione, aggiungere un nuovo file di `App_Code` cartella denominata `PageExtensionMethods.vb`. Aggiungere un metodo di estensione denominato `FindControlRecursive` che accetta come input un `String` parametro denominato `controlID`. Per i metodi di estensione per il corretto funzionamento, è essenziale che la classe deve essere contrassegnato come un `Module` e che i metodi di estensione il prefisso di `<Extension()>` attributo. Inoltre, tutti i metodi di estensione devono accettare come primo parametro un oggetto del tipo a cui si applica il metodo di estensione.

Aggiungere il codice seguente per il `PageExtensionMethods.vb` file per definire questo `Module` e `FindControlRecursive` metodo di estensione:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample12.vb)]

Con questo codice, ripristinare il `IDIssues.aspx` classe code-behind della pagina e impostare come commento corrente `FindControl` chiamate al metodo. Sostituirli con chiamate a `Page.FindControlRecursive("controlID")`. È interessante sui metodi di estensione che sono visualizzate direttamente all'interno di elenchi a discesa IntelliSense. Come illustrato nella figura 7, quando si digita `Page` e quindi fare clic su periodo, il `FindControlRecursive` metodo è incluso nell'elenco a discesa con gli altri IntelliSense `Control` metodi della classe.


[![Nei IntelliSense elenchi a discesa sono inclusi i metodi di estensione](control-id-naming-in-content-pages-vb/_static/image14.png)](control-id-naming-in-content-pages-vb/_static/image13.png)

**Figura 07**: metodi di estensione sono inclusi in elenchi di discesa IntelliSense ([fare clic per visualizzare l'immagine ingrandita](control-id-naming-in-content-pages-vb/_static/image15.png))


Immettere il codice seguente nel `SubmitButton_Click` gestore dell'evento e quindi provare a visitare la pagina, immettere la tua età e fare clic sul pulsante "Invia". Come illustrato nella figura 6, l'output risultante verrà visualizzato il messaggio, "anni age!"


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample13.vb)]

> [!NOTE]
> Poiché i metodi di estensione rappresentano una novità di c# 3.0 e Visual Basic 9, se si utilizza Visual Studio 2005 non è possibile utilizzare i metodi di estensione. In alternativa, è necessario implementare la `FindControlRecursive` metodo in una classe helper. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) è un esempio in post di blog il suo [Maser le pagine ASP.NET e `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Passaggio 4: Il corretto utilizzo`id`valore nello Script sul lato Client dell'attributo

Come accennato nell'introduzione di questa esercitazione, un controllo Web del sottoposto a rendering `id` attributo viene spesso utilizzato nello script sul lato client per fare riferimento a livello di codice a un particolare elemento HTML. Ad esempio, il codice JavaScript seguente fa riferimento a un elemento HTML dal relativo `id` e quindi Visualizza il relativo valore in una finestra di messaggio modale:


[!code-csharp[Main](control-id-naming-in-content-pages-vb/samples/sample14.cs)]

Tenere presente che, in ASP.NET, pagine che non includono un contenitore di denominazione, elemento HTML di cui è stato eseguito il rendering `id` attributo è identico al controllo di Web `ID` valore della proprietà. Per questo motivo, può essere tentati di livello di codice in `id` i valori di attributo nel codice JavaScript. Ovvero, se si desidera accedere il `Age` controllo TextBox Web mediante script sul lato client, eseguire questa operazione tramite una chiamata a `document.getElementById("Age")`.

Il problema con questo approccio è che quando si utilizzano pagine master (o altri controlli contenitore di denominazione), il codice HTML `id` non è un sinonimo del controllo Web `ID` proprietà. Potrebbe essere la prima scelta per visitare la pagina tramite un browser e visualizzare l'origine per determinare l'effettiva `id` attributo. Quando si conosce il rendering `id` valore, è possibile incollarlo nella chiamata a `getElementById` per accedere all'elemento HTML è necessario utilizzare lo script sul lato client. Questo approccio è minore di ideale perché alcune modifiche apportate alla pagina gerarchia dei controllano o modifiche al `ID` implica la modifica delle proprietà dei controlli di denominazione risultante `id` attributo, violando il codice JavaScript.

Buone notizie consiste nel fatto che il `id` valore dell'attributo che viene eseguito il rendering è accessibile nel codice sul lato server tramite il controllo Web [ `ClientID` proprietà](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). È consigliabile utilizzare questa proprietà per determinare il `id` valore utilizzato nello script sul lato client dell'attributo. Ad esempio, per aggiungere una funzione JavaScript alla pagina che, quando viene chiamato, viene visualizzato il valore della `Age` casella di testo in una finestra di messaggio modale, aggiungere il codice seguente per il `Page_Load` gestore eventi:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample15.vb)]

Il codice sopra riportato inserisce il valore di `Age` della casella di testo `ClientID` proprietà nella chiamata a JavaScript `getElementById`. Se si accede a questa pagina tramite un browser e visualizzare l'origine HTML, sarà possibile trovare il codice JavaScript seguente:


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample16.html)]

Si noti come il corretto `id` valore dell'attributo, `ctl00_MainContent_Age`, viene visualizzato all'interno della chiamata a `getElementById`. Dal momento che questo valore viene calcolato in fase di esecuzione, funziona indipendentemente dalle modifiche successive alla gerarchia di controllo della pagina.

> [!NOTE]
> In questo esempio di JavaScript viene semplicemente come aggiungere una funzione JavaScript che contiene riferimenti corretti all'elemento HTML visualizzato da un controllo server. Per utilizzare questa funzione è necessario creare JavaScript aggiuntive per chiamare la funzione quando il documento viene caricato o quando un'azione utente specifico di seguito. Per ulteriori informazioni su questi argomenti correlati, leggere e [lavorando con Script lato Client](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Riepilogo

Alcuni controlli server ASP.NET fungono da contenitori di denominazione, che influisce eseguito il rendering `id` attributo valori dei relativi controlli figlio e l'ambito dei controlli canvassed dal `FindControl` metodo. Per quanto riguarda le pagine master, sia la pagina master e i relativi controlli ContentPlaceHolder sono contenitori di denominazione. Di conseguenza, è necessario inserire via maggiore lavoro per fare riferimento a livello di programmazione di controlli di pagina di contenuto usando `FindControl`. In questa esercitazione sono state presentate due tecniche: chiamante e drill-del controllo ContentPlaceHolder relativo `FindControl` ; (metodo) e in sequenza nostro `FindControl` implementazione cerca tale in modo ricorsivo attraverso tutti i contenitori di denominazione.

Oltre ai problemi sul lato server introducono contenitori di denominazione per quanto riguarda i controlli Web di riferimento, ci sono anche problemi sul lato client. In assenza di contenitori di denominazione, il controllo Web del `ID` valore della proprietà e il rendering `id` valore dell'attributo sono equivalenti. Ma con l'aggiunta del contenitore di denominazione, il rendering `id` attributo include sia il `ID` i valori del controllo Web e il failover dei contenitori di denominazione in cronologia del relativa gerarchia controllo. Questi problemi di denominazione sono il problema fino a quando si utilizza il controllo di Web `ClientID` proprietà per determinare il rendering `id` valore nello script sul lato client dell'attributo.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Pagine Master ASP.NET e`FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Creazione di interfacce utente di immissione dati dinamici](https://msdn.microsoft.com/library/aa479330.aspx)
- [Estensione delle funzionalità di tipo di Base con i metodi di estensione](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Procedura: Fare riferimento a contenuto della pagina Master ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Pagine master: Suggerimenti, consigli e trap](http://www.odetocode.com/articles/450.aspx)
- [Utilizzo di Script sul lato Client](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 3.5 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott può essere raggiunto al [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Zack Jones e Suchi Barnerjee. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Precedente](urls-in-master-pages-vb.md)
[Successivo](interacting-with-the-master-page-from-the-content-page-vb.md)
