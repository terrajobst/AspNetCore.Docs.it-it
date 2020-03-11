---
title: Sviluppare app ASP.NET Core usando OpenAPI
author: ryanbrandenburg
description: Viene illustrato come utilizzare lo strumento ' Microsoft. dotnet-openapi ' per aggiungere riferimenti a file OpenAPI.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: 079e36511b63c186ffa7726bdb1e3c3bcbda9d34
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655267"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="c829f-103">Sviluppare app ASP.NET Core usando gli strumenti di OpenAPI</span><span class="sxs-lookup"><span data-stu-id="c829f-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="c829f-104">Di Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="c829f-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="c829f-105">[Microsoft. dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) è uno [strumento globale .NET Core](/dotnet/core/tools/global-tools) per la gestione dei riferimenti [openapi](https://github.com/OAI/OpenAPI-Specification) all'interno di un progetto.</span><span class="sxs-lookup"><span data-stu-id="c829f-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="c829f-106">Installazione</span><span class="sxs-lookup"><span data-stu-id="c829f-106">Installation</span></span>

<span data-ttu-id="c829f-107">Per installare `Microsoft.dotnet-openapi`, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c829f-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="c829f-108">Add</span><span class="sxs-lookup"><span data-stu-id="c829f-108">Add</span></span>

<span data-ttu-id="c829f-109">L'aggiunta di un riferimento OpenAPI usando uno qualsiasi dei comandi in questa pagina aggiunge un elemento `<OpenApiReference />` simile al seguente al file con *estensione csproj* :</span><span class="sxs-lookup"><span data-stu-id="c829f-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="c829f-110">Il riferimento precedente è necessario per l'app per chiamare il codice client generato.</span><span class="sxs-lookup"><span data-stu-id="c829f-110">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/dotnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="c829f-111">Aggiungi file</span><span class="sxs-lookup"><span data-stu-id="c829f-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="c829f-112">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c829f-112">Options</span></span>

| <span data-ttu-id="c829f-113">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="c829f-113">Short option</span></span>| <span data-ttu-id="c829f-114">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="c829f-114">Long option</span></span>| <span data-ttu-id="c829f-115">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c829f-115">Description</span></span> | <span data-ttu-id="c829f-116">Esempio</span><span class="sxs-lookup"><span data-stu-id="c829f-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="c829f-117">-p</span><span class="sxs-lookup"><span data-stu-id="c829f-117">-p</span></span>|<span data-ttu-id="c829f-118">--updateProject</span><span class="sxs-lookup"><span data-stu-id="c829f-118">--updateProject</span></span> | <span data-ttu-id="c829f-119">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="c829f-119">The project to operate on.</span></span> |<span data-ttu-id="c829f-120">file di aggiunta di DotNet openapi *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="c829f-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="c829f-121">-c</span><span class="sxs-lookup"><span data-stu-id="c829f-121">-c</span></span>|<span data-ttu-id="c829f-122">--generatore di codice</span><span class="sxs-lookup"><span data-stu-id="c829f-122">--code-generator</span></span>| <span data-ttu-id="c829f-123">Generatore di codice da applicare al riferimento.</span><span class="sxs-lookup"><span data-stu-id="c829f-123">The code generator to apply to the reference.</span></span> <span data-ttu-id="c829f-124">Le opzioni sono `NSwagCSharp` e `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="c829f-124">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="c829f-125">Se `--code-generator` non viene specificato, l'impostazione predefinita degli strumenti è `NSwagCSharp`.</span><span class="sxs-lookup"><span data-stu-id="c829f-125">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="c829f-126">DotNet openapi Add file .\OpenApi.json--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="c829f-126">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="c829f-127">-H</span><span class="sxs-lookup"><span data-stu-id="c829f-127">-h</span></span>|<span data-ttu-id="c829f-128">--help</span><span class="sxs-lookup"><span data-stu-id="c829f-128">--help</span></span>|<span data-ttu-id="c829f-129">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="c829f-129">Show help information</span></span>|<span data-ttu-id="c829f-130">file di aggiunta di DotNet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="c829f-130">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="c829f-131">Argomenti</span><span class="sxs-lookup"><span data-stu-id="c829f-131">Arguments</span></span>

|  <span data-ttu-id="c829f-132">Argomento</span><span class="sxs-lookup"><span data-stu-id="c829f-132">Argument</span></span>  | <span data-ttu-id="c829f-133">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c829f-133">Description</span></span> | <span data-ttu-id="c829f-134">Esempio</span><span class="sxs-lookup"><span data-stu-id="c829f-134">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="c829f-135">file di origine</span><span class="sxs-lookup"><span data-stu-id="c829f-135">source-file</span></span> | <span data-ttu-id="c829f-136">Origine da cui creare un riferimento.</span><span class="sxs-lookup"><span data-stu-id="c829f-136">The source to create a reference from.</span></span> <span data-ttu-id="c829f-137">Deve essere un file OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="c829f-137">Must be an OpenAPI file.</span></span> |<span data-ttu-id="c829f-138">file DotNet openapi Add *.\OpenAPI.JSON*</span><span class="sxs-lookup"><span data-stu-id="c829f-138">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="c829f-139">Aggiungere URL</span><span class="sxs-lookup"><span data-stu-id="c829f-139">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="c829f-140">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c829f-140">Options</span></span>

