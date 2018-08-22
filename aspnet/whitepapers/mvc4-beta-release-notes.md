---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive la versione di ASP.NET MVC 4 Beta per Visual Studio 2010.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: f1d949ec716ea8cb677c54fe5b07431161c58fbc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823931"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Questo documento descrive la versione di ASP.NET MVC 4 Beta per Visual Studio 2010.
> 
> > [!NOTE]
> > Non si tratta della versione più recente. Sono disponibili le note sulla versione di ASP.NET MVC 4 RC [qui](mvc4-release-notes.md).


- [Note sull'installazione](#_Toc303253802)
- [Documentazione](#_Toc303253803)
- [Supporto](#_Toc303253804)
- [Requisiti software](#_Toc303253805)
- [L'aggiornamento di un progetto ASP.NET MVC 3 ad ASP.NET MVC 4](#_Toc303253806)
- [Nuove funzionalità in ASP.NET MVC 4 Beta](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Applicazione a pagina singola ASP.NET](#_Toc317096198)
    - [Miglioramenti ai modelli di progetto predefinito](#_Toc303253808)
    - [Modello di progetto per dispositivi mobili](#_Toc303253809)
    - [Modalità di visualizzazione](#_Toc303253810)
    - [jQuery Mobile, la selezione tipo di visualizzazione e si esegue l'override del Browser](#_Toc303253811)
    - [Ricette per la generazione di codice in Visual Studio](#_Toc303253812)
    - [Supporto di attività per i controller asincroni](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Problemi noti e modifiche di rilievo](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Note sull'installazione

ASP.NET MVC 4 Beta per Visual Studio 2010 possono essere installata dal [home page di ASP.NET MVC 4](../mvc/mvc4.md) usando l'installazione guidata piattaforma Web.

È necessario disinstallare le anteprime installate in precedenza di ASP.NET MVC 4 prima di installare ASP.NET MVC 4 Beta.

Questa versione non è compatibile con l'anteprima per sviluppatori di .NET Framework 4.5. Prima di installare la versione Beta di ASP.NET MVC 4, è necessario disinstallare l'anteprima per sviluppatori di .NET 4.5.

ASP.NET MVC 4 può essere installato e può eseguire side-by-side con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentazione

Documentazione per ASP.NET MVC è disponibile nel sito Web MSDN all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Esercitazioni e altre informazioni su ASP.NET MVC sono disponibili nella pagina del sito Web ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Supporto

Questa è una versione di anteprima e non è ufficialmente supportata. Se hai domande sull'uso di questa versione, pubblicare un post nel forum di ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), in cui i membri della community di ASP.NET sono spesso in grado di fornire supporto informale.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisiti software

I componenti ASP.NET MVC 4 per Visual Studio richiedono PowerShell 2.0 e Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>L'aggiornamento di un progetto ASP.NET MVC 3 ad ASP.NET MVC 4

ASP.NET MVC 4 può essere installato nello stesso computer, che offre flessibilità nella scelta del momento aggiornare un'applicazione ASP.NET MVC 3 ad ASP.NET MVC 4 affiancato con ASP.NET MVC 3.

Il modo più semplice per eseguire l'aggiornamento è per creare un nuovo progetto ASP.NET MVC 4 e copiare tutte le visualizzazioni, controller, codice e i contenuti i file dal progetto MVC 3 esistente al nuovo progetto e quindi aggiornare l'assembly fa riferimento nel nuovo progetto in modo che corrisponda il progetto precedente. Se sono state apportate modifiche al file Web. config del progetto MVC 3, è anche necessario unire queste modifiche nel file Web. config nel progetto MVC 4.

Per aggiornare manualmente un'applicazione ASP.NET MVC 3 esistente alla versione 4, eseguire le operazioni seguenti:

1. In tutti i file Web. config nel progetto (è disponibile nella radice del progetto, uno nella cartella Views e uno nella cartella Views di ciascuna area nel progetto), sostituire ogni istanza del testo seguente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    con il testo corrispondente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. Nel file Web. config radice, aggiornare il *webPages:Version* elemento "2.0.0.0" e aggiungere un nuovo *PreserveLoginUrl* chiave contenente il valore "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. In Esplora soluzioni, eliminare il riferimento a *System* (che punta alla versione 3 di DLL). Quindi aggiungere un riferimento a *System* (v4.0.0.0). In particolare, apportare le modifiche seguenti per aggiornare i riferimenti all'assembly. Di seguito sono riportati i dettagli:

    1. In Esplora soluzioni, eliminare i riferimenti agli assembly seguenti: 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. Aggiunta di riferimenti agli assembly seguenti: 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. In Esplora soluzioni fare clic sul nome del progetto e quindi Scarica progetto. Quindi fare doppio clic il nome del nuovo e selezionare Modifica *ProjectName*csproj.
5. Individuare il *ProjectTypeGuids* elemento e sostituire {E53F8FEA-EAE0-44A6-8774-FFD645390401} con {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) che si stava modificando, fare clic sul progetto e quindi scegliere Ricarica progetto.
7. Se il progetto fa riferimento a tutte le librerie di terze parti che vengono compilate usando versioni precedenti di ASP.NET MVC, aprire il file Web. config radice e aggiungere le tre *bindingRedirect* elementi sotto il  *configurazione* sezione: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nuove funzionalità in ASP.NET MVC 4 Beta

Questa sezione vengono descritte le funzionalità che sono state introdotte nella versione ASP.NET MVC 4 Beta.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API Web ASP.NET

ASP.NET MVC 4 oggi include API Web ASP.NET, un nuovo framework per la creazione di servizi HTTP che può raggiungere una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è anche una piattaforma ideale per creare servizi RESTful.

API Web ASP.NET include il supporto per le funzionalità seguenti:

- **Modello di programmazione HTTP moderni:** direttamente accedere e modificare le richieste HTTP e risposte in API Web usando un modello a oggetti fortemente tipizzato, nuovo HTTP. Lo stesso modello di programmazione e della pipeline HTTP è disponibile in modo simmetrico nel client tramite il nuovo tipo di HttpClient.
- **Supporto completo per le route**: Web API ora supportano il set completo di funzionalità di route che sono sempre state una parte dello stack Web, compresi i vincoli e i parametri di route. Inoltre, il mapping alle azioni fornisce supporto completo per le convenzioni, in modo che non è più necessario applicare gli attributi, ad esempio [HttpPost] alle classi e metodi.
- **Negoziazione del contenuto**: il client e server possono lavorare insieme per determinare il formato corretto per i dati restituiti da un'API. Microsoft offre supporto predefinito per XML, JSON e formati di codifica URL Form ed è possibile estendere questo supporto mediante l'aggiunta di formattatori personalizzati o anche sostituire la strategia di negoziazione del contenuto predefinita.
- **Associazione e la convalida del modello:** raccoglitori offrono un modo semplice per estrarre dati da varie parti di una richiesta HTTP e convertire le parti del messaggio in oggetti .NET che possono essere utilizzati per le azioni di API Web.
- **Filtri:** API Web supporta ora i filtri, inclusi filtri ben noti, ad esempio l'attributo [Authorize]. È possibile creare e collegare i propri filtri per azioni, autorizzazione e la gestione delle eccezioni.
- **Composizione della query:** restituendo semplicemente IQueryable&lt;T&gt;, l'API Web supporta l'esecuzione di query tramite le convenzioni degli URL OData.
- **Migliorare la testabilità dei dettagli HTTP:** anziché impostare i dettagli HTTP in oggetti di contesto statico, è ora possono utilizzare le azioni di API Web con le istanze di HttpRequestMessage e HttpResponseMessage. Esistono anche versioni generiche di questi oggetti per permettere l'utilizzo di tipi personalizzati oltre ai tipi di HTTP.
- **Inversion of Control (IoC) tramite DependencyResolver migliorata:** API Web ora utilizza il modello di indicatore di posizione del servizio implementato da resolver di dipendenza di MVC per ottenere le istanze per molti servizi diversi.
- **Configurazione basata su codice:** configurazione Web API avviene esclusivamente tramite il codice, uscire dal file di configurazione del file clean.
- **Self-hosting:** API Web possono essere ospitate nel proprio processo oltre a IIS continuando comunque a utilizzare tutta la potenza di route e altre funzionalità dell'API Web.

Per altre informazioni su API Web ASP.NET, visitare [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Applicazione a pagina singola ASP.NET

ASP.NET MVC 4 include ora un'anteprima dell'esperienza per la compilazione di applicazioni a pagina singola con significative interazioni lato client con JavaScript e le API Web. Questo supporto include:

- Un set di librerie JavaScript per le interazioni locale più avanzate con i dati memorizzati nella cache
- Componenti aggiuntivi di Web API per unità di lavoro e DAL supporto
- Un modello di progetto MVC con scaffolding per essere subito operativi

Per altre informazioni sull'applicazione a pagina singola supportano in ASP.NET MVC 4, visitare [ https://www.asp.net/single-page-application ](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Miglioramenti ai modelli di progetto predefinito

Il modello utilizzato per creare nuovi progetti ASP.NET MVC 4 è stato aggiornato per creare un sito Web più dall'aspetto moderno:

![](mvc4-beta-release-notes/_static/image1.png)

Oltre ai miglioramenti cosmetici, ha migliorato la funzionalità nel nuovo modello. Il modello Usa una tecnica denominata rendering adattivo una buona cosa sia nei browser desktop e browser per dispositivi mobili senza eseguire alcuna personalizzazione.

![](mvc4-beta-release-notes/_static/image2.png)

Per visualizzare il rendering adattivo in azione, è possibile usare un emulatore per dispositivi mobili o semplicemente provare a ridimensionare la finestra del browser desktop siano più piccoli. Quando la finestra del browser ottiene sufficientemente piccola, si passerà il layout della pagina.

Un altro miglioramento apportato al modello di progetto predefinito è l'uso di JavaScript per fornire un'interfaccia utente più completa. I collegamenti di log in and Register utilizzati nel modello sono esempi di come usare la finestra di dialogo dell'interfaccia utente di jQuery per presentare una schermata di accesso avanzati:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modello di progetto per dispositivi mobili

Se si inizia un nuovo progetto e si desidera creare un sito in modo specifico per dispositivi mobili e browser tablet, è possibile usare il nuovo modello di progetto di applicazione per dispositivi mobili. Si basa sulle jQuery Mobile, una libreria open source per la creazione dell'interfaccia utente ottimizzato per il tocco:

![](mvc4-beta-release-notes/_static/image4.png)

Questo modello contiene la stessa struttura dell'applicazione come modello di applicazione Internet (e il codice del controller è praticamente identico), ma è applicato lo stile tramite jQuery Mobile per l'aspetto e comportamento correttamente sui dispositivi mobili basati su touch. Per altre informazioni su come strutturare e definire lo stile dell'interfaccia utente per dispositivi mobili, vedere la [sito Web del progetto per dispositivi mobili jQuery](http://jquerymobile.com/).

Se si dispone già di un sito basata su desktop che si desidera aggiungere visualizzazioni ottimizzate per dispositivi mobili, o se si desidera creare un singolo sito che viene usato in modo diverso con stile viste nei browser desktop e per dispositivi mobili, è possibile usare la nuova funzionalità di modalità di visualizzazione. (Vedere la sezione successiva).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modalità di visualizzazione

La nuova funzionalità di modalità di visualizzazione consente a un'applicazione di selezionare le viste in base al browser che effettua la richiesta. Ad esempio, se il browser desktop richiede la Home page, l'applicazione potrebbe utilizzare il modello Views\Home\Index.cshtml. Se un browser per dispositivi mobili richiede la Home page, l'applicazione potrebbe restituire il modello Views\Home\Index.mobile.cshtml.

Layout e visualizzazioni parziali possono anche eseguire l'override per i tipi di browser specifico. Ad esempio:

- Se la cartella Views\Shared contiene entrambi le \_layout. cshtml e \_cshtml modelli, per impostazione predefinita l'applicazione utilizzerà \_cshtml durante le richieste dal browser per dispositivi mobili e \_Layout. cshtml durante le altre richieste.
- Se una cartella contiene entrambe \_MyPartial.cshtml e \_MyPartial.mobile.cshtml, l'istruzione @Html.Partial("\_MyPartial") verrà eseguito il rendering \_MyPartial.mobile.cshtml durante le richieste provenienti da dispositivi mobili i browser, e \_MyPartial.cshtml durante le altre richieste.

Se si desidera creare più specifiche viste, layout o visualizzazioni parziali per altri dispositivi, è possibile registrare una nuova *DefaultDisplayMode* istanza per specificare il nome da cercare quando una richiesta soddisfa determinate condizioni. Ad esempio, è possibile aggiungere il codice seguente per il *Application\_avviare* metodo nel file Global. asax per registrare la stringa "iPhone" come modalità di visualizzazione che si applica quando il browser di iPhone Apple effettua una richiesta:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Quando si esegue questo codice, quando un browser di iPhone Apple effettua una richiesta, l'applicazione userà il Views\Shared\\_Layout.iPhone.cshtml layout (se presente).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, la selezione tipo di visualizzazione e si esegue l'override del Browser

jQuery Mobile è una libreria open source per la creazione di interfaccia utente web ottimizzato per il tocco. Se si desidera usare jQuery Mobile con un'applicazione ASP.NET MVC 4, è possibile scaricare e installare un pacchetto NuGet che semplifica le operazioni iniziali. Per installarlo dalla Console di gestione pacchetti Visual Studio, digitare il comando seguente:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Vengono installati jQuery Mobile e alcuni file di supporto, inclusi i seguenti:

- Views/Shared/\_cshtml, ovvero un layout per jQuery Mobile.
- Un componente di cambio di visualizzazione, che è costituito ilViews/Shared/\_ViewSwitcher.cshtml visualizzazione parziale e il controller ViewSwitcherController.cs.

Dopo aver installato il pacchetto, eseguire l'applicazione usando un browser per dispositivi mobili (o equivalente, ad esempio il Firefox [cambio modalità per agente utente](http://chrispederick.com/work/user-agent-switcher/) componente aggiuntivo). Si noterà che le pagine apparire piuttosto diverse, poiché jQuery Mobile gestiti il layout e stile. Per sfruttare i vantaggi di questo, è possibile eseguire le operazioni seguenti:

- Creare le sostituzioni di visualizzazione specifici del dispositivo mobile, come descritto in [modalità di visualizzazione](#_Toc303253810) in precedenza (ad esempio, creare Views\Home\Index.mobile.cshtml per eseguire l'override Views\Home\Index.cshtml per i browser per dispositivi mobili).
- Leggere il [jQuery Mobile documentazione](http://jquerymobile.com/) per altre informazioni su come aggiungere touchscreen ottimizzata di elementi dell'interfaccia utente nelle visualizzazioni per dispositivi mobili.

Una convenzione per le pagine web ottimizzata per dispositivi mobili consiste nell'aggiungere un collegamento il cui testo è qualcosa di simile a visualizzazione Desktop o in modalità completa del sito che consente agli utenti di passare a una versione desktop della pagina. Il pacchetto jQuery.Mobile.MVC include un esempio di componente di cambio di visualizzazione per questo scopo. Viene utilizzato il valore predefinito Views\Shared\\_Layout.Mobile.cshtml visualizzazione, e ha un aspetto simile al seguente quando viene eseguito il rendering della pagina:

![](mvc4-beta-release-notes/_static/image5.png)

Se i visitatori fare clic sul collegamento, che si passasse alla versione desktop della stessa pagina.

Poiché i layout desktop non includerà un cambio di visualizzazione per impostazione predefinita, i visitatori non sarà necessario un modo per passare alla modalità mobile. A tale scopo, aggiungere il seguente riferimento a  *\_ViewSwitcher* al layout del desktop, solo all'interno di *&lt;body&gt;* elemento:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Il cambio di visualizzazione viene utilizzata una nuova funzionalità denominata eseguendo l'override del Browser. Questa funzionalità consente all'applicazione di considerare le richieste come provenienti da un browser (agente utente) diverso da quello sta effettivamente da. Nella tabella seguente elenca i metodi che si esegue l'override del Browser offre.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Esegue l'override del valore della richiesta degli utenti effettivi dell'agente tramite l'agente utente specificato. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Restituisce il valore di sostituzione dell'agente utente della richiesta o la stringa agente utente effettivo se non è stato specificato alcun override. |
| `HttpContext.GetOverriddenBrowser()` | Restituisce un *HttpBrowserCapabilitiesBase* istanza che corrisponde all'agente utente attualmente impostato per la richiesta (effettivo o sottoposto a override). È possibile utilizzare questo valore per ottenere le proprietà, ad esempio *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Rimuove qualsiasi agente utente sottoposto a override per la richiesta corrente. |

Override del browser è una funzionalità fondamentale di ASP.NET MVC 4 ed è disponibile anche se non si installa il pacchetto jQuery.Mobile.MVC. Tuttavia, interessa solo visualizzazione, layout e selezione di visualizzazione parziale, ovvero non influenza qualsiasi altra funzionalità di ASP.NET varia a seconda le *Request* oggetto.

Per impostazione predefinita, la sostituzione di agente utente verrà archiviata utilizzando un cookie. Se si desidera archiviare la sostituzione in un' posizione (ad esempio, in un database), è possibile sostituire il provider predefinito (*BrowserOverrideStores.Current*). Documentazione per il provider sarà disponibile a complemento di una versione successiva di ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Ricette per la generazione di codice in Visual Studio

La nuova funzionalità recipe consente a Visual Studio generare il codice specifico della soluzione basato su pacchetti che è possibile installare tramite NuGet. Il framework recipe rende più semplice per gli sviluppatori di scrivere plug-in generazione di codice, è anche possibile usare per sostituire i generatori di codice incorporato per Aggiungi Controller, Aggiungi Area e Aggiungi visualizzazione. Poiché le recipe vengono distribuite come pacchetti NuGet, possono facilmente essere archiviati nel controllo del codice sorgente e condivisa con tutti gli sviluppatori sul progetto automaticamente. Sono anche disponibili in base a una per ogni soluzione.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Supporto di attività per i controller asincroni

È ora possibile scrivere metodi di azione asincroni come singolo i metodi che restituiscono un oggetto di tipo *Task* oppure *Task&lt;ActionResult&gt;*.

Ad esempio, se si usa Visual c# 5 (o utilizzando il [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), è possibile creare un metodo di azione asincrona che appare come segue:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

Nel metodo di azione precedente, le chiamate a *GetHeadlinesAsync* e *sportsService.GetScoresAsync* vengono chiamati in modo asincrono e non bloccare un thread dal pool di thread.

Metodi di azione asincroni che restituiscono *attività* istanze possono anche supportare i timeout. Per rendere il metodo di azione annullabile, aggiungere un parametro di tipo *CancellationToken* alla firma del metodo di azione. Nell'esempio seguente viene illustrato un metodo di azione asincrona che ha un timeout di 2500 millisecondi e che visualizza un' *TimedOut* visualizzare al client se si verifica un timeout.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta supporta la versione di settembre 2011 1.5 di Windows Azure SDK.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

- **Dopo l'installazione di ASP.NET MVC 4 Beta, l'editor con estensione CSHTML o VBHTML nell'editor di Visual Studio 2010 Service Pack 1 CSHTML o VBHTML potrebbe sospendere per un lungo periodo dopo aver digitato frammento o JavaScript nei file con estensione cshtml o vbhtml.** Ciò si verifica solo in applicazioni ASP.NET MVC 4 che hanno stato appena create e non sono ancora state compilate.

    La soluzione alternativa consiste nel compilare il progetto per ottenere gli assembly nella cartella bin. Si noti che se si pulisce il progetto che rimuove gli assembly dalla cartella bin, il problema dell'editor verrà ripristinato.

    Questo verrà corretto nella prossima versione.
- **Modelli di progetto c# per Visual Studio 11 Beta contengono una stringa di connessione non corrette in Global.asax.cs.** La connessione predefinita specificata nell'applicazione\_metodo Start per i progetti creati in Visual Studio 11 Beta contengono una stringa di connessione di database locale che contiene una barra rovesciata senza escape (\) carattere. Ciò comporta un errore di connessione al momento i tentativi di accesso di un DbContext di Entity Framework, che genera un'eccezione SqlException.

    Per risolvere questo problema, escape del carattere di barra rovesciata nell'App\_Start (metodo) di Global.asax.cs perché possa essere letto come segue:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Le applicazioni ASP.NET MVC 4 che hanno come destinazione .NET 4.5, verranno generata una FileLoadException durante il tentativo di accedere all'assembly System.Net.Http.dll quando viene eseguito in .NET 4.0.** Nelle applicazioni ASP.NET MVC 4 create in .NET 4.5 è incluso un binding reindirizzamento che comporterà un FileLoadException che dichiara che "è stato possibile caricare file o l'assembly 'System' o una delle relative dipendenze." Quando l'applicazione viene eseguita in un sistema con .NET 4.0 installato. Per risolvere questo problema, rimuovere il reindirizzamento di associazione seguente dal file Web. config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    L'elemento di associazione di assembly in Web. config modificato deve apparire come segue:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Il modello di elemento "Aggiungi Controller" nei progetti Visual Basic genera uno spazio dei nomi non corretto quando viene richiamato</strong><strong>all'interno di un'area.</strong> Quando si aggiunge un controller a un'area in un progetto ASP.NET MVC che Usa Visual Basic, il modello di elemento inserisce il controller lo spazio dei nomi errato. Il risultato è un errore di "file non trovato" quando si passa a qualsiasi azione nel controller.  
  
  Lo spazio dei nomi generato omette tutti gli elementi dopo lo spazio dei nomi radice. Ad esempio, è lo spazio dei nomi generato *RootNamespace* , ma deve essere *RootNamespace.Areas.AreaName.Controllers* .
- **Modifiche di rilievo nel motore di visualizzazione Razor.** Come parte della riscrittura del parser Razor, i tipi seguenti sono state rimosse da *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  I metodi seguenti sono state inoltre rimosse: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Quando si WebMatrix.WebData.dll è incluso nella directory /bin di un'App ASP.NET MVC 4, subentra l'URL per l'autenticazione basata su form.** Aggiunta dell'assembly WebMatrix.WebData.dll all'applicazione (ad esempio, selezionando "Pagine Web ASP.NET con la sintassi delle Razor" quando si usa la finestra di dialogo Aggiungi dipendenze distribuibili) sostituirà il reindirizzamento di autenticazione account di accesso/account/accesso anziché / account/login come previsto dall'Account di Controller MVC di ASP.NET predefinito. Per evitare questo comportamento e usare l'URL specificato è già nella sezione autenticazione di Web. config, è possibile aggiungere un'impostazione App denominata PreserveLoginUrl e impostarlo su true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Gestione pacchetti NuGet non viene installato quando si prova a installare ASP.NET MVC 4 per installazioni side-by-side di Visual Studio 2010 e Visual Web Developer 2010.** Per eseguire Visual Studio 2010 e Visual Web Developer 2010 affiancato con ASP.NET MVC 4 è necessario installare ASP.NET MVC 4 dopo che sono già state installate entrambe le versioni di Visual Studio.
- **Disinstallazione di ASP.NET MVC 4 ha esito negativo se i prerequisiti sono già stati disinstallati.** Per disinstallare correttamente l'applicazione ASP.NET MVC 4you necessario disinstallare ASP.NET MVC 4 prima di disinstallare Visual Studio.
- **Esecuzione di un progetto API Web predefinito vengono visualizzate le istruzioni che indirizzano in modo non corretto all'utente di aggiungere le route usando il metodo RegisterApis, che non esiste.** Le route devono essere aggiunti nel metodo RegisterRoutes utilizzando la tabella di route ASP.NET.
- **Installazione di ASP.NET MVC 4 Beta interrompe le applicazioni ASP.NET MVC 3 RTM.** Le applicazioni ASP.NET MVC 3 che sono state create con la versione RTM versione (non con la versione di ASP.NET MVC 3 Tools Update) le seguenti modifiche per funzionare richiedono side-by-side con ASP.NET MVC 4 Beta. Compilazione del progetto senza apportare questi risultati degli aggiornamenti in errori di compilazione. 

    **Aggiornamenti necessari**

  1. Nel file Web. config radice, aggiungere una nuova *&lt;appSettings&gt;* voce con la chiave *webPages:Version* e il valore *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. In Esplora soluzioni fare clic sul nome del progetto e quindi Scarica progetto. Quindi fare doppio clic il nome del nuovo e selezionare Modifica *ProjectName*csproj.
  3. Individuare i riferimenti assembly seguenti: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Sostituirli con il codice seguente:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Salvare le modifiche, chiudere il file di progetto (con estensione csproj) la modifica, quindi fare clic sul progetto e scegliere Ricarica.
