---
title: Utilizzo di una Cache distribuita
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 870f082d-6d43-453d-b311-45f3aeb4d2c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/distributed
ms.openlocfilehash: 09a1a30de38b9eb40d4fa6a684a7d43ac3e0413c
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-a-distributed-cache"></a><span data-ttu-id="c8d2f-103">Utilizzo di una cache distribuita</span><span class="sxs-lookup"><span data-stu-id="c8d2f-103">Working with a distributed cache</span></span>

<span data-ttu-id="c8d2f-104">Da [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="c8d2f-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="c8d2f-105">Le cache distribuite possono migliorare le prestazioni e scalabilità delle applicazioni ASP.NET di base, soprattutto quando è ospitato in un ambiente di farm di server o cloud.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="c8d2f-106">In questo articolo viene illustrato come utilizzare le implementazioni e astrazioni di cache distribuita incorporata del ASP.NET di base.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

[<span data-ttu-id="c8d2f-107">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="c8d2f-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample)

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="c8d2f-108">Che cos'è una Cache distribuita</span><span class="sxs-lookup"><span data-stu-id="c8d2f-108">What is a Distributed Cache</span></span>

<span data-ttu-id="c8d2f-109">Una cache distribuita è condiviso da più server di app (vedere [nozioni fondamentali di memorizzazione nella cache](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="c8d2f-109">A distributed cache is shared by multiple app servers (see [Caching Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="c8d2f-110">Le informazioni nella cache non vengono archiviate nella memoria del server web singole e i dati memorizzati nella cache sono disponibili a tutti i server dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-110">The information in the cache is not stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="c8d2f-111">Ciò offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="c8d2f-111">This provides several advantages:</span></span>

1. <span data-ttu-id="c8d2f-112">Dati memorizzati nella cache sono coerenti in tutti i server web.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="c8d2f-113">Gli utenti non visualizzati risultati diversi a seconda di quale web server gestisce la richiesta</span><span class="sxs-lookup"><span data-stu-id="c8d2f-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="c8d2f-114">Dati memorizzati nella cache viene conservata riavvio del server web e le distribuzioni.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="c8d2f-115">Server web singoli possono essere rimossi o aggiunti senza conseguenze per la cache.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="c8d2f-116">L'archivio di dati di origine ha meno le richieste effettuate a esso (non più in memoria cache o no cache affatto).</span><span class="sxs-lookup"><span data-stu-id="c8d2f-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="c8d2f-117">Se si usa una Cache distribuita di SQL Server, solo alcuni di questi vantaggi sono true se un'istanza separata del database viene utilizzata per la cache di per i dati di origine dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="c8d2f-118">Ad esempio una cache, una cache distribuita può migliorare notevolmente la velocità di risposta di un'app, poiché in genere i dati possono essere recuperati dalla cache più veloce da un database relazionale (o un servizio web).</span><span class="sxs-lookup"><span data-stu-id="c8d2f-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="c8d2f-119">Configurazione della cache è specifica dell'implementazione.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="c8d2f-120">In questo articolo viene descritto come configurare entrambi Redis e distribuite di SQL Server memorizza nella cache.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="c8d2f-121">Indipendentemente dall'implementazione è selezionata, l'applicazione interagisce con la cache utilizzando una comune `IDistributedCache` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="c8d2f-122">L'interfaccia IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="c8d2f-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="c8d2f-123">Il `IDistributedCache` interfaccia include metodi sincroni e asincroni.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="c8d2f-124">L'interfaccia consente gli elementi da aggiungere, recuperare e rimossi dall'implementazione della cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="c8d2f-125">Il `IDistributedCache` interfaccia include i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8d2f-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="c8d2f-126">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="c8d2f-126">**Get, GetAsync**</span></span>

<span data-ttu-id="c8d2f-127">Accetta una chiave di stringa e recupera un elemento memorizzato nella cache come una `byte[]` se trovato nella cache.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="c8d2f-128">**Set di SetAsync**</span><span class="sxs-lookup"><span data-stu-id="c8d2f-128">**Set, SetAsync**</span></span>

<span data-ttu-id="c8d2f-129">Aggiunge un elemento (come `byte[]`) alla cache utilizzando una chiave di stringa.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="c8d2f-130">**Aggiornamento, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="c8d2f-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="c8d2f-131">Aggiorna un elemento nella cache in base alla relativa chiave, reimpostare il timeout di scadenza scorrevole (se presente).</span><span class="sxs-lookup"><span data-stu-id="c8d2f-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="c8d2f-132">**Rimuovere, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="c8d2f-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="c8d2f-133">Rimuove una voce della cache in base alla relativa chiave.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="c8d2f-134">Utilizzare il `IDistributedCache` interfaccia:</span><span class="sxs-lookup"><span data-stu-id="c8d2f-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="c8d2f-135">Aggiungere i pacchetti NuGet necessari al file di progetto.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="c8d2f-136">Configurare l'implementazione specifica di `IDistributedCache` nel `Startup` della classe `ConfigureServices` (metodo) e aggiungerlo al contenitore di.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="c8d2f-137">L'App [`Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache' dal costruttore.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-137">From the app's [`Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache\` from the constructor.</span></span> <span data-ttu-id="c8d2f-138">L'istanza verrà fornito da [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="c8d2f-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="c8d2f-139">Non è necessario utilizzare una durata nell'ambito o Singleton per `IDistributedCache` istanze (almeno per le implementazioni predefinite).</span><span class="sxs-lookup"><span data-stu-id="c8d2f-139">There is no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="c8d2f-140">È inoltre possibile creare un'istanza ogni volta che potrebbe essere necessario uno (anziché [Dependency Injection](../../fundamentals/dependency-injection.md)), ma ciò può rendere più difficile da testare, il codice e viola il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="c8d2f-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="c8d2f-141">Nell'esempio seguente viene illustrato come utilizzare un'istanza di `IDistributedCache` in un componente del middleware semplice:</span><span class="sxs-lookup"><span data-stu-id="c8d2f-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

<span data-ttu-id="c8d2f-142">[!code-csharp[Principale](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]</span><span class="sxs-lookup"><span data-stu-id="c8d2f-142">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]</span></span>

<span data-ttu-id="c8d2f-143">Nel codice precedente, il valore memorizzato nella cache viene letto ma non è stato scritto.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-143">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="c8d2f-144">In questo esempio, il valore viene impostato solo quando un server viene avviato e non cambia.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-144">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="c8d2f-145">In uno scenario con più server, il server più recente per l'avvio sovrascriverà i valori precedenti che sono stati impostati da altri server.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-145">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="c8d2f-146">Il `Get` e `Set` metodi utilizzano il `byte[]` tipo.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-146">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="c8d2f-147">Pertanto, il valore di stringa deve essere convertito utilizzando `Encoding.UTF8.GetString` (per `Get`) e `Encoding.UTF8.GetBytes` (per `Set`).</span><span class="sxs-lookup"><span data-stu-id="c8d2f-147">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="c8d2f-148">Nell'esempio di codice da *Startup.cs* Mostra il valore da impostare:</span><span class="sxs-lookup"><span data-stu-id="c8d2f-148">The following code from *Startup.cs* shows the value being set:</span></span>

<span data-ttu-id="c8d2f-149">[!code-csharp[Principale](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]</span><span class="sxs-lookup"><span data-stu-id="c8d2f-149">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]</span></span>

> [!NOTE]
> <span data-ttu-id="c8d2f-150">Poiché `IDistributedCache` è configurato nel `ConfigureServices` (metodo), è disponibile per il `Configure` metodo come parametro.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-150">Since `IDistributedCache` is configured in the `ConfigureServices` method, it is available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="c8d2f-151">Aggiungerlo come parametro consentirà l'istanza configurata essere fornito mediante DI.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-151">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="c8d2f-152">Tramite un Redis Cache distribuita</span><span class="sxs-lookup"><span data-stu-id="c8d2f-152">Using a Redis Distributed Cache</span></span>

<span data-ttu-id="c8d2f-153">[Redis](http://redis.io) è un archivio Apri origine dati in memoria, che viene spesso utilizzato come una cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-153">[Redis](http://redis.io) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="c8d2f-154">È possibile usare in locale e, è possibile configurare un [Cache Redis di Azure](https://azure.microsoft.com/services/cache/) per le app ASP.NET Core ospitato in Azure.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-154">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="c8d2f-155">L'app ASP.NET Core consente di configurare l'implementazione di cache con un `RedisDistributedCache` istanza.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-155">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="c8d2f-156">Configurare l'implementazione di Redis in `ConfigureServices` e diritti di accesso nel codice dell'app richiedendo un'istanza di `IDistributedCache` (vedere il codice precedente).</span><span class="sxs-lookup"><span data-stu-id="c8d2f-156">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="c8d2f-157">Nell'esempio di codice, un `RedisCache` implementazione viene utilizzata quando il server è configurato per un `Staging` ambiente.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-157">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="c8d2f-158">In questo modo il `ConfigureStagingServices` metodo consente di configurare il `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="c8d2f-158">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

<span data-ttu-id="c8d2f-159">[!code-csharp[Principale](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]</span><span class="sxs-lookup"><span data-stu-id="c8d2f-159">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]</span></span>

> [!NOTE]
> <span data-ttu-id="c8d2f-160">Per installare Redis sul computer locale, installare il pacchetto chocolatey [http://chocolatey.org/packages/redis-64/](http://chocolatey.org/packages/redis-64/) ed eseguire `redis-server` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-160">To install Redis on your local machine, install the chocolatey package [http://chocolatey.org/packages/redis-64/](http://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="c8d2f-161">Utilizzo di un Server SQL Cache distribuita</span><span class="sxs-lookup"><span data-stu-id="c8d2f-161">Using a SQL Server Distributed Cache</span></span>

<span data-ttu-id="c8d2f-162">L'implementazione di SqlServerCache consente la cache distribuita utilizzare un database di SQL Server come archivio di backup.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-162">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="c8d2f-163">Per creare il Server SQL table è possibile utilizzare lo strumento della cache sql, lo strumento crea una tabella con il nome e lo schema specificato.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-163">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

<span data-ttu-id="c8d2f-164">Per utilizzare lo strumento della cache sql, aggiungere `SqlConfig.Tools` per il `<ItemGroup>` elemento del *csproj* file ed eseguire il ripristino dotnet.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-164">To use the sql-cache tool, add `SqlConfig.Tools` to the `<ItemGroup>` element of the *.csproj* file and run dotnet restore.</span></span>

<span data-ttu-id="c8d2f-165">[!code-csharp[Principale](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]</span><span class="sxs-lookup"><span data-stu-id="c8d2f-165">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]</span></span>

<span data-ttu-id="c8d2f-166">Test SqlConfig.Tools eseguendo il comando seguente</span><span class="sxs-lookup"><span data-stu-id="c8d2f-166">Test SqlConfig.Tools by running the following command</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

<span data-ttu-id="c8d2f-167">strumento della cache SQL visualizzano la Guida di utilizzo e le opzioni di comando, ora è possibile creare tabelle in sql server, eseguire "Creazione di cache di sql" comando:</span><span class="sxs-lookup"><span data-stu-id="c8d2f-167">sql-cache tool  will display usage, options and command help, now you can create tables into sql server, running "sql-cache create" command :</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

<span data-ttu-id="c8d2f-168">La tabella creata con lo schema seguente:</span><span class="sxs-lookup"><span data-stu-id="c8d2f-168">The created table has the following schema:</span></span>

![Tabella della Cache di SQL Server](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="c8d2f-170">Come tutte le implementazioni di cache, l'app deve ottenere e impostare i valori della cache utilizzando un'istanza di `IDistributedCache`, non un `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-170">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="c8d2f-171">L'esempio implementa `SqlServerCache` nel `Production` ambiente (in modo che è configurato `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="c8d2f-171">The sample implements `SqlServerCache` in the `Production` environment (so it is configured in `ConfigureProductionServices`).</span></span>

<span data-ttu-id="c8d2f-172">[!code-csharp[Principale](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]</span><span class="sxs-lookup"><span data-stu-id="c8d2f-172">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]</span></span>

> [!NOTE]
> <span data-ttu-id="c8d2f-173">Il `ConnectionString` (e, facoltativamente, `SchemaName` e `TableName`) in genere devono essere archiviate di fuori di controllo del codice sorgente (ad esempio segreti utente), poiché potrebbero contenere le credenziali.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-173">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="c8d2f-174">Suggerimenti</span><span class="sxs-lookup"><span data-stu-id="c8d2f-174">Recommendations</span></span>

<span data-ttu-id="c8d2f-175">Quando si decide quale implementazione di `IDistributedCache` diritto per l'app, scegliere tra Redis e SQL Server in base all'infrastruttura esistente e ambiente, i requisiti di prestazioni ed esperienza del team.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-175">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="c8d2f-176">Se il team è più in grado di utilizzare con Redis, è un'ottima scelta.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-176">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="c8d2f-177">Se SQL Server preferita dal team, è possibile la certezza che anche l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-177">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="c8d2f-178">Si noti che una soluzione di memorizzazione nella cache tradizionale archivia dati in memoria che consente il recupero veloce dei dati.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-178">Note that A traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="c8d2f-179">È necessario archiviare i dati utilizzati in una cache e archiviare tutti i dati in un archivio permanente di back-end, ad esempio SQL Server o di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-179">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="c8d2f-180">Cache redis è una soluzione di memorizzazione nella cache che consente una velocità effettiva elevata e latenza bassa rispetto alle Cache SQL.</span><span class="sxs-lookup"><span data-stu-id="c8d2f-180">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

<span data-ttu-id="c8d2f-181">Risorse aggiuntive:</span><span class="sxs-lookup"><span data-stu-id="c8d2f-181">Additional resources:</span></span>

* [<span data-ttu-id="c8d2f-182">Nella memorizzazione nella cache</span><span class="sxs-lookup"><span data-stu-id="c8d2f-182">In memory caching</span></span>](memory.md)
* [<span data-ttu-id="c8d2f-183">Redis Cache in Azure</span><span class="sxs-lookup"><span data-stu-id="c8d2f-183">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="c8d2f-184">Database SQL in Azure</span><span class="sxs-lookup"><span data-stu-id="c8d2f-184">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
