---
title: gRPC per la configurazione di ASP.NET Core
author: jamesnk
description: Informazioni su come configurare gRPC per le app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 851c9ca1f7d62f6f368df66bb38eb4bbaf64bf32
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/23/2019
ms.locfileid: "66041886"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="3f14c-103">gRPC per la configurazione di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f14c-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="3f14c-104">Configurare le opzioni di servizi</span><span class="sxs-lookup"><span data-stu-id="3f14c-104">Configure services options</span></span>

<span data-ttu-id="3f14c-105">La tabella seguente descrive le opzioni per la configurazione dei servizi gRPC:</span><span class="sxs-lookup"><span data-stu-id="3f14c-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="3f14c-106">Opzione</span><span class="sxs-lookup"><span data-stu-id="3f14c-106">Option</span></span> | <span data-ttu-id="3f14c-107">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="3f14c-107">Default Value</span></span> | <span data-ttu-id="3f14c-108">Descrizione</span><span class="sxs-lookup"><span data-stu-id="3f14c-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="3f14c-109">La dimensione massima in byte che possono essere inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="3f14c-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="3f14c-110">È stato effettuato un tentativo di inviare un messaggio che supera i risultati di dimensioni massime configurate per i messaggi in un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="3f14c-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="3f14c-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="3f14c-111">4 MB</span></span> | <span data-ttu-id="3f14c-112">La dimensione massima in byte che possono essere ricevuti dal server.</span><span class="sxs-lookup"><span data-stu-id="3f14c-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="3f14c-113">Se il server riceve un messaggio che supera questo limite, genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="3f14c-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="3f14c-114">Se si aumenta questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="3f14c-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="3f14c-115">Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo del servizio.</span><span class="sxs-lookup"><span data-stu-id="3f14c-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="3f14c-116">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="3f14c-116">The default is `false`.</span></span> <span data-ttu-id="3f14c-117">Se questa impostazione `true` possono verificarsi perdite di informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="3f14c-117">Setting this to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="3f14c-118">gzip</span><span class="sxs-lookup"><span data-stu-id="3f14c-118">gzip</span></span> | <span data-ttu-id="3f14c-119">Raccolta di provider di compressione utilizzato per comprimere e decomprimere i messaggi.</span><span class="sxs-lookup"><span data-stu-id="3f14c-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="3f14c-120">Provider di compressione personalizzato può essere creato e aggiunto alla raccolta.</span><span class="sxs-lookup"><span data-stu-id="3f14c-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="3f14c-121">Il valore predefinito configurato supporta provider **gzip** la compressione.</span><span class="sxs-lookup"><span data-stu-id="3f14c-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="3f14c-122">L'algoritmo di compressione utilizzato per comprimere i messaggi inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="3f14c-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="3f14c-123">L'algoritmo deve corrispondere a un provider di compressione in `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="3f14c-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="3f14c-124">Affinché il algorthm comprimere una risposta il client deve indicare supporta l'algoritmo inviandolo **grpc-codifica** intestazione.</span><span class="sxs-lookup"><span data-stu-id="3f14c-124">For the algorthm to compress a response the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="3f14c-125">Il livello di compressione utilizzato per comprimere i messaggi inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="3f14c-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="3f14c-126">Opzioni possono essere configurate per tutti i servizi, fornendo un delegato di opzioni per la `AddGrpc` chiamare in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3f14c-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

<span data-ttu-id="3f14c-127">Opzioni per un singolo servizio di eseguire l'override le opzioni globali disponibili nella `AddGrpc` e possono essere configurati usando `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="3f14c-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="additional-resources"></a><span data-ttu-id="3f14c-128">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3f14c-128">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
