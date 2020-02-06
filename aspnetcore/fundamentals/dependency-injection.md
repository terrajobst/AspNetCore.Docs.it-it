---
title: Inserimento delle dipendenze in ASP.NET Core
author: guardrex
description: Informazioni su come ASP.NET Core implementa l'inserimento delle dipendenze e su come usare questa funzionalità.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/05/2020
uid: fundamentals/dependency-injection
ms.openlocfilehash: 7c0789dafcb7dfacd15ac448a39bad94649963c8
ms.sourcegitcommit: bd896935e91236e03241f75e6534ad6debcecbbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/06/2020
ms.locfileid: "77044922"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="b1cf7-103">Inserimento delle dipendenze in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1cf7-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="b1cf7-104">Di [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b1cf7-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b1cf7-105">ASP.NET Core supporta il modello progettuale software per l'inserimento delle dipendenze, ovvero una tecnica per ottenere l'[inversione del controllo (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) tra classi e relative dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="b1cf7-106">Per altre informazioni specifiche per l'inserimento delle dipendenze all'interno di controller MVC, vedere <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="b1cf7-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b1cf7-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="b1cf7-108">Panoramica dell'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="b1cf7-108">Overview of dependency injection</span></span>

<span data-ttu-id="b1cf7-109">Una *dipendenza* è qualsiasi oggetto richiesto da un altro oggetto.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="b1cf7-110">Esaminare la classe `MyDependency` seguente con un metodo `WriteMessage` da cui dipendono altre classi in un'app:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

<span data-ttu-id="b1cf7-111">È possibile creare un'istanza della classe `MyDependency` per rendere il metodo `WriteMessage`disponibile per una classe.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="b1cf7-112">La classe `MyDependency` è una dipendenza della classe `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

<span data-ttu-id="b1cf7-113">La classe crea e dipende direttamente dall'istanza `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="b1cf7-114">Le dipendenze del codice (come nell'esempio precedente) sono problematiche e devono essere evitate per i motivi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="b1cf7-115">Per sostituire `MyDependency` con un'implementazione diversa, la classe deve essere modificata.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="b1cf7-116">Se `MyDependency` ha dipendenze, devono essere configurate dalla classe.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="b1cf7-117">In un progetto di grandi dimensioni con più classi che dipendono da `MyDependency`, il codice di configurazione diventa sparso in tutta l'app.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="b1cf7-118">È difficile eseguire unit test di questa implementazione.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="b1cf7-119">L'app dovrebbe usare una classe `MyDependency` fittizia o stub, ma ciò non è possibile con questo approccio.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="b1cf7-120">L'inserimento delle dipendenze consente di risolvere questi problemi tramite:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="b1cf7-121">L'uso di un'interfaccia o di una classe di base per astrarre l'implementazione delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="b1cf7-122">La registrazione della dipendenza in un contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="b1cf7-123">ASP.NET Core offre il contenitore di servizi predefinito <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="b1cf7-124">I servizi vengono registrati nel metodo `Startup.ConfigureServices` dell'app.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="b1cf7-125">L'*inserimento* del servizio nel costruttore della classe in cui viene usato.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="b1cf7-126">Il framework si assume la responsabilità della creazione di un'istanza della dipendenza e della sua eliminazione quando non è più necessaria.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="b1cf7-127">Nell'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), l'interfaccia `IMyDependency` definisce un metodo che il servizio fornisce all'app:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b1cf7-128">Questa interfaccia viene implementata da un tipo concreto, `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b1cf7-129">`MyDependency` richiede <xref:Microsoft.Extensions.Logging.ILogger`1> nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="b1cf7-130">Non è insolito usare l'inserimento delle dipendenze in modo concatenato.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="b1cf7-131">Ogni dipendenza richiesta richiede a sua volta le proprie dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="b1cf7-132">Il contenitore risolve le dipendenze nel grafico e restituisce il servizio completamente risolto.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="b1cf7-133">Il set di dipendenze che devono essere risolte viene generalmente chiamato *albero delle dipendenze* o *grafico dipendenze* o *grafico degli oggetti*.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="b1cf7-134">`IMyDependency` e `ILogger<TCategoryName>` devono essere registrati nel contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="b1cf7-135">`IMyDependency` viene registrato in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b1cf7-136">`ILogger<TCategoryName>` viene registrato dall'infrastruttura di astrazioni di registrazione, pertanto si tratta di un [servizio fornito dal framework](#framework-provided-services) registrato per impostazione predefinita dal framework.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="b1cf7-137">Il contenitore risolve `ILogger<TCategoryName>` avvalendosi di [tipi aperti (generici)](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminando l'esigenza di registrare ogni singolo [tipo costruito (generico)](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span><span class="sxs-lookup"><span data-stu-id="b1cf7-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="b1cf7-138">Nell'app di esempio, il servizio `IMyDependency` viene registrato con il tipo concreto `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="b1cf7-139">La registrazione definisce come ambito della durata di servizio la durata di una singola richiesta.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="b1cf7-140">Le [durate dei servizi](#service-lifetimes) sono descritte più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="b1cf7-141">Ogni metodo di estensione `services.Add{SERVICE_NAME}` aggiunge (e potenzialmente configura) i servizi.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="b1cf7-142">Ad esempio, `services.AddMvc()` aggiunge i servizi richiesti da Razor Pages e MVC.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="b1cf7-143">È consigliabile che le app seguano questa convenzione.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="b1cf7-144">Inserire i metodi di estensione nello spazio dei nomi [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) per incapsulare i gruppi di registrazioni di servizio.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="b1cf7-145">Se il costruttore del servizio richiede un [tipo incorporato](/dotnet/csharp/language-reference/keywords/built-in-types-table), ad esempio `string`, è possibile inserirlo usando la [configurazione](xref:fundamentals/configuration/index) o il [modello di opzioni](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="b1cf7-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

