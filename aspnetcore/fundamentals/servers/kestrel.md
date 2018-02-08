---
title: Implementazione del server Web Kestrel in ASP.NET Core
author: tdykstra
description: Presentazione di Kestrel, il server Web multipiattaforma per ASP.NET Core basato su libuv.
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Introduzione all'implementazione del server Web Kestrel in ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Stephen Halter](https://twitter.com/halter73)

Kestrel è un [server Web per ASP.NET Core](index.md) multipiattaforma basato su [libuv](https://github.com/libuv/libuv), una libreria di I/O asincrona multipiattaforma. Kestrel è il server Web incluso per impostazione predefinita nei modelli di progetto di ASP.NET Core. 

Kestrel supporta le funzionalità seguenti:

  * HTTPS
  * Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)
  * Socket Unix ad alte prestazioni dietro Nginx 

Kestrel è supportato in tutte le piattaforme e le versioni supportate da .NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Visualizzare o scaricare il codice di esempio per 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Visualizzare o scaricare il codice di esempio per 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Quando usare Kestrel con un proxy inverso

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

Per motivi di sicurezza, un proxy inverso è obbligatorio per le distribuzioni perimetrali, ovvero le distribuzioni esposte al traffico Internet. Nelle versioni 1. x di Kestrel non è disponibile una serie completa di difese contro gli attacchi. Questo include, senza limitazioni, timeout appropriati, limiti delle dimensioni e limiti delle connessioni simultanee.

---

Uno scenario che richiede un proxy inverso è il caso in cui varie applicazioni che condividono lo stesso IP e la stessa porta sono in esecuzione in un singolo server. Questo scenario non è direttamente applicabile a Kestrel, perché Kestrel non supporta la condivisione dello stesso IP e della porta tra più processi. Quando lo si configura per l'ascolto su una porta, Kestrel gestisce tutto il traffico per tale porta indipendentemente dall'intestazione host. Un proxy inverso che può condividere porte deve quindi eseguire l'inoltro a Kestrel su un unico IP e un'unica porta.

Anche se un server proxy inverso non è necessario, può risultare utile per altri motivi:

* Può limitare la superficie di attacco esposta.
* Offre un livello facoltativo aggiuntivo di configurazione e protezione.
* Può offrire un'integrazione migliore con l'infrastruttura esistente.
* Semplifica il bilanciamento del carico e la configurazione SSL. Solo il server proxy inverso richiede un certificato SSL e tale server può comunicare con i server applicazioni sulla rete interna mediante HTTP semplice.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Come usare Kestrel nelle app ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il pacchetto [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) è incluso nel metapacchetto [Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

I modelli di progetto ASP.NET Core usano Kestrel per impostazione predefinita. In *Program.cs* il codice modello chiama `CreateDefaultBuilder`, che chiama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) in background.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Se è necessario configurare le opzioni di Kestrel, chiamare `UseKestrel` in *Program.cs* come visualizzato nell'esempio seguente:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Installare il pacchetto NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Chiamare il metodo di estensione [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) su `WebHostBuilder` nel metodo `Main`, specificando le [opzioni Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) desiderate come visualizzato nelle sezione seguente.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Opzioni Kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il server Web Kestrel dispone di opzioni di configurazione dei vincoli che risultano particolarmente utili nelle distribuzioni con connessione Internet. Ecco alcuni limiti che è possibile impostare:

- Numero massimo di connessioni client
- Dimensione massima del corpo della richiesta
- Velocità minima dei dati del corpo della richiesta

Questi ed altri vincoli sono disponibili nella proprietà `Limits` della classe [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs). La proprietà `Limits` contiene un'istanza della classe [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs). 

**Numero massimo di connessioni client**

È possibile impostare il numero massimo di connessioni TCP aperte simultaneamente per l'intera applicazione con il seguente codice:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

