---
title: Implementazione del server web kestrel in ASP.NET Core
author: tdykstra
description: Introduce Kestrel, il server web multipiattaforma per ASP.NET Core in base a libuv.
keywords: ASP.NET Core, Kestrel, libuv, i prefissi url
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 451c36fc9095b6e076e5287c992b6163205c523b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Introduzione all'implementazione di server web Kestrel in ASP.NET Core

Da [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), e [Stephen Halter](https://twitter.com/halter73)

Kestrel è una multipiattaforma [server web per ASP.NET Core](index.md) in base a [libuv](https://github.com/libuv/libuv), una libreria dei / o asincroni multipiattaforma. Kestrel è il server web che è incluso per impostazione predefinita nei modelli di progetto ASP.NET Core. 

Kestrel supporta le funzionalità seguenti:

  * HTTPS
  * Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)
  * Socket ad alte prestazioni dietro Nginx UNIX 

Kestrel è supportata in tutte le piattaforme e le versioni che supporta .NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Consente di visualizzare o scaricare il codice di esempio per 2. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Consente di visualizzare o scaricare il codice di esempio per 1. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Quando utilizzare Kestrel con un proxy inverso

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

È possibile usare Kestrel da solo o con un *server proxy inverso*, ad esempio IIS, Nginx o Apache. Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

È possibile usare una delle due configurazioni &mdash; con o senza un server proxy inverso &mdash; anche se Kestrel viene esposto solo a una rete interna.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se l'applicazione accetta le richieste solo da una rete interna, è possibile usare Kestrel da solo.

![Kestrel comunica direttamente con la rete interna](kestrel/_static/kestrel-to-internal.png)

Se si espone l'applicazione a Internet, è necessario usare IIS, Nginx o Apache come *server proxy inverso*. Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

Un proxy inverso è obbligatorio per le distribuzioni di edge (esposte per il traffico da Internet) per motivi di sicurezza. Nelle versioni 1. x di Kestrel non è disponibile una serie completa di difese contro gli attacchi. Queste comprendono, ma non è limitata a timeout appropriato, limiti di dimensioni e i limiti di connessioni simultanee.

---

Uno scenario che richiede un proxy inverso è quando si dispone di più applicazioni che condividono lo stesso IP e porta in esecuzione in un singolo server. Che non funziona con Kestrel direttamente poiché Kestrel non supporta lo stesso IP e porta tra più processi di condivisione. Quando si configura Kestrel per l'ascolto su una porta, gestisce tutto il traffico per tale porta indipendentemente dall'intestazione host. Un proxy inverso che può condividere porte è necessario inoltrare quindi a Kestrel su una porta e un IP univoco.

Anche se non è necessario un server proxy inverso, utilizzando uno potrebbe essere una buona scelta per altri motivi:

* È possibile limitare la superficie esposta.
* Fornisce un ulteriore livello facoltativo di configurazione e protezione avanzata.
* Potrebbe essere migliore integrazione con l'infrastruttura esistente.
* Semplifica il bilanciamento del carico e configurazione SSL. Solo il server di proxy inverso richiede un certificato SSL e tale server può comunicare con il server applicazioni nella rete interna tramite HTTP semplice.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Come usare Kestrel nelle applicazioni ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) incluso nel pacchetto di [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

Modelli di progetto ASP.NET Core utilizzano Kestrel per impostazione predefinita. In *Program.cs*, il codice chiama modello `CreateDefaultBuilder`, che chiama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) dietro le quinte.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Se è necessario configurare le opzioni Kestrel, chiamare `UseKestrel` in *Program.cs* come illustrato nell'esempio seguente:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Installare il [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pacchetto NuGet.

Chiamare il [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) metodo di estensione su `WebHostBuilder` nel `Main` metodo, specificando uno [opzioni Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) che è necessario, come illustrato nella sezione successiva.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Opzioni kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il server web Kestrel dispone di opzioni di configurazione del vincolo che risultano particolarmente utili nelle distribuzioni con connessione Internet. Ecco alcuni dei limiti di cui che è possibile impostare:

- Numero massimo di connessioni client
- Dimensione massima del corpo della richiesta
- Velocità minima dei dati del corpo della richiesta

Impostare questi vincoli e gli altri il `Limits` proprietà del [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) classe. Il `Limits` proprietà contiene un'istanza di [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) classe. 

**Connessioni client massima**

