---
title: Debug ASP.NET Core Blazer
author: guardrex
description: Informazioni su come eseguire il debug di app blazer.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/22/2019
uid: blazor/debug
ms.openlocfilehash: e9477e504d32fd1dd5d6c87392386d1131f46e9f
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2019
ms.locfileid: "70964002"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="f5861-103">Debug ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="f5861-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="f5861-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="f5861-104">Daniel Roth</span></span>](https://github.com/danroth27)

<span data-ttu-id="f5861-105">Il supporto *anticipato* è disponibile per il debug di app webassembly in esecuzione su webassembly in Chrome.</span><span class="sxs-lookup"><span data-stu-id="f5861-105">*Early* support exists for debugging Blazor WebAssembly apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="f5861-106">Le funzionalità del debugger sono limitate.</span><span class="sxs-lookup"><span data-stu-id="f5861-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="f5861-107">Gli scenari disponibili includono:</span><span class="sxs-lookup"><span data-stu-id="f5861-107">Available scenarios include:</span></span>

* <span data-ttu-id="f5861-108">Impostare e rimuovere punti di interruzione.</span><span class="sxs-lookup"><span data-stu-id="f5861-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="f5861-109">Singolo passaggio (`F10`) tramite il codice o l'esecuzione del`F8`codice Resume ().</span><span class="sxs-lookup"><span data-stu-id="f5861-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="f5861-110">Nella visualizzazione variabili *locali* osservare i valori delle variabili locali di tipo `int`, `string`e `bool`.</span><span class="sxs-lookup"><span data-stu-id="f5861-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="f5861-111">Vedere lo stack di chiamate, incluse le catene di chiamate che passano da JavaScript a .NET e da .NET a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f5861-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="f5861-112">*Non è possibile*:</span><span class="sxs-lookup"><span data-stu-id="f5861-112">You *can't*:</span></span>

* <span data-ttu-id="f5861-113">Osservare i valori delle variabili locali che non sono `int`, `string`o `bool`.</span><span class="sxs-lookup"><span data-stu-id="f5861-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="f5861-114">Osservare i valori di qualsiasi proprietà o campo di una classe.</span><span class="sxs-lookup"><span data-stu-id="f5861-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="f5861-115">Passare il mouse sulle variabili per visualizzarne i valori.</span><span class="sxs-lookup"><span data-stu-id="f5861-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="f5861-116">Valutare le espressioni nella console.</span><span class="sxs-lookup"><span data-stu-id="f5861-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="f5861-117">Eseguire un passaggio tra le chiamate asincrone.</span><span class="sxs-lookup"><span data-stu-id="f5861-117">Step across async calls.</span></span>
* <span data-ttu-id="f5861-118">Eseguire la maggior parte degli altri scenari di debug comuni.</span><span class="sxs-lookup"><span data-stu-id="f5861-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="f5861-119">Lo sviluppo di altri scenari di debug è un'attività in corso del team di progettazione.</span><span class="sxs-lookup"><span data-stu-id="f5861-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5861-120">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f5861-120">Prerequisites</span></span>

<span data-ttu-id="f5861-121">Il debug richiede uno dei seguenti browser:</span><span class="sxs-lookup"><span data-stu-id="f5861-121">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="f5861-122">Google Chrome (versione 70 o successiva)</span><span class="sxs-lookup"><span data-stu-id="f5861-122">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="f5861-123">Anteprima di Microsoft Edge ([canale di sviluppo Edge](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="f5861-123">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="f5861-124">Routine</span><span class="sxs-lookup"><span data-stu-id="f5861-124">Procedure</span></span>

1. <span data-ttu-id="f5861-125">Eseguire un'app webassembly Blazer in `Debug` Configuration.</span><span class="sxs-lookup"><span data-stu-id="f5861-125">Run a Blazor WebAssembly app in `Debug` configuration.</span></span> <span data-ttu-id="f5861-126">Passare l' `--configuration Debug` opzione al comando [DotNet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="f5861-126">Pass the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="f5861-127">Accedere all'app nel browser.</span><span class="sxs-lookup"><span data-stu-id="f5861-127">Access the app in the browser.</span></span>
1. <span data-ttu-id="f5861-128">Posizionare lo stato attivo della tastiera sull'app, non sul pannello strumenti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f5861-128">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="f5861-129">Il pannello strumenti di sviluppo può essere chiuso al momento dell'avvio del debug.</span><span class="sxs-lookup"><span data-stu-id="f5861-129">The developer tools panel can be closed when debugging is initiated.</span></span>
1. <span data-ttu-id="f5861-130">Selezionare il seguente tasto di scelta rapida specifico di Blazer:</span><span class="sxs-lookup"><span data-stu-id="f5861-130">Select the following Blazor-specific keyboard shortcut:</span></span>
   * <span data-ttu-id="f5861-131">`Shift+Alt+D`in Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="f5861-131">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="f5861-132">`Shift+Cmd+D`in macOS</span><span class="sxs-lookup"><span data-stu-id="f5861-132">`Shift+Cmd+D` on macOS</span></span>
1. <span data-ttu-id="f5861-133">Seguire i passaggi elencati sullo schermo per riavviare il browser con il debug remoto abilitato.</span><span class="sxs-lookup"><span data-stu-id="f5861-133">Follow the steps listed on the screen to restart the browser with remote debugging enabled.</span></span>
1. <span data-ttu-id="f5861-134">Per avviare la sessione di debug, selezionare di nuovo il seguente tasto di scelta rapida specifico di Blazer:</span><span class="sxs-lookup"><span data-stu-id="f5861-134">Select the following Blazor-specific keyboard shortcut once again to start the debug session:</span></span>
   * <span data-ttu-id="f5861-135">`Shift+Alt+D`in Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="f5861-135">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="f5861-136">`Shift+Cmd+D`in macOS</span><span class="sxs-lookup"><span data-stu-id="f5861-136">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="f5861-137">Abilita debug remoto</span><span class="sxs-lookup"><span data-stu-id="f5861-137">Enable remote debugging</span></span>

<span data-ttu-id="f5861-138">Se il debug remoto è disabilitato, la pagina di errore **di una scheda del browser di cui è possibile eseguire il debug** viene generata da Chrome.</span><span class="sxs-lookup"><span data-stu-id="f5861-138">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="f5861-139">La pagina di errore contiene le istruzioni per eseguire Chrome con la porta di debug aperta, in modo che il proxy di debug di Blazer possa connettersi all'app.</span><span class="sxs-lookup"><span data-stu-id="f5861-139">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="f5861-140">*Chiudere tutte le istanze di Chrome* e riavviare Chrome come indicato.</span><span class="sxs-lookup"><span data-stu-id="f5861-140">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="f5861-141">Eseguire il debug dell'app</span><span class="sxs-lookup"><span data-stu-id="f5861-141">Debug the app</span></span>

<span data-ttu-id="f5861-142">Quando Chrome è in esecuzione con il debug remoto abilitato, il tasto di scelta rapida per il debug apre una nuova scheda del debugger. Dopo qualche istante la scheda **Sources (origini** ) Mostra un elenco degli assembly .NET nell'app.</span><span class="sxs-lookup"><span data-stu-id="f5861-142">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="f5861-143">Espandere ogni assembly e trovare i file di origine *. cs*/ *. Razor* disponibili per il debug.</span><span class="sxs-lookup"><span data-stu-id="f5861-143">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="f5861-144">Impostare i punti di interruzione, tornare alla scheda dell'app e i punti di interruzione vengono raggiunti quando viene eseguito il codice.</span><span class="sxs-lookup"><span data-stu-id="f5861-144">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="f5861-145">Quando viene raggiunto un punto di interruzione, l'esecuzione`F10`del codice a un solo passaggio (`F8`) tramite il codice o riprende () normalmente.</span><span class="sxs-lookup"><span data-stu-id="f5861-145">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

<span data-ttu-id="f5861-146">Blazer fornisce un proxy di debug che implementa il [protocollo devtools di Chrome](https://chromedevtools.github.io/devtools-protocol/) e potenzia il protocollo con. Informazioni specifiche del NET.</span><span class="sxs-lookup"><span data-stu-id="f5861-146">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="f5861-147">Quando si preme il tasto di scelta rapida per il debug, blazer punta il DevTools di Chrome sul proxy.</span><span class="sxs-lookup"><span data-stu-id="f5861-147">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="f5861-148">Il proxy si connette alla finestra del browser che si sta tentando di eseguire il debug, quindi è necessario abilitare il debug remoto.</span><span class="sxs-lookup"><span data-stu-id="f5861-148">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="f5861-149">Mappe di origine del browser</span><span class="sxs-lookup"><span data-stu-id="f5861-149">Browser source maps</span></span>

<span data-ttu-id="f5861-150">Le mappe di origine del browser consentono al browser di eseguire il mapping dei file compilati ai file di origine originali e vengono comunemente usati per il debug sul lato client.</span><span class="sxs-lookup"><span data-stu-id="f5861-150">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="f5861-151">Tuttavia, Blazer non esegue attualmente C# il mapping direttamente a JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="f5861-151">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="f5861-152">Al contrario, blazer esegue l'interpretazione IL nel browser, quindi le mappe di origine non sono rilevanti.</span><span class="sxs-lookup"><span data-stu-id="f5861-152">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="f5861-153">Suggerimento per la risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="f5861-153">Troubleshooting tip</span></span>

<span data-ttu-id="f5861-154">Se si verificano errori, è possibile che venga utile il suggerimento seguente:</span><span class="sxs-lookup"><span data-stu-id="f5861-154">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="f5861-155">Nella scheda **debugger** aprire gli strumenti di sviluppo nel browser.</span><span class="sxs-lookup"><span data-stu-id="f5861-155">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="f5861-156">Nella console eseguire `localStorage.clear()` per rimuovere tutti i punti di interruzione.</span><span class="sxs-lookup"><span data-stu-id="f5861-156">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
