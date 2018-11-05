---
title: Avvio dell'app in ASP.NET Core
author: ardalis
description: Informazioni su come la classe Startup in ASP.NET Core configura i servizi e la pipeline delle richieste dell'app.
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: 2212344cb3c651714e8c520b096ab0c4eaf5a180
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206456"
---
# <a name="app-startup-in-aspnet-core"></a>Avvio dell'app in ASP.NET Core

[Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra) e [Luke Latham](https://github.com/guardrex)

La classe `Startup` configura i servizi e la pipeline delle richieste dell'app.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([procedura per il download](xref:index#how-to-download-a-sample)).

## <a name="the-startup-class"></a>Classe Startup

Le app ASP.NET Core usano una classe `Startup` denominata `Startup` per convenzione. La classe `Startup`:

* Può includere facoltativamente un metodo [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) per configurare i servizi dell'app.
* Deve includere un metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) per creare la pipeline di elaborazione delle richieste dell'app.

`ConfigureServices` e `Configure` sono chiamate dal runtime all'avvio dell'app:

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Specificare la classe `Startup` con il metodo [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_):

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

L'host Web offre alcuni servizi che sono disponibili nel costruttore della classe `Startup`. L'app aggiunge servizi aggiuntivi tramite `ConfigureServices`. I servizi dell'host e dell'app saranno quindi disponibili in `Configure` e nell'app.

Un uso comune dell'[inserimento di dipendenze](xref:fundamentals/dependency-injection) nella classe `Startup` consiste nell'inserire:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) per configurare i servizi in base all'ambiente.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) per leggere la configurazione.
* [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) per creare un logger in `Startup.ConfigureServices`.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

