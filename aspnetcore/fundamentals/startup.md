---
title: Avvio dell'applicazione in ASP.NET Core
author: ardalis
description: Informazioni su come la classe Startup in ASP.NET Core configura i servizi e la pipeline delle richieste dell'app.
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: f0b907e4322809dfe2bcd287bb064f35f5ebe150
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36314120"
---
# <a name="application-startup-in-aspnet-core"></a>Avvio dell'applicazione in ASP.NET Core

[Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra) e [Luke Latham](https://github.com/guardrex)

La classe `Startup` configura i servizi e la pipeline delle richieste dell'app.

## <a name="the-startup-class"></a>Classe Startup

Le app ASP.NET Core usano una classe `Startup` denominata `Startup` per convenzione. La classe `Startup`:

* Può includere facoltativamente un metodo [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) per configurare i servizi dell'app.
* Deve includere un metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) per creare la pipeline di elaborazione delle richieste dell'app.

`ConfigureServices` e `Configure` sono chiamate dal runtime all'avvio dell'app:

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Specificare la classe `Startup` con il metodo [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_):

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Il costruttore della classe `Startup` accetta le dipendenze definite dall'host. Un uso comune dell'[inserimento di dipendenze](xref:fundamentals/dependency-injection) nella classe `Startup` consiste nell'inserire:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) per configurare i servizi in base all'ambiente.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) per configurare l'app durante l'avvio.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

