---
title: Servizi gRPC con ASP.NET Core
author: juntaoluo
description: Informazioni sui concetti di base per la scrittura di servizi gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 38111c152c581c50767f9cd4e5fa257bd3fd804e
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022307"
---
# <a name="grpc-services-with-aspnet-core"></a>Servizi gRPC con ASP.NET Core

Questo documento illustra come iniziare a usare i servizi di gRPC con ASP.NET Core.

## <a name="prerequisites"></a>Prerequisiti

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>Introduzione all'uso del servizio gRPC in ASP.NET Core

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Per istruzioni dettagliate su come creare un progetto gRPC, vedere [Introduzione ai servizi gRPC](xref:tutorials/grpc/grpc-start) .

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio per Mac](#tab/visual-studio-code+visual-studio-mac)

Eseguire `dotnet new grpc -o GrpcGreeter` dalla riga di comando.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>Aggiungere servizi gRPC a un'app ASP.NET Core

gRPC richiede il pacchetto [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .

### <a name="configure-grpc"></a>Configurare gRPC

gRPC è abilitato con il `AddGrpc` metodo:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

Ogni servizio gRPC viene aggiunto alla pipeline di routing tramite il `MapGrpcService` metodo:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

ASP.NET Core middleware e funzionalità condividono la pipeline di routing, di conseguenza è possibile configurare un'app per gestire gestori di richieste aggiuntivi. I gestori di richieste aggiuntivi, ad esempio i controller MVC, funzionano in parallelo con i servizi gRPC configurati.

## <a name="integration-with-aspnet-core-apis"></a>Integrazione con API ASP.NET Core

i servizi gRPC hanno accesso completo alle funzionalità di ASP.NET Core come l' [inserimento](xref:fundamentals/dependency-injection) delle dipendenze e la [registrazione](xref:fundamentals/logging/index). Ad esempio, l'implementazione del servizio può risolvere un servizio logger dal contenitore DI inserimento delle dipendenze tramite il costruttore:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

Per impostazione predefinita, l'implementazione del servizio gRPC è in grado di risolvere altri servizi DI con qualsiasi durata (singleton, ambito o temporaneo).

### <a name="resolve-httpcontext-in-grpc-methods"></a>Risolvere HttpContext nei metodi gRPC

L'API gRPC consente di accedere ad alcuni dati del messaggio HTTP/2, ad esempio il metodo, l'host, l'intestazione e i trailer. L'accesso avviene tramite `ServerCallContext` l'argomento passato a ogni metodo gRPC:

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext`non fornisce l'accesso completo a `HttpContext` in tutte le API ASP.NET. Il `GetHttpContext` metodo`HttpContext` di estensione fornisce accesso completo a che rappresenta il messaggio http/2 sottostante nelle API ASP.NET:

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
