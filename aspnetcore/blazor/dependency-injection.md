---
title: ASP.NET Core Blazor inserimento delle dipendenze
author: guardrex
description: Scopri in che modo le app Blazor possono inserire i servizi nei componenti.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/dependency-injection
ms.openlocfilehash: 17dd0f927064ae7c2b1e3e439fd93e2cb220a5a4
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879773"
---
# <a name="aspnet-core-opno-locblazor-dependency-injection"></a><span data-ttu-id="a94f3-103">ASP.NET Core Blazor inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="a94f3-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="a94f3-104">Di [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="a94f3-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="a94f3-105"> supporta l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a94f3-105"> supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a94f3-106">Le app possono usare i servizi predefiniti inserendoli in componenti.</span><span class="sxs-lookup"><span data-stu-id="a94f3-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="a94f3-107">Le app possono anche definire e registrare servizi personalizzati e renderli disponibili nell'app tramite DI.</span><span class="sxs-lookup"><span data-stu-id="a94f3-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="a94f3-108">DI è una tecnica per accedere ai servizi configurati in una posizione centrale.</span><span class="sxs-lookup"><span data-stu-id="a94f3-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="a94f3-109">Questa operazione può essere utile nelle app Blazor per:</span><span class="sxs-lookup"><span data-stu-id="a94f3-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="a94f3-110">Condividere una singola istanza di una classe di servizio in molti componenti, noti come un servizio *singleton* .</span><span class="sxs-lookup"><span data-stu-id="a94f3-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="a94f3-111">Separare i componenti da classi di servizi concrete usando astrazioni di riferimento.</span><span class="sxs-lookup"><span data-stu-id="a94f3-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="a94f3-112">Si consideri, ad esempio, un'interfaccia `IDataAccess` per l'accesso ai dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="a94f3-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="a94f3-113">L'interfaccia viene implementata da una classe di `DataAccess` concreta e registrata come servizio nel contenitore del servizio dell'app.</span><span class="sxs-lookup"><span data-stu-id="a94f3-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="a94f3-114">Quando un componente usa il per ricevere un'implementazione di `IDataAccess`, il componente non è associato al tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="a94f3-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="a94f3-115">L'implementazione può essere scambiata, ad esempio per un'implementazione fittizia negli unit test.</span><span class="sxs-lookup"><span data-stu-id="a94f3-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="a94f3-116">Servizi predefiniti</span><span class="sxs-lookup"><span data-stu-id="a94f3-116">Default services</span></span>

<span data-ttu-id="a94f3-117">I servizi predefiniti vengono aggiunti automaticamente alla raccolta di servizi dell'app.</span><span class="sxs-lookup"><span data-stu-id="a94f3-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="a94f3-118">Servizio</span><span class="sxs-lookup"><span data-stu-id="a94f3-118">Service</span></span> | <span data-ttu-id="a94f3-119">Durata</span><span class="sxs-lookup"><span data-stu-id="a94f3-119">Lifetime</span></span> | <span data-ttu-id="a94f3-120">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a94f3-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="a94f3-121">Singleton</span><span class="sxs-lookup"><span data-stu-id="a94f3-121">Singleton</span></span> | <span data-ttu-id="a94f3-122">Fornisce metodi per l'invio di richieste HTTP e la ricezione di risposte HTTP da una risorsa identificata da un URI.</span><span class="sxs-lookup"><span data-stu-id="a94f3-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="a94f3-123">L'istanza di `HttpClient` in un'app webassembly Blazor usa il browser per gestire il traffico HTTP in background.</span><span class="sxs-lookup"><span data-stu-id="a94f3-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="a94f3-124">per impostazione predefinita, le app Server Blazor non includono un `HttpClient` configurato come servizio.</span><span class="sxs-lookup"><span data-stu-id="a94f3-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="a94f3-125">Fornire un `HttpClient` a un'app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="a94f3-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="a94f3-126">Per ulteriori informazioni, vedere <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="a94f3-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="a94f3-127">Singleton</span><span class="sxs-lookup"><span data-stu-id="a94f3-127">Singleton</span></span> | <span data-ttu-id="a94f3-128">Rappresenta un'istanza di un runtime JavaScript in cui vengono inviate le chiamate a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a94f3-128">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="a94f3-129">Per ulteriori informazioni, vedere <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="a94f3-129">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="a94f3-130">Singleton</span><span class="sxs-lookup"><span data-stu-id="a94f3-130">Singleton</span></span> | <span data-ttu-id="a94f3-131">Contiene gli helper per lavorare con gli URI e lo stato di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a94f3-131">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="a94f3-132">Per ulteriori informazioni, vedere [URI e Helper dello stato di navigazione](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="a94f3-132">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="a94f3-133">Un provider di servizi personalizzato non fornisce automaticamente i servizi predefiniti elencati nella tabella.</span><span class="sxs-lookup"><span data-stu-id="a94f3-133">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="a94f3-134">Se si usa un provider di servizi personalizzato e si richiede uno dei servizi indicati nella tabella, aggiungere i servizi necessari al nuovo provider di servizi.</span><span class="sxs-lookup"><span data-stu-id="a94f3-134">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="a94f3-135">Aggiungere servizi a un'app</span><span class="sxs-lookup"><span data-stu-id="a94f3-135">Add services to an app</span></span>

