---
title: ASP.NET Core Blazor inserimento delle dipendenze
author: guardrex
description: Scopri in che modo le app Blazor possono inserire i servizi nei componenti.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 4cdde9ee8c9fd9adf00894a067d32965b180e5ec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658074"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="c0754-103">Inserimento delle dipendenze di ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="c0754-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="c0754-104">Di [Rainer Stropek](https://www.timecockpit.com) e [Mike entusiasmanti](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="c0754-104">By [Rainer Stropek](https://www.timecockpit.com) and [Mike Rousos](https://github.com/mjrousos)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="c0754-105">Blazer supporta l' [inserimento delle dipendenze (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c0754-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c0754-106">Le app possono usare i servizi predefiniti inserendoli in componenti.</span><span class="sxs-lookup"><span data-stu-id="c0754-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="c0754-107">Le app possono anche definire e registrare servizi personalizzati e renderli disponibili nell'app tramite DI.</span><span class="sxs-lookup"><span data-stu-id="c0754-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="c0754-108">DI è una tecnica per accedere ai servizi configurati in una posizione centrale.</span><span class="sxs-lookup"><span data-stu-id="c0754-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="c0754-109">Questa operazione può essere utile nelle app Blazor per:</span><span class="sxs-lookup"><span data-stu-id="c0754-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="c0754-110">Condividere una singola istanza di una classe di servizio in molti componenti, noti come un servizio *singleton* .</span><span class="sxs-lookup"><span data-stu-id="c0754-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="c0754-111">Separare i componenti da classi di servizi concrete usando astrazioni di riferimento.</span><span class="sxs-lookup"><span data-stu-id="c0754-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="c0754-112">Si consideri, ad esempio, un'interfaccia `IDataAccess` per l'accesso ai dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="c0754-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="c0754-113">L'interfaccia viene implementata da una classe di `DataAccess` concreta e registrata come servizio nel contenitore del servizio dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0754-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="c0754-114">Quando un componente usa il per ricevere un'implementazione di `IDataAccess`, il componente non è associato al tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="c0754-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="c0754-115">L'implementazione può essere scambiata, ad esempio per un'implementazione fittizia negli unit test.</span><span class="sxs-lookup"><span data-stu-id="c0754-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="c0754-116">Servizi predefiniti</span><span class="sxs-lookup"><span data-stu-id="c0754-116">Default services</span></span>

<span data-ttu-id="c0754-117">I servizi predefiniti vengono aggiunti automaticamente alla raccolta di servizi dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0754-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="c0754-118">Service</span><span class="sxs-lookup"><span data-stu-id="c0754-118">Service</span></span> | <span data-ttu-id="c0754-119">Durata</span><span class="sxs-lookup"><span data-stu-id="c0754-119">Lifetime</span></span> | <span data-ttu-id="c0754-120">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c0754-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="c0754-121">Singleton</span><span class="sxs-lookup"><span data-stu-id="c0754-121">Singleton</span></span> | <span data-ttu-id="c0754-122">Fornisce metodi per l'invio di richieste HTTP e la ricezione di risposte HTTP da una risorsa identificata da un URI.</span><span class="sxs-lookup"><span data-stu-id="c0754-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="c0754-123">L'istanza di `HttpClient` in un'app webassembly Blazer usa il browser per gestire il traffico HTTP in background.</span><span class="sxs-lookup"><span data-stu-id="c0754-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="c0754-124">Per impostazione predefinita, le app del server Blazer non includono un `HttpClient` configurato come servizio.</span><span class="sxs-lookup"><span data-stu-id="c0754-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="c0754-125">Fornire un `HttpClient` a un'app del server blazer.</span><span class="sxs-lookup"><span data-stu-id="c0754-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="c0754-126">Per altre informazioni, vedere <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="c0754-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="c0754-127">Singleton (webassembly Blazer)</span><span class="sxs-lookup"><span data-stu-id="c0754-127">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="c0754-128">Con ambito (server Blazer)</span><span class="sxs-lookup"><span data-stu-id="c0754-128">Scoped (Blazor Server)</span></span> | <span data-ttu-id="c0754-129">Rappresenta un'istanza di un runtime JavaScript in cui vengono inviate le chiamate a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c0754-129">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="c0754-130">Per altre informazioni, vedere <xref:blazor/call-javascript-from-dotnet>.</span><span class="sxs-lookup"><span data-stu-id="c0754-130">For more information, see <xref:blazor/call-javascript-from-dotnet>.</span></span> |
| `NavigationManager` | <span data-ttu-id="c0754-131">Singleton (webassembly Blazer)</span><span class="sxs-lookup"><span data-stu-id="c0754-131">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="c0754-132">Con ambito (server Blazer)</span><span class="sxs-lookup"><span data-stu-id="c0754-132">Scoped (Blazor Server)</span></span> | <span data-ttu-id="c0754-133">Contiene gli helper per lavorare con gli URI e lo stato di navigazione.</span><span class="sxs-lookup"><span data-stu-id="c0754-133">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="c0754-134">Per ulteriori informazioni, vedere [URI e Helper dello stato di navigazione](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="c0754-134">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="c0754-135">Un provider di servizi personalizzato non fornisce automaticamente i servizi predefiniti elencati nella tabella.</span><span class="sxs-lookup"><span data-stu-id="c0754-135">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="c0754-136">Se si usa un provider di servizi personalizzato e si richiede uno dei servizi indicati nella tabella, aggiungere i servizi necessari al nuovo provider di servizi.</span><span class="sxs-lookup"><span data-stu-id="c0754-136">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="c0754-137">Aggiungere servizi a un'app</span><span class="sxs-lookup"><span data-stu-id="c0754-137">Add services to an app</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="c0754-138">WebAssembly Blazor</span><span class="sxs-lookup"><span data-stu-id="c0754-138">Blazor WebAssembly</span></span>

<span data-ttu-id="c0754-139">Configurare i servizi per la raccolta di servizi dell'app nel metodo `Main` di *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="c0754-139">Configure services for the app's service collection in the `Main` method of *Program.cs*.</span></span> <span data-ttu-id="c0754-140">Nell'esempio seguente, l'implementazione del `MyDependency` è registrata per `IMyDependency`:</span><span class="sxs-lookup"><span data-stu-id="c0754-140">In the following example, the `MyDependency` implementation is registered for `IMyDependency`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<IMyDependency, MyDependency>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="c0754-141">Una volta compilato l'host, è possibile accedere ai servizi dall'ambito radice prima DI eseguire il rendering di tutti i componenti.</span><span class="sxs-lookup"><span data-stu-id="c0754-141">Once the host is built, services can be accessed from the root DI scope before any components are rendered.</span></span> <span data-ttu-id="c0754-142">Questa operazione può essere utile per eseguire la logica di inizializzazione prima del rendering del contenuto:</span><span class="sxs-lookup"><span data-stu-id="c0754-142">This can be useful for running initialization logic before rendering content:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync();

        await host.RunAsync();
    }
}
```

<span data-ttu-id="c0754-143">L'host fornisce anche un'istanza di configurazione centrale per l'app.</span><span class="sxs-lookup"><span data-stu-id="c0754-143">The host also provides a central configuration instance for the app.</span></span> <span data-ttu-id="c0754-144">Basandosi sull'esempio precedente, l'URL del servizio meteo viene passato da un'origine di configurazione predefinita, ad esempio *appSettings. JSON*, a `InitializeWeatherAsync`:</span><span class="sxs-lookup"><span data-stu-id="c0754-144">Building on the preceding example, the weather service's URL is passed from a default configuration source (for example, *appsettings.json*) to `InitializeWeatherAsync`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync(
            host.Configuration["WeatherServiceUrl"]);

        await host.RunAsync();
    }
}
```

