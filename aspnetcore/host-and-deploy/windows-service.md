---
title: Ospitare ASP.NET Core in un servizio Windows
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 5eba685bbe55d43bb063a01798bc691a1ba0d6fc
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613086"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="905b9-103">Ospitare ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="905b9-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="905b9-104">Di [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="905b9-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="905b9-105">È possibile ospitare un'app ASP.NET Core in Windows senza usare IIS come [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="905b9-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="905b9-106">Quando è ospitata come servizio Windows, l'app può essere avviata automaticamente dopo riavvii e arresti anomali senza alcun intervento manuale.</span><span class="sxs-lookup"><span data-stu-id="905b9-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="905b9-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="905b9-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="905b9-108">Introduzione</span><span class="sxs-lookup"><span data-stu-id="905b9-108">Get started</span></span>

<span data-ttu-id="905b9-109">Le modifiche minime seguenti sono necessarie per configurare un progetto ASP.NET Core esistente per l'esecuzione in un servizio:</span><span class="sxs-lookup"><span data-stu-id="905b9-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="905b9-110">Nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="905b9-110">In the project file:</span></span>

   1. <span data-ttu-id="905b9-111">Verificare la presenza dell'identificatore di runtime o aggiungerlo al **\<PropertyGroup >** che contiene il framework di destinazione:</span><span class="sxs-lookup"><span data-stu-id="905b9-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="905b9-112">Aggiungere un riferimento al pacchetto per [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="905b9-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="905b9-113">Modificare `Program.Main` nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="905b9-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="905b9-114">Chiamare [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) invece di `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="905b9-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="905b9-115">Se il codice chiama `UseContentRoot`, usare un percorso di pubblicazione dell'app anziché `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="905b9-115">If the code calls `UseContentRoot`, use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     <span data-ttu-id="905b9-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span><span class="sxs-lookup"><span data-stu-id="905b9-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span></span>

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     <span data-ttu-id="905b9-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span><span class="sxs-lookup"><span data-stu-id="905b9-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span></span>

     ::: moniker-end

1. <span data-ttu-id="905b9-118">Pubblicare l'app in una cartella.</span><span class="sxs-lookup"><span data-stu-id="905b9-118">Publish the app to a folder.</span></span> <span data-ttu-id="905b9-119">Usare [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) che esegue la pubblicazione in una cartella.</span><span class="sxs-lookup"><span data-stu-id="905b9-119">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

   <span data-ttu-id="905b9-120">Per pubblicare l'app di esempio dalla riga di comando, eseguire il comando seguente in una finestra della console dalla cartella di progetto:</span><span class="sxs-lookup"><span data-stu-id="905b9-120">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release --output c:\svc
   ```

1. <span data-ttu-id="905b9-121">Usare lo strumento da riga di comando [sc.exe](https://technet.microsoft.com/library/bb490995) per creare il servizio (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span><span class="sxs-lookup"><span data-stu-id="905b9-121">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span></span> <span data-ttu-id="905b9-122">Il valore `binPath` rappresenta il percorso dell'eseguibile dell'app, che include il nome del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="905b9-122">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="905b9-123">**Lo spazio tra il segno di uguale e le virgolette che indicano l'inizio del percorso è obbligatorio.**</span><span class="sxs-lookup"><span data-stu-id="905b9-123">**The space between the equal sign and the quote character that starts the path is required.**</span></span>

   <span data-ttu-id="905b9-124">Per l'app di esempio e il comando che segue, il servizio è:</span><span class="sxs-lookup"><span data-stu-id="905b9-124">For the sample app and command that follows, the service is:</span></span>

   * <span data-ttu-id="905b9-125">**MyService** denominato.</span><span class="sxs-lookup"><span data-stu-id="905b9-125">Named **MyService**.</span></span>
   * <span data-ttu-id="905b9-126">Pubblicato nella cartella *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="905b9-126">Published to *c:\\svc* folder.</span></span>
   * <span data-ttu-id="905b9-127">Ha un eseguibile dell'app denominato *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="905b9-127">Has an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="905b9-128">Aprire una shell comandi con privilegi amministrativi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="905b9-128">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   ```

   <span data-ttu-id="905b9-129">**Verificare che sia presente lo spazio tra l'argomento `binPath=` e il relativo valore.**</span><span class="sxs-lookup"><span data-stu-id="905b9-129">**Make sure the space is present between the `binPath=` argument and its value.**</span></span>

1. <span data-ttu-id="905b9-130">Avviare il servizio con il comando `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="905b9-130">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="905b9-131">Per avviare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="905b9-131">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="905b9-132">L'avvio del servizio richiede alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="905b9-132">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="905b9-133">Il comando `sc query <SERVICE_NAME>` può essere utilizzato per controllare lo stato del servizio:</span><span class="sxs-lookup"><span data-stu-id="905b9-133">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="905b9-134">Usare il comando seguente per verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="905b9-134">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="905b9-135">Quando il servizio è nello stato `RUNNING` e se il servizio è un'app Web, selezionare l'app dal suo percorso (per impostazione predefinita, `http://localhost:5000`, che reindirizza a `https://localhost:5001` quando si utilizza [il middleware di reindirizzamento HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="905b9-135">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="905b9-136">Per il servizio app di esempio, selezionare l'app in `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="905b9-136">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="905b9-137">Arrestare il servizio con il comando `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="905b9-137">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="905b9-138">Per arrestare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="905b9-138">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="905b9-139">Dopo il breve lasso di tempo necessario per arrestare il servizio, disinstallare il servizio con il comando `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="905b9-139">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="905b9-140">Verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="905b9-140">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="905b9-141">Quando il servizio app di esempio è nello stato `STOPPED`, usare il comando seguente per disinstallare il servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="905b9-141">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="905b9-142">Fornire un modo per l'esecuzione all'esterno di un servizio</span><span class="sxs-lookup"><span data-stu-id="905b9-142">Provide a way to run outside of a service</span></span>

<span data-ttu-id="905b9-143">È più semplice testare ed eseguire il debug durante l'esecuzione all'esterno di un servizio, pertanto può essere utile aggiungere codice che chiama `RunAsService` solo in determinate condizioni.</span><span class="sxs-lookup"><span data-stu-id="905b9-143">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="905b9-144">Ad esempio, l'app può essere eseguita come un'app console con un argomento della riga di comando `--console` o se il debugger è collegato:</span><span class="sxs-lookup"><span data-stu-id="905b9-144">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="905b9-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="905b9-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span></span>

<span data-ttu-id="905b9-146">Poiché la configurazione di ASP.NET Core richiede coppie nome-valore per gli argomenti della riga di comando, l'opzione `--console` viene rimossa prima che gli argomenti vengono passati a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="905b9-146">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="905b9-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="905b9-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span></span>

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="905b9-148">Gestire gli eventi di arresto e avvio</span><span class="sxs-lookup"><span data-stu-id="905b9-148">Handle stopping and starting events</span></span>

<span data-ttu-id="905b9-149">Per gestire gli eventi [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportare le modifiche aggiuntive seguenti:</span><span class="sxs-lookup"><span data-stu-id="905b9-149">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="905b9-150">Creare una classe che derivi dalla classe [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="905b9-150">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="905b9-151">Creare un metodo di estensione per [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) che passi l'oggetto `WebHostService` personalizzato a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="905b9-151">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="905b9-152">In `Program.Main` chiamare il nuovo metodo di estensione, `RunAsCustomService`, anziché [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="905b9-152">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   <span data-ttu-id="905b9-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="905b9-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   <span data-ttu-id="905b9-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="905b9-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

<span data-ttu-id="905b9-155">Se il codice `WebHostService` personalizzato richiede un servizio dall'inserimento di dipendenze, ad esempio un logger, ottenerlo dalla proprietà [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="905b9-155">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="905b9-156">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="905b9-156">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="905b9-157">I servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o un servizio di bilanciamento del carico potrebbero richiedere una configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="905b9-157">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="905b9-158">Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="905b9-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="905b9-159">Configurazione dell'endpoint Kestrel</span><span class="sxs-lookup"><span data-stu-id="905b9-159">Kestrel endpoint configuration</span></span>

<span data-ttu-id="905b9-160">Per informazioni sulla configurazione dell'endpoint Kestrel, inclusa la configurazione HTTPS e il supporto SNI, vedere [Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="905b9-160">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
