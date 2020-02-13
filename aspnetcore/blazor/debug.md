---
title: Debug ASP.NET Core Blazor
author: guardrex
description: Informazioni su come eseguire il debug di app Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159989"
---
# <a name="debug-aspnet-core-opno-locblazor"></a><span data-ttu-id="7fdbe-103">Debug ASP.NET Core [!OP.NO-LOC(Blazor)]</span><span class="sxs-lookup"><span data-stu-id="7fdbe-103">Debug ASP.NET Core [!OP.NO-LOC(Blazor)]</span></span>

[<span data-ttu-id="7fdbe-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="7fdbe-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="7fdbe-105">Il supporto *anticipato* esiste per il debug [!OP.NO-LOC(Blazor)] webassembly usando gli strumenti di sviluppo del browser nei browser basati su cromo (Chrome/Microsoft Edge).</span><span class="sxs-lookup"><span data-stu-id="7fdbe-105">*Early* support exists for debugging [!OP.NO-LOC(Blazor)] WebAssembly using the browser dev tools in Chromium-based browsers (Chrome/Edge).</span></span> <span data-ttu-id="7fdbe-106">Il lavoro è in corso per:</span><span class="sxs-lookup"><span data-stu-id="7fdbe-106">Work is in progress to:</span></span>

* <span data-ttu-id="7fdbe-107">Abilita completamente il debug in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-107">Fully enable debugging in Visual Studio.</span></span>
* <span data-ttu-id="7fdbe-108">Abilitare il debug in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-108">Enable debugging in Visual Studio Code.</span></span>

<span data-ttu-id="7fdbe-109">Le funzionalità del debugger sono limitate.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-109">Debugger capabilities are limited.</span></span> <span data-ttu-id="7fdbe-110">Gli scenari disponibili includono:</span><span class="sxs-lookup"><span data-stu-id="7fdbe-110">Available scenarios include:</span></span>

* <span data-ttu-id="7fdbe-111">Impostare e rimuovere punti di interruzione.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-111">Set and remove breakpoints.</span></span>
* <span data-ttu-id="7fdbe-112">Singolo passaggio (`F10`) tramite il codice o la ripresa (`F8`) esecuzione del codice.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-112">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="7fdbe-113">Nella visualizzazione variabili *locali* osservare i valori di tutte le variabili locali di tipo `int`, `string`e `bool`.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-113">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="7fdbe-114">Vedere lo stack di chiamate, incluse le catene di chiamate che passano da JavaScript a .NET e da .NET a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-114">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="7fdbe-115">*Non è possibile*:</span><span class="sxs-lookup"><span data-stu-id="7fdbe-115">You *can't*:</span></span>

* <span data-ttu-id="7fdbe-116">Osservare i valori delle variabili locali che non sono `int`, `string`o `bool`.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-116">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="7fdbe-117">Osservare i valori di qualsiasi proprietà o campo di una classe.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-117">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="7fdbe-118">Passare il mouse sulle variabili per visualizzarne i valori.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-118">Hover over variables to see their values.</span></span>
* <span data-ttu-id="7fdbe-119">Valutare le espressioni nella console.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-119">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="7fdbe-120">Eseguire un passaggio tra le chiamate asincrone.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-120">Step across async calls.</span></span>
* <span data-ttu-id="7fdbe-121">Eseguire la maggior parte degli altri scenari di debug comuni.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-121">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="7fdbe-122">Lo sviluppo di altri scenari di debug è un'attività in corso del team di progettazione.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-122">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7fdbe-123">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="7fdbe-123">Prerequisites</span></span>

<span data-ttu-id="7fdbe-124">Il debug richiede uno dei seguenti browser:</span><span class="sxs-lookup"><span data-stu-id="7fdbe-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="7fdbe-125">Google Chrome (versione 70 o successiva)</span><span class="sxs-lookup"><span data-stu-id="7fdbe-125">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="7fdbe-126">Anteprima di Microsoft Edge ([canale di sviluppo Edge](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="7fdbe-126">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="7fdbe-127">Procedura</span><span class="sxs-lookup"><span data-stu-id="7fdbe-127">Procedure</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fdbe-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fdbe-128">Visual Studio</span></span>](#tab/visual-studio)

> [!WARNING]
> <span data-ttu-id="7fdbe-129">Il supporto del debug in Visual Studio si trova in una fase iniziale dello sviluppo.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-129">Debugging support in Visual Studio is at an early stage of development.</span></span> <span data-ttu-id="7fdbe-130">Il debug di **F5** attualmente non è supportato.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-130">**F5** debugging isn't currently supported.</span></span>

1. <span data-ttu-id="7fdbe-131">Eseguire un'app webassembly [!OP.NO-LOC(Blazor)] in `Debug` configurazione senza debug (**Ctrl**+**F5** anziché **F5**).</span><span class="sxs-lookup"><span data-stu-id="7fdbe-131">Run a [!OP.NO-LOC(Blazor)] WebAssembly app in `Debug` configuration without debugging (**Ctrl**+**F5** instead of **F5**).</span></span>
1. <span data-ttu-id="7fdbe-132">Aprire le proprietà di debug dell'app (ultima voce nel menu **debug** ) e copiare l'URL dell' **app**http.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-132">Open the Debug properties of the app (last entry in the **Debug** menu) and copy the HTTP **App URL**.</span></span> <span data-ttu-id="7fdbe-133">Individuare l'indirizzo HTTP (non l'indirizzo HTTPS) dell'app usando un browser basato su cromo (Microsoft Edge beta o Chrome).</span><span class="sxs-lookup"><span data-stu-id="7fdbe-133">Browse to the HTTP address (not the HTTPS address) of the app using a Chromium-based browser (Edge Beta or Chrome).</span></span>
1. <span data-ttu-id="7fdbe-134">Posizionare lo stato attivo sulla tastiera nell'app nella finestra del browser, non nel pannello strumenti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-134">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span> <span data-ttu-id="7fdbe-135">È consigliabile lasciare il pannello strumenti di sviluppo chiuso per questa procedura.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-135">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="7fdbe-136">Dopo l'avvio del debug, è possibile riaprire il pannello strumenti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-136">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="7fdbe-137">Selezionare il seguente tasto di scelta rapida specifico per [!OP.NO-LOC(Blazor)]:</span><span class="sxs-lookup"><span data-stu-id="7fdbe-137">Select the following [!OP.NO-LOC(Blazor)]-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="7fdbe-138">`Shift+Alt+D` in Windows</span><span class="sxs-lookup"><span data-stu-id="7fdbe-138">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="7fdbe-139">`Shift+Cmd+D` in macOS</span><span class="sxs-lookup"><span data-stu-id="7fdbe-139">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="7fdbe-140">Se viene visualizzata la **scheda non è possibile trovare il browser di cui è possibile eseguire il debug**, vedere [abilitare il debug remoto](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="7fdbe-140">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="7fdbe-141">Dopo l'abilitazione del debug remoto:</span><span class="sxs-lookup"><span data-stu-id="7fdbe-141">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="7fdbe-142">1 \.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-142">1\.</span></span> <span data-ttu-id="7fdbe-143">Verrà aperta una nuova finestra del browser</span><span class="sxs-lookup"><span data-stu-id="7fdbe-143">A new browser window opens.</span></span> <span data-ttu-id="7fdbe-144">Chiudere la finestra precedente.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-144">Close the prior window.</span></span>

   <span data-ttu-id="7fdbe-145">2 \.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-145">2\.</span></span> <span data-ttu-id="7fdbe-146">Posizionare lo stato attivo sulla tastiera nell'app nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-146">Place the keyboard focus on the app in the browser window.</span></span>

   <span data-ttu-id="7fdbe-147">3 \.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-147">3\.</span></span> <span data-ttu-id="7fdbe-148">Selezionare il tasto di scelta rapida specifico per [!OP.NO-LOC(Blazor)]nella nuova finestra del browser: `Shift+Alt+D` in Windows o `Shift+Cmd+D` in macOS.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-148">Select the [!OP.NO-LOC(Blazor)]-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

   <span data-ttu-id="7fdbe-149">4 \.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-149">4\.</span></span> <span data-ttu-id="7fdbe-150">Verrà visualizzata la scheda **devtools** nel browser.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-150">The **DevTools** tab opens in the browser.</span></span> <span data-ttu-id="7fdbe-151">**Selezionare nuovamente la scheda dell'app nella finestra del browser.**</span><span class="sxs-lookup"><span data-stu-id="7fdbe-151">**Reselect the app's tab in the browser window.**</span></span>

   <span data-ttu-id="7fdbe-152">Per aggiungere l'app a Visual Studio, vedere la sezione [Connetti a processo in Visual Studio](#attach-to-process-in-visual-studio) .</span><span class="sxs-lookup"><span data-stu-id="7fdbe-152">To attach the app to Visual Studio, see the [Attach to process in Visual Studio](#attach-to-process-in-visual-studio) section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7fdbe-153">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="7fdbe-153">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="7fdbe-154">Eseguire un'app webassembly [!OP.NO-LOC(Blazor)] nella configurazione `Debug` passando l'opzione `--configuration Debug` al comando [DotNet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-154">Run a [!OP.NO-LOC(Blazor)] WebAssembly app in `Debug` configuration by passing the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="7fdbe-155">Passare all'app nell'URL HTTP visualizzato nella finestra della shell.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-155">Navigate to the app at the HTTP URL shown in the shell's window.</span></span>
1. <span data-ttu-id="7fdbe-156">Posizionare lo stato attivo della tastiera sull'app, non sul pannello strumenti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-156">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="7fdbe-157">È consigliabile lasciare il pannello strumenti di sviluppo chiuso per questa procedura.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-157">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="7fdbe-158">Dopo l'avvio del debug, è possibile riaprire il pannello strumenti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-158">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="7fdbe-159">Selezionare il seguente tasto di scelta rapida specifico per [!OP.NO-LOC(Blazor)]:</span><span class="sxs-lookup"><span data-stu-id="7fdbe-159">Select the following [!OP.NO-LOC(Blazor)]-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="7fdbe-160">`Shift+Alt+D` in Windows</span><span class="sxs-lookup"><span data-stu-id="7fdbe-160">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="7fdbe-161">`Shift+Cmd+D` in macOS</span><span class="sxs-lookup"><span data-stu-id="7fdbe-161">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="7fdbe-162">Se viene visualizzata la **scheda non è possibile trovare il browser di cui è possibile eseguire il debug**, vedere [abilitare il debug remoto](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="7fdbe-162">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="7fdbe-163">Dopo l'abilitazione del debug remoto:</span><span class="sxs-lookup"><span data-stu-id="7fdbe-163">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="7fdbe-164">1 \.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-164">1\.</span></span> <span data-ttu-id="7fdbe-165">Verrà aperta una nuova finestra del browser</span><span class="sxs-lookup"><span data-stu-id="7fdbe-165">A new browser window opens.</span></span> <span data-ttu-id="7fdbe-166">Chiudere la finestra precedente.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-166">Close the prior window.</span></span>

   <span data-ttu-id="7fdbe-167">2 \.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-167">2\.</span></span> <span data-ttu-id="7fdbe-168">Posizionare lo stato attivo sulla tastiera nell'app nella finestra del browser, non nel pannello strumenti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-168">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span>

   <span data-ttu-id="7fdbe-169">3 \.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-169">3\.</span></span> <span data-ttu-id="7fdbe-170">Selezionare il tasto di scelta rapida specifico per [!OP.NO-LOC(Blazor)]nella nuova finestra del browser: `Shift+Alt+D` in Windows o `Shift+Cmd+D` in macOS.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-170">Select the [!OP.NO-LOC(Blazor)]-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

---

## <a name="enable-remote-debugging"></a><span data-ttu-id="7fdbe-171">Abilitare il debug remoto</span><span class="sxs-lookup"><span data-stu-id="7fdbe-171">Enable remote debugging</span></span>

<span data-ttu-id="7fdbe-172">Se il debug remoto è disabilitato, la pagina di errore **di una scheda del browser di cui è possibile eseguire il debug** viene generata da Chrome.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-172">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="7fdbe-173">La pagina di errore contiene le istruzioni per eseguire Chrome con la porta di debug aperta, in modo che il proxy di debug Blazor possa connettersi all'app.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-173">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="7fdbe-174">*Chiudere tutte le istanze di Chrome* e riavviare Chrome come indicato.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-174">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="7fdbe-175">Eseguire il debug dell'app</span><span class="sxs-lookup"><span data-stu-id="7fdbe-175">Debug the app</span></span>

<span data-ttu-id="7fdbe-176">Quando Chrome è in esecuzione con il debug remoto abilitato, il tasto di scelta rapida per il debug apre una nuova scheda del debugger. Dopo qualche istante la scheda **Sources (origini** ) Mostra un elenco degli assembly .NET nell'app.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-176">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="7fdbe-177">Espandere ogni assembly e trovare i file di origine *. cs*/ *. Razor* disponibili per il debug.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-177">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="7fdbe-178">Impostare i punti di interruzione, tornare alla scheda dell'app e i punti di interruzione vengono raggiunti quando viene eseguito il codice.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-178">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="7fdbe-179">Quando viene raggiunto un punto di interruzione, un singolo passaggio (`F10`) tramite il codice o riprende (`F8`) l'esecuzione del codice normalmente.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-179">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="7fdbe-180"> fornisce un proxy di debug che implementa il [protocollo devtools di Chrome](https://chromedevtools.github.io/devtools-protocol/) e potenzia il protocollo con. Informazioni specifiche del NET.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-180"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="7fdbe-181">Quando si preme il tasto di scelta rapida per il debug, Blazor punta il DevTools di Chrome sul proxy.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-181">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="7fdbe-182">Il proxy si connette alla finestra del browser che si sta tentando di eseguire il debug, quindi è necessario abilitare il debug remoto.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-182">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="attach-to-process-in-visual-studio"></a><span data-ttu-id="7fdbe-183">Connetti a processo in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fdbe-183">Attach to process in Visual Studio</span></span>

<span data-ttu-id="7fdbe-184">La connessione al processo dell'app in Visual Studio è uno scenario di debug *temporaneo* per Blazor webassembly mentre il debug di **F5** è in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-184">Attaching to the app's process in Visual Studio is a *temporary* debugging scenario for Blazor WebAssembly while **F5** debugging is in development.</span></span>

<span data-ttu-id="7fdbe-185">Per alleghi il processo dell'app in esecuzione a Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7fdbe-185">To attach the running app's process to Visual Studio:</span></span>

1. <span data-ttu-id="7fdbe-186">In Visual Studio selezionare **Debug** > **Connetti a processo**.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-186">In Visual Studio, select **Debug** > **Attach to Process**.</span></span>
1. <span data-ttu-id="7fdbe-187">Per il **tipo di connessione**, selezionare il **protocollo DevTools per Chrome (nessuna autenticazione)** .</span><span class="sxs-lookup"><span data-stu-id="7fdbe-187">For the **Connection type**, select **Chrome devtools protocol websocket (no authentication)**.</span></span>
1. <span data-ttu-id="7fdbe-188">Per la **destinazione della connessione**, incollare l'indirizzo http (non l'indirizzo https) dell'app.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-188">For the **Connection target**, paste in the HTTP address (not the HTTPS address) of the app.</span></span>
1. <span data-ttu-id="7fdbe-189">Selezionare **Aggiorna** per aggiornare le voci in **processi disponibili**.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-189">Select **Refresh** to refresh the entries under **Available processes**.</span></span>
1. <span data-ttu-id="7fdbe-190">Selezionare il processo del browser di cui eseguire il debug e selezionare **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-190">Select the browser process to debug and select **Attach**.</span></span>
1. <span data-ttu-id="7fdbe-191">Nella finestra di dialogo **Seleziona tipo di codice** selezionare il tipo di codice per il browser specifico a cui si sta eseguendo la connessione (Microsoft Edge o Chrome) e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-191">In the **Select Code Type** dialog, select the code type for the specific browser you're attaching to (Edge or Chrome) and then select **OK**.</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="7fdbe-192">Mappe di origine del browser</span><span class="sxs-lookup"><span data-stu-id="7fdbe-192">Browser source maps</span></span>

<span data-ttu-id="7fdbe-193">Le mappe di origine del browser consentono al browser di eseguire il mapping dei file compilati ai file di origine originali e vengono comunemente usati per il debug sul lato client.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-193">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="7fdbe-194">Tuttavia, non Blazor attualmente viene C# effettuato il mapping direttamente a JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-194">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="7fdbe-195">Al contrario, Blazor l'interpretazione del nel browser, quindi le mappe di origine non sono rilevanti.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-195">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="7fdbe-196">Risolvere i problemi</span><span class="sxs-lookup"><span data-stu-id="7fdbe-196">Troubleshoot</span></span>

<span data-ttu-id="7fdbe-197">Se si verificano errori, è possibile che venga utile il suggerimento seguente:</span><span class="sxs-lookup"><span data-stu-id="7fdbe-197">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="7fdbe-198">Nella scheda **debugger** aprire gli strumenti di sviluppo nel browser.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-198">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="7fdbe-199">Nella console di eseguire `localStorage.clear()` per rimuovere tutti i punti di interruzione.</span><span class="sxs-lookup"><span data-stu-id="7fdbe-199">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