### <a name="blazor-server"></a><span data-ttu-id="c0754-145">Server Blazor</span><span class="sxs-lookup"><span data-stu-id="c0754-145">Blazor Server</span></span>

<span data-ttu-id="c0754-146">Dopo aver creato una nuova app, esaminare il metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c0754-146">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="c0754-147">Al metodo `ConfigureServices` viene passato un <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, ovvero un elenco di oggetti del descrittore del servizio (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="c0754-147">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="c0754-148">I servizi vengono aggiunti fornendo descrittori del servizio alla raccolta di servizi.</span><span class="sxs-lookup"><span data-stu-id="c0754-148">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="c0754-149">Nell'esempio seguente viene illustrato il concetto con l'interfaccia `IDataAccess` e la relativa implementazione concreta `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="c0754-149">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a><span data-ttu-id="c0754-150">Durata del servizio</span><span class="sxs-lookup"><span data-stu-id="c0754-150">Service lifetime</span></span>

<span data-ttu-id="c0754-151">I servizi possono essere configurati con le durate mostrate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="c0754-151">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="c0754-152">Durata</span><span class="sxs-lookup"><span data-stu-id="c0754-152">Lifetime</span></span> | <span data-ttu-id="c0754-153">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c0754-153">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="c0754-154">le app webassembly Blazor attualmente non dispongono di un concetto di ambiti di.</span><span class="sxs-lookup"><span data-stu-id="c0754-154">Blazor WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="c0754-155">i servizi registrati `Scoped`si comportano come `Singleton` Services.</span><span class="sxs-lookup"><span data-stu-id="c0754-155">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="c0754-156">Tuttavia, il modello di hosting del server Blazor supporta la durata `Scoped`.</span><span class="sxs-lookup"><span data-stu-id="c0754-156">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="c0754-157">Nelle app di Blazor server una registrazione del servizio con ambito ha come ambito la *connessione*.</span><span class="sxs-lookup"><span data-stu-id="c0754-157">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="c0754-158">Per questo motivo, è preferibile usare i servizi con ambito per i servizi che devono avere come ambito l'utente corrente, anche se l'obiettivo corrente è eseguire sul lato client nel browser.</span><span class="sxs-lookup"><span data-stu-id="c0754-158">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="c0754-159">La creazione di una *singola istanza* del servizio.</span><span class="sxs-lookup"><span data-stu-id="c0754-159">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="c0754-160">Tutti i componenti che richiedono un servizio di `Singleton` ricevono un'istanza dello stesso servizio.</span><span class="sxs-lookup"><span data-stu-id="c0754-160">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="c0754-161">Ogni volta che un componente ottiene un'istanza di un servizio `Transient` dal contenitore dei servizi, riceve una *nuova istanza* del servizio.</span><span class="sxs-lookup"><span data-stu-id="c0754-161">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="c0754-162">Il sistema DI è basato sul sistema DI ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0754-162">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="c0754-163">Per altre informazioni, vedere <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="c0754-163">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="c0754-164">Richiedere un servizio in un componente</span><span class="sxs-lookup"><span data-stu-id="c0754-164">Request a service in a component</span></span>

