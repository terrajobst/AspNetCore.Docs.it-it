---
title: Migrazione dei servizi gRPC da C-core a ASP.NET Core
author: juntaoluo
description: Informazioni su come spostare un'app basata su gRPC C-core esistente per l'esecuzione all'inizio dello stack di ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 47d74edd821124f0c8390d704ca7931b7eb6c4cd
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982601"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>Migrazione dei servizi gRPC da C-core a ASP.NET Core

A cura di [John Luo](https://github.com/juntaoluo)

A causa dell'implementazione di stack sottostante, non tutte le funzionalità funzionano nello stesso modo tra [basate su C-core gRPC](https://grpc.io/blog/grpc-stacks) App e App basate su ASP.NET Core. Questo documento vengono evidenziate le differenze principali per la migrazione tra i due stack.

## <a name="grpc-service-implementation-lifetime"></a>durata di implementazione del servizio gRPC

Nello stack di ASP.NET Core, servizi gRPC, per impostazione predefinita, vengono creati con una [durata con ambito](xref:fundamentals/dependency-injection#service-lifetimes). Al contrario, gRPC C-core per impostazione predefinita associata a un servizio con un [durata del singleton](xref:fundamentals/dependency-injection#service-lifetimes).

Una durata con ambita consente l'implementazione del servizio risolvere altri servizi con durata con ambita. Ad esempio, una durata con ambita può anche risolvere `DBContext` dal contenitore di inserimento delle dipendenze tramite inserimento del costruttore. Utilizzo di durata con ambito:

* Per ogni richiesta viene creata una nuova istanza dell'implementazione del servizio.
* Non è possibile condividere lo stato tra le richieste tramite i membri di istanza sul tipo di implementazione.
* Nella previsione consiste nell'archiviare degli stati condivisi in un servizio singleton nel contenitore di inserimento delle dipendenze. Gli stati condivisi archiviati vengono risolti nel costruttore dell'implementazione del servizio gRPC.

Per altre informazioni su durate del servizio, vedere <xref:fundamentals/dependency-injection#service-lifetimes>.

### <a name="add-a-singleton-service"></a>Aggiungere un servizio singleton

Per facilitare la transizione da un'implementazione di C-core gRPC ad ASP.NET Core, è possibile modificare la durata del servizio di implementazione del servizio dal ha come ambito singleton. Ciò implica l'aggiunta di un'istanza dell'implementazione del servizio per il contenitore di inserimento delle dipendenze:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

Tuttavia, un'implementazione del servizio con una durata singleton non è più in grado di risolvere i servizi con ambiti tramite inserimento del costruttore.

## <a name="configure-grpc-services-options"></a>Configurare le opzioni di servizi gRPC

Nelle App basate su C-core, impostazioni, ad esempio `grpc.max_receive_message_length` e `grpc.max_send_message_length` sono configurati con `ChannelOption` quando [creazione istanza del Server](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).

In ASP.NET Core, gRPC fornisce la configurazione tramite il `GrpcServiceOptions` tipo. Ad esempio, gRPC di un servizio la dimensione massima dei messaggi in ingresso può essere configurata tramite `AddGrpc`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

Per altre informazioni sulla configurazione, vedere <xref:grpc/configuration>.

## <a name="logging"></a>Registrazione

Le app basate su C-core si basano sul `GrpcEnvironment` al [configurare il logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) a scopo di debug. Lo stack di ASP.NET Core fornisce questa funzionalità tramite il [API di registrazione](xref:fundamentals/logging/index). Ad esempio, un logger può essere aggiunto al servizio gRPC tramite inserimento del costruttore:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

Le app basate su C-core configurare HTTPS tramite il [Server.Ports proprietà](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports). Un concetto simile viene utilizzato per configurare i server in ASP.NET Core. Ad esempio, Usa Kestrel [configurazione dell'endpoint](xref:fundamentals/servers/kestrel#endpoint-configuration) per questa funzionalità.

## <a name="interceptors-and-middleware"></a>Middleware e intercettori

ASP.NET Core [middleware](xref:fundamentals/middleware/index) offre funzionalità simili rispetto agli intercettori nelle App basate su C-core gRPC. Middleware e intercettori concettualmente sono uguali perché entrambi vengono usati per costruire una pipeline che gestisce una richiesta gRPC. Entrambi consentono operazioni da eseguire prima o dopo il componente successivo nella pipeline. Tuttavia, middleware di ASP.NET Core agisce sui messaggi HTTP/2 sottostanti, mentre gli intercettori operano sul livello di astrazione usando gRPC il [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
