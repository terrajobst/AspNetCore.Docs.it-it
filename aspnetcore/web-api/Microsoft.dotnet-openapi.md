---
title: Sviluppare app ASP.NET Core usando OpenAPI
author: ryanbrandenburg
description: Viene illustrato come utilizzare lo strumento ' Microsoft. dotnet-openapi ' per aggiungere riferimenti a file OpenAPI.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: f5eae9e871bc8efc30d500769adb845ff244a90c
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317767"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="9ac51-103">Sviluppare app ASP.NET Core usando gli strumenti di OpenAPI</span><span class="sxs-lookup"><span data-stu-id="9ac51-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="9ac51-104">Di Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="9ac51-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="9ac51-105">[Microsoft. dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) è uno [strumento globale .NET Core](/dotnet/core/tools/global-tools) per la gestione dei riferimenti [openapi](https://github.com/OAI/OpenAPI-Specification) all'interno di un progetto.</span><span class="sxs-lookup"><span data-stu-id="9ac51-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="9ac51-106">Installazione</span><span class="sxs-lookup"><span data-stu-id="9ac51-106">Installation</span></span>

<span data-ttu-id="9ac51-107">Per installare `Microsoft.dotnet-openapi`, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9ac51-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="9ac51-108">Aggiunta</span><span class="sxs-lookup"><span data-stu-id="9ac51-108">Add</span></span>

<span data-ttu-id="9ac51-109">L'aggiunta di un riferimento openapi usando uno qualsiasi dei comandi in questa pagina `<OpenApiReference />` aggiunge un elemento simile al seguente al file con *estensione csproj* :</span><span class="sxs-lookup"><span data-stu-id="9ac51-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="9ac51-110">Il riferimento precedente è necessario per l'app per chiamare il codice client generato.</span><span class="sxs-lookup"><span data-stu-id="9ac51-110">The preceding reference is required for the app to call the generated client code.</span></span>

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

### <a name="add-file"></a><span data-ttu-id="9ac51-111">Aggiungi file</span><span class="sxs-lookup"><span data-stu-id="9ac51-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="9ac51-112">Opzioni</span><span class="sxs-lookup"><span data-stu-id="9ac51-112">Options</span></span>

| <span data-ttu-id="9ac51-113">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="9ac51-113">Short option</span></span>| <span data-ttu-id="9ac51-114">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="9ac51-114">Long option</span></span>| <span data-ttu-id="9ac51-115">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9ac51-115">Description</span></span> | <span data-ttu-id="9ac51-116">Esempio</span><span class="sxs-lookup"><span data-stu-id="9ac51-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="9ac51-117">-v</span><span class="sxs-lookup"><span data-stu-id="9ac51-117">-v</span></span>|<span data-ttu-id="9ac51-118">--Verbose</span><span class="sxs-lookup"><span data-stu-id="9ac51-118">--verbose</span></span> | <span data-ttu-id="9ac51-119">Mostra output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="9ac51-119">Show verbose output.</span></span> |<span data-ttu-id="9ac51-120">DotNet openapi Add file *-v* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="9ac51-120">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="9ac51-121">-p</span><span class="sxs-lookup"><span data-stu-id="9ac51-121">-p</span></span>|<span data-ttu-id="9ac51-122">--updateProject</span><span class="sxs-lookup"><span data-stu-id="9ac51-122">--updateProject</span></span> | <span data-ttu-id="9ac51-123">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="9ac51-123">The project to operate on.</span></span> |<span data-ttu-id="9ac51-124">file di aggiunta di DotNet openapi *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="9ac51-124">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="9ac51-125">-c</span><span class="sxs-lookup"><span data-stu-id="9ac51-125">-c</span></span>|<span data-ttu-id="9ac51-126">--generatore di codice</span><span class="sxs-lookup"><span data-stu-id="9ac51-126">--code-generator</span></span>| <span data-ttu-id="9ac51-127">Generatore di codice da applicare al riferimento.</span><span class="sxs-lookup"><span data-stu-id="9ac51-127">The code generator to apply to the reference.</span></span> <span data-ttu-id="9ac51-128">Le opzioni `NSwagCSharp` sono `NSwagTypeScript`e.</span><span class="sxs-lookup"><span data-stu-id="9ac51-128">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="9ac51-129">Se `--code-generator` non viene specificato `NSwagCSharp`, l'impostazione predefinita degli strumenti è.</span><span class="sxs-lookup"><span data-stu-id="9ac51-129">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="9ac51-130">DotNet openapi Add file .\OpenApi.json--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="9ac51-130">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="9ac51-131">-h</span><span class="sxs-lookup"><span data-stu-id="9ac51-131">-h</span></span>|<span data-ttu-id="9ac51-132">--Guida</span><span class="sxs-lookup"><span data-stu-id="9ac51-132">--help</span></span>|<span data-ttu-id="9ac51-133">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="9ac51-133">Show help information</span></span>|<span data-ttu-id="9ac51-134">file di aggiunta di DotNet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="9ac51-134">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="9ac51-135">Argomenti</span><span class="sxs-lookup"><span data-stu-id="9ac51-135">Arguments</span></span>

