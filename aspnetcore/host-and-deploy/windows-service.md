---
title: Ospitare ASP.NET Core in un servizio Windows
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/06/2020
uid: host-and-deploy/windows-service
ms.openlocfilehash: 71f7bf3f5dcf8068d0ada03675ef7948267b79f4
ms.sourcegitcommit: bd896935e91236e03241f75e6534ad6debcecbbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/06/2020
ms.locfileid: "77044901"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="9f985-103">Ospitare ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="9f985-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="9f985-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9f985-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9f985-105">È possibile ospitare un'app ASP.NET Core in Windows come [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) senza usare IIS.</span><span class="sxs-lookup"><span data-stu-id="9f985-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="9f985-106">Quando è ospitata come servizio di Windows, l'app viene avviata automaticamente dopo il riavvio del server.</span><span class="sxs-lookup"><span data-stu-id="9f985-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="9f985-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f985-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f985-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="9f985-108">Prerequisites</span></span>

* [<span data-ttu-id="9f985-109">ASP.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="9f985-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="9f985-110">PowerShell 6.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="9f985-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="9f985-111">Modello di servizio di ruolo di lavoro</span><span class="sxs-lookup"><span data-stu-id="9f985-111">Worker Service template</span></span>

<span data-ttu-id="9f985-112">Il modello di servizio di ruolo di lavoro di ASP.NET Core rappresenta un punto di partenza per la scrittura di app di servizi a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="9f985-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="9f985-113">Per usare il modello come base per un'app di servizio Windows:</span><span class="sxs-lookup"><span data-stu-id="9f985-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="9f985-114">Creare un'app di servizio di ruolo di lavoro dal modello .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f985-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="9f985-115">Seguire le indicazioni nella sezione [Configurazione dell'app](#app-configuration) per aggiornare l'app di servizio di ruolo di lavoro affinché venga eseguita come servizio Windows.</span><span class="sxs-lookup"><span data-stu-id="9f985-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="9f985-116">Configurazione dell'app</span><span class="sxs-lookup"><span data-stu-id="9f985-116">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9f985-117">L'app richiede un riferimento al pacchetto per [Microsoft. Extensions. Hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="9f985-117">The app requires a package reference for [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span></span>

<span data-ttu-id="9f985-118">`IHostBuilder.UseWindowsService` viene chiamato durante la compilazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="9f985-118">`IHostBuilder.UseWindowsService` is called when building the host.</span></span> <span data-ttu-id="9f985-119">Se l'app è in esecuzione come servizio di Windows, il metodo:</span><span class="sxs-lookup"><span data-stu-id="9f985-119">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="9f985-120">Imposta la durata dell'host su `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="9f985-120">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="9f985-121">Imposta la [radice del contenuto](xref:fundamentals/index#content-root) su [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory).</span><span class="sxs-lookup"><span data-stu-id="9f985-121">Sets the [content root](xref:fundamentals/index#content-root) to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span> <span data-ttu-id="9f985-122">Per altre informazioni, vedere la sezione [Directory corrente e radice del contenuto](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="9f985-122">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
* <span data-ttu-id="9f985-123">Abilita la registrazione nel registro eventi:</span><span class="sxs-lookup"><span data-stu-id="9f985-123">Enables logging to the event log:</span></span>
  * <span data-ttu-id="9f985-124">Il nome dell'applicazione viene usato come nome di origine predefinito.</span><span class="sxs-lookup"><span data-stu-id="9f985-124">The application name is used as the default source name.</span></span>
  * <span data-ttu-id="9f985-125">Il livello di registrazione predefinito è *avviso* o superiore per un'app basata su un modello di ASP.NET Core che chiama `CreateDefaultBuilder` per compilare l'host.</span><span class="sxs-lookup"><span data-stu-id="9f985-125">The default log level is *Warning* or higher for an app based on an ASP.NET Core template that calls `CreateDefaultBuilder` to build the host.</span></span>
  * <span data-ttu-id="9f985-126">Eseguire l'override del livello di registrazione predefinito con la chiave `Logging:EventLog:LogLevel:Default` in *appSettings. json*/*appSettings. { Environment}. JSON* o un altro provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="9f985-126">Override the default log level with the `Logging:EventLog:LogLevel:Default` key in *appsettings.json*/*appsettings.{Environment}.json* or other configuration provider.</span></span>
  * <span data-ttu-id="9f985-127">Solo gli amministratori possono creare nuove origini eventi.</span><span class="sxs-lookup"><span data-stu-id="9f985-127">Only administrators can create new event sources.</span></span> <span data-ttu-id="9f985-128">Quando non è possibile creare un'origine evento usando il nome dell'applicazione, viene registrato un avviso nell'origine *Applicazione* e i log eventi vengono disabilitati.</span><span class="sxs-lookup"><span data-stu-id="9f985-128">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

<span data-ttu-id="9f985-129">In `CreateHostBuilder` di *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f985-129">In `CreateHostBuilder` of *Program.cs*:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

<span data-ttu-id="9f985-130">Questo argomento è accompagnato dalle app di esempio seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f985-130">The following sample apps accompany this topic:</span></span>

* <span data-ttu-id="9f985-131">Esempio di servizio di lavoro in background &ndash; un esempio di app non Web basato sul [modello di servizio](#worker-service-template) del ruolo di lavoro che usa i [servizi ospitati](xref:fundamentals/host/hosted-services) per le attività in background.</span><span class="sxs-lookup"><span data-stu-id="9f985-131">Background Worker Service Sample &ndash; A non-web app sample based on the [Worker Service template](#worker-service-template) that uses [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>
* <span data-ttu-id="9f985-132">Esempio di servizio app Web &ndash; un esempio di app Web Razor Pages che viene eseguito come servizio Windows con [servizi ospitati](xref:fundamentals/host/hosted-services) per le attività in background.</span><span class="sxs-lookup"><span data-stu-id="9f985-132">Web App Service Sample &ndash; A Razor Pages web app sample that runs as a Windows Service with [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>

<span data-ttu-id="9f985-133">Per informazioni aggiuntive su MVC, vedere gli articoli in <xref:mvc/overview> e <xref:migration/22-to-30>.</span><span class="sxs-lookup"><span data-stu-id="9f985-133">For MVC guidance, see the articles under <xref:mvc/overview> and <xref:migration/22-to-30>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9f985-134">L'app richiede i riferimenti ai pacchetti [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) e [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="9f985-134">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="9f985-135">Per eseguire test e debug durante l'esecuzione all'esterno di un servizio, aggiungere il codice per determinare se l'app è in esecuzione come servizio o è un'app console.</span><span class="sxs-lookup"><span data-stu-id="9f985-135">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="9f985-136">Controllare se il debugger è collegato o se è presente un'opzione `--console`.</span><span class="sxs-lookup"><span data-stu-id="9f985-136">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="9f985-137">Se una delle due condizioni è vera (l'app non è eseguita come servizio), chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="9f985-137">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="9f985-138">Se le condizioni sono false (l'app è eseguita come servizio):</span><span class="sxs-lookup"><span data-stu-id="9f985-138">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="9f985-139">Chiamare <xref:System.IO.Directory.SetCurrentDirectory*> e usare un percorso per la posizione di pubblicazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f985-139">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="9f985-140">Non chiamare <xref:System.IO.Directory.GetCurrentDirectory*> per ottenere il percorso perché un'app servizio Windows restituisce la cartella *C:\\WINDOWS\\system32* quando viene chiamato <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="9f985-140">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="9f985-141">Per altre informazioni, vedere la sezione [Directory corrente e radice del contenuto](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="9f985-141">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="9f985-142">Questo passaggio viene eseguito prima che l'app venga configurata in `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9f985-142">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="9f985-143">Chiamare <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> per eseguire l'app come servizio.</span><span class="sxs-lookup"><span data-stu-id="9f985-143">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="9f985-144">Dato che il [provider di configurazione della riga di comando](xref:fundamentals/configuration/index#command-line-configuration-provider) richiede coppie nome-valore per gli argomenti della riga di comando, l'opzione `--console` viene rimossa dagli argomenti prima che <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> riceva gli argomenti.</span><span class="sxs-lookup"><span data-stu-id="9f985-144">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="9f985-145">Per scrivere nel registro eventi di Windows, aggiungere il provider EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="9f985-145">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="9f985-146">Impostare il livello di registrazione con la chiave `Logging:LogLevel:Default` nel file *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="9f985-146">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="9f985-147">Nel seguente esempio dell'app di esempio, viene chiamato `RunAsCustomService` invece di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> per gestire gli eventi di durata all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f985-147">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="9f985-148">Per altre informazioni, vedere la sezione [Gestire gli eventi di avvio e arresto](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="9f985-148">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="9f985-149">Tipo di distribuzione</span><span class="sxs-lookup"><span data-stu-id="9f985-149">Deployment type</span></span>

<span data-ttu-id="9f985-150">Per informazioni e consigli sugli scenari di distribuzione, vedere [Distribuzione di applicazioni .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="9f985-150">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="9f985-151">SDK</span><span class="sxs-lookup"><span data-stu-id="9f985-151">SDK</span></span>

<span data-ttu-id="9f985-152">Per un servizio basato su app Web che usa i Framework Razor Pages o MVC, specificare Web SDK nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="9f985-152">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="9f985-153">Se il servizio esegue solo attività in background, ad esempio [servizi ospitati](xref:fundamentals/host/hosted-services), specificare l'SDK del ruolo di lavoro nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="9f985-153">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="9f985-154">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="9f985-154">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="9f985-155">La distribuzione dipendente dal framework si basa sulla presenza di una versione condivisa a livello di sistema di .NET Core nel sistema di destinazione.</span><span class="sxs-lookup"><span data-stu-id="9f985-155">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="9f985-156">Quando lo scenario di distribuzione dipendente dal framework viene implementato in base alle indicazioni di questo articolo, l'SDK genera un file eseguibile (con estensione *exe*) detto *eseguibile dipendente dal framework*.</span><span class="sxs-lookup"><span data-stu-id="9f985-156">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9f985-157">Se si usa l' [SDK Web](#sdk), un file *Web. config* , che in genere viene generato quando si pubblica un'app ASP.NET Core, non è necessario per un'app dei servizi Windows.</span><span class="sxs-lookup"><span data-stu-id="9f985-157">If using the [Web SDK](#sdk), a *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="9f985-158">Per disabilitare la creazione del file *web.config*, aggiungere la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="9f985-158">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="9f985-159">L'[identificatore di runtime (RID)](/dotnet/core/rid-catalog) di Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="9f985-159">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="9f985-160">Nell'esempio seguente il RID è impostato su `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="9f985-160">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="9f985-161">La proprietà `<SelfContained>` è impostata su `false`.</span><span class="sxs-lookup"><span data-stu-id="9f985-161">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="9f985-162">Queste proprietà indicano all'SDK di generare un file eseguibile (con estensione *exe*) per Windows e un'app che dipende dal framework .NET Core condiviso.</span><span class="sxs-lookup"><span data-stu-id="9f985-162">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="9f985-163">Un file *web.config*, che viene normalmente generato quando si pubblica un'app ASP.NET Core, non è necessario per un'app di servizi Windows.</span><span class="sxs-lookup"><span data-stu-id="9f985-163">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="9f985-164">Per disabilitare la creazione del file *web.config*, aggiungere la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="9f985-164">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="9f985-165">L'[identificatore di runtime (RID)](/dotnet/core/rid-catalog) di Windows ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="9f985-165">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="9f985-166">Nell'esempio seguente il RID è impostato su `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="9f985-166">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="9f985-167">La proprietà `<SelfContained>` è impostata su `false`.</span><span class="sxs-lookup"><span data-stu-id="9f985-167">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="9f985-168">Queste proprietà indicano all'SDK di generare un file eseguibile (con estensione *exe*) per Windows e un'app che dipende dal framework .NET Core condiviso.</span><span class="sxs-lookup"><span data-stu-id="9f985-168">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="9f985-169">La proprietà `<UseAppHost>` è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="9f985-169">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="9f985-170">Questa proprietà fornisce il servizio con un percorso di attivazione (un file eseguibile, *.exe*) per una distribuzione dipendente dal framework.</span><span class="sxs-lookup"><span data-stu-id="9f985-170">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="9f985-171">Un file *web.config*, che viene normalmente generato quando si pubblica un'app ASP.NET Core, non è necessario per un'app di servizi Windows.</span><span class="sxs-lookup"><span data-stu-id="9f985-171">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="9f985-172">Per disabilitare la creazione del file *web.config*, aggiungere la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="9f985-172">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="9f985-173">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="9f985-173">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="9f985-174">Una distribuzione autonoma non si basa sulla presenza di un framework condiviso nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="9f985-174">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="9f985-175">Il runtime e le dipendenze dell'app vengono distribuiti con l'app.</span><span class="sxs-lookup"><span data-stu-id="9f985-175">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="9f985-176">Un [identificatore di runtime (RID)](/dotnet/core/rid-catalog) di Windows viene incluso nel `<PropertyGroup>` che contiene il framework di destinazione:</span><span class="sxs-lookup"><span data-stu-id="9f985-176">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="9f985-177">Per eseguire la pubblicazione per più identificatori di runtime:</span><span class="sxs-lookup"><span data-stu-id="9f985-177">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="9f985-178">Specificare gli identificatori di runtime in un elenco delimitato da punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="9f985-178">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="9f985-179">Usare il nome di proprietà [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plurale).</span><span class="sxs-lookup"><span data-stu-id="9f985-179">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="9f985-180">Per altre informazioni, vedere il [Catalogo RID di .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="9f985-180">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9f985-181">Una proprietà `<SelfContained>` è impostata su `true`:</span><span class="sxs-lookup"><span data-stu-id="9f985-181">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="9f985-182">Account utente del servizio</span><span class="sxs-lookup"><span data-stu-id="9f985-182">Service user account</span></span>

<span data-ttu-id="9f985-183">Per creare un account utente per un servizio, usare il cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) da una shell dei comandi di PowerShell 6 amministrativa.</span><span class="sxs-lookup"><span data-stu-id="9f985-183">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="9f985-184">In Aggiornamento di Windows 10 (ottobre 2018) (versione 1809/build 10.0.17763) o versioni successive:</span><span class="sxs-lookup"><span data-stu-id="9f985-184">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="9f985-185">In un sistema operativo Windows precedente ad Aggiornamento di Windows 10 (ottobre 2018) (versione 1809/build 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="9f985-185">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="9f985-186">Fornire una [password complessa](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="9f985-186">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="9f985-187">Se non viene specificato il parametro `-AccountExpires` per il cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) con un valore <xref:System.DateTime> di scadenza, l'account non scade.</span><span class="sxs-lookup"><span data-stu-id="9f985-187">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="9f985-188">Per altre informazioni, vedere [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) e [Service User Accounts](/windows/desktop/services/service-user-accounts) (Account utente di servizio).</span><span class="sxs-lookup"><span data-stu-id="9f985-188">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="9f985-189">Un approccio alternativo per la gestione degli utenti quando si usa Active Directory consiste nell'usare account del servizio gestito.</span><span class="sxs-lookup"><span data-stu-id="9f985-189">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="9f985-190">Per altre informazioni, vedere [Panoramica degli account del servizio gestito del gruppo](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="9f985-190">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="9f985-191">Diritti Accesso come servizio</span><span class="sxs-lookup"><span data-stu-id="9f985-191">Log on as a service rights</span></span>

<span data-ttu-id="9f985-192">Per stabilire i diritti *Accesso come servizio* per un account utente di servizio:</span><span class="sxs-lookup"><span data-stu-id="9f985-192">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="9f985-193">Aprire l'editor Criteri di sicurezza locali eseguendo *secpol.msc*.</span><span class="sxs-lookup"><span data-stu-id="9f985-193">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="9f985-194">Espandere il nodo **Criteri locali** e selezionare **Assegnazione diritti utente**.</span><span class="sxs-lookup"><span data-stu-id="9f985-194">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="9f985-195">Aprire il criterio **Accesso come servizio**.</span><span class="sxs-lookup"><span data-stu-id="9f985-195">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="9f985-196">Selezionare **Aggiungi utente o gruppo**.</span><span class="sxs-lookup"><span data-stu-id="9f985-196">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="9f985-197">Specificare il nome oggetto (account utente) in uno dei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f985-197">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="9f985-198">Digitare l'account utente (`{DOMAIN OR COMPUTER NAME\USER}`) nel campo del nome oggetto e scegliere **OK** per aggiungere l'utente al criterio.</span><span class="sxs-lookup"><span data-stu-id="9f985-198">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="9f985-199">Fare clic su **Avanzate**.</span><span class="sxs-lookup"><span data-stu-id="9f985-199">Select **Advanced**.</span></span> <span data-ttu-id="9f985-200">Selezionare **Trova**.</span><span class="sxs-lookup"><span data-stu-id="9f985-200">Select **Find Now**.</span></span> <span data-ttu-id="9f985-201">Selezionare l'account utente dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="9f985-201">Select the user account from the list.</span></span> <span data-ttu-id="9f985-202">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f985-202">Select **OK**.</span></span> <span data-ttu-id="9f985-203">Scegliere di nuovo **OK** per aggiungere l'utente al criterio.</span><span class="sxs-lookup"><span data-stu-id="9f985-203">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="9f985-204">Scegliere **OK** o **Applica** per accettare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="9f985-204">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="9f985-205">Creare e gestire il servizio di Windows</span><span class="sxs-lookup"><span data-stu-id="9f985-205">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="9f985-206">Creare un servizio</span><span class="sxs-lookup"><span data-stu-id="9f985-206">Create a service</span></span>

<span data-ttu-id="9f985-207">Usare i comandi di PowerShell per registrare un servizio.</span><span class="sxs-lookup"><span data-stu-id="9f985-207">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="9f985-208">Da una shell dei comandi di PowerShell 6 amministrativa eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f985-208">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="9f985-209">`{EXE PATH}` &ndash; percorso alla cartella dell'app nell'host, ad esempio `d:\myservice`.</span><span class="sxs-lookup"><span data-stu-id="9f985-209">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="9f985-210">Non includere l'eseguibile dell'app nel percorso.</span><span class="sxs-lookup"><span data-stu-id="9f985-210">Don't include the app's executable in the path.</span></span> <span data-ttu-id="9f985-211">Non è necessario aggiungere una barra finale.</span><span class="sxs-lookup"><span data-stu-id="9f985-211">A trailing slash isn't required.</span></span>
* <span data-ttu-id="9f985-212">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; account utente del servizio, ad esempio `Contoso\ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="9f985-212">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="9f985-213">`{SERVICE NAME}` &ndash; nome del servizio (ad esempio, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="9f985-213">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="9f985-214">`{EXE FILE PATH}` &ndash; percorso eseguibile dell'app, ad esempio `d:\myservice\myservice.exe`.</span><span class="sxs-lookup"><span data-stu-id="9f985-214">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="9f985-215">Includere il nome del file eseguibile con l'estensione.</span><span class="sxs-lookup"><span data-stu-id="9f985-215">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="9f985-216">`{DESCRIPTION}` Descrizione del servizio &ndash;, ad esempio `My sample service`.</span><span class="sxs-lookup"><span data-stu-id="9f985-216">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="9f985-217">`{DISPLAY NAME}` nome visualizzato del servizio &ndash;, ad esempio `My Service`.</span><span class="sxs-lookup"><span data-stu-id="9f985-217">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="9f985-218">Avviare un servizio</span><span class="sxs-lookup"><span data-stu-id="9f985-218">Start a service</span></span>

<span data-ttu-id="9f985-219">Per avviare un servizio, usare il comando di PowerShell 6 seguente:</span><span class="sxs-lookup"><span data-stu-id="9f985-219">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="9f985-220">L'avvio del servizio richiede alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="9f985-220">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="9f985-221">Determinare lo stato di un servizio</span><span class="sxs-lookup"><span data-stu-id="9f985-221">Determine a service's status</span></span>

<span data-ttu-id="9f985-222">Per verificare lo stato di un servizio, usare il comando di PowerShell 6 seguente:</span><span class="sxs-lookup"><span data-stu-id="9f985-222">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="9f985-223">Lo stato viene indicato con uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f985-223">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="9f985-224">Arrestare un servizio</span><span class="sxs-lookup"><span data-stu-id="9f985-224">Stop a service</span></span>

<span data-ttu-id="9f985-225">Per arrestare un servizio, usare il comando di PowerShell 6 seguente:</span><span class="sxs-lookup"><span data-stu-id="9f985-225">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="9f985-226">Rimuovere un servizio</span><span class="sxs-lookup"><span data-stu-id="9f985-226">Remove a service</span></span>

<span data-ttu-id="9f985-227">Dopo il breve lasso di tempo necessario per arrestare un servizio, rimuovere il servizio con il comando di PowerShell 6 seguente:</span><span class="sxs-lookup"><span data-stu-id="9f985-227">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="9f985-228">Gestire gli eventi di avvio e arresto</span><span class="sxs-lookup"><span data-stu-id="9f985-228">Handle starting and stopping events</span></span>

<span data-ttu-id="9f985-229">Per gestire gli eventi <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> e <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>:</span><span class="sxs-lookup"><span data-stu-id="9f985-229">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="9f985-230">Creare una classe che deriva da <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con i metodi `OnStarting`, `OnStarted` e `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="9f985-230">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="9f985-231">Creare un metodo di estensione per <xref:Microsoft.AspNetCore.Hosting.IWebHost> che passa `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="9f985-231">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="9f985-232">In `Program.Main` chiamare il metodo di estensione `RunAsCustomService` invece di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="9f985-232">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="9f985-233">Per visualizzare il percorso di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, fare riferimento all'esempio di codice illustrato nella sezione [Tipo di distribuzione](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="9f985-233">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="9f985-234">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="9f985-234">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="9f985-235">I servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o un servizio di bilanciamento del carico potrebbero richiedere una configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="9f985-235">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="9f985-236">Per altre informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="9f985-236">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="9f985-237">Configurare gli endpoint</span><span class="sxs-lookup"><span data-stu-id="9f985-237">Configure endpoints</span></span>

<span data-ttu-id="9f985-238">Per impostazione predefinita, ASP.NET Core è associato a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="9f985-238">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="9f985-239">Configurare l'URL e la porta impostando la variabile di ambiente `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="9f985-239">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="9f985-240">Per ulteriori approcci alla configurazione di porte e URL, vedere l'articolo relativo al server pertinente:</span><span class="sxs-lookup"><span data-stu-id="9f985-240">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="9f985-241">Le linee guida precedenti riguardano il supporto per gli endpoint HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9f985-241">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="9f985-242">Ad esempio, configurare l'app per HTTPS quando si usa l'autenticazione con un servizio Windows.</span><span class="sxs-lookup"><span data-stu-id="9f985-242">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="9f985-243">L'uso del certificato di sviluppo ASP.NET Core HTTPS per proteggere un endpoint del servizio non è supportato.</span><span class="sxs-lookup"><span data-stu-id="9f985-243">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="9f985-244">Directory corrente e radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="9f985-244">Current directory and content root</span></span>

<span data-ttu-id="9f985-245">La directory di lavoro corrente restituita chiamando <xref:System.IO.Directory.GetCurrentDirectory*> per un servizio Windows è la cartella *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="9f985-245">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="9f985-246">La cartella *system32* non è un percorso appropriato per archiviare i file di un servizio, ad esempio i file di impostazioni.</span><span class="sxs-lookup"><span data-stu-id="9f985-246">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="9f985-247">Usare uno degli approcci seguenti per gestire e accedere agli asset e ai file di impostazioni di un servizio.</span><span class="sxs-lookup"><span data-stu-id="9f985-247">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="9f985-248">Usare ContentRootPath o ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="9f985-248">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="9f985-249">Usare [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) o <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> per individuare le risorse di un'app.</span><span class="sxs-lookup"><span data-stu-id="9f985-249">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

<span data-ttu-id="9f985-250">Quando l'app viene eseguita come servizio, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> imposta l'<xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> su [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory).</span><span class="sxs-lookup"><span data-stu-id="9f985-250">When the app runs as a service, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> sets the <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span>

<span data-ttu-id="9f985-251">File di impostazioni predefinite dell'app, *appSettings. JSON* e *appSettings. { Environment}. JSON*, viene caricato dalla radice del contenuto dell'app chiamando [CreateDefaultBuilder durante la costruzione dell'host](xref:fundamentals/host/generic-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="9f985-251">The app's default settings files, *appsettings.json* and *appsettings.{Environment}.json*, are loaded from the app's content root by calling [CreateDefaultBuilder during host construction](xref:fundamentals/host/generic-host#set-up-a-host).</span></span>

<span data-ttu-id="9f985-252">Per gli altri file di impostazioni caricati dal codice dello sviluppatore in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, non è necessario chiamare <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="9f985-252">For other settings files loaded by developer code in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, there's no need to call <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="9f985-253">Nell'esempio seguente il file *custom_settings. JSON* esiste nella radice del contenuto dell'app e viene caricato senza impostare esplicitamente un percorso di base:</span><span class="sxs-lookup"><span data-stu-id="9f985-253">In the following example, the *custom_settings.json* file exists in the app's content root and is loaded without explicitly setting a base path:</span></span>

[!code-csharp[](windows-service/samples_snapshot/CustomSettingsExample.cs?highlight=13)]

<span data-ttu-id="9f985-254">Non tentare di usare <xref:System.IO.Directory.GetCurrentDirectory*> per ottenere un percorso di risorsa perché un'app di servizio Windows restituisce la cartella *C:\\Windows\\system32* come directory corrente.</span><span class="sxs-lookup"><span data-stu-id="9f985-254">Don't attempt to use <xref:System.IO.Directory.GetCurrentDirectory*> to obtain a resource path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder as its current directory.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="9f985-255">Impostare il percorso radice del contenuto sulla cartella dell'app</span><span class="sxs-lookup"><span data-stu-id="9f985-255">Set the content root path to the app's folder</span></span>

<span data-ttu-id="9f985-256"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> è lo stesso percorso fornito all'argomento `binPath` durante la creazione di un servizio.</span><span class="sxs-lookup"><span data-stu-id="9f985-256">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="9f985-257">Anziché chiamare `GetCurrentDirectory` per creare percorsi per i file di impostazioni, chiamare <xref:System.IO.Directory.SetCurrentDirectory*> con il percorso della radice del [contenuto](xref:fundamentals/index#content-root)dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f985-257">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="9f985-258">In `Program.Main`, determinare il percorso della cartella dell'eseguibile del servizio e usare il percorso per stabilire la radice del contenuto dell'app:</span><span class="sxs-lookup"><span data-stu-id="9f985-258">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="9f985-259">Archiviare i file di un servizio in un percorso appropriato nel disco</span><span class="sxs-lookup"><span data-stu-id="9f985-259">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="9f985-260">Specificare un percorso assoluto con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> quando si usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> per la cartella contenente i file.</span><span class="sxs-lookup"><span data-stu-id="9f985-260">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="9f985-261">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="9f985-261">Troubleshoot</span></span>

<span data-ttu-id="9f985-262">Per risolvere i problemi relativi a un'app di servizio Windows, vedere <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="9f985-262">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="9f985-263">Errori comuni</span><span class="sxs-lookup"><span data-stu-id="9f985-263">Common errors</span></span>

* <span data-ttu-id="9f985-264">È in uso una versione precedente o provvisoria di PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9f985-264">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="9f985-265">Il servizio registrato non usa l'output **pubblicato** dell'app dal comando [DotNet Publish](/dotnet/core/tools/dotnet-publish) .</span><span class="sxs-lookup"><span data-stu-id="9f985-265">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="9f985-266">L'output del comando [DotNet Build](/dotnet/core/tools/dotnet-build) non è supportato per la distribuzione di app.</span><span class="sxs-lookup"><span data-stu-id="9f985-266">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="9f985-267">Gli asset pubblicati si trovano in una delle cartelle seguenti, a seconda del tipo di distribuzione:</span><span class="sxs-lookup"><span data-stu-id="9f985-267">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="9f985-268">*bin/Release/{Target Framework}/Publish* (FDD)</span><span class="sxs-lookup"><span data-stu-id="9f985-268">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="9f985-269">*bin/Release/{Target Framework}/{Runtime Identifier}/Publish* (SCD)</span><span class="sxs-lookup"><span data-stu-id="9f985-269">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="9f985-270">Il servizio non è nello stato in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="9f985-270">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="9f985-271">I percorsi delle risorse utilizzate dall'app, ad esempio i certificati, non sono corretti.</span><span class="sxs-lookup"><span data-stu-id="9f985-271">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="9f985-272">Il percorso di base di un servizio Windows è *c:\\Windows\\system32*.</span><span class="sxs-lookup"><span data-stu-id="9f985-272">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="9f985-273">L'utente non dispone *di diritti di accesso come servizio* .</span><span class="sxs-lookup"><span data-stu-id="9f985-273">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="9f985-274">La password dell'utente è scaduta o non è stata passata correttamente durante l'esecuzione del comando `New-Service` PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9f985-274">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="9f985-275">L'app richiede l'autenticazione ASP.NET Core ma non è configurata per le connessioni protette (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="9f985-275">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="9f985-276">La porta dell'URL della richiesta non è corretta o non è configurata correttamente nell'app.</span><span class="sxs-lookup"><span data-stu-id="9f985-276">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="9f985-277">Log eventi di sistema e dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="9f985-277">System and Application Event Logs</span></span>

<span data-ttu-id="9f985-278">Accedere ai registri eventi di sistema e dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="9f985-278">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="9f985-279">Aprire il menu Start, cercare *Visualizzatore eventi*e selezionare l'app **Visualizzatore eventi** .</span><span class="sxs-lookup"><span data-stu-id="9f985-279">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="9f985-280">In **Visualizzatore eventi** aprire il nodo **Registri di Windows**.</span><span class="sxs-lookup"><span data-stu-id="9f985-280">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="9f985-281">Selezionare **sistema** per aprire il registro eventi di sistema.</span><span class="sxs-lookup"><span data-stu-id="9f985-281">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="9f985-282">Selezionare **Applicazione** per aprire il log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9f985-282">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="9f985-283">Cercare gli errori associati all'app in cui si è verificato il problema.</span><span class="sxs-lookup"><span data-stu-id="9f985-283">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="9f985-284">Eseguire l'app da un prompt dei comandi</span><span class="sxs-lookup"><span data-stu-id="9f985-284">Run the app at a command prompt</span></span>

<span data-ttu-id="9f985-285">Molti errori di avvio non producono informazioni utili nei log eventi.</span><span class="sxs-lookup"><span data-stu-id="9f985-285">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="9f985-286">È possibile individuare la causa di alcuni errori eseguendo l'app da un prompt dei comandi nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="9f985-286">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="9f985-287">Per registrare dettagli aggiuntivi dall'app, abbassare il [livello di registrazione](xref:fundamentals/logging/index#log-level) o eseguire l'app nell' [ambiente di sviluppo](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="9f985-287">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="9f985-288">Cancella cache di pacchetti</span><span class="sxs-lookup"><span data-stu-id="9f985-288">Clear package caches</span></span>

<span data-ttu-id="9f985-289">Un'app funzionante potrebbe non riuscire immediatamente dopo l'aggiornamento del .NET Core SDK nel computer di sviluppo o la modifica delle versioni del pacchetto all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f985-289">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="9f985-290">In alcuni casi i pacchetti incoerenti possono interrompere un'app quando si eseguono aggiornamenti principali.</span><span class="sxs-lookup"><span data-stu-id="9f985-290">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="9f985-291">La maggior parte di questi problemi può essere risolta attenendosi alle istruzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f985-291">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="9f985-292">Eliminare le cartelle *bin* e *obj*.</span><span class="sxs-lookup"><span data-stu-id="9f985-292">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="9f985-293">Cancellare le cache dei pacchetti eseguendo [le impostazioni locali di DotNet NuGet All--Clear](/dotnet/core/tools/dotnet-nuget-locals) da una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="9f985-293">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="9f985-294">La cancellazione delle cache dei pacchetti può essere eseguita anche con lo strumento [NuGet. exe](https://www.nuget.org/downloads) ed eseguendo il comando `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="9f985-294">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="9f985-295">*nuget.exe* non è un'installazione inclusa con il sistema operativo desktop Windows e deve essere ottenuta separatamente dal [sito Web NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="9f985-295">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="9f985-296">Ripristinare e ricompilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="9f985-296">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="9f985-297">Eliminare tutti i file nella cartella di distribuzione nel server prima di ridistribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="9f985-297">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="9f985-298">App lenta o bloccata</span><span class="sxs-lookup"><span data-stu-id="9f985-298">Slow or hanging app</span></span>

<span data-ttu-id="9f985-299">Un *dump di arresto anomalo* del sistema è uno snapshot della memoria del sistema e può contribuire a determinare la provocazione di un arresto anomalo dell'app, dell'avvio o dell'applicazione lenta.</span><span class="sxs-lookup"><span data-stu-id="9f985-299">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="9f985-300">Arresto anomalo o eccezione di un'app</span><span class="sxs-lookup"><span data-stu-id="9f985-300">App crashes or encounters an exception</span></span>

<span data-ttu-id="9f985-301">Ottenere e analizzare un dump da [Segnalazione errori Windows](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="9f985-301">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="9f985-302">Creare una cartella per i file dump di arresto anomalo del sistema in `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="9f985-302">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="9f985-303">Eseguire lo [script di PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) con il nome dell'eseguibile dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="9f985-303">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="9f985-304">Eseguire l'app nelle condizioni che causano l'arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="9f985-304">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="9f985-305">Dopo che si è verificato l'arresto anomalo, eseguire lo [script di PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="9f985-305">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="9f985-306">Dopo l'arresto anomalo di un'app e la raccolta dei dump, l'app può terminare normalmente.</span><span class="sxs-lookup"><span data-stu-id="9f985-306">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="9f985-307">Lo script di PowerShell configura Segnalazione errori Windows per raccogliere fino a cinque dump per ogni app.</span><span class="sxs-lookup"><span data-stu-id="9f985-307">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="9f985-308">I dump di arresto anomalo del sistema potrebbero richiedere una grande quantità di spazio su disco (fino a diversi gigabyte ognuno).</span><span class="sxs-lookup"><span data-stu-id="9f985-308">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="9f985-309">L'app si blocca, si verifica un errore durante l'avvio o viene eseguita normalmente</span><span class="sxs-lookup"><span data-stu-id="9f985-309">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="9f985-310">Quando un'app si *blocca* (smette di rispondere ma non si arresta in modo anomalo), si verifica un errore durante l'avvio o viene eseguita normalmente, vedere [file di dump in modalità utente: scegliere lo strumento migliore](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) per selezionare uno strumento appropriato per produrre il dump.</span><span class="sxs-lookup"><span data-stu-id="9f985-310">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="9f985-311">Analizzare il dump</span><span class="sxs-lookup"><span data-stu-id="9f985-311">Analyze the dump</span></span>

<span data-ttu-id="9f985-312">È possibile analizzare un dump usando diversi approcci.</span><span class="sxs-lookup"><span data-stu-id="9f985-312">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="9f985-313">Per altre informazioni, vedere [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analisi di un file dump in modalità utente).</span><span class="sxs-lookup"><span data-stu-id="9f985-313">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f985-314">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9f985-314">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="9f985-315">[Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)</span><span class="sxs-lookup"><span data-stu-id="9f985-315">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="9f985-316">[Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)</span><span class="sxs-lookup"><span data-stu-id="9f985-316">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
