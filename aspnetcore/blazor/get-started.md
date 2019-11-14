---
title: Introduzione a ASP.NET Core Blazor
author: guardrex
description: Inizia a usare Blazor compilando un'app Blazor con gli strumenti che preferisci.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 9b4aee0be30568f098c756e9ab4cb5298e9a049b
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963006"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="3e5af-103">Introduzione a ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="3e5af-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="3e5af-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3e5af-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="3e5af-105">Introduzione a Blazor:</span><span class="sxs-lookup"><span data-stu-id="3e5af-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="3e5af-106">Installare [.NET Core 3,1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="3e5af-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="3e5af-107">Installare il modello di [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) eseguendo il comando seguente in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="3e5af-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="3e5af-108">[Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Il pacchetto di modelli include una versione di anteprima mentre Blazor webassembly è in anteprima.</span><span class="sxs-lookup"><span data-stu-id="3e5af-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. <span data-ttu-id="3e5af-109">Seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="3e5af-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3e5af-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3e5af-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="3e5af-111">1 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-111">1\.</span></span> <span data-ttu-id="3e5af-112">Installare [Visual Studio 16,4 Preview 2 o versione successiva](https://visualstudio.microsoft.com/vs/preview/) con il carico di lavoro **sviluppo di ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="3e5af-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="3e5af-113">2 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-113">2\.</span></span> <span data-ttu-id="3e5af-114">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="3e5af-114">Create a new project.</span></span>

   <span data-ttu-id="3e5af-115">3 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-115">3\.</span></span> <span data-ttu-id="3e5af-116">Selezionare **Blazor app**.</span><span class="sxs-lookup"><span data-stu-id="3e5af-116">Select **Blazor App**.</span></span> <span data-ttu-id="3e5af-117">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="3e5af-117">Select **Next**.</span></span>

   <span data-ttu-id="3e5af-118">4 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-118">4\.</span></span> <span data-ttu-id="3e5af-119">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="3e5af-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="3e5af-120">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="3e5af-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="3e5af-121">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3e5af-121">Select **Create**.</span></span>

   <span data-ttu-id="3e5af-122">5 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-122">5\.</span></span> <span data-ttu-id="3e5af-123">Per un'esperienza Blazor webassembly, scegliere il modello **app webassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="3e5af-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="3e5af-124">Per un'esperienza di Blazor server, scegliere il modello **app serverBlazor** .</span><span class="sxs-lookup"><span data-stu-id="3e5af-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="3e5af-125">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3e5af-125">Select **Create**.</span></span> <span data-ttu-id="3e5af-126">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3e5af-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3e5af-127">6 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-127">6\.</span></span> <span data-ttu-id="3e5af-128">Premere **CTRL**+**F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="3e5af-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3e5af-129">Se è stato installato il Blazor estensione di Visual Studio per una versione di anteprima precedente di ASP.NET Core Blazor (anteprima 6 o precedente), è possibile disinstallare l'estensione.</span><span class="sxs-lookup"><span data-stu-id="3e5af-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="3e5af-130">L'installazione dei modelli di Blazor in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e5af-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3e5af-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3e5af-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="3e5af-132">1 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-132">1\.</span></span> <span data-ttu-id="3e5af-133">Installare [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="3e5af-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="3e5af-134">2 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-134">2\.</span></span> <span data-ttu-id="3e5af-135">Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="3e5af-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="3e5af-136">3 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-136">3\.</span></span> <span data-ttu-id="3e5af-137">Per un'esperienza Blazor webassembly, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="3e5af-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="3e5af-138">Per un'esperienza di Blazor server, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="3e5af-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="3e5af-139">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3e5af-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3e5af-140">4 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-140">4\.</span></span> <span data-ttu-id="3e5af-141">Aprire la cartella *WebApplication1* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3e5af-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="3e5af-142">5 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-142">5\.</span></span> <span data-ttu-id="3e5af-143">Per un progetto di Blazor server, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="3e5af-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="3e5af-144">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="3e5af-144">Select **Yes**.</span></span>

   <span data-ttu-id="3e5af-145">6 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-145">6\.</span></span> <span data-ttu-id="3e5af-146">Se si usa un'app Server Blazor, eseguire l'app usando il debugger Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3e5af-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="3e5af-147">Se si usa un'app webassembly Blazor, eseguire `dotnet run` dalla cartella del progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="3e5af-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="3e5af-148">7 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-148">7\.</span></span> <span data-ttu-id="3e5af-149">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="3e5af-149">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3e5af-150">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="3e5af-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="3e5af-151">Per un'esperienza Blazor webassembly, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="3e5af-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="3e5af-152">Per un'esperienza di Blazor server, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="3e5af-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="3e5af-153">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3e5af-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3e5af-154">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="3e5af-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="3e5af-155">Installare la versione più recente di [.NET Core 3,0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) .</span><span class="sxs-lookup"><span data-stu-id="3e5af-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="3e5af-156">Facoltativamente, installare il modello di [Blazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) installando [.NET Core 3,1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) ed eseguendo il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="3e5af-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. <span data-ttu-id="3e5af-157">Seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="3e5af-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3e5af-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3e5af-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="3e5af-159">1 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-159">1\.</span></span> <span data-ttu-id="3e5af-160">Installare la versione più recente di [Visual Studio](https://visualstudio.com/vs/) con il carico di lavoro **sviluppo di ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="3e5af-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="3e5af-161">2 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-161">2\.</span></span> <span data-ttu-id="3e5af-162">Facoltativamente, installare [Visual Studio 16,4 Preview 2 o versione successiva](https://visualstudio.microsoft.com/vs/preview/) con il carico di lavoro **sviluppo di ASP.NET e Web** per lo sviluppo di app webassembly Blazor.</span><span class="sxs-lookup"><span data-stu-id="3e5af-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="3e5af-163">3 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-163">3\.</span></span> <span data-ttu-id="3e5af-164">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="3e5af-164">Create a new project.</span></span>

   <span data-ttu-id="3e5af-165">4 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-165">4\.</span></span> <span data-ttu-id="3e5af-166">Selezionare **Blazor app**.</span><span class="sxs-lookup"><span data-stu-id="3e5af-166">Select **Blazor App**.</span></span> <span data-ttu-id="3e5af-167">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="3e5af-167">Select **Next**.</span></span>

   <span data-ttu-id="3e5af-168">5 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-168">5\.</span></span> <span data-ttu-id="3e5af-169">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="3e5af-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="3e5af-170">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="3e5af-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="3e5af-171">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3e5af-171">Select **Create**.</span></span>

   <span data-ttu-id="3e5af-172">6 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-172">6\.</span></span> <span data-ttu-id="3e5af-173">Per un'esperienza Blazor webassembly, scegliere il modello **app webassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="3e5af-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="3e5af-174">Per un'esperienza di Blazor server, scegliere il modello **app serverBlazor** .</span><span class="sxs-lookup"><span data-stu-id="3e5af-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="3e5af-175">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3e5af-175">Select **Create**.</span></span> <span data-ttu-id="3e5af-176">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3e5af-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3e5af-177">7 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-177">7\.</span></span> <span data-ttu-id="3e5af-178">Premere **F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="3e5af-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3e5af-179">Se è stato installato il Blazor estensione di Visual Studio per una versione di anteprima precedente di ASP.NET Core Blazor (anteprima 6 o precedente), è possibile disinstallare l'estensione.</span><span class="sxs-lookup"><span data-stu-id="3e5af-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="3e5af-180">L'installazione dei modelli di Blazor in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e5af-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3e5af-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3e5af-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="3e5af-182">1 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-182">1\.</span></span> <span data-ttu-id="3e5af-183">Installare [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="3e5af-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="3e5af-184">2 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-184">2\.</span></span> <span data-ttu-id="3e5af-185">Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="3e5af-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="3e5af-186">3 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-186">3\.</span></span> <span data-ttu-id="3e5af-187">Per un'esperienza Blazor webassembly, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="3e5af-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="3e5af-188">Per un'esperienza di Blazor server, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="3e5af-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="3e5af-189">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3e5af-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3e5af-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-190">4\.</span></span> <span data-ttu-id="3e5af-191">Aprire la cartella *WebApplication1* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3e5af-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="3e5af-192">5 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-192">5\.</span></span> <span data-ttu-id="3e5af-193">Per un progetto di Blazor server, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="3e5af-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="3e5af-194">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="3e5af-194">Select **Yes**.</span></span>

   <span data-ttu-id="3e5af-195">6 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-195">6\.</span></span> <span data-ttu-id="3e5af-196">Se si usa un'app Server Blazor, eseguire l'app usando il debugger Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3e5af-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="3e5af-197">Se si usa un'app webassembly Blazor, eseguire `dotnet run` dalla cartella del progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="3e5af-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="3e5af-198">7 \.</span><span class="sxs-lookup"><span data-stu-id="3e5af-198">7\.</span></span> <span data-ttu-id="3e5af-199">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="3e5af-199">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3e5af-200">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="3e5af-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="3e5af-201">Per un'esperienza Blazor webassembly, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="3e5af-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="3e5af-202">Per un'esperienza di Blazor server, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="3e5af-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="3e5af-203">Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="3e5af-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="3e5af-204">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="3e5af-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="3e5af-205">Nelle schede della barra laterale sono disponibili più pagine:</span><span class="sxs-lookup"><span data-stu-id="3e5af-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="3e5af-206">Home</span><span class="sxs-lookup"><span data-stu-id="3e5af-206">Home</span></span>
* <span data-ttu-id="3e5af-207">Counter</span><span class="sxs-lookup"><span data-stu-id="3e5af-207">Counter</span></span>
* <span data-ttu-id="3e5af-208">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="3e5af-208">Fetch data</span></span>

<span data-ttu-id="3e5af-209">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="3e5af-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="3e5af-210">Per incrementare un contatore in una pagina Web è in genere necessario scrivere JavaScript, ma con Blazor C#è possibile utilizzare.</span><span class="sxs-lookup"><span data-stu-id="3e5af-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="3e5af-211">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="3e5af-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="3e5af-212">Una richiesta per `/counter` nel browser, come specificato dalla direttiva `@page` nella parte superiore, causa il rendering del contenuto da parte del componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="3e5af-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="3e5af-213">I componenti eseguono il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="3e5af-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="3e5af-214">Ogni volta che viene selezionato il pulsante **Click me** :</span><span class="sxs-lookup"><span data-stu-id="3e5af-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="3e5af-215">Viene generato l'evento `onclick`.</span><span class="sxs-lookup"><span data-stu-id="3e5af-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="3e5af-216">Viene chiamato il metodo `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="3e5af-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="3e5af-217">Il `currentCount` viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="3e5af-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="3e5af-218">Il rendering del componente viene eseguito nuovamente.</span><span class="sxs-lookup"><span data-stu-id="3e5af-218">The component is rendered again.</span></span>

<span data-ttu-id="3e5af-219">Il runtime confronta il nuovo contenuto con il contenuto precedente e applica solo il contenuto modificato al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="3e5af-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="3e5af-220">Aggiungere un componente a un altro componente usando la sintassi HTML.</span><span class="sxs-lookup"><span data-stu-id="3e5af-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="3e5af-221">Ad esempio, aggiungere il componente `Counter` alla Home page dell'app aggiungendo un elemento `<Counter />` al componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="3e5af-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="3e5af-222">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="3e5af-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="3e5af-223">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="3e5af-223">Run the app.</span></span> <span data-ttu-id="3e5af-224">Il contatore della Home page è fornito dal componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="3e5af-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="3e5af-225">I parametri del componente vengono specificati utilizzando attributi o [contenuto figlio](xref:blazor/components#child-content), che consentono di impostare le proprietà per il componente figlio.</span><span class="sxs-lookup"><span data-stu-id="3e5af-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="3e5af-226">Per aggiungere un parametro al componente `Counter`, aggiornare il blocco `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="3e5af-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="3e5af-227">Aggiungere una proprietà pubblica per `IncrementAmount` con un attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="3e5af-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="3e5af-228">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="3e5af-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="3e5af-229">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="3e5af-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="3e5af-230">Specificare il `IncrementAmount` nell'elemento `<Counter>` del componente `Index` usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="3e5af-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="3e5af-231">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="3e5af-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="3e5af-232">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="3e5af-232">Run the app.</span></span> <span data-ttu-id="3e5af-233">Il componente `Index` ha il proprio contatore che viene incrementato di dieci ogni volta che viene selezionato il pulsante **Click me** .</span><span class="sxs-lookup"><span data-stu-id="3e5af-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="3e5af-234">Il componente `Counter` (*Counter. Razor*) in `/counter` continua a incrementare di uno.</span><span class="sxs-lookup"><span data-stu-id="3e5af-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e5af-235">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="3e5af-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="3e5af-236">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3e5af-236">Additional resources</span></span>

* <xref:signalr/introduction>
