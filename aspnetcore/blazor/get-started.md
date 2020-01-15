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
ms.openlocfilehash: 2135c2a090d60ec7a46fa4f899f0f14987b6b4e0
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75951716"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="c0571-103">Introduzione a ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="c0571-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="c0571-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c0571-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="c0571-105">Introduzione a Blazor:</span><span class="sxs-lookup"><span data-stu-id="c0571-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="c0571-106">Installare [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="c0571-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="c0571-107">Facoltativamente, installare il modello di [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="c0571-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="c0571-108">Installare [.NET Core 3,1 o versione successiva (anteprima) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="c0571-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="c0571-109">Eseguire il comando seguente in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c0571-109">Run the following command in a command shell.</span></span> <span data-ttu-id="c0571-110">[Microsoft.AspNetCore.Blazor.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Il pacchetto di modelli include una versione di anteprima mentre Blazor webassembly è in anteprima.</span><span class="sxs-lookup"><span data-stu-id="c0571-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="c0571-111">Seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="c0571-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0571-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0571-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="c0571-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-113">1\.</span></span> <span data-ttu-id="c0571-114">Installare [Visual Studio 16,4 o versione successiva](https://visualstudio.microsoft.com/vs/preview/) con il carico di lavoro **sviluppo di ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="c0571-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="c0571-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-115">2\.</span></span> <span data-ttu-id="c0571-116">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="c0571-116">Create a new project.</span></span>

   <span data-ttu-id="c0571-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-117">3\.</span></span> <span data-ttu-id="c0571-118">Selezionare **Blazor app**.</span><span class="sxs-lookup"><span data-stu-id="c0571-118">Select **Blazor App**.</span></span> <span data-ttu-id="c0571-119">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="c0571-119">Select **Next**.</span></span>

   <span data-ttu-id="c0571-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-120">4\.</span></span> <span data-ttu-id="c0571-121">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="c0571-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="c0571-122">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="c0571-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="c0571-123">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c0571-123">Select **Create**.</span></span>

   <span data-ttu-id="c0571-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-124">5\.</span></span> <span data-ttu-id="c0571-125">Per un'esperienza Blazor webassembly, scegliere il modello **app webassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="c0571-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="c0571-126">Per un'esperienza di Blazor server, scegliere il modello **app serverBlazor** .</span><span class="sxs-lookup"><span data-stu-id="c0571-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="c0571-127">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c0571-127">Select **Create**.</span></span> <span data-ttu-id="c0571-128">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="c0571-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="c0571-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-129">6\.</span></span> <span data-ttu-id="c0571-130">Premere **CTRL**+**F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="c0571-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c0571-131">Se è stato installato il Blazor estensione di Visual Studio per una versione di anteprima precedente di ASP.NET Core Blazor (anteprima 6 o precedente), è possibile disinstallare l'estensione.</span><span class="sxs-lookup"><span data-stu-id="c0571-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="c0571-132">L'installazione dei modelli di Blazor in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0571-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c0571-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c0571-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="c0571-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-134">1\.</span></span> <span data-ttu-id="c0571-135">Installare [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="c0571-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="c0571-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-136">2\.</span></span> <span data-ttu-id="c0571-137">Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="c0571-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="c0571-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-138">3\.</span></span> <span data-ttu-id="c0571-139">Per un'esperienza Blazor webassembly, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c0571-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="c0571-140">Per un'esperienza di Blazor server, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c0571-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="c0571-141">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="c0571-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="c0571-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-142">4\.</span></span> <span data-ttu-id="c0571-143">Aprire la cartella *WebApplication1* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c0571-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="c0571-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-144">5\.</span></span> <span data-ttu-id="c0571-145">Per un progetto di Blazor server, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="c0571-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="c0571-146">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="c0571-146">Select **Yes**.</span></span>

   <span data-ttu-id="c0571-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-147">6\.</span></span> <span data-ttu-id="c0571-148">Se si usa un'app Server Blazor, eseguire l'app usando il debugger Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c0571-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="c0571-149">Se si usa un'app webassembly Blazor, eseguire `dotnet run` dalla cartella del progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0571-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="c0571-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-150">7\.</span></span> <span data-ttu-id="c0571-151">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="c0571-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c0571-152">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c0571-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="c0571-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-153">1\.</span></span> <span data-ttu-id="c0571-154">Installare [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="c0571-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="c0571-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-155">2\.</span></span> <span data-ttu-id="c0571-156">Selezionare **File** > **nuova soluzione** o creare un **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="c0571-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="c0571-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-157">3\.</span></span> <span data-ttu-id="c0571-158">Nella barra laterale selezionare **.NET Core** > **app**.</span><span class="sxs-lookup"><span data-stu-id="c0571-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="c0571-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-159">4\.</span></span> <span data-ttu-id="c0571-160">Selezionare il modello **app ServerBlazor** .</span><span class="sxs-lookup"><span data-stu-id="c0571-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="c0571-161">Attualmente è disponibile in Visual Studio per Mac solo il modello di Blazor server.</span><span class="sxs-lookup"><span data-stu-id="c0571-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="c0571-162">Per un'esperienza Blazor webassembly, seguire le istruzioni riportate nella scheda **interfaccia della riga di comando di .NET Core** . Dopo aver selezionato il modello di Blazor server, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="c0571-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="c0571-163">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="c0571-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="c0571-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-164">5\.</span></span> <span data-ttu-id="c0571-165">Impostare il **Framework di destinazione** su **.NET Core 3,1** e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="c0571-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="c0571-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-166">6\.</span></span> <span data-ttu-id="c0571-167">Nel campo **nome progetto** assegnare un nome all'app `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="c0571-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="c0571-168">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c0571-168">Select **Create**.</span></span>

   <span data-ttu-id="c0571-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-169">7\.</span></span> <span data-ttu-id="c0571-170">Selezionare **esegui** > **Esegui senza eseguire debug** per eseguire l'app *senza il debugger*.</span><span class="sxs-lookup"><span data-stu-id="c0571-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="c0571-171">Eseguire l'app con **Avvia debug** per eseguire l'app *con il debugger*.</span><span class="sxs-lookup"><span data-stu-id="c0571-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="c0571-172">Se viene visualizzato un messaggio per considerare attendibile il certificato di sviluppo, considerare attendibile il certificato e continuare.</span><span class="sxs-lookup"><span data-stu-id="c0571-172">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c0571-173">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c0571-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="c0571-174">Per un'esperienza Blazor webassembly, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c0571-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="c0571-175">Per un'esperienza di Blazor server, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c0571-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="c0571-176">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="c0571-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="c0571-177">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="c0571-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="c0571-178">Installare la versione più recente di [.NET Core 3,0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="c0571-178">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="c0571-179">Facoltativamente, installare il modello di [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="c0571-179">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="c0571-180">Installare [.NET Core 3,1 o versione successiva (anteprima) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="c0571-180">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="c0571-181">Eseguire il comando seguente in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c0571-181">Run the following command in a command shell.</span></span> <span data-ttu-id="c0571-182">[Microsoft.AspNetCore.Blazor.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Il pacchetto di modelli include una versione di anteprima mentre Blazor webassembly è in anteprima.</span><span class="sxs-lookup"><span data-stu-id="c0571-182">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="c0571-183">Seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="c0571-183">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0571-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0571-184">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="c0571-185">1 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-185">1\.</span></span> <span data-ttu-id="c0571-186">Installare la versione più recente di [Visual Studio](https://visualstudio.com/vs/) con il carico di lavoro **sviluppo di ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="c0571-186">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="c0571-187">2 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-187">2\.</span></span> <span data-ttu-id="c0571-188">Facoltativamente, installare [Visual Studio 16,4 Preview 2 o versione successiva](https://visualstudio.microsoft.com/vs/preview/) con il carico di lavoro **sviluppo di ASP.NET e Web** per lo sviluppo di app webassembly Blazor.</span><span class="sxs-lookup"><span data-stu-id="c0571-188">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="c0571-189">3 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-189">3\.</span></span> <span data-ttu-id="c0571-190">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="c0571-190">Create a new project.</span></span>

   <span data-ttu-id="c0571-191">4 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-191">4\.</span></span> <span data-ttu-id="c0571-192">Selezionare **Blazor app**.</span><span class="sxs-lookup"><span data-stu-id="c0571-192">Select **Blazor App**.</span></span> <span data-ttu-id="c0571-193">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="c0571-193">Select **Next**.</span></span>

   <span data-ttu-id="c0571-194">5 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-194">5\.</span></span> <span data-ttu-id="c0571-195">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="c0571-195">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="c0571-196">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="c0571-196">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="c0571-197">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c0571-197">Select **Create**.</span></span>

   <span data-ttu-id="c0571-198">6 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-198">6\.</span></span> <span data-ttu-id="c0571-199">Per un'esperienza Blazor webassembly, scegliere il modello **app webassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="c0571-199">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="c0571-200">Per un'esperienza di Blazor server, scegliere il modello **app serverBlazor** .</span><span class="sxs-lookup"><span data-stu-id="c0571-200">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="c0571-201">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c0571-201">Select **Create**.</span></span> <span data-ttu-id="c0571-202">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="c0571-202">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="c0571-203">7 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-203">7\.</span></span> <span data-ttu-id="c0571-204">Premere **F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="c0571-204">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c0571-205">Se è stato installato il Blazor estensione di Visual Studio per una versione di anteprima precedente di ASP.NET Core Blazor (anteprima 6 o precedente), è possibile disinstallare l'estensione.</span><span class="sxs-lookup"><span data-stu-id="c0571-205">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="c0571-206">L'installazione dei modelli di Blazor in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0571-206">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c0571-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c0571-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="c0571-208">1 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-208">1\.</span></span> <span data-ttu-id="c0571-209">Installare [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="c0571-209">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="c0571-210">2 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-210">2\.</span></span> <span data-ttu-id="c0571-211">Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="c0571-211">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="c0571-212">3 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-212">3\.</span></span> <span data-ttu-id="c0571-213">Per un'esperienza Blazor webassembly, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c0571-213">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="c0571-214">Per un'esperienza di Blazor server, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c0571-214">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="c0571-215">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="c0571-215">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="c0571-216">4 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-216">4\.</span></span> <span data-ttu-id="c0571-217">Aprire la cartella *WebApplication1* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c0571-217">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="c0571-218">5 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-218">5\.</span></span> <span data-ttu-id="c0571-219">Per un progetto di Blazor server, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="c0571-219">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="c0571-220">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="c0571-220">Select **Yes**.</span></span>

   <span data-ttu-id="c0571-221">6 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-221">6\.</span></span> <span data-ttu-id="c0571-222">Se si usa un'app Server Blazor, eseguire l'app usando il debugger Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c0571-222">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="c0571-223">Se si usa un'app webassembly Blazor, eseguire `dotnet run` dalla cartella del progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="c0571-223">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="c0571-224">7 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-224">7\.</span></span> <span data-ttu-id="c0571-225">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="c0571-225">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c0571-226">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c0571-226">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="c0571-227">1 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-227">1\.</span></span> <span data-ttu-id="c0571-228">Installare [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="c0571-228">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="c0571-229">[Consente di impostare l'anteprima del canale di aggiornamento](/visualstudio/mac/install-preview).</span><span class="sxs-lookup"><span data-stu-id="c0571-229">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="c0571-230">2 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-230">2\.</span></span> <span data-ttu-id="c0571-231">Selezionare **File** > **nuova soluzione** o creare un **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="c0571-231">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="c0571-232">3 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-232">3\.</span></span> <span data-ttu-id="c0571-233">Nella barra laterale selezionare **.NET Core** > **app**.</span><span class="sxs-lookup"><span data-stu-id="c0571-233">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="c0571-234">4 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-234">4\.</span></span> <span data-ttu-id="c0571-235">Selezionare il modello **app ServerBlazor** .</span><span class="sxs-lookup"><span data-stu-id="c0571-235">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="c0571-236">Attualmente è disponibile in Visual Studio per Mac solo il modello di Blazor server.</span><span class="sxs-lookup"><span data-stu-id="c0571-236">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="c0571-237">Per un'esperienza Blazor webassembly, seguire le istruzioni riportate nella scheda **interfaccia della riga di comando di .NET Core** . Dopo aver selezionato il modello di Blazor server, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="c0571-237">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="c0571-238">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="c0571-238">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="c0571-239">5 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-239">5\.</span></span> <span data-ttu-id="c0571-240">Impostare il **Framework di destinazione** su **.NET Core 3,0** e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="c0571-240">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="c0571-241">6 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-241">6\.</span></span> <span data-ttu-id="c0571-242">Nel campo **nome progetto** assegnare un nome all'app `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="c0571-242">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="c0571-243">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c0571-243">Select **Create**.</span></span>

   <span data-ttu-id="c0571-244">7 \.</span><span class="sxs-lookup"><span data-stu-id="c0571-244">7\.</span></span> <span data-ttu-id="c0571-245">Selezionare **esegui** > **Esegui senza eseguire debug** per eseguire l'app *senza il debugger*.</span><span class="sxs-lookup"><span data-stu-id="c0571-245">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="c0571-246">Eseguire l'app con **Avvia debug** per eseguire l'app *con il debugger*.</span><span class="sxs-lookup"><span data-stu-id="c0571-246">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c0571-247">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c0571-247">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="c0571-248">Per un'esperienza Blazor webassembly, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c0571-248">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="c0571-249">Per un'esperienza di Blazor server, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c0571-249">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="c0571-250">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="c0571-250">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="c0571-251">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="c0571-251">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="c0571-252">Nelle schede della barra laterale sono disponibili più pagine:</span><span class="sxs-lookup"><span data-stu-id="c0571-252">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="c0571-253">Home page di</span><span class="sxs-lookup"><span data-stu-id="c0571-253">Home</span></span>
* <span data-ttu-id="c0571-254">Counter</span><span class="sxs-lookup"><span data-stu-id="c0571-254">Counter</span></span>
* <span data-ttu-id="c0571-255">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="c0571-255">Fetch data</span></span>

<span data-ttu-id="c0571-256">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="c0571-256">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="c0571-257">Per incrementare un contatore in una pagina Web è in genere necessario scrivere JavaScript, ma con Blazor C#è possibile utilizzare.</span><span class="sxs-lookup"><span data-stu-id="c0571-257">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="c0571-258">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="c0571-258">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="c0571-259">Una richiesta di `/counter` nel browser, come specificato dalla direttiva `@page` nella parte superiore, fa sì che il componente `Counter` esegua il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="c0571-259">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="c0571-260">I componenti eseguono il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="c0571-260">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="c0571-261">Ogni volta che viene selezionato il pulsante **Click me** :</span><span class="sxs-lookup"><span data-stu-id="c0571-261">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="c0571-262">Viene generato l'evento `onclick`.</span><span class="sxs-lookup"><span data-stu-id="c0571-262">The `onclick` event is fired.</span></span>
* <span data-ttu-id="c0571-263">Viene chiamato il metodo `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="c0571-263">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="c0571-264">Il `currentCount` viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="c0571-264">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="c0571-265">Il rendering del componente viene eseguito nuovamente.</span><span class="sxs-lookup"><span data-stu-id="c0571-265">The component is rendered again.</span></span>

<span data-ttu-id="c0571-266">Il runtime confronta il nuovo contenuto con il contenuto precedente e applica solo il contenuto modificato al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="c0571-266">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="c0571-267">Aggiungere un componente a un altro componente usando la sintassi HTML.</span><span class="sxs-lookup"><span data-stu-id="c0571-267">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="c0571-268">Ad esempio, aggiungere il componente `Counter` alla Home page dell'app aggiungendo un elemento `<Counter />` al componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="c0571-268">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="c0571-269">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="c0571-269">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="c0571-270">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="c0571-270">Run the app.</span></span> <span data-ttu-id="c0571-271">La Home page presenta il proprio contatore fornito dal componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="c0571-271">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="c0571-272">I parametri del componente vengono specificati utilizzando attributi o [contenuto figlio](xref:blazor/components#child-content), che consentono di impostare le proprietà per il componente figlio.</span><span class="sxs-lookup"><span data-stu-id="c0571-272">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="c0571-273">Per aggiungere un parametro al componente `Counter`, aggiornare il blocco `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="c0571-273">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="c0571-274">Aggiungere una proprietà pubblica per `IncrementAmount` con un attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="c0571-274">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="c0571-275">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="c0571-275">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="c0571-276">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="c0571-276">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="c0571-277">Specificare il `IncrementAmount` nell'elemento `<Counter>` del componente di `Index` utilizzando un attributo.</span><span class="sxs-lookup"><span data-stu-id="c0571-277">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="c0571-278">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="c0571-278">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="c0571-279">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="c0571-279">Run the app.</span></span> <span data-ttu-id="c0571-280">Il componente `Index` dispone di un contatore che aumenta di dieci ogni volta che viene selezionato il pulsante **Click me** .</span><span class="sxs-lookup"><span data-stu-id="c0571-280">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="c0571-281">Il componente `Counter` (*Counter. Razor*) in `/counter` continua a incrementare di uno.</span><span class="sxs-lookup"><span data-stu-id="c0571-281">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0571-282">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c0571-282">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="c0571-283">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c0571-283">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
