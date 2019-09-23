---
title: Sviluppare app ASP.NET Core usando OpenAPI
author: ryanbrandenburg
description: Viene illustrato come utilizzare lo strumento ' Microsoft. dotnet-openapi ' per aggiungere riferimenti a file OpenAPI.
ms.author: rybrande
ms.date: 08/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: a9b38bb7e69744d72867bf69cecf1fa92d7c15b3
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187460"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="25df0-103">Sviluppare app ASP.NET Core usando gli strumenti di OpenAPI</span><span class="sxs-lookup"><span data-stu-id="25df0-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="25df0-104">Di Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="25df0-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="25df0-105">`Microsoft.dotnet-openapi`è uno strumento globale .NET Core per la gestione dei riferimenti [openapi](https://github.com/OAI/OpenAPI-Specification) all'interno di un progetto.</span><span class="sxs-lookup"><span data-stu-id="25df0-105">`Microsoft.dotnet-openapi` is a .NET Core global tool for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="25df0-106">Installazione</span><span class="sxs-lookup"><span data-stu-id="25df0-106">Installation</span></span>

<span data-ttu-id="25df0-107">Per installare `Microsoft.dotnet-openapi`, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="25df0-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-openapi
```

<span data-ttu-id="25df0-108">`Microsoft.dotnet-openapi`è uno [strumento globale di .NET Core](/dotnet/core/tools/global-tools).</span><span class="sxs-lookup"><span data-stu-id="25df0-108">`Microsoft.dotnet-openapi` is a [.NET Core Global Tool](/dotnet/core/tools/global-tools).</span></span>

## <a name="add"></a><span data-ttu-id="25df0-109">Aggiunta</span><span class="sxs-lookup"><span data-stu-id="25df0-109">Add</span></span>

<span data-ttu-id="25df0-110">L'aggiunta di un riferimento openapi usando uno qualsiasi dei comandi in questa pagina `<OpenApiReference />` aggiunge un elemento simile al seguente al file con *estensione csproj* :</span><span class="sxs-lookup"><span data-stu-id="25df0-110">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />`  element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="25df0-111">Il riferimento precedente è necessario per l'app per chiamare il codice client generato.</span><span class="sxs-lookup"><span data-stu-id="25df0-111">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -v|--verbose | Show verbose output. |dotnet openapi add project *-v* ../Ref/ProjRef.csproj |
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="25df0-112">Aggiungi file</span><span class="sxs-lookup"><span data-stu-id="25df0-112">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="25df0-113">Opzioni</span><span class="sxs-lookup"><span data-stu-id="25df0-113">Options</span></span>

| <span data-ttu-id="25df0-114">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="25df0-114">Short option</span></span>| <span data-ttu-id="25df0-115">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="25df0-115">Long option</span></span>| <span data-ttu-id="25df0-116">Descrizione</span><span class="sxs-lookup"><span data-stu-id="25df0-116">Description</span></span> | <span data-ttu-id="25df0-117">Esempio</span><span class="sxs-lookup"><span data-stu-id="25df0-117">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="25df0-118">-v</span><span class="sxs-lookup"><span data-stu-id="25df0-118">-v</span></span>|<span data-ttu-id="25df0-119">--Verbose</span><span class="sxs-lookup"><span data-stu-id="25df0-119">--verbose</span></span> | <span data-ttu-id="25df0-120">Mostra output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="25df0-120">Show verbose output.</span></span> |<span data-ttu-id="25df0-121">DotNet openapi Add file *-v* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="25df0-121">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="25df0-122">-p</span><span class="sxs-lookup"><span data-stu-id="25df0-122">-p</span></span>|<span data-ttu-id="25df0-123">--updateProject</span><span class="sxs-lookup"><span data-stu-id="25df0-123">--updateProject</span></span> | <span data-ttu-id="25df0-124">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="25df0-124">The project to operate on.</span></span> |<span data-ttu-id="25df0-125">file di aggiunta di DotNet openapi *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="25df0-125">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="25df0-126">-c</span><span class="sxs-lookup"><span data-stu-id="25df0-126">-c</span></span>|<span data-ttu-id="25df0-127">--generatore di codice</span><span class="sxs-lookup"><span data-stu-id="25df0-127">--code-generator</span></span>| <span data-ttu-id="25df0-128">Generatore di codice da applicare al riferimento.</span><span class="sxs-lookup"><span data-stu-id="25df0-128">The code generator to apply to the reference.</span></span> <span data-ttu-id="25df0-129">Le opzioni `NSwagCSharp` sono `NSwagTypeScript`e.</span><span class="sxs-lookup"><span data-stu-id="25df0-129">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="25df0-130">Se `--code-generator` non viene specificato `NSwagCSharp`, l'impostazione predefinita degli strumenti è.</span><span class="sxs-lookup"><span data-stu-id="25df0-130">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="25df0-131">DotNet openapi Add file .\OpenApi.json--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="25df0-131">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="25df0-132">-h</span><span class="sxs-lookup"><span data-stu-id="25df0-132">-h</span></span>|<span data-ttu-id="25df0-133">--Guida</span><span class="sxs-lookup"><span data-stu-id="25df0-133">--help</span></span>|<span data-ttu-id="25df0-134">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="25df0-134">Show help information</span></span>|<span data-ttu-id="25df0-135">file di aggiunta di DotNet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="25df0-135">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="25df0-136">Argomenti</span><span class="sxs-lookup"><span data-stu-id="25df0-136">Arguments</span></span>

