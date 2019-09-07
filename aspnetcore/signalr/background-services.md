---
title: Host ASP.NET Core SignalR in servizi in background
author: bradygaster
description: Informazioni su come inviare messaggi ai client SignalR dalle classi BackgroundService di .NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: 23a53f33a03ce3b76cf6846c3f214a6cad055209
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773944"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="ac4ec-103">Host ASP.NET Core SignalR in servizi in background</span><span class="sxs-lookup"><span data-stu-id="ac4ec-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="ac4ec-104">Di [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="ac4ec-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="ac4ec-105">Questo articolo fornisce indicazioni per:</span><span class="sxs-lookup"><span data-stu-id="ac4ec-105">This article provides guidance for:</span></span>

* <span data-ttu-id="ac4ec-106">Ospitare Hub SignalR usando un processo di lavoro in background ospitato con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="ac4ec-107">Invio di messaggi ai client connessi dall'interno di un [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="ac4ec-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(come scaricare)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="ac4ec-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="ac4ec-109">Cablare SignalR durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="ac4ec-109">Wire up SignalR during startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ac4ec-110">L'hosting di ASP.NET Core Hub SignalR nel contesto di un processo di lavoro in background è identico all'hosting di un hub in un'app Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="ac4ec-111">Nel metodo, la chiamata `services.AddSignalR` di aggiunge i servizi richiesti al livello di inserimento delle dipendenze di ASP.NET Core per supportare SignalR. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="ac4ec-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="ac4ec-112">In `Startup.Configure`il metodovienechiamatonelcallbackpercollegaregliendpointdell'hubnellapipelinedellarichiestaASP.NETCore.`MapHub` `UseEndpoints`</span><span class="sxs-lookup"><span data-stu-id="ac4ec-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSignalR();
        services.AddHostedService<Worker>();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHub<ClockHub>("/hubs/clock");
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="ac4ec-113">L'hosting di ASP.NET Core Hub SignalR nel contesto di un processo di lavoro in background è identico all'hosting di un hub in un'app Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="ac4ec-114">Nel metodo, la chiamata `services.AddSignalR` di aggiunge i servizi richiesti al livello di inserimento delle dipendenze di ASP.NET Core per supportare SignalR. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="ac4ec-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="ac4ec-115">In `Startup.Configure`viene chiamato `UseSignalR` il metodo per collegare gli endpoint dell'hub nella pipeline della richiesta ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-115">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="ac4ec-116">Nell'esempio precedente, la `ClockHub` classe implementa la `Hub<T>` classe per creare un hub fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="ac4ec-117">È stato configurato `Startup` nella classe per rispondere alle richieste all'endpoint `/hubs/clock`. `ClockHub`</span><span class="sxs-lookup"><span data-stu-id="ac4ec-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="ac4ec-118">Per altre informazioni sugli hub fortemente tipizzati, vedere [usare gli hub in SignalR per ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="ac4ec-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="ac4ec-119">Questa funzionalità non è limitata alla classe di [\<> Hub t](xref:Microsoft.AspNetCore.SignalR.Hub`1) .</span><span class="sxs-lookup"><span data-stu-id="ac4ec-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="ac4ec-120">Anche tutte le classi che ereditano dall' [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), ad esempio [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), funzioneranno.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="ac4ec-121">L'interfaccia utilizzata dalla fortemente tipizzata `ClockHub` è l' `IClock` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="ac4ec-122">Chiamare un hub SignalR da un servizio in background</span><span class="sxs-lookup"><span data-stu-id="ac4ec-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="ac4ec-123">Durante l'avvio, `Worker` la classe, `BackgroundService`a, viene cablata `AddHostedService`utilizzando.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-123">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="ac4ec-124">Poiché SignalR viene cablato anche durante la `Startup` fase, in cui ogni hub è collegato a un singolo endpoint nella pipeline di richieste http di ASP.NET Core, ogni hub è rappresentato da `IHubContext<T>` un nel server.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-124">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="ac4ec-125">Utilizzando le funzionalità di di ASP.NET Core, altre classi di cui è stata creata un'istanza `BackgroundService` dal livello di hosting, come le classi, le classi controller MVC o i modelli di pagine Razor, possono ottenere riferimenti `IHubContext<ClockHub, IClock>` a Hub sul lato server accettando istanze di durante la costruzione.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="ac4ec-126">Poiché il `ExecuteAsync` metodo viene chiamato in modo iterativo nel servizio in background, la data e l'ora correnti del server vengono inviate ai client connessi `ClockHub`tramite.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="ac4ec-127">Reagire agli eventi SignalR con servizi in background</span><span class="sxs-lookup"><span data-stu-id="ac4ec-127">React to SignalR events with background services</span></span>

<span data-ttu-id="ac4ec-128">Analogamente a un'app a pagina singola che usa il client JavaScript per SignalR o un'app desktop .NET <xref:signalr/dotnet-client>, può usare usando, un' `BackgroundService` implementazione di o `IHostedService` può essere usata anche per connettersi agli hub SignalR e rispondere agli eventi.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="ac4ec-129">La `ClockHubClient` classe implementa l' `IClock` interfaccia e l' `IHostedService` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="ac4ec-130">In questo modo è possibile cablare il `Startup` sistema durante l'esecuzione continua e rispondere agli eventi dell'hub dal server.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-130">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="ac4ec-131">Durante l' `ClockHubClient` inizializzazione, crea un'istanza di `HubConnection` un oggetto e collega `IClock.ShowTime` il `ShowTime` metodo come gestore per l'evento dell'hub.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="ac4ec-132">`IHostedService.StartAsync` Nell' implementazione`HubConnection` di viene avviato in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="ac4ec-133">Durante il `IHostedService.StopAsync` metodo, l' `HubConnection` oggetto viene eliminato in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="ac4ec-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="ac4ec-134">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ac4ec-134">Additional resources</span></span>

* [<span data-ttu-id="ac4ec-135">Introduzione</span><span class="sxs-lookup"><span data-stu-id="ac4ec-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="ac4ec-136">Hub</span><span class="sxs-lookup"><span data-stu-id="ac4ec-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ac4ec-137">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="ac4ec-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="ac4ec-138">Hub fortemente tipizzati</span><span class="sxs-lookup"><span data-stu-id="ac4ec-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
