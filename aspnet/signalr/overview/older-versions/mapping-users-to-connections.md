---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Il mapping degli utenti di SignalR per le connessioni in SignalR 1. x | Documenti Microsoft
author: pfletcher
description: In questo argomento viene illustrato come mantenere le informazioni relative agli utenti e le relative connessioni.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 896bf4142ce090e39ed5697ff053cd56728318ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28037016"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="a86da-103">Il mapping degli utenti di SignalR per le connessioni in SignalR 1. x</span><span class="sxs-lookup"><span data-stu-id="a86da-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="a86da-104">da [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a86da-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a86da-105">In questo argomento viene illustrato come mantenere le informazioni relative agli utenti e le relative connessioni.</span><span class="sxs-lookup"><span data-stu-id="a86da-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="a86da-106">Introduzione</span><span class="sxs-lookup"><span data-stu-id="a86da-106">Introduction</span></span>

<span data-ttu-id="a86da-107">Ogni client che si connette a un hub passa un id connessione univoco. È possibile recuperare il valore di `Context.ConnectionId` proprietà del contesto di hub.</span><span class="sxs-lookup"><span data-stu-id="a86da-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="a86da-108">Se l'applicazione deve eseguire il mapping di un utente per l'id di connessione e mantenere tale mapping, è possibile utilizzare uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="a86da-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="a86da-109">[Archiviazione in memoria](#inmemory), ad esempio un dizionario</span><span class="sxs-lookup"><span data-stu-id="a86da-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="a86da-110">Gruppo di SignalR per ogni utente</span><span class="sxs-lookup"><span data-stu-id="a86da-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="a86da-111">[Archiviazione permanente, esterna](#database), ad esempio una tabella di database o l'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="a86da-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="a86da-112">Ognuna di queste implementazioni è illustrato in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="a86da-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="a86da-113">Utilizzare il `OnConnected`, `OnDisconnected`, e `OnReconnected` metodi il `Hub` classe per tenere traccia dello stato di connessione utente.</span><span class="sxs-lookup"><span data-stu-id="a86da-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="a86da-114">L'approccio migliore per l'applicazione dipende da:</span><span class="sxs-lookup"><span data-stu-id="a86da-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="a86da-115">Il numero di server web che ospita l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a86da-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="a86da-116">Se è necessario ottenere un elenco di utenti attualmente connessi.</span><span class="sxs-lookup"><span data-stu-id="a86da-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="a86da-117">Se è necessario rendere persistenti le informazioni utente e gruppo quando l'applicazione o il server viene riavviato.</span><span class="sxs-lookup"><span data-stu-id="a86da-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="a86da-118">Indica se la latenza della chiamata a un server esterno è un problema.</span><span class="sxs-lookup"><span data-stu-id="a86da-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="a86da-119">Nella tabella seguente quale approccio funziona per queste considerazioni.</span><span class="sxs-lookup"><span data-stu-id="a86da-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="a86da-120">Più di un server</span><span class="sxs-lookup"><span data-stu-id="a86da-120">More than one server</span></span> | <span data-ttu-id="a86da-121">Ottenere l'elenco degli utenti attualmente connessi.</span><span class="sxs-lookup"><span data-stu-id="a86da-121">Get list of currently connected users</span></span> | <span data-ttu-id="a86da-122">Rendere persistenti le informazioni dopo il riavvio</span><span class="sxs-lookup"><span data-stu-id="a86da-122">Persist information after restarts</span></span> | <span data-ttu-id="a86da-123">Prestazioni ottimali</span><span class="sxs-lookup"><span data-stu-id="a86da-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="a86da-124">In memoria</span><span class="sxs-lookup"><span data-stu-id="a86da-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="a86da-125">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="a86da-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="a86da-126">Rese permanenti, esterno</span><span class="sxs-lookup"><span data-stu-id="a86da-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="a86da-127">Archiviazione in memoria</span><span class="sxs-lookup"><span data-stu-id="a86da-127">In-memory storage</span></span>

<span data-ttu-id="a86da-128">Nell'esempio seguente viene illustrato come mantenere le informazioni di connessione e l'utente in un dizionario che verrà archiviato in memoria.</span><span class="sxs-lookup"><span data-stu-id="a86da-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="a86da-129">Il dizionario utilizza un `HashSet` per archiviare l'id di connessione. In qualsiasi momento un utente potrebbe avere più di una connessione all'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="a86da-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="a86da-130">Ad esempio, un utente connesso tramite più dispositivi o più di una scheda del browser avrebbe più di un id di connessione.</span><span class="sxs-lookup"><span data-stu-id="a86da-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="a86da-131">Se l'applicazione si arresta, tutte le informazioni vengono persi, ma verrà nuovamente popolato come gli utenti ristabilire le connessioni.</span><span class="sxs-lookup"><span data-stu-id="a86da-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="a86da-132">Archiviazione in memoria non funziona se l'ambiente include più di un server web, perché ogni server ha una raccolta separata di connessioni.</span><span class="sxs-lookup"><span data-stu-id="a86da-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="a86da-133">Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti per le connessioni.</span><span class="sxs-lookup"><span data-stu-id="a86da-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="a86da-134">La chiave per il HashSet sarà il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="a86da-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="a86da-135">Nell'esempio seguente viene illustrato come utilizzare la classe di mapping di connessione da un hub.</span><span class="sxs-lookup"><span data-stu-id="a86da-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="a86da-136">L'istanza della classe viene archiviata in un nome di variabile `_connections`.</span><span class="sxs-lookup"><span data-stu-id="a86da-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="a86da-137">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="a86da-137">Single-user groups</span></span>

<span data-ttu-id="a86da-138">È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si desidera raggiungere solo tale utente.</span><span class="sxs-lookup"><span data-stu-id="a86da-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="a86da-139">Il nome di ogni gruppo è il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="a86da-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="a86da-140">Se un utente dispone di più di una connessione, viene aggiunto ogni id di connessione per il gruppo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="a86da-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="a86da-141">È consigliabile non rimuovere manualmente l'utente dal gruppo di quando l'utente si disconnette.</span><span class="sxs-lookup"><span data-stu-id="a86da-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="a86da-142">Questa azione viene eseguita automaticamente dal framework di SignalR.</span><span class="sxs-lookup"><span data-stu-id="a86da-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="a86da-143">Nell'esempio seguente viene illustrato come implementare i gruppi utente singolo.</span><span class="sxs-lookup"><span data-stu-id="a86da-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="a86da-144">Archiviazione permanente, esterna</span><span class="sxs-lookup"><span data-stu-id="a86da-144">Permanent, external storage</span></span>

<span data-ttu-id="a86da-145">In questo argomento viene illustrato come utilizzare un database o un archivio tabelle di Azure per archiviare le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="a86da-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="a86da-146">Questo approccio funziona quando si dispone di più server web, poiché ogni server web può interagire con lo stesso repository di dati.</span><span class="sxs-lookup"><span data-stu-id="a86da-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="a86da-147">Se i server web arresta lavoro o il riavvio dell'applicazione, il `OnDisconnected` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="a86da-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="a86da-148">Pertanto, è possibile che il repository di dati disporrà di record per ID di connessione che non sono più validi.</span><span class="sxs-lookup"><span data-stu-id="a86da-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="a86da-149">Per eliminare questi record orfani, si desidera invalidare qualsiasi connessione che è stato creato all'esterno di un intervallo di tempo è rilevante per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a86da-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="a86da-150">Gli esempi in questa sezione includono un valore per il rilevamento quando la connessione è stata creata, ma non mostrano come pulire i vecchi record perché è consigliabile eseguire questa operazione come processo in background.</span><span class="sxs-lookup"><span data-stu-id="a86da-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="a86da-151">Database</span><span class="sxs-lookup"><span data-stu-id="a86da-151">Database</span></span>

<span data-ttu-id="a86da-152">Nell'esempio seguente viene illustrato come mantenere le informazioni di connessione e l'utente in un database.</span><span class="sxs-lookup"><span data-stu-id="a86da-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="a86da-153">È possibile usare qualsiasi tecnologia di accesso ai dati; Tuttavia, nell'esempio seguente viene illustrato come definire i modelli tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a86da-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="a86da-154">Questi modelli di entità corrispondono ai campi e tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="a86da-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="a86da-155">La struttura dei dati può variare notevolmente a seconda dei requisiti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a86da-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="a86da-156">Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a tutte le entità di connessione.</span><span class="sxs-lookup"><span data-stu-id="a86da-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="a86da-157">Dall'hub, quindi, è possibile rilevare lo stato di ogni connessione con il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a86da-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="a86da-158">Archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="a86da-158">Azure table storage</span></span>

<span data-ttu-id="a86da-159">Nell'esempio seguente viene archiviazione tabelle di Azure è simile all'esempio di database.</span><span class="sxs-lookup"><span data-stu-id="a86da-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="a86da-160">Non include tutte le informazioni necessarie per iniziare a utilizzare il servizio di archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="a86da-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="a86da-161">Per informazioni, vedere [come usare l'archiviazione di tabelle da .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="a86da-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="a86da-162">L'esempio seguente mostra un'entità di tabella per archiviare le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="a86da-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="a86da-163">I dati vengono partizionati in base al nome utente e identifica ogni entità dall'id di connessione, in modo che un utente può avere più connessioni in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="a86da-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="a86da-164">Nell'hub, registrare lo stato di ogni connessione utente.</span><span class="sxs-lookup"><span data-stu-id="a86da-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