<span data-ttu-id="c0754-165">Una volta aggiunti i servizi alla raccolta di servizi, inserire i servizi nei componenti usando la direttiva [\@Inject](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="c0754-165">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="c0754-166">`@inject` dispone di due parametri:</span><span class="sxs-lookup"><span data-stu-id="c0754-166">`@inject` has two parameters:</span></span>

* <span data-ttu-id="c0754-167">Digitare &ndash; il tipo di servizio da inserire.</span><span class="sxs-lookup"><span data-stu-id="c0754-167">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="c0754-168">Proprietà &ndash; il nome della proprietà che riceve il servizio app inserito.</span><span class="sxs-lookup"><span data-stu-id="c0754-168">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="c0754-169">La proprietà non richiede la creazione manuale.</span><span class="sxs-lookup"><span data-stu-id="c0754-169">The property doesn't require manual creation.</span></span> <span data-ttu-id="c0754-170">Il compilatore crea la proprietà.</span><span class="sxs-lookup"><span data-stu-id="c0754-170">The compiler creates the property.</span></span>

<span data-ttu-id="c0754-171">Per altre informazioni, vedere <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="c0754-171">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="c0754-172">Usare più istruzioni `@inject` per inserire servizi diversi.</span><span class="sxs-lookup"><span data-stu-id="c0754-172">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="c0754-173">Nell'esempio riportato di seguito viene illustrato come usare `@inject`.</span><span class="sxs-lookup"><span data-stu-id="c0754-173">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="c0754-174">Il servizio che implementa `Services.IDataAccess` viene inserito nel `DataRepository`della proprietà del componente.</span><span class="sxs-lookup"><span data-stu-id="c0754-174">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="c0754-175">Si noti che il codice usa solo l'astrazione `IDataAccess`:</span><span class="sxs-lookup"><span data-stu-id="c0754-175">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="c0754-176">Internamente, la proprietà generata (`DataRepository`) utilizza l'attributo `InjectAttribute`.</span><span class="sxs-lookup"><span data-stu-id="c0754-176">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="c0754-177">In genere, questo attributo non viene utilizzato direttamente.</span><span class="sxs-lookup"><span data-stu-id="c0754-177">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="c0754-178">Se è necessaria una classe base per i componenti e le proprietà inserite sono necessarie anche per la classe base, aggiungere manualmente il `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="c0754-178">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="c0754-179">Nei componenti derivati dalla classe di base, la direttiva `@inject` non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="c0754-179">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="c0754-180">Il `InjectAttribute` della classe base è sufficiente:</span><span class="sxs-lookup"><span data-stu-id="c0754-180">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="c0754-181">Usare l'inserimento DI dipendenze nei servizi</span><span class="sxs-lookup"><span data-stu-id="c0754-181">Use DI in services</span></span>

<span data-ttu-id="c0754-182">Servizi complessi potrebbe richiedere servizi aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="c0754-182">Complex services might require additional services.</span></span> <span data-ttu-id="c0754-183">Nell'esempio precedente, `DataAccess` potrebbe richiedere il `HttpClient` servizio predefinito.</span><span class="sxs-lookup"><span data-stu-id="c0754-183">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="c0754-184">`@inject` (o `InjectAttribute`) non è disponibile per l'uso nei servizi.</span><span class="sxs-lookup"><span data-stu-id="c0754-184">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="c0754-185">È necessario usare invece l' *inserimento del costruttore* .</span><span class="sxs-lookup"><span data-stu-id="c0754-185">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="c0754-186">I servizi necessari vengono aggiunti aggiungendo parametri al costruttore del servizio.</span><span class="sxs-lookup"><span data-stu-id="c0754-186">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="c0754-187">Quando si crea il servizio, vengono riconosciuti i servizi richiesti nel costruttore e forniti DI conseguenza.</span><span class="sxs-lookup"><span data-stu-id="c0754-187">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

<span data-ttu-id="c0754-188">Prerequisiti per l'inserimento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="c0754-188">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="c0754-189">È necessario che esista un costruttore i cui argomenti possono essere tutti soddisfatti da DI.</span><span class="sxs-lookup"><span data-stu-id="c0754-189">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="c0754-190">Sono consentiti parametri aggiuntivi non analizzati da DI se specificano i valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="c0754-190">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="c0754-191">Il costruttore applicabile deve essere *pubblico*.</span><span class="sxs-lookup"><span data-stu-id="c0754-191">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="c0754-192">È necessario che esista un costruttore applicabile.</span><span class="sxs-lookup"><span data-stu-id="c0754-192">One applicable constructor must exist.</span></span> <span data-ttu-id="c0754-193">In caso di ambiguità, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="c0754-193">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="c0754-194">Classi di componenti di base dell'utilità per gestire un ambito DI</span><span class="sxs-lookup"><span data-stu-id="c0754-194">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="c0754-195">Nelle app ASP.NET Core, i servizi con ambito hanno in genere come ambito la richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="c0754-195">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="c0754-196">Al termine della richiesta, tutti i servizi con ambito o temporaneo vengono eliminati dal sistema DI.</span><span class="sxs-lookup"><span data-stu-id="c0754-196">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="c0754-197">Nelle app Blazor server l'ambito della richiesta dura per la durata della connessione client, che può comportare un tempo di permanenza dei servizi temporanei e con ambito maggiore del previsto.</span><span class="sxs-lookup"><span data-stu-id="c0754-197">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span> <span data-ttu-id="c0754-198">Nelle app Blazor webassembly i servizi registrati con una durata con ambito vengono considerati come singleton, quindi vivono più a lungo rispetto ai servizi con ambito nelle app ASP.NET Core tipiche.</span><span class="sxs-lookup"><span data-stu-id="c0754-198">In Blazor WebAssembly apps, services registered with a scoped lifetime are treated as singletons, so they live longer than scoped services in typical ASP.NET Core apps.</span></span>

<span data-ttu-id="c0754-199">Un approccio che limita la durata di un servizio nelle app Blazor è l'uso del tipo di `OwningComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="c0754-199">An approach that limits a service lifetime in Blazor apps is use of the `OwningComponentBase` type.</span></span> <span data-ttu-id="c0754-200">`OwningComponentBase` è un tipo astratto derivato da `ComponentBase` che consente di creare un ambito di che corrisponde alla durata del componente.</span><span class="sxs-lookup"><span data-stu-id="c0754-200">`OwningComponentBase` is an abstract type derived from `ComponentBase` that creates a DI scope corresponding to the lifetime of the component.</span></span> <span data-ttu-id="c0754-201">Con questo ambito, è possibile usare i servizi DI i con una durata con ambito e fare in modo che siano attivi fino a quando il componente.</span><span class="sxs-lookup"><span data-stu-id="c0754-201">Using this scope, it's possible to use DI services with a scoped lifetime and have them live as long as the component.</span></span> <span data-ttu-id="c0754-202">Quando il componente viene eliminato definitivamente, vengono eliminati anche i servizi del provider di servizi con ambito.</span><span class="sxs-lookup"><span data-stu-id="c0754-202">When the component is destroyed, services from the component's scoped service provider are disposed as well.</span></span> <span data-ttu-id="c0754-203">Questa operazione può essere utile per i servizi che:</span><span class="sxs-lookup"><span data-stu-id="c0754-203">This can be useful for services that:</span></span>

* <span data-ttu-id="c0754-204">È necessario riutilizzarlo all'interno di un componente, perché la durata temporanea non è appropriata.</span><span class="sxs-lookup"><span data-stu-id="c0754-204">Should be reused within a component, as the transient lifetime is inappropriate.</span></span>
* <span data-ttu-id="c0754-205">Non devono essere condivise tra i componenti, perché la durata singleton non è appropriata.</span><span class="sxs-lookup"><span data-stu-id="c0754-205">Shouldn't be shared across components, as the singleton lifetime is inappropriate.</span></span>

<span data-ttu-id="c0754-206">Sono disponibili due versioni del tipo di `OwningComponentBase`:</span><span class="sxs-lookup"><span data-stu-id="c0754-206">Two versions of the `OwningComponentBase` type are available:</span></span>

* <span data-ttu-id="c0754-207">`OwningComponentBase` è un figlio astratto e disposable del tipo di `ComponentBase` con una proprietà `ScopedServices` protetta di tipo `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="c0754-207">`OwningComponentBase` is an abstract, disposable child of the `ComponentBase` type with a protected `ScopedServices` property of type `IServiceProvider`.</span></span> <span data-ttu-id="c0754-208">Questo provider può essere utilizzato per risolvere i servizi che hanno come ambito la durata del componente.</span><span class="sxs-lookup"><span data-stu-id="c0754-208">This provider can be used to resolve services that are scoped to the lifetime of the component.</span></span>

  <span data-ttu-id="c0754-209">I servizi DI inserimento nel componente usando `@inject` o l'`InjectAttribute` (`[Inject]`) non vengono creati nell'ambito del componente.</span><span class="sxs-lookup"><span data-stu-id="c0754-209">DI services injected into the component using `@inject` or the `InjectAttribute` (`[Inject]`) aren't created in the component's scope.</span></span> <span data-ttu-id="c0754-210">Per utilizzare l'ambito del componente, è necessario risolvere i servizi utilizzando `ScopedServices.GetRequiredService` o `ScopedServices.GetService`.</span><span class="sxs-lookup"><span data-stu-id="c0754-210">To use the component's scope, services must be resolved using `ScopedServices.GetRequiredService` or `ScopedServices.GetService`.</span></span> <span data-ttu-id="c0754-211">Tutti i servizi risolti utilizzando il provider di `ScopedServices` hanno le dipendenze fornite dallo stesso ambito.</span><span class="sxs-lookup"><span data-stu-id="c0754-211">Any services resolved using the `ScopedServices` provider have their dependencies provided from that same scope.</span></span>

  ```razor
  @page "/preferences"
  @using Microsoft.Extensions.DependencyInjection
  @inherits OwningComponentBase

  <h1>User (@UserService.Name)</h1>

  <ul>
      @foreach (var setting in SettingService.GetSettings())
      {
          <li>@setting.SettingName: @setting.SettingValue</li>
      }
  </ul>

  @code {
      private IUserService UserService { get; set; }
      private ISettingService SettingService { get; set; }

      protected override void OnInitialized()
      {
          UserService = ScopedServices.GetRequiredService<IUserService>();
          SettingService = ScopedServices.GetRequiredService<ISettingService>();
      }
  }
  ```

* <span data-ttu-id="c0754-212">`OwningComponentBase<T>` deriva da `OwningComponentBase` e aggiunge una `Service` di proprietà che restituisce un'istanza di `T` dal provider DI entità.</span><span class="sxs-lookup"><span data-stu-id="c0754-212">`OwningComponentBase<T>` derives from `OwningComponentBase` and adds a property `Service` that returns an instance of `T` from the scoped DI provider.</span></span> <span data-ttu-id="c0754-213">Questo tipo è un modo pratico per accedere ai servizi con ambito senza usare un'istanza di `IServiceProvider` quando è presente un servizio primario richiesto dall'app dal contenitore DI inserimento delle dipendenze usando l'ambito del componente.</span><span class="sxs-lookup"><span data-stu-id="c0754-213">This type is a convenient way to access scoped services without using an instance of `IServiceProvider` when there's one primary service the app requires from the DI container using the component's scope.</span></span> <span data-ttu-id="c0754-214">La proprietà `ScopedServices` è disponibile, in modo che l'app possa ottenere i servizi di altri tipi, se necessario.</span><span class="sxs-lookup"><span data-stu-id="c0754-214">The `ScopedServices` property is available, so the app can get services of other types, if necessary.</span></span>

  ```razor
  @page "/users"
  @attribute [Authorize]
  @inherits OwningComponentBase<AppDbContext>

  <h1>Users (@Service.Users.Count())</h1>

  <ul>
      @foreach (var user in Service.Users)
      {
          <li>@user.UserName</li>
      }
  </ul>
  ```

## <a name="use-of-entity-framework-dbcontext-from-di"></a><span data-ttu-id="c0754-215">Uso di Entity Framework DbContext da DI</span><span class="sxs-lookup"><span data-stu-id="c0754-215">Use of Entity Framework DbContext from DI</span></span>

<span data-ttu-id="c0754-216">Un tipo di servizio comune da recuperare da un in app Web è Entity Framework (EF) `DbContext` oggetti.</span><span class="sxs-lookup"><span data-stu-id="c0754-216">One common service type to retrieve from DI in web apps is Entity Framework (EF) `DbContext` objects.</span></span> <span data-ttu-id="c0754-217">Per impostazione predefinita, la registrazione dei servizi EF con `IServiceCollection.AddDbContext` aggiunge il `DbContext` come servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="c0754-217">Registering EF services using `IServiceCollection.AddDbContext` adds the `DbContext` as a scoped service by default.</span></span> <span data-ttu-id="c0754-218">La registrazione come servizio con ambito può causare problemi nelle app Blazor perché causa la lunga durata delle istanze di `DbContext` e la condivisione nell'app.</span><span class="sxs-lookup"><span data-stu-id="c0754-218">Registering as a scoped service can lead to problems in Blazor apps because it causes `DbContext` instances to be long-lived and shared across the app.</span></span> <span data-ttu-id="c0754-219">`DbContext` non è thread-safe e non deve essere usato simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="c0754-219">`DbContext` isn't thread-safe and must not be used concurrently.</span></span>

<span data-ttu-id="c0754-220">A seconda dell'app, l'uso di `OwningComponentBase` per limitare l'ambito di un `DbContext` a un singolo componente *può* risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="c0754-220">Depending on the app, using `OwningComponentBase` to limit the scope of a `DbContext` to a single component *may* solve the issue.</span></span> <span data-ttu-id="c0754-221">Se un componente non usa un `DbContext` in parallelo, la derivazione del componente da `OwningComponentBase` e il recupero del `DbContext` da `ScopedServices` è sufficiente perché garantisce quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c0754-221">If a component doesn't use a `DbContext` in parallel, deriving the component from `OwningComponentBase` and retrieving the `DbContext` from `ScopedServices` is sufficient because it ensures that:</span></span>

* <span data-ttu-id="c0754-222">I componenti separati non condividono un `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c0754-222">Separate components don't share a `DbContext`.</span></span>
* <span data-ttu-id="c0754-223">Il `DbContext` risiede solo fino a quando il componente dipende da esso.</span><span class="sxs-lookup"><span data-stu-id="c0754-223">The `DbContext` lives only as long as the component depending on it.</span></span>

<span data-ttu-id="c0754-224">Se un singolo componente può usare contemporaneamente un `DbContext` (ad esempio, ogni volta che un utente seleziona un pulsante), anche usando `OwningComponentBase` non si evitano problemi con le operazioni EF simultanee.</span><span class="sxs-lookup"><span data-stu-id="c0754-224">If a single component might use a `DbContext` concurrently (for example, every time a user selects a button), even using `OwningComponentBase` doesn't avoid issues with concurrent EF operations.</span></span> <span data-ttu-id="c0754-225">In tal caso, usare un `DbContext` diverso per ogni operazione EF logica.</span><span class="sxs-lookup"><span data-stu-id="c0754-225">In that case, use a different `DbContext` for each logical EF operation.</span></span> <span data-ttu-id="c0754-226">Usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="c0754-226">Use either of the following approaches:</span></span>

* <span data-ttu-id="c0754-227">Creare il `DbContext` direttamente usando `DbContextOptions<TContext>` come argomento, che può essere recuperato da DI ed è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="c0754-227">Create the `DbContext` directly using `DbContextOptions<TContext>` as an argument, which can be retrieved from DI and is thread safe.</span></span>

    ```razor
    @page "/example"
    @inject DbContextOptions<AppDbContext> DbContextOptions

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = new AppDbContext(DbContextOptions))
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

* <span data-ttu-id="c0754-228">Registrare il `DbContext` nel contenitore del servizio con una durata temporanea:</span><span class="sxs-lookup"><span data-stu-id="c0754-228">Register the `DbContext` in the service container with a transient lifetime:</span></span>
  * <span data-ttu-id="c0754-229">Quando si registra il contesto, utilizzare `ServiceLifetime.Transient`.</span><span class="sxs-lookup"><span data-stu-id="c0754-229">When registering the context, use `ServiceLifetime.Transient`.</span></span> <span data-ttu-id="c0754-230">Il metodo di estensione `AddDbContext` accetta due parametri facoltativi di tipo `ServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="c0754-230">The `AddDbContext` extension method takes two optional parameters of type `ServiceLifetime`.</span></span> <span data-ttu-id="c0754-231">Per utilizzare questo approccio, è necessario `ServiceLifetime.Transient`solo il parametro `contextLifetime`.</span><span class="sxs-lookup"><span data-stu-id="c0754-231">To use this approach, only the `contextLifetime` parameter needs to be `ServiceLifetime.Transient`.</span></span> <span data-ttu-id="c0754-232">`optionsLifetime` possibile mantenete il valore predefinito di `ServiceLifetime.Scoped`.</span><span class="sxs-lookup"><span data-stu-id="c0754-232">`optionsLifetime` can keep its default value of `ServiceLifetime.Scoped`.</span></span>

    ```csharp
    services.AddDbContext<AppDbContext>(options =>
         options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")),
         ServiceLifetime.Transient);
    ```  

  * <span data-ttu-id="c0754-233">I `DbContext` temporanei possono essere inseriti normalmente (usando `@inject`) in componenti che non eseguono più operazioni EF in parallelo.</span><span class="sxs-lookup"><span data-stu-id="c0754-233">The transient `DbContext` can be injected as normal (using `@inject`) into components that will not execute multiple EF operations in parallel.</span></span> <span data-ttu-id="c0754-234">Quelli che possono eseguire contemporaneamente più operazioni EF possono richiedere oggetti `DbContext` distinti per ogni operazione parallela usando `IServiceProvider.GetRequiredService`.</span><span class="sxs-lookup"><span data-stu-id="c0754-234">Those that may perform multiple EF operations simultaneously can request separate `DbContext` objects for each parallel operation using `IServiceProvider.GetRequiredService`.</span></span>

    ```razor
    @page "/example"
    @using Microsoft.Extensions.DependencyInjection
    @inject IServiceProvider ServiceProvider

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = ServiceProvider.GetRequiredService<AppDbContext>())
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

## <a name="additional-resources"></a><span data-ttu-id="c0754-235">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c0754-235">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
