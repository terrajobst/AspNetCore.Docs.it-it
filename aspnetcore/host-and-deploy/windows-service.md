---
title: Ospitare ASP.NET Core in un servizio Windows
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 308a8bd10371cc70c431b8858ef7d82c1bb624da
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975425"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="73712-103">Ospitare ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="73712-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="73712-104">Di [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="73712-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="73712-105">È possibile ospitare un'app ASP.NET Core in Windows come [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) senza usare IIS.</span><span class="sxs-lookup"><span data-stu-id="73712-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="73712-106">Quando è ospitata come servizio di Windows, l'app viene avviata automaticamente dopo il riavvio del server.</span><span class="sxs-lookup"><span data-stu-id="73712-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="73712-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="73712-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73712-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="73712-108">Prerequisites</span></span>

* [<span data-ttu-id="73712-109">ASP.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="73712-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="73712-110">PowerShell 6.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="73712-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="73712-111">Modello di servizio di ruolo di lavoro</span><span class="sxs-lookup"><span data-stu-id="73712-111">Worker Service template</span></span>

<span data-ttu-id="73712-112">Il modello di servizio di ruolo di lavoro di ASP.NET Core rappresenta un punto di partenza per la scrittura di app di servizi a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="73712-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="73712-113">Per usare il modello come base per un'app di servizio Windows:</span><span class="sxs-lookup"><span data-stu-id="73712-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="73712-114">Creare un'app di servizio di ruolo di lavoro dal modello .NET Core.</span><span class="sxs-lookup"><span data-stu-id="73712-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="73712-115">Seguire le indicazioni nella sezione [Configurazione dell'app](#app-configuration) per aggiornare l'app di servizio di ruolo di lavoro affinché venga eseguita come servizio Windows.</span><span class="sxs-lookup"><span data-stu-id="73712-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="73712-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73712-116">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="73712-117">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="73712-117">Create a new project.</span></span>
1. <span data-ttu-id="73712-118">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="73712-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="73712-119">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="73712-119">Select **Next**.</span></span>
1. <span data-ttu-id="73712-120">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="73712-120">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="73712-121">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="73712-121">Select **Create**.</span></span>
1. <span data-ttu-id="73712-122">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="73712-122">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="73712-123">Selezionare il modello del **Servizio di ruolo di lavoro**.</span><span class="sxs-lookup"><span data-stu-id="73712-123">Select the **Worker Service** template.</span></span> <span data-ttu-id="73712-124">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="73712-124">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="73712-125">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="73712-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="73712-126">Usare il servizio di ruolo di lavoro (`worker`) con il comando[dotnet new](/dotnet/core/tools/dotnet-new) da una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="73712-126">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="73712-127">Nell'esempio seguente viene creata un'app del servizio di ruolo di lavoro denominata `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="73712-127">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="73712-128">Una cartella per l'app `ContosoWorkerService` viene creata automaticamente quando viene eseguito il comando.</span><span class="sxs-lookup"><span data-stu-id="73712-128">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="73712-129">Configurazione dell'app</span><span class="sxs-lookup"><span data-stu-id="73712-129">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="73712-130">`IHostBuilder.UseWindowsService`, fornito dal pacchetto [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices), viene chiamato durante la creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="73712-130">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="73712-131">Se l'app è in esecuzione come servizio di Windows, il metodo:</span><span class="sxs-lookup"><span data-stu-id="73712-131">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="73712-132">Imposta la durata dell'host su `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="73712-132">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="73712-133">Imposta la radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="73712-133">Sets the content root.</span></span>
* <span data-ttu-id="73712-134">Abilita la registrazione nel log eventi con il nome dell'applicazione come nome di origine predefinito.</span><span class="sxs-lookup"><span data-stu-id="73712-134">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="73712-135">Il livello di registrazione può essere configurato con la chiave `Logging:LogLevel:Default` nel file *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="73712-135">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="73712-136">Solo gli amministratori possono creare nuove origini eventi.</span><span class="sxs-lookup"><span data-stu-id="73712-136">Only administrators can create new event sources.</span></span> <span data-ttu-id="73712-137">Quando non è possibile creare un'origine evento usando il nome dell'applicazione, viene registrato un avviso nell'origine *Applicazione* e i log eventi vengono disabilitati.</span><span class="sxs-lookup"><span data-stu-id="73712-137">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="73712-138">L'app richiede i riferimenti ai pacchetti [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) e [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="73712-138">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="73712-139">Per eseguire test e debug durante l'esecuzione all'esterno di un servizio, aggiungere il codice per determinare se l'app è in esecuzione come servizio o è un'app console.</span><span class="sxs-lookup"><span data-stu-id="73712-139">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="73712-140">Controllare se il debugger è collegato o se è presente un'opzione `--console`.</span><span class="sxs-lookup"><span data-stu-id="73712-140">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="73712-141">Se una delle due condizioni è vera (l'app non è eseguita come servizio), chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="73712-141">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="73712-142">Se le condizioni sono false (l'app è eseguita come servizio):</span><span class="sxs-lookup"><span data-stu-id="73712-142">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="73712-143">Chiamare <xref:System.IO.Directory.SetCurrentDirectory*> e usare un percorso per la posizione di pubblicazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="73712-143">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="73712-144">Non chiamare <xref:System.IO.Directory.GetCurrentDirectory*> per ottenere il percorso perché un'app servizio Windows restituisce la cartella *C:\\WINDOWS\\system32* quando viene chiamato <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="73712-144">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="73712-145">Per altre informazioni, vedere la sezione [Directory corrente e radice del contenuto](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="73712-145">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="73712-146">Questo passaggio viene eseguito prima che l'app venga configurata in `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="73712-146">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="73712-147">Chiamare <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> per eseguire l'app come servizio.</span><span class="sxs-lookup"><span data-stu-id="73712-147">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="73712-148">Dato che il [provider di configurazione della riga di comando](xref:fundamentals/configuration/index#command-line-configuration-provider) richiede coppie nome-valore per gli argomenti della riga di comando, l'opzione `--console` viene rimossa dagli argomenti prima che <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> riceva gli argomenti.</span><span class="sxs-lookup"><span data-stu-id="73712-148">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="73712-149">Per scrivere nel registro eventi di Windows, aggiungere il provider EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="73712-149">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="73712-150">Impostare il livello di registrazione con la chiave `Logging:LogLevel:Default` nel file *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="73712-150">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="73712-151">Nel seguente esempio dell'app di esempio, viene chiamato `RunAsCustomService` invece di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> per gestire gli eventi di durata all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="73712-151">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="73712-152">Per altre informazioni, vedere la sezione [Gestire gli eventi di avvio e arresto](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="73712-152">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="73712-153">Tipo di distribuzione</span><span class="sxs-lookup"><span data-stu-id="73712-153">Deployment type</span></span>

<span data-ttu-id="73712-154">Per informazioni e consigli sugli scenari di distribuzione, vedere [Distribuzione di applicazioni .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="73712-154">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="73712-155">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="73712-155">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="73712-156">La distribuzione dipendente dal framework si basa sulla presenza di una versione condivisa a livello di sistema di .NET Core nel sistema di destinazione.</span><span class="sxs-lookup"><span data-stu-id="73712-156">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="73712-157">Quando lo scenario di distribuzione dipendente dal framework viene implementato in base alle indicazioni di questo articolo, l'SDK genera un file eseguibile (con estensione *exe*) detto *eseguibile dipendente dal framework*.</span><span class="sxs-lookup"><span data-stu-id="73712-157">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="73712-158">Aggiungere gli elementi di proprietà seguenti al file di progetto:</span><span class="sxs-lookup"><span data-stu-id="73712-158">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="73712-159">`<OutputType>` &ndash; Tipo di output dell'app (`Exe` per eseguibile).</span><span class="sxs-lookup"><span data-stu-id="73712-159">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="73712-160">`<LangVersion>` &ndash; Versione del linguaggio C# (`latest` o `preview`).</span><span class="sxs-lookup"><span data-stu-id="73712-160">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="73712-161">Un file *web.config*, che viene normalmente generato quando si pubblica un'app ASP.NET Core, non è necessario per un'app di servizi Windows.</span><span class="sxs-lookup"><span data-stu-id="73712-161">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="73712-162">Per disabilitare la creazione del file *web.config*, aggiungere la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="73712-162">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="73712-163">L'[identificatore di runtime (RID)](/dotnet/core/rid-catalog) di Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="73712-163">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="73712-164">Nell'esempio seguente il RID è impostato su `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="73712-164">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="73712-165">La proprietà `<SelfContained>` è impostata su `false`.</span><span class="sxs-lookup"><span data-stu-id="73712-165">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="73712-166">Queste proprietà indicano all'SDK di generare un file eseguibile (con estensione *exe*) per Windows e un'app che dipende dal framework .NET Core condiviso.</span><span class="sxs-lookup"><span data-stu-id="73712-166">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="73712-167">Un file *web.config*, che viene normalmente generato quando si pubblica un'app ASP.NET Core, non è necessario per un'app di servizi Windows.</span><span class="sxs-lookup"><span data-stu-id="73712-167">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="73712-168">Per disabilitare la creazione del file *web.config*, aggiungere la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="73712-168">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="73712-169">L'[identificatore di runtime (RID)](/dotnet/core/rid-catalog) di Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="73712-169">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="73712-170">Nell'esempio seguente il RID è impostato su `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="73712-170">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="73712-171">La proprietà `<SelfContained>` è impostata su `false`.</span><span class="sxs-lookup"><span data-stu-id="73712-171">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="73712-172">Queste proprietà indicano all'SDK di generare un file eseguibile (con estensione *exe*) per Windows e un'app che dipende dal framework .NET Core condiviso.</span><span class="sxs-lookup"><span data-stu-id="73712-172">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="73712-173">La proprietà `<UseAppHost>` è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="73712-173">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="73712-174">Questa proprietà fornisce il servizio con un percorso di attivazione (un file eseguibile, *.exe*) per una distribuzione dipendente dal framework.</span><span class="sxs-lookup"><span data-stu-id="73712-174">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="73712-175">Un file *web.config*, che viene normalmente generato quando si pubblica un'app ASP.NET Core, non è necessario per un'app di servizi Windows.</span><span class="sxs-lookup"><span data-stu-id="73712-175">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="73712-176">Per disabilitare la creazione del file *web.config*, aggiungere la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="73712-176">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="73712-177">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="73712-177">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="73712-178">Una distribuzione autonoma non si basa sulla presenza di un framework condiviso nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="73712-178">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="73712-179">Il runtime e le dipendenze dell'app vengono distribuiti con l'app.</span><span class="sxs-lookup"><span data-stu-id="73712-179">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="73712-180">Un [identificatore di runtime (RID)](/dotnet/core/rid-catalog) di Windows viene incluso nel `<PropertyGroup>` che contiene il framework di destinazione:</span><span class="sxs-lookup"><span data-stu-id="73712-180">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="73712-181">Per eseguire la pubblicazione per più identificatori di runtime:</span><span class="sxs-lookup"><span data-stu-id="73712-181">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="73712-182">Specificare gli identificatori di runtime in un elenco delimitato da punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="73712-182">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="73712-183">Usare il nome di proprietà [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plurale).</span><span class="sxs-lookup"><span data-stu-id="73712-183">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="73712-184">Per altre informazioni, vedere il [Catalogo RID di .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="73712-184">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="73712-185">Una proprietà `<SelfContained>` è impostata su `true`:</span><span class="sxs-lookup"><span data-stu-id="73712-185">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="73712-186">Account utente del servizio</span><span class="sxs-lookup"><span data-stu-id="73712-186">Service user account</span></span>

<span data-ttu-id="73712-187">Per creare un account utente per un servizio, usare il cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) da una shell dei comandi di PowerShell 6 amministrativa.</span><span class="sxs-lookup"><span data-stu-id="73712-187">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="73712-188">In Aggiornamento di Windows 10 (ottobre 2018) (versione 1809/build 10.0.17763) o versioni successive:</span><span class="sxs-lookup"><span data-stu-id="73712-188">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="73712-189">In un sistema operativo Windows precedente ad Aggiornamento di Windows 10 (ottobre 2018) (versione 1809/build 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="73712-189">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="73712-190">Fornire una [password complessa](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="73712-190">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="73712-191">Se non viene specificato il parametro `-AccountExpires` per il cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) con un valore <xref:System.DateTime> di scadenza, l'account non scade.</span><span class="sxs-lookup"><span data-stu-id="73712-191">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="73712-192">Per altre informazioni, vedere [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) e [Service User Accounts](/windows/desktop/services/service-user-accounts) (Account utente di servizio).</span><span class="sxs-lookup"><span data-stu-id="73712-192">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="73712-193">Un approccio alternativo per la gestione degli utenti quando si usa Active Directory consiste nell'usare account del servizio gestito.</span><span class="sxs-lookup"><span data-stu-id="73712-193">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="73712-194">Per altre informazioni, vedere [Panoramica degli account del servizio gestito del gruppo](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="73712-194">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="73712-195">Diritti Accesso come servizio</span><span class="sxs-lookup"><span data-stu-id="73712-195">Log on as a service rights</span></span>

<span data-ttu-id="73712-196">Per stabilire i diritti *Accesso come servizio* per un account utente di servizio:</span><span class="sxs-lookup"><span data-stu-id="73712-196">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="73712-197">Aprire l'editor Criteri di sicurezza locali eseguendo *secpol.msc*.</span><span class="sxs-lookup"><span data-stu-id="73712-197">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="73712-198">Espandere il nodo **Criteri locali** e selezionare **Assegnazione diritti utente**.</span><span class="sxs-lookup"><span data-stu-id="73712-198">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="73712-199">Aprire il criterio **Accesso come servizio**.</span><span class="sxs-lookup"><span data-stu-id="73712-199">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="73712-200">Selezionare **Aggiungi utente o gruppo**.</span><span class="sxs-lookup"><span data-stu-id="73712-200">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="73712-201">Specificare il nome oggetto (account utente) in uno dei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="73712-201">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="73712-202">Digitare l'account utente (`{DOMAIN OR COMPUTER NAME\USER}`) nel campo del nome oggetto e scegliere **OK** per aggiungere l'utente al criterio.</span><span class="sxs-lookup"><span data-stu-id="73712-202">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="73712-203">Selezionare **Avanzate**.</span><span class="sxs-lookup"><span data-stu-id="73712-203">Select **Advanced**.</span></span> <span data-ttu-id="73712-204">Selezionare **Trova**.</span><span class="sxs-lookup"><span data-stu-id="73712-204">Select **Find Now**.</span></span> <span data-ttu-id="73712-205">Selezionare l'account utente dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="73712-205">Select the user account from the list.</span></span> <span data-ttu-id="73712-206">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="73712-206">Select **OK**.</span></span> <span data-ttu-id="73712-207">Scegliere di nuovo **OK** per aggiungere l'utente al criterio.</span><span class="sxs-lookup"><span data-stu-id="73712-207">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="73712-208">Scegliere **OK** o **Applica** per accettare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="73712-208">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="73712-209">Creare e gestire il servizio di Windows</span><span class="sxs-lookup"><span data-stu-id="73712-209">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="73712-210">Creare un servizio</span><span class="sxs-lookup"><span data-stu-id="73712-210">Create a service</span></span>

<span data-ttu-id="73712-211">Usare i comandi di PowerShell per registrare un servizio.</span><span class="sxs-lookup"><span data-stu-id="73712-211">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="73712-212">Da una shell dei comandi di PowerShell 6 amministrativa eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="73712-212">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="73712-213">`{EXE PATH}` &ndash; Percorso della cartella dell'app nell'host (ad esempio `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="73712-213">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="73712-214">Non includere l'eseguibile dell'app nel percorso.</span><span class="sxs-lookup"><span data-stu-id="73712-214">Don't include the app's executable in the path.</span></span> <span data-ttu-id="73712-215">Non è necessario aggiungere una barra finale.</span><span class="sxs-lookup"><span data-stu-id="73712-215">A trailing slash isn't required.</span></span>
* <span data-ttu-id="73712-216">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Account utente del servizio (ad esempio `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="73712-216">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="73712-217">`{NAME}` &ndash; Nome del servizio (ad esempio `MyService`).</span><span class="sxs-lookup"><span data-stu-id="73712-217">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="73712-218">`{EXE FILE PATH}` &ndash; Percorso dell'eseguibile dell'app (ad esempio `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="73712-218">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="73712-219">Includere il nome del file eseguibile con l'estensione.</span><span class="sxs-lookup"><span data-stu-id="73712-219">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="73712-220">`{DESCRIPTION}` &ndash; Descrizione del servizio (ad esempio `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="73712-220">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="73712-221">`{DISPLAY NAME}` &ndash; Nome visualizzato del servizio (ad esempio `My Service`).</span><span class="sxs-lookup"><span data-stu-id="73712-221">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="73712-222">Avviare un servizio</span><span class="sxs-lookup"><span data-stu-id="73712-222">Start a service</span></span>

<span data-ttu-id="73712-223">Per avviare un servizio, usare il comando di PowerShell 6 seguente:</span><span class="sxs-lookup"><span data-stu-id="73712-223">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="73712-224">L'avvio del servizio richiede alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="73712-224">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="73712-225">Determinare lo stato di un servizio</span><span class="sxs-lookup"><span data-stu-id="73712-225">Determine a service's status</span></span>

<span data-ttu-id="73712-226">Per verificare lo stato di un servizio, usare il comando di PowerShell 6 seguente:</span><span class="sxs-lookup"><span data-stu-id="73712-226">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="73712-227">Lo stato viene indicato con uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="73712-227">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="73712-228">Arrestare un servizio</span><span class="sxs-lookup"><span data-stu-id="73712-228">Stop a service</span></span>

<span data-ttu-id="73712-229">Per arrestare un servizio, usare il comando di PowerShell 6 seguente:</span><span class="sxs-lookup"><span data-stu-id="73712-229">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="73712-230">Rimuovere un servizio</span><span class="sxs-lookup"><span data-stu-id="73712-230">Remove a service</span></span>

<span data-ttu-id="73712-231">Dopo il breve lasso di tempo necessario per arrestare un servizio, rimuovere il servizio con il comando di PowerShell 6 seguente:</span><span class="sxs-lookup"><span data-stu-id="73712-231">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="73712-232">Gestire gli eventi di avvio e arresto</span><span class="sxs-lookup"><span data-stu-id="73712-232">Handle starting and stopping events</span></span>

<span data-ttu-id="73712-233">Per gestire gli eventi <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> e <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>:</span><span class="sxs-lookup"><span data-stu-id="73712-233">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="73712-234">Creare una classe che deriva da <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con i metodi `OnStarting`, `OnStarted` e `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="73712-234">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="73712-235">Creare un metodo di estensione per <xref:Microsoft.AspNetCore.Hosting.IWebHost> che passa `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="73712-235">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="73712-236">In `Program.Main` chiamare il metodo di estensione `RunAsCustomService` invece di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="73712-236">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="73712-237">Per visualizzare il percorso di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, fare riferimento all'esempio di codice illustrato nella sezione [Tipo di distribuzione](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="73712-237">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="73712-238">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="73712-238">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="73712-239">I servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o un servizio di bilanciamento del carico potrebbero richiedere una configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="73712-239">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="73712-240">Per altre informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="73712-240">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="73712-241">Configurare HTTPS</span><span class="sxs-lookup"><span data-stu-id="73712-241">Configure HTTPS</span></span>

<span data-ttu-id="73712-242">Per configurare un servizio con un endpoint sicuro:</span><span class="sxs-lookup"><span data-stu-id="73712-242">To configure a service with a secure endpoint:</span></span>

1. <span data-ttu-id="73712-243">Creare un certificato X.509 per il sistema di hosting usando i meccanismi di acquisizione e distribuzione dei certificati della piattaforma.</span><span class="sxs-lookup"><span data-stu-id="73712-243">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="73712-244">Specificare una [configurazione dell'endpoint HTTPS del server Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) per usare il certificato.</span><span class="sxs-lookup"><span data-stu-id="73712-244">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="73712-245">L'uso del certificato di sviluppo ASP.NET Core HTTPS per proteggere un endpoint del servizio non è supportato.</span><span class="sxs-lookup"><span data-stu-id="73712-245">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="73712-246">Directory corrente e radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="73712-246">Current directory and content root</span></span>

<span data-ttu-id="73712-247">La directory di lavoro corrente restituita chiamando <xref:System.IO.Directory.GetCurrentDirectory*> per un servizio Windows è la cartella *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="73712-247">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="73712-248">La cartella *system32* non è un percorso appropriato per archiviare i file di un servizio, ad esempio i file di impostazioni.</span><span class="sxs-lookup"><span data-stu-id="73712-248">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="73712-249">Usare uno degli approcci seguenti per gestire e accedere agli asset e ai file di impostazioni di un servizio.</span><span class="sxs-lookup"><span data-stu-id="73712-249">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="73712-250">Usare ContentRootPath o ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="73712-250">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="73712-251">Usare [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) o <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> per individuare le risorse di un'app.</span><span class="sxs-lookup"><span data-stu-id="73712-251">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="73712-252">Impostare il percorso radice del contenuto sulla cartella dell'app</span><span class="sxs-lookup"><span data-stu-id="73712-252">Set the content root path to the app's folder</span></span>

<span data-ttu-id="73712-253"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> è lo stesso percorso fornito all'argomento `binPath` durante la creazione di un servizio.</span><span class="sxs-lookup"><span data-stu-id="73712-253">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="73712-254">Invece di chiamare `GetCurrentDirectory` per creare i percorsi dei file di impostazioni, chiamare <xref:System.IO.Directory.SetCurrentDirectory*> con il percorso radice del contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="73712-254">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="73712-255">In `Program.Main`, determinare il percorso della cartella dell'eseguibile del servizio e usare il percorso per stabilire la radice del contenuto dell'app:</span><span class="sxs-lookup"><span data-stu-id="73712-255">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="73712-256">Archiviare i file di un servizio in un percorso appropriato nel disco</span><span class="sxs-lookup"><span data-stu-id="73712-256">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="73712-257">Specificare un percorso assoluto con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> quando si usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> per la cartella contenente i file.</span><span class="sxs-lookup"><span data-stu-id="73712-257">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73712-258">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="73712-258">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="73712-259">[Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)</span><span class="sxs-lookup"><span data-stu-id="73712-259">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="73712-260">[Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)</span><span class="sxs-lookup"><span data-stu-id="73712-260">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
