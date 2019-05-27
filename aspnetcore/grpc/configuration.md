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
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC per la configurazione di ASP.NET Core

## <a name="configure-services-options"></a>Configurare le opzioni di servizi

La tabella seguente descrive le opzioni per la configurazione dei servizi gRPC:

| Opzione | Valore predefinito | Descrizione |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | La dimensione massima in byte che possono essere inviati dal server. È stato effettuato un tentativo di inviare un messaggio che supera i risultati di dimensioni massime configurate per i messaggi in un'eccezione. |
| `ReceiveMaxMessageSize` | 4 MB | La dimensione massima in byte che possono essere ricevuti dal server. Se il server riceve un messaggio che supera questo limite, genera un'eccezione. Se si aumenta questo valore consente al server ricevere messaggi di grandi dimensioni, ma può influire negativamente sul consumo di memoria. |
| `EnableDetailedErrors` | `false` | Se `true`dettagliati vengono restituiti i messaggi di eccezione ai client quando viene generata un'eccezione in un metodo del servizio. Il valore predefinito è `false`. Se questa impostazione `true` possono verificarsi perdite di informazioni riservate. |
| `CompressionProviders` | gzip | Raccolta di provider di compressione utilizzato per comprimere e decomprimere i messaggi. Provider di compressione personalizzato può essere creato e aggiunto alla raccolta. Il valore predefinito configurato supporta provider **gzip** la compressione. |
| `ResponseCompressionAlgorithm` | `null` | L'algoritmo di compressione utilizzato per comprimere i messaggi inviati dal server. L'algoritmo deve corrispondere a un provider di compressione in `CompressionProviders`. Affinché il algorthm comprimere una risposta il client deve indicare supporta l'algoritmo inviandolo **grpc-codifica** intestazione. |
| `ResponseCompressionLevel` | `null` | Il livello di compressione utilizzato per comprimere i messaggi inviati dal server. |

Opzioni possono essere configurate per tutti i servizi, fornendo un delegato di opzioni per la `AddGrpc` chiamare in `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

Opzioni per un singolo servizio di eseguire l'override le opzioni globali disponibili nella `AddGrpc` e possono essere configurati usando `AddServiceOptions<TService>`:

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