<span data-ttu-id="b1cf7-146">È richiesta un'istanza del servizio tramite il costruttore di una classe in cui il servizio viene usato e assegnato a un campo privato.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="b1cf7-147">Il campo viene usato per accedere al servizio in base alle esigenze per l'intera classe.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="b1cf7-148">Nell'app di esempio, viene richiesta l'istanza `IMyDependency` usata per chiamare il metodo `WriteMessage` del servizio:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

## <a name="services-injected-into-startup"></a><span data-ttu-id="b1cf7-149">Servizi inseriti all'avvio</span><span class="sxs-lookup"><span data-stu-id="b1cf7-149">Services injected into Startup</span></span>

<span data-ttu-id="b1cf7-150">Quando si usa l'host generico (<xref:Microsoft.Extensions.Hosting.IHostBuilder>), è possibile inserire solo i tipi di servizio seguenti nel costruttore `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-150">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="b1cf7-151">I servizi possono essere inseriti in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-151">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="b1cf7-152">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-152">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="b1cf7-153">Servizi forniti dal framework</span><span class="sxs-lookup"><span data-stu-id="b1cf7-153">Framework-provided services</span></span>

<span data-ttu-id="b1cf7-154">Il metodo `Startup.ConfigureServices` è responsabile della definizione dei servizi utilizzati dall'app, incluse le funzionalità della piattaforma, ad esempio Entity Framework Core e ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-154">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="b1cf7-155">Inizialmente, la `IServiceCollection` fornita ai `ConfigureServices` dispone di servizi definiti dal Framework a seconda della [configurazione dell'host](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-155">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="b1cf7-156">Non è insolito che un'app basata su un modello di ASP.NET Core disponga di centinaia di servizi registrati dal Framework.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-156">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="b1cf7-157">Nella tabella seguente è elencato un piccolo esempio di servizi registrati dal Framework.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-157">A small sample of framework-registered services is listed in the following table.</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="b1cf7-158">Tipo di servizio</span><span class="sxs-lookup"><span data-stu-id="b1cf7-158">Service Type</span></span> | <span data-ttu-id="b1cf7-159">Durata</span><span class="sxs-lookup"><span data-stu-id="b1cf7-159">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="b1cf7-160">Temporaneo</span><span class="sxs-lookup"><span data-stu-id="b1cf7-160">Transient</span></span> |
| `IHostApplicationLifetime` | <span data-ttu-id="b1cf7-161">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-161">Singleton</span></span> |
| `IWebHostEnvironment` | <span data-ttu-id="b1cf7-162">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-162">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="b1cf7-163">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-163">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="b1cf7-164">Temporaneo</span><span class="sxs-lookup"><span data-stu-id="b1cf7-164">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="b1cf7-165">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-165">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="b1cf7-166">Temporaneo</span><span class="sxs-lookup"><span data-stu-id="b1cf7-166">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="b1cf7-167">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-167">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="b1cf7-168">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-168">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="b1cf7-169">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-169">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="b1cf7-170">Temporaneo</span><span class="sxs-lookup"><span data-stu-id="b1cf7-170">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="b1cf7-171">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-171">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="b1cf7-172">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-172">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="b1cf7-173">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-173">Singleton</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="b1cf7-174">Tipo di servizio</span><span class="sxs-lookup"><span data-stu-id="b1cf7-174">Service Type</span></span> | <span data-ttu-id="b1cf7-175">Durata</span><span class="sxs-lookup"><span data-stu-id="b1cf7-175">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="b1cf7-176">Temporaneo</span><span class="sxs-lookup"><span data-stu-id="b1cf7-176">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="b1cf7-177">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-177">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="b1cf7-178">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-178">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="b1cf7-179">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-179">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="b1cf7-180">Temporaneo</span><span class="sxs-lookup"><span data-stu-id="b1cf7-180">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="b1cf7-181">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-181">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="b1cf7-182">Temporaneo</span><span class="sxs-lookup"><span data-stu-id="b1cf7-182">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="b1cf7-183">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-183">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="b1cf7-184">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-184">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="b1cf7-185">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-185">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="b1cf7-186">Temporaneo</span><span class="sxs-lookup"><span data-stu-id="b1cf7-186">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="b1cf7-187">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-187">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="b1cf7-188">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-188">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="b1cf7-189">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-189">Singleton</span></span> |

::: moniker-end

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="b1cf7-190">Registrare servizi aggiuntivi con i metodi di estensione</span><span class="sxs-lookup"><span data-stu-id="b1cf7-190">Register additional services with extension methods</span></span>

<span data-ttu-id="b1cf7-191">Quando è disponibile un metodo di estensione della raccolta di servizi per la registrazione di un servizio (e dei servizi dipendenti, se necessario), la convenzione consiste nell'usare un singolo metodo di estensione `Add{SERVICE_NAME}` per registrare tutti i servizi richiesti da tale servizio.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-191">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="b1cf7-192">Il codice seguente è un esempio di come aggiungere altri servizi al contenitore usando i metodi di estensione [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) e <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-192">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    ...
}
```

<span data-ttu-id="b1cf7-193">Per altre informazioni, vedere la classe <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> nella documentazione delle API.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-193">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="b1cf7-194">Durate dei servizi</span><span class="sxs-lookup"><span data-stu-id="b1cf7-194">Service lifetimes</span></span>

<span data-ttu-id="b1cf7-195">Scegliere una durata appropriata per ogni servizio registrato.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-195">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="b1cf7-196">I servizi ASP.NET Core possono essere configurati con le impostazioni di durata seguenti:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-196">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="b1cf7-197">Temporaneo</span><span class="sxs-lookup"><span data-stu-id="b1cf7-197">Transient</span></span>

<span data-ttu-id="b1cf7-198">I servizi con durata temporanea (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) vengono creati ogni volta che vengono richiesti dal contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-198">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="b1cf7-199">Questa impostazione di durata è consigliata per i servizi semplici senza stati.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-199">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="b1cf7-200">Con ambito</span><span class="sxs-lookup"><span data-stu-id="b1cf7-200">Scoped</span></span>

<span data-ttu-id="b1cf7-201">I servizi con durata con ambito (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) vengono creati una volta per ogni richiesta client (connessione).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-201">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="b1cf7-202">Quando si usa un servizio con ambito in un middleware, inserire il servizio nel metodo `Invoke` o `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-202">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="b1cf7-203">Evitare l'inserimento tramite il costruttore, che impone al servizio il comportamento singleton.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-203">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="b1cf7-204">Per altre informazioni, vedere <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-204">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="b1cf7-205">Singleton</span><span class="sxs-lookup"><span data-stu-id="b1cf7-205">Singleton</span></span>

