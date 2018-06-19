---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Il modello ASP.NET 2.0 pagina | Documenti Microsoft
author: microsoft
description: In ASP.NET 1. x, gli sviluppatori dovevano una scelta tra un modello di codice inline e un modello di codice code-behind. Codice possa essere implementata utilizzando entrambi i attr Src...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: fda85ec03f845cafa7720382bf85652937932c44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891295"
---
<a name="the-aspnet-20-page-model"></a>Il modello ASP.NET 2.0 pagina
====================
by [Microsoft](https://github.com/microsoft)

> In ASP.NET 1. x, gli sviluppatori dovevano una scelta tra un modello di codice inline e un modello di codice code-behind. Codice possa essere implementata utilizzando l'attributo Src o l'attributo CodeBehind del @Page direttiva. In ASP.NET 2.0, gli sviluppatori hanno ancora una scelta tra codice inline e code-behind, ma sono stati apportati miglioramenti significativi al modello code-behind.


In ASP.NET 1. x, gli sviluppatori dovevano una scelta tra un modello di codice inline e un modello di codice code-behind. Codice possa essere implementata utilizzando l'attributo Src o l'attributo CodeBehind del @Page direttiva. In ASP.NET 2.0, gli sviluppatori hanno ancora una scelta tra codice inline e code-behind, ma sono stati apportati miglioramenti significativi al modello code-behind.

## <a name="improvements-in-the-code-behind-model"></a>Miglioramenti del modello Code-Behind

Per comprendere pienamente le modifiche nel modello code-behind ASP.NET 2.0, è consigliabile verificare rapidamente il modello originale era inclusa in ASP.NET 1. x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Il modello Code-Behind ASP.NET 1. x

In ASP.NET 1. x, il modello code-behind costituita da un file ASPX (Web Form) e un file code-behind che contiene il codice di programmazione. I due file sono stati connessi con il @Page direttiva nel file ASPX. Ogni controllo nella pagina ASPX aveva una dichiarazione corrispondente nel file code-behind come una variabile di istanza. Il file code-behind contenuto codice per l'associazione di eventi e generato codice necessario per la finestra di progettazione di Visual Studio. Questo modello collaborato abbastanza bene, ma poiché ogni elemento ASP.NET nella pagina ASPX necessari codice corrispondente nel file code-behind, si è verificato alcun true separazione del codice e contenuto. Ad esempio, se una finestra di progettazione aggiunta un nuovo controllo server a un file ASPX all'esterno di IDE di Visual Studio, l'applicazione causa l'interruzione a causa della mancanza di una dichiarazione per tale controllo nel file code-behind.

## <a name="the-code-behind-model-in-aspnet-20"></a>Il modello Code-Behind in ASP.NET 2.0

ASP.NET 2.0 migliora notevolmente da questo modello. In ASP.NET 2.0, codice viene implementato utilizzando il nuovo *classi parziali* forniti in ASP.NET 2.0. La classe code-behind ASP.NET 2.0 è definiti come una classe parziale, vale a dire che contiene solo una parte della definizione della classe. La parte rimanente della definizione della classe in modo dinamico viene generata da ASP.NET 2.0 mediante la pagina ASPX in fase di esecuzione oppure quando il sito Web precompilato. Il collegamento tra il file code-behind e la pagina ASPX viene comunque stabilito utilizzando la direttiva @ Page. Tuttavia, invece di un attributo CodeBehind o Src, ASP.NET 2.0 Usa ora l'attributo CodeFile. L'attributo Inherits viene inoltre utilizzato per specificare il nome della classe per la pagina.

Una direttiva @ Page tipica potrebbe essere simile al seguente:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Una definizione di classe tipica in un file code-behind ASP.NET 2.0 potrebbe essere simile al seguente:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> In c# e Visual Basic sono gli unici linguaggi gestiti che supportano attualmente le classi parziali. Di conseguenza, gli sviluppatori che usano j# non sarà in grado di utilizzare il modello code-behind ASP.NET 2.0.


Il nuovo modello migliora il modello code-behind perché gli sviluppatori avranno ora i file di codice che contengono solo il codice che hanno creato. Fornisce inoltre una separazione del codice e contenuto true perché non esistono dichiarazioni di variabili di istanza nel file code-behind.

> [!NOTE]
> Poiché la classe parziale per la pagina ASPX in cui ha luogo binding degli eventi, gli sviluppatori Visual Basic consentono di realizzare un lieve miglioramento delle prestazioni utilizzando la parola chiave handle nel code-behind per associare gli eventi. C# non dispone di alcun equivalente (parola chiave).


## <a name="new--page-directive-attributes"></a>Nuovi attributi direttiva @ Page

ASP.NET 2.0 aggiunge molti nuovi attributi per la direttiva @ Page. Gli attributi seguenti sono nuovi in ASP.NET 2.0.

## <a name="async"></a>Async

L'attributo Async consente di configurare una pagina può essere eseguita in modo asincrono. Anche frontespizi asincrona più avanti in questo modulo.

## <a name="asynctimeout"></a>AsyncTimeout

Specificare il timeout per le pagine asincrone. Il valore predefinito è 45 secondi.

## <a name="codefile"></a>CodeFile

L'attributo CodeFile è la sostituzione per l'attributo CodeBehind in Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

L'attributo CodeFileBaseClass viene utilizzato nei casi in cui si desidera più pagine per derivare da una singola classe di base. A causa dell'implementazione di classi parziali in ASP.NET, senza questo attributo, di una classe di base che usa i campi comuni condivisi per fare riferimento a controlli dichiarati in una pagina ASPX non funzionerà correttamente perché ASP. Il motore di compilazione reti crea automaticamente nuovi membri in base a controlli nella pagina. Pertanto, se si desidera una classe base comune per due o più pagine in ASP.NET, sarà necessario definire nell'attributo CodeFileBaseClass specificare la classe base e quindi derivare ogni classe di pagine da tale classe base. È inoltre richiesto l'attributo CodeFile quando questo attributo viene utilizzato.

## <a name="compilationmode"></a>CompilationMode

Questo attributo consente di impostare la proprietà CompilationMode della pagina ASPX. La proprietà CompilationMode è un'enumerazione contenente i valori **sempre**, **Auto**, e **mai**. Il valore predefinito è **sempre**. Il **Auto** impostazione impedirà ASP.NET compilazione dinamica della pagina se possibile. Esclusione di pagine dalla compilazione dinamica migliora le prestazioni. Se, tuttavia, una pagina che è escluso contiene codice che deve essere compilato, è verrà generato un errore quando viene visualizzata la pagina.

## <a name="enableeventvalidation"></a>EnableEventValidation

Questo attributo specifica se convalidare gli eventi di callback e postback. Quando questa opzione è abilitata, gli argomenti per i postback o gli eventi di callback vengono controllati per verificare che hanno origine dal controllo del server che è stato eseguito il rendering.

## <a name="enabletheming"></a>EnableTheming

Questo attributo specifica se i temi ASP.NET vengono utilizzati in una pagina. Il valore predefinito è **false**. I temi ASP.NET sono vedere [modulo 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Questo attributo specifica se i pragma di linea devono essere aggiunto durante la compilazione. Direttive pragma di riga sono le opzioni utilizzate dai debugger per contrassegnare determinate sezioni del codice.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Questo attributo specifica se JavaScript viene inserito nella pagina per mantenere la posizione di scorrimento tra i vari postback. Questo attributo è **false** per impostazione predefinita.

Quando questo attributo è **true**, ASP.NET verrà aggiunto un &lt;script&gt; blocco durante il postback simile al seguente:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Si noti che per questo blocco di script src WebResource. Questa risorsa non è un percorso fisico. Quando viene richiesto questo script, ASP.NET compila in modo dinamico lo script.

### <a name="masterpagefile"></a>MasterPageFile

Questo attributo specifica il file pagina master per la pagina corrente. Il percorso può essere relativo o assoluto. Pagine master rientrano [modulo 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Questo attributo consente di eseguire l'override di proprietà relative all'aspetto dell'interfaccia utente definiti da un tema ASP.NET 2.0. I temi sono trattati in [modulo 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Tema

Specifica il tema della pagina. Se non viene specificato un valore per l'attributo StyleSheetTheme, l'attributo Theme sostituisce tutti gli stili applicati ai controlli della pagina.

## <a name="title"></a>Titolo

Imposta il titolo della pagina. Il valore specificato in questo campo verrà visualizzato nel &lt;titolo&gt; elemento della pagina sottoposta a rendering.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Imposta il valore per l'enumerazione ViewStateEncryptionMode. I valori disponibili sono **sempre**, **Auto**, e **mai**. Il valore predefinito è **Auto**. Quando questo attributo è impostato su un valore di **Auto**, viewstate è crittografato è richiesto da un controllo chiamando la **RegisterRequiresViewStateEncryption** metodo.

## <a name="setting-public-property-values-via-the--page-directive"></a>Impostazione dei valori di proprietà pubblica tramite la direttiva @ Page

Un'altra nuova funzionalità della direttiva @ Page in ASP.NET 2.0 è la possibilità di impostare il valore iniziale di proprietà pubbliche di una classe di base. Si supponga, ad esempio, si dispone di una proprietà pubblica denominata **SomeText** nella classe base e si d simile deve essere inizializzata con **Hello** quando viene caricata una pagina. Questo scopo, è possibile impostare semplicemente il valore nella direttiva @ Page come illustrato di seguito:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

Il **SomeText** attributo della direttiva @ Page imposta il valore iniziale della proprietà SomeText nella classe base per *Hello!*. Il video seguente viene riportata una procedura di impostazione del valore iniziale di una proprietà pubblica in una classe di base utilizzando la direttiva @ Page.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Aprirlo Video a schermo intero](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Nuove proprietà pubblica della classe della pagina

Le proprietà pubbliche seguenti sono nuove in ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Restituisce il percorso relativo dell'applicazione per la pagina o il controllo. Ad esempio, per una pagina disponibile all'indirizzo http://app/folder/page.aspx, la proprietà restituisce ~ / cartella /.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Restituisce il percorso virtuale relativo alla pagina o controllo. Ad esempio per una pagina disponibile all'indirizzo http://app/folder/page.aspx, la proprietà restituisce ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Ottiene o imposta il timeout utilizzato per la gestione della pagina asincroni. (Le pagine asincrone verranno descritta più avanti in questo modulo.)

## <a name="clientquerystring"></a>ClientQueryString

Una proprietà di sola lettura che restituisce la parte della stringa di query dell'URL richiesto. Questo valore è codificato in URL. È possibile utilizzare il metodo UrlDecode della classe HttpServerUtility di decodificarlo.

## <a name="clientscript"></a>ClientScript

Questa proprietà restituisce un oggetto ClientScriptManager che può essere usato per gestire l'emissione di ASP.NETs di script sul lato client. (Più avanti in questo modulo viene descritta la classe ClientScriptManager).

## <a name="enableeventvalidation"></a>EnableEventValidation

Questa proprietà controlla se la convalida dell'evento è abilitato per gli eventi di callback e postback. Quando abilitata, gli argomenti per i postback o gli eventi di callback vengono verificati per garantire che hanno origine dal controllo del server che è stato eseguito il rendering.

## <a name="enabletheming"></a>EnableTheming

Questa proprietà ottiene o imposta un valore booleano che specifica se consentire o meno un tema ASP.NET 2.0 si applica alla pagina.

## <a name="form"></a>Form

Questa proprietà restituisce il formato HTML nella pagina ASPX come oggetto HtmlForm.

## <a name="header"></a>Header

Questa proprietà restituisce un riferimento a un oggetto di HtmlHead che contiene l'intestazione di pagina. È possibile utilizzare l'oggetto HtmlHead restituito da get/set fogli di stile, metatag e così via.

## <a name="idseparator"></a>IdSeparator

Questa proprietà di sola lettura Ottiene il carattere utilizzato per separare gli identificatori di controllo quando ASP.NET compila un ID univoco per i controlli in una pagina. Non è progettato per l'utilizzo diretto dal codice.

## <a name="isasync"></a>IsAsync

Questa proprietà consente di pagine asincrone. Le pagine asincrone sono descritti più avanti in questo modulo.

## <a name="iscallback"></a>IsCallback

Questa proprietà di sola lettura restituisce **true** se la pagina è il risultato di una richiamata. Richiamate vengono descritti più avanti in questo modulo.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Questa proprietà di sola lettura restituisce **true** se la pagina fa parte di un cross-page postback. Postback tra pagine sono descritte più avanti in questo modulo.

## <a name="items"></a>Elementi

Restituisce un riferimento a un'istanza IDictionary che contiene tutti gli oggetti archiviati nel contesto delle pagine. È possibile aggiungere elementi a questo oggetto IDictionary e saranno disponibili per tutta la durata del contesto.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Questa proprietà controlla se ASP.NET genera JavaScript che gestisce che le pagine scorrere posizione nel browser dopo che si verifica un postback. (Dettagli di questa proprietà sono state discusse in precedenza in questo modulo).

## <a name="master"></a>master

Questa proprietà di sola lettura restituisce un riferimento all'istanza MasterPage per una pagina a cui è stata applicata una pagina master.

## <a name="masterpagefile"></a>MasterPageFile

Ottiene o imposta il nome del file pagina master per la pagina. Questa proprietà può essere impostata solo nel metodo PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Questa proprietà ottiene o imposta la lunghezza massima per lo stato di pagine in byte. Se la proprietà è impostata su un numero positivo, lo stato di visualizzazione delle pagine verrà essere suddiviso in più campi nascosti in modo che non superi il numero di byte specificato. Se la proprietà è un numero negativo, lo stato di visualizzazione non essere suddiviso in blocchi.

## <a name="pageadapter"></a>PageAdapter

Restituisce un riferimento all'oggetto PageAdapter che modifica la pagina per il browser richiedente.

## <a name="previouspage"></a>Pagina precedente

Restituisce un riferimento alla pagina precedente in caso di un server o di un cross-page postback.

## <a name="skinid"></a>SkinID

Specifica l'interfaccia di ASP.NET 2.0 da applicare alla pagina.

## <a name="stylesheettheme"></a>StyleSheetTheme

Questa proprietà ottiene o imposta il foglio di stile viene applicato a una pagina.

## <a name="templatecontrol"></a>TemplateControl

Restituisce un riferimento al controllo contenitore per la pagina.

## <a name="theme"></a>Tema

Ottiene o imposta il nome del tema ASP.NET 2.0 applicato alla pagina. Questo valore deve essere impostato prima il metodo PreInit.

## <a name="title"></a>Titolo

Questa proprietà ottiene o imposta il titolo della pagina, così come ottenuto dall'intestazione di pagine.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Ottiene o imposta il ViewStateEncryptionMode della pagina. Vedere una descrizione dettagliata di questa proprietà in precedenza in questo modulo.

## <a name="new-protected-properties-of-the-page-class"></a>Nuove proprietà protette della classe della pagina

Di seguito sono le nuove proprietà protette della classe della pagina in ASP.NET 2.0.

## <a name="adapter"></a>Adattatore

Restituisce un riferimento a ControlAdapter che esegue il rendering della pagina del dispositivo che ne ha richiesto.

## <a name="asyncmode"></a>AsyncMode

Questa proprietà indica se la pagina viene elaborata in modo asincrono. È progettato per l'utilizzo dal runtime e non direttamente nel codice.

## <a name="clientidseparator"></a>ClientIDSeparator

Questa proprietà restituisce il carattere utilizzato come separatore durante la creazione di ID univoco client per i controlli. È progettato per l'utilizzo dal runtime e non direttamente nel codice.

## <a name="pagestatepersister"></a>PageStatePersister

Questa proprietà restituisce l'oggetto PageStatePersister per la pagina. Questa proprietà viene utilizzata principalmente dagli sviluppatori di controlli ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Questa proprietà restituisce un suffic univoco che viene aggiunto al percorso del file per la memorizzazione nella cache i browser. Il valore predefinito è \_ \_ufps = e un numero di 6 cifre.

## <a name="new-public-methods-for-the-page-class"></a>Nuovi metodi pubblici per la classe di pagina

I metodi pubblici seguenti sono nuovi per la classe delle pagine ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Questo metodo registra delegati del gestore eventi per l'esecuzione asincrona delle pagine. Le pagine asincrone sono descritti più avanti in questo modulo.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Applica le proprietà in un foglio di stile pagine alla pagina.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Inizia questo metodo un'attività asincrona.

### <a name="getvalidators"></a>GetValidators

Restituisce una raccolta di validator per il gruppo di convalida specificato o il gruppo di convalida predefinito se non è specificato.

## <a name="registerasynctask"></a>RegisterAsyncTask

Questo metodo registra una nuova attività asincrona. Le pagine asincrone sono descritte più avanti in questo modulo.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Questo metodo indica ad ASP.NET che lo stato di controllo di pagine deve essere resa persistente.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Questo metodo indica ad ASP.NET che viewstate pagine richiede la crittografia.

## <a name="resolveclienturl"></a>ResolveClientUrl

Restituisce un URL relativo che può essere utilizzato per le richieste client per le immagini e così via.

## <a name="setfocus"></a>SetFocus

Questo metodo imposterà lo stato attivo al controllo specificato durante il caricamento della pagina iniziale.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Questo metodo annullerà la registrazione del controllo che viene passato non è più che richiedono la persistenza dello stato di controllo.

## <a name="changes-to-the-page-lifecycle"></a>Modifiche al ciclo di vita di pagina

Il ciclo di vita di pagina in ASP.NET 2.0 non è stato modificato in modo significativo, ma sono disponibili alcuni nuovi metodi che è necessario essere consapevoli di. Il ciclo di vita di ASP.NET 2.0 pagina viene indicato di seguito.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (nuovo in ASP.NET 2.0)

L'evento PreInit è la prima fase del ciclo di vita che uno sviluppatore possa accedere. L'aggiunta di questo evento consente di modificare i temi di ASP.NET 2.0, pagine master a livello di codice, accedere alle proprietà per un profilo di ASP.NET 2.0 e così via. Se si è in uno stato di postback, è importante tenere presente che Viewstate non è ancora stata applicata ai controlli in questa fase del ciclo di vita. Pertanto, se uno sviluppatore modifica una proprietà di un controllo in questa fase, probabilmente sovrascritto in un secondo momento nel ciclo di vita di pagine.

## <a name="init"></a>Init

L'evento Init non è stato modificato da ASP.NET 1. x. Si tratta in cui si desidera leggere o inizializzare le proprietà dei controlli della pagina. In questa fase, pagine master, temi e così via sono già applicati alla pagina.

## <a name="initcomplete-new-in-20"></a>InitComplete (nuovo in 2.0)

L'evento InitComplete viene chiamato alla fine della fase di inizializzazione di pagine. A questo punto del ciclo di vita, è possibile accedere a controlli nella pagina, ma il relativo stato non è ancora popolato.

## <a name="preload-new-in-20"></a>Precaricamento (nuovo in 2.0)

Questo evento viene chiamato dopo l'applicazione di tutti i dati di postback e prima pagina\_carico.

## <a name="load"></a>Load

L'evento di caricamento non è stato modificato da ASP.NET 1. x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (nuovo in 2.0)

L'evento LoadComplete è l'ultimo evento in fase di caricamento di pagine. In questa fase, tutti i dati di postback e viewstate è stato applicato alla pagina.

## <a name="prerender"></a>PreRender

Se si desidera usare viewstate correttamente da mantenere per i controlli che vengono aggiunti alla pagina in modo dinamico, l'evento PreRender è l'ultima opportunità di aggiungerli.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (nuovo in 2.0)

In fase di PreRenderComplete, tutti i controlli sono stati aggiunti alla pagina e la pagina è pronta per essere eseguito il rendering. L'evento PreRenderComplete è l'ultimo evento generato prima di salvata viewstate pagine.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (nuovo in 2.0)

L'evento SaveStateComplete viene chiamato immediatamente dopo tutti gli stati di viewstate e controllo pagina sono stato salvato. Si tratta dell'ultimo evento prima che la pagina viene sottoposto a rendering nel browser.

## <a name="render"></a>Rendering

Non è stato modificato il metodo Render poiché ASP.NET 1. x. Si tratta in cui viene inizializzato l'oggetto HtmlTextWriter e il rendering della pagina nel browser.

## <a name="cross-page-postback-in-aspnet-20"></a>Cross-Page Postback in ASP.NET 2.0

In ASP.NET 1. x, i postback veniva richiesto di registrare nella stessa pagina. Postback tra pagine non riusciti. ASP.NET 2.0 consente di registrare nuovamente a una pagina diversa tramite l'interfaccia IButtonControl. Qualsiasi controllo che implementa l'interfaccia IButtonControl nuovo (pulsante LinkButton e ImageButton oltre ai controlli personalizzati di terze parti) possa sfruttare questa nuova funzionalità tramite l'utilizzo dell'attributo PostBackUrl. Il codice seguente viene illustrato un controllo pulsante che esegue il postback a una seconda pagina.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Quando viene eseguito il postback della pagina, la pagina che viene avviato il postback è accessibile tramite la proprietà di pagina precedente nella seconda pagina. Questa funzionalità viene implementata tramite il Web Form nuovo\_DoPostBackWithOptions funzione sul lato client che ASP.NET 2.0 esegue il rendering della pagina quando un controllo viene rinviata a una pagina diversa. Questa funzione JavaScript viene fornita per il nuovo gestore WebResource.axd che genera script per il client.

Il video seguente è una procedura dettagliata di un cross-page postback.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Aprirlo Video a schermo intero](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Ulteriori informazioni sui postback tra pagine

### <a name="viewstate"></a>ViewState

È possibile avere richiesto manualmente già sulle conseguenze di viewstate dalla prima pagina in uno scenario di postback tra pagine. Dopo tutto, qualsiasi controllo che non implementano IPostBackDataHandler verrà persistente lo stato tramite viewstate, pertanto, per l'accesso alle proprietà del controllo nella seconda pagina di un postback tra pagine, è necessario avere accesso a viewstate per la pagina. ASP.NET 2.0 si occupa di questo scenario con un nuovo campo nascosto nella seconda pagina chiamato \_ \_pagina precedente. Il \_ \_campo modulo pagina precedente contiene viewstate per la prima pagina in modo da poter accedere alle proprietà di tutti i controlli nella seconda pagina.

### <a name="circumventing-findcontrol"></a>Evitando di utilizzare FindControl

Nella procedura dettagliata video di un postback tra pagine, utilizzato il metodo FindControl per ottenere un riferimento al controllo casella di testo nella prima pagina. Tale metodo funziona per tale scopo, ma FindControl è costoso e richiede la scrittura di codice aggiuntivo. Fortunatamente, ASP.NET 2.0 offre un'alternativa al FindControl per questo scopo che funzionerà in molti scenari. La direttiva PreviousPageType consente di disporre di un riferimento alla pagina precedente fortemente tipizzato utilizzando l'attributo VirtualPath o TypeName. L'attributo TypeName consente di specificare il tipo di pagina precedente, mentre l'attributo VirtualPath consente di fare riferimento alla pagina precedente utilizzando un percorso virtuale. Dopo avere impostato la direttiva PreviousPageType, è necessario esporre i controlli e così via a cui si desidera consentire l'accesso utilizzando le proprietà pubbliche.

## <a name="lab-1-cross-page-postback"></a>Laboratorio 1 Cross-Page Postback

In questo laboratorio si creerà un'applicazione che utilizza le nuove funzionalità di postback tra pagine di ASP.NET 2.0.

1. Aprire Visual Studio 2005 e creare un nuovo sito Web ASP.NET.
2. Aggiungere un nuovo Web Form denominato Page2.
3. Aprire il Default.aspx in visualizzazione progettazione e aggiungere un controllo Button e un controllo casella di testo. 

    1. Fornire il controllo pulsante di un ID di **SubmitButton** e la casella di testo di un ID di controllo **UserName**.
    2. Impostare la proprietà PostBackUrl del pulsante Page2.
4. Aprire Page2 nella visualizzazione origine.
5. Aggiungere una direttiva @ PreviousPageType come illustrato di seguito:
6. Aggiungere il codice seguente alla pagina\_carico di code-behind del Page2: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Compilare il progetto facendo clic in fase di compilazione dal menu Compila.
8. Aggiungere il codice seguente per il code-behind per Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Modificare la pagina\_il carico in Page2 al seguente: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Compilare il progetto.
11. Eseguire il progetto.
12. Immettere il nome nella casella di testo e fare clic sul pulsante.
13. Che cos'è il risultato?

## <a name="asynchronous-pages-in-aspnet-20"></a>Pagine asincrone in ASP.NET 2.0

Molti dei problemi di contesa in ASP.NET sono causati dalla latenza di chiamate esterne (ad esempio, le chiamate di database o servizio Web), la latenza dei / o di file e così via. Quando viene effettuata una richiesta rispetto a un'applicazione ASP.NET, ASP.NET utilizza uno dei thread di lavoro per soddisfare tale richiesta. La richiesta appartiene tale thread fino a quando la richiesta è stata completata e la risposta è stata inviata. ASP.NET 2.0 cerca di risolvere i problemi di latenza con questi tipi di problemi aggiungendo la possibilità di eseguire le pagine in modo asincrono. Ciò significa che un thread di lavoro è possibile avviare la richiesta e quindi passare esecuzione aggiuntive a un altro thread, pertanto restituite al pool di thread disponibili rapidamente. Al termine dei / o file, chiamata al database e così via, viene ottenuto un nuovo thread dal pool di thread per completare la richiesta.

Effettua una pagina di eseguire in modo asincrono il primo passaggio consiste nell'impostare il **Async** attributo della direttiva della pagina come illustrato di seguito:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Questo attributo indica ad ASP.NET per implementare il IHttpAsyncHandler per la pagina.

Il passaggio successivo consiste nel chiamare il metodo AddOnPreRenderCompleteAsync in un punto del ciclo di vita della pagina prima di PreRender. (Questo metodo viene chiamato in genere nella pagina\_carico.) Il metodo AddOnPreRenderCompleteAsync accetta due parametri. un BeginEventHandler e un EndEventHandler. BeginEventHandler restituisce un elemento IAsyncResult che viene quindi passato come parametro per il EndEventHandler.

Il video seguente è una procedura dettagliata di una richiesta asincrona di pagina.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Aprirlo Video a schermo intero](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> Una pagina async non esegue il rendering nel browser finché non viene completata la EndEventHandler. Senza dubbio ma che alcuni sviluppatori verranno considerato richieste asincrone come callback asincrono. È importante tenere presente che non lo siano. Il vantaggio per le richieste asincrone è che il primo thread di lavoro può essere restituito al pool di thread per soddisfare nuove richieste, riducendo così la contesa dei / o bound perché e così via.


## <a name="script-callbacks-in-aspnet-20"></a>Gli script di callback in ASP.NET 2.0

Gli sviluppatori Web siano sempre presenti per individuare i modi evitare lo sfarfallio associata a un callback. In ASP.NET 1. x, SmartNavigation è il metodo più comune per evitare lo sfarfallio, ma SmartNavigation provocato problemi per alcuni sviluppatori a causa della complessità dell'implementazione del client. ASP.NET 2.0 risolve questo problema con gli script di callback. Gli script di callback utilizzare XMLHttp per effettuare richieste nei server Web tramite JavaScript. La richiesta XMLHttp restituisce dati XML che possono quindi essere modificati tramite DOM del browser Codice XMLHttp è nascosto da parte dell'utente per il nuovo gestore WebResource.axd.

Esistono diversi passaggi necessari per configurare un callback di script in ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Passaggio 1: Implementare l'interfaccia ICallbackEventHandler

Consentire ad ASP.NET di riconoscere la pagina come parte di un callback di script, è necessario implementare l'interfaccia ICallbackEventHandler. È possibile farlo nel file code-behind come illustrato di seguito:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

È inoltre possibile eseguire questo utilizzo simili direttiva @ implementa in modo:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

La direttiva @ Implements viene utilizzata in genere quando si utilizza codice ASP.NET inline.

## <a name="step-2--call-getcallbackeventreference"></a>Passaggio 2: Chiamare GetCallbackEventReference

Come accennato in precedenza, la chiamata XMLHttp è incapsulata nel gestore WebResource.axd. Quando viene eseguito il rendering della pagina, ASP.NET verrà aggiunto a una chiamata a Web Form\_DoCallback, uno script di client che viene fornito da WebResource. Il Web Form\_DoCallback funzione sostituisce la \_ \_funzione dello script per un callback. Tenere presente che \_ \_dello script a livello di codice invia il form nella pagina. In uno scenario di callback, si desidera evitare che un postback, pertanto \_ \_dello script non è sufficiente.

> [!NOTE]
> \_\_viene comunque eseguito il rendering dello script alla pagina in uno scenario di callback di script client. Tuttavia, non è usato per il callback.


Gli argomenti per il Web Form\_DoCallback funzione sul lato client sono forniti tramite la funzione sul lato server GetCallbackEventReference che viene chiamato in genere nella pagina\_carico. Una chiamata a GetCallbackEventReference tipica potrebbe essere simile al seguente:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> In questo caso, cm è un'istanza di ClientScriptManager. La classe ClientScriptManager verrà descritta più avanti in questo modulo.


Esistono diverse versioni di overload di GetCallbackEventReference. In questo caso, gli argomenti sono:

`this`

Un riferimento al controllo in cui viene chiamato GetCallbackEventReference. In questo caso, è la pagina stessa.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Un argomento stringa che verrà passato dal codice lato client per l'evento sul lato server. In questo caso, passando il valore di un elenco a discesa Im chiamato ddlCompany.

`ShowCompanyName`

Il nome della funzione sul lato client che accetta il valore restituito (come stringa) dall'evento di callback sul lato server. Questa funzione verrà chiamata solo quando il callback sul lato server ha esito positivo. Pertanto, ai fini di efficienza, è in genere consigliabile utilizzare la versione di overload di GetCallbackEventReference che accetta un argomento stringa aggiuntiva che specifica il nome di una funzione sul lato client per eseguire in caso di errore.

`null`

Stringa che rappresenta una funzione sul lato client che avviato prima del callback al server. In questo caso, è nessuno script, pertanto l'argomento è null.

`true`

Valore booleano che specifica se eseguire il callback in modo asincrono.

La chiamata a Web Form\_DoCallback sul client passa questi argomenti. Pertanto, quando questa pagina viene eseguito il rendering sul client, che il codice avrà un aspetto come illustrato di seguito:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Si noti che la firma della funzione nel client è leggermente diversa. La funzione sul lato client passa 5 stringhe e un valore booleano. La stringa aggiuntiva (che è null nell'esempio precedente) contiene la funzione sul lato client che consente di gestire eventuali errori dal callback sul lato server.

## <a name="step-3--hook-the-client-side-control-event"></a>Passaggio 3: Collegare l'evento di controllo lato Client

Si noti che il valore restituito di GetCallbackEventReference precedente è stato assegnato a una variabile di stringa. Tale stringa viene utilizzata per associare un evento lato client per il controllo che avvia il callback. In questo esempio, il callback viene avviato da un elenco a discesa della pagina, pertanto desidera associare il *OnChange* evento.

Per associare l'evento sul lato client, aggiungere semplicemente un gestore per il codice sul lato client come indicato di seguito:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Tenere presente che *cbRef* è il valore restituito dalla chiamata a GetCallbackEventReference. Contiene la chiamata a Web Form\_DoCallback che è stato illustrato in precedenza.

## <a name="step-4--register-the-client-side-script"></a>Passaggio 4: Registrare Script lato Client

Tenere presente che la chiamata a GetCallbackEventReference specificato che uno script sul lato client è stato chiamato **ShowCompanyName** verrà eseguito quando il callback sul lato server ha esito positivo. Lo script deve essere aggiunto alla pagina utilizzando un'istanza ClientScriptManager. (La classe ClientScriptManager saranno descritti più avanti in questo modulo.) Si così che simile:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Passaggio 5: Chiamare i metodi dell'interfaccia ICallbackEventHandler

ICallbackEventHandler contiene due metodi che è necessario implementare nel codice. Sono **RaiseCallbackEvent** e **GetCallbackEvent**.

**RaiseCallbackEvent** accetta una stringa come argomento e non restituisce nulla. L'argomento di stringa viene passato dalla chiamata sul lato client a Web Form\_DoCallback. In questo caso, tale valore è il *valore* attributo dell'elenco a discesa denominato ddlCompany. Il codice lato server deve essere inserito nel metodo RaiseCallbackEvent. Ad esempio, il callback effettua una WebRequest su una risorsa esterna, tale codice deve essere inserito in RaiseCallbackEvent.

**GetCallbackEvent** è responsabile dell'elaborazione la restituzione del callback al client. Non accetta argomenti e restituisce una stringa. La stringa restituita sarà essere passata come argomento alla funzione sul lato client, in questo caso *ShowCompanyName*.

Dopo aver completato i passaggi precedenti, si è pronti per eseguire un callback di script in ASP.NET 2.0.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Aprirlo Video a schermo intero](the-asp-net-2-0-page-model/_static/callback1.wmv)


Gli script di callback in ASP.NET sono supportati in qualsiasi browser che supporta le chiamate XMLHttp. Che include tutti i browser moderni in uso oggi. Internet Explorer utilizza l'oggetto XMLHttp ActiveX mentre altri browser moderni (incluso il future di Internet Explorer 7) utilizzano un oggetto XMLHttp intrinseco. Per determinare a livello di codice se il browser supporta i callback, è possibile utilizzare il **Request.Browser.SupportCallback** proprietà. Questa proprietà restituirà **true** se il client richiedente supporta gli script di callback.

## <a name="working-with-client-script-in-aspnet-20"></a>Utilizzo di Script Client in ASP.NET 2.0

Gli script client in ASP.NET 2.0 vengono gestiti tramite l'utilizzo della classe ClientScriptManager. La classe ClientScriptManager tiene traccia di script client utilizzando un tipo e un nome. Ciò impedisce che lo stesso script a livello di codice da inserire in una pagina con più di una volta.

> [!NOTE]
> Dopo la registrazione di uno script in una pagina correttamente, qualsiasi tentativo successivo per registrare lo stesso script semplicemente comporterà lo script non è registrato un secondo momento. Non vengono aggiunti gli script duplicati e viene generata alcuna eccezione. Per evitare inutile calcolo, sono disponibili metodi che è possibile utilizzare per determinare se uno script è già registrato in modo che non è tentare di effettuare la registrazione più volte.


I metodi di ClientScriptManager necessario conoscere per tutti gli sviluppatori ASP.NET correnti:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Questo metodo aggiunge uno script nella parte superiore della pagina rendering. Ciò è utile per l'aggiunta di funzioni che verranno chiamate in modo esplicito nel client.

Esistono due versioni di overload di questo metodo. Tre dei quattro argomenti sono comuni tra di essi. Ad esempio:

`type (string)`

Il ***tipo*** argomento identifica un tipo per lo script. È in genere consigliabile utilizzare il tipo della pagina (this. GetType()) per il tipo.

`key (string)`

Il ***chiave*** argomento è una chiave definita dall'utente per lo script. Deve essere univoco per ogni script. Se si tenta di aggiungere uno script con la stessa chiave e tipo di uno script già aggiunto, non verrà aggiunto.

`script (string)`

Il ***script*** argomento è una stringa contenente lo script effettivo da aggiungere. Si consiglia di utilizzare StringBuilder per creare lo script e quindi utilizzare il metodo ToString () sull'oggetto StringBuilder per assegnare il ***script*** argomento.

Se si utilizza il RegisterClientScriptBlock overload che accetta solo tre argomenti, è necessario includere gli elementi di script (&lt;script&gt; e &lt;/script&gt;) nello script.

È possibile scegliere di utilizzare l'overload di RegisterClientScriptBlock che accetta un quarto argomento. Il quarto argomento è un valore booleano che specifica se ASP.NET deve aggiungere gli elementi di script per l'utente. Se questo argomento è **true**, lo script non deve includere gli elementi di script in modo esplicito.

Utilizzare il metodo IsClientScriptBlockRegistered per determinare se uno script è già stato registrato. In questo modo si evita un tentativo di registrare uno script che è già stato registrato.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (nuovo in 2.0)

Il tag RegisterClientScriptInclude crea un blocco di script che si collega a un file script esterno. Dispone di due overload. Uno accetta una chiave e un URL. Il secondo aggiunge un terzo argomento che specifica il tipo.

Ad esempio, il codice seguente genera un blocco di script che si collega a jsfunctions.js nella radice della cartella degli script dell'applicazione:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Questo codice genera il codice seguente nella pagina sottoposta a rendering:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Il blocco di script viene eseguito il rendering nella parte inferiore della pagina.


Utilizzare il metodo IsClientScriptIncludeRegistered per determinare se uno script è già stato registrato. In questo modo si evita un tentativo di registrare uno script.

## <a name="registerstartupscript"></a>RegisterStartupScript

Il metodo RegisterStartupScript accetta gli stessi argomenti metodo RegisterClientScriptBlock. Registrato con RegisterStartupScript lo script viene eseguito dopo il caricamento della pagina, ma prima dell'evento sul lato client OnLoad. In 1. x, sono stati inclusi gli script registrati con RegisterStartupScript appena prima della chiusura &lt;/form&gt; tag mentre sono stati inclusi gli script registrati con RegisterClientScriptBlock immediatamente dopo l'apertura &lt;modulo&gt; tag. In ASP.NET 2.0, entrambi sono posizionati immediatamente prima della chiusura &lt;/form&gt; tag.

> [!NOTE]
> Se si registra una funzione con RegisterStartupScript, tale funzione non verrà eseguita finché non viene esplicitamente chiamata nel codice sul lato client.


Utilizzare il metodo IsStartupScriptRegistered per determinare se è già stato registrato uno script ed evitare un tentativo di registrare uno script.

## <a name="other-clientscriptmanager-methods"></a>Altri metodi ClientScriptManager

Ecco alcuni degli altri metodi utili della classe ClientScriptManager.


|  <strong>GetCallbackEventReference</strong>   |                                                 Vedere gli script di callback in precedenza in questo modulo.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Ottiene un riferimento di JavaScript (javascript:&lt;chiamare&gt;) che può essere utilizzato per registrare nuovamente da un evento lato client.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Ottiene una stringa che può essere utilizzata per avviare un post dal client.                                    |
|      <strong>GetWebResourceUrl</strong>       | Restituisce un URL a una risorsa incorporata in un assembly. Deve essere usata in combinazione con <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Registra una risorsa Web con la pagina. Si tratta di risorse incorporato in un assembly e gestito dal gestore WebResource.axd nuovo.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Registra un campo modulo nascosto nella pagina.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registra il codice sul lato client che viene eseguito quando viene inviato il form HTML.                                   |

