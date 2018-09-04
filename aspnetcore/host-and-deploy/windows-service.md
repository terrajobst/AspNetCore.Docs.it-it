---
title: Ospitare ASP.NET Core in un servizio Windows
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 68afe77b05a717cffecc32188f18e9fde208b81f
ms.sourcegitcommit: 3ca20ed63bf1469f4365f0c1fbd00c98a3191c84
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2018
ms.locfileid: "41751489"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="e02b9-103">Ospitare ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="e02b9-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="e02b9-104">Di [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e02b9-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="e02b9-105">È possibile ospitare un'app ASP.NET Core in Windows senza usare IIS come [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="e02b9-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="e02b9-106">Quando è ospitata come servizio Windows, l'app può essere avviata automaticamente dopo riavvii e arresti anomali senza alcun intervento manuale.</span><span class="sxs-lookup"><span data-stu-id="e02b9-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="e02b9-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e02b9-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="e02b9-108">Introduzione</span><span class="sxs-lookup"><span data-stu-id="e02b9-108">Get started</span></span>

<span data-ttu-id="e02b9-109">Le modifiche minime seguenti sono necessarie per configurare un progetto ASP.NET Core esistente per l'esecuzione in un servizio:</span><span class="sxs-lookup"><span data-stu-id="e02b9-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="e02b9-110">Nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="e02b9-110">In the project file:</span></span>

   1. <span data-ttu-id="e02b9-111">Verificare la presenza dell'identificatore di runtime o aggiungerlo al **\<PropertyGroup >** che contiene il framework di destinazione:</span><span class="sxs-lookup"><span data-stu-id="e02b9-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

   1. <span data-ttu-id="e02b9-112">Aggiungere un riferimento al pacchetto per [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="e02b9-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="e02b9-113">Modificare `Program.Main` nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="e02b9-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="e02b9-114">Chiamare [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) invece di `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="e02b9-115">Chiamare [UseContentRoot](xref:fundamentals/host/web-host#content-root) e usare un percorso di pubblicazione dell'app invece di `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="e02b9-116">Pubblicare l'app.</span><span class="sxs-lookup"><span data-stu-id="e02b9-116">Publish the app.</span></span> <span data-ttu-id="e02b9-117">Usare [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="e02b9-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="e02b9-118">Quando si usa Visual Studio, selezionare **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="e02b9-118">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="e02b9-119">Per pubblicare l'app di esempio dalla riga di comando, eseguire il comando seguente in una finestra della console dalla cartella di progetto:</span><span class="sxs-lookup"><span data-stu-id="e02b9-119">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="e02b9-120">Usare lo strumento da riga di comando [sc.exe](https://technet.microsoft.com/library/bb490995) per creare il servizio.</span><span class="sxs-lookup"><span data-stu-id="e02b9-120">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="e02b9-121">Il valore `binPath` rappresenta il percorso dell'eseguibile dell'app, che include il nome del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="e02b9-121">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="e02b9-122">**Lo spazio tra il segno di uguale e le virgolette che indicano l'inizio del percorso è obbligatorio.**</span><span class="sxs-lookup"><span data-stu-id="e02b9-122">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="e02b9-123">Per creare un servizio pubblicato nella cartella del progetto, usare il percorso della cartella *publish*.</span><span class="sxs-lookup"><span data-stu-id="e02b9-123">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="e02b9-124">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e02b9-124">In the following example:</span></span>

   * <span data-ttu-id="e02b9-125">Il progetto si trova nella cartella `c:\my_services\AspNetCoreService`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-125">The project resides in the `c:\my_services\AspNetCoreService` folder.</span></span>
   * <span data-ttu-id="e02b9-126">Il progetto viene pubblicato nella configurazione `Release`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-126">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="e02b9-127">Il moniker framework di destinazione (TFM) è `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-127">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="e02b9-128">L'identificatore relativo (RID) è `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-128">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="e02b9-129">L'eseguibile dell'app è denominato *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="e02b9-129">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="e02b9-130">Il servizio è denominato **MyService**.</span><span class="sxs-lookup"><span data-stu-id="e02b9-130">The service is named **MyService**.</span></span>

   <span data-ttu-id="e02b9-131">Esempio:</span><span class="sxs-lookup"><span data-stu-id="e02b9-131">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="e02b9-132">Verificare che sia presente lo spazio tra l'argomento `binPath=` e il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="e02b9-132">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="e02b9-133">Per pubblicare e avviare il servizio da una cartella diversa:</span><span class="sxs-lookup"><span data-stu-id="e02b9-133">To publish and start the service from a different folder:</span></span>
   
      1. <span data-ttu-id="e02b9-134">Usare l'opzione [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) sul comando `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-134">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="e02b9-135">Se si usa Visual Studio, configurare il **Percorso di destinazione** nella pagina delle proprietà di pubblicazione **FolderProfile** prima di selezionare il pulsante **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="e02b9-135">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
   1. <span data-ttu-id="e02b9-136">Creare il servizio con il comando `sc.exe` usando il percorso della cartella di output.</span><span class="sxs-lookup"><span data-stu-id="e02b9-136">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="e02b9-137">Includere il nome dell'eseguibile del servizio nel percorso fornito a `binPath`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-137">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="e02b9-138">Avviare il servizio con il comando `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-138">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="e02b9-139">Per avviare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e02b9-139">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="e02b9-140">L'avvio del servizio richiede alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="e02b9-140">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="e02b9-141">Il comando `sc query <SERVICE_NAME>` può essere utilizzato per controllare lo stato del servizio:</span><span class="sxs-lookup"><span data-stu-id="e02b9-141">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="e02b9-142">Usare il comando seguente per verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="e02b9-142">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="e02b9-143">Quando il servizio è nello stato `RUNNING` e se il servizio è un'app Web, selezionare l'app dal suo percorso (per impostazione predefinita, `http://localhost:5000`, che reindirizza a `https://localhost:5001` quando si utilizza [il middleware di reindirizzamento HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="e02b9-143">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="e02b9-144">Per il servizio app di esempio, selezionare l'app in `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-144">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="e02b9-145">Arrestare il servizio con il comando `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-145">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="e02b9-146">Per arrestare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e02b9-146">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="e02b9-147">Dopo il breve lasso di tempo necessario per arrestare il servizio, disinstallare il servizio con il comando `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="e02b9-147">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="e02b9-148">Verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="e02b9-148">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="e02b9-149">Quando il servizio app di esempio è nello stato `STOPPED`, usare il comando seguente per disinstallare il servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="e02b9-149">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="e02b9-150">Fornire un modo per l'esecuzione all'esterno di un servizio</span><span class="sxs-lookup"><span data-stu-id="e02b9-150">Provide a way to run outside of a service</span></span>

<span data-ttu-id="e02b9-151">È più semplice testare ed eseguire il debug durante l'esecuzione all'esterno di un servizio, pertanto può essere utile aggiungere codice che chiama `RunAsService` solo in determinate condizioni.</span><span class="sxs-lookup"><span data-stu-id="e02b9-151">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="e02b9-152">Ad esempio, l'app può essere eseguita come un'app console con un argomento della riga di comando `--console` o se il debugger è collegato:</span><span class="sxs-lookup"><span data-stu-id="e02b9-152">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="e02b9-153">Poiché la configurazione di ASP.NET Core richiede coppie nome-valore per gli argomenti della riga di comando, l'opzione `--console` viene rimossa prima che gli argomenti vengono passati a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="e02b9-153">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="e02b9-154">`isService` non viene passato da `Main` in `CreateWebHostBuilder` perché la firma di `CreateWebHostBuilder` deve essere `CreateWebHostBuilder(string[])` per garantire il corretto funzionamento dei [test di integrazione](xref:test/integration-tests).</span><span class="sxs-lookup"><span data-stu-id="e02b9-154">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="e02b9-155">Gestire gli eventi di arresto e avvio</span><span class="sxs-lookup"><span data-stu-id="e02b9-155">Handle stopping and starting events</span></span>

<span data-ttu-id="e02b9-156">Per gestire gli eventi [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportare le modifiche aggiuntive seguenti:</span><span class="sxs-lookup"><span data-stu-id="e02b9-156">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="e02b9-157">Creare una classe che derivi dalla classe [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="e02b9-157">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="e02b9-158">Creare un metodo di estensione per [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) che passi l'oggetto `WebHostService` personalizzato a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="e02b9-158">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="e02b9-159">In `Program.Main` chiamare il nuovo metodo di estensione, `RunAsCustomService`, anziché [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="e02b9-159">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="e02b9-160">`isService` non viene passato da `Main` in `CreateWebHostBuilder` perché la firma di `CreateWebHostBuilder` deve essere `CreateWebHostBuilder(string[])` per garantire il corretto funzionamento dei [test di integrazione](xref:test/integration-tests).</span><span class="sxs-lookup"><span data-stu-id="e02b9-160">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="e02b9-161">Se il codice `WebHostService` personalizzato richiede un servizio dall'inserimento di dipendenze, ad esempio un logger, ottenerlo dalla proprietà [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="e02b9-161">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="e02b9-162">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="e02b9-162">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="e02b9-163">I servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o un servizio di bilanciamento del carico potrebbero richiedere una configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="e02b9-163">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="e02b9-164">Per ulteriori informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="e02b9-164">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="e02b9-165">Configurare HTTPS</span><span class="sxs-lookup"><span data-stu-id="e02b9-165">Configure HTTPS</span></span>

<span data-ttu-id="e02b9-166">Specificare una [configurazione dell'endpoint HTTPS del server Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="e02b9-166">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="e02b9-167">Directory corrente e radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="e02b9-167">Current directory and content root</span></span>

<span data-ttu-id="e02b9-168">La directory di lavoro corrente restituita dalla chiamata di `Directory.GetCurrentDirectory()` per un servizio Windows è la cartella *C:\WINDOWS\system32*.</span><span class="sxs-lookup"><span data-stu-id="e02b9-168">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\WINDOWS\system32* folder.</span></span> <span data-ttu-id="e02b9-169">La cartella *system32* non è un percorso appropriato per archiviare i file di un servizio, ad esempio i file di impostazioni.</span><span class="sxs-lookup"><span data-stu-id="e02b9-169">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="e02b9-170">Usare uno degli approcci seguenti per gestire e accedere alle risorse e ai file di impostazioni di un servizio con [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) quando si usa un'interfaccia [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="e02b9-170">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="e02b9-171">Usare il percorso radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="e02b9-171">Use the content root path.</span></span> <span data-ttu-id="e02b9-172">L'elemento `IHostingEnvironment.ContentRootPath` è lo stesso percorso fornito all'argomento `binPath` durante la creazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="e02b9-172">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="e02b9-173">Invece di usare `Directory.GetCurrentDirectory()` per creare i percorsi dei file di impostazioni, usare il percorso radice del contenuto e mantenere i file nella radice del contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="e02b9-173">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="e02b9-174">Archiviare i file in un percorso appropriato nel disco.</span><span class="sxs-lookup"><span data-stu-id="e02b9-174">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="e02b9-175">Specificare un percorso assoluto con `SetBasePath` alla cartella contenente i file.</span><span class="sxs-lookup"><span data-stu-id="e02b9-175">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e02b9-176">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e02b9-176">Additional resources</span></span>

* <span data-ttu-id="e02b9-177">[Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)</span><span class="sxs-lookup"><span data-stu-id="e02b9-177">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
