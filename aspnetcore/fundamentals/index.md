---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Individuare i concetti fondamentali per la compilazione di applicazioni ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: 97c0b289b259332d57f8175e05020fe03d505723
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233310"
---
# <a name="aspnet-core-fundamentals"></a>Nozioni fondamentali su ASP.NET Core

Un'applicazione ASP.NET Core è un'applicazione console che crea un server Web nel suo metodo `Main`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

Il metodo `Main` richiama `WebHost.CreateDefaultBuilder`, che segue il modello di generatore per creare un host applicazioni Web. Il generatore dispone di metodi che definiscono il server Web (ad esempio, `UseKestrel`) e la classe di avvio (`UseStartup`). Nell'esempio precedente il server Web [Kestrel](xref:fundamentals/servers/kestrel) viene allocato automaticamente. L'host Web di ASP.NET Core tenta l'esecuzione in IIS, se disponibile. Altri server Web, come [HTTP.sys](xref:fundamentals/servers/httpsys), possono essere usati richiamando il metodo di estensione appropriato. `UseStartup` viene spiegato dettagliatamente nella sezione successiva.

`IWebHostBuilder`, il tipo restituito della chiamata `WebHost.CreateDefaultBuilder`, offre molti metodi facoltativi. Alcuni di questi metodi includono `UseHttpSys` per ospitare l'app in HTTP.sys e `UseContentRoot` per specificare la directory del contenuto radice. I metodi `Build` e `Run` compilano l'oggetto `IWebHost` che ospita l'app e inizia l'ascolto delle richieste HTTP.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

Il metodo `Main` usa `WebHostBuilder`, che segue il modello di generatore per creare un host applicazioni Web. Il generatore dispone di metodi che definiscono il server Web (ad esempio, `UseKestrel`) e la classe di avvio (`UseStartup`). Nell'esempio precedente viene usato il server Web [Kestrel](xref:fundamentals/servers/kestrel). Altri server Web, come [WebListener](xref:fundamentals/servers/weblistener), possono essere usati richiamando il metodo di estensione appropriato. `UseStartup` viene spiegato dettagliatamente nella sezione successiva.

`WebHostBuilder` offre molti metodi facoltativi, incluso `UseIISIntegration` per l'hosting in IIS e IIS Express e `UseContentRoot` per specificare la directory del contenuto radice. I metodi `Build` e `Run` compilano l'oggetto `IWebHost` che ospita l'app e inizia l'ascolto delle richieste HTTP.

---

## <a name="startup"></a>Avvio

Il metodo `UseStartup` su `WebHostBuilder` specifica la classe `Startup` per l'app:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

Nella classe `Startup` viene definita la pipeline di gestione delle richieste e vengono configurati tutti i servizi necessari per l'app. La classe `Startup` deve essere pubblica e deve contenere i metodi seguenti:

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

