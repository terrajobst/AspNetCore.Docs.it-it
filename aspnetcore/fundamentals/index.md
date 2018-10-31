---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Scoprire i concetti fondamentali per la compilazione di app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/index
ms.openlocfilehash: ab140051648c1640b3c4f382bfd8201c5c0c2039
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207472"
---
# <a name="aspnet-core-fundamentals"></a>Nozioni fondamentali su ASP.NET Core

Un'app ASP.NET Core è un'app console che crea un server Web nel suo metodo `Program.Main`. Il metodo `Main` è il *punto di ingresso gestito* nell'app:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

Host .NET Core:

* Carica il [runtime di .NET Core](https://github.com/dotnet/coreclr).
* Usa il primo argomento della riga di comando come percorso del file binario gestito che contiene il punto di ingresso (`Main`) e inizia l'esecuzione del codice.

Il metodo `Main` richiama [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), che segue il [modello di generatore](https://wikipedia.org/wiki/Builder_pattern) per creare un host Web. Il generatore dispone di metodi che definiscono il server Web (ad esempio, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e la classe di avvio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). Nell'esempio precedente il server Web [Kestrel](xref:fundamentals/servers/kestrel) viene allocato automaticamente. L'host Web di ASP.NET Core tenta l'esecuzione in IIS, se disponibile. Altri server Web, come [HTTP.sys](xref:fundamentals/servers/httpsys), possono essere usati richiamando il metodo di estensione appropriato. `UseStartup` viene spiegato dettagliatamente nella sezione successiva.

<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, il tipo restituito della chiamata `WebHost.CreateDefaultBuilder`, offre molti metodi facoltativi. Alcuni di questi metodi includono `UseHttpSys` per ospitare l'app in HTTP.sys e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> per specificare la directory del contenuto radice. I metodi <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilano l'oggetto <xref:Microsoft.AspNetCore.Hosting.IWebHost> che ospita l'app e inizia l'ascolto delle richieste HTTP.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

Host .NET Core:

* Carica il [runtime di .NET Core](https://github.com/dotnet/coreclr).
* Usa il primo argomento della riga di comando come percorso del file binario gestito che contiene il punto di ingresso (`Main`) e inizia l'esecuzione del codice.

Il metodo `Main` usa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, che segue il [modello di generatore](https://wikipedia.org/wiki/Builder_pattern) per creare un host app Web. Il generatore dispone di metodi che definiscono il server Web (ad esempio, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e la classe di avvio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). Nell'esempio precedente viene usato il server Web [Kestrel](xref:fundamentals/servers/kestrel). Altri server Web, come [WebListener](xref:fundamentals/servers/weblistener), possono essere usati richiamando il metodo di estensione appropriato. `UseStartup` viene spiegato dettagliatamente nella sezione successiva.

`WebHostBuilder` offre molti metodi facoltativi, incluso <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> per l'hosting in IIS e IIS Express e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> per specificare la directory del contenuto radice. I metodi <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilano l'oggetto <xref:Microsoft.AspNetCore.Hosting.IWebHost> che ospita l'app e inizia l'ascolto delle richieste HTTP.

::: moniker-end

## <a name="startup"></a>Avvio

Il metodo `UseStartup` su `WebHostBuilder` specifica la classe `Startup` per l'app:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

Nella classe `Startup` viene definita la pipeline di gestione delle richieste e vengono configurati tutti i servizi necessari per l'app. La classe `Startup` deve essere pubblica e deve contenere i metodi seguenti:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> definisce i [servizi](#dependency-injection-services) usati dall'app, come ad esempio ASP.NET Core MVC, Entity Framework Core, Identity. <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> definisce il [middleware](xref:fundamentals/middleware/index) chiamato nella pipeline delle richieste.

Per ulteriori informazioni, vedere <xref:fundamentals/startup>.

## <a name="content-root"></a>Radice del contenuto

La radice del contenuto è il percorso di base per i contenuti usati dall'app, ad esempio [Razor Pages](xref:razor-pages/index), visualizzazioni MVC e asset statici. Per impostazione predefinita, la posizione della radice del contenuto è uguale al percorso di base dell'app per il file eseguibile che ospita l'app.

## <a name="web-root-webroot"></a>Radice Web (webroot)

La radice Web di un'app è la directory del progetto contenente risorse statiche pubbliche, come ad esempio CSS, JavaScript e i file di immagine. Per impostazione predefinita *wwwroot* è la radice Web.

Per i file Razor (*. cshtml*), il carattere tilde `~/` punta alla radice Web. I percorsi che iniziano con `~/` sono indicati come percorsi virtuali.

## <a name="dependency-injection-services"></a>Iniezione di dipendenze (servizi)

Un *servizio* è un componente destinato a un utilizzo comune in un'app. I servizi sono resi disponibili tramite l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection). ASP.NET Core include un'inversione nativa del contenitore del controllo (IoC) che supporta l'[inserimento del costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per impostazione predefinita. Se si vuole, è possibile sostituire il contenitore predefinito. Oltre al vantaggio dell'[accoppiamento debole](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), l'iniezione delle dipendenze rende disponibili i servizi in tutta l'app, ad esempio la [registrazione](xref:fundamentals/logging/index).

Per ulteriori informazioni, vedere <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Middleware

In ASP.NET Core la pipeline della richiesta viene composta usando un [middleware](xref:fundamentals/middleware/index). Il middleware ASP.NET Core esegue le operazioni asincrone su un `HttpContext`, quindi richiama il middleware successivo nella pipeline oppure termina la richiesta.

Per convenzione viene aggiunto un componente middleware denominato "XYZ" alla pipeline richiamando un metodo di estensione `UseXYZ` nel metodo `Configure`.

ASP.NET Core include un ampio set di middleware predefinito ed è possibile scrivere middleware personalizzato. [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), che consente di disaccoppiare le app Web dai server Web, è supportato nelle app ASP.NET Core.

Per altre informazioni, vedere <xref:fundamentals/middleware/index> e <xref:fundamentals/owin>.

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Inizializzare richieste HTTP

<xref:System.Net.Http.IHttpClientFactory> è disponibile per accedere alle istanze di <xref:System.Net.Http.HttpClient> per effettuare richieste HTTP.

Per ulteriori informazioni, vedere <xref:fundamentals/http-requests>.

::: moniker-end

## <a name="environments"></a>Ambienti

Gli ambienti, come ad esempio *Sviluppo* e *Produzione*, sono un concetto fondamentale in ASP.NET Core e possono essere impostati usando una variabile di ambiente, un file di impostazioni e un argomento della riga di comando.

Per ulteriori informazioni, vedere <xref:fundamentals/environments>.

## <a name="hosting"></a>Hosting

Le app ASP.NET Core configurano e avviano un *host*, che è responsabile della gestione dell'avvio e della durata delle app.

Per ulteriori informazioni, vedere <xref:fundamentals/host/index>.

## <a name="servers"></a>Server

Il modello di hosting di ASP.NET Core non è direttamente in ascolto delle richieste. Il modello di hosting si basa su un'implementazione del server HTTP per inoltrare la richiesta all'app. La richiesta inoltrata viene inclusa come un set di oggetti di funzionalità cui è possibile accedere tramite le interfacce. ASP.NET Core include un server Web gestito, multipiattaforma, denominato [Kestrel](xref:fundamentals/servers/kestrel). Kestrel viene eseguito di solito con un server Web di produzione, ad esempio [IIS](https://www.iis.net/) oppure [Nginx](http://nginx.org) in una configurazione con proxy inverso. Kestrel può essere eseguito anche come server perimetrale pubblico esposto direttamente a Internet in ASP.NET Core 2.0 o versione successiva.

Per ulteriori informazioni, vedere <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Configurazione

ASP.NET Core usa un modello di configurazione basato sulle coppie nome-valore. Il modello di configurazione non è basato su <xref:System.Configuration> o su *web.config*. La configurazione ottiene le impostazioni da un set ordinato di provider di configurazione. I provider di configurazione predefiniti supportano un'ampia gamma di formati di file (XML, JSON, INI), di variabili di ambiente e di argomenti della riga di comando. È anche possibile scrivere i propri provider di configurazione personalizzata.

Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.

## <a name="logging"></a>Registrazione

ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione. I provider predefiniti supportano l'invio dei registri a una o più destinazioni. È possibile usare framework di registrazione di terze parti.

Per ulteriori informazioni, vedere <xref:fundamentals/logging/index>.

## <a name="error-handling"></a>Gestione degli errori

ASP.NET Core include scenari predefiniti per la gestione degli errori nelle app, tra cui una pagina di eccezioni per gli sviluppatori, pagine di errore personalizzate, pagine di codici di stato statici e la gestione delle eccezioni all'avvio.

Per ulteriori informazioni, vedere <xref:fundamentals/error-handling>.

## <a name="routing"></a>Routing

ASP.NET Core offre scenari per il routing delle richieste di app ai gestori di route.

Per ulteriori informazioni, vedere <xref:fundamentals/routing>.

## <a name="background-tasks"></a>Attività in background

Le attività in background vengono implementate come *servizi ospitati*. Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.

Per ulteriori informazioni, vedere <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Accedere a HttpContext

`HttpContext` è disponibile automaticamente durante l'elaborazione delle richieste con Razor Pages e MVC. In circostanze in cui `HttpContext` non è immediatamente disponibile, è possibile accedere a `HttpContext` tramite l'interfaccia <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> e la relativa implementazione predefinita, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.

Per ulteriori informazioni, vedere <xref:fundamentals/httpcontext>.