<span data-ttu-id="a94f3-136">Dopo aver creato una nuova app, esaminare il metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a94f3-136">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="a94f3-137">Al metodo `ConfigureServices` viene passato un <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, ovvero un elenco di oggetti del descrittore del servizio (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="a94f3-137">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="a94f3-138">I servizi vengono aggiunti fornendo descrittori del servizio alla raccolta di servizi.</span><span class="sxs-lookup"><span data-stu-id="a94f3-138">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="a94f3-139">Nell'esempio seguente viene illustrato il concetto con l'interfaccia `IDataAccess` e la relativa implementazione concreta `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="a94f3-139">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="a94f3-140">I servizi possono essere configurati con le durate mostrate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="a94f3-140">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="a94f3-141">Durata</span><span class="sxs-lookup"><span data-stu-id="a94f3-141">Lifetime</span></span> | <span data-ttu-id="a94f3-142">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a94f3-142">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="a94f3-143">le app webassembly Blazor attualmente non dispongono di un concetto di ambiti di.</span><span class="sxs-lookup"><span data-stu-id="a94f3-143">Blazor WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="a94f3-144">i servizi registrati `Scoped`si comportano come `Singleton` Services.</span><span class="sxs-lookup"><span data-stu-id="a94f3-144">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="a94f3-145">Tuttavia, il modello di hosting del server Blazor supporta la durata `Scoped`.</span><span class="sxs-lookup"><span data-stu-id="a94f3-145">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="a94f3-146">Nelle app di Blazor server una registrazione del servizio con ambito ha come ambito la *connessione*.</span><span class="sxs-lookup"><span data-stu-id="a94f3-146">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="a94f3-147">Per questo motivo, è preferibile usare i servizi con ambito per i servizi che devono avere come ambito l'utente corrente, anche se l'obiettivo corrente è eseguire sul lato client nel browser.</span><span class="sxs-lookup"><span data-stu-id="a94f3-147">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="a94f3-148">La creazione di una *singola istanza* del servizio.</span><span class="sxs-lookup"><span data-stu-id="a94f3-148">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="a94f3-149">Tutti i componenti che richiedono un servizio di `Singleton` ricevono un'istanza dello stesso servizio.</span><span class="sxs-lookup"><span data-stu-id="a94f3-149">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="a94f3-150">Ogni volta che un componente ottiene un'istanza di un servizio `Transient` dal contenitore dei servizi, riceve una *nuova istanza* del servizio.</span><span class="sxs-lookup"><span data-stu-id="a94f3-150">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="a94f3-151">Il sistema DI è basato sul sistema DI ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a94f3-151">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="a94f3-152">Per ulteriori informazioni, vedere <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="a94f3-152">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="a94f3-153">Richiedere un servizio in un componente</span><span class="sxs-lookup"><span data-stu-id="a94f3-153">Request a service in a component</span></span>

