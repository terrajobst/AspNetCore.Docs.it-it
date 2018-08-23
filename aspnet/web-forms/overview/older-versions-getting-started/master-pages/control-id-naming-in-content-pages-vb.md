---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
title: Controllare la denominazione degli ID nelle pagine di contenuto (VB) | Microsoft Docs
author: rick-anderson
description: Viene illustrato come controlli ContentPlaceHolder fungono da un contenitore di denominazione e pertanto rendono il lavoro a livello di codice con un controllo difficile (tramite FindConrol)...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: dbb024a6-f043-4fc5-ad66-56556711875b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 6e9a751538ca28250e4e776ff2c6c3f0185ffbe6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835841"
---
<a name="control-id-naming-in-content-pages-vb"></a>ID di controllo di denominazione nelle pagine di contenuto (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_VB.pdf)

> Viene illustrato come controlli ContentPlaceHolder fungono da un contenitore di denominazione e pertanto rendono il lavoro a livello di codice con un controllo difficile (tramite FindConrol). Esamina il problema e le soluzioni alternative. Spiega inoltre come accedere a livello di codice il valore ClientID risultante.


## <a name="introduction"></a>Introduzione

Tutti i controlli server ASP.NET includono un `ID` proprietà che identifica il controllo in modo univoco e costituisce il mezzo con cui accede al controllo a livello di codice nella classe code-behind. Analogamente, possono includere gli elementi in un documento HTML un' `id` attributo che identifica in modo univoco l'elemento; questi `id` valori vengono spesso usati in script lato client per fare riferimento a livello di codice a un particolare elemento HTML. Detto questo, si può presupporre che quando un controllo server ASP.NET viene eseguito il rendering in HTML, relativi `ID` viene utilizzato come valore di `id` valore dell'elemento HTML di cui è stato eseguito rendering. Questo non è necessariamente il caso poiché in alcune circostanze un singolo controllo con un singolo `ID` valore potrebbe essere visualizzato più volte nel markup sottoposto a rendering. Si consideri un controllo GridView che include un TemplateField con un controllo Web Label con un' `ID` pari a `ProductName`. Quando il controllo GridView è associato all'origine dati in fase di esecuzione, questa etichetta viene ripetuta una volta per ogni riga GridView. Ogni rendering esigenze etichetta univoca `id` valore.

Per gestire tali scenari, ASP.NET consente a determinati controlli contrassegnare come contenitori di denominazione. Un contenitore di denominazione viene utilizzato come un nuovo `ID` dello spazio dei nomi. Tutti i controlli server presenti all'interno del contenitore di denominazione sono loro visualizzabile `id` valore preceduto il `ID` del controllo contenitore di denominazione. Ad esempio, il `GridView` e `GridViewRow` classi sono entrambe denominazione dei contenitori. Di conseguenza, un controllo etichetta definito in un GridView TemplateField con `ID` `ProductName` viene assegnato un rendering `id` valore `GridViewID_GridViewRowID_ProductName`. In quanto *GridViewRowID* univoca per ogni riga GridView, risultante `id` valori siano univoci.

> [!NOTE]
> Il [ `INamingContainer` interfaccia](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) viene utilizzato per indicare che un particolare controllo server ASP.NET dovrebbe funzionare come un contenitore di denominazione. Il `INamingContainer` interfaccia non compitare qualsiasi metodo che deve implementare il controllo del server; piuttosto, viene utilizzato come indicatore. Generare il markup sottoposto a rendering, se un controllo implementa questa interfaccia quindi il motore ASP.NET aggiunge automaticamente il prefisso relativo `ID` valore per i relativi discendenti sottoposto a rendering `id` i valori di attributo. Questo processo è descritto più dettagliatamente nel passaggio 2.