|  <span data-ttu-id="25df0-137">Argomento</span><span class="sxs-lookup"><span data-stu-id="25df0-137">Argument</span></span>  | <span data-ttu-id="25df0-138">Descrizione</span><span class="sxs-lookup"><span data-stu-id="25df0-138">Description</span></span> | <span data-ttu-id="25df0-139">Esempio</span><span class="sxs-lookup"><span data-stu-id="25df0-139">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="25df0-140">file di origine</span><span class="sxs-lookup"><span data-stu-id="25df0-140">source-file</span></span> | <span data-ttu-id="25df0-141">Origine da cui creare un riferimento.</span><span class="sxs-lookup"><span data-stu-id="25df0-141">The source to create a reference from.</span></span> <span data-ttu-id="25df0-142">Deve essere un file OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="25df0-142">Must be an OpenAPI file.</span></span> |<span data-ttu-id="25df0-143">file DotNet openapi Add *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="25df0-143">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="25df0-144">Aggiungi URL</span><span class="sxs-lookup"><span data-stu-id="25df0-144">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="25df0-145">Opzioni</span><span class="sxs-lookup"><span data-stu-id="25df0-145">Options</span></span>

| <span data-ttu-id="25df0-146">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="25df0-146">Short option</span></span>| <span data-ttu-id="25df0-147">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="25df0-147">Long option</span></span>| <span data-ttu-id="25df0-148">Descrizione</span><span class="sxs-lookup"><span data-stu-id="25df0-148">Description</span></span> | <span data-ttu-id="25df0-149">Esempio</span><span class="sxs-lookup"><span data-stu-id="25df0-149">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="25df0-150">-v</span><span class="sxs-lookup"><span data-stu-id="25df0-150">-v</span></span>|<span data-ttu-id="25df0-151">--Verbose</span><span class="sxs-lookup"><span data-stu-id="25df0-151">--verbose</span></span> | <span data-ttu-id="25df0-152">Mostra output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="25df0-152">Show verbose output.</span></span> |<span data-ttu-id="25df0-153">DotNet openapi Add URL *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="25df0-153">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="25df0-154">-p</span><span class="sxs-lookup"><span data-stu-id="25df0-154">-p</span></span>|<span data-ttu-id="25df0-155">--updateProject</span><span class="sxs-lookup"><span data-stu-id="25df0-155">--updateProject</span></span> | <span data-ttu-id="25df0-156">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="25df0-156">The project to operate on.</span></span> |<span data-ttu-id="25df0-157">URL di aggiunta di DotNet openapi *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="25df0-157">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="25df0-158">-o</span><span class="sxs-lookup"><span data-stu-id="25df0-158">-o</span></span>|<span data-ttu-id="25df0-159">--output-file</span><span class="sxs-lookup"><span data-stu-id="25df0-159">--output-file</span></span> | <span data-ttu-id="25df0-160">Posizione in cui inserire la copia locale del file OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="25df0-160">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="25df0-161">DotNet openapi Add URL `https://contoso.com/openapi.json` *--output-file client. JSON*</span><span class="sxs-lookup"><span data-stu-id="25df0-161">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="25df0-162">-c</span><span class="sxs-lookup"><span data-stu-id="25df0-162">-c</span></span>|<span data-ttu-id="25df0-163">--generatore di codice</span><span class="sxs-lookup"><span data-stu-id="25df0-163">--code-generator</span></span>| <span data-ttu-id="25df0-164">Generatore di codice da applicare al riferimento.</span><span class="sxs-lookup"><span data-stu-id="25df0-164">The code generator to apply to the reference.</span></span> <span data-ttu-id="25df0-165">Le opzioni `NSwagCSharp` sono `NSwagTypeScript`e.</span><span class="sxs-lookup"><span data-stu-id="25df0-165">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="25df0-166">DotNet openapi Add file .\OpenApi.json--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="25df0-166">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="25df0-167">-h</span><span class="sxs-lookup"><span data-stu-id="25df0-167">-h</span></span>|<span data-ttu-id="25df0-168">--Guida</span><span class="sxs-lookup"><span data-stu-id="25df0-168">--help</span></span>|<span data-ttu-id="25df0-169">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="25df0-169">Show help information</span></span>|<span data-ttu-id="25df0-170">URL di aggiunta di DotNet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="25df0-170">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="25df0-171">Argomenti</span><span class="sxs-lookup"><span data-stu-id="25df0-171">Arguments</span></span>

