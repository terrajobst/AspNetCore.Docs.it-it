---
title: Registrazione e diagnostica in gRPC in .NET
author: jamesnk
description: Informazioni su come raccogliere dati diagnostici dall'app gRPC in .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: ce6ad96d9e26c9cd3844093536745f8f9bea4a76
ms.sourcegitcommit: 0365af91518004c4a44a30dc3a8ac324558a399b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204345"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a><span data-ttu-id="6a80b-103">Registrazione e diagnostica in gRPC in .NET</span><span class="sxs-lookup"><span data-stu-id="6a80b-103">Logging and diagnostics in gRPC on .NET</span></span>

<span data-ttu-id="6a80b-104">Di [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="6a80b-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="6a80b-105">Questo articolo fornisce indicazioni per la raccolta di dati diagnostici dall'app gRPC per semplificare la risoluzione dei problemi.</span><span class="sxs-lookup"><span data-stu-id="6a80b-105">This article provides guidance for gathering diagnostics from your gRPC app to help troubleshoot issues.</span></span>

## <a name="grpc-services-logging"></a><span data-ttu-id="6a80b-106">registrazione dei servizi gRPC</span><span class="sxs-lookup"><span data-stu-id="6a80b-106">gRPC services logging</span></span>

> [!WARNING]
> <span data-ttu-id="6a80b-107">I log lato server possono contenere informazioni riservate dall'app.</span><span class="sxs-lookup"><span data-stu-id="6a80b-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="6a80b-108">**Non pubblicare mai log non** elaborati dalle app di produzione nei forum pubblici come github.</span><span class="sxs-lookup"><span data-stu-id="6a80b-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="6a80b-109">Poiché i servizi gRPC sono ospitati in ASP.NET Core, viene usato il sistema di registrazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6a80b-109">Since gRPC services are hosted on ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="6a80b-110">Nella configurazione predefinita, gRPC registra pochissime informazioni, ma ciò può essere configurato.</span><span class="sxs-lookup"><span data-stu-id="6a80b-110">In the default configuration, gRPC logs very little information, but this can configured.</span></span> <span data-ttu-id="6a80b-111">Per informazioni dettagliate sulla configurazione della registrazione ASP.NET Core, vedere la documentazione relativa alla [registrazione di ASP.NET Core](xref:fundamentals/logging/index#configuration) .</span><span class="sxs-lookup"><span data-stu-id="6a80b-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="6a80b-112">gRPC aggiunge i `Grpc` log nella categoria.</span><span class="sxs-lookup"><span data-stu-id="6a80b-112">gRPC adds logs under the `Grpc` category.</span></span> <span data-ttu-id="6a80b-113">Per abilitare i log dettagliati da gRPC, `Grpc` configurare i prefissi `Debug` per il livello nel file *appSettings. JSON* aggiungendo gli elementi seguenti alla `LogLevel` sottosezione in: `Logging`</span><span class="sxs-lookup"><span data-stu-id="6a80b-113">To enable detailed logs from gRPC, configure the `Grpc` prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/logging-config.json?highlight=7)]

<span data-ttu-id="6a80b-114">È anche possibile configurarlo in startup.cs `ConfigureLogging`con:</span><span class="sxs-lookup"><span data-stu-id="6a80b-114">You can also configure this in *Startup.cs* with `ConfigureLogging`:</span></span>

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5)]

<span data-ttu-id="6a80b-115">Se non si usa la configurazione basata su JSON, impostare il valore di configurazione seguente nel sistema di configurazione:</span><span class="sxs-lookup"><span data-stu-id="6a80b-115">If you aren't using JSON-based configuration, set the following configuration value in your configuration system:</span></span>

* `Logging:LogLevel:Grpc` = `Debug`

<span data-ttu-id="6a80b-116">Controllare la documentazione del sistema di configurazione per determinare come specificare i valori di configurazione annidati.</span><span class="sxs-lookup"><span data-stu-id="6a80b-116">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="6a80b-117">Quando si usano le `_` `:` variabili di ambiente, ad esempio, vengono usati due caratteri al posto di ( `Logging__LogLevel__Grpc`ad esempio,).</span><span class="sxs-lookup"><span data-stu-id="6a80b-117">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Grpc`).</span></span>

<span data-ttu-id="6a80b-118">È consigliabile usare il `Debug` livello quando si raccolgono dati di diagnostica più dettagliati per l'app.</span><span class="sxs-lookup"><span data-stu-id="6a80b-118">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="6a80b-119">Il `Trace` livello produce diagnostica di basso livello e raramente è necessario per diagnosticare i problemi nell'app.</span><span class="sxs-lookup"><span data-stu-id="6a80b-119">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

### <a name="sample-logging-output"></a><span data-ttu-id="6a80b-120">Esempio di output di registrazione</span><span class="sxs-lookup"><span data-stu-id="6a80b-120">Sample logging output</span></span>

<span data-ttu-id="6a80b-121">Di seguito è riportato un esempio di output della `Debug` console al livello di un servizio gRPC:</span><span class="sxs-lookup"><span data-stu-id="6a80b-121">Here is an example of console output at the `Debug` level of a gRPC service:</span></span>

```
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