| <span data-ttu-id="c829f-141">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="c829f-141">Short option</span></span>| <span data-ttu-id="c829f-142">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="c829f-142">Long option</span></span>| <span data-ttu-id="c829f-143">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c829f-143">Description</span></span> | <span data-ttu-id="c829f-144">Esempio</span><span class="sxs-lookup"><span data-stu-id="c829f-144">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="c829f-145">-p</span><span class="sxs-lookup"><span data-stu-id="c829f-145">-p</span></span>|<span data-ttu-id="c829f-146">--updateProject</span><span class="sxs-lookup"><span data-stu-id="c829f-146">--updateProject</span></span> | <span data-ttu-id="c829f-147">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="c829f-147">The project to operate on.</span></span> |<span data-ttu-id="c829f-148">URL di aggiunta di DotNet openapi *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="c829f-148">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="c829f-149">-o</span><span class="sxs-lookup"><span data-stu-id="c829f-149">-o</span></span>|<span data-ttu-id="c829f-150">--output-file</span><span class="sxs-lookup"><span data-stu-id="c829f-150">--output-file</span></span> | <span data-ttu-id="c829f-151">Posizione in cui inserire la copia locale del file OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="c829f-151">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="c829f-152">DotNet openapi Add URL `https://contoso.com/openapi.json` *--output-file client. JSON*</span><span class="sxs-lookup"><span data-stu-id="c829f-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="c829f-153">-c</span><span class="sxs-lookup"><span data-stu-id="c829f-153">-c</span></span>|<span data-ttu-id="c829f-154">--generatore di codice</span><span class="sxs-lookup"><span data-stu-id="c829f-154">--code-generator</span></span>| <span data-ttu-id="c829f-155">Generatore di codice da applicare al riferimento.</span><span class="sxs-lookup"><span data-stu-id="c829f-155">The code generator to apply to the reference.</span></span> <span data-ttu-id="c829f-156">Le opzioni sono `NSwagCSharp` e `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="c829f-156">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="c829f-157">DotNet openapi Add file .\OpenApi.json--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="c829f-157">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="c829f-158">-H</span><span class="sxs-lookup"><span data-stu-id="c829f-158">-h</span></span>|<span data-ttu-id="c829f-159">--help</span><span class="sxs-lookup"><span data-stu-id="c829f-159">--help</span></span>|<span data-ttu-id="c829f-160">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="c829f-160">Show help information</span></span>|<span data-ttu-id="c829f-161">URL di aggiunta di DotNet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="c829f-161">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="c829f-162">Argomenti</span><span class="sxs-lookup"><span data-stu-id="c829f-162">Arguments</span></span>

|  <span data-ttu-id="c829f-163">Argomento</span><span class="sxs-lookup"><span data-stu-id="c829f-163">Argument</span></span>  | <span data-ttu-id="c829f-164">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c829f-164">Description</span></span> | <span data-ttu-id="c829f-165">Esempio</span><span class="sxs-lookup"><span data-stu-id="c829f-165">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="c829f-166">URL origine</span><span class="sxs-lookup"><span data-stu-id="c829f-166">source-URL</span></span> | <span data-ttu-id="c829f-167">Origine da cui creare un riferimento.</span><span class="sxs-lookup"><span data-stu-id="c829f-167">The source to create a reference from.</span></span> <span data-ttu-id="c829f-168">Deve essere un URL.</span><span class="sxs-lookup"><span data-stu-id="c829f-168">Must be a URL.</span></span> |<span data-ttu-id="c829f-169">`https://contoso.com/openapi.json` di Aggiungi URL DotNet openapi</span><span class="sxs-lookup"><span data-stu-id="c829f-169">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="c829f-170">Rimuovere</span><span class="sxs-lookup"><span data-stu-id="c829f-170">Remove</span></span>

<span data-ttu-id="c829f-171">Rimuove il riferimento OpenAPI che corrisponde al nome file specificato dal file con *estensione csproj* .</span><span class="sxs-lookup"><span data-stu-id="c829f-171">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="c829f-172">Quando viene rimosso il riferimento OpenAPI, i client non verranno generati.</span><span class="sxs-lookup"><span data-stu-id="c829f-172">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="c829f-173">I file local *. JSON* e *. YAML* vengono eliminati.</span><span class="sxs-lookup"><span data-stu-id="c829f-173">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="c829f-174">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c829f-174">Options</span></span>