|  <span data-ttu-id="9ac51-136">Argomento</span><span class="sxs-lookup"><span data-stu-id="9ac51-136">Argument</span></span>  | <span data-ttu-id="9ac51-137">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9ac51-137">Description</span></span> | <span data-ttu-id="9ac51-138">Esempio</span><span class="sxs-lookup"><span data-stu-id="9ac51-138">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="9ac51-139">file di origine</span><span class="sxs-lookup"><span data-stu-id="9ac51-139">source-file</span></span> | <span data-ttu-id="9ac51-140">Origine da cui creare un riferimento.</span><span class="sxs-lookup"><span data-stu-id="9ac51-140">The source to create a reference from.</span></span> <span data-ttu-id="9ac51-141">Deve essere un file OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="9ac51-141">Must be an OpenAPI file.</span></span> |<span data-ttu-id="9ac51-142">file DotNet openapi Add *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="9ac51-142">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="9ac51-143">Aggiungi URL</span><span class="sxs-lookup"><span data-stu-id="9ac51-143">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="9ac51-144">Opzioni</span><span class="sxs-lookup"><span data-stu-id="9ac51-144">Options</span></span>

| <span data-ttu-id="9ac51-145">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="9ac51-145">Short option</span></span>| <span data-ttu-id="9ac51-146">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="9ac51-146">Long option</span></span>| <span data-ttu-id="9ac51-147">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9ac51-147">Description</span></span> | <span data-ttu-id="9ac51-148">Esempio</span><span class="sxs-lookup"><span data-stu-id="9ac51-148">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="9ac51-149">-v</span><span class="sxs-lookup"><span data-stu-id="9ac51-149">-v</span></span>|<span data-ttu-id="9ac51-150">--Verbose</span><span class="sxs-lookup"><span data-stu-id="9ac51-150">--verbose</span></span> | <span data-ttu-id="9ac51-151">Mostra output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="9ac51-151">Show verbose output.</span></span> |<span data-ttu-id="9ac51-152">DotNet openapi Add URL *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9ac51-152">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="9ac51-153">-p</span><span class="sxs-lookup"><span data-stu-id="9ac51-153">-p</span></span>|<span data-ttu-id="9ac51-154">--updateProject</span><span class="sxs-lookup"><span data-stu-id="9ac51-154">--updateProject</span></span> | <span data-ttu-id="9ac51-155">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="9ac51-155">The project to operate on.</span></span> |<span data-ttu-id="9ac51-156">URL di aggiunta di DotNet openapi *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9ac51-156">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="9ac51-157">-o</span><span class="sxs-lookup"><span data-stu-id="9ac51-157">-o</span></span>|<span data-ttu-id="9ac51-158">--output-file</span><span class="sxs-lookup"><span data-stu-id="9ac51-158">--output-file</span></span> | <span data-ttu-id="9ac51-159">Posizione in cui inserire la copia locale del file OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="9ac51-159">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="9ac51-160">DotNet openapi Add URL `https://contoso.com/openapi.json` *--output-file client. JSON*</span><span class="sxs-lookup"><span data-stu-id="9ac51-160">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="9ac51-161">-c</span><span class="sxs-lookup"><span data-stu-id="9ac51-161">-c</span></span>|<span data-ttu-id="9ac51-162">--generatore di codice</span><span class="sxs-lookup"><span data-stu-id="9ac51-162">--code-generator</span></span>| <span data-ttu-id="9ac51-163">Generatore di codice da applicare al riferimento.</span><span class="sxs-lookup"><span data-stu-id="9ac51-163">The code generator to apply to the reference.</span></span> <span data-ttu-id="9ac51-164">Le opzioni `NSwagCSharp` sono `NSwagTypeScript`e.</span><span class="sxs-lookup"><span data-stu-id="9ac51-164">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="9ac51-165">DotNet openapi Add file .\OpenApi.json--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="9ac51-165">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="9ac51-166">-h</span><span class="sxs-lookup"><span data-stu-id="9ac51-166">-h</span></span>|<span data-ttu-id="9ac51-167">--Guida</span><span class="sxs-lookup"><span data-stu-id="9ac51-167">--help</span></span>|<span data-ttu-id="9ac51-168">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="9ac51-168">Show help information</span></span>|<span data-ttu-id="9ac51-169">URL di aggiunta di DotNet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="9ac51-169">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="9ac51-170">Argomenti</span><span class="sxs-lookup"><span data-stu-id="9ac51-170">Arguments</span></span>

