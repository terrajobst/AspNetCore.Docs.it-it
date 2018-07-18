---
title: SignalR HubContext
author: rachelappel
description: Imparare a usare il servizio ASP.NET Core SignalR HubContext per l'invio di notifiche ai client all'esterno di un hub.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: ccfcdc8337275fd26e09c1a43db36cf9ab90cf46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277761"
---
# <a name="send-messages-from-outside-a-hub"></a>Invio di messaggi provenienti dall'esterno di un hub

Da [Mikael Mengistu](https://twitter.com/MikaelM_12)

L'hub SignalR è l'astrazione fondamentale per l'invio di messaggi ai client connessi al server di SignalR. È anche possibile inviare messaggi da altre posizioni all'interno dell'app tramite il IHubContext servizio. In questo articolo viene illustrato come accedere a un `IHubContext` di SignalR per inviare notifiche ai client all'esterno di un hub.

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Ottenere un'istanza di `IHubContext`

In ASP.NET SignalR Core, è possibile accedere a un'istanza di `IHubContext` tramite l'inserimento di dipendenze. È possibile inserire un'istanza di `IHubContext` in un controller, middleware o altro servizio (DISCONNECTED Inbound). Utilizzare l'istanza per inviare messaggi al client.

> [!NOTE]
> Questo comportamento è diverso da ASP.NET SignalR che utilizza GlobalHost per fornire l'accesso all'`IHubContext`. ASP.NET Core ha un framework di dependency injection che elimina la necessità di questo singleton globale.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Inserire un'istanza di `IHubContext` in un controller

È possibile inserire un'istanza di `IHubContext` in un controller, aggiungendola al costruttore eventi:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

A questo punto, con l'accesso a un'istanza di `IHubContext`, è possibile chiamare i metodi dell'hub come se fossi nell'hub stesso.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Ottenere un'istanza di `IHubContext` nel middleware

L'accesso di `IHubContext` all'interno della pipeline middleware come illustrato di seguito:

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Quando vengono chiamati i metodi dell'hub all'esterno del `Hub` classe, non è disponibile alcun chiamante associato alla chiamata. Pertanto, non è possibile accedere per il `ConnectionId`, `Caller`, e `Others` proprietà.

## <a name="related-resources"></a>Risorse correlate

* [Introduzione](xref:tutorials/signalr)
* [Hub](xref:signalr/hubs)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
