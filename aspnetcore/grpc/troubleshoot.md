---
title: Risolvere i problemi di gRPC in .NET Core
author: jamesnk
description: Risolvere gli errori quando si usa gRPC in .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/17/2019
uid: grpc/troubleshoot
ms.openlocfilehash: 7621266dfe26b7126d1607e195dd5dcaab4efa55
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886485"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="e2667-103">Risolvere i problemi di gRPC in .NET Core</span><span class="sxs-lookup"><span data-stu-id="e2667-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="e2667-104">Di [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="e2667-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="e2667-105">Mancata corrispondenza tra la configurazione SSL/TLS del client e del servizio</span><span class="sxs-lookup"><span data-stu-id="e2667-105">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="e2667-106">Il modello e gli esempi di gRPC usano [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) per proteggere i servizi gRPC per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e2667-106">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="e2667-107">i client gRPC devono usare una connessione sicura per chiamare correttamente i servizi gRPC protetti.</span><span class="sxs-lookup"><span data-stu-id="e2667-107">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="e2667-108">È possibile verificare che ASP.NET Core servizio gRPC stia usando TLS nei log scritti all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="e2667-108">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="e2667-109">Il servizio sarà in ascolto su un endpoint HTTPS:</span><span class="sxs-lookup"><span data-stu-id="e2667-109">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="e2667-110">Il client .NET Core deve usare `https` nell'indirizzo del server per effettuare chiamate con una connessione protetta:</span><span class="sxs-lookup"><span data-stu-id="e2667-110">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
}
```

<span data-ttu-id="e2667-111">Tutte le implementazioni client di gRPC supportano TLS.</span><span class="sxs-lookup"><span data-stu-id="e2667-111">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="e2667-112">i client gRPC di altri linguaggi richiedono in genere il canale `SslCredentials`configurato con.</span><span class="sxs-lookup"><span data-stu-id="e2667-112">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="e2667-113">`SslCredentials`Specifica il certificato che verrà utilizzato dal client e deve essere utilizzato al posto di credenziali non sicure.</span><span class="sxs-lookup"><span data-stu-id="e2667-113">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="e2667-114">Per esempi di configurazione delle diverse implementazioni client di gRPC per l'uso di TLS, vedere [autenticazione gRPC](https://www.grpc.io/docs/guides/auth/).</span><span class="sxs-lookup"><span data-stu-id="e2667-114">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="e2667-115">Chiamare i servizi gRPC non sicuri con il client .NET Core</span><span class="sxs-lookup"><span data-stu-id="e2667-115">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="e2667-116">È necessaria una configurazione aggiuntiva per chiamare i servizi gRPC non protetti con il client .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2667-116">Additional configuration is required to call insecure gRPC services with the .NET Core client.</span></span> <span data-ttu-id="e2667-117">Il client gRPC deve impostare l' `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` opzione su `true` e usare `http` nell'indirizzo del server:</span><span class="sxs-lookup"><span data-stu-id="e2667-117">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="e2667-118">Non è possibile avviare ASP.NET Core app gRPC in macOS</span><span class="sxs-lookup"><span data-stu-id="e2667-118">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="e2667-119">Gheppio non supporta HTTP/2 con TLS in macOS e versioni precedenti di Windows, ad esempio Windows 7.</span><span class="sxs-lookup"><span data-stu-id="e2667-119">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="e2667-120">Per impostazione predefinita, il ASP.NET Core modello e gli esempi di gRPC usano TLS.</span><span class="sxs-lookup"><span data-stu-id="e2667-120">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="e2667-121">Quando si tenta di avviare il server gRPC, viene visualizzato il messaggio di errore seguente:</span><span class="sxs-lookup"><span data-stu-id="e2667-121">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="e2667-122">Impossibile eseguire il binding https://localhost:5001 a nell'interfaccia loopback IPv4: ' HTTP/2 su TLS non è supportato in macOS perché manca il supporto per ALPN .'.</span><span class="sxs-lookup"><span data-stu-id="e2667-122">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="e2667-123">Per risolvere questo problema, configurare gheppio e il client gRPC per l'uso di HTTP/2 *senza* TLS.</span><span class="sxs-lookup"><span data-stu-id="e2667-123">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="e2667-124">Questa operazione deve essere eseguita solo durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="e2667-124">You should only do this during development.</span></span> <span data-ttu-id="e2667-125">Se non si usa TLS, i messaggi gRPC vengono inviati senza crittografia.</span><span class="sxs-lookup"><span data-stu-id="e2667-125">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="e2667-126">Gheppio deve configurare un endpoint HTTP/2 senza TLS in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e2667-126">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

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

<span data-ttu-id="e2667-127">Quando un endpoint HTTP/2 viene configurato senza TLS, l'endpoint [ListenOptions. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) deve essere impostato su `HttpProtocols.Http2`.</span><span class="sxs-lookup"><span data-stu-id="e2667-127">When an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="e2667-128">`HttpProtocols.Http1AndHttp2`non può essere usato perché TLS è necessario per negoziare HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="e2667-128">`HttpProtocols.Http1AndHttp2` can't be used because TLS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="e2667-129">Senza TLS, tutte le connessioni all'endpoint vengono predefinite a HTTP/1.1 e le chiamate a gRPC hanno esito negativo.</span><span class="sxs-lookup"><span data-stu-id="e2667-129">Without TLS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="e2667-130">Il client di gRPC deve essere configurato anche per non usare TLS.</span><span class="sxs-lookup"><span data-stu-id="e2667-130">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="e2667-131">Per altre informazioni, vedere [chiamare i servizi gRPC non sicuri con il client .NET Core](#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="e2667-131">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="e2667-132">HTTP/2 senza TLS deve essere usato solo durante lo sviluppo di app.</span><span class="sxs-lookup"><span data-stu-id="e2667-132">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="e2667-133">Le app di produzione devono sempre usare la sicurezza del trasporto.</span><span class="sxs-lookup"><span data-stu-id="e2667-133">Production apps should always use transport security.</span></span> <span data-ttu-id="e2667-134">Per ulteriori informazioni, vedere [considerazioni sulla sicurezza in gRPC per ASP.NET Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="e2667-134">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="e2667-135">gli C# asset gRPC non sono codice generato da  *\*file. proto*</span><span class="sxs-lookup"><span data-stu-id="e2667-135">gRPC C# assets are not code generated from *\*.proto* files</span></span>

<span data-ttu-id="e2667-136">per la generazione di codice gRPC di client concreti e classi di base del servizio è necessario fare riferimento a file e strumenti protobuf da un progetto.</span><span class="sxs-lookup"><span data-stu-id="e2667-136">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="e2667-137">È necessario includere:</span><span class="sxs-lookup"><span data-stu-id="e2667-137">You must include:</span></span>

* <span data-ttu-id="e2667-138">file con *estensione proto* che si desidera utilizzare nel `<Protobuf>` gruppo di elementi.</span><span class="sxs-lookup"><span data-stu-id="e2667-138">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="e2667-139">Il progetto deve fare riferimento ai [file *. proto* ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) importati.</span><span class="sxs-lookup"><span data-stu-id="e2667-139">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="e2667-140">Riferimento al pacchetto per il pacchetto di [strumenti GRPC gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="e2667-140">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="e2667-141">Per ulteriori informazioni sulla generazione di C# asset gRPC, <xref:grpc/basics>vedere.</span><span class="sxs-lookup"><span data-stu-id="e2667-141">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="e2667-142">Per impostazione predefinita, `<Protobuf>` un riferimento genera un client concreto e una classe di base del servizio.</span><span class="sxs-lookup"><span data-stu-id="e2667-142">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="e2667-143">L' `GrpcServices` attributo dell'elemento di riferimento può essere usato per C# limitare la generazione di asset.</span><span class="sxs-lookup"><span data-stu-id="e2667-143">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="e2667-144">Le `GrpcServices` opzioni valide sono:</span><span class="sxs-lookup"><span data-stu-id="e2667-144">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="e2667-145">`Both`(impostazione predefinita quando non è presente)</span><span class="sxs-lookup"><span data-stu-id="e2667-145">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

<span data-ttu-id="e2667-146">Un ASP.NET Core app Web che ospita i servizi gRPC richiede solo la classe di base del servizio generata:</span><span class="sxs-lookup"><span data-stu-id="e2667-146">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="e2667-147">Un'app client gRPC che effettua chiamate gRPC richiede solo il client concreto generato:</span><span class="sxs-lookup"><span data-stu-id="e2667-147">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
