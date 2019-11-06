---
title: Avvio dell'app in ASP.NET Core
author: rick-anderson
description: Informazioni su come la classe Startup in ASP.NET Core configura i servizi e la pipeline delle richieste dell'app.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/02/2019
uid: fundamentals/startup
ms.openlocfilehash: 081eaa772d136477a37a3392877886327e0cda7c
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634042"
---
# <a name="app-startup-in-aspnet-core"></a><span data-ttu-id="ac6cc-103">Avvio dell'app in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac6cc-103">App startup in ASP.NET Core</span></span>

<span data-ttu-id="ac6cc-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex) e [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="ac6cc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="ac6cc-105">La classe `Startup` configura i servizi e la pipeline delle richieste dell'app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="ac6cc-106">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="ac6cc-106">The Startup class</span></span>

<span data-ttu-id="ac6cc-107">Le app ASP.NET Core usano una classe `Startup` denominata `Startup` per convenzione.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="ac6cc-108">La classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-108">The `Startup` class:</span></span>

* <span data-ttu-id="ac6cc-109">Include facoltativamente un metodo <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> per configurare i *servizi* dell'app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-109">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="ac6cc-110">Un servizio è un componente riutilizzabile che fornisce la funzionalità delle app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-110">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="ac6cc-111">I servizi vengono *registrati* in `ConfigureServices` e utilizzati nell'app tramite l' [inserimento di dipendenze (DI)](xref:fundamentals/dependency-injection) o <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-111">Services are *registered* in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="ac6cc-112">Include un metodo <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> per creare la pipeline di elaborazione delle richieste dell'app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-112">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="ac6cc-113">`ConfigureServices` e `Configure` sono chiamate dal runtime di ASP.NET Core all'avvio dell'app:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-113">`ConfigureServices` and `Configure` are called by the ASP.NET Core runtime when the app starts:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="ac6cc-114">L'esempio precedente è per [Razor Pages](xref:razor-pages/index). La versione per MVC è simile.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-114">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

