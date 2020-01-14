---
title: Introduzione a ASP.NET Core Blazor
author: guardrex
description: Inizia a usare Blazor compilando un'app Blazor con gli strumenti che preferisci.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/09/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 554f4daff92a0839ee7679287a4618e9b51e0fe5
ms.sourcegitcommit: 925cdbd94613243f33bc7613a62ea34006219931
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2020
ms.locfileid: "75921300"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="661d8-103">Introduzione a ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="661d8-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="661d8-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="661d8-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="661d8-105">Introduzione a Blazor:</span><span class="sxs-lookup"><span data-stu-id="661d8-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="661d8-106">Installare [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="661d8-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="661d8-107">Facoltativamente, installare il modello di [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="661d8-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="661d8-108">Installare [.NET Core 3,1 o versione successiva (anteprima) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="661d8-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="661d8-109">Eseguire il comando seguente in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="661d8-109">Run the following command in a command shell.</span></span> <span data-ttu-id="661d8-110">[Microsoft.AspNetCore.Blazor.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Il pacchetto di modelli include una versione di anteprima mentre Blazor webassembly è in anteprima.</span><span class="sxs-lookup"><span data-stu-id="661d8-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="661d8-111">Seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="661d8-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="661d8-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="661d8-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="661d8-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-113">1\.</span></span> <span data-ttu-id="661d8-114">Installare [Visual Studio 16,4 o versione successiva](https://visualstudio.microsoft.com/vs/preview/) con il carico di lavoro **sviluppo di ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="661d8-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="661d8-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-115">2\.</span></span> <span data-ttu-id="661d8-116">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="661d8-116">Create a new project.</span></span>

   <span data-ttu-id="661d8-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-117">3\.</span></span> <span data-ttu-id="661d8-118">Selezionare **Blazor app**.</span><span class="sxs-lookup"><span data-stu-id="661d8-118">Select **Blazor App**.</span></span> <span data-ttu-id="661d8-119">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="661d8-119">Select **Next**.</span></span>

   <span data-ttu-id="661d8-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-120">4\.</span></span> <span data-ttu-id="661d8-121">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="661d8-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="661d8-122">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="661d8-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="661d8-123">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="661d8-123">Select **Create**.</span></span>

   <span data-ttu-id="661d8-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-124">5\.</span></span> <span data-ttu-id="661d8-125">Per un'esperienza Blazor webassembly, scegliere il modello **app webassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="661d8-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="661d8-126">Per un'esperienza di Blazor server, scegliere il modello **app serverBlazor** .</span><span class="sxs-lookup"><span data-stu-id="661d8-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="661d8-127">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="661d8-127">Select **Create**.</span></span> <span data-ttu-id="661d8-128">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="661d8-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="661d8-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-129">6\.</span></span> <span data-ttu-id="661d8-130">Premere **CTRL**+**F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="661d8-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="661d8-131">Se è stato installato il Blazor estensione di Visual Studio per una versione di anteprima precedente di ASP.NET Core Blazor (anteprima 6 o precedente), è possibile disinstallare l'estensione.</span><span class="sxs-lookup"><span data-stu-id="661d8-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="661d8-132">L'installazione dei modelli di Blazor in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="661d8-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="661d8-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="661d8-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="661d8-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-134">1\.</span></span> <span data-ttu-id="661d8-135">Installare [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="661d8-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="661d8-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-136">2\.</span></span> <span data-ttu-id="661d8-137">Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="661d8-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="661d8-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-138">3\.</span></span> <span data-ttu-id="661d8-139">Per un'esperienza Blazor webassembly, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="661d8-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="661d8-140">Per un'esperienza di Blazor server, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="661d8-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="661d8-141">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="661d8-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="661d8-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-142">4\.</span></span> <span data-ttu-id="661d8-143">Aprire la cartella *WebApplication1* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="661d8-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="661d8-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-144">5\.</span></span> <span data-ttu-id="661d8-145">Per un progetto di Blazor server, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="661d8-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="661d8-146">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="661d8-146">Select **Yes**.</span></span>

   <span data-ttu-id="661d8-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-147">6\.</span></span> <span data-ttu-id="661d8-148">Se si usa un'app Server Blazor, eseguire l'app usando il debugger Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="661d8-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="661d8-149">Se si usa un'app webassembly Blazor, eseguire `dotnet run` dalla cartella del progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="661d8-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="661d8-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-150">7\.</span></span> <span data-ttu-id="661d8-151">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="661d8-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="661d8-152">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="661d8-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="661d8-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-153">1\.</span></span> <span data-ttu-id="661d8-154">Installare [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="661d8-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="661d8-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-155">2\.</span></span> <span data-ttu-id="661d8-156">Selezionare **File** > **nuova soluzione** o creare un **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="661d8-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="661d8-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-157">3\.</span></span> <span data-ttu-id="661d8-158">Nella barra laterale selezionare **.NET Core** > **app**.</span><span class="sxs-lookup"><span data-stu-id="661d8-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="661d8-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-159">4\.</span></span> <span data-ttu-id="661d8-160">Selezionare il modello **app ServerBlazor** .</span><span class="sxs-lookup"><span data-stu-id="661d8-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="661d8-161">Attualmente è disponibile in Visual Studio per Mac solo il modello di Blazor server.</span><span class="sxs-lookup"><span data-stu-id="661d8-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="661d8-162">Per un'esperienza Blazor webassembly, seguire le istruzioni riportate nella scheda **interfaccia della riga di comando di .NET Core** . Dopo aver selezionato il modello di Blazor server, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="661d8-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="661d8-163">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="661d8-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="661d8-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-164">5\.</span></span> <span data-ttu-id="661d8-165">Impostare il **Framework di destinazione** su **.NET Core 3,1** e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="661d8-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="661d8-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-166">6\.</span></span> <span data-ttu-id="661d8-167">Nel campo **nome progetto** assegnare un nome all'app `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="661d8-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="661d8-168">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="661d8-168">Select **Create**.</span></span>

   <span data-ttu-id="661d8-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-169">7\.</span></span> <span data-ttu-id="661d8-170">Selezionare **esegui** > **Esegui senza eseguire debug** per eseguire l'app *senza il debugger*.</span><span class="sxs-lookup"><span data-stu-id="661d8-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="661d8-171">Eseguire l'app con **Avvia debug** per eseguire l'app *con il debugger*.</span><span class="sxs-lookup"><span data-stu-id="661d8-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="661d8-172">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="661d8-172">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="661d8-173">Per un'esperienza Blazor webassembly, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="661d8-173">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="661d8-174">Per un'esperienza di Blazor server, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="661d8-174">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="661d8-175">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="661d8-175">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="661d8-176">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="661d8-176">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="661d8-177">Installare la versione più recente di [.NET Core 3,0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="661d8-177">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="661d8-178">Facoltativamente, installare il modello di [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="661d8-178">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="661d8-179">Installare [.NET Core 3,1 o versione successiva (anteprima) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="661d8-179">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="661d8-180">Eseguire il comando seguente in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="661d8-180">Run the following command in a command shell.</span></span> <span data-ttu-id="661d8-181">[Microsoft.AspNetCore.Blazor.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Il pacchetto di modelli include una versione di anteprima mentre Blazor webassembly è in anteprima.
</span><span class="sxs-lookup"><span data-stu-id="661d8-181">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="661d8-182">Seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="661d8-182">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="661d8-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="661d8-183">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="661d8-184">1 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-184">1\.</span></span> <span data-ttu-id="661d8-185">Installare la versione più recente di [Visual Studio](https://visualstudio.com/vs/) con il carico di lavoro **sviluppo di ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="661d8-185">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="661d8-186">2 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-186">2\.</span></span> <span data-ttu-id="661d8-187">Facoltativamente, installare [Visual Studio 16,4 Preview 2 o versione successiva](https://visualstudio.microsoft.com/vs/preview/) con il carico di lavoro **sviluppo di ASP.NET e Web** per lo sviluppo di app webassembly Blazor.</span><span class="sxs-lookup"><span data-stu-id="661d8-187">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="661d8-188">3 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-188">3\.</span></span> <span data-ttu-id="661d8-189">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="661d8-189">Create a new project.</span></span>

   <span data-ttu-id="661d8-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-190">4\.</span></span> <span data-ttu-id="661d8-191">Selezionare **Blazor app**.</span><span class="sxs-lookup"><span data-stu-id="661d8-191">Select **Blazor App**.</span></span> <span data-ttu-id="661d8-192">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="661d8-192">Select **Next**.</span></span>

   <span data-ttu-id="661d8-193">5 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-193">5\.</span></span> <span data-ttu-id="661d8-194">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="661d8-194">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="661d8-195">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="661d8-195">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="661d8-196">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="661d8-196">Select **Create**.</span></span>

   <span data-ttu-id="661d8-197">6 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-197">6\.</span></span> <span data-ttu-id="661d8-198">Per un'esperienza Blazor webassembly, scegliere il modello **app webassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="661d8-198">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="661d8-199">Per un'esperienza di Blazor server, scegliere il modello **app serverBlazor** .</span><span class="sxs-lookup"><span data-stu-id="661d8-199">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="661d8-200">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="661d8-200">Select **Create**.</span></span> <span data-ttu-id="661d8-201">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="661d8-201">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="661d8-202">7 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-202">7\.</span></span> <span data-ttu-id="661d8-203">Premere **F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="661d8-203">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="661d8-204">Se è stato installato il Blazor estensione di Visual Studio per una versione di anteprima precedente di ASP.NET Core Blazor (anteprima 6 o precedente), è possibile disinstallare l'estensione.</span><span class="sxs-lookup"><span data-stu-id="661d8-204">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="661d8-205">L'installazione dei modelli di Blazor in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="661d8-205">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="661d8-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="661d8-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="661d8-207">1 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-207">1\.</span></span> <span data-ttu-id="661d8-208">Installare [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="661d8-208">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="661d8-209">2 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-209">2\.</span></span> <span data-ttu-id="661d8-210">Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="661d8-210">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="661d8-211">3 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-211">3\.</span></span> <span data-ttu-id="661d8-212">Per un'esperienza Blazor webassembly, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="661d8-212">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="661d8-213">Per un'esperienza di Blazor server, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="661d8-213">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="661d8-214">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="661d8-214">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="661d8-215">4 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-215">4\.</span></span> <span data-ttu-id="661d8-216">Aprire la cartella *WebApplication1* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="661d8-216">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="661d8-217">5 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-217">5\.</span></span> <span data-ttu-id="661d8-218">Per un progetto di Blazor server, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="661d8-218">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="661d8-219">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="661d8-219">Select **Yes**.</span></span>

   <span data-ttu-id="661d8-220">6 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-220">6\.</span></span> <span data-ttu-id="661d8-221">Se si usa un'app Server Blazor, eseguire l'app usando il debugger Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="661d8-221">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="661d8-222">Se si usa un'app webassembly Blazor, eseguire `dotnet run` dalla cartella del progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="661d8-222">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="661d8-223">7 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-223">7\.</span></span> <span data-ttu-id="661d8-224">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="661d8-224">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="661d8-225">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="661d8-225">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="661d8-226">1 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-226">1\.</span></span> <span data-ttu-id="661d8-227">Installare [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="661d8-227">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="661d8-228">[Consente di impostare l'anteprima del canale di aggiornamento](/visualstudio/mac/install-preview).</span><span class="sxs-lookup"><span data-stu-id="661d8-228">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="661d8-229">2 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-229">2\.</span></span> <span data-ttu-id="661d8-230">Selezionare **File** > **nuova soluzione** o creare un **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="661d8-230">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="661d8-231">3 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-231">3\.</span></span> <span data-ttu-id="661d8-232">Nella barra laterale selezionare **.NET Core** > **app**.</span><span class="sxs-lookup"><span data-stu-id="661d8-232">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="661d8-233">4 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-233">4\.</span></span> <span data-ttu-id="661d8-234">Selezionare il modello **app ServerBlazor** .</span><span class="sxs-lookup"><span data-stu-id="661d8-234">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="661d8-235">Attualmente è disponibile in Visual Studio per Mac solo il modello di Blazor server.</span><span class="sxs-lookup"><span data-stu-id="661d8-235">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="661d8-236">Per un'esperienza Blazor webassembly, seguire le istruzioni riportate nella scheda **interfaccia della riga di comando di .NET Core** . Dopo aver selezionato il modello di Blazor server, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="661d8-236">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="661d8-237">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="661d8-237">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="661d8-238">5 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-238">5\.</span></span> <span data-ttu-id="661d8-239">Impostare il **Framework di destinazione** su **.NET Core 3,0** e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="661d8-239">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="661d8-240">6 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-240">6\.</span></span> <span data-ttu-id="661d8-241">Nel campo **nome progetto** assegnare un nome all'app `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="661d8-241">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="661d8-242">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="661d8-242">Select **Create**.</span></span>

   <span data-ttu-id="661d8-243">7 \.</span><span class="sxs-lookup"><span data-stu-id="661d8-243">7\.</span></span> <span data-ttu-id="661d8-244">Selezionare **esegui** > **Esegui senza eseguire debug** per eseguire l'app *senza il debugger*.</span><span class="sxs-lookup"><span data-stu-id="661d8-244">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="661d8-245">Eseguire l'app con **Avvia debug** per eseguire l'app *con il debugger*.</span><span class="sxs-lookup"><span data-stu-id="661d8-245">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="661d8-246">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="661d8-246">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="661d8-247">Per un'esperienza Blazor webassembly, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="661d8-247">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="661d8-248">Per un'esperienza di Blazor server, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="661d8-248">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="661d8-249">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="661d8-249">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="661d8-250">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="661d8-250">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="661d8-251">Nelle schede della barra laterale sono disponibili più pagine:</span><span class="sxs-lookup"><span data-stu-id="661d8-251">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="661d8-252">Home page di</span><span class="sxs-lookup"><span data-stu-id="661d8-252">Home</span></span>
* <span data-ttu-id="661d8-253">Counter</span><span class="sxs-lookup"><span data-stu-id="661d8-253">Counter</span></span>
* <span data-ttu-id="661d8-254">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="661d8-254">Fetch data</span></span>

<span data-ttu-id="661d8-255">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="661d8-255">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="661d8-256">Per incrementare un contatore in una pagina Web è in genere necessario scrivere JavaScript, ma con Blazor C#è possibile utilizzare.</span><span class="sxs-lookup"><span data-stu-id="661d8-256">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="661d8-257">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="661d8-257">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="661d8-258">Una richiesta di `/counter` nel browser, come specificato dalla direttiva `@page` nella parte superiore, fa sì che il componente `Counter` esegua il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="661d8-258">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="661d8-259">I componenti eseguono il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="661d8-259">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="661d8-260">Ogni volta che viene selezionato il pulsante **Click me** :</span><span class="sxs-lookup"><span data-stu-id="661d8-260">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="661d8-261">Viene generato l'evento `onclick`.</span><span class="sxs-lookup"><span data-stu-id="661d8-261">The `onclick` event is fired.</span></span>
* <span data-ttu-id="661d8-262">Viene chiamato il metodo `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="661d8-262">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="661d8-263">Il `currentCount` viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="661d8-263">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="661d8-264">Il rendering del componente viene eseguito nuovamente.</span><span class="sxs-lookup"><span data-stu-id="661d8-264">The component is rendered again.</span></span>

<span data-ttu-id="661d8-265">Il runtime confronta il nuovo contenuto con il contenuto precedente e applica solo il contenuto modificato al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="661d8-265">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="661d8-266">Aggiungere un componente a un altro componente usando la sintassi HTML.</span><span class="sxs-lookup"><span data-stu-id="661d8-266">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="661d8-267">Ad esempio, aggiungere il componente `Counter` alla Home page dell'app aggiungendo un elemento `<Counter />` al componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="661d8-267">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="661d8-268">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="661d8-268">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="661d8-269">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="661d8-269">Run the app.</span></span> <span data-ttu-id="661d8-270">La Home page presenta il proprio contatore fornito dal componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="661d8-270">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="661d8-271">I parametri del componente vengono specificati utilizzando attributi o [contenuto figlio](xref:blazor/components#child-content), che consentono di impostare le proprietà per il componente figlio.</span><span class="sxs-lookup"><span data-stu-id="661d8-271">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="661d8-272">Per aggiungere un parametro al componente `Counter`, aggiornare il blocco `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="661d8-272">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="661d8-273">Aggiungere una proprietà pubblica per `IncrementAmount` con un attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="661d8-273">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="661d8-274">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="661d8-274">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="661d8-275">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="661d8-275">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="661d8-276">Specificare il `IncrementAmount` nell'elemento `<Counter>` del componente di `Index` utilizzando un attributo.</span><span class="sxs-lookup"><span data-stu-id="661d8-276">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="661d8-277">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="661d8-277">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="661d8-278">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="661d8-278">Run the app.</span></span> <span data-ttu-id="661d8-279">Il componente `Index` dispone di un contatore che aumenta di dieci ogni volta che viene selezionato il pulsante **Click me** .</span><span class="sxs-lookup"><span data-stu-id="661d8-279">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="661d8-280">Il componente `Counter` (*Counter. Razor*) in `/counter` continua a incrementare di uno.</span><span class="sxs-lookup"><span data-stu-id="661d8-280">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="661d8-281">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="661d8-281">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="661d8-282">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="661d8-282">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
