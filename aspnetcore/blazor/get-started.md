---
title: Introduzione a ASP.NET Core Blazor
author: guardrex
description: Inizia a usare Blazor compilando un'app Blazor con gli strumenti che preferisci.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 89c7529d2b8ec97db731f7c7268e19937c398115
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083246"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="4d7c7-103">Introduzione a ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="4d7c7-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="4d7c7-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4d7c7-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="4d7c7-105">Introduzione a Blazor:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="4d7c7-106">Installare [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="4d7c7-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="4d7c7-107">Facoltativamente, installare il modello [Webassembly Blazer](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="4d7c7-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="4d7c7-108">Installare l' [SDK di .NET Core 3.1.102 o versione successiva (anteprima)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="4d7c7-108">Install the [.NET Core 3.1.102 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="4d7c7-109">Eseguire il comando seguente in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-109">Run the following command in a command shell.</span></span> <span data-ttu-id="4d7c7-110">Il pacchetto [Microsoft. AspNetCore. Components. webassembly. templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) include una versione di anteprima mentre Webassembly blazer è in anteprima.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-110">The [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview2.20160.5
   ```

   > [!NOTE]
   > <span data-ttu-id="4d7c7-111">Per usare il modello di assembly webassembly 3,2 Preview 2 blazer è **necessario** .NET Core SDK versione 3.1.102 o successiva.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-111">.NET Core SDK version 3.1.102 or later is **required** to use the 3.2 Preview 2 Blazor WebAssembly template.</span></span> <span data-ttu-id="4d7c7-112">Verificare la versione di .NET Core SDK installata eseguendo `dotnet --version` in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-112">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="4d7c7-113">Seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-113">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studio"></a>[<span data-ttu-id="4d7c7-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4d7c7-114">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="4d7c7-115">1 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-115">1\.</span></span> <span data-ttu-id="4d7c7-116">Installare [Visual Studio 2019 versione 16,4 o successiva](https://visualstudio.microsoft.com/vs/preview/) con il carico di lavoro di **sviluppo ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="4d7c7-116">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="4d7c7-117">2 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-117">2\.</span></span> <span data-ttu-id="4d7c7-118">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-118">Create a new project.</span></span>

   <span data-ttu-id="4d7c7-119">3 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-119">3\.</span></span> <span data-ttu-id="4d7c7-120">Selezionare **app Blazer**.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-120">Select **Blazor App**.</span></span> <span data-ttu-id="4d7c7-121">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-121">Select **Next**.</span></span>

   <span data-ttu-id="4d7c7-122">4 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-122">4\.</span></span> <span data-ttu-id="4d7c7-123">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-123">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="4d7c7-124">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="4d7c7-125">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-125">Select **Create**.</span></span>

   <span data-ttu-id="4d7c7-126">5 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-126">5\.</span></span> <span data-ttu-id="4d7c7-127">Per un'esperienza di webassembly blazer, scegliere il modello **app Webassembly Blazer** .</span><span class="sxs-lookup"><span data-stu-id="4d7c7-127">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="4d7c7-128">Per un'esperienza del server blazer, scegliere il modello di **app del server Blazer** .</span><span class="sxs-lookup"><span data-stu-id="4d7c7-128">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="4d7c7-129">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-129">Select **Create**.</span></span> <span data-ttu-id="4d7c7-130">Per informazioni sui due modelli di hosting blazer, *Server Blazer* e *webassembly Blazer*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-130">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span> <span data-ttu-id="4d7c7-131">Se il modello webassembly Blazer non è presente, tornare al passaggio precedente e reinstallare il modello.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-131">If the Blazor WebAssembly template isn't present, return to the previous step and reinstall the template.</span></span>

   <span data-ttu-id="4d7c7-132">6 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-132">6\.</span></span> <span data-ttu-id="4d7c7-133">Premere **CTRL**+**F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-133">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4d7c7-134">Se è stata installata l'estensione di Visual Studio Blazor per una versione di anteprima precedente di ASP.NET Core Blazor (anteprima 6 o precedente), è possibile disinstallare l'estensione.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-134">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="4d7c7-135">L'installazione dei modelli di Blazor in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-135">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-code"></a>[<span data-ttu-id="4d7c7-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4d7c7-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="4d7c7-137">1 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-137">1\.</span></span> <span data-ttu-id="4d7c7-138">Installare [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="4d7c7-138">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="4d7c7-139">2 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-139">2\.</span></span> <span data-ttu-id="4d7c7-140">Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span><span class="sxs-lookup"><span data-stu-id="4d7c7-140">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span></span>

   <span data-ttu-id="4d7c7-141">3 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-141">3\.</span></span> <span data-ttu-id="4d7c7-142">Per un'esperienza di webassembly Blazor, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-142">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="4d7c7-143">Per un'esperienza del server Blazor, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-143">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="4d7c7-144">Per informazioni sui due modelli di hosting blazer, *Server Blazer* e *webassembly Blazer*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-144">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="4d7c7-145">4 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-145">4\.</span></span> <span data-ttu-id="4d7c7-146">Aprire la cartella *WebApplication1* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-146">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="4d7c7-147">5 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-147">5\.</span></span> <span data-ttu-id="4d7c7-148">Per un progetto server Blazor, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-148">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="4d7c7-149">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-149">Select **Yes**.</span></span>

   <span data-ttu-id="4d7c7-150">6 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-150">6\.</span></span> <span data-ttu-id="4d7c7-151">Se si usa un'app del server Blazor, eseguire l'app usando il debugger Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-151">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="4d7c7-152">Se si usa un'app webassembly blazer, eseguire `dotnet run` dalla cartella del progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-152">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="4d7c7-153">7 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-153">7\.</span></span> <span data-ttu-id="4d7c7-154">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mac"></a>[<span data-ttu-id="4d7c7-155">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="4d7c7-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="4d7c7-156">1 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-156">1\.</span></span> <span data-ttu-id="4d7c7-157">Installare [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="4d7c7-157">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="4d7c7-158">2 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-158">2\.</span></span> <span data-ttu-id="4d7c7-159">Selezionare **File** > **nuova soluzione** o creare un **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-159">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="4d7c7-160">3 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-160">3\.</span></span> <span data-ttu-id="4d7c7-161">Nella barra laterale selezionare **.NET Core** > **app**.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-161">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="4d7c7-162">4 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-162">4\.</span></span> <span data-ttu-id="4d7c7-163">Selezionare il modello **applicazione server Blazer** .</span><span class="sxs-lookup"><span data-stu-id="4d7c7-163">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="4d7c7-164">Attualmente è disponibile in Visual Studio per Mac solo il modello di server blazer.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-164">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="4d7c7-165">Per un'esperienza di webassembly blazer, seguire le istruzioni riportate nella scheda **interfaccia della riga di comando di .NET Core** . Dopo aver selezionato il modello di server blazer, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-165">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="4d7c7-166">Per informazioni sui due modelli di hosting blazer, *Server Blazer* e *webassembly Blazer*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-166">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="4d7c7-167">5 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-167">5\.</span></span> <span data-ttu-id="4d7c7-168">Impostare il **Framework di destinazione** su **.NET Core 3,1** e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-168">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="4d7c7-169">6 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-169">6\.</span></span> <span data-ttu-id="4d7c7-170">Nel campo **nome progetto** assegnare un nome all'app `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-170">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="4d7c7-171">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-171">Select **Create**.</span></span>

   <span data-ttu-id="4d7c7-172">7 \.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-172">7\.</span></span> <span data-ttu-id="4d7c7-173">Selezionare **esegui** > **Esegui senza eseguire debug** per eseguire l'app *senza il debugger*.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-173">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="4d7c7-174">Eseguire l'app con **Avvia debug** per eseguire l'app *con il debugger*.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-174">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="4d7c7-175">Se viene visualizzato un messaggio per considerare attendibile il certificato di sviluppo, considerare attendibile il certificato e continuare.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-175">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-cli"></a>[<span data-ttu-id="4d7c7-176">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="4d7c7-176">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="4d7c7-177">Per un'esperienza di webassembly Blazor, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-177">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="4d7c7-178">Per un'esperienza del server Blazor, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-178">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="4d7c7-179">Per informazioni sui due modelli di hosting blazer, *Server Blazer* e *webassembly Blazer*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-179">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="4d7c7-180">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-180">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="4d7c7-181">Nelle schede della barra laterale sono disponibili più pagine:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-181">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="4d7c7-182">Home</span><span class="sxs-lookup"><span data-stu-id="4d7c7-182">Home</span></span>
* <span data-ttu-id="4d7c7-183">Counter</span><span class="sxs-lookup"><span data-stu-id="4d7c7-183">Counter</span></span>
* <span data-ttu-id="4d7c7-184">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="4d7c7-184">Fetch data</span></span>

<span data-ttu-id="4d7c7-185">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-185">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="4d7c7-186">Per incrementare un contatore in una pagina Web è in genere necessario scrivere JavaScript, ma con Blazor C#è possibile utilizzare.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-186">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="4d7c7-187">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-187">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="4d7c7-188">Una richiesta di `/counter` nel browser, come specificato dalla direttiva `@page` nella parte superiore, fa sì che il componente `Counter` esegua il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-188">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="4d7c7-189">I componenti eseguono il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-189">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="4d7c7-190">Ogni volta che viene selezionato il pulsante **Click me** :</span><span class="sxs-lookup"><span data-stu-id="4d7c7-190">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="4d7c7-191">Viene generato l'evento `onclick`.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-191">The `onclick` event is fired.</span></span>
* <span data-ttu-id="4d7c7-192">Viene chiamato il metodo `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-192">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="4d7c7-193">Il `currentCount` viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-193">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="4d7c7-194">Il rendering del componente viene eseguito nuovamente.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-194">The component is rendered again.</span></span>

<span data-ttu-id="4d7c7-195">Il runtime confronta il nuovo contenuto con il contenuto precedente e applica solo il contenuto modificato al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="4d7c7-195">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="4d7c7-196">Aggiungere un componente a un altro componente usando la sintassi HTML.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-196">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="4d7c7-197">Ad esempio, aggiungere il componente `Counter` alla Home page dell'app aggiungendo un elemento `<Counter />` al componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-197">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="4d7c7-198">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-198">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="4d7c7-199">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-199">Run the app.</span></span> <span data-ttu-id="4d7c7-200">La Home page presenta il proprio contatore fornito dal componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-200">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="4d7c7-201">I parametri del componente vengono specificati utilizzando attributi o [contenuto figlio](xref:blazor/components#child-content), che consentono di impostare le proprietà per il componente figlio.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-201">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="4d7c7-202">Per aggiungere un parametro al componente `Counter`, aggiornare il blocco `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-202">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="4d7c7-203">Aggiungere una proprietà pubblica per `IncrementAmount` con un attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-203">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="4d7c7-204">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-204">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="4d7c7-205">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-205">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="4d7c7-206">Specificare il `IncrementAmount` nell'elemento `<Counter>` del componente di `Index` utilizzando un attributo.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-206">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="4d7c7-207">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="4d7c7-207">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="4d7c7-208">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-208">Run the app.</span></span> <span data-ttu-id="4d7c7-209">Il componente `Index` dispone di un contatore che aumenta di dieci ogni volta che viene selezionato il pulsante **Click me** .</span><span class="sxs-lookup"><span data-stu-id="4d7c7-209">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="4d7c7-210">Il componente `Counter` (*Counter. Razor*) in `/counter` continua a incrementare di uno.</span><span class="sxs-lookup"><span data-stu-id="4d7c7-210">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d7c7-211">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="4d7c7-211">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="4d7c7-212">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4d7c7-212">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
