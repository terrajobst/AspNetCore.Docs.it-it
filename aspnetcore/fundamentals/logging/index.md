---
title: Registrazione in ASP.NET Core
author: ardalis
description: Informazioni sul framework di registrazione di ASP.NET Core. Informazioni sui provider di registrazione predefiniti e sui provider di terze parti più diffusi.
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: 969ad303c3fee06aa40d43140153ffbf58b735db
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126287"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="4471c-104">Registrazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4471c-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="4471c-105">Di [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4471c-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4471c-106">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="4471c-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="4471c-107">I provider predefiniti consentono di inviare i log a una o più destinazioni. È possibile collegare un framework di registrazione di terze parti.</span><span class="sxs-lookup"><span data-stu-id="4471c-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="4471c-108">Questo articolo illustra come usare l'API e i provider di registrazione incorporati nel codice.</span><span class="sxs-lookup"><span data-stu-id="4471c-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4471c-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4471c-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4471c-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4471c-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

## <a name="how-to-create-logs"></a><span data-ttu-id="4471c-111">Come creare log</span><span class="sxs-lookup"><span data-stu-id="4471c-111">How to create logs</span></span>

<span data-ttu-id="4471c-112">Per creare i log, implementare un oggetto [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) dal contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="4471c-112">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="4471c-113">Quindi chiamare i metodi di registrazione su tale oggetto logger:</span><span class="sxs-lookup"><span data-stu-id="4471c-113">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="4471c-114">Questo esempio crea log con la classe `TodoController` come *categoria*.</span><span class="sxs-lookup"><span data-stu-id="4471c-114">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="4471c-115">Le categorie sono descritte [più avanti in questo articolo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="4471c-115">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="4471c-116">ASP.NET Core non fornisce metodi di logger asincroni perché la registrazione deve essere così rapida che non vale la pena di usare un metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="4471c-116">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="4471c-117">Se la situazione richiede l'uso di metodi asincroni, considerare l'ipotesi di cambiare modalità di registrazione.</span><span class="sxs-lookup"><span data-stu-id="4471c-117">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="4471c-118">Se l'archivio dati è lento, scrivere i messaggi di log prima in un archivio veloce, quindi spostarli in un archivio lento in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="4471c-118">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="4471c-119">Registrare ad esempio in una coda di messaggi che viene letta e salvata in modo permanente in un archivio lento da un altro processo.</span><span class="sxs-lookup"><span data-stu-id="4471c-119">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="4471c-120">Come aggiungere provider</span><span class="sxs-lookup"><span data-stu-id="4471c-120">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4471c-121">Un provider di registrazione recupera i messaggi creati dall'utente con un oggetto `ILogger` e li visualizza o li archivia.</span><span class="sxs-lookup"><span data-stu-id="4471c-121">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="4471c-122">Il provider Console, ad esempio, visualizza i messaggi nella console e il provider del Servizio app di Azure può archiviarli nell'archivio BLOB di Azure.</span><span class="sxs-lookup"><span data-stu-id="4471c-122">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="4471c-123">Per usare un provider, chiamare il metodo di estensione `Add<ProviderName>` del provider in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4471c-123">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="4471c-124">Il modello di progetto predefinito consente la registrazione con il metodo [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___):</span><span class="sxs-lookup"><span data-stu-id="4471c-124">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4471c-125">Un provider di registrazione recupera i messaggi creati dall'utente con un oggetto `ILogger` e li visualizza o li archivia.</span><span class="sxs-lookup"><span data-stu-id="4471c-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="4471c-126">Il provider Console, ad esempio, visualizza i messaggi nella console e il provider del Servizio app di Azure può archiviarli nell'archivio BLOB di Azure.</span><span class="sxs-lookup"><span data-stu-id="4471c-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="4471c-127">Per usare un provider, installare il relativo pacchetto NuGet e chiamare il metodo di estensione del provider in un'istanza di [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="4471c-127">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="4471c-128">L'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) di ASP.NET Core fornisce l'istanza di `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="4471c-128">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="4471c-129">I metodi di estensione `AddConsole` e `AddDebug` sono definiti nei pacchetti [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="4471c-129">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="4471c-130">Ogni metodo di estensione chiama il metodo `ILoggerFactory.AddProvider` passando un'istanza del provider.</span><span class="sxs-lookup"><span data-stu-id="4471c-130">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="4471c-131">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) aggiunge provider di log nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="4471c-131">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="4471c-132">Per ottenere l'output del log dal codice eseguito in precedenza, aggiungere i provider di log nel costruttore della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="4471c-132">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="4471c-133">Altre informazioni su ciascun [provider di registrazione incorporato](#built-in-logging-providers) e i collegamenti ai [provider di registrazione di terze parti](#third-party-logging-providers) sono disponibili più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="4471c-133">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="settings-file-configuration"></a><span data-ttu-id="4471c-134">Configurazione del file di impostazioni</span><span class="sxs-lookup"><span data-stu-id="4471c-134">Settings file configuration</span></span>

