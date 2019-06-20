---
title: Struttura di directory di ASP.NET Core
author: guardrex
description: Informazioni sulla struttura di directory delle app ASP.NET Core pubblicate.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 06/17/2019
uid: host-and-deploy/directory-structure
ms.openlocfilehash: f1df047decc7a0a6b7dcee57a690c55eea428b05
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "67166976"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="7733c-103">Struttura di directory di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7733c-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="7733c-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7733c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7733c-105">La directory *publish* contiene gli asset distribuibili prodotti dal comando [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="7733c-105">The *publish* directory contains the app's deployable assets produced by the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="7733c-106">La directory contiene:</span><span class="sxs-lookup"><span data-stu-id="7733c-106">The directory contains:</span></span>

* <span data-ttu-id="7733c-107">File dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="7733c-107">Application files</span></span>
* <span data-ttu-id="7733c-108">File di configurazione</span><span class="sxs-lookup"><span data-stu-id="7733c-108">Configuration files</span></span>
* <span data-ttu-id="7733c-109">Asset statici</span><span class="sxs-lookup"><span data-stu-id="7733c-109">Static assets</span></span>
* <span data-ttu-id="7733c-110">Pacchetti</span><span class="sxs-lookup"><span data-stu-id="7733c-110">Packages</span></span>
* <span data-ttu-id="7733c-111">Un runtime (solo [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd))</span><span class="sxs-lookup"><span data-stu-id="7733c-111">A runtime ([self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) only)</span></span>

| <span data-ttu-id="7733c-112">Tipo di app</span><span class="sxs-lookup"><span data-stu-id="7733c-112">App Type</span></span> | <span data-ttu-id="7733c-113">Struttura di directory</span><span class="sxs-lookup"><span data-stu-id="7733c-113">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="7733c-114">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="7733c-114">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="7733c-115">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="7733c-115">publish&dagger;</span></span><ul><li><span data-ttu-id="7733c-116">Views&dagger; (app MVC; se non sono precompilate visualizzazioni)</span><span class="sxs-lookup"><span data-stu-id="7733c-116">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="7733c-117">Pages&dagger; (MVC o app Razor Pages; se non sono precompilate pagine)</span><span class="sxs-lookup"><span data-stu-id="7733c-117">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="7733c-118">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="7733c-118">wwwroot&dagger;</span></span></li><li><span data-ttu-id="7733c-119">\*\.dll files</span><span class="sxs-lookup"><span data-stu-id="7733c-119">\*\.dll files</span></span></li><li><span data-ttu-id="7733c-120">{NOME ASSEMBLY}.deps.json</span><span class="sxs-lookup"><span data-stu-id="7733c-120">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="7733c-121">{NOME ASSEMBLY}.dll</span><span class="sxs-lookup"><span data-stu-id="7733c-121">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="7733c-122">{NOME ASSEMBLY}.pdb</span><span class="sxs-lookup"><span data-stu-id="7733c-122">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="7733c-123">{NOME ASSEMBLY}.Views.dll</span><span class="sxs-lookup"><span data-stu-id="7733c-123">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="7733c-124">{NOME ASSEMBLY}.Views.pdb</span><span class="sxs-lookup"><span data-stu-id="7733c-124">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="7733c-125">{NOME ASSEMBLY}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="7733c-125">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="7733c-126">web.config (distribuzioni IIS)</span><span class="sxs-lookup"><span data-stu-id="7733c-126">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="7733c-127">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="7733c-127">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="7733c-128">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="7733c-128">publish&dagger;</span></span><ul><li><span data-ttu-id="7733c-129">Views&dagger; (app MVC; se non sono precompilate visualizzazioni)</span><span class="sxs-lookup"><span data-stu-id="7733c-129">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="7733c-130">Pages&dagger; (MVC o app Razor Pages; se non sono precompilate pagine)</span><span class="sxs-lookup"><span data-stu-id="7733c-130">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="7733c-131">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="7733c-131">wwwroot&dagger;</span></span></li><li><span data-ttu-id="7733c-132">\*.dll files</span><span class="sxs-lookup"><span data-stu-id="7733c-132">\*.dll files</span></span></li><li><span data-ttu-id="7733c-133">{NOME ASSEMBLY}.deps.json</span><span class="sxs-lookup"><span data-stu-id="7733c-133">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="7733c-134">{NOME ASSEMBLY}.dll</span><span class="sxs-lookup"><span data-stu-id="7733c-134">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="7733c-135">{NOME ASSEMBLY}.exe</span><span class="sxs-lookup"><span data-stu-id="7733c-135">{ASSEMBLY NAME}.exe</span></span></li><li><span data-ttu-id="7733c-136">{NOME ASSEMBLY}.pdb</span><span class="sxs-lookup"><span data-stu-id="7733c-136">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="7733c-137">{NOME ASSEMBLY}.Views.dll</span><span class="sxs-lookup"><span data-stu-id="7733c-137">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="7733c-138">{NOME ASSEMBLY}.Views.pdb</span><span class="sxs-lookup"><span data-stu-id="7733c-138">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="7733c-139">{NOME ASSEMBLY}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="7733c-139">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="7733c-140">web.config (distribuzioni IIS)</span><span class="sxs-lookup"><span data-stu-id="7733c-140">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="7733c-141">&dagger;Indica una directory</span><span class="sxs-lookup"><span data-stu-id="7733c-141">&dagger;Indicates a directory</span></span>

<span data-ttu-id="7733c-142">La directory *publish* rappresenta il *percorso radice del contenuto*, anche denominato *percorso di base dell'applicazione*, della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="7733c-142">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="7733c-143">Qualsiasi nome venga assegnato alla directory *publish* dell'applicazione distribuita sul server, il relativo percorso viene usato come percorso fisico del server per l'app ospitata.</span><span class="sxs-lookup"><span data-stu-id="7733c-143">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="7733c-144">La directory *wwwroot*, se presente, contiene solo gli asset statici.</span><span class="sxs-lookup"><span data-stu-id="7733c-144">The *wwwroot* directory, if present, only contains static assets.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7733c-145">La creazione di una cartella *Logs* è utile per la [registrazione di debug avanzata del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="7733c-145">Creating a *Logs* folder is useful for [ASP.NET Core Module enhanced debug logging](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="7733c-146">Le cartelle nel percorso specificato per il valore `<handlerSetting>` non vengono create automaticamente dal modulo e devono essere già presenti nella distribuzione per consentire al modulo di scrivere il log di debug.</span><span class="sxs-lookup"><span data-stu-id="7733c-146">Folders in the path provided to the `<handlerSetting>` value aren't created by the module automatically and should pre-exist in the deployment to allow the module to write the debug log.</span></span>

<span data-ttu-id="7733c-147">È possibile creare una directory *Logs* per la distribuzione usando uno dei due approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="7733c-147">A *Logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="7733c-148">Aggiungere l'elemento `<Target>` seguente al file di progetto:</span><span class="sxs-lookup"><span data-stu-id="7733c-148">Add the following `<Target>` element to the project file:</span></span>

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   <span data-ttu-id="7733c-149">L'elemento `<MakeDir>` crea una cartella *Logs* vuota nell'output pubblicato.</span><span class="sxs-lookup"><span data-stu-id="7733c-149">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="7733c-150">L'elemento usa la proprietà `PublishDir` per determinare il percorso di destinazione per la creazione della cartella.</span><span class="sxs-lookup"><span data-stu-id="7733c-150">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="7733c-151">Diversi metodi di distribuzione, ad esempio Distribuzione Web, ignorano le cartelle vuote durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="7733c-151">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="7733c-152">L'elemento `<WriteLinesToFile>` genera un file nella cartella *Logs*, che garantisce la distribuzione della cartella nel server.</span><span class="sxs-lookup"><span data-stu-id="7733c-152">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="7733c-153">La creazione della cartella con questo metodo ha esito negativo se il processo di lavoro non dispone dell'accesso in scrittura alla cartella di destinazione.</span><span class="sxs-lookup"><span data-stu-id="7733c-153">Folder creation using this approach fails if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="7733c-154">Creare fisicamente la directory *Logs* sul server nella distribuzione.</span><span class="sxs-lookup"><span data-stu-id="7733c-154">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="7733c-155">La directory di distribuzione richiede autorizzazioni di lettura/esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7733c-155">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="7733c-156">La directory *Logs* richiede autorizzazioni di lettura/scrittura.</span><span class="sxs-lookup"><span data-stu-id="7733c-156">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="7733c-157">Le directory aggiuntive in cui vengono scritti i file richiedono autorizzazioni di lettura/scrittura.</span><span class="sxs-lookup"><span data-stu-id="7733c-157">Additional directories where files are written require Read/Write permissions.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7733c-158">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7733c-158">Additional resources</span></span>

* [<span data-ttu-id="7733c-159">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="7733c-159">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* [<span data-ttu-id="7733c-160">Distribuzione di applicazioni .NET Core</span><span class="sxs-lookup"><span data-stu-id="7733c-160">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* [<span data-ttu-id="7733c-161">Framework di destinazione</span><span class="sxs-lookup"><span data-stu-id="7733c-161">Target frameworks</span></span>](/dotnet/standard/frameworks)
* [<span data-ttu-id="7733c-162">Catalogo RID di .NET Core</span><span class="sxs-lookup"><span data-stu-id="7733c-162">.NET Core RID Catalog</span></span>](/dotnet/core/rid-catalog)
