---
title: Gestire utenti e gruppi in SignalR
author: rachelappel
description: Panoramica di ASP.NET Core SignalR utente e gruppo di gestione.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/groups
ms.openlocfilehash: 2a2f129863cf7d5cdfa3c0e5b2d901af52144671
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358439"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="49ace-103">Gestire utenti e gruppi in SignalR</span><span class="sxs-lookup"><span data-stu-id="49ace-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="49ace-104">Da [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="49ace-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="49ace-105">SignalR consente ai messaggi da inviare a tutte le connessioni associate a un utente specifico, nonché per gruppi di connessioni denominati.</span><span class="sxs-lookup"><span data-stu-id="49ace-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="49ace-106">[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="49ace-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="49ace-107">Utenti di SignalR</span><span class="sxs-lookup"><span data-stu-id="49ace-107">Users in SignalR</span></span>

<span data-ttu-id="49ace-108">SignalR consente di inviare messaggi a tutte le connessioni associate a un utente specifico.</span><span class="sxs-lookup"><span data-stu-id="49ace-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="49ace-109">Per impostazione predefinita SignalR utilizza il `ClaimTypes.NameIdentifier` dal `ClaimsPrincipal` associato alla connessione come l'identificatore utente.</span><span class="sxs-lookup"><span data-stu-id="49ace-109">By default SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="49ace-110">Un singolo utente può avere più connessioni a un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="49ace-110">A single user can have multiple connections to a SignalR application.</span></span> <span data-ttu-id="49ace-111">Ad esempio, un utente potrebbe essere connesso sul desktop, nonché il proprio telefono.</span><span class="sxs-lookup"><span data-stu-id="49ace-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="49ace-112">Ogni dispositivo presenta una connessione SignalR separata, ma sono tutti associate con lo stesso utente.</span><span class="sxs-lookup"><span data-stu-id="49ace-112">Each device has a separate SignalR connection, but they are all associated with the same user.</span></span> <span data-ttu-id="49ace-113">Se un messaggio viene inviato all'utente, tutte le connessioni associate all'utente verrà visualizzato il messaggio.</span><span class="sxs-lookup"><span data-stu-id="49ace-113">If a message is sent to the user, all of the connections associated with that user will receive the message.</span></span>

<span data-ttu-id="49ace-114">Inviare un messaggio a un utente specifico passando l'identificatore utente per il `User` funzione al metodo dell'hub, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="49ace-114">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="49ace-115">L'identificatore utente è distinzione maiuscole/minuscole.</span><span class="sxs-lookup"><span data-stu-id="49ace-115">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="49ace-116">L'identificatore utente può essere personalizzato tramite la creazione di un `IUserIdProvider`e registrandolo in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="49ace-116">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="49ace-117">AddSignalR deve essere chiamato prima di registrare i servizi SignalR personalizzati.</span><span class="sxs-lookup"><span data-stu-id="49ace-117">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="49ace-118">Gruppi di SignalR</span><span class="sxs-lookup"><span data-stu-id="49ace-118">Groups in SignalR</span></span>

<span data-ttu-id="49ace-119">Un gruppo è una raccolta di connessioni associate con un nome.</span><span class="sxs-lookup"><span data-stu-id="49ace-119">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="49ace-120">Messaggi possono essere inviati a tutte le connessioni in un gruppo.</span><span class="sxs-lookup"><span data-stu-id="49ace-120">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="49ace-121">I gruppi sono il modo consigliato per inviare una o più connessioni poiché i gruppi vengono gestiti dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="49ace-121">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="49ace-122">Una connessione può essere un membro di più gruppi.</span><span class="sxs-lookup"><span data-stu-id="49ace-122">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="49ace-123">In questo modo gruppi ideale per simile a un'applicazione di chat, in cui ogni stanza può essere rappresentato come un gruppo.</span><span class="sxs-lookup"><span data-stu-id="49ace-123">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="49ace-124">Le connessioni possono essere aggiunto o rimosso dai gruppi tramite il `AddToGroupAsync` e `RemoveFromGroupAsync` metodi.</span><span class="sxs-lookup"><span data-stu-id="49ace-124">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="49ace-125">L'appartenenza al gruppo non viene mantenuto quando viene ristabilita una connessione.</span><span class="sxs-lookup"><span data-stu-id="49ace-125">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="49ace-126">È necessario partecipare di nuovo il gruppo di quando viene ristabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="49ace-126">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="49ace-127">Non è possibile contare i membri di un gruppo, poiché queste informazioni non sono disponibile se l'applicazione viene ridimensionato in base a più server.</span><span class="sxs-lookup"><span data-stu-id="49ace-127">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="49ace-128">I nomi dei gruppi sono distinzione maiuscole/minuscole.</span><span class="sxs-lookup"><span data-stu-id="49ace-128">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="49ace-129">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="49ace-129">Related resources</span></span>

* [<span data-ttu-id="49ace-130">Introduzione</span><span class="sxs-lookup"><span data-stu-id="49ace-130">Get started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="49ace-131">Hub</span><span class="sxs-lookup"><span data-stu-id="49ace-131">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="49ace-132">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="49ace-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
