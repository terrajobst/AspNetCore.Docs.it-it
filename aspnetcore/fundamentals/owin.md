---
title: Open Web Interface for .NET (OWIN)
author: ardalis
description: Scoprire come ASP.NET Core supporta l'interfaccia Web aperta per .NET (OWIN), che consente alle App web di essere scollegata dal server web.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 42ffa01745b7a492b3b8cb2778805f254863b890
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a>Introduzione per aprire interfaccia Web per .NET (OWIN)

Da [Steve Smith](https://ardalis.com/) e [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core supporta Open Web Interface for .NET (OWIN). OWIN consente alle app Web di essere disaccoppiate dai server Web. Definisce un modo standard dei middleware per essere utilizzata in una pipeline per gestire le richieste e risposte associate. Middleware e delle applicazioni ASP.NET Core possono interagire con middleware, server e applicazioni basate su OWIN.

OWIN fornisce un livello di separazione che consente di due modelli con modelli a oggetti diversi da utilizzare insieme. Il `Microsoft.AspNetCore.Owin` pacchetto fornisce due implementazioni dell'adattatore:
- ASP.NET Core per OWIN 
- OWIN per ASP.NET Core

In questo modo ASP.NET di base deve essere ospitato su un OWIN compatibile/host server o per altri componenti compatibile OWIN da eseguire su ASP.NET Core.

Nota: L'uso di questi adapter comporta con una riduzione delle prestazioni. Applicazioni che usano solo i componenti di base di ASP.NET è consigliabile utilizzare il pacchetto Owin o schede.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>Middleware OWIN in esecuzione nella pipeline ASP.NET

Supporto OWIN del ASP.NET di base viene distribuito come parte di `Microsoft.AspNetCore.Owin` pacchetto. Per importare supporto OWIN nel progetto di installazione del pacchetto.

Middleware OWIN conforme al [specifica OWIN](http://owin.org/spec/spec/owin-1.0.0.html), che richiede un `Func<IDictionary<string, object>, Task>` impostare interfaccia e chiavi specifiche (ad esempio `owin.ResponseBody`). Middleware OWIN semplice seguente visualizza "Hello World":

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

La firma di esempio restituisce un `Task` e accetta un `IDictionary<string, object>` come richiesto da OWIN.

Il codice seguente viene illustrato come aggiungere il `OwinHello` middleware (illustrato in precedenza) per la pipeline ASP.NET con il `UseOwin` metodo di estensione.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

È possibile configurare altre azioni da intraprendere sul posto all'interno della pipeline OWIN.

> [!NOTE]
> Le intestazioni di risposta devono essere modificate solo prima la prima scrittura nel flusso di risposta.

> [!NOTE]
> Più chiamate al metodo `UseOwin` è sconsigliato per motivi di prestazioni. Componenti OWIN funzionerà meglio se raggruppati insieme.

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>Tramite l'Hosting ASP.NET in un server basato su OWIN

Server basati su OWIN può ospitare applicazioni ASP.NET. Un server di questo tipo è [Nowin](https://github.com/Bobris/Nowin), un server web di OWIN .NET. Nell'esempio di questo articolo, ho incluso un progetto che fa riferimento a Nowin e viene utilizzato per creare un `IServer` in grado di self-hosting ASP.NET Core.

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer`è un'interfaccia che richiede un `Features` proprietà e un `Start` metodo.

`Start`è responsabile della configurazione e avvio del server, che in questo caso viene eseguito tramite una serie di chiamate API fluent impostare indirizzi analizzati il IServerAddressesFeature. Si noti che la configurazione di Microsoft Office fluent di `_builder` variabile specifica che le richieste verranno gestite dal `appFunc` definito in precedenza nel metodo. Questo `Func` viene chiamato su ogni richiesta per elaborare le richieste in ingresso.

Verrà inoltre aggiunto un `IWebHostBuilder` estensione rendono più semplice aggiungere e configurare il server Nowin.

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

A questo punto, tutto ciò che è necessario per eseguire un'applicazione ASP.NET utilizzando questo server personalizzato per chiamare l'estensione in *Program.cs*:

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

Ulteriori informazioni su ASP.NET [server](servers/index.md).

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>Eseguire ASP.NET Core in un server basato su OWIN e utilizzare il supporto di WebSocket

Un altro esempio di funzionalità dei server basati su OWIN può essere utilizzato da ASP.NET Core è l'accesso a funzionalità quali WebSocket. Il server web di OWIN .NET utilizzato nell'esempio precedente è il supporto per i socket Web incorporato, che può essere utilizzato da un'applicazione ASP.NET Core. L'esempio seguente mostra un'app web semplici che supporta i WebSocket e di restituisce tutti gli elementi inviati al server tramite WebSockets.

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

Questo [esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) viene configurata utilizzando lo stesso `NowinServer` a quello precedente, l'unica differenza è in modalità con cui l'applicazione è configurata nel relativo `Configure` (metodo). Un test utilizzando [un client websocket semplice](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) illustra l'applicazione:

![Client di Test Web Socket](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>Ambiente OWIN

È possibile creare un ambiente OWIN tramite il `HttpContext`.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>Chiavi OWIN

Dipende da OWIN un `IDictionary<string,object>` oggetto per comunicare informazioni attraverso uno scambio di richiesta/risposta HTTP. ASP.NET Core implementa le chiavi elencate di seguito. Vedere il [specifica primario, estensioni](http://owin.org/#spec), e [chiavi comuni e le linee guida chiave OWIN](http://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>Dati della richiesta (versione 1.0.0 OWIN)

| Chiave               | Valore (tipo) | Descrizione |
| ----------------- | ------------ | ----------- |
| owin.RequestScheme | `String` |  |
| owin.RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owin.RequestHeaders | `IDictionary<string,string[]>`  | |
| owin.RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>Dati della richiesta (OWIN v1.1.0)

| Chiave               | Valore (tipo) | Descrizione |
| ----------------- | ------------ | ----------- |
| owin.RequestId | `String` | Facoltativo |

### <a name="response-data-owin-v100"></a>Dati di risposta (versione 1.0.0 OWIN)

| Chiave               | Valore (tipo) | Descrizione |
| ----------------- | ------------ | ----------- |
| owin.ResponseStatusCode | `int` | Facoltativo |
| owin.ResponseReasonPhrase | `String` | Facoltativo |
| owin.ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin.ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>Altri dati (versione 1.0.0 OWIN)

| Chiave               | Valore (tipo) | Descrizione |
| ----------------- | ------------ | ----------- |
| owin.CallCancelled | `CancellationToken` |  |
| owin.Version  | `String` | |   


### <a name="common-keys"></a>Chiavi comuni

| Chiave               | Valore (tipo) | Descrizione |
| ----------------- | ------------ | ----------- |
| ssl.ClientCertificate | `X509Certificate` |  |
| ssl.LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| server.RemotePort | `String` | |     
| server.LocalIpAddress  | `String` | |    
| server.LocalPort  | `String` | |    
| server.IsLocal  | `bool` | |    
| server.OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Chiave               | Valore (tipo) | Descrizione |
| ----------------- | ------------ | ----------- |
| sendfile.SendAsync | Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | Per ogni richiesta |


### <a name="opaque-v030"></a>Opaque v0.3.0

| Chiave               | Valore (tipo) | Descrizione |
| ----------------- | ------------ | ----------- |
| opaque.Version | `String` |  |
| opaque.Upgrade | `OpaqueUpgrade` | Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| opaque.Stream | `Stream` |  |
| opaque.CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>WebSocket v0.3.0

| Chiave               | Valore (tipo) | Descrizione |
| ----------------- | ------------ | ----------- |
| websocket.Version | `String` |  |
| websocket.Accept | `WebSocketAccept` | Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket.AcceptAlt |  | Non-spec |
| websocket.SubProtocol | `String` | Vedere [RFC6455 sezione 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) passaggio 5.5 |
| websocket.SendAsync | `WebSocketSendAsync` | Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CallCancelled | `CancellationToken` |  |
| websocket.ClientCloseStatus | `int` | Facoltativo |
| websocket.ClientCloseDescription | `String` | Facoltativo |


## <a name="additional-resources"></a>Risorse aggiuntive

* [Middleware](middleware.md)

* [Server](servers/index.md)
