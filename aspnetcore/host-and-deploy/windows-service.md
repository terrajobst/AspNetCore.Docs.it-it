---
title: Host ASP.NET Core in un servizio Windows
author: tdykstra
description: Informazioni su come pubblicare un'applicazione ASP.NET Core in un servizio Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: b0b27f274de1ca88b20bf582127132527b553ce0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="e3c20-103">Host ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="e3c20-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="e3c20-104">[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e3c20-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="e3c20-105">Il metodo consigliato per ospitare un'applicazione ASP.NET Core in Windows senza utilizza IIS, è possibile eseguirlo in un [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="e3c20-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="e3c20-106">Quando è ospitato come servizio Windows, l'app è possibile eseguire automaticamente inizio dopo aver riavviato e si blocca senza intervento umano.</span><span class="sxs-lookup"><span data-stu-id="e3c20-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="e3c20-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e3c20-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="e3c20-108">Per istruzioni su come eseguire l'app di esempio, vedere l'esempio *README.md* file.</span><span class="sxs-lookup"><span data-stu-id="e3c20-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3c20-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e3c20-109">Prerequisites</span></span>

* <span data-ttu-id="e3c20-110">L'app deve essere eseguita nel runtime di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e3c20-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="e3c20-111">Nel *csproj* file, specificare i valori appropriati per [TargetFramework](/nuget/schema/target-frameworks) e [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="e3c20-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="e3c20-112">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="e3c20-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="e3c20-113">Quando si crea un progetto in Visual Studio, usare il **ASP.NET Core applicazione (.NET Framework)** modello.</span><span class="sxs-lookup"><span data-stu-id="e3c20-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="e3c20-114">Se l'applicazione riceve richieste da Internet (non solo da una rete interna), è necessario utilizzare il [HTTP.sys](xref:fundamentals/servers/httpsys) server web (noto in precedenza come [WebListener](xref:fundamentals/servers/weblistener) per le app di ASP.NET Core 1. x) anziché [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e3c20-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="e3c20-115">È consigliabile IIS per l'utilizzo come server proxy inverso per le distribuzioni di bordo con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e3c20-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="e3c20-116">Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="e3c20-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="e3c20-117">Introduzione</span><span class="sxs-lookup"><span data-stu-id="e3c20-117">Get started</span></span>

<span data-ttu-id="e3c20-118">Questa sezione illustra le modifiche minime necessarie per configurare un progetto ASP.NET Core esistente per l'esecuzione in un servizio.</span><span class="sxs-lookup"><span data-stu-id="e3c20-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="e3c20-119">Installare il pacchetto NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="e3c20-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="e3c20-120">Apportare le modifiche seguenti in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="e3c20-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="e3c20-121">Chiamare `host.RunAsService` anziché `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="e3c20-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="e3c20-122">Se il codice chiama `UseContentRoot`, utilizzare un percorso per il percorso di pubblicazione anziché `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="e3c20-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e3c20-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e3c20-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e3c20-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e3c20-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   * * *

3. <span data-ttu-id="e3c20-125">Pubblicare l'app in una cartella.</span><span class="sxs-lookup"><span data-stu-id="e3c20-125">Publish the app to a folder.</span></span> <span data-ttu-id="e3c20-126">Utilizzare [dotnet pubblicare](/dotnet/articles/core/tools/dotnet-publish) o [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) che pubblica una cartella.</span><span class="sxs-lookup"><span data-stu-id="e3c20-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="e3c20-127">Verificare la creazione e avvio del servizio.</span><span class="sxs-lookup"><span data-stu-id="e3c20-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="e3c20-128">Aprire una shell dei comandi con privilegi di amministratore per utilizzare il [sc.exe](https://technet.microsoft.com/library/bb490995) strumento da riga di comando per creare e avviare un servizio.</span><span class="sxs-lookup"><span data-stu-id="e3c20-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="e3c20-129">Se il servizio è denominato MyService, pubblicata `c:\svc`, e denominata AspNetCoreService, i comandi sono:</span><span class="sxs-lookup"><span data-stu-id="e3c20-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="e3c20-130">Il `binPath` valore rappresenta il percorso dell'eseguibile dell'app, che include il nome del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="e3c20-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Finestra della console, creare e avviare l'esempio](windows-service/_static/create-start.png)

   <span data-ttu-id="e3c20-132">Questi comandi terminato, scegliere il percorso stesso come quando viene eseguito come un'applicazione console (per impostazione predefinita, `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="e3c20-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![In esecuzione in un servizio](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="e3c20-134">Fornire un modo per l'esecuzione all'esterno di un servizio</span><span class="sxs-lookup"><span data-stu-id="e3c20-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="e3c20-135">È più semplice testare ed eseguire il debug durante l'esecuzione all'esterno di un servizio, pertanto è facoltativa per aggiungere il codice che chiama `RunAsService` solo in determinate condizioni.</span><span class="sxs-lookup"><span data-stu-id="e3c20-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="e3c20-136">Ad esempio, eseguire l'app come un'applicazione console con un `--console` argomento della riga di comando o se il debugger è collegato:</span><span class="sxs-lookup"><span data-stu-id="e3c20-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e3c20-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e3c20-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e3c20-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e3c20-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

* * *
## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="e3c20-139">Gestire l'arresto e avvio degli eventi</span><span class="sxs-lookup"><span data-stu-id="e3c20-139">Handle stopping and starting events</span></span>

<span data-ttu-id="e3c20-140">Per gestire `OnStarting`, `OnStarted`, e `OnStopping` degli eventi, apportare le modifiche aggiuntive seguenti:</span><span class="sxs-lookup"><span data-stu-id="e3c20-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="e3c20-141">Creare una classe che deriva da `WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="e3c20-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="e3c20-142">Creare un metodo di estensione per `IWebHost` che passa l'oggetto personalizzato `WebHostService` a `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="e3c20-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="e3c20-143">In `Program.Main`, chiamare il nuovo metodo di estensione, `RunAsCustomService`, invece di `RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="e3c20-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e3c20-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e3c20-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e3c20-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e3c20-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   * * *
<span data-ttu-id="e3c20-146">Se l'oggetto personalizzato `WebHostService` codice richiede un servizio dall'inserimento di dipendenze (ad esempio un logger), scaricarlo dal `Services` proprietà di `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="e3c20-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="e3c20-147">Server proxy e scenari di servizio di bilanciamento di carico</span><span class="sxs-lookup"><span data-stu-id="e3c20-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="e3c20-148">Servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o il bilanciamento del carico potrebbero richiedere un'ulteriore configurazione.</span><span class="sxs-lookup"><span data-stu-id="e3c20-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="e3c20-149">Per altre informazioni, vedere [configurare ASP.NET Core per lavorare con i server proxy e bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="e3c20-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="e3c20-150">Ringraziamenti</span><span class="sxs-lookup"><span data-stu-id="e3c20-150">Acknowledgments</span></span>

<span data-ttu-id="e3c20-151">In questo articolo è stato scritto con l'aiuto di origini pubblicate:</span><span class="sxs-lookup"><span data-stu-id="e3c20-151">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="e3c20-152">Hosting ASP.NET Core come servizio Windows</span><span class="sxs-lookup"><span data-stu-id="e3c20-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="e3c20-153">Come ospitare il ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="e3c20-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
