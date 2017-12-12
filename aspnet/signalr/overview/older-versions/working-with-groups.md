---
uid: signalr/overview/older-versions/working-with-groups
title: Utilizzo dei gruppi in SignalR 1. x | Documenti Microsoft
author: pfletcher
description: In questo argomento viene descritto come rendere persistenti le informazioni di appartenenza al gruppo con l'API di Hub.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 04da74f23663313e70e54fd4f2f9e5f005791cff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="79761-103">Utilizzo dei gruppi in SignalR 1. x</span><span class="sxs-lookup"><span data-stu-id="79761-103">Working with Groups in SignalR 1.x</span></span>
====================
<span data-ttu-id="79761-104">da [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="79761-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="79761-105">In questo argomento viene descritto come aggiungere utenti ai gruppi e rendere persistenti le informazioni di appartenenza al gruppo.</span><span class="sxs-lookup"><span data-stu-id="79761-105">This topic describes how to add users to groups and persist group membership information.</span></span>


## <a name="overview"></a><span data-ttu-id="79761-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="79761-106">Overview</span></span>

<span data-ttu-id="79761-107">Gruppi di SignalR forniscono un metodo per la trasmissione di messaggi a un subset specificato di client connessi.</span><span class="sxs-lookup"><span data-stu-id="79761-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="79761-108">Un gruppo può avere qualsiasi numero di client e un client può essere un membro di un numero qualsiasi di gruppi.</span><span class="sxs-lookup"><span data-stu-id="79761-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="79761-109">Non è necessario creare in modo esplicito gruppi.</span><span class="sxs-lookup"><span data-stu-id="79761-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="79761-110">In effetti, la prima volta, che specificare il nome in una chiamata a Groups.Add viene automaticamente creato un gruppo e viene eliminata quando si rimuove l'ultima connessione dall'appartenenza in essa contenuti.</span><span class="sxs-lookup"><span data-stu-id="79761-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="79761-111">Per un'introduzione all'uso di gruppi, vedere [come gestire l'appartenenza al gruppo dalla classe dell'Hub](index.md) nell'API di hub - Guida di Server.</span><span class="sxs-lookup"><span data-stu-id="79761-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="79761-112">Non vi è alcuna API per ottenere un elenco di appartenenze di gruppo oppure un elenco di gruppi.</span><span class="sxs-lookup"><span data-stu-id="79761-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="79761-113">SignalR invia messaggi al client e i gruppi basati su un modello di pubblicazione/sottoscrizione e il server non gestisce gli elenchi di gruppi o appartenenza al gruppo.</span><span class="sxs-lookup"><span data-stu-id="79761-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="79761-114">Ciò consente di ottimizzare la scalabilità, poiché ogni volta che si aggiunge un nodo a una web farm, qualsiasi stato che gestisce SignalR deve essere propagata nel nuovo nodo.</span><span class="sxs-lookup"><span data-stu-id="79761-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="79761-115">Quando si aggiunge un utente a un gruppo con il `Groups.Add` (metodo), l'utente riceve i messaggi indirizzati a tale gruppo per la durata della connessione corrente, ma l'appartenenza dell'utente in tale gruppo non viene mantenuto oltre la connessione corrente.</span><span class="sxs-lookup"><span data-stu-id="79761-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="79761-116">Se si desidera mantenere in modo permanente le informazioni sui gruppi e appartenenza al gruppo, è necessario archiviare i dati in un repository, ad esempio un database o l'archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="79761-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="79761-117">Quindi, ogni volta che un utente si connette all'applicazione, recuperare dal repository quali gruppi a cui appartiene l'utente e aggiungere manualmente l'utente a tali gruppi.</span><span class="sxs-lookup"><span data-stu-id="79761-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="79761-118">Quando la riconnessione dopo un'interruzione temporanea, l'utente aggiunge automaticamente anche nuovamente i gruppi assegnate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="79761-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="79761-119">Automaticamente nuova partecipazione a un gruppo si applica solo quando la riconnessione, non per stabilire una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="79761-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="79761-120">Un token di firma digitale viene passato dal client che contiene l'elenco dei gruppi assegnate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="79761-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="79761-121">Se si desidera verificare se l'utente appartiene al gruppo richiesto, è possibile eseguire l'override del comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="79761-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="79761-122">Questo argomento include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="79761-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="79761-123">Aggiunta e rimozione di utenti</span><span class="sxs-lookup"><span data-stu-id="79761-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="79761-124">La chiamata di membri di un gruppo</span><span class="sxs-lookup"><span data-stu-id="79761-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="79761-125">L'appartenenza al gruppo di archiviazione in un database</span><span class="sxs-lookup"><span data-stu-id="79761-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="79761-126">L'appartenenza al gruppo di archiviazione nell'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="79761-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="79761-127">Verifica dell'appartenenza al gruppo durante la riconnessione</span><span class="sxs-lookup"><span data-stu-id="79761-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="79761-128">Aggiunta e rimozione di utenti</span><span class="sxs-lookup"><span data-stu-id="79761-128">Adding and removing users</span></span>

<span data-ttu-id="79761-129">Per aggiungere o rimuovere utenti da un gruppo, si chiama il [Aggiungi](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) o [rimuovere](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metodi e passare l'id connessione dell'utente e il nome del gruppo come parametri.</span><span class="sxs-lookup"><span data-stu-id="79761-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="79761-130">Non è necessario rimuovere manualmente un utente da un gruppo, al termine della connessione.</span><span class="sxs-lookup"><span data-stu-id="79761-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="79761-131">Nell'esempio seguente il `Groups.Add` e `Groups.Remove` metodi usati nei metodi dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="79761-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="79761-132">Il `Groups.Add` e `Groups.Remove` metodi eseguire in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="79761-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="79761-133">Se si desidera aggiungere un client a un gruppo e invia immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il metodo Groups.Add termina per prima.</span><span class="sxs-lookup"><span data-stu-id="79761-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="79761-134">Gli esempi di codice seguente viene illustrato come eseguire questa operazione, utilizzando il codice funzioni in .NET 4.5 e uno usando il codice funzioni in .NET 4.</span><span class="sxs-lookup"><span data-stu-id="79761-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="79761-135">Asincrona .NET 4.5 esempio</span><span class="sxs-lookup"><span data-stu-id="79761-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="79761-136">Asincrona .NET 4 esempio</span><span class="sxs-lookup"><span data-stu-id="79761-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="79761-137">In generale, non è necessario includere `await` quando si chiama il `Groups.Remove` metodo perché l'id di connessione che si sta tentando di rimuovere potrebbe non essere più disponibile.</span><span class="sxs-lookup"><span data-stu-id="79761-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="79761-138">In tal caso, `TaskCanceledException` dopo la richiesta scade, viene generata un'eccezione. Se l'applicazione deve verificare che l'utente è stato rimosso dal gruppo prima di inviare un messaggio al gruppo, è possibile aggiungere `await` prima Groups.Remove e quindi catch il `TaskCanceledException` eccezione che potrebbe essere generata.</span><span class="sxs-lookup"><span data-stu-id="79761-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="79761-139">La chiamata di membri di un gruppo</span><span class="sxs-lookup"><span data-stu-id="79761-139">Calling members of a group</span></span>

<span data-ttu-id="79761-140">È possibile inviare messaggi a tutti i membri di un gruppo o solo i membri specificati del gruppo, come illustrato negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="79761-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="79761-141">**Tutti** connessi i client in un gruppo specificato.</span><span class="sxs-lookup"><span data-stu-id="79761-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="79761-142">Tutti i client in un gruppo specificato connessi **tranne client specificati**, identificato dall'ID connessione.</span><span class="sxs-lookup"><span data-stu-id="79761-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="79761-143">Tutti i client in un gruppo specificato connessi **eccetto il client chiamante**.</span><span class="sxs-lookup"><span data-stu-id="79761-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="79761-144">L'appartenenza al gruppo di archiviazione in un database</span><span class="sxs-lookup"><span data-stu-id="79761-144">Storing group membership in a database</span></span>

<span data-ttu-id="79761-145">Nell'esempio seguente viene illustrato come mantenere le informazioni utente e di gruppo in un database.</span><span class="sxs-lookup"><span data-stu-id="79761-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="79761-146">È possibile usare qualsiasi tecnologia di accesso ai dati; Tuttavia, nell'esempio seguente viene illustrato come definire i modelli tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="79761-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="79761-147">Questi modelli di entità corrispondono ai campi e tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="79761-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="79761-148">La struttura dei dati può variare notevolmente a seconda dei requisiti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="79761-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="79761-149">In questo esempio include una classe denominata `ConversationRoom` che sono univoci a un'applicazione che consente agli utenti di partecipare a conversazioni relativi ad argomenti diversi, ad esempio sports o garden.</span><span class="sxs-lookup"><span data-stu-id="79761-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="79761-150">In questo esempio include anche una classe per le connessioni.</span><span class="sxs-lookup"><span data-stu-id="79761-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="79761-151">La classe di connessione non è assolutamente necessaria per l'appartenenza al gruppo di rilevamento, ma spesso è parte della soluzione efficace per tenere traccia degli utenti.</span><span class="sxs-lookup"><span data-stu-id="79761-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="79761-152">Quindi, nell'hub, è possibile recuperare le informazioni di gruppo e utente dal database e aggiungere manualmente l'utente ai gruppi appropriati.</span><span class="sxs-lookup"><span data-stu-id="79761-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="79761-153">L'esempio non include codice per tenere traccia delle connessioni utente.</span><span class="sxs-lookup"><span data-stu-id="79761-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="79761-154">In questo esempio, il `await` parola chiave non viene applicata prima `Groups.Add` perché un messaggio non viene inviato immediatamente ai membri del gruppo.</span><span class="sxs-lookup"><span data-stu-id="79761-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="79761-155">Se si desidera inviare un messaggio a tutti i membri del gruppo immediatamente dopo l'aggiunta del nuovo membro, si desidera applicare il `await` (parola chiave) per assicurarsi che l'operazione asincrona è stata completata.</span><span class="sxs-lookup"><span data-stu-id="79761-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="79761-156">L'appartenenza al gruppo di archiviazione nell'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="79761-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="79761-157">Utilizzo di archiviazione tabelle di Azure per archiviare informazioni utente e gruppo è simile all'utilizzo di un database.</span><span class="sxs-lookup"><span data-stu-id="79761-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="79761-158">L'esempio seguente mostra un'entità di tabella che archivia il nome utente e il nome del gruppo.</span><span class="sxs-lookup"><span data-stu-id="79761-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="79761-159">Nell'hub, recuperare i gruppi assegnati quando l'utente si connette.</span><span class="sxs-lookup"><span data-stu-id="79761-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="79761-160">Verifica dell'appartenenza al gruppo durante la riconnessione</span><span class="sxs-lookup"><span data-stu-id="79761-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="79761-161">Per impostazione predefinita, SignalR nuovamente assegna automaticamente un utente ai gruppi appropriati quando si riconnette da un'interruzione temporanea, ad esempio quando una connessione viene eliminata e nuovamente stabilita prima del timeout della connessione. Informazioni di gruppo dell'utente vengono passate in un token durante la riconnessione e tale token viene verificato sul server.</span><span class="sxs-lookup"><span data-stu-id="79761-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="79761-162">Per informazioni sul processo di verifica per la nuova partecipazione a gruppi di utenti, vedere [nuova partecipazione a gruppi durante la riconnessione](index.md).</span><span class="sxs-lookup"><span data-stu-id="79761-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="79761-163">In generale, utilizzare il comportamento predefinito di automaticamente nuova partecipazione a gruppi in riconnessione.</span><span class="sxs-lookup"><span data-stu-id="79761-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="79761-164">Gruppi di SignalR non servono come meccanismo di sicurezza per limitare l'accesso ai dati sensibili.</span><span class="sxs-lookup"><span data-stu-id="79761-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="79761-165">Tuttavia, se l'applicazione deve controllare l'appartenenza al gruppo un utente durante la riconnessione, è possibile ignorare il comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="79761-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="79761-166">Modifica del comportamento predefinito può aggiungere un carico di lavoro al database perché l'appartenenza al gruppo dell'utente deve essere recuperato per ogni riconnessione anziché solo quando l'utente si connette.</span><span class="sxs-lookup"><span data-stu-id="79761-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="79761-167">Se è necessario verificare l'appartenenza al gruppo in riconnettersi, creare un nuovo modulo di pipeline di hub che restituisce un elenco di gruppi assegnati, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="79761-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="79761-168">Aggiungere quindi tale modulo alla pipeline dell'hub, come evidenziato qui sotto.</span><span class="sxs-lookup"><span data-stu-id="79761-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