`ConfigureServices` definisce i [servizi](#dependency-injection-services) usati dall'app, come ad esempio ASP.NET Core MVC, Entity Framework Core, Identity. `Configure` definisce il [middleware](xref:fundamentals/middleware/index) nella pipeline delle richieste.

Per altre informazioni, vedere [Avvio dell'applicazione](xref:fundamentals/startup).

## <a name="content-root"></a>Radice del contenuto

La radice del contenuto è il percorso di base per i contenuti usati dall'applicazione, ad esempio viste, [Razor Pages](xref:mvc/razor-pages/index)e asset statici. Per impostazione predefinita, la radice del contenuto è uguale al percorso di base dell'applicazione per il file eseguibile che ospita l'app.

## <a name="web-root"></a>Radice Web

La radice Web di un'app è la directory del progetto contenente risorse statiche pubbliche, come ad esempio CSS, JavaScript e i file di immagine.

## <a name="dependency-injection-services"></a>Inserimento di dipendenze (servizi)

Un servizio è un componente destinato a un uso comune in un'app. I servizi sono resi disponibili tramite l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection). ASP.NET Core include nativo **si**/nPer **o**f **C**contenitore ontrollo (IoC) che supporta [inserimento costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per impostazione predefinita. Se si vuole, è possibile sostituire il contenitore nativo predefinito. Oltre al suo ampio beneficio di accoppiamento, l'inserimento delle dipendenze rende disponibili i servizi in tutta l'app. Un esempio è dato dalla [registrazione](xref:fundamentals/logging/index).

Per altre informazioni, vedere [Dependency injection](xref:fundamentals/dependency-injection) (Inserimento delle dipendenze).

## <a name="middleware"></a>Middleware

In ASP.NET Core la pipeline della richiesta viene composta usando un [middleware](xref:fundamentals/middleware/index). ASP.NET Core middleware esegue la logica asincrona su un `HttpContext`, quindi richiama il middleware successivo nella sequenza oppure termina la richiesta direttamente. Viene aggiunto un componente del middleware denominato "XYZ" richiamando un metodo di estensione `UseXYZ` nel metodo `Configure`.

ASP.NET Core include un'ampia gamma di middleware incorporato:

* [File statici](xref:fundamentals/static-files)
* [Routing](xref:fundamentals/routing)
* [Autenticazione](xref:security/authentication/index)
* [Middleware di compressione delle risposte](xref:performance/response-compression)
* [Middleware di riscrittura URL](xref:fundamentals/url-rewriting)

I middleware basati su [OWIN](http://owin.org) sono disponibili per le app ASP.NET Core ed è possibile scrivere il proprio middleware personalizzato.

Per altre informazioni, vedere [Middleware](xref:fundamentals/middleware/index) e [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) (Interfaccia Web aperta per .NET (OWIN).

## <a name="initiate-http-requests"></a>Inizializzare richieste HTTP

Per informazioni sull'uso di `IHttpClientFactory` per accedere alle istanze di `HttpClient` per effettuare richieste HTTP, vedere [Inizializzare richieste HTTP](xref:fundamentals/http-requests).

## <a name="environments"></a>Ambienti

Gli ambienti, come ad esempio "Sviluppo" e "Produzione", sono un concetto fondamentale in ASP.NET Core e possono essere impostati usando variabili di ambiente.

Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments).

## <a name="configuration"></a>Configurazione

ASP.NET Core usa un modello di configurazione basato sulle coppie nome-valore. Il modello di configurazione non è basato su `System.Configuration` o su *web.config*. La configurazione ottiene le impostazioni da un set ordinato di provider di configurazione. I provider di configurazione integrati supportano un'ampia gamma di formati di file (XML, JSON, INI) e di variabili di ambiente per abilitare la configurazione basata sull'ambiente. È anche possibile scrivere i propri provider di configurazione personalizzata.

Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).

## <a name="logging"></a>Registrazione

ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione. I provider predefiniti supportano l'invio dei registri a una o più destinazioni. È possibile usare framework di registrazione di terze parti.

[Registrazione](xref:fundamentals/logging/index)

## <a name="error-handling"></a>Gestione degli errori

ASP.NET Core ha funzionalità incorporate per la gestione degli errori nelle app, tra cui una pagina di eccezioni per gli sviluppatori, pagine di errore personalizzate, pagine di codici di stato statico e la gestione delle eccezioni all'avvio.

Per altre informazioni, vedere [Gestire gli errori](xref:fundamentals/error-handling).

## <a name="routing"></a>Routing

ASP.NET Core offre funzionalità per il routing delle richieste di app ai gestori di route.

Per altre informazioni, vedere [Routing](xref:fundamentals/routing).

## <a name="file-providers"></a>Provider di file

ASP.NET Core astrae l'accesso al file system tramite l'uso di provider di file, che offrono un'interfaccia comune per l'uso di file tra le piattaforme.

Per altre informazioni, vedere [Provider di file](xref:fundamentals/file-providers).

## <a name="static-files"></a>File statici

Il middleware dei file statici viene usato per i file statici, come ad esempio HTML, CSS, file di immagine e JavaScript.

Per altre informazioni, vedere [File statici](xref:fundamentals/static-files).

## <a name="hosting"></a>Hosting

Le app ASP.NET Core configurano e avviano un *host*, che è responsabile della gestione dell'avvio e della durata delle app.

Per altre informazioni, vedere [Hosting in ASP.NET Core](xref:fundamentals/host/index).

## <a name="session-and-application-state"></a>Stato di sessione e applicazione

Lo stato della sessione è una funzionalità di ASP.NET Core che è possibile usare per salvare e archiviare i dati utente mentre l'utente visualizza l'app Web.

Per altre informazioni, vedere [Stato di sessione e applicazione](xref:fundamentals/app-state).

## <a name="servers"></a>Server

Il modello di hosting di ASP.NET Core non è direttamente in ascolto delle richieste. Il modello di hosting si basa su un'implementazione del server HTTP per inoltrare la richiesta all'app. La richiesta inoltrata viene inclusa come un set di oggetti di funzionalità cui è possibile accedere tramite le interfacce. ASP.NET Core include un server Web gestito, multipiattaforma, denominato [Kestrel](xref:fundamentals/servers/kestrel). Kestrel viene spesso eseguito con un server Web di produzione, come ad esempio [IIS](https://www.iis.net/) o [Nginx](http://nginx.org). Kestrel può essere eseguito come un server perimetrale.

Per altre informazioni, vedere [Server](xref:fundamentals/servers/index) e gli argomenti seguenti:

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (in precedenza denominato [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>Globalizzazione e localizzazione

La creazione di un sito Web multilingue con ASP.NET Core consente al sito di raggiungere un gruppo di destinatari più ampio. AP.NET Core offre servizi e middleware per la localizzazione in diverse lingue e culture.

Per altre informazioni, vedere [Globalizzazione e localizzazione](xref:fundamentals/localization).

## <a name="request-features"></a>Funzionalità di richiesta

I dettagli dell'implementazione di server Web relativi alle richieste e alle risposte HTTP vengono definiti nelle interfacce. Le interfacce vengono usate dalle implementazioni del server e dal middleware per creare e modificare la pipeline di hosting dell'app.

Per altre informazioni, vedere [Funzionalità di richiesta](xref:fundamentals/request-features).

## <a name="background-tasks"></a>Attività in background

Le attività in background vengono implementate come *servizi ospitati*. Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).

Per altre informazioni, vedere [Attività in background con servizi ospitati](xref:fundamentals/host/hosted-services).

## <a name="open-web-interface-for-net-owin"></a>Open Web Interface for .NET (OWIN)

ASP.NET Core supporta Open Web Interface for .NET (OWIN). OWIN consente alle app Web di essere disaccoppiate dai server Web.

Per altre informazioni, vedere [OWIN](xref:fundamentals/owin).

## <a name="websockets"></a>Oggetti WebSocket

[WebSocket](https://wikipedia.org/wiki/WebSocket) è un protocollo che consente canali di comunicazione persistente bidirezionale su connessioni TCP. Viene usato per app come chat, teleborsa, giochi e per qualsiasi funzionalità in tempo reale in un'app Web. ASP.NET Core supporta le funzionalità WebSocket.

Per altre informazioni, vedere [Oggetti WebSocket](xref:fundamentals/websockets).

## <a name="microsoftaspnetcoreall-metapackage"></a>Metapacchetto Microsoft.AspNetCore.All

Il metapacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) per ASP.NET include:

* Tutti i pacchetti supportati dal team ASP.NET Core.
* Tutti i pacchetti supportati da Entity Framework Core. 
* Le dipendenze interne e di terze parti usate da ASP.NET Core e da Entity Framework Core.

Per altre informazioni, vedere [Metapacchetto Microsoft.AspNetCore.All ](xref:fundamentals/metapackage).

## <a name="net-core-vs-net-framework-runtime"></a>Runtime .NET core e .NET Framework

Un'app di ASP.NET Core può avere come destinazione il runtime di .NET Core o di .NET Framework.

Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Scegliere tra ASP.NET Core e ASP.NET

Per altre informazioni sulla scelta tra ASP.NET Core e ASP.NET, vedere [Scegliere tra ASP.NET Core e ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).
