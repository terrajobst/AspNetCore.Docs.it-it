---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mapping degli utenti di SignalR alle connessioni | Microsoft Docs
author: Rick-Anderson
description: In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni. Patrick Fletcher hanno contribuito alla scrittura di questo argomento. Versioni del software utilizzate in questo argomento...
ms.author: riande
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: f24d3a4828bc0863f8213e53f054aa7914edffad
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287764"
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="1359d-105">Mapping degli utenti di SignalR alle connessioni</span><span class="sxs-lookup"><span data-stu-id="1359d-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="1359d-106">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1359d-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="1359d-107">In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni.</span><span class="sxs-lookup"><span data-stu-id="1359d-107">This topic shows how to retain information about users and their connections.</span></span>
>
> <span data-ttu-id="1359d-108">Patrick Fletcher hanno contribuito alla scrittura di questo argomento.</span><span class="sxs-lookup"><span data-stu-id="1359d-108">Patrick Fletcher helped write this topic.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="1359d-109">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="1359d-109">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="1359d-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="1359d-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="1359d-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="1359d-111">.NET 4.5</span></span>
> - <span data-ttu-id="1359d-112">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="1359d-112">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="1359d-113">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="1359d-113">Previous versions of this topic</span></span>
>
> <span data-ttu-id="1359d-114">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="1359d-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="1359d-115">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="1359d-115">Questions and comments</span></span>
>
> <span data-ttu-id="1359d-116">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="1359d-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="1359d-117">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="1359d-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="introduction"></a><span data-ttu-id="1359d-118">Introduzione</span><span class="sxs-lookup"><span data-stu-id="1359d-118">Introduction</span></span>

<span data-ttu-id="1359d-119">Ogni client che si connette a un hub passa un id univoco della connessione. È possibile recuperare questo valore nel `Context.ConnectionId` proprietà del contesto dell'hub.</span><span class="sxs-lookup"><span data-stu-id="1359d-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="1359d-120">Se l'applicazione deve eseguire il mapping di un utente per l'id di connessione e mantenere tale mapping, è possibile usare uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="1359d-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="1359d-121">Il Provider di ID utente (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="1359d-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="1359d-122">[Archiviazione in memoria](#inmemory), ad esempio un dizionario</span><span class="sxs-lookup"><span data-stu-id="1359d-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="1359d-123">Gruppo di SignalR per ogni utente</span><span class="sxs-lookup"><span data-stu-id="1359d-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="1359d-124">[Archiviazione esterna permanente](#database), ad esempio una tabella di database o l'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="1359d-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="1359d-125">Ognuna di queste implementazioni è illustrato in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="1359d-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="1359d-126">Si utilizza il `OnConnected`, `OnDisconnected`, e `OnReconnected` metodi del `Hub` classe tenere traccia dello stato di connessione utente.</span><span class="sxs-lookup"><span data-stu-id="1359d-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="1359d-127">L'approccio migliore per l'applicazione dipende:</span><span class="sxs-lookup"><span data-stu-id="1359d-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="1359d-128">Il numero di server web che ospitano l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1359d-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="1359d-129">Indica se è necessario ottenere un elenco di utenti attualmente connessi.</span><span class="sxs-lookup"><span data-stu-id="1359d-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="1359d-130">Indica se è necessario rendere persistenti le informazioni di gruppo e utente quando l'applicazione o il server viene riavviato.</span><span class="sxs-lookup"><span data-stu-id="1359d-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="1359d-131">Indica se la latenza della chiamata a un server esterno è un problema.</span><span class="sxs-lookup"><span data-stu-id="1359d-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="1359d-132">La tabella seguente illustra l'approccio funziona per queste considerazioni.</span><span class="sxs-lookup"><span data-stu-id="1359d-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="1359d-133">Più di un server</span><span class="sxs-lookup"><span data-stu-id="1359d-133">More than one server</span></span> | <span data-ttu-id="1359d-134">Ottiene l'elenco di utenti attualmente connessi</span><span class="sxs-lookup"><span data-stu-id="1359d-134">Get list of currently connected users</span></span> | <span data-ttu-id="1359d-135">Rendere persistenti le informazioni dopo il riavvio</span><span class="sxs-lookup"><span data-stu-id="1359d-135">Persist information after restarts</span></span> | <span data-ttu-id="1359d-136">Ottenere prestazioni ottimali</span><span class="sxs-lookup"><span data-stu-id="1359d-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="1359d-137">Provider UserID</span><span class="sxs-lookup"><span data-stu-id="1359d-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="1359d-138">In memoria</span><span class="sxs-lookup"><span data-stu-id="1359d-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="1359d-139">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="1359d-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="1359d-140">Permanenti, esterni</span><span class="sxs-lookup"><span data-stu-id="1359d-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="1359d-141">Provider IUserID</span><span class="sxs-lookup"><span data-stu-id="1359d-141">IUserID provider</span></span>

