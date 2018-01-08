---
title: Sviluppo di app ASP.NET Core con dotnet watch
author: rick-anderson
description: Questa esercitazione illustra come installare e usare lo strumento watcher per file dell'interfaccia della riga di comando di .NET Core (dotnet watch) in un'applicazione ASP.NET Core.
keywords: ASP.NET Core, usando dotnet watch
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 1b26beaa41f4b38e0cfd2c8300cb521a3dcce47d
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="b5a25-104">Sviluppo di app ASP.NET Core con dotnet watch</span><span class="sxs-lookup"><span data-stu-id="b5a25-104">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="b5a25-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="b5a25-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="b5a25-106">`dotnet watch` è uno strumento che esegue un comando dell'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools) quando i file di origine vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="b5a25-106">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="b5a25-107">Ad esempio, una modifica di file può attivare la compilazione, l'esecuzione di test o la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="b5a25-107">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="b5a25-108">In questa esercitazione si usa un'app Web API esistente con due endpoint: uno restituisce una somma e uno restituisce un prodotto.</span><span class="sxs-lookup"><span data-stu-id="b5a25-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="b5a25-109">Il metodo del prodotto contiene un bug che verrà corretto nel corso di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="b5a25-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="b5a25-110">Scaricare l'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="b5a25-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="b5a25-111">Contiene due progetti: *WebApp* (un'API Web di ASP.NET Core) e *WebAppTests* (unit test per l'API Web).</span><span class="sxs-lookup"><span data-stu-id="b5a25-111">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="b5a25-112">In una shell dei comandi passare alla cartella *WebApp* ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b5a25-112">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="b5a25-113">L'output della console visualizza messaggi simili al seguente che indicano che l'app è in esecuzione e in attesa di richieste:</span><span class="sxs-lookup"><span data-stu-id="b5a25-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="b5a25-114">In un Web browser passare a `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="b5a25-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="b5a25-115">Dovrebbe essere visualizzato il risultato `9`.</span><span class="sxs-lookup"><span data-stu-id="b5a25-115">You should see the result of `9`.</span></span>

<span data-ttu-id="b5a25-116">Passare all'API del prodotto (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="b5a25-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="b5a25-117">Restituisce `9` e non `20` come previsto.</span><span class="sxs-lookup"><span data-stu-id="b5a25-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="b5a25-118">La correzione verrà apportata in una fase successiva dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="b5a25-118">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="b5a25-119">Aggiungere `dotnet watch` a un progetto</span><span class="sxs-lookup"><span data-stu-id="b5a25-119">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="b5a25-120">Aggiungere un riferimento al pacchetto `Microsoft.DotNet.Watcher.Tools` nel file *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="b5a25-120">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="b5a25-121">Installare il pacchetto `Microsoft.DotNet.Watcher.Tools` eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b5a25-121">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="b5a25-122">Esecuzione dei comandi dell'interfaccia della riga di comando di .NET Core con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="b5a25-122">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="b5a25-123">Qualsiasi [comando dell'interfaccia della riga di comando di .NET Core](/dotnet/core/tools#cli-commands) può essere eseguito con `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="b5a25-123">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="b5a25-124">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b5a25-124">For example:</span></span>

| <span data-ttu-id="b5a25-125">Comando</span><span class="sxs-lookup"><span data-stu-id="b5a25-125">Command</span></span> | <span data-ttu-id="b5a25-126">Comando con watch</span><span class="sxs-lookup"><span data-stu-id="b5a25-126">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="b5a25-127">dotnet run</span><span class="sxs-lookup"><span data-stu-id="b5a25-127">dotnet run</span></span> | <span data-ttu-id="b5a25-128">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="b5a25-128">dotnet watch run</span></span> |
| <span data-ttu-id="b5a25-129">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="b5a25-129">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="b5a25-130">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="b5a25-130">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="b5a25-131">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="b5a25-131">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="b5a25-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="b5a25-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="b5a25-133">dotnet test</span><span class="sxs-lookup"><span data-stu-id="b5a25-133">dotnet test</span></span> | <span data-ttu-id="b5a25-134">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="b5a25-134">dotnet watch test</span></span> |

<span data-ttu-id="b5a25-135">Eseguire `dotnet watch run` nella cartella *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="b5a25-135">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="b5a25-136">L'output della console indica che `watch` è stato avviato.</span><span class="sxs-lookup"><span data-stu-id="b5a25-136">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="b5a25-137">Apportare modifiche ai dati con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="b5a25-137">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="b5a25-138">Assicurarsi che `dotnet watch` sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b5a25-138">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="b5a25-139">Correggere il bug nel metodo `Product` di *MathController.cs* in modo che restituisca il prodotto e non la somma:</span><span class="sxs-lookup"><span data-stu-id="b5a25-139">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="b5a25-140">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="b5a25-140">Save the file.</span></span> <span data-ttu-id="b5a25-141">L'output della console indica che `dotnet watch` ha rilevato una modifica del file e ha riavviato l'app.</span><span class="sxs-lookup"><span data-stu-id="b5a25-141">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="b5a25-142">Verificare che `http://localhost:<port number>/api/math/product?a=4&b=5` restituisca il risultato corretto.</span><span class="sxs-lookup"><span data-stu-id="b5a25-142">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="b5a25-143">Esecuzione di test usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="b5a25-143">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="b5a25-144">Modificare il metodo `Product` di *MathController.cs* di nuovo in modo che restituisca la somma e salvare il file.</span><span class="sxs-lookup"><span data-stu-id="b5a25-144">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="b5a25-145">In una shell dei comandi passare alla cartella *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="b5a25-145">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="b5a25-146">Eseguire `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="b5a25-146">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="b5a25-147">Eseguire `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="b5a25-147">Run `dotnet watch test`.</span></span> <span data-ttu-id="b5a25-148">L'output indica che un test non è stato superato e che il watcher è in attesa di modifiche ai file:</span><span class="sxs-lookup"><span data-stu-id="b5a25-148">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="b5a25-149">Correggere il codice del metodo `Product` in modo che restituisca il prodotto.</span><span class="sxs-lookup"><span data-stu-id="b5a25-149">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="b5a25-150">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="b5a25-150">Save the file.</span></span>

<span data-ttu-id="b5a25-151">`dotnet watch` rileva la modifica ai file ed esegue di nuovo i test.</span><span class="sxs-lookup"><span data-stu-id="b5a25-151">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="b5a25-152">L'output della console indica che i test sono stati superati.</span><span class="sxs-lookup"><span data-stu-id="b5a25-152">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="b5a25-153">dotnet-watch in GitHub</span><span class="sxs-lookup"><span data-stu-id="b5a25-153">dotnet-watch in GitHub</span></span>

<span data-ttu-id="b5a25-154">dotnet-watch fa parte del [repository DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch) GitHub.</span><span class="sxs-lookup"><span data-stu-id="b5a25-154">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="b5a25-155">La [sezione MSBuild](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) del file [Leggimi di dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) spiega come dotnet-watch può essere configurato dal file di progetto MSBuild che viene controllato.</span><span class="sxs-lookup"><span data-stu-id="b5a25-155">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="b5a25-156">Il file [Leggimi di dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contiene informazioni su dotnet-watch non trattate in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="b5a25-156">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
