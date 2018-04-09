---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET e strumenti Web per note sulla versione di Visual Studio 2013 | Documenti Microsoft
author: microsoft
description: Questo documento descrive la versione di ASP.NET e strumenti Web per Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: e9ddd96f186564834ff6bb2c30cf0ed5444cbf1b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET e strumenti Web per note sulla versione di Visual Studio 2013
====================
by [Microsoft](https://github.com/microsoft)

> Questo documento descrive la versione di ASP.NET e strumenti Web per Visual Studio 2013.


## <a name="contents"></a>Sommario

- [Note sull'installazione](#TOC1)
- [Documentazione](#TOC2)
- [Requisiti software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuove funzionalità di ASP.NET e strumenti Web per Visual Studio 2013

- [Uno di ASP.NET](#TOC6)
- [Nuova esperienza di progetto Web](#newproj)
- [Scaffolding di ASP.NET](#scaffold)
- [Browser Link](#browser-link)
- [Miglioramenti di Editor di Visual Studio Web](#web-editor)
- [Supporto di applicazioni Web di servizio App di Azure in Visual Studio](#waws)
- [Miglioramenti di pubblicazione sul Web](#publish)
- [NuGet 2.7](#nuget)
- [Web Form ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [Identità di ASP.NET](#TOC8)
- [Componenti Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [Sospensione di App ASP.NET](#TOC15)
- [Problemi noti e modifiche di rilievo](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Note sull'installazione

ASP.NET e strumenti Web per Visual Studio 2013 sono inclusi nel programma di installazione principale e può essere scaricati [qui](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentazione

Sono disponibili esercitazioni e altre informazioni su ASP.NET e strumenti Web per Visual Studio 2013 il [sito web ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Requisiti software

ASP.NET e strumenti Web richiede Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuove funzionalità di ASP.NET e strumenti Web per Visual Studio 2013

Nelle sezioni seguenti vengono descritte le funzionalità che sono state introdotte nella versione.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Uno di ASP.NET

Con la versione di Visual Studio 2013, abbiamo adottato un passo di unificare l'esperienza di utilizzare le tecnologie ASP.NET, in modo che sia possibile combinare e corrispondono a quelli desiderati. Ad esempio, è possibile avviare un progetto con MVC e aggiungere le pagine Web Form al progetto in un secondo momento o lo scaffolding di API Web in un progetto di Web Form. Uno di ASP.NET è rendendo più semplice per la qualità di sviluppatore per eseguire le operazioni che ti piace in ASP.NET. Indipendentemente da quale tecnologia si sceglie, è possibile disporre fiducia che si compila il framework sottostante attendibile di ASP.NET uno.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nuova esperienza di progetto Web

È stata migliorata l'esperienza di creazione di nuovi progetti web in Visual Studio 2013. Nel **nuovo progetto Web ASP.NET** finestra di dialogo è possibile selezionare il tipo di progetto si desidera Configura qualsiasi combinazione di tecnologie (Web Form, MVC, Web API), configurare le opzioni di autenticazione e aggiungere un progetto di unit test.

![Nuovo progetto ASP.NET](release-notes/_static/image1.png)

La nuova finestra di dialogo consente di modificare le opzioni di autenticazione predefinito per la maggior parte dei modelli. Ad esempio, quando si crea un progetto di Web Form ASP.NET è possibile selezionare una delle opzioni seguenti:

- Nessuna autenticazione
- Account utente (appartenenza ASP.NET o provider social Accedi)
- Account aziendale (Active Directory in un'applicazione internet)
- Autenticazione di Windows (Active Directory in un'applicazione intranet)

![Opzioni di autenticazione](release-notes/_static/image2.png)

Per ulteriori informazioni sul processo di nuovo per la creazione di progetti web, vedere [creazione di progetti Web ASP.NET in Visual Studio 2013](creating-web-projects-in-visual-studio.md). Per ulteriori informazioni sulle nuove opzioni di autenticazione, vedere [ASP.NET Identity](#TOC8) più avanti in questo documento.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>Scaffolding di ASP.NET

Lo Scaffolding di ASP.NET è un framework di generazione di codice per applicazioni Web ASP.NET. Che consente di aggiungere il codice di boilerplate al progetto che interagisce con un modello di dati.

Nelle versioni precedenti di Visual Studio, lo scaffolding è limitato ai progetti ASP.NET MVC. Con Visual Studio 2013, è ora possibile usare lo scaffolding per qualsiasi progetto ASP.NET, inclusi Web Form. Visual Studio 2013 non supporta attualmente la generazione di pagine per un progetto di Web Form, ma è comunque possibile utilizzare lo scaffolding con Web Form aggiungendo le dipendenze MVC al progetto. Verrà aggiunto il supporto per la generazione di pagine Web Form in un aggiornamento futuro.

Quando si usa lo scaffolding, è assicurare che tutti obbligatori le dipendenze siano installate nel progetto. Ad esempio, se si avvia con un progetto di Web Form ASP.NET e quindi Usa lo scaffolding per aggiungere un Controller API Web, i pacchetti NuGet necessari e i riferimenti vengono aggiunti al progetto automaticamente.

Per aggiungere lo scaffolding di MVC a un progetto di Web Form, aggiungere un **nuovo elemento di scaffolding** e selezionare **MVC 5 dipendenze** nella finestra di dialogo. Sono disponibili due opzioni per lo scaffolding MVC; Minimo e completo. Se si seleziona minima, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto. Se si seleziona l'opzione completo, vengono aggiunte le dipendenze minime, nonché i file di contenuto richiesti per un progetto MVC.

Supporto per lo scaffolding controller asincrono utilizza le nuove funzionalità asincrone di Entity Framework 6.

Per ulteriori informazioni ed esercitazioni, vedere [Panoramica lo Scaffolding di ASP.NET](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Collegamento browser-SignalR canale tra il browser e Visual Studio

Il nuovo [collegamento Browser](using-browser-link.md) funzionalità consente di connettersi più browser a Visual Studio e aggiornarli tutti facendo clic su un pulsante nella barra degli strumenti. È possibile connettersi più browser al sito di sviluppo, inclusi gli emulatori di dispositivi mobili e fare clic su Aggiorna per aggiornare tutti i browser tutti nello stesso momento. Collegamento del browser espone inoltre un'API per consentire agli sviluppatori di scrivere le estensioni di collegamento del Browser.

![](release-notes/_static/image3.png)

Consentendo agli sviluppatori di sfruttare l'API di collegamento del Browser, è possibile creare scenari molto avanzati che attraversa i limiti tra Visual Studio e qualsiasi browser che è connesso. Essentials Web consente di sfruttare l'API per creare un'esperienza integrata tra Visual Studio e strumenti di sviluppo del browser, remoti controllo emulatori di dispositivi mobili e altro ancora.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Miglioramenti di Editor di Visual Studio Web

Visual Studio 2013 include un nuovo editor HTML per i file Razor e HTML nelle applicazioni web. Il nuovo editor HTML fornisce un singolo schema unificato basato su HTML5. Include il completamento di parentesi graffe, jQuery UI e AngularJS attributo IntelliSense, l'attributo di raggruppamento di IntelliSense, ID e nome della classe Intellisense e altri miglioramenti, tra cui prestazioni migliori, formattazione e degli smart tag.

Nella schermata seguente viene illustrato come utilizzare l'attributo Bootstrap IntelliSense nell'editor HTML.

![IntelliSense nell'editor HTML](release-notes/_static/image4.png)

Visual Studio 2013 include anche con entrambi CoffeeScript e meno editor predefinito. Editor LESS dotato di tutte le funzionalità interessanti da editor CSS e dispone di Intellisense specifiche per le variabili e "mixins" in tutti i meno documenti il @import catena.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Supporto di applicazioni Web di servizio App di Azure in Visual Studio

In Visual Studio 2013 con Azure SDK per .NET 2.2, è possibile utilizzare **Esplora Server** per interagire direttamente con le applicazioni web remoto. È possibile accedere al proprio account Azure, creare nuove applicazioni web, configurare le app, visualizzare log in tempo reale e altro ancora. Viene rilasciato proveniente dopo SDK 2.2, sarà possibile eseguire in modalità di debug in modalità remota in Azure. La maggior parte delle nuove funzionalità per App Web di servizio App di Azure funziona anche in Visual Studio 2012, quando si installa la versione corrente di Azure SDK per .NET.

Per altre informazioni, vedere le seguenti risorse:

- [Creare un'app web ASP.NET in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Risolvere i problemi relativi a un'app web nel servizio App di Azure con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Miglioramenti di pubblicazione sul Web

Visual Studio 2013 include funzionalità nuove e migliorate pubblicazione sul Web. Ecco alcuni di essi:

- Facilmente [automatizzare la crittografia dei file Web. config](https://go.microsoft.com/fwlink/?LinkId=325529). (Questo collegamento e i due seguenti puntano alla documentazione su MSDN che potrebbero non essere disponibile fino al ritardo nel giorno in 10/17).
- Facilmente [automatizzare l'esecuzione di un'applicazione durante la distribuzione](https://go.microsoft.com/fwlink/?LinkId=325530).
- Distribuzione Web per [utilizzare checksum del file anziché Data ultima modifica](https://go.microsoft.com/fwlink/?LinkId=325531) per determinare quali file devono essere copiati nel server.
- Consente di pubblicare rapidamente singoli file selezionati (inclusi i file Web. config) quando si utilizza il protocollo FTP o metodi di pubblicazione di sistema di file, nonché con distribuzione Web.

Per ulteriori informazioni sulla distribuzione di web ASP.NET, vedere [sito ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

2.7 NuGet include un set completo delle nuove funzionalità descritte in dettaglio [note sulla versione 2.7 di NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Questa versione di NuGet rimuove inoltre la necessità di fornire il consenso esplicito per la funzionalità di ripristino pacchetto NuGet scaricare i pacchetti. Consenso (e la relativa casella di controllo nella finestra di dialogo Preferenze di NuGet) vengono concesse tramite l'installazione di NuGet. Ripristino del pacchetto semplicemente funziona ora per impostazione predefinita.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Web Form ASP.NET

### <a name="one-aspnet"></a>Uno di ASP.NET

I modelli di progetto di Web Form si integrano perfettamente con l'esperienza di ASP.NET uno nuovo. È possibile aggiungere il supporto MVC e API Web al progetto Web Form ed è possibile configurare l'autenticazione utilizzando la creazione guidata progetto ASP.NET uno. Per ulteriori informazioni, vedere [creazione di progetti Web ASP.NET in Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>Identità ASP.NET

I modelli di progetto di Web Form supportano il nuovo framework di ASP.NET Identity. Inoltre, i modelli supportano ora la creazione di un progetto di Web Form intranet. Per ulteriori informazioni, vedere [metodi di autenticazione](creating-web-projects-in-visual-studio.md#auth) in **creazione di progetti Web ASP.NET in Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Utilizzano i modelli Web Form [Bootstrap](http://twitter.github.io/bootstrap/) per fornire un elegante e reattiva aspetto che è possibile personalizzare facilmente. Per ulteriori informazioni, vedere [Bootstrap nei modelli di progetto web di Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Uno di ASP.NET

I modelli di progetto Web MVC integrano con la nuova esperienza di ASP.NET uno. È possibile personalizzare il progetto MVC e configurare l'autenticazione mediante la creazione guidata progetto ASP.NET uno. Un'esercitazione introduttiva su ASP.NET MVC 5 è reperibile in [Introduzione a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Per informazioni sull'aggiornamento di progetti MVC 4 e 5 MVC, vedere [come aggiornare un ASP.NET MVC 4 e il progetto API Web per ASP.NET MVC 5 e l'API Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>Identità ASP.NET

I modelli di progetto MVC sono stati aggiornati per l'utilizzo di ASP.NET Identity per l'autenticazione e la gestione delle identità. Un'esercitazione che presenta l'autenticazione di Facebook e Google e la nuova API di appartenenza è reperibile in [creare un'App di ASP.NET MVC 5 con Facebook, Google OAuth2 e OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) e [crea un'applicazione MVC ASP.NET con autenticazione e Database di SQL Server e distribuire in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

Il modello di progetto MVC è stato aggiornato per utilizzare [Bootstrap](http://getbootstrap.com/) per fornire un elegante e reattiva aspetto che è possibile personalizzare facilmente. Per ulteriori informazioni, vedere [Bootstrap nei modelli di progetto web di Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtri di autenticazione

I filtri di autenticazione sono un nuovo tipo di filtro in ASP.NET MVC che vengono eseguiti prima dei filtri di autorizzazione nella pipeline ASP.NET MVC e consentono di specificare l'autenticazione logica per ogni azione, per ogni controller o a livello globale per tutti i controller. Filtri di autenticazione nella richiesta di credenziali di elaborare e forniscono un'entità corrispondente. Filtri di autenticazione possono anche aggiungere richieste di autenticazione in risposta alle richieste non autorizzate.

### <a name="filter-overrides"></a>Esegue l'override di filtro

È ora possibile sostituire i filtri da applicare a un metodo di azione specificato o un controller, specificando un filtro di sostituzione. I filtri di sostituzione è specificare un set di tipi di filtro che non devono essere eseguiti per un determinato ambito (azione o controller). Ciò consente di configurare i filtri che si applicano globalmente ma quindi escludere alcuni filtri globali dall'applicazione per azioni specifiche o controller.

### <a name="attribute-routing"></a>Routing con attributi

ASP.NET MVC supporta ora routing degli attributi, grazie a Tim McCall, l'autore di un contributo [ http://attributerouting.net ](http://attributerouting.net). Con il routing di attributo è possibile specificare i percorsi di annotando azioni e controller.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Routing con attributi

ASP.NET Web API supporta ora routing degli attributi, grazie a Tim McCall, l'autore di un contributo [ http://attributerouting.net ](http://attributerouting.net). Con il routing di attributo è possibile specificare i percorsi di API Web annotando azioni e controller simile al seguente:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Attributo routing offre maggiore controllo sull'URI dell'API web. Ad esempio, è possibile definire facilmente una gerarchia di risorse utilizzando un singolo controller API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Attributo routing inoltre fornisce una sintassi utile per specificare i parametri facoltativi, i valori predefiniti e vincoli della route:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Per ulteriori informazioni sul routing di attributo, vedere [Routing degli attributi in Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

I modelli di progetto API Web e applicazione a pagina singola supportano ora l'autorizzazione mediante OAuth 2.0. OAuth 2.0 è un framework per autorizzare l'accesso client alle risorse protette. Funziona per un'ampia gamma di client, inclusi browser e dispositivi mobili.

Supporto per OAuth 2.0 è basato sul nuovo middleware di sicurezza fornito dai componenti Microsoft OWIN per l'autenticazione della connessione e l'implementazione del server di autorizzazione. In alternativa, possono essere autorizzati client utilizza un server di autorizzazione dell'organizzazione, ad esempio Azure Active Directory o ADFS in Windows Server 2012 R2.

### <a name="odata-improvements"></a>Miglioramenti di OData

**Supporto per $select, $expand, $batch e $value**

ASP.NET Web API OData include ora il supporto completo per $select, $expand e $value. È inoltre possibile utilizzare $batch per la richiesta batch e l'elaborazione di insiemi di modifiche.

Opzioni di $select e $expand consentono la modifica della forma dei dati restituiti da un endpoint OData. Per ulteriori informazioni, vedere [Introducing $select e $expand supporto in Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Estendibilità migliorate**

I formattatori OData ora sono estendibili. È possibile aggiungere dei metadati della voce Atom, supporta voci di collegamento multimediale e di flusso denominate, aggiungere annotazioni dell'istanza e personalizzare la modalità di generazione di collegamenti.

**Supporto senza tipi**

È ora possibile compilare servizi OData senza la necessità di definire i tipi CLR per i tipi di entità. Al contrario, i controller OData possono richiedere o restituire istanze di **IEdmObject**, quali sono i formattatori OData di serializzazione/deserializzazione.

**Riutilizzare un modello esistente**

Se si dispone già di un entity data model (EDM) esistente, è possibile ora riutilizzarlo direttamente, invece di generarne uno nuovo. Ad esempio, se si utilizza Entity Framework, è possibile utilizzare il modello EDM EF compila automaticamente.

### <a name="request-batching"></a>Richiedere l'invio in batch

Richiesta batch consente di combinare più operazioni in una singola richiesta HTTP POST, per ridurre il traffico di rete e fornire un Morbidezza, meno l'interfaccia utente "frammentate". API Web ASP.NET supporta diverse strategie per la richiesta di batch:

- Utilizzare l'endpoint $batch di un servizio OData.
- Pacchetto di più richieste in una singola richiesta multipart MIME.
- Utilizzare un formato di batch personalizzato.

Per abilitare la richiesta di invio in batch, è sufficiente aggiungere una route con un gestore di invio in batch per la configurazione di Web API:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

È inoltre possibile controllare se le richieste o eseguiti in sequenza o in qualsiasi ordine.

### <a name="portable-aspnet-web-api-client"></a>Portatile ASP.NET Web API Client

È ora possibile utilizzare il Client di ASP.NET Web API per creare librerie di classi portabile che funzionano tra le applicazioni Windows Store e Windows Phone 8. È inoltre possibile creare i formattatori portatili che possono essere condiviso tra client e server.

### <a name="improved-testability"></a>Testabilità migliorata

Web API 2 rende molto più semplice per unit test controller API. Solo creare un'istanza del controller API con la configurazione e il messaggio di richiesta e quindi chiamare il metodo di azione che si desidera verificare. È anche facile simulare il **UrlHelper** (classe), per i metodi di azione che eseguono la generazione di collegamento.

### <a name="ihttpactionresult"></a>IHttpActionResult

È ora possibile implementare IHttpActionResult per incapsulare il risultato dei metodi di azione di API Web. Un IHttpActionResult restituito da un metodo di azione di API Web viene eseguito dal runtime di ASP.NET Web API per produrre il messaggio di risposta risultante. Un IHttpActionResult possono essere restituiti da qualsiasi azione di API Web per semplificare unit test dell'implementazione di API Web. Per praticità che molte implementazioni IHttpActionResult disponibili immediatamente, compresi i risultati per la restituzione di codici di stato specifici, formattato contenute o negoziato contenuto risposte.

### <a name="httprequestcontext"></a>HttpRequestContext

Il nuovo **HttpRequestContext** tiene traccia di qualsiasi stato che è associato alla richiesta ma non è immediatamente disponibile dalla richiesta. Ad esempio, è possibile utilizzare il **HttpRequestContext** per ottenere i dati della route, l'entità associata alla richiesta, il certificato client, il **UrlHelper** e la radice del percorso virtuale. È possibile creare facilmente un **HttpRequestContext** per unità a scopo di test.

Poiché l'entità per la richiesta viene passata con la richiesta invece di basarsi sui **CurrentPrincipal**, l'entità è ora disponibile per tutta la durata della richiesta in pipeline di Web API.

### <a name="cors"></a>CORS

Grazie a un altro ottimo contributo Brock Allen, ASP.NET ora supporta completamente richiesta condivisione Multiorigine (CORS).

Protezione del browser impedisce che una pagina web effettua le richieste AJAX a un altro dominio. [CORS](http://www.w3.org/TR/cors/) è uno standard W3C che consente a un server per ridurre i criteri di origine stesso. Tramite condivisione CORS, un server può consentire in modo esplicito alcune richieste cross-origin durante il rifiuto di altri utenti.

Web API 2 supporta ora CORS, tra cui la gestione automatica delle richieste di verifica preliminare. Per ulteriori informazioni, vedere [abilitazione richieste Cross-Origin in ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtri di autenticazione

I filtri di autenticazione sono un nuovo tipo di filtro nell'API Web ASP.NET che vengono eseguiti prima dei filtri di autorizzazione nella pipeline ASP.NET Web API e consentono di specificare l'autenticazione logica per ogni azione, per ogni controller o a livello globale per tutti i controller. Filtri di autenticazione nella richiesta di credenziali di elaborare e forniscono un'entità corrispondente. Filtri di autenticazione possono anche aggiungere richieste di autenticazione in risposta alle richieste non autorizzate.

### <a name="filter-overrides"></a>Esegue l'override di filtro

È ora possibile sostituire i filtri da applicare a un metodo di azione specificato o un controller, specificando un filtro di sostituzione. I filtri di sostituzione è specificare un set di tipi di filtro che non deve essere eseguita per un determinato ambito (azione o controller). Ciò consente di aggiungere filtri globali, ma quindi escludere alcuni dalle azioni specifiche o controller.

### <a name="owin-integration"></a>Integrazione di OWIN

ASP.NET Web API ora completamente supporta OWIN e può essere eseguito in qualsiasi host che supporta OWIN. È inoltre incluso un **HostAuthenticationFilter** che fornisce l'integrazione con il sistema di autenticazione OWIN.

Con l'integrazione di OWIN, è possibile self-ospitare API Web nel proprio processo insieme ad altro middleware OWIN, ad esempio SignalR. Per ulteriori informazioni, vedere [OWIN utilizzare Self-Host ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

Le sezioni seguenti descrivono le funzionalità di SignalR 2.0.

- [Basato su OWIN](#builtonowin)
- [MapHubs e MapConnection sono ora MapSignalR](#MapSignalR)
- [Supporto di domini](#crossdomain)
- [iOS e Android supportano tramite MonoTouch e MonoDroid](#mobile)
- [Client .NET portabili](#portable)
- [Nuovo pacchetto di Host indipendente](#selfhost)
- [Supporto server compatibili con le versioni precedenti](#backwardcompat)
- [Rimosso il server di supporto per .NET 4.0](#remove40)
- [Invia un messaggio a un elenco di client e gruppi](#messagelist)
- [Invia un messaggio a un utente specifico](#sendtouser)
- [Supporto migliore per la gestione degli errori](#errorhandling)
- [Più facile unit test di hub](#unittesting)
- [Gestione degli errori JavaScript](#javascripterror)

Per un esempio di come aggiornare un progetto esistente di 1. x a SignalR 2.0, vedere [l'aggiornamento di un SignalR 1. x progetto](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Basato su OWIN

SignalR 2.0 si basa completamente in [OWIN (l'interfaccia Web aperta per .NET)](http://owin.org/). Questa modifica rende il processo di installazione per SignalR molto più coerente tra applicazioni SignalR ospitato sul web e indipendente, ma è necessaria anche una serie di modifiche API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs e MapConnection sono ora MapSignalR

Per compatibilità con gli standard OWIN, questi metodi sono stati rinominati in `MapSignalR`. `MapSignalR` chiamata senza parametri verranno eseguito il mapping di tutti gli hub (come `MapHubs` versione 1.x); per eseguire il mapping singoli **PersistentConnection** oggetti, specificare il tipo di connessione come parametro di tipo e l'estensione dell'URL per la connessione come il primo argomento.

Il `MapSignalR` metodo viene chiamato in una classe di avvio di Owin. Visual Studio 2013 contiene un nuovo modello per una classe di avvio di Owin. Per utilizzare questo modello, effettuare le operazioni seguenti:

1. Pulsante destro del mouse sul progetto
2. Selezionare **aggiungere**, **nuovo elemento...**
3. Selezionare **classe di avvio di Owin**. Denominare la nuova classe **Startup.cs**.

In un **applicazione Web,** Owin avvio classe contenente il `MapSignalR` metodo viene quindi aggiunto al processo di avvio di Owin usando una voce nel nodo delle impostazioni dell'applicazione del file Web. config, come illustrato di seguito.

In un **self-hosted applicazione**, la classe di avvio viene passata come parametro di tipo di `WebApp.Start` (metodo).

**Mapping degli hub e le connessioni in SignalR 1.x (dal file dell'applicazione globale in un'applicazione web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Hub di mapping e le connessioni SignalR 2.0 (da un file di classe di avvio di Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

In un **self-hosted applicazione**, la classe di avvio viene passata come parametro di tipo per il `WebApp.Start` (metodo), come illustrato di seguito.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Supporto di domini

In SignalR 1. x, richieste tra domini sono stati controllati da un flag EnableCrossDomain singolo. Questo flag è controllato da JSONP sia CORS richieste. Per una maggiore flessibilità, il supporto CORS tutti è stato rimosso dal componente server di SignalR (lients JavaScript comunque usare CORS in genere se è stato rilevato che il browser supporti), e nuovi middleware OWIN è stato reso disponibile per supportare questi scenari.

In SignalR 2.0 se JSONP è obbligatorio sul client (per supportare le richieste tra domini nei browser meno recenti), dovranno essere abilitata in modo esplicito impostando `EnableJSONP` sul `HubConfiguration` oggetto `true`, come illustrato di seguito. JSONP è disabilitata per impostazione predefinita, perché è meno sicura rispetto a CORS.

Per aggiungere il nuovo middleware CORS in SignalR 2.0, aggiungere il `Microsoft.Owin.Cors` libreria del progetto e chiamare `UseCors` prima del middleware di SignalR, come illustrato nella sezione seguente.

**Aggiunta al progetto owin**: per installare questa libreria, eseguire il comando seguente nella Console di gestione pacchetti:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Questo comando consente di aggiungere il 2.0.0 versione del pacchetto al progetto.

**La chiamata UseCors**

I frammenti di codice seguente viene illustrato come implementare le connessioni tra domini in SignalR 1. x e 2.0.

**Implementazione le richieste tra domini in SignalR 1.x (dal file dell'applicazione globale)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementazione le richieste tra domini in SignalR 2.0 (da un file di codice c#)**

Il codice seguente viene illustrato come abilitare CORS o JSONP in un progetto SignalR 2.0. In questo esempio di codice Usa `Map` e `RunSignalR` anziché `MapSignalR`, in modo che il middleware CORS viene eseguita solo per le richieste di SignalR che richiedono il supporto CORS (anziché per tutto il traffico nel percorso specificato `MapSignalR`.) `Map` utilizzabili per qualsiasi altro middleware che deve essere eseguito per un prefisso di URL specifico, anziché per l'intera applicazione.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS e Android supportano tramite MonoTouch e MonoDroid

È stato aggiunto il supporto per iOS e Android client che utilizzano componenti MonoTouch e MonoDroid dal [Xamarin libreria](https://xamarin.com/). Per ulteriori informazioni su come utilizzarle, vedere [utilizzando i componenti di Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Questi componenti è disponibili nel [Store Xamarin](https://store.xamarin.com/) quando non è disponibile la versione RTW SignalR.

<a id="portable"></a> # # # Portabile client .NET

Per una migliore facilitare lo sviluppo multipiattaforma, di Silverlight, WinRT e i client di Windows Phone sono stati sostituiti con un singolo client .NET portatile che supporta le piattaforme seguenti:

- NET 4.5
- Silverlight 5
- WinRT (.NET per applicazioni Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nuovo pacchetto di Host indipendente

È ora disponibile un pacchetto NuGet per renderne più semplice iniziare a SignalR indipendente (SignalR applicazioni ospitate in un processo o un'altra applicazione, anziché ospitato in un server web). Per aggiornare un progetto di host indipendente compilato con SignalR 1. x, rimuovere il pacchetto Microsoft.AspNet.SignalR.Owin e aggiungere il pacchetto Microsoft.AspNet.SignalR.SelfHost. Per ulteriori informazioni sulle attività iniziali con il pacchetto di servizio indipendente, vedere [esercitazione: SignalR indipendente](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Supporto server compatibili con le versioni precedenti

Nelle versioni precedenti di SignalR, le versioni del pacchetto SignalR utilizzati nel client e server devono essere identici. Per supportare applicazioni thick client che sarebbero difficile da aggiornare, SignalR 2.0 supporta ora l'utilizzo di una versione più recente di server con un client precedente. **Nota: SignalR 2.0 non supporta i server compilati con le versioni precedenti con i client più recenti.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Rimosso il server di supporto per .NET 4.0

SignalR 2.0 ha eliminato il supporto per l'interoperabilità di server con .NET 4.0. .NET 4.5 deve essere utilizzata con server 2.0 SignalR. È ancora presente un client .NET 4.0 per SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Invia un messaggio a un elenco di client e gruppi

In SignalR 2.0, è possibile inviare un messaggio utilizzando un elenco di client e l'ID del gruppo. I frammenti di codice seguente viene illustrato come eseguire questa operazione.

**Invia un messaggio a un elenco di client e gruppi usando PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Invia un messaggio a un elenco di client e gruppi tramite hub**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Invia un messaggio a un utente specifico

Questa funzionalità consente agli utenti di specificare l'ID utente in base a un IRequest tramite una nuova interfaccia IUserIdProvider:

**L'interfaccia IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Per impostazione predefinita verrà un'implementazione che utilizza IPrincipal.Identity.Name dell'utente come nome utente.

Nell'hub, sarà in grado di inviare messaggi agli utenti tramite una nuova API:

**Tramite l'API Clients.User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Supporto migliore per la gestione degli errori

Gli utenti possono ora generare **HubException** da qualsiasi chiamata dell'hub. Il costruttore del **HubException** può accettare un messaggio stringa e un oggetto dati aggiuntivi errore. SignalR verrà automaticamente a serializzare l'eccezione e inviarlo al client e in cui verrà utilizzato per rifiutare la chiamata del metodo hub/test non superato.

Il **Mostra eccezioni hub dettagliati** impostazione non ha alcun effetto sui **HubException** inviati al client o non; sempre inviata.

**Codice sul lato server che illustrano l'invio al client un HubException**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Codice client JavaScript che illustra risponde a un HubException inviati dal server**

[!code-html[Main](release-notes/samples/sample16.html)]

**Codice client .NET che illustra risponde a un HubException inviati dal server**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Più facile unit test di hub

SignalR 2.0 include un'interfaccia denominata `IHubCallerConnectionContext` su hub che rende più semplice creare di chiamate sul lato client fittizio. I frammenti di codice seguente viene illustrato l'utilizzo di questa interfaccia con più diffusi test harness [xUnit.net](https://github.com/xunit/xunit) e [moq](https://code.google.com/p/moq/).

**Testing unità SignalR con xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Testing unità SignalR con moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Gestione degli errori JavaScript

In SignalR 2.0, tutti i callback di gestione degli errori JavaScript restituiscono oggetti errore JavaScript anziché stringhe non elaborate. In questo modo SignalR propagare le informazioni più dettagliate per i gestori di errori. È possibile ottenere l'eccezione interna dal `source` proprietà dell'errore.

**Codice client JavaScript che gestisce l'eccezione Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>Identità ASP.NET

### <a name="new-aspnet-membership-system"></a>Nuovo sistema di appartenenze ASP.NET

Identità di ASP.NET è il nuovo sistema di appartenenza per le applicazioni ASP.NET. ASP.NET Identity rende più facile da integrare dati di profilo per ogni utente con dati dell'applicazione. Identità di ASP.NET consente inoltre di scegliere il modello di persistenza per i profili utente nell'applicazione. È possibile archiviare i dati in un database di SQL Server o un altro archivio dati, inclusi gli archivi dati NoSQL, ad esempio tabelle di archiviazione di Azure. Per ulteriori informazioni, vedere [singoli account utente di](creating-web-projects-in-visual-studio.md#indauth) in **creazione di progetti Web ASP.NET in Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Autenticazione basata sulle attestazioni

ASP.NET ora supporta l'autenticazione basata sulle attestazioni, in cui l'identità dell'utente è rappresentato come un set di attestazioni da un'autorità emittente attendibile. Gli utenti possono essere autenticati utilizzando un nome utente e password gestiti in un database dell'applicazione o tramite i provider di identità di social networking (ad esempio: Accounts Microsoft, Facebook, Google, Twitter), o usando gli account aziendali tramite Azure Active Directory o Active Directory Federation Services (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integrazione con Azure Active Directory e Windows Server Active Directory

È ora possibile creare progetti ASP.NET che usano Azure Active Directory o Windows Server Active Directory (AD) per l'autenticazione. Per ulteriori informazioni, vedere [account aziendali](creating-web-projects-in-visual-studio.md#orgauth) in **creazione di progetti Web ASP.NET in Visual Studio 2013**.

### <a name="owin-integration"></a>Integrazione di OWIN

Autenticazione ASP.NET è basata su middleware OWIN che può essere usato in qualsiasi host basato su OWIN. Per ulteriori informazioni su OWIN, vedere il seguente [componenti di Microsoft OWIN](#TOC7) sezione.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Componenti Microsoft OWIN

[Aprire l'interfaccia Web per .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server web .NET e le applicazioni web. OWIN disaccoppia l'applicazione web dal server, rendendo web applicazioni indipendente dall'host. Ad esempio, è possibile ospitare un'applicazione web basata su OWIN in IIS o indipendente in un processo personalizzato.

Le modifiche introdotte nei componenti Microsoft OWIN (noto anche come progetto Katana) includono nuovi componenti server e l'host, le nuove librerie di supporto e middleware e nuovo middleware di autenticazione.

Per ulteriori informazioni su OWIN e Katana, vedere [novità di OWIN e Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Nota: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applicazioni non è possibile eseguire in modalità classica IIS; deve essere eseguiti in modalità integrata.**

**Nota: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) le applicazioni devono essere eseguite con attendibilità totale.**

### <a name="new-servers-and-hosts"></a>Gli host e i nuovi server

Con questa versione, i nuovi componenti sono stati aggiunti per abilitare scenari di host indipendente. Questi componenti includono i pacchetti NuGet seguenti:

- **Microsoft.Owin.Host.HttpListener**. Fornisce un server OWIN che utilizza **HttpListener** in ascolto per le richieste HTTP e l'utente nella pipeline OWIN.
- **Owin** fornisce una libreria per gli sviluppatori che desiderano una pipeline OWIN in un processo personalizzato, ad esempio un'applicazione console o il servizio Windows di host indipendente.
- **OwinHost**. Fornisce un file eseguibile autonomo che esegue il wrapping `Microsoft.Owin.Hosting` e consente di ospitare una pipeline OWIN in modo automatico senza dover scrivere un'applicazione host personalizzata.

Inoltre, il `Microsoft.Owin.Host.SystemWeb` pacchetto consente ora di middleware di fornire suggerimenti per la **SystemWeb** server, che indica che il middleware deve essere chiamato durante una determinata fase della pipeline ASP.NET. Questa funzionalità è particolarmente utile per il middleware di autenticazione, che deve essere eseguito nelle prime fasi della pipeline ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Middleware e librerie di supporto

Sebbene sia possibile scrivere componenti OWIN utilizzando solo le definizioni di funzione e il tipo dalla specifica di OWIN, il nuovo `Microsoft.Owin` pacchetto fornisce un insieme di astrazioni più semplice. Questo pacchetto consente di combinare diversi pacchetti precedenti (ad esempio, `Owin.Extensions`, `Owin.Types`) in un modello di oggetto ben strutturati che può quindi essere facilmente utilizzato da altri componenti OWIN. In effetti, la maggior parte dei componenti Microsoft OWIN ora utilizzare il pacchetto.

> [!NOTE]
> [OWIN](http://www.owin.org) applicazioni non è possibile eseguire in modalità classica IIS; deve essere eseguiti in modalità integrata.

> [!NOTE]
> [OWIN](http://www.owin.org) le applicazioni devono essere eseguite con attendibilità totale.

Questa versione include anche il pacchetto italiano, che include middleware per la convalida di un'applicazione OWIN in esecuzione più middleware della pagina di errore per consentire di analizzare gli errori.

### <a name="authentication-components"></a>Componenti di autenticazione

I componenti di autenticazione seguenti sono disponibili.

- **Microsoft.Owin.Security.ActiveDirectory**. Abilita l'autenticazione utilizzando i servizi directory in locale o basato su cloud.
- **Cookies** Abilita l'autenticazione utilizzando i cookie. Questo pacchetto è stato in precedenza denominato `Microsoft.Owin.Security.Forms`.
- **Owin** Abilita l'autenticazione utilizzando servizio basato su OAuth di Facebook.
- **Owin** Abilita l'autenticazione utilizzando servizio basati su OpenID di Google.
- **Owin** Abilita l'autenticazione con token JWT.
- **MicrosoftAccount** Abilita l'autenticazione tramite account Microsoft.
- **Microsoft.Owin.Security.OAuth**. Fornisce un server di autorizzazione OAuth nonché middleware per autenticare i token di connessione.
- **Twitter** Abilita l'autenticazione utilizzando servizio basato su OAuth di Twitter.

Questa versione include anche il `Microsoft.Owin.Cors` pacchetto che contiene il middleware per l'elaborazione delle richieste HTTP cross-origin.

> [!NOTE]
> Supporto per la firma di JWT è stato rimosso nella versione finale di Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Per un elenco delle nuove funzionalità e altre modifiche in Entity Framework 6, vedere [cronologia delle versioni di Entity Framework](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 include le nuove funzionalità seguenti:

- Supporto per la modifica di scheda. Preivously, il **Formatta documento** comando rientro automatico e in Visual Studio di formattazione automatica non ha funzionato correttamente quando si utilizza il **Mantieni tabulazioni** opzione. Questa modifica consente di correggere la formattazione di codice Razor per scheda di formattazione di Visual Studio.
- Supporto per le regole di riscrittura dell'URL per la generazione di collegamenti.
- Rimozione dell'attributo di trasparenza di sicurezza.
  > [!NOTE]
  > Questa è una modifica di rilievo e rende Razor 3 incompatibile con MVC4 e versioni precedenti, mentre non è compatibile con MVC5 o gli assembly compilati con MVC5 2 Razor.

Problemi di Razor 3 risolti in Visual Studio 2013 da versioni non definitive sono reperibili [qui](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Sospensione di App ASP.NET

App Suspend ASP.NET è una funzionalità di modifica di gioco in .NET Framework 4.5.1 che cambia radicalmente l'esperienza utente e il modello economico per l'hosting di un numero elevato di siti ASP.NET in un singolo computer. Per ulteriori informazioni, vedere [ASP.NET App Suspend-reattiva condiviso di hosting web .NET](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti problemi noti e modifiche di rilievo in ASP.NET e strumenti Web per Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Ripristino del nuovo pacchetto non viene eseguita sui Mono quando si usa file SLN](https://nuget.codeplex.com/workitem/3596) : verrà risolto in un download nuget.exe future e [NuGet.CommandLine pacchetto](http://www.nuget.org/packages/NuGet.CommandLine/) aggiornare.
- [Ripristino del pacchetto nuovo non funziona con i progetti di Wix](https://nuget.codeplex.com/workitem/3598) : verrà risolto in un download nuget.exe future e [NuGet.CommandLine pacchetto](http://www.nuget.org/packages/NuGet.CommandLine/) aggiornare.
- [Ripristino automatico del pacchetto non funziona per i progetti in una cartella della soluzione](https://nuget.codeplex.com/workitem/3625) – verrà risolto in NuGet 2.8.

### <a name="aspnet-web-api"></a>API Web ASP.NET

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` non restituisce `IQueryable<T>` sempre, come è stato aggiunto supporto per `$select` e `$expand`.

    Gli esempi precedenti per `ODataQueryOptions<T>` sempre eseguito il cast il valore restituito da `ApplyTo` a `IQueryable<T>`. Operazione manualmente in precedenza, poiché la query opzioni che è supportato in precedenza (`$filter`, `$orderby`, `$skip`, `$top`) non modificare la forma della query. Ora che è supportato `$select` e `$expand` il valore restituito da `ApplyTo` non saranno `IQueryable<T>` sempre.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Se si utilizza il codice di esempio precedenti, esso continuerà lavoro se il client non invia `$select` e `$expand`. Tuttavia, se si desidera supportare `$select` e `$expand` è necessario modificare il codice a questa.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url o RequestContext.Url è null durante una richiesta batch**

    In uno scenario di batch **UrlHelper** è null quando si accede da **Request.Url** o **RequestContext.Url**.

    Questo problema non è stato rilevato qui: [BatchRequestContext.Url è null per la richiesta di batch](http://aspnetwebstack.codeplex.com/workitem/1301).

    La soluzione alternativa per questo problema consiste nel creare una nuova istanza della **UrlHelper**, come nell'esempio seguente:

    **Creazione di una nuova istanza della UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Quando si utilizza MVC5 e OrgAuth, se le viste di cui eseguire la convalida AntiForgerToken, potrebbero venire attraverso il seguente errore durante la registrazione di dati alla visualizzazione:

    **Errore**:

    *Errore del server nell'applicazione '/'.*

    <em>Un'attestazione di tipo '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'o'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' non era presente sul ClaimsIdentity specificato. Per abilitare il supporto del token antifalsificazione con autenticazione basata sulle attestazioni, verificare che il provider di attestazioni configurato fornisce entrambe queste attestazioni nelle istanze di oggetto ClaimsIdentity che genera l'errore. Se il provider di attestazioni configurato Usa invece un tipo di attestazione diverso come identificatore univoco, può essere configurato impostando la proprietà statica AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Soluzione alternativa**:

    In Global. asax per risolvere questo problema, aggiungere la riga seguente:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Questo verrà risolto per la versione successiva.
2. Dopo l'aggiornamento di un'applicazione MVC4 a MVC5, compilare la soluzione e avviarlo. Verrà visualizzato l'errore seguente:

    [A] Non è possibile eseguire il cast di System.Web.WebPages.Razor.Configuration.HostSection [B]System.Web.WebPages.Razor.Configuration.HostSection. Il tipo deriva da ' webpages, Version = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' nel contesto 'Default' nella posizione ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ v 4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'. Tipo B deriva da ' webpages, Version = 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' nel contesto 'Default' nella posizione ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.

    Per correggere l'errore riportato sopra, aprire *tutti* i file Web. config (inclusi quelli nella cartella Views) del progetto e di eseguire le operazioni seguenti:

   1. Aggiornare tutte le occorrenze della versione "4.0.0.0" di "System" a "5.0.0.0".
   2. Aggiorna tutte le occorrenze della versione "2.0.0.0" di "System.Web.Helpers", &quot;System.Web.WebPages&quot; e &quot;webpages&quot; per "3.0.0.0"

      Ad esempio, dopo aver apportato le suddette modifiche, le associazioni di assembly dovrebbero essere simile al seguente:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Per informazioni sull'aggiornamento di progetti MVC 4 e 5 MVC, vedere [come aggiornare un ASP.NET MVC 4 e il progetto API Web per ASP.NET MVC 5 e l'API Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Quando si utilizza la convalida lato client con jQuery non intrusiva convalida, il messaggio di convalida a volte non è corretto per un elemento input HTML con tipo = 'number'. L'errore di convalida per un valore obbligatorio ("Age il campo è obbligatorio") viene visualizzato quando un numero non valido viene immesso anziché il messaggio corretto che è necessario un numero valido.

    Questo problema è stato trovato in genere con codice di scaffolding per un modello con una proprietà integer nelle visualizzazioni Creazione e modifica.

    Per risolvere questo problema, modificare l'helper di editor da:

    `@Html.EditorFor(person => person.Age)`

    A:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 non supporta più di attendibilità parziale. Progetti di collegare i file binari MVC o WebAPI devono rimuovere il [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) attributo e [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) attributo. Rimozione di questi attributi comporterà l'eliminazione di errori del compilatore, ad esempio le operazioni seguenti.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Nota, come effetto collaterale di questo è possibile utilizzare assembly 4.0 e 5.0 nella stessa applicazione. È necessario aggiornarli tutti alla versione 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Modello SPA con Facebook autorizzazione potrebbe causare instabilità in Internet Explorer mentre il sito web è ospitato nell'area intranet

Il modello SPA fornisce registro esterno con Facebook. Quando il progetto creato con il modello è in esecuzione in locale, eseguire l'accesso potrebbe essere Internet Explorer in modo anomalo.

Soluzione:

1. Ospitare il sito web nell'area internet. o

2. Testare lo scenario in un browser diverso da Internet Explorer.

### <a name="web-forms-scaffolding"></a>Web Form lo Scaffolding

Lo Scaffolding di Web Form è stato rimosso da VS2013 e sarà disponibile in un futuro aggiornamento a Visual Studio. Tuttavia, è possibile usare ancora scaffolding all'interno di un progetto di Web Form aggiungendo le dipendenze MVC e generando lo scaffolding per MVC. Il progetto conterrà una combinazione di Web Form e MVC.

Per aggiungere MVC al progetto Web Form, aggiungere un nuovo elemento di scaffolding e selezionare **MVC 5 dipendenze**. Selezionare minima o completa, a seconda della necessità di tutti i file di contenuto, ad esempio gli script. Quindi, aggiungere un elemento di scaffolding per MVC, che verranno create viste e un controller nel progetto.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC e Web API Scaffolding - HTTP 404, errore non trovato

Se si verifica un errore durante l'aggiunta di un elemento di scaffolding per un progetto, è possibile che il progetto verrà lasciato in uno stato incoerente. Alcune delle modifiche apportate lo scaffolding di essere eseguito il rollback, ma altre modifiche, ad esempio i pacchetti NuGet installati, non sarà possibile eseguire il rollback. Se le modifiche alla configurazione di routing rollback, gli utenti riceveranno un errore HTTP 404 quando passando a elementi di scaffolding.

Soluzione alternativa:

- Per correggere l'errore per MVC, aggiungere un nuovo elemento di scaffolding e selezionare le dipendenze di MVC 5 (minima o completa). Questo processo verrà aggiungere tutte le modifiche necessarie al progetto.
- Per correggere l'errore per l'API Web:

  1. Aggiungere alla classe WebApiConfig al progetto.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configurare WebApiConfig.Register nell'applicazione\_Start (metodo) in Global. asax, come indicato di seguito:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
