---
title: Gestire utenti e gruppi in SignalR
author: bradygaster
description: Panoramica di ASP.NET Core SignalR la gestione di utenti e gruppi.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/groups
ms.openlocfilehash: 7e8c85dcbc46daa68988374f499f19a9d2cead47
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665865"
---
# <a name="manage-users-and-groups-in-opno-locsignalr"></a><span data-ttu-id="c5abf-103">Gestire utenti e gruppi in SignalR</span><span class="sxs-lookup"><span data-stu-id="c5abf-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="c5abf-104">Di [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="c5abf-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

SignalR<span data-ttu-id="c5abf-105"> consente l'invio di messaggi a tutte le connessioni associate a un utente specifico, nonché a gruppi denominati di connessioni.</span><span class="sxs-lookup"><span data-stu-id="c5abf-105"> allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="c5abf-106">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(procedura per il download)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="c5abf-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-opno-locsignalr"></a><span data-ttu-id="c5abf-107">Utenti in SignalR</span><span class="sxs-lookup"><span data-stu-id="c5abf-107">Users in SignalR</span></span>

SignalR<span data-ttu-id="c5abf-108"> consente di inviare messaggi a tutte le connessioni associate a un utente specifico.</span><span class="sxs-lookup"><span data-stu-id="c5abf-108"> allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="c5abf-109">Per impostazione predefinita, SignalR usa il `ClaimTypes.NameIdentifier` dal `ClaimsPrincipal` associato alla connessione come identificatore utente.</span><span class="sxs-lookup"><span data-stu-id="c5abf-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="c5abf-110">Un singolo utente può avere più connessioni a un'app SignalR.</span><span class="sxs-lookup"><span data-stu-id="c5abf-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="c5abf-111">Ad esempio, un utente potrebbe essere connesso sul desktop e sul telefono.</span><span class="sxs-lookup"><span data-stu-id="c5abf-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="c5abf-112">Ogni dispositivo ha una connessione di SignalR separata, ma tutti sono associati allo stesso utente.</span><span class="sxs-lookup"><span data-stu-id="c5abf-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="c5abf-113">Se un messaggio viene inviato all'utente, tutte le connessioni associate a tale utente riceveranno il messaggio.</span><span class="sxs-lookup"><span data-stu-id="c5abf-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="c5abf-114">È possibile accedere all'identificatore utente per una connessione tramite la proprietà `Context.UserIdentifier` nell'hub.</span><span class="sxs-lookup"><span data-stu-id="c5abf-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="c5abf-115">Inviare un messaggio a un utente specifico passando l'identificatore utente alla funzione `User` nel metodo dell'hub, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c5abf-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="c5abf-116">L'identificatore utente distingue tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="c5abf-116">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-opno-locsignalr"></a><span data-ttu-id="c5abf-117">Gruppi in SignalR</span><span class="sxs-lookup"><span data-stu-id="c5abf-117">Groups in SignalR</span></span>

<span data-ttu-id="c5abf-118">Un gruppo è una raccolta di connessioni associate a un nome.</span><span class="sxs-lookup"><span data-stu-id="c5abf-118">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="c5abf-119">I messaggi possono essere inviati a tutte le connessioni in un gruppo.</span><span class="sxs-lookup"><span data-stu-id="c5abf-119">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="c5abf-120">I gruppi sono il metodo consigliato per l'invio a una connessione o a più connessioni perché i gruppi sono gestiti dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c5abf-120">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="c5abf-121">Una connessione può essere un membro di più gruppi.</span><span class="sxs-lookup"><span data-stu-id="c5abf-121">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="c5abf-122">In questo modo i gruppi sono ideali per un elemento come un'applicazione di chat, in cui ogni stanza può essere rappresentata come un gruppo.</span><span class="sxs-lookup"><span data-stu-id="c5abf-122">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="c5abf-123">Le connessioni possono essere aggiunte o rimosse dai gruppi tramite i metodi `AddToGroupAsync` e `RemoveFromGroupAsync`.</span><span class="sxs-lookup"><span data-stu-id="c5abf-123">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="c5abf-124">L'appartenenza al gruppo non viene mantenuta quando si riconnette una connessione.</span><span class="sxs-lookup"><span data-stu-id="c5abf-124">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="c5abf-125">La connessione deve essere nuovamente aggiunta al gruppo quando viene ristabilita.</span><span class="sxs-lookup"><span data-stu-id="c5abf-125">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="c5abf-126">Non è possibile contare i membri di un gruppo, poiché queste informazioni non sono disponibili se l'applicazione viene ridimensionata in più server.</span><span class="sxs-lookup"><span data-stu-id="c5abf-126">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="c5abf-127">Per proteggere l'accesso alle risorse durante l'uso di gruppi, usare la funzionalità di [autenticazione e autorizzazione](xref:signalr/authn-and-authz) in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c5abf-127">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="c5abf-128">Se si aggiungono utenti a un gruppo solo quando le credenziali sono valide per il gruppo, i messaggi inviati a tale gruppo passeranno solo agli utenti autorizzati.</span><span class="sxs-lookup"><span data-stu-id="c5abf-128">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="c5abf-129">Tuttavia, i gruppi non sono una funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="c5abf-129">However, groups are not a security feature.</span></span> <span data-ttu-id="c5abf-130">Le attestazioni di autenticazione hanno funzionalità che non vengono eseguite dai gruppi, ad esempio la scadenza e la revoca.</span><span class="sxs-lookup"><span data-stu-id="c5abf-130">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="c5abf-131">Se l'autorizzazione dell'utente per accedere al gruppo viene revocata, è necessario rilevarla manualmente e rimuoverle dal gruppo.</span><span class="sxs-lookup"><span data-stu-id="c5abf-131">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="c5abf-132">I nomi dei gruppi fanno distinzione tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="c5abf-132">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="c5abf-133">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="c5abf-133">Related resources</span></span>

* [<span data-ttu-id="c5abf-134">Operazioni preliminari</span><span class="sxs-lookup"><span data-stu-id="c5abf-134">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="c5abf-135">Hub</span><span class="sxs-lookup"><span data-stu-id="c5abf-135">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c5abf-136">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="c5abf-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
