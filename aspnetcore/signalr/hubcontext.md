---
title: SignalR HubContext
author: tdykstra
description: Informazioni su come usare il servizio ASP.NET Core SignalR HubContext per l'invio di notifiche ai client all'esterno di un hub.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: 6b955c2064d7d6a045594e56326e2f7df282675f
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095307"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="00c98-103">Invio di messaggi provenienti dall'esterno di un hub</span><span class="sxs-lookup"><span data-stu-id="00c98-103">Send messages from outside a hub</span></span>

<span data-ttu-id="00c98-104">Da [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="00c98-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="00c98-105">L'hub SignalR è l'astrazione fondamentale per l'invio di messaggi ai client connessi al server di SignalR.</span><span class="sxs-lookup"><span data-stu-id="00c98-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="00c98-106">È anche possibile inviare messaggi da altre posizioni all'interno dell'app tramite il `IHubContext` servizio.</span><span class="sxs-lookup"><span data-stu-id="00c98-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="00c98-107">Questo articolo illustra come accedere a un SignalR `IHubContext` per inviare notifiche ai client all'esterno di un hub.</span><span class="sxs-lookup"><span data-stu-id="00c98-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="00c98-108">[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="00c98-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="00c98-109">Ottenere un'istanza di `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="00c98-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="00c98-110">In ASP.NET Core SignalR, è possibile accedere a un'istanza di `IHubContext` tramite inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="00c98-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="00c98-111">È possibile inserire un'istanza di `IHubContext` in un controller, middleware o altro servizio di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="00c98-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="00c98-112">Usare l'istanza per inviare messaggi ai client.</span><span class="sxs-lookup"><span data-stu-id="00c98-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="00c98-113">Questo comportamento è diverso da SignalR ASP.NET che consente di fornire l'accesso a GlobalHost il `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="00c98-113">This differs from ASP.NET SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="00c98-114">ASP.NET Core include un framework di inserimento delle dipendenze che elimina la necessità di questo singleton globale.</span><span class="sxs-lookup"><span data-stu-id="00c98-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="00c98-115">Inserire un'istanza di `IHubContext` in un controller</span><span class="sxs-lookup"><span data-stu-id="00c98-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="00c98-116">È possibile inserire un'istanza di `IHubContext` in un controller, aggiungerlo al costruttore:</span><span class="sxs-lookup"><span data-stu-id="00c98-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="00c98-117">A questo punto, con accesso a un'istanza di `IHubContext`, è possibile chiamare i metodi dell'hub come se fossero nell'hub stesso.</span><span class="sxs-lookup"><span data-stu-id="00c98-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="00c98-118">Ottenere un'istanza di `IHubContext` nel middleware</span><span class="sxs-lookup"><span data-stu-id="00c98-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="00c98-119">Accesso di `IHubContext` all'interno della pipeline middleware come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="00c98-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

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
> <span data-ttu-id="00c98-120">Quando i metodi dell'hub vengono chiamati all'esterno del `Hub` classe, non vi è alcun chiamante associati con la chiamata.</span><span class="sxs-lookup"><span data-stu-id="00c98-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="00c98-121">Pertanto, non è possibile accedere per il `ConnectionId`, `Caller`, e `Others` proprietà.</span><span class="sxs-lookup"><span data-stu-id="00c98-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="00c98-122">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="00c98-122">Related resources</span></span>

* [<span data-ttu-id="00c98-123">Introduzione</span><span class="sxs-lookup"><span data-stu-id="00c98-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="00c98-124">Hub</span><span class="sxs-lookup"><span data-stu-id="00c98-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="00c98-125">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="00c98-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
