---
title: Ospitare ASP.NET Core in un servizio Windows
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f53c303dc63e092f08e933fea79eb805523cde9b
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861394"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="56b8c-103">Ospitare ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="56b8c-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="56b8c-104">Di [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="56b8c-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="56b8c-105">È possibile ospitare un'app ASP.NET Core in Windows come [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) senza usare IIS.</span><span class="sxs-lookup"><span data-stu-id="56b8c-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="56b8c-106">Quando è ospitata come servizio Windows, l'app viene avviata automaticamente dopo ogni riavvio.</span><span class="sxs-lookup"><span data-stu-id="56b8c-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="56b8c-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56b8c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="56b8c-108">Tipo di distribuzione</span><span class="sxs-lookup"><span data-stu-id="56b8c-108">Deployment type</span></span>

<span data-ttu-id="56b8c-109">È possibile creare una distribuzione di Windows dipendente dal framework o autonoma.</span><span class="sxs-lookup"><span data-stu-id="56b8c-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="56b8c-110">Per informazioni e consigli sugli scenari di distribuzione, vedere [Distribuzione di applicazioni .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="56b8c-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="56b8c-111">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="56b8c-111">Framework-dependent deployment</span></span>

<span data-ttu-id="56b8c-112">La distribuzione dipendente dal framework si basa sulla presenza di una versione condivisa a livello di sistema di .NET Core nel sistema di destinazione.</span><span class="sxs-lookup"><span data-stu-id="56b8c-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="56b8c-113">Quando lo scenario di distribuzione dipendente dal framework viene usato con un'app servizio Windows ASP.NET Core, l'SDK genera un file eseguibile (*\*.exe*), denominato *eseguibile dipendente dal framework*.</span><span class="sxs-lookup"><span data-stu-id="56b8c-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="56b8c-114">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="56b8c-114">Self-contained deployment</span></span>

<span data-ttu-id="56b8c-115">Una distribuzione autonoma non si basa sulla presenza dei componenti condivisi nel sistema di destinazione.</span><span class="sxs-lookup"><span data-stu-id="56b8c-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="56b8c-116">Il runtime e le dipendenze dell'app vengono distribuiti con l'app nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="56b8c-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="56b8c-117">Convertire un progetto in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="56b8c-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="56b8c-118">Apportare le modifiche seguenti a un progetto ASP.NET Core esistente per eseguire l'app come servizio:</span><span class="sxs-lookup"><span data-stu-id="56b8c-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="56b8c-119">Aggiornamenti ai file di progetto</span><span class="sxs-lookup"><span data-stu-id="56b8c-119">Project file updates</span></span>

<span data-ttu-id="56b8c-120">In base al [tipo di distribuzione](#deployment-type) scelto, aggiornare il file di progetto:</span><span class="sxs-lookup"><span data-stu-id="56b8c-120">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="56b8c-121">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="56b8c-121">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="56b8c-122">Aggiungere un [RID (Runtime Identifier)](/dotnet/core/rid-catalog) Windows al `<PropertyGroup>` che contiene il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="56b8c-122">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="56b8c-123">Aggiungere la proprietà `<SelfContained>` impostata su `false`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-123">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="56b8c-124">Disabilitare la creazione di un file *web.config* aggiungendo la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-124">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="56b8c-125">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="56b8c-125">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="56b8c-126">Verificare la presenza di un [RID](/dotnet/core/rid-catalog) di Windows o aggiungere un RID al `<PropertyGroup>` che contiene il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="56b8c-126">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="56b8c-127">Disabilitare la creazione di un file *web.config* aggiungendo la proprietà `<IsTransformWebConfigDisabled>` impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-127">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="56b8c-128">Per eseguire la pubblicazione per più identificatori di runtime:</span><span class="sxs-lookup"><span data-stu-id="56b8c-128">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="56b8c-129">Specificare gli identificatori di runtime in un elenco delimitato da punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="56b8c-129">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="56b8c-130">Usare il nome di proprietà `<RuntimeIdentifiers>` (plurale).</span><span class="sxs-lookup"><span data-stu-id="56b8c-130">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="56b8c-131">Per altre informazioni, vedere il [Catalogo RID di .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="56b8c-131">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="56b8c-132">Aggiungere un riferimento al pacchetto per [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="56b8c-132">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="56b8c-133">Per abilitare la registrazione nel registro eventi di Windows, aggiungere un riferimento al pacchetto per [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="56b8c-133">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="56b8c-134">Per altre informazioni, vedere la sezione [Gestire gli eventi di avvio e arresto](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="56b8c-134">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="56b8c-135">Aggiornamenti a Program.Main</span><span class="sxs-lookup"><span data-stu-id="56b8c-135">Program.Main updates</span></span>

<span data-ttu-id="56b8c-136">Modificare `Program.Main` nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="56b8c-136">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="56b8c-137">Per eseguire test e debug durante l'esecuzione all'esterno di un servizio, aggiungere il codice per determinare se l'app è in esecuzione come servizio o è un'app console.</span><span class="sxs-lookup"><span data-stu-id="56b8c-137">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="56b8c-138">Controllare se il debugger è collegato o se è presente un argomento della riga di comando `--console`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-138">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="56b8c-139">Se una delle due condizioni è vera (l'app non è eseguita come servizio), chiamare <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> sull'host Web.</span><span class="sxs-lookup"><span data-stu-id="56b8c-139">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="56b8c-140">Se le condizioni sono false (l'app è eseguita come servizio):</span><span class="sxs-lookup"><span data-stu-id="56b8c-140">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="56b8c-141">Chiamare <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> e usare un percorso per la posizione di pubblicazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="56b8c-141">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> and use a path to the app's published location.</span></span> <span data-ttu-id="56b8c-142">Non chiamare <xref:System.IO.Directory.GetCurrentDirectory*> per ottenere il percorso perché un'app servizio Windows restituisce la cartella *C:\\WINDOWS\\system32* quando viene chiamato `GetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-142">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when `GetCurrentDirectory` is called.</span></span> <span data-ttu-id="56b8c-143">Per altre informazioni, vedere la sezione [Directory corrente e radice del contenuto](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="56b8c-143">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="56b8c-144">Chiamare <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> per eseguire l'app come servizio.</span><span class="sxs-lookup"><span data-stu-id="56b8c-144">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="56b8c-145">Dato che il [provider di configurazione della riga di comando](xref:fundamentals/configuration/index#command-line-configuration-provider) richiede coppie nome-valore per gli argomenti della riga di comando, l'opzione `--console` viene rimossa dagli argomenti prima che <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> li riceva.</span><span class="sxs-lookup"><span data-stu-id="56b8c-145">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="56b8c-146">Per scrivere nel registro eventi di Windows, aggiungere il provider EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="56b8c-146">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="56b8c-147">Impostare il livello di registrazione con la chiave `Logging:LogLevel:Default` nel file *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="56b8c-147">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="56b8c-148">Per scopi di testing e dimostrazione, il file di impostazioni di produzione dell'app di esempio imposta il livello di registrazione su `Information`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-148">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="56b8c-149">In ambiente di produzione il valore viene in genere impostato su `Error`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-149">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="56b8c-150">Per ulteriori informazioni, vedere <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="56b8c-150">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

### <a name="publish-the-app"></a><span data-ttu-id="56b8c-151">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="56b8c-151">Publish the app</span></span>

<span data-ttu-id="56b8c-152">Pubblicare l'app usando [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="56b8c-152">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="56b8c-153">Quando si usa Visual Studio, selezionare **FolderProfile** e configurare il **Percorso di destinazione** prima di selezionare il pulsante **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="56b8c-153">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="56b8c-154">Per pubblicare l'app di esempio con strumenti dell'interfaccia della riga di comando, eseguire il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) da un prompt dei comandi dalla cartella del progetto passando una configurazione Versione all'opzione [-c|--configuration](/dotnet/core/tools/dotnet-publish#options).</span><span class="sxs-lookup"><span data-stu-id="56b8c-154">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="56b8c-155">Usare l'opzione [-o|--output](/dotnet/core/tools/dotnet-publish#options) con un percorso per pubblicare in una cartella all'esterno dell'app.</span><span class="sxs-lookup"><span data-stu-id="56b8c-155">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

#### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="56b8c-156">Pubblicare una distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="56b8c-156">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="56b8c-157">Nell'esempio seguente l'app viene pubblicata nella cartella *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="56b8c-157">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

#### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="56b8c-158">Pubblicare una distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="56b8c-158">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="56b8c-159">L'identificatore di runtime deve essere specificato nella proprietà `<RuntimeIdenfifier>` (o `<RuntimeIdentifiers>`) del file di progetto.</span><span class="sxs-lookup"><span data-stu-id="56b8c-159">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="56b8c-160">Specificare il runtime per l'opzione [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) del comando `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-160">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="56b8c-161">Nell'esempio seguente l'app viene pubblicata per il runtime `win7-x64` nella cartella *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="56b8c-161">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

### <a name="create-a-user-account"></a><span data-ttu-id="56b8c-162">Creare un account utente</span><span class="sxs-lookup"><span data-stu-id="56b8c-162">Create a user account</span></span>

<span data-ttu-id="56b8c-163">Creare un account utente per il servizio con il comando `net user`:</span><span class="sxs-lookup"><span data-stu-id="56b8c-163">Create a user account for the service using the `net user` command:</span></span>

```console
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="56b8c-164">Per l'app di esempio, creare un account utente con il nome `ServiceUser` e una password.</span><span class="sxs-lookup"><span data-stu-id="56b8c-164">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="56b8c-165">Nel comando seguente sostituire `{PASSWORD}` con una [password complessa](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="56b8c-165">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```console
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="56b8c-166">Se è necessario aggiungere l'utente a un gruppo, usare il comando `net localgroup`, dove `{GROUP}` è il nome del gruppo:</span><span class="sxs-lookup"><span data-stu-id="56b8c-166">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```console
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="56b8c-167">Per altre informazioni, vedere [Service User Accounts](/windows/desktop/services/service-user-accounts) (Account utente di servizio).</span><span class="sxs-lookup"><span data-stu-id="56b8c-167">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

### <a name="set-permissions"></a><span data-ttu-id="56b8c-168">Impostare le autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="56b8c-168">Set permissions</span></span>

<span data-ttu-id="56b8c-169">Concedere l'accesso per scrittura/lettura/esecuzione alla cartella dell'app usando il comando [icacls](/windows-server/administration/windows-commands/icacls):</span><span class="sxs-lookup"><span data-stu-id="56b8c-169">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

```console
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="56b8c-170">`{PATH}` &ndash; Percorso della cartella dell'app.</span><span class="sxs-lookup"><span data-stu-id="56b8c-170">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="56b8c-171">`{USER ACCOUNT}` &ndash; Account utente (SID).</span><span class="sxs-lookup"><span data-stu-id="56b8c-171">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="56b8c-172">`(OI)` &ndash; Il flag Eredità oggetto propaga le autorizzazioni ai file subordinati.</span><span class="sxs-lookup"><span data-stu-id="56b8c-172">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="56b8c-173">`(CI)` &ndash; Il flag Eredità contenitore propaga le autorizzazioni alle cartelle subordinate.</span><span class="sxs-lookup"><span data-stu-id="56b8c-173">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="56b8c-174">`{PERMISSION FLAGS}` &ndash; Imposta le autorizzazioni di accesso dell'app.</span><span class="sxs-lookup"><span data-stu-id="56b8c-174">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="56b8c-175">Scrittura (`W`)</span><span class="sxs-lookup"><span data-stu-id="56b8c-175">Write (`W`)</span></span>
  * <span data-ttu-id="56b8c-176">Lettura (`R`)</span><span class="sxs-lookup"><span data-stu-id="56b8c-176">Read (`R`)</span></span>
  * <span data-ttu-id="56b8c-177">Esecuzione (`X`)</span><span class="sxs-lookup"><span data-stu-id="56b8c-177">Execute (`X`)</span></span>
  * <span data-ttu-id="56b8c-178">Completo (`F`)</span><span class="sxs-lookup"><span data-stu-id="56b8c-178">Full (`F`)</span></span>
  * <span data-ttu-id="56b8c-179">Modifica (`M`)</span><span class="sxs-lookup"><span data-stu-id="56b8c-179">Modify (`M`)</span></span>
* <span data-ttu-id="56b8c-180">`/t` &ndash; Applicare in modo ricorsivo alle cartelle e ai file subordinati esistenti.</span><span class="sxs-lookup"><span data-stu-id="56b8c-180">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="56b8c-181">Per l'app di esempio pubblicata nella cartella *c:\\svc* e l'account `ServiceUser` con autorizzazioni di scrittura/lettura/esecuzione, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="56b8c-181">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```console
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="56b8c-182">Per altre informazioni, vedere [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="56b8c-182">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="manage-the-service"></a><span data-ttu-id="56b8c-183">Gestire il servizio</span><span class="sxs-lookup"><span data-stu-id="56b8c-183">Manage the service</span></span>

### <a name="create-the-service"></a><span data-ttu-id="56b8c-184">Creare il servizio</span><span class="sxs-lookup"><span data-stu-id="56b8c-184">Create the service</span></span>

<span data-ttu-id="56b8c-185">Usare lo strumento da riga di comando [sc.exe](https://technet.microsoft.com/library/bb490995) per creare il servizio.</span><span class="sxs-lookup"><span data-stu-id="56b8c-185">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="56b8c-186">Il valore `binPath` rappresenta il percorso dell'eseguibile dell'app, che include il nome del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="56b8c-186">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="56b8c-187">**Lo spazio tra il segno di uguale e il carattere di virgoletta di ogni parametro e valore è obbligatorio.**</span><span class="sxs-lookup"><span data-stu-id="56b8c-187">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

```console
sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
```

* <span data-ttu-id="56b8c-188">`{SERVICE NAME}` &ndash; Nome da assegnare al servizio in [Gestione controllo servizi](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="56b8c-188">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
* <span data-ttu-id="56b8c-189">`{PATH}` &ndash; Percorso dell'eseguibile del servizio.</span><span class="sxs-lookup"><span data-stu-id="56b8c-189">`{PATH}` &ndash; The path to the service executable.</span></span>
* <span data-ttu-id="56b8c-190">`{DOMAIN}` &ndash; Dominio di un computer aggiunto a un dominio.</span><span class="sxs-lookup"><span data-stu-id="56b8c-190">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="56b8c-191">Se il computer non è aggiunto a un dominio, il nome del computer locale.</span><span class="sxs-lookup"><span data-stu-id="56b8c-191">If the machine isn't domain-joined, the local machine name.</span></span>
* <span data-ttu-id="56b8c-192">`{USER ACCOUNT}` &ndash; Account utente con cui viene eseguito il servizio.</span><span class="sxs-lookup"><span data-stu-id="56b8c-192">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
* <span data-ttu-id="56b8c-193">`{PASSWORD}` &ndash; Password dell'account utente.</span><span class="sxs-lookup"><span data-stu-id="56b8c-193">`{PASSWORD}` &ndash; The user account password.</span></span>

> [!WARNING]
> <span data-ttu-id="56b8c-194">**Non** omettere il parametro `obj`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-194">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="56b8c-195">Il valore predefinito per `obj` è l'account [LocalSystem](/windows/desktop/services/localsystem-account).</span><span class="sxs-lookup"><span data-stu-id="56b8c-195">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="56b8c-196">L'esecuzione di un servizio nel contesto dell'account `LocalSystem` presenta rischi significativi per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="56b8c-196">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="56b8c-197">Eseguire sempre un servizio con un account utente con privilegi limitati.</span><span class="sxs-lookup"><span data-stu-id="56b8c-197">Always run a service with a user account that has restricted privileges.</span></span>

<span data-ttu-id="56b8c-198">Nell'esempio seguente per l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="56b8c-198">In the following example for the sample app:</span></span>

* <span data-ttu-id="56b8c-199">Il servizio è denominato **MyService**.</span><span class="sxs-lookup"><span data-stu-id="56b8c-199">The service is named **MyService**.</span></span>
* <span data-ttu-id="56b8c-200">Il servizio pubblicato risiede nella cartella *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="56b8c-200">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="56b8c-201">L'eseguibile dell'app è denominato *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="56b8c-201">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="56b8c-202">Racchiudere il valore `binPath` tra virgolette doppie (").</span><span class="sxs-lookup"><span data-stu-id="56b8c-202">Enclose the `binPath` value in double quotation marks (").</span></span>
* <span data-ttu-id="56b8c-203">Il servizio viene eseguito nel contesto dell'account `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-203">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="56b8c-204">Sostituire `{DOMAIN}` con il dominio dell'account utente o il nome del computer locale.</span><span class="sxs-lookup"><span data-stu-id="56b8c-204">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="56b8c-205">Racchiudere il valore `obj` tra virgolette doppie (").</span><span class="sxs-lookup"><span data-stu-id="56b8c-205">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="56b8c-206">Esempio: se il sistema host è un computer locale denominato `MairaPC`, impostare `obj` su `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-206">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
* <span data-ttu-id="56b8c-207">Sostituire `{PASSWORD}` con la password dell'account utente.</span><span class="sxs-lookup"><span data-stu-id="56b8c-207">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="56b8c-208">Racchiudere il valore `password` tra virgolette doppie (").</span><span class="sxs-lookup"><span data-stu-id="56b8c-208">Enclose the `password` value in double quotation marks (").</span></span>

```console
sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
```

> [!IMPORTANT]
> <span data-ttu-id="56b8c-209">Assicurarsi che siano presenti gli spazi tra i segni di uguale e i valori dei parametri.</span><span class="sxs-lookup"><span data-stu-id="56b8c-209">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

### <a name="start-the-service"></a><span data-ttu-id="56b8c-210">Avviare il servizio</span><span class="sxs-lookup"><span data-stu-id="56b8c-210">Start the service</span></span>

<span data-ttu-id="56b8c-211">Avviare il servizio con il comando `sc start {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-211">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

<span data-ttu-id="56b8c-212">Per avviare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="56b8c-212">To start the sample app service, use the following command:</span></span>

```console
sc start MyService
```

<span data-ttu-id="56b8c-213">L'avvio del servizio richiede alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="56b8c-213">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="56b8c-214">Determinare lo stato del servizio</span><span class="sxs-lookup"><span data-stu-id="56b8c-214">Determine the service status</span></span>

<span data-ttu-id="56b8c-215">Per verificare lo stato del servizio, usare il comando `sc query {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-215">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="56b8c-216">Lo stato viene indicato con uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="56b8c-216">The status is reported as one of the following values:</span></span>

* `START_PENDING`
* `RUNNING`
* `STOP_PENDING`
* `STOPPED`

<span data-ttu-id="56b8c-217">Usare il comando seguente per verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="56b8c-217">Use the following command to check the status of the sample app service:</span></span>

```console
sc query MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="56b8c-218">Individuare un servizio app Web</span><span class="sxs-lookup"><span data-stu-id="56b8c-218">Browse a web app service</span></span>

<span data-ttu-id="56b8c-219">Quando il servizio è nello stato `RUNNING` e se il servizio è un'app Web, selezionare l'app dal suo percorso (per impostazione predefinita, `http://localhost:5000`, che reindirizza a `https://localhost:5001` quando si utilizza [il middleware di reindirizzamento HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="56b8c-219">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="56b8c-220">Per il servizio app di esempio, selezionare l'app in `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-220">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="56b8c-221">Arresta il servizio</span><span class="sxs-lookup"><span data-stu-id="56b8c-221">Stop the service</span></span>

<span data-ttu-id="56b8c-222">Arrestare il servizio con il comando `sc stop {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-222">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

<span data-ttu-id="56b8c-223">Per arrestare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="56b8c-223">The following command stops the sample app service:</span></span>

```console
sc stop MyService
```

### <a name="delete-the-service"></a><span data-ttu-id="56b8c-224">Eliminare il servizio</span><span class="sxs-lookup"><span data-stu-id="56b8c-224">Delete the service</span></span>

<span data-ttu-id="56b8c-225">Dopo il breve lasso di tempo necessario per arrestare il servizio, disinstallare il servizio con il comando `sc delete {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="56b8c-225">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

<span data-ttu-id="56b8c-226">Verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="56b8c-226">Check the status of the sample app service:</span></span>

```console
sc query MyService
```

<span data-ttu-id="56b8c-227">Quando il servizio app di esempio è nello stato `STOPPED`, usare il comando seguente per disinstallare il servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="56b8c-227">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

```console
sc delete MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="56b8c-228">Gestire gli eventi di avvio e arresto</span><span class="sxs-lookup"><span data-stu-id="56b8c-228">Handle starting and stopping events</span></span>

<span data-ttu-id="56b8c-229">Per gestire gli eventi <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> e <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, apportare le modifiche aggiuntive seguenti:</span><span class="sxs-lookup"><span data-stu-id="56b8c-229">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="56b8c-230">Creare una classe che deriva da <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con i metodi `OnStarting`, `OnStarted` e `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="56b8c-230">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="56b8c-231">Creare un metodo di estensione per <xref:Microsoft.AspNetCore.Hosting.IWebHost> che passa `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="56b8c-231">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="56b8c-232">In `Program.Main` chiamare il metodo di estensione `RunAsCustomService` invece di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="56b8c-232">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="56b8c-233">Per visualizzare il percorso di <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, fare riferimento all'esempio di codice illustrato nella sezione [Convertire un progetto in un servizio Windows](#convert-a-project-into-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="56b8c-233">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="56b8c-234">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="56b8c-234">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="56b8c-235">I servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o un servizio di bilanciamento del carico potrebbero richiedere una configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="56b8c-235">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="56b8c-236">Per ulteriori informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="56b8c-236">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="56b8c-237">Configurare HTTPS</span><span class="sxs-lookup"><span data-stu-id="56b8c-237">Configure HTTPS</span></span>

<span data-ttu-id="56b8c-238">Per configurare il servizio con un endpoint sicuro:</span><span class="sxs-lookup"><span data-stu-id="56b8c-238">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="56b8c-239">Creare un certificato X.509 per il sistema di hosting usando i meccanismi di acquisizione e distribuzione dei certificati della piattaforma.</span><span class="sxs-lookup"><span data-stu-id="56b8c-239">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="56b8c-240">Specificare una [configurazione dell'endpoint HTTPS del server Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) per usare il certificato.</span><span class="sxs-lookup"><span data-stu-id="56b8c-240">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="56b8c-241">L'uso del certificato di sviluppo ASP.NET Core HTTPS per proteggere un endpoint del servizio non è supportato.</span><span class="sxs-lookup"><span data-stu-id="56b8c-241">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="56b8c-242">Directory corrente e radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="56b8c-242">Current directory and content root</span></span>

<span data-ttu-id="56b8c-243">La directory di lavoro corrente restituita chiamando <xref:System.IO.Directory.GetCurrentDirectory*> per un servizio Windows è la cartella *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="56b8c-243">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="56b8c-244">La cartella *system32* non è un percorso appropriato per archiviare i file di un servizio, ad esempio i file di impostazioni.</span><span class="sxs-lookup"><span data-stu-id="56b8c-244">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="56b8c-245">Usare uno degli approcci seguenti per gestire e accedere agli asset e ai file di impostazioni di un servizio.</span><span class="sxs-lookup"><span data-stu-id="56b8c-245">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="56b8c-246">Impostare il percorso radice del contenuto sulla cartella dell'app</span><span class="sxs-lookup"><span data-stu-id="56b8c-246">Set the content root path to the app's folder</span></span>

<span data-ttu-id="56b8c-247">L'elemento <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> è lo stesso percorso fornito all'argomento `binPath` durante la creazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="56b8c-247">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="56b8c-248">Invece di chiamare `GetCurrentDirectory` per creare i percorsi dei file di impostazioni, chiamare <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> con il percorso radice del contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="56b8c-248">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> with the path to the app's content root.</span></span>

<span data-ttu-id="56b8c-249">In `Program.Main`, determinare il percorso della cartella dell'eseguibile del servizio e usare il percorso per stabilire la radice del contenuto dell'app:</span><span class="sxs-lookup"><span data-stu-id="56b8c-249">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);

CreateWebHostBuilder(args)
    .UseContentRoot(pathToContentRoot)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="56b8c-250">Archiviare i file del servizio in un percorso appropriato nel disco</span><span class="sxs-lookup"><span data-stu-id="56b8c-250">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="56b8c-251">Specificare un percorso assoluto con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> quando si usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> per la cartella contenente i file.</span><span class="sxs-lookup"><span data-stu-id="56b8c-251">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56b8c-252">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="56b8c-252">Additional resources</span></span>

* <span data-ttu-id="56b8c-253">[Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)</span><span class="sxs-lookup"><span data-stu-id="56b8c-253">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
