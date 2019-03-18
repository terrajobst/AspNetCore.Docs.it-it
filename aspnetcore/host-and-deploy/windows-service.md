---
title: Ospitare ASP.NET Core in un servizio Windows
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/08/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ecc7f3a8cd813c2803d03294e38d726905eeb1b8
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841423"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="0c0c2-103">Ospitare ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="0c0c2-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="0c0c2-104">Di [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0c0c2-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0c0c2-105">È possibile ospitare un'app ASP.NET Core in Windows come [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) senza usare IIS.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="0c0c2-106">Quando è ospitata come servizio Windows, l'app viene avviata automaticamente dopo ogni riavvio.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="0c0c2-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0c0c2-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c0c2-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="0c0c2-108">Prerequisites</span></span>

* [<span data-ttu-id="0c0c2-109">PowerShell 6</span><span class="sxs-lookup"><span data-stu-id="0c0c2-109">PowerShell 6</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="deployment-type"></a><span data-ttu-id="0c0c2-110">Tipo di distribuzione</span><span class="sxs-lookup"><span data-stu-id="0c0c2-110">Deployment type</span></span>

<span data-ttu-id="0c0c2-111">È possibile creare una distribuzione di Windows dipendente dal framework o autonoma.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-111">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="0c0c2-112">Per informazioni e consigli sugli scenari di distribuzione, vedere [Distribuzione di applicazioni .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-112">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="0c0c2-113">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="0c0c2-113">Framework-dependent deployment</span></span>

<span data-ttu-id="0c0c2-114">La distribuzione dipendente dal framework si basa sulla presenza di una versione condivisa a livello di sistema di .NET Core nel sistema di destinazione.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-114">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="0c0c2-115">Quando lo scenario di distribuzione dipendente dal framework viene usato con un'app servizio Windows ASP.NET Core, l'SDK genera un file eseguibile (*\*.exe*), denominato *eseguibile dipendente dal framework*.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-115">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="0c0c2-116">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="0c0c2-116">Self-contained deployment</span></span>

<span data-ttu-id="0c0c2-117">Una distribuzione autonoma non si basa sulla presenza dei componenti condivisi nel sistema di destinazione.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-117">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="0c0c2-118">Il runtime e le dipendenze dell'app vengono distribuiti con l'app nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-118">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="0c0c2-119">Convertire un progetto in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="0c0c2-119">Convert a project into a Windows Service</span></span>

<span data-ttu-id="0c0c2-120">Apportare le modifiche seguenti a un progetto ASP.NET Core esistente per eseguire l'app come servizio:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-120">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="0c0c2-121">Aggiornamenti ai file di progetto</span><span class="sxs-lookup"><span data-stu-id="0c0c2-121">Project file updates</span></span>

<span data-ttu-id="0c0c2-122">In base al [tipo di distribuzione](#deployment-type) scelto, aggiornare il file di progetto:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-122">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="0c0c2-123">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="0c0c2-123">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="0c0c2-124">Aggiungere un [RID (Runtime Identifier)](/dotnet/core/rid-catalog) Windows al `<PropertyGroup>` che contiene il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-124">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="0c0c2-125">Nell'esempio seguente il RID è impostato su `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-125">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="0c0c2-126">Aggiungere la proprietà `<SelfContained>` impostata su `false`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-126">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="0c0c2-127">Queste proprietà indicano all'SDK di generare un file eseguibile (*.exe*) per Windows.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-127">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="0c0c2-128">Un file *web.config*, che viene normalmente generato quando si pubblica un'app ASP.NET Core, non è necessario per un'app di servizi Windows.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-128">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="0c0c2-129">Per disabilitare la creazione del file *web.config*, aggiungere la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-129">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="0c0c2-130">Aggiungere la proprietà `<UseAppHost>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-130">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="0c0c2-131">Questa proprietà fornisce il servizio con un percorso di attivazione (un file eseguibile, *.exe*) per una distribuzione dipendente dal framework.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-131">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="0c0c2-132">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="0c0c2-132">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="0c0c2-133">Verificare la presenza di un [RID](/dotnet/core/rid-catalog) di Windows o aggiungere un RID al `<PropertyGroup>` che contiene il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-133">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="0c0c2-134">Disabilitare la creazione di un file *web.config* aggiungendo la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-134">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="0c0c2-135">Per eseguire la pubblicazione per più identificatori di runtime:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-135">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="0c0c2-136">Specificare gli identificatori di runtime in un elenco delimitato da punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-136">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="0c0c2-137">Usare il nome di proprietà `<RuntimeIdentifiers>` (plurale).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-137">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="0c0c2-138">Per altre informazioni, vedere il [Catalogo RID di .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-138">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="0c0c2-139">Aggiungere un riferimento al pacchetto per [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-139">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="0c0c2-140">Per abilitare la registrazione nel registro eventi di Windows, aggiungere un riferimento al pacchetto per [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-140">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="0c0c2-141">Per altre informazioni, vedere la sezione [Gestire gli eventi di avvio e arresto](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-141">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="0c0c2-142">Aggiornamenti a Program.Main</span><span class="sxs-lookup"><span data-stu-id="0c0c2-142">Program.Main updates</span></span>

<span data-ttu-id="0c0c2-143">Modificare `Program.Main` nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-143">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="0c0c2-144">Per eseguire test e debug durante l'esecuzione all'esterno di un servizio, aggiungere il codice per determinare se l'app è in esecuzione come servizio o è un'app console.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-144">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="0c0c2-145">Controllare se il debugger è collegato o se è presente un argomento della riga di comando `--console`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-145">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="0c0c2-146">Se una delle due condizioni è vera (l'app non è eseguita come servizio), chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> sull'host Web.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-146">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="0c0c2-147">Se le condizioni sono false (l'app è eseguita come servizio):</span><span class="sxs-lookup"><span data-stu-id="0c0c2-147">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="0c0c2-148">Chiamare <xref:System.IO.Directory.SetCurrentDirectory*> e usare un percorso per la posizione di pubblicazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-148">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="0c0c2-149">Non chiamare <xref:System.IO.Directory.GetCurrentDirectory*> per ottenere il percorso perché un'app servizio Windows restituisce la cartella *C:\\WINDOWS\\system32* quando viene chiamato <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-149">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="0c0c2-150">Per altre informazioni, vedere la sezione [Directory corrente e radice del contenuto](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-150">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="0c0c2-151">Chiamare <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> per eseguire l'app come servizio.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-151">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="0c0c2-152">Dato che il [provider di configurazione della riga di comando](xref:fundamentals/configuration/index#command-line-configuration-provider) richiede coppie nome-valore per gli argomenti della riga di comando, l'opzione `--console` viene rimossa dagli argomenti prima che <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> li riceva.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-152">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="0c0c2-153">Per scrivere nel registro eventi di Windows, aggiungere il provider EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-153">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="0c0c2-154">Impostare il livello di registrazione con la chiave `Logging:LogLevel:Default` nel file *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-154">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="0c0c2-155">Per scopi di testing e dimostrazione, il file di impostazioni di produzione dell'app di esempio imposta il livello di registrazione su `Information`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-155">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="0c0c2-156">In ambiente di produzione il valore viene in genere impostato su `Error`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-156">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="0c0c2-157">Per ulteriori informazioni, vedere <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-157">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a><span data-ttu-id="0c0c2-158">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="0c0c2-158">Publish the app</span></span>

<span data-ttu-id="0c0c2-159">Pubblicare l'app usando [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-159">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="0c0c2-160">Quando si usa Visual Studio, selezionare **FolderProfile** e configurare il **Percorso di destinazione** prima di selezionare il pulsante **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-160">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="0c0c2-161">Per pubblicare l'app di esempio con strumenti dell'interfaccia della riga di comando, eseguire il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) da un prompt dei comandi dalla cartella del progetto passando una configurazione Versione all'opzione [-c|--configuration](/dotnet/core/tools/dotnet-publish#options).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-161">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="0c0c2-162">Usare l'opzione [-o|--output](/dotnet/core/tools/dotnet-publish#options) con un percorso per pubblicare in una cartella all'esterno dell'app.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-162">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="0c0c2-163">Pubblicare una distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="0c0c2-163">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="0c0c2-164">Nell'esempio seguente l'app viene pubblicata nella cartella *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-164">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="0c0c2-165">Pubblicare una distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="0c0c2-165">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="0c0c2-166">L'identificatore di runtime deve essere specificato nella proprietà `<RuntimeIdenfifier>` (o `<RuntimeIdentifiers>`) del file di progetto.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-166">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="0c0c2-167">Specificare il runtime per l'opzione [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) del comando `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-167">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="0c0c2-168">Nell'esempio seguente l'app viene pubblicata per il runtime `win7-x64` nella cartella *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-168">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a><span data-ttu-id="0c0c2-169">Creare un account utente</span><span class="sxs-lookup"><span data-stu-id="0c0c2-169">Create a user account</span></span>

<span data-ttu-id="0c0c2-170">Creare un account utente per il servizio usando il comando `net user` da una shell dei comandi di PowerShell 6 amministrativa:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-170">Create a user account for the service using the `net user` command from an administrative PowerShell 6 command shell:</span></span>

```powershell
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="0c0c2-171">La scadenza della password predefinita è di sei settimane.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-171">The default password expiration is six weeks.</span></span>

<span data-ttu-id="0c0c2-172">Per l'app di esempio, creare un account utente con il nome `ServiceUser` e una password.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-172">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="0c0c2-173">Nel comando seguente sostituire `{PASSWORD}` con una [password complessa](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-173">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```powershell
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="0c0c2-174">Se è necessario aggiungere l'utente a un gruppo, usare il comando `net localgroup`, dove `{GROUP}` è il nome del gruppo:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-174">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```powershell
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="0c0c2-175">Per altre informazioni, vedere [Service User Accounts](/windows/desktop/services/service-user-accounts) (Account utente di servizio).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-175">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="0c0c2-176">Un approccio alternativo per la gestione degli utenti quando si usa Active Directory consiste nell'usare account del servizio gestito.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-176">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="0c0c2-177">Per altre informazioni, vedere [Panoramica degli account del servizio gestito del gruppo](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-177">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="set-permission-log-on-as-a-service"></a><span data-ttu-id="0c0c2-178">Impostare l'autorizzazione: Accedi come servizio</span><span class="sxs-lookup"><span data-stu-id="0c0c2-178">Set permission: Log on as a service</span></span>

<span data-ttu-id="0c0c2-179">Concedere l'accesso per scrittura/lettura/esecuzione alla cartella dell'app usando il comando [icacls](/windows-server/administration/windows-commands/icacls):</span><span class="sxs-lookup"><span data-stu-id="0c0c2-179">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

```powershell
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="0c0c2-180">`{PATH}` &ndash; Percorso della cartella dell'app.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-180">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="0c0c2-181">`{USER ACCOUNT}` &ndash; Account utente (SID).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-181">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="0c0c2-182">`(OI)` &ndash; Il flag Eredità oggetto propaga le autorizzazioni ai file subordinati.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-182">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="0c0c2-183">`(CI)` &ndash; Il flag Eredità contenitore propaga le autorizzazioni alle cartelle subordinate.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-183">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="0c0c2-184">`{PERMISSION FLAGS}` &ndash; Imposta le autorizzazioni di accesso dell'app.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-184">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="0c0c2-185">Scrittura (`W`)</span><span class="sxs-lookup"><span data-stu-id="0c0c2-185">Write (`W`)</span></span>
  * <span data-ttu-id="0c0c2-186">Lettura (`R`)</span><span class="sxs-lookup"><span data-stu-id="0c0c2-186">Read (`R`)</span></span>
  * <span data-ttu-id="0c0c2-187">Esecuzione (`X`)</span><span class="sxs-lookup"><span data-stu-id="0c0c2-187">Execute (`X`)</span></span>
  * <span data-ttu-id="0c0c2-188">Completo (`F`)</span><span class="sxs-lookup"><span data-stu-id="0c0c2-188">Full (`F`)</span></span>
  * <span data-ttu-id="0c0c2-189">Modifica (`M`)</span><span class="sxs-lookup"><span data-stu-id="0c0c2-189">Modify (`M`)</span></span>
* <span data-ttu-id="0c0c2-190">`/t` &ndash; Applicare in modo ricorsivo alle cartelle e ai file subordinati esistenti.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-190">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="0c0c2-191">Per l'app di esempio pubblicata nella cartella *c:\\svc* e l'account `ServiceUser` con autorizzazioni di scrittura/lettura/esecuzione, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-191">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```powershell
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="0c0c2-192">Per altre informazioni, vedere [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-192">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="create-the-service"></a><span data-ttu-id="0c0c2-193">Creare il servizio</span><span class="sxs-lookup"><span data-stu-id="0c0c2-193">Create the service</span></span>

<span data-ttu-id="0c0c2-194">Usare lo script di PowerShell [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) per la registrazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-194">Use the [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell script to register the service.</span></span> <span data-ttu-id="0c0c2-195">Da un prompt dei comandi di PowerShell 6 amministrativo, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-195">From an administrative PowerShell 6 command prompt, execute the following command:</span></span>

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Path "{PATH}" 
    -Exe {ASSEMBLY}.exe 
    -User {DOMAIN\USER}
```

<span data-ttu-id="0c0c2-196">Nell'esempio seguente per l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-196">In the following example for the sample app:</span></span>

* <span data-ttu-id="0c0c2-197">Il servizio è denominato **MyService**.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-197">The service is named **MyService**.</span></span>
* <span data-ttu-id="0c0c2-198">Il servizio pubblicato risiede nella cartella *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-198">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="0c0c2-199">L'eseguibile dell'app è denominato *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-199">The app executable is named *SampleApp.exe*.</span></span>
* <span data-ttu-id="0c0c2-200">Il servizio viene eseguito nel contesto dell'account `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-200">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="0c0c2-201">Nell'esempio seguente il nome del computer locale è `Desktop-PC`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-201">In the following example, the local machine name is `Desktop-PC`.</span></span>

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Path "c:\svc" 
    -Exe SampleApp.exe 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a><span data-ttu-id="0c0c2-202">Gestire il servizio</span><span class="sxs-lookup"><span data-stu-id="0c0c2-202">Manage the service</span></span>

### <a name="start-the-service"></a><span data-ttu-id="0c0c2-203">Avviare il servizio</span><span class="sxs-lookup"><span data-stu-id="0c0c2-203">Start the service</span></span>

<span data-ttu-id="0c0c2-204">Avviare il servizio con il comando di PowerShell 6 `Start-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-204">Start the service with the `Start-Service -Name {NAME}` PowerShell 6 command.</span></span>

<span data-ttu-id="0c0c2-205">Per avviare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-205">To start the sample app service, use the following command:</span></span>

```powershell
Start-Service -Name MyService
```

<span data-ttu-id="0c0c2-206">L'avvio del servizio richiede alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-206">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="0c0c2-207">Determinare lo stato del servizio</span><span class="sxs-lookup"><span data-stu-id="0c0c2-207">Determine the service status</span></span>

<span data-ttu-id="0c0c2-208">Per verificare lo stato del servizio, usare il comando di PowerShell 6`Get-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-208">To check the status of the service, use the `Get-Service -Name {NAME}` PowerShell 6 command.</span></span> <span data-ttu-id="0c0c2-209">Lo stato viene indicato con uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-209">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

<span data-ttu-id="0c0c2-210">Usare il comando seguente per verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-210">Use the following command to check the status of the sample app service:</span></span>

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="0c0c2-211">Individuare un servizio app Web</span><span class="sxs-lookup"><span data-stu-id="0c0c2-211">Browse a web app service</span></span>

<span data-ttu-id="0c0c2-212">Quando il servizio è nello stato `RUNNING` e se il servizio è un'app Web, selezionare l'app dal suo percorso (per impostazione predefinita, `http://localhost:5000`, che reindirizza a `https://localhost:5001` quando si utilizza [il middleware di reindirizzamento HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-212">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="0c0c2-213">Per il servizio app di esempio, selezionare l'app in `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-213">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="0c0c2-214">Arresta il servizio</span><span class="sxs-lookup"><span data-stu-id="0c0c2-214">Stop the service</span></span>

<span data-ttu-id="0c0c2-215">Arrestare il servizio con il comando di Powershell 6 `Stop-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-215">Stop the service with the `Stop-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="0c0c2-216">Per arrestare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-216">The following command stops the sample app service:</span></span>

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a><span data-ttu-id="0c0c2-217">Rimuovere il servizio</span><span class="sxs-lookup"><span data-stu-id="0c0c2-217">Remove the service</span></span>

<span data-ttu-id="0c0c2-218">Dopo il breve lasso di tempo necessario per arrestare il servizio, rimuovere il servizio con il comando di PowerShell 6`Remove-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-218">After a short delay to stop a service, remove the service with the `Remove-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="0c0c2-219">Verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-219">Check the status of the sample app service:</span></span>

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="0c0c2-220">Gestire gli eventi di avvio e arresto</span><span class="sxs-lookup"><span data-stu-id="0c0c2-220">Handle starting and stopping events</span></span>

<span data-ttu-id="0c0c2-221">Per gestire gli eventi <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> e <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, apportare le modifiche aggiuntive seguenti:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-221">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="0c0c2-222">Creare una classe che deriva da <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con i metodi `OnStarting`, `OnStarted` e `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-222">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="0c0c2-223">Creare un metodo di estensione per <xref:Microsoft.AspNetCore.Hosting.IWebHost> che passa `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-223">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="0c0c2-224">In `Program.Main` chiamare il metodo di estensione `RunAsCustomService` invece di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-224">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="0c0c2-225">Per visualizzare il percorso di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, fare riferimento all'esempio di codice illustrato nella sezione [Convertire un progetto in un servizio Windows](#convert-a-project-into-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="0c0c2-225">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="0c0c2-226">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="0c0c2-226">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="0c0c2-227">I servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o un servizio di bilanciamento del carico potrebbero richiedere una configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-227">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="0c0c2-228">Per ulteriori informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-228">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="0c0c2-229">Configurare HTTPS</span><span class="sxs-lookup"><span data-stu-id="0c0c2-229">Configure HTTPS</span></span>

<span data-ttu-id="0c0c2-230">Per configurare il servizio con un endpoint sicuro:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-230">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="0c0c2-231">Creare un certificato X.509 per il sistema di hosting usando i meccanismi di acquisizione e distribuzione dei certificati della piattaforma.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-231">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="0c0c2-232">Specificare una [configurazione dell'endpoint HTTPS del server Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) per usare il certificato.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-232">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="0c0c2-233">L'uso del certificato di sviluppo ASP.NET Core HTTPS per proteggere un endpoint del servizio non è supportato.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-233">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="0c0c2-234">Directory corrente e radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="0c0c2-234">Current directory and content root</span></span>

<span data-ttu-id="0c0c2-235">La directory di lavoro corrente restituita chiamando <xref:System.IO.Directory.GetCurrentDirectory*> per un servizio Windows è la cartella *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-235">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="0c0c2-236">La cartella *system32* non è un percorso appropriato per archiviare i file di un servizio, ad esempio i file di impostazioni.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-236">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="0c0c2-237">Usare uno degli approcci seguenti per gestire e accedere agli asset e ai file di impostazioni di un servizio.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-237">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="0c0c2-238">Impostare il percorso radice del contenuto sulla cartella dell'app</span><span class="sxs-lookup"><span data-stu-id="0c0c2-238">Set the content root path to the app's folder</span></span>

<span data-ttu-id="0c0c2-239">L'elemento <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> è lo stesso percorso fornito all'argomento `binPath` durante la creazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-239">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="0c0c2-240">Invece di chiamare `GetCurrentDirectory` per creare i percorsi dei file di impostazioni, chiamare <xref:System.IO.Directory.SetCurrentDirectory*> con il percorso radice del contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-240">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="0c0c2-241">In `Program.Main`, determinare il percorso della cartella dell'eseguibile del servizio e usare il percorso per stabilire la radice del contenuto dell'app:</span><span class="sxs-lookup"><span data-stu-id="0c0c2-241">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="0c0c2-242">Archiviare i file del servizio in un percorso appropriato nel disco</span><span class="sxs-lookup"><span data-stu-id="0c0c2-242">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="0c0c2-243">Specificare un percorso assoluto con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> quando si usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> per la cartella contenente i file.</span><span class="sxs-lookup"><span data-stu-id="0c0c2-243">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0c0c2-244">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0c0c2-244">Additional resources</span></span>

* <span data-ttu-id="0c0c2-245">[Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)</span><span class="sxs-lookup"><span data-stu-id="0c0c2-245">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