<span data-ttu-id="ac6cc-115">La classe `Startup` viene specificata al momento della compilazione dell'[host](xref:fundamentals/index#host) dell'app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-115">The `Startup` class is specified when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="ac6cc-116">La classe `Startup` viene in genere specificata chiamando il metodo [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) nel generatore host:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-116">The `Startup` class is typically specified by calling the [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

<span data-ttu-id="ac6cc-117">L'host fornisce i servizi disponibili al costruttore della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-117">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="ac6cc-118">L'app aggiunge servizi aggiuntivi tramite `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-118">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="ac6cc-119">I servizi dell'host e dell'app saranno disponibili in `Configure` e nell'app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-119">Both the host and app services are available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="ac6cc-120">Quando si usa l' [host generico](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>), è possibile inserire solo i tipi di servizio seguenti nel costruttore `Startup`:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-120">Only the following service types can be injected into the `Startup` constructor when using the [Generic Host](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

<span data-ttu-id="ac6cc-121">La maggior parte dei servizi non è disponibile fino a quando non viene chiamato il metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-121">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac6cc-122">L'host fornisce i servizi disponibili al costruttore della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-122">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="ac6cc-123">L'app aggiunge servizi aggiuntivi tramite `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-123">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="ac6cc-124">I servizi dell'host e dell'app saranno quindi disponibili in `Configure` e nell'app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-124">Both the host and app services are then available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="ac6cc-125">Un uso comune dell'[inserimento di dipendenze](xref:fundamentals/dependency-injection) nella classe `Startup` consiste nell'inserire:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-125">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="ac6cc-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> per configurare i servizi in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-126"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> to configure services by environment.</span></span>
* <span data-ttu-id="ac6cc-127"><xref:Microsoft.Extensions.Configuration.IConfiguration> per leggere la configurazione.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-127"><xref:Microsoft.Extensions.Configuration.IConfiguration> to read configuration.</span></span>
* <span data-ttu-id="ac6cc-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory> per creare un logger in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-128"><xref:Microsoft.Extensions.Logging.ILoggerFactory> to create a logger in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

<span data-ttu-id="ac6cc-129">La maggior parte dei servizi non è disponibile fino a quando non viene chiamato il metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-129">Most services are not available until the `Configure` method is called.</span></span>

::: moniker-end

### <a name="multiple-startup"></a><span data-ttu-id="ac6cc-130">Avvio multiplo</span><span class="sxs-lookup"><span data-stu-id="ac6cc-130">Multiple Startup</span></span>

<span data-ttu-id="ac6cc-131">Quando l'app definisce classi `Startup` separate per i diversi ambienti (ad esempio `StartupDevelopment`), la classe `Startup` appropriata viene selezionata durante il runtime.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-131">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="ac6cc-132">La classe il cui suffisso di nome corrisponde all'ambiente corrente ha la priorità.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-132">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="ac6cc-133">Se l'app viene eseguita nell'ambiente di sviluppo e include sia una classe `Startup` che una classe `StartupDevelopment`, viene usata la classe `StartupDevelopment`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-133">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="ac6cc-134">Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span><span class="sxs-lookup"><span data-stu-id="ac6cc-134">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="ac6cc-135">Per altre informazioni sull'host, vedere [L'host](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="ac6cc-135">See [The host](xref:fundamentals/index#host) for more information on the host.</span></span> <span data-ttu-id="ac6cc-136">Per informazioni sulla gestione degli errori durante l'avvio, vedere [Gestione delle eccezioni durante l'avvio](xref:fundamentals/error-handling#startup-exception-handling).</span><span class="sxs-lookup"><span data-stu-id="ac6cc-136">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="ac6cc-137">Metodo ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="ac6cc-137">The ConfigureServices method</span></span>

<span data-ttu-id="ac6cc-138">Il metodo <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> è:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-138">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="ac6cc-139">Parametro facoltativo.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-139">Optional.</span></span>
* <span data-ttu-id="ac6cc-140">Chiamato dall'host prima del metodo `Configure` per configurare i servizi dell'app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-140">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="ac6cc-141">Dove le [opzioni di configurazione](xref:fundamentals/configuration/index) sono impostate per convenzione.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-141">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="ac6cc-142">L'host può configurare alcuni servizi prima che vengano chiamati i metodi `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-142">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="ac6cc-143">Per altre informazioni, vedere [L'host](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="ac6cc-143">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="ac6cc-144">Per le funzionalità che richiedono l'installazione sostanziale, sono disponibili i metodi di estensione `Add{Service}` in <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-144">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="ac6cc-145">Ad esempio, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores e **Add**RazorPages:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-145">For example, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores, and **Add**RazorPages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

<span data-ttu-id="ac6cc-146">L'aggiunta dei servizi al contenitore dei servizi li rende disponibili all'interno dell'app e nel metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-146">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="ac6cc-147">I servizi vengono risolti tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection) o da <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-147">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac6cc-148">Vedere [SetCompatibilityVersion](xref:mvc/compatibility-version) per altre informazioni su `SetCompatibilityVersion`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-148">See [SetCompatibilityVersion](xref:mvc/compatibility-version) for more information on `SetCompatibilityVersion`.</span></span>

::: moniker-end

## <a name="the-configure-method"></a><span data-ttu-id="ac6cc-149">Metodo Configure</span><span class="sxs-lookup"><span data-stu-id="ac6cc-149">The Configure method</span></span>

<span data-ttu-id="ac6cc-150">Il metodo <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> viene usato per specificare come risponde l'app alle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-150">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="ac6cc-151">La pipeline delle richieste viene configurata aggiungendo i componenti [middleware](xref:fundamentals/middleware/index) a un'istanza <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-151">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="ac6cc-152">`IApplicationBuilder` è disponibile per il metodo `Configure` ma non viene registrato nel contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-152">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="ac6cc-153">L'hosting crea `IApplicationBuilder` e lo passa direttamente a `Configure`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-153">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="ac6cc-154">I [modelli ASP.NET Core](/dotnet/core/tools/dotnet-new) configurano la pipeline con il supporto per:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-154">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="ac6cc-155">Pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="ac6cc-155">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="ac6cc-156">Gestore di eccezioni</span><span class="sxs-lookup"><span data-stu-id="ac6cc-156">Exception handler</span></span>](xref:fundamentals/error-handling#exception-handler-page)
* [<span data-ttu-id="ac6cc-157">Protocollo HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="ac6cc-157">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="ac6cc-158">Reindirizzamento HTTPS</span><span class="sxs-lookup"><span data-stu-id="ac6cc-158">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="ac6cc-159">File statici</span><span class="sxs-lookup"><span data-stu-id="ac6cc-159">Static files</span></span>](xref:fundamentals/static-files)
* <span data-ttu-id="ac6cc-160">ASP.NET Core [MVC](xref:mvc/overview) e [Razor Pages](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="ac6cc-160">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>

::: moniker range="< aspnetcore-3.0"

* [<span data-ttu-id="ac6cc-161">Regolamento generale sulla protezione dei dati (GDPR)</span><span class="sxs-lookup"><span data-stu-id="ac6cc-161">General Data Protection Regulation (GDPR)</span></span>](xref:security/gdpr)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="ac6cc-162">L'esempio precedente è per [Razor Pages](xref:razor-pages/index). La versione per MVC è simile.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-162">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

::: moniker-end

<span data-ttu-id="ac6cc-163">Ogni metodo di estensione `Use` aggiunge uno o più componenti middleware alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-163">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="ac6cc-164">Ad esempio, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configura il [middleware](xref:fundamentals/middleware/index) per fornire i [file statici](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="ac6cc-164">For instance, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configures [middleware](xref:fundamentals/middleware/index) to serve [static files](xref:fundamentals/static-files).</span></span>

<span data-ttu-id="ac6cc-165">Ogni componente del middleware è responsabile della chiamata del componente seguente nella pipeline o del corto circuito della catena, se necessario.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-165">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac6cc-166">I servizi aggiuntivi, ad esempio `IWebHostEnvironment`, `ILoggerFactory` o qualsiasi elemento definito in `ConfigureServices`, possono essere specificati nella firma del metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-166">Additional services, such as `IWebHostEnvironment`, `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="ac6cc-167">Questi servizi vengono inseriti se sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-167">These services are injected if they're available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ac6cc-168">I servizi aggiuntivi, ad esempio `IHostingEnvironment` e `ILoggerFactory` o qualsiasi elemento definito in `ConfigureServices`, possono essere specificati nella firma del metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-168">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="ac6cc-169">Questi servizi vengono inseriti se sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-169">These services are injected if they're available.</span></span>

::: moniker-end

<span data-ttu-id="ac6cc-170">Per altre informazioni su come usare `IApplicationBuilder` e sull'ordine di elaborazione del middleware, vedere <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-170">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a><span data-ttu-id="ac6cc-171">Configurare i servizi senza Startup</span><span class="sxs-lookup"><span data-stu-id="ac6cc-171">Configure services without Startup</span></span>

<span data-ttu-id="ac6cc-172">Per configurare i servizi e la pipeline di elaborazione delle richieste senza usare una classe `Startup`, chiamare i metodi pratici `ConfigureServices` e `Configure` sul generatore di host.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-172">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="ac6cc-173">Se vengono effettuate più chiamate a `ConfigureServices`, le chiamate vengono aggiunte l'una all'altra.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-173">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="ac6cc-174">In presenza di più chiamate del metodo `Configure` viene usata l'ultima chiamata di `Configure`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-174">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="ac6cc-175">Estendere l'avvio con filtri di avvio</span><span class="sxs-lookup"><span data-stu-id="ac6cc-175">Extend Startup with startup filters</span></span>

<span data-ttu-id="ac6cc-176">Usare <xref:Microsoft.AspNetCore.Hosting.IStartupFilter>:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-176">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter>:</span></span>

* <span data-ttu-id="ac6cc-177">Per configurare middleware all'inizio o alla fine della pipeline di [configurazione](#the-configure-method) middleware di un'app senza una chiamata esplicita a `Use{Middleware}`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-177">To configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline without an explicit call to `Use{Middleware}`.</span></span> <span data-ttu-id="ac6cc-178">`IStartupFilter` viene usato da ASP.NET Core per aggiungere i valori predefiniti all'inizio della pipeline senza dover fare in modo che l'autore dell'app registri in modo esplicito il middleware predefinito.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-178">`IStartupFilter` is used by ASP.NET Core to add defaults to the beginning of the pipeline without having to make the app author explicitly register the default middleware.</span></span> <span data-ttu-id="ac6cc-179">`IStartupFilter` consente la chiamata di un componente diverso `Use{Middleware}` per conto dell'autore dell'app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-179">`IStartupFilter` allows a different component call `Use{Middleware}` on behalf of the app author.</span></span>
* <span data-ttu-id="ac6cc-180">Per creare una pipeline di `Configure` metodi.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-180">To create a pipeline of `Configure` methods.</span></span> <span data-ttu-id="ac6cc-181">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) può impostare un middleware da eseguire prima o dopo l'aggiunta del middleware dalle librerie.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-181">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) can set a middleware to run before or after middleware added by libraries.</span></span>

<span data-ttu-id="ac6cc-182">`IStartupFilter` implementa <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, che riceve e restituisce un `Action<IApplicationBuilder>`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-182">`IStartupFilter` implements <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="ac6cc-183"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> definisce una classe per configurare la pipeline delle richieste di un'app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-183">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="ac6cc-184">Per altre informazioni, vedere [Creare una pipeline middleware con IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="ac6cc-184">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="ac6cc-185">Ogni `IStartupFilter` può aggiungere uno o più middleware nella pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-185">Each `IStartupFilter` can add one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="ac6cc-186">I filtri vengono richiamati nell'ordine in cui sono stati aggiunti al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-186">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="ac6cc-187">Poiché i filtri possono aggiungere il middleware prima o dopo il passaggio del controllo al filtro successivo, i filtri vengono aggiunti all'inizio o alla fine della pipeline dell'app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-187">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="ac6cc-188">L'esempio seguente dimostra come registrare un middleware con `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-188">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="ac6cc-189">Il middleware `RequestSetOptionsMiddleware` imposta un valore di opzioni da un parametro di stringa di query:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-189">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="ac6cc-190">`RequestSetOptionsMiddleware` è configurato nella classe `RequestSetOptionsStartupFilter`:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-190">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

<span data-ttu-id="ac6cc-191">`RequestSetOptionsMiddleware` è configurato nella classe `RequestSetOptionsStartupFilter`:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-191">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

<span data-ttu-id="ac6cc-192">`IStartupFilter` è registrato nel contenitore di servizi in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-192">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

<span data-ttu-id="ac6cc-193">Quando viene specificato un parametro di stringa di query per `option`, il middleware elabora l'assegnazione del valore prima che il middleware ASP.NET Core esegua il rendering della risposta.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-193">When a query string parameter for `option` is provided, the middleware processes the value assignment before the ASP.NET Core middleware renders the response.</span></span>

<span data-ttu-id="ac6cc-194">L'ordine di esecuzione del middleware viene impostato in base all'ordine delle registrazioni `IStartupFilter`:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-194">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="ac6cc-195">Più implementazioni `IStartupFilter` possono interagire con gli stessi oggetti.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-195">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="ac6cc-196">Se l'ordinamento è un fattore importante, ordinare le registrazioni di servizio `IStartupFilter` in un ordine corrispondente a quello di esecuzione dei relativi middleware.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-196">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="ac6cc-197">Le librerie possono aggiungere middleware con una o più implementazioni `IStartupFilter` che viene eseguito prima o dopo altri middleware dell'app registrati con `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-197">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="ac6cc-198">Per richiamare un middleware `IStartupFilter` prima che un middleware venga aggiunto da `IStartupFilter` di una libreria:</span><span class="sxs-lookup"><span data-stu-id="ac6cc-198">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`:</span></span>

  * <span data-ttu-id="ac6cc-199">Posizionare la registrazione del servizio prima che la libreria venga aggiunta al contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-199">Position the service registration before the library is added to the service container.</span></span>
  * <span data-ttu-id="ac6cc-200">Per richiamarlo in un secondo momento, posizionare la registrazione dei servizi dopo l'aggiunta della libreria.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-200">To invoke afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="ac6cc-201">Aggiungere elementi di configurazione all'avvio da un assembly esterno</span><span class="sxs-lookup"><span data-stu-id="ac6cc-201">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="ac6cc-202">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-202">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="ac6cc-203">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="ac6cc-203">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ac6cc-204">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ac6cc-204">Additional resources</span></span>

* [<span data-ttu-id="ac6cc-205">L'host</span><span class="sxs-lookup"><span data-stu-id="ac6cc-205">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
