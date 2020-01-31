---
title: Introduzione a ASP.NET Core Blazor
author: guardrex
description: Inizia a usare Blazor compilando un'app Blazor con gli strumenti che preferisci.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: bd33d874b3d6122f2ab820e9b147b0e62ba03a58
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869580"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="d6d88-103">Introduzione a ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="d6d88-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="d6d88-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d6d88-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="d6d88-105">Introduzione a Blazor:</span><span class="sxs-lookup"><span data-stu-id="d6d88-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="d6d88-106">Installare [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d6d88-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="d6d88-107">Facoltativamente, installare il modello [Webassembly Blazer](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="d6d88-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="d6d88-108">Installare [.NET Core 3,1 o versione successiva (anteprima) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d6d88-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="d6d88-109">Eseguire il comando seguente in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="d6d88-109">Run the following command in a command shell.</span></span> <span data-ttu-id="d6d88-110">Il pacchetto [Microsoft. AspNetCore. Blazer. templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) include una versione di anteprima mentre Webassembly blazer è in anteprima.</span><span class="sxs-lookup"><span data-stu-id="d6d88-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
   ```

1. <span data-ttu-id="d6d88-111">Seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="d6d88-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6d88-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6d88-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="d6d88-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-113">1\.</span></span> <span data-ttu-id="d6d88-114">Installare [Visual Studio 2019 versione 16,4 o successiva](https://visualstudio.microsoft.com/vs/preview/) con il carico di lavoro di **sviluppo ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="d6d88-114">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d6d88-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-115">2\.</span></span> <span data-ttu-id="d6d88-116">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="d6d88-116">Create a new project.</span></span>

   <span data-ttu-id="d6d88-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-117">3\.</span></span> <span data-ttu-id="d6d88-118">Selezionare **app Blazor**.</span><span class="sxs-lookup"><span data-stu-id="d6d88-118">Select **Blazor App**.</span></span> <span data-ttu-id="d6d88-119">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="d6d88-119">Select **Next**.</span></span>

   <span data-ttu-id="d6d88-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-120">4\.</span></span> <span data-ttu-id="d6d88-121">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="d6d88-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d6d88-122">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="d6d88-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d6d88-123">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="d6d88-123">Select **Create**.</span></span>

   <span data-ttu-id="d6d88-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-124">5\.</span></span> <span data-ttu-id="d6d88-125">Per un'esperienza di webassembly Blazor, scegliere il modello **app Webassembly Blazor** .</span><span class="sxs-lookup"><span data-stu-id="d6d88-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="d6d88-126">Per un'esperienza del server Blazor, scegliere il modello di **app del server Blazor** .</span><span class="sxs-lookup"><span data-stu-id="d6d88-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="d6d88-127">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="d6d88-127">Select **Create**.</span></span> <span data-ttu-id="d6d88-128">Per informazioni sui due modelli di hosting Blazor, *Server Blazor* e *webassembly Blazor*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d6d88-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d6d88-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-129">6\.</span></span> <span data-ttu-id="d6d88-130">Premere **CTRL**+**F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="d6d88-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d6d88-131">Se è stata installata l'estensione di Visual Studio Blazor per una versione di anteprima precedente di ASP.NET Core Blazor (anteprima 6 o precedente), è possibile disinstallare l'estensione.</span><span class="sxs-lookup"><span data-stu-id="d6d88-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="d6d88-132">L'installazione dei modelli di Blazor in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6d88-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d6d88-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6d88-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="d6d88-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-134">1\.</span></span> <span data-ttu-id="d6d88-135">Installare [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="d6d88-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="d6d88-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-136">2\.</span></span> <span data-ttu-id="d6d88-137">Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="d6d88-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="d6d88-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-138">3\.</span></span> <span data-ttu-id="d6d88-139">Per un'esperienza di webassembly Blazor, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="d6d88-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="d6d88-140">Per un'esperienza del server Blazor, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="d6d88-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="d6d88-141">Per informazioni sui due modelli di hosting Blazor, *Server Blazor* e *webassembly Blazor*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d6d88-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d6d88-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-142">4\.</span></span> <span data-ttu-id="d6d88-143">Aprire la cartella *WebApplication1* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d6d88-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="d6d88-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-144">5\.</span></span> <span data-ttu-id="d6d88-145">Per un progetto server Blazor, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="d6d88-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="d6d88-146">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="d6d88-146">Select **Yes**.</span></span>

   <span data-ttu-id="d6d88-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-147">6\.</span></span> <span data-ttu-id="d6d88-148">Se si usa un'app del server Blazor, eseguire l'app usando il debugger Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d6d88-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="d6d88-149">Se si usa un'app webassembly Blazor, eseguire `dotnet run` dalla cartella del progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="d6d88-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="d6d88-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-150">7\.</span></span> <span data-ttu-id="d6d88-151">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d6d88-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d6d88-152">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="d6d88-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="d6d88-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-153">1\.</span></span> <span data-ttu-id="d6d88-154">Installare [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="d6d88-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="d6d88-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-155">2\.</span></span> <span data-ttu-id="d6d88-156">Selezionare **File** > **nuova soluzione** o creare un **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="d6d88-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="d6d88-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-157">3\.</span></span> <span data-ttu-id="d6d88-158">Nella barra laterale selezionare **.NET Core** > **app**.</span><span class="sxs-lookup"><span data-stu-id="d6d88-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="d6d88-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-159">4\.</span></span> <span data-ttu-id="d6d88-160">Selezionare il modello **applicazione server Blazer** .</span><span class="sxs-lookup"><span data-stu-id="d6d88-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="d6d88-161">Attualmente è disponibile in Visual Studio per Mac solo il modello di server blazer.</span><span class="sxs-lookup"><span data-stu-id="d6d88-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="d6d88-162">Per un'esperienza di webassembly blazer, seguire le istruzioni riportate nella scheda **interfaccia della riga di comando di .NET Core** . Dopo aver selezionato il modello di server blazer, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="d6d88-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="d6d88-163">Per informazioni sui due modelli di hosting Blazor, *Server Blazor* e *webassembly Blazor*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d6d88-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="d6d88-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-164">5\.</span></span> <span data-ttu-id="d6d88-165">Impostare il **Framework di destinazione** su **.NET Core 3,1** e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="d6d88-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="d6d88-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-166">6\.</span></span> <span data-ttu-id="d6d88-167">Nel campo **nome progetto** assegnare un nome all'app `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="d6d88-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="d6d88-168">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="d6d88-168">Select **Create**.</span></span>

   <span data-ttu-id="d6d88-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="d6d88-169">7\.</span></span> <span data-ttu-id="d6d88-170">Selezionare **esegui** > **Esegui senza eseguire debug** per eseguire l'app *senza il debugger*.</span><span class="sxs-lookup"><span data-stu-id="d6d88-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="d6d88-171">Eseguire l'app con **Avvia debug** per eseguire l'app *con il debugger*.</span><span class="sxs-lookup"><span data-stu-id="d6d88-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="d6d88-172">Se viene visualizzato un messaggio per considerare attendibile il certificato di sviluppo, considerare attendibile il certificato e continuare.</span><span class="sxs-lookup"><span data-stu-id="d6d88-172">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d6d88-173">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="d6d88-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="d6d88-174">Per un'esperienza di webassembly Blazor, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="d6d88-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d6d88-175">Per un'esperienza del server Blazor, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="d6d88-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d6d88-176">Per informazioni sui due modelli di hosting Blazor, *Server Blazor* e *webassembly Blazor*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d6d88-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d6d88-177">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d6d88-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="d6d88-178">Nelle schede della barra laterale sono disponibili più pagine:</span><span class="sxs-lookup"><span data-stu-id="d6d88-178">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="d6d88-179">Home page di</span><span class="sxs-lookup"><span data-stu-id="d6d88-179">Home</span></span>
* <span data-ttu-id="d6d88-180">Counter</span><span class="sxs-lookup"><span data-stu-id="d6d88-180">Counter</span></span>
* <span data-ttu-id="d6d88-181">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="d6d88-181">Fetch data</span></span>

<span data-ttu-id="d6d88-182">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="d6d88-182">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d6d88-183">Per incrementare un contatore in una pagina Web è in genere necessario scrivere JavaScript, ma con Blazor C#è possibile utilizzare.</span><span class="sxs-lookup"><span data-stu-id="d6d88-183">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="d6d88-184">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="d6d88-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="d6d88-185">Una richiesta di `/counter` nel browser, come specificato dalla direttiva `@page` nella parte superiore, fa sì che il componente `Counter` esegua il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="d6d88-185">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="d6d88-186">I componenti eseguono il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="d6d88-186">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="d6d88-187">Ogni volta che viene selezionato il pulsante **Click me** :</span><span class="sxs-lookup"><span data-stu-id="d6d88-187">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="d6d88-188">Viene generato l'evento `onclick`.</span><span class="sxs-lookup"><span data-stu-id="d6d88-188">The `onclick` event is fired.</span></span>
* <span data-ttu-id="d6d88-189">Viene chiamato il metodo `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="d6d88-189">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="d6d88-190">Il `currentCount` viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="d6d88-190">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="d6d88-191">Il rendering del componente viene eseguito nuovamente.</span><span class="sxs-lookup"><span data-stu-id="d6d88-191">The component is rendered again.</span></span>

<span data-ttu-id="d6d88-192">Il runtime confronta il nuovo contenuto con il contenuto precedente e applica solo il contenuto modificato al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="d6d88-192">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="d6d88-193">Aggiungere un componente a un altro componente usando la sintassi HTML.</span><span class="sxs-lookup"><span data-stu-id="d6d88-193">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="d6d88-194">Ad esempio, aggiungere il componente `Counter` alla Home page dell'app aggiungendo un elemento `<Counter />` al componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="d6d88-194">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="d6d88-195">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="d6d88-195">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="d6d88-196">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="d6d88-196">Run the app.</span></span> <span data-ttu-id="d6d88-197">La Home page presenta il proprio contatore fornito dal componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="d6d88-197">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="d6d88-198">I parametri del componente vengono specificati utilizzando attributi o [contenuto figlio](xref:blazor/components#child-content), che consentono di impostare le proprietà per il componente figlio.</span><span class="sxs-lookup"><span data-stu-id="d6d88-198">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="d6d88-199">Per aggiungere un parametro al componente `Counter`, aggiornare il blocco `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="d6d88-199">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="d6d88-200">Aggiungere una proprietà pubblica per `IncrementAmount` con un attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d6d88-200">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="d6d88-201">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d6d88-201">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="d6d88-202">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="d6d88-202">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="d6d88-203">Specificare il `IncrementAmount` nell'elemento `<Counter>` del componente di `Index` utilizzando un attributo.</span><span class="sxs-lookup"><span data-stu-id="d6d88-203">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="d6d88-204">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="d6d88-204">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="d6d88-205">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="d6d88-205">Run the app.</span></span> <span data-ttu-id="d6d88-206">Il componente `Index` dispone di un contatore che aumenta di dieci ogni volta che viene selezionato il pulsante **Click me** .</span><span class="sxs-lookup"><span data-stu-id="d6d88-206">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="d6d88-207">Il componente `Counter` (*Counter. Razor*) in `/counter` continua a incrementare di uno.</span><span class="sxs-lookup"><span data-stu-id="d6d88-207">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6d88-208">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="d6d88-208">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="d6d88-209">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d6d88-209">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
