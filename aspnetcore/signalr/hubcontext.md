---
title: SignalR HubContext
author: tdykstra
description: Informazioni su come usare il servizio ASP.NET Core SignalR HubContext per l'invio di notifiche ai client all'esterno di un hub.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/01/2018
uid: signalr/hubcontext
ms.openlocfilehash: ce350147e743df7f1671dd86da7c83f04bf0fe22
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569934"
---
# <a name="send-messages-from-outside-a-hub"></a>Invio di messaggi provenienti dall'esterno di un hub

Da [Mikael Mengistu](https://twitter.com/MikaelM_12)

L'hub SignalR è l'astrazione fondamentale per l'invio di messaggi ai client connessi al server di SignalR. È anche possibile inviare messaggi da altre posizioni all'interno dell'app tramite il `IHubContext` servizio. In questo articolo viene illustrato come accedere a un `IHubContext` di SignalR per inviare notifiche ai client all'esterno di un hub.

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(come scaricare)](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Ottenere un'istanza di IHubContext

In ASP.NET Core SignalR, è possibile accedere a un'istanza di `IHubContext` tramite inserimento delle dipendenze. È possibile inserire un'istanza di `IHubContext` in un controller, middleware o altro servizio di inserimento delle dipendenze. Usare l'istanza per inviare messaggi ai client.

> [!NOTE]
> Questo comportamento è diverso da ASP.NET 4.x SignalR che consente di fornire l'accesso a GlobalHost il `IHubContext`. ASP.NET Core ha un framework di dependency injection che elimina la necessità di questo singleton globale.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Inserire un'istanza di IHubContext in un controller

È possibile inserire un'istanza di `IHubContext` in un controller, aggiungendola al costruttore eventi:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

A questo punto, con l'accesso a un'istanza di `IHubContext`, è possibile chiamare i metodi dell'hub come se fossi nell'hub stesso.


[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Ottenere un'istanza di IHubContext in middleware

Accesso di `IHubContext` all'interno della pipeline middleware come illustrato di seguito:

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Quando i metodi dell'hub vengono chiamati all'esterno del `Hub` classe, non vi è alcun chiamante associati con la chiamata. Pertanto, non è possibile accedere per il `ConnectionId`, `Caller`, e `Others` proprietà.

### <a name="inject-a-strongly-typed-hubcontext"></a>Inserire un HubContext fortemente tipizzati

Per inserire un HubContext fortemente tipizzato, assicurarsi che l'Hub eredita da `Hub<T>`. Inserire usando il `IHubContext<THub, T>` interfaccia anziché `IHubContext<THub>`.

```csharp
public class ChatController : Controller
{
    public IHubContext<ChatHub, IChatClient> _strongChatHubContext { get; }

    public ChatController(IHubContext<ChatHub, IChatClient> chatHubContext)
    {
        _strongChatHubContext = chatHubContext;
    }

    public async Task SendMessage(string message)
    {
        await _strongChatHubContext.Clients.All.ReceiveMessage(message);
    }
}
```

## <a name="related-resources"></a>Risorse correlate

* [Introduzione](xref:tutorials/signalr)
* [Hub](xref:signalr/hubs)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
