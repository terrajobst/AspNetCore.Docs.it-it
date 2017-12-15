---
title: Avvio dell'applicazione in ASP.NET Core
author: ardalis
description: Individuazione come la classe di avvio in ASP.NET Core consente di configurare servizi e delle pipeline delle richieste dell'applicazione.
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 8adb96c7261a2e7b1556f0daddcf6f135862b53a
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2017
---
# <a name="application-startup-in-aspnet-core"></a>Avvio dell'applicazione in ASP.NET Core

Da [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), e [Luke Latham](https://github.com/guardrex)

La `Startup` classe consente di configurare servizi e delle pipeline delle richieste dell'applicazione.

## <a name="the-startup-class"></a>La classe di avvio

Utilizzare le applicazioni ASP.NET Core un `Startup` (classe), denominato `Startup` per convenzione. La `Startup` classe:

* Può includere facoltativamente un [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) metodo per configurare i servizi dell'applicazione.
* Deve includere un [configura](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) metodo per creare una pipeline di elaborazione delle richieste dell'applicazione.

`ConfigureServices`e `Configure` vengono chiamati dal runtime all'avvio dell'app:

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Specificare il `Startup` classe con il [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) metodo:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Il `Startup` costruttore della classe accetta le dipendenze definite dall'host. Un utilizzo comune di [inserimento di dipendenze](xref:fundamentals/dependency-injection) nel `Startup` classe è per inserire [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) per configurare i servizi dall'ambiente:

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Un'alternativa all'inserimento `IHostingStartup` consiste nell'usare un approccio basato su convenzioni. L'app è possibile definire separato `Startup` classi per diversi ambienti (ad esempio, `StartupDevelopment`), e la classe di avvio appropriato viene selezionata in fase di esecuzione. La classe il cui suffisso di nome corrispondente all'ambiente corrente ha la priorità. Se l'app viene eseguita nell'ambiente di sviluppo e include sia un `Startup` classe e un `StartupDevelopment` (classe), la `StartupDevelopment` classe viene utilizzata. Per ulteriori informazioni vedere [utilizzo di più ambienti](xref:fundamentals/environments#startup-conventions).

Per altre informazioni, vedere `WebHostBuilder`, vedere il [Hosting](xref:fundamentals/hosting) argomento. Per informazioni sulla gestione degli errori durante l'avvio, vedere [la gestione delle eccezioni di avvio](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Il metodo ConfigureServices

Il [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) metodo:

* Parametro facoltativo.
* Chiamato dall'host web prima di `Configure` metodo per configurare i servizi dell'applicazione.
* Dove [opzioni di configurazione](xref:fundamentals/configuration/index) vengono impostate per convenzione.

Aggiunta dei servizi al contenitore del servizio li rende disponibili all'interno dell'app e il `Configure` metodo. I servizi vengono risolti tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection) o da [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

L'host web può configurare alcuni servizi prima `Startup` metodi vengono chiamati. I dettagli sono disponibili nel [Hosting](xref:fundamentals/hosting) argomento. 

Per le funzionalità che richiedono l'installazione sostanziale, esistono `Add[Service]` metodi di estensione in [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Un'app web tipico registra services per Entity Framework, l'identità e MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>Servizi disponibili nella finestra di avvio

L'host web fornisce alcuni servizi che sono disponibili per il `Startup` costruttore della classe. L'applicazione aggiunge servizi aggiuntivi tramite `ConfigureServices`. Servizi host e app sono quindi disponibili in `Configure` e in tutta l'applicazione.

## <a name="the-configure-method"></a>Metodo Configure

Il [configura](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) consente di specificare la modalità con cui l'app risponde alle richieste HTTP. La pipeline delle richieste è configurata tramite l'aggiunta di [middleware](xref:fundamentals/middleware) componenti da un [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) istanza. `IApplicationBuilder`è disponibile per il `Configure` (metodo), ma non è registrato nel contenitore dei servizi. Crea un `IApplicationBuilder` e lo passa direttamente al `Configure` ([origine riferimento](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

Il [modelli ASP.NET Core](/dotnet/core/tools/dotnet-new) configurare la pipeline con il supporto per una pagina di eccezione developer [BrowserLink](http://vswebessentials.com/features/browserlink), ASP.NET MVC, file statici e pagine di errore:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Ogni `Use` metodo di estensione aggiunge un componente del middleware alla pipeline delle richieste. Ad esempio, il `UseMvc` metodo di estensione aggiunge la [middleware routing](xref:fundamentals/routing) alla pipeline delle richieste e configura [MVC](xref:mvc/overview) come gestore predefinito.

Servizi aggiuntivi, ad esempio `IHostingEnvironment` e `ILoggerFactory`, può anche essere specificato nella firma del metodo. Quando specificato, vengono inseriti servizi aggiuntivi se sono disponibili.

Per ulteriori informazioni su come usare `IApplicationBuilder`, vedere [Middleware](xref:fundamentals/middleware).

## <a name="convenience-methods"></a>Metodi pratici

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) e [configura](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) metodi pratici consente invece di specificare un `Startup` classe. Più chiamate a `ConfigureServices` aggiungere uno a altro. Più chiamate al metodo `Configure` usare l'ultima chiamata al metodo.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a>Filtri di avvio

Utilizzare [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) per configurare middleware all'inizio o alla fine di un'app [configura](#the-configure-method) pipeline middleware. `IStartupFilter`è utile per garantire l'esecuzione di un tipo di middleware prima o dopo l'aggiunta da parte di librerie all'inizio o alla fine della pipeline di elaborazione di richieste dell'applicazione middleware.

`IStartupFilter`implementa un singolo metodo, [configura](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), che riceve e restituisce un `Action<IApplicationBuilder>`. Un [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) definisce una classe per configurare la pipeline di richieste di un'app. Per ulteriori informazioni, vedere [creazione di una pipeline middleware con IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).

Ogni `IStartupFilter` implementa uno o più middlewares nella pipeline delle richieste. I filtri vengono richiamati nell'ordine in che cui sono stati aggiunti al contenitore del servizio. I filtri possono aggiungere middleware prima o dopo il passaggio di controllo per il filtro successivo, in questo modo viene aggiunta all'inizio o alla fine della pipeline dell'applicazione.

Il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)) viene illustrato come registrare un tipo di middleware con `IStartupFilter`. L'app di esempio include un tipo di middleware che imposta un valore per le opzioni da un parametro di stringa di query:

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

Il `RequestSetOptionsMiddleware` è configurato nel `RequestSetOptionsStartupFilter` classe:

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

Il `IStartupFilter` è registrato nel contenitore dei servizi in `ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Quando un parametro di stringa di query per `option` viene fornito, il middleware elabora il valore assegnato alla prima il middleware MVC esegue il rendering nella risposta:

![Finestra del browser che illustra la pagina di indice viene eseguito il rendering. Il valore dell'opzione viene eseguito il rendering in base che richiede la pagina con il parametro di stringa di query e il valore di opzione è impostata su 'Dal Middleware' 'Dal Middleware'.](startup/_static/index.png)

Ordine di esecuzione di middleware è impostata dall'ordine delle `IStartupFilter` registrazioni:

* Più `IStartupFilter` implementazioni possono interagire con gli stessi oggetti. Se l'ordine è importante, ordinare i `IStartupFilter` registrazioni per corrispondere all'ordine che deve essere eseguita loro middlewares del servizio.
* Librerie possono aggiungere middleware con uno o più `IStartupFilter` implementazioni da eseguire prima o dopo l'altro middleware app registrato con `IStartupFilter`. Per richiamare un `IStartupFilter` middleware prima di un tipo di middleware aggiunto da una libreria `IStartupFilter`, posizionare la registrazione del servizio prima che la libreria viene aggiunta al contenitore del servizio. Per richiamare in seguito, posizionare la registrazione del servizio dopo aver aggiunto la libreria.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Hosting](xref:fundamentals/hosting)
* [Uso di più ambienti](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware)
* [Registrazione](xref:fundamentals/logging/index)
* [Configurazione](xref:fundamentals/configuration/index)
* [Classe StartupLoader: metodo FindStartupType (origine di riferimento)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116))
