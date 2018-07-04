---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive la versione di ASP.NET MVC 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: ''
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: f86220d296e0058b0d482cef066c33e5474d51ef
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369457"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Questo documento descrive la versione di ASP.NET MVC 4.


- [Note sull'installazione](#_Toc303253802)
- [Documentazione](#_Toc303253803)
- [Supporto](#_Toc303253804)
- [Requisiti software](#_Toc303253805)
- [Nuove funzionalità in ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Miglioramenti ai modelli di progetto predefinito](#_Toc303253808)
    - [Modello di progetto per dispositivi mobili](#_Toc303253809)
    - [Modalità di visualizzazione](#_Toc303253810)
    - [jQuery Mobile, la selezione tipo di visualizzazione e si esegue l'override del Browser](#_Toc303253811)
    - [Supporto di attività per i controller asincroni](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Migrazioni del database](#_Toc303253818)
    - [Modello di progetto vuoto](#_Toc303253819)
    - [Aggiungere Controller per qualsiasi cartella del progetto](#_Toc303253820)
    - [Creazione di bundle e minimizzazione](#_Toc303253821)
    - [Abilitare gli account di accesso di Facebook e altri siti usando OAuth e OpenID](#_Toc303253822)
- [L'aggiornamento di un progetto ASP.NET MVC 3 ad ASP.NET MVC 4](#_Toc303253806)
- [Modifiche dalla versione finale candidata di ASP.NET MVC 4](#_Toc303253817)
- [Problemi noti e modifiche di rilievo](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Note sull'installazione

ASP.NET MVC 4 per Visual Studio 2010 possono essere installate dal [home page di ASP.NET MVC 4](../mvc/mvc4.md) usando l'installazione guidata piattaforma Web.

È consigliabile disinstallare qualsiasi installati in precedenza le anteprime di ASP.NET MVC 4 prima di installare ASP.NET MVC 4. È possibile eseguire l'aggiornamento di ASP.NET MVC 4 Beta e Release Candidate ad ASP.NET MVC 4 senza disinstallare.

Questa versione non è compatibile con le versioni di anteprima di .NET Framework 4.5. Separatamente, è necessario aggiornare tutte le versioni di anteprima installata di .NET Framework 4.5 con la versione finale prima di installare ASP.NET MVC 4.

ASP.NET MVC 4 può essere installato ed eseguito side-by-side con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentazione

Documentazione per ASP.NET MVC è disponibile nel sito Web MSDN all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Esercitazioni e altre informazioni su ASP.NET MVC sono disponibili nella pagina del sito Web ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Supporto

ASP.NET MVC 4 è completamente supportato. Se hai domande sull'uso di questa versione è anche possibile registrarli al forum su ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), in cui i membri della community di ASP.NET sono spesso in grado di fornire supporto informale.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisiti software

I componenti ASP.NET MVC 4 per Visual Studio richiedono PowerShell 2.0 e Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nuove funzionalità in ASP.NET MVC 4

Questa sezione vengono descritte le funzionalità che sono state introdotte nella versione ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API Web ASP.NET

ASP.NET MVC 4 include API Web ASP.NET, un nuovo framework per la creazione di servizi HTTP che possono raggiungere una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è anche una piattaforma ideale per creare servizi RESTful.

API Web ASP.NET include il supporto per le funzionalità seguenti:

- **Modello di programmazione HTTP moderni:** direttamente accedere e modificare le richieste HTTP e risposte in API Web usando un modello a oggetti fortemente tipizzato, nuovo HTTP. Lo stesso modello di programmazione e della pipeline HTTP è disponibile in modo simmetrico nel client tramite il nuovo *HttpClient* tipo.
- **Supporto completo per le route:** API Web ASP.NET supporta il set completo di funzionalità di route del Routing di ASP.NET, tra cui i parametri di route e vincoli. Inoltre, utilizzare le convenzioni di semplice per eseguire il mapping di azioni a metodi HTTP.
- **Negoziazione del contenuto:** client e server possono lavorare insieme per determinare il formato corretto per i dati restituiti da un'API web. API Web ASP.NET include il supporto predefinito per XML, JSON, e formati di codifica URL Form ed è possibile estendere questo supporto mediante l'aggiunta di formattatori personalizzati, o anche sostituire la strategia di negoziazione del contenuto predefinita.
- **Associazione e la convalida del modello:** raccoglitori offrono un modo semplice per estrarre dati da varie parti di una richiesta HTTP e convertire le parti del messaggio in oggetti .NET che possono essere utilizzati per le azioni di API Web. La convalida viene eseguita anche in base alle annotazioni dei dati di parametri di azione.
- **Filtri:** API Web ASP.NET supporta filtri, inclusi filtri ben noti, ad esempio il *[Authorize]* attributo. È possibile creare e collegare i propri filtri per azioni, autorizzazione e la gestione delle eccezioni.
- **Composizione della query:** usare il *[Queryable]* attributo di filtro su un'azione che restituisce *IQueryable* per abilitare il supporto per l'esecuzione di query dell'API web tramite le convenzioni di query OData.
- **Migliorare la testabilità:** anziché impostare i dettagli HTTP in oggetti di contesto statico, web lavoro le azioni di API con le istanze di *HttpRequestMessage* e *HttpResponseMessage*. Creare un progetto di unit test con il progetto API Web per iniziare a usare rapidamente la scrittura di unit test per le funzionalità dell'API Web.
- **Configurazione basata su codice:** configurazione ASP.NET Web API avviene esclusivamente tramite il codice, uscire dal file di configurazione del file clean. Utilizzare il modello di localizzazione servizi forniti per configurare i punti di estendibilità.
- **Supporto migliorato per i contenitori di inversione del controllo (IoC):** API Web ASP.NET offre il supporto per contenitori IoC tramite un'astrazione di resolver di dipendenza migliorata
- **Self-hosting:** API Web possono essere ospitate nel proprio processo oltre a IIS continuando comunque a utilizzare tutta la potenza di route e altre funzionalità dell'API Web.
- **Creare la Guida personalizzata e testare le pagine:** è ora possibile facilmente Guida personalizzata di compilazione e test delle pagine per le API web usando il nuovo *IApiExplorer* servizio per ottenere una descrizione completa di runtime della propria API web.
- **Monitoraggio e diagnostica:** API Web ASP.NET ora fornisce l'infrastruttura di traccia leggero che semplifica l'integrazione con soluzioni di registrazione esistente, ad esempio System. Diagnostics, ETW e Framework di registrazione di terze parti. È possibile abilitare la traccia, fornendo un' *ITraceWriter* implementazione e aggiungerlo alla configurazione web API.
- **Generazione del collegamento:** usare l'API Web ASP.NET *UrlHelper* per generare i collegamenti a risorse correlate nella stessa applicazione.
- **Modello di progetto API Web:** selezionare il nuovo form del progetto API Web la procedura guidata nuovo progetto MVC 4 per rendersi attivi rapidamente e in esecuzione con l'API Web ASP.NET.
- **Lo scaffolding:** usare il **Aggiungi Controller** tipo di modello basato su finestra di dialogo per eseguire rapidamente lo scaffolding di un controller API web basato su un Entity Framework.

Per altre informazioni su API Web ASP.NET, visitare [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Miglioramenti ai modelli di progetto predefinito

Il modello utilizzato per creare nuovi progetti ASP.NET MVC 4 è stato aggiornato per creare un sito Web più dall'aspetto moderno:

![](mvc4-release-notes/_static/image1.png)

Oltre ai miglioramenti cosmetici, ha migliorato la funzionalità nel nuovo modello. Il modello Usa una tecnica denominata rendering adattivo una buona cosa sia nei browser desktop e browser per dispositivi mobili senza eseguire alcuna personalizzazione.

![](mvc4-release-notes/_static/image2.png)

Per visualizzare il rendering adattivo in azione, è possibile usare un emulatore per dispositivi mobili o semplicemente provare a ridimensionare la finestra del browser desktop siano più piccoli. Quando la finestra del browser ottiene sufficientemente piccola, si passerà il layout della pagina.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modello di progetto per dispositivi mobili

Se si inizia un nuovo progetto e si desidera creare un sito in modo specifico per dispositivi mobili e browser tablet, è possibile usare il nuovo modello di progetto di applicazione per dispositivi mobili. Si basa sulle jQuery Mobile, una libreria open source per la creazione dell'interfaccia utente ottimizzato per il tocco:

![](mvc4-release-notes/_static/image3.png)

Questo modello contiene la stessa struttura dell'applicazione come modello di applicazione Internet (e il codice del controller è praticamente identico), ma è applicato lo stile tramite jQuery Mobile per l'aspetto e comportamento correttamente sui dispositivi mobili basati su touch. Per altre informazioni su come strutturare e definire lo stile dell'interfaccia utente per dispositivi mobili, vedere la [sito Web del progetto per dispositivi mobili jQuery](http://jquerymobile.com/).

Se si dispone già di un sito basata su desktop che si desidera aggiungere visualizzazioni ottimizzate per dispositivi mobili, o se si desidera creare un singolo sito che viene usato in modo diverso con stile viste nei browser desktop e per dispositivi mobili, è possibile usare la nuova funzionalità di modalità di visualizzazione. (Vedere la sezione successiva).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modalità di visualizzazione

La nuova funzionalità di modalità di visualizzazione consente a un'applicazione di selezionare le viste in base al browser che effettua la richiesta. Ad esempio, se il browser desktop richiede la Home page, l'applicazione potrebbe utilizzare il modello Views\Home\Index.cshtml. Se un browser per dispositivi mobili richiede la Home page, l'applicazione potrebbe restituire il modello Views\Home\Index.mobile.cshtml.

Layout e visualizzazioni parziali possono anche eseguire l'override per i tipi di browser specifico. Ad esempio:

- Se la cartella Views\Shared contiene entrambi le \_layout. cshtml e \_cshtml modelli, per impostazione predefinita l'applicazione utilizzerà \_cshtml durante le richieste dal browser per dispositivi mobili e \_Layout. cshtml durante le altre richieste.
- Se una cartella contiene entrambe \_MyPartial.cshtml e \_MyPartial.mobile.cshtml, l'istruzione @Html.Partial("\_MyPartial") verrà eseguito il rendering \_MyPartial.mobile.cshtml durante le richieste provenienti da dispositivi mobili i browser, e \_MyPartial.cshtml durante le altre richieste.

Se si desidera creare più specifiche viste, layout o visualizzazioni parziali per altri dispositivi, è possibile registrare una nuova *DefaultDisplayMode* istanza per specificare il nome da cercare quando una richiesta soddisfa determinate condizioni. Ad esempio, è possibile aggiungere il codice seguente per il *Application\_avviare* metodo nel file Global. asax per registrare la stringa "iPhone" come modalità di visualizzazione che si applica quando il browser di iPhone Apple effettua una richiesta:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Quando si esegue questo codice, quando un browser di iPhone Apple effettua una richiesta, l'applicazione userà il Views\Shared\\_Layout.iPhone.cshtml layout (se presente). Per ulteriori informazioni sulla modalità di visualizzazione, vedere [le funzionalità per dispositivi mobili di ASP.NET MVC 4](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Le applicazioni usando DisplayModeProvider devono installare il [DisplayModes fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacchetto NuGet. Il [ASP.NET autunno 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322) include le [DisplayModes fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacchetto NuGet in nuovi modelli di progetto. Visualizzare [ASP.NET MVC 4 per dispositivi mobili la memorizzazione nella cache Bug Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) per i dettagli della correzione.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile e le funzionalità per dispositivi mobili

Per informazioni sulla compilazione di applicazioni per dispositivi mobili con ASP.NET MVC 4 con jQuery Mobile, vedere l'esercitazione [le funzionalità per dispositivi mobili di ASP.NET MVC 4](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Supporto di attività per i controller asincroni

È ora possibile scrivere metodi di azione asincroni come singolo i metodi che restituiscono un oggetto di tipo *Task* oppure *Task&lt;ActionResult&gt;*.

 Per altre informazioni, vedere [usando i metodi asincroni in ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 supporta le versioni 1.6 e versioni successive di Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migrazioni del database

I progetti ASP.NET MVC 4 ora includono Entity Framework 5. Una delle eccezionali funzionalità di Entity Framework 5 è il supporto per le migrazioni del database. Questa funzionalità consente facile evoluzione dello schema del database utilizzando una migrazione incentrato sul codice mantenendo i dati nel database. Per altre informazioni sulle migrazioni di database, vedere [aggiunge un nuovo campo al modello di filmato e tabella](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) nel [Introduzione a ASP.NET MVC 4 esercitazione](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Modello di progetto vuoto

Il modello di progetto MVC vuoto a questo punto è realmente vuoto in modo che è possibile avviare da uno slate pulito completamente. La versione precedente del modello di progetto vuoto è stata rinominata al livello Basic.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Aggiungere Controller per qualsiasi cartella del progetto

È ora possibile fare clic e selezionare **Aggiungi Controller** da qualsiasi cartella del progetto MVC. Ciò offre maggiore flessibilità per organizzare i controller che preferisci, tra cui mantenere i controller MVC e API Web in cartelle separate.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Creazione di bundle e minimizzazione

Il framework di creazione di bundle e minimizzazione consente di ridurre il numero di richieste HTTP da una pagina Web deve rendere combinando i singoli file in un file in dotazione solo per gli script e CSS. Quindi possibile ridurre la dimensione complessiva di tali richieste da minimizzazione i contenuti del bundle. Minimizzazione possono includere le attività tra cui l'eliminazione di spazi vuoti per abbreviare i nomi delle variabili a anche dei selettori CSS basati presentano una semantica di compressione. Aggregazioni vengono dichiarate e configurate nel codice e sono facilmente riferimenti nelle visualizzazioni tramite i metodi helper che possono generare una un singolo collegamento per l'aggregazione o, durante il debug, più i collegamenti al contenuto del bundle singoli. Per altre informazioni, vedere [Bundling and Minification](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Abilitare gli account di accesso di Facebook e altri siti usando OAuth e OpenID

I modelli predefiniti nel modello di progetto ASP.NET MVC 4 Internet include ora il supporto per l'accesso di OAuth e OpenID usando la libreria DotNetOpenAuth. Per informazioni sulla configurazione di un provider OAuth o OpenID, vedere [supporto di OAuth/OpenID per Web Form, MVC e pagine Web](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) e il [OAuth e OpenID funzionalità documentazione in ASP.NET Web Pages](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>L'aggiornamento di un progetto ASP.NET MVC 3 ad ASP.NET MVC 4

ASP.NET MVC 4 può essere installato nello stesso computer, che offre flessibilità nella scelta del momento aggiornare un'applicazione ASP.NET MVC 3 ad ASP.NET MVC 4 affiancato con ASP.NET MVC 3.

Il modo più semplice per eseguire l'aggiornamento è per creare un nuovo progetto ASP.NET MVC 4 e copiare tutte le visualizzazioni, controller, codice e i contenuti i file dal progetto MVC 3 esistente al nuovo progetto e quindi aggiornare l'assembly fa riferimento nel nuovo progetto per la corrispondenza di qualsiasi modello disponibile non MVC in assiemi cluded in uso. Se sono state apportate modifiche al file Web. config del progetto MVC 3, è anche necessario unire queste modifiche nel file Web. config nel progetto MVC 4.

Per aggiornare manualmente un'applicazione ASP.NET MVC 3 esistente alla versione 4, eseguire le operazioni seguenti:

1. Nel file Web. config tutti i file del progetto (è disponibile nella radice del progetto, uno nella cartella Views e uno nella cartella Views di ciascuna area nel progetto), sostituire ogni istanza del testo seguente (Nota: System.Web.WebPages, Version = 1.0.0.0 non viene trovato progetti creati con Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    con il testo corrispondente:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. Nel file Web. config radice, aggiornare il *webPages:Version* elemento "2.0.0.0" e aggiungere un nuovo *PreserveLoginUrl* chiave contenente il valore "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. In Esplora soluzioni fare clic su riferimenti e scegliere Gestisci pacchetti NuGet. Nel riquadro sinistro, selezionare **origine pacchetto ufficiale Online\NuGet**, quindi aggiornare le seguenti:

    - ASP.NET MVC 4
    - (Facoltativo) jQuery, jQuery convalida e interfaccia utente di jQuery
    - (Facoltativo) Entity Framework
    - (Optonal) Modernizr
4. In Esplora soluzioni fare clic sul nome del progetto e quindi Scarica progetto. Quindi fare doppio clic il nome del nuovo e selezionare Modifica *ProjectName*csproj.
5. Individuare il *ProjectTypeGuids* elemento e sostituire {E53F8FEA-EAE0-44A6-8774-FFD645390401} con {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) che si stava modificando, fare clic sul progetto e quindi scegliere Ricarica progetto.
7. Se il progetto fa riferimento a tutte le librerie di terze parti che vengono compilate usando versioni precedenti di ASP.NET MVC, aprire il file Web. config radice e aggiungere le tre *bindingRedirect* elementi sotto il  *configurazione* sezione: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Modifiche dalla versione finale candidata di ASP.NET MVC 4

Le note sulla versione per ASP.NET MVC 4 Release Candidate sono disponibili qui:

Le modifiche principali da ASP.NET MVC 4 Release Candidate in questa versione sono riepilogate di seguito:

- **Per ogni configurazione del controller:** controller API Web ASP.NET può essere attribuito con un attributo personalizzato che implementa *IControllerConfiguration* per configurare le proprie formattatori, selettore di azioni e gli strumenti di associazione di parametro . Il *HttpControllerConfigurationAttribute* è stata rimossa.
- **Per i gestori di messaggi di route:** è ora possibile specificare il gestore di messaggio finale nella catena di richiesta per una route specificata. Ciò consente il supporto per i framework rani utili usare il routing per inviare i propri (non -*IHttpController*) degli endpoint.
- **Le notifiche di stato:** il *ProgressMessageHandler* genera notifiche di stato di avanzamento per entrambe le entità di richiesta in fase di caricamento e le entità di risposta in corso il download. Tramite questo gestore di è possibile tenere traccia di fino a quando si un corpo della richiesta di caricamento o download di un corpo della risposta.
- **Eseguire il push del contenuto:** il *PushStreamContent* classe rende possibili scenari in cui un producer di dati deve scrivere direttamente la richiesta o risposta (in modo sincrono o asincrono) utilizzando un flusso. Quando la *PushStreamContent* è pronto per accettare i dati effettua una chiamata a un delegato di azione con il flusso di output. Lo sviluppatore può quindi scrivere nel flusso per fino a quando è necessario e Chiudi è stato completato il flusso durante la scrittura. Il *PushStreamContent* rileva la chiusura del flusso e completa sottostante asincrona *attività* per la scrittura del contenuto.
- **Creazione di risposte di errore:** usare il *errore HTTP* tipo per rappresentare in modo coerente le informazioni di errore da, ad esempio errori di convalida e le eccezioni continuando a rispettare il *IncludeErrorDetailPolicy*. Usare le nuove *CreateErrorResponse* per creare facilmente le risposte di errore con i metodi di estensione *HttpError* come contenuto. Il *HttpError* contenuto non è completamente contenuto negoziato.
- **MediaRangeMapping rimosso:** intervalli dei tipi di supporti vengono ora gestiti da negoziatore del contenuto predefinito.
- **Associazione di parametri predefiniti per i parametri di tipo semplice è ora [FromUri]:** nelle versioni precedenti di ASP.NET Web API, l'associazione di parametri di impostazione predefinita per i parametri di tipo semplice utilizzata l'associazione di modelli. L'associazione di parametri predefiniti per i parametri di tipo semplice è ora *[FromUri]*.
- **Selezione azione rispetta i parametri obbligatori:** selezione dell'azione nell'API Web ASP.NET ora selezionerà solo un'azione se vengono forniti tutti i parametri obbligatori che provengono dall'URI. Un parametro può essere specificato come facoltativo, fornendo un valore predefinito per l'argomento nella firma del metodo di azione.
- **Personalizzare le associazioni di parametro HTTP:** usare il *ParameterBindingAttribute* per personalizzare l'associazione di parametri per un parametro di azione specifico o usare i *ParameterBindingRules* sul *HttpConfiguration* personalizzare le associazioni di parametro pubblico più ampio.
- **Miglioramenti di MediaTypeFormatter:** formattatori ora hanno accesso a tutte *HttpContent* istanza.
- **La memorizzazione nel buffer selezione criteri host:** implementare e configurare il *IHostBufferPolicySelector* nell'API Web ASP.NET per abilitare gli host determinare i criteri per l'utilizzo di memorizzazione nel buffer da utilizzare service.
- **Accedere ai certificati client in modo indipendente dalla host:** usare il *GetClientCertificate* metodo di estensione per ottenere il certificato client fornito dal messaggio di richiesta.
- **Estendibilità di negoziazione del contenuto:** personalizzare la negoziazione del contenuto mediante la derivazione dal *DefaultContentNegotiator* ed eseguendo l'override di qualsiasi aspetto della negoziazione del contenuto che si desidera.
- **Supporto per la restituzione di risposte non accettabile 406:** può ora restituire le risposte non accettabile 406 nell'API Web ASP.NET quando non viene trovato un formattatore appropriato tramite la creazione di un *DefaultContentNegotiator* con il *excludeMatchOnTypeOnly* parametro impostato su *true*.
- **Leggere i dati del modulo come NameValueCollection o JToken:** è possibile leggere i dati del modulo nella stringa di query o nel corpo della richiesta come un *NameValueCollection* utilizzando le *ParseQueryString, che* e *ReadAsFormDataAsync* i metodi di estensione rispettivamente. Analogamente, è possibile leggere i dati del modulo nella stringa di query o nel corpo della richiesta come una *JToken* usando la *TryReadQueryAsJson* e *ReadAsAsync*&lt;T&gt; metodi di estensione rispettivamente.
- **I miglioramenti più parti:** è ora possibile scrivere un *MultipartStreamProvider* che completamente personalizzata in base al tipo di dati multipart MIME che può leggere e presentare i risultati in modo ottimale per l'utente. È anche possibile connettere un passaggio di post-elaborazione nel *MultipartStreamProvider* che consente l'implementazione di effettuare qualsiasi post-elaborazione, esperto in parti di corpo multipart MIME. Ad esempio, il *MultipartFormDataStreamProvider* implementazione legge il form HTML parti di dati e li aggiunge a un *NameValueCollection* in modo che siano facili da esaminare dal chiamante.
- **Collegare i miglioramenti di generazione:** il *UrlHelper* non dipende più *HttpControllerContext*. È ora possibile accedere la *UrlHelper* da qualsiasi contesto in cui le *HttpRequestMessage* è disponibile.
- **Modifica dell'ordine di esecuzione del gestore dei messaggi:** gestori di messaggi vengono ora eseguiti nell'ordine in cui sono configurati anziché in ordine inverso.
- **Helper per il collegamento dei gestori di messaggi:** la nuova *HttpClientFactory* che può collegare *DelegatingHandlers* e creare un *HttpClient* con di pronto per iniziare la pipeline desiderata. Fornisce inoltre funzionalità per il collegamento dei con gestori interni alternativi (il valore predefinito è *HttpClientHandler*), nonché effettuare il collegamento di quando si usa *HttpMessageInvoker* o un altro  *DelegatingHandler* invece di *HttpClient* come top-invoker.
- **Supporto per le reti CDN nell'ottimizzazione Web ASP.NET:** ottimizzazione Web ASP.NET offre ora il supporto per un altro URL che punta alla risorsa stessa in una rete di distribuzione di contenuti del bundle percorsi alternativi della rete CDN che consente di specificare per ognuno. Che supporta le reti CDN consente di ottenere i bundle script e stile geograficamente più vicini agli utenti finali di applicazioni Web. Le app di produzione devono implementare un fallback quando non è disponibile la rete CDN. Testare il fallback.
- **Esegue il routing ASP.NET Web API e configurazione spostato *webapiconfig. Register* metodo statico che può essere resused nel codice di test.** Le route di ASP.NET Web API sono state aggiunte in precedenza in *RouteConfig.RegisterRoutes* insieme a MVC standard esegue il routing. L'impostazione predefinita esegue il routing ASP.NET Web API e la configurazione a questo punto vengono gestiti in un oggetto separato *webapiconfig. Register* metodo per facilitare il test.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

- **La versione RC e RTM di ASP.NET MVC 4 in modo errato restituito visualizzazioni desktop memorizzate nella cache quando devono essere restituite visualizzazioni per dispositivi mobili.**

    - Visualizzare [ASP.NET MVC 4 per dispositivi mobili la memorizzazione nella cache Bug fisso](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) per i dettagli della correzione. La correzione può essere installata dal [DisplayModes fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacchetto NuGet.
- **Modifiche di rilievo nel motore di visualizzazione Razor**. I tipi seguenti sono state rimosse da *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  I metodi seguenti sono state inoltre rimosse: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Quando si WebMatrix.WebData.dll è incluso nella directory /bin di un'App ASP.NET MVC 4, subentra l'URL per l'autenticazione basata su form.** Aggiunta dell'assembly WebMatrix.WebData.dll all'applicazione (ad esempio, selezionando "Pagine Web ASP.NET con la sintassi delle Razor" quando si usa la finestra di dialogo Aggiungi dipendenze distribuibili) sostituirà il reindirizzamento di autenticazione account di accesso/account/accesso anziché / account/login come previsto dall'Account di Controller MVC di ASP.NET predefinito. Per evitare questo comportamento e usare l'URL specificato è già nella sezione autenticazione di Web. config, è possibile aggiungere un'impostazione App denominata PreserveLoginUrl e impostarlo su true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Gestione pacchetti NuGet non viene installato quando si prova a installare ASP.NET MVC 4 per installazioni side-by-side di Visual Studio 2010 e Visual Web Developer 2010.** Per eseguire Visual Studio 2010 e Visual Web Developer 2010 affiancato con ASP.NET MVC 4 è necessario installare ASP.NET MVC 4 dopo che sono già state installate entrambe le versioni di Visual Studio.
- **Disinstallazione di ASP.NET MVC 4 ha esito negativo se i prerequisiti sono già stati disinstallati.** Per disinstallare correttamente l'applicazione ASP.NET MVC 4you necessario disinstallare ASP.NET MVC 4 prima di disinstallare Visual Studio.
- **Installazione di ASP.NET MVC 4 interrompe le applicazioni ASP.NET MVC 3 RTM.** Le applicazioni ASP.NET MVC 3 che sono state create con la versione RTM versione (non con il [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/download/details.aspx?id=1491) release) richiedono le seguenti modifiche per il funzionamento side-by-side con ASP.NET MVC 4. Compilazione del progetto senza apportare questi risultati degli aggiornamenti in errori di compilazione. 

    **Aggiornamenti necessari**

  1. Nel file Web. config radice, aggiungere una nuova *&lt;appSettings&gt;* voce con la chiave *webPages:Version* e il valore *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. In Esplora soluzioni fare clic sul nome del progetto e quindi Scarica progetto. Quindi fare doppio clic il nome del nuovo e selezionare Modifica *ProjectName*csproj.
  3. Individuare i riferimenti assembly seguenti: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Sostituirli con il codice seguente:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) la modifica, quindi fare clic sul progetto e scegliere Ricarica.

- **La modifica di un progetto ASP.NET MVC 4 alla destinazione 4.0 da 4.5 non aggiorna il riferimento all'assembly EntityFramework:** se si modifica un progetto ASP.NET MVC 4 di destinazione 4.0 dopo la destinazione è 4.5 il riferimento all'assembly EntityFramework punteranno ancora alla la versione 4.5. Per risolvere questa disinstallazione problema e reinstallare il pacchetto EntityFramework NuGet.
- **403-accesso negato durante l'esecuzione di un'applicazione ASP.NET MVC 4 in Azure dopo la modifica alla destinazione 4.0 da 4.5:** se si modifica un progetto ASP.NET MVC 4 alla destinazione 4.0 dopo la destinazione è 4.5 e distribuirla in Azure si verifichi un errore 403 accesso negato in fase di esecuzione. Per risolvere questo problema, aggiungere quanto segue al file Web. config: `<modules runAllManagedModulesForAllRequests="true" />`
- **Arresto anomalo di Visual Studio 2012 quando si digita un '\' in un valore letterale stringa in un file Razor.** Usare la risoluzione del problema prima di tutto immettere la virgoletta di chiusura del valore letterale stringa.
- <strong>Esplorando &quot;Account/gestire&quot; nei risultati modello Internet di un errore di runtime per le lingue cinese, TRK e in cinese tradizionale.</strong> Per risolvere il problema di modificare la pagina per separare <em>@User.Identity.Name</em> posizionandola come unico contenuto all'interno di <em>&lt;sicuro&gt;</em> tag.
- **Provider di LinkedIn e Google non sono supportati all'interno di siti Web di Azure.** Usare i provider di autenticazione alternativi durante la distribuzione di siti Web di Azure.
- **Quando si usa UriPathExtensionMapping con IIS 8 Express o IIS, si riceverebbe 404 errori non trovato quando si prova a usare l'estensione.** Il gestore di file statici interferisce con le richieste di API web che usano *UriPathExtensionMappings*. Impostare *runAllManagedModulesForAllRequests = true* in Web. config per risolvere il problema.
- **Metodo Controller.Execute non venga più chiamato.** Tutti i controller MVC ora vengono sempre eseguiti in modo asincrono.
