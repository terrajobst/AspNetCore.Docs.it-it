---
title: Host ASP.NET Core SignalR in servizi in background
author: bradygaster
description: Informazioni su come inviare messaggi ai client SignalR dalle classi di .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: dcd62f0c7056a3f987291b6c8bb8b87f94160865
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087774"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="7672c-103">Host ASP.NET Core SignalR in servizi in background</span><span class="sxs-lookup"><span data-stu-id="7672c-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="7672c-104">Da [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="7672c-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="7672c-105">Questo articolo fornisce indicazioni per:</span><span class="sxs-lookup"><span data-stu-id="7672c-105">This article provides guidance for:</span></span>

* <span data-ttu-id="7672c-106">Hosting SignalR Hubs usando un processo di lavoro in background ospitato con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7672c-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="7672c-107">L'invio di messaggi al client all'interno di un .NET Core connected [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span><span class="sxs-lookup"><span data-stu-id="7672c-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="7672c-108">[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(come scaricare)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="7672c-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="7672c-109">Durante l'avvio di rete di SignalR</span><span class="sxs-lookup"><span data-stu-id="7672c-109">Wire up SignalR during startup</span></span>

<span data-ttu-id="7672c-110">L'hosting di ASP.NET Core SignalR Hubs nel contesto di un processo di lavoro in background è identico all'hosting di un Hub in un'app web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7672c-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="7672c-111">Nel `Startup.ConfigureServices` metodo, chiamare il metodo `services.AddSignalR` aggiunge i servizi necessari per il livello di ASP.NET Core inserimento delle dipendenze per il supporto di SignalR.</span><span class="sxs-lookup"><span data-stu-id="7672c-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="7672c-112">Nelle `Startup.Configure`, il `UseSignalR` viene chiamato il metodo consente di associare gli endpoint dell'Hub nella pipeline delle richieste ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7672c-112">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

<span data-ttu-id="7672c-113">Nell'esempio precedente, il `ClockHub` classe implementa il `Hub<T>` classe per creare un Hub fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="7672c-113">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="7672c-114">Il `ClockHub` è stata configurata nel `Startup` classi di rispondere alle richieste all'endpoint `/hubs/clock`.</span><span class="sxs-lookup"><span data-stu-id="7672c-114">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="7672c-115">Per altre informazioni sugli hub fortemente tipizzato, vedere [usare gli hub di SignalR per ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="7672c-115">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="7672c-116">Questa funzionalità non è limitata per i [Hub\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) classe.</span><span class="sxs-lookup"><span data-stu-id="7672c-116">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="7672c-117">Qualsiasi classe che eredita da [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), ad esempio [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), funzionerà anche.</span><span class="sxs-lookup"><span data-stu-id="7672c-117">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="7672c-118">L'interfaccia utilizzata per l'oggetto fortemente tipizzato `ClockHub` è il `IClock` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="7672c-118">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="7672c-119">Chiamare un SignalR Hub da un servizio in background</span><span class="sxs-lookup"><span data-stu-id="7672c-119">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="7672c-120">Durante l'avvio, il `Worker` (classe), una `BackgroundService`, sono stati evidenziati usando `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="7672c-120">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="7672c-121">Poiché SignalR è collegato anche durante la `Startup` fase, in cui ogni Hub è associata a un singolo endpoint nella pipeline di richieste HTTP di ASP.NET Core, ogni Hub è rappresentato da un `IHubContext<T>` nel server.</span><span class="sxs-lookup"><span data-stu-id="7672c-121">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="7672c-122">Usando ASP.NET Core include l'inserimento delle dipendenze, le altre classi creare un'istanza dal livello di hosting, ad esempio `BackgroundService` classi, le classi Controller MVC o modelli di pagina Razor, possono ottenere riferimenti a hub sul lato server mediante l'accettazione di istanze di `IHubContext<ClockHub, IClock>` durante la costruzione.</span><span class="sxs-lookup"><span data-stu-id="7672c-122">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="7672c-123">Come le `ExecuteAsync` metodo viene chiamato in modo iterativo nel servizio in background, il server Data e ora correnti vengono inviati ai client connessi tramite il `ClockHub`.</span><span class="sxs-lookup"><span data-stu-id="7672c-123">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="7672c-124">Reagire agli eventi di SignalR con i servizi in background</span><span class="sxs-lookup"><span data-stu-id="7672c-124">React to SignalR events with background services</span></span>

<span data-ttu-id="7672c-125">Ad esempio un'App a singola pagina tramite il client JavaScript per SignalR o un'app desktop .NET è possibile eseguire tramite l'utilizzando la <xref:signalr/dotnet-client>, una `BackgroundService` o `IHostedService` implementazione può anche essere utilizzata per connettersi a un hub SignalR e rispondere agli eventi.</span><span class="sxs-lookup"><span data-stu-id="7672c-125">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="7672c-126">Il `ClockHubClient` classe implementa sia il `IClock` interfaccia e `IHostedService` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="7672c-126">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="7672c-127">In questo modo può essere cablata backup durante `Startup` eseguiti in modo continuo e rispondere agli eventi dell'Hub dal server.</span><span class="sxs-lookup"><span data-stu-id="7672c-127">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span> 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="7672c-128">Durante l'inizializzazione, il `ClockHubClient` crea un'istanza di un `HubConnection` e collega le `IClock.ShowTime` metodo del gestore per l'Hub `ShowTime` evento.</span><span class="sxs-lookup"><span data-stu-id="7672c-128">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="7672c-129">Nel `IHostedService.StartAsync` implementazione di `HubConnection` viene avviato in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="7672c-129">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="7672c-130">Durante la `IHostedService.StopAsync` metodo, il `HubConnection` viene eliminato in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="7672c-130">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="7672c-131">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7672c-131">Additional resources</span></span>

* [<span data-ttu-id="7672c-132">Introduzione</span><span class="sxs-lookup"><span data-stu-id="7672c-132">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="7672c-133">Hub</span><span class="sxs-lookup"><span data-stu-id="7672c-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="7672c-134">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="7672c-134">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="7672c-135">Hub fortemente tipizzato</span><span class="sxs-lookup"><span data-stu-id="7672c-135">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
