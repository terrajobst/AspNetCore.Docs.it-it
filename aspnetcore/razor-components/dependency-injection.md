---
title: Inserimento di dipendenze componenti Razor
author: guardrex
description: Scopri App Blazor e Razor componenti possono utilizzare servizi inserendole in componenti.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 40aec2e3a5032039c7d921f67d7d333b03c07fb1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2019
ms.locfileid: "59515488"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="ff1ad-103">Inserimento di dipendenze componenti Razor</span><span class="sxs-lookup"><span data-stu-id="ff1ad-103">Razor Components dependency injection</span></span>

<span data-ttu-id="ff1ad-104">Da [Stropek Rainer](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="ff1ad-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="ff1ad-105">Supporta i componenti di Razor [inserimento delle dipendenze (dipendenze)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ff1ad-105">Razor Components supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ff1ad-106">Le app possono usare i servizi incorporati inserendole in componenti.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="ff1ad-107">Le app possono anche definire e registrare servizi personalizzati e renderli disponibili in tutta l'app tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="ff1ad-108">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="ff1ad-108">Dependency injection</span></span>

<span data-ttu-id="ff1ad-109">L'inserimento delle dipendenze è una tecnica per l'accesso ai servizi configurati in una posizione centrale.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="ff1ad-110">Ciò può risultare utile nelle App Razor componenti per:</span><span class="sxs-lookup"><span data-stu-id="ff1ad-110">This can be useful in Razor Components apps to:</span></span>

* <span data-ttu-id="ff1ad-111">Condividere una singola istanza di una classe di servizio tra molti componenti, noti come un *singleton* servizio.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-111">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="ff1ad-112">Separare i componenti dalle classi concrete servizio utilizzando le astrazioni di riferimento.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-112">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="ff1ad-113">Si consideri ad esempio un'interfaccia `IDataAccess` per l'accesso ai dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-113">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="ff1ad-114">L'interfaccia è implementata da un concreto `DataAccess` classe e registrato come servizio nel contenitore dei servizi dell'app.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-114">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="ff1ad-115">Quando un componente Usa l'inserimento delle dipendenze per ricevere un `IDataAccess` implementazione, il componente non è associato al tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-115">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="ff1ad-116">L'implementazione è possibile scambiare, forse a un'implementazione fittizia negli unit test.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-116">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="ff1ad-117">Per altre informazioni, vedere <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-117">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="ff1ad-118">Aggiungere servizi per l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="ff1ad-118">Add services to DI</span></span>

<span data-ttu-id="ff1ad-119">Dopo aver creato una nuova app, esaminare il `Startup.ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="ff1ad-119">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="ff1ad-120">Il `ConfigureServices` viene passato un <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, ovvero un elenco di oggetti descrittore di servizio (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="ff1ad-120">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="ff1ad-121">I servizi vengono aggiunti, fornendo i descrittori di servizio per l'insieme al servizio.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-121">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="ff1ad-122">Nell'esempio seguente viene illustrato il concetto con il `IDataAccess` interfaccia e la relativa implementazione concreta `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="ff1ad-122">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="ff1ad-123">Servizi possono essere configurati con le durate illustrate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-123">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="ff1ad-124">Durata</span><span class="sxs-lookup"><span data-stu-id="ff1ad-124">Lifetime</span></span> | <span data-ttu-id="ff1ad-125">Descrizione</span><span class="sxs-lookup"><span data-stu-id="ff1ad-125">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="ff1ad-126">L'inserimento delle dipendenze consente di creare un *singola istanza* del servizio.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-126">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="ff1ad-127">Tutti i componenti che richiedono un `Singleton` servizio ricevere un'istanza dello stesso servizio.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-127">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="ff1ad-128">Ogni volta che un componente ottiene un'istanza di un `Transient` servizio dal contenitore dei servizi, riceve un *nuova istanza* del servizio.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-128">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="ff1ad-129">Blazor lato client non ha attualmente il concetto di ambiti di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-129">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="ff1ad-130">`Scoped` si comporta come `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-130">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="ff1ad-131">Tuttavia, i componenti di ASP.NET Core Razor supporta il `Scoped` durata.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-131">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="ff1ad-132">In un componente di Razor, la registrazione di un servizio con ambito ha come ambita la connessione.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-132">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="ff1ad-133">Per questo motivo, tramite i servizi con ambiti è preferibile per i servizi che devono avere come ambiti l'utente corrente, anche se si intende corrente per l'esecuzione del client nel browser.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-133">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |

<span data-ttu-id="ff1ad-134">Il sistema di inserimento delle dipendenze si basa sul sistema di inserimento delle dipendenze in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-134">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="ff1ad-135">Per altre informazioni, vedere <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-135">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="default-services"></a><span data-ttu-id="ff1ad-136">Servizi predefiniti</span><span class="sxs-lookup"><span data-stu-id="ff1ad-136">Default services</span></span>

<span data-ttu-id="ff1ad-137">I servizi predefiniti vengono aggiunti automaticamente alla raccolta di servizio dell'app.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-137">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="ff1ad-138">Service</span><span class="sxs-lookup"><span data-stu-id="ff1ad-138">Service</span></span> | <span data-ttu-id="ff1ad-139">Descrizione</span><span class="sxs-lookup"><span data-stu-id="ff1ad-139">Description</span></span> |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="ff1ad-140">Fornisce metodi per inviare richieste HTTP e ricevere risposte HTTP da una risorsa identificata da un URI (singleton).</span><span class="sxs-lookup"><span data-stu-id="ff1ad-140">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="ff1ad-141">Si noti che questa istanza di `HttpClient` Usa il browser per la gestione del traffico HTTP in background.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-141">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="ff1ad-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) viene impostata automaticamente per il prefisso URI di base dell'app.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="ff1ad-143">`HttpClient` viene fornito solo per le app Blazor lato client.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-143">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="ff1ad-144">Rappresenta un'istanza di un runtime JavaScript a cui possono essere inviate chiamate.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-144">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="ff1ad-145">Per altre informazioni, vedere <xref:razor-components/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-145">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="ff1ad-146">Contiene gli helper per l'uso di URI e l'esplorazione dello stato (singleton).</span><span class="sxs-lookup"><span data-stu-id="ff1ad-146">Contains helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="ff1ad-147">`IUriHelper` viene fornito all'App Blazor sia componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-147">`IUriHelper` is provided to both Blazor and Razor Components apps.</span></span> |

<span data-ttu-id="ff1ad-148">È possibile usare un provider di servizi personalizzati anziché il provider di servizi predefiniti aggiunto dal modello predefinito.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-148">It's possible to use a custom service provider instead of the default service provider added by the default template.</span></span> <span data-ttu-id="ff1ad-149">Un provider di servizi personalizzati non fornisce automaticamente i servizi predefiniti elencati nella tabella.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-149">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="ff1ad-150">Se si usa un provider di servizi personalizzati e si richiedono i servizi illustrati nella tabella, aggiungere i servizi necessari per il nuovo provider del servizio.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-150">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="ff1ad-151">Richiesta di un servizio in un componente</span><span class="sxs-lookup"><span data-stu-id="ff1ad-151">Request a service in a component</span></span>

<span data-ttu-id="ff1ad-152">Dopo che i servizi vengono aggiunti alla raccolta di servizio, inserire i modelli Razor dei componenti tramite i servizi di [ @inject ](xref:mvc/views/razor#section-4) direttiva Razor.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-152">After services are added to the service collection, inject the services into the components' Razor templates using the [@inject](xref:mvc/views/razor#section-4) Razor directive.</span></span> <span data-ttu-id="ff1ad-153">`@inject` presenta due parametri:</span><span class="sxs-lookup"><span data-stu-id="ff1ad-153">`@inject` has two parameters:</span></span>

* <span data-ttu-id="ff1ad-154">Nome del tipo: Il tipo del servizio da inserire.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-154">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="ff1ad-155">Nome proprietà: Il nome della proprietà la ricezione del servizio app inserito.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-155">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="ff1ad-156">Si noti che la proprietà non richiede la creazione manuale.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-156">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="ff1ad-157">Il compilatore crea la proprietà.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-157">The compiler creates the property.</span></span>

<span data-ttu-id="ff1ad-158">Per altre informazioni, vedere <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-158">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="ff1ad-159">Usare più `@inject` istruzioni per inserire diversi servizi.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-159">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="ff1ad-160">Nell'esempio riportato di seguito viene illustrato come usare `@inject`.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-160">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="ff1ad-161">Il servizio che implementa `Services.IDataAccess` viene inserito nelle proprietà del componente `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-161">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="ff1ad-162">Si noti come Usa solo il codice di `IDataAccess` astrazione:</span><span class="sxs-lookup"><span data-stu-id="ff1ad-162">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

<span data-ttu-id="ff1ad-163">Internamente, la proprietà generata (`DataRepository`) è dotato di `InjectAttribute` attributo.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-163">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="ff1ad-164">In genere, questo attributo non viene utilizzato direttamente.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-164">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="ff1ad-165">Se è necessaria per i componenti di una classe base e inserito le proprietà sono necessari anche per la classe di base, `InjectAttribute` possono essere aggiunte manualmente:</span><span class="sxs-lookup"><span data-stu-id="ff1ad-165">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="ff1ad-166">In componenti derivati dalla classe di base, il `@inject` direttiva non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-166">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="ff1ad-167">Il `InjectAttribute` della classe di base è sufficiente:</span><span class="sxs-lookup"><span data-stu-id="ff1ad-167">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="ff1ad-168">Inserimento delle dipendenze in servizi</span><span class="sxs-lookup"><span data-stu-id="ff1ad-168">Dependency injection in services</span></span>

<span data-ttu-id="ff1ad-169">Servizi complessi potrebbero richiedere servizi aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-169">Complex services might require additional services.</span></span> <span data-ttu-id="ff1ad-170">Nell'esempio precedente `DataAccess` potrebbe richiedere il `HttpClient` del servizio predefinito.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-170">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="ff1ad-171">`@inject` (o `InjectAttribute`) non è disponibile per l'utilizzo nei servizi.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-171">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="ff1ad-172">*Inserimento del costruttore* devono quindi essere utilizzate.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-172">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="ff1ad-173">I servizi necessari vengono aggiunti tramite l'aggiunta di parametri al costruttore del servizio.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-173">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="ff1ad-174">Quando l'inserimento delle dipendenze consente di creare il servizio, riconosce i servizi richiede nel costruttore e consente loro di conseguenza.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-174">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="ff1ad-175">Prerequisiti per l'inserimento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="ff1ad-175">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="ff1ad-176">Deve essere presente un costruttore cui argomenti possono essere tutti specificati tramite inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-176">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="ff1ad-177">Si noti che sono consentiti parametri aggiuntivi non coperti da inserimento delle dipendenze, se si specificano valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-177">Note that additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="ff1ad-178">Il costruttore applicabile deve essere *pubblica*.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-178">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="ff1ad-179">Deve essere presente solo un costruttore applicabile.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-179">There must only be one applicable constructor.</span></span> <span data-ttu-id="ff1ad-180">In caso di ambiguità, l'inserimento delle dipendenze genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="ff1ad-180">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff1ad-181">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ff1ad-181">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
