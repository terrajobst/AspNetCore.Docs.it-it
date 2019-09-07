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
# <a name="host-aspnet-core-signalr-in-background-services"></a>Host ASP.NET Core SignalR in servizi in background

Di [Brady Gaster](https://twitter.com/bradygaster)

Questo articolo fornisce indicazioni per:

* Ospitare Hub SignalR usando un processo di lavoro in background ospitato con ASP.NET Core.
* Invio di messaggi ai client connessi dall'interno di un [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)di .NET Core.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(come scaricare)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>Cablare SignalR durante l'avvio

::: moniker range=">= aspnetcore-3.0"

L'hosting di ASP.NET Core Hub SignalR nel contesto di un processo di lavoro in background è identico all'hosting di un hub in un'app Web ASP.NET Core. Nel metodo, la chiamata `services.AddSignalR` di aggiunge i servizi richiesti al livello di inserimento delle dipendenze di ASP.NET Core per supportare SignalR. `Startup.ConfigureServices` In `Startup.Configure`il metodovienechiamatonelcallbackpercollegaregliendpointdell'hubnellapipelinedellarichiestaASP.NETCore.`MapHub` `UseEndpoints`

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

L'hosting di ASP.NET Core Hub SignalR nel contesto di un processo di lavoro in background è identico all'hosting di un hub in un'app Web ASP.NET Core. Nel metodo, la chiamata `services.AddSignalR` di aggiunge i servizi richiesti al livello di inserimento delle dipendenze di ASP.NET Core per supportare SignalR. `Startup.ConfigureServices` In `Startup.Configure`viene chiamato `UseSignalR` il metodo per collegare gli endpoint dell'hub nella pipeline della richiesta ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

Nell'esempio precedente, la `ClockHub` classe implementa la `Hub<T>` classe per creare un hub fortemente tipizzato. È stato configurato `Startup` nella classe per rispondere alle richieste all'endpoint `/hubs/clock`. `ClockHub`

Per altre informazioni sugli hub fortemente tipizzati, vedere [usare gli hub in SignalR per ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Questa funzionalità non è limitata alla classe di [\<> Hub t](xref:Microsoft.AspNetCore.SignalR.Hub`1) . Anche tutte le classi che ereditano dall' [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), ad esempio [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), funzioneranno.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

L'interfaccia utilizzata dalla fortemente tipizzata `ClockHub` è l' `IClock` interfaccia.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Chiamare un hub SignalR da un servizio in background

Durante l'avvio, `Worker` la classe, `BackgroundService`a, viene cablata `AddHostedService`utilizzando.

```csharp
services.AddHostedService<Worker>();
```

Poiché SignalR viene cablato anche durante la `Startup` fase, in cui ogni hub è collegato a un singolo endpoint nella pipeline di richieste http di ASP.NET Core, ogni hub è rappresentato da `IHubContext<T>` un nel server. Utilizzando le funzionalità di di ASP.NET Core, altre classi di cui è stata creata un'istanza `BackgroundService` dal livello di hosting, come le classi, le classi controller MVC o i modelli di pagine Razor, possono ottenere riferimenti `IHubContext<ClockHub, IClock>` a Hub sul lato server accettando istanze di durante la costruzione.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Poiché il `ExecuteAsync` metodo viene chiamato in modo iterativo nel servizio in background, la data e l'ora correnti del server vengono inviate ai client connessi `ClockHub`tramite.

## <a name="react-to-signalr-events-with-background-services"></a>Reagire agli eventi SignalR con servizi in background

Analogamente a un'app a pagina singola che usa il client JavaScript per SignalR o un'app desktop .NET <xref:signalr/dotnet-client>, può usare usando, un' `BackgroundService` implementazione di o `IHostedService` può essere usata anche per connettersi agli hub SignalR e rispondere agli eventi.

La `ClockHubClient` classe implementa l' `IClock` interfaccia e l' `IHostedService` interfaccia. In questo modo è possibile cablare il `Startup` sistema durante l'esecuzione continua e rispondere agli eventi dell'hub dal server.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Durante l' `ClockHubClient` inizializzazione, crea un'istanza di `HubConnection` un oggetto e collega `IClock.ShowTime` il `ShowTime` metodo come gestore per l'evento dell'hub.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

`IHostedService.StartAsync` Nell' implementazione`HubConnection` di viene avviato in modo asincrono.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Durante il `IHostedService.StopAsync` metodo, l' `HubConnection` oggetto viene eliminato in modo asincrono.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Introduzione](xref:tutorials/signalr)
* [Hub](xref:signalr/hubs)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
* [Hub fortemente tipizzati](xref:signalr/hubs#strongly-typed-hubs)
