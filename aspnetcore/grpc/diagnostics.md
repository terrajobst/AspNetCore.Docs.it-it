---
title: Registrazione e diagnostica in gRPC in .NET
author: jamesnk
description: Informazioni su come raccogliere dati diagnostici dall'app gRPC in .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 7194e91b40a08c4a7ee619b8f207900af2683aa1
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250729"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>Registrazione e diagnostica in gRPC in .NET

Di [James Newton-King](https://twitter.com/jamesnk)

Questo articolo fornisce indicazioni per la raccolta di dati diagnostici dall'app gRPC per semplificare la risoluzione dei problemi.

## <a name="grpc-services-logging"></a>registrazione dei servizi gRPC

> [!WARNING]
> I log lato server possono contenere informazioni riservate dall'app. **Non pubblicare mai log non** elaborati dalle app di produzione nei forum pubblici come github.

Poiché i servizi gRPC sono ospitati in ASP.NET Core, viene usato il sistema di registrazione ASP.NET Core. Nella configurazione predefinita, gRPC registra pochissime informazioni, ma ciò può essere configurato. Per informazioni dettagliate sulla configurazione della registrazione ASP.NET Core, vedere la documentazione relativa alla [registrazione di ASP.NET Core](xref:fundamentals/logging/index#configuration) .

gRPC aggiunge i `Grpc` log nella categoria. Per abilitare i log dettagliati da gRPC, `Grpc` configurare i prefissi `Debug` per il livello nel file *appSettings. JSON* aggiungendo gli elementi seguenti alla `LogLevel` sottosezione in: `Logging`

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

È anche possibile configurarlo in startup.cs `ConfigureLogging`con:

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

Se non si usa la configurazione basata su JSON, impostare il valore di configurazione seguente nel sistema di configurazione:

* `Logging:LogLevel:Grpc` = `Debug`

Controllare la documentazione del sistema di configurazione per determinare come specificare i valori di configurazione annidati. Quando si usano le `_` `:` variabili di ambiente, ad esempio, vengono usati due caratteri al posto di ( `Logging__LogLevel__Grpc`ad esempio,).

È consigliabile usare il `Debug` livello quando si raccolgono dati di diagnostica più dettagliati per l'app. Il `Trace` livello produce diagnostica di basso livello e raramente è necessario per diagnosticare i problemi nell'app.

### <a name="sample-logging-output"></a>Esempio di output di registrazione

Di seguito è riportato un esempio di output della `Debug` console al livello di un servizio gRPC:

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

## <a name="access-server-side-logs"></a>Accedere ai log lato server

La modalità di accesso ai log lato server dipende dall'ambiente in cui si esegue.

### <a name="as-a-console-app"></a>Come app console

Se è in esecuzione un'app console, il [logger della console](xref:fundamentals/logging/index#console-provider) deve essere abilitato per impostazione predefinita. i log gRPC verranno visualizzati nella console di.

### <a name="other-environments"></a>Altri ambienti

Se l'app viene distribuita in un altro ambiente, ad esempio Docker, Kubernetes o il servizio Windows, vedere <xref:fundamentals/logging/index> per altre informazioni su come configurare i provider di registrazione appropriati per l'ambiente.

## <a name="grpc-client-logging"></a>registrazione client gRPC

> [!WARNING]
> I log lato client possono contenere informazioni riservate dall'app. **Non pubblicare mai log non** elaborati dalle app di produzione nei forum pubblici come github.

Per ottenere i log dal client .NET, è possibile impostare la `GrpcChannelOptions.LoggerFactory` proprietà quando viene creato il canale del client. Se si chiama un servizio gRPC da un'app ASP.NET Core, la factory del logger può essere risolta dall'inserimento delle dipendenze (DI):

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

Un modo alternativo per abilitare la registrazione client consiste nell'usare la [Factory client gRPC](xref:grpc/clientfactory) per creare il client. Un client DI gRPC registrato con la factory client ed è stato risolto da DI, userà automaticamente la registrazione configurata dell'app.

Se l'app non USA di, è possibile creare una nuova `ILoggerFactory` istanza con [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*). Per accedere a questo metodo, aggiungere il pacchetto [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) all'app.

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a>Esempio di output di registrazione

Di seguito è riportato un esempio di output della `Debug` console a livello di client gRPC:

```console
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