È previsto un limite separato per le connessioni che sono state aggiornate da HTTP o HTTPS a un altro protocollo (ad esempio su una richiesta WebSocket). Dopo l'aggiornamento di una connessione, questa non viene conteggiata per il limite `MaxConcurrentConnections`. 

Per impostazione predefinita, il numero massimo di connessioni è illimitato (null).

**Dimensione massima del corpo della richiesta**

La dimensione massima predefinita del corpo della richiesta è pari a 30.000.000 di byte, equivalenti a circa 28,6 MB. 

Il metodo consigliato per ignorare il limite in un'applicazione ASP.NET Core MVC è l'uso dell'attributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) in un metodo di azione:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

L'esempio seguente visualizza come configurare il vincolo per l'intera applicazione e ogni richiesta:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

È possibile eseguire l'override dell'impostazione per una richiesta specifica nel middleware:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Se si prova a configurare il limite per una richiesta dopo che l'applicazione ha avviato la lettura della richiesta stessa, viene generata un'eccezione. Una proprietà `IsReadOnly` indica se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.

**Velocità minima dei dati del corpo della richiesta**

Kestrel controlla ogni secondo se i dati vengono ricevuto alla velocità in byte al secondo specificata. Se la velocità scende sotto il valore minimo, la connessione raggiunge il timeout. Il periodo di tolleranza è la quantità di tempo che Kestrel concede al client per aumentare la velocità di trasmissione fino al valore minimo. Durante tale periodo la velocità non viene rilevata. Il periodo di prova consente di evitare l'interruzione di connessioni che inizialmente inviano i dati a velocità ridotta a causa dell'avvio lento del protocollo TCP.

La velocità minima predefinita è di 240 byte al secondo, con un periodo di tolleranza di 5 secondi.

Anche per la risposta è prevista una velocità minima. Il codice per impostare il limite della richiesta e della risposta è identico e varia solo per `RequestBody` o `Response` nei nomi di proprietà e di interfaccia. 

L'esempio seguente visualizza come configurare la velocità minima dei dati in *Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