<span data-ttu-id="1359d-142">Questa funzionalità consente agli utenti di specificare che cos'è l'ID utente basato su un IRequest tramite una nuova interfaccia IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="1359d-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="1359d-143">**Il IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="1359d-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="1359d-144">Per impostazione predefinita, vi sarà un'implementazione che usa l'utente `IPrincipal.Identity.Name` come nome utente.</span><span class="sxs-lookup"><span data-stu-id="1359d-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="1359d-145">Per modificare questa impostazione, registrare l'implementazione di `IUserIdProvider` con l'host globale all'avvio dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="1359d-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="1359d-146">All'interno di un hub, sarà in grado di inviare messaggi a questi utenti tramite l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="1359d-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="1359d-147">**Invia un messaggio a un utente specifico**</span><span class="sxs-lookup"><span data-stu-id="1359d-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="1359d-148">Archiviazione in memoria</span><span class="sxs-lookup"><span data-stu-id="1359d-148">In-memory storage</span></span>

<span data-ttu-id="1359d-149">Gli esempi seguenti illustrano come mantenere le informazioni di connessione e dell'utente in un dizionario che viene archiviato in memoria.</span><span class="sxs-lookup"><span data-stu-id="1359d-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="1359d-150">Il dizionario utilizza un `HashSet` per archiviare l'id di connessione. In qualsiasi momento un utente potrebbe avere più di una connessione all'applicazione SignalR.</span><span class="sxs-lookup"><span data-stu-id="1359d-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="1359d-151">Ad esempio, un utente che è connessa tramite più dispositivi o più di una scheda del browser avrebbe più di un id di connessione.</span><span class="sxs-lookup"><span data-stu-id="1359d-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="1359d-152">Se l'applicazione viene arrestata, tutte le informazioni vengono persi, ma verrà nuovamente popolato come gli utenti di ristabilire le connessioni.</span><span class="sxs-lookup"><span data-stu-id="1359d-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="1359d-153">Archiviazione in memoria non funziona se l'ambiente include più di un server web, poiché ogni server avrà una raccolta separata di connessioni.</span><span class="sxs-lookup"><span data-stu-id="1359d-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="1359d-154">Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti alle connessioni.</span><span class="sxs-lookup"><span data-stu-id="1359d-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="1359d-155">La chiave per il HashSet sarà il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="1359d-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="1359d-156">L'esempio successivo illustra come usare la classe di mapping di connessione da un hub.</span><span class="sxs-lookup"><span data-stu-id="1359d-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="1359d-157">L'istanza della classe è archiviata in un nome di variabile `_connections`.</span><span class="sxs-lookup"><span data-stu-id="1359d-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="1359d-158">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="1359d-158">Single-user groups</span></span>

