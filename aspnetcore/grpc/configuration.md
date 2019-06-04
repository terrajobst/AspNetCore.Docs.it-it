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
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC per la configurazione di ASP.NET Core

## <a name="configure-services-options"></a>Configurare le opzioni di servizi

La tabella seguente descrive le opzioni per la configurazione dei servizi gRPC:

| Opzione | Valore predefinito | Descrizione |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | La dimensione massima in byte che possono essere inviati dal server. È stato effettuato un tentativo di inviare un messaggio che supera i risultati di dimensioni massime configurate per i messaggi in un'eccezione. |
| `ReceiveMaxMessageSize` | 4 MB | La dimensione massima in byte che possono essere ricevuti dal server. Se il server riceve un messaggio che supera questo limite, genera un'eccezione. Se si aumenta questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria. |
| `EnableDetailedErrors` | `false` | Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo del servizio. Il valore predefinito è `false`. L'impostazione `EnableDetailedErrors` a `true` possono verificarsi perdite di informazioni riservate. |
| `CompressionProviders` | gzip | Raccolta di provider di compressione utilizzato per comprimere e decomprimere i messaggi. Provider di compressione personalizzato può essere creato e aggiunto alla raccolta. Il valore predefinito configurato supporta provider **gzip** la compressione. |
| `ResponseCompressionAlgorithm` | `null` | L'algoritmo di compressione utilizzato per comprimere i messaggi inviati dal server. L'algoritmo deve corrispondere a un provider di compressione in `CompressionProviders`. Per l'algoritmo comprimere una risposta, il client deve indicare supporta l'algoritmo inviandolo **grpc-codifica** intestazione. |
| `ResponseCompressionLevel` | `null` | Il livello di compressione utilizzato per comprimere i messaggi inviati dal server. |

Opzioni possono essere configurate per tutti i servizi, fornendo un delegato di opzioni per la `AddGrpc` chiamare in `Startup.ConfigureServices`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Opzioni per un singolo servizio di eseguire l'override le opzioni globali disponibili nella `AddGrpc` e possono essere configurati usando `AddServiceOptions<TService>`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Configurare le opzioni client

Il codice seguente imposta la trasmissione di massimo di client e dimensione del messaggio di ricezione:

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
