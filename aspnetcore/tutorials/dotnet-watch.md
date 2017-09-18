---
title: Sviluppo di app ASP.NET Core con dotnet watch
author: rick-anderson
description: Illustra come usare dotnet watch.
keywords: ASP.NET Core, usando dotnet watch
ms.author: riande
manager: wpickett
ms.date: 03/09/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 2ddcbfc30a839ed8dd72a632644bf73dcea777ac
ms.sourcegitcommit: b02db6da115e55140da91b67355aaf56aae1703f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="5f36b-104">Sviluppo di app ASP.NET Core con dotnet watch</span><span class="sxs-lookup"><span data-stu-id="5f36b-104">Developing ASP.NET Core apps using dotnet watch</span></span>


<span data-ttu-id="5f36b-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="5f36b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="5f36b-106">`dotnet watch` è uno strumento che esegue un comando `dotnet` quando i file di origine vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="5f36b-106">`dotnet watch` is a tool that runs a `dotnet` command when source files change.</span></span> <span data-ttu-id="5f36b-107">Ad esempio, una modifica di file può attivare la compilazione, i test o la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="5f36b-107">For example, a file change can trigger compilation, tests, or deployment.</span></span>

<span data-ttu-id="5f36b-108">In questa esercitazione si usa un'app Web API esistente con due endpoint: uno restituisce una somma e uno restituisce un prodotto.</span><span class="sxs-lookup"><span data-stu-id="5f36b-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="5f36b-109">Il metodo del prodotto contiene un bug che verrà corretto nel corso di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5f36b-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="5f36b-110">Scaricare l'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="5f36b-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="5f36b-111">Contiene due progetti, `WebApp` (un'app Web) e `WebAppTests` (unit test per l'app Web).</span><span class="sxs-lookup"><span data-stu-id="5f36b-111">It contains two projects, `WebApp` (a web app) and `WebAppTests` (unit tests for the web app).</span></span>

<span data-ttu-id="5f36b-112">In una console passare alla cartella WebApp ed eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="5f36b-112">In a console, navigate to the WebApp folder and run the following commands:</span></span>

- `dotnet restore`
- `dotnet run`

<span data-ttu-id="5f36b-113">L'output della console visualizzerà messaggi simili al seguente che indicano che l'app è in esecuzione e in attesa di richieste:</span><span class="sxs-lookup"><span data-stu-id="5f36b-113">The console output will show messages similar to the following (indicating that the app is running and waiting for requests):</span></span>

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="5f36b-114">In un Web browser passare a `http://localhost:5000/api/math/sum?a=4&b=5`; il risultato dovrebbe essere `9`.</span><span class="sxs-lookup"><span data-stu-id="5f36b-114">In a web browser, navigate to `http://localhost:5000/api/math/sum?a=4&b=5`, you should see the result `9`.</span></span>

<span data-ttu-id="5f36b-115">Passare all'API del prodotto (`http://localhost:5000/api/math/product?a=4&b=5`), viene restituito `9`, non `20` come previsto.</span><span class="sxs-lookup"><span data-stu-id="5f36b-115">Navigate to the product API (`http://localhost:5000/api/math/product?a=4&b=5`), it returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="5f36b-116">La correzione verrà apportata in una fase successiva dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5f36b-116">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="5f36b-117">Aggiungere `dotnet watch` a un progetto</span><span class="sxs-lookup"><span data-stu-id="5f36b-117">Add `dotnet watch` to a project</span></span>

- <span data-ttu-id="5f36b-118">Aggiungere `Microsoft.DotNet.Watcher.Tools` al file *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="5f36b-118">Add `Microsoft.DotNet.Watcher.Tools` to the *.csproj* file:</span></span>
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
 </ItemGroup> 
 ```

- <span data-ttu-id="5f36b-119">Eseguire `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="5f36b-119">Run `dotnet restore`.</span></span>

## <a name="running-dotnet-commands-using-dotnet-watch"></a><span data-ttu-id="5f36b-120">Esecuzione di comandi `dotnet` usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="5f36b-120">Running `dotnet` commands using `dotnet watch`</span></span>

<span data-ttu-id="5f36b-121">Qualsiasi comando `dotnet` può essere eseguito con `dotnet watch`, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="5f36b-121">Any `dotnet` command can be run with `dotnet watch`, for example:</span></span>

