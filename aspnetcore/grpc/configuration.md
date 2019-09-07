---
title: gRPC per la configurazione di ASP.NET Core
author: jamesnk
description: Informazioni su come configurare gRPC per le app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 09/05/2019
uid: grpc/configuration
ms.openlocfilehash: d6f095820271a3bb07e05e29299fbb82b042983b
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773675"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC per la configurazione di ASP.NET Core

## <a name="configure-services-options"></a>Configurare le opzioni dei servizi

Nella tabella seguente vengono descritte le opzioni per la configurazione dei servizi gRPC:

| Opzione | Default Value | Descrizione |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | Dimensione massima in byte del messaggio che può essere inviata dal server. Il tentativo di inviare un messaggio che supera le dimensioni massime configurate per i messaggi comporta un'eccezione. |
| `MaxReceiveMessageSize` | 4 MB | Dimensione massima in byte del messaggio che può essere ricevuto dal server. Se il server riceve un messaggio che supera questo limite, viene generata un'eccezione. L'aumento di questo valore consente al server di ricevere messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria. |
| `EnableDetailedErrors` | `false` | Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo del servizio. Il valore predefinito è `false`. L' `EnableDetailedErrors` impostazione `true` di su può infiltrare le informazioni riservate. |
| `CompressionProviders` | gzip | Raccolta di provider di compressione utilizzati per comprimere e decomprimere i messaggi. I provider di compressione personalizzati possono essere creati e aggiunti alla raccolta. I provider configurati predefiniti supportano la compressione **gzip** . |
| `ResponseCompressionAlgorithm` | `null` | Algoritmo di compressione usato per comprimere i messaggi inviati dal server. L'algoritmo deve corrispondere a un provider di `CompressionProviders`compressione in. Per comprimere una risposta, il client deve indicare che supporta l'algoritmo inviando l'algoritmo nell'intestazione **grpc-Accept-Encoding** . |
| `ResponseCompressionLevel` | `null` | Livello di compressione utilizzato per comprimere i messaggi inviati dal server. |

È possibile configurare le opzioni per tutti i servizi fornendo le `AddGrpc` opzioni delegate alla chiamata in: `Startup.ConfigureServices`

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Le opzioni per un singolo servizio eseguono l'override delle opzioni `AddGrpc` globali fornite in e possono `AddServiceOptions<TService>`essere configurate utilizzando:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Configurare le opzioni client

Nella tabella seguente vengono descritte le opzioni per la configurazione dei canali gRPC:

| Opzione | Default Value | DESCRIZIONE |
| ------ | ------------- | ----------- |
| `HttpClient` | Nuova istanza | Oggetto `HttpClient` utilizzato per eseguire chiamate gRPC. Un client può essere impostato per configurare un oggetto `HttpClientHandler`personalizzato o aggiungere ulteriori gestori alla pipeline HTTP per le chiamate gRPC. Se non `HttpClient` viene specificato alcun valore, viene `HttpClient` creata una nuova istanza per il canale. Verrà eliminato automaticamente. |
| `DisposeHttpClient` | `false` | Se `true`e si specifica `HttpClient` un oggetto, l' `HttpClient` `GrpcChannel` istanza verrà eliminata quando viene eliminato. |
| `LoggerFactory` | `null` | `LoggerFactory` Utilizzato dal client per registrare le informazioni sulle chiamate gRPC. Un' `LoggerFactory` istanza può essere risolta dall'inserimento delle dipendenze `LoggerFactory.Create`o creata utilizzando. Per esempi di configurazione della registrazione, <xref:fundamentals/logging/index>vedere. |
| `MaxSendMessageSize` | `null` | Dimensione massima in byte del messaggio che può essere inviato dal client. Il tentativo di inviare un messaggio che supera le dimensioni massime configurate per i messaggi comporta un'eccezione. |
| `MaxReceiveMessageSize` | 4 MB | Dimensione massima del messaggio, in byte, che può essere ricevuta dal client. Se il client riceve un messaggio che supera questo limite, viene generata un'eccezione. L'aumento di questo valore consente al client di ricevere messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria. |
| `Credentials` | `null` | Istanza di `ChannelCredentials`. Le credenziali vengono usate per aggiungere i metadati di autenticazione alle chiamate gRPC. |
| `CompressionProviders` | gzip | Raccolta di provider di compressione utilizzati per comprimere e decomprimere i messaggi. I provider di compressione personalizzati possono essere creati e aggiunti alla raccolta. I provider configurati predefiniti supportano la compressione **gzip** . |

Il codice seguente:

* Imposta la dimensione massima del messaggio di invio e ricezione sul canale.
* Crea un client.

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:tutorials/grpc/grpc-start>
