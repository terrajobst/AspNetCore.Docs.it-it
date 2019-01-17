---
title: Sviluppare app ASP.NET Core usando un watcher per file
author: rick-anderson
description: Questa esercitazione illustra come installare e usare lo strumento watcher per file (dotnet watch) dell'interfaccia della riga di comando di .NET Core in un'app ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: f1e0d91b27df4af7cbfb6f2547c94c0370c65d0d
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207502"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="d038e-103">Sviluppare app ASP.NET Core usando un watcher per file</span><span class="sxs-lookup"><span data-stu-id="d038e-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="d038e-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="d038e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="d038e-105">`dotnet watch` è uno strumento che esegue un comando dell'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools) quando i file di origine vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="d038e-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="d038e-106">Ad esempio, una modifica di file può attivare la compilazione, l'esecuzione di test o la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="d038e-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="d038e-107">Questa esercitazione usa un'API Web esistente con due endpoint, di cui uno restituisce una somma e l'altro un prodotto.</span><span class="sxs-lookup"><span data-stu-id="d038e-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="d038e-108">Il metodo del prodotto ha un bug, che verrà corretto in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d038e-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="d038e-109">Scaricare l'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="d038e-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="d038e-110">L'esempio è costituito da due progetti: *WebApp* (un'API Web di ASP.NET Core) e *WebAppTests* (unit test per l'API Web).</span><span class="sxs-lookup"><span data-stu-id="d038e-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="d038e-111">In una shell dei comandi passare alla cartella *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="d038e-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="d038e-112">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d038e-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="d038e-113">L'output della console visualizza messaggi simili al seguente che indicano che l'app è in esecuzione e in attesa di richieste:</span><span class="sxs-lookup"><span data-stu-id="d038e-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="d038e-114">In un Web browser passare a `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="d038e-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="d038e-115">Dovrebbe essere visualizzato il risultato `9`.</span><span class="sxs-lookup"><span data-stu-id="d038e-115">You should see the result of `9`.</span></span>

<span data-ttu-id="d038e-116">Passare all'API del prodotto (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="d038e-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="d038e-117">Restituisce `9` e non `20` come previsto.</span><span class="sxs-lookup"><span data-stu-id="d038e-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="d038e-118">Questo problema verrà corretto più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d038e-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="d038e-119">Aggiungere `dotnet watch` a un progetto</span><span class="sxs-lookup"><span data-stu-id="d038e-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="d038e-120">Lo strumento watcher per file `dotnet watch` è incluso nella versione 2.1.300 di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="d038e-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="d038e-121">Se si usa una versione precedente di .NET Core SDK, i passaggi seguenti sono obbligatori.</span><span class="sxs-lookup"><span data-stu-id="d038e-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="d038e-122">Aggiungere un riferimento al pacchetto `Microsoft.DotNet.Watcher.Tools` nel file *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="d038e-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="d038e-123">Installare il pacchetto `Microsoft.DotNet.Watcher.Tools` eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d038e-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="d038e-124">Eseguire i comandi dell'interfaccia della riga di comando di .NET Core con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="d038e-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="d038e-125">Qualsiasi [comando dell'interfaccia della riga di comando di .NET Core](/dotnet/core/tools#cli-commands) può essere eseguito con `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="d038e-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="d038e-126">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d038e-126">For example:</span></span>

| <span data-ttu-id="d038e-127">Comando</span><span class="sxs-lookup"><span data-stu-id="d038e-127">Command</span></span> | <span data-ttu-id="d038e-128">Comando con watch</span><span class="sxs-lookup"><span data-stu-id="d038e-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="d038e-129">dotnet run</span><span class="sxs-lookup"><span data-stu-id="d038e-129">dotnet run</span></span> | <span data-ttu-id="d038e-130">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="d038e-130">dotnet watch run</span></span> |
| <span data-ttu-id="d038e-131">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="d038e-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="d038e-132">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="d038e-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="d038e-133">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="d038e-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="d038e-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="d038e-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="d038e-135">dotnet test</span><span class="sxs-lookup"><span data-stu-id="d038e-135">dotnet test</span></span> | <span data-ttu-id="d038e-136">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="d038e-136">dotnet watch test</span></span> |

