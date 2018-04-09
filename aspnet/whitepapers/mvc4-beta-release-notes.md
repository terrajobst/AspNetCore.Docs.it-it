---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive la versione di ASP.NET MVC 4 Beta per Visual Studio 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: d29f09d726e835c1eb1fc38e643a4bfe7f00f61c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Questo documento descrive la versione di ASP.NET MVC 4 Beta per Visual Studio 2010.
> 
> > [!NOTE]
> > Questa operazione è la versione più recente. Sono disponibili le note sulla versione di ASP.NET MVC 4 RC [qui](mvc4-release-notes.md).


- [Note sull'installazione](#_Toc303253802)
- [Documentazione](#_Toc303253803)
- [Supporto](#_Toc303253804)
- [Requisiti software](#_Toc303253805)
- [Aggiornamento di un progetto ASP.NET MVC 3 ad ASP.NET MVC 4](#_Toc303253806)
- [Nuove funzionalità in ASP.NET MVC 4 Beta](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Applicazione a pagina singola di ASP.NET](#_Toc317096198)
    - [Miglioramenti apportati a modelli di progetto predefiniti](#_Toc303253808)
    - [Modello di progetto per dispositivi mobili](#_Toc303253809)
    - [Modalità di visualizzazione](#_Toc303253810)
    - [jQuery Mobile, la selezione tipo di visualizzazione e si esegue l'override di Browser](#_Toc303253811)
    - [Procedure per la generazione di codice in Visual Studio](#_Toc303253812)
    - [Attività di supporto per i controller asincroni](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Problemi noti e modifiche di rilievo](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Note sull'installazione

ASP.NET MVC 4 Beta per Visual Studio 2010 può essere installato mediante il [home page di ASP.NET MVC 4](../mvc/mvc4.md) utilizzando l'installazione guidata piattaforma Web.

È necessario disinstallare qualsiasi installato in precedenza le anteprime di ASP.NET MVC 4 prima di installare ASP.NET MVC 4 Beta.

Questa versione non è compatibile con l'anteprima per sviluppatori di .NET Framework 4.5. Prima di installare ASP.NET MVC 4 Beta, è necessario disinstallare l'anteprima per sviluppatori di .NET 4.5.

ASP.NET MVC 4 può essere installato ed è possibile eseguire side-by-side con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentazione

Documentazione per ASP.NET MVC è disponibile nel sito Web MSDN all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Esercitazioni e altre informazioni su ASP.NET MVC sono disponibili nella pagina del sito Web ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Supporto

Questa è una versione di anteprima e non è ufficialmente supportata. Se si hanno domande sull'utilizzo di questa versione, pubblicare un post nel forum di ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), in cui i membri della community ASP.NET sono spesso in grado di fornire supporto informale.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisiti software

I componenti di ASP.NET MVC 4 per Visual Studio richiedono PowerShell 2.0 e Microsoft Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Aggiornamento di un progetto ASP.NET MVC 3 ad ASP.NET MVC 4

ASP.NET MVC 4 può essere installato nello stesso computer, che offre una flessibilità nella scelta del momento di aggiornare un'applicazione ASP.NET MVC 3 ad ASP.NET MVC 4 in modo affiancato con ASP.NET MVC 3.

Il modo più semplice per eseguire l'aggiornamento è per creare un nuovo progetto ASP.NET MVC 4 e copiare tutte le visualizzazioni, controller, codice e i contenuti file dal progetto MVC 3 esistente al nuovo progetto e quindi aggiornare l'assembly fa riferimento nel nuovo progetto in base al progetto precedente. Se sono state apportate modifiche al file Web. config nel progetto MVC 3, è anche necessario unire le modifiche nel file Web. config nel progetto MVC 4.

Per aggiornare manualmente un'applicazione ASP.NET MVC 3 alla versione 4, eseguire le operazioni seguenti:

1. In tutti i file Web. config nel progetto (è presente nella radice del progetto, uno nella cartella Views e uno nella cartella Views per ogni area del progetto), sostituire ogni istanza del testo seguente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    con il testo corrispondente seguente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. Nel file Web. config radice, aggiornare il *webPages:Version* elemento "2.0.0.0" e aggiungere un nuovo *PreserveLoginUrl* chiave con il valore "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. In Esplora soluzioni, eliminare il riferimento a *System* (che punta alla versione DLL di 3). Quindi aggiungere un riferimento a *System* (v4.0.0.0). In particolare, apportare le modifiche seguenti per aggiornare i riferimenti all'assembly. Di seguito sono riportati i dettagli:

    1. In Esplora soluzioni, eliminare i riferimenti agli assembly seguenti: 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. Aggiungere un riferimento agli assembly seguenti: 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. In Esplora soluzioni fare doppio clic sul nome del progetto e scegliere Scarica progetto. Quindi fare di nuovo il nome e selezionare Modifica *ProjectName*con estensione csproj.
5. Individuare il *ProjectTypeGuids* elemento e sostituire {E53F8FEA-EAE0-44A6-8774-FFD645390401} con {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) che si stava modificando, fare clic sul progetto e quindi scegliere Ricarica progetto.
7. Se il progetto fa riferimento a tutte le librerie di terze parti che vengono compilate utilizzando versioni precedenti di ASP.NET MVC, aprire il file Web. config radice e aggiungere i seguenti tre *bindingRedirect* elementi sotto il  *configurazione* sezione: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nuove funzionalità in ASP.NET MVC 4 Beta

Questa sezione vengono descritte le funzionalità che sono state introdotte nella versione ASP.NET MVC 4 Beta.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API Web ASP.NET

ASP.NET MVC 4 include ora l'API Web ASP.NET, un nuovo framework per la creazione di servizi HTTP che può raggiungere una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è una piattaforma ideale per la creazione di servizi RESTful.

ASP.NET Web API include il supporto per le funzionalità seguenti:

- **Modello di programmazione HTTP moderna:** direttamente accedere e modificare le richieste HTTP e le risposte alle API Web usando un modello a oggetti fortemente tipizzato nuovo HTTP. Lo stesso modello di programmazione e pipeline HTTP è disponibile in modo simmetrico nel client tramite il nuovo tipo HttpClient.
- **Supporto completo per le route**: le API Web supportano ora il set completo di funzionalità di route che sono stati sempre una parte dello stack Web, inclusi i parametri di route e vincoli. Mapping di azioni presenta inoltre il supporto completo per informazioni sulle convenzioni, in modo non è più necessario applicare gli attributi, ad esempio [HttpPost] alle classi e metodi.
- **Negoziazione di contenuto**: il client e server possono essere usati insieme per determinare il formato corretto per la restituzione da un'API dei dati. Si forniscono il supporto predefinito per XML, JSON e formati con codifica URL Form e, è possibile estendere questo supporto mediante l'aggiunta di formattatori personalizzati o anche sostituire la strategia di negoziazione di contenuto predefinito.
- **Associazione e la convalida del modello:** raccoglitori offrono un modo semplice per estrarre dati da varie parti di una richiesta HTTP e convertire le parti del messaggio in oggetti .NET che possono essere utilizzati dalle azioni di API Web.
- **Filtri:** API Web supporta ora i filtri, inclusi i filtri noti, ad esempio l'attributo [Authorize]. È possibile creare e collegare i propri filtri per le azioni, autorizzazione e la gestione delle eccezioni.
- **Composizione della query:** restituendo semplicemente IQueryable&lt;T&gt;, l'API Web supporterà l'esecuzione di query tramite le convenzioni degli URL OData.
- **Migliorare la testabilità dei dettagli HTTP:** anziché impostare i dettagli HTTP negli oggetti di contesto statico, è ora possono utilizzare le azioni di API Web con le istanze di HttpRequestMessage e HttpResponseMessage. Versioni generiche di questi oggetti esistono anche per consentire di utilizzare i tipi personalizzati oltre ai tipi di HTTP.
- **Migliorate Inversion of Control (IoC) tramite DependencyResolver:** API Web Usa ora il modello di localizzatore servizio implementato da resolver di dipendenza di MVC per ottenere le istanze per molte funzionalità differenti.
- **Configurazione basata su codice:** configurazione Web API viene eseguita esclusivamente tramite codice, lasciando il file di configurazione pulire i file.
- **Servizio indipendente:** API Web possono essere ospitate in un proprio processo oltre a IIS pur continuando a utilizzare le potenzialità di route e altre funzionalità dell'API Web.

Per ulteriori informazioni su ASP.NET Web API, visitare [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Applicazione a pagina singola di ASP.NET

ASP.NET MVC 4 include ora un'anteprima dell'esperienza per la compilazione di applicazioni a pagina singola con significative interazioni sul lato client utilizzando JavaScript e l'API Web. Questo supporto include:

- Un set di librerie JavaScript per maggiore interazione locale con i dati memorizzati nella cache
- Componenti aggiuntivi di API Web per unità di lavoro e il supporto DAL
- Un modello di progetto MVC con scaffolding per iniziare rapidamente

Per ulteriori dettagli sull'applicazione di pagina singolo supportano in ASP.NET MVC 4, visitare [ https://www.asp.net/single-page-application ](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Miglioramenti apportati a modelli di progetto predefiniti

Il modello utilizzato per creare nuovi progetti ASP.NET MVC 4 è stato aggiornato per creare un sito Web dall'aspetto moderno più:

![](mvc4-beta-release-notes/_static/image1.png)

Oltre ai miglioramenti cosmetici, ha migliorato la funzionalità nel nuovo modello. Il modello utilizza una tecnica denominata per il rendering adattivo per essere utilizzati sia nei browser desktop e browser per dispositivi mobili senza personalizzazione.

![](mvc4-beta-release-notes/_static/image2.png)

Per visualizzare il rendering adattivo in azione, è possibile utilizzare un emulatore di dispositivi mobili o semplicemente provare a ridimensionare la finestra del browser desktop per essere più piccoli. Quando la finestra del browser ottiene sufficientemente piccola, viene modificato il layout della pagina.

Un altro miglioramento per il modello di progetto predefinito è l'utilizzo di JavaScript per fornire un'interfaccia utente più dettagliata. I collegamenti di accesso e registrazione utilizzate nel modello sono riportati esempi di come utilizzare la finestra di dialogo dell'interfaccia utente di jQuery per presentare una schermata di accesso avanzato:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modello di progetto per dispositivi mobili

Se si inizia un nuovo progetto e si desidera creare un sito in modo specifico per dispositivi mobili e tablet browser, è possibile utilizzare il nuovo modello di progetto di applicazione per dispositivi mobili. Si basa su jQuery Mobile, una libreria open source per la compilazione ottimizzata per il tocco dell'interfaccia utente:

![](mvc4-beta-release-notes/_static/image4.png)

Questo modello contiene la stessa struttura dell'applicazione come modello di applicazione Internet (e il codice del controller è praticamente identico), ma è applicato lo stile tramite jQuery Mobile per l'aspetto e il comportamento anche su dispositivi mobili basati su tocco. Per ulteriori informazioni sull'organizzazione e lo stile dell'interfaccia utente per dispositivi mobili, vedere il [sito Web del progetto per dispositivi mobili jQuery](http://jquerymobile.com/).

Se si dispone già di un sito basata su desktop che si desidera aggiungere viste ottimizzata, o se si desidera creare un singolo sito che viene utilizzato uno stile differente viste per i browser desktop e mobile, è possibile utilizzare la nuova funzionalità di modalità di visualizzazione. (Vedere la sezione successiva).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modalità di visualizzazione

La nuova funzionalità di modalità di visualizzazione consente a un'applicazione di selezionare le viste in base al browser che effettua la richiesta. Se, ad esempio, un browser desktop richiede la Home page, l'applicazione potrebbe utilizzare il modello di Views\Home\Index.cshtml. Se un browser per dispositivi mobili richiede la Home page, l'applicazione potrebbe restituire il modello Views\Home\Index.mobile.cshtml.

Layout e parziali possono essere sostituiti anche per i tipi di browser specifico. Ad esempio:

- Se la cartella Views\Shared contiene sia il \_cshtml e \_Layout.mobile.cshtml modelli, per impostazione predefinita, l'applicazione utilizzerà \_Layout.mobile.cshtml durante le richieste dal browser per dispositivi mobili e \_Cshtml durante le altre richieste.
- Se una cartella contiene sia \_MyPartial.cshtml e \_MyPartial.mobile.cshtml, l'istruzione @Html.Partial("\_MyPartial") verrà eseguito il rendering \_MyPartial.mobile.cshtml durante le richieste provenienti da dispositivi mobili browser, e \_MyPartial.cshtml durante le altre richieste.

Se si desidera creare visualizzazioni più specifiche, layout o visualizzazioni parziali per altri dispositivi, è possibile registrare un nuovo *DefaultDisplayMode* istanza per specificare il nome per la ricerca quando una richiesta soddisfa le condizioni particolari. Ad esempio, è possibile aggiungere il codice seguente per il *applicazione\_avviare* metodo nel file Global. asax per registrare la stringa "iPhone" come modalità di visualizzazione che si applica quando il browser Apple iPhone che effettua una richiesta:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Quando si esegue questo codice, quando un browser Apple iPhone che effettua una richiesta, l'applicazione utilizzerà il Views\Shared\\_Layout.iPhone.cshtml layout (se presente).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, la selezione tipo di visualizzazione e si esegue l'override di Browser

jQuery Mobile è una libreria open source per la compilazione di interfaccia utente web ottimizzata per il tocco. Se si desidera utilizzare jQuery Mobile con un'applicazione ASP.NET MVC 4, è possibile scaricare e installare un pacchetto NuGet che consente di iniziare. Per installarlo dalla Console di gestione di pacchetti di Visual Studio, digitare il comando seguente:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Vengono installati jQuery Mobile e alcuni file di supporto, inclusi i seguenti:

- Views/Shared/\_Layout.Mobile.cshtml, ovvero un layout per jQuery Mobile.
- Un componente Selezione tipo di visualizzazione, che è costituito ilViews/Shared/\_visualizzazione parziale ViewSwitcher.cshtml e il controller ViewSwitcherController.cs.

Dopo aver installato il pacchetto, eseguire l'applicazione utilizzando un browser per dispositivi mobili (o equivalente, quali il Firefox [selezione del tipo di agente utente](http://chrispederick.com/work/user-agent-switcher/) componente aggiuntivo). Si noterà che le pagine apparire piuttosto diverse, poiché jQuery Mobile gestisce layout e stile. Per sfruttare i vantaggi di questo, è possibile eseguire le operazioni seguenti:

- Creare sostituzioni visualizzazione mobile specifica, come descritto in [modalità di visualizzazione](#_Toc303253810) in precedenza (ad esempio, creare Views\Home\Index.mobile.cshtml per eseguire l'override di Views\Home\Index.cshtml per browser per dispositivi mobili).
- Lettura di [jQuery Mobile documentazione](http://jquerymobile.com/) per ulteriori informazioni su come aggiungere gli elementi dell'interfaccia utente ottimizzata per il tocco nelle visualizzazioni per dispositivi mobili.

Una convenzione per dispositivi mobili con ottimizzazione per le pagine web consiste nell'aggiungere un collegamento il cui testo è un elemento come visualizzazione Desktop o modalità completa del sito che consente agli utenti di passare a una versione desktop della pagina. Il pacchetto jQuery.Mobile.MVC include un esempio di componente di selezione di visualizzazione per questo scopo. Usarlo per il valore predefinito Views\Shared\\_Layout.Mobile.cshtml visualizzazione cui aspetto è simile al seguente quando viene eseguito il rendering della pagina:

![](mvc4-beta-release-notes/_static/image5.png)

Se i visitatori fare clic sul collegamento, che si passasse alla versione desktop della stessa pagina.

Poiché il layout del desktop non includerà una selezione di visualizzazione per impostazione predefinita, i visitatori non disporrà di un modo per ottenere la modalità per dispositivi mobili. A tale scopo, aggiungere il seguente riferimento a  *\_ViewSwitcher* al layout del desktop, solo all'interno di *&lt;corpo&gt;* elemento:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

La selezione della vista utilizza una nuova funzionalità denominata eseguendo l'override del Browser. Questa funzionalità consente all'applicazione di considerare le richieste come provenienti da un altro browser (agente utente) rispetto a quello sta effettivamente da. Nella tabella seguente sono elencati i metodi che fornisce l'override del Browser.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Esegue l'override di valore dell'agente utente effettivo della richiesta utilizzando l'agente utente specificato. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Restituisce il valore di sostituzione dell'agente utente della richiesta o la stringa agente utente effettivo se non è stato specificato alcun override. |
| `HttpContext.GetOverriddenBrowser()` | Restituisce un *HttpBrowserCapabilitiesBase* istanza che corrisponde all'agente utente attualmente impostata per la richiesta (effettivo o sottoposto a override). È possibile utilizzare questo valore per ottenere le proprietà, ad esempio *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Rimuove qualsiasi agente utente sottoposto a override per la richiesta corrente. |

Sostituzione di browser è una funzionalità centrale di ASP.NET MVC 4 ed è disponibile anche se non si installa il pacchetto jQuery.Mobile.MVC. Tuttavia, interessa solo visualizzazione, layout e la selezione della visualizzazione parziale, non influisce sulla qualsiasi altra funzionalità ASP.NET che varia a seconda di *Request* oggetto.

Per impostazione predefinita, la sostituzione dell'agente utente viene archiviata utilizzando un cookie. Se si desidera archiviare la sostituzione in un' posizione (ad esempio, in un database), è possibile sostituire il provider predefinito (*BrowserOverrideStores.Current*). Documentazione per il provider sarà disponibile a complemento di una versione successiva di ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Procedure per la generazione di codice in Visual Studio

La nuova funzionalità ricette consente a Visual Studio generare codice specifici della soluzione in base a pacchetti che è possibile installare mediante NuGet. Il framework ricette rende più semplice per gli sviluppatori di scrivere plug-in generazione di codice, è inoltre possibile utilizzare per sostituire i generatori di codice incorporato per Aggiungi visualizzazione, Aggiungi Area e Aggiungi Controller. Poiché le recipe vengono distribuite come pacchetti NuGet, possono essere archiviati nel controllo del codice sorgente e automaticamente condivisi con tutti gli sviluppatori per il progetto. Sono anche disponibili in base per ogni soluzione.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Attività di supporto per i controller asincroni

Ora è possibile scrivere metodi di azione asincroni come un'unica i metodi che restituiscono un oggetto di tipo *attività* o *attività&lt;ActionResult&gt;*.

Ad esempio, se si utilizza Visual c# 5 (o tramite il [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), è possibile creare un metodo di azione asincrono che è simile alla seguente:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

Nel metodo di azione precedente, le chiamate a *GetHeadlinesAsync* e *sportsService.GetScoresAsync* vengono chiamati in modo asincrono e blocca un thread dal pool di thread.

Metodi di azione asincroni che restituiscono *attività* istanze possono inoltre supportare i timeout. Per rendere il metodo di azione annullabile, aggiungere un parametro di tipo *CancellationToken* alla firma del metodo di azione. Nell'esempio seguente viene illustrato un metodo di azione asincrono che dispone di un timeout di 2500 millisecondi e che consente di visualizzare un *TimedOut* visualizzare al client se si verifica un timeout.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta supporta la versione di settembre 2011 1.5 di Windows Azure SDK.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

- **Dopo l'installazione di ASP.NET MVC 4 Beta, l'editor CSHTML o VBHTML nell'editor di Visual Studio 2010 Service Pack 1 CSHTML o VBHTML può bloccarsi a lungo dopo aver digitato frammento o JavaScript all'interno di file cshtml o vbhtml.** Questo errore si verifica solo in applicazioni ASP.NET MVC 4 che appena create e non sono ancora state compilate.

    La soluzione alternativa consiste nel compilare il progetto per ottenere gli assembly nella cartella bin. Si noti che se si esegue la pulitura di progetto che consente di rimuovere gli assembly dalla cartella bin, il problema di editor, verrà ripristinato.

    Questo verrà corretto nella versione successiva.
- **I modelli di progetto c# per Visual Studio 11 Beta contengono una stringa di connessione non corrette in Global.asax.cs.** La connessione predefinita specificata nell'applicazione\_Start (metodo) per i progetti creati in Visual Studio 11 Beta contengono una stringa di connessione del database locale che contiene una barra rovesciata senza escape (\) carattere. Ciò comporta un errore di connessione durante il tentativo di accedere a un elemento DbContext Framework entità, che genera un SqlException.

    Per risolvere questo problema, escape del carattere di barra rovesciata nell'App\_Start (metodo) di Global.asax.cs perché possa essere letto come segue:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Le applicazioni ASP.NET MVC 4 che usano come destinazione .NET 4.5, verranno generata una FileLoadException durante il tentativo di accedere all'assembly System.Net.Http.dll quando viene eseguito in .NET 4.0.** Applicazioni ASP.NET MVC 4 create in .NET 4.5 contengono un'associazione reindirizzamento che comporta un FileLoadException che informa che "Impossibile caricare file o l'assembly 'System.NET. http' o una delle sue dipendenze." Quando l'applicazione viene eseguita in un sistema con .NET 4.0 installato. Per risolvere questo problema, rimuovere il seguente reindirizzamento dell'associazione da Web. config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    L'elemento di associazione di assembly in Web. config modificato compariranno come indicato di seguito:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Il modello di elemento "Aggiungi Controller" nei progetti Visual Basic genera uno spazio dei nomi non corretto quando viene richiamato</strong><strong>all'interno di un'area.</strong> Quando si aggiunge un controller in un'area in un progetto ASP.NET MVC che utilizza Visual Basic, lo spazio dei nomi errato il modello di elemento inserisce il controller. Il risultato è un errore di "file non trovato" quando si passa a qualsiasi azione nel controller.  
  
  Lo spazio dei nomi generato omette tutto dopo lo spazio dei nomi radice. Ad esempio, lo spazio dei nomi generato è *RootNamespace* , ma deve essere *RootNamespace.Areas.AreaName.Controllers* .
- **Modifiche di rilievo nel motore di visualizzazione Razor.** Come parte di una riscrittura del parser Razor, i tipi seguenti sono stati rimossi da *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  I metodi seguenti sono state inoltre rimosse: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Quando WebMatrix.WebData.dll è incluso nella directory /bin di un'App ASP.NET MVC 4, vengono visualizzate sull'URL per l'autenticazione basata su form.** Aggiunta dell'assembly WebMatrix.WebData.dll all'applicazione (ad esempio, selezionando "Pagine Web ASP.NET con la sintassi delle Razor" quando si utilizza la finestra di dialogo Aggiungi dipendenze distribuibili) sostituiranno il reindirizzamento di autenticazione account di accesso per l'accesso/account/anziché / account/account di accesso come previsto, il valore predefinito di ASP.NET MVC Controller di Account. Per evitare questo comportamento e usare l'URL specificato già nella sezione autenticazione di Web. config, è possibile aggiungere un appSetting chiamato PreserveLoginUrl e impostarlo su true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Gestione pacchetti NuGet non viene installato quando si prova a installare ASP.NET MVC 4 per le installazioni affiancate di Visual Studio 2010 e Visual Web Developer 2010.** Per eseguire Visual Studio 2010 e Visual Web Developer 2010 in modo affiancato con ASP.NET MVC 4 è necessario installare ASP.NET MVC 4 dopo entrambe le versioni di Visual Studio sono già installate.
- **Disinstallazione di ASP.NET MVC 4 ha esito negativo se i prerequisiti sono già stati disinstallati.** Per disinstallare correttamente ASP.NET MVC 4you necessario disinstallare ASP.NET MVC 4 prima di disinstallare Visual Studio.
- **Esecuzione di un progetto di API Web predefinita vengono visualizzate le istruzioni che indirizzare in modo non corretto all'utente di aggiungere le route utilizzando il metodo RegisterApis, che non esiste.** Le route devono essere aggiunto nel metodo RegisterRoutes utilizzando la tabella di route ASP.NET.
- **Installazione di ASP.NET MVC 4 Beta interrompe le applicazioni ASP.NET MVC 3 RTM.** Le applicazioni ASP.NET MVC 3 che sono state create con la versione RTM versione (non con la versione di ASP.NET MVC 3 Tools Update) le seguenti modifiche per funzionare richiedono side-by-side con ASP.NET MVC 4 Beta. Compilazione del progetto senza apportare questi risultati gli aggiornamenti in errori di compilazione. 

    **Aggiornamenti necessari**

  1. Nel file Web. config radice, aggiungere un nuovo *&lt;appSettings&gt;* voce con la chiave *webPages:Version* e il valore *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. In Esplora soluzioni fare doppio clic sul nome del progetto e scegliere Scarica progetto. Quindi fare di nuovo il nome e selezionare Modifica *ProjectName*con estensione csproj.
  3. Individuare i riferimenti agli assembly seguenti: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Sostituirli con gli elementi seguenti:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) la modifica, quindi fare clic sul progetto e scegliere Ricarica.