<span data-ttu-id="a94f3-154">Una volta aggiunti i servizi alla raccolta di servizi, inserire i servizi nei componenti usando la direttiva [\@Inject](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="a94f3-154">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="a94f3-155">`@inject` dispone di due parametri:</span><span class="sxs-lookup"><span data-stu-id="a94f3-155">`@inject` has two parameters:</span></span>

* <span data-ttu-id="a94f3-156">Digitare &ndash; il tipo di servizio da inserire.</span><span class="sxs-lookup"><span data-stu-id="a94f3-156">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="a94f3-157">Proprietà &ndash; il nome della proprietà che riceve il servizio app inserito.</span><span class="sxs-lookup"><span data-stu-id="a94f3-157">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="a94f3-158">La proprietà non richiede la creazione manuale.</span><span class="sxs-lookup"><span data-stu-id="a94f3-158">The property doesn't require manual creation.</span></span> <span data-ttu-id="a94f3-159">Il compilatore crea la proprietà.</span><span class="sxs-lookup"><span data-stu-id="a94f3-159">The compiler creates the property.</span></span>

<span data-ttu-id="a94f3-160">Per ulteriori informazioni, vedere <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="a94f3-160">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="a94f3-161">Usare più istruzioni `@inject` per inserire servizi diversi.</span><span class="sxs-lookup"><span data-stu-id="a94f3-161">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="a94f3-162">Nell'esempio riportato di seguito viene illustrato come usare `@inject`.</span><span class="sxs-lookup"><span data-stu-id="a94f3-162">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="a94f3-163">Il servizio che implementa `Services.IDataAccess` viene inserito nel `DataRepository`della proprietà del componente.</span><span class="sxs-lookup"><span data-stu-id="a94f3-163">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="a94f3-164">Si noti che il codice usa solo l'astrazione `IDataAccess`:</span><span class="sxs-lookup"><span data-stu-id="a94f3-164">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="a94f3-165">Internamente, la proprietà generata (`DataRepository`) utilizza l'attributo `InjectAttribute`.</span><span class="sxs-lookup"><span data-stu-id="a94f3-165">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="a94f3-166">In genere, questo attributo non viene utilizzato direttamente.</span><span class="sxs-lookup"><span data-stu-id="a94f3-166">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="a94f3-167">Se è necessaria una classe base per i componenti e le proprietà inserite sono necessarie anche per la classe base, aggiungere manualmente il `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="a94f3-167">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="a94f3-168">Nei componenti derivati dalla classe di base, la direttiva `@inject` non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="a94f3-168">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="a94f3-169">Il `InjectAttribute` della classe base è sufficiente:</span><span class="sxs-lookup"><span data-stu-id="a94f3-169">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="a94f3-170">Usare l'inserimento DI dipendenze nei servizi</span><span class="sxs-lookup"><span data-stu-id="a94f3-170">Use DI in services</span></span>

<span data-ttu-id="a94f3-171">Servizi complessi potrebbe richiedere servizi aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="a94f3-171">Complex services might require additional services.</span></span> <span data-ttu-id="a94f3-172">Nell'esempio precedente, `DataAccess` potrebbe richiedere il `HttpClient` servizio predefinito.</span><span class="sxs-lookup"><span data-stu-id="a94f3-172">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="a94f3-173">`@inject` (o `InjectAttribute`) non è disponibile per l'uso nei servizi.</span><span class="sxs-lookup"><span data-stu-id="a94f3-173">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="a94f3-174">È necessario usare invece l' *inserimento del costruttore* .</span><span class="sxs-lookup"><span data-stu-id="a94f3-174">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="a94f3-175">I servizi necessari vengono aggiunti aggiungendo parametri al costruttore del servizio.</span><span class="sxs-lookup"><span data-stu-id="a94f3-175">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="a94f3-176">Quando si crea il servizio, vengono riconosciuti i servizi richiesti nel costruttore e forniti DI conseguenza.</span><span class="sxs-lookup"><span data-stu-id="a94f3-176">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="a94f3-177">Prerequisiti per l'inserimento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="a94f3-177">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="a94f3-178">È necessario che esista un costruttore i cui argomenti possono essere tutti soddisfatti da DI.</span><span class="sxs-lookup"><span data-stu-id="a94f3-178">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="a94f3-179">Sono consentiti parametri aggiuntivi non analizzati da DI se specificano i valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="a94f3-179">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="a94f3-180">Il costruttore applicabile deve essere *pubblico*.</span><span class="sxs-lookup"><span data-stu-id="a94f3-180">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="a94f3-181">È necessario che esista un costruttore applicabile.</span><span class="sxs-lookup"><span data-stu-id="a94f3-181">One applicable constructor must exist.</span></span> <span data-ttu-id="a94f3-182">In caso di ambiguità, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="a94f3-182">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="a94f3-183">Classi di componenti di base dell'utilità per gestire un ambito DI</span><span class="sxs-lookup"><span data-stu-id="a94f3-183">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="a94f3-184">Nelle app ASP.NET Core, i servizi con ambito hanno in genere come ambito la richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="a94f3-184">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="a94f3-185">Al termine della richiesta, tutti i servizi con ambito o temporaneo vengono eliminati dal sistema DI.</span><span class="sxs-lookup"><span data-stu-id="a94f3-185">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="a94f3-186">Nelle app Blazor server l'ambito della richiesta dura per la durata della connessione client, che può comportare un tempo di permanenza dei servizi temporanei e con ambito maggiore del previsto.</span><span class="sxs-lookup"><span data-stu-id="a94f3-186">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="a94f3-187">Per definire l'ambito dei servizi per la durata di un componente, può usare le classi di base `OwningComponentBase` e `OwningComponentBase<TService>`.</span><span class="sxs-lookup"><span data-stu-id="a94f3-187">To scope services to the lifetime of a component, can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="a94f3-188">Queste classi di base espongono una proprietà `ScopedServices` di tipo `IServiceProvider` che risolve i servizi che hanno come ambito la durata del componente.</span><span class="sxs-lookup"><span data-stu-id="a94f3-188">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="a94f3-189">Per creare un componente che eredita da una classe di base in Razor, usare la direttiva `@inherits`.</span><span class="sxs-lookup"><span data-stu-id="a94f3-189">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

```cshtml
@page "/users"
@attribute [Authorize]
@inherits OwningComponentBase<Data.ApplicationDbContext>

<h1>Users (@Service.Users.Count())</h1>
<ul>
    @foreach (var user in Service.Users)
    {
        <li>@user.UserName</li>
    }
</ul>
```

> [!NOTE]
> <span data-ttu-id="a94f3-190">I servizi inseriti nel componente usando `@inject` o l'`InjectAttribute` non vengono creati nell'ambito del componente e sono associati all'ambito della richiesta.</span><span class="sxs-lookup"><span data-stu-id="a94f3-190">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a94f3-191">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a94f3-191">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