<span data-ttu-id="4471c-135">Ognuno degli esempi precedenti nella sezione [Come aggiungere provider](#how-to-add-providers) carica la configurazione del provider di log dalla sezione `Logging` dei file di impostazioni dell'app.</span><span class="sxs-lookup"><span data-stu-id="4471c-135">Each of the preceding examples in the [How to add providers](#how-to-add-providers) section loads logging provider configuration from the `Logging` section of app settings files.</span></span> <span data-ttu-id="4471c-136">L'esempio seguente mostra il contenuto di un file *appsettings.Development.json* tipico:</span><span class="sxs-lookup"><span data-stu-id="4471c-136">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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
      "IncludeScopes": "true"
    }
  }
}
```

<span data-ttu-id="4471c-137">Le chiavi `LogLevel` rappresentano i nomi dei log.</span><span class="sxs-lookup"><span data-stu-id="4471c-137">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="4471c-138">La chiave `Default` si applica ai log non esplicitamente elencati.</span><span class="sxs-lookup"><span data-stu-id="4471c-138">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="4471c-139">Il valore rappresenta il [livello del log](#log-level) applicato al log specificato.</span><span class="sxs-lookup"><span data-stu-id="4471c-139">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="4471c-140">Le chiavi dei log che impostano `IncludeScopes` (`Console` nell'esempio) specificano se gli [ambiti dei log](#log-scopes) sono abilitati per il log indicato.</span><span class="sxs-lookup"><span data-stu-id="4471c-140">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="4471c-141">Le chiavi `LogLevel` rappresentano i nomi dei log.</span><span class="sxs-lookup"><span data-stu-id="4471c-141">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="4471c-142">La chiave `Default` si applica ai log non esplicitamente elencati.</span><span class="sxs-lookup"><span data-stu-id="4471c-142">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="4471c-143">Il valore rappresenta il [livello del log](#log-level) applicato al log specificato.</span><span class="sxs-lookup"><span data-stu-id="4471c-143">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

## <a name="sample-logging-output"></a><span data-ttu-id="4471c-144">Esempio di output di registrazione</span><span class="sxs-lookup"><span data-stu-id="4471c-144">Sample logging output</span></span>

<span data-ttu-id="4471c-145">Con il codice di esempio illustrato nella sezione precedente si visualizzeranno i log nella console durante l'esecuzione dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4471c-145">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="4471c-146">Ecco un esempio di output della console:</span><span class="sxs-lookup"><span data-stu-id="4471c-146">Here's an example of console output:</span></span>

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

<span data-ttu-id="4471c-147">Questi log sono stati creati in `http://localhost:5000/api/todo/0`, attivando l'esecuzione di entrambe le chiamate a `ILogger` illustrate nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="4471c-147">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="4471c-148">Ecco un esempio degli stessi log nella finestra Debug quando si esegue l'applicazione di esempio in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4471c-148">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="4471c-149">I log che sono stati creati con le chiamate a `ILogger` illustrate nella sezione precedente iniziano con "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="4471c-149">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="4471c-150">I log che iniziano con le categorie "Microsoft" sono di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4471c-150">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="4471c-151">ASP.NET Core stesso e il codice dell'applicazione usano la stessa API di registrazione e gli stessi provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="4471c-151">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="4471c-152">Il resto di questo articolo spiega alcuni dettagli e le opzioni per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="4471c-152">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="4471c-153">Pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="4471c-153">NuGet packages</span></span>

<span data-ttu-id="4471c-154">Le interfacce `ILogger` e `ILoggerFactory` si trovano in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) e le loro implementazioni predefinite si trovano in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="4471c-154">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="4471c-155">Categoria dei log</span><span class="sxs-lookup"><span data-stu-id="4471c-155">Log category</span></span>

<span data-ttu-id="4471c-156">Ogni log creato include una *categoria*.</span><span class="sxs-lookup"><span data-stu-id="4471c-156">A *category* is included with each log that you create.</span></span> <span data-ttu-id="4471c-157">La categoria viene specificata quando si crea un oggetto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="4471c-157">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="4471c-158">La categoria può essere qualsiasi stringa, ma una convenzione prevede l'uso del nome completo della classe da cui vengono scritti i log.</span><span class="sxs-lookup"><span data-stu-id="4471c-158">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="4471c-159">Ad esempio: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="4471c-159">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="4471c-160">È possibile specificare la categoria sotto forma di stringa o usare un metodo di estensione che deriva la categoria dal tipo.</span><span class="sxs-lookup"><span data-stu-id="4471c-160">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="4471c-161">Per specificare la categoria sotto forma di stringa, chiamare `CreateLogger` su un'istanza di `ILoggerFactory`, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="4471c-161">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="4471c-162">In genere è più semplice usare `ILogger<T>`, come nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="4471c-162">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="4471c-163">Ciò equivale a chiamare `CreateLogger` con il nome completo del tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="4471c-163">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="4471c-164">Livello di registrazione</span><span class="sxs-lookup"><span data-stu-id="4471c-164">Log level</span></span>

