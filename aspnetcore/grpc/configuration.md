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

## <a name="configure-kestrel-options"></a>Configurare le opzioni di Kestrel

Opzioni di configurazione che influiscono sul comportamento di gRPC per ASP.NET server kestrel.

### <a name="request-body-data-rate-limit"></a>Limite di velocità dati di corpo della richiesta

Per impostazione predefinita, il server Kestrel impone una [velocità dei dati del corpo della richiesta minima](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>). Per client di streaming e duplex flusso delle chiamate, questa frequenza può non essere soddisfatta e la connessione potrebbe essere scaduta. Il valore minimo della richiesta corpo limite di velocità dati deve essere disabilitata quando il servizio gRPC include client di streaming e duplex flusso delle chiamate:

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

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