|  <span data-ttu-id="9ac51-171">Argomento</span><span class="sxs-lookup"><span data-stu-id="9ac51-171">Argument</span></span>  | <span data-ttu-id="9ac51-172">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9ac51-172">Description</span></span> | <span data-ttu-id="9ac51-173">Esempio</span><span class="sxs-lookup"><span data-stu-id="9ac51-173">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="9ac51-174">URL origine</span><span class="sxs-lookup"><span data-stu-id="9ac51-174">source-URL</span></span> | <span data-ttu-id="9ac51-175">Origine da cui creare un riferimento.</span><span class="sxs-lookup"><span data-stu-id="9ac51-175">The source to create a reference from.</span></span> <span data-ttu-id="9ac51-176">Deve essere un URL.</span><span class="sxs-lookup"><span data-stu-id="9ac51-176">Must be a URL.</span></span> |<span data-ttu-id="9ac51-177">URL di aggiunta di DotNet openapi`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9ac51-177">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="9ac51-178">Rimuovi</span><span class="sxs-lookup"><span data-stu-id="9ac51-178">Remove</span></span>

<span data-ttu-id="9ac51-179">Rimuove il riferimento OpenAPI che corrisponde al nome file specificato dal file con *estensione csproj* .</span><span class="sxs-lookup"><span data-stu-id="9ac51-179">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="9ac51-180">Quando viene rimosso il riferimento OpenAPI, i client non verranno generati.</span><span class="sxs-lookup"><span data-stu-id="9ac51-180">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="9ac51-181">I file local *. JSON* e *. YAML* vengono eliminati.</span><span class="sxs-lookup"><span data-stu-id="9ac51-181">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="9ac51-182">Opzioni</span><span class="sxs-lookup"><span data-stu-id="9ac51-182">Options</span></span>

| <span data-ttu-id="9ac51-183">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="9ac51-183">Short option</span></span>| <span data-ttu-id="9ac51-184">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="9ac51-184">Long option</span></span>| <span data-ttu-id="9ac51-185">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9ac51-185">Description</span></span>| <span data-ttu-id="9ac51-186">Esempio</span><span class="sxs-lookup"><span data-stu-id="9ac51-186">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="9ac51-187">-v</span><span class="sxs-lookup"><span data-stu-id="9ac51-187">-v</span></span>|<span data-ttu-id="9ac51-188">--Verbose</span><span class="sxs-lookup"><span data-stu-id="9ac51-188">--verbose</span></span> | <span data-ttu-id="9ac51-189">Mostra output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="9ac51-189">Show verbose output.</span></span> |<span data-ttu-id="9ac51-190">openapi di DotNet Remove *-v*</span><span class="sxs-lookup"><span data-stu-id="9ac51-190">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="9ac51-191">-p</span><span class="sxs-lookup"><span data-stu-id="9ac51-191">-p</span></span>|<span data-ttu-id="9ac51-192">--updateProject</span><span class="sxs-lookup"><span data-stu-id="9ac51-192">--updateProject</span></span> | <span data-ttu-id="9ac51-193">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="9ac51-193">The project to operate on.</span></span> |<span data-ttu-id="9ac51-194">DotNet openapi Remove *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="9ac51-194">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="9ac51-195">-h</span><span class="sxs-lookup"><span data-stu-id="9ac51-195">-h</span></span>|<span data-ttu-id="9ac51-196">--Guida</span><span class="sxs-lookup"><span data-stu-id="9ac51-196">--help</span></span>|<span data-ttu-id="9ac51-197">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="9ac51-197">Show help information</span></span>|<span data-ttu-id="9ac51-198">openapi di DotNet--Guida</span><span class="sxs-lookup"><span data-stu-id="9ac51-198">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="9ac51-199">Argomenti</span><span class="sxs-lookup"><span data-stu-id="9ac51-199">Arguments</span></span>