Denominazione dei contenitori non solo modificare il rendering `id` valore dell'attributo, ma influisce anche sul modo in cui il controllo potrebbe a livello di programmazione farvi riferimento dalla classe code-behind della pagina ASP.NET. Il `FindControl("controlID")` metodo viene comunemente usato per fare riferimento a livello di codice un controllo Web. Tuttavia, `FindControl` non penetrare tramite i contenitori di denominazione. Di conseguenza, non è possibile usare direttamente il `Page.FindControl` metodo fare riferimento ai controlli all'interno di un controllo GridView o altro contenitore di denominazione.

Come può avere conclusione, le pagine master ed elementi ContentPlaceHolder vengono entrambi implementati come contenitori di denominazione. In questa esercitazione esamineremo come master l'elemento HTML effetto pages `id` i valori e i modi per fare riferimento a livello di programmazione di controlli Web all'interno di una pagina di contenuto con `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Passaggio 1: Aggiunta di una nuova pagina ASP.NET

Per illustrare i concetti illustrati in questa esercitazione, aggiungiamo una nuova pagina ASP.NET al nostro sito Web. Creare una nuova pagina di contenuto denominata `IDIssues.aspx` nella cartella radice di associazione per il `Site.master` pagina master.


![Aggiungere il contenuto di pagina IDIssues.aspx nella cartella radice](control-id-naming-in-content-pages-vb/_static/image1.png)

**Figura 01**: Aggiungi pagina contenuto `IDIssues.aspx` nella cartella radice


Visual Studio crea automaticamente un controllo contenuto per ognuna delle elementi quattro ContentPlaceHolder della pagina master. Come indicato nella [ *più elementi ContentPlaceHolder e contenuto predefinito* ](multiple-contentplaceholders-and-default-content-vb.md) dell'esercitazione, se un controllo contenuto non è presente il contenuto della pagina master predefinito ContentPlaceHolder viene generato invece. Poiché il `QuickLoginUI` e `LeftColumnContent` ContentPlaceHolder contengono il markup predefinite appropriate per questa pagina, proseguire e rimuovere i relativi controlli contenuto da `IDIssues.aspx`. A questo punto, il contenuto markup della pagina dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample1.aspx)]

Nel [ *specificazione di titolo, metatag e altre intestazioni HTML nella pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) esercitazione viene creata una classe di base di pagine personalizzate (`BasePage`) che configura automaticamente il titolo della pagina se si tratta di impostare in modo non esplicito. Per il `IDIssues.aspx` pagina per poter usare questa funzionalità, classe code-behind della pagina deve derivare dal `BasePage` classe (anziché `System.Web.UI.Page`). Modificare la definizione della classe code-behind in modo che risulti simile al seguente:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample2.vb)]

Aggiornare infine il `Web.sitemap` file da includere una voce per questa lezione di nuovo. Aggiungere un `<siteMapNode>` e impostare relativi `title` e `url` gli attributi su "Problemi di denominazione ID di controllo" e `~/IDIssues.aspx`, rispettivamente. Dopo aver apportato questa aggiunta di `Web.sitemap` markup del file dovrebbe essere simile al seguente:


[!code-xml[Main](control-id-naming-in-content-pages-vb/samples/sample3.xml)]

Come illustrato dalla figura 2, la nuova voce della mappa del sito in `Web.sitemap` verrà immediatamente aggiornata nella sezione lezioni nella colonna sinistra.


![La sezione lezioni ora include un collegamento a &quot;controllare i problemi di denominazione ID&quot;](control-id-naming-in-content-pages-vb/_static/image2.png)

**Figura 02**: la sezione lezioni ora include un collegamento a "Problemi di denominazione degli ID di controllo"


## <a name="step-2-examining-the-renderedidchanges"></a>Passaggio 2: Esaminare il rendering`ID`modifiche

Per comprendere meglio le modifiche di ASP.NET rende motore per il rendering `id` controlla i valori del server, aggiungiamo alcuni controlli Web per il `IDIssues.aspx` pagina e quindi visualizzare il markup sottoposto a rendering inviato al browser. In particolare, digitare il testo ". immettere la tua età:" seguita da un controllo TextBox Web. Ulteriormente verso il basso nella pagina aggiungere un controllo pulsante Web e un controllo etichetta Web. Impostare la casella di testo `ID` e `Columns` delle proprietà per `Age` e 3, rispettivamente. Impostare il pulsante `Text` e `ID` delle proprietà su "Submit" e `SubmitButton`. Cancellare il contenuto dell'etichetta `Text` proprietà e set relativo `ID` a `Results`.

Il contenuto del markup del controllo dichiarativa a questo punto dovrebbe essere simile al seguente:


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample4.aspx)]

Figura 3 mostra la pagina visualizzata mediante Progettazione di Visual Studio.


[![La pagina include tre controlli Web: una casella di testo, pulsante ed etichetta](control-id-naming-in-content-pages-vb/_static/image4.png)](control-id-naming-in-content-pages-vb/_static/image3.png)

**Figura 03**: la pagina include tre controlli Web: una casella di testo, pulsante ed etichetta ([fare clic per visualizzare l'immagine con dimensioni normali](control-id-naming-in-content-pages-vb/_static/image5.png))


Visita la pagina tramite un browser e visualizzare l'origine HTML. Come il markup seguente mostra, il `id` i valori degli elementi HTML per i controlli TextBox, Button ed etichetta Web sono una combinazione del `ID` i valori dei controlli Web e il `ID` i valori dei contenitori di denominazione nella pagina.


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample5.html)]

Come indicato in precedenza in questa esercitazione, la pagina master e relativi elementi ContentPlaceHolder fungono da contenitori di denominazione. Di conseguenza, entrambi contribuiscono sottoposto a rendering `ID` i valori dei relativi controlli annidati. Richiedere la casella di testo `id` dell'attributo, ad esempio: `ctl00_MainContent_Age`. Si tenga presente che il controllo TextBox `ID` valore era `Age`. Questo è il prefisso del relativo controllo ContentPlaceHolder `ID` valore, `MainContent`. Inoltre, questo valore ha il prefisso della pagina master `ID` valore, `ctl00`. Il risultato finale è un `id` valore dell'attributo costituito il `ID` i valori della pagina master, il controllo ContentPlaceHolder e la casella di testo.

Figura 4 illustra questo comportamento. Per determinare il rendering `id` del `Age` casella di testo iniziano con la `ID` valore del controllo casella di testo, `Age`. Successivamente, proseguite sulla gerarchia dei controlli. In ogni contenitore di denominazione (i nodi con un colore rosa chiaro) corrente viene eseguito il rendering del prefisso `id` con il contenitore di denominazione `id`.


![Gli attributi di id di rendering sono valori basati su in the ID dei contenitori di denominazione](control-id-naming-in-content-pages-vb/_static/image6.png)

**Figura 04**: il rendering `id` gli attributi sono basate sul `ID` i valori dei contenitori di denominazione


> [!NOTE]
> Come accennato, il `ctl00` parte dell'oggetto sottoposto a rendering `id` attributo costituisce il `ID` valore della pagina master, ma è giusto chiedersi come questo `ID` coniato valore. È non è specificato, in un punto qualsiasi nella nostra pagina master e il contenuto. La maggior parte dei controlli del server in una pagina ASP.NET vengono aggiunte in modo esplicito tramite il markup della pagina dichiarativo. Il `MainContent` controllo ContentPlaceHolder è stato specificato in modo esplicito nel markup di `Site.master`; gli `Age` nella casella di testo è stato definito `IDIssues.aspx`del markup. È possibile specificare il `ID` valori per questi tipi di controlli nella finestra delle proprietà o dalla sintassi dichiarativa. Altri controlli, ad esempio la pagina master stessa, non sono definite nel markup dichiarativo. Di conseguenza, i `ID` valori devono essere generati automaticamente per noi. I set di motore ASP.NET il `ID` valori in fase di esecuzione per questi controlli il cui ID non sono state impostate in modo esplicito. Usa il modello di denominazione `ctlXX`, dove *XX* è un valore intero aumentino in sequenza.


Poiché pagina master stessa viene utilizzato come un contenitore di denominazione, i controlli Web definiti nella pagina master anche avranno modificato sottoposto a rendering `id` i valori di attributo. Ad esempio, il `DisplayDate` etichetta viene aggiunta alla pagina master nel [ *creazione di un Layout a livello di sito con pagine Master* ](creating-a-site-wide-layout-using-master-pages-vb.md) esercitazione prevede i seguenti viene eseguito il rendering di markup:


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample6.html)]

Si noti che il `id` attributo include entrambi della pagina master `ID` valore (`ctl00`) e il `ID` valore del controllo Web Label (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Passaggio 3: A livello di codice che fanno riferimento a controlli Web tramite`FindControl`

Ogni controllo server ASP.NET include un `FindControl("controlID")` metodo di ricerca di discendenti del controllo per un controllo denominato *controlID*. Se viene trovato un controllo di questo tipo, viene restituito; Se non viene trovato alcun controllo corrisponda, `FindControl` restituisce `Nothing`.

`FindControl` è utile negli scenari in cui è necessario un controllo di accesso ma non è un riferimento diretto a esso. Quando si usano i controlli Web, ad esempio GridView, ad esempio, una volta definiti i controlli all'interno dei campi del controllo GridView nella sintassi dichiarativa, ma in fase di esecuzione viene creata un'istanza del controllo per ogni riga GridView. Di conseguenza, i controlli generati in fase di esecuzione esistano, ma non è disponibile un riferimento diretto disponibile nella classe code-behind. Di conseguenza è necessario usare `FindControl` a livello di codice con un controllo specifico all'interno dei campi del controllo GridView. (Per altre informazioni sull'uso `FindControl` per accedere ai controlli all'interno dei modelli del controllo Web di dati, vedere [formattazione basata su dati personalizzati](../../data-access/custom-formatting/custom-formatting-based-upon-data-vb.md).) Questo stesso scenario si verifica quando si aggiunge dinamicamente i controlli Web a un Web Form, un argomento discusso in [creazione di interfacce utente voce dati dinamiche](https://msdn.microsoft.com/library/aa479330.aspx).

Per illustrare l'utilizzo di `FindControl` metodo di ricerca dei controlli all'interno di una pagina di contenuto, creare un gestore eventi per il `SubmitButton`del `Click` evento. Nel gestore dell'evento, aggiungere il codice seguente, che a livello di codice fa riferimento il `Age` nella casella di testo e `Results` assegnare un'etichetta usando la `FindControl` (metodo) e quindi visualizza un messaggio in `Results` in base all'input dell'utente.

> [!NOTE]
> Naturalmente, non è necessario usare `FindControl` per fare riferimento ai controlli Label e TextBox per questo esempio. È possibile farvi riferimento direttamente tramite il loro `ID` i valori delle proprietà. Utilizzerò `FindControl` qui per illustrare cosa accade quando si usa `FindControl` da una pagina contenuto.


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample7.vb)]

Mentre la sintassi utilizzata per chiamare il `FindControl` metodo differisce leggermente nelle prime due righe di `SubmitButton_Click`, sono semanticamente equivalenti. Tenere presente che tutti i controlli server ASP.NET includono un `FindControl` (metodo). Ciò include la `Page` (classe), da quali ASP.NET tutte le classi code-behind devono derivare da. Pertanto, la chiamata `FindControl("controlID")` è equivalente alla chiamata `Page.FindControl("controlID")`, presupponendo che è stato ancora eseguito l'override di `FindControl` metodo nella classe code-behind o in una classe di base personalizzata.

Dopo aver immesso il codice, visitare il `IDIssues.aspx` pagina tramite un browser, immettere la tua età e fare clic sul pulsante "Invia". Fare clic sul pulsante "Invia" su un `NullReferenceException` generata (vedere la figura 5).


[![Viene generata un'eccezione NullReferenceException](control-id-naming-in-content-pages-vb/_static/image8.png)](control-id-naming-in-content-pages-vb/_static/image7.png)

**Figura 05**: un' `NullReferenceException` generata ([fare clic per visualizzare l'immagine con dimensioni normali](control-id-naming-in-content-pages-vb/_static/image9.png))


Se si imposta un punto di interruzione nel `SubmitButton_Click` si noterà che entrambe le chiamate al gestore dell'evento `FindControl` restituire `Nothing`. Il `NullReferenceException` viene generato quando si tenta di accedere al `Age` della casella di testo `Text` proprietà.

Il problema è che `Control.FindControl` Cerca soltanto *controllo*del discendenti che sono nello stesso contenitore di denominazione. Poiché la pagina master costituisca un nuovo contenitore di denominazione, una chiamata a `Page.FindControl("controlID")` mai interessano l'oggetto pagina master `ctl00`. (Vedere la figura 4 per visualizzare la gerarchia dei controlli, che mostra le `Page` oggetto dell'elemento padre dell'oggetto pagina master `ctl00`.) Pertanto, il `Results` Label e `Age` nella casella di testo non sono stati trovati e `ResultsLabel` e `AgeTextBox` vengono assegnati i valori di `Nothing`.

Esistono due soluzioni alternative per risolvere questo problema: cui possiamo esaminare, contenitore di denominazione uno alla volta, il controllo appropriato. oppure possiamo creare nostro `FindControl` metodo che interessano i contenitori di denominazione. Esaminiamo ognuna di queste opzioni.

### <a name="drilling-into-the-appropriate-naming-container"></a>Drill-down di contenitore di denominazione appropriata

Per utilizzare `FindControl` per fare riferimento al `Results` etichetta o `Age` casella di testo, dobbiamo chiamare `FindControl` da un controllo predecessore nello stesso contenitore di denominazione. Come illustrato nella figura 4, il `MainContent` controllo ContentPlaceHolder è il predecessore di solo `Results` o `Age` che è all'interno del contenitore di denominazione stesso. In altre parole, la chiamata di `FindControl` metodo dal `MainContent` (controllo), come illustrato nel frammento di codice riportato di seguito, restituisce correttamente un riferimento al `Results` o `Age` controlli.


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample8.vb)]

Tuttavia, è possibile lavorare con i `MainContent` ContentPlaceHolder dalla classe code-behind della pagina con il contenuto usando la sintassi precedente perché è definito il controllo ContentPlaceHolder nella pagina master. In alternativa, è necessario usare `FindControl` per ottenere un riferimento a `MainContent`. Sostituire il codice nel `SubmitButton_Click` gestore dell'evento con le modifiche seguenti:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample9.vb)]

Se si visita la pagina tramite un browser, immettere la tua età e fare clic sul pulsante "Invia", un `NullReferenceException` viene generato. Se si imposta un punto di interruzione nel `SubmitButton_Click` gestore dell'evento si noterà che questa eccezione si verifica quando si prova a chiamare le `MainContent` dell'oggetto `FindControl` (metodo). Il `MainContent` è uguale all'oggetto `Nothing` perché la `FindControl` (metodo) non è possibile individuare un oggetto denominato "MainContent". La causa principale è quello utilizzato con il `Results` etichetta e `Age` controlli casella di testo: `FindControl` inizia la ricerca nella parte superiore della gerarchia dei controlli e non attacco penetrare in contenitori di denominazione, ma il `MainContent` ContentPlaceHolder è all'interno della pagina master, ovvero un contenitore di denominazione.

Per poter usare `FindControl` per ottenere un riferimento a `MainContent`, prima è necessario un riferimento al controllo pagina master. Dopo aver ottenuto un riferimento alla pagina master è possibile ottenere un riferimento al `MainContent` ContentPlaceHolder tramite `FindControl` e, da qui, fa riferimento il `Results` etichetta e `Age` casella di testo (anche in questo caso, l'utilizzo `FindControl`). Ma come si ottiene un riferimento alla pagina master? Controllando il `id` attributi nel rendering dei tag è evidente che la pagina master `ID` valore `ctl00`. Pertanto, è possibile usare `Page.FindControl("ctl00")` per ottenere un riferimento alla pagina master, quindi usare tale oggetto per ottenere un riferimento a `MainContent`e così via. Il frammento seguente illustra questa logica:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample10.vb)]

Mentre questo codice funzionerà senza dubbio, si presuppone che generati automaticamente della pagina master `ID` sarà sempre `ctl00`. Non è mai una buona idea verificare supposizioni su valori generati automaticamente.

Fortunatamente, un riferimento alla pagina master è accessibile tramite il `Page` della classe `Master` proprietà. Pertanto, anziché dover usare `FindControl("ctl00")` per ottenere un riferimento della pagina master per poter accedere il `MainContent` ContentPlaceHolder, è possibile invece usare `Page.Master.FindControl("MainContent")`. Aggiornamento di `SubmitButton_Click` gestore dell'evento con il codice seguente:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample11.vb)]

In questo momento, visitando la pagina tramite un browser, immettendo la tua età e fare clic sul pulsante "Invia" Visualizza il messaggio nel `Results` assegnare un'etichetta, come previsto.


[![L'età dell'utente viene visualizzato nell'etichetta](control-id-naming-in-content-pages-vb/_static/image11.png)](control-id-naming-in-content-pages-vb/_static/image10.png)

**Figura 06**: età dell'utente viene visualizzato nell'etichetta ([fare clic per visualizzare l'immagine con dimensioni normali](control-id-naming-in-content-pages-vb/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>In modo ricorsivo la ricerca tramite i contenitori di denominazione

Il motivo per l'esempio di codice precedente fa riferimento il `MainContent` controllo ContentPlaceHolder dalla pagina master e quindi il `Results` etichetta e `Age` dei controlli casella di testo `MainContent`, perché il `Control.FindControl` solo questo Cerca in all'interno *controllo*contenitore di denominazione. Visto `FindControl` rimane all'interno del contenitore di denominazione è utile nella maggior parte degli scenari perché i due controlli in due contenitori di denominazione diversi potrebbero avere lo stesso `ID` valori. Si consideri il caso di un controllo GridView che definisce un controllo etichetta Web denominato `ProductName` all'interno di uno dei relativi TemplateFields. Quando l'associazione dati a GridView in fase di esecuzione, un `ProductName` etichetta viene creato per ogni riga GridView. Se `FindControl` eseguire ricerche tramite denominazione tutti i contenitori e viene chiamato `Page.FindControl("ProductName")`, quale istanza di etichetta deve il `FindControl` restituire? Il `ProductName` etichetta nella prima riga GridView? Quello nell'ultima riga?

Pertanto la sua `Control.FindControl` ricerca appena *controllo*dei nomi del contenitore ha senso in maggior parte dei casi. Ma esistono altri casi, ad esempio quella per Stati Uniti, in cui è disponibile un valore univoco `ID` in tutti i contenitori di denominazione e si vuole evitare di dover meticolosamente fanno riferimento a ogni contenitore di denominazione nella gerarchia dei controlli a un controllo di accesso. Con un `FindControl` variante che cerca in modo ricorsivo tutti i contenitori di denominazione consente di rilevare, troppo. Sfortunatamente, .NET Framework non include un metodo di questo tipo.

La buona notizia è che possiamo creare il nostro `FindControl` metodo tale in modo ricorsivo cerca tutti i contenitori di denominazione. Infatti, utilizzando *metodi di estensione* è possibile aggiungere un `FindControlRecursive` metodo per il `Control` classe di accompagnamento relativo esistente per `FindControl` (metodo).

> [!NOTE]
> I metodi di estensione sono una funzionalità nuove per c# 3.0 e Visual Basic 9, quali sono le lingue forniti con .NET Framework versione 3.5 e Visual Studio 2008. In breve, i metodi di estensione consentono agli sviluppatori di creare un nuovo metodo per un tipo di classe esistente tramite una sintassi speciale. Per altre informazioni su questa funzionalità utile, vedere l'articolo [estendendo la funzionalità di tipo di Base con i metodi di estensione](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Per creare il metodo di estensione, aggiungere un nuovo file per il `App_Code` cartella denominata `PageExtensionMethods.vb`. Aggiungere un metodo di estensione denominato `FindControlRecursive` che accetta come input un `String` parametro denominato `controlID`. Per i metodi di estensione per il corretto funzionamento, è fondamentale che la classe deve essere contrassegnato come un `Module` e che i metodi di estensione presentino le `<Extension()>` attributo. Inoltre, tutti i metodi di estensione devono accettare come primo parametro un oggetto del tipo a cui viene applicato il metodo di estensione.

Aggiungere il codice seguente per il `PageExtensionMethods.vb` file per definire questo `Module` e il `FindControlRecursive` metodo di estensione:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample12.vb)]

Con questo codice, tornare al `IDIssues.aspx` classe code-behind della pagina e rimuovere i commenti corrente `FindControl` chiamate al metodo. Sostituirli con chiamate a `Page.FindControlRecursive("controlID")`. Che cos'è interessante sui metodi di estensione è che questi vengano visualizzati direttamente all'interno di elenchi a discesa di IntelliSense. Come illustrato nella figura 7, quando si digita `Page` e quindi premere periodo, il `FindControlRecursive` metodo è incluso in IntelliSense elenco a discesa insieme all'altro `Control` metodi della classe.


[![Sono inclusi i metodi di estensione in IntelliSense discesa](control-id-naming-in-content-pages-vb/_static/image14.png)](control-id-naming-in-content-pages-vb/_static/image13.png)

**Figura 07**: sono inclusi i metodi di estensione in IntelliSense discesa ([fare clic per visualizzare l'immagine con dimensioni normali](control-id-naming-in-content-pages-vb/_static/image15.png))


Immettere il codice seguente nel `SubmitButton_Click` gestore dell'evento e quindi testarla visitando la pagina, immettendo l'età e facendo clic sul pulsante "Invia". Come illustrato nella figura 6, l'output risultante sarà il messaggio, "Sei anni d'età".


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample13.vb)]

> [!NOTE]
> Poiché i metodi di estensione ha familiarità con c# 3.0 e Visual Basic 9, se si usa Visual Studio 2005 è possibile utilizzare i metodi di estensione. In alternativa, è necessario implementare il `FindControlRecursive` metodo in una classe helper. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) è un esempio di questo tipo nel suo post di blog [le pagine della ASP.NET e `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Passaggio 4: Uso sia quello corretto`id`valore nello Script lato Client dell'attributo

