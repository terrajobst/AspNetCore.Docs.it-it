---
title: Host ASP.NET Core SignalR in servizi in background
author: bradygaster
description: Informazioni su come inviare messaggi ai client SignalR dalle classi di .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: b359bd7f6b0667aeb8d9c8f5eb450637b1347b19
ms.sourcegitcommit: e418cb9cddeb3de06fa0cb4fdb5529da03ff6d63
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739670"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>Host ASP.NET Core SignalR in servizi in background

Da [Brady Gaster](https://twitter.com/bradygaster)

Questo articolo fornisce indicazioni per:

* Hosting SignalR Hubs usando un processo di lavoro in background ospitato con ASP.NET Core.
* L'invio di messaggi al client all'interno di un .NET Core connected [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(come scaricare)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>Durante l'avvio di rete di SignalR

L'hosting di ASP.NET Core SignalR Hubs nel contesto di un processo di lavoro in background è identico all'hosting di un Hub in un'app web ASP.NET Core. Nel `Startup.ConfigureServices` metodo, chiamare il metodo `services.AddSignalR` aggiunge i servizi necessari per il livello di ASP.NET Core inserimento delle dipendenze per il supporto di SignalR. Nelle `Startup.Configure`, il `UseSignalR` viene chiamato il metodo consente di associare gli endpoint dell'Hub nella pipeline delle richieste ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

Nell'esempio precedente, il `ClockHub` classe implementa il `Hub<T>` classe per creare un Hub fortemente tipizzato. Il `ClockHub` è stata configurata nel `Startup` classi di rispondere alle richieste all'endpoint `/hubs/clock`.

Per altre informazioni sugli hub fortemente tipizzato, vedere [usare gli hub di SignalR per ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Questa funzionalità non è limitata per i [Hub\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) classe. Qualsiasi classe che eredita da [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), ad esempio [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), funzionerà anche.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

L'interfaccia utilizzata per l'oggetto fortemente tipizzato `ClockHub` è il `IClock` interfaccia.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Chiamare un SignalR Hub da un servizio in background

Durante l'avvio, il `Worker` (classe), una `BackgroundService`, sono stati evidenziati usando `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Poiché SignalR è collegato anche durante la `Startup` fase, in cui ogni Hub è associata a un singolo endpoint nella pipeline di richieste HTTP di ASP.NET Core, ogni Hub è rappresentato da un `IHubContext<T>` nel server. Usando ASP.NET Core include l'inserimento delle dipendenze, le altre classi creare un'istanza dal livello di hosting, ad esempio `BackgroundService` classi, le classi Controller MVC o modelli di pagina Razor, possono ottenere riferimenti a hub sul lato server mediante l'accettazione di istanze di `IHubContext<ClockHub, IClock>` durante la costruzione.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Come le `ExecuteAsync` metodo viene chiamato in modo iterativo nel servizio in background, il server Data e ora correnti vengono inviati ai client connessi tramite il `ClockHub`.

## <a name="react-to-signalr-events-with-background-services"></a>Reagire agli eventi di SignalR con i servizi in background

Ad esempio un'App a singola pagina tramite il client JavaScript per SignalR o un'app desktop .NET è possibile eseguire tramite l'utilizzando la <xref:signalr/dotnet-client>, una `BackgroundService` o `IHostedService` implementazione può anche essere utilizzata per connettersi a un hub SignalR e rispondere agli eventi.

Il `ClockHubClient` classe implementa sia il `IClock` interfaccia e `IHostedService` interfaccia. In questo modo può essere cablata backup durante `Startup` eseguiti in modo continuo e rispondere agli eventi dell'Hub dal server. 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Durante l'inizializzazione, il `ClockHubClient` crea un'istanza di un `HubConnection` e collega le `IClock.ShowTime` metodo del gestore per l'Hub `ShowTime` evento.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

Nel `IHostedService.StartAsync` implementazione di `HubConnection` viene avviato in modo asincrono.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Durante la `IHostedService.StopAsync` metodo, il `HubConnection` viene eliminato in modo asincrono.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Introduzione](xref:tutorials/signalr)
* [Hub](xref:signalr/hubs)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
* [Hub fortemente tipizzato](xref:signalr/hubs#strongly-typed-hubs)