| <span data-ttu-id="5f36b-122">Comando</span><span class="sxs-lookup"><span data-stu-id="5f36b-122">Command</span></span> | <span data-ttu-id="5f36b-123">Comando con watch</span><span class="sxs-lookup"><span data-stu-id="5f36b-123">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="5f36b-124">dotnet run</span><span class="sxs-lookup"><span data-stu-id="5f36b-124">dotnet run</span></span> | <span data-ttu-id="5f36b-125">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="5f36b-125">dotnet watch run</span></span> |
| <span data-ttu-id="5f36b-126">dotnet run -f net451</span><span class="sxs-lookup"><span data-stu-id="5f36b-126">dotnet run -f net451</span></span> | <span data-ttu-id="5f36b-127">dotnet watch run -f net451</span><span class="sxs-lookup"><span data-stu-id="5f36b-127">dotnet watch run -f net451</span></span> |
| <span data-ttu-id="5f36b-128">dotnet run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="5f36b-128">dotnet run -f net451 -- --arg1</span></span> | <span data-ttu-id="5f36b-129">dotnet watch run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="5f36b-129">dotnet watch run -f net451 -- --arg1</span></span> |
| <span data-ttu-id="5f36b-130">dotnet test</span><span class="sxs-lookup"><span data-stu-id="5f36b-130">dotnet test</span></span> | <span data-ttu-id="5f36b-131">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="5f36b-131">dotnet watch test</span></span> |

<span data-ttu-id="5f36b-132">Eseguire `dotnet watch run` nella cartella `WebApp`.</span><span class="sxs-lookup"><span data-stu-id="5f36b-132">Run `dotnet watch run` in the `WebApp` folder.</span></span> <span data-ttu-id="5f36b-133">L'output della console indicherà che `watch` è stato avviato.</span><span class="sxs-lookup"><span data-stu-id="5f36b-133">The console output will indicate `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="5f36b-134">Apportare modifiche ai dati con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="5f36b-134">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="5f36b-135">Assicurarsi che `dotnet watch` sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5f36b-135">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="5f36b-136">Correggere il bug nel metodo `Product` di `MathController` in modo che restituisca il prodotto e non la somma.</span><span class="sxs-lookup"><span data-stu-id="5f36b-136">Fix the bug in the `Product` method of the `MathController` so it returns the product and not the sum.</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="5f36b-137">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="5f36b-137">Save the file.</span></span> <span data-ttu-id="5f36b-138">L'output della console visualizzerà i messaggi che indicano che `dotnet watch` ha rilevato una modifica del file e ha riavviato l'app.</span><span class="sxs-lookup"><span data-stu-id="5f36b-138">The console output will show messages indicating that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="5f36b-139">Verificare che `http://localhost:5000/api/math/product?a=4&b=5` restituisca il risultato corretto.</span><span class="sxs-lookup"><span data-stu-id="5f36b-139">Verify `http://localhost:5000/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="5f36b-140">Esecuzione di test usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="5f36b-140">Running tests using `dotnet watch`</span></span>

- <span data-ttu-id="5f36b-141">Modificare il metodo `Product` di `MathController` di nuovo in modo che restituisca la somma e salvare il file.</span><span class="sxs-lookup"><span data-stu-id="5f36b-141">Change the `Product` method of the `MathController` back to returning the sum and save the file.</span></span>
- <span data-ttu-id="5f36b-142">In una finestra di comando passare alla cartella `WebAppTests`.</span><span class="sxs-lookup"><span data-stu-id="5f36b-142">In a command window, naviagate to the `WebAppTests` folder.</span></span>
- <span data-ttu-id="5f36b-143">Eseguire `dotnet restore`</span><span class="sxs-lookup"><span data-stu-id="5f36b-143">Run `dotnet restore`</span></span>
- <span data-ttu-id="5f36b-144">Eseguire `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="5f36b-144">Run `dotnet watch test`.</span></span> <span data-ttu-id="5f36b-145">Viene visualizzato l'output che indica che un test non è stato superato e che il watcher è in attesa di modifiche ai file:</span><span class="sxs-lookup"><span data-stu-id="5f36b-145">You see output indicating that a test failed and that watcher is waiting for file changes:</span></span>

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- <span data-ttu-id="5f36b-146">Correggere il codice del metodo `Product` in modo che restituisca il prodotto.</span><span class="sxs-lookup"><span data-stu-id="5f36b-146">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="5f36b-147">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="5f36b-147">Save the file.</span></span>

<span data-ttu-id="5f36b-148">`dotnet watch` rileva la modifica ai file ed esegue di nuovo i test.</span><span class="sxs-lookup"><span data-stu-id="5f36b-148">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="5f36b-149">L'output della console indicherà che i test sono stati superati.</span><span class="sxs-lookup"><span data-stu-id="5f36b-149">The console output will show the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="5f36b-150">dotnet-watch in GitHub</span><span class="sxs-lookup"><span data-stu-id="5f36b-150">dotnet-watch in GitHub</span></span>

<span data-ttu-id="5f36b-151">dotnet-watch fa parte del [repository DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools) GitHub.</span><span class="sxs-lookup"><span data-stu-id="5f36b-151">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="5f36b-152">La [sezione MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) del file [Leggimi di dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) spiega come dotnet-watch può essere configurato dal file di progetto MSBuild che viene controllato.</span><span class="sxs-lookup"><span data-stu-id="5f36b-152">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="5f36b-153">Il file [Leggimi di dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contiene informazioni su dotnet-watch non trattate in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5f36b-153">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