Come accennato nell'introduzione dell'esercitazione, un controllo Web del sottoposto a rendering `id` attributo viene spesso usato in uno script lato client per fare riferimento a livello di codice a un particolare elemento HTML. Ad esempio, il codice JavaScript seguente fa riferimento a un elemento HTML dal `id` e quindi Visualizza il relativo valore in una finestra di messaggio modale:


[!code-csharp[Main](control-id-naming-in-content-pages-vb/samples/sample14.cs)]

Tenere presente che in ASP.NET le pagine che non includono un contenitore di denominazione, elemento HTML di cui è stato eseguito il rendering `id` attributo è identico per il controllo Web `ID` valore della proprietà. Per questo motivo, si è tentati di livello di codice in `id` i valori degli attributi nel codice JavaScript. Vale a dire, se si conosce si accederà il `Age` controllo TextBox Web tramite uno script lato client, eseguire questa operazione tramite una chiamata a `document.getElementById("Age")`.

Il problema con questo approccio è che quando si usano le pagine master (o altri controlli contenitore di denominazione), il codice HTML visualizzabile `id` non è un sinonimo del controllo Web `ID` proprietà. La prima scelta è possibile visitare la pagina tramite un browser e visualizzare il codice sorgente per determinare l'effettiva `id` attributo. Quando si conosce il rendering `id` valore, è possibile incollarlo nella chiamata a `getElementById` per accedere all'elemento HTML è necessario utilizzare tramite uno script lato client. Questo approccio è ideale perché alcune modifiche alla pagina gerarchia dei controlli o modifiche per il `ID` comporta la modifica delle proprietà dei controlli denominazione risultante `id` attributo, violando il codice JavaScript.

