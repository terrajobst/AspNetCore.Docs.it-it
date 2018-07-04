---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Il modello di pagina 2.0 ASP.NET | Microsoft Docs
author: microsoft
description: In ASP.NET 1.x, gli sviluppatori poteva scegliere tra un modello di codice inline e un modello di codice code-behind. Code-behind può essere implementato usando entrambi i attr Src...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 9d62aee5e0754b1910b923ad9ae501ebed91097e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379887"
---
<a name="the-aspnet-20-page-model"></a>Il modello ASP.NET 2.0 pagina
====================
da [Microsoft](https://github.com/microsoft)

> In ASP.NET 1.x, gli sviluppatori poteva scegliere tra un modello di codice inline e un modello di codice code-behind. Code-behind può essere implementato usando l'attributo Src o l'attributo CodeBehind del @Page direttiva. In ASP.NET 2.0, gli sviluppatori hanno ancora una scelta tra code-behind e il codice inline, ma sono stati apportati miglioramenti significativi per il modello code-behind.


In ASP.NET 1.x, gli sviluppatori poteva scegliere tra un modello di codice inline e un modello di codice code-behind. Code-behind può essere implementato usando l'attributo Src o l'attributo CodeBehind del @Page direttiva. In ASP.NET 2.0, gli sviluppatori hanno ancora una scelta tra code-behind e il codice inline, ma sono stati apportati miglioramenti significativi per il modello code-behind.

## <a name="improvements-in-the-code-behind-model"></a>Miglioramenti nel modello Code-Behind

Per comprendere pienamente le modifiche nel modello code-behind in ASP.NET 2.0, è consigliabile rivedere velocemente il modello così com'era inclusa in ASP.NET 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Il modello Code-Behind in ASP.NET 1.x

In ASP.NET 1.x, il modello code-behind è costituita da un file ASPX (Web Form) e un file code-behind che contiene il codice di programmazione. I due file sono stati connessi tramite il @Page direttiva nel file ASPX. Ogni controllo nella pagina ASPX aveva una dichiarazione corrispondente nel file code-behind come una variabile di istanza. Il file code-behind anche contenuti codice per l'associazione di eventi e il codice necessario per la finestra di progettazione di Visual Studio generato. Questo modello ha funzionato abbastanza bene, ma poiché ciascun elemento ASP.NET nella pagina ASPX necessari codice corrispondente nel file code-behind, si è verificato alcun true separazione del codice e il contenuto. Ad esempio, se una finestra di progettazione aggiunta un nuovo controllo server in un file ASPX di fuori di IDE di Visual Studio, l'applicazione causa l'interruzione a causa dell'assenza di una dichiarazione per il controllo nel file code-behind.

## <a name="the-code-behind-model-in-aspnet-20"></a>Il modello Code-Behind in ASP.NET 2.0

ASP.NET 2.0 migliora notevolmente questo modello. In ASP.NET 2.0, code-behind viene implementato usando le nuove *classi parziali* forniti in ASP.NET 2.0. La classe code-behind in ASP.NET 2.0 è definiti come una classe parziale, vale a dire che contiene solo una parte della definizione di classe. La parte rimanente della definizione di classe viene dinamicamente generata da ASP.NET 2.0 utilizzando la pagina ASPX in fase di esecuzione oppure quando il sito Web precompilato. Il collegamento tra il file code-behind e la pagina ASPX viene comunque stabilito usando la direttiva @ Page. Tuttavia, invece di un attributo Src o CodeBehind, ASP.NET 2.0 ora Usa l'attributo CodeFile. L'attributo Inherits inoltre consente di specificare il nome della classe per la pagina.

Una direttiva @ Page tipica potrebbe essere simile al seguente:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Una definizione di classe tipico in un file code-behind ASP.NET 2.0 potrebbe essere simile al seguente:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# e Visual Basic sono linguaggi gestiti soli che supportano attualmente le classi parziali. Di conseguenza, gli sviluppatori che usano j# non sarà in grado di usare il modello code-behind in ASP.NET 2.0.


Il nuovo modello consente di migliorare il modello code-behind poiché gli sviluppatori avranno ora i file di codice che contengono solo il codice che ha creato. Fornisce inoltre una separazione di codice e i contenuti true perché non esistono Nessun dichiarazioni di variabili di istanza nel file code-behind.

> [!NOTE]
> Poiché la classe parziale per la pagina ASPX è in cui associazione degli eventi viene eseguita, gli sviluppatori Visual Basic possono realizzare un lieve miglioramento delle prestazioni con la parola chiave gli handle nel code-behind per associare gli eventi. C# non ha nessuna parola chiave equivalente.


## <a name="new--page-directive-attributes"></a>Nuovi attributi direttiva @ Page

ASP.NET 2.0 aggiunge molti nuovi attributi per la direttiva @ Page. Gli attributi seguenti sono novità di ASP.NET 2.0.

## <a name="async"></a>Async

L'attributo Async consente di configurare pagina può essere eseguita in modo asincrono. Verranno trattate le pagine asincrone più avanti in questo modulo.

## <a name="asynctimeout"></a>AsyncTimeout

Specificare il timeout per le pagine asincrone. Il valore predefinito è 45 secondi.

## <a name="codefile"></a>CodeFile

L'attributo CodeFile è la sostituzione per l'attributo CodeBehind in Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

L'attributo CodeFileBaseClass viene usato nei casi in cui si desidera più pagine di derivare da una singola classe di base. A causa dell'implementazione di classi parziali in ASP.NET, senza questo attributo, una classe di base che usa campi comuni condivisi per fare riferimento ai controlli dichiarati in una pagina ASPX non funzionerà correttamente perché ASP. Motore di compilazione reti creerà automaticamente i nuovi membri basati sui controlli nella pagina. Pertanto, se si desidera che una classe base comune per due o più pagine in ASP.NET, sarà necessario definire specificare la classe di base nell'attributo CodeFileBaseClass e quindi derivare ogni classe di pagine da tale classe. L'attributo CodeFile è necessaria inoltre quando questo attributo viene usato.

## <a name="compilationmode"></a>CompilationMode

Questo attributo consente di impostare la proprietà CompilationMode della pagina ASPX. La proprietà CompilationMode è un'enumerazione che contiene i valori **Always**, **automatico**, e **Never**. Il valore predefinito è **sempre**. Il **automatica** impostazione impedirà ASP.NET di compilazione in modo dinamico la pagina se possibile. Esclusione di pagine dalla compilazione dinamica aumenta le prestazioni. Se, tuttavia, una pagina che viene esclusa contiene codice che deve essere compilato, è verrà generato un errore quando viene visualizzata la pagina.

## <a name="enableeventvalidation"></a>EnableEventValidation

Questo attributo specifica se gli eventi postback e callback vengono convalidati. Quando questa opzione è abilitata, gli argomenti di postback o eventi callback vengono controllati per verificare che siano stati originati dal controllo server che originariamente eseguito il rendering.

## <a name="enabletheming"></a>EnableTheming

Questo attributo specifica se vengono utilizzati i temi ASP.NET in una pagina. Il valore predefinito è **false**. Temi ASP.NET vengono analizzati [10 modulo](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Questo attributo specifica se direttive pragma di riga devono essere aggiunto durante la compilazione. Direttive pragma di riga sono opzioni usate dai debugger per contrassegnare determinate sezioni del codice.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Questo attributo specifica se JavaScript è inserita la pagina per mantenere la posizione di scorrimento tra i vari postback. Questo attributo viene **false** per impostazione predefinita.

Quando questo attributo è **true**, ASP.NET aggiungerà un &lt;script&gt; blocco durante il postback che si presenta come segue:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Si noti che l'elemento src per questo blocco di script WebResource. axd. Questa risorsa non è un percorso fisico. Quando viene richiesto questo script, ASP.NET compila in modo dinamico lo script.

### <a name="masterpagefile"></a>MasterPageFile

Questo attributo specifica il file pagina master per la pagina corrente. Il percorso può essere relativo o assoluto. Pagine master vengono analizzate [modulo 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Questo attributo consente di eseguire l'override di proprietà dell'aspetto dell'interfaccia utente definita da un tema di ASP.NET 2.0. I temi sono illustrati nella [10 modulo](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Tema

Specifica il tema della pagina. Se non viene specificato un valore per l'attributo StyleSheetTheme, l'attributo Theme esegue l'override di tutti gli stili applicati ai controlli della pagina.

## <a name="title"></a>Titolo

Imposta il titolo della pagina. Il valore specificato qui verrà visualizzato il &lt;titolo&gt; elemento della pagina sottoposta a rendering.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Imposta il valore per l'enumerazione ViewStateEncryptionMode. I valori disponibili sono **Always**, **automatico**, e **Never**. Il valore predefinito è **automatica**. Quando questo attributo è impostato su un valore di **automatica**, viewstate è crittografato è richiesto da un controllo chiamando la **RegisterRequiresViewStateEncryption** (metodo).

## <a name="setting-public-property-values-via-the--page-directive"></a>Impostazione dei valori di proprietà pubblico tramite la direttiva @ Page

Un'altra nuova funzionalità della direttiva @ Page in ASP.NET 2.0 è la possibilità di impostare il valore iniziale della proprietà pubbliche di una classe di base. Si supponga, ad esempio, di avere una proprietà pubblica denominata **SomeText** nella classe base ed è il d, ad esempio, deve essere inizializzata con **Hello** quando viene caricata una pagina. È possibile farlo impostando semplicemente il valore nella direttiva @ Page come segue:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

Il **SomeText** attributo della direttiva @ Page imposta il valore iniziale della proprietà SomeText nella classe di base per *Hello!*. Il video seguente è una procedura dettagliata dell'impostazione del valore iniziale di una proprietà pubblica in una classe base usando la direttiva @ Page.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Aprirlo Video a schermo intero](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Nuove proprietà pubbliche della classe della pagina

Le proprietà pubbliche seguenti sono novità di ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Restituisce il percorso relativo dell'applicazione per la pagina o controllo. Ad esempio, per una pagina disponibile all'indirizzo http://app/folder/page.aspx, la proprietà restituisce ~ / cartella /.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Restituisce il percorso di directory virtuale relativa alla pagina o controllo. Ad esempio per una pagina disponibile all'indirizzo http://app/folder/page.aspx, la proprietà restituisce ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Ottiene o imposta il timeout utilizzato per la gestione di pagina asincrona. (Le pagine asincrone verranno trattate più avanti in questo modulo.)

## <a name="clientquerystring"></a>ClientQueryString

Una proprietà di sola lettura che restituisce la parte della stringa di query dell'URL richiesto. Questo valore è codificato in URL. È possibile utilizzare il metodo UrlDecode della classe HttpServerUtility decodificarla.

## <a name="clientscript"></a>ClientScript

Questa proprietà restituisce un oggetto ClientScriptManager che può essere utilizzato per gestire ASP.NETs emissione di script sul lato client. (La classe ClientScriptManager è descritta più avanti in questo modulo).

## <a name="enableeventvalidation"></a>EnableEventValidation

Questa proprietà controlla se la convalida degli eventi è abilitata per gli eventi postback e callback. Quando abilitata, gli argomenti di postback o eventi callback vengono verificati per assicurarsi che siano stati originati dal controllo server che originariamente eseguito il rendering.

## <a name="enabletheming"></a>EnableTheming

Questa proprietà ottiene o imposta un valore booleano che specifica se un tema di ASP.NET 2.0 si applica alla pagina.

## <a name="form"></a>Form

Questa proprietà restituisce il form HTML nella pagina ASPX come oggetto HtmlForm.

## <a name="header"></a>Header

Questa proprietà restituisce un riferimento a un oggetto HtmlHead che contiene l'intestazione di pagina. È possibile usare l'oggetto HtmlHead restituito per ottenere o impostare i fogli di stile, metatag e così via.

## <a name="idseparator"></a>IdSeparator

Questa proprietà di sola lettura Ottiene il carattere utilizzato per separare gli identificatori di controllo quando ASP.NET compila un ID univoco per i controlli in una pagina. Non è progettato per l'utilizzo diretto dal codice.

## <a name="isasync"></a>IsAsync

Questa proprietà consente di pagine asincrone. Le pagine asincrone sono descritti più avanti in questo modulo.

## <a name="iscallback"></a>IsCallback

Questa proprietà di sola lettura restituisce **true** se la pagina è il risultato di una funzione di richiamata. Funzione di richiamata è illustrate più avanti in questo modulo.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Questa proprietà di sola lettura restituisce **true** se la pagina fa parte di un postback fra pagine. Postback tra pagine sono descritte più avanti in questo modulo.

## <a name="items"></a>Elementi

Restituisce un riferimento a un'istanza IDictionary contenente tutti gli oggetti archiviati nel contesto di pagine. È possibile aggiungere elementi a questo oggetto IDictionary e saranno disponibili all'utente per tutta la durata del contesto.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Questa proprietà controlla se ASP.NET genera JavaScript che gestisce che le pagine scorrere posizione nel browser dopo che si verifica un postback. (Dettagli di questa proprietà sono stati illustrati in precedenza in questo modulo).

## <a name="master"></a>Master

Questa proprietà di sola lettura restituisce un riferimento all'istanza di pagina master per una pagina a cui è stata applicata una pagina master.

## <a name="masterpagefile"></a>MasterPageFile

Ottiene o imposta il nome del file pagina master per la pagina. Questa proprietà può essere impostata solo nel metodo PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Questa proprietà ottiene o imposta la lunghezza massima per lo stato di pagine in byte. Se la proprietà è impostata su un numero positivo, lo stato di visualizzazione di pagine sarà essere suddivisi in più campi nascosti in modo che non superi il numero di byte specificato. Se la proprietà è un numero negativo, lo stato di visualizzazione non verrà suddiviso in blocchi.

## <a name="pageadapter"></a>PageAdapter

Restituisce un riferimento all'oggetto PageAdapter che modifica la pagina per il browser richiedente.

## <a name="previouspage"></a>PreviousPage

Restituisce un riferimento alla pagina precedente in caso di un server. Transfer o un postback fra pagine.

## <a name="skinid"></a>SkinID

Specifica l'interfaccia di ASP.NET 2.0 da applicare alla pagina.

## <a name="stylesheettheme"></a>StyleSheetTheme

Questa proprietà ottiene o imposta il foglio di stile viene applicato a una pagina.

## <a name="templatecontrol"></a>TemplateControl

Restituisce un riferimento al controllo contenitore per la pagina.

## <a name="theme"></a>Tema

Ottiene o imposta il nome del tema ASP.NET 2.0 applicato alla pagina. Questo valore deve essere impostato prima il metodo PreInit.

## <a name="title"></a>Titolo

Questa proprietà ottiene o imposta il titolo della pagina come ottenuta dall'intestazione di pagine.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Ottiene o imposta il ViewStateEncryptionMode della pagina. Vedere una descrizione dettagliata di questa proprietà in precedenza in questo modulo.

## <a name="new-protected-properties-of-the-page-class"></a>Nuove proprietà protette della classe della pagina

Di seguito sono le nuove proprietà protette della classe della pagina in ASP.NET 2.0.

## <a name="adapter"></a>Adattatore

Restituisce un riferimento a ControlAdapter rendering della pagina nel dispositivo che ha richiesto.

## <a name="asyncmode"></a>AsyncMode

Questa proprietà indica se la pagina viene elaborata in modo asincrono. Si tratta per l'utilizzo dal runtime e non direttamente nel codice.

## <a name="clientidseparator"></a>ClientIDSeparator

Questa proprietà restituisce il carattere utilizzato come separatore durante la creazione di ID univoco client per i controlli. Si tratta per l'utilizzo dal runtime e non direttamente nel codice.

## <a name="pagestatepersister"></a>PageStatePersister

Questa proprietà restituisce l'oggetto PageStatePersister per la pagina. Questa proprietà viene utilizzata principalmente dagli sviluppatori di controlli ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Questa proprietà restituisce un suffic univoco che viene aggiunto al percorso del file per la memorizzazione nella cache i browser. Il valore predefinito è \_ \_ufps = e un numero di 6 cifre.

## <a name="new-public-methods-for-the-page-class"></a>Nuovi metodi pubblici per la classe delle pagine

I seguenti metodi pubblici sono nuovi per la classe delle pagine in ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Questo metodo registra i delegati dei gestori eventi per l'esecuzione asincrona delle pagine. Le pagine asincrone sono descritti più avanti in questo modulo.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Applica le proprietà in un foglio di stile pagine alla pagina.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Gli esseri questo metodo un'attività asincrona.

### <a name="getvalidators"></a>GetValidators

Restituisce una raccolta di validator per il gruppo di convalida specificato o il gruppo di convalida predefinito se non è specificato.

## <a name="registerasynctask"></a>RegisterAsyncTask

Questo metodo registra una nuova attività asincrona. Le pagine asincrone sono descritte più avanti in questo modulo.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Questo metodo indica ad ASP.NET che lo stato del controllo pagine debba essere resi persistenti.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Questo metodo indica ad ASP.NET che viewstate pagine richiede la crittografia.

## <a name="resolveclienturl"></a>ResolveClientUrl

Restituisce un URL relativo che può essere usato per le richieste client per le immagini e così via.

## <a name="setfocus"></a>Funzione SetFocus

Questo metodo imposterà lo stato attivo al controllo che viene specificato quando la pagina viene caricata inizialmente.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Questo metodo verrà annullata la registrazione di controllo che viene passato come non è più che richiedono la persistenza dello stato di controllo.

## <a name="changes-to-the-page-lifecycle"></a>Modifiche al ciclo di vita di pagina

Il ciclo di vita di pagina in ASP.NET 2.0 non è stato modificato in modo significativo, ma sono disponibili alcuni nuovi metodi che è necessario essere consapevoli di. Il ciclo di vita di pagina ASP.NET 2.0 è descritto di seguito.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (novità di ASP.NET 2.0)

L'evento PreInit è la prima fase del ciclo di vita che uno sviluppatore possa accedere. L'aggiunta di questo evento rende possibile modificare i temi di ASP.NET 2.0, le pagine master a livello di codice, accedere alle proprietà per un profilo di ASP.NET 2.0 e così via. Se trovano in uno stato di postback, è importante tenere presente che Viewstate non è ancora stata applicata ai controlli in questa fase del ciclo di vita. Pertanto, se uno sviluppatore viene modificata una proprietà di un controllo in questa fase, verrà probabilmente sovrascritto in un secondo momento nel ciclo di vita di pagine.

## <a name="init"></a>Init

L'evento Init non è stato modificato da ASP.NET 1.x. Si tratta in cui si desidera leggere o inizializzare le proprietà dei controlli della pagina. In questa fase, le pagine master, temi, e così via sono già applicato alla pagina.

## <a name="initcomplete-new-in-20"></a>InitComplete (nuove in 2.0)

L'evento InitComplete viene chiamato alla fine della fase di inizializzazione di pagine. In questa fase del ciclo di vita, è possibile accedere controlli della pagina, ma il relativo stato non è stato ancora popolato.

## <a name="preload-new-in-20"></a>Precaricare (nuove in 2.0)

Questo evento viene chiamato dopo l'applicazione di tutti i dati di postback e appena prima della pagina\_carico.

## <a name="load"></a>Load

L'evento di caricamento non è stato modificato da ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (nuove in 2.0)

L'evento LoadComplete è l'ultimo evento in fase di caricamento delle pagine. In questa fase, tutti i dati di postback e di viewstate è stato applicato alla pagina.

## <a name="prerender"></a>PreRender

Se si desidera per viewstate venga mantenuto correttamente per i controlli che vengono aggiunti alla pagina in modo dinamico, l'evento PreRender è l'ultima possibilità per aggiungerli.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (nuove in 2.0)

Nella fase PreRenderComplete, tutti i controlli sono stati aggiunti alla pagina e la pagina è pronta per essere eseguito il rendering. L'evento PreRenderComplete è l'ultimo evento generato prima che venga salvato il viewstate di pagine.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (nuove in 2.0)

L'evento SaveStateComplete viene chiamato immediatamente dopo aver salvato tutti gli stati di viewstate e controllo pagina. Si tratta dell'ultimo evento prima che la pagina viene sottoposto a rendering nel browser.

## <a name="render"></a>Rendering

Il metodo Render non è stata modificata in ASP.NET 1.x. Si tratta in cui viene inizializzato l'oggetto HtmlTextWriter e il rendering della pagina nel browser.

## <a name="cross-page-postback-in-aspnet-20"></a>Postback tra pagine in ASP.NET 2.0

In ASP.NET 1.x, durante i postback erano necessari per eseguire la stessa pagina. Postback tra pagine non erano consentiti. ASP.NET 2.0 aggiunge la possibilità di eseguire il postback di una pagina diversa tramite l'interfaccia IButtonControl. Qualsiasi controllo che implementa l'interfaccia IButtonControl nuovo (pulsante LinkButton e ImageButton oltre a controlli personalizzati di terze parti) possa sfruttare questa nuova funzionalità tramite l'uso dell'attributo PostBackUrl. Il codice seguente illustra un controllo pulsante che esegue il postback a una seconda pagina.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Quando viene eseguita il postback della pagina, la pagina che avvia il postback è accessibile tramite la proprietà PreviousPage nella seconda pagina. Questa funzionalità viene implementata tramite il nuovo WebForm\_DoPostBackWithOptions funzione sul lato client che ASP.NET 2.0 esegue il rendering della pagina quando un controllo viene rinviata a un'altra pagina. Questa funzione JavaScript viene fornita per il nuovo gestore WebResource. axd che genera lo script al client.

Il video seguente è una procedura dettagliata di un postback fra pagine.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Aprirlo Video a schermo intero](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Altri dettagli sul postback tra pagine

### <a name="viewstate"></a>ViewState

Potrebbe avere richiesto familiarità già su ciò che accade all'interno del viewstate dalla prima pagina in uno scenario di postback fra pagine. Dopo tutto, qualsiasi controllo che non implementa IPostBackDataHandler sarà persistente lo stato tramite viewstate, pertanto, per disporre dell'accesso alle proprietà del controllo nella seconda pagina di un postback fra pagine, è necessario avere accesso all'interno del viewstate per la pagina. ASP.NET 2.0 si occupa di questo scenario usando un nuovo campo nascosto nella seconda pagina chiamato \_ \_PREVIOUSPAGE. Il \_ \_campo del form PREVIOUSPAGE contiene il viewstate per la prima pagina in modo da poter accedere alle proprietà di tutti i controlli nella seconda pagina.

### <a name="circumventing-findcontrol"></a>Evitando di utilizzare FindControl

Nella procedura dettagliata video di un postback fra pagine, ho utilizzato il metodo FindControl per ottenere un riferimento al controllo casella di testo nella prima pagina. Questo metodo funziona per tale scopo, ma FindControl è costoso e richiede la scrittura di codice aggiuntivo. Per fortuna, ASP.NET 2.0 fornisce un'alternativa a FindControl per questo scopo che funzionerà in molti scenari. La direttiva PreviousPageType consente di creare un riferimento fortemente tipizzato alla pagina precedente usando l'attributo VirtualPath o TypeName. L'attributo TypeName consente di specificare il tipo di pagina precedente mentre l'attributo VirtualPath consente di fare riferimento alla pagina precedente usando un percorso virtuale. Dopo aver impostato la direttiva PreviousPageType, è necessario esporre i controlli e così via al quale si desidera consentire l'accesso usando le proprietà pubbliche.

## <a name="lab-1-cross-page-postback"></a>Laboratorio 1 Cross-Page Postback

In questo laboratorio si creerà un'applicazione che usa la nuova funzionalità di postback fra pagine di ASP.NET 2.0.

1. Aprire Visual Studio 2005 e creare un nuovo sito Web ASP.NET.
2. Aggiungere un nuovo Web Form denominato Page2.
3. Aprire Default. aspx in visualizzazione progettazione e aggiungere un controllo Button e un controllo casella di testo. 

    1. Assegnare un ID di controllo Button **Pulsanteinvia** e un ID di controllare la casella di testo **UserName**.
    2. Impostare la proprietà PostBackUrl del pulsante per Page2.
4. Aprire Page2 nella visualizzazione origine.
5. Aggiungere una direttiva @ PreviousPageType come illustrato di seguito:
6. Aggiungere il codice seguente alla pagina\_carico del code-behind del page2.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Compilare il progetto facendo clic in fase di compilazione dal menu Compila.
8. Aggiungere il codice seguente per il code-behind per default. aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. La pagina Cambia\_il carico in page2.aspx al seguente: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Compilare il progetto.
11. Eseguire il progetto.
12. Immettere il nome nella casella di testo e fare clic sul pulsante.
13. Che cos'è il risultato?

## <a name="asynchronous-pages-in-aspnet-20"></a>Pagine asincrone in ASP.NET 2.0

Molti dei problemi di contesa in ASP.NET sono causati da latenza delle chiamate esterne (ad esempio, le chiamate di database o servizio Web), la latenza dei / o di file e così via. Quando una richiesta viene effettuata per un'applicazione ASP.NET, ASP.NET usa uno dei relativi thread di lavoro per soddisfare tale richiesta. Tale richiesta è il proprietario del thread fino al completamento della richiesta e la risposta è stata inviata. ASP.NET 2.0 si cerca di risolvere i problemi di latenza con questi tipi di problemi aggiungendo la possibilità di eseguire le pagine in modo asincrono. Ciò significa che un thread di lavoro può avviare la richiesta e quindi consegnare esecuzione aggiuntivi a un altro thread, in tal modo la restituzione al pool di thread disponibili rapidamente. Quando è stata completata il / o file, chiamata al database e così via, un nuovo thread viene ottenuto dal pool di thread per completare la richiesta.

Il primo passaggio nella creazione di una pagina eseguita in modo asincrono consiste nell'impostare il **Async** attributo della direttiva della pagina come segue:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Questo attributo indica ad ASP.NET per implementare il IHttpAsyncHandler per la pagina.

Il passaggio successivo consiste nel chiamare il metodo AddOnPreRenderCompleteAsync in un punto nel ciclo di vita della pagina prima PreRender. (Questo metodo viene chiamato in genere nella pagina\_Load.) Il metodo AddOnPreRenderCompleteAsync accetta due parametri; un BeginEventHandler e un EndEventHandler. BeginEventHandler restituisce un elemento IAsyncResult che viene quindi passato come parametro per il EndEventHandler.

Il video seguente è una procedura dettagliata di una richiesta di pagina asincrona.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Aprirlo Video a schermo intero](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> Una pagina asincrona non esegue il rendering nel browser fino a quando non è stata completata la EndEventHandler. Senza dubbio, ma che alcuni sviluppatori saranno considerare le richieste asincrone siano simili ai callback asincrono. È importante tenere presente che non si trovano. Il vantaggio per le richieste asincrone è che il primo thread di lavoro può essere restituito al pool di thread per soddisfare nuove richieste, riducendo in conflitto a causa dei / o bound e così via.


## <a name="script-callbacks-in-aspnet-20"></a>Gli script di callback in ASP.NET 2.0

Gli sviluppatori Web hanno sempre cercato di modi evitare lo sfarfallio associati a un callback. In ASP.NET 1.x, SmartNavigation era il metodo più comune per evitare lo sfarfallio, ma SmartNavigation causato problemi per alcuni sviluppatori a causa della complessità dell'implementazione nel client. ASP.NET 2.0 risolve questo problema con gli script di callback. I callback dello script usano XMLHttp per effettuare richieste nei server Web tramite JavaScript. La richiesta XMLHttp restituisce dati XML che possono quindi essere modificati tramite DOM. del browser Codice XMLHttp è nascosta da parte dell'utente per il nuovo gestore WebResource. axd.

Sono disponibili diversi passaggi necessari per configurare un callback di script in ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Passaggio 1: Implementare l'interfaccia ICallbackEventHandler

Affinché ASP.NET riconoscere la pagina come ambito partecipante a un callback di script, è necessario implementare l'interfaccia ICallbackEventHandler. È possibile eseguire questa operazione nel file code-behind in questo modo:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Si può anche eseguire questa operazione usando simili direttiva @ Implements così:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

La direttiva @ Implements si usa in genere quando si usa il codice ASP.NET inline.

## <a name="step-2--call-getcallbackeventreference"></a>Passaggio 2: Chiamata GetCallbackEventReference

Come accennato in precedenza, la chiamata di XMLHttp è incapsulata nel gestore WebResource. axd. Quando viene eseguito il rendering della pagina, ASP.NET aggiungerà una chiamata a Web Form\_DoCallback, uno script client viene fornito da WebResource. axd. Il Web Form\_DoCallback funzione sostituisce le \_ \_funzione dello script per un callback. Tenere presente che \_ \_dello script a livello di codice invia il form nella pagina. In uno scenario di callback, si desidera impedire un postback, così \_ \_dello script non è sufficiente.

> [!NOTE]
> \_\_viene ancora eseguito il rendering dello script alla pagina in uno scenario di callback di script client. Tuttavia, non viene utilizzato per il callback.


Gli argomenti per il Web Form\_DoCallback funzione sul lato client sono disponibili tramite la funzione sul lato server GetCallbackEventReference che viene normalmente chiamato nella pagina\_carico. Una tipica chiamata a GetCallbackEventReference potrebbe essere simile al seguente:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> In questo caso, cm è un'istanza di ClientScriptManager. Classe ClientScriptManager verrà trattata più avanti in questo modulo.


Esistono diverse versioni di overload di GetCallbackEventReference. In questo caso, gli argomenti sono come segue:

`this`

Un riferimento al controllo in cui viene chiamato GetCallbackEventReference. In questo caso, è la pagina stessa.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Un argomento stringa che verrà passato dal codice lato client per l'evento sul lato server. In questo caso, messaggistica immediata passando il valore di un elenco a discesa denominato ddlCompany.

`ShowCompanyName`

Il nome della funzione sul lato client, che accetta il valore restituito (come stringa) dall'evento di callback sul lato server. Questa funzione verrà chiamata solo quando il callback sul lato server ha esito positivo. Pertanto, ai fini di efficienza, è in genere consigliabile usare la versione di overload di GetCallbackEventReference che accetta un argomento stringa aggiuntiva che specifica il nome di una funzione sul lato client, da eseguire in caso di errore.

`null`

Stringa che rappresenta una funzione sul lato client, che è stato avviato prima del callback al server. In questo caso, vi è alcuno script questo tipo, in modo che l'argomento è null.

`true`

Valore booleano che specifica se eseguire il callback in modo asincrono o meno.

La chiamata a Web Form\_DoCallback sul client passa questi argomenti. Pertanto, quando questa pagina viene eseguito il rendering sul client, tale codice apparirà come segue:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Si noti che la firma della funzione sul client è leggermente diversa. La funzione lato client passa 5 stringhe e un valore booleano. La stringa aggiuntiva, ovvero null nell'esempio precedente, contiene la funzione sul lato client, che consente di gestire eventuali errori dal callback sul lato server.

## <a name="step-3--hook-the-client-side-control-event"></a>Passaggio 3: Associare l'evento di controllo lato Client

Si noti che il valore restituito di GetCallbackEventReference precedente è stato assegnato a una variabile di stringa. Tale stringa viene utilizzata per associare un evento lato client per il controllo che avvia il callback. In questo esempio, il callback viene avviato da un elenco a discesa nella pagina, in modo che si desidera collegare il *OnChange* evento.

Per eseguire l'hook dell'evento dal lato client, è sufficiente aggiungere un gestore al markup lato client come indicato di seguito:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

È importante ricordare che *cbRef* è il valore restituito dalla chiamata a GetCallbackEventReference. Contiene la chiamata a Web Form\_DoCallback che è stato illustrato in precedenza.

## <a name="step-4--register-the-client-side-script"></a>Passaggio 4: Registrare Script lato Client

Tenere presente che la chiamata a GetCallbackEventReference specificato che uno script lato client è stato chiamato **ShowCompanyName** verrebbero eseguite durante il callback sul lato server ha esito positivo. Lo script deve essere aggiunto alla pagina di utilizzo di un'istanza ClientScriptManager. (La classe ClientScriptManager saranno descritti più avanti in questo modulo.) È effettuare quindi tale simile a:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Passaggio 5: Chiamare i metodi dell'interfaccia ICallbackEventHandler

ICallbackEventHandler contiene due metodi che è necessario implementare nel codice. Si trovano **RaiseCallbackEvent** e **GetCallbackEvent**.

**RaiseCallbackEvent** accetta una stringa come argomento e non restituisce nulla. L'argomento di stringa viene passato dalla chiamata lato client a Web Form\_DoCallback. In questo caso, tale valore è il *valore* attributo dell'elenco a discesa denominato ddlCompany. Il codice lato server deve essere inserito nel metodo RaiseCallbackEvent. Ad esempio, se il callback è un oggetto WebRequest contro una risorsa esterna, tale codice deve essere inserito in RaiseCallbackEvent.

**GetCallbackEvent** è responsabile dell'elaborazione il ritorno del callback al client. Non accetta alcun argomento e restituisce una stringa. La stringa restituita sarà essere passata come argomento alla funzione sul lato client, in questo caso *ShowCompanyName*.

Dopo aver completato i passaggi precedenti, si è pronti per eseguire un callback di script in ASP.NET 2.0.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Aprirlo Video a schermo intero](the-asp-net-2-0-page-model/_static/callback1.wmv)


Gli script di callback in ASP.NET sono supportati in qualsiasi browser che supporta le chiamate di XMLHttp. Che include tutti i moderni browser in uso oggi stesso. Internet Explorer utilizza l'oggetto XMLHttp ActiveX mentre altri browser moderni, inclusi l'imminente Internet Explorer 7, usare un oggetto XMLHttp intrinseco. Per determinare a livello di programmazione se un browser supporta i callback, è possibile usare la **Request.Browser.SupportCallback** proprietà. Questa proprietà restituirà **true** se il client richiedente supporta gli script di callback.

## <a name="working-with-client-script-in-aspnet-20"></a>Utilizzo di Script Client in ASP.NET 2.0

Gli script client in ASP.NET 2.0 vengono gestiti tramite l'uso della classe ClientScriptManager. Classe ClientScriptManager tiene traccia di script client con un tipo e un nome. Ciò impedisce che lo stesso script a livello di codice inserito in una pagina più volte.

> [!NOTE]
> Dopo aver registrato correttamente uno script in una pagina, eventuali tentativi successivi per registrare lo stesso script semplicemente comporterà lo script non è registrato un secondo momento. Non vengono aggiunti gli script duplicati e si verifica alcuna eccezione. Per evitare di calcolo inutile, sono disponibili metodi che è possibile usare per determinare se uno script è già registrato in modo che non si tenta di registrarlo più volte.


I metodi di ClientScriptManager dovrebbero essere noti a tutti gli sviluppatori ASP.NET correnti:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Questo metodo aggiunge uno script nella parte superiore della pagina sottoposta a rendering. Ciò è utile per l'aggiunta di funzioni che verranno chiamate in modo esplicito nel client.

Esistono due versioni di overload di questo metodo. Tre dei quattro argomenti sono comuni tra di essi. Ad esempio:

`type (string)`

Il ***tipo*** argomento identifica un tipo per lo script. In genere è consigliabile usare il tipo della pagina (this. GetType()) per il tipo.

`key (string)`

Il ***chiave*** argomento è una chiave definita dall'utente per lo script. Deve essere univoco per ogni script. Se si prova ad aggiungere uno script con la stessa chiave e tipo di uno script già aggiunto, non verrà aggiunto.

`script (string)`

Il ***script*** argomento è una stringa che contiene l'effettivo script da aggiungere. Si consiglia di utilizzare StringBuilder per creare lo script e quindi usare il metodo ToString () sull'oggetto StringBuilder per assegnare il ***script*** argomento.

Se si usa il RegisterClientScriptBlock overload che accetta solo tre argomenti, è necessario includere gli elementi di script (&lt;script&gt; e &lt;/script&gt;) nello script.

È possibile usare l'overload di RegisterClientScriptBlock che accetta un quarto argomento. Il quarto argomento è un valore booleano che specifica se ASP.NET deve aggiungere elementi script per l'utente. Se questo argomento è **true**, lo script non deve includere gli elementi di script in modo esplicito.

Usare il metodo IsClientScriptBlockRegistered per determinare se uno script è già stato registrato. In questo modo si evita un tentativo di registrare nuovamente uno script che è già stato registrato.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (nuove in 2.0)

Il tag RegisterClientScriptInclude crea un blocco di script che si collega a un file di script esterni. Ha due overload. Uno accetta una chiave e un URL. Il secondo aggiunge un terzo argomento che specifica il tipo.

Ad esempio, il codice seguente genera un blocco di script che si collega a jsfunctions.js nella radice della cartella degli script dell'applicazione:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Questo codice genera il codice seguente nella pagina sottoposta a rendering:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Il blocco di script viene eseguito il rendering nella parte inferiore della pagina.


Usare il metodo IsClientScriptIncludeRegistered per determinare se uno script è già stato registrato. In questo modo si evita un tentativo di registrare nuovamente uno script.

## <a name="registerstartupscript"></a>RegisterStartupScript

Il metodo RegisterStartupScript accetta gli stessi argomenti di metodo RegisterClientScriptBlock. Registrato con RegisterStartupScript lo script viene eseguito dopo il caricamento della pagina, ma prima dell'evento dal lato client OnLoad. Nella versione 1.X, sono stati inseriti gli script registrati con RegisterStartupScript subito prima del tag &lt;/form&gt; anche se sono stati inseriti gli script registrati con RegisterClientScriptBlock immediatamente dopo l'apertura del tag &lt;form&gt; tag. In ASP.NET 2.0, entrambi sono posizionati immediatamente prima della chiusura &lt;/form&gt; tag.

> [!NOTE]
> Se si registra una funzione con RegisterStartupScript, tale funzione non verrà eseguita fino a quando non è esplicitamente chiamarlo nel codice lato client.


Usare il metodo IsStartupScriptRegistered per determinare se è già stato registrato uno script ed evitare un tentativo di registrare nuovamente uno script.

## <a name="other-clientscriptmanager-methods"></a>Altri metodi ClientScriptManager

Ecco alcuni degli altri utili metodi della classe ClientScriptManager.


|  <strong>GetCallbackEventReference</strong>   |                                                 Vedere gli script di callback in precedenza in questo modulo.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Ottiene un riferimento di JavaScript (javascript:&lt;chiamare&gt;) che può essere utilizzato per registrare il failback da un evento lato client.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Ottiene una stringa che può essere utilizzata per eseguire una chiamata post dal client.                                    |
|      <strong>GetWebResourceUrl</strong>       | Restituisce un URL a una risorsa incorporata in un assembly. Deve essere usata in combinazione con <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Registra una risorsa Web con la pagina. Queste sono le risorse incorporate in un assembly e gestita dal gestore WebResource. axd nuovo.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Registra un campo del form nascosto con la pagina.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registra il codice lato client che viene eseguito quando viene inviato il form HTML.                                   |