Un'alternativa all'inserimento di `IHostingEnvironment` è l'uso di un approccio basato sulle convenzioni. L'app può definire classi `Startup` separate per i diversi ambienti (ad esempio, `StartupDevelopment`) e la classe di avvio appropriata viene selezionata durante il runtime. La classe il cui suffisso di nome corrisponde all'ambiente corrente ha la priorità. Se l'app viene eseguita nell'ambiente di sviluppo e include sia una classe `Startup` che una classe `StartupDevelopment`, viene usata la classe `StartupDevelopment`. Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Per altre informazioni su `WebHostBuilder`, vedere l'argomento [Hosting](xref:fundamentals/host/index). Per informazioni sulla gestione degli errori durante l'avvio, vedere [Gestione delle eccezioni durante l'avvio](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Metodo ConfigureServices

Il metodo [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) è:

* Facoltativo
* Chiamato dall'host Web prima del metodo `Configure` per configurare i servizi dell'app.
* Dove le [opzioni di configurazione](xref:fundamentals/configuration/index) sono impostate per convenzione.

L'aggiunta dei servizi al contenitore dei servizi li rende disponibili all'interno dell'app e nel metodo `Configure`. I servizi vengono risolti tramite l'[inserimento di dipendenze](xref:fundamentals/dependency-injection) o da [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

L'host Web può configurare alcuni servizi prima che vengano chiamati i metodi `Startup`. Per informazioni dettagliate, vedere l'argomento [Hosting in ASP.NET Core](xref:fundamentals/host/index).

Per le funzionalità che richiedono l'installazione sostanziale, sono disponibili i metodi di estensione `Add[Service]` in [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Un'app Web tipica registra i servizi per Entity Framework, Identity e MVC:

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

::: moniker range=">= aspnetcore-2.1"

<a name="setcompatibilityversion"></a>

### <a name="setcompatibilityversion-for-aspnet-core-mvc"></a>SetCompatibilityVersion per ASP.NET Core MVC 

Il metodo `SetCompatibilityVersion` consente a un'app di acconsentire o rifiutare esplicitamente modifiche potenzialmente importanti del comportamento introdotte in ASP.NET Core MVC 2.1+. Queste modifiche potenzialmente importanti del comportamento riguardano in genere il funzionamento del sottosistema MVC e la modalità con cui il **codice dell'utente** viene chiamato dal runtime. Se si acconsente esplicitamente, si ottiene il comportamento più recente e il comportamento a lungo termine di ASP.NET Core.

Il codice seguente imposta la modalità di compatibilità su ASP.NET Core 2.1:

[!code-csharp[Main](startup/sampleCompatibility/Startup.cs?name=snippet1)]

È consigliabile testare l'applicazione usando la versione più recente (`CompatibilityVersion.Version_2_1`). Microsoft prevede che la maggior parte delle applicazioni non riscontrerà modifiche importanti del comportamento quando viene usata la versione più recente. 

Le applicazioni che chiamano `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` sono protette dalle modifiche potenzialmente importanti del comportamento introdotte in ASP.NET Core 2.1 MVC e versioni 2.x successive. La protezione:

* Non è valida per tutte le modifiche 2.1 e successive, ma è destinata alle modifiche potenzialmente importanti del comportamento del runtime ASP.NET Core nel sottosistema MVC.
* Non viene estesa alla versione principale successiva.

La compatibilità predefinita per le applicazioni ASP.NET Core 2.1 e versioni 2.x che **non** chiamano `SetCompatibilityVersion` è la compatibilità 2.0. In altri termini il fatto di non chiamare `SetCompatibilityVersion` equivale a chiamare `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Il codice seguente imposta la modalità di compatibilità su ASP.NET Core 2.1, con l'eccezione dei comportamenti seguenti:

* [AllowCombiningAuthorizeFilters](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [InputFormatterExceptionPolicy](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](startup/sampleCompatibility/Startup2.cs?name=snippet1)]

Per le app che riscontrano modifiche potenzialmente importanti del comportamento, l'uso delle opzioni di compatibilità appropriate:

* Consente di usare la versione più recente e di rifiutare esplicitamente modifiche importanti del comportamento specifiche.
* Garantisce il tempo necessario per l'aggiornamento dell'app, che in tal modo potrà funzionare con le modifiche più recenti.

I commenti relativi alla classe di origine [MvcOptions](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) offrono una spiegazione chiara delle modifiche e dei motivi per cui queste rappresentano un miglioramento per la maggior parte degli utenti.

In una data futura verrà rilasciata la [versione ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap). I comportamenti precedenti supportati dalle opzioni di compatibilità verranno rimossi nella versione 3.0. Queste modifiche positive andranno a vantaggio della maggior parte degli utenti. Se si introducono ora queste modifiche, la maggior parte delle applicazioni può trarne vantaggio da subito e si dispone del tempo sufficiente per aggiornare le altre applicazioni.

::: moniker-end

## <a name="services-available-in-startup"></a>Servizi disponibili in Startup

L'host Web offre alcuni servizi che sono disponibili nel costruttore della classe `Startup`. L'app aggiunge servizi aggiuntivi tramite `ConfigureServices`. I servizi dell'host e dell'app saranno quindi disponibili in `Configure` e nell'applicazione.

## <a name="the-configure-method"></a>Metodo Configure

Il metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) consente di specificare la risposta dell'app alle richieste HTTP. La pipeline viene configurata aggiungendo i componenti [middleware](xref:fundamentals/middleware/index) a un'istanza [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder` è disponibile per il metodo `Configure` ma non viene registrato nel contenitore dei servizi. L'hosting crea `IApplicationBuilder` e lo passa direttamente a `Configure` ([origine riferimento](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

I [modelli ASP.NET Core](/dotnet/core/tools/dotnet-new) configurano la pipeline con il supporto per una pagina delle eccezioni dello sviluppatore, [BrowserLink](http://vswebessentials.com/features/browserlink), pagine di errore, file statici e ASP.NET MVC:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Ogni metodo di estensione `Use` aggiunge un componente middleware alla pipeline delle richieste. Ad esempio, il metodo di estensione `UseMvc` aggiunge il [middleware di routing](xref:fundamentals/routing) alla pipeline delle richieste e configura [MVC](xref:mvc/overview) come gestore predefinito.

Ogni componente del middleware è responsabile della chiamata del componente seguente nella pipeline o del corto circuito della catena, se necessario. Se non si verifica un corto circuito lungo la catena del middleware, ogni middleware ha una seconda possibilità di elaborare la richiesta prima che venga inviata al client.

È anche possibile specificare servizi aggiuntivi, ad esempio `IHostingEnvironment` e `ILoggerFactory`, nella firma del metodo. Quando vengono specificati, i servizi aggiuntivi vengono inseriti se disponibili.

Per altre informazioni su come usare `IApplicationBuilder` e sull'ordine di elaborazione del middleware, vedere [Middleware](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Metodi pratici

È possibile usare i metodi pratici [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) e [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) anziché specificare una classe `Startup`. Se vengono effettuate più chiamate a `ConfigureServices`, le chiamate vengono aggiunte l'una all'altra. Se vengono effettuate più chiamate a `Configure`, viene usata l'ultima chiamata al metodo.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Filtri di avvio

Usare [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) per configurare il middleware all'inizio e alla fine della pipeline del middleware [Configure](#the-configure-method) di un'app. `IStartupFilter` è utile per assicurarsi che un middleware venga eseguito prima o dopo il middleware aggiunto dalle librerie all'inizio o alla fine della pipeline di elaborazione delle richieste dell'app.

`IStartupFilter` implementa un singolo metodo, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), che riceve e restituisce `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) definisce una classe per configurare la pipeline delle richieste di un'app. Per altre informazioni, vedere [Creare una pipeline middleware con IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder).

Ogni `IStartupFilter` implementa uno o più middleware nella pipeline delle richieste. I filtri vengono richiamati nell'ordine in cui sono stati aggiunti al contenitore di servizi. Poiché i filtri possono aggiungere il middleware prima o dopo il passaggio del controllo al filtro successivo, i filtri vengono aggiunti all'inizio o alla fine della pipeline dell'app.

L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([come eseguire il download](xref:tutorials/index#how-to-download-a-sample)) illustra come registrare un middleware con `IStartupFilter`. L'app di esempio include un middleware che imposta un valore di opzioni da un parametro di stringa di query:

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` è configurato nella classe `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` è registrato nel contenitore di servizi in `ConfigureServices`:

[!code-csharp[](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Quando viene specificato un parametro di stringa di query per `option`, il middleware elabora l'assegnazione del valore prima che il middleware MVC esegua il rendering della risposta:

![Finestra del browser con la pagine di indice di cui è stato eseguito il rendering. Il valore dell'opzione viene visualizzato come 'From Middleware' in base alla richiesta della pagina con il parametro di stringa di query e al valore dell'opzione impostato su 'From Middleware'.](startup/_static/index.png)

L'ordine di esecuzione del middleware viene impostato in base all'ordine delle registrazioni `IStartupFilter`:

* Più implementazioni `IStartupFilter` possono interagire con gli stessi oggetti. Se l'ordinamento è un fattore importante, ordinare le registrazioni di servizio `IStartupFilter` in un ordine corrispondente a quello di esecuzione dei relativi middleware.
* Le librerie possono aggiungere middleware con una o più implementazioni `IStartupFilter` che viene eseguito prima o dopo altri middleware dell'app registrati con `IStartupFilter`. Per richiamare un middleware `IStartupFilter` prima di un middleware aggiunto da `IStartupFilter` di una libreria, posizionare la registrazione dei servizi prima dell'aggiunta della libreria al contenitore di servizi. Per richiamarlo in un secondo momento, posizionare la registrazione dei servizi dopo l'aggiunta della libreria.

## <a name="adding-configuration-at-startup-from-an-external-assembly"></a>Aggiunta di elementi di configurazione all'avvio da un assembly esterno

Un' implementazione [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app. Per altre informazioni, vedere [Migliorare un'app da un assembly esterno](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Hosting](xref:fundamentals/host/index)
* [Usare più ambienti](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware/index)
* [Registrazione](xref:fundamentals/logging/index)
* [Configurazione](xref:fundamentals/configuration/index)
* [Classe StartupLoader: metodo FindStartupType (origine riferimento)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