|  <span data-ttu-id="9ac51-200">Argomento</span><span class="sxs-lookup"><span data-stu-id="9ac51-200">Argument</span></span>  | <span data-ttu-id="9ac51-201">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9ac51-201">Description</span></span>| <span data-ttu-id="9ac51-202">Esempio</span><span class="sxs-lookup"><span data-stu-id="9ac51-202">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="9ac51-203">file di origine</span><span class="sxs-lookup"><span data-stu-id="9ac51-203">source-file</span></span> | <span data-ttu-id="9ac51-204">Origine a cui rimuovere il riferimento.</span><span class="sxs-lookup"><span data-stu-id="9ac51-204">The source to remove the reference to.</span></span> |<span data-ttu-id="9ac51-205">*.\OpenAPI.JSON* DotNet openapi Remove</span><span class="sxs-lookup"><span data-stu-id="9ac51-205">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="9ac51-206">Aggiorna</span><span class="sxs-lookup"><span data-stu-id="9ac51-206">Refresh</span></span>

<span data-ttu-id="9ac51-207">Aggiorna la versione locale di un file scaricato usando il contenuto più recente dell'URL di download.</span><span class="sxs-lookup"><span data-stu-id="9ac51-207">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="9ac51-208">Opzioni</span><span class="sxs-lookup"><span data-stu-id="9ac51-208">Options</span></span>

| <span data-ttu-id="9ac51-209">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="9ac51-209">Short option</span></span>| <span data-ttu-id="9ac51-210">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="9ac51-210">Long option</span></span>| <span data-ttu-id="9ac51-211">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9ac51-211">Description</span></span> | <span data-ttu-id="9ac51-212">Esempio</span><span class="sxs-lookup"><span data-stu-id="9ac51-212">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="9ac51-213">-v</span><span class="sxs-lookup"><span data-stu-id="9ac51-213">-v</span></span>|<span data-ttu-id="9ac51-214">--Verbose</span><span class="sxs-lookup"><span data-stu-id="9ac51-214">--verbose</span></span> | <span data-ttu-id="9ac51-215">Mostra output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="9ac51-215">Show verbose output.</span></span> | <span data-ttu-id="9ac51-216">aggiornamento DotNet openapi *-v*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9ac51-216">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="9ac51-217">-p</span><span class="sxs-lookup"><span data-stu-id="9ac51-217">-p</span></span>|<span data-ttu-id="9ac51-218">--updateProject</span><span class="sxs-lookup"><span data-stu-id="9ac51-218">--updateProject</span></span> | <span data-ttu-id="9ac51-219">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="9ac51-219">The project to operate on.</span></span> | <span data-ttu-id="9ac51-220">aggiornamento DotNet openapi *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9ac51-220">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="9ac51-221">-h</span><span class="sxs-lookup"><span data-stu-id="9ac51-221">-h</span></span>|<span data-ttu-id="9ac51-222">--Guida</span><span class="sxs-lookup"><span data-stu-id="9ac51-222">--help</span></span>|<span data-ttu-id="9ac51-223">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="9ac51-223">Show help information</span></span>|<span data-ttu-id="9ac51-224">aggiornamento di DotNet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="9ac51-224">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="9ac51-225">Argomenti</span><span class="sxs-lookup"><span data-stu-id="9ac51-225">Arguments</span></span>

|  <span data-ttu-id="9ac51-226">Argomento</span><span class="sxs-lookup"><span data-stu-id="9ac51-226">Argument</span></span>  | <span data-ttu-id="9ac51-227">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9ac51-227">Description</span></span> | <span data-ttu-id="9ac51-228">Esempio</span><span class="sxs-lookup"><span data-stu-id="9ac51-228">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="9ac51-229">URL origine</span><span class="sxs-lookup"><span data-stu-id="9ac51-229">source-URL</span></span> | <span data-ttu-id="9ac51-230">URL da cui aggiornare il riferimento.</span><span class="sxs-lookup"><span data-stu-id="9ac51-230">The URL to refresh the reference from.</span></span> | <span data-ttu-id="9ac51-231">aggiornamento DotNet openapi`https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="9ac51-231">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