È possibile configurare la velocità per ogni singola richiesta nel middleware:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Per informazioni su altre opzioni di Kestrel, vedere le classi seguenti:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Per informazioni sulle opzioni di Kestrel, vedere la [classe KestrelServerOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Configurazione dell'endpoint

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Per impostazione predefinita ASP.NET Core è associato a `http://localhost:5000`. È possibile configurare i prefissi URL e le porte sulle quali è in ascolto Kestrel chiamando i metodi `Listen` o `ListenUnixSocket` su `KestrelServerOptions`. Anche `UseUrls`, l'argomento della riga di comando `urls` e la variabile di ambiente ASPNETCORE_URLS funzionano, ma con le limitazioni indicate [più avanti in questo articolo](#useurls-limitations).

**Associazione a un socket TCP**

Il metodo `Listen` esegue l'associazione a un socket TCP. Un'espressione lambda per le opzioni consente di configurare un certificato SSL:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Si noti come in questo esempio SSL viene configurato per un endpoint specifico mediante [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). È possibile usare la stessa API per configurare altre impostazioni di Kestrel per endpoint specifici.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Associazione a un socket Unix**

È possibile impostare l'ascolto su un socket Unix per migliorare le prestazioni con Nginx, come visualizzato nell'esempio seguente:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Porta 0**

Se si specifica il numero di porta 0, Kestrel esegue l'associazione dinamica a una porta disponibile. L'esempio seguente indica come determinare la porta alla quale Kestrel ha eseguito l'associazione in fase di esecuzione:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**Limitazioni di UseUrls**

È possibile configurare gli endpoint chiamando il metodo `UseUrls` o usando l'argomento della riga di comando `urls` o la variabile di ambiente ASPNETCORE_URLS. Questi metodi sono utili se si vuole che il codice funzioni con server diversi da Kestrel. Tenere tuttavia presenti queste limitazioni:

* Non è possibile usare SSL con questi metodi.
* Se si usa sia il metodo `Listen` sia `UseUrls`, gli endpoint `Listen` eseguono l'override degli endpoint `UseUrls`.

**Configurazione endpoint per IIS**

Se si usa IIS i binding URL per IIS sostituiscono tutti i binding impostati chiamando `Listen` o `UseUrls`. Per altre informazioni, vedere [Introduzione al modulo ASP.NET Core](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Per impostazione predefinita ASP.NET Core è associato a `http://localhost:5000`. È possibile configurare le porte e i prefissi URL sui quali è in ascolto Kestrel usando il metodo di estensione `UseUrls`, l'argomento della riga di comando `urls` o il sistema di configurazione di ASP.NET Core. Per altre informazioni su questi metodi, vedere [Hosting](../../fundamentals/hosting.md). Per informazioni sul funzionamento dell'associazione URL quando si usa IIS come proxy inverso, vedere [Modulo ASP.NET Core](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>Prefissi URL

Se si chiama `UseUrls` o si usa l'argomento della riga di comando `urls` o la variabile di ambiente ASPNETCORE_URLS, i prefissi URL possono avere uno dei formati seguenti. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Sono validi solo i prefissi URL HTTP. Kestrel non supporta SSL quando le associazioni vengono configurate URL usando `UseUrls`.

* Indirizzo IPv4 con numero di porta

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.


* Indirizzo IPv6 con numero di porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] è l'equivalente IPv6 di 0.0.0.0 per IPv4.


* Nome host con numero di porta

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  I nomi host, * e + non sono casi particolari. Tutto ciò che non è un indirizzo IP o un elemento "localhost" riconosciuto esegue l'associazione a tutti gli IP IPv4 e IPv6. Se è necessario associare nomi host diversi ad applicazioni ASP.NET Core diverse sulla stessa porta, usare [HTTP.sys](httpsys.md) o un server proxy inverso come IIS, Nginx o Apache.

* Nome "localhost" con numero di porta o IP di loopback con numero di porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Se è specificato `localhost`, Kestrel prova l'associazione alle interfacce di loopback sia IPv4 che IPv6. Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato. Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* Indirizzo IPv4 con numero di porta

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.


* Indirizzo IPv6 con numero di porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] è l'equivalente IPv6 di 0.0.0.0 per IPv4.


* Nome host con numero di porta

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  I nomi host, \* e + non sono casi particolari. Tutto ciò che non è un indirizzo IP o un elemento "localhost" riconosciuto esegue l'associazione a tutti gli IP IPv4 e IPv6. Se è necessario associare nomi host diversi ad applicazioni ASP.NET Core diverse sulla stessa porta, usare [WebListener](weblistener.md) o un server proxy inverso come IIS, Nginx o Apache.

* Nome "localhost" con numero di porta o IP di loopback con numero di porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Se è specificato `localhost`, Kestrel prova l'associazione alle interfacce di loopback sia IPv4 che IPv6. Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato. Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso. 

* Socket UNIX

  ```
  http://unix:/run/dan-live.sock  
  ```

**Porta 0**

Se si specifica il numero di porta 0, Kestrel esegue l'associazione dinamica a una porta disponibile. L'associazione alla porta 0 è consentita per qualsiasi nome host o IP ad eccezione del nome `localhost`.

L'esempio seguente indica come determinare la porta alla quale Kestrel ha eseguito l'associazione in fase di esecuzione:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**Prefissi URL per SSL**

Assicurarsi di includere i prefissi URL con `https:` se si chiama il metodo di estensione `UseHttps`, come visualizzato di seguito.

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
> HTTPS e HTTP non possono essere ospitati sulla stessa porta.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [App di esempio per 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Codice sorgente di Kestrel](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [App di esempio per 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Codice sorgente di Kestrel](https://github.com/aspnet/KestrelHttpServer)

---
