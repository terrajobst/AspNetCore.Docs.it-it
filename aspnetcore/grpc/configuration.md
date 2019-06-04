---
title: gRPC per la configurazione di ASP.NET Core
author: jamesnk
description: Informazioni su come configurare gRPC per le app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 5/30/2019
uid: grpc/configuration
ms.openlocfilehash: 1f8250dc9aa8b82da384ee28287011baa19dc11f
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491241"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="faa79-103">gRPC per la configurazione di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="faa79-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="faa79-104">Configurare le opzioni di servizi</span><span class="sxs-lookup"><span data-stu-id="faa79-104">Configure services options</span></span>

<span data-ttu-id="faa79-105">La tabella seguente descrive le opzioni per la configurazione dei servizi gRPC:</span><span class="sxs-lookup"><span data-stu-id="faa79-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="faa79-106">Opzione</span><span class="sxs-lookup"><span data-stu-id="faa79-106">Option</span></span> | <span data-ttu-id="faa79-107">Valore predefinito</span><span class="sxs-lookup"><span data-stu-id="faa79-107">Default Value</span></span> | <span data-ttu-id="faa79-108">Descrizione</span><span class="sxs-lookup"><span data-stu-id="faa79-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="faa79-109">La dimensione massima in byte che possono essere inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="faa79-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="faa79-110">È stato effettuato un tentativo di inviare un messaggio che supera i risultati di dimensioni massime configurate per i messaggi in un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="faa79-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="faa79-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="faa79-111">4 MB</span></span> | <span data-ttu-id="faa79-112">La dimensione massima in byte che possono essere ricevuti dal server.</span><span class="sxs-lookup"><span data-stu-id="faa79-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="faa79-113">Se il server riceve un messaggio che supera questo limite, genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="faa79-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="faa79-114">Se si aumenta questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria.</span><span class="sxs-lookup"><span data-stu-id="faa79-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="faa79-115">Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo del servizio.</span><span class="sxs-lookup"><span data-stu-id="faa79-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="faa79-116">Il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="faa79-116">The default is `false`.</span></span> <span data-ttu-id="faa79-117">L'impostazione `EnableDetailedErrors` a `true` possono verificarsi perdite di informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="faa79-117">Setting `EnableDetailedErrors` to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="faa79-118">gzip</span><span class="sxs-lookup"><span data-stu-id="faa79-118">gzip</span></span> | <span data-ttu-id="faa79-119">Raccolta di provider di compressione utilizzato per comprimere e decomprimere i messaggi.</span><span class="sxs-lookup"><span data-stu-id="faa79-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="faa79-120">Provider di compressione personalizzato può essere creato e aggiunto alla raccolta.</span><span class="sxs-lookup"><span data-stu-id="faa79-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="faa79-121">Il valore predefinito configurato supporta provider **gzip** la compressione.</span><span class="sxs-lookup"><span data-stu-id="faa79-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="faa79-122">L'algoritmo di compressione utilizzato per comprimere i messaggi inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="faa79-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="faa79-123">L'algoritmo deve corrispondere a un provider di compressione in `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="faa79-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="faa79-124">Per l'algoritmo comprimere una risposta, il client deve indicare supporta l'algoritmo inviandolo **grpc-codifica** intestazione.</span><span class="sxs-lookup"><span data-stu-id="faa79-124">For the algorithm to compress a response, the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="faa79-125">Il livello di compressione utilizzato per comprimere i messaggi inviati dal server.</span><span class="sxs-lookup"><span data-stu-id="faa79-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="faa79-126">Opzioni possono essere configurate per tutti i servizi, fornendo un delegato di opzioni per la `AddGrpc` chiamare in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="faa79-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

<span data-ttu-id="faa79-127">Opzioni per un singolo servizio di eseguire l'override le opzioni globali disponibili nella `AddGrpc` e possono essere configurati usando `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="faa79-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a><span data-ttu-id="faa79-128">Configurare le opzioni client</span><span class="sxs-lookup"><span data-stu-id="faa79-128">Configure client options</span></span>

<span data-ttu-id="faa79-129">Il codice seguente imposta la trasmissione di massimo di client e dimensione del messaggio di ricezione:</span><span class="sxs-lookup"><span data-stu-id="faa79-129">The following code sets the client maximum send and receive message size:</span></span>

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a><span data-ttu-id="faa79-130">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="faa79-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
