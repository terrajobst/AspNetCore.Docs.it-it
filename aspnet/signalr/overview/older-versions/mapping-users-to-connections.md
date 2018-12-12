---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Mapping degli utenti di SignalR alle connessioni in SignalR 1.x | Microsoft Docs
author: pfletcher
description: In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: ba7bbea25cd35f8ba23901c5f8db08404c91bd52
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287482"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="d3675-103">Mapping degli utenti di SignalR alle connessioni in SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="d3675-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="d3675-104">dal [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d3675-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="d3675-105">In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni.</span><span class="sxs-lookup"><span data-stu-id="d3675-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="d3675-106">Introduzione</span><span class="sxs-lookup"><span data-stu-id="d3675-106">Introduction</span></span>

<span data-ttu-id="d3675-107">Ogni client che si connette a un hub passa un id univoco della connessione. È possibile recuperare questo valore nel `Context.ConnectionId` proprietà del contesto dell'hub.</span><span class="sxs-lookup"><span data-stu-id="d3675-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="d3675-108">Se l'applicazione deve eseguire il mapping di un utente per l'id di connessione e mantenere tale mapping, è possibile usare uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="d3675-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="d3675-109">[Archiviazione in memoria](#inmemory), ad esempio un dizionario</span><span class="sxs-lookup"><span data-stu-id="d3675-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="d3675-110">Gruppo di SignalR per ogni utente</span><span class="sxs-lookup"><span data-stu-id="d3675-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="d3675-111">[Archiviazione esterna permanente](#database), ad esempio una tabella di database o l'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="d3675-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="d3675-112">Ognuna di queste implementazioni è illustrato in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="d3675-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="d3675-113">Si utilizza il `OnConnected`, `OnDisconnected`, e `OnReconnected` metodi del `Hub` classe tenere traccia dello stato di connessione utente.</span><span class="sxs-lookup"><span data-stu-id="d3675-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="d3675-114">L'approccio migliore per l'applicazione dipende:</span><span class="sxs-lookup"><span data-stu-id="d3675-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="d3675-115">Il numero di server web che ospitano l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d3675-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="d3675-116">Indica se è necessario ottenere un elenco di utenti attualmente connessi.</span><span class="sxs-lookup"><span data-stu-id="d3675-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="d3675-117">Indica se è necessario rendere persistenti le informazioni di gruppo e utente quando l'applicazione o il server viene riavviato.</span><span class="sxs-lookup"><span data-stu-id="d3675-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="d3675-118">Indica se la latenza della chiamata a un server esterno è un problema.</span><span class="sxs-lookup"><span data-stu-id="d3675-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="d3675-119">La tabella seguente illustra l'approccio funziona per queste considerazioni.</span><span class="sxs-lookup"><span data-stu-id="d3675-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="d3675-120">Più di un server</span><span class="sxs-lookup"><span data-stu-id="d3675-120">More than one server</span></span> | <span data-ttu-id="d3675-121">Ottiene l'elenco di utenti attualmente connessi</span><span class="sxs-lookup"><span data-stu-id="d3675-121">Get list of currently connected users</span></span> | <span data-ttu-id="d3675-122">Rendere persistenti le informazioni dopo il riavvio</span><span class="sxs-lookup"><span data-stu-id="d3675-122">Persist information after restarts</span></span> | <span data-ttu-id="d3675-123">Ottenere prestazioni ottimali</span><span class="sxs-lookup"><span data-stu-id="d3675-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="d3675-124">In memoria</span><span class="sxs-lookup"><span data-stu-id="d3675-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="d3675-125">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="d3675-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="d3675-126">Permanenti, esterni</span><span class="sxs-lookup"><span data-stu-id="d3675-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="d3675-127">Archiviazione in memoria</span><span class="sxs-lookup"><span data-stu-id="d3675-127">In-memory storage</span></span>

<span data-ttu-id="d3675-128">Gli esempi seguenti illustrano come mantenere le informazioni di connessione e dell'utente in un dizionario che viene archiviato in memoria.</span><span class="sxs-lookup"><span data-stu-id="d3675-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="d3675-129">Il dizionario utilizza un `HashSet` per archiviare l'id di connessione. In qualsiasi momento un utente potrebbe avere più di una connessione all'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="d3675-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="d3675-130">Ad esempio, un utente che è connessa tramite più dispositivi o più di una scheda del browser avrebbe più di un id di connessione.</span><span class="sxs-lookup"><span data-stu-id="d3675-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="d3675-131">Se l'applicazione viene arrestata, tutte le informazioni vengono persi, ma verrà nuovamente popolato come gli utenti di ristabilire le connessioni.</span><span class="sxs-lookup"><span data-stu-id="d3675-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="d3675-132">Archiviazione in memoria non funziona se l'ambiente include più di un server web, poiché ogni server avrà una raccolta separata di connessioni.</span><span class="sxs-lookup"><span data-stu-id="d3675-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="d3675-133">Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti alle connessioni.</span><span class="sxs-lookup"><span data-stu-id="d3675-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="d3675-134">La chiave per il HashSet sarà il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="d3675-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="d3675-135">L'esempio successivo illustra come usare la classe di mapping di connessione da un hub.</span><span class="sxs-lookup"><span data-stu-id="d3675-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="d3675-136">L'istanza della classe è archiviata in un nome di variabile `_connections`.</span><span class="sxs-lookup"><span data-stu-id="d3675-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="d3675-137">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="d3675-137">Single-user groups</span></span>

<span data-ttu-id="d3675-138">È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si vuole raggiungere solo tale utente.</span><span class="sxs-lookup"><span data-stu-id="d3675-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="d3675-139">Il nome di ogni gruppo è il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="d3675-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="d3675-140">Se un utente ha più di una connessione, ogni id di connessione viene aggiunto al gruppo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="d3675-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="d3675-141">È consigliabile non rimuovere manualmente l'utente dal gruppo quando l'utente si disconnette.</span><span class="sxs-lookup"><span data-stu-id="d3675-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="d3675-142">Questa azione viene eseguita automaticamente dal framework di SignalR.</span><span class="sxs-lookup"><span data-stu-id="d3675-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="d3675-143">Nell'esempio seguente viene illustrato come implementare i gruppi utente singolo.</span><span class="sxs-lookup"><span data-stu-id="d3675-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="d3675-144">Archiviazione esterna permanente</span><span class="sxs-lookup"><span data-stu-id="d3675-144">Permanent, external storage</span></span>

<span data-ttu-id="d3675-145">In questo argomento viene illustrato come utilizzare un database o un archivio tabelle di Azure per archiviare le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="d3675-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="d3675-146">Questo approccio funziona quando si dispone di più server web, poiché ogni server web può interagire con lo stesso repository di dati.</span><span class="sxs-lookup"><span data-stu-id="d3675-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="d3675-147">Se i server web arresta lavoro o del riavvio dell'applicazione, il `OnDisconnected` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="d3675-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="d3675-148">Pertanto, è possibile che il repository dei dati avrà i record per ID di connessione che non sono più valide.</span><span class="sxs-lookup"><span data-stu-id="d3675-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="d3675-149">Per eliminare tali record orfani, si consiglia di invalidare eventuali connessione creata di fuori di un intervallo di tempo è rilevante per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d3675-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="d3675-150">Gli esempi in questa sezione includono un valore per la registrazione quando la connessione è stata creata, ma non mostrano come pulire i record obsoleti poiché è possibile eseguire questa operazione come processo in background.</span><span class="sxs-lookup"><span data-stu-id="d3675-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="d3675-151">Database</span><span class="sxs-lookup"><span data-stu-id="d3675-151">Database</span></span>

<span data-ttu-id="d3675-152">Gli esempi seguenti illustrano come mantenere le informazioni di connessione e dell'utente in un database.</span><span class="sxs-lookup"><span data-stu-id="d3675-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="d3675-153">È possibile usare qualsiasi tecnologia di accesso ai dati; Tuttavia, l'esempio seguente viene illustrato come definire i modelli usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d3675-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="d3675-154">Questi modelli di entità corrispondono ai campi e tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="d3675-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="d3675-155">Struttura dei dati potrebbe variare notevolmente a seconda dei requisiti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d3675-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="d3675-156">Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a tutte le entità di connessione.</span><span class="sxs-lookup"><span data-stu-id="d3675-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="d3675-157">Quindi, dall'hub, è possibile monitorare lo stato di ogni connessione con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="d3675-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="d3675-158">Archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="d3675-158">Azure table storage</span></span>

<span data-ttu-id="d3675-159">L'esempio di archiviazione tabelle di Azure seguente è simile all'esempio del database.</span><span class="sxs-lookup"><span data-stu-id="d3675-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="d3675-160">Non include tutte le informazioni che occorre per iniziare a usare il servizio di archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="d3675-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="d3675-161">Per informazioni, vedere [come usare l'archiviazione tabelle da .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="d3675-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="d3675-162">L'esempio seguente illustra un'entità di tabella per archiviare le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="d3675-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="d3675-163">Partiziona i dati in base al nome utente e identifica ogni entità dall'id di connessione, in modo che un utente può avere più connessioni in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="d3675-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="d3675-164">Nell'hub, tenere traccia dello stato di ogni connessione utente.</span><span class="sxs-lookup"><span data-stu-id="d3675-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
