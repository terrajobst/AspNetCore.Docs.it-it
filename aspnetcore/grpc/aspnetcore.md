---
title: Servizi gRPC con ASP.NET Core
author: juntaoluo
description: Informazioni sui concetti di base durante la scrittura di servizi gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 1f019fac23982a95fa37d43099522f4b3e9d107a
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/23/2019
ms.locfileid: "66039276"
---
# <a name="grpc-services-with-aspnet-core"></a>Servizi gRPC con ASP.NET Core

Questo documento illustra come iniziare a usare servizi gRPC usando ASP.NET Core.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>Introduzione all'uso del servizio gRPC in ASP.NET Core

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visualizzare [Introduzione a servizi gRPC](xref:tutorials/grpc/grpc-start) per istruzioni dettagliate su come creare un progetto gRPC.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio per Mac](#tab/visual-studio-code+visual-studio-mac)

Eseguire `dotnet new grpc -o GrpcGreeter` dalla riga di comando.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>Aggiungere servizi gRPC a un'app ASP.NET Core

gRPC richiede i pacchetti seguenti:

* [Grpc.AspNetCore.Server](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) per protobuf API del messaggio.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a>Configurare gRPC

gRPC è abilitata con la `AddGrpc` metodo:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=5)]

Ogni servizio gRPC viene aggiunto alla pipeline di routing tramite il `MapGrpcService` metodo:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=21)]

Le funzionalità e il middleware di ASP.NET Core condividono la pipeline di routing, pertanto è possibile configurare un'app per servire i gestori di richiesta aggiuntivi. I gestori di richiesta aggiuntivi, ad esempio i controller MVC, operare in parallelo con i servizi configurati gRPC.

## <a name="integration-with-aspnet-core-apis"></a>Integrazione con le API ASP.NET Core

gRPC servizi hanno accesso completo alle funzionalità di ASP.NET Core, ad esempio [inserimento delle dipendenze](xref:fundamentals/dependency-injection) (inserimento delle dipendenze) e [Logging](xref:fundamentals/logging/index). Ad esempio, l'implementazione del servizio può risolvere un servizio del logger dal contenitore di inserimento delle dipendenze tramite il costruttore:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

Per impostazione predefinita, l'implementazione del servizio gRPC può risolvere altri servizi di inserimento delle dipendenze con qualsiasi ciclo di vita (Singleton, con ambito o temporaneo).

### <a name="resolve-httpcontext-in-grpc-methods"></a>Risolvere HttpContext nei metodi gRPC

GRPC API fornisce l'accesso ad alcuni dati di messaggio HTTP/2, ad esempio il metodo, host, intestazione e trailer. L'accesso avviene tramite il `ServerCallContext` argomento passato a ogni metodo gRPC:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext` non fornisce accesso completo a `HttpContext` tutti APIs ASP.NET. Il `GetHttpContext` metodo di estensione fornisce accesso completo per il `HttpContext` che rappresenta il messaggio HTTP/2 sottostante in APIs ASP.NET:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?name=snippet1)]

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
