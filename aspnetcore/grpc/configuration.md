---
title: configurazione di gRPC per .NET
author: jamesnk
description: Informazioni su come configurare gRPC per le app .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 02/26/2020
uid: grpc/configuration
ms.openlocfilehash: cabe2d86f535bf3063dd7ede9e8a3bc5de70e244
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666901"
---
# <a name="grpc-for-net-configuration"></a>configurazione di gRPC per .NET

## <a name="configure-services-options"></a>Configurare le opzioni dei servizi

i servizi gRPC sono configurati con `AddGrpc` in *Startup.cs*. Nella tabella seguente vengono descritte le opzioni per la configurazione dei servizi gRPC:

| Opzione | Default Value | Descrizione |
| ------ | ------------- | ----------- |
| MaxSendMessageSize | `null` | Dimensione massima in byte del messaggio che può essere inviata dal server. Il tentativo di inviare un messaggio che supera le dimensioni massime configurate per i messaggi comporta un'eccezione. |
| MaxReceiveMessageSize | 4 MB | Dimensione massima in byte del messaggio che può essere ricevuto dal server. Se il server riceve un messaggio che supera questo limite, viene generata un'eccezione. L'aumento di questo valore consente al server di ricevere messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria. |
| EnableDetailedErrors | `false` | Se `true`, i messaggi di eccezione dettagliati vengono restituiti ai client quando viene generata un'eccezione in un metodo del servizio. Il valore predefinito è `false`. L'impostazione `EnableDetailedErrors` su `true` può comportare la perdita di informazioni riservate. |
| CompressionProviders | gzip | Raccolta di provider di compressione utilizzati per comprimere e decomprimere i messaggi. I provider di compressione personalizzati possono essere creati e aggiunti alla raccolta. I provider configurati predefiniti supportano la compressione **gzip** . |
| <span style="word-break:normal;word-wrap:normal">ResponseCompressionAlgorithm</span> | `null` | Algoritmo di compressione usato per comprimere i messaggi inviati dal server. L'algoritmo deve corrispondere a un provider di compressione in `CompressionProviders`. Per comprimere una risposta, il client deve indicare che supporta l'algoritmo inviando l'algoritmo nell'intestazione **grpc-Accept-Encoding** . |
| ResponseCompressionLevel | `null` | Livello di compressione utilizzato per comprimere i messaggi inviati dal server. |
| Intercettori | nessuno | Raccolta di intercettori eseguiti con ogni chiamata gRPC. Gli intercettori vengono eseguiti nell'ordine in cui sono registrati. Gli intercettori configurati a livello globale vengono eseguiti prima degli intercettori configurati per un singolo servizio. Per ulteriori informazioni sugli intercettori gRPC, vedere [GRPC Interceptors vs. middleware](xref:grpc/migration#grpc-interceptors-vs-middleware). |

È possibile configurare le opzioni per tutti i servizi fornendo un delegato Options per la chiamata `AddGrpc` in `Startup.ConfigureServices`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Le opzioni per un singolo servizio eseguono l'override delle opzioni globali fornite in `AddGrpc` e possono essere configurate utilizzando `AddServiceOptions<TService>`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Configurare le opzioni client

la configurazione del client gRPC è impostata su `GrpcChannelOptions`. Nella tabella seguente vengono descritte le opzioni per la configurazione dei canali gRPC:

| Opzione | Default Value | Descrizione |
| ------ | ------------- | ----------- |
| HttpClient | Nuova istanza | `HttpClient` utilizzata per eseguire chiamate gRPC. Un client può essere impostato per configurare un `HttpClientHandler`personalizzato o per aggiungere ulteriori gestori alla pipeline HTTP per le chiamate gRPC. Se non viene specificato alcun `HttpClient`, viene creata una nuova istanza di `HttpClient` per il canale. Verrà eliminato automaticamente. |
| DisposeHttpClient | `false` | Se `true`e viene specificata una `HttpClient`, l'istanza di `HttpClient` verrà eliminata quando viene eliminato il `GrpcChannel`. |
| LoggerFactory | `null` | Il `LoggerFactory` usato dal client per registrare le informazioni sulle chiamate gRPC. Un'istanza di `LoggerFactory` può essere risolta dall'inserimento delle dipendenze o creata utilizzando `LoggerFactory.Create`. Per esempi di configurazione della registrazione, vedere <xref:grpc/diagnostics#grpc-client-logging>. |
| MaxSendMessageSize | `null` | Dimensione massima in byte del messaggio che può essere inviato dal client. Il tentativo di inviare un messaggio che supera le dimensioni massime configurate per i messaggi comporta un'eccezione. |
| <span style="word-break:normal;word-wrap:normal">MaxReceiveMessageSize</span> | 4 MB | Dimensione massima del messaggio, in byte, che può essere ricevuta dal client. Se il client riceve un messaggio che supera questo limite, viene generata un'eccezione. L'aumento di questo valore consente al client di ricevere messaggi di dimensioni maggiori, ma può influire negativamente sull'utilizzo della memoria. |
| Credenziali | `null` | Istanza di `ChannelCredentials`. Le credenziali vengono usate per aggiungere i metadati di autenticazione alle chiamate gRPC. |
| CompressionProviders | gzip | Raccolta di provider di compressione utilizzati per comprimere e decomprimere i messaggi. I provider di compressione personalizzati possono essere creati e aggiunti alla raccolta. I provider configurati predefiniti supportano la compressione **gzip** . |

Il codice seguente:

* Imposta la dimensione massima del messaggio di invio e ricezione sul canale.
* Crea un client.

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/diagnostics>
* <xref:tutorials/grpc/grpc-start>
