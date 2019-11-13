---
title: integrazione delle factory client gRPC in .NET Core
author: jamesnk
description: Informazioni su come creare client gRPC usando la factory client.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 11/12/2019
no-loc:
- SignalR
uid: grpc/clientfactory
ms.openlocfilehash: 3042bb61367f8b9a9f3142217ad329270ab2cca5
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963688"
---
# <a name="grpc-client-factory-integration-in-net-core"></a><span data-ttu-id="4cfc1-103">integrazione delle factory client gRPC in .NET Core</span><span class="sxs-lookup"><span data-stu-id="4cfc1-103">gRPC client factory integration in .NET Core</span></span>

<span data-ttu-id="4cfc1-104">l'integrazione di gRPC con `HttpClientFactory` offre un modo centralizzato per creare client gRPC.</span><span class="sxs-lookup"><span data-stu-id="4cfc1-104">gRPC integration with `HttpClientFactory` offers a centralized way to create gRPC clients.</span></span> <span data-ttu-id="4cfc1-105">Può essere usato come alternativa alla configurazione di [istanze client gRPC](xref:grpc/client)autonome.</span><span class="sxs-lookup"><span data-stu-id="4cfc1-105">It can be used as an alternative to [configuring stand-alone gRPC client instances](xref:grpc/client).</span></span> <span data-ttu-id="4cfc1-106">L'integrazione di Factory è disponibile nel pacchetto NuGet [.NET. ClientFactory di Grpc](https://www.nuget.org/packages/Grpc.Net.ClientFactory) .</span><span class="sxs-lookup"><span data-stu-id="4cfc1-106">Factory integration is available in the [Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="4cfc1-107">La Factory offre i vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="4cfc1-107">The factory offers the following benefits:</span></span>

* <span data-ttu-id="4cfc1-108">Fornisce una posizione centralizzata per la configurazione di istanze client gRPC logiche</span><span class="sxs-lookup"><span data-stu-id="4cfc1-108">Provides a central location for configuring logical gRPC client instances</span></span>
* <span data-ttu-id="4cfc1-109">Gestisce la durata del `HttpClientMessageHandler` sottostante</span><span class="sxs-lookup"><span data-stu-id="4cfc1-109">Manages the lifetime of the underlying `HttpClientMessageHandler`</span></span>
* <span data-ttu-id="4cfc1-110">Propagazione automatica della scadenza e dell'annullamento in un ASP.NET Core servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="4cfc1-110">Automatic propagation of deadline and cancellation in an ASP.NET Core gRPC service</span></span>

## <a name="register-grpc-clients"></a><span data-ttu-id="4cfc1-111">Registrare i client di gRPC</span><span class="sxs-lookup"><span data-stu-id="4cfc1-111">Register gRPC clients</span></span>

<span data-ttu-id="4cfc1-112">Per registrare un client gRPC, è possibile usare il metodo di estensione `AddGrpcClient` generico all'interno di `Startup.ConfigureServices`, specificando la classe client tipizzata gRPC e l'indirizzo del servizio:</span><span class="sxs-lookup"><span data-stu-id="4cfc1-112">To register a gRPC client, the generic `AddGrpcClient` extension method can be used within `Startup.ConfigureServices`, specifying the gRPC typed client class and service address:</span></span>

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

<span data-ttu-id="4cfc1-113">Il tipo DI client gRPC è registrato come temporaneo con l'inserimento DI dipendenze.</span><span class="sxs-lookup"><span data-stu-id="4cfc1-113">The gRPC client type is registered as transient with dependency injection (DI).</span></span> <span data-ttu-id="4cfc1-114">Il client può ora essere inserito e utilizzato direttamente nei tipi creati da DI.</span><span class="sxs-lookup"><span data-stu-id="4cfc1-114">The client can now be injected and consumed directly in types created by DI.</span></span> <span data-ttu-id="4cfc1-115">I controller ASP.NET Core MVC, gli hub SignalR e i servizi gRPC sono posti in cui è possibile inserire automaticamente i client gRPC:</span><span class="sxs-lookup"><span data-stu-id="4cfc1-115">ASP.NET Core MVC controllers, SignalR hubs and gRPC services are places where gRPC clients can automatically be injected:</span></span>

```csharp
public class AggregatorService : Aggregator.AggregatorBase
{
    private readonly Greeter.GreeterClient _client;

    public AggregatorService(Greeter.GreeterClient client)
    {
        _client = client;
    }

    public override async Task SayHellos(HelloRequest request,
        IServerStreamWriter<HelloReply> responseStream, ServerCallContext context)
    {
        // Forward the call on to the greeter service
        using (var call = _client.SayHellos(request))
        {
            await foreach (var response in call.ResponseStream.ReadAllAsync())
            {
                await responseStream.WriteAsync(response);
            }
        }
    }
}
```

## <a name="configure-httpclient"></a><span data-ttu-id="4cfc1-116">Configurare HttpClient</span><span class="sxs-lookup"><span data-stu-id="4cfc1-116">Configure HttpClient</span></span>

<span data-ttu-id="4cfc1-117">`HttpClientFactory` crea il `HttpClient` usato dal client gRPC.</span><span class="sxs-lookup"><span data-stu-id="4cfc1-117">`HttpClientFactory` creates the `HttpClient` used by the gRPC client.</span></span> <span data-ttu-id="4cfc1-118">I metodi di `HttpClientFactory` standard possono essere usati per aggiungere il middleware della richiesta in uscita o per configurare la `HttpClientHandler` sottostante del `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="4cfc1-118">Standard `HttpClientFactory` methods can be used to add outgoing request middleware or to configure the underlying `HttpClientHandler` of the `HttpClient`:</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .ConfigurePrimaryHttpMessageHandler(() =>
    {
        var handler = new HttpClientHandler();
        handler.ClientCertificates.Add(LoadCertificate());
        return handler;
    });
```

<span data-ttu-id="4cfc1-119">Per altre informazioni, vedere [creare richieste HTTP con IHttpClientFactory](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="4cfc1-119">For more information, see [Make HTTP requests using IHttpClientFactory](xref:fundamentals/http-requests).</span></span>

## <a name="configure-channel-and-interceptors"></a><span data-ttu-id="4cfc1-120">Configurare canale e intercettori</span><span class="sxs-lookup"><span data-stu-id="4cfc1-120">Configure Channel and Interceptors</span></span>

<span data-ttu-id="4cfc1-121">sono disponibili metodi specifici di gRPC per:</span><span class="sxs-lookup"><span data-stu-id="4cfc1-121">gRPC-specific methods are available to:</span></span>

* <span data-ttu-id="4cfc1-122">Configurare il canale sottostante di un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="4cfc1-122">Configure a gRPC client's underlying channel.</span></span>
* <span data-ttu-id="4cfc1-123">Aggiungere `Interceptor` istanze che il client userà per eseguire chiamate gRPC.</span><span class="sxs-lookup"><span data-stu-id="4cfc1-123">Add `Interceptor` instances that the client will use when making gRPC calls.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .AddInterceptor(() => new LoggingInterceptor())
    .ConfigureChannel(o =>
    {
        o.Credentials = new CustomCredentials();
    });
```

## <a name="deadline-and-cancellation-propagation"></a><span data-ttu-id="4cfc1-124">Scadenza e propagazione dell'annullamento</span><span class="sxs-lookup"><span data-stu-id="4cfc1-124">Deadline and cancellation propagation</span></span>

<span data-ttu-id="4cfc1-125">i client gRPC creati dalla factory in un servizio gRPC possono essere configurati con `EnableCallContextPropagation()` per propagare automaticamente la scadenza e il token di annullamento alle chiamate figlio.</span><span class="sxs-lookup"><span data-stu-id="4cfc1-125">gRPC clients created by the factory in a gRPC service can be configured with `EnableCallContextPropagation()` to automatically propagate the deadline and cancellation token to child calls.</span></span> <span data-ttu-id="4cfc1-126">Il metodo di estensione `EnableCallContextPropagation()` è disponibile nel pacchetto NuGet [Grpc. AspNetCore. Server. ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) .</span><span class="sxs-lookup"><span data-stu-id="4cfc1-126">The `EnableCallContextPropagation()` extension method is available in the [Grpc.AspNetCore.Server.ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="4cfc1-127">La propagazione del contesto di chiamata funziona leggendo la scadenza e il token di annullamento dal contesto della richiesta gRPC corrente e propagando automaticamente le chiamate in uscita effettuate dal client gRPC.</span><span class="sxs-lookup"><span data-stu-id="4cfc1-127">Call context propagation works by reading the deadline and cancellation token from the current gRPC request context and automatically propagating them to outgoing calls made by the gRPC client.</span></span> <span data-ttu-id="4cfc1-128">La propagazione del contesto di chiamata è un ottimo modo per garantire che gli scenari gRPC complessi e nidificati propaghino sempre la scadenza e l'annullamento.</span><span class="sxs-lookup"><span data-stu-id="4cfc1-128">Call context propagation is an excellent way of ensuring that complex, nested gRPC scenarios always propagate the deadline and cancellation.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

<span data-ttu-id="4cfc1-129">Per ulteriori informazioni sulle scadenze e sull'annullamento RPC, vedere [ciclo di vita RPC](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span><span class="sxs-lookup"><span data-stu-id="4cfc1-129">For more information about deadlines and RPC cancellation, see [RPC life cycle](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4cfc1-130">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4cfc1-130">Additional resources</span></span>

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
