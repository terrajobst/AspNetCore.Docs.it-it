---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Il mapping degli utenti di SignalR per le connessioni | Documenti Microsoft
author: tfitzmac
description: In questo argomento viene illustrato come mantenere le informazioni relative agli utenti e le relative connessioni. Patrick Fletcher hanno contribuito alla scrittura di questo argomento. Versioni di software utilizzate in questo argomento...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: c4f95a3b65c57dd7cb7c5c7f1ee09daa17fa9616
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="85e98-105">Il mapping degli utenti di SignalR per le connessioni</span><span class="sxs-lookup"><span data-stu-id="85e98-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="85e98-106">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="85e98-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="85e98-107">In questo argomento viene illustrato come mantenere le informazioni relative agli utenti e le relative connessioni.</span><span class="sxs-lookup"><span data-stu-id="85e98-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="85e98-108">Patrick Fletcher hanno contribuito alla scrittura di questo argomento.</span><span class="sxs-lookup"><span data-stu-id="85e98-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="85e98-109">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="85e98-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="85e98-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="85e98-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="85e98-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="85e98-111">.NET 4.5</span></span>
> - <span data-ttu-id="85e98-112">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="85e98-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="85e98-113">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="85e98-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="85e98-114">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="85e98-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="85e98-115">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="85e98-115">Questions and comments</span></span>
> 
> <span data-ttu-id="85e98-116">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="85e98-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="85e98-117">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="85e98-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="85e98-118">Introduzione</span><span class="sxs-lookup"><span data-stu-id="85e98-118">Introduction</span></span>