<span data-ttu-id="b1cf7-206">I servizi con durata singleton (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) vengono creati la prima volta che vengono richiesti (o quando si esegue `Startup.ConfigureServices` e viene specificata un'istanza con la registrazione del servizio).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-206">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="b1cf7-207">Tutte le richieste successive usano la stessa istanza.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-207">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="b1cf7-208">Se l'app richiede un comportamento singleton, è consigliabile consentire al contenitore del servizio di gestire la durata del servizio.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-208">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="b1cf7-209">Non implementare lo schema progettuale singleton e fornire codice utente per gestire la durata dell'oggetto nella classe.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-209">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="b1cf7-210">Può essere pericoloso risolvere un servizio con ambito da un singleton.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-210">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="b1cf7-211">Ciò potrebbe causare uno stato non corretto per il servizio durante l'elaborazione delle richieste successive.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-211">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="b1cf7-212">Metodi di registrazione del servizio</span><span class="sxs-lookup"><span data-stu-id="b1cf7-212">Service registration methods</span></span>

<span data-ttu-id="b1cf7-213">I metodi di estensione per la registrazione del servizio offrono overload che risultano utili in scenari specifici.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-213">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="b1cf7-214">Metodo</span><span class="sxs-lookup"><span data-stu-id="b1cf7-214">Method</span></span> | <span data-ttu-id="b1cf7-215">Automatico</span><span class="sxs-lookup"><span data-stu-id="b1cf7-215">Automatic</span></span><br><span data-ttu-id="b1cf7-216">object</span><span class="sxs-lookup"><span data-stu-id="b1cf7-216">object</span></span><br><span data-ttu-id="b1cf7-217">eliminazione</span><span class="sxs-lookup"><span data-stu-id="b1cf7-217">disposal</span></span> | <span data-ttu-id="b1cf7-218">Selezione multipla</span><span class="sxs-lookup"><span data-stu-id="b1cf7-218">Multiple</span></span><br><span data-ttu-id="b1cf7-219">implementazioni</span><span class="sxs-lookup"><span data-stu-id="b1cf7-219">implementations</span></span> | <span data-ttu-id="b1cf7-220">Pass args</span><span class="sxs-lookup"><span data-stu-id="b1cf7-220">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="b1cf7-221">Esempio:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-221">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="b1cf7-222">Sì</span><span class="sxs-lookup"><span data-stu-id="b1cf7-222">Yes</span></span> | <span data-ttu-id="b1cf7-223">Sì</span><span class="sxs-lookup"><span data-stu-id="b1cf7-223">Yes</span></span> | <span data-ttu-id="b1cf7-224">No</span><span class="sxs-lookup"><span data-stu-id="b1cf7-224">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="b1cf7-225">Esempi:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-225">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="b1cf7-226">Sì</span><span class="sxs-lookup"><span data-stu-id="b1cf7-226">Yes</span></span> | <span data-ttu-id="b1cf7-227">Sì</span><span class="sxs-lookup"><span data-stu-id="b1cf7-227">Yes</span></span> | <span data-ttu-id="b1cf7-228">Sì</span><span class="sxs-lookup"><span data-stu-id="b1cf7-228">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="b1cf7-229">Esempio:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-229">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="b1cf7-230">Sì</span><span class="sxs-lookup"><span data-stu-id="b1cf7-230">Yes</span></span> | <span data-ttu-id="b1cf7-231">No</span><span class="sxs-lookup"><span data-stu-id="b1cf7-231">No</span></span> | <span data-ttu-id="b1cf7-232">No</span><span class="sxs-lookup"><span data-stu-id="b1cf7-232">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="b1cf7-233">Esempi:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-233">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="b1cf7-234">No</span><span class="sxs-lookup"><span data-stu-id="b1cf7-234">No</span></span> | <span data-ttu-id="b1cf7-235">Sì</span><span class="sxs-lookup"><span data-stu-id="b1cf7-235">Yes</span></span> | <span data-ttu-id="b1cf7-236">Sì</span><span class="sxs-lookup"><span data-stu-id="b1cf7-236">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="b1cf7-237">Esempi:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-237">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="b1cf7-238">No</span><span class="sxs-lookup"><span data-stu-id="b1cf7-238">No</span></span> | <span data-ttu-id="b1cf7-239">No</span><span class="sxs-lookup"><span data-stu-id="b1cf7-239">No</span></span> | <span data-ttu-id="b1cf7-240">Sì</span><span class="sxs-lookup"><span data-stu-id="b1cf7-240">Yes</span></span> |

<span data-ttu-id="b1cf7-241">Per ulteriori informazioni sull'eliminazione dei tipi, vedere la sezione [Eliminazione dei servizi](#disposal-of-services).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-241">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="b1cf7-242">Uno scenario comune per più implementazioni è costituito dai [tipi di simulazioni per i test](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-242">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="b1cf7-243">I metodi `TryAdd{LIFETIME}` registrano il servizio solo se esiste già un'implementazione registrata.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-243">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="b1cf7-244">Nell'esempio seguente, la prima riga registra `MyDependency` per `IMyDependency`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-244">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="b1cf7-245">La seconda riga non ha alcun effetto perché `IMyDependency` dispone già di un'implementazione registrata:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-245">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="b1cf7-246">Per altre informazioni, vedere:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-246">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="b1cf7-247">I metodi [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) registrano il servizio solo se non esiste già un'implementazione *dello stesso tipo*.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-247">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="b1cf7-248">Più servizi vengono risolti tramite `IEnumerable<{SERVICE}>`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-248">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="b1cf7-249">Durante la registrazione di servizi, lo sviluppatore desidera aggiungere un'istanza solo se non ne è già stata aggiunta una dello stesso tipo.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-249">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="b1cf7-250">In genere, questo metodo viene utilizzato dagli autori di librerie per evitare di registrare due copie di un'istanza nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-250">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="b1cf7-251">Nell'esempio seguente, la prima riga registra `MyDep` per `IMyDep1`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-251">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="b1cf7-252">La seconda riga registra `MyDep` per `IMyDep2`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-252">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="b1cf7-253">La seconda riga non ha alcun effetto perché `IMyDep1` dispone già di un'implementazione registrata di `MyDep`:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-253">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="b1cf7-254">Comportamento dell'inserimento del costruttore</span><span class="sxs-lookup"><span data-stu-id="b1cf7-254">Constructor injection behavior</span></span>

<span data-ttu-id="b1cf7-255">I servizi possono essere risolti con due meccanismi:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-255">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="b1cf7-256"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; consente la creazione di oggetti senza registrazione del servizio nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-256"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="b1cf7-257">`ActivatorUtilities` viene usato con astrazioni esposte all'utente, ad esempio helper tag, controller MVC e strumenti di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-257">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="b1cf7-258">I costruttori possono accettare argomenti non forniti tramite l'inserimento di dipendenze, ma gli argomenti devono assegnare valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-258">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="b1cf7-259">Quando i servizi vengono risolti da `IServiceProvider` o `ActivatorUtilities`, l'inserimento del costruttore richiede un costruttore *pubblico*.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-259">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="b1cf7-260">Quando i servizi vengono risolti da `ActivatorUtilities`, l'inserimento del costruttore richiede l'esistenza di un solo costruttore applicabile.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-260">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="b1cf7-261">Sebbene siano supportati gli overload dei costruttori, può essere presente un solo overload i cui argomenti possono essere tutti specificati tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-261">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="b1cf7-262">Contesti di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b1cf7-262">Entity Framework contexts</span></span>

<span data-ttu-id="b1cf7-263">I contesti di Entity Framework vengono in genere aggiunti al contenitore di servizi usando la [durata con ambito](#service-lifetimes) perché le operazioni di database delle app Web hanno di solito come ambito la richiesta client.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-263">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="b1cf7-264">La durata predefinita è con ambito se non viene specificata una durata con un overload [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) quando si registra il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-264">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="b1cf7-265">I servizi con una durata specificata non devono usare un contesto di database con una durata più breve rispetto al servizio.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-265">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="b1cf7-266">Opzioni di durata e di registrazione</span><span class="sxs-lookup"><span data-stu-id="b1cf7-266">Lifetime and registration options</span></span>

<span data-ttu-id="b1cf7-267">Per illustrare la differenza tra le opzioni di durata e registrazione, si considerino le interfacce seguenti che rappresentano le attività come un'operazione con un identificatore univoco, `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-267">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="b1cf7-268">A seconda del modo in cui la durata di un servizio operativo viene configurata per le interfacce seguenti, il contenitore fornisce la stessa istanza o un'istanza diversa del servizio quando richiesto da una classe:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-268">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b1cf7-269">Le interfacce vengono implementate nella classe `Operation`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-269">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="b1cf7-270">Il costruttore `Operation` genera un GUID se non ne viene fornito uno:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-270">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b1cf7-271">Viene registrato un `OperationService` che dipende da ognuno degli altri tipi `Operation`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-271">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="b1cf7-272">Quando viene richiesto `OperationService` tramite l'inserimento delle dipendenze, riceve una nuova istanza di ogni servizio o un'istanza esistente in base alla durata del servizio dipendente.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-272">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="b1cf7-273">Se i servizi temporanei vengono creati quando vengono richiesti dal contenitore, `OperationId` del servizio `IOperationTransient` è diverso da `OperationId` di `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-273">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="b1cf7-274">`OperationService` riceve una nuova istanza della classe `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-274">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="b1cf7-275">La nuova istanza restituisce un `OperationId` diverso.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-275">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="b1cf7-276">Se i servizi con ambito vengono creati per ogni richiesta client, `OperationId` del servizio `IOperationScoped` corrisponde a `OperationService` all'interno di una richiesta client.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-276">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="b1cf7-277">In tutte le richieste client entrambi i servizi condividono un valore `OperationId` diverso.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-277">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="b1cf7-278">Se i servizi singleton e con istanza singleton vengono creati una sola volta e usati per tutte le richieste client e tutti i servizi, `OperationId` rimane costante in tutte le richieste di servizio.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-278">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="b1cf7-279">In `Startup.ConfigureServices`, ogni tipo viene aggiunto al contenitore in base alla durata denominata:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-279">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="b1cf7-280">Il servizio `IOperationSingletonInstance` usa un'istanza specifica con un ID noto di `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-280">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="b1cf7-281">È chiaro quando questo tipo è in uso (il relativo GUID è composto da tutti zero).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-281">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="b1cf7-282">L'app di esempio dimostra le durate degli oggetti all'interno e tra le singole richieste.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-282">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="b1cf7-283">L'`IndexModel` dell'app di esempio richiede ogni genere di tipo `IOperation` e `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-283">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="b1cf7-284">La pagina visualizza quindi tutti i valori `OperationId` della classe del modello di pagina e del servizio tramite assegnazioni di proprietà:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-284">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

<span data-ttu-id="b1cf7-285">L'output seguente mostra i risultati delle due richieste:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-285">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="b1cf7-286">**Prima richiesta:**</span><span class="sxs-lookup"><span data-stu-id="b1cf7-286">**First request:**</span></span>

<span data-ttu-id="b1cf7-287">Operazioni del controller:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-287">Controller operations:</span></span>

<span data-ttu-id="b1cf7-288">Transient: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="b1cf7-288">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="b1cf7-289">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="b1cf7-289">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="b1cf7-290">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="b1cf7-290">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="b1cf7-291">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="b1cf7-291">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="b1cf7-292">Operazioni di `OperationService`:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-292">`OperationService` operations:</span></span>

<span data-ttu-id="b1cf7-293">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="b1cf7-293">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="b1cf7-294">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="b1cf7-294">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="b1cf7-295">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="b1cf7-295">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="b1cf7-296">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="b1cf7-296">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="b1cf7-297">**Seconda richiesta:**</span><span class="sxs-lookup"><span data-stu-id="b1cf7-297">**Second request:**</span></span>

<span data-ttu-id="b1cf7-298">Operazioni del controller:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-298">Controller operations:</span></span>

<span data-ttu-id="b1cf7-299">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="b1cf7-299">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="b1cf7-300">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="b1cf7-300">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="b1cf7-301">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="b1cf7-301">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="b1cf7-302">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="b1cf7-302">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="b1cf7-303">Operazioni di `OperationService`:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-303">`OperationService` operations:</span></span>

<span data-ttu-id="b1cf7-304">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="b1cf7-304">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="b1cf7-305">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="b1cf7-305">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="b1cf7-306">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="b1cf7-306">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="b1cf7-307">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="b1cf7-307">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="b1cf7-308">Osservare i valori `OperationId` che variano all'interno della richiesta e da una richiesta e l'altra:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-308">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="b1cf7-309">Gli oggetti *temporanei* sono sempre diversi.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-309">*Transient* objects are always different.</span></span> <span data-ttu-id="b1cf7-310">Il valore `OperationId` temporaneo per la prima e la seconda richiesta client è diverso per entrambe le operazioni `OperationService` e in tutte le richieste client.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-310">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="b1cf7-311">Viene fornita una nuova istanza per ogni richiesta di servizio e richiesta client.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-311">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="b1cf7-312">Gli oggetti *con ambito* sono gli stessi all'interno di una richiesta client, ma variano nelle diverse richieste client.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-312">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="b1cf7-313">Gli oggetti *singleton* sono gli stessi per ogni oggetto e ogni richiesta, indipendentemente dal fatto che venga fornita un'istanza `Operation` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-313">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="b1cf7-314">Chiamare i servizi da main</span><span class="sxs-lookup"><span data-stu-id="b1cf7-314">Call services from main</span></span>

<span data-ttu-id="b1cf7-315">Creare un <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>IServiceScope con [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) per risolvere un servizio con ambito nell'ambito di applicazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-315">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="b1cf7-316">Questo approccio è utile per l'accesso a un servizio con ambito all'avvio e per l'esecuzione di attività di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-316">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="b1cf7-317">L'esempio seguente indica come ottenere un contesto per `MyScopedService` in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-317">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateHostBuilder(args).Build();

        using (var serviceScope = host.Services.CreateScope())
        {
            var services = serviceScope.ServiceProvider;

            try
            {
                var serviceContext = services.GetRequiredService<MyScopedService>();
                // Use the context here
            }
            catch (Exception ex)
            {
                var logger = services.GetRequiredService<ILogger<Program>>();
                logger.LogError(ex, "An error occurred.");
            }
        }
    
        await host.RunAsync();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateWebHostBuilder(args).Build();

        using (var serviceScope = host.Services.CreateScope())
        {
            var services = serviceScope.ServiceProvider;

            try
            {
                var serviceContext = services.GetRequiredService<MyScopedService>();
                // Use the context here
            }
            catch (Exception ex)
            {
                var logger = services.GetRequiredService<ILogger<Program>>();
                logger.LogError(ex, "An error occurred.");
            }
        }
    
        await host.RunAsync();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

::: moniker-end

## <a name="scope-validation"></a><span data-ttu-id="b1cf7-318">Convalida dell'ambito</span><span class="sxs-lookup"><span data-stu-id="b1cf7-318">Scope validation</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b1cf7-319">Quando l'app è in esecuzione nell'ambiente di sviluppo e chiama [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) per compilare l'host, il provider di servizi predefinito esegue i controlli per verificare che:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-319">When the app is running in the Development environment and calls [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) to build the host, the default service provider performs checks to verify that:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b1cf7-320">Quando l'app è in esecuzione nell'ambiente di sviluppo e chiama [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) per compilare l'host, il provider di servizi predefinito esegue i controlli per verificare che:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-320">When the app is running in the Development environment and calls [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) to build the host, the default service provider performs checks to verify that:</span></span>

::: moniker-end

* <span data-ttu-id="b1cf7-321">I servizi con ambito non vengano risolti direttamente o indirettamente dal provider di servizi radice.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-321">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="b1cf7-322">I servizi con ambito non vengano inseriti direttamente o indirettamente in singleton.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-322">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="b1cf7-323">Il provider di servizi radice venga creato con la chiamata di <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*>.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-323">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="b1cf7-324">La durata del provider di servizi radice corrisponde alla durata dell'app/server quando il provider viene avviato con l'app e viene eliminato alla chiusura dell'app.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-324">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="b1cf7-325">I servizi con ambito vengono eliminati dal contenitore che li ha creati.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-325">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="b1cf7-326">Se un servizio con ambito viene creato nel contenitore radice, la durata del servizio viene di fatto convertita in singleton, perché il servizio viene eliminato solo dal contenitore radice alla chiusura dell'app/server.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-326">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="b1cf7-327">La convalida degli ambiti servizio rileva queste situazioni nel contesto della chiamata di `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-327">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="b1cf7-328">Per altre informazioni, vedere <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-328">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="b1cf7-329">Servizi di richiesta</span><span class="sxs-lookup"><span data-stu-id="b1cf7-329">Request Services</span></span>

<span data-ttu-id="b1cf7-330">I servizi disponibili all'interno di una richiesta ASP.NET Core da `HttpContext` vengono esposti tramite la raccolta [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-330">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="b1cf7-331">I servizi di richiesta rappresentano i servizi configurati e richiesti come parte dell'app.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-331">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="b1cf7-332">Quando gli oggetti specificano dipendenze, le dipendenze sono soddisfatte dai tipi individuati in `RequestServices`, non in `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-332">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="b1cf7-333">In generale, l'app non deve usare queste proprietà direttamente.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-333">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="b1cf7-334">Richiedere invece i tipi richiesti dalle classi tramite i costruttori di classi e consentire al framework di inserire le dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-334">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="b1cf7-335">Si ottengono classi più facili da testare.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-335">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="b1cf7-336">È consigliabile richiedere le dipendenze come parametri del costruttore per l'accesso alla raccolta `RequestServices`.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-336">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="b1cf7-337">Progettare i servizi per l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="b1cf7-337">Design services for dependency injection</span></span>

<span data-ttu-id="b1cf7-338">Le procedure consigliate prevedono di:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-338">Best practices are to:</span></span>

* <span data-ttu-id="b1cf7-339">Progettare i servizi per usare l'inserimento delle dipendenze per ottenere le relative dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-339">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="b1cf7-340">Evitare membri e classi statiche con stato.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-340">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="b1cf7-341">Progettare le app in modo da usare i servizi singleton, evitando così di creare uno stato globale.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-341">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="b1cf7-342">Evitare la creazione diretta di istanze delle classi dipendenti all'interno di servizi.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-342">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="b1cf7-343">La creazione diretta di istanze associa il codice a una determinata implementazione.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-343">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="b1cf7-344">Fare in modo che le classi siano piccole, con factoring corretto e facili da testare.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-344">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="b1cf7-345">Se una classe sembra avere troppe dipendenze inserite, ciò significa in genere che la classe ha un numero eccessivo di responsabilità e sta violando il [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) (principio di responsabilità singola).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-345">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="b1cf7-346">Tentare di effettuare il refactoring della classe spostando alcune delle responsabilità in una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-346">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="b1cf7-347">Tenere presente che le classi del modello di pagina Razor Pages e le classi del controller MVC devono essere incentrate sulle problematiche dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-347">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="b1cf7-348">Di conseguenza, le regole business e i dettagli di implementazione di accesso ai dati devono essere inseriti in classi appropriate per queste [problematiche separate](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-348">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="b1cf7-349">Eliminazione dei servizi</span><span class="sxs-lookup"><span data-stu-id="b1cf7-349">Disposal of services</span></span>

<span data-ttu-id="b1cf7-350">Il contenitore chiama <xref:System.IDisposable.Dispose*> per i tipi <xref:System.IDisposable> creati.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-350">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="b1cf7-351">Se un'istanza viene aggiunta al contenitore dal codice utente, non viene eliminata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-351">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

## <a name="default-service-container-replacement"></a><span data-ttu-id="b1cf7-352">Sostituzione del contenitore di servizi predefinito</span><span class="sxs-lookup"><span data-stu-id="b1cf7-352">Default service container replacement</span></span>

<span data-ttu-id="b1cf7-353">Il contenitore dei servizi incorporato è progettato per soddisfare le esigenze del Framework e della maggior parte delle app consumer.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-353">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="b1cf7-354">Si consiglia di usare il contenitore predefinito a meno che non sia necessaria una funzionalità specifica che il contenitore incorporato non supporta, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-354">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="b1cf7-355">Inserimento di proprietà</span><span class="sxs-lookup"><span data-stu-id="b1cf7-355">Property injection</span></span>
* <span data-ttu-id="b1cf7-356">Inserimento in base al nome</span><span class="sxs-lookup"><span data-stu-id="b1cf7-356">Injection based on name</span></span>
* <span data-ttu-id="b1cf7-357">Contenitori figlio</span><span class="sxs-lookup"><span data-stu-id="b1cf7-357">Child containers</span></span>
* <span data-ttu-id="b1cf7-358">Gestione della durata personalizzata</span><span class="sxs-lookup"><span data-stu-id="b1cf7-358">Custom lifetime management</span></span>
* <span data-ttu-id="b1cf7-359">Supporto di `Func<T>` per l'inizializzazione differita</span><span class="sxs-lookup"><span data-stu-id="b1cf7-359">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="b1cf7-360">Registrazione basata sulle convenzioni</span><span class="sxs-lookup"><span data-stu-id="b1cf7-360">Convention-based registration</span></span>

<span data-ttu-id="b1cf7-361">Con ASP.NET Core app è possibile usare i contenitori di terze parti seguenti:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-361">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="b1cf7-362">Autofac</span><span class="sxs-lookup"><span data-stu-id="b1cf7-362">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="b1cf7-363">DryIoc</span><span class="sxs-lookup"><span data-stu-id="b1cf7-363">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="b1cf7-364">Grazia</span><span class="sxs-lookup"><span data-stu-id="b1cf7-364">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="b1cf7-365">LightInject</span><span class="sxs-lookup"><span data-stu-id="b1cf7-365">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="b1cf7-366">Lamar</span><span class="sxs-lookup"><span data-stu-id="b1cf7-366">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="b1cf7-367">Stashbox</span><span class="sxs-lookup"><span data-stu-id="b1cf7-367">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="b1cf7-368">Unity</span><span class="sxs-lookup"><span data-stu-id="b1cf7-368">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="b1cf7-369">Thread safety</span><span class="sxs-lookup"><span data-stu-id="b1cf7-369">Thread safety</span></span>

<span data-ttu-id="b1cf7-370">Creare servizi singleton thread-safe.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-370">Create thread-safe singleton services.</span></span> <span data-ttu-id="b1cf7-371">Se un servizio singleton ha una dipendenza in un servizio temporaneo, è necessario che anche il servizio temporaneo sia thread-safe, a seconda di come viene usato dal singleton.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-371">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="b1cf7-372">Non è necessario che il metodo factory del singolo servizio, ad esempio il secondo argomento per [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), sia thread-safe.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-372">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="b1cf7-373">Come nel caso di un costruttore di tipo (`static`), è garantito che venga chiamato una sola volta da un singolo thread.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-373">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="b1cf7-374">Consigli</span><span class="sxs-lookup"><span data-stu-id="b1cf7-374">Recommendations</span></span>

* <span data-ttu-id="b1cf7-375">La risoluzione basata sui servizi `async/await` e `Task` non è supportata.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-375">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="b1cf7-376">Dato che C# non supporta i costruttori asincroni, il modello consigliato consiste nell'usare i metodi asincroni dopo avere risolto in modo sincrono il servizio.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-376">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="b1cf7-377">Evitare di archiviare i dati e la configurazione direttamente nel contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-377">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="b1cf7-378">Ad esempio, il carrello acquisti di un utente non dovrebbe in genere essere aggiunto al contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-378">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="b1cf7-379">La configurazione deve usare il [modello di opzioni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-379">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="b1cf7-380">Analogamente, evitare gli oggetti "contenitori di dati" che hanno la sola funzione di consentire l'accesso ad altri oggetti.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-380">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="b1cf7-381">È preferibile richiedere l'elemento effettivo tramite inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-381">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="b1cf7-382">Evitare l'accesso statico ai servizi (ad esempio, la tipizzazione statica [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) da usare in altre posizioni).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-382">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="b1cf7-383">Evitare di usare il *modello di localizzatore del servizio*.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-383">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="b1cf7-384">Ad esempio, non richiamare <xref:System.IServiceProvider.GetService*> per ottenere un'istanza del servizio quando è invece possibile usare l'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-384">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="b1cf7-385">**Non corretto:**</span><span class="sxs-lookup"><span data-stu-id="b1cf7-385">**Incorrect:**</span></span>

  ```csharp
  public class MyClass()
  {
      public void MyMethod()
      {
          var optionsMonitor = 
              _services.GetService<IOptionsMonitor<MyOptions>>();
          var option = optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

  <span data-ttu-id="b1cf7-386">**Corretti**:</span><span class="sxs-lookup"><span data-stu-id="b1cf7-386">**Correct**:</span></span>

  ```csharp
  public class MyClass
  {
      private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

      public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
      {
          _optionsMonitor = optionsMonitor;
      }

      public void MyMethod()
      {
          var option = _optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

* <span data-ttu-id="b1cf7-387">Un'altra variazione del localizzatore del servizio da evitare è inserire una factory che risolve le dipendenze in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-387">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="b1cf7-388">Queste procedure combinano le strategie di [inversione del controllo](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-388">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="b1cf7-389">Evitare l'accesso statico a `HttpContext` (ad esempio [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="b1cf7-389">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="b1cf7-390">È tuttavia possibile che in alcuni casi queste raccomandazioni debbano essere ignorate.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-390">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="b1cf7-391">Le eccezioni sono rare e principalmente si tratta di casi speciali all'interno del framework stesso.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-391">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="b1cf7-392">L'inserimento di dipendenze è un'*alternativa* ai modelli di accesso agli oggetti statici/globali.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-392">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="b1cf7-393">Se l'inserimento di dipendenze viene usato con l'accesso agli oggetti statico i vantaggi non saranno evidenti.</span><span class="sxs-lookup"><span data-stu-id="b1cf7-393">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1cf7-394">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b1cf7-394">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="b1cf7-395">Scrittura di codice pulito in ASP.NET Core con inserimento delle dipendenze (MSDN)</span><span class="sxs-lookup"><span data-stu-id="b1cf7-395">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* <span data-ttu-id="b1cf7-396">[Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) (Principio delle dipendenze esplicite)</span><span class="sxs-lookup"><span data-stu-id="b1cf7-396">[Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)</span></span>
* <span data-ttu-id="b1cf7-397">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Martin Fowler) (Contenitori di inversione del controllo e modello di inserimento delle dipendenze)</span><span class="sxs-lookup"><span data-stu-id="b1cf7-397">[Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)</span></span>
* <span data-ttu-id="b1cf7-398">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (Come registrare un servizio con più interfacce in ASP.NET Core DI)</span><span class="sxs-lookup"><span data-stu-id="b1cf7-398">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)</span></span>