<span data-ttu-id="1359d-159">È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si vuole raggiungere solo tale utente.</span><span class="sxs-lookup"><span data-stu-id="1359d-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="1359d-160">Il nome di ogni gruppo è il nome dell'utente.</span><span class="sxs-lookup"><span data-stu-id="1359d-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="1359d-161">Se un utente ha più di una connessione, ogni id di connessione viene aggiunto al gruppo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="1359d-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="1359d-162">È consigliabile non rimuovere manualmente l'utente dal gruppo quando l'utente si disconnette.</span><span class="sxs-lookup"><span data-stu-id="1359d-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="1359d-163">Questa azione viene eseguita automaticamente dal framework di SignalR.</span><span class="sxs-lookup"><span data-stu-id="1359d-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="1359d-164">Nell'esempio seguente viene illustrato come implementare i gruppi utente singolo.</span><span class="sxs-lookup"><span data-stu-id="1359d-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="1359d-165">Archiviazione esterna permanente</span><span class="sxs-lookup"><span data-stu-id="1359d-165">Permanent, external storage</span></span>

<span data-ttu-id="1359d-166">In questo argomento viene illustrato come utilizzare un database o un archivio tabelle di Azure per archiviare le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="1359d-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="1359d-167">Questo approccio funziona quando si dispone di più server web, poiché ogni server web può interagire con lo stesso repository di dati.</span><span class="sxs-lookup"><span data-stu-id="1359d-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="1359d-168">Se i server web arresta lavoro o del riavvio dell'applicazione, il `OnDisconnected` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="1359d-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="1359d-169">Pertanto, è possibile che il repository dei dati avrà i record per ID di connessione che non sono più valide.</span><span class="sxs-lookup"><span data-stu-id="1359d-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="1359d-170">Per eliminare tali record orfani, si consiglia di invalidare eventuali connessione creata di fuori di un intervallo di tempo è rilevante per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1359d-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="1359d-171">Gli esempi in questa sezione includono un valore per la registrazione quando la connessione è stata creata, ma non mostrano come pulire i record obsoleti poiché è possibile eseguire questa operazione come processo in background.</span><span class="sxs-lookup"><span data-stu-id="1359d-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="1359d-172">Database</span><span class="sxs-lookup"><span data-stu-id="1359d-172">Database</span></span>

<span data-ttu-id="1359d-173">Gli esempi seguenti illustrano come mantenere le informazioni di connessione e dell'utente in un database.</span><span class="sxs-lookup"><span data-stu-id="1359d-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="1359d-174">È possibile usare qualsiasi tecnologia di accesso ai dati; Tuttavia, l'esempio seguente viene illustrato come definire i modelli usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1359d-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="1359d-175">Questi modelli di entità corrispondono ai campi e tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="1359d-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="1359d-176">Struttura dei dati potrebbe variare notevolmente a seconda dei requisiti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1359d-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="1359d-177">Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a tutte le entità di connessione.</span><span class="sxs-lookup"><span data-stu-id="1359d-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="1359d-178">Quindi, dall'hub, è possibile monitorare lo stato di ogni connessione con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="1359d-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="1359d-179">Archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="1359d-179">Azure table storage</span></span>

<span data-ttu-id="1359d-180">L'esempio di archiviazione tabelle di Azure seguente è simile all'esempio del database.</span><span class="sxs-lookup"><span data-stu-id="1359d-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="1359d-181">Non include tutte le informazioni che occorre per iniziare a usare il servizio di archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="1359d-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="1359d-182">Per informazioni, vedere [come usare l'archiviazione tabelle da .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="1359d-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="1359d-183">L'esempio seguente illustra un'entità di tabella per archiviare le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="1359d-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="1359d-184">Partiziona i dati in base al nome utente e identifica ogni entità dall'id di connessione, in modo che un utente può avere più connessioni in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="1359d-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="1359d-185">Nell'hub, tenere traccia dello stato di ogni connessione utente.</span><span class="sxs-lookup"><span data-stu-id="1359d-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