Un'alternativa all'inserimento di `IHostingEnvironment` è l'uso di un approccio basato sulle convenzioni. Quando l'app definisce classi `Startup` separate per i diversi ambienti (ad esempio `StartupDevelopment`), la classe `Startup` appropriata viene selezionata durante il runtime. La classe il cui suffisso di nome corrisponde all'ambiente corrente ha la priorità. Se l'app viene eseguita nell'ambiente di sviluppo e include sia una classe `Startup` che una classe `StartupDevelopment`, viene usata la classe `StartupDevelopment`. Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Per altre informazioni su `WebHostBuilder`, vedere l'argomento [Hosting](xref:fundamentals/host/index). Per informazioni sulla gestione degli errori durante l'avvio, vedere [Gestione delle eccezioni durante l'avvio](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Metodo ConfigureServices

Il metodo [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) è:

* Facoltativo
* Chiamato dall'host Web prima del metodo `Configure` per configurare i servizi dell'app.
* Dove le [opzioni di configurazione](xref:fundamentals/configuration/index) sono impostate per convenzione.

Il modello tipico consiste nel chiamare tutti i metodi `Add{Service}` e quindi chiamare tutti i metodi `services.Configure{Service}`. Per un esempio, vedere [Configurare i servizi di identità](xref:security/authentication/identity#pw).

L'aggiunta dei servizi al contenitore dei servizi li rende disponibili all'interno dell'app e nel metodo `Configure`. I servizi vengono risolti tramite l'[inserimento di dipendenze](xref:fundamentals/dependency-injection) o da [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

L'host Web può configurare alcuni servizi prima che vengano chiamati i metodi `Startup`. Per informazioni dettagliate, vedere l'argomento [Hosting in ASP.NET Core](xref:fundamentals/host/index).

Per le funzionalità che richiedono l'installazione sostanziale, sono disponibili i metodi di estensione `Add[Service]` in [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Un'app ASP.NET Core tipica registra i servizi per Entity Framework, Identity e MVC:

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="the-configure-method"></a>Metodo Configure

Il metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) consente di specificare la risposta dell'app alle richieste HTTP. La pipeline viene configurata aggiungendo i componenti [middleware](xref:fundamentals/middleware/index) a un'istanza [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder` è disponibile per il metodo `Configure` ma non viene registrato nel contenitore dei servizi. L'hosting crea `IApplicationBuilder` e lo passa direttamente a `Configure`.

I [modelli ASP.NET Core](/dotnet/core/tools/dotnet-new) configurano la pipeline con il supporto per una pagina delle eccezioni dello sviluppatore, [BrowserLink](http://vswebessentials.com/features/browserlink), pagine di errore, file statici e ASP.NET Core MVC:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Ogni metodo di estensione `Use` aggiunge un componente middleware alla pipeline delle richieste. Ad esempio, il metodo di estensione `UseMvc` aggiunge il [middleware di routing](xref:fundamentals/routing) alla pipeline delle richieste e configura [MVC](xref:mvc/overview) come gestore predefinito.

Ogni componente del middleware è responsabile della chiamata del componente seguente nella pipeline o del corto circuito della catena, se necessario. Se non si verifica un corto circuito lungo la catena del middleware, ogni middleware ha una seconda possibilità di elaborare la richiesta prima che venga inviata al client.

È anche possibile specificare servizi aggiuntivi, ad esempio `IHostingEnvironment` e `ILoggerFactory`, nella firma del metodo. Quando vengono specificati, i servizi aggiuntivi vengono inseriti se disponibili.

Per altre informazioni su come usare `IApplicationBuilder` e sull'ordine di elaborazione del middleware, vedere [Middleware](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Metodi pratici

È possibile usare i metodi pratici [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) e [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) anziché specificare una classe `Startup`. Se vengono effettuate più chiamate a `ConfigureServices`, le chiamate vengono aggiunte l'una all'altra. Se vengono effettuate più chiamate a `Configure`, viene usata l'ultima chiamata al metodo.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="extend-startup-with-startup-filters"></a>Estendere l'avvio con filtri di avvio

Usare [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) per configurare il middleware all'inizio e alla fine della pipeline del middleware [Configure](#the-configure-method) di un'app. `IStartupFilter` è utile per assicurarsi che un middleware venga eseguito prima o dopo il middleware aggiunto dalle librerie all'inizio o alla fine della pipeline di elaborazione delle richieste dell'app.

`IStartupFilter` implementa un singolo metodo, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), che riceve e restituisce `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) definisce una classe per configurare la pipeline delle richieste di un'app. Per altre informazioni, vedere [Creare una pipeline middleware con IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Ogni `IStartupFilter` implementa uno o più middleware nella pipeline delle richieste. I filtri vengono richiamati nell'ordine in cui sono stati aggiunti al contenitore di servizi. Poiché i filtri possono aggiungere il middleware prima o dopo il passaggio del controllo al filtro successivo, i filtri vengono aggiunti all'inizio o alla fine della pipeline dell'app.

L'app di esempio illustra come registrare un middleware con `IStartupFilter`. L'app di esempio include un middleware che imposta un valore di opzioni da un parametro di stringa di query:

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` è configurato nella classe `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` viene registrato nel contenitore dei servizi in [IWebHostBuilder.ConfigureServices](xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.ConfigureServices*) per illustrare come il filtro di avvio estenda `Startup` dall'esterno della classe `Startup`:

[!code-csharp[](startup/sample/Program.cs?name=snippet1&highlight=4-5)]

Quando viene specificato un parametro di stringa di query per `option`, il middleware elabora l'assegnazione del valore prima che il middleware MVC esegua il rendering della risposta:

![Finestra del browser con la pagine di indice di cui è stato eseguito il rendering. Il valore dell'opzione viene visualizzato come 'From Middleware' in base alla richiesta della pagina con il parametro di stringa di query e al valore dell'opzione impostato su 'From Middleware'.](startup/_static/index.png)

L'ordine di esecuzione del middleware viene impostato in base all'ordine delle registrazioni `IStartupFilter`:

* Più implementazioni `IStartupFilter` possono interagire con gli stessi oggetti. Se l'ordinamento è un fattore importante, ordinare le registrazioni di servizio `IStartupFilter` in un ordine corrispondente a quello di esecuzione dei relativi middleware.
* Le librerie possono aggiungere middleware con una o più implementazioni `IStartupFilter` che viene eseguito prima o dopo altri middleware dell'app registrati con `IStartupFilter`. Per richiamare un middleware `IStartupFilter` prima di un middleware aggiunto da `IStartupFilter` di una libreria, posizionare la registrazione dei servizi prima dell'aggiunta della libreria al contenitore di servizi. Per richiamarlo in un secondo momento, posizionare la registrazione dei servizi dopo l'aggiunta della libreria.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Aggiungere elementi di configurazione all'avvio da un assembly esterno

Un' implementazione [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app. Per altre informazioni, vedere [Migliorare un'app da un assembly esterno](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/host/index>
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
