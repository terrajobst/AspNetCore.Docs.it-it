---
title: Backplane Redis per ASP.NET Core SignalR con scalabilità orizzontale
author: bradygaster
description: Informazioni su come configurare un backplane Redis per abilitare la scalabilità orizzontale per un'app ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/redis-backplane
ms.openlocfilehash: 379d46fcaabb8eb0d04e521a5ad698229f947b7c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963914"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-opno-locsignalr-scale-out"></a><span data-ttu-id="df14c-103">Configurare un backplane Redis per ASP.NET Core SignalR con scalabilità orizzontale</span><span class="sxs-lookup"><span data-stu-id="df14c-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="df14c-104">Di [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster)e [Tom Dykstra](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="df14c-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="df14c-105">Questo articolo SignalRillustra gli aspetti specifici della configurazione di un server [Redis](https://redis.io/) da usare per la scalabilità orizzontale di un'app ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="df14c-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="df14c-106">Configurare un backplane Redis</span><span class="sxs-lookup"><span data-stu-id="df14c-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="df14c-107">Distribuire un server Redis.</span><span class="sxs-lookup"><span data-stu-id="df14c-107">Deploy a Redis server.</span></span>

  > [!IMPORTANT] 
  > <span data-ttu-id="df14c-108">Per l'uso in produzione, è consigliabile un backplane Redis solo quando viene eseguito nella stessa data center dell'app SignalR.</span><span class="sxs-lookup"><span data-stu-id="df14c-108">For production use, a Redis backplane is recommended only when it runs in the same data center as the SignalR app.</span></span> <span data-ttu-id="df14c-109">In caso contrario, la latenza di rete comporta un peggioramento delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="df14c-109">Otherwise, network latency degrades performance.</span></span> <span data-ttu-id="df14c-110">Se l'app SignalR è in esecuzione nel cloud di Azure, si consiglia il servizio Azure SignalR invece di un backplane Redis.</span><span class="sxs-lookup"><span data-stu-id="df14c-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="df14c-111">È possibile usare il servizio cache Redis di Azure per gli ambienti di sviluppo e test.</span><span class="sxs-lookup"><span data-stu-id="df14c-111">You can use the Azure Redis Cache Service for development and test environments.</span></span>

  <span data-ttu-id="df14c-112">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="df14c-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="df14c-113">Documentazione di redis</span><span class="sxs-lookup"><span data-stu-id="df14c-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="df14c-114">Documentazione di cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="df14c-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="df14c-115">Nell'app SignalR installare il pacchetto NuGet `Microsoft.AspNetCore.SignalR.Redis`.</span><span class="sxs-lookup"><span data-stu-id="df14c-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span> <span data-ttu-id="df14c-116">(Esiste anche un pacchetto di `Microsoft.AspNetCore.SignalR.StackExchangeRedis`, ma quello per ASP.NET Core 2,2 e versioni successive).</span><span class="sxs-lookup"><span data-stu-id="df14c-116">(There is also a `Microsoft.AspNetCore.SignalR.StackExchangeRedis` package, but that one is for ASP.NET Core 2.2 and later.)</span></span>

* <span data-ttu-id="df14c-117">Nel metodo `Startup.ConfigureServices` chiamare `AddRedis` dopo `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="df14c-117">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="df14c-118">Configurare le opzioni in base alle esigenze:</span><span class="sxs-lookup"><span data-stu-id="df14c-118">Configure options as needed:</span></span>
 
  <span data-ttu-id="df14c-119">La maggior parte delle opzioni può essere impostata nella stringa di connessione o nell'oggetto [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) .</span><span class="sxs-lookup"><span data-stu-id="df14c-119">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="df14c-120">Le opzioni specificate in `ConfigurationOptions` eseguono l'override di quelle impostate nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="df14c-120">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="df14c-121">Nell'esempio seguente viene illustrato come impostare le opzioni nell'oggetto `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="df14c-121">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="df14c-122">Questo esempio aggiunge un prefisso del canale in modo che più app possano condividere la stessa istanza di redis, come illustrato nel passaggio seguente.</span><span class="sxs-lookup"><span data-stu-id="df14c-122">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="df14c-123">Nel codice precedente, `options.Configuration` viene inizializzato con tutto ciò che è stato specificato nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="df14c-123">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="df14c-124">Nell'app SignalR installare uno dei pacchetti NuGet seguenti:</span><span class="sxs-lookup"><span data-stu-id="df14c-124">In the SignalR app, install one of the following NuGet packages:</span></span>

  * <span data-ttu-id="df14c-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis`-dipende da StackExchange. Redis 2. X.X.</span><span class="sxs-lookup"><span data-stu-id="df14c-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` - Depends on StackExchange.Redis 2.X.X.</span></span> <span data-ttu-id="df14c-126">Questo è il pacchetto consigliato per ASP.NET Core 2,2 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="df14c-126">This is the recommended package for ASP.NET Core 2.2 and later.</span></span>
  * <span data-ttu-id="df14c-127">`Microsoft.AspNetCore.SignalR.Redis`-dipende da StackExchange. Redis 1. X.X.</span><span class="sxs-lookup"><span data-stu-id="df14c-127">`Microsoft.AspNetCore.SignalR.Redis` - Depends on StackExchange.Redis 1.X.X.</span></span> <span data-ttu-id="df14c-128">Il pacchetto non verrà distribuito in ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="df14c-128">This package will not be shipping in ASP.NET Core 3.0.</span></span>

* <span data-ttu-id="df14c-129">Nel metodo `Startup.ConfigureServices` chiamare `AddStackExchangeRedis` dopo `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="df14c-129">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="df14c-130">Configurare le opzioni in base alle esigenze:</span><span class="sxs-lookup"><span data-stu-id="df14c-130">Configure options as needed:</span></span>
 
  <span data-ttu-id="df14c-131">La maggior parte delle opzioni può essere impostata nella stringa di connessione o nell'oggetto [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) .</span><span class="sxs-lookup"><span data-stu-id="df14c-131">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="df14c-132">Le opzioni specificate in `ConfigurationOptions` eseguono l'override di quelle impostate nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="df14c-132">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="df14c-133">Nell'esempio seguente viene illustrato come impostare le opzioni nell'oggetto `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="df14c-133">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="df14c-134">Questo esempio aggiunge un prefisso del canale in modo che più app possano condividere la stessa istanza di redis, come illustrato nel passaggio seguente.</span><span class="sxs-lookup"><span data-stu-id="df14c-134">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="df14c-135">Nel codice precedente, `options.Configuration` viene inizializzato con tutto ciò che è stato specificato nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="df14c-135">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="df14c-136">Per informazioni sulle opzioni di redis, vedere la [documentazione su Redis di stackexchange](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="df14c-136">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="df14c-137">Se si usa un server Redis per più app SignalR, usare un prefisso di canale diverso per ogni app SignalR.</span><span class="sxs-lookup"><span data-stu-id="df14c-137">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="df14c-138">L'impostazione di un prefisso del canale isola una SignalR app da altre che usano prefissi di canale diversi.</span><span class="sxs-lookup"><span data-stu-id="df14c-138">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="df14c-139">Se non si assegnano prefissi diversi, un messaggio inviato da un'app a tutti i relativi client passerà a tutti i client di tutte le app che usano il server Redis come backplane.</span><span class="sxs-lookup"><span data-stu-id="df14c-139">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="df14c-140">Configurare il software di bilanciamento del carico server farm per le sessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="df14c-140">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="df14c-141">Di seguito sono riportati alcuni esempi di documentazione su come eseguire questa operazione:</span><span class="sxs-lookup"><span data-stu-id="df14c-141">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="df14c-142">IIS</span><span class="sxs-lookup"><span data-stu-id="df14c-142">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="df14c-143">HAProxy</span><span class="sxs-lookup"><span data-stu-id="df14c-143">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="df14c-144">Nginx</span><span class="sxs-lookup"><span data-stu-id="df14c-144">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="df14c-145">pfSense</span><span class="sxs-lookup"><span data-stu-id="df14c-145">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="df14c-146">Errori del server Redis</span><span class="sxs-lookup"><span data-stu-id="df14c-146">Redis server errors</span></span>

<span data-ttu-id="df14c-147">Quando un server Redis diventa inattivo, SignalR genera eccezioni che indicano che i messaggi non vengono recapitati.</span><span class="sxs-lookup"><span data-stu-id="df14c-147">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="df14c-148">Alcuni messaggi di eccezione tipici:</span><span class="sxs-lookup"><span data-stu-id="df14c-148">Some typical exception messages:</span></span>

* <span data-ttu-id="df14c-149">*Scrittura del messaggio non riuscita*</span><span class="sxs-lookup"><span data-stu-id="df14c-149">*Failed writing message*</span></span>
* <span data-ttu-id="df14c-150">*Non è stato possibile richiamare il metodo Hub ' MethodName '*</span><span class="sxs-lookup"><span data-stu-id="df14c-150">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="df14c-151">*Connessione a Redis non riuscita*</span><span class="sxs-lookup"><span data-stu-id="df14c-151">*Connection to Redis failed*</span></span>

SignalR<span data-ttu-id="df14c-152"> non memorizza nel buffer i messaggi per inviarli quando viene eseguito il backup del server.</span><span class="sxs-lookup"><span data-stu-id="df14c-152"> doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="df14c-153">Tutti i messaggi inviati durante il server Redis vengono persi.</span><span class="sxs-lookup"><span data-stu-id="df14c-153">Any messages sent while the Redis server is down are lost.</span></span>

SignalR<span data-ttu-id="df14c-154"> si riconnette automaticamente quando il server Redis è nuovamente disponibile.</span><span class="sxs-lookup"><span data-stu-id="df14c-154"> automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="df14c-155">Comportamento personalizzato per gli errori di connessione</span><span class="sxs-lookup"><span data-stu-id="df14c-155">Custom behavior for connection failures</span></span>

<span data-ttu-id="df14c-156">Ecco un esempio che illustra come gestire gli eventi di errore di connessione Redis.</span><span class="sxs-lookup"><span data-stu-id="df14c-156">Here's an example that shows how to handle Redis connection failure events.</span></span>

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

::: moniker range="> aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddMessagePackProtocol()
        .AddStackExchangeRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

## <a name="redis-clustering"></a><span data-ttu-id="df14c-157">Clustering di redis</span><span class="sxs-lookup"><span data-stu-id="df14c-157">Redis Clustering</span></span>

<span data-ttu-id="df14c-158">Il [clustering di redis](https://redis.io/topics/cluster-spec) è un metodo per ottenere la disponibilità elevata usando più server Redis.</span><span class="sxs-lookup"><span data-stu-id="df14c-158">[Redis Clustering](https://redis.io/topics/cluster-spec) is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="df14c-159">Il clustering non è ufficialmente supportato, ma potrebbe funzionare.</span><span class="sxs-lookup"><span data-stu-id="df14c-159">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="df14c-160">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="df14c-160">Next steps</span></span>

<span data-ttu-id="df14c-161">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="df14c-161">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="df14c-162">Documentazione di redis</span><span class="sxs-lookup"><span data-stu-id="df14c-162">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="df14c-163">Documentazione di StackExchange Redis</span><span class="sxs-lookup"><span data-stu-id="df14c-163">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="df14c-164">Documentazione di cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="df14c-164">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)