| <span data-ttu-id="c829f-175">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="c829f-175">Short option</span></span>| <span data-ttu-id="c829f-176">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="c829f-176">Long option</span></span>| <span data-ttu-id="c829f-177">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c829f-177">Description</span></span>| <span data-ttu-id="c829f-178">Esempio</span><span class="sxs-lookup"><span data-stu-id="c829f-178">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="c829f-179">-p</span><span class="sxs-lookup"><span data-stu-id="c829f-179">-p</span></span>|<span data-ttu-id="c829f-180">--updateProject</span><span class="sxs-lookup"><span data-stu-id="c829f-180">--updateProject</span></span> | <span data-ttu-id="c829f-181">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="c829f-181">The project to operate on.</span></span> |<span data-ttu-id="c829f-182">DotNet openapi Remove *--updateProject .\Ref.csproj* .\OpenAPI.JSON</span><span class="sxs-lookup"><span data-stu-id="c829f-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="c829f-183">-H</span><span class="sxs-lookup"><span data-stu-id="c829f-183">-h</span></span>|<span data-ttu-id="c829f-184">--help</span><span class="sxs-lookup"><span data-stu-id="c829f-184">--help</span></span>|<span data-ttu-id="c829f-185">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="c829f-185">Show help information</span></span>|<span data-ttu-id="c829f-186">openapi di DotNet--Guida</span><span class="sxs-lookup"><span data-stu-id="c829f-186">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="c829f-187">Argomenti</span><span class="sxs-lookup"><span data-stu-id="c829f-187">Arguments</span></span>

|  <span data-ttu-id="c829f-188">Argomento</span><span class="sxs-lookup"><span data-stu-id="c829f-188">Argument</span></span>  | <span data-ttu-id="c829f-189">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c829f-189">Description</span></span>| <span data-ttu-id="c829f-190">Esempio</span><span class="sxs-lookup"><span data-stu-id="c829f-190">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="c829f-191">file di origine</span><span class="sxs-lookup"><span data-stu-id="c829f-191">source-file</span></span> | <span data-ttu-id="c829f-192">Origine a cui rimuovere il riferimento.</span><span class="sxs-lookup"><span data-stu-id="c829f-192">The source to remove the reference to.</span></span> |<span data-ttu-id="c829f-193">*.\OpenAPI.JSON* DotNet openapi Remove</span><span class="sxs-lookup"><span data-stu-id="c829f-193">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="c829f-194">Aggiorna</span><span class="sxs-lookup"><span data-stu-id="c829f-194">Refresh</span></span>

<span data-ttu-id="c829f-195">Aggiorna la versione locale di un file scaricato usando il contenuto più recente dell'URL di download.</span><span class="sxs-lookup"><span data-stu-id="c829f-195">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="c829f-196">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c829f-196">Options</span></span>

| <span data-ttu-id="c829f-197">Short-opzione</span><span class="sxs-lookup"><span data-stu-id="c829f-197">Short option</span></span>| <span data-ttu-id="c829f-198">Opzione Long</span><span class="sxs-lookup"><span data-stu-id="c829f-198">Long option</span></span>| <span data-ttu-id="c829f-199">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c829f-199">Description</span></span> | <span data-ttu-id="c829f-200">Esempio</span><span class="sxs-lookup"><span data-stu-id="c829f-200">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="c829f-201">-p</span><span class="sxs-lookup"><span data-stu-id="c829f-201">-p</span></span>|<span data-ttu-id="c829f-202">--updateProject</span><span class="sxs-lookup"><span data-stu-id="c829f-202">--updateProject</span></span> | <span data-ttu-id="c829f-203">Progetto su cui operare.</span><span class="sxs-lookup"><span data-stu-id="c829f-203">The project to operate on.</span></span> | <span data-ttu-id="c829f-204">aggiornamento DotNet openapi *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="c829f-204">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="c829f-205">-H</span><span class="sxs-lookup"><span data-stu-id="c829f-205">-h</span></span>|<span data-ttu-id="c829f-206">--help</span><span class="sxs-lookup"><span data-stu-id="c829f-206">--help</span></span>|<span data-ttu-id="c829f-207">Mostra informazioni della Guida</span><span class="sxs-lookup"><span data-stu-id="c829f-207">Show help information</span></span>|<span data-ttu-id="c829f-208">aggiornamento di DotNet openapi--Help</span><span class="sxs-lookup"><span data-stu-id="c829f-208">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="c829f-209">Argomenti</span><span class="sxs-lookup"><span data-stu-id="c829f-209">Arguments</span></span>

|  <span data-ttu-id="c829f-210">Argomento</span><span class="sxs-lookup"><span data-stu-id="c829f-210">Argument</span></span>  | <span data-ttu-id="c829f-211">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c829f-211">Description</span></span> | <span data-ttu-id="c829f-212">Esempio</span><span class="sxs-lookup"><span data-stu-id="c829f-212">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="c829f-213">URL origine</span><span class="sxs-lookup"><span data-stu-id="c829f-213">source-URL</span></span> | <span data-ttu-id="c829f-214">URL da cui aggiornare il riferimento.</span><span class="sxs-lookup"><span data-stu-id="c829f-214">The URL to refresh the reference from.</span></span> | <span data-ttu-id="c829f-215">`https://contoso.com/openapi.json` di aggiornamento DotNet openapi</span><span class="sxs-lookup"><span data-stu-id="c829f-215">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
