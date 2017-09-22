---
title: Registrazione di ASP.NET Core
author: ardalis
description: "Introduce il framework di registrazione di ASP.NET Core. Include una sezione per ogni provider di registrazione predefiniti e collegamenti ad alcuni provider di terze parti più diffusi."
keywords: ASP.NET Core, registrazione, la registrazione providers,Microsoft.Extensions.Logging,ILogger,ILoggerFactory,LogLevel,WithFilter,TraceSource,EventLog,EventSource,scopes
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ca81f01fe1c5026514eafedf852b4bc8f3b6fd21
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="b0c8d-105">Introduzione alla registrazione di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b0c8d-105">Introduction to Logging in ASP.NET Core</span></span>

<span data-ttu-id="b0c8d-106">Da [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b0c8d-106">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="b0c8d-107">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di log.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-107">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="b0c8d-108">I provider predefiniti consentono di inviare i log per una o più destinazioni, ed è possibile collegare un framework di registrazione di terze parti.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-108">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="b0c8d-109">In questo articolo viene illustrato come utilizzare i provider e l'API di registrazione incorporate nel codice.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-109">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0c8d-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[<span data-ttu-id="b0c8d-111">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="b0c8d-111">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0c8d-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[<span data-ttu-id="b0c8d-113">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="b0c8d-113">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)

---

## <a name="how-to-create-logs"></a><span data-ttu-id="b0c8d-114">Come creare i registri</span><span class="sxs-lookup"><span data-stu-id="b0c8d-114">How to create logs</span></span>

<span data-ttu-id="b0c8d-115">Per creare registri, ottenere un `ILogger` dall'oggetto di [inserimento di dipendenze](dependency-injection.md) contenitore:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-115">To create logs, get an `ILogger` object from the [dependency injection](dependency-injection.md) container:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="b0c8d-116">L'oggetto logger quindi chiamare i metodi di registrazione:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-116">Then call logging methods on that logger object:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="b0c8d-117">Questo esempio viene creata con i registri di `TodoController` di classi di *categoria*.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-117">This example creates logs with the `TodoController` class as the *category*.</span></span>  <span data-ttu-id="b0c8d-118">Vengono descritte le categorie [più avanti in questo articolo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-118">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="b0c8d-119">ASP.NET Core non forniscono async logger metodi perché la registrazione deve essere così rapida non è importante il costo dell'utilizzo di async.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-119">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="b0c8d-120">Se si trova in una situazione in cui non è true, provare a modificare la modalità di accesso.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-120">If you're in a situation where that's not true, consider changing the way you log.</span></span>  <span data-ttu-id="b0c8d-121">Se l'archivio dati è lenta, scrivere i messaggi di log a un archivio veloce prima, quindi spostarli in un archivio lenta in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-121">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="b0c8d-122">Ad esempio, accedere a una coda di messaggi che viene letta e resa persistente nell'archiviazione lento da un altro processo.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-122">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="b0c8d-123">Come aggiungere i provider</span><span class="sxs-lookup"><span data-stu-id="b0c8d-123">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0c8d-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b0c8d-125">Un provider di log preleva i messaggi creati con un `ILogger` dell'oggetto e consente di visualizzare o archiviarli.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="b0c8d-126">Ad esempio, il provider di Console visualizza i messaggi nella console e il provider di servizio App di Azure può archiviare nell'archiviazione blob di Azure.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="b0c8d-127">Per utilizzare un provider, chiamare il provider `Add<ProviderName>` il metodo di estensione in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-127">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="b0c8d-128">Il modello di progetto predefinito imposta livello di registrazione il modo in cui è visualizzato nel codice precedente, ma la `ConfigureLogging` chiamata viene eseguita `CreateDefaultBuilder` (metodo).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-128">The default project template sets up logging the way you see it in the preceding code, but the `ConfigureLogging` call is done by the `CreateDefaultBuilder` method.</span></span> <span data-ttu-id="b0c8d-129">Ecco il codice *Program.cs* create da modelli di progetto:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-129">Here's the code in *Program.cs* that is created by project templates:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0c8d-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b0c8d-131">Un provider di log preleva i messaggi creati con un `ILogger` dell'oggetto e consente di visualizzare o archiviarli.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-131">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="b0c8d-132">Ad esempio, il provider di Console visualizza i messaggi nella console e il provider di servizio App di Azure può archiviare nell'archiviazione blob di Azure.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-132">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="b0c8d-133">Per utilizzare un provider, installare il pacchetto NuGet e chiamare il metodo di estensione del provider in un'istanza di `ILoggerFactory`, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-133">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="b0c8d-134">ASP.NET Core [inserimento di dipendenze](dependency-injection.md) (DI) fornisce il `ILoggerFactory` istanza.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-134">ASP.NET Core [dependency injection](dependency-injection.md) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="b0c8d-135">Il `AddConsole` e `AddDebug` metodi di estensione sono definiti nel [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pacchetti.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-135">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="b0c8d-136">Chiama ogni metodo di estensione di `ILoggerFactory.AddProvider` , passando in un'istanza del provider.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-136">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="b0c8d-137">L'applicazione di esempio per l'articolo aggiunge dei provider di registrazione nel `Configure` metodo la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-137">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="b0c8d-138">Se si desidera ottenere l'output di log dal codice eseguito in precedenza, aggiungere il provider di log nel `Startup` invece costruttore di classe.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-138">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="b0c8d-139">Vengono fornite informazioni su ogni [provider di log incorporato](#built-in-logging-providers) e i collegamenti agli [provider di log di terze parti](#third-party-logging-providers) più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-139">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="b0c8d-140">Esempio di output di registrazione</span><span class="sxs-lookup"><span data-stu-id="b0c8d-140">Sample logging output</span></span>

<span data-ttu-id="b0c8d-141">Con il codice di esempio illustrato nella sezione precedente, si noterà registri nella console durante l'esecuzione dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-141">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="b0c8d-142">Di seguito è riportato un esempio di output della console:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-142">Here's an example of console output:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```
 
<span data-ttu-id="b0c8d-143">Questi log sono stati creati, passare a `http://localhost:5000/api/todo/0`, che attiva l'esecuzione di entrambi `ILogger` chiamate illustrate nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-143">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="b0c8d-144">Ecco un esempio degli stessi log come appaiono nella finestra di Debug quando si esegue l'applicazione di esempio in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-144">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="b0c8d-145">I log sono stati creati con la `ILogger` chiamate illustrate nella sezione precedente iniziano con "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="b0c8d-145">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="b0c8d-146">I registri che iniziano con le categorie di "Microsoft" sono da ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-146">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="b0c8d-147">ASP.NET Core stesso e il codice dell'applicazione utilizza la stessa API di registrazione e gli stessi provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-147">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="b0c8d-148">Il resto di questo articolo illustra alcuni dettagli e le opzioni per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-148">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="b0c8d-149">Pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="b0c8d-149">NuGet packages</span></span>

<span data-ttu-id="b0c8d-150">Il `ILogger` e `ILoggerFactory` presenti interfacce [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), e le implementazioni predefinite per essi sono [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-150">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="b0c8d-151">Categoria di log</span><span class="sxs-lookup"><span data-stu-id="b0c8d-151">Log category</span></span>

<span data-ttu-id="b0c8d-152">Oggetto *categoria* è incluso in ogni log creati.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-152">A *category* is included with each log that you create.</span></span>  <span data-ttu-id="b0c8d-153">Specificare la categoria quando si crea un `ILogger` oggetto.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-153">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="b0c8d-154">La categoria può essere qualsiasi stringa, ma una convenzione consiste nell'utilizzare il nome completo della classe da cui vengono scritti i log.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-154">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span>  <span data-ttu-id="b0c8d-155">Ad esempio: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="b0c8d-155">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="b0c8d-156">È possibile specificare la categoria sotto forma di stringa o utilizzare un metodo di estensione da cui deriva il tipo di categoria.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-156">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="b0c8d-157">Per specificare la categoria sotto forma di stringa, chiamare `CreateLogger` su un `ILoggerFactory` dell'istanza, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-157">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="b0c8d-158">La maggior parte dei casi, sarà più facile da utilizzare `ILogger<T>`, come nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-158">Most of the time, it will be easier to use  `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="b0c8d-159">Ciò equivale a chiamare il metodo `CreateLogger` con il nome completo del tipo di `T`.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-159">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="b0c8d-160">Livello di registrazione</span><span class="sxs-lookup"><span data-stu-id="b0c8d-160">Log level</span></span>

<span data-ttu-id="b0c8d-161">Ogni volta che si scrive un log, specificare il relativo [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-161">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="b0c8d-162">Il livello di log indica il livello di gravità o priorità.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-162">The log level indicates the degree of severity or importance.</span></span>  <span data-ttu-id="b0c8d-163">Ad esempio, è possibile scrivere un `Information` log al termine di un metodo in genere, un `Warning` quando un metodo viene restituito un errore 404 codice restituito e un log `Error` log quando viene rilevata un'eccezione imprevista.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-163">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="b0c8d-164">Nell'esempio di codice seguente, i nomi dei metodi (ad esempio, `LogWarning`) specificare il livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-164">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span>  <span data-ttu-id="b0c8d-165">Il primo parametro è il [Log ID evento](#log-event-id) (descritta più avanti in questo articolo).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-165">The first parameter is the [Log event ID](#log-event-id) (explained later in this article).</span></span>  <span data-ttu-id="b0c8d-166">I parametri rimanenti costruire una stringa di messaggio di log.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-166">The remaining parameters construct a log message string.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="b0c8d-167">Metodi di log che includono il livello nel nome del metodo sono [i metodi di estensione ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-167">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="b0c8d-168">Dietro le quinte, chiamano questi metodi un `Log` metodo che accetta un `LogLevel` parametro.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-168">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="b0c8d-169">È possibile chiamare il `Log` metodo direttamente, anziché uno di questi metodi di estensione, ma la sintassi è relativamente complesso.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-169">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="b0c8d-170">Per ulteriori informazioni, vedere il [interfaccia ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) e [codice sorgente di estensioni del logger](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-170">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="b0c8d-171">ASP.NET Core definisce i seguenti [livelli di log](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordinato seguito da almeno a più alto livello di gravità.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-171">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="b0c8d-172">Traccia = 0</span><span class="sxs-lookup"><span data-stu-id="b0c8d-172">Trace = 0</span></span>

  <span data-ttu-id="b0c8d-173">Per informazioni che sono utile solo per uno sviluppatore di debug di un problema.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-173">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="b0c8d-174">Questi messaggi possono contenere dati sensibili dell'applicazione e pertanto non devono essere abilitati in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-174">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="b0c8d-175">*Disattivata per impostazione predefinita.*</span><span class="sxs-lookup"><span data-stu-id="b0c8d-175">*Disabled by default.*</span></span> <span data-ttu-id="b0c8d-176">Esempio: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="b0c8d-176">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="b0c8d-177">Eseguire il debug = 1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-177">Debug = 1</span></span>

  <span data-ttu-id="b0c8d-178">Per informazioni a breve termine utilità durante lo sviluppo e debug.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-178">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="b0c8d-179">Esempio: `Entering method Configure with flag set to true.` è in genere non consentirebbe `Debug` livello vengono registrati nell'ambiente di produzione, a meno che la risoluzione, a causa dell'elevato volume di log.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-179">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="b0c8d-180">Informazioni = 2</span><span class="sxs-lookup"><span data-stu-id="b0c8d-180">Information = 2</span></span>

  <span data-ttu-id="b0c8d-181">Per gestire il flusso generale dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-181">For tracking the general flow of the application.</span></span> <span data-ttu-id="b0c8d-182">Questi log sono in genere un valore a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-182">These logs typically have some long-term value.</span></span> <span data-ttu-id="b0c8d-183">Esempio: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="b0c8d-183">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="b0c8d-184">Avviso = 3</span><span class="sxs-lookup"><span data-stu-id="b0c8d-184">Warning = 3</span></span>

  <span data-ttu-id="b0c8d-185">Per gli eventi imprevisti o anomali nel flusso dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-185">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="b0c8d-186">Potrebbero essere presenti errori o altre condizioni che non provocano l'applicazione di interrompere, ma che potrebbe essere necessario analizzare.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-186">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="b0c8d-187">Eccezioni gestite sono un punto comune per utilizzare il `Warning` livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-187">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="b0c8d-188">Esempio: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="b0c8d-188">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="b0c8d-189">Errore = 4</span><span class="sxs-lookup"><span data-stu-id="b0c8d-189">Error = 4</span></span>

  <span data-ttu-id="b0c8d-190">Per errori ed eccezioni che non possono essere gestiti.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-190">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="b0c8d-191">Questi messaggi indicano un errore nell'operazione (ad esempio la richiesta HTTP corrente) o l'attività corrente, non un errore a livello di applicazione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-191">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="b0c8d-192">Messaggio di log di esempio:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="b0c8d-192">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="b0c8d-193">Critico = 5</span><span class="sxs-lookup"><span data-stu-id="b0c8d-193">Critical = 5</span></span>

  <span data-ttu-id="b0c8d-194">Per gli errori che richiedono attenzione immediata.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-194">For failures that require immediate attention.</span></span> <span data-ttu-id="b0c8d-195">Esempi: scenari di perdita dati, spazio su disco insufficiente.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-195">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="b0c8d-196">È possibile utilizzare il livello di registrazione per controllare la quantità del log output viene scritto un supporto di archiviazione specifico o visualizzare una finestra.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-196">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="b0c8d-197">Ad esempio, nell'ambiente di produzione è consigliabile tutti i log di `Information` livello e inferiore per passare a un archivio dati di volume e tutti i log di `Warning` livello e versioni successive per passare a un archivio dati di valore.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-197">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="b0c8d-198">Durante lo sviluppo, in genere è possibile inviare i log di `Warning` o gravità superiore nella console.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-198">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="b0c8d-199">Quando è necessario risolvere i problemi, è possibile aggiungere `Debug` livello.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-199">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="b0c8d-200">Il [filtri Log](#log-filtering) sezione più avanti in questo articolo viene descritto come controllare quali livelli di registrazione gestisce un provider.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-200">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="b0c8d-201">Scrive il framework di ASP.NET Core `Debug` framework eventi nei registri di livello.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-201">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="b0c8d-202">Gli esempi di log in precedenza in questo articolo esclusi log seguenti `Information` livello, quindi nessun `Debug` venivano visualizzati vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-202">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="b0c8d-203">Di seguito è riportato un esempio di registri della console se si esegue l'applicazione di esempio configurato per mostrare `Debug` e i registri superiore per il provider di console.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-203">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a><span data-ttu-id="b0c8d-204">ID del registro eventi</span><span class="sxs-lookup"><span data-stu-id="b0c8d-204">Log event ID</span></span>

<span data-ttu-id="b0c8d-205">Ogni volta che si scrive un log, specificare un *ID evento*.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-205">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="b0c8d-206">L'app di esempio esegue questa funzione utilizzando un oggetto definito localmente `LoggingEvents` classe:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-206">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="b0c8d-207">ID evento è un valore intero che è possibile utilizzare per associare un set di eventi registrati tra loro.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-207">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="b0c8d-208">Ad esempio, un log per l'aggiunta di un elemento al carrello acquisti potrebbe essere l'ID evento 1000 e un log per il completamento di un fornitore potrebbe essere l'ID evento 1001.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-208">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="b0c8d-209">Nell'output di registrazione, l'ID evento può essere archiviato in un campo o inclusi nel testo del messaggio, a seconda del provider.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-209">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span>  <span data-ttu-id="b0c8d-210">Il provider di Debug non vengono visualizzati gli ID degli eventi, ma il provider di console Mostra le parentesi quadre dopo la categoria:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-210">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a><span data-ttu-id="b0c8d-211">Stringa di formato di messaggio di log</span><span class="sxs-lookup"><span data-stu-id="b0c8d-211">Log message format string</span></span>

<span data-ttu-id="b0c8d-212">Ogni volta che si scrive un log, fornire un messaggio di testo.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-212">Each time you write a log, you provide a text message.</span></span> <span data-ttu-id="b0c8d-213">La stringa di messaggio può contenere segnaposto denominati in quale argomento vengono inseriti i valori, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-213">The message string can contain named placeholders into which argument values are placed, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="b0c8d-214">L'ordine dei segnaposto, non i relativi nomi determina quali parametri vengono utilizzati per essi.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-214">The order of placeholders, not their names, determines which parameters are used for them.</span></span> <span data-ttu-id="b0c8d-215">Si supponga, ad esempio, di avere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-215">For example, if you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="b0c8d-216">Il messaggio di log risultante sarebbe simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-216">The resulting log message would look like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="b0c8d-217">Il framework di registrazione dei messaggi di formattazione in questo modo per rendere possibile per i provider di registrazione implementare [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="b0c8d-218">Poiché gli argomenti stessi vengono passati al sistema di registrazione, non solo la stringa di messaggio formattato, i provider di registrazione è possono archiviare i valori dei parametri come campi oltre la stringa di messaggio.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-218">Because the arguments themselves are passed to the logging system, not just the formatted message string, logging providers can store the parameter values as fields in addition to the message string.</span></span> <span data-ttu-id="b0c8d-219">Ad esempio, se indirizza il log di output per l'archiviazione tabelle di Azure e la chiamata al metodo logger è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-219">For example, if you are directing your log output to Azure Table Storage, and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="b0c8d-220">Ogni entità di tabelle di Azure potrebbe avere `ID` e `RequestTime` proprietà, che dovrebbe semplificare le query sui dati di log.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-220">Each Azure Table entity could have `ID` and `RequestTime` properties, which would simplify queries on log data.</span></span> <span data-ttu-id="b0c8d-221">È possibile trovare tutti i log all'interno di un particolare `RequestTime` intervallo, senza la necessità di analizzare il valore di timeout del messaggio di testo.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-221">You could find all logs within a particular `RequestTime` range, without having to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="b0c8d-222">Registrazione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="b0c8d-222">Logging exceptions</span></span>

<span data-ttu-id="b0c8d-223">I metodi di logger dispongono di overload che consentono di passare un'eccezione, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="b0c8d-224">Provider diversi gestire le informazioni sull'eccezione in modi diversi.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="b0c8d-225">Di seguito è riportato un esempio di output di Debug di provider di codice sopra riportato.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="b0c8d-226">Il filtraggio di log</span><span class="sxs-lookup"><span data-stu-id="b0c8d-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0c8d-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b0c8d-228">È possibile specificare un livello di log minimo per un determinato provider e la categoria o per tutti i provider o tutte le categorie.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span>  <span data-ttu-id="b0c8d-229">Tutti i log sotto il livello minimo non sono passati a tale provider, in modo non ottenere visualizzati o archiviati.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="b0c8d-230">Se si desidera eliminare tutti i log, è possibile specificare `LogLevel.None` come il livello di registrazione minima.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="b0c8d-231">Il valore integer `LogLevel.None` è 6, ovvero superiore `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="b0c8d-232">**Creare regole di filtro nella configurazione**</span><span class="sxs-lookup"><span data-stu-id="b0c8d-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="b0c8d-233">I modelli di progetto creano codice che chiama `CreateDefaultBuilder` per impostare la registrazione per i provider Console ed eseguire il Debug.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="b0c8d-234">Il `CreateDefaultBuilder` metodo imposta inoltre il livello di registrazione per la ricerca per la configurazione di un `Logging` sezione utilizzando codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="b0c8d-235">I dati di configurazione specificano i livelli di log minimo dal provider e categoria, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](logging/sample2/appsettings.json)]

<span data-ttu-id="b0c8d-236">Questo codice JSON crea sei regole di filtro, uno per il provider di Debug, quattro per il provider della Console e quello che si applica a tutti i provider.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="b0c8d-237">Verrà visualizzato in un secondo momento la modalità di una di queste regole viene scelto per ogni provider quando un `ILogger` viene creato l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="b0c8d-238">**Regole di filtro nel codice**</span><span class="sxs-lookup"><span data-stu-id="b0c8d-238">**Filter rules in code**</span></span>

<span data-ttu-id="b0c8d-239">È possibile registrare le regole di filtro nel codice, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="b0c8d-240">Il secondo `AddFilter` specifica il provider di Debug tramite il nome del tipo.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="b0c8d-241">Il primo `AddFilter` si applica a tutti i provider di quanto non consente di specificare un tipo di provider.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="b0c8d-242">**Funzionamento delle regole di filtro vengono applicati.**</span><span class="sxs-lookup"><span data-stu-id="b0c8d-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="b0c8d-243">I dati di configurazione e `AddFilter` codice illustrato negli esempi precedenti creare le regole indicate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="b0c8d-244">I primi sei provengono da esempio di configurazione e gli ultimi due provengono dall'esempio di codice.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-244">The first six come from the configuration example and the last two come from the code example.</span></span>

<span data-ttu-id="b0c8d-245">Number</span><span class="sxs-lookup"><span data-stu-id="b0c8d-245">Number</span></span>|<span data-ttu-id="b0c8d-246">Provider</span><span class="sxs-lookup"><span data-stu-id="b0c8d-246">Provider</span></span>|<span data-ttu-id="b0c8d-247">Categorie che iniziano con</span><span class="sxs-lookup"><span data-stu-id="b0c8d-247">Categories that begin with</span></span>|<span data-ttu-id="b0c8d-248">Livello di registrazione minima</span><span class="sxs-lookup"><span data-stu-id="b0c8d-248">Minimum log level</span></span>|
------|--------|--------------------------|-----------------|
<span data-ttu-id="b0c8d-249">1</span><span class="sxs-lookup"><span data-stu-id="b0c8d-249">1</span></span>|<span data-ttu-id="b0c8d-250">Debug</span><span class="sxs-lookup"><span data-stu-id="b0c8d-250">Debug</span></span>|<span data-ttu-id="b0c8d-251">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="b0c8d-251">All categories</span></span>|<span data-ttu-id="b0c8d-252">Informazioni</span><span class="sxs-lookup"><span data-stu-id="b0c8d-252">Information</span></span>|
<span data-ttu-id="b0c8d-253">2</span><span class="sxs-lookup"><span data-stu-id="b0c8d-253">2</span></span>|<span data-ttu-id="b0c8d-254">Console</span><span class="sxs-lookup"><span data-stu-id="b0c8d-254">Console</span></span>|<span data-ttu-id="b0c8d-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="b0c8d-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span>|<span data-ttu-id="b0c8d-256">Avviso</span><span class="sxs-lookup"><span data-stu-id="b0c8d-256">Warning</span></span>|
<span data-ttu-id="b0c8d-257">3</span><span class="sxs-lookup"><span data-stu-id="b0c8d-257">3</span></span>|<span data-ttu-id="b0c8d-258">Console</span><span class="sxs-lookup"><span data-stu-id="b0c8d-258">Console</span></span>|<span data-ttu-id="b0c8d-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="b0c8d-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>|<span data-ttu-id="b0c8d-260">Debug</span><span class="sxs-lookup"><span data-stu-id="b0c8d-260">Debug</span></span>|
<span data-ttu-id="b0c8d-261">4</span><span class="sxs-lookup"><span data-stu-id="b0c8d-261">4</span></span>|<span data-ttu-id="b0c8d-262">Console</span><span class="sxs-lookup"><span data-stu-id="b0c8d-262">Console</span></span>|<span data-ttu-id="b0c8d-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="b0c8d-263">Microsoft.AspNetCore.Mvc.Razor</span></span>|<span data-ttu-id="b0c8d-264">Errore</span><span class="sxs-lookup"><span data-stu-id="b0c8d-264">Error</span></span>|
<span data-ttu-id="b0c8d-265">5</span><span class="sxs-lookup"><span data-stu-id="b0c8d-265">5</span></span>|<span data-ttu-id="b0c8d-266">Console</span><span class="sxs-lookup"><span data-stu-id="b0c8d-266">Console</span></span>|<span data-ttu-id="b0c8d-267">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="b0c8d-267">All categories</span></span>|<span data-ttu-id="b0c8d-268">Informazioni</span><span class="sxs-lookup"><span data-stu-id="b0c8d-268">Information</span></span>|
<span data-ttu-id="b0c8d-269">6</span><span class="sxs-lookup"><span data-stu-id="b0c8d-269">6</span></span>|<span data-ttu-id="b0c8d-270">Tutti i provider</span><span class="sxs-lookup"><span data-stu-id="b0c8d-270">All providers</span></span>|<span data-ttu-id="b0c8d-271">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="b0c8d-271">All categories</span></span>|<span data-ttu-id="b0c8d-272">Debug</span><span class="sxs-lookup"><span data-stu-id="b0c8d-272">Debug</span></span>
<span data-ttu-id="b0c8d-273">7</span><span class="sxs-lookup"><span data-stu-id="b0c8d-273">7</span></span>|<span data-ttu-id="b0c8d-274">Tutti i provider</span><span class="sxs-lookup"><span data-stu-id="b0c8d-274">All providers</span></span>|<span data-ttu-id="b0c8d-275">System</span><span class="sxs-lookup"><span data-stu-id="b0c8d-275">System</span></span>|<span data-ttu-id="b0c8d-276">Debug</span><span class="sxs-lookup"><span data-stu-id="b0c8d-276">Debug</span></span>
<span data-ttu-id="b0c8d-277">8</span><span class="sxs-lookup"><span data-stu-id="b0c8d-277">8</span></span>|<span data-ttu-id="b0c8d-278">Debug</span><span class="sxs-lookup"><span data-stu-id="b0c8d-278">Debug</span></span>|<span data-ttu-id="b0c8d-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="b0c8d-279">Microsoft</span></span>|<span data-ttu-id="b0c8d-280">Traccia</span><span class="sxs-lookup"><span data-stu-id="b0c8d-280">Trace</span></span>

<span data-ttu-id="b0c8d-281">Quando si crea un `ILogger` oggetto da scrivere i log, con la `ILoggerFactory` oggetto seleziona una singola regola per ogni provider da applicare al logger.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="b0c8d-282">Tutti i messaggi scritti da tale `ILogger` oggetto sono filtrati in base alle regole selezionate.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="b0c8d-283">La regola più specifica possibile per ogni coppia di categoria e il provider viene selezionata dalle regole disponibili.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="b0c8d-284">Viene utilizzato l'algoritmo seguente per ogni provider quando un `ILogger` viene creato per una determinata categoria:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="b0c8d-285">Selezionare tutte le regole che soddisfano il provider o il relativo alias.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-285">Select all rules that match the provider or its alias.</span></span>  <span data-ttu-id="b0c8d-286">Se non vengono rilevati, selezionare tutte le regole con un provider vuoto.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="b0c8d-287">Dal risultato del passaggio precedente, selezionare le regole con più lunga corrispondente prefisso di categoria.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="b0c8d-288">Se non vengono rilevati, selezionare tutte le regole che non specifica una categoria.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="b0c8d-289">Se sono selezionate più regole di richiedere la **ultimo** uno.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="b0c8d-290">Se è selezionata alcuna regola, utilizzare `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-290">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="b0c8d-291">Ad esempio, si supponga di avere l'elenco di regole precedente e si crea un `ILogger` oggetto per la categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="b0c8d-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="b0c8d-292">Per il provider di Debug, si applicano le regole 1, 6 e 8.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="b0c8d-293">Regola 8 è più specifico, in modo che sia quello selezionato.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="b0c8d-294">Per il provider di Console, si applicano le regole 3, 4, 5 e 6.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="b0c8d-295">La regola 3 è più specifica.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="b0c8d-296">Quando si creano log con un `ILogger` per categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", registri di `Trace` livello e sopra verrà inviata al provider di Debug e i registri di `Debug` livello e versioni successive, verrà inviata al provider di Console.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="b0c8d-297">**Alias di provider**</span><span class="sxs-lookup"><span data-stu-id="b0c8d-297">**Provider aliases**</span></span>

<span data-ttu-id="b0c8d-298">È possibile utilizzare il nome del tipo per specificare un provider di configurazione, ma ogni provider definisce un più breve *alias* che è più facile da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="b0c8d-299">Per i provider predefiniti, utilizzare l'alias seguente:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="b0c8d-300">Console</span><span class="sxs-lookup"><span data-stu-id="b0c8d-300">Console</span></span>
- <span data-ttu-id="b0c8d-301">Debug</span><span class="sxs-lookup"><span data-stu-id="b0c8d-301">Debug</span></span>
- <span data-ttu-id="b0c8d-302">Registro eventi</span><span class="sxs-lookup"><span data-stu-id="b0c8d-302">EventLog</span></span>
- <span data-ttu-id="b0c8d-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="b0c8d-303">AzureAppServices</span></span>
- <span data-ttu-id="b0c8d-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="b0c8d-304">TraceSource</span></span>
- <span data-ttu-id="b0c8d-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="b0c8d-305">EventSource</span></span>

<span data-ttu-id="b0c8d-306">**Livello minimo predefinito**</span><span class="sxs-lookup"><span data-stu-id="b0c8d-306">**Default minimum level**</span></span>

<span data-ttu-id="b0c8d-307">È un'impostazione del livello minima che diventa effettivo solo se si applica alcuna regola di configurazione o codice per un determinato provider e la categoria.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-307">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="b0c8d-308">Nell'esempio seguente viene illustrato come impostare il livello minimo:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="b0c8d-309">Se si non imposta in modo esplicito il livello minimo, il valore predefinito è `Information`, vale a dire che `Trace` e `Debug` registri vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-309">IF you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="b0c8d-310">**Funzioni di filtro**</span><span class="sxs-lookup"><span data-stu-id="b0c8d-310">**Filter functions**</span></span>

<span data-ttu-id="b0c8d-311">È possibile scrivere codice in una funzione di filtro da applicare le regole di filtro.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="b0c8d-312">Una funzione di filtro viene richiamata per tutti i provider e le categorie che non dispongono di regole assegnate tramite configurazione o codice.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-312">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="b0c8d-313">Codice della funzione dispone di accesso per il tipo di provider, categoria e livello di registrazione per stabilire o meno un messaggio deve essere registrato.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="b0c8d-314">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-314">For example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0c8d-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b0c8d-316">Alcuni provider di registrazione consente di specificare quando devono essere scritti in un supporto di archiviazione oppure ignorati registri in base a categoria e livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="b0c8d-317">Il `AddConsole` e `AddDebug` metodi di estensione forniscono gli overload che consentono di passare in Criteri di filtro.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="b0c8d-318">Esempio di codice seguente fa sì che il provider di console ignorare i log seguenti `Warning` livello, mentre il provider di Debug ignora i registri che il framework crea.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="b0c8d-319">Il `AddEventLog` metodo presenta un overload che accetta un `EventLogSettings` istanza, che può contenere una funzione di filtro relative `Filter` proprietà.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="b0c8d-320">Il provider TraceSource non è disponibile alcuna di tali overload, poiché il relativo livello di registrazione e gli altri parametri sono basati sul `SourceSwitch` e `TraceListener` Usa.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-320">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the  `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="b0c8d-321">È possibile impostare le regole di filtro per tutti i provider registrati con un `ILoggerFactory` istanza utilizzando il `WithFilter` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="b0c8d-322">Nell'esempio seguente consente di limitare framework accede (categoria inizia con "Microsoft" o "System") per gli avvisi accettandone nel registro applicazioni a livello di debug.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="b0c8d-323">Se si desidera utilizzare i filtri per impedire che tutti i log scritti per una determinata categoria, è possibile specificare `LogLevel.None` come il livello di log minimo per tale categoria.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="b0c8d-324">Il valore integer `LogLevel.None` è 6, ovvero superiore `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="b0c8d-325">Il `WithFilter` viene fornito il metodo di estensione per il [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="b0c8d-326">Il metodo restituisce un nuovo `ILoggerFactory` istanza che consenta di filtrare i messaggi del registro passati a tutti i provider di logger registrati con esso.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="b0c8d-327">Non si applica agli altri `ILoggerFactory` istanze, tra cui originale `ILoggerFactory` istanza.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-327">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="b0c8d-328">Ambiti di log</span><span class="sxs-lookup"><span data-stu-id="b0c8d-328">Log scopes</span></span>

<span data-ttu-id="b0c8d-329">È possibile raggruppare un set di operazioni logiche all'interno di un *ambito* per collegare gli stessi dati per ogni log da cui viene creato come parte di tale set.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span>  <span data-ttu-id="b0c8d-330">Ad esempio, è possibile ogni log creato come parte dell'elaborazione di una transazione per includere l'ID transazione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="b0c8d-331">Un ambito è un `IDisposable` tipo restituito dal `ILogger.BeginScope<TState>` metodo e viene mantenuta fino a quando non viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-331">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="b0c8d-332">Utilizzare un ambito mediante il wrapping del logger chiama in un `using` blocco, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="b0c8d-333">Il codice seguente consente gli ambiti per il provider di console:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0c8d-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b0c8d-335">In *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-335">In *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0c8d-336">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-336">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b0c8d-337">In *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-337">In *Startup.cs*:</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="b0c8d-338">Ogni messaggio del log include le informazioni con ambite:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-338">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="b0c8d-339">Provider di registrazione predefiniti</span><span class="sxs-lookup"><span data-stu-id="b0c8d-339">Built-in logging providers</span></span>

<span data-ttu-id="b0c8d-340">ASP.NET Core sono disponibili i provider seguenti:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-340">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="b0c8d-341">Console</span><span class="sxs-lookup"><span data-stu-id="b0c8d-341">Console</span></span>](#console)
* [<span data-ttu-id="b0c8d-342">Debug</span><span class="sxs-lookup"><span data-stu-id="b0c8d-342">Debug</span></span>](#debug)
* [<span data-ttu-id="b0c8d-343">EventSource</span><span class="sxs-lookup"><span data-stu-id="b0c8d-343">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="b0c8d-344">EventLog</span><span class="sxs-lookup"><span data-stu-id="b0c8d-344">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="b0c8d-345">TraceSource</span><span class="sxs-lookup"><span data-stu-id="b0c8d-345">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="b0c8d-346">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="b0c8d-346">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="b0c8d-347">Il provider di console</span><span class="sxs-lookup"><span data-stu-id="b0c8d-347">The console provider</span></span>

<span data-ttu-id="b0c8d-348">Il [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package provider invia l'output di log nella console.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-348">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0c8d-349">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0c8d-350">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-350">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="b0c8d-351">[AddConsole overload](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) consentono passare un valore booleano che indica se gli ambiti sono supportati, una funzione di filtro e un livello di log minimo.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-351">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span>  <span data-ttu-id="b0c8d-352">Un'altra opzione consiste nel passare un `IConfiguration` oggetto, che è possibile specificare livelli di registrazione e il supporto di ambiti.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-352">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="b0c8d-353">Se si intende il provider di console per l'utilizzo nell'ambiente di produzione, tenere presente che ha un impatto significativo sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-353">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="b0c8d-354">Quando si crea un nuovo progetto in Visual Studio, il `AddConsole` metodo è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-354">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="b0c8d-355">Questo codice si intende il `Logging` sezione il *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-355">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](logging/sample//appsettings.json)]

<span data-ttu-id="b0c8d-356">Le impostazioni visualizzate limite framework accede agli avvisi, consentendo l'app per accedere a livello di debug, come illustrato nel [filtri Log](#log-filtering) sezione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-356">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="b0c8d-357">Per altre informazioni, vedere [Configurazione](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-357">For more information, see [Configuration](configuration.md).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="b0c8d-358">Il provider di Debug</span><span class="sxs-lookup"><span data-stu-id="b0c8d-358">The Debug provider</span></span>

<span data-ttu-id="b0c8d-359">Il [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package provider scrive l'output del log usando il [debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) classe (`Debug.WriteLine` chiamate).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-359">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="b0c8d-360">In Linux, il provider scrive i log per */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-360">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0c8d-361">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-361">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0c8d-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="b0c8d-363">[Gli overload AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) consentono passare un livello di registrazione minima o una funzione di filtro.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-363">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="b0c8d-364">Il provider di EventSource</span><span class="sxs-lookup"><span data-stu-id="b0c8d-364">The EventSource provider</span></span>

<span data-ttu-id="b0c8d-365">Per le app destinate a ASP.NET Core 1.1.0 o versione successiva, il [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) package provider può implementare la traccia eventi.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-365">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="b0c8d-366">In Windows, viene utilizzato [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-366">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="b0c8d-367">Il provider è multipiattaforma, ma sono non disponibili gli strumenti di raccolta e la visualizzazione ancora per Linux o Mac OS alcun evento.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-367">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0c8d-368">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-368">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0c8d-369">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-369">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="b0c8d-370">Un buon metodo per raccogliere e visualizzare i log consiste nell'utilizzare il [PerfView utilità](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-370">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="b0c8d-371">Sono disponibili altri strumenti per la visualizzazione dei log ETW, ma PerfView fornisce un'esperienza ottimale per l'utilizzo con gli eventi ETW generati da ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-371">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="b0c8d-372">Per configurare PerfView per raccogliere gli eventi registrati da questo provider, aggiungere la stringa `*Microsoft-Extensions-Logging` per il **altri provider** elenco.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-372">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="b0c8d-373">(Non perdete l'asterisco all'inizio della stringa).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-373">(Don't miss the asterisk at the start of the string.)</span></span>

![Altri provider di Perfview](logging/_static/perfview-additional-providers.png)

<span data-ttu-id="b0c8d-375">Acquisizione di eventi in Nano Server richiede operazioni di configurazione aggiuntive:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-375">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="b0c8d-376">Connettersi Nano Server di comunicazione remota di PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-376">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="b0c8d-377">Creare una sessione ETW:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-377">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="b0c8d-378">Aggiungere i provider ETW per [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core e altri in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-378">Add ETW providers for [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="b0c8d-379">Il provider ASP.NET Core GUID è `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-379">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="b0c8d-380">Esecuzione del sito e tutte le azioni che si desidera che le informazioni relative.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-380">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="b0c8d-381">Arrestare la sessione di traccia è terminato:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-381">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="b0c8d-382">Il valore risultante *C:\trace.etl* file può essere analizzati con PerfView come in altre edizioni di Windows.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-382">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="b0c8d-383">Il provider del registro eventi di Windows</span><span class="sxs-lookup"><span data-stu-id="b0c8d-383">The Windows EventLog provider</span></span>

<span data-ttu-id="b0c8d-384">Il [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) package provider invia l'output di log nel registro eventi di Windows.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-384">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0c8d-385">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-385">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0c8d-386">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-386">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="b0c8d-387">[Overload di metodo AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) consentono di passare in `EventLogSettings` o un livello di log minimo.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-387">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="b0c8d-388">Il provider TraceSource</span><span class="sxs-lookup"><span data-stu-id="b0c8d-388">The TraceSource provider</span></span>

<span data-ttu-id="b0c8d-389">Il [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider pacchetto utilizza il [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) librerie e i provider.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0c8d-390">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-390">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0c8d-391">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-391">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="b0c8d-392">[Gli overload AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) consentono di passare un'opzione di origine e un listener di traccia.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-392">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="b0c8d-393">Per utilizzare questo provider, un'applicazione deve essere eseguito su di .NET Framework (anziché .NET Core).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-393">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="b0c8d-394">I provider consente di instradare i messaggi a una varietà di [listener](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), ad esempio il [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) utilizzato nell'applicazione di esempio.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-394">The provider lets you route messages to a variety of [listeners](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="b0c8d-395">Nell'esempio seguente viene configurata una `TraceSource` provider che registra `Warning` e messaggi superiore alla finestra della console.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-395">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="b0c8d-396">Il provider di servizio App di Azure</span><span class="sxs-lookup"><span data-stu-id="b0c8d-396">The Azure App Service provider</span></span>

<span data-ttu-id="b0c8d-397">Il [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) package provider scrive i log in file di testo nel file system di un'applicazione servizio App di Azure e a [nell'archiviazione blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in un account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-397">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="b0c8d-398">Il provider è disponibile solo per le app destinate a ASP.NET Core 1.1.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-398">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0c8d-399">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-399">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

> [!NOTE]
> <span data-ttu-id="b0c8d-400">Componenti di base di ASP.NET 2.0 è disponibile in anteprima.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-400">ASP.NET Core 2.0 is in preview.</span></span>  <span data-ttu-id="b0c8d-401">Le app create con la versione di anteprima più recente potrebbero non essere eseguita quando distribuito in Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-401">Apps created with the latest preview release may not run when deployed to Azure App Service.</span></span> <span data-ttu-id="b0c8d-402">Quando viene rilasciata Core ASP.NET 2.0, Azure App Service eseguirà 2.0 App e il servizio App di Azure provider funzionerà come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-402">When ASP.NET Core 2.0 is released, Azure App Service will run 2.0 apps, and the Azure App Service provider will work as indicated here.</span></span>

<span data-ttu-id="b0c8d-403">Non è necessario installare il pacchetto del provider o una chiamata di `AddAzureWebAppDiagnostics` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-403">You don't have to install the provider package or call the `AddAzureWebAppDiagnostics` extension method.</span></span>  <span data-ttu-id="b0c8d-404">Il provider è automaticamente disponibile per l'app quando si distribuisce l'applicazione di servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-404">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0c8d-405">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0c8d-405">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="b0c8d-406">Un `AddAzureWebAppDiagnostics` overload consente di passare in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), con cui è possibile eseguire l'override delle impostazioni predefinite, ad esempio il modello di output di registrazione, il nome di blob e il limite di dimensioni di file.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-406">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="b0c8d-407">(*Modello output* è una stringa di formato di messaggio che viene applicata a tutti i registri, nella parte superiore a quella specificata dall'utente quando si chiama un `ILogger` metodo.)</span><span class="sxs-lookup"><span data-stu-id="b0c8d-407">(*Output template* is a message format string that is applied to all logs, on top of the one that you provide when you call an `ILogger` method.)</span></span>  

---

<span data-ttu-id="b0c8d-408">Quando si distribuisce un'applicazione di servizio App, l'applicazione rispetta le impostazioni di [i log di diagnostica](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) sezione del **servizio App** pagina del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-408">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="b0c8d-409">Quando si modificano queste impostazioni, le modifiche avranno effetto immediatamente senza la necessità di riavviare l'app oppure ridistribuire codice.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-409">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Impostazioni di registrazione di Azure](logging/_static/azure-logging-settings.png)

<span data-ttu-id="b0c8d-411">Il percorso predefinito per i file di log è il *unità d:\\home\\LogFiles\\applicazione* cartella e il nome file predefinito è *diagnostica yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-411">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="b0c8d-412">Il limite di dimensioni di file predefinito è 10 MB e il numero massimo predefinito dei file mantenuti è 2.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-412">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="b0c8d-413">Il nome di blob predefinito è *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-413">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="b0c8d-414">Per ulteriori informazioni sul comportamento predefinito, vedere [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-414">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="b0c8d-415">Il provider funziona solo quando il progetto viene eseguito nell'ambiente Azure.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-415">The provider only works when your project runs in the Azure environment.</span></span>  <span data-ttu-id="b0c8d-416">Non ha alcun effetto quando si esegue localmente &mdash; non vengono scritti in file locali o di archiviazione di sviluppo locale per i BLOB.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-416">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="b0c8d-417">Provider di log di terze parti</span><span class="sxs-lookup"><span data-stu-id="b0c8d-417">Third-party logging providers</span></span>

<span data-ttu-id="b0c8d-418">Ecco alcuni Framework di registrazione di terze parti che utilizzano ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b0c8d-418">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="b0c8d-419">[elmah.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -provider per il servizio Elmah.Io</span><span class="sxs-lookup"><span data-stu-id="b0c8d-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="b0c8d-420">[JSNLog](http://jsnlog.com) -registra eccezioni JavaScript e altri eventi lato client nel registro sul lato server.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-420">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="b0c8d-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -provider per il servizio Loggr</span><span class="sxs-lookup"><span data-stu-id="b0c8d-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="b0c8d-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) -provider per la libreria NLog</span><span class="sxs-lookup"><span data-stu-id="b0c8d-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="b0c8d-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) -provider per la libreria Serilog</span><span class="sxs-lookup"><span data-stu-id="b0c8d-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="b0c8d-424">Alcuni framework di terze parti è possibile eseguire [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="b0c8d-424">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="b0c8d-425">Utilizzare un framework di terze parti è simile all'utilizzo di uno dei provider predefiniti: aggiungere un pacchetto NuGet al progetto e chiamare un metodo di estensione su `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-425">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="b0c8d-426">Per ulteriori informazioni, vedere la documentazione del framework ogni.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-426">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="b0c8d-427">È possibile creare i propri provider personalizzati, nonché per supportare i propri requisiti di registrazione o di altri framework di registrazione.</span><span class="sxs-lookup"><span data-stu-id="b0c8d-427">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>