Per l'intera applicazione con il codice seguente, è possibile impostare il numero massimo di connessioni simultanee aperte TCP:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

È previsto un limite separato per le connessioni che sono stati aggiornati da HTTP o HTTPS a un altro protocollo (ad esempio, in una richiesta WebSocket). Dopo l'aggiornamento di una connessione, non è stato preso in considerazione il `MaxConcurrentConnections` limite. 

Il numero massimo di connessioni è illimitato (null) per impostazione predefinita.

**Dimensione massima del corpo della richiesta**

Dimensioni predefinite del corpo massima della richiesta sono 30,000,000 byte, ovvero circa 28,6 MB. 

Il metodo consigliato per ignorare il limite in un'applicazione ASP.NET MVC di base consiste nell'utilizzare il [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attributo in un metodo di azione:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Di seguito è riportato un esempio che illustra come configurare il vincolo per l'intera applicazione, ogni richiesta:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

È possibile sostituire l'impostazione di una richiesta nel middleware specifica:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Se si tenta di configurare il limite per una richiesta dopo l'applicazione ha avviato la richiesta di lettura, viene generata un'eccezione. È presente un `IsReadOnly` proprietà che indica se il `MaxRequestBodySize` proprietà è in stato di sola lettura, pertanto è troppo tardi per configurare il limite.

**Velocità di dati minima richiesta del corpo**

Kestrel controlla ogni secondo i vocabolari alla frequenza specificata in byte al secondo. Se la velocità scende sotto il valore minimo, la connessione è scaduta. Il periodo di tolleranza è la quantità di tempo che Kestrel fornisce al client per aumentare la velocità di trasmissione fino a minimo. la frequenza non è selezionata questo periodo di tempo. Il periodo di prova consente di evitare l'eliminazione di connessioni che sono inizialmente l'invio dei dati a una velocità lenta a causa di TCP avvio lento.

La frequenza minima predefinita è 240 byte al secondo, con un periodo di tolleranza di 5 secondi.

Una frequenza minima si applica anche alla risposta. Il codice per impostare il limite di richieste e il limite di risposta è lo stesso, ad eccezione che `RequestBody` o `Response` nei nomi di proprietà e di interfaccia. 

Di seguito è riportato un esempio che illustra come configurare i tassi di dati minimo nel *Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

È possibile configurare le frequenze per ogni richiesta nel middleware:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Per informazioni sulle altre opzioni Kestrel, vedere le classi seguenti:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Per informazioni sulle opzioni di Kestrel, vedere [KestrelServerOptions classe](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Configurazione dell'endpoint

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Per impostazione predefinita ASP.NET Core associa a `http://localhost:5000`. Configurare i prefissi URL e delle porte per Kestrel in ascolto su chiamando `Listen` o `ListenUnixSocket` metodi su `KestrelServerOptions`. (`UseUrls`, `urls` argomento della riga di comando, mentre la variabile di ambiente ASPNETCORE_URLS funzionano anche ma presentano le limitazioni indicate [più avanti in questo articolo](#useurls-limitations).)

**Associare a un socket TCP**

Il `Listen` metodo associa a un socket TCP, e un'espressione lambda opzioni consente di configurare un certificato SSL:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Si noti come in questo esempio consente di configurare SSL per un determinato endpoint mediante [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). È possibile utilizzare la stessa API per configurare altre impostazioni Kestrel per determinati endpoint.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Collegare a un socket di Unix**

Può restare in ascolto su un socket Unix per migliorare le prestazioni con Nginx, come illustrato in questo esempio:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Porta 0**

Se si specifica il numero di porta 0, Kestrel associa dinamicamente a una porta disponibile. Nell'esempio seguente viene illustrato come determinare quale porta Kestrel effettivamente associato a in fase di esecuzione:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**Limitazioni di UseUrls**

È possibile configurare gli endpoint chiamando il `UseUrls` metodo o utilizzando il `urls` argomento della riga di comando o la variabile di ambiente ASPNETCORE_URLS. Questi metodi sono utili se si desidera che il codice per utilizzare server diverso da Kestrel. Tuttavia, è necessario tenere presenti queste limitazioni:

* È possibile utilizzare SSL con questi metodi.
* Se si utilizzano entrambi la `Listen` (metodo) e `UseUrls`, `Listen` eseguire l'override di endpoint il `UseUrls` endpoint.

**Configurazione dell'endpoint per IIS**

Se si utilizza IIS, i binding di URL per IIS esegue l'override di tutti i binding è impostata tramite la chiamata a `Listen` o `UseUrls`. Per ulteriori informazioni, vedere [Introduzione a ASP.NET Core modulo](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Per impostazione predefinita ASP.NET Core associa a `http://localhost:5000`. È possibile configurare i prefissi URL e delle porte per Kestrel in ascolto su utilizzando il `UseUrls` metodo di estensione, il `urls` argomento della riga di comando o il sistema di configurazione di ASP.NET Core. Per ulteriori informazioni su questi metodi, vedere [Hosting](../../fundamentals/hosting.md). Per informazioni sul funzionamento dell'associazione URL quando si utilizza IIS come un proxy inverso, vedere [ASP.NET Core modulo](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>Prefissi URL

Se si chiama `UseUrls` o utilizzare il `urls` argomento della riga di comando o una variabile di ambiente ASPNETCORE_URLS, i prefissi URL possono essere in uno dei formati seguenti. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Solo i prefissi URL HTTP sono validi. Kestrel non supporta SSL quando si configura associazioni URL utilizzando `UseUrls`.

* Indirizzo IPv4 con numero di porta

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 è un caso speciale che viene associato a tutti gli indirizzi IPv4.


* Indirizzo IPv6 con numero di porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [:] equivale a IPv6 IPv4 0.0.0.0.


* Nome host con numero di porta

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  I nomi, host *, e +, non sono speciali. Tutto ciò che non è un indirizzo IP o "localhost" riconosciuta verrà associato a tutti i indirizzi IP IPv6 e IPv4. Se è necessario associare i nomi host diversi per diverse applicazioni ASP.NET Core sulla stessa porta, utilizzare [HTTP.sys](httpsys.md) o in un server proxy inverso, ad esempio IIS, Nginx o Apache.

* Nome "Localhost" con l'IP di loopback o di numero di porta con numero di porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quando `localhost` viene specificato, Kestrel tenta di associare le interfacce di loopback IPv4 e IPv6. Se la porta richiesta è in uso da un altro servizio in entrambe le interfacce loopback, Kestrel non verrà avviato. Se nessuna delle due interfacce di loopback non è disponibile per qualsiasi motivo (la maggior parte delle comune perché non è supportato IPv6), Kestrel registra un avviso. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* Indirizzo IPv4 con numero di porta

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 è un caso speciale che viene associato a tutti gli indirizzi IPv4.


* Indirizzo IPv6 con numero di porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [:] equivale a IPv6 IPv4 0.0.0.0.


* Nome host con numero di porta

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  I nomi, host \*, e + non sono speciali. Tutto ciò che non è un indirizzo IP o "localhost" riconosciuta associa a tutti i indirizzi IP IPv6 e IPv4. Se è necessario associare i nomi host diversi per diverse applicazioni ASP.NET Core sulla stessa porta, utilizzare [WebListener](weblistener.md) o in un server proxy inverso, ad esempio IIS, Nginx o Apache.

* Nome "Localhost" con l'IP di loopback o di numero di porta con numero di porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quando `localhost` viene specificato, Kestrel tenta di associare le interfacce di loopback IPv4 e IPv6. Se la porta richiesta è in uso da un altro servizio in entrambe le interfacce loopback, Kestrel non verrà avviato. Se nessuna delle due interfacce di loopback non è disponibile per qualsiasi motivo (la maggior parte delle comune perché non è supportato IPv6), Kestrel registra un avviso. 

* Socket di UNIX

  ```
  http://unix:/run/dan-live.sock  
  ```

**Porta 0**

Se si specifica il numero di porta 0, Kestrel associa dinamicamente a una porta disponibile. Associazione alla porta 0 è consentito per qualsiasi nome host o l'indirizzo IP, ad eccezione di `localhost` nome.

Nell'esempio seguente viene illustrato come determinare quale porta Kestrel effettivamente associato a in fase di esecuzione:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**Prefissi URL per SSL**

Assicurarsi di includere i prefissi URL con `https:` se si chiama il `UseHttps` il metodo di estensione, come illustrato di seguito.

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> Non può trovarsi sulla stessa porta HTTPS e HTTP.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Applicazione di esempio per 2. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Codice sorgente kestrel](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Applicazione di esempio per 1. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Codice sorgente kestrel](https://github.com/aspnet/KestrelHttpServer)

---
