---
title: Host in un servizio Windows
author: tdykstra
description: Informazioni su come pubblicare un'applicazione ASP.NET Core in un servizio Windows.
keywords: Hosting ASP.NET Core, il servizio di Windows,
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: 5b54c77ff9e019b1d550aa687923077a3e9ba5c2
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="a09e9-104">Ospitare un'applicazione ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="a09e9-104">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="a09e9-105">Da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a09e9-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a09e9-106">È il modo consigliato per ospitare un'applicazione ASP.NET Core in Windows, quando non si utilizza IIS per l'esecuzione in un [servizio Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="a09e9-106">The recommended way to host an ASP.NET Core app on Windows when you don't use IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="a09e9-107">In questo modo è possibile avviare automaticamente dopo il riavvio di arresti anomali del sistema, senza attendere che un utente per l'accesso.</span><span class="sxs-lookup"><span data-stu-id="a09e9-107">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="a09e9-108">[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) vedere il [passaggi successivi](#next-steps) per istruzioni su come eseguirlo.</span><span class="sxs-lookup"><span data-stu-id="a09e9-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a09e9-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="a09e9-109">Prerequisites</span></span>

* <span data-ttu-id="a09e9-110">L'app deve essere eseguita nel runtime di .NET framework.</span><span class="sxs-lookup"><span data-stu-id="a09e9-110">The app must run on the .NET framework runtime.</span></span>  <span data-ttu-id="a09e9-111">Nel *csproj* file, specificare i valori appropriati per [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) e [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="a09e9-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="a09e9-112">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="a09e9-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="a09e9-113">Quando si crea un progetto in Visual Studio, usare il **ASP.NET Core applicazione (.NET Framework)** modello.</span><span class="sxs-lookup"><span data-stu-id="a09e9-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="a09e9-114">Se l'applicazione riceve richieste da internet (non solo da una rete interna), è necessario utilizzare il [WebListener](xref:fundamentals/servers/weblistener) server web anziché [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="a09e9-114">If the app will get requests from the internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="a09e9-115">Kestrel deve essere utilizzata con IIS per le distribuzioni di bordo.</span><span class="sxs-lookup"><span data-stu-id="a09e9-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="a09e9-116">Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="a09e9-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="a09e9-117">Per iniziare</span><span class="sxs-lookup"><span data-stu-id="a09e9-117">Getting started</span></span>

<span data-ttu-id="a09e9-118">Questa sezione illustra le modifiche minime necessarie per configurare un progetto ASP.NET Core esistente per l'esecuzione in un servizio.</span><span class="sxs-lookup"><span data-stu-id="a09e9-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="a09e9-119">Installare il pacchetto NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="a09e9-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="a09e9-120">Apportare le modifiche seguenti in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="a09e9-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="a09e9-121">Chiamare `host.RunAsService` anziché `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="a09e9-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="a09e9-122">Se il codice chiama `UseContentRoot`, utilizzare un percorso per il percorso di pubblicazione invece di`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="a09e9-122">If your code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="a09e9-123">Pubblicare l'applicazione in una cartella.</span><span class="sxs-lookup"><span data-stu-id="a09e9-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="a09e9-124">Utilizzare [dotnet pubblicare](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) o [profilo di pubblicazione di Visual Studio](xref:publishing/web-publishing-vs) che pubblica una cartella.</span><span class="sxs-lookup"><span data-stu-id="a09e9-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:publishing/web-publishing-vs) that publishes to a folder.</span></span>

