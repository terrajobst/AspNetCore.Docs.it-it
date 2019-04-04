---
title: Registrazione in ASP.NET Core
author: tdykstra
description: Informazioni sul framework di registrazione di ASP.NET Core. Informazioni sui provider di registrazione predefiniti e sui provider di terze parti più diffusi.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/logging/index
ms.openlocfilehash: c6543ec1f2295c21c6a693ac8bd16ee07ec11381
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327407"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="83f9d-104">Registrazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="83f9d-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="83f9d-105">Di [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="83f9d-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="83f9d-106">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione predefiniti e di terze parti.</span><span class="sxs-lookup"><span data-stu-id="83f9d-106">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="83f9d-107">Questo articolo illustra come usare l'API di registrazione con i provider predefiniti.</span><span class="sxs-lookup"><span data-stu-id="83f9d-107">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="83f9d-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="83f9d-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="83f9d-109">Aggiungere provider</span><span class="sxs-lookup"><span data-stu-id="83f9d-109">Add providers</span></span>

<span data-ttu-id="83f9d-110">Un provider di registrazione visualizza o archivia i log.</span><span class="sxs-lookup"><span data-stu-id="83f9d-110">A logging provider displays or stores logs.</span></span> <span data-ttu-id="83f9d-111">Il provider Console, ad esempio, visualizza i log nella console, mentre il provider Azure Application Insights li archivia in Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="83f9d-111">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="83f9d-112">I log possono essere inviati a più destinazioni aggiungendo più provider.</span><span class="sxs-lookup"><span data-stu-id="83f9d-112">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="83f9d-113">Per aggiungere un provider, chiamare il metodo di estensione `Add{provider name}` del provider in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="83f9d-113">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

<span data-ttu-id="83f9d-114">Il modello di progetto predefinito chiama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, che aggiunge i provider di registrazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="83f9d-114">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="83f9d-115">Console</span><span class="sxs-lookup"><span data-stu-id="83f9d-115">Console</span></span>
* <span data-ttu-id="83f9d-116">Debug</span><span class="sxs-lookup"><span data-stu-id="83f9d-116">Debug</span></span>
* <span data-ttu-id="83f9d-117">EventSource (a partire da ASP.NET Core 2.2)</span><span class="sxs-lookup"><span data-stu-id="83f9d-117">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="83f9d-118">Se si usa `CreateDefaultBuilder`, è possibile sostituire i provider predefiniti con quelli di propria scelta.</span><span class="sxs-lookup"><span data-stu-id="83f9d-118">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="83f9d-119">Chiamare <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> e aggiungere i provider desiderati.</span><span class="sxs-lookup"><span data-stu-id="83f9d-119">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="83f9d-120">Per usare un provider, installare il relativo pacchetto NuGet e chiamare il metodo di estensione del provider in un'istanza di <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span><span class="sxs-lookup"><span data-stu-id="83f9d-120">To use a provider, install its NuGet package and call the provider's extension method on an instance of <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="83f9d-121">L'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) di ASP.NET Core fornisce l'istanza di `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-121">ASP.NET Core [dependency injection (DI)](xref:fundamentals/dependency-injection) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="83f9d-122">I metodi di estensione `AddConsole` e `AddDebug` sono definiti nei pacchetti [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="83f9d-122">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="83f9d-123">Ogni metodo di estensione chiama il metodo `ILoggerFactory.AddProvider` passando un'istanza del provider.</span><span class="sxs-lookup"><span data-stu-id="83f9d-123">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="83f9d-124">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) aggiunge provider di log nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-124">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="83f9d-125">Per ottenere l'output di log dal codice eseguito in precedenza, aggiungere i provider di registrazione nel costruttore della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-125">To obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="83f9d-126">Altre informazioni sui [provider di registrazione predefiniti](#built-in-logging-providers) e sui [provider di registrazione di terze parti](#third-party-logging-providers) vengono fornite più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-126">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="83f9d-127">Creare log</span><span class="sxs-lookup"><span data-stu-id="83f9d-127">Create logs</span></span>

<span data-ttu-id="83f9d-128">Ottenere un oggetto <xref:Microsoft.Extensions.Logging.ILogger%601> dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="83f9d-128">Get an <xref:Microsoft.Extensions.Logging.ILogger%601> object from DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="83f9d-129">Il controller di esempio seguente crea i log `Information` e `Warning`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-129">The following controller example creates `Information` and `Warning` logs.</span></span> <span data-ttu-id="83f9d-130">La *categoria* è `TodoApiSample.Controllers.TodoController` (nome completo della classe `TodoController` nell'app di esempio):</span><span class="sxs-lookup"><span data-stu-id="83f9d-130">The *category* is `TodoApiSample.Controllers.TodoController` (the fully qualified class name of `TodoController` in the sample app):</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="83f9d-131">L'esempio di Razor Pages seguente crea log con `Information` come *livello* e `TodoApiSample.Pages.AboutModel` come *categoria*:</span><span class="sxs-lookup"><span data-stu-id="83f9d-131">The following Razor Pages example creates logs with `Information` as the *level* and `TodoApiSample.Pages.AboutModel` as the *category*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="83f9d-132">L'esempio precedente crea log con `Information` e `Warning` come *livello* e la classe `TodoController` come *categoria*.</span><span class="sxs-lookup"><span data-stu-id="83f9d-132">The preceding example creates logs with `Information` and `Warning` as the *level* and `TodoController` class as the *category*.</span></span> 

::: moniker-end

<span data-ttu-id="83f9d-133">Il *livello* del log indica la gravità dell'evento registrato.</span><span class="sxs-lookup"><span data-stu-id="83f9d-133">The Log *level* indicates the severity of the logged event.</span></span> <span data-ttu-id="83f9d-134">La *categoria* del log è una stringa associata a ogni log.</span><span class="sxs-lookup"><span data-stu-id="83f9d-134">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="83f9d-135">L'istanza di `ILogger<T>` crea log con nome completo di tipo `T` come categoria.</span><span class="sxs-lookup"><span data-stu-id="83f9d-135">The `ILogger<T>` instance creates logs that have the fully qualified name of type `T` as the category.</span></span> <span data-ttu-id="83f9d-136">[Livelli](#log-level) e [categorie](#log-category) vengono illustrati in modo dettagliato più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-136">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="83f9d-137">Creare log nella classe Startup</span><span class="sxs-lookup"><span data-stu-id="83f9d-137">Create logs in Startup</span></span>

<span data-ttu-id="83f9d-138">Per scrivere log nella classe `Startup`, includere un parametro `ILogger` nella firma del costruttore:</span><span class="sxs-lookup"><span data-stu-id="83f9d-138">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a><span data-ttu-id="83f9d-139">Creare log nella classe Program</span><span class="sxs-lookup"><span data-stu-id="83f9d-139">Create logs in Program</span></span>

<span data-ttu-id="83f9d-140">Per scrivere log nella classe `Program`, ottenere un'istanza di `ILogger` dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="83f9d-140">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="83f9d-141">Evitare l'uso di metodi logger asincroni</span><span class="sxs-lookup"><span data-stu-id="83f9d-141">No asynchronous logger methods</span></span>

<span data-ttu-id="83f9d-142">La registrazione deve essere così rapida da non giustificare l'impatto sulle prestazioni del codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="83f9d-142">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="83f9d-143">Se l'archivio dati di registrazione è lento, non scrivere direttamente al suo interno.</span><span class="sxs-lookup"><span data-stu-id="83f9d-143">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="83f9d-144">Scrivere invece i messaggi di log prima in un archivio veloce e quindi spostarli nell'archivio lento in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="83f9d-144">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="83f9d-145">Registrare ad esempio in una coda di messaggi che viene letta e salvata in modo permanente in un archivio lento da un altro processo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-145">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="configuration"></a><span data-ttu-id="83f9d-146">Configurazione</span><span class="sxs-lookup"><span data-stu-id="83f9d-146">Configuration</span></span>

<span data-ttu-id="83f9d-147">La configurazione dei provider di registrazione viene fornita da uno o più provider di configurazione:</span><span class="sxs-lookup"><span data-stu-id="83f9d-147">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="83f9d-148">Formati di file (INI, JSON e XML).</span><span class="sxs-lookup"><span data-stu-id="83f9d-148">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="83f9d-149">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="83f9d-149">Command-line arguments.</span></span>
* <span data-ttu-id="83f9d-150">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="83f9d-150">Environment variables.</span></span>
* <span data-ttu-id="83f9d-151">Oggetti .NET in memoria.</span><span class="sxs-lookup"><span data-stu-id="83f9d-151">In-memory .NET objects.</span></span>
* <span data-ttu-id="83f9d-152">Archiviazione con [Secret Manager](xref:security/app-secrets) senza crittografia.</span><span class="sxs-lookup"><span data-stu-id="83f9d-152">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="83f9d-153">Un archivio utente non crittografato, come [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="83f9d-153">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="83f9d-154">Provider personalizzati (installati o creati).</span><span class="sxs-lookup"><span data-stu-id="83f9d-154">Custom providers (installed or created).</span></span>

<span data-ttu-id="83f9d-155">Ad esempio, la configurazione di registrazione viene comunemente fornita dalla sezione `Logging` del file di impostazioni dell'app.</span><span class="sxs-lookup"><span data-stu-id="83f9d-155">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="83f9d-156">L'esempio seguente mostra il contenuto di un file *appsettings.Development.json* tipico:</span><span class="sxs-lookup"><span data-stu-id="83f9d-156">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

<span data-ttu-id="83f9d-157">La proprietà `Logging` può avere le proprietà `LogLevel` e quella del provider di log (nell'esempio, Console).</span><span class="sxs-lookup"><span data-stu-id="83f9d-157">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="83f9d-158">La proprietà `LogLevel` in `Logging` specifica il [livello](#log-level) minimo per la registrazione per determinate categorie.</span><span class="sxs-lookup"><span data-stu-id="83f9d-158">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="83f9d-159">Nell'esempio, le categorie `System` e `Microsoft` eseguono la registrazione al livello `Information`, mentre tutte le altre la eseguono al livello `Debug`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-159">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="83f9d-160">Altre proprietà in `Logging` specificano i provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="83f9d-160">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="83f9d-161">L'esempio si riferisce al provider Console.</span><span class="sxs-lookup"><span data-stu-id="83f9d-161">The example is for the Console provider.</span></span> <span data-ttu-id="83f9d-162">Se un provider supporta gli [ambiti di log](#log-scopes), `IncludeScopes` indica se tali ambiti sono abilitati.</span><span class="sxs-lookup"><span data-stu-id="83f9d-162">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="83f9d-163">Una proprietà del provider (ad esempio `Console` nell'esempio) può specificare anche una proprietà `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-163">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="83f9d-164">`LogLevel` in un provider specifica i livelli di registrazione per tale provider.</span><span class="sxs-lookup"><span data-stu-id="83f9d-164">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="83f9d-165">Se i livelli sono specificati in `Logging.{providername}.LogLevel`, eseguono l'override di eventuali valori impostati in `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-165">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

<span data-ttu-id="83f9d-166">Le chiavi `LogLevel` rappresentano i nomi dei log.</span><span class="sxs-lookup"><span data-stu-id="83f9d-166">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="83f9d-167">La chiave `Default` si applica ai log non esplicitamente elencati.</span><span class="sxs-lookup"><span data-stu-id="83f9d-167">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="83f9d-168">Il valore rappresenta il [livello del log](#log-level) applicato al log specificato.</span><span class="sxs-lookup"><span data-stu-id="83f9d-168">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="83f9d-169">Per informazioni sull'implementazione dei provider di configurazione, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="83f9d-169">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="83f9d-170">Esempio di output di registrazione</span><span class="sxs-lookup"><span data-stu-id="83f9d-170">Sample logging output</span></span>

<span data-ttu-id="83f9d-171">Con il codice di esempio illustrato nella sezione precedente i log vengono visualizzati nella console quando l'app viene eseguita dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="83f9d-171">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="83f9d-172">Ecco un esempio di output della console:</span><span class="sxs-lookup"><span data-stu-id="83f9d-172">Here's an example of console output:</span></span>

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

<span data-ttu-id="83f9d-173">I log precedenti sono stati generati inviando una richiesta HTTP Get all'app di esempio all'indirizzo `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-173">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="83f9d-174">Ecco un esempio degli stessi log visualizzati nella finestra Debug quando si esegue l'app di esempio in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="83f9d-174">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="83f9d-175">I log che sono stati creati dalle chiamate a `ILogger` illustrate nella sezione precedente iniziano con "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="83f9d-175">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="83f9d-176">I log che iniziano con le categorie "Microsoft" provengono dal codice del framework ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="83f9d-176">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="83f9d-177">ASP.NET Core e il codice dell'applicazione usano la stessa API di registrazione e gli stessi provider.</span><span class="sxs-lookup"><span data-stu-id="83f9d-177">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="83f9d-178">Il resto di questo articolo spiega alcuni dettagli e le opzioni per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="83f9d-178">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="83f9d-179">Pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="83f9d-179">NuGet packages</span></span>

<span data-ttu-id="83f9d-180">Le interfacce `ILogger` e `ILoggerFactory` si trovano in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) e le loro implementazioni predefinite si trovano in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="83f9d-180">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="83f9d-181">Categoria dei log</span><span class="sxs-lookup"><span data-stu-id="83f9d-181">Log category</span></span>

<span data-ttu-id="83f9d-182">Quando viene creato un oggetto `ILogger`, viene specificata la relativa *categoria*.</span><span class="sxs-lookup"><span data-stu-id="83f9d-182">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="83f9d-183">La categoria è inclusa in ogni messaggio di log creato da tale istanza di `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-183">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="83f9d-184">La categoria può essere qualsiasi stringa, ma per convenzione si usa il nome della classe, ad esempio "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="83f9d-184">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="83f9d-185">Usare `ILogger<T>` per ottenere un'istanza di `ILogger` che usa il nome completo di tipo `T` come categoria:</span><span class="sxs-lookup"><span data-stu-id="83f9d-185">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="83f9d-186">Per specificare in modo esplicito la categoria, chiamare `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="83f9d-186">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="83f9d-187">L'uso di `ILogger<T>` equivale a chiamare `CreateLogger` con il nome completo di tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-187">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="83f9d-188">Livello di registrazione</span><span class="sxs-lookup"><span data-stu-id="83f9d-188">Log level</span></span>

<span data-ttu-id="83f9d-189">Ogni log specifica un valore <xref:Microsoft.Extensions.Logging.LogLevel>.</span><span class="sxs-lookup"><span data-stu-id="83f9d-189">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="83f9d-190">Il livello di registrazione indica la gravità o l'importanza.</span><span class="sxs-lookup"><span data-stu-id="83f9d-190">The log level indicates the severity or importance.</span></span> <span data-ttu-id="83f9d-191">È ad esempio possibile scrivere un log `Information` quando un metodo termina normalmente e un log `Warning` quando un metodo restituisce un codice di stato *404 Non trovato*.</span><span class="sxs-lookup"><span data-stu-id="83f9d-191">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="83f9d-192">Il codice seguente crea i log `Information` e `Warning`:</span><span class="sxs-lookup"><span data-stu-id="83f9d-192">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="83f9d-193">Nel codice precedente il primo parametro è l'[ID dell'evento di log](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="83f9d-193">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="83f9d-194">Il secondo parametro è un modello di messaggio con segnaposto per i valori degli argomenti forniti dai parametri dei metodi rimanenti.</span><span class="sxs-lookup"><span data-stu-id="83f9d-194">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="83f9d-195">I parametri dei metodi sono descritti nella [sezione relativa al modello di messaggio](#log-message-template) più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-195">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="83f9d-196">I metodi di registrazione che includono il livello nel nome del metodo (ad esempio `LogInformation` e `LogWarning`) sono [metodi di estensione per ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="83f9d-196">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="83f9d-197">Questi metodi chiamano un metodo `Log` che accetta un parametro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-197">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="83f9d-198">È possibile chiamare il metodo `Log` direttamente anziché chiamare uno di questi metodi di estensione, ma la sintassi è relativamente complessa.</span><span class="sxs-lookup"><span data-stu-id="83f9d-198">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="83f9d-199">Per altre informazioni, vedere <xref:Microsoft.Extensions.Logging.ILogger> e il [codice sorgente delle estensioni del logger](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="83f9d-199">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="83f9d-200">ASP.NET Core definisce i livelli di registrazione seguenti, ordinati dal meno grave al più grave.</span><span class="sxs-lookup"><span data-stu-id="83f9d-200">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="83f9d-201">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="83f9d-201">Trace = 0</span></span>

  <span data-ttu-id="83f9d-202">Per informazioni in genere utili solo per il debug.</span><span class="sxs-lookup"><span data-stu-id="83f9d-202">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="83f9d-203">Questi messaggi possono contenere dati sensibili dell'applicazione e pertanto non devono essere abilitati in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="83f9d-203">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="83f9d-204">*Disattivato per impostazione predefinita.*</span><span class="sxs-lookup"><span data-stu-id="83f9d-204">*Disabled by default.*</span></span>

* <span data-ttu-id="83f9d-205">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="83f9d-205">Debug = 1</span></span>

  <span data-ttu-id="83f9d-206">Per informazioni che possono essere utili per lo sviluppo e il debug.</span><span class="sxs-lookup"><span data-stu-id="83f9d-206">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="83f9d-207">Esempio: `Entering method Configure with flag set to true.` Abilitare i livelli di registrazione `Debug` nell'ambiente di produzione solo in fase di risoluzione dei problemi, per non fare aumentare eccessivamente il volume dei log.</span><span class="sxs-lookup"><span data-stu-id="83f9d-207">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="83f9d-208">Information = 2</span><span class="sxs-lookup"><span data-stu-id="83f9d-208">Information = 2</span></span>

  <span data-ttu-id="83f9d-209">Per tenere traccia del flusso generale dell'app.</span><span class="sxs-lookup"><span data-stu-id="83f9d-209">For tracking the general flow of the app.</span></span> <span data-ttu-id="83f9d-210">Queste registrazioni hanno in genere un valore a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="83f9d-210">These logs typically have some long-term value.</span></span> <span data-ttu-id="83f9d-211">Esempio: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="83f9d-211">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="83f9d-212">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="83f9d-212">Warning = 3</span></span>

  <span data-ttu-id="83f9d-213">Per gli eventi imprevisti o anomali nel flusso dell'app.</span><span class="sxs-lookup"><span data-stu-id="83f9d-213">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="83f9d-214">Tali eventi potrebbero includere errori o altre condizioni che non provocano l'arresto dell'app, ma che potrebbe essere necessario analizzare.</span><span class="sxs-lookup"><span data-stu-id="83f9d-214">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="83f9d-215">Le eccezioni gestite sono una situazione comune in cui usare il livello di registrazione `Warning`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-215">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="83f9d-216">Esempio: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="83f9d-216">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="83f9d-217">Error = 4</span><span class="sxs-lookup"><span data-stu-id="83f9d-217">Error = 4</span></span>

  <span data-ttu-id="83f9d-218">Per errori ed eccezioni che non possono essere gestiti.</span><span class="sxs-lookup"><span data-stu-id="83f9d-218">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="83f9d-219">Questi messaggi indicano un errore nell'operazione o nell'attività in corso (ad esempio la richiesta HTTP corrente), non un errore a livello di app.</span><span class="sxs-lookup"><span data-stu-id="83f9d-219">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="83f9d-220">Messaggio di registrazione di esempio:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="83f9d-220">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="83f9d-221">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="83f9d-221">Critical = 5</span></span>

  <span data-ttu-id="83f9d-222">Per gli errori che richiedono attenzione immediata.</span><span class="sxs-lookup"><span data-stu-id="83f9d-222">For failures that require immediate attention.</span></span> <span data-ttu-id="83f9d-223">Esempi: scenari di perdita di dati, spazio su disco insufficiente.</span><span class="sxs-lookup"><span data-stu-id="83f9d-223">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="83f9d-224">Usare il livello di registrazione per controllare la quantità di output di log scritto in un supporto di archiviazione specifico o in una finestra.</span><span class="sxs-lookup"><span data-stu-id="83f9d-224">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="83f9d-225">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="83f9d-225">For example:</span></span>

* <span data-ttu-id="83f9d-226">Nell'ambiente di produzione, inviare i messaggi con livello da `Trace` a `Information` a un archivio dati per grandi volumi.</span><span class="sxs-lookup"><span data-stu-id="83f9d-226">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="83f9d-227">Inviare i messaggi con livello da `Warning` a `Critical` a un archivio dati di valore.</span><span class="sxs-lookup"><span data-stu-id="83f9d-227">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="83f9d-228">Durante lo sviluppo, inviare i messaggi con livello da `Warning` a `Critical` alla console e aggiungere il livello da `Trace` a `Information` quando si svolgono attività di risoluzione dei problemi.</span><span class="sxs-lookup"><span data-stu-id="83f9d-228">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="83f9d-229">La sezione [Filtro dei log](#log-filtering) più avanti in questo articolo descrive come controllare quali livelli di registrazione gestisce un provider.</span><span class="sxs-lookup"><span data-stu-id="83f9d-229">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="83f9d-230">ASP.NET Core scrive log per gli eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="83f9d-230">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="83f9d-231">Gli esempi di log illustrati in precedenza in questo articolo escludono i log al di sotto del livello `Information`, quindi non vengono creati log di livello `Debug` o `Trace`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-231">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="83f9d-232">Ecco un esempio di log della console prodotti eseguendo l'app di esempio configurata in modo da mostrare i log di livello `Debug`:</span><span class="sxs-lookup"><span data-stu-id="83f9d-232">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="83f9d-233">ID evento di registrazione</span><span class="sxs-lookup"><span data-stu-id="83f9d-233">Log event ID</span></span>

<span data-ttu-id="83f9d-234">Ogni log può specificare un *ID evento*.</span><span class="sxs-lookup"><span data-stu-id="83f9d-234">Each log can specify an *event ID*.</span></span> <span data-ttu-id="83f9d-235">A tale scopo, l'app di esempio usa una classe `LoggingEvents` definita a livello locale:</span><span class="sxs-lookup"><span data-stu-id="83f9d-235">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="83f9d-236">Un ID evento associa un set di eventi.</span><span class="sxs-lookup"><span data-stu-id="83f9d-236">An event ID associates a set of events.</span></span> <span data-ttu-id="83f9d-237">Ad esempio, tutti i log correlati alla visualizzazione di un elenco di elementi in una pagina potrebbero essere indicati con 1001.</span><span class="sxs-lookup"><span data-stu-id="83f9d-237">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="83f9d-238">Il provider di log può archiviare l'ID evento in un campo ID o nel messaggio di registrazione oppure non archiviarlo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-238">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="83f9d-239">Il provider Debug non visualizza gli ID evento.</span><span class="sxs-lookup"><span data-stu-id="83f9d-239">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="83f9d-240">Il provider Console visualizza gli ID evento tra parentesi quadre dopo la categoria:</span><span class="sxs-lookup"><span data-stu-id="83f9d-240">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="83f9d-241">Modello di messaggio di registrazione</span><span class="sxs-lookup"><span data-stu-id="83f9d-241">Log message template</span></span>

<span data-ttu-id="83f9d-242">Ogni log specifica un modello di messaggio.</span><span class="sxs-lookup"><span data-stu-id="83f9d-242">Each log specifies a message template.</span></span> <span data-ttu-id="83f9d-243">Il modello di messaggio può contenere segnaposto per i quali vengono forniti argomenti.</span><span class="sxs-lookup"><span data-stu-id="83f9d-243">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="83f9d-244">Usare nomi per i segnaposto, non numeri.</span><span class="sxs-lookup"><span data-stu-id="83f9d-244">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="83f9d-245">L'ordine dei segnaposto, non il loro nome, determina i parametri da usare per fornire i valori corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="83f9d-245">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="83f9d-246">Nel codice seguente, si noti che i nomi dei parametri non sono in sequenza nel modello di messaggio:</span><span class="sxs-lookup"><span data-stu-id="83f9d-246">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="83f9d-247">Questo codice crea un messaggio di log con i valori dei parametri in sequenza:</span><span class="sxs-lookup"><span data-stu-id="83f9d-247">This code creates a log message with the parameter values in sequence:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="83f9d-248">Il framework di registrazione funziona in questo modo per consentire ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="83f9d-248">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="83f9d-249">Al sistema di registrazione vengono passati gli argomenti e non solo il modello di messaggio formattato.</span><span class="sxs-lookup"><span data-stu-id="83f9d-249">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="83f9d-250">Queste informazioni consentono ai provider di registrazione di archiviare i valori dei parametri come campi.</span><span class="sxs-lookup"><span data-stu-id="83f9d-250">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="83f9d-251">Si supponga, ad esempio, che il metodo logger esegua chiamate simili alla seguente:</span><span class="sxs-lookup"><span data-stu-id="83f9d-251">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="83f9d-252">Se si inviano i log all'archiviazione tabelle di Azure, ogni entità tabella di Azure può avere le proprietà `ID` e `RequestTime`, il che semplifica le query sui dati dei log.</span><span class="sxs-lookup"><span data-stu-id="83f9d-252">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="83f9d-253">Una query può trovare tutti i log entro un determinato intervallo `RequestTime` senza che sia necessario analizzare il messaggio di testo per determinare l'intervallo di tempo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-253">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="83f9d-254">Registrazione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="83f9d-254">Logging exceptions</span></span>

<span data-ttu-id="83f9d-255">I metodi logger dispongono di overload che consentono di passare un'eccezione, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="83f9d-255">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="83f9d-256">Provider diversi gestiscono le informazioni sulle eccezioni in modi diversi.</span><span class="sxs-lookup"><span data-stu-id="83f9d-256">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="83f9d-257">Ecco un esempio di output del provider Debug dal codice sopra riportato.</span><span class="sxs-lookup"><span data-stu-id="83f9d-257">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="83f9d-258">Filtro dei log</span><span class="sxs-lookup"><span data-stu-id="83f9d-258">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="83f9d-259">È possibile specificare un livello di registrazione minimo per un provider e una categoria specifici oppure per tutti i provider o tutte le categorie.</span><span class="sxs-lookup"><span data-stu-id="83f9d-259">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="83f9d-260">Tutti i log sotto il livello minimo non sono passati al provider, quindi non vengono visualizzati o archiviati.</span><span class="sxs-lookup"><span data-stu-id="83f9d-260">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="83f9d-261">Per eliminare tutti i log, specificare `LogLevel.None` come livello di registrazione minimo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-261">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="83f9d-262">Il valore intero di `LogLevel.None` è 6, maggiore di `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="83f9d-262">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="83f9d-263">Creare regole di filtro nella configurazione</span><span class="sxs-lookup"><span data-stu-id="83f9d-263">Create filter rules in configuration</span></span>

<span data-ttu-id="83f9d-264">Il codice del modello di progetto chiama `CreateDefaultBuilder` per configurare la registrazione per i provider Console e Debug.</span><span class="sxs-lookup"><span data-stu-id="83f9d-264">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="83f9d-265">Il metodo `CreateDefaultBuilder` imposta inoltre la registrazione per la ricerca della configurazione di una sezione `Logging` tramite codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="83f9d-265">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

<span data-ttu-id="83f9d-266">I dati di configurazione specificano i livelli di registrazione minimi in base al provider e alla categoria, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="83f9d-266">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="83f9d-267">Questo codice JSON crea sei regole di filtro, una per il provider Debug, quattro per il provider Console e una per tutti i provider.</span><span class="sxs-lookup"><span data-stu-id="83f9d-267">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="83f9d-268">Quando viene creato un oggetto `ILogger`, viene scelta una singola regola per ogni provider.</span><span class="sxs-lookup"><span data-stu-id="83f9d-268">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="83f9d-269">Regole di filtro nel codice</span><span class="sxs-lookup"><span data-stu-id="83f9d-269">Filter rules in code</span></span>

<span data-ttu-id="83f9d-270">L'esempio seguente illustra come registrare le regole di filtro nel codice:</span><span class="sxs-lookup"><span data-stu-id="83f9d-270">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="83f9d-271">Il secondo `AddFilter` specifica il provider Debug tramite il nome del tipo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-271">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="83f9d-272">Il primo `AddFilter` si applica a tutti i provider in quanto non consente di specificare un tipo di provider.</span><span class="sxs-lookup"><span data-stu-id="83f9d-272">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="83f9d-273">Applicazione delle regole di filtro</span><span class="sxs-lookup"><span data-stu-id="83f9d-273">How filtering rules are applied</span></span>

<span data-ttu-id="83f9d-274">I dati di configurazione e il codice `AddFilter` illustrato negli esempi precedenti creano le regole indicate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="83f9d-274">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="83f9d-275">I primi sei provengono dall'esempio di configurazione e gli ultimi due dall'esempio di codice.</span><span class="sxs-lookup"><span data-stu-id="83f9d-275">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="83f9d-276">Number</span><span class="sxs-lookup"><span data-stu-id="83f9d-276">Number</span></span> | <span data-ttu-id="83f9d-277">Provider</span><span class="sxs-lookup"><span data-stu-id="83f9d-277">Provider</span></span>      | <span data-ttu-id="83f9d-278">Categorie che iniziano con...</span><span class="sxs-lookup"><span data-stu-id="83f9d-278">Categories that begin with ...</span></span>          | <span data-ttu-id="83f9d-279">Livello di registrazione minimo</span><span class="sxs-lookup"><span data-stu-id="83f9d-279">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="83f9d-280">1</span><span class="sxs-lookup"><span data-stu-id="83f9d-280">1</span></span>      | <span data-ttu-id="83f9d-281">Debug</span><span class="sxs-lookup"><span data-stu-id="83f9d-281">Debug</span></span>         | <span data-ttu-id="83f9d-282">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="83f9d-282">All categories</span></span>                          | <span data-ttu-id="83f9d-283">Informazioni</span><span class="sxs-lookup"><span data-stu-id="83f9d-283">Information</span></span>       |
| <span data-ttu-id="83f9d-284">2</span><span class="sxs-lookup"><span data-stu-id="83f9d-284">2</span></span>      | <span data-ttu-id="83f9d-285">Console</span><span class="sxs-lookup"><span data-stu-id="83f9d-285">Console</span></span>       | <span data-ttu-id="83f9d-286">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="83f9d-286">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="83f9d-287">Avviso</span><span class="sxs-lookup"><span data-stu-id="83f9d-287">Warning</span></span>           |
| <span data-ttu-id="83f9d-288">3</span><span class="sxs-lookup"><span data-stu-id="83f9d-288">3</span></span>      | <span data-ttu-id="83f9d-289">Console</span><span class="sxs-lookup"><span data-stu-id="83f9d-289">Console</span></span>       | <span data-ttu-id="83f9d-290">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="83f9d-290">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="83f9d-291">Debug</span><span class="sxs-lookup"><span data-stu-id="83f9d-291">Debug</span></span>             |
| <span data-ttu-id="83f9d-292">4</span><span class="sxs-lookup"><span data-stu-id="83f9d-292">4</span></span>      | <span data-ttu-id="83f9d-293">Console</span><span class="sxs-lookup"><span data-stu-id="83f9d-293">Console</span></span>       | <span data-ttu-id="83f9d-294">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="83f9d-294">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="83f9d-295">Error</span><span class="sxs-lookup"><span data-stu-id="83f9d-295">Error</span></span>             |
| <span data-ttu-id="83f9d-296">5</span><span class="sxs-lookup"><span data-stu-id="83f9d-296">5</span></span>      | <span data-ttu-id="83f9d-297">Console</span><span class="sxs-lookup"><span data-stu-id="83f9d-297">Console</span></span>       | <span data-ttu-id="83f9d-298">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="83f9d-298">All categories</span></span>                          | <span data-ttu-id="83f9d-299">Informazioni</span><span class="sxs-lookup"><span data-stu-id="83f9d-299">Information</span></span>       |
| <span data-ttu-id="83f9d-300">6</span><span class="sxs-lookup"><span data-stu-id="83f9d-300">6</span></span>      | <span data-ttu-id="83f9d-301">Tutti i provider</span><span class="sxs-lookup"><span data-stu-id="83f9d-301">All providers</span></span> | <span data-ttu-id="83f9d-302">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="83f9d-302">All categories</span></span>                          | <span data-ttu-id="83f9d-303">Debug</span><span class="sxs-lookup"><span data-stu-id="83f9d-303">Debug</span></span>             |
| <span data-ttu-id="83f9d-304">7</span><span class="sxs-lookup"><span data-stu-id="83f9d-304">7</span></span>      | <span data-ttu-id="83f9d-305">Tutti i provider</span><span class="sxs-lookup"><span data-stu-id="83f9d-305">All providers</span></span> | <span data-ttu-id="83f9d-306">System</span><span class="sxs-lookup"><span data-stu-id="83f9d-306">System</span></span>                                  | <span data-ttu-id="83f9d-307">Debug</span><span class="sxs-lookup"><span data-stu-id="83f9d-307">Debug</span></span>             |
| <span data-ttu-id="83f9d-308">8</span><span class="sxs-lookup"><span data-stu-id="83f9d-308">8</span></span>      | <span data-ttu-id="83f9d-309">Debug</span><span class="sxs-lookup"><span data-stu-id="83f9d-309">Debug</span></span>         | <span data-ttu-id="83f9d-310">Microsoft</span><span class="sxs-lookup"><span data-stu-id="83f9d-310">Microsoft</span></span>                               | <span data-ttu-id="83f9d-311">Traccia</span><span class="sxs-lookup"><span data-stu-id="83f9d-311">Trace</span></span>             |

<span data-ttu-id="83f9d-312">Quando viene creato un oggetto `ILogger`, l'oggetto `ILoggerFactory` seleziona una singola regola per ogni provider da applicare al logger.</span><span class="sxs-lookup"><span data-stu-id="83f9d-312">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="83f9d-313">Tutti i messaggi scritti da un'istanza di `ILogger` vengono filtrati in base alle regole selezionate.</span><span class="sxs-lookup"><span data-stu-id="83f9d-313">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="83f9d-314">Tra le regole disponibili viene selezionata la regola più specifica possibile per ogni coppia di categoria e provider.</span><span class="sxs-lookup"><span data-stu-id="83f9d-314">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="83f9d-315">L'algoritmo seguente viene usato per ogni provider quando viene creato un `ILogger` per una determinata categoria:</span><span class="sxs-lookup"><span data-stu-id="83f9d-315">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="83f9d-316">Selezionare tutte le regole corrispondenti al provider o al relativo alias.</span><span class="sxs-lookup"><span data-stu-id="83f9d-316">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="83f9d-317">Se non viene trovata alcuna corrispondenza, selezionare tutte le regole con un provider vuoto.</span><span class="sxs-lookup"><span data-stu-id="83f9d-317">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="83f9d-318">Dal risultato del passaggio precedente, selezionare le regole con il prefisso di categoria corrispondente più lungo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-318">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="83f9d-319">Se non viene trovata alcuna corrispondenza, selezionare tutte le regole che non specificano una categoria.</span><span class="sxs-lookup"><span data-stu-id="83f9d-319">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="83f9d-320">Se sono selezionate più regole, scegliere l'**ultima**.</span><span class="sxs-lookup"><span data-stu-id="83f9d-320">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="83f9d-321">Se non è selezionata alcuna regola, usare `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-321">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="83f9d-322">Con l'elenco di regole precedente, si supponga di creare un oggetto `ILogger` per la categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="83f9d-322">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="83f9d-323">Per il provider Debug si applicano le regole 1, 6 e 8.</span><span class="sxs-lookup"><span data-stu-id="83f9d-323">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="83f9d-324">La regola 8 è più specifica, così sarà quella selezionata.</span><span class="sxs-lookup"><span data-stu-id="83f9d-324">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="83f9d-325">Per il provider Console si applicano le regole 3, 4, 5 e 6.</span><span class="sxs-lookup"><span data-stu-id="83f9d-325">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="83f9d-326">La regola 3 è la più specifica.</span><span class="sxs-lookup"><span data-stu-id="83f9d-326">Rule 3 is most specific.</span></span>

<span data-ttu-id="83f9d-327">L'istanza di `ILogger` risultante invia i log di livello `Trace` o superiore al provider Debug.</span><span class="sxs-lookup"><span data-stu-id="83f9d-327">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="83f9d-328">I log di livello `Debug` o superiore vengono inviati al provider Console.</span><span class="sxs-lookup"><span data-stu-id="83f9d-328">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="83f9d-329">Alias dei provider</span><span class="sxs-lookup"><span data-stu-id="83f9d-329">Provider aliases</span></span>

<span data-ttu-id="83f9d-330">Ogni provider definisce un *alias* che può essere utilizzato nella configurazione al posto del nome completo di tipo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-330">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="83f9d-331">Per i provider predefiniti, usare gli alias seguenti:</span><span class="sxs-lookup"><span data-stu-id="83f9d-331">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="83f9d-332">Console</span><span class="sxs-lookup"><span data-stu-id="83f9d-332">Console</span></span>
* <span data-ttu-id="83f9d-333">Debug</span><span class="sxs-lookup"><span data-stu-id="83f9d-333">Debug</span></span>
* <span data-ttu-id="83f9d-334">EventLog</span><span class="sxs-lookup"><span data-stu-id="83f9d-334">EventLog</span></span>
* <span data-ttu-id="83f9d-335">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="83f9d-335">AzureAppServices</span></span>
* <span data-ttu-id="83f9d-336">TraceSource</span><span class="sxs-lookup"><span data-stu-id="83f9d-336">TraceSource</span></span>
* <span data-ttu-id="83f9d-337">EventSource</span><span class="sxs-lookup"><span data-stu-id="83f9d-337">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="83f9d-338">Livello minimo predefinito</span><span class="sxs-lookup"><span data-stu-id="83f9d-338">Default minimum level</span></span>

<span data-ttu-id="83f9d-339">Esiste un'impostazione del livello minimo che diventa effettiva solo se non si applica alcuna regola di configurazione o codice per un provider e una categoria specifici.</span><span class="sxs-lookup"><span data-stu-id="83f9d-339">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="83f9d-340">L'esempio seguente illustra come impostare il livello minimo:</span><span class="sxs-lookup"><span data-stu-id="83f9d-340">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="83f9d-341">Se non si imposta in modo esplicito il livello minimo, il valore predefinito è `Information`, che indica che i log `Trace` e `Debug` vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="83f9d-341">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="83f9d-342">Funzioni di filtro</span><span class="sxs-lookup"><span data-stu-id="83f9d-342">Filter functions</span></span>

<span data-ttu-id="83f9d-343">Una funzione di filtro viene richiamata per tutti i provider e le categorie a cui non sono assegnate regole tramite la configurazione o il codice.</span><span class="sxs-lookup"><span data-stu-id="83f9d-343">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="83f9d-344">Il codice della funzione ha accesso al tipo di provider, alla categoria e al livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="83f9d-344">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="83f9d-345">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="83f9d-345">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="83f9d-346">Alcuni provider di registrazione consentono di specificare quando i log devono essere scritti in un supporto di archiviazione oppure ignorati in base al livello di registrazione e alla categoria.</span><span class="sxs-lookup"><span data-stu-id="83f9d-346">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="83f9d-347">I metodi di estensione `AddConsole` e `AddDebug` forniscono overload che accettano criteri di filtro.</span><span class="sxs-lookup"><span data-stu-id="83f9d-347">The `AddConsole` and `AddDebug` extension methods provide overloads that accept filtering criteria.</span></span> <span data-ttu-id="83f9d-348">L'esempio di codice seguente fa sì che il provider Console ignori i log di livello inferiore a `Warning`, mentre il provider Debug ignora i log creati dal framework.</span><span class="sxs-lookup"><span data-stu-id="83f9d-348">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="83f9d-349">Il metodo `AddEventLog` ha un overload che accetta un'istanza di `EventLogSettings`, che può contenere una funzione di filtro nella proprietà `Filter`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-349">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="83f9d-350">Il provider TraceSource non fornisce alcun overload di questo tipo, in quanto il suo livello di registrazione e altri parametri si basano sugli oggetti `SourceSwitch` e `TraceListener` in uso.</span><span class="sxs-lookup"><span data-stu-id="83f9d-350">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="83f9d-351">Per impostare regole di filtro per tutti i provider registrati con un'istanza di `ILoggerFactory`, usare il metodo di estensione `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-351">To set filtering rules for all providers that are registered with an `ILoggerFactory` instance, use the `WithFilter` extension method.</span></span> <span data-ttu-id="83f9d-352">L'esempio seguente limita i log del framework (la categoria inizia con "Microsoft" o "System") agli avvisi consentendo la registrazione a livello debug per i log creati dal codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="83f9d-352">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while logging at debug level for logs created by application code.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="83f9d-353">Per impedire la scrittura di log, specificare `LogLevel.None` come livello di registrazione minimo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-353">To prevent any logs from being written, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="83f9d-354">Il valore intero di `LogLevel.None` è 6, maggiore di `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="83f9d-354">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="83f9d-355">Il metodo di estensione `WithFilter` viene fornito dal pacchetto NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter).</span><span class="sxs-lookup"><span data-stu-id="83f9d-355">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="83f9d-356">Il metodo restituisce una nuova istanza di `ILoggerFactory` che filtra i messaggi di registrazione passati a tutti i provider di logger registrati con esso.</span><span class="sxs-lookup"><span data-stu-id="83f9d-356">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="83f9d-357">Non si applica ad altre istanze di `ILoggerFactory`, inclusa l'istanza originale di `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-357">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="83f9d-358">Livelli e categorie di sistema</span><span class="sxs-lookup"><span data-stu-id="83f9d-358">System categories and levels</span></span>

<span data-ttu-id="83f9d-359">Di seguito sono elencate alcune categorie usate da ASP.NET Core ed Entity Framework Core, con note sui log previsti per ognuna:</span><span class="sxs-lookup"><span data-stu-id="83f9d-359">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="83f9d-360">Category</span><span class="sxs-lookup"><span data-stu-id="83f9d-360">Category</span></span>                            | <span data-ttu-id="83f9d-361">Note</span><span class="sxs-lookup"><span data-stu-id="83f9d-361">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="83f9d-362">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="83f9d-362">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="83f9d-363">Diagnostica generale di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="83f9d-363">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="83f9d-364">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="83f9d-364">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="83f9d-365">Chiavi considerate, trovate e usate.</span><span class="sxs-lookup"><span data-stu-id="83f9d-365">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="83f9d-366">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="83f9d-366">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="83f9d-367">Host consentiti.</span><span class="sxs-lookup"><span data-stu-id="83f9d-367">Hosts allowed.</span></span> |
| <span data-ttu-id="83f9d-368">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="83f9d-368">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="83f9d-369">Tempo impiegato per il completamento delle richieste HTTP e ora di inizio delle richieste.</span><span class="sxs-lookup"><span data-stu-id="83f9d-369">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="83f9d-370">Assembly di avvio dell'hosting caricati.</span><span class="sxs-lookup"><span data-stu-id="83f9d-370">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="83f9d-371">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="83f9d-371">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="83f9d-372">Diagnostica di MVC e Razor.</span><span class="sxs-lookup"><span data-stu-id="83f9d-372">MVC and Razor diagnostics.</span></span> <span data-ttu-id="83f9d-373">Associazione di modelli, esecuzione di filtri, compilazione di viste, selezione di azioni.</span><span class="sxs-lookup"><span data-stu-id="83f9d-373">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="83f9d-374">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="83f9d-374">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="83f9d-375">Informazioni sulla corrispondenza di route.</span><span class="sxs-lookup"><span data-stu-id="83f9d-375">Route matching information.</span></span> |
| <span data-ttu-id="83f9d-376">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="83f9d-376">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="83f9d-377">Risposte di avvio, arresto e keep-alive per la connessione.</span><span class="sxs-lookup"><span data-stu-id="83f9d-377">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="83f9d-378">Informazioni sui certificati HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83f9d-378">HTTPS certificate information.</span></span> |
| <span data-ttu-id="83f9d-379">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="83f9d-379">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="83f9d-380">File forniti.</span><span class="sxs-lookup"><span data-stu-id="83f9d-380">Files served.</span></span> |
| <span data-ttu-id="83f9d-381">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="83f9d-381">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="83f9d-382">Diagnostica generale di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="83f9d-382">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="83f9d-383">Attività e configurazione del database, rilevamento delle modifiche, migrazioni.</span><span class="sxs-lookup"><span data-stu-id="83f9d-383">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="83f9d-384">Ambiti dei log</span><span class="sxs-lookup"><span data-stu-id="83f9d-384">Log scopes</span></span>

 <span data-ttu-id="83f9d-385">Un *ambito* può raggruppare un set di operazioni logiche.</span><span class="sxs-lookup"><span data-stu-id="83f9d-385">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="83f9d-386">Questo raggruppamento può essere usato per collegare gli stessi dati a ogni log creato come parte di un set.</span><span class="sxs-lookup"><span data-stu-id="83f9d-386">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="83f9d-387">Ad esempio, ogni log creato come parte dell'elaborazione di una transazione può includere l'ID transazione.</span><span class="sxs-lookup"><span data-stu-id="83f9d-387">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="83f9d-388">Un ambito è un tipo `IDisposable` restituito dal metodo <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> e viene mantenuto fino all'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="83f9d-388">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="83f9d-389">Un ambito si usa mediante il wrapping delle chiamate del logger in un blocco `using`:</span><span class="sxs-lookup"><span data-stu-id="83f9d-389">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="83f9d-390">Il codice seguente abilita gli ambiti per il provider Console:</span><span class="sxs-lookup"><span data-stu-id="83f9d-390">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="83f9d-391">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="83f9d-391">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="83f9d-392">La configurazione dell'opzione del logger della console `IncludeScopes` è necessaria per abilitare la registrazione basata sull'ambito.</span><span class="sxs-lookup"><span data-stu-id="83f9d-392">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="83f9d-393">Per informazioni sulla configurazione, vedere la sezione [Configurazione](#configuration).</span><span class="sxs-lookup"><span data-stu-id="83f9d-393">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="83f9d-394">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="83f9d-394">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="83f9d-395">La configurazione dell'opzione del logger della console `IncludeScopes` è necessaria per abilitare la registrazione basata sull'ambito.</span><span class="sxs-lookup"><span data-stu-id="83f9d-395">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="83f9d-396">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="83f9d-396">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="83f9d-397">Ogni messaggio di registrazione include le informazioni sull'ambito:</span><span class="sxs-lookup"><span data-stu-id="83f9d-397">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="83f9d-398">Provider di registrazione predefiniti</span><span class="sxs-lookup"><span data-stu-id="83f9d-398">Built-in logging providers</span></span>

<span data-ttu-id="83f9d-399">ASP.NET Core include i provider seguenti:</span><span class="sxs-lookup"><span data-stu-id="83f9d-399">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="83f9d-400">Console</span><span class="sxs-lookup"><span data-stu-id="83f9d-400">Console</span></span>](#console-provider)
* [<span data-ttu-id="83f9d-401">Debug</span><span class="sxs-lookup"><span data-stu-id="83f9d-401">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="83f9d-402">EventSource</span><span class="sxs-lookup"><span data-stu-id="83f9d-402">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="83f9d-403">EventLog</span><span class="sxs-lookup"><span data-stu-id="83f9d-403">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="83f9d-404">TraceSource</span><span class="sxs-lookup"><span data-stu-id="83f9d-404">TraceSource</span></span>](#tracesource-provider)

<span data-ttu-id="83f9d-405">Le opzioni per la [registrazione in Azure](#logging-in-azure) sono illustrate più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-405">Options for [Logging in Azure](#logging-in-azure) are covered later in this article.</span></span>

<span data-ttu-id="83f9d-406">Per informazioni sulla registrazione stdout, vedere <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> e <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="83f9d-406">For information about stdout logging, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> and <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="83f9d-407">Provider Console</span><span class="sxs-lookup"><span data-stu-id="83f9d-407">Console provider</span></span>

<span data-ttu-id="83f9d-408">Il pacchetto del provider [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) invia l'output della registrazione alla console.</span><span class="sxs-lookup"><span data-stu-id="83f9d-408">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="83f9d-409">Gli [overload di AddConsole](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) consentono di passare un livello di registrazione minimo, una funzione di filtro e un valore booleano che indica se gli ambiti sono supportati.</span><span class="sxs-lookup"><span data-stu-id="83f9d-409">[AddConsole overloads](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) let you pass in a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="83f9d-410">Un'altra opzione consiste nel passare un oggetto `IConfiguration`, che può specificare livelli di registrazione e il supporto di ambiti.</span><span class="sxs-lookup"><span data-stu-id="83f9d-410">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="83f9d-411">Il provider Console ha un impatto significativo sulle prestazioni e non è in genere appropriato per l'uso in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="83f9d-411">The console provider has a significant impact on performance and is generally not appropriate for use in production.</span></span>

<span data-ttu-id="83f9d-412">Quando si crea un nuovo progetto in Visual Studio, il metodo `AddConsole` è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="83f9d-412">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="83f9d-413">Questo codice fa riferimento alla sezione `Logging` del file *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="83f9d-413">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="83f9d-414">Le impostazioni visualizzate limitano i log del framework agli avvisi, consentendo all'app di registrare a livello di debug, come descritto nella sezione [Filtri dei log](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="83f9d-414">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="83f9d-415">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="83f9d-415">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

<span data-ttu-id="83f9d-416">Per visualizzare l'output di registrazione del provider Console, aprire un prompt dei comandi nella cartella del progetto ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="83f9d-416">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```console
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="83f9d-417">Provider Debug</span><span class="sxs-lookup"><span data-stu-id="83f9d-417">Debug provider</span></span>

<span data-ttu-id="83f9d-418">Il pacchetto del provider [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) scrive l'output della registrazione usando la classe [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (chiamate del metodo `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="83f9d-418">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="83f9d-419">In Linux, questo provider scrive i log in */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="83f9d-419">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="83f9d-420">Gli [overload di AddDebug](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) consentono passare un livello di registrazione minimo o una funzione di filtro.</span><span class="sxs-lookup"><span data-stu-id="83f9d-420">[AddDebug overloads](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="83f9d-421">Provider EventSource</span><span class="sxs-lookup"><span data-stu-id="83f9d-421">EventSource provider</span></span>

<span data-ttu-id="83f9d-422">Per le app destinate a ASP.NET Core 1.1.0 o versione successiva, il pacchetto di provider [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) può implementare la traccia degli eventi.</span><span class="sxs-lookup"><span data-stu-id="83f9d-422">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="83f9d-423">In Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="83f9d-423">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="83f9d-424">Il provider è multipiattaforma, ma non sono ancora disponibili gli strumenti di raccolta e visualizzazione degli eventi per Linux o macOS.</span><span class="sxs-lookup"><span data-stu-id="83f9d-424">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

<span data-ttu-id="83f9d-425">Un buon metodo per raccogliere e visualizzare i log consiste nell'usare l'[utilità PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="83f9d-425">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="83f9d-426">Sono disponibili altri strumenti per la visualizzazione dei log ETW, ma PerfView fornisce un'esperienza ottimale per l'uso con gli eventi ETW generati da ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="83f9d-426">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="83f9d-427">Per configurare PerfView per la raccolta degli eventi registrati da questo provider, aggiungere la stringa `*Microsoft-Extensions-Logging` nell'elenco **Provider aggiuntivi**</span><span class="sxs-lookup"><span data-stu-id="83f9d-427">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="83f9d-428">(non dimenticare l'asterisco all'inizio della stringa).</span><span class="sxs-lookup"><span data-stu-id="83f9d-428">(Don't miss the asterisk at the start of the string.)</span></span>

![Provider aggiuntivi di PerfView](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="83f9d-430">Provider EventLog di Windows</span><span class="sxs-lookup"><span data-stu-id="83f9d-430">Windows EventLog provider</span></span>

<span data-ttu-id="83f9d-431">Il pacchetto di provider [Microsoft.Extensions.Logging.AventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) invia l'output del log al registro eventi di Windows.</span><span class="sxs-lookup"><span data-stu-id="83f9d-431">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="83f9d-432">Gli [overload di AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) consentono di passare `EventLogSettings` o un livello di registrazione minimo.</span><span class="sxs-lookup"><span data-stu-id="83f9d-432">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="83f9d-433">Provider TraceSource</span><span class="sxs-lookup"><span data-stu-id="83f9d-433">TraceSource provider</span></span>

<span data-ttu-id="83f9d-434">Il pacchetto di provider [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa le librerie e i provider <xref:System.Diagnostics.TraceSource>.</span><span class="sxs-lookup"><span data-stu-id="83f9d-434">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

<span data-ttu-id="83f9d-435">Gli [overload di AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) consentono di passare un cambio di origine e un listener di traccia.</span><span class="sxs-lookup"><span data-stu-id="83f9d-435">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="83f9d-436">Per usare questo provider, un'app deve essere in esecuzione in .NET Framework (invece che in .NET Core).</span><span class="sxs-lookup"><span data-stu-id="83f9d-436">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="83f9d-437">Il provider può instradare i messaggi a diversi [listener](/dotnet/framework/debug-trace-profile/trace-listeners), come <xref:System.Diagnostics.TextWriterTraceListener>, usato nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="83f9d-437">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="83f9d-438">L'esempio seguente configura un provider `TraceSource` che registra messaggi `Warning` e di livello superiore nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="83f9d-438">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a><span data-ttu-id="83f9d-439">Registrazione in Azure</span><span class="sxs-lookup"><span data-stu-id="83f9d-439">Logging in Azure</span></span>

<span data-ttu-id="83f9d-440">Per informazioni sulla registrazione in Azure, vedere le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="83f9d-440">For information about logging in Azure, see the following sections:</span></span>

* [<span data-ttu-id="83f9d-441">Provider di Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="83f9d-441">Azure App Service provider</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="83f9d-442">Flusso di registrazione di Azure</span><span class="sxs-lookup"><span data-stu-id="83f9d-442">Azure log streaming</span></span>](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [<span data-ttu-id="83f9d-443">Registrazione traccia di Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="83f9d-443">Azure Application Insights trace logging</span></span>](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="83f9d-444">Provider del Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="83f9d-444">Azure App Service provider</span></span>

<span data-ttu-id="83f9d-445">Il pacchetto di provider [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) scrive i log in file di testo nel file system di un'app del Servizio app di Azure e nell'[archivio di BLOB](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in un account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="83f9d-445">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="83f9d-446">Il pacchetto di provider è disponibile per le app destinate a .NET Core 1.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="83f9d-446">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="83f9d-447">Se la destinazione è .NET Core, tenere presente quanto segue:</span><span class="sxs-lookup"><span data-stu-id="83f9d-447">If targeting .NET Core, note the following points:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="83f9d-448">Il pacchetto del provider è incluso nel [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="83f9d-448">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="83f9d-449">Il pacchetto del provider non è incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="83f9d-449">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="83f9d-450">Per usare il provider, installare il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="83f9d-450">To use the provider, install the package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="83f9d-451">Non chiamare in modo esplicito <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>.</span><span class="sxs-lookup"><span data-stu-id="83f9d-451">Don't explicitly call <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>.</span></span> <span data-ttu-id="83f9d-452">Il provider diventa automaticamente disponibile per l'app quando l'app viene distribuita nel Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="83f9d-452">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="83f9d-453">Se la destinazione è .NET Framework o il riferimento è il metapacchetto `Microsoft.AspNetCore.App`, aggiungere il pacchetto di provider al progetto.</span><span class="sxs-lookup"><span data-stu-id="83f9d-453">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="83f9d-454">Richiamare `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="83f9d-454">Invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="83f9d-455">Un overload di <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> consente di passare <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="83f9d-455">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="83f9d-456">L'oggetto impostazioni può eseguire l'override delle impostazioni predefinite, ad esempio il modello di output di registrazione, il nome di BLOB e il limite di dimensioni di file.</span><span class="sxs-lookup"><span data-stu-id="83f9d-456">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="83f9d-457">Il *modello di output* è un modello di messaggio che viene applicato a tutti i log in aggiunta a quanto specificato quando si chiama un metodo `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-457">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="83f9d-458">Per configurare le impostazioni del provider, usare <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> e <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="83f9d-458">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

<span data-ttu-id="83f9d-459">Quando si distribuisce un'app del servizio app, l'applicazione rispetta le impostazioni della sezione [Log di diagnostica](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) della pagina **Servizio app** del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="83f9d-459">When you deploy to an App Service app, the application honors the settings in the [Diagnostic Logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="83f9d-460">Quando queste impostazioni vengono aggiornate, le modifiche hanno effetto immediato senza richiedere un riavvio o la ridistribuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="83f9d-460">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Impostazioni di registrazione di Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="83f9d-462">Il percorso predefinito per i file di log è nella cartella *D:\\home\\LogFiles\\Application* e il nome di file predefinito è *diagnostics-aaaammgg.txt*.</span><span class="sxs-lookup"><span data-stu-id="83f9d-462">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="83f9d-463">Il limite predefinito per le dimensioni del file è 10 MB e il numero massimo predefinito di file conservati è 2.</span><span class="sxs-lookup"><span data-stu-id="83f9d-463">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="83f9d-464">Il nome BLOB predefinito è *{nome-app}{timestamp}/aaaa/mm/gg/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="83f9d-464">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="83f9d-465">Il provider funziona solo quando il progetto viene eseguito nell'ambiente di Azure.</span><span class="sxs-lookup"><span data-stu-id="83f9d-465">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="83f9d-466">Non ha alcun effetto quando il progetto viene eseguito localmente, in quanto non scrive nulla in file locali o in archivi di sviluppo locali per i BLOB.</span><span class="sxs-lookup"><span data-stu-id="83f9d-466">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

### <a name="azure-log-streaming"></a><span data-ttu-id="83f9d-467">Flusso di registrazione di Azure</span><span class="sxs-lookup"><span data-stu-id="83f9d-467">Azure log streaming</span></span>

<span data-ttu-id="83f9d-468">Il flusso di registrazione di Azure consente di visualizzare l'attività di registrazione in tempo reale da:</span><span class="sxs-lookup"><span data-stu-id="83f9d-468">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="83f9d-469">Server applicazioni</span><span class="sxs-lookup"><span data-stu-id="83f9d-469">The app server</span></span>
* <span data-ttu-id="83f9d-470">Server Web</span><span class="sxs-lookup"><span data-stu-id="83f9d-470">The web server</span></span>
* <span data-ttu-id="83f9d-471">Traccia delle richieste non riuscite</span><span class="sxs-lookup"><span data-stu-id="83f9d-471">Failed request tracing</span></span>

<span data-ttu-id="83f9d-472">Per configurare il flusso di registrazione di Azure:</span><span class="sxs-lookup"><span data-stu-id="83f9d-472">To configure Azure log streaming:</span></span>

* <span data-ttu-id="83f9d-473">Passare alla pagina **Log di diagnostica** dalla pagina del portale dell'app.</span><span class="sxs-lookup"><span data-stu-id="83f9d-473">Navigate to the **Diagnostics Logs** page from your app's portal page.</span></span>
* <span data-ttu-id="83f9d-474">Impostare **Registrazione applicazioni (file system)** su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="83f9d-474">Set **Application Logging (Filesystem)** to **On**.</span></span>

![Pagina dei log di diagnostica del portale di Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="83f9d-476">Passare alla pagina **Flusso di registrazione** per visualizzare i messaggi dell'app.</span><span class="sxs-lookup"><span data-stu-id="83f9d-476">Navigate to the **Log Streaming** page to view app messages.</span></span> <span data-ttu-id="83f9d-477">I messaggi vengono registrati dall'app tramite l'interfaccia `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-477">They're logged by the app through the `ILogger` interface.</span></span>

![Flusso di registrazione dell'applicazione nel portale di Azure](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="83f9d-479">Registrazione di traccia di Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="83f9d-479">Azure Application Insights trace logging</span></span>

<span data-ttu-id="83f9d-480">Application Insights SDK è in grado di raccogliere e segnalare i log generati dall'infrastruttura di registrazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="83f9d-480">The Application Insights SDK can collect and report logs generated by the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="83f9d-481">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="83f9d-481">For more information, see the following resources:</span></span>

* [<span data-ttu-id="83f9d-482">Panoramica di Application Insights</span><span class="sxs-lookup"><span data-stu-id="83f9d-482">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* [<span data-ttu-id="83f9d-483">Application Insights per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="83f9d-483">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* <span data-ttu-id="83f9d-484">[Adattatori di registrazione di Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span><span class="sxs-lookup"><span data-stu-id="83f9d-484">[Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span></span>
* [<span data-ttu-id="83f9d-485">Esempi di implementazione di ILogger per Application Insights</span><span class="sxs-lookup"><span data-stu-id="83f9d-485">Application Insights ILogger implementation samples</span></span>](/azure/azure-monitor/app/ilogger)

::: moniker-end

## <a name="third-party-logging-providers"></a><span data-ttu-id="83f9d-486">Provider di registrazione di terze parti</span><span class="sxs-lookup"><span data-stu-id="83f9d-486">Third-party logging providers</span></span>

<span data-ttu-id="83f9d-487">Framework di registrazione di terze parti che usano ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="83f9d-487">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="83f9d-488">[elmah.io](https://elmah.io/) ([repository GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="83f9d-488">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="83f9d-489">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repository GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="83f9d-489">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="83f9d-490">[JSNLog](http://jsnlog.com/) ([repository GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="83f9d-490">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="83f9d-491">[KissLog.net](https://kisslog.net/) ([repository GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="83f9d-491">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="83f9d-492">[Loggr](http://loggr.net/) ([repository GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="83f9d-492">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="83f9d-493">[NLog](http://nlog-project.org/) ([repository GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="83f9d-493">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="83f9d-494">[Sentry](https://sentry.io/welcome/) ([repository GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="83f9d-494">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="83f9d-495">[Serilog](https://serilog.net/) ([repository GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="83f9d-495">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>
* <span data-ttu-id="83f9d-496">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([repository Github](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="83f9d-496">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="83f9d-497">Alcuni framework di terze parti possono eseguire la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="83f9d-497">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="83f9d-498">L'uso di un framework di terze parti è simile a quello di uno dei provider predefiniti:</span><span class="sxs-lookup"><span data-stu-id="83f9d-498">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="83f9d-499">Aggiungere un pacchetto NuGet al progetto.</span><span class="sxs-lookup"><span data-stu-id="83f9d-499">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="83f9d-500">Chiamare un oggetto `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="83f9d-500">Call an `ILoggerFactory`.</span></span>

<span data-ttu-id="83f9d-501">Per altre informazioni, vedere la documentazione di ogni provider.</span><span class="sxs-lookup"><span data-stu-id="83f9d-501">For more information, see each provider's documentation.</span></span> <span data-ttu-id="83f9d-502">I provider di registrazione di terze parti non sono supportati da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="83f9d-502">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="83f9d-503">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="83f9d-503">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
