---
title: Chiamare i servizi gRPC con il client .NET
author: jamesnk
description: Informazioni su come chiamare i servizi gRPC con il client gRPC .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 6a6a649f7194354b16f3d67160be02428cc01170
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667174"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="c2cd3-103">Chiamare i servizi gRPC con il client .NET</span><span class="sxs-lookup"><span data-stu-id="c2cd3-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="c2cd3-104">Una libreria client di .NET gRPC è disponibile nel pacchetto NuGet [.NET. client di gRPC](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="c2cd3-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="c2cd3-105">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="c2cd3-105">This document explains how to:</span></span>

* <span data-ttu-id="c2cd3-106">Configurare un client gRPC per chiamare i servizi gRPC.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="c2cd3-107">Eseguire chiamate gRPC a metodi unari, streaming server, flusso client e flusso bidirezionale.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="c2cd3-108">Configurare il client gRPC</span><span class="sxs-lookup"><span data-stu-id="c2cd3-108">Configure gRPC client</span></span>

<span data-ttu-id="c2cd3-109">i client gRPC sono tipi di client concreti [generati da file *\*. proto* ](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="c2cd3-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="c2cd3-110">Il client gRPC concreto dispone di metodi che vengono convertiti nel servizio gRPC nel file *\*. proto* .</span><span class="sxs-lookup"><span data-stu-id="c2cd3-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="c2cd3-111">Un client gRPC viene creato da un canale.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="c2cd3-112">Iniziare usando `GrpcChannel.ForAddress` per creare un canale e quindi usare il canale per creare un client gRPC:</span><span class="sxs-lookup"><span data-stu-id="c2cd3-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="c2cd3-113">Un canale rappresenta una connessione di lunga durata a un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="c2cd3-114">Quando viene creato un canale, questo viene configurato con le opzioni correlate alla chiamata a un servizio.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="c2cd3-115">Ad esempio, il `HttpClient` utilizzato per effettuare chiamate, le dimensioni massime dei messaggi di invio e ricezione e la registrazione possono essere specificate in `GrpcChannelOptions` e utilizzate con `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="c2cd3-116">Per un elenco completo delle opzioni, vedere [Opzioni di configurazione client](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="c2cd3-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="c2cd3-117">Prestazioni e utilizzo del canale e del client:</span><span class="sxs-lookup"><span data-stu-id="c2cd3-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="c2cd3-118">La creazione di un canale può essere un'operazione costosa.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="c2cd3-119">Il riutilizzo di un canale per le chiamate gRPC offre vantaggi in merito alle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="c2cd3-120">i client gRPC vengono creati con i canali.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="c2cd3-121">i client gRPC sono oggetti leggeri e non devono essere memorizzati nella cache o riutilizzati.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="c2cd3-122">È possibile creare più client gRPC da un canale, inclusi tipi diversi di client.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="c2cd3-123">Un canale e i client creati dal canale possono essere usati in modo sicuro da più thread.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="c2cd3-124">I client creati dal canale possono effettuare più chiamate simultanee.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="c2cd3-125">`GrpcChannel.ForAddress` non è l'unica opzione per la creazione di un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="c2cd3-126">Se si chiamano i servizi gRPC da un'app ASP.NET Core, prendere in considerazione l' [integrazione di gRPC client Factory](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="c2cd3-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="c2cd3-127">l'integrazione di gRPC con `HttpClientFactory` offre un'alternativa centralizzata alla creazione di client gRPC.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="c2cd3-128">È necessaria una configurazione aggiuntiva per [chiamare i servizi gRPC non protetti con il client .NET](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="c2cd3-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!NOTE]
> <span data-ttu-id="c2cd3-129">La chiamata a gRPC su HTTP/2 con `Grpc.Net.Client` non è attualmente supportata in Novell.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-129">Calling gRPC over HTTP/2 with `Grpc.Net.Client` is currently not supported on Xamarin.</span></span> <span data-ttu-id="c2cd3-130">Stiamo lavorando per migliorare il supporto HTTP/2 in una versione futura di Novell.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-130">We are working to improve HTTP/2 support in a future Xamarin release.</span></span> <span data-ttu-id="c2cd3-131">[Grpc. Core](https://www.nuget.org/packages/Grpc.Core) e [Grpc-Web](xref:grpc/browser) sono valide alternative che funzionano oggi.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) and [gRPC-Web](xref:grpc/browser) are viable alternatives that work today.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="c2cd3-132">Effettuare chiamate a gRPC</span><span class="sxs-lookup"><span data-stu-id="c2cd3-132">Make gRPC calls</span></span>

<span data-ttu-id="c2cd3-133">Una chiamata gRPC viene avviata chiamando un metodo sul client.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-133">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="c2cd3-134">Il client gRPC gestirà la serializzazione dei messaggi e indirizza la chiamata gRPC al servizio corretto.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-134">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="c2cd3-135">gRPC dispone di tipi diversi di metodi.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-135">gRPC has different types of methods.</span></span> <span data-ttu-id="c2cd3-136">Il modo in cui si usa il client per effettuare una chiamata gRPC dipende dal tipo di metodo che si sta chiamando.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-136">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="c2cd3-137">I tipi di metodo gRPC sono:</span><span class="sxs-lookup"><span data-stu-id="c2cd3-137">The gRPC method types are:</span></span>

* <span data-ttu-id="c2cd3-138">Unaria</span><span class="sxs-lookup"><span data-stu-id="c2cd3-138">Unary</span></span>
* <span data-ttu-id="c2cd3-139">Streaming Server</span><span class="sxs-lookup"><span data-stu-id="c2cd3-139">Server streaming</span></span>
* <span data-ttu-id="c2cd3-140">Flusso client</span><span class="sxs-lookup"><span data-stu-id="c2cd3-140">Client streaming</span></span>
* <span data-ttu-id="c2cd3-141">Streaming bidirezionale</span><span class="sxs-lookup"><span data-stu-id="c2cd3-141">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="c2cd3-142">Chiamata unaria</span><span class="sxs-lookup"><span data-stu-id="c2cd3-142">Unary call</span></span>

<span data-ttu-id="c2cd3-143">Una chiamata unaria inizia con il client che invia un messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-143">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="c2cd3-144">Al termine del servizio viene restituito un messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-144">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="c2cd3-145">Ogni metodo del servizio unario nel file *\*. proto* genererà due metodi .NET sul tipo di client gRPC concreto per chiamare il metodo: un metodo asincrono e un metodo di blocco.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-145">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="c2cd3-146">Ad esempio, in `GreeterClient` sono disponibili due modi per chiamare `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="c2cd3-146">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="c2cd3-147">`GreeterClient.SayHelloAsync` chiama `Greeter.SayHello` servizio in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-147">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="c2cd3-148">Può essere atteso.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-148">Can be awaited.</span></span>
* <span data-ttu-id="c2cd3-149">`GreeterClient.SayHello`: chiama il servizio `Greeter.SayHello` e si blocca fino al completamento.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-149">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="c2cd3-150">Non usare nel codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-150">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="c2cd3-151">Chiamata di streaming del server</span><span class="sxs-lookup"><span data-stu-id="c2cd3-151">Server streaming call</span></span>

<span data-ttu-id="c2cd3-152">Una chiamata di streaming del server inizia con il client che invia un messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-152">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="c2cd3-153">`ResponseStream.MoveNext()` legge i messaggi trasmessi dal servizio.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-153">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="c2cd3-154">La chiamata di streaming del server è completa quando `ResponseStream.MoveNext()` restituisce `false`.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-154">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    while (await call.ResponseStream.MoveNext())
    {
        Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

<span data-ttu-id="c2cd3-155">Se si utilizza C# 8 o versioni successive, è possibile utilizzare la sintassi `await foreach` per leggere i messaggi.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-155">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="c2cd3-156">Il metodo di estensione `IAsyncStreamReader<T>.ReadAllAsync()` legge tutti i messaggi dal flusso di risposta:</span><span class="sxs-lookup"><span data-stu-id="c2cd3-156">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    await foreach (var response in call.ResponseStream.ReadAllAsync())
    {
        Console.WriteLine("Greeting: " + response.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

### <a name="client-streaming-call"></a><span data-ttu-id="c2cd3-157">Chiamata di streaming client</span><span class="sxs-lookup"><span data-stu-id="c2cd3-157">Client streaming call</span></span>

<span data-ttu-id="c2cd3-158">Una chiamata di streaming client viene avviata *senza* che il client invii un messaggio.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-158">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="c2cd3-159">Il client può scegliere di inviare messaggi con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-159">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="c2cd3-160">Al termine dell'invio dei messaggi da parte del client `RequestStream.CompleteAsync` necessario chiamare per inviare una notifica al servizio.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-160">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="c2cd3-161">La chiamata viene completata quando il servizio restituisce un messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-161">The call is finished when the service returns a response message.</span></span>

```csharp
var client = new Counter.CounterClient(channel);
using (var call = client.AccumulateCount())
{
    for (var i = 0; i < 3; i++)
    {
        await call.RequestStream.WriteAsync(new CounterRequest { Count = 1 });
    }
    await call.RequestStream.CompleteAsync();

    var response = await call;
    Console.WriteLine($"Count: {response.Count}");
    // Count: 3
}
```

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="c2cd3-162">Chiamata di streaming bidirezionale</span><span class="sxs-lookup"><span data-stu-id="c2cd3-162">Bi-directional streaming call</span></span>

<span data-ttu-id="c2cd3-163">Una chiamata di streaming bidirezionale inizia *senza* che il client invii un messaggio.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-163">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="c2cd3-164">Il client può scegliere di inviare messaggi con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-164">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="c2cd3-165">I messaggi trasmessi dal servizio sono accessibili con `ResponseStream.MoveNext()` o `ResponseStream.ReadAllAsync()`.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-165">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="c2cd3-166">La chiamata di streaming bidirezionale è completa quando il `ResponseStream` non contiene più messaggi.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-166">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

```csharp
using (var call = client.Echo())
{
    Console.WriteLine("Starting background task to receive messages");
    var readTask = Task.Run(async () =>
    {
        await foreach (var response in call.ResponseStream.ReadAllAsync())
        {
            Console.WriteLine(response.Message);
            // Echo messages sent to the service
        }
    });

    Console.WriteLine("Starting to send messages");
    Console.WriteLine("Type a message to echo then press enter.");
    while (true)
    {
        var result = Console.ReadLine();
        if (string.IsNullOrEmpty(result))
        {
            break;
        }

        await call.RequestStream.WriteAsync(new EchoMessage { Message = result });
    }

    Console.WriteLine("Disconnecting");
    await call.RequestStream.CompleteAsync();
    await readTask;
}
```

<span data-ttu-id="c2cd3-167">Durante una chiamata di streaming bidirezionale, il client e il servizio possono inviare messaggi l'uno all'altro in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-167">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="c2cd3-168">La migliore logica client per l'interazione con una chiamata bidirezionale varia a seconda della logica del servizio.</span><span class="sxs-lookup"><span data-stu-id="c2cd3-168">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2cd3-169">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c2cd3-169">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
