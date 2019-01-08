---
title: Redis backplane per ASP.NET Core SignalR. scale-out
author: tdykstra
description: Informazioni su come configurare un backplane di Redis per abilitare la scalabilità orizzontale per un'app ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: fde7f08ae2310b0b9bf9b7fe25a1dfc4a568adb6
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099355"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a><span data-ttu-id="c63c1-103">Configurare un backplane di Redis per ASP.NET Core SignalR. scale-out</span><span class="sxs-lookup"><span data-stu-id="c63c1-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="c63c1-104">Dal [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), e [Tom Dykstra](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="c63c1-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="c63c1-105">Questo articolo descrive aspetti di SignalR specifiche della configurazione di un [Redis](https://redis.io/) server da utilizzare per scalabilità orizzontale di un'app ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c63c1-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="c63c1-106">Configurare un backplane di Redis</span><span class="sxs-lookup"><span data-stu-id="c63c1-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="c63c1-107">Distribuire un server Redis.</span><span class="sxs-lookup"><span data-stu-id="c63c1-107">Deploy a Redis server.</span></span>

  > [!IMPORTANT] 
  > <span data-ttu-id="c63c1-108">Per la produzione backplane di Redis è consigliabile solo quando viene eseguita nello stesso data center dell'App per SignalR.</span><span class="sxs-lookup"><span data-stu-id="c63c1-108">For production use, a Redis backplane is recommended only when it runs in the same data center as the SignalR app.</span></span> <span data-ttu-id="c63c1-109">In caso contrario, la latenza di rete comporta una riduzione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="c63c1-109">Otherwise, network latency degrades performance.</span></span> <span data-ttu-id="c63c1-110">Se l'app di SignalR è in esecuzione nel cloud di Azure, servizio Azure SignalR è consigliabile invece un backplane di Redis.</span><span class="sxs-lookup"><span data-stu-id="c63c1-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="c63c1-111">È possibile usare il servizio Cache Redis di Azure per lo sviluppo e ambienti di test.</span><span class="sxs-lookup"><span data-stu-id="c63c1-111">You can use the Azure Redis Cache Service for development and test environments.</span></span>

  <span data-ttu-id="c63c1-112">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="c63c1-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="c63c1-113">Documentazione di redis</span><span class="sxs-lookup"><span data-stu-id="c63c1-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="c63c1-114">Documentazione di Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="c63c1-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="c63c1-115">Nell'app SignalR, installare il `Microsoft.AspNetCore.SignalR.Redis` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="c63c1-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span> <span data-ttu-id="c63c1-116">(È anche disponibile un `Microsoft.AspNetCore.SignalR.StackExchangeRedis` creare un pacchetto, ma che uno è per ASP.NET Core 2.2 e versioni successive.)</span><span class="sxs-lookup"><span data-stu-id="c63c1-116">(There is also a `Microsoft.AspNetCore.SignalR.StackExchangeRedis` package, but that one is for ASP.NET Core 2.2 and later.)</span></span>

* <span data-ttu-id="c63c1-117">Nel `Startup.ConfigureServices` metodo, chiamare `AddRedis` dopo `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="c63c1-117">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="c63c1-118">Configurare le opzioni in base alle esigenze:</span><span class="sxs-lookup"><span data-stu-id="c63c1-118">Configure options as needed:</span></span>
 
  <span data-ttu-id="c63c1-119">La maggior parte delle opzioni possono essere impostate nella stringa di connessione o nel [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) oggetto.</span><span class="sxs-lookup"><span data-stu-id="c63c1-119">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="c63c1-120">Le opzioni specificate in `ConfigurationOptions` eseguono l'override impostato nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="c63c1-120">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="c63c1-121">Nell'esempio seguente viene illustrato come impostare le opzioni `ConfigurationOptions` oggetto.</span><span class="sxs-lookup"><span data-stu-id="c63c1-121">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="c63c1-122">Questo esempio viene aggiunto un prefisso di canale in modo che più App possono condividere la stessa istanza di Redis, come illustrato nel passaggio seguente.</span><span class="sxs-lookup"><span data-stu-id="c63c1-122">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="c63c1-123">Nel codice precedente, `options.Configuration` viene inizializzata con qualsiasi elemento è stato specificato nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="c63c1-123">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="c63c1-124">Nell'app SignalR, installare uno dei pacchetti NuGet seguenti:</span><span class="sxs-lookup"><span data-stu-id="c63c1-124">In the SignalR app, install one of the following NuGet packages:</span></span>

  * <span data-ttu-id="c63c1-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` -Dipende 2.X.X stackexchange. Redis.</span><span class="sxs-lookup"><span data-stu-id="c63c1-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` - Depends on StackExchange.Redis 2.X.X.</span></span> <span data-ttu-id="c63c1-126">Questo è il pacchetto consigliato per ASP.NET Core 2.2 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="c63c1-126">This is the recommended package for ASP.NET Core 2.2 and later.</span></span>
  * <span data-ttu-id="c63c1-127">`Microsoft.AspNetCore.SignalR.Redis` -Dipende 1.X.X stackexchange. Redis.</span><span class="sxs-lookup"><span data-stu-id="c63c1-127">`Microsoft.AspNetCore.SignalR.Redis` - Depends on StackExchange.Redis 1.X.X.</span></span> <span data-ttu-id="c63c1-128">Questo pacchetto non spediranno in ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="c63c1-128">This package will not be shipping in ASP.NET Core 3.0.</span></span>

* <span data-ttu-id="c63c1-129">Nel `Startup.ConfigureServices` metodo, chiamare `AddStackExchangeRedis` dopo `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="c63c1-129">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="c63c1-130">Configurare le opzioni in base alle esigenze:</span><span class="sxs-lookup"><span data-stu-id="c63c1-130">Configure options as needed:</span></span>
 
  <span data-ttu-id="c63c1-131">La maggior parte delle opzioni possono essere impostate nella stringa di connessione o nel [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) oggetto.</span><span class="sxs-lookup"><span data-stu-id="c63c1-131">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="c63c1-132">Le opzioni specificate in `ConfigurationOptions` eseguono l'override impostato nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="c63c1-132">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="c63c1-133">Nell'esempio seguente viene illustrato come impostare le opzioni `ConfigurationOptions` oggetto.</span><span class="sxs-lookup"><span data-stu-id="c63c1-133">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="c63c1-134">Questo esempio viene aggiunto un prefisso di canale in modo che più App possono condividere la stessa istanza di Redis, come illustrato nel passaggio seguente.</span><span class="sxs-lookup"><span data-stu-id="c63c1-134">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="c63c1-135">Nel codice precedente, `options.Configuration` viene inizializzata con qualsiasi elemento è stato specificato nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="c63c1-135">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="c63c1-136">Per informazioni sulle opzioni di Redis, vedere la [documentazione di StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="c63c1-136">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="c63c1-137">Se si usa un server di Redis per più App SignalR, usare un prefisso di canale diversi per ogni app SignalR.</span><span class="sxs-lookup"><span data-stu-id="c63c1-137">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="c63c1-138">L'impostazione di un prefisso di canale consente di isolare un'app di SignalR da altri utenti che usano i prefissi di canale diversi.</span><span class="sxs-lookup"><span data-stu-id="c63c1-138">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="c63c1-139">Se non si assegnano prefissi diversi, un messaggio inviato da un'app a tutti i propri client verrà inviata a tutti i client di tutte le app che usano il server Redis come backplane.</span><span class="sxs-lookup"><span data-stu-id="c63c1-139">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="c63c1-140">Configurare il server farm di bilanciamento del carico per le sessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="c63c1-140">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="c63c1-141">Di seguito sono riportati alcuni esempi di documentazione su come eseguire questa operazione:</span><span class="sxs-lookup"><span data-stu-id="c63c1-141">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="c63c1-142">IIS</span><span class="sxs-lookup"><span data-stu-id="c63c1-142">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="c63c1-143">HAProxy</span><span class="sxs-lookup"><span data-stu-id="c63c1-143">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="c63c1-144">Nginx</span><span class="sxs-lookup"><span data-stu-id="c63c1-144">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="c63c1-145">pfSense</span><span class="sxs-lookup"><span data-stu-id="c63c1-145">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="c63c1-146">Errori del server redis</span><span class="sxs-lookup"><span data-stu-id="c63c1-146">Redis server errors</span></span>

<span data-ttu-id="c63c1-147">Quando un server Redis diventa inattiva, SignalR genera eccezioni che indicano i messaggi non recapitati.</span><span class="sxs-lookup"><span data-stu-id="c63c1-147">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="c63c1-148">Alcuni messaggi di eccezione tipico:</span><span class="sxs-lookup"><span data-stu-id="c63c1-148">Some typical exception messages:</span></span>

* <span data-ttu-id="c63c1-149">*Scrittura non riuscita del messaggio*</span><span class="sxs-lookup"><span data-stu-id="c63c1-149">*Failed writing message*</span></span>
* <span data-ttu-id="c63c1-150">*Impossibile richiamare il metodo dell'hub 'MethodName'*</span><span class="sxs-lookup"><span data-stu-id="c63c1-150">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="c63c1-151">*Impossibile stabilire la connessione a Redis*</span><span class="sxs-lookup"><span data-stu-id="c63c1-151">*Connection to Redis failed*</span></span>

<span data-ttu-id="c63c1-152">SignalR non memorizzare nel buffer i messaggi di inviarli quando il server torna allo stato attivo.</span><span class="sxs-lookup"><span data-stu-id="c63c1-152">SignalR doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="c63c1-153">Tutti i messaggi inviati mentre il server Redis è inattivo vengono persi.</span><span class="sxs-lookup"><span data-stu-id="c63c1-153">Any messages sent while the Redis server is down are lost.</span></span>

<span data-ttu-id="c63c1-154">SignalR si riconnette automaticamente quando il server Redis è nuovamente disponibile.</span><span class="sxs-lookup"><span data-stu-id="c63c1-154">SignalR automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="c63c1-155">Comportamento personalizzato per gli errori di connessione</span><span class="sxs-lookup"><span data-stu-id="c63c1-155">Custom behavior for connection failures</span></span>

<span data-ttu-id="c63c1-156">Ecco un esempio che illustra come gestire gli eventi di errore di connessione Redis.</span><span class="sxs-lookup"><span data-stu-id="c63c1-156">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="clustering"></a><span data-ttu-id="c63c1-157">Clustering</span><span class="sxs-lookup"><span data-stu-id="c63c1-157">Clustering</span></span>

<span data-ttu-id="c63c1-158">Il clustering è un metodo per ottenere una disponibilità elevata mediante più server di Redis.</span><span class="sxs-lookup"><span data-stu-id="c63c1-158">Clustering is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="c63c1-159">Il servizio cluster non è ufficialmente supportato, ma potrebbe funzionare.</span><span class="sxs-lookup"><span data-stu-id="c63c1-159">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c63c1-160">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c63c1-160">Next steps</span></span>

<span data-ttu-id="c63c1-161">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="c63c1-161">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="c63c1-162">Documentazione di redis</span><span class="sxs-lookup"><span data-stu-id="c63c1-162">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="c63c1-163">Documentazione di StackExchange Redis</span><span class="sxs-lookup"><span data-stu-id="c63c1-163">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="c63c1-164">Documentazione di Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="c63c1-164">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)