<span data-ttu-id="4471c-165">Ogni volta che si scrive un log, si specifica il relativo [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="4471c-165">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="4471c-166">Il livello di registrazione indica il livello di gravità o priorità.</span><span class="sxs-lookup"><span data-stu-id="4471c-166">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="4471c-167">È ad esempio possibile scrivere un log `Information` quando un metodo termina normalmente, un log `Warning` quando un metodo restituisce un codice di errore 404 e un log `Error` quando viene rilevata un'eccezione imprevista.</span><span class="sxs-lookup"><span data-stu-id="4471c-167">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="4471c-168">Nell'esempio di codice seguente i nomi dei metodi (ad esempio `LogWarning`) specificano il livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="4471c-168">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="4471c-169">Il primo parametro è l'[ID dell'evento di registrazione](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="4471c-169">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="4471c-170">Il secondo parametro è un [modello di messaggio](#log-message-template) con segnaposto per i valori degli argomenti forniti dai rimanenti parametri del metodo.</span><span class="sxs-lookup"><span data-stu-id="4471c-170">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="4471c-171">I parametri del metodo sono descritti in maggiore dettaglio in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="4471c-171">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="4471c-172">I metodi di registrazione che includono il livello nel nome del metodo sono i [metodi di estensione di ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="4471c-172">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="4471c-173">Questi metodi chiamano in background un metodo `Log` che accetta un parametro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="4471c-173">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="4471c-174">È possibile chiamare il metodo `Log` direttamente anziché chiamare uno di questi metodi di estensione, ma la sintassi è relativamente complessa.</span><span class="sxs-lookup"><span data-stu-id="4471c-174">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="4471c-175">Per altre informazioni, vedere l'[interfaccia ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) e il [codice sorgente delle estensioni del logger](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="4471c-175">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="4471c-176">ASP.NET Core definisce i seguenti [livelli di registrazione](/dotnet/api/microsoft.extensions.logging.loglevel), ordinati di seguito dal meno grave al più grave.</span><span class="sxs-lookup"><span data-stu-id="4471c-176">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="4471c-177">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="4471c-177">Trace = 0</span></span>

  <span data-ttu-id="4471c-178">Per informazioni utili solo per uno sviluppatore che esegue il debug di un problema.</span><span class="sxs-lookup"><span data-stu-id="4471c-178">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="4471c-179">Questi messaggi possono contenere dati sensibili dell'applicazione e pertanto non devono essere abilitati in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="4471c-179">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="4471c-180">*Disattivato per impostazione predefinita.*</span><span class="sxs-lookup"><span data-stu-id="4471c-180">*Disabled by default.*</span></span> <span data-ttu-id="4471c-181">Esempio: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="4471c-181">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="4471c-182">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="4471c-182">Debug = 1</span></span>

  <span data-ttu-id="4471c-183">Per informazioni con utilità a breve termine durante lo sviluppo e il debug.</span><span class="sxs-lookup"><span data-stu-id="4471c-183">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="4471c-184">Esempio: `Entering method Configure with flag set to true.` In genere non si abilitano i livelli di registrazione `Debug` nell'ambiente di produzione, a meno che non occorra risolvere problemi, per l'elevato volume delle registrazioni.</span><span class="sxs-lookup"><span data-stu-id="4471c-184">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="4471c-185">Information = 2</span><span class="sxs-lookup"><span data-stu-id="4471c-185">Information = 2</span></span>

  <span data-ttu-id="4471c-186">Per tenere traccia del flusso generale dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4471c-186">For tracking the general flow of the application.</span></span> <span data-ttu-id="4471c-187">Queste registrazioni hanno in genere un valore a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="4471c-187">These logs typically have some long-term value.</span></span> <span data-ttu-id="4471c-188">Esempio: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="4471c-188">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="4471c-189">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="4471c-189">Warning = 3</span></span>

  <span data-ttu-id="4471c-190">Per gli eventi imprevisti o anomali del flusso dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4471c-190">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="4471c-191">Potrebbero includere errori o altre condizioni che non provocano l'arresto dell'applicazione, ma che potrebbe essere necessario analizzare.</span><span class="sxs-lookup"><span data-stu-id="4471c-191">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="4471c-192">Le eccezioni gestite sono una situazione comune in cui usare il livello di registrazione `Warning`.</span><span class="sxs-lookup"><span data-stu-id="4471c-192">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="4471c-193">Esempio: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="4471c-193">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="4471c-194">Error = 4</span><span class="sxs-lookup"><span data-stu-id="4471c-194">Error = 4</span></span>

  <span data-ttu-id="4471c-195">Per errori ed eccezioni che non possono essere gestiti.</span><span class="sxs-lookup"><span data-stu-id="4471c-195">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="4471c-196">Questi messaggi indicano un errore nell'operazione (ad esempio la richiesta HTTP corrente) o nell'attività corrente, non un errore a livello di applicazione.</span><span class="sxs-lookup"><span data-stu-id="4471c-196">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="4471c-197">Messaggio di registrazione di esempio:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="4471c-197">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="4471c-198">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="4471c-198">Critical = 5</span></span>

  <span data-ttu-id="4471c-199">Per gli errori che richiedono attenzione immediata.</span><span class="sxs-lookup"><span data-stu-id="4471c-199">For failures that require immediate attention.</span></span> <span data-ttu-id="4471c-200">Esempi: scenari di perdita di dati, spazio su disco insufficiente.</span><span class="sxs-lookup"><span data-stu-id="4471c-200">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="4471c-201">È possibile usare il livello di registrazione per controllare la quantità di output scritto su un supporto di archiviazione specifico o in una finestra.</span><span class="sxs-lookup"><span data-stu-id="4471c-201">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="4471c-202">Ad esempio, nell'ambiente di produzione è consigliabile che tutti i log di livello `Information` e inferiore passino a un archivio dati di volume e che tutti i log di livello `Warning` e superiore passino a un archivio dati di valori.</span><span class="sxs-lookup"><span data-stu-id="4471c-202">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="4471c-203">Durante lo sviluppo, in genere è possibile inviare i log di livello `Warning` o con gravità superiore alla console.</span><span class="sxs-lookup"><span data-stu-id="4471c-203">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="4471c-204">Per la risoluzione dei problemi, è possibile aggiungere il livello `Debug`.</span><span class="sxs-lookup"><span data-stu-id="4471c-204">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="4471c-205">La sezione [Filtro dei log](#log-filtering) più avanti in questo articolo descrive come controllare quali livelli di registrazione gestisce un provider.</span><span class="sxs-lookup"><span data-stu-id="4471c-205">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="4471c-206">Il framework di ASP.NET Core scrive log di livello `Debug` per gli eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="4471c-206">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="4471c-207">Gli esempi visti in precedenza in questo articolo escludono i log al di sotto del livello `Information`, quindi non compare alcun log di livello `Debug`.</span><span class="sxs-lookup"><span data-stu-id="4471c-207">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="4471c-208">Ecco un esempio di log della console se si esegue l'applicazione di esempio configurata per visualizzare i log di livello `Debug` e superiore per il provider della console.</span><span class="sxs-lookup"><span data-stu-id="4471c-208">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="4471c-209">ID evento di registrazione</span><span class="sxs-lookup"><span data-stu-id="4471c-209">Log event ID</span></span>

<span data-ttu-id="4471c-210">Ogni volta che si scrive un log, è possibile specificare il relativo *ID evento*.</span><span class="sxs-lookup"><span data-stu-id="4471c-210">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="4471c-211">L'app di esempio esegue questa operazione tramite una classe `LoggingEvents` definita a livello locale:</span><span class="sxs-lookup"><span data-stu-id="4471c-211">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="4471c-212">Un ID evento è un valore intero che può essere usato per associare tra loro un set di eventi registrati.</span><span class="sxs-lookup"><span data-stu-id="4471c-212">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="4471c-213">Ad esempio, la registrazione dell'aggiunta di un elemento al carrello potrebbe avere ID evento 1000 e la registrazione per il completamento di un acquisto potrebbe avere ID evento 1001.</span><span class="sxs-lookup"><span data-stu-id="4471c-213">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="4471c-214">Nella registrazione dell'output, l'ID evento può essere archiviato in un campo o incluso nel messaggio di testo, a seconda del provider.</span><span class="sxs-lookup"><span data-stu-id="4471c-214">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="4471c-215">Il provider Debug non visualizza gli ID evento, mentre il provider Console li visualizza tra parentesi quadre dopo la categoria:</span><span class="sxs-lookup"><span data-stu-id="4471c-215">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="4471c-216">Modello di messaggio di registrazione</span><span class="sxs-lookup"><span data-stu-id="4471c-216">Log message template</span></span>

<span data-ttu-id="4471c-217">Ogni volta che si scrive un messaggio di registrazione, si fornisce un modello di messaggio.</span><span class="sxs-lookup"><span data-stu-id="4471c-217">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="4471c-218">Il modello di messaggio può essere una stringa o può contenere segnaposto denominati in cui vengono inseriti i valori degli argomenti.</span><span class="sxs-lookup"><span data-stu-id="4471c-218">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="4471c-219">Il modello non è una stringa formattata e i segnaposto devono essere denominati, non numerati.</span><span class="sxs-lookup"><span data-stu-id="4471c-219">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="4471c-220">L'ordine dei segnaposto, non il loro nome, determina i parametri da usare per fornire i valori corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="4471c-220">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="4471c-221">Se si ha il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4471c-221">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="4471c-222">Il messaggio di registrazione risultante è simile a:</span><span class="sxs-lookup"><span data-stu-id="4471c-222">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="4471c-223">Il framework di registrazione formatta i messaggi in questo modo per consentire ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="4471c-223">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="4471c-224">Poiché gli argomenti stessi vengono passati al sistema di registrazione, non solo il modello di messaggio formattato, i provider di registrazione possono archiviare i valori dei parametri come campi oltre al modello di messaggio.</span><span class="sxs-lookup"><span data-stu-id="4471c-224">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="4471c-225">Se si indirizza l'output del log all'archiviazione tabelle di Azure e il metodo logger ha l'aspetto seguente:</span><span class="sxs-lookup"><span data-stu-id="4471c-225">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="4471c-226">Ogni entità della tabella di Azure può avere le proprietà `ID` e `RequestTime`, il che semplifica le query sui dati del log.</span><span class="sxs-lookup"><span data-stu-id="4471c-226">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="4471c-227">È possibile trovare tutti i log entro un determinato intervallo `RequestTime` senza che sia necessario analizzare il tempo fuori dal messaggio di testo.</span><span class="sxs-lookup"><span data-stu-id="4471c-227">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="4471c-228">Registrazione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="4471c-228">Logging exceptions</span></span>

<span data-ttu-id="4471c-229">I metodi logger dispongono di overload che consentono di passare un'eccezione, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="4471c-229">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="4471c-230">Provider diversi gestiscono le informazioni sulle eccezioni in modi diversi.</span><span class="sxs-lookup"><span data-stu-id="4471c-230">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="4471c-231">Ecco un esempio di output del provider Debug dal codice sopra riportato.</span><span class="sxs-lookup"><span data-stu-id="4471c-231">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="4471c-232">Filtro dei log</span><span class="sxs-lookup"><span data-stu-id="4471c-232">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4471c-233">È possibile specificare un livello di registrazione minimo per un provider e una categoria specifici oppure per tutti i provider o tutte le categorie.</span><span class="sxs-lookup"><span data-stu-id="4471c-233">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="4471c-234">Tutti i log sotto il livello minimo non sono passati al provider, quindi non vengono visualizzati o archiviati.</span><span class="sxs-lookup"><span data-stu-id="4471c-234">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="4471c-235">Se si vuole eliminare tutti i log, è possibile specificare `LogLevel.None` come livello di registrazione minimo.</span><span class="sxs-lookup"><span data-stu-id="4471c-235">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="4471c-236">Il valore intero di `LogLevel.None` è 6, maggiore di `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="4471c-236">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="4471c-237">**Creare regole di filtro nella configurazione**</span><span class="sxs-lookup"><span data-stu-id="4471c-237">**Create filter rules in configuration**</span></span>

<span data-ttu-id="4471c-238">I modelli di progetto creano codice che chiama `CreateDefaultBuilder` per impostare la registrazione per i provider Console e Debug.</span><span class="sxs-lookup"><span data-stu-id="4471c-238">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="4471c-239">Il metodo `CreateDefaultBuilder` imposta inoltre la registrazione per la ricerca della configurazione di una sezione `Logging` tramite codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4471c-239">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="4471c-240">I dati di configurazione specificano i livelli di registrazione minimi in base al provider e alla categoria, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="4471c-240">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="4471c-241">Questo codice JSON crea sei regole di filtro, una per il provider Debug, quattro per il provider Console e una che si applica a tutti i provider.</span><span class="sxs-lookup"><span data-stu-id="4471c-241">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="4471c-242">Più avanti si vedrà come si sceglie una sola di queste regole per ogni provider quando si crea un oggetto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="4471c-242">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="4471c-243">**Regole di filtro nel codice**</span><span class="sxs-lookup"><span data-stu-id="4471c-243">**Filter rules in code**</span></span>

<span data-ttu-id="4471c-244">È possibile registrare le regole di filtro nel codice, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="4471c-244">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="4471c-245">Il secondo `AddFilter` specifica il provider Debug tramite il nome del tipo.</span><span class="sxs-lookup"><span data-stu-id="4471c-245">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="4471c-246">Il primo `AddFilter` si applica a tutti i provider in quanto non consente di specificare un tipo di provider.</span><span class="sxs-lookup"><span data-stu-id="4471c-246">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="4471c-247">**Applicazione delle regole di filtro**</span><span class="sxs-lookup"><span data-stu-id="4471c-247">**How filtering rules are applied**</span></span>

<span data-ttu-id="4471c-248">I dati di configurazione e il codice `AddFilter` illustrato negli esempi precedenti creano le regole indicate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="4471c-248">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="4471c-249">I primi sei provengono dall'esempio di configurazione e gli ultimi due dall'esempio di codice.</span><span class="sxs-lookup"><span data-stu-id="4471c-249">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="4471c-250">Number</span><span class="sxs-lookup"><span data-stu-id="4471c-250">Number</span></span> | <span data-ttu-id="4471c-251">Provider</span><span class="sxs-lookup"><span data-stu-id="4471c-251">Provider</span></span>      | <span data-ttu-id="4471c-252">Categorie che iniziano con...</span><span class="sxs-lookup"><span data-stu-id="4471c-252">Categories that begin with ...</span></span>          | <span data-ttu-id="4471c-253">Livello di registrazione minimo</span><span class="sxs-lookup"><span data-stu-id="4471c-253">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="4471c-254">1</span><span class="sxs-lookup"><span data-stu-id="4471c-254">1</span></span>      | <span data-ttu-id="4471c-255">Debug</span><span class="sxs-lookup"><span data-stu-id="4471c-255">Debug</span></span>         | <span data-ttu-id="4471c-256">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="4471c-256">All categories</span></span>                          | <span data-ttu-id="4471c-257">Informazioni</span><span class="sxs-lookup"><span data-stu-id="4471c-257">Information</span></span>       |
| <span data-ttu-id="4471c-258">2</span><span class="sxs-lookup"><span data-stu-id="4471c-258">2</span></span>      | <span data-ttu-id="4471c-259">Console</span><span class="sxs-lookup"><span data-stu-id="4471c-259">Console</span></span>       | <span data-ttu-id="4471c-260">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="4471c-260">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="4471c-261">Avviso</span><span class="sxs-lookup"><span data-stu-id="4471c-261">Warning</span></span>           |
| <span data-ttu-id="4471c-262">3</span><span class="sxs-lookup"><span data-stu-id="4471c-262">3</span></span>      | <span data-ttu-id="4471c-263">Console</span><span class="sxs-lookup"><span data-stu-id="4471c-263">Console</span></span>       | <span data-ttu-id="4471c-264">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="4471c-264">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="4471c-265">Debug</span><span class="sxs-lookup"><span data-stu-id="4471c-265">Debug</span></span>             |
| <span data-ttu-id="4471c-266">4</span><span class="sxs-lookup"><span data-stu-id="4471c-266">4</span></span>      | <span data-ttu-id="4471c-267">Console</span><span class="sxs-lookup"><span data-stu-id="4471c-267">Console</span></span>       | <span data-ttu-id="4471c-268">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="4471c-268">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="4471c-269">Error</span><span class="sxs-lookup"><span data-stu-id="4471c-269">Error</span></span>             |
| <span data-ttu-id="4471c-270">5</span><span class="sxs-lookup"><span data-stu-id="4471c-270">5</span></span>      | <span data-ttu-id="4471c-271">Console</span><span class="sxs-lookup"><span data-stu-id="4471c-271">Console</span></span>       | <span data-ttu-id="4471c-272">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="4471c-272">All categories</span></span>                          | <span data-ttu-id="4471c-273">Informazioni</span><span class="sxs-lookup"><span data-stu-id="4471c-273">Information</span></span>       |
| <span data-ttu-id="4471c-274">6</span><span class="sxs-lookup"><span data-stu-id="4471c-274">6</span></span>      | <span data-ttu-id="4471c-275">Tutti i provider</span><span class="sxs-lookup"><span data-stu-id="4471c-275">All providers</span></span> | <span data-ttu-id="4471c-276">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="4471c-276">All categories</span></span>                          | <span data-ttu-id="4471c-277">Debug</span><span class="sxs-lookup"><span data-stu-id="4471c-277">Debug</span></span>             |
| <span data-ttu-id="4471c-278">7</span><span class="sxs-lookup"><span data-stu-id="4471c-278">7</span></span>      | <span data-ttu-id="4471c-279">Tutti i provider</span><span class="sxs-lookup"><span data-stu-id="4471c-279">All providers</span></span> | <span data-ttu-id="4471c-280">System</span><span class="sxs-lookup"><span data-stu-id="4471c-280">System</span></span>                                  | <span data-ttu-id="4471c-281">Debug</span><span class="sxs-lookup"><span data-stu-id="4471c-281">Debug</span></span>             |
| <span data-ttu-id="4471c-282">8</span><span class="sxs-lookup"><span data-stu-id="4471c-282">8</span></span>      | <span data-ttu-id="4471c-283">Debug</span><span class="sxs-lookup"><span data-stu-id="4471c-283">Debug</span></span>         | <span data-ttu-id="4471c-284">Microsoft</span><span class="sxs-lookup"><span data-stu-id="4471c-284">Microsoft</span></span>                               | <span data-ttu-id="4471c-285">Traccia</span><span class="sxs-lookup"><span data-stu-id="4471c-285">Trace</span></span>             |

<span data-ttu-id="4471c-286">Quando si crea un oggetto `ILogger` con cui scrivere i log, l'oggetto `ILoggerFactory` seleziona una singola regola per ogni provider da applicare al logger.</span><span class="sxs-lookup"><span data-stu-id="4471c-286">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="4471c-287">Tutti i messaggi scritti da tale oggetto `ILogger` sono filtrati in base alle regole selezionate.</span><span class="sxs-lookup"><span data-stu-id="4471c-287">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="4471c-288">Tra le regole disponibili viene selezionata la regola più specifica possibile per ogni coppia di categoria e provider.</span><span class="sxs-lookup"><span data-stu-id="4471c-288">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="4471c-289">L'algoritmo seguente viene usato per ogni provider quando viene creato un `ILogger` per una determinata categoria:</span><span class="sxs-lookup"><span data-stu-id="4471c-289">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="4471c-290">Selezionare tutte le regole corrispondenti al provider o al relativo alias.</span><span class="sxs-lookup"><span data-stu-id="4471c-290">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="4471c-291">Se non ne vengono rilevate, selezionare tutte le regole con un provider vuoto.</span><span class="sxs-lookup"><span data-stu-id="4471c-291">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="4471c-292">Dal risultato del passaggio precedente, selezionare le regole con il prefisso di categoria corrispondente più lungo.</span><span class="sxs-lookup"><span data-stu-id="4471c-292">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="4471c-293">Se non vengono rilevate, selezionare tutte le regole che non specificano una categoria.</span><span class="sxs-lookup"><span data-stu-id="4471c-293">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="4471c-294">Se sono selezionate più regole, scegliere l'**ultima**.</span><span class="sxs-lookup"><span data-stu-id="4471c-294">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="4471c-295">Se non è selezionata alcuna regola, usare `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="4471c-295">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="4471c-296">Si supponga ad esempio di avere l'elenco di regole precedente e di creare un oggetto `ILogger` per la categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="4471c-296">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="4471c-297">Per il provider Debug si applicano le regole 1, 6 e 8.</span><span class="sxs-lookup"><span data-stu-id="4471c-297">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="4471c-298">La regola 8 è più specifica, così sarà quella selezionata.</span><span class="sxs-lookup"><span data-stu-id="4471c-298">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="4471c-299">Per il provider Console si applicano le regole 3, 4, 5 e 6.</span><span class="sxs-lookup"><span data-stu-id="4471c-299">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="4471c-300">La regola 3 è la più specifica.</span><span class="sxs-lookup"><span data-stu-id="4471c-300">Rule 3 is most specific.</span></span>

<span data-ttu-id="4471c-301">Quando si creano log con un `ILogger` per la categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", i log di livello `Trace` e superiore andranno al provider Debug e i log di livello `Debug` e superiore andranno al provider Console.</span><span class="sxs-lookup"><span data-stu-id="4471c-301">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="4471c-302">**Alias dei provider**</span><span class="sxs-lookup"><span data-stu-id="4471c-302">**Provider aliases**</span></span>

<span data-ttu-id="4471c-303">È possibile usare il nome del tipo per specificare un provider nella configurazione, ma ogni provider definisce un *alias* più breve che è più facile da usare.</span><span class="sxs-lookup"><span data-stu-id="4471c-303">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="4471c-304">Per i provider predefiniti, usare gli alias seguenti:</span><span class="sxs-lookup"><span data-stu-id="4471c-304">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="4471c-305">Console</span><span class="sxs-lookup"><span data-stu-id="4471c-305">Console</span></span>
- <span data-ttu-id="4471c-306">Debug</span><span class="sxs-lookup"><span data-stu-id="4471c-306">Debug</span></span>
- <span data-ttu-id="4471c-307">EventLog</span><span class="sxs-lookup"><span data-stu-id="4471c-307">EventLog</span></span>
- <span data-ttu-id="4471c-308">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="4471c-308">AzureAppServices</span></span>
- <span data-ttu-id="4471c-309">TraceSource</span><span class="sxs-lookup"><span data-stu-id="4471c-309">TraceSource</span></span>
- <span data-ttu-id="4471c-310">EventSource</span><span class="sxs-lookup"><span data-stu-id="4471c-310">EventSource</span></span>

<span data-ttu-id="4471c-311">**Livello minimo predefinito**</span><span class="sxs-lookup"><span data-stu-id="4471c-311">**Default minimum level**</span></span>

<span data-ttu-id="4471c-312">Esiste un'impostazione del livello minimo che diventa effettiva solo se non si applica alcuna regola di configurazione o codice per un provider e una categoria specifici.</span><span class="sxs-lookup"><span data-stu-id="4471c-312">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="4471c-313">L'esempio seguente illustra come impostare il livello minimo:</span><span class="sxs-lookup"><span data-stu-id="4471c-313">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="4471c-314">Se non si imposta in modo esplicito il livello minimo, il valore predefinito è `Information`, che indica che i log `Trace` e `Debug` vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="4471c-314">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="4471c-315">**Funzioni di filtro**</span><span class="sxs-lookup"><span data-stu-id="4471c-315">**Filter functions**</span></span>

<span data-ttu-id="4471c-316">È possibile scrivere codice in una funzione di filtro per applicare le regole di filtro.</span><span class="sxs-lookup"><span data-stu-id="4471c-316">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="4471c-317">Una funzione di filtro viene richiamata per tutti i provider e le categorie a cui non sono assegnate regole tramite la configurazione o il codice.</span><span class="sxs-lookup"><span data-stu-id="4471c-317">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="4471c-318">Il codice della funzione ha accesso al tipo di provider, alla categoria e al livello di registrazione per stabilire se un messaggio deve essere registrato.</span><span class="sxs-lookup"><span data-stu-id="4471c-318">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="4471c-319">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4471c-319">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4471c-320">Alcuni provider di registrazione consentono di specificare quando i log devono essere scritti in un supporto di archiviazione oppure ignorati in base al livello di registrazione e alla categoria.</span><span class="sxs-lookup"><span data-stu-id="4471c-320">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="4471c-321">I metodi di estensione `AddConsole` e `AddDebug` forniscono overload che consentono di passare criteri di filtro.</span><span class="sxs-lookup"><span data-stu-id="4471c-321">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="4471c-322">L'esempio di codice seguente fa sì che il provider Console ignori i log di livello inferiore a `Warning`, mentre il provider Debug ignora i log creati dal framework.</span><span class="sxs-lookup"><span data-stu-id="4471c-322">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="4471c-323">Il metodo `AddEventLog` ha un overload che accetta un'istanza di `EventLogSettings`, che può contenere una funzione di filtro nella proprietà `Filter`.</span><span class="sxs-lookup"><span data-stu-id="4471c-323">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="4471c-324">Il provider TraceSource non fornisce alcun overload di questo tipo, in quanto il suo livello di registrazione e altri parametri si basano sugli oggetti `SourceSwitch` e `TraceListener` in uso.</span><span class="sxs-lookup"><span data-stu-id="4471c-324">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="4471c-325">È possibile impostare le regole di filtro per tutti i provider registrati con un'istanza di `ILoggerFactory` tramite il metodo di estensione `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="4471c-325">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="4471c-326">L'esempio seguente limita i log del framework (la categoria inizia con "Microsoft" o "System") agli avvisi consentendo all'app di registrare a livello debug.</span><span class="sxs-lookup"><span data-stu-id="4471c-326">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="4471c-327">Per usare i filtri per impedire la scrittura di tutti i log per una determinata categoria, è possibile specificare `LogLevel.None` come il livello di registrazione minimo per tale categoria.</span><span class="sxs-lookup"><span data-stu-id="4471c-327">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="4471c-328">Il valore intero di `LogLevel.None` è 6, maggiore di `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="4471c-328">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="4471c-329">Il metodo di estensione `WithFilter` viene fornito dal pacchetto NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter).</span><span class="sxs-lookup"><span data-stu-id="4471c-329">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="4471c-330">Il metodo restituisce una nuova istanza di `ILoggerFactory` che filtra i messaggi di registrazione passati a tutti i provider di logger registrati con esso.</span><span class="sxs-lookup"><span data-stu-id="4471c-330">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="4471c-331">Non si applica ad altre istanze di `ILoggerFactory`, inclusa l'istanza originale di `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="4471c-331">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="4471c-332">Ambiti dei log</span><span class="sxs-lookup"><span data-stu-id="4471c-332">Log scopes</span></span>

<span data-ttu-id="4471c-333">È possibile raggruppare un set di operazioni logiche all'interno di un *ambito* per collegare gli stessi dati a ogni log creato come parte del set.</span><span class="sxs-lookup"><span data-stu-id="4471c-333">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="4471c-334">È ad esempio possibile fare in modo che ogni log creato come parte dell'elaborazione di una transazione includa l'ID della transazione.</span><span class="sxs-lookup"><span data-stu-id="4471c-334">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="4471c-335">Un ambito è un tipo `IDisposable` restituito dal metodo [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) e viene mantenuto fino all'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="4471c-335">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="4471c-336">Un ambito si usa mediante il wrapping delle chiamate del logger in un blocco `using`, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4471c-336">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="4471c-337">Il codice seguente abilita gli ambiti per il provider Console:</span><span class="sxs-lookup"><span data-stu-id="4471c-337">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="4471c-338">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4471c-338">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="4471c-339">La configurazione dell'opzione del logger della console `IncludeScopes` è necessaria per abilitare la registrazione basata sull'ambito.</span><span class="sxs-lookup"><span data-stu-id="4471c-339">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="4471c-340">`IncludeScopes` può essere configurata tramite i file di configurazione *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="4471c-340">`IncludeScopes` can be configured via *appsettings* configuration files.</span></span> <span data-ttu-id="4471c-341">Per altre informazioni, vedere la sezione [Configurazione del file di impostazioni](#settings-file-configuration).</span><span class="sxs-lookup"><span data-stu-id="4471c-341">For more information, see the [Settings file configuration](#settings-file-configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4471c-342">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4471c-342">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="4471c-343">La configurazione dell'opzione del logger della console `IncludeScopes` è necessaria per abilitare la registrazione basata sull'ambito.</span><span class="sxs-lookup"><span data-stu-id="4471c-343">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4471c-344">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4471c-344">*Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="4471c-345">Ogni messaggio di registrazione include le informazioni sull'ambito:</span><span class="sxs-lookup"><span data-stu-id="4471c-345">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="4471c-346">Provider di registrazione predefiniti</span><span class="sxs-lookup"><span data-stu-id="4471c-346">Built-in logging providers</span></span>

<span data-ttu-id="4471c-347">ASP.NET Core include i provider seguenti:</span><span class="sxs-lookup"><span data-stu-id="4471c-347">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="4471c-348">Console</span><span class="sxs-lookup"><span data-stu-id="4471c-348">Console</span></span>](#console-provider)
* [<span data-ttu-id="4471c-349">Debug</span><span class="sxs-lookup"><span data-stu-id="4471c-349">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="4471c-350">EventSource</span><span class="sxs-lookup"><span data-stu-id="4471c-350">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="4471c-351">EventLog</span><span class="sxs-lookup"><span data-stu-id="4471c-351">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="4471c-352">TraceSource</span><span class="sxs-lookup"><span data-stu-id="4471c-352">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="4471c-353">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="4471c-353">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="4471c-354">Provider Console</span><span class="sxs-lookup"><span data-stu-id="4471c-354">Console provider</span></span>

<span data-ttu-id="4471c-355">Il pacchetto del provider [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) invia l'output della registrazione alla console.</span><span class="sxs-lookup"><span data-stu-id="4471c-355">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"


```csharp
logging.AddConsole()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="4471c-356">[Gli overload di AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) consentono di passare un livello di registrazione minimo, una funzione di filtro e un valore booleano che indica se gli ambiti sono supportati.</span><span class="sxs-lookup"><span data-stu-id="4471c-356">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="4471c-357">Un'altra opzione consiste nel passare un oggetto `IConfiguration`, che può specificare livelli di registrazione e il supporto di ambiti.</span><span class="sxs-lookup"><span data-stu-id="4471c-357">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="4471c-358">Se si intende usare il provider di console nell'ambiente di produzione, tenere presente che ha un impatto significativo sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="4471c-358">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="4471c-359">Quando si crea un nuovo progetto in Visual Studio, il metodo `AddConsole` è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4471c-359">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="4471c-360">Questo codice fa riferimento alla sezione `Logging` del file *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="4471c-360">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="4471c-361">Le impostazioni visualizzate limitano i log del framework agli avvisi, consentendo all'app di registrare a livello di debug, come descritto nella sezione [Filtri dei log](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="4471c-361">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="4471c-362">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="4471c-362">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="4471c-363">Provider Debug</span><span class="sxs-lookup"><span data-stu-id="4471c-363">Debug provider</span></span>

<span data-ttu-id="4471c-364">Il pacchetto del provider [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) scrive l'output della registrazione usando la classe [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (chiamate del metodo `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="4471c-364">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="4471c-365">In Linux, questo provider scrive i log in */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="4471c-365">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="4471c-366">Gli [overload di AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) consentono passare un livello di registrazione minimo o una funzione di filtro.</span><span class="sxs-lookup"><span data-stu-id="4471c-366">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="4471c-367">Provider EventSource</span><span class="sxs-lookup"><span data-stu-id="4471c-367">EventSource provider</span></span>

<span data-ttu-id="4471c-368">Per le app destinate a ASP.NET Core 1.1.0 o versione successiva, il pacchetto di provider [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) può implementare la traccia degli eventi.</span><span class="sxs-lookup"><span data-stu-id="4471c-368">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="4471c-369">In Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="4471c-369">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="4471c-370">Il provider è multipiattaforma, ma non sono ancora disponibili gli strumenti di raccolta e visualizzazione degli eventi per Linux o macOS.</span><span class="sxs-lookup"><span data-stu-id="4471c-370">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger()
```

::: moniker-end

<span data-ttu-id="4471c-371">Un buon metodo per raccogliere e visualizzare i log consiste nell'usare l'[utilità PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="4471c-371">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="4471c-372">Sono disponibili altri strumenti per la visualizzazione dei log ETW, ma PerfView fornisce un'esperienza ottimale per l'uso con gli eventi ETW generati da ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4471c-372">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="4471c-373">Per configurare PerfView per la raccolta degli eventi registrati da questo provider, aggiungere la stringa `*Microsoft-Extensions-Logging` nell'elenco **Provider aggiuntivi**</span><span class="sxs-lookup"><span data-stu-id="4471c-373">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="4471c-374">(non dimenticare l'asterisco all'inizio della stringa).</span><span class="sxs-lookup"><span data-stu-id="4471c-374">(Don't miss the asterisk at the start of the string.)</span></span>

![Provider aggiuntivi di PerfView](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="4471c-376">Provider EventLog di Windows</span><span class="sxs-lookup"><span data-stu-id="4471c-376">Windows EventLog provider</span></span>

<span data-ttu-id="4471c-377">Il pacchetto di provider [Microsoft.Extensions.Logging.AventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) invia l'output del log al registro eventi di Windows.</span><span class="sxs-lookup"><span data-stu-id="4471c-377">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="4471c-378">Gli [overload di AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) consentono di passare `EventLogSettings` o un livello di registrazione minimo.</span><span class="sxs-lookup"><span data-stu-id="4471c-378">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="4471c-379">Provider TraceSource</span><span class="sxs-lookup"><span data-stu-id="4471c-379">TraceSource provider</span></span>

<span data-ttu-id="4471c-380">Il pacchetto di provider [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa le librerie e i provider di [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="4471c-380">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="4471c-381">Gli [overload di AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) consentono di passare un cambio di origine e un listener di traccia.</span><span class="sxs-lookup"><span data-stu-id="4471c-381">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="4471c-382">Per usare questo provider, un'applicazione deve essere eseguita su di .NET Framework (anziché su .NET Core).</span><span class="sxs-lookup"><span data-stu-id="4471c-382">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="4471c-383">Il provider consente di instradare i messaggi a vari [listener](/dotnet/framework/debug-trace-profile/trace-listeners), come il listener [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) usato nell'applicazione di esempio.</span><span class="sxs-lookup"><span data-stu-id="4471c-383">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="4471c-384">L'esempio seguente configura un provider `TraceSource` che registra messaggi `Warning` e di livello superiore nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="4471c-384">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="4471c-385">Provider del Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="4471c-385">Azure App Service provider</span></span>

<span data-ttu-id="4471c-386">Il pacchetto di provider [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) scrive i log in file di testo nel file system di un'app del Servizio app di Azure e nell'[archivio di BLOB](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in un account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="4471c-386">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="4471c-387">Il provider è disponibile solo per le app destinate ad ASP.NET Core 1.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="4471c-387">The provider is available only for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4471c-388">Se la destinazione è .NET Core, non è necessario installare il pacchetto di provider o chiamare [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="4471c-388">If targeting .NET Core, don't install the provider package or explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="4471c-389">Il provider è automaticamente disponibile per l'app quando l'app viene distribuita nel Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="4471c-389">The provider is automatically available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="4471c-390">Se la destinazione è .NET Framework, aggiungere il pacchetto di provider al progetto e richiamare `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="4471c-390">If targeting .NET Framework, add the provider package to the project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="4471c-391">Un overload di [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) consente di passare [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) con cui è possibile eseguire l'override delle impostazioni predefinite, ad esempio il modello di output del log, il nome di BLOB e il limite delle dimensioni del file.</span><span class="sxs-lookup"><span data-stu-id="4471c-391">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="4471c-392">(Il *modello di output* è un modello di messaggio che viene applicato a tutti i log oltre quello specificato dall'utente quando si chiama un metodo `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="4471c-392">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

::: moniker-end

<span data-ttu-id="4471c-393">Quando si distribuisce un'app del Servizio app, l'app rispetta le impostazioni della sezione dei [log di diagnostica](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) dalla pagina del **Servizio app** del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="4471c-393">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="4471c-394">Quando queste impostazioni vengono aggiornate, le modifiche hanno effetto immediato senza richiedere un riavvio o la ridistribuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4471c-394">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Impostazioni di registrazione di Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="4471c-396">Il percorso predefinito per i file di log è nella cartella *D:\\home\\LogFiles\\Application* e il nome di file predefinito è *diagnostics-aaaammgg.txt*.</span><span class="sxs-lookup"><span data-stu-id="4471c-396">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="4471c-397">Il limite predefinito per le dimensioni del file è 10 MB e il numero massimo predefinito di file conservati è 2.</span><span class="sxs-lookup"><span data-stu-id="4471c-397">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="4471c-398">Il nome BLOB predefinito è *{nome-app}{timestamp}/aaaa/mm/gg/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="4471c-398">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="4471c-399">Per altre informazioni sul comportamento predefinito, vedere [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="4471c-399">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="4471c-400">Il provider funziona solo quando il progetto viene eseguito nell'ambiente di Azure.</span><span class="sxs-lookup"><span data-stu-id="4471c-400">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="4471c-401">Non ha alcun effetto quando il progetto viene eseguito localmente, in quanto non scrive nulla in file locali o in archivi di sviluppo locali per i BLOB.</span><span class="sxs-lookup"><span data-stu-id="4471c-401">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="4471c-402">Provider di registrazione di terze parti</span><span class="sxs-lookup"><span data-stu-id="4471c-402">Third-party logging providers</span></span>

<span data-ttu-id="4471c-403">Framework di registrazione di terze parti che usano ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4471c-403">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="4471c-404">[elmah.io](https://elmah.io/) ([repository GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="4471c-404">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="4471c-405">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repository GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="4471c-405">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="4471c-406">[JSNLog](http://jsnlog.com/) ([repository GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="4471c-406">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="4471c-407">[Loggr](http://loggr.net/) ([repository GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="4471c-407">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="4471c-408">[NLog](http://nlog-project.org/) ([repository GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="4471c-408">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="4471c-409">[Serilog](https://serilog.net/) ([repository GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="4471c-409">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="4471c-410">Alcuni framework di terze parti possono eseguire la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="4471c-410">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="4471c-411">L'uso di un framework di terze parti è simile a quello di uno dei provider predefiniti:</span><span class="sxs-lookup"><span data-stu-id="4471c-411">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="4471c-412">Aggiungere un pacchetto NuGet al progetto.</span><span class="sxs-lookup"><span data-stu-id="4471c-412">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="4471c-413">Chiamare un metodo di estensione in `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="4471c-413">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="4471c-414">Per altre informazioni, vedere la documentazione di ciascun framework.</span><span class="sxs-lookup"><span data-stu-id="4471c-414">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="4471c-415">Flusso di registrazione di Azure</span><span class="sxs-lookup"><span data-stu-id="4471c-415">Azure log streaming</span></span>

<span data-ttu-id="4471c-416">Il flusso di registrazione di Azure consente di visualizzare l'attività di registrazione in tempo reale da:</span><span class="sxs-lookup"><span data-stu-id="4471c-416">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="4471c-417">Server applicazioni</span><span class="sxs-lookup"><span data-stu-id="4471c-417">The application server</span></span>
* <span data-ttu-id="4471c-418">Server Web</span><span class="sxs-lookup"><span data-stu-id="4471c-418">The web server</span></span>
* <span data-ttu-id="4471c-419">Traccia delle richieste non riuscite</span><span class="sxs-lookup"><span data-stu-id="4471c-419">Failed request tracing</span></span>

<span data-ttu-id="4471c-420">Per configurare il flusso di registrazione di Azure:</span><span class="sxs-lookup"><span data-stu-id="4471c-420">To configure Azure log streaming:</span></span>

* <span data-ttu-id="4471c-421">Passare alla pagina **Log di diagnostica** dalla pagina del portale dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="4471c-421">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="4471c-422">Attivare **Registrazione applicazioni (file system)**.</span><span class="sxs-lookup"><span data-stu-id="4471c-422">Set **Application Logging (Filesystem)** to on.</span></span>

![Pagina dei log di diagnostica del portale di Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="4471c-424">Passare alla pagina **Flusso di registrazione** per visualizzare i messaggi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4471c-424">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="4471c-425">Vengono registrati dall'applicazione tramite l'interfaccia `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="4471c-425">They're logged by application through the `ILogger` interface.</span></span>

![Flusso di registrazione dell'applicazione nel portale di Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="4471c-427">Registrazione di traccia di Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="4471c-427">Azure Application Insights trace logging</span></span>

<span data-ttu-id="4471c-428">[Application Insights](https://azure.microsoft.com/services/application-insights/) SDK raccoglie la telemetria di traccia dei registri generati con l'infrastruttura di registrazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4471c-428">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="4471c-429">Per altre informazioni, vedere [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging) (Wiki di Microsoft/ApplicationInsights-aspnetcore: registrazione).</span><span class="sxs-lookup"><span data-stu-id="4471c-429">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4471c-430">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4471c-430">Additional resources</span></span>

[<span data-ttu-id="4471c-431">Registrazione ad alte prestazioni con LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="4471c-431">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
