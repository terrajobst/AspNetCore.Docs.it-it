---
title: Sviluppo di app ASP.NET Core con dotnet watch
author: rick-anderson
description: Questa esercitazione illustra come installare e usare lo strumento watcher per file dell'interfaccia della riga di comando di .NET Core (dotnet watch) in un'applicazione ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: cb15e28cb98ea82091cf5ddeed12df8926079e52
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="7729e-103">Sviluppo di app ASP.NET Core con dotnet watch</span><span class="sxs-lookup"><span data-stu-id="7729e-103">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="7729e-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="7729e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="7729e-105">`dotnet watch` è uno strumento che esegue un comando dell'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools) quando i file di origine vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="7729e-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="7729e-106">Ad esempio, una modifica di file può attivare la compilazione, l'esecuzione di test o la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="7729e-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="7729e-107">In questa esercitazione si usa un'app Web API esistente con due endpoint: uno restituisce una somma e uno restituisce un prodotto.</span><span class="sxs-lookup"><span data-stu-id="7729e-107">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="7729e-108">Il metodo del prodotto contiene un bug che verrà corretto nel corso di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7729e-108">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="7729e-109">Scaricare l'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="7729e-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="7729e-110">Contiene due progetti: *WebApp* (un'API Web di ASP.NET Core) e *WebAppTests* (unit test per l'API Web).</span><span class="sxs-lookup"><span data-stu-id="7729e-110">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="7729e-111">In una shell dei comandi passare alla cartella *WebApp* ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="7729e-111">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="7729e-112">L'output della console visualizza messaggi simili al seguente che indicano che l'app è in esecuzione e in attesa di richieste:</span><span class="sxs-lookup"><span data-stu-id="7729e-112">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="7729e-113">In un Web browser passare a `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="7729e-113">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="7729e-114">Dovrebbe essere visualizzato il risultato `9`.</span><span class="sxs-lookup"><span data-stu-id="7729e-114">You should see the result of `9`.</span></span>

<span data-ttu-id="7729e-115">Passare all'API del prodotto (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="7729e-115">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="7729e-116">Restituisce `9` e non `20` come previsto.</span><span class="sxs-lookup"><span data-stu-id="7729e-116">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="7729e-117">La correzione verrà apportata in una fase successiva dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7729e-117">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="7729e-118">Aggiungere `dotnet watch` a un progetto</span><span class="sxs-lookup"><span data-stu-id="7729e-118">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="7729e-119">Aggiungere un riferimento al pacchetto `Microsoft.DotNet.Watcher.Tools` nel file *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="7729e-119">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="7729e-120">Installare il pacchetto `Microsoft.DotNet.Watcher.Tools` eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="7729e-120">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="7729e-121">Esecuzione dei comandi dell'interfaccia della riga di comando di .NET Core con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="7729e-121">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="7729e-122">Qualsiasi [comando dell'interfaccia della riga di comando di .NET Core](/dotnet/core/tools#cli-commands) può essere eseguito con `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="7729e-122">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="7729e-123">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7729e-123">For example:</span></span>

| <span data-ttu-id="7729e-124">Comando</span><span class="sxs-lookup"><span data-stu-id="7729e-124">Command</span></span> | <span data-ttu-id="7729e-125">Comando con watch</span><span class="sxs-lookup"><span data-stu-id="7729e-125">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="7729e-126">dotnet run</span><span class="sxs-lookup"><span data-stu-id="7729e-126">dotnet run</span></span> | <span data-ttu-id="7729e-127">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="7729e-127">dotnet watch run</span></span> |
| <span data-ttu-id="7729e-128">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="7729e-128">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="7729e-129">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="7729e-129">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="7729e-130">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="7729e-130">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="7729e-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="7729e-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="7729e-132">dotnet test</span><span class="sxs-lookup"><span data-stu-id="7729e-132">dotnet test</span></span> | <span data-ttu-id="7729e-133">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="7729e-133">dotnet watch test</span></span> |

<span data-ttu-id="7729e-134">Eseguire `dotnet watch run` nella cartella *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="7729e-134">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="7729e-135">L'output della console indica che `watch` è stato avviato.</span><span class="sxs-lookup"><span data-stu-id="7729e-135">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="7729e-136">Apportare modifiche ai dati con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="7729e-136">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="7729e-137">Assicurarsi che `dotnet watch` sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7729e-137">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="7729e-138">Correggere il bug nel metodo `Product` di *MathController.cs* in modo che restituisca il prodotto e non la somma:</span><span class="sxs-lookup"><span data-stu-id="7729e-138">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="7729e-139">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7729e-139">Save the file.</span></span> <span data-ttu-id="7729e-140">L'output della console indica che `dotnet watch` ha rilevato una modifica del file e ha riavviato l'app.</span><span class="sxs-lookup"><span data-stu-id="7729e-140">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="7729e-141">Verificare che `http://localhost:<port number>/api/math/product?a=4&b=5` restituisca il risultato corretto.</span><span class="sxs-lookup"><span data-stu-id="7729e-141">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="7729e-142">Esecuzione di test usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="7729e-142">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="7729e-143">Modificare il metodo `Product` di *MathController.cs* di nuovo in modo che restituisca la somma e salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7729e-143">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="7729e-144">In una shell dei comandi passare alla cartella *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="7729e-144">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="7729e-145">Eseguire `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="7729e-145">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="7729e-146">Eseguire `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="7729e-146">Run `dotnet watch test`.</span></span> <span data-ttu-id="7729e-147">L'output indica che un test non è stato superato e che il watcher è in attesa di modifiche ai file:</span><span class="sxs-lookup"><span data-stu-id="7729e-147">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="7729e-148">Correggere il codice del metodo `Product` in modo che restituisca il prodotto.</span><span class="sxs-lookup"><span data-stu-id="7729e-148">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="7729e-149">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7729e-149">Save the file.</span></span>

<span data-ttu-id="7729e-150">`dotnet watch` rileva la modifica ai file ed esegue di nuovo i test.</span><span class="sxs-lookup"><span data-stu-id="7729e-150">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="7729e-151">L'output della console indica che i test sono stati superati.</span><span class="sxs-lookup"><span data-stu-id="7729e-151">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="7729e-152">dotnet-watch in GitHub</span><span class="sxs-lookup"><span data-stu-id="7729e-152">dotnet-watch in GitHub</span></span>

<span data-ttu-id="7729e-153">dotnet-watch fa parte del [repository DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch) GitHub.</span><span class="sxs-lookup"><span data-stu-id="7729e-153">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="7729e-154">La [sezione MSBuild](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) del file [Leggimi di dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) spiega come dotnet-watch può essere configurato dal file di progetto MSBuild che viene controllato.</span><span class="sxs-lookup"><span data-stu-id="7729e-154">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="7729e-155">Il file [Leggimi di dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contiene informazioni su dotnet-watch non trattate in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7729e-155">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