|  <span data-ttu-id="25df0-172">Argomento</span><span class="sxs-lookup"><span data-stu-id="25df0-172">Argument</span></span>  | <span data-ttu-id="25df0-173">Descrizione</span><span class="sxs-lookup"><span data-stu-id="25df0-173">Description</span></span> | <span data-ttu-id="25df0-174">Esempio</span><span class="sxs-lookup"><span data-stu-id="25df0-174">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="25df0-175">URL origine</span><span class="sxs-lookup"><span data-stu-id="25df0-175">source-URL</span></span> | <span data-ttu-id="25df0-176">Origine da cui creare un riferimento.</span><span class="sxs-lookup"><span data-stu-id="25df0-176">The source to create a reference from.</span></span> <span data-ttu-id="25df0-177">Deve essere un URL.</span><span class="sxs-lookup"><span data-stu-id="25df0-177">Must be a URL.</span></span> |<span data-ttu-id="25df0-178">URL di aggiunta di DotNet openapi`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="25df0-178">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="25df0-179">Rimuovi</span><span class="sxs-lookup"><span data-stu-id="25df0-179">Remove</span></span>

<span data-ttu-id="25df0-180">Rimuove il riferimento OpenAPI che corrisponde al nome file specificato dal file con *estensione csproj* .</span><span class="sxs-lookup"><span data-stu-id="25df0-180">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="25df0-181">Quando viene rimosso il riferimento OpenAPI, i client non verranno generati.</span><span class="sxs-lookup"><span data-stu-id="25df0-181">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="25df0-182">I file local *. JSON* e *. YAML* vengono eliminati.</span><span class="sxs-lookup"><span data-stu-id="25df0-182">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="25df0-183">Opzioni</span><span class="sxs-lookup"><span data-stu-id="25df0-183">Options</span></span>

| <span data-ttu-id="25df0-184">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="25df0-184">Short option</span></span>| <span data-ttu-id="25df0-185">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="25df0-185">Long option</span></span>| <span data-ttu-id="25df0-186">Descrizione</span><span class="sxs-lookup"><span data-stu-id="25df0-186">Description</span></span>| <span data-ttu-id="25df0-187">Esempio</span><span class="sxs-lookup"><span data-stu-id="25df0-187">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="25df0-188">-v</span><span class="sxs-lookup"><span data-stu-id="25df0-188">-v</span></span>|<span data-ttu-id="25df0-189">--Verbose</span><span class="sxs-lookup"><span data-stu-id="25df0-189">--verbose</span></span> | <span data-ttu-id="25df0-190">Mostra output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="25df0-190">Show verbose output.</span></span> |<span data-ttu-id="25df0-191">openapi di DotNet Remove *-v*</span><span class="sxs-lookup"><span data-stu-id="25df0-191">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="25df0-192">-p</span><span class="sxs-lookup"><span data-stu-id="25df0-192">-p</span></span>|<span data-ttu-id="25df0-193">--updateProject</span><span class="sxs-lookup"><span data-stu-id="25df0-193">--updateProject</span></span> | <span data-ttu-id="25df0-194">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="25df0-194">The project to operate on.</span></span> |<span data-ttu-id="25df0-195">DotNet openapi Remove *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="25df0-195">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="25df0-196">-h</span><span class="sxs-lookup"><span data-stu-id="25df0-196">-h</span></span>|<span data-ttu-id="25df0-197">--Guida</span><span class="sxs-lookup"><span data-stu-id="25df0-197">--help</span></span>|<span data-ttu-id="25df0-198">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="25df0-198">Show help information</span></span>|<span data-ttu-id="25df0-199">openapi di DotNet--Guida</span><span class="sxs-lookup"><span data-stu-id="25df0-199">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="25df0-200">Argomenti</span><span class="sxs-lookup"><span data-stu-id="25df0-200">Arguments</span></span>

|  <span data-ttu-id="25df0-201">Argomento</span><span class="sxs-lookup"><span data-stu-id="25df0-201">Argument</span></span>  | <span data-ttu-id="25df0-202">Descrizione</span><span class="sxs-lookup"><span data-stu-id="25df0-202">Description</span></span>| <span data-ttu-id="25df0-203">Esempio</span><span class="sxs-lookup"><span data-stu-id="25df0-203">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="25df0-204">file di origine</span><span class="sxs-lookup"><span data-stu-id="25df0-204">source-file</span></span> | <span data-ttu-id="25df0-205">Origine a cui rimuovere il riferimento.</span><span class="sxs-lookup"><span data-stu-id="25df0-205">The source to remove the reference to.</span></span> |<span data-ttu-id="25df0-206">*.\OpenAPI.JSON* DotNet openapi Remove</span><span class="sxs-lookup"><span data-stu-id="25df0-206">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="25df0-207">Aggiorna</span><span class="sxs-lookup"><span data-stu-id="25df0-207">Refresh</span></span>

<span data-ttu-id="25df0-208">Aggiorna la versione locale di un file scaricato usando il contenuto più recente dell'URL di download.</span><span class="sxs-lookup"><span data-stu-id="25df0-208">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="25df0-209">Opzioni</span><span class="sxs-lookup"><span data-stu-id="25df0-209">Options</span></span>

| <span data-ttu-id="25df0-210">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="25df0-210">Short option</span></span>| <span data-ttu-id="25df0-211">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="25df0-211">Long option</span></span>| <span data-ttu-id="25df0-212">Descrizione</span><span class="sxs-lookup"><span data-stu-id="25df0-212">Description</span></span> | <span data-ttu-id="25df0-213">Esempio</span><span class="sxs-lookup"><span data-stu-id="25df0-213">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="25df0-214">-v</span><span class="sxs-lookup"><span data-stu-id="25df0-214">-v</span></span>|<span data-ttu-id="25df0-215">--Verbose</span><span class="sxs-lookup"><span data-stu-id="25df0-215">--verbose</span></span> | <span data-ttu-id="25df0-216">Mostra output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="25df0-216">Show verbose output.</span></span> | <span data-ttu-id="25df0-217">aggiornamento DotNet openapi *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="25df0-217">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="25df0-218">-p</span><span class="sxs-lookup"><span data-stu-id="25df0-218">-p</span></span>|<span data-ttu-id="25df0-219">--updateProject</span><span class="sxs-lookup"><span data-stu-id="25df0-219">--updateProject</span></span> | <span data-ttu-id="25df0-220">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="25df0-220">The project to operate on.</span></span> | <span data-ttu-id="25df0-221">aggiornamento DotNet openapi *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="25df0-221">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="25df0-222">-h</span><span class="sxs-lookup"><span data-stu-id="25df0-222">-h</span></span>|<span data-ttu-id="25df0-223">--Guida</span><span class="sxs-lookup"><span data-stu-id="25df0-223">--help</span></span>|<span data-ttu-id="25df0-224">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="25df0-224">Show help information</span></span>|<span data-ttu-id="25df0-225">aggiornamento di DotNet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="25df0-225">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="25df0-226">Argomenti</span><span class="sxs-lookup"><span data-stu-id="25df0-226">Arguments</span></span>

|  <span data-ttu-id="25df0-227">Argomento</span><span class="sxs-lookup"><span data-stu-id="25df0-227">Argument</span></span>  | <span data-ttu-id="25df0-228">Descrizione</span><span class="sxs-lookup"><span data-stu-id="25df0-228">Description</span></span> | <span data-ttu-id="25df0-229">Esempio</span><span class="sxs-lookup"><span data-stu-id="25df0-229">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="25df0-230">URL origine</span><span class="sxs-lookup"><span data-stu-id="25df0-230">source-URL</span></span> | <span data-ttu-id="25df0-231">URL da cui aggiornare il riferimento.</span><span class="sxs-lookup"><span data-stu-id="25df0-231">The URL to refresh the reference from.</span></span> | <span data-ttu-id="25df0-232">aggiornamento DotNet openapi`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="25df0-232">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
