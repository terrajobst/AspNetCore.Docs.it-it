---
title: gRPC per la configurazione di ASP.NET Core
author: jamesnk
description: Informazioni su come configurare gRPC per le app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 05/30/2019
uid: grpc/configuration
ms.openlocfilehash: e269d701f45c0b852a9006107f0162cc5af2c38a
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814919"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="78626-103">gRPC per la configurazione di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78626-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="78626-104">Configurare le opzioni di servizi</span><span class="sxs-lookup"><span data-stu-id="78626-104">Configure services options</span></span>

<span data-ttu-id="78626-105">La tabella seguente descrive le opzioni per la configurazione dei servizi gRPC:</span><span class="sxs-lookup"><span data-stu-id="78626-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="78626-106">Opzione</span><span class="sxs-lookup"><span data-stu-id="78626-106">Option</span></span> | <span data-ttu-id="78626-107">Default Value</span><span class="sxs-lookup"><span data-stu-id="78626-107">Default Value</span></span> | <span data-ttu-id="78626-108">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="78626-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="78626-109">La dimensione massima in byte che possono essere inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="78626-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="78626-110">È stato effettuato un tentativo di inviare un messaggio che supera i risultati di dimensioni massime configurate per i messaggi in un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="78626-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="78626-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="78626-111">4 MB</span></span> | <span data-ttu-id="78626-112">La dimensione massima in byte che possono essere ricevuti dal server.</span><span class="sxs-lookup"><span data-stu-id="78626-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="78626-113">Se il server riceve un messaggio che supera questo limite, genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="78626-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="78626-114">Se si aumenta questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="78626-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="78626-115">Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo del servizio.</span><span class="sxs-lookup"><span data-stu-id="78626-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="78626-116">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="78626-116">The default is `false`.</span></span> <span data-ttu-id="78626-117">L'impostazione `EnableDetailedErrors` a `true` possono verificarsi perdite di informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="78626-117">Setting `EnableDetailedErrors` to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="78626-118">gzip</span><span class="sxs-lookup"><span data-stu-id="78626-118">gzip</span></span> | <span data-ttu-id="78626-119">Raccolta di provider di compressione utilizzato per comprimere e decomprimere i messaggi.</span><span class="sxs-lookup"><span data-stu-id="78626-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="78626-120">Provider di compressione personalizzato può essere creato e aggiunto alla raccolta.</span><span class="sxs-lookup"><span data-stu-id="78626-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="78626-121">Il valore predefinito configurato supporta provider **gzip** la compressione.</span><span class="sxs-lookup"><span data-stu-id="78626-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="78626-122">L'algoritmo di compressione utilizzato per comprimere i messaggi inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="78626-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="78626-123">L'algoritmo deve corrispondere a un provider di compressione in `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="78626-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="78626-124">Per l'algoritmo comprimere una risposta, il client deve indicare supporta l'algoritmo inviandolo **grpc-codifica** intestazione.</span><span class="sxs-lookup"><span data-stu-id="78626-124">For the algorithm to compress a response, the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="78626-125">Il livello di compressione utilizzato per comprimere i messaggi inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="78626-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="78626-126">Opzioni possono essere configurate per tutti i servizi, fornendo un delegato di opzioni per la `AddGrpc` chiamare in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="78626-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

<span data-ttu-id="78626-127">Opzioni per un singolo servizio di eseguire l'override le opzioni globali disponibili nella `AddGrpc` e possono essere configurati usando `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="78626-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a><span data-ttu-id="78626-128">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="78626-128">Configure client options</span></span>

<span data-ttu-id="78626-129">Il codice seguente imposta la trasmissione di massimo di client e dimensione del messaggio di ricezione:</span><span class="sxs-lookup"><span data-stu-id="78626-129">The following code sets the client maximum send and receive message size:</span></span>

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a><span data-ttu-id="78626-130">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="78626-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