<span data-ttu-id="d038e-137">Eseguire `dotnet watch run` nella cartella *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="d038e-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="d038e-138">L'output della console indica che `watch` è stato avviato.</span><span class="sxs-lookup"><span data-stu-id="d038e-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="d038e-139">Apportare modifiche con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="d038e-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="d038e-140">Assicurarsi che `dotnet watch` sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d038e-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="d038e-141">Correggere il bug nel metodo `Product` di *MathController.cs* in modo che restituisca il prodotto e non la somma:</span><span class="sxs-lookup"><span data-stu-id="d038e-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="d038e-142">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="d038e-142">Save the file.</span></span> <span data-ttu-id="d038e-143">L'output della console indica che `dotnet watch` ha rilevato una modifica del file e ha riavviato l'app.</span><span class="sxs-lookup"><span data-stu-id="d038e-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="d038e-144">Verificare che `http://localhost:<port number>/api/math/product?a=4&b=5` restituisca il risultato corretto.</span><span class="sxs-lookup"><span data-stu-id="d038e-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="d038e-145">Eseguire test con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="d038e-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="d038e-146">Modificare il metodo `Product` di *MathController.cs* in modo che restituisca la somma.</span><span class="sxs-lookup"><span data-stu-id="d038e-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="d038e-147">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="d038e-147">Save the file.</span></span>
1. <span data-ttu-id="d038e-148">In una shell dei comandi passare alla cartella *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="d038e-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="d038e-149">Eseguire [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="d038e-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="d038e-150">Eseguire `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="d038e-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="d038e-151">L'output indica che un test non è stato superato e che il watcher è in attesa di modifiche ai file:</span><span class="sxs-lookup"><span data-stu-id="d038e-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="d038e-152">Correggere il codice del metodo `Product` in modo che restituisca il prodotto.</span><span class="sxs-lookup"><span data-stu-id="d038e-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="d038e-153">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="d038e-153">Save the file.</span></span>

<span data-ttu-id="d038e-154">`dotnet watch` rileva la modifica ai file ed esegue di nuovo i test.</span><span class="sxs-lookup"><span data-stu-id="d038e-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="d038e-155">L'output della console indica che i test sono stati superati.</span><span class="sxs-lookup"><span data-stu-id="d038e-155">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="d038e-156">Personalizzare l'elenco dei file da controllare</span><span class="sxs-lookup"><span data-stu-id="d038e-156">Customize files list to watch</span></span>

<span data-ttu-id="d038e-157">Per impostazione predefinita, `dotnet-watch` tiene traccia di tutti i file che soddisfano i criteri GLOB seguenti:</span><span class="sxs-lookup"><span data-stu-id="d038e-157">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="d038e-158">È possibile aggiungere altri elementi all'elenco di controllo modificando il file con estensione *csproj*.</span><span class="sxs-lookup"><span data-stu-id="d038e-158">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="d038e-159">Gli elementi possono essere specificati singolarmente o tramite criteri GLOB.</span><span class="sxs-lookup"><span data-stu-id="d038e-159">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="d038e-160">Esclusione di file dal controllo</span><span class="sxs-lookup"><span data-stu-id="d038e-160">Opt-out of files to be watched</span></span>

<span data-ttu-id="d038e-161">È possibile configurare `dotnet-watch` in modo che ignori le impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="d038e-161">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="d038e-162">Per ignorare file specifici, aggiungere l'attributo `Watch="false"` alla definizione di un elemento nel file con estensione *csproj*:</span><span class="sxs-lookup"><span data-stu-id="d038e-162">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a><span data-ttu-id="d038e-163">Progetti di controllo personalizzati</span><span class="sxs-lookup"><span data-stu-id="d038e-163">Custom watch projects</span></span>

<span data-ttu-id="d038e-164">`dotnet-watch` non è limitato a progetti C#.</span><span class="sxs-lookup"><span data-stu-id="d038e-164">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="d038e-165">È possibile creare progetti di controllo personalizzati per gestire scenari diversi.</span><span class="sxs-lookup"><span data-stu-id="d038e-165">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="d038e-166">Si consideri il layout di progetto seguente:</span><span class="sxs-lookup"><span data-stu-id="d038e-166">Consider the following project layout:</span></span>

* <span data-ttu-id="d038e-167">**test/**</span><span class="sxs-lookup"><span data-stu-id="d038e-167">**test/**</span></span>
  * <span data-ttu-id="d038e-168">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="d038e-168">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="d038e-169">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="d038e-169">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="d038e-170">Se l'obiettivo consiste nel controllare entrambi i progetti, creare un file di progetto personalizzato configurato per controllarli entrambi:</span><span class="sxs-lookup"><span data-stu-id="d038e-170">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

<span data-ttu-id="d038e-171">Per avviare il controllo dei file per entrambi i progetti, passare alla cartella *test*.</span><span class="sxs-lookup"><span data-stu-id="d038e-171">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="d038e-172">Eseguire il seguente comando:</span><span class="sxs-lookup"><span data-stu-id="d038e-172">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="d038e-173">VSTest viene eseguito quando un qualsiasi file viene modificato in uno dei progetti di test.</span><span class="sxs-lookup"><span data-stu-id="d038e-173">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="d038e-174">`dotnet-watch` in GitHub</span><span class="sxs-lookup"><span data-stu-id="d038e-174">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="d038e-175">`dotnet-watch` fa parte del [repository aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch) di GitHub.</span><span class="sxs-lookup"><span data-stu-id="d038e-175">`dotnet-watch` is part of the GitHub [aspnet/AspNetCore repository](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span></span>
