---
title: gRPC per la configurazione di ASP.NET Core
author: jamesnk
description: Informazioni su come configurare gRPC per le app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983133"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="453e1-103">gRPC per la configurazione di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="453e1-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="453e1-104">Configurare le opzioni di servizi</span><span class="sxs-lookup"><span data-stu-id="453e1-104">Configure services options</span></span>

<span data-ttu-id="453e1-105">La tabella seguente descrive le opzioni per la configurazione dei servizi gRPC:</span><span class="sxs-lookup"><span data-stu-id="453e1-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="453e1-106">Opzione</span><span class="sxs-lookup"><span data-stu-id="453e1-106">Option</span></span> | <span data-ttu-id="453e1-107">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="453e1-107">Default Value</span></span> | <span data-ttu-id="453e1-108">Descrizione</span><span class="sxs-lookup"><span data-stu-id="453e1-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="453e1-109">La dimensione massima in byte che possono essere inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="453e1-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="453e1-110">È stato effettuato un tentativo di inviare un messaggio che supera i risultati di dimensioni massime configurate per i messaggi in un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="453e1-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="453e1-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="453e1-111">4 MB</span></span> | <span data-ttu-id="453e1-112">La dimensione massima in byte che possono essere ricevuti dal server.</span><span class="sxs-lookup"><span data-stu-id="453e1-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="453e1-113">Se il server riceve un messaggio che supera questo limite, genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="453e1-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="453e1-114">Se si aumenta questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="453e1-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="453e1-115">Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo del servizio.</span><span class="sxs-lookup"><span data-stu-id="453e1-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="453e1-116">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="453e1-116">The default is `false`.</span></span> <span data-ttu-id="453e1-117">Se questa impostazione `true` possono verificarsi perdite di informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="453e1-117">Setting this to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="453e1-118">gzip</span><span class="sxs-lookup"><span data-stu-id="453e1-118">gzip</span></span> | <span data-ttu-id="453e1-119">Raccolta di provider di compressione utilizzato per comprimere e decomprimere i messaggi.</span><span class="sxs-lookup"><span data-stu-id="453e1-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="453e1-120">Provider di compressione personalizzato può essere creato e aggiunto alla raccolta.</span><span class="sxs-lookup"><span data-stu-id="453e1-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="453e1-121">Il valore predefinito configurato supporta provider **gzip** la compressione.</span><span class="sxs-lookup"><span data-stu-id="453e1-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="453e1-122">L'algoritmo di compressione utilizzato per comprimere i messaggi inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="453e1-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="453e1-123">L'algoritmo deve corrispondere a un provider di compressione in `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="453e1-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="453e1-124">Affinché il algorthm comprimere una risposta il client deve indicare supporta l'algoritmo inviandolo **grpc-codifica** intestazione.</span><span class="sxs-lookup"><span data-stu-id="453e1-124">For the algorthm to compress a response the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="453e1-125">Il livello di compressione utilizzato per comprimere i messaggi inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="453e1-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="453e1-126">Opzioni possono essere configurate per tutti i servizi, fornendo un delegato di opzioni per la `AddGrpc` chiamare in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="453e1-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

<span data-ttu-id="453e1-127">Opzioni per un singolo servizio di eseguire l'override le opzioni globali disponibili nella `AddGrpc` e possono essere configurati usando `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="453e1-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a><span data-ttu-id="453e1-128">Configurare le opzioni di Kestrel</span><span class="sxs-lookup"><span data-stu-id="453e1-128">Configure Kestrel options</span></span>

<span data-ttu-id="453e1-129">Opzioni di configurazione che influiscono sul comportamento di gRPC per ASP.NET server kestrel.</span><span class="sxs-lookup"><span data-stu-id="453e1-129">Kestrel server has configuration options that affect the behavior of gRPC for ASP.NET.</span></span>

### <a name="request-body-data-rate-limit"></a><span data-ttu-id="453e1-130">Limite di velocità dati di corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="453e1-130">Request body data rate limit</span></span>

<span data-ttu-id="453e1-131">Per impostazione predefinita, il server Kestrel impone una [velocità dei dati del corpo della richiesta minima](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span><span class="sxs-lookup"><span data-stu-id="453e1-131">By default, the Kestrel server imposes a [minimum request body data rate](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span></span> <span data-ttu-id="453e1-132">Per client di streaming e duplex flusso delle chiamate, questa frequenza può non essere soddisfatta e la connessione potrebbe essere scaduta. Il valore minimo della richiesta corpo limite di velocità dati deve essere disabilitata quando il servizio gRPC include client di streaming e duplex flusso delle chiamate:</span><span class="sxs-lookup"><span data-stu-id="453e1-132">For client streaming and duplex streaming calls, this rate may not be satisfied and the connection may be timed out. The minimum request body data rate limit must be disabled when the gRPC service includes client streaming and duplex streaming calls:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a><span data-ttu-id="453e1-133">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="453e1-133">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
