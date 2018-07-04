---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET and Web Tools per Visual Studio 2013 Release Notes | Microsoft Docs
author: microsoft
description: Questo documento descrive la versione di ASP.NET and Web Tools per Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 94050d752348cadf4ed1044e6d8f96834ec981d8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370565"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET and Web Tools per Visual Studio 2013 Release Notes
====================
da [Microsoft](https://github.com/microsoft)

> Questo documento descrive la versione di ASP.NET and Web Tools per Visual Studio 2013.


## <a name="contents"></a>Sommario

- [Note sull'installazione](#TOC1)
- [Documentazione](#TOC2)
- [Requisiti software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuove funzionalità di ASP.NET and Web Tools per Visual Studio 2013

- [One ASP.NET](#TOC6)
- [Nuova esperienza di progetto Web](#newproj)
- [Scaffolding di ASP.NET](#scaffold)
- [Browser Link](#browser-link)
- [Miglioramenti dell'Editor Web di Visual Studio](#web-editor)
- [Supporto di App Web di servizio App di Azure in Visual Studio](#waws)
- [Miglioramenti di pubblicazione sul Web](#publish)
- [NuGet 2.7](#nuget)
- [Web Form ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [API Web ASP.NET 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Componenti Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [Sospensione di App ASP.NET](#TOC15)
- [Problemi noti e modifiche di rilievo](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Note sull'installazione

ASP.NET and Web Tools per Visual Studio 2013 sono inclusi nel programma di installazione principale e può essere scaricato [qui](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET and Web Tools per Visual Studio 2013 sono disponibili i [sito web ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Requisiti software

ASP.NET and Web Tools richiede Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuove funzionalità di ASP.NET and Web Tools per Visual Studio 2013

Le sezioni seguenti descrivono le funzionalità che sono state introdotte nella versione.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>One ASP.NET

Con la versione di Visual Studio 2013, abbiamo adottato un passo avanti per unificare l'esperienza di utilizzo di tecnologie ASP.NET, in modo che è facilmente possibile combinare e associare quelli desiderati. Ad esempio, è possibile avviare un progetto con MVC e facilmente aggiungere pagine Web Form al progetto in un secondo momento o eseguire lo scaffolding API Web in un progetto di Web Form. One ASP.NET riguarda rendendo più semplice per è come uno sviluppatore di eseguire le operazioni che preferisci in ASP.NET. Indipendentemente da quale tecnologia si sceglie, è possibile avere fiducia che si sta compilando il framework sottostante attendibile di One ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nuova esperienza di progetto Web

Sono state migliorate l'esperienza di creazione di nuovi progetti web in Visual Studio 2013. Nel **nuovo progetto Web ASP.NET** finestra di dialogo è possibile selezionare il tipo di progetto si desidera, configura qualsiasi combinazione di tecnologie (Web Form, MVC, Web API), configurare le opzioni di autenticazione e aggiunta un progetto unit test.

![Nuovo progetto ASP.NET](release-notes/_static/image1.png)

La nuova finestra di dialogo consente di modificare le opzioni di autenticazione predefinito per molti dei modelli. Ad esempio, quando si crea un progetto di Web Form ASP.NET è possibile selezionare una delle opzioni seguenti:

- Nessuna autenticazione
- Account utente individuali (appartenenza ASP.NET o log provider basati su social network in)
- Account dell'organizzazione (Active Directory in un'applicazione internet)
- Autenticazione di Windows (Active Directory in un'applicazione intranet)

![Opzioni di autenticazione](release-notes/_static/image2.png)

Per altre informazioni sul processo di nuovo per la creazione di progetti web, vedere [creazione di progetti Web ASP.NET in Visual Studio 2013](creating-web-projects-in-visual-studio.md). Per altre informazioni sulle nuove opzioni di autenticazione, vedere [ASP.NET Identity](#TOC8) più avanti in questo documento.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>Scaffolding di ASP.NET

Scaffolding di ASP.NET è un framework di generazione di codice per applicazioni Web ASP.NET. Semplifica inoltre aggiungere il codice boilerplate per il progetto che interagisce con un modello di dati.

Nelle versioni precedenti di Visual Studio, lo scaffolding era limitato ai progetti ASP.NET MVC. Con Visual Studio 2013, è ora possibile usare lo scaffolding per qualsiasi progetto ASP.NET, tra cui Web Form. Visual Studio 2013 non supporta attualmente la generazione di pagine per un progetto di Web Form, ma è comunque possibile usare lo scaffolding con Web Form mediante l'aggiunta di dipendenze MVC al progetto. Verrà aggiunto il supporto per la generazione di pagine per Web Form in un aggiornamento futuro.

Quando si usa lo scaffolding, si assicura che tutti i necessari le dipendenze siano installate nel progetto. Ad esempio, se si avvia con un progetto di Web Form ASP.NET e quindi Usa lo scaffolding per aggiungere un Controller API Web, i pacchetti NuGet necessari e i riferimenti vengono aggiunti al progetto automaticamente.

Per aggiungere lo scaffolding di MVC in un progetto di Web Form, aggiungere un **nuovo elemento di scaffolding** e selezionare **dipendenze MVC 5** nella finestra di dialogo. Sono disponibili due opzioni per lo scaffolding di MVC; Minimal e complete. Se si seleziona minima, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto. Se si seleziona l'opzione completa, vengono aggiunte le dipendenze minime, nonché i necessari file di contenuto per un progetto MVC.

Supporto per lo scaffolding di controller asincroni Usa le nuove funzionalità asincrone da Entity Framework 6.

Per altre informazioni ed esercitazioni, vedere [Panoramica di Scaffolding ASP.NET](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Collegamento browser-canale SignalR tra browser e Visual Studio

Il nuovo [collegamento del Browser](using-browser-link.md) funzionalità ti permette di connettere più browser a Visual Studio e aggiornarli tutti facendo clic su un pulsante sulla barra degli strumenti. È possibile connettersi più browser al sito di sviluppo, inclusi gli emulatori dei dispositivi mobili e fare clic su Aggiorna per aggiornare tutti i browser tutti nello stesso momento. Collegamento del browser espone anche un'API per consentire agli sviluppatori di scrivere le estensioni di collegamento del Browser.

![](release-notes/_static/image3.png)

Consentendo agli sviluppatori di sfruttare l'API di collegamento del Browser, è possibile creare scenari molto avanzati che supera i limiti tra Visual Studio e qualsiasi browser in cui è connesso. Web Essentials si avvale dell'API per creare un'esperienza integrata tra Visual Studio e strumenti per sviluppatori del browser, remoti controllano gli emulatori di dispositivi mobili e molto altro ancora.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Miglioramenti dell'Editor Web di Visual Studio

Visual Studio 2013 include un nuovo editor di codice HTML per i file Razor e file HTML nelle applicazioni web. Il nuovo editor HTML fornisce un singolo schema unificato basato su HTML5. Dispone di completamento parentesi graffa automatico, interfaccia utente di jQuery e AngularJS attributo IntelliSense, l'attributo di raggruppamento di IntelliSense, ID e nome della classe Intellisense e altri miglioramenti, tra cui prestazioni migliori, formattazione e degli smart tag.

Lo screenshot seguente illustra l'uso di Bootstrap attributo IntelliSense nell'editor HTML.

![IntelliSense nell'editor HTML](release-notes/_static/image4.png)

Visual Studio 2013 include anche con entrambi CoffeeScript e meno compilate editor. L'editor LESS viene fornito con tutte le funzionalità ad accesso sporadico dall'editor CSS e include Intellisense specifico per le variabili e "mixins" in tutti i meno documenti il @import catena.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Supporto di App Web di servizio App di Azure in Visual Studio

In Visual Studio 2013 con Azure SDK per .NET 2.2, è possibile usare **Esplora Server** interagire direttamente con le app web remota. È possibile accedere al proprio account Azure, creare nuove app web, configurare le app, visualizzare i log in tempo reale e altro ancora. Viene rilasciata provenienti subito dopo SDK 2.2, sarà possibile eseguire in modalità debug in modalità remota in Azure. La maggior parte delle nuove funzionalità per App Web di servizio App di Azure funziona anche in Visual Studio 2012 quando si installa la versione corrente di Azure SDK per .NET.

Per altre informazioni, vedere le seguenti risorse:

- [Creare un'app web ASP.NET in servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Miglioramenti di pubblicazione sul Web

Visual Studio 2013 include nuove e migliorate funzionalità di pubblicazione sul Web. Ecco alcune di esse:

- Facilmente [automatizzare la crittografia del file Web. config](https://go.microsoft.com/fwlink/?LinkId=325529). (Questo collegamento e i due seguenti puntano alla documentazione su MSDN che potrebbe non essere disponibili solo nelle fasi finali il giorno 10/17).
- Facilmente [automatizzare l'esecuzione di un'applicazione in modalità offline durante la distribuzione](https://go.microsoft.com/fwlink/?LinkId=325530).
- Configurare distribuzione Web per [usare checksum del file anziché Data ultima modifica](https://go.microsoft.com/fwlink/?LinkId=325531) per determinare quali file devono essere copiati nel server.
- Pubblicare velocemente i singoli file selezionati (tra cui Web. config) quando si Usa FTP o metodi di pubblicazione di sistema di file, nonché con distribuzione Web.

Per altre informazioni sulla distribuzione web ASP.NET, vedere [sito Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 include un set completo delle nuove funzionalità descritte in dettaglio all'indirizzo [note sulla versione per NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Questa versione di NuGet rimuove anche la necessità di fornire il consenso esplicito per funzionalità di ripristino di pacchetti di NuGet scaricare i pacchetti. Consenso (e la casella di controllo associata nella finestra di dialogo Preferenze di NuGet) vengono concesse tramite l'installazione di NuGet. Ripristino dei pacchetti semplicemente funziona ora per impostazione predefinita.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Web Form ASP.NET

### <a name="one-aspnet"></a>One ASP.NET

I modelli di progetto Web Form si integrano facilmente con la nuova esperienza di One ASP.NET. È possibile aggiungere il supporto MVC e API Web al progetto Web Form, ed è possibile configurare l'autenticazione tramite la creazione guidata progetto di One ASP.NET. Per altre informazioni, vedere [creazione di progetti Web ASP.NET in Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>Identità ASP.NET

I modelli di progetto Web Form supportano il nuovo framework di ASP.NET Identity. Inoltre, i modelli supportano ora la creazione di un progetto di Web Form intranet. Per altre informazioni, vedere [metodi di autenticazione](creating-web-projects-in-visual-studio.md#auth) nelle **creazione di progetti Web ASP.NET in Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Usano i modelli Web Form [Bootstrap](http://twitter.github.io/bootstrap/) per fornire un sofisticato e reattivo aspetto che è possibile personalizzare con facilità. Per altre informazioni, vedere [Bootstrap nei modelli di progetto web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

I modelli di progetto Web MVC si integrano facilmente con la nuova esperienza di One ASP.NET. È possibile personalizzare il progetto MVC e configurare l'autenticazione mediante la creazione guidata progetto di One ASP.NET. Un'esercitazione introduttiva su ASP.NET MVC 5 reperibili [Introduzione a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Per informazioni sull'aggiornamento di progetti MVC 4 a MVC 5, vedere [come aggiornare un ASP.NET MVC 4 e un progetto API Web ASP.NET MVC 5 e API Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>Identità ASP.NET

I modelli di progetto MVC sono stati aggiornati per usare ASP.NET Identity per l'autenticazione e la gestione delle identità. Un'esercitazione con l'autenticazione di Facebook e Google e la nuova API di appartenenza reperibili [creare un'App ASP.NET MVC 5 con Facebook e Google OAuth2 e OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) e [creare un'app ASP.NET MVC con autenticazione e Database SQL e distribuirla nel servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

Il modello di progetto MVC è stato aggiornato per usare [Bootstrap](http://getbootstrap.com/) per fornire un sofisticato e reattivo aspetto che è possibile personalizzare con facilità. Per altre informazioni, vedere [Bootstrap nei modelli di progetto web Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtri di autenticazione

I filtri di autenticazione sono un nuovo tipo di filtro in ASP.NET MVC che vengono eseguiti prima dei filtri di autorizzazione nella pipeline ASP.NET MVC e consentono di specificare l'autenticazione per la logica per ogni azione, per ogni controller o a livello globale per tutti i controller. Filtri di autenticazione elaborano credenziali nella richiesta e forniscono un'entità corrispondente. Filtri di autenticazione possono anche aggiungere richieste di autenticazione in risposta a richieste non autorizzate.

### <a name="filter-overrides"></a>Override del filtro

È ora possibile ignorare i filtri da applicare a un metodo di azione specificato o un controller, specificando un filtro di override. I filtri di sostituzione specificano un set di tipi di filtro che non devono essere eseguiti per un determinato ambito (azione o controller). In questo modo è possibile configurare i filtri che si applicano a livello globale ma quindi escludere determinati filtri globali dell'applicazione a controller o azioni specifiche.

### <a name="attribute-routing"></a>Routing con attributi

ASP.NET MVC supporta ora il routing con attributi, grazie a un contributo da Tim McCall, l'autore del [ http://attributerouting.net ](http://attributerouting.net). Con il routing con attributi è possibile specificare le route annotando le azioni e controller.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Routing con attributi

API Web ASP.NET supporta ora il routing con attributi, grazie a un contributo da Tim McCall, l'autore del [ http://attributerouting.net ](http://attributerouting.net). Con il routing con attributi è possibile specificare le route di API Web annotando le azioni e controller simile al seguente:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Routing con attributi offre maggiore controllo sugli URI nell'API web. Ad esempio, è possibile definire con facilità una gerarchia di risorse usando un singolo controller API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Anche il routing con attributi offre una comoda sintassi che consentono di specificare i parametri facoltativi, i valori predefiniti e i vincoli di route:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Per altre informazioni sul routing con attributi, vedere [Routing con attributi nell'API Web 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

I modelli di progetto API Web e applicazione a pagina singola supportano ora l'autorizzazione con OAuth 2.0. OAuth 2.0 è un framework per autorizzare l'accesso client alle risorse protette. Funziona per un'ampia gamma di client, inclusi browser e dispositivi mobili.

Supporto per OAuth 2.0 si basa sul nuovo middleware di sicurezza fornito dai componenti Microsoft OWIN per l'autenticazione della connessione e l'implementazione del server autorizzazione. In alternativa, i client possono essere autorizzati tramite un server di autorizzazione dell'organizzazione, ad esempio Azure Active Directory o ADFS in Windows Server 2012 R2.

### <a name="odata-improvements"></a>Miglioramenti di OData

**Espandere il supporto per $select, $, $batch e $value**

ASP.NET Web API OData include ora il supporto completo per $select, $expand e $value. È anche possibile usare $batch per la richiesta di invio in batch e l'elaborazione di insiemi di modifiche.

Opzioni di $select e $expand consentono la modifica della forma dei dati restituiti da un endpoint OData. Per altre informazioni, vedere [Introducing $select e $expand supporto in Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Estensibilità migliorata**

I formattatori OData sono ora estendibili. È possibile aggiungere metadati della voce Atom, supporta voci di collegamento denominate flusso ed elementi multimediali, aggiungere le annotazioni di istanza e Personalizza modalità di generazione di collegamenti.

**Senza tipo di supporto**

È ora possibile compilare servizi OData senza la necessità di definire i tipi CLR per tipi di entità. Al contrario, i controller OData possono accettano o restituiscono istanze di **IEdmObject**, quali sono i formattatori OData serializzare o deserializzare.

**Riutilizzare un modello esistente**

Se si dispone già di un entity data model (EDM) esistente, è possibile ora riutilizzarlo direttamente, anziché compilarne uno nuovo. Ad esempio, se si usa Entity Framework, è possibile utilizzare il modello EDM in cui Entity Framework crea automaticamente.

### <a name="request-batching"></a>Richiedere l'invio in batch

Richiesta di invio in batch combina più operazioni in una singola richiesta HTTP POST, per ridurre il traffico di rete e fornire una semplice e meno interfaccia utente "frammentate". API Web ASP.NET supporta ora diverse strategie per la richiesta batch:

- Usare l'endpoint $batch di un servizio OData.
- Creare un pacchetto più richieste in un'unica richiesta multipart MIME.
- Usare un formato personalizzato di invio in batch.

Per abilitare la richiesta di invio in batch, è sufficiente aggiungere una route con un gestore di invio in batch per la configurazione dell'API Web:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

È inoltre possibile controllare se le richieste o eseguiti in sequenza o in qualsiasi ordine.

### <a name="portable-aspnet-web-api-client"></a>Portabile ASP.NET Web API Client

È ora possibile usare il Client di API Web ASP.NET per creare librerie di classi portabili che funzionano tra le applicazioni Windows Store e Windows Phone 8. È anche possibile creare i formattatori portabili che possono essere condivisi tra client e server.

### <a name="improved-testability"></a>Testabilità migliorata

Web API 2 rende molto più semplice per unit test i controller API. Solo creare un'istanza del controller API con il messaggio di richiesta e la configurazione e quindi chiamare il metodo di azione che si desidera eseguire il test. È anche facile da simulare il **UrlHelper** (classe), per i metodi di azione che esegue la generazione di collegamenti.

### <a name="ihttpactionresult"></a>IHttpActionResult

È ora possibile implementare IHttpActionResult per incapsulare il risultato di metodi di azione API Web. Un IHttpActionResult restituiti da un metodo di azione API Web viene eseguito dal runtime di ASP.NET Web API per produrre il messaggio di risposta risultante. Un IHttpActionResult può essere restituito da qualsiasi azione API Web per semplificare l'unit test dell'implementazione di API Web. Per maggiore praticità che viene fornito un numero di implementazioni IHttpActionResult con risultati per la restituzione di codici di stato specifici anomali, formattazione risposte contenute o sottoposti alla negoziazione del contenuto.

### <a name="httprequestcontext"></a>HttpRequestContext

Il nuovo **HttpRequestContext** rileva qualsiasi stato che è associato alla richiesta ma non è immediatamente disponibile dalla richiesta. Ad esempio, è possibile usare la **HttpRequestContext** per ottenere i dati di route, l'entità associata alla richiesta, il certificato client, il **UrlHelper** e la radice del percorso virtuale. È possibile creare facilmente un' **HttpRequestContext** per unità a scopo di test.

Poiché l'entità per la richiesta viene passata con la richiesta invece di affidarsi **thread. CurrentPrincipal**, l'entità è ora disponibile tutta la durata della richiesta mentre è in pipeline API Web.

### <a name="cors"></a>CORS

Grazie a un altro notevole contributo da Brock Allen, ASP.NET supporta ora il Multiorigine richiesta Sharing (CORS).

Protezione del browser impedisce a una pagina web da creare richieste AJAX a un altro dominio. [CORS](http://www.w3.org/TR/cors/) è uno standard W3C che consente a un server di ridurre i criteri di corrispondenza dell'origine. Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre.

API Web 2 supporta ora CORS, tra cui la gestione automatica delle richieste preliminari. Per altre informazioni, vedere [l'abilitazione di richieste Multiorigine nell'API Web ASP.NET](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtri di autenticazione

I filtri di autenticazione sono un nuovo tipo di filtro nell'API Web ASP.NET che vengono eseguiti prima dei filtri di autorizzazione nella pipeline ASP.NET Web API e consentono di specificare l'autenticazione per la logica per ogni azione, per ogni controller o a livello globale per tutti i controller. Filtri di autenticazione elaborano credenziali nella richiesta e forniscono un'entità corrispondente. Filtri di autenticazione possono anche aggiungere richieste di autenticazione in risposta a richieste non autorizzate.

### <a name="filter-overrides"></a>Filtrare le sostituzioni

È ora possibile ignorare i filtri da applicare a un metodo di azione specificato o un controller, specificando un filtro di override. I filtri di sostituzione specificano un set di tipi di filtro che non deve essere eseguito per un determinato ambito (azione o controller). In questo modo è possibile aggiungere i filtri globali, ma escludere quindi alcuni dal controller o azioni specifiche.

### <a name="owin-integration"></a>Integrazione di OWIN

ASP.NET Web API ora completamente supporta OWIN e può essere eseguito in qualsiasi host che supporta OWIN. È inoltre incluso un **HostAuthenticationFilter** che offre l'integrazione con il sistema di autenticazione OWIN.

Con l'integrazione di OWIN, è possibile self-ospitare API Web nel proprio processo insieme ad altre applicazioni middleware OWIN, quali SignalR. Per altre informazioni, vedere [Usa OWIN all'API Web ASP.NET del](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

Le sezioni seguenti descrivono le funzionalità di SignalR 2.0.

- [Basato su OWIN](#builtonowin)
- [MapHubs e MapConnection sono ora MapSignalR](#MapSignalR)
- [Supporto tra domini](#crossdomain)
- [iOS e Android supportano tramite MonoTouch e MonoDroid](#mobile)
- [Client portabile .NET](#portable)
- [Nuovo pacchetto di self-hosting](#selfhost)
- [Supporto del server compatibili](#backwardcompat)
- [Rimosso il supporto di server per .NET 4.0](#remove40)
- [Invia un messaggio a un elenco di client e i gruppi](#messagelist)
- [Invia un messaggio a un utente specifico](#sendtouser)
- [Migliore supporto della gestione degli errori](#errorhandling)
- [Unità semplice degli hub di test](#unittesting)
- [Gestione degli errori JavaScript](#javascripterror)

Per un esempio di come aggiornare un progetto 1.x esistente a SignalR 2.0, vedere [l'aggiornamento di un SignalR 1.x progetto](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Basato su OWIN

SignalR 2.0 si basa completamente d' [OWIN (Open Web Interface for .NET)](http://owin.org/). Questa modifica rende il processo di installazione per SignalR molto più coerente tra le applicazioni di SignalR self-hosted e ospitato sul web, ma è anche necessario un numero di modifiche apportate all'API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs e MapConnection sono ora MapSignalR

Per garantire la compatibilità con gli standard OWIN, questi metodi sono stati rinominati in `MapSignalR`. `MapSignalR` chiamata senza parametri verranno eseguito il mapping di tutti gli hub (come `MapHubs` versione 1.x); per eseguire il mapping singoli **PersistentConnection** oggetti, specificare il tipo di connessione come parametro di tipo e l'estensione di URL per la connessione come il primo argomento.

Il `MapSignalR` viene chiamato in una classe di avvio Owin. Visual Studio 2013 contiene un nuovo modello per una classe di avvio Owin; Per usare questo modello, effettuare le operazioni seguenti:

1. Pulsante destro del mouse sul progetto
2. Selezionare **aggiungere**, **nuovo elemento...**
3. Selezionare **Owin Startup class**. Denominare la nuova classe **Startup.cs**.

In un **applicazione Web** la classe di avvio Owin contenente il `MapSignalR` metodo viene quindi aggiunto al processo di avvio di Owin tramite una voce nel nodo delle impostazioni dell'applicazione del file Web. config, come illustrato di seguito.

In un **self-hosted application**, la classe di avvio viene passata come parametro di tipo di `WebApp.Start` (metodo).

**Mapping di hub e connessioni in SignalR 1.x (dal file dell'applicazione globale in un'applicazione web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Mapping di hub e connessioni in SignalR 2.0 (da un file di classe Owin Startup):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

In un **self-hosted application**, la classe di avvio viene passata come parametro di tipo per il `WebApp.Start` metodo, come illustrato di seguito.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Supporto tra domini

In SignalR 1.x, richieste tra domini sono state controllate da un singolo flag EnableCrossDomain. Questo flag controllata richieste sia JSONP e la condivisione CORS. Per una maggiore flessibilità, supporto di CORS tutti è stata rimossa dal componente server di SignalR (JavaScript lients comunque usare CORS in genere se è stato rilevato che il browser supporta lo), e nuovi middleware OWIN è stato reso disponibile per supportare questi scenari.

In SignalR 2.0 se JSONP è necessario nel client (per supportare le richieste tra domini in browser meno recenti), è necessario abilitare in modo esplicito, impostando `EnableJSONP` nella `HubConfiguration` oggetto `true`, come illustrato di seguito. JSONP è disabilitata per impostazione predefinita, perché è meno sicuro rispetto a CORS.

Per aggiungere il middleware CORS nuovo in SignalR 2.0, aggiungere il `Microsoft.Owin.Cors` libreria al progetto e chiamate `UseCors` prima il middleware di SignalR, come illustrato nella sezione seguente.

**Aggiunta di owin al progetto**: per installare questa libreria, eseguire il comando seguente nella Console di gestione pacchetti:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Questo comando consente di aggiungere la versione 2.0.0 versione del pacchetto al progetto.

**Chiamare UseCors**

I frammenti di codice seguente viene illustrato come implementare le connessioni tra domini in SignalR 1.x e 2.0.

**Implementazione di richieste tra domini in SignalR 1.x (dal file dell'applicazione globale)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementazione di richieste tra domini in SignalR 2.0 (da un file di codice c#)**

Il codice seguente illustra come abilitare JSONP o CORS in un progetto SignalR 2.0. Questo esempio di codice viene utilizzato `Map` e `RunSignalR` invece di `MapSignalR`, in modo che il middleware CORS viene eseguita solo per le richieste di SignalR che richiedono il supporto CORS (invece che per tutto il traffico nel percorso specificato `MapSignalR`.) `Map` utilizzabili per qualsiasi altro middleware che deve essere eseguita per un prefisso URL specifico, anziché per l'intera applicazione.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS e Android supportano tramite MonoTouch e MonoDroid

È stato aggiunto il supporto per iOS e Android client che usano MonoTouch e MonoDroid componenti dal [libreria Xamarin](https://xamarin.com/). Per altre informazioni su come usarli, vedere [usando i componenti di Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Questi componenti saranno disponibili nel [Store Xamarin](https://store.xamarin.com/) quando la versione di SignalR RTW sarà disponibile.

<a id="portable"></a> # # # Portabile .NET client

Per ottenere una migliore semplificano lo sviluppo multipiattaforma, di Silverlight, WinRT e i client di Windows Phone sono stati sostituiti con un singolo client portabili .NET che supporta le piattaforme seguenti:

- NET 4.5
- Silverlight 5
- WinRT (.NET per App di Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nuovo pacchetto di self-hosting

È ora disponibile un pacchetto NuGet per renderne più semplice iniziare a usare hosting indipendente di SignalR (SignalR applicazioni ospitate in un processo o un'altra applicazione, anziché ospitato in un server web). Per aggiornare un progetto self-hosting compilato con SignalR 1.x, rimuovere il pacchetto Microsoft.AspNet.SignalR.Owin e aggiungere il pacchetto Microsoft.AspNet.SignalR.SelfHost. Per altre informazioni su come iniziare con il pacchetto Self-hosting, vedere [esercitazione: hosting indipendente di SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Supporto del server compatibili

Nelle versioni precedenti di SignalR, le versioni del pacchetto SignalR utilizzati nel client e server devono essere identici. Per supportare applicazioni thick client che sarebbero difficile aggiornare, SignalR 2.0 ora supporta l'utilizzo di una versione più recente di server con un client precedente. **Nota: SignalR 2.0 non supporta i server compilati con versioni precedenti con i client più recenti.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Rimosso il supporto di server per .NET 4.0

SignalR 2.0 ha eliminato il supporto per l'interoperabilità di server con .NET 4.0. .NET 4.5 deve essere usato con i server di SignalR 2.0. È ancora presente un client .NET 4.0 per SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Invia un messaggio a un elenco di client e i gruppi

In SignalR 2.0, è possibile inviare un messaggio utilizzando un elenco di client e ID del gruppo. I frammenti di codice seguente viene illustrato come eseguire questa operazione.

**Invia un messaggio a un elenco di client e i gruppi usando PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Invia un messaggio a un elenco di client e i gruppi di hub di**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Invia un messaggio a un utente specifico

Questa funzionalità consente agli utenti di specificare che cos'è l'ID utente basato su un IRequest tramite una nuova interfaccia IUserIdProvider:

**L'interfaccia IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Per impostazione predefinita vi sarà un'implementazione che usa IPrincipal.Identity.Name dell'utente come nome utente.

Nell'hub, sarà in grado di inviare messaggi a questi utenti tramite una nuova API:

**Usando l'API Clients.User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Migliore supporto della gestione degli errori

Gli utenti possono ora generare **HubException** da qualsiasi chiamata dell'hub. Il costruttore del **HubException** può richiedere un messaggio stringa e un oggetto dati aggiuntivi errore. SignalR verrà auto-serializzare l'eccezione e inviarlo al client in cui si consentirà di rifiuto o meno la chiamata al metodo dell'hub.

Il **mostrano le eccezioni dell'hub dettagliati** impostazione non ha alcun effetto sui **HubException** viene inviato al client o meno; viene sempre inviato.

**Codice lato server che illustra l'invio di un HubException al client**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Codice JavaScript client dimostrazione risponde a un HubException inviati dal server**

[!code-html[Main](release-notes/samples/sample16.html)]

**Codice client .NET che illustrano risponde a un HubException inviati dal server**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Unità semplice degli hub di test

SignalR 2.0 include un'interfaccia denominata `IHubCallerConnectionContext` sugli hub che rende più semplice creare le chiamate sul lato client fittizio. I frammenti di codice seguenti illustrano l'utilizzo di questa interfaccia con più diffusi test harness [xUnit.net](https://github.com/xunit/xunit) e [moq](https://code.google.com/p/moq/).

**Unit test di SignalR con xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Unit test di SignalR con moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Gestione degli errori JavaScript

In SignalR 2.0, tutte le richiamate di gestione degli errori JavaScript restituiscono oggetti errore JavaScript anziché stringhe non elaborate. In questo modo SignalR far fluire le informazioni più dettagliate per i gestori degli errori. È possibile ottenere l'eccezione interna dal `source` proprietà dell'errore.

**Codice JavaScript client che gestisce l'eccezione Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>Identità ASP.NET

### <a name="new-aspnet-membership-system"></a>Nuovo sistema di appartenenze ASP.NET

ASP.NET Identity è il nuovo sistema di appartenenza per le applicazioni ASP.NET. ASP.NET Identity rende più facile l'integrazione dei dati di profili specifici dell'utente con i dati dell'applicazione. ASP.NET Identity consente inoltre di scegliere il modello di persistenza per i profili utente nell'applicazione. È possibile archiviare i dati in un database di SQL Server o un altro archivio dati, inclusi gli archivi dati NoSQL, ad esempio tabelle di archiviazione di Azure. Per altre informazioni, vedere [account utente individuali](creating-web-projects-in-visual-studio.md#indauth) nelle **creazione di progetti Web ASP.NET in Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Autenticazione basata sulle attestazioni

ASP.NET supporta ora l'autenticazione basata su attestazioni, in cui l'identità dell'utente viene rappresentato come un set di attestazioni da un'autorità emittente attendibile. Gli utenti possono essere autenticati usando un nome utente e password gestiti in un database dell'applicazione o tramite i provider di identità di social networking (ad esempio: Accounts Microsoft, Facebook, Google, Twitter), o usando gli account dell'organizzazione tramite Azure Active Directory o Active Directory Federation Services (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integrazione con Azure Active Directory e Windows Server Active Directory

È ora possibile creare progetti ASP.NET che usano Azure Active Directory o Windows Server Active Directory (AD) per l'autenticazione. Per altre informazioni, vedere [gli account aziendali](creating-web-projects-in-visual-studio.md#orgauth) nelle **creazione di progetti Web ASP.NET in Visual Studio 2013**.

### <a name="owin-integration"></a>Integrazione di OWIN

Autenticazione ASP.NET ora si basa sul middleware OWIN che può essere usato in qualsiasi host basati su OWIN. Per altre informazioni su OWIN, vedere gli argomenti seguenti [componenti Microsoft OWIN](#TOC7) sezione.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Componenti Microsoft OWIN

[Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web. OWIN consente di disaccoppiare l'applicazione web dal server, rendendo web applicazioni indipendente dall'host. Ad esempio, è possibile ospitare un'applicazione web basata su OWIN in IIS o Self-hosting in un processo personalizzato.

Modifiche introdotte nei componenti Microsoft OWIN (noto anche come il progetto Katana) includono nuovi componenti server e host, le nuove librerie helper e middleware e nuovo di middleware di autenticazione.

Per altre informazioni su OWIN e Katana, vedere [nuove funzionalità di OWIN e Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Nota: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) eseguire le applicazioni in modalità classica IIS; devono essere eseguite in modalità integrata.**

**Nota: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) le applicazioni devono essere eseguite con attendibilità totale.**

### <a name="new-servers-and-hosts"></a>Gli host e i nuovi server

Con questa versione sono stati aggiunti nuovi componenti per abilitare gli scenari self-hosting. Questi componenti includono i pacchetti NuGet seguenti:

- **Microsoft.Owin.Host.HttpListener**. Fornisce un server OWIN che utilizza **HttpListener** per ascoltare le richieste HTTP e indirizzarli alla pipeline OWIN.
- **Owin** fornisce una libreria per gli sviluppatori che desiderano una pipeline OWIN in un processo personalizzato, ad esempio un'applicazione console o il servizio Windows di host indipendente.
- **OwinHost**. Fornisce un file eseguibile autonomo che esegue il wrapping `Microsoft.Owin.Hosting` e consente di self-hosting una pipeline OWIN senza la necessità di scrivere un'applicazione host personalizzata.

Inoltre, il `Microsoft.Owin.Host.SystemWeb` pacchetto, è ora possibile middleware per specificare gli hint per la **SystemWeb** server, che indica che il middleware deve essere chiamato durante una determinata fase della pipeline ASP.NET. Questa funzionalità è particolarmente utile per il middleware di autenticazione, che deve essere eseguito nelle prime fasi della pipeline ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Middleware e librerie di supporto

Sebbene sia possibile scrivere i componenti OWIN usando solo le definizioni di funzione e il tipo della specifica di OWIN, il nuovo `Microsoft.Owin` pacchetto fornisce un insieme di astrazioni più semplici da usare. Questo pacchetto combina diversi pacchetti precedenti (ad esempio, `Owin.Extensions`, `Owin.Types`) in un modello a oggetti single, ben strutturati che può quindi essere utilizzato facilmente da altri componenti OWIN. In effetti, la maggior parte dei componenti Microsoft OWIN ora usano questo pacchetto.

> [!NOTE]
> [OWIN](http://www.owin.org) eseguire le applicazioni in modalità classica IIS; devono essere eseguite in modalità integrata.

> [!NOTE]
> [OWIN](http://www.owin.org) le applicazioni devono essere eseguite con attendibilità totale.

Questa versione include anche il pacchetto Microsoft.Owin.Diagnostics, che include middleware per convalidare un'applicazione OWIN in esecuzione, il middleware di pagina di errore consentono di analizzare gli errori.

### <a name="authentication-components"></a>Componenti di autenticazione

Sono disponibili i seguenti componenti di autenticazione.

- **Microsoft.Owin.Security.ActiveDirectory**. Abilita l'autenticazione con servizi di directory in locale o basato sul cloud.
- **Microsoft.Owin.Security.Cookies** Abilita l'autenticazione tramite cookie. Questo pacchetto era denominato precedentemente `Microsoft.Owin.Security.Forms`.
- **Owin** Abilita l'autenticazione con servizio basato su OAuth di Facebook.
- **Microsoft.Owin.Security.Google** Abilita l'autenticazione con servizio basato su OpenID di Google.
- **Microsoft.Owin.Security.Jwt** Abilita l'autenticazione tramite token JWT.
- **Microsoft.Owin.Security.MicrosoftAccount** Abilita l'autenticazione con account Microsoft.
- **Microsoft.Owin.Security.OAuth**. Fornisce un server di autorizzazione OAuth, nonché middleware per l'autenticazione di token di connessione.
- **Microsoft.Owin.Security.Twitter** Abilita l'autenticazione con servizio basato su OAuth di Twitter.

Questa versione include anche il `Microsoft.Owin.Cors` pacchetto che contiene il middleware per l'elaborazione delle richieste HTTP cross-origin.

> [!NOTE]
> Supporto per la firma del token JWT è stato rimosso nella versione finale di Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Per un elenco delle nuove funzionalità e altre modifiche in Entity Framework 6, vedere [cronologia delle versioni di Entity Framework](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 include le nuove funzionalità seguenti:

- Supporto per la modifica della scheda. Preivously, il **Formatta documento** comando, il rientro automatico e automatica di formattazione in Visual Studio non funzionava correttamente quando si usa la **Mantieni tabulazioni** opzione. Questa modifica corregge formattazione codice Razor per scheda di formattazione di Visual Studio.
- Supporto per le regole di riscrittura dell'URL durante la generazione di collegamenti.
- Rimozione dell'attributo di trasparenza di sicurezza.
  > [!NOTE]
  > Questa è una modifica di rilievo e rende Razor 3 incompatibile con MVC4 e versioni precedenti, mentre 2 Razor non è compatibile con MVC 5 o assembly compilato per MVC 5.

Problemi di Razor 3 risolti in Visual Studio 2013 da versioni non definitive sono reperibili [qui](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Sospensione di App ASP.NET

App Suspend ASP.NET è una funzionalità straordinaria di .NET Framework 4.5.1 che cambia radicalmente l'esperienza utente e modello economico per l'hosting di un numero elevato di siti ASP.NET in un singolo computer. Per altre informazioni, vedere [App Suspend ASP.NET: reattiva shared hosting web .NET](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti problemi noti e modifiche di rilievo in ASP.NET e Web Tools per Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Nuovo ripristino dei pacchetti non funziona su Mono tramite file. SLN](https://nuget.codeplex.com/workitem/3596) : verrà risolto in un download nuget.exe imminenti e [pacchetto NuGet. CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) aggiornare.
- [Nuovo ripristino dei pacchetti non funziona con i progetti di Wix](https://nuget.codeplex.com/workitem/3598) : verrà risolto in un download nuget.exe imminenti e [pacchetto NuGet. CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) aggiornare.
- [Ripristino automatico del pacchetto non funziona per i progetti in una cartella della soluzione](https://nuget.codeplex.com/workitem/3625) – verranno risolti in NuGet 2.8.

### <a name="aspnet-web-api"></a>API Web ASP.NET

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` non restituisce `IQueryable<T>` sempre, come è stato aggiunto il supporto per `$select` e `$expand`.

    Gli esempi precedenti per `ODataQueryOptions<T>` sempre sottoposto a cast il valore restituito da `ApplyTo` a `IQueryable<T>`. L'azione in precedenza è riuscita perché le opzioni di query che è supportato in precedenza (`$filter`, `$orderby`, `$skip`, `$top`) non modificare la forma della query. Ora che Supportiamo `$select` e `$expand` il valore restituito da `ApplyTo` non saranno `IQueryable<T>` sempre.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Se si usa il codice di esempio illustrato in precedenza, continueranno a funzionare se il client non invii `$select` e `$expand`. Tuttavia, se si desidera supportare `$select` e `$expand` è necessario modificare tale codice a questo.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url o RequestContext.Url è null durante una richiesta batch**

    In uno scenario di invio in batch **UrlHelper** è null quando si accede da **Request.Url** oppure **RequestContext.Url**.

    Questo problema viene attualmente controllato qui: [BatchRequestContext.Url è null per la richiesta di invio in batch](http://aspnetwebstack.codeplex.com/workitem/1301).

    La soluzione alternativa per risolvere questo problema consiste nel creare una nuova istanza della **UrlHelper**, come illustrato nell'esempio seguente:

    **Creazione di una nuova istanza della UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Quando si usa MVC 5 e OrgAuth, se le viste che consentono di eseguire convalide AntiForgerToken, potrebbe riscontrare l'errore seguente quando si registrano i dati alla visualizzazione:

    **Errore**:

    *Errore del server nell'applicazione '/'.*

    <em>Un'attestazione di tipo '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'o'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' non era presente sull'oggetto ClaimsIdentity specificato. Per abilitare il supporto di token antifalsificazione con l'autenticazione basata su attestazioni, verificare che il provider di attestazioni configurato fornisce entrambe queste attestazioni nelle istanze di oggetto ClaimsIdentity che genera l'errore. Se il provider di attestazioni configurato Usa invece un tipo di attestazione diversi come identificatore univoco, può essere configurato impostando la proprietà statica AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Soluzione alternativa**:

    Aggiungere la riga seguente in Global. asax per risolvere il problema:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Questo problema verrà risolto per la versione successiva.
2. Dopo l'aggiornamento di un'applicazione MVC4 a MVC 5, compilare la soluzione e avviare il programma. Viene visualizzato l'errore seguente:

    [A] Non è possibile eseguire il cast System.Web.WebPages.Razor.Configuration.HostSection [B]System.Web.WebPages.Razor.Configuration.HostSection. Tipo deriva da ' webpages, Version = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' nel contesto 'Default' nella posizione ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'. Tipo B deriva da ' webpages, versione = 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' nel contesto 'Default' nella posizione ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.

    Per correggere l'errore riportato sopra, aprire *tutti* i file Web. config (inclusi quelli presenti nella cartella Views) nel progetto e procedi nel modo seguente:

   1. Aggiornare tutte le occorrenze della versione "4.0.0.0" di "System" a "versione=5.0.0.0".
   2. Aggiornare tutte le occorrenze della versione "2.0.0.0" di "Spazi", &quot;System.Web.WebPages&quot; e &quot;webpages&quot; per "3.0.0.0"

      Ad esempio, dopo aver apportato le suddette modifiche, le associazioni di assembly dovrebbero essere simile al seguente:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Per informazioni sull'aggiornamento di progetti MVC 4 a MVC 5, vedere [come aggiornare un ASP.NET MVC 4 e un progetto API Web ASP.NET MVC 5 e API Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Quando si usa la convalida lato client jQuery Unobtrusive Validation, il messaggio di convalida in alcuni casi non è corretto per un elemento HTML input con tipo = 'number'. L'errore di convalida per un valore obbligatorio ("età il campo è obbligatorio") viene visualizzato quando viene immesso un numero non valido anziché il messaggio corretto che è necessario un numero valido.

    Questo problema in genere non viene trovato con il codice con scaffolding per un modello con una proprietà integer nelle visualizzazioni Creazione e modifica.

    Per risolvere questo problema, modificare l'helper di editor da:

    `@Html.EditorFor(person => person.Age)`

    A:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 non supporta più condizioni di attendibilità parziale. I progetti di collegamento ai file binari di MVC o API Web devono rimuovere il [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) attributo e il [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) attributo. Rimozione di questi attributi comporterà l'eliminazione di errori del compilatore simile al seguente.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Si noti che poiché un effetto collaterale di questo si può usare assembly 4.0 e 5.0 nella stessa applicazione. È necessario aggiornare tutti gli elementi a 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Modello di applicazione a singola pagina con l'autorizzazione di Facebook può causare instabilità in Internet Explorer mentre il sito web è ospitata nell'area intranet

Il modello SPA fornisce l'accesso esterno con Facebook. Quando il progetto creato con il modello è in esecuzione in locale, l'accesso potrebbe essere Internet Explorer in modo anomalo.

Soluzione:

1. Ospitare il sito web nell'area internet. o

2. Testare lo scenario in un browser diverso da Internet Explorer.

### <a name="web-forms-scaffolding"></a>Web Forms Scaffolding

Estensione Web Forms Scaffolding è stato rimosso da VS2013 e sarà disponibile in un futuro aggiornamento a Visual Studio. Tuttavia, è possibile utilizzare comunque all'interno di un progetto di Web Forms scaffolding aggiungendo dipendenze MVC e generare lo scaffolding di MVC. Il progetto conterrà una combinazione di Web Form e MVC.

Per aggiungere MVC al progetto Web Form, aggiungere un nuovo elemento di scaffolding e selezionare **dipendenze MVC 5**. Selezionare minima o completa in base alla necessità di tutti i file di contenuto, ad esempio gli script. Quindi, aggiungere un elemento di scaffolding di MVC, che verrà creato un controller e viste nel progetto.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC e Scaffolding API Web - HTTP 404, errore non trovato

Se si verifica un errore durante l'aggiunta di un elemento di scaffolding per un progetto, è possibile che il progetto verrà lasciato in uno stato incoerente. Alcune delle modifiche apportate da scaffolding verrà eseguito il rollback, ma altre modifiche, ad esempio i pacchetti NuGet installati, non essere il rollback. Se le modifiche alla configurazione di routing viene eseguito il rollback, gli utenti riceveranno un errore HTTP 404 quando passare a scaffolding elementi.

Soluzione alternativa:

- Per correggere l'errore per MVC, aggiungere un nuovo elemento di scaffolding e selezionare dipendenze MVC 5 (minima o completa). Questo processo verrà aggiunto a tutte le modifiche necessarie al progetto.
- Per correggere l'errore per l'API Web:

  1. Aggiungere la classe WebApiConfig al progetto.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configurare webapiconfig. Register nell'applicazione\_metodo Start in Global. asax come indicato di seguito:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