## <a name="access-server-side-logs"></a><span data-ttu-id="6a80b-122">Accedere ai log lato server</span><span class="sxs-lookup"><span data-stu-id="6a80b-122">Access server-side logs</span></span>

<span data-ttu-id="6a80b-123">La modalità di accesso ai log lato server dipende dall'ambiente in cui si esegue.</span><span class="sxs-lookup"><span data-stu-id="6a80b-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app"></a><span data-ttu-id="6a80b-124">Come app console</span><span class="sxs-lookup"><span data-stu-id="6a80b-124">As a console app</span></span>

<span data-ttu-id="6a80b-125">Se è in esecuzione un'app console, il [logger della console](xref:fundamentals/logging/index#console-provider) deve essere abilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6a80b-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="6a80b-126">i log gRPC verranno visualizzati nella console di.</span><span class="sxs-lookup"><span data-stu-id="6a80b-126">gRPC logs will appear in the console.</span></span>

### <a name="other-environments"></a><span data-ttu-id="6a80b-127">Altri ambienti</span><span class="sxs-lookup"><span data-stu-id="6a80b-127">Other environments</span></span>

<span data-ttu-id="6a80b-128">Se l'app viene distribuita in un altro ambiente, ad esempio Docker, Kubernetes o il servizio Windows, vedere <xref:fundamentals/logging/index> per altre informazioni su come configurare i provider di registrazione appropriati per l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="6a80b-128">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="grpc-client-logging"></a><span data-ttu-id="6a80b-129">registrazione client gRPC</span><span class="sxs-lookup"><span data-stu-id="6a80b-129">gRPC client logging</span></span>

> [!WARNING]
> <span data-ttu-id="6a80b-130">I log lato client possono contenere informazioni riservate dall'app.</span><span class="sxs-lookup"><span data-stu-id="6a80b-130">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="6a80b-131">**Non pubblicare mai log non** elaborati dalle app di produzione nei forum pubblici come github.</span><span class="sxs-lookup"><span data-stu-id="6a80b-131">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="6a80b-132">Per ottenere i log dal client .NET, è possibile impostare la `GrpcChannelOptions.LoggerFactory` proprietà quando viene creato il canale del client.</span><span class="sxs-lookup"><span data-stu-id="6a80b-132">To get logs from the .NET client, you can set the `GrpcChannelOptions.LoggerFactory` property when the client's channel is created.</span></span> <span data-ttu-id="6a80b-133">Se si chiama un servizio gRPC da un'app ASP.NET Core, la factory del logger può essere risolta dall'inserimento delle dipendenze (DI):</span><span class="sxs-lookup"><span data-stu-id="6a80b-133">If you are calling a gRPC service from an ASP.NET Core app then the logger factory can be resolved from dependency injection (DI):</span></span>

[!code-csharp[](diagnostics/net-client-dependency-injection.cs?highlight=7,16)]

<span data-ttu-id="6a80b-134">Un modo alternativo per abilitare la registrazione client consiste nell'usare la [Factory client gRPC](xref:grpc/clientfactory) per creare il client.</span><span class="sxs-lookup"><span data-stu-id="6a80b-134">An alternative way to enable client logging is to use the [gRPC client factory](xref:grpc/clientfactory) to create the client.</span></span> <span data-ttu-id="6a80b-135">Un client DI gRPC registrato con la factory client ed è stato risolto da DI, userà automaticamente la registrazione configurata dell'app.</span><span class="sxs-lookup"><span data-stu-id="6a80b-135">A gRPC client registered with the client factory and resolved from DI will automatically use the app's configured logging.</span></span>

<span data-ttu-id="6a80b-136">Se l'app non USA di, è possibile creare una nuova `ILoggerFactory` istanza con [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span><span class="sxs-lookup"><span data-stu-id="6a80b-136">If your app isn't using DI then you can create a new `ILoggerFactory` instance with [LoggerFactory.Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span></span> <span data-ttu-id="6a80b-137">Per accedere a questo metodo, aggiungere il pacchetto [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) all'app.</span><span class="sxs-lookup"><span data-stu-id="6a80b-137">To access this method add the [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) package to your app.</span></span>

[!code-csharp[](diagnostics/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a><span data-ttu-id="6a80b-138">Esempio di output di registrazione</span><span class="sxs-lookup"><span data-stu-id="6a80b-138">Sample logging output</span></span>

<span data-ttu-id="6a80b-139">Di seguito è riportato un esempio di output della `Debug` console a livello di client gRPC:</span><span class="sxs-lookup"><span data-stu-id="6a80b-139">Here is an example of console output at the `Debug` level of a gRPC client:</span></span>

```
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="additional-resources"></a><span data-ttu-id="6a80b-140">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6a80b-140">Additional resources</span></span>

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
