---
title: Chiamare i servizi gRPC con il client .NET
author: jamesnk
description: Informazioni su come chiamare i servizi gRPC con il client gRPC .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 56f79b303a8d53699e8eb6156d328c0da1259416
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011141"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="b1f97-103">Chiamare i servizi gRPC con il client .NET</span><span class="sxs-lookup"><span data-stu-id="b1f97-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="b1f97-104">Una libreria client di .NET gRPC è disponibile nel pacchetto NuGet [.NET. client di gRPC](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="b1f97-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="b1f97-105">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="b1f97-105">This document explains how to:</span></span>

* <span data-ttu-id="b1f97-106">Configurare un client gRPC per chiamare i servizi gRPC.</span><span class="sxs-lookup"><span data-stu-id="b1f97-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="b1f97-107">Eseguire chiamate gRPC a metodi unari, streaming server, flusso client e flusso bidirezionale.</span><span class="sxs-lookup"><span data-stu-id="b1f97-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="b1f97-108">Configurare il client gRPC</span><span class="sxs-lookup"><span data-stu-id="b1f97-108">Configure gRPC client</span></span>

<span data-ttu-id="b1f97-109">i client gRPC sono tipi di client concreti [generati  *\*da file. proto* ](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="b1f97-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="b1f97-110">Il client gRPC concreto dispone di metodi che vengono convertiti nel servizio gRPC nel  *\*file. proto* .</span><span class="sxs-lookup"><span data-stu-id="b1f97-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="b1f97-111">Un client gRPC viene creato da un canale.</span><span class="sxs-lookup"><span data-stu-id="b1f97-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="b1f97-112">Iniziare usando `GrpcChannel.ForAddress` per creare un canale e quindi usare il canale per creare un client gRPC:</span><span class="sxs-lookup"><span data-stu-id="b1f97-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="b1f97-113">Un canale rappresenta una connessione di lunga durata a un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="b1f97-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="b1f97-114">Quando viene creato un canale, questo viene configurato con le opzioni correlate alla chiamata a un servizio.</span><span class="sxs-lookup"><span data-stu-id="b1f97-114">When a channel is created it is configured with options related to calling a service.</span></span> <span data-ttu-id="b1f97-115">Ad esempio, l' `HttpClient` oggetto utilizzato per effettuare chiamate, la dimensione massima dei messaggi di invio e ricezione e la registrazione possono `GrpcChannelOptions` essere specificati e `GrpcChannel.ForAddress`utilizzati con.</span><span class="sxs-lookup"><span data-stu-id="b1f97-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="b1f97-116">Per un elenco completo delle opzioni, vedere [Opzioni di configurazione client](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="b1f97-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

<span data-ttu-id="b1f97-117">La creazione di un canale può essere un'operazione costosa e il riutilizzo di un canale per le chiamate gRPC offre vantaggi in merito alle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="b1f97-117">Creating a channel can be an expensive operation and reusing a channel for gRPC calls offers performance benefits.</span></span> <span data-ttu-id="b1f97-118">È possibile creare più client gRPC concreti da un canale, inclusi tipi diversi di client.</span><span class="sxs-lookup"><span data-stu-id="b1f97-118">Multiple concrete gRPC clients can be created from a channel, including different types of clients.</span></span> <span data-ttu-id="b1f97-119">I tipi di client gRPC concreti sono oggetti leggeri e possono essere creati quando necessario.</span><span class="sxs-lookup"><span data-stu-id="b1f97-119">Concrete gRPC client types are lightweight objects and can be created when needed.</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="b1f97-120">`GrpcChannel.ForAddress`non è l'unica opzione per la creazione di un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="b1f97-120">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="b1f97-121">Se si chiamano i servizi gRPC da un'app ASP.NET Core, prendere in considerazione l' [integrazione di gRPC client Factory](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="b1f97-121">If you are calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="b1f97-122">l'integrazione di `HttpClientFactory` gRPC con offre un'alternativa centralizzata alla creazione di client gRPC.</span><span class="sxs-lookup"><span data-stu-id="b1f97-122">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="b1f97-123">È necessaria una configurazione aggiuntiva per [chiamare i servizi gRPC non protetti con il client .NET](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="b1f97-123">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="b1f97-124">Effettuare chiamate a gRPC</span><span class="sxs-lookup"><span data-stu-id="b1f97-124">Make gRPC calls</span></span>

<span data-ttu-id="b1f97-125">Una chiamata gRPC viene avviata chiamando un metodo sul client.</span><span class="sxs-lookup"><span data-stu-id="b1f97-125">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="b1f97-126">Il client gRPC gestirà la serializzazione dei messaggi e indirizza la chiamata gRPC al servizio corretto.</span><span class="sxs-lookup"><span data-stu-id="b1f97-126">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="b1f97-127">gRPC dispone di tipi diversi di metodi.</span><span class="sxs-lookup"><span data-stu-id="b1f97-127">gRPC has different types of methods.</span></span> <span data-ttu-id="b1f97-128">Il modo in cui si usa il client per effettuare una chiamata gRPC dipende dal tipo di metodo che si sta chiamando.</span><span class="sxs-lookup"><span data-stu-id="b1f97-128">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="b1f97-129">I tipi di metodo gRPC sono:</span><span class="sxs-lookup"><span data-stu-id="b1f97-129">The gRPC method types are:</span></span>

* <span data-ttu-id="b1f97-130">Unario</span><span class="sxs-lookup"><span data-stu-id="b1f97-130">Unary</span></span>
* <span data-ttu-id="b1f97-131">Streaming Server</span><span class="sxs-lookup"><span data-stu-id="b1f97-131">Server streaming</span></span>
* <span data-ttu-id="b1f97-132">Flusso client</span><span class="sxs-lookup"><span data-stu-id="b1f97-132">Client streaming</span></span>
* <span data-ttu-id="b1f97-133">Streaming bidirezionale</span><span class="sxs-lookup"><span data-stu-id="b1f97-133">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="b1f97-134">Chiamata unaria</span><span class="sxs-lookup"><span data-stu-id="b1f97-134">Unary call</span></span>

<span data-ttu-id="b1f97-135">Una chiamata unaria inizia con il client che invia un messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="b1f97-135">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="b1f97-136">Al termine del servizio viene restituito un messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="b1f97-136">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="b1f97-137">Ogni metodo del servizio unario nel  *\*file. proto* genererà due metodi .NET sul tipo di client gRPC concreto per chiamare il metodo: un metodo asincrono e un metodo di blocco.</span><span class="sxs-lookup"><span data-stu-id="b1f97-137">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="b1f97-138">Ad esempio, in `GreeterClient` sono disponibili due modi per chiamare `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="b1f97-138">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="b1f97-139">`GreeterClient.SayHelloAsync`: chiama `Greeter.SayHello` il servizio in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="b1f97-139">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="b1f97-140">Può essere atteso.</span><span class="sxs-lookup"><span data-stu-id="b1f97-140">Can be awaited.</span></span>
* <span data-ttu-id="b1f97-141">`GreeterClient.SayHello`: chiama `Greeter.SayHello` il servizio e blocca fino al completamento.</span><span class="sxs-lookup"><span data-stu-id="b1f97-141">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="b1f97-142">Non usare nel codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="b1f97-142">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="b1f97-143">Chiamata di streaming del server</span><span class="sxs-lookup"><span data-stu-id="b1f97-143">Server streaming call</span></span>

<span data-ttu-id="b1f97-144">Una chiamata di streaming del server inizia con il client che invia un messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="b1f97-144">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="b1f97-145">`ResponseStream.MoveNext()`legge i messaggi trasmessi dal servizio.</span><span class="sxs-lookup"><span data-stu-id="b1f97-145">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="b1f97-146">La chiamata di streaming del server è `ResponseStream.MoveNext()` completata `false`quando restituisce.</span><span class="sxs-lookup"><span data-stu-id="b1f97-146">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="b1f97-147">Se si utilizza C# 8 o versioni successive, è `await foreach` possibile utilizzare la sintassi per leggere i messaggi.</span><span class="sxs-lookup"><span data-stu-id="b1f97-147">If you are using C# 8 or later then the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="b1f97-148">Il `IAsyncStreamReader<T>.ReadAllAsync()` metodo di estensione legge tutti i messaggi dal flusso di risposta:</span><span class="sxs-lookup"><span data-stu-id="b1f97-148">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="b1f97-149">Chiamata di streaming client</span><span class="sxs-lookup"><span data-stu-id="b1f97-149">Client streaming call</span></span>

<span data-ttu-id="b1f97-150">Una chiamata di streaming client viene avviata *senza* che il client invii un messaggio.</span><span class="sxs-lookup"><span data-stu-id="b1f97-150">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="b1f97-151">Il client può scegliere di inviare messaggi inviati con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="b1f97-151">The client can choose to send sends messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="b1f97-152">Al termine dell'invio dei messaggi `RequestStream.CompleteAsync` da parte del client, è necessario chiamare per inviare una notifica al servizio.</span><span class="sxs-lookup"><span data-stu-id="b1f97-152">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="b1f97-153">La chiamata viene completata quando il servizio restituisce un messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="b1f97-153">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="b1f97-154">Chiamata di streaming bidirezionale</span><span class="sxs-lookup"><span data-stu-id="b1f97-154">Bi-directional streaming call</span></span>

<span data-ttu-id="b1f97-155">Una chiamata di streaming bidirezionale inizia *senza* che il client invii un messaggio.</span><span class="sxs-lookup"><span data-stu-id="b1f97-155">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="b1f97-156">Il client può scegliere di inviare messaggi con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="b1f97-156">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="b1f97-157">I messaggi trasmessi dal servizio sono accessibili con `ResponseStream.MoveNext()` o. `ResponseStream.ReadAllAsync()`</span><span class="sxs-lookup"><span data-stu-id="b1f97-157">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="b1f97-158">La chiamata di streaming bidirezionale è completa quando `ResponseStream` non ha più messaggi.</span><span class="sxs-lookup"><span data-stu-id="b1f97-158">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="b1f97-159">Durante una chiamata di streaming bidirezionale, il client e il servizio possono inviare messaggi l'uno all'altro in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="b1f97-159">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="b1f97-160">La migliore logica client per l'interazione con una chiamata bidirezionale varia a seconda della logica del servizio.</span><span class="sxs-lookup"><span data-stu-id="b1f97-160">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1f97-161">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b1f97-161">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