La buona notizia è che il `id` valore dell'attributo che viene eseguito il rendering è accessibile nel codice lato server tramite il controllo Web [ `ClientID` proprietà](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). È consigliabile utilizzare questa proprietà per determinare il `id` valore utilizzato nello script lato client dell'attributo. Ad esempio, per aggiungere una funzione JavaScript alla pagina che, quando viene chiamato, viene visualizzato il valore della `Age` nella casella di testo in una finestra di messaggio modale, aggiungere il codice seguente per il `Page_Load` gestore dell'evento:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample15.vb)]

Il codice riportato sopra viene inserito il valore della `Age` della casella di testo `ClientID` proprietà nella chiamata JavaScript a `getElementById`. Se si accede a questa pagina tramite un browser e visualizzare l'origine HTML, sono disponibili il codice JavaScript seguente:


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample16.html)]

Si noti che la modalità corretta `id` valore dell'attributo, `ctl00_MainContent_Age`, viene visualizzata nella chiamata a `getElementById`. Poiché questo valore viene calcolato in fase di esecuzione, funziona indipendentemente dalle modifiche successive alla gerarchia dei controlli di pagina.

> [!NOTE]
> In questo esempio di JavaScript viene semplicemente come aggiungere una funzione JavaScript che fa riferimento in modo corretto all'elemento HTML visualizzato da un controllo server. Per usare questa funzione è necessario creare JavaScript aggiuntive per chiamare la funzione quando il documento viene caricato o quando accade realmente un'azione utente specifico. Per altre informazioni su questi argomenti correlati, leggere [utilizzo di Script sul lato Client](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Riepilogo

Alcuni controlli server ASP.NET fungono da contenitori di denominazione, influendo eseguito il rendering `id` attributo valori dei relativi controlli discendenti, nonché l'ambito dei controlli canvassed dal `FindControl` (metodo). Per quanto riguarda le pagine master, sia la pagina master stessa e i relativi controlli ContentPlaceHolder sono contenitori di denominazione. Di conseguenza, è necessario inserire via più consistenti per fare riferimento a livello di programmazione di controlli all'interno del contenuto pagina utilizzando `FindControl`. In questa esercitazione sono stati esaminati due tecniche: chiamante e il drill-del controllo ContentPlaceHolder relativi `FindControl` metodo; e l'implementazione personalizzata `FindControl` implementazione che in modo ricorsivo vengono ricercati tutti i contenitori di denominazione.

Oltre ai problemi di server-side introducono i contenitori di denominazione per quanto riguarda i controlli Web di riferimento, esistono anche i problemi lato client. In assenza di denominazione dei contenitori, il controllo Web `ID` valore della proprietà e viene eseguito il rendering `id` valore dell'attributo sono equivalenti. Ma con l'aggiunta di contenitore di denominazione, il rendering `id` attributo include sia il `ID` valori del controllo Web e i contenitori di denominazione nella cronologia della relativa gerarchia dei controlli. Questi problemi di denominazione sono il problema, purché si utilizzino del controllo Web `ClientID` proprietà per determinare il rendering `id` valore nello script lato client dell'attributo.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Pagine Master ASP.NET e `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Creazione di interfacce utente di immissione dati dinamici](https://msdn.microsoft.com/library/aa479330.aspx)
- [Estensione delle funzionalità di tipo di Base con i metodi di estensione](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Procedura: Fare riferimento a contenuto pagina Master ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Pagine master: Suggerimenti, trucchi e trabocchetti](http://www.odetocode.com/articles/450.aspx)
- [Utilizzo di Script sul lato Client](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla.com, ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach manualmente ASP.NET 3.5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Zack Jones e Suchi Barnerjee. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](urls-in-master-pages-vb.md)
> [Successivo](interacting-with-the-master-page-from-the-content-page-vb.md)
