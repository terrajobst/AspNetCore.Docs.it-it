---
title: Risolvere i problemi di gRPC in .NET Core
author: jamesnk
description: Risolvere gli errori quando si usa gRPC in .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/12/2019
uid: grpc/troubleshoot
ms.openlocfilehash: ad74bfa57d2dde316734d55d86075f463e78ee56
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029034"
---
# <a name="troubleshoot-grpc-on-net-core"></a>Risolvere i problemi di gRPC in .NET Core

Di [James Newton-King](https://twitter.com/jamesnk)

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a>Mancata corrispondenza tra la configurazione SSL/TLS del client e del servizio

Il modello e gli esempi di gRPC usano [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) per proteggere i servizi gRPC per impostazione predefinita. i client gRPC devono usare una connessione sicura per chiamare correttamente i servizi gRPC protetti.

È possibile verificare che ASP.NET Core servizio gRPC stia usando TLS nei log scritti all'avvio dell'app. Il servizio sarà in ascolto su un endpoint HTTPS:

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

Il client .NET Core deve usare `https` nell'indirizzo del server per effettuare chiamate con una connessione protetta:

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
}
```

Tutte le implementazioni client di gRPC supportano TLS. i client gRPC di altri linguaggi richiedono in genere il canale `SslCredentials`configurato con. `SslCredentials`Specifica il certificato che verrà utilizzato dal client e deve essere utilizzato al posto di credenziali non sicure. Per esempi di configurazione delle diverse implementazioni client di gRPC per l'uso di TLS, vedere [autenticazione gRPC](https://www.grpc.io/docs/guides/auth/).

## <a name="call-insecure-grpc-services-with-net-core-client"></a>Chiamare i servizi gRPC non sicuri con il client .NET Core

È necessaria una configurazione aggiuntiva per chiamare i servizi gRPC non protetti con il client .NET Core. Il client gRPC deve impostare l' `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` opzione su `true` e usare `http` nell'indirizzo del server:

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a>Non è possibile avviare ASP.NET Core app gRPC in macOS

Gheppio non supporta HTTP/2 con TLS in macOS e versioni precedenti di Windows, ad esempio Windows 7. Per impostazione predefinita, il ASP.NET Core modello e gli esempi di gRPC usano TLS. Quando si tenta di avviare il server gRPC, viene visualizzato il messaggio di errore seguente:

> Impossibile eseguire il binding https://localhost:5001 a nell'interfaccia loopback IPv4: ' HTTP/2 su TLS non è supportato in macOS perché manca il supporto per ALPN .'.

Per risolvere questo problema, configurare gheppio e il client gRPC per l'uso di HTTP/2 *senza* TLS. Questa operazione deve essere eseguita solo durante lo sviluppo. Se non si usa TLS, i messaggi gRPC vengono inviati senza crittografia.

Gheppio deve configurare un endpoint HTTP/2 senza TLS in *Program.cs*:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

Il client di gRPC deve essere configurato anche per non usare TLS. Per altre informazioni, vedere [chiamare i servizi gRPC non sicuri con il client .NET Core](#call-insecure-grpc-services-with-net-core-client).

> [!WARNING]
> HTTP/2 senza TLS deve essere usato solo durante lo sviluppo di app. Le app di produzione devono sempre usare la sicurezza del trasporto. Per ulteriori informazioni, vedere [considerazioni sulla sicurezza in gRPC per ASP.NET Core](xref:grpc/security#transport-security).

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a>gli C# asset gRPC non sono codice generato da  *\*file. proto*

per la generazione di codice gRPC di client concreti e classi di base del servizio è necessario fare riferimento a file e strumenti protobuf da un progetto. È necessario includere:

* file con *estensione proto* che si desidera utilizzare nel `<Protobuf>` gruppo di elementi. Il progetto deve fare riferimento ai [file *. proto* ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) importati.
* Riferimento al pacchetto per il pacchetto di [strumenti GRPC gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).

Per ulteriori informazioni sulla generazione di C# asset gRPC, <xref:grpc/basics>vedere.

Per impostazione predefinita, `<Protobuf>` un riferimento genera un client concreto e una classe di base del servizio. L' `GrpcServices` attributo dell'elemento di riferimento può essere usato per C# limitare la generazione di asset. Le `GrpcServices` opzioni valide sono:

* `Both`(impostazione predefinita quando non è presente)
* `Server`
* `Client`
* `None`

Un ASP.NET Core app Web che ospita i servizi gRPC richiede solo la classe di base del servizio generata:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

Un'app client gRPC che effettua chiamate gRPC richiede solo il client concreto generato:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
