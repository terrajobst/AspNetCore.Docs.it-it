---
title: Redis backplane per ASP.NET Core SignalR. scale-out
author: tdykstra
description: Informazioni su come configurare un backplane di Redis per abilitare la scalabilità orizzontale per un'app ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: 343cb5b2c7ed7162bae7865553a783fea45f0cfb
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284468"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a>Configurare un backplane di Redis per ASP.NET Core SignalR. scale-out

Dal [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), e [Tom Dykstra](https://github.com/tdykstra),

Questo articolo descrive aspetti di SignalR specifiche della configurazione di un [Redis](https://redis.io/) server da utilizzare per scalabilità orizzontale di un'app ASP.NET Core SignalR.

## <a name="set-up-a-redis-backplane"></a>Configurare un backplane di Redis

* Distribuire un server Redis.

  Per la produzione backplane di Redis è consigliato solo per l'infrastruttura locale. Per ridurre al minimo la latenza, il server Redis deve essere nello stesso data center dell'App per SignalR. Se l'app di SignalR è in esecuzione nel cloud di Azure, servizio Azure SignalR è consigliabile invece un backplane di Redis. È possibile usare il servizio Cache Redis di Azure per lo sviluppo e ambienti di test. Per altre informazioni, vedere le seguenti risorse:

  * <xref:signalr/scale>
  * [Documentazione di redis](https://redis.io/)
  * [Documentazione di Cache Redis di Azure](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* Nell'app SignalR, installare il `Microsoft.AspNetCore.SignalR.Redis` pacchetto NuGet. (È anche disponibile un `Microsoft.AspNetCore.SignalR.StackExchangeRedis` creare un pacchetto, ma che uno è per ASP.NET Core 2.2 e versioni successive.)

* Nel `Startup.ConfigureServices` metodo, chiamare `AddRedis` dopo `AddSignalR`:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* Configurare le opzioni in base alle esigenze:
 
  La maggior parte delle opzioni possono essere impostate nella stringa di connessione o nel [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) oggetto. Le opzioni specificate in `ConfigurationOptions` eseguono l'override impostato nella stringa di connessione.

  Nell'esempio seguente viene illustrato come impostare le opzioni `ConfigurationOptions` oggetto. Questo esempio viene aggiunto un prefisso di canale in modo che più App possono condividere la stessa istanza di Redis, come illustrato nel passaggio seguente.

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  Nel codice precedente, `options.Configuration` viene inizializzata con qualsiasi elemento è stato specificato nella stringa di connessione.

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* Nell'app SignalR, installare uno dei pacchetti NuGet seguenti:

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis` -Dipende 2.X.X stackexchange. Redis. Questo è il pacchetto consigliato per ASP.NET Core 2.2 e versioni successive.
  * `Microsoft.AspNetCore.SignalR.Redis` -Dipende 1.X.X stackexchange. Redis. Questo pacchetto non spediranno in ASP.NET Core 3.0.

* Nel `Startup.ConfigureServices` metodo, chiamare `AddStackExchangeRedis` dopo `AddSignalR`:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* Configurare le opzioni in base alle esigenze:
 
  La maggior parte delle opzioni possono essere impostate nella stringa di connessione o nel [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) oggetto. Le opzioni specificate in `ConfigurationOptions` eseguono l'override impostato nella stringa di connessione.

  Nell'esempio seguente viene illustrato come impostare le opzioni `ConfigurationOptions` oggetto. Questo esempio viene aggiunto un prefisso di canale in modo che più App possono condividere la stessa istanza di Redis, come illustrato nel passaggio seguente.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  Nel codice precedente, `options.Configuration` viene inizializzata con qualsiasi elemento è stato specificato nella stringa di connessione.

  Per informazioni sulle opzioni di Redis, vedere la [documentazione di StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

* Se si usa un server di Redis per più App SignalR, usare un prefisso di canale diversi per ogni app SignalR.

  L'impostazione di un prefisso di canale consente di isolare un'app di SignalR da altri utenti che usano i prefissi di canale diversi. Se non si assegnano prefissi diversi, un messaggio inviato da un'app a tutti i propri client verrà inviata a tutti i client di tutte le app che usano il server Redis come backplane.

* Configurare il server farm di bilanciamento del carico per le sessioni permanenti. Di seguito sono riportati alcuni esempi di documentazione su come eseguire questa operazione:

  * [IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Errori del server redis

Quando un server Redis diventa inattiva, SignalR genera eccezioni che indicano i messaggi non recapitati. Alcuni messaggi di eccezione tipico:

* *Scrittura non riuscita del messaggio*
* *Impossibile richiamare il metodo dell'hub 'MethodName'*
* *Impossibile stabilire la connessione a Redis*

SignalR non memorizzare nel buffer i messaggi di inviarli quando il server torna allo stato attivo. Tutti i messaggi inviati mentre il server Redis è inattivo vengono persi.

SignalR si riconnette automaticamente quando il server Redis è nuovamente disponibile.

### <a name="custom-behavior-for-connection-failures"></a>Comportamento personalizzato per gli errori di connessione

Ecco un esempio che illustra come gestire gli eventi di errore di connessione Redis.

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

## <a name="clustering"></a>Clustering

Il clustering è un metodo per ottenere una disponibilità elevata mediante più server di Redis. Il servizio cluster non è ufficialmente supportato, ma potrebbe funzionare.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* <xref:signalr/scale>
* [Documentazione di redis](https://redis.io/documentation)
* [Documentazione di StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/)
* [Documentazione di Cache Redis di Azure](https://docs.microsoft.com/en-us/azure/redis-cache/)
