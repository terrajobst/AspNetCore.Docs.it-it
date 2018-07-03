---
title: Gestire utenti e gruppi in SignalR
author: rachelappel
description: Panoramica di ASP.NET Core SignalR utente e gruppo di gestione.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 3e5e310c84bc3ed5790d5b67a917bd54162ea163
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402525"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="74304-103">Gestire utenti e gruppi in SignalR</span><span class="sxs-lookup"><span data-stu-id="74304-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="74304-104">Da [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="74304-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="74304-105">SignalR consente ai messaggi da inviare a tutte le connessioni associate a un utente specifico, nonché per gruppi di connessioni con nome.</span><span class="sxs-lookup"><span data-stu-id="74304-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="74304-106">[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="74304-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="74304-107">Utenti di SignalR</span><span class="sxs-lookup"><span data-stu-id="74304-107">Users in SignalR</span></span>

<span data-ttu-id="74304-108">SignalR consente di inviare messaggi a tutte le connessioni associate a un utente specifico.</span><span class="sxs-lookup"><span data-stu-id="74304-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="74304-109">Per impostazione predefinita, SignalR utilizza il `ClaimTypes.NameIdentifier` dal `ClaimsPrincipal` associato alla connessione come identificatore dell'utente.</span><span class="sxs-lookup"><span data-stu-id="74304-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="74304-110">Un singolo utente può avere più connessioni a un'app per SignalR.</span><span class="sxs-lookup"><span data-stu-id="74304-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="74304-111">Ad esempio, un utente potrebbe essere connesso sul desktop, nonché il telefono.</span><span class="sxs-lookup"><span data-stu-id="74304-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="74304-112">Ogni dispositivo ha una connessione SignalR separata, ma sono tutte associate con lo stesso utente.</span><span class="sxs-lookup"><span data-stu-id="74304-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="74304-113">Se un messaggio viene inviato all'utente, tutte le connessioni associate all'utente di ricevere il messaggio.</span><span class="sxs-lookup"><span data-stu-id="74304-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="74304-114">L'identificatore utente per una connessione sono accessibili dal `Context.UserIdentifier` proprietà nell'hub.</span><span class="sxs-lookup"><span data-stu-id="74304-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="74304-115">Inviare un messaggio a un utente specifico passando l'identificatore utente per il `User` funzionare nel metodo dell'hub, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="74304-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="74304-116">L'identificatore utente è tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="74304-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="74304-117">L'identificatore utente può essere personalizzato tramite la creazione di un' `IUserIdProvider`e la relativa registrazione nel `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="74304-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="74304-118">AddSignalR deve essere chiamato prima di registrare i servizi SignalR personalizzati.</span><span class="sxs-lookup"><span data-stu-id="74304-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="74304-119">Gruppi in SignalR</span><span class="sxs-lookup"><span data-stu-id="74304-119">Groups in SignalR</span></span>

<span data-ttu-id="74304-120">Un gruppo è una raccolta di connessioni associate a un nome.</span><span class="sxs-lookup"><span data-stu-id="74304-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="74304-121">I messaggi possono essere inviati a tutte le connessioni in un gruppo.</span><span class="sxs-lookup"><span data-stu-id="74304-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="74304-122">I gruppi sono il metodo consigliato per l'invio di una connessione o più connessioni poiché i gruppi vengono gestiti dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="74304-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="74304-123">Una connessione può essere un membro di più gruppi.</span><span class="sxs-lookup"><span data-stu-id="74304-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="74304-124">In questo modo gruppi ideale per App come un'applicazione di chat, in cui ogni chat può essere rappresentato come un gruppo.</span><span class="sxs-lookup"><span data-stu-id="74304-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="74304-125">Le connessioni possono essere aggiunti o rimossi dai gruppi tramite il `AddToGroupAsync` e `RemoveFromGroupAsync` metodi.</span><span class="sxs-lookup"><span data-stu-id="74304-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="74304-126">L'appartenenza al gruppo non viene mantenuta quando si riconnette una connessione.</span><span class="sxs-lookup"><span data-stu-id="74304-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="74304-127">Per partecipare di nuovo il gruppo quando viene ristabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="74304-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="74304-128">Non è possibile contare i membri di un gruppo, poiché queste informazioni non sono disponibile se l'applicazione viene ridimensionata a più server.</span><span class="sxs-lookup"><span data-stu-id="74304-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="74304-129">I nomi dei gruppi sono tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="74304-129">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="74304-130">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="74304-130">Related resources</span></span>

* [<span data-ttu-id="74304-131">Introduzione</span><span class="sxs-lookup"><span data-stu-id="74304-131">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="74304-132">Hub</span><span class="sxs-lookup"><span data-stu-id="74304-132">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="74304-133">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="74304-133">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
