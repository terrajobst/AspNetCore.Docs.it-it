---
title: Chiamare i servizi gRPC con il client .NET
author: jamesnk
description: Informazioni su come chiamare i servizi gRPC con il client gRPC .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 1e7887388a752fb35d00e65db210c3924c6ab192
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829101"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="837b3-103">Chiamare i servizi gRPC con il client .NET</span><span class="sxs-lookup"><span data-stu-id="837b3-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="837b3-104">Una libreria client di .NET gRPC è disponibile nel pacchetto NuGet [.NET. client di gRPC](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="837b3-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="837b3-105">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="837b3-105">This document explains how to:</span></span>

* <span data-ttu-id="837b3-106">Configurare un client gRPC per chiamare i servizi gRPC.</span><span class="sxs-lookup"><span data-stu-id="837b3-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="837b3-107">Eseguire chiamate gRPC a metodi unari, streaming server, flusso client e flusso bidirezionale.</span><span class="sxs-lookup"><span data-stu-id="837b3-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="837b3-108">Configurare il client gRPC</span><span class="sxs-lookup"><span data-stu-id="837b3-108">Configure gRPC client</span></span>

<span data-ttu-id="837b3-109">i client gRPC sono tipi di client concreti [generati da file *\*. proto* ](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="837b3-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="837b3-110">Il client gRPC concreto dispone di metodi che vengono convertiti nel servizio gRPC nel file *\*. proto* .</span><span class="sxs-lookup"><span data-stu-id="837b3-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="837b3-111">Un client gRPC viene creato da un canale.</span><span class="sxs-lookup"><span data-stu-id="837b3-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="837b3-112">Iniziare usando `GrpcChannel.ForAddress` per creare un canale e quindi usare il canale per creare un client gRPC:</span><span class="sxs-lookup"><span data-stu-id="837b3-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="837b3-113">Un canale rappresenta una connessione di lunga durata a un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="837b3-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="837b3-114">Quando viene creato un canale, questo viene configurato con le opzioni correlate alla chiamata a un servizio.</span><span class="sxs-lookup"><span data-stu-id="837b3-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="837b3-115">Ad esempio, il `HttpClient` utilizzato per effettuare chiamate, le dimensioni massime dei messaggi di invio e ricezione e la registrazione possono essere specificate in `GrpcChannelOptions` e utilizzate con `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="837b3-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="837b3-116">Per un elenco completo delle opzioni, vedere [Opzioni di configurazione client](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="837b3-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="837b3-117">Prestazioni e utilizzo del canale e del client:</span><span class="sxs-lookup"><span data-stu-id="837b3-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="837b3-118">La creazione di un canale può essere un'operazione costosa.</span><span class="sxs-lookup"><span data-stu-id="837b3-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="837b3-119">Il riutilizzo di un canale per le chiamate gRPC offre vantaggi in merito alle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="837b3-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="837b3-120">i client gRPC vengono creati con i canali.</span><span class="sxs-lookup"><span data-stu-id="837b3-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="837b3-121">i client gRPC sono oggetti leggeri e non devono essere memorizzati nella cache o riutilizzati.</span><span class="sxs-lookup"><span data-stu-id="837b3-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="837b3-122">È possibile creare più client gRPC da un canale, inclusi tipi diversi di client.</span><span class="sxs-lookup"><span data-stu-id="837b3-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="837b3-123">Un canale e i client creati dal canale possono essere usati in modo sicuro da più thread.</span><span class="sxs-lookup"><span data-stu-id="837b3-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="837b3-124">I client creati dal canale possono effettuare più chiamate simultanee.</span><span class="sxs-lookup"><span data-stu-id="837b3-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="837b3-125">`GrpcChannel.ForAddress` non è l'unica opzione per la creazione di un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="837b3-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="837b3-126">Se si chiamano i servizi gRPC da un'app ASP.NET Core, prendere in considerazione l' [integrazione di gRPC client Factory](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="837b3-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="837b3-127">l'integrazione di gRPC con `HttpClientFactory` offre un'alternativa centralizzata alla creazione di client gRPC.</span><span class="sxs-lookup"><span data-stu-id="837b3-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="837b3-128">È necessaria una configurazione aggiuntiva per [chiamare i servizi gRPC non protetti con il client .NET](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="837b3-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="837b3-129">Effettuare chiamate a gRPC</span><span class="sxs-lookup"><span data-stu-id="837b3-129">Make gRPC calls</span></span>

<span data-ttu-id="837b3-130">Una chiamata gRPC viene avviata chiamando un metodo sul client.</span><span class="sxs-lookup"><span data-stu-id="837b3-130">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="837b3-131">Il client gRPC gestirà la serializzazione dei messaggi e indirizza la chiamata gRPC al servizio corretto.</span><span class="sxs-lookup"><span data-stu-id="837b3-131">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="837b3-132">gRPC dispone di tipi diversi di metodi.</span><span class="sxs-lookup"><span data-stu-id="837b3-132">gRPC has different types of methods.</span></span> <span data-ttu-id="837b3-133">Il modo in cui si usa il client per effettuare una chiamata gRPC dipende dal tipo di metodo che si sta chiamando.</span><span class="sxs-lookup"><span data-stu-id="837b3-133">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="837b3-134">I tipi di metodo gRPC sono:</span><span class="sxs-lookup"><span data-stu-id="837b3-134">The gRPC method types are:</span></span>

* <span data-ttu-id="837b3-135">Unario</span><span class="sxs-lookup"><span data-stu-id="837b3-135">Unary</span></span>
* <span data-ttu-id="837b3-136">Streaming Server</span><span class="sxs-lookup"><span data-stu-id="837b3-136">Server streaming</span></span>
* <span data-ttu-id="837b3-137">Flusso client</span><span class="sxs-lookup"><span data-stu-id="837b3-137">Client streaming</span></span>
* <span data-ttu-id="837b3-138">Streaming bidirezionale</span><span class="sxs-lookup"><span data-stu-id="837b3-138">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="837b3-139">Chiamata unaria</span><span class="sxs-lookup"><span data-stu-id="837b3-139">Unary call</span></span>

<span data-ttu-id="837b3-140">Una chiamata unaria inizia con il client che invia un messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="837b3-140">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="837b3-141">Al termine del servizio viene restituito un messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="837b3-141">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="837b3-142">Ogni metodo del servizio unario nel file *\*. proto* genererà due metodi .NET sul tipo di client gRPC concreto per chiamare il metodo: un metodo asincrono e un metodo di blocco.</span><span class="sxs-lookup"><span data-stu-id="837b3-142">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="837b3-143">Ad esempio, in `GreeterClient` sono disponibili due modi per chiamare `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="837b3-143">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="837b3-144">`GreeterClient.SayHelloAsync` chiama `Greeter.SayHello` servizio in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="837b3-144">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="837b3-145">Può essere atteso.</span><span class="sxs-lookup"><span data-stu-id="837b3-145">Can be awaited.</span></span>
* <span data-ttu-id="837b3-146">`GreeterClient.SayHello`: chiama il servizio `Greeter.SayHello` e si blocca fino al completamento.</span><span class="sxs-lookup"><span data-stu-id="837b3-146">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="837b3-147">Non usare nel codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="837b3-147">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="837b3-148">Chiamata di streaming del server</span><span class="sxs-lookup"><span data-stu-id="837b3-148">Server streaming call</span></span>

<span data-ttu-id="837b3-149">Una chiamata di streaming del server inizia con il client che invia un messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="837b3-149">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="837b3-150">`ResponseStream.MoveNext()` legge i messaggi trasmessi dal servizio.</span><span class="sxs-lookup"><span data-stu-id="837b3-150">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="837b3-151">La chiamata di streaming del server è completa quando `ResponseStream.MoveNext()` restituisce `false`.</span><span class="sxs-lookup"><span data-stu-id="837b3-151">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="837b3-152">Se si utilizza C# 8 o versioni successive, è possibile utilizzare la sintassi `await foreach` per leggere i messaggi.</span><span class="sxs-lookup"><span data-stu-id="837b3-152">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="837b3-153">Il metodo di estensione `IAsyncStreamReader<T>.ReadAllAsync()` legge tutti i messaggi dal flusso di risposta:</span><span class="sxs-lookup"><span data-stu-id="837b3-153">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="837b3-154">Chiamata di streaming client</span><span class="sxs-lookup"><span data-stu-id="837b3-154">Client streaming call</span></span>

<span data-ttu-id="837b3-155">Una chiamata di streaming client viene avviata *senza* che il client invii un messaggio.</span><span class="sxs-lookup"><span data-stu-id="837b3-155">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="837b3-156">Il client può scegliere di inviare messaggi con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="837b3-156">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="837b3-157">Al termine dell'invio dei messaggi da parte del client `RequestStream.CompleteAsync` necessario chiamare per inviare una notifica al servizio.</span><span class="sxs-lookup"><span data-stu-id="837b3-157">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="837b3-158">La chiamata viene completata quando il servizio restituisce un messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="837b3-158">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="837b3-159">Chiamata di streaming bidirezionale</span><span class="sxs-lookup"><span data-stu-id="837b3-159">Bi-directional streaming call</span></span>

<span data-ttu-id="837b3-160">Una chiamata di streaming bidirezionale inizia *senza* che il client invii un messaggio.</span><span class="sxs-lookup"><span data-stu-id="837b3-160">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="837b3-161">Il client può scegliere di inviare messaggi con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="837b3-161">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="837b3-162">I messaggi trasmessi dal servizio sono accessibili con `ResponseStream.MoveNext()` o `ResponseStream.ReadAllAsync()`.</span><span class="sxs-lookup"><span data-stu-id="837b3-162">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="837b3-163">La chiamata di streaming bidirezionale è completa quando il `ResponseStream` non contiene più messaggi.</span><span class="sxs-lookup"><span data-stu-id="837b3-163">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="837b3-164">Durante una chiamata di streaming bidirezionale, il client e il servizio possono inviare messaggi l'uno all'altro in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="837b3-164">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="837b3-165">La migliore logica client per l'interazione con una chiamata bidirezionale varia a seconda della logica del servizio.</span><span class="sxs-lookup"><span data-stu-id="837b3-165">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="837b3-166">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="837b3-166">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