* <span data-ttu-id="a09e9-125">Verificare la creazione e avvio del servizio.</span><span class="sxs-lookup"><span data-stu-id="a09e9-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="a09e9-126">Aprire una finestra del prompt dei comandi di amministratore da utilizzare il [sc.exe](https://technet.microsoft.com/library/bb490995) strumento da riga di comando per creare e avviare un servizio.</span><span class="sxs-lookup"><span data-stu-id="a09e9-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="a09e9-127">Il nome del servizio MyService, pubblicare l'app per `c:\svc`e l'app stessa denominato AspNetCoreService, i comandi sono analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="a09e9-127">If you name your service MyService, you publish your app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  <span data-ttu-id="a09e9-128">Il `binPath` valore è il percorso file eseguibile dell'applicazione, inclusi il nome del file eseguibile stesso.</span><span class="sxs-lookup"><span data-stu-id="a09e9-128">The `binPath` value is the path to your app's executable, including the executable filename itself.</span></span>

  ![Finestra della console, creare e avviare l'esempio](windows-service/_static/create-start.png)

  <span data-ttu-id="a09e9-130">Questi comandi terminato, è possibile esplorare per lo stesso percorso quando si esegue come un'applicazione console (per impostazione predefinita, `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="a09e9-130">When these commands finish, you can browse to the same path as when you run as a console app (by default, `http://localhost:5000`)</span></span>

  ![In esecuzione in un servizio](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="a09e9-132">Fornire un modo per l'esecuzione all'esterno di un servizio</span><span class="sxs-lookup"><span data-stu-id="a09e9-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="a09e9-133">È più semplice testare ed eseguire il debug durante l'esecuzione all'esterno di un servizio, pertanto è facoltativa per aggiungere il codice che chiama `host.RunAsService` solo in determinate condizioni.</span><span class="sxs-lookup"><span data-stu-id="a09e9-133">It's easier to test and debug when you're running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="a09e9-134">Ad esempio, è possibile eseguire come un'applicazione console se viene visualizzato un `--console` argomento della riga di comando o se il debugger è collegato.</span><span class="sxs-lookup"><span data-stu-id="a09e9-134">For example, you could run as a console app if you get a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="a09e9-135">Gestire l'arresto e avvio degli eventi</span><span class="sxs-lookup"><span data-stu-id="a09e9-135">Handle stopping and starting events</span></span>

<span data-ttu-id="a09e9-136">Se si desidera gestire `OnStarting`, `OnStarted`, e `OnStopping` degli eventi, apportare le modifiche aggiuntive seguenti:</span><span class="sxs-lookup"><span data-stu-id="a09e9-136">If you want to handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="a09e9-137">Creare una classe che deriva da `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="a09e9-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="a09e9-138">Creare un metodo di estensione per `IWebHost` che passa personalizzato `WebHostService` a `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="a09e9-138">Create an extension method for `IWebHost` that passes your custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="a09e9-139">In `Program.Main` modifica chiami il nuovo metodo di estensione anziché `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="a09e9-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="a09e9-140">Se personalizzato `WebHostService` codice deve ottenere un servizio dall'inserimento di dipendenze (ad esempio un logger), è possibile ottenere dal `Services` proprietà di `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="a09e9-140">If your custom `WebHostService` code needs to get a service from dependency injection (such as a logger), you can get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="a09e9-141">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="a09e9-141">Next steps</span></span>

<span data-ttu-id="a09e9-142">Il [applicazione di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) che accompagna questo articolo è un'applicazione web MVC semplice che è stata modificata come illustrato negli esempi di codice precedente.</span><span class="sxs-lookup"><span data-stu-id="a09e9-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="a09e9-143">Per eseguirlo in un servizio, effettuare i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a09e9-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="a09e9-144">Pubblicare *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="a09e9-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="a09e9-145">Aprire una finestra di amministratore.</span><span class="sxs-lookup"><span data-stu-id="a09e9-145">Open an administrator window.</span></span>

* <span data-ttu-id="a09e9-146">Immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a09e9-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="a09e9-147">In un browser, passare alla indirizzo http://localhost:5000/ per verificare che sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="a09e9-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="a09e9-148">Se l'app non viene avviato fino come previsto durante l'esecuzione in un servizio, un modo rapido per rendere accessibili i messaggi di errore consiste nell'aggiungere un provider di log, ad esempio il [provider registro eventi di Windows](xref:fundamentals/logging#eventlog).</span><span class="sxs-lookup"><span data-stu-id="a09e9-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="a09e9-149">Riconoscimenti</span><span class="sxs-lookup"><span data-stu-id="a09e9-149">Acknowledgments</span></span>

<span data-ttu-id="a09e9-150">In questo articolo è stato scritto con l'aiuto di origini che sono stati già pubblicate.</span><span class="sxs-lookup"><span data-stu-id="a09e9-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="a09e9-151">Il primo e più utili di essi sono questi stati:</span><span class="sxs-lookup"><span data-stu-id="a09e9-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="a09e9-152">Hosting ASP.NET Core come servizio Windows</span><span class="sxs-lookup"><span data-stu-id="a09e9-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="a09e9-153">Come ospitare il ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="a09e9-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
