---
title: SignalR di ASP.NET Core host nei servizi in background
author: bradygaster
description: Informazioni su come inviare messaggi ai client SignalR dalle classi BackgroundService di .NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/background-services
ms.openlocfilehash: 000732115153eeafed3948c2a07acf77ffc34218
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73964051"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a><span data-ttu-id="b1264-103">SignalR di ASP.NET Core host nei servizi in background</span><span class="sxs-lookup"><span data-stu-id="b1264-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="b1264-104">Di [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="b1264-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="b1264-105">Questo articolo fornisce indicazioni per:</span><span class="sxs-lookup"><span data-stu-id="b1264-105">This article provides guidance for:</span></span>

* <span data-ttu-id="b1264-106">Hosting di hub SignalR usando un processo di lavoro in background ospitato con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1264-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="b1264-107">Invio di messaggi ai client connessi dall'interno di un [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1264-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="b1264-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(procedura per il download)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="b1264-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-opno-locsignalr-during-startup"></a><span data-ttu-id="b1264-109">Collegare SignalR durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="b1264-109">Wire up SignalR during startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b1264-110">L'hosting di ASP.NET Core Hub SignalR nel contesto di un processo di lavoro in background è identico all'hosting di un hub in un'app Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1264-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="b1264-111">Nel metodo `Startup.ConfigureServices` la chiamata di `services.AddSignalR` aggiunge i servizi richiesti al livello di inserimento delle dipendenze (DI) ASP.NET Core per supportare SignalR.</span><span class="sxs-lookup"><span data-stu-id="b1264-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="b1264-112">In `Startup.Configure`, il metodo `MapHub` viene chiamato nel callback `UseEndpoints` per collegare gli endpoint dell'hub nella pipeline della richiesta ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1264-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

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

<span data-ttu-id="b1264-113">L'hosting di ASP.NET Core Hub SignalR nel contesto di un processo di lavoro in background è identico all'hosting di un hub in un'app Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1264-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="b1264-114">Nel metodo `Startup.ConfigureServices` la chiamata di `services.AddSignalR` aggiunge i servizi richiesti al livello di inserimento delle dipendenze (DI) ASP.NET Core per supportare SignalR.</span><span class="sxs-lookup"><span data-stu-id="b1264-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="b1264-115">In `Startup.Configure`, viene chiamato il metodo `UseSignalR` per collegare gli endpoint dell'hub nella pipeline della richiesta di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1264-115">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="b1264-116">Nell'esempio precedente, la classe `ClockHub` implementa la classe `Hub<T>` per creare un hub fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="b1264-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="b1264-117">Il `ClockHub` è stato configurato nella classe `Startup` per rispondere alle richieste nell'endpoint `/hubs/clock`.</span><span class="sxs-lookup"><span data-stu-id="b1264-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="b1264-118">Per altre informazioni sugli hub fortemente tipizzati, vedere [usare hub in SignalR per ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="b1264-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="b1264-119">Questa funzionalità non è limitata alla classe dell' [Hub\<t >](xref:Microsoft.AspNetCore.SignalR.Hub`1) .</span><span class="sxs-lookup"><span data-stu-id="b1264-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="b1264-120">Anche tutte le classi che ereditano dall' [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), ad esempio [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), funzioneranno.</span><span class="sxs-lookup"><span data-stu-id="b1264-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="b1264-121">L'interfaccia utilizzata dalla `ClockHub` fortemente tipizzata è l'interfaccia `IClock`.</span><span class="sxs-lookup"><span data-stu-id="b1264-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a><span data-ttu-id="b1264-122">Chiamare un hub SignalR da un servizio in background</span><span class="sxs-lookup"><span data-stu-id="b1264-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="b1264-123">Durante l'avvio, la classe `Worker`, una `BackgroundService`, viene cablata utilizzando `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="b1264-123">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="b1264-124">Poiché SignalR viene cablato anche durante la fase di `Startup`, in cui ogni hub è collegato a un singolo endpoint nella pipeline di richieste HTTP ASP.NET Core, ogni hub è rappresentato da un `IHubContext<T>` sul server.</span><span class="sxs-lookup"><span data-stu-id="b1264-124">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="b1264-125">Utilizzando le funzionalità DI DI ASP.NET Core, altre classi create dal livello host, ad esempio classi `BackgroundService`, classi controller MVC o modelli di pagine Razor, possono ottenere riferimenti a Hub sul lato server accettando istanze di `IHubContext<ClockHub, IClock>` durante la costruzione.</span><span class="sxs-lookup"><span data-stu-id="b1264-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="b1264-126">Poiché il metodo `ExecuteAsync` viene chiamato in modo iterativo nel servizio in background, la data e l'ora correnti del server vengono inviate ai client connessi usando il `ClockHub`.</span><span class="sxs-lookup"><span data-stu-id="b1264-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-opno-locsignalr-events-with-background-services"></a><span data-ttu-id="b1264-127">Reagire agli eventi SignalR con i servizi in background</span><span class="sxs-lookup"><span data-stu-id="b1264-127">React to SignalR events with background services</span></span>

<span data-ttu-id="b1264-128">Analogamente a un'app a pagina singola che usa il client JavaScript per SignalR o un'app desktop .NET può eseguire usando il <xref:signalr/dotnet-client>, è possibile usare un'implementazione di `BackgroundService` o `IHostedService` anche per connettersi a hub SignalR e rispondere agli eventi.</span><span class="sxs-lookup"><span data-stu-id="b1264-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="b1264-129">La classe `ClockHubClient` implementa sia l'interfaccia `IClock` che l'interfaccia `IHostedService`.</span><span class="sxs-lookup"><span data-stu-id="b1264-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="b1264-130">In questo modo è possibile cablare il sistema durante `Startup` per l'esecuzione continua e rispondere agli eventi dell'hub dal server.</span><span class="sxs-lookup"><span data-stu-id="b1264-130">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="b1264-131">Durante l'inizializzazione, il `ClockHubClient` crea un'istanza di un `HubConnection` e collega il metodo `IClock.ShowTime` come gestore per l'evento `ShowTime` dell'hub.</span><span class="sxs-lookup"><span data-stu-id="b1264-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="b1264-132">Nell'implementazione `IHostedService.StartAsync` il `HubConnection` viene avviato in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="b1264-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="b1264-133">Durante il `IHostedService.StopAsync` metodo, il `HubConnection` viene eliminato in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="b1264-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="b1264-134">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b1264-134">Additional resources</span></span>

* [<span data-ttu-id="b1264-135">Introduzione</span><span class="sxs-lookup"><span data-stu-id="b1264-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="b1264-136">Hub</span><span class="sxs-lookup"><span data-stu-id="b1264-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b1264-137">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="b1264-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="b1264-138">Hub fortemente tipizzati</span><span class="sxs-lookup"><span data-stu-id="b1264-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
