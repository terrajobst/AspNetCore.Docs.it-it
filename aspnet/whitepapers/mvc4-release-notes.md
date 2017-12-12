---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Documenti Microsoft
author: rick-anderson
description: Questo documento descrive la versione di ASP.NET MVC 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: fb9d2eaa83fe7486279815c21aec204bdfdf122d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
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
    - [Miglioramenti apportati a modelli di progetto predefiniti](#_Toc303253808)
    - [Modello di progetto per dispositivi mobili](#_Toc303253809)
    - [Modalità di visualizzazione](#_Toc303253810)
    - [jQuery Mobile, la selezione tipo di visualizzazione e si esegue l'override di Browser](#_Toc303253811)
    - [Attività di supporto per i controller asincroni](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Migrazioni di database](#_Toc303253818)
    - [Modello di progetto vuoto](#_Toc303253819)
    - [Aggiungi Controller a qualsiasi cartella del progetto](#_Toc303253820)
    - [Bundling and Minification](#_Toc303253821)
    - [Abilitazione di account di accesso di Facebook e ad altri siti con OAuth e OpenID](#_Toc303253822)
- [Aggiornamento di un progetto ASP.NET MVC 3 ad ASP.NET MVC 4](#_Toc303253806)
- [Modifiche dalla versione finale candidata di ASP.NET MVC 4](#_Toc303253817)
- [Problemi noti e modifiche di rilievo](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Note sull'installazione

ASP.NET MVC 4 per Visual Studio 2010 può essere installato mediante il [home page di ASP.NET MVC 4](../mvc/mvc4.md) utilizzando l'installazione guidata piattaforma Web.

È consigliabile disinstallare qualsiasi installato in precedenza le anteprime di ASP.NET MVC 4 prima di installare ASP.NET MVC 4. È possibile aggiornare la versione finale candidata ASP.NET MVC 4 Beta per ASP.NET MVC 4 senza disinstallare.

Questa versione non è compatibile con le versioni di anteprima di .NET Framework 4.5. Separatamente, è necessario aggiornare eventuali versioni di anteprima installata di .NET Framework 4.5 con la versione finale prima di installare ASP.NET MVC 4.

ASP.NET MVC 4 può essere installato ed eseguito side-by-side con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentazione

Documentazione per ASP.NET MVC è disponibile nel sito Web MSDN all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkId=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Esercitazioni e altre informazioni su ASP.NET MVC sono disponibili nella pagina del sito Web ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Supporto

ASP.NET MVC 4 è completamente supportato. In caso di domande sull'utilizzo di questa versione è anche possibile registrarli nel forum di ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), in cui i membri della community ASP.NET sono spesso in grado di fornire supporto informale.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisiti software

I componenti di ASP.NET MVC 4 per Visual Studio richiedono PowerShell 2.0 e Microsoft Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nuove funzionalità in ASP.NET MVC 4

Questa sezione vengono descritte le funzionalità che sono state introdotte nella versione di ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API Web ASP.NET

ASP.NET MVC 4 include ASP.NET Web API, un nuovo framework per la creazione di servizi HTTP che possono raggiungere una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è una piattaforma ideale per la creazione di servizi RESTful.

ASP.NET Web API include il supporto per le funzionalità seguenti:

- **Modello di programmazione moderno HTTP:** direttamente accedere e modificare le richieste HTTP e le risposte alle API Web utilizzando un modello a oggetti fortemente tipizzato nuovo HTTP. Lo stesso modello di programmazione e pipeline HTTP è disponibile in modo simmetrico nel client tramite il nuovo *HttpClient* tipo.
- **Supporto completo per le route:** API Web ASP.NET supporta il set completo di funzionalità di route del Routing di ASP.NET, inclusi i parametri di route e vincoli. Inoltre, è possibile utilizzare le convenzioni semplice per eseguire il mapping di azioni a metodi HTTP.
- **Negoziazione del contenuto:** client e server possono essere usati insieme per determinare il formato corretto per i dati restituiti da un'API web. ASP.NET Web API fornisce supporto predefinito per XML, JSON e formati con codifica URL Form ed è possibile estendere questo supporto mediante l'aggiunta di formattatori personalizzati o anche sostituire la strategia di negoziazione di contenuto predefinito.
- **Associazione e la convalida del modello:** raccoglitori offrono un modo semplice per estrarre dati da varie parti di una richiesta HTTP e convertire le parti del messaggio in oggetti .NET che possono essere utilizzati dalle azioni di API Web. La convalida viene eseguita anche parametri dell'azione in base alle annotazioni di dati.
- **Filtri:** ASP.NET Web API supporta i filtri, inclusi filtri noti come il *[Authorize]* attributo. È possibile creare e collegare i propri filtri per le azioni, autorizzazione e la gestione delle eccezioni.
- **Composizione della query:** utilizzare il *[Queryable]* attributo di filtro in un'azione che restituisce *IQueryable* per abilitare il supporto per le query l'API web tramite le convenzioni di query OData.
- **Migliorare la testabilità:** anziché in oggetti di contesto statico, impostare i dettagli HTTP, web lavoro azioni API con istanze di *HttpRequestMessage* e *HttpResponseMessage*. Creare un progetto di unit test con il progetto API Web per iniziare rapidamente la scrittura di unit test per la funzionalità API Web.
- **Configurazione basata su codice:** configurazione di ASP.NET Web API viene eseguita esclusivamente tramite codice, lasciando il file di configurazione parziale di file. Per configurare i punti di estendibilità, utilizzare il modello di localizzatore di servizi specificato.
- **Migliorato il supporto per i contenitori Inversion of Control (IoC):** ASP.NET Web API fornisce supporto ottimale per i contenitori di inversione di controllo tramite un'astrazione di resolver di dipendenza migliorata
- **Servizio indipendente:** API Web possono essere ospitate in un proprio processo oltre a IIS pur continuando a utilizzare tutta la potenza delle route e altre funzionalità dell'API Web.
- **Creare una Guida personalizzata e testare le pagine:** è ora possibile facilmente Guida personalizzata di compilazione e test delle pagine per l'API web utilizzando il nuovo *IApiExplorer* servizio per ottenere una descrizione completa di runtime di web API.
- **Monitoraggio e diagnostica:** API Web ASP.NET ora fornisce l'infrastruttura di traccia leggero che semplifica l'integrazione con soluzioni esistenti di registrazione, ad esempio System. Diagnostics, ETW e Framework di registrazione di terze parti. È possibile abilitare la traccia, fornendo un *ITraceWriter* implementazione e aggiungerlo alla configurazione web API.
- **Generazione di collegamento:** utilizzare l'API Web ASP.NET *UrlHelper* per generare i collegamenti a risorse correlate nella stessa applicazione.
- **Modello di progetto Web API:** selezionare il nuovo modulo di progetto API Web la procedura guidata nuovo progetto MVC 4 per diventare rapidamente e in esecuzione con l'API Web ASP.NET.
- **Lo scaffolding:** utilizzare il **Aggiungi Controller** finestra di dialogo per eseguire rapidamente lo scaffolding di un controller web API basato su un Entity Framework basato su tipo di modello.

Per ulteriori informazioni su ASP.NET Web API, visitare [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Miglioramenti apportati a modelli di progetto predefiniti

Il modello utilizzato per creare nuovi progetti ASP.NET MVC 4 è stato aggiornato per creare un sito Web dall'aspetto moderno più:

![](mvc4-release-notes/_static/image1.png)

Oltre ai miglioramenti cosmetici, ha migliorato la funzionalità nel nuovo modello. Il modello utilizza una tecnica denominata per il rendering adattivo per essere utilizzati sia nei browser desktop e browser per dispositivi mobili senza personalizzazione.

![](mvc4-release-notes/_static/image2.png)

Per visualizzare il rendering adattivo in azione, è possibile utilizzare un emulatore di dispositivi mobili o semplicemente provare a ridimensionare la finestra del browser desktop per essere più piccoli. Quando la finestra del browser ottiene sufficientemente piccola, viene modificato il layout della pagina.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modello di progetto per dispositivi mobili

Se si inizia un nuovo progetto e si desidera creare un sito in modo specifico per dispositivi mobili e tablet browser, è possibile utilizzare il nuovo modello di progetto di applicazione per dispositivi mobili. Si basa su jQuery Mobile, una libreria open source per la compilazione ottimizzata per il tocco dell'interfaccia utente:

![](mvc4-release-notes/_static/image3.png)

Questo modello contiene la stessa struttura dell'applicazione come modello di applicazione Internet (e il codice del controller è praticamente identico), ma è applicato lo stile tramite jQuery Mobile per l'aspetto e il comportamento anche su dispositivi mobili basati su tocco. Per ulteriori informazioni sull'organizzazione e lo stile dell'interfaccia utente per dispositivi mobili, vedere il [sito Web del progetto per dispositivi mobili jQuery](http://jquerymobile.com/).

Se si dispone già di un sito basata su desktop che si desidera aggiungere viste ottimizzata, o se si desidera creare un singolo sito che viene utilizzato uno stile differente viste per i browser desktop e mobile, è possibile utilizzare la nuova funzionalità di modalità di visualizzazione. (Vedere la sezione successiva).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modalità di visualizzazione

La nuova funzionalità di modalità di visualizzazione consente a un'applicazione di selezionare le viste in base al browser che effettua la richiesta. Se, ad esempio, un browser desktop richiede la Home page, l'applicazione potrebbe utilizzare il modello di Views\Home\Index.cshtml. Se un browser per dispositivi mobili richiede la Home page, l'applicazione potrebbe restituire il modello Views\Home\Index.mobile.cshtml.

Layout e parziali possono essere sostituiti anche per i tipi di browser specifico. Ad esempio:

- Se la cartella Views\Shared contiene sia il \_cshtml e \_Layout.mobile.cshtml modelli, per impostazione predefinita, l'applicazione utilizzerà \_Layout.mobile.cshtml durante le richieste dal browser per dispositivi mobili e \_Cshtml durante le altre richieste.
- Se una cartella contiene sia \_MyPartial.cshtml e \_MyPartial.mobile.cshtml, l'istruzione @Html.Partial("\_MyPartial") verrà eseguito il rendering \_MyPartial.mobile.cshtml durante le richieste provenienti da dispositivi mobili browser, e \_MyPartial.cshtml durante le altre richieste.

Se si desidera creare visualizzazioni più specifiche, layout o visualizzazioni parziali per altri dispositivi, è possibile registrare un nuovo *DefaultDisplayMode* istanza per specificare il nome per la ricerca quando una richiesta soddisfa le condizioni particolari. Ad esempio, è possibile aggiungere il codice seguente per il *applicazione\_avviare* metodo nel file Global. asax per registrare la stringa "iPhone" come modalità di visualizzazione che si applica quando il browser Apple iPhone che effettua una richiesta:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Quando si esegue questo codice, quando un browser Apple iPhone che effettua una richiesta, l'applicazione utilizzerà il Views\Shared\\_Layout.iPhone.cshtml layout (se presente). Per ulteriori informazioni sulla modalità di visualizzazione, vedere [funzionalità di ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Le applicazioni utilizzando DisplayModeProvider devono installare il [DisplayModes fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacchetto NuGet. Il [ASP.NET autunno 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322) include il [DisplayModes fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacchetto NuGet in nuovi modelli di progetto. Vedere [ASP.NET MVC 4 Mobile la memorizzazione nella cache Bug Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) per informazioni dettagliate su per risolvere il problema.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>Funzionalità di dispositivi mobili e jQuery Mobile

Per informazioni sulla compilazione di applicazioni per dispositivi mobili con ASP.NET MVC 4 tramite jQuery Mobile, vedere l'esercitazione [funzionalità di ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Attività di supporto per i controller asincroni

Ora è possibile scrivere metodi di azione asincroni come un'unica i metodi che restituiscono un oggetto di tipo *attività* o *attività&lt;ActionResult&gt;*.

 Per ulteriori informazioni vedere [utilizzando metodi asincroni in ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 supporta le versioni 1.6 e versioni successive di Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migrazioni di database

Progetti ASP.NET MVC 4 includono ora Entity Framework 5. Una delle caratteristiche in Entity Framework 5 è il supporto per le migrazioni di database. Questa funzionalità consente di sviluppare facilmente lo schema del database utilizzando una migrazione focalizzato sul codice mantenendo i dati nel database. Per ulteriori informazioni sulla migrazione di database, vedere [aggiungendo un nuovo campo al modello di film e di tabella](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) nel [Introduzione all'esercitazione di ASP.NET MVC 4](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Modello di progetto vuoto

Il modello di progetto MVC vuoto è realmente vuoto in modo che è possibile avviare da una situazione completamente pulita. La versione precedente del modello di progetto vuoto è stata rinominata in base.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Aggiungi Controller a qualsiasi cartella del progetto

È possibile fare clic destro e selezionare **Aggiungi Controller** da qualsiasi cartella del progetto MVC. Ciò offre una maggiore flessibilità per organizzare i controller, ma si desidera, tra cui mantenere i controller MVC e Web API in cartelle separate.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Bundling and Minification

Il framework di aggregazione e di riduzione consente di ridurre il numero di richieste HTTP che una pagina Web deve rendere combinando i singoli file in un singolo file per gli script e CSS in dotazione. Quindi possibile ridurre la dimensione complessiva di tali richieste per la riduzione di contenuti del bundle. Minimizzazione possono includere attività quali l'eliminazione degli spazi vuoti per abbreviare i nomi delle variabili di compressione anche selettori CSS in base alle loro semantica. Bundle vengono dichiarati e configurati nel codice e sono facilmente riferimento nelle visualizzazioni tramite i metodi helper che possono generare uno un singolo collegamento per il bundle di oppure, quando il debug, più i collegamenti al contenuto del bundle singoli. Per ulteriori informazioni vedere [Bundling and Minification](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Abilitazione di account di accesso di Facebook e ad altri siti con OAuth e OpenID

I modelli predefiniti nel modello di progetto ASP.NET MVC 4 Internet include ora il supporto per l'account di accesso OAuth e OpenID utilizzando la libreria DotNetOpenAuth. Per informazioni sulla configurazione di un provider OAuth o OpenID, vedere [supporto OAuth/OpenID per Web Form, MVC e pagine Web](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) e [le funzionalità documentazione in ASP.NET Web Pages OAuth e OpenID](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Aggiornamento di un progetto ASP.NET MVC 3 ad ASP.NET MVC 4

ASP.NET MVC 4 può essere installato nello stesso computer, che offre una flessibilità nella scelta del momento di aggiornare un'applicazione ASP.NET MVC 3 ad ASP.NET MVC 4 in modo affiancato con ASP.NET MVC 3.

Il modo più semplice per eseguire l'aggiornamento è per creare un nuovo progetto ASP.NET MVC 4 e copiare tutte le visualizzazioni, controller, codice e i contenuti file dal progetto MVC 3 esistente al nuovo progetto e quindi aggiornare l'assembly fa riferimento nel nuovo progetto per la corrispondenza di qualsiasi modello di non MVC in assiemi cluded in uso. Se sono state apportate modifiche al file Web. config nel progetto MVC 3, è anche necessario unire le modifiche nel file Web. config nel progetto MVC 4.

Per aggiornare manualmente un'applicazione ASP.NET MVC 3 alla versione 4, eseguire le operazioni seguenti:

1. In Web. config di tutti i file del progetto (è presente nella radice del progetto, uno nella cartella Views e uno nella cartella Views per ogni area del progetto), sostituire ogni istanza del testo seguente (Nota: System.Web.WebPages, Version = 1.0.0.0, non viene trovato progetti creati con Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    con il testo corrispondente seguente:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. Nel file Web. config radice, aggiornare il *webPages:Version* elemento "2.0.0.0" e aggiungere un nuovo *PreserveLoginUrl* chiave con il valore "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. In Esplora soluzioni, fare clic sui riferimenti e scegliere Gestisci pacchetti NuGet. Nel riquadro a sinistra, selezionare **origine pacchetto ufficiale Online\NuGet**, quindi aggiornare le operazioni seguenti:

    - ASP.NET MVC 4
    - (Facoltativo) jQuery, jQuery jQuery UI e convalida
    - (Facoltativo) Entity Framework
    - (Optonal) Modernizr
4. In Esplora soluzioni fare doppio clic sul nome del progetto e scegliere Scarica progetto. Quindi fare di nuovo il nome e selezionare Modifica *ProjectName*con estensione csproj.
5. Individuare il *ProjectTypeGuids* elemento e sostituire {E53F8FEA-EAE0-44A6-8774-FFD645390401} con {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) che si stava modificando, fare clic sul progetto e quindi scegliere Ricarica progetto.
7. Se il progetto fa riferimento a tutte le librerie di terze parti che vengono compilate utilizzando versioni precedenti di ASP.NET MVC, aprire il file Web. config radice e aggiungere i seguenti tre *bindingRedirect* elementi sotto il  *configurazione* sezione: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Modifiche dalla versione finale candidata di ASP.NET MVC 4

Le note sulla versione per ASP.NET MVC 4 Release Candidate è reperibile qui:

Le modifiche principali da ASP.NET MVC 4 Release Candidate in questa versione sono riepilogate di seguito:

- **Per ogni configurazione del controller:** controller API Web ASP.NET può essere attribuito un attributo personalizzato che implementa *IControllerConfiguration* per impostare i propri formattatori selettore di azioni e gestori di associazione di parametro . Il *HttpControllerConfigurationAttribute* è stato rimosso.
- **Per i gestori di messaggi route:** è ora possibile specificare il gestore di messaggio finale nella catena di richiesta per una route specificata. In questo modo il supporto per Framework marcia-lungo di utilizzare il routing di invio a propria (non -*IHttpController*) gli endpoint.
- **Le notifiche sullo stato di avanzamento:** il *ProgressMessageHandler* genera la notifica dello stato per l'entità richiesta caricate sia entità risposta scaricate. Tramite questo gestore è possibile tenere traccia di quanto si un corpo della richiesta di caricamento o download di un corpo della risposta.
- **Push contenuto:** il *PushStreamContent* classe consente gli scenari in cui un produttore di dati desidera scrivere direttamente nella richiesta o risposta (in modo sincrono o asincrono) utilizzando un flusso. Quando il *PushStreamContent* è pronto per accettare i dati di cui effettua una chiamata a un delegato di azione con il flusso di output. Lo sviluppatore può quindi scrivere nel flusso per fino a quando necessario e chiudere il flusso durante la scrittura è stata completata. Il *PushStreamContent* rileva la chiusura del flusso e completa sottostante asincrona *attività* per scrivere il contenuto.
- **Creazione di risposte di errore:** utilizzare il *HttpError* tipo per rappresentare in modo coerente le informazioni di errore da quali errori di convalida e le eccezioni ancora rispettando il *IncludeErrorDetailPolicy* . Usare il nuovo *CreateErrorResponse* metodi di estensione per creare facilmente le risposte di errore con *HttpError* come contenuto. Il *HttpError* contenuto è completamente contenuto negoziato.
- **MediaRangeMapping rimosso:** intervalli dei tipi di supporto sono ora gestiti da negoziatore del contenuto predefinito.
- **Associazione di parametro predefinito per i parametri di tipo semplice è ora [FromUri]:** nelle versioni precedenti di ASP.NET Web API, l'associazione di parametri di valore predefinito per i parametri di tipo semplice associazione di modelli. L'associazione di parametro predefinito per i parametri di tipo semplice è ora *[FromUri]*.
- **Selezione di azione rispetta i parametri obbligatori:** selezione azione in ASP.NET Web API ora selezionerà solo un'azione se vengono forniti tutti i parametri obbligatori che provengono dall'URI. Un parametro può essere specificato come facoltativo, fornendo un valore predefinito per l'argomento nella firma del metodo di azione.
- **Personalizzare le associazioni di parametri HTTP:** utilizzare il *ParameterBindingAttribute* per personalizzare l'associazione di parametri per un parametro di azione specifica oppure utilizzare il *ParameterBindingRules* in il *HttpConfiguration* personalizzare le associazioni di parametro più diffuso.
- **Miglioramenti di MediaTypeFormatter:** formattatori è ora possibile accedere a tutte *HttpContent* istanza.
- **Il buffer di selezione dei criteri host:** implementare e configurare il *IHostBufferPolicySelector* servizio nell'API Web ASP.NET per abilitare l'host per determinare i criteri per l'utilizzo di memorizzazione nel buffer da utilizzare.
- **I certificati client di accedere in modo indipendente host:** utilizzare il *GetClientCertificate* metodo di estensione per ottenere il certificato client fornito dal messaggio di richiesta.
- **Estensibilità di negoziazione del contenuto:** personalizzare negoziazione del contenuto mediante derivazione da di *DefaultContentNegotiator* ed eseguendo l'override di qualsiasi aspetto della negoziazione del contenuto che si desidera.
- **Supporto per la restituzione di risposte non accettabile 406:** può ora restituire risposte inaccettabile 406 nell'API Web ASP.NET quando non viene trovato un formattatore appropriato tramite la creazione di un *DefaultContentNegotiator* con il *excludeMatchOnTypeOnly* parametro impostato su *true*.
- **Leggere i dati del form come NameValueCollection o JToken:** è possibile leggere i dati del form nella stringa di query dell'URI o nel corpo della richiesta come un *NameValueCollection* utilizzando il *ParseQueryString, che* e  *ReadAsFormDataAsync* metodi di estensione rispettivamente. Analogamente, è possibile leggere i dati del form nella stringa di query dell'URI o nel corpo della richiesta come un *JToken* utilizzando il *TryReadQueryAsJson* e *ReadAsAsync*&lt;T&gt; metodi di estensione rispettivamente.
- **Miglioramenti multipart:** è ora possibile scrivere un *MultipartStreamProvider* che completamente personalizzata in base al tipo di dati in più parti MIME che può leggere e presentare il risultato in modo ottimale all'utente. È anche possibile associare un passaggio di post-elaborazione nel *MultipartStreamProvider* che consente all'implementazione di effettuare qualsiasi di post-elaborazione si desiderano le parti di corpo multipart MIME. Ad esempio, il *MultipartFormDataStreamProvider* implementazione legge il form HTML parti di dati e li aggiunge a un *NameValueCollection* in modo che siano facili da ottenere dal chiamante.
- **Collegare i miglioramenti di generazione:** il *UrlHelper* non dipende più *HttpControllerContext*. È ora possibile accedere il *UrlHelper* da qualsiasi contesto in cui il *HttpRequestMessage* è disponibile.
- **Modifica dell'ordine di esecuzione del gestore dei messaggi:** gestori di messaggi vengono ora eseguiti nell'ordine in cui sono configurati anziché in ordine inverso.
- **Supporto per il collegamento dei gestori di messaggi:** nuovo *HttpClientFactory* che può associare *DelegatingHandlers* e creare un *HttpClient* con il pipeline desiderata pronta. Fornisce inoltre funzionalità per il collegamento dei con gestori interni alternativi (il valore predefinito è *HttpClientHandler*), nonché effettuare il collegamento di quando si utilizza *HttpMessageInvoker* o un altro  *DelegatingHandler* anziché *HttpClient* come invoker dell'inizio.
- **Supporto per le reti CDN in ASP.NET Web ottimizzazione:** ottimizzazione Web di ASP.NET ora fornisce supporto per i percorsi di rete CDN alternativi che consente di specificare per ogni bundle un ulteriore URL che fa riferimento alla stessa risorsa in una rete di distribuzione del contenuto. Supporto CDN consente di ottenere i bundle script e stile geograficamente più vicino agli utenti finali di applicazioni Web.
- **Esegue il routing ASP.NET Web API e configurazione è stato spostato in *WebApiConfig.Register* metodo statico che può essere resused nel codice di test.** Le route ASP.NET Web API sono state aggiunte in precedenza in *RouteConfig.RegisterRoutes* instrada insieme il MVC standard. L'API Web ASP.NET instrada configurazione predefinita e vengono ora gestiti in un apposito *WebApiConfig.Register* metodo per semplificare il test.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

- **La versione RC e la versione RTM di ASP.NET MVC 4 restituito erroneamente visualizzazioni desktop memorizzate nella cache quando visualizzazioni mobili devono essere restituite.**

    - Vedere [ASP.NET MVC 4 Mobile la memorizzazione nella cache Bug predefinito](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) per informazioni dettagliate su per risolvere il problema. Per risolvere il problema può essere installato mediante il [DisplayModes fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacchetto NuGet.
- **Modifiche di rilievo nel motore di visualizzazione Razor**. I tipi seguenti sono stati rimossi da *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 I metodi seguenti sono state inoltre rimosse: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Quando WebMatrix.WebData.dll siano presenti nella directory /bin di un'app di ASP.NET MVC 4, assume l'URL per l'autenticazione basata su form.** Aggiunta dell'assembly WebMatrix.WebData.dll all'applicazione (ad esempio, selezionando "Pagine Web ASP.NET con la sintassi delle Razor" quando si utilizza la finestra di dialogo Aggiungi dipendenze distribuibili) sostituiranno il reindirizzamento di autenticazione account di accesso per l'accesso/account/anziché / account/account di accesso come previsto, il valore predefinito di ASP.NET MVC Controller di Account. Per evitare questo comportamento e usare l'URL specificato già nella sezione autenticazione di Web. config, è possibile aggiungere un appSetting chiamato PreserveLoginUrl e impostarlo su true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Gestione pacchetti NuGet non viene installato durante l'installazione di ASP.NET MVC 4 per le installazioni affiancate di Visual Studio 2010 e Visual Web Developer 2010.** Per eseguire Visual Studio 2010 e Visual Web Developer 2010 in modo affiancato con ASP.NET MVC 4 è necessario installare ASP.NET MVC 4 dopo entrambe le versioni di Visual Studio sono già installate.
- **La disinstallazione di ASP.NET MVC 4 ha esito negativo se i prerequisiti sono già stati disinstallati.** Per disinstallare correttamente ASP.NET MVC 4you necessario disinstallare ASP.NET MVC 4 prima di disinstallare Visual Studio.
- **L'installazione di ASP.NET MVC 4 interrompe le applicazioni ASP.NET MVC 3 RTM.** Le applicazioni ASP.NET MVC 3 che sono state create con la versione RTM versione (non con il [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/en-us/download/details.aspx?id=1491) rilasciare) richiedono le seguenti modifiche per il funzionamento side-by-side con ASP.NET MVC 4. Compilazione del progetto senza apportare questi risultati gli aggiornamenti in errori di compilazione. 

    **Aggiornamenti necessari**

    1. Nel file Web. config radice, aggiungere un nuovo  *&lt;appSettings&gt;*  voce con la chiave *webPages:Version* e il valore *1.0.0.0*. 

        [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
    2. In Esplora soluzioni fare doppio clic sul nome del progetto e scegliere Scarica progetto. Quindi fare di nuovo il nome e selezionare Modifica *ProjectName*con estensione csproj.
    3. Individuare i riferimenti agli assembly seguenti: 

        [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

        Sostituirli con gli elementi seguenti:

        [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
    4. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) la modifica, quindi fare clic sul progetto e scegliere Ricarica.
- **La modifica di un progetto ASP.NET MVC 4 di destinazione 4.0 da 4.5 non aggiorna il riferimento all'assembly EntityFramework:** se si modifica un progetto ASP.NET MVC 4 di destinazione 4.0 dopo il riferimento all'assembly EntityFramework punteranno ancora alla interessati a 4.5 la versione 4.5. Per correggere la disinstallazione di questo problema e reinstallare il pacchetto EntityFramework NuGet.
- **403-accesso negato durante l'esecuzione di un'applicazione ASP.NET MVC 4 in Azure dopo la modifica di destinazione 4.0 da 4.5:** se si modifica un progetto ASP.NET MVC 4 di destinazione 4.0 dopo destinata 4.5 e distribuirla in Azure venga visualizzato un errore 403 accesso negato in fase di esecuzione. Per risolvere questo problema, aggiungere quanto segue al file Web. config:`<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 si blocca quando si digita un '\' in una stringa letterale in un file Razor.** Per utilizzare il problema prima di tutto immettere la virgoletta di chiusura della stringa letterale.
- **La selezione di &quot;/Gestisci Account&quot; nei risultati modello Internet di un errore di runtime per le lingue cinese semplificato, REV e cinese tradizionale.** Per risolvere il problema, modificare la pagina per separare  *@User.Identity.Name*  mettendola come il contenuto solo all'interno di  *&lt;sicuro&gt;*  tag.
- **Provider di Google e LinkedIn non sono supportati all'interno di siti Web di Azure.** Utilizzare i provider di autenticazione alternativo per la distribuzione di siti Web di Azure.
- **Quando si utilizza UriPathExtensionMapping con IIS 8 Express/IIS, si riceverebbe 404 errori non trovato quando si tenta di utilizzare l'estensione.** Gestore di file statici interferisce con le richieste di API web che usano *UriPathExtensionMappings*. Impostare *runAllManagedModulesForAllRequests = true* in Web. config per risolvere il problema.
- **Metodo Controller.Execute non viene chiamato.** Tutti i controller MVC ora vengono sempre eseguiti in modo asincrono.
