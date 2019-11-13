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
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a>SignalR di ASP.NET Core host nei servizi in background

Di [Brady Gaster](https://twitter.com/bradygaster)

Questo articolo fornisce indicazioni per:

* Hosting di hub SignalR usando un processo di lavoro in background ospitato con ASP.NET Core.
* Invio di messaggi ai client connessi dall'interno di un [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)di .NET Core.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(procedura per il download)](xref:index#how-to-download-a-sample)

## <a name="wire-up-opno-locsignalr-during-startup"></a>Collegare SignalR durante l'avvio

::: moniker range=">= aspnetcore-3.0"

L'hosting di ASP.NET Core Hub SignalR nel contesto di un processo di lavoro in background è identico all'hosting di un hub in un'app Web di ASP.NET Core. Nel metodo `Startup.ConfigureServices` la chiamata di `services.AddSignalR` aggiunge i servizi richiesti al livello di inserimento delle dipendenze (DI) ASP.NET Core per supportare SignalR. In `Startup.Configure`, il metodo `MapHub` viene chiamato nel callback `UseEndpoints` per collegare gli endpoint dell'hub nella pipeline della richiesta ASP.NET Core.

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

L'hosting di ASP.NET Core Hub SignalR nel contesto di un processo di lavoro in background è identico all'hosting di un hub in un'app Web di ASP.NET Core. Nel metodo `Startup.ConfigureServices` la chiamata di `services.AddSignalR` aggiunge i servizi richiesti al livello di inserimento delle dipendenze (DI) ASP.NET Core per supportare SignalR. In `Startup.Configure`, viene chiamato il metodo `UseSignalR` per collegare gli endpoint dell'hub nella pipeline della richiesta di ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

Nell'esempio precedente, la classe `ClockHub` implementa la classe `Hub<T>` per creare un hub fortemente tipizzato. Il `ClockHub` è stato configurato nella classe `Startup` per rispondere alle richieste nell'endpoint `/hubs/clock`.

Per altre informazioni sugli hub fortemente tipizzati, vedere [usare hub in SignalR per ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Questa funzionalità non è limitata alla classe dell' [Hub\<t >](xref:Microsoft.AspNetCore.SignalR.Hub`1) . Anche tutte le classi che ereditano dall' [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), ad esempio [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), funzioneranno.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

L'interfaccia utilizzata dalla `ClockHub` fortemente tipizzata è l'interfaccia `IClock`.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a>Chiamare un hub SignalR da un servizio in background

Durante l'avvio, la classe `Worker`, una `BackgroundService`, viene cablata utilizzando `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Poiché SignalR viene cablato anche durante la fase di `Startup`, in cui ogni hub è collegato a un singolo endpoint nella pipeline di richieste HTTP ASP.NET Core, ogni hub è rappresentato da un `IHubContext<T>` sul server. Utilizzando le funzionalità DI DI ASP.NET Core, altre classi create dal livello host, ad esempio classi `BackgroundService`, classi controller MVC o modelli di pagine Razor, possono ottenere riferimenti a Hub sul lato server accettando istanze di `IHubContext<ClockHub, IClock>` durante la costruzione.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Poiché il metodo `ExecuteAsync` viene chiamato in modo iterativo nel servizio in background, la data e l'ora correnti del server vengono inviate ai client connessi usando il `ClockHub`.

## <a name="react-to-opno-locsignalr-events-with-background-services"></a>Reagire agli eventi SignalR con i servizi in background

Analogamente a un'app a pagina singola che usa il client JavaScript per SignalR o un'app desktop .NET può eseguire usando il <xref:signalr/dotnet-client>, è possibile usare un'implementazione di `BackgroundService` o `IHostedService` anche per connettersi a hub SignalR e rispondere agli eventi.

La classe `ClockHubClient` implementa sia l'interfaccia `IClock` che l'interfaccia `IHostedService`. In questo modo è possibile cablare il sistema durante `Startup` per l'esecuzione continua e rispondere agli eventi dell'hub dal server.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Durante l'inizializzazione, il `ClockHubClient` crea un'istanza di un `HubConnection` e collega il metodo `IClock.ShowTime` come gestore per l'evento `ShowTime` dell'hub.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

Nell'implementazione `IHostedService.StartAsync` il `HubConnection` viene avviato in modo asincrono.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Durante il `IHostedService.StopAsync` metodo, il `HubConnection` viene eliminato in modo asincrono.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Introduzione](xref:tutorials/signalr)
* [Hub](xref:signalr/hubs)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
* [Hub fortemente tipizzati](xref:signalr/hubs#strongly-typed-hubs)