<span data-ttu-id="85e98-119">Ogni client che si connette a un hub passa un id connessione univoco. È possibile recuperare il valore di `Context.ConnectionId` proprietà del contesto di hub.</span><span class="sxs-lookup"><span data-stu-id="85e98-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="85e98-120">Se l'applicazione deve eseguire il mapping di un utente per l'id di connessione e mantenere tale mapping, è possibile utilizzare uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="85e98-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="85e98-121">Il Provider di ID utente (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="85e98-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="85e98-122">[Archiviazione in memoria](#inmemory), ad esempio un dizionario</span><span class="sxs-lookup"><span data-stu-id="85e98-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="85e98-123">Gruppo di SignalR per ogni utente</span><span class="sxs-lookup"><span data-stu-id="85e98-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="85e98-124">[Archiviazione permanente, esterna](#database), ad esempio una tabella di database o l'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="85e98-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="85e98-125">Ognuna di queste implementazioni è illustrato in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="85e98-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="85e98-126">Utilizzare il `OnConnected`, `OnDisconnected`, e `OnReconnected` metodi il `Hub` classe per tenere traccia dello stato di connessione utente.</span><span class="sxs-lookup"><span data-stu-id="85e98-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="85e98-127">L'approccio migliore per l'applicazione dipende da:</span><span class="sxs-lookup"><span data-stu-id="85e98-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="85e98-128">Il numero di server web che ospita l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="85e98-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="85e98-129">Se è necessario ottenere un elenco di utenti attualmente connessi.</span><span class="sxs-lookup"><span data-stu-id="85e98-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="85e98-130">Se è necessario rendere persistenti le informazioni utente e gruppo quando l'applicazione o il server viene riavviato.</span><span class="sxs-lookup"><span data-stu-id="85e98-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="85e98-131">Indica se la latenza della chiamata a un server esterno è un problema.</span><span class="sxs-lookup"><span data-stu-id="85e98-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="85e98-132">Nella tabella seguente quale approccio funziona per queste considerazioni.</span><span class="sxs-lookup"><span data-stu-id="85e98-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="85e98-133">Più di un server</span><span class="sxs-lookup"><span data-stu-id="85e98-133">More than one server</span></span> | <span data-ttu-id="85e98-134">Ottenere l'elenco degli utenti attualmente connessi.</span><span class="sxs-lookup"><span data-stu-id="85e98-134">Get list of currently connected users</span></span> | <span data-ttu-id="85e98-135">Rendere persistenti le informazioni dopo il riavvio</span><span class="sxs-lookup"><span data-stu-id="85e98-135">Persist information after restarts</span></span> | <span data-ttu-id="85e98-136">Prestazioni ottimali</span><span class="sxs-lookup"><span data-stu-id="85e98-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="85e98-137">Provider di ID utente</span><span class="sxs-lookup"><span data-stu-id="85e98-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="85e98-138">In memoria</span><span class="sxs-lookup"><span data-stu-id="85e98-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="85e98-139">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="85e98-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="85e98-140">Rese permanenti, esterno</span><span class="sxs-lookup"><span data-stu-id="85e98-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="85e98-141">Provider IUserID</span><span class="sxs-lookup"><span data-stu-id="85e98-141">IUserID provider</span></span>

<span data-ttu-id="85e98-142">Questa funzionalità consente agli utenti di specificare l'ID utente in base a un IRequest tramite una nuova interfaccia IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="85e98-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="85e98-143">**Il IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="85e98-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="85e98-144">Per impostazione predefinita, vi sarà un'implementazione che l'utente utilizza `IPrincipal.Identity.Name` come nome utente.</span><span class="sxs-lookup"><span data-stu-id="85e98-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="85e98-145">Per modificare questa impostazione, registrare l'implementazione di `IUserIdProvider` con l'host globale all'avvio dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="85e98-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="85e98-146">All'interno di un hub, sarà in grado di inviare messaggi agli utenti tramite l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="85e98-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="85e98-147">**Invia un messaggio a un utente specifico**</span><span class="sxs-lookup"><span data-stu-id="85e98-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="85e98-148">Archiviazione in memoria</span><span class="sxs-lookup"><span data-stu-id="85e98-148">In-memory storage</span></span>

<span data-ttu-id="85e98-149">Nell'esempio seguente viene illustrato come mantenere le informazioni di connessione e l'utente in un dizionario che verrà archiviato in memoria.</span><span class="sxs-lookup"><span data-stu-id="85e98-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="85e98-150">Il dizionario utilizza un `HashSet` per archiviare l'id di connessione. In qualsiasi momento un utente potrebbe avere più di una connessione all'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="85e98-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="85e98-151">Ad esempio, un utente connesso tramite più dispositivi o più di una scheda del browser avrebbe più di un id di connessione.</span><span class="sxs-lookup"><span data-stu-id="85e98-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="85e98-152">Se l'applicazione si arresta, tutte le informazioni vengono persi, ma verrà nuovamente popolato come gli utenti ristabilire le connessioni.</span><span class="sxs-lookup"><span data-stu-id="85e98-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="85e98-153">Archiviazione in memoria non funziona se l'ambiente include più di un server web, perché ogni server ha una raccolta separata di connessioni.</span><span class="sxs-lookup"><span data-stu-id="85e98-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="85e98-154">Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti per le connessioni.</span><span class="sxs-lookup"><span data-stu-id="85e98-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="85e98-155">La chiave per il HashSet sarà il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="85e98-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="85e98-156">Nell'esempio seguente viene illustrato come utilizzare la classe di mapping di connessione da un hub.</span><span class="sxs-lookup"><span data-stu-id="85e98-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="85e98-157">L'istanza della classe viene archiviata in un nome di variabile `_connections`.</span><span class="sxs-lookup"><span data-stu-id="85e98-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="85e98-158">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="85e98-158">Single-user groups</span></span>

<span data-ttu-id="85e98-159">È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si desidera raggiungere solo tale utente.</span><span class="sxs-lookup"><span data-stu-id="85e98-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="85e98-160">Il nome di ogni gruppo è il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="85e98-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="85e98-161">Se un utente dispone di più di una connessione, viene aggiunto ogni id di connessione per il gruppo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="85e98-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="85e98-162">È consigliabile non rimuovere manualmente l'utente dal gruppo di quando l'utente si disconnette.</span><span class="sxs-lookup"><span data-stu-id="85e98-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="85e98-163">Questa azione viene eseguita automaticamente dal framework di SignalR.</span><span class="sxs-lookup"><span data-stu-id="85e98-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="85e98-164">Nell'esempio seguente viene illustrato come implementare i gruppi utente singolo.</span><span class="sxs-lookup"><span data-stu-id="85e98-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="85e98-165">Archiviazione permanente, esterna</span><span class="sxs-lookup"><span data-stu-id="85e98-165">Permanent, external storage</span></span>

<span data-ttu-id="85e98-166">In questo argomento viene illustrato come utilizzare un database o un archivio tabelle di Azure per archiviare le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="85e98-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="85e98-167">Questo approccio funziona quando si dispone di più server web, poiché ogni server web può interagire con lo stesso repository di dati.</span><span class="sxs-lookup"><span data-stu-id="85e98-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="85e98-168">Se i server web arresta lavoro o il riavvio dell'applicazione, il `OnDisconnected` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="85e98-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="85e98-169">Pertanto, è possibile che il repository di dati disporrà di record per ID di connessione che non sono più validi.</span><span class="sxs-lookup"><span data-stu-id="85e98-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="85e98-170">Per eliminare questi record orfani, si desidera invalidare qualsiasi connessione che è stato creato all'esterno di un intervallo di tempo è rilevante per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="85e98-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="85e98-171">Gli esempi in questa sezione includono un valore per il rilevamento quando la connessione è stata creata, ma non mostrano come pulire i vecchi record perché è consigliabile eseguire questa operazione come processo in background.</span><span class="sxs-lookup"><span data-stu-id="85e98-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="85e98-172">Database</span><span class="sxs-lookup"><span data-stu-id="85e98-172">Database</span></span>

<span data-ttu-id="85e98-173">Nell'esempio seguente viene illustrato come mantenere le informazioni di connessione e l'utente in un database.</span><span class="sxs-lookup"><span data-stu-id="85e98-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="85e98-174">È possibile usare qualsiasi tecnologia di accesso ai dati; Tuttavia, nell'esempio seguente viene illustrato come definire i modelli tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="85e98-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="85e98-175">Questi modelli di entità corrispondono ai campi e tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="85e98-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="85e98-176">La struttura dei dati può variare notevolmente a seconda dei requisiti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="85e98-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="85e98-177">Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a tutte le entità di connessione.</span><span class="sxs-lookup"><span data-stu-id="85e98-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="85e98-178">Dall'hub, quindi, è possibile rilevare lo stato di ogni connessione con il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="85e98-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="85e98-179">Archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="85e98-179">Azure table storage</span></span>

<span data-ttu-id="85e98-180">Nell'esempio seguente viene archiviazione tabelle di Azure è simile all'esempio di database.</span><span class="sxs-lookup"><span data-stu-id="85e98-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="85e98-181">Non include tutte le informazioni necessarie per iniziare a utilizzare il servizio di archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="85e98-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="85e98-182">Per informazioni, vedere [come usare l'archiviazione di tabelle da .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="85e98-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="85e98-183">L'esempio seguente mostra un'entità di tabella per archiviare le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="85e98-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="85e98-184">I dati vengono partizionati in base al nome utente e identifica ogni entità dall'id di connessione, in modo che un utente può avere più connessioni in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="85e98-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="85e98-185">Nell'hub, registrare lo stato di ogni connessione utente.</span><span class="sxs-lookup"><span data-stu-id="85e98-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
