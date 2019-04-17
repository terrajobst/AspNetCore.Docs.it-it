---
title: Ospitare ASP.NET Core in un servizio Windows
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/04/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 544eefa87898e82ec2bf8f9f61ce4e26dd554bb7
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068336"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="e1ba5-103">Ospitare ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="e1ba5-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="e1ba5-104">Di [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="e1ba5-105">È possibile ospitare un'app ASP.NET Core in Windows come [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) senza usare IIS.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="e1ba5-106">Quando è ospitata come servizio Windows, l'app viene avviata automaticamente dopo ogni riavvio.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="e1ba5-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e1ba5-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1ba5-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e1ba5-108">Prerequisites</span></span>

* [<span data-ttu-id="e1ba5-109">PowerShell 6.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="e1ba5-109">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

> [!NOTE]
> <span data-ttu-id="e1ba5-110">Per il sistema operativo Windows OS precedente all'aggiornamento di Windows 10 di ottobre 2018 (versione 1809/build 10.0.17763), il modulo [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) deve essere importato con il [modulo WindowsCompatibility](https://github.com/PowerShell/WindowsCompatibility) per ottenere accesso al cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) usato nella sezione [Creare un account utente](#create-a-user-account):</span><span class="sxs-lookup"><span data-stu-id="e1ba5-110">For Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763), the [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) module must be imported with the [WindowsCompatibility module](https://github.com/PowerShell/WindowsCompatibility) to gain access to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet used in the [Create a user account](#create-a-user-account) section:</span></span>
>
> ```powershell
> Install-Module WindowsCompatibility -Scope CurrentUser
> Import-WinModule Microsoft.PowerShell.LocalAccounts
> ```

## <a name="deployment-type"></a><span data-ttu-id="e1ba5-111">Tipo di distribuzione</span><span class="sxs-lookup"><span data-stu-id="e1ba5-111">Deployment type</span></span>

<span data-ttu-id="e1ba5-112">È possibile creare una distribuzione di Windows dipendente dal framework o autonoma.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-112">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="e1ba5-113">Per informazioni e consigli sugli scenari di distribuzione, vedere [Distribuzione di applicazioni .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-113">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="e1ba5-114">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="e1ba5-114">Framework-dependent deployment</span></span>

<span data-ttu-id="e1ba5-115">La distribuzione dipendente dal framework si basa sulla presenza di una versione condivisa a livello di sistema di .NET Core nel sistema di destinazione.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-115">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="e1ba5-116">Quando lo scenario di distribuzione dipendente dal framework viene usato con un'app servizio Windows ASP.NET Core, l'SDK genera un file eseguibile (*\*.exe*), denominato *eseguibile dipendente dal framework*.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-116">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="e1ba5-117">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="e1ba5-117">Self-contained deployment</span></span>

<span data-ttu-id="e1ba5-118">Una distribuzione autonoma non si basa sulla presenza dei componenti condivisi nel sistema di destinazione.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-118">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="e1ba5-119">Il runtime e le dipendenze dell'app vengono distribuiti con l'app nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-119">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="e1ba5-120">Convertire un progetto in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="e1ba5-120">Convert a project into a Windows Service</span></span>

<span data-ttu-id="e1ba5-121">Apportare le modifiche seguenti a un progetto ASP.NET Core esistente per eseguire l'app come servizio:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-121">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="e1ba5-122">Aggiornamenti ai file di progetto</span><span class="sxs-lookup"><span data-stu-id="e1ba5-122">Project file updates</span></span>

<span data-ttu-id="e1ba5-123">In base al [tipo di distribuzione](#deployment-type) scelto, aggiornare il file di progetto:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-123">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="e1ba5-124">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="e1ba5-124">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="e1ba5-125">Aggiungere un [RID (Runtime Identifier)](/dotnet/core/rid-catalog) Windows al `<PropertyGroup>` che contiene il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-125">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="e1ba5-126">Nell'esempio seguente il RID è impostato su `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-126">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="e1ba5-127">Aggiungere la proprietà `<SelfContained>` impostata su `false`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-127">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="e1ba5-128">Queste proprietà indicano all'SDK di generare un file eseguibile (*.exe*) per Windows.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-128">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="e1ba5-129">Un file *web.config*, che viene normalmente generato quando si pubblica un'app ASP.NET Core, non è necessario per un'app di servizi Windows.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-129">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="e1ba5-130">Per disabilitare la creazione del file *web.config*, aggiungere la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-130">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

::: moniker range=">= aspnetcore-2.2"

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

<span data-ttu-id="e1ba5-131">Aggiungere la proprietà `<UseAppHost>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-131">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="e1ba5-132">Questa proprietà fornisce il servizio con un percorso di attivazione (un file eseguibile, *.exe*) per una distribuzione dipendente dal framework.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-132">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="e1ba5-133">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="e1ba5-133">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="e1ba5-134">Verificare la presenza di un [RID](/dotnet/core/rid-catalog) di Windows o aggiungere un RID al `<PropertyGroup>` che contiene il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-134">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="e1ba5-135">Disabilitare la creazione di un file *web.config* aggiungendo la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-135">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="e1ba5-136">Per eseguire la pubblicazione per più identificatori di runtime:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-136">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="e1ba5-137">Specificare gli identificatori di runtime in un elenco delimitato da punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-137">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="e1ba5-138">Usare il nome di proprietà `<RuntimeIdentifiers>` (plurale).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-138">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="e1ba5-139">Per altre informazioni, vedere il [Catalogo RID di .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-139">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="e1ba5-140">Aggiungere un riferimento al pacchetto per [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-140">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="e1ba5-141">Per abilitare la registrazione nel registro eventi di Windows, aggiungere un riferimento al pacchetto per [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-141">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="e1ba5-142">Per altre informazioni, vedere la sezione [Gestire gli eventi di avvio e arresto](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-142">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="e1ba5-143">Aggiornamenti a Program.Main</span><span class="sxs-lookup"><span data-stu-id="e1ba5-143">Program.Main updates</span></span>

<span data-ttu-id="e1ba5-144">Modificare `Program.Main` nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-144">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="e1ba5-145">Per eseguire test e debug durante l'esecuzione all'esterno di un servizio, aggiungere il codice per determinare se l'app è in esecuzione come servizio o è un'app console.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-145">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="e1ba5-146">Controllare se il debugger è collegato o se è presente un argomento della riga di comando `--console`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-146">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="e1ba5-147">Se una delle due condizioni è vera (l'app non è eseguita come servizio), chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> sull'host Web.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-147">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="e1ba5-148">Se le condizioni sono false (l'app è eseguita come servizio):</span><span class="sxs-lookup"><span data-stu-id="e1ba5-148">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="e1ba5-149">Chiamare <xref:System.IO.Directory.SetCurrentDirectory*> e usare un percorso per la posizione di pubblicazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-149">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="e1ba5-150">Non chiamare <xref:System.IO.Directory.GetCurrentDirectory*> per ottenere il percorso perché un'app servizio Windows restituisce la cartella *C:\\WINDOWS\\system32* quando viene chiamato <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-150">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="e1ba5-151">Per altre informazioni, vedere la sezione [Directory corrente e radice del contenuto](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-151">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="e1ba5-152">Chiamare <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> per eseguire l'app come servizio.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-152">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="e1ba5-153">Dato che il [provider di configurazione della riga di comando](xref:fundamentals/configuration/index#command-line-configuration-provider) richiede coppie nome-valore per gli argomenti della riga di comando, l'opzione `--console` viene rimossa dagli argomenti prima che <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> li riceva.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-153">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="e1ba5-154">Per scrivere nel registro eventi di Windows, aggiungere il provider EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-154">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="e1ba5-155">Impostare il livello di registrazione con la chiave `Logging:LogLevel:Default` nel file *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-155">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="e1ba5-156">Per scopi di testing e dimostrazione, il file di impostazioni di produzione dell'app di esempio imposta il livello di registrazione su `Information`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-156">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="e1ba5-157">In ambiente di produzione il valore viene in genere impostato su `Error`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-157">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="e1ba5-158">Per ulteriori informazioni, vedere <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-158">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a><span data-ttu-id="e1ba5-159">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="e1ba5-159">Publish the app</span></span>

<span data-ttu-id="e1ba5-160">Pubblicare l'app usando [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-160">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="e1ba5-161">Quando si usa Visual Studio, selezionare **FolderProfile** e configurare il **Percorso di destinazione** prima di selezionare il pulsante **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-161">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="e1ba5-162">Per pubblicare l'app di esempio con strumenti dell'interfaccia della riga di comando, eseguire il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) in una shell dei comandi di Windows dalla cartella del progetto passando una configurazione Versione all'opzione [-c|--configuration](/dotnet/core/tools/dotnet-publish#options).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-162">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command in a Windows command shell from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="e1ba5-163">Usare l'opzione [-o|--output](/dotnet/core/tools/dotnet-publish#options) con un percorso per pubblicare in una cartella all'esterno dell'app.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-163">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="e1ba5-164">Pubblicare una distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="e1ba5-164">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="e1ba5-165">Nell'esempio seguente l'app viene pubblicata nella cartella *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-165">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="e1ba5-166">Pubblicare una distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="e1ba5-166">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="e1ba5-167">L'identificatore di runtime deve essere specificato nella proprietà `<RuntimeIdenfifier>` (o `<RuntimeIdentifiers>`) del file di progetto.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-167">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="e1ba5-168">Specificare il runtime per l'opzione [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) del comando `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-168">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="e1ba5-169">Nell'esempio seguente l'app viene pubblicata per il runtime `win7-x64` nella cartella *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-169">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a><span data-ttu-id="e1ba5-170">Creare un account utente</span><span class="sxs-lookup"><span data-stu-id="e1ba5-170">Create a user account</span></span>

<span data-ttu-id="e1ba5-171">Creare un account utente per il servizio usando il cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) da una shell dei comandi di PowerShell 6 amministrativa:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-171">Create a user account for the service using the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell:</span></span>

```powershell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="e1ba5-172">Fornire una [password complessa](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-172">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="e1ba5-173">Per l'app di esempio, creare un account utente con il nome `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-173">For the sample app, create a user account with the name `ServiceUser`.</span></span>

```powershell
New-LocalUser -Name ServiceUser
```

<span data-ttu-id="e1ba5-174">Se non viene specificato il parametro `-AccountExpires` per il cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) con un valore <xref:System.DateTime> di scadenza, l'account non scade.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-174">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="e1ba5-175">Per altre informazioni, vedere [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) e [Service User Accounts](/windows/desktop/services/service-user-accounts) (Account utente di servizio).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-175">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="e1ba5-176">Un approccio alternativo per la gestione degli utenti quando si usa Active Directory consiste nell'usare account del servizio gestito.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-176">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="e1ba5-177">Per altre informazioni, vedere [Panoramica degli account del servizio gestito del gruppo](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-177">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="set-permission-log-on-as-a-service"></a><span data-ttu-id="e1ba5-178">Impostare l'autorizzazione: Accedi come servizio</span><span class="sxs-lookup"><span data-stu-id="e1ba5-178">Set permission: Log on as a service</span></span>

<span data-ttu-id="e1ba5-179">Concedere l'accesso per scrittura/lettura/esecuzione alla cartella dell'app usando il comando [icacls](/windows-server/administration/windows-commands/icacls) da una shell dei comandi amministrativa di PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-179">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "{PATH}" /grant "{USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS}" /t
```

* `{PATH}` <span data-ttu-id="e1ba5-180">&ndash; Percorso della cartella dell'app.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-180">&ndash; Path to the app's folder.</span></span>
* `{USER ACCOUNT}` <span data-ttu-id="e1ba5-181">&ndash; Account utente (SID).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-181">&ndash; The user account (SID).</span></span>
* `(OI)` <span data-ttu-id="e1ba5-182">&ndash; Il flag Eredità oggetto propaga le autorizzazioni ai file subordinati.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-182">&ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* `(CI)` <span data-ttu-id="e1ba5-183">&ndash; Il flag Eredità contenitore propaga le autorizzazioni alle cartelle subordinate.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-183">&ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* `{PERMISSION FLAGS}` <span data-ttu-id="e1ba5-184">&ndash; Imposta le autorizzazioni di accesso dell'app.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-184">&ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="e1ba5-185">Scrittura (`W`)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-185">Write (`W`)</span></span>
  * <span data-ttu-id="e1ba5-186">Lettura (`R`)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-186">Read (`R`)</span></span>
  * <span data-ttu-id="e1ba5-187">Esecuzione (`X`)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-187">Execute (`X`)</span></span>
  * <span data-ttu-id="e1ba5-188">Completo (`F`)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-188">Full (`F`)</span></span>
  * <span data-ttu-id="e1ba5-189">Modifica (`M`)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-189">Modify (`M`)</span></span>
* `/t` <span data-ttu-id="e1ba5-190">&ndash; Applicare in modo ricorsivo alle cartelle e ai file subordinati esistenti.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-190">&ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="e1ba5-191">Per l'app di esempio pubblicata nella cartella *c:\\svc* e l'account `ServiceUser` con autorizzazioni di scrittura/lettura/esecuzione, usare il comando seguente da una shell dei comandi amministrativa di PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-191">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "c:\svc" /grant "ServiceUser:(OI)(CI)WRX" /t
```

<span data-ttu-id="e1ba5-192">Per altre informazioni, vedere [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-192">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="create-the-service"></a><span data-ttu-id="e1ba5-193">Creare il servizio</span><span class="sxs-lookup"><span data-stu-id="e1ba5-193">Create the service</span></span>

<span data-ttu-id="e1ba5-194">Usare lo script di PowerShell [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) per la registrazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-194">Use the [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell script to register the service.</span></span> <span data-ttu-id="e1ba5-195">Da una shell dei comandi amministrativa di PowerShell 6, eseguire lo script con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-195">From an administrative PowerShell 6 command shell, execute the script with the following command:</span></span>

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Exe "{PATH TO EXE}\{ASSEMBLY NAME}.exe" 
    -User {DOMAIN\USER}
```

<span data-ttu-id="e1ba5-196">Nell'esempio seguente per l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-196">In the following example for the sample app:</span></span>

* <span data-ttu-id="e1ba5-197">Il servizio è denominato **MyService**.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-197">The service is named **MyService**.</span></span>
* <span data-ttu-id="e1ba5-198">Il servizio pubblicato risiede nella cartella *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-198">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="e1ba5-199">L'eseguibile dell'app è denominato *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-199">The app executable is named *SampleApp.exe*.</span></span>
* <span data-ttu-id="e1ba5-200">Il servizio viene eseguito nel contesto dell'account `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-200">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="e1ba5-201">Nel comando di esempio seguente il nome del computer locale è `Desktop-PC`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-201">In the following example command, the local machine name is `Desktop-PC`.</span></span> <span data-ttu-id="e1ba5-202">Sostituire `Desktop-PC` con il nome del computer o il dominio del sistema specifico.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-202">Replace `Desktop-PC` with the computer name or domain for your system.</span></span>

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Exe "c:\svc\SampleApp.exe" 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a><span data-ttu-id="e1ba5-203">Gestire il servizio</span><span class="sxs-lookup"><span data-stu-id="e1ba5-203">Manage the service</span></span>

### <a name="start-the-service"></a><span data-ttu-id="e1ba5-204">Avviare il servizio</span><span class="sxs-lookup"><span data-stu-id="e1ba5-204">Start the service</span></span>

<span data-ttu-id="e1ba5-205">Avviare il servizio con il comando di PowerShell 6 `Start-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-205">Start the service with the `Start-Service -Name {NAME}` PowerShell 6 command.</span></span>

<span data-ttu-id="e1ba5-206">Per avviare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-206">To start the sample app service, use the following command:</span></span>

```powershell
Start-Service -Name MyService
```

<span data-ttu-id="e1ba5-207">L'avvio del servizio richiede alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-207">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="e1ba5-208">Determinare lo stato del servizio</span><span class="sxs-lookup"><span data-stu-id="e1ba5-208">Determine the service status</span></span>

<span data-ttu-id="e1ba5-209">Per verificare lo stato del servizio, usare il comando di PowerShell 6`Get-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-209">To check the status of the service, use the `Get-Service -Name {NAME}` PowerShell 6 command.</span></span> <span data-ttu-id="e1ba5-210">Lo stato viene indicato con uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-210">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

<span data-ttu-id="e1ba5-211">Usare il comando seguente per verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-211">Use the following command to check the status of the sample app service:</span></span>

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="e1ba5-212">Individuare un servizio app Web</span><span class="sxs-lookup"><span data-stu-id="e1ba5-212">Browse a web app service</span></span>

<span data-ttu-id="e1ba5-213">Quando il servizio è nello stato `RUNNING` e se il servizio è un'app Web, selezionare l'app dal suo percorso (per impostazione predefinita, `http://localhost:5000`, che reindirizza a `https://localhost:5001` quando si utilizza [il middleware di reindirizzamento HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-213">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="e1ba5-214">Per il servizio app di esempio, selezionare l'app in `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-214">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="e1ba5-215">Arresta il servizio</span><span class="sxs-lookup"><span data-stu-id="e1ba5-215">Stop the service</span></span>

<span data-ttu-id="e1ba5-216">Arrestare il servizio con il comando di Powershell 6 `Stop-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-216">Stop the service with the `Stop-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="e1ba5-217">Per arrestare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-217">The following command stops the sample app service:</span></span>

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a><span data-ttu-id="e1ba5-218">Rimuovere il servizio</span><span class="sxs-lookup"><span data-stu-id="e1ba5-218">Remove the service</span></span>

<span data-ttu-id="e1ba5-219">Dopo il breve lasso di tempo necessario per arrestare il servizio, rimuovere il servizio con il comando di PowerShell 6`Remove-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-219">After a short delay to stop a service, remove the service with the `Remove-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="e1ba5-220">Verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-220">Check the status of the sample app service:</span></span>

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="e1ba5-221">Gestire gli eventi di avvio e arresto</span><span class="sxs-lookup"><span data-stu-id="e1ba5-221">Handle starting and stopping events</span></span>

<span data-ttu-id="e1ba5-222">Per gestire gli eventi <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> e <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, apportare le modifiche aggiuntive seguenti:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-222">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="e1ba5-223">Creare una classe che deriva da <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con i metodi `OnStarting`, `OnStarted` e `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-223">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="e1ba5-224">Creare un metodo di estensione per <xref:Microsoft.AspNetCore.Hosting.IWebHost> che passa `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-224">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="e1ba5-225">In `Program.Main` chiamare il metodo di estensione `RunAsCustomService` invece di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-225">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="e1ba5-226">Per visualizzare il percorso di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, fare riferimento all'esempio di codice illustrato nella sezione [Convertire un progetto in un servizio Windows](#convert-a-project-into-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="e1ba5-226">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="e1ba5-227">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="e1ba5-227">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="e1ba5-228">I servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o un servizio di bilanciamento del carico potrebbero richiedere una configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-228">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="e1ba5-229">Per ulteriori informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-229">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="e1ba5-230">Configurare HTTPS</span><span class="sxs-lookup"><span data-stu-id="e1ba5-230">Configure HTTPS</span></span>

<span data-ttu-id="e1ba5-231">Per configurare il servizio con un endpoint sicuro:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-231">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="e1ba5-232">Creare un certificato X.509 per il sistema di hosting usando i meccanismi di acquisizione e distribuzione dei certificati della piattaforma.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-232">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="e1ba5-233">Specificare una [configurazione dell'endpoint HTTPS del server Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) per usare il certificato.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-233">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="e1ba5-234">L'uso del certificato di sviluppo ASP.NET Core HTTPS per proteggere un endpoint del servizio non è supportato.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-234">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="e1ba5-235">Directory corrente e radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="e1ba5-235">Current directory and content root</span></span>

<span data-ttu-id="e1ba5-236">La directory di lavoro corrente restituita chiamando <xref:System.IO.Directory.GetCurrentDirectory*> per un servizio Windows è la cartella *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-236">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="e1ba5-237">La cartella *system32* non è un percorso appropriato per archiviare i file di un servizio, ad esempio i file di impostazioni.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-237">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="e1ba5-238">Usare uno degli approcci seguenti per gestire e accedere agli asset e ai file di impostazioni di un servizio.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-238">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="e1ba5-239">Impostare il percorso radice del contenuto sulla cartella dell'app</span><span class="sxs-lookup"><span data-stu-id="e1ba5-239">Set the content root path to the app's folder</span></span>

<span data-ttu-id="e1ba5-240">L'elemento <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> è lo stesso percorso fornito all'argomento `binPath` durante la creazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-240">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="e1ba5-241">Invece di chiamare `GetCurrentDirectory` per creare i percorsi dei file di impostazioni, chiamare <xref:System.IO.Directory.SetCurrentDirectory*> con il percorso radice del contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-241">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="e1ba5-242">In `Program.Main`, determinare il percorso della cartella dell'eseguibile del servizio e usare il percorso per stabilire la radice del contenuto dell'app:</span><span class="sxs-lookup"><span data-stu-id="e1ba5-242">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="e1ba5-243">Archiviare i file del servizio in un percorso appropriato nel disco</span><span class="sxs-lookup"><span data-stu-id="e1ba5-243">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="e1ba5-244">Specificare un percorso assoluto con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> quando si usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> per la cartella contenente i file.</span><span class="sxs-lookup"><span data-stu-id="e1ba5-244">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1ba5-245">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e1ba5-245">Additional resources</span></span>

* <span data-ttu-id="e1ba5-246">[Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)</span><span class="sxs-lookup"><span data-stu-id="e1ba5-246">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
