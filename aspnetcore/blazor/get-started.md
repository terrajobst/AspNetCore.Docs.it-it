---
title: Introduzione a ASP.NET Core Blazor
author: guardrex
description: Inizia a usare Blazor compilando un'app Blazor con gli strumenti che preferisci.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/25/2019
uid: blazor/get-started
ms.openlocfilehash: ef9113dbfdbbd5920c4358cdac0c77c60f40b7c8
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288792"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="6c969-103">Introduzione a ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="6c969-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="6c969-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6c969-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="6c969-105">Introduzione a Blazor:</span><span class="sxs-lookup"><span data-stu-id="6c969-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="6c969-106">Installare la versione più recente di [.NET Core 3,0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) .</span><span class="sxs-lookup"><span data-stu-id="6c969-106">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="6c969-107">Installare il modello [Webassembly Blazor](xref:blazor/hosting-models#blazor-webassembly) eseguendo il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="6c969-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview9.19465.2
   ```

1. <span data-ttu-id="6c969-108">Seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="6c969-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c969-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c969-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="6c969-110">1 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-110">1\.</span></span> <span data-ttu-id="6c969-111">Installare la versione più recente di [Visual Studio](https://visualstudio.com/vs/) con il carico di lavoro **sviluppo di ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="6c969-111">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="6c969-112">2 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-112">2\.</span></span> <span data-ttu-id="6c969-113">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="6c969-113">Create a new project.</span></span>

   <span data-ttu-id="6c969-114">3 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-114">3\.</span></span> <span data-ttu-id="6c969-115">Selezionare **app Blazer**.</span><span class="sxs-lookup"><span data-stu-id="6c969-115">Select **Blazor App**.</span></span> <span data-ttu-id="6c969-116">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="6c969-116">Select **Next**.</span></span>

   <span data-ttu-id="6c969-117">4 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-117">4\.</span></span> <span data-ttu-id="6c969-118">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="6c969-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="6c969-119">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="6c969-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="6c969-120">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="6c969-120">Select **Create**.</span></span>

   <span data-ttu-id="6c969-121">5 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-121">5\.</span></span> <span data-ttu-id="6c969-122">Per un'esperienza di webassembly blazer, scegliere il modello **app Webassembly Blazer** .</span><span class="sxs-lookup"><span data-stu-id="6c969-122">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="6c969-123">Per un'esperienza del server blazer, scegliere il modello di **app del server Blazer** .</span><span class="sxs-lookup"><span data-stu-id="6c969-123">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="6c969-124">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="6c969-124">Select **Create**.</span></span> <span data-ttu-id="6c969-125">Per informazioni sui due modelli di hosting Blazor, *Server Blazor* e *webassembly Blazor*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6c969-125">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6c969-126">6 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-126">6\.</span></span> <span data-ttu-id="6c969-127">Premere **F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="6c969-127">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6c969-128">Se è stata installata l'estensione di Visual Studio Blazor per una versione di anteprima precedente di ASP.NET Core Blazor (anteprima 6 o precedente), è possibile disinstallare l'estensione.</span><span class="sxs-lookup"><span data-stu-id="6c969-128">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="6c969-129">L'installazione dei modelli di Blazor in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6c969-129">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6c969-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6c969-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="6c969-131">1 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-131">1\.</span></span> <span data-ttu-id="6c969-132">Installare [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="6c969-132">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="6c969-133">2 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-133">2\.</span></span> <span data-ttu-id="6c969-134">Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="6c969-134">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="6c969-135">3 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-135">3\.</span></span> <span data-ttu-id="6c969-136">Per un'esperienza di webassembly blazer, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="6c969-136">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="6c969-137">Per un'esperienza del server Blazor, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="6c969-137">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="6c969-138">Per informazioni sui due modelli di hosting Blazor, *Server Blazor* e *webassembly Blazor*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6c969-138">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6c969-139">4 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-139">4\.</span></span> <span data-ttu-id="6c969-140">Aprire la cartella *WebApplication1* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6c969-140">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="6c969-141">5 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-141">5\.</span></span> <span data-ttu-id="6c969-142">Per un progetto server blazer, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="6c969-142">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="6c969-143">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="6c969-143">Select **Yes**.</span></span>

   <span data-ttu-id="6c969-144">6 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-144">6\.</span></span> <span data-ttu-id="6c969-145">Se si usa un'app del server blazer, eseguire l'app usando il debugger Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6c969-145">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="6c969-146">Se si usa un'app webassembly blazer, eseguire `dotnet run` dalla cartella del progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="6c969-146">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="6c969-147">7 \.</span><span class="sxs-lookup"><span data-stu-id="6c969-147">7\.</span></span> <span data-ttu-id="6c969-148">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6c969-148">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6c969-149">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="6c969-149">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="6c969-150">Per un'esperienza di webassembly Blazor, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="6c969-150">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="6c969-151">Per un'esperienza del server Blazor, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="6c969-151">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="6c969-152">Per informazioni sui due modelli di hosting Blazor, *Server Blazor* e *webassembly Blazor*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6c969-152">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6c969-153">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6c969-153">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="6c969-154">Nelle schede della barra laterale sono disponibili più pagine:</span><span class="sxs-lookup"><span data-stu-id="6c969-154">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="6c969-155">Home</span><span class="sxs-lookup"><span data-stu-id="6c969-155">Home</span></span>
* <span data-ttu-id="6c969-156">Counter</span><span class="sxs-lookup"><span data-stu-id="6c969-156">Counter</span></span>
* <span data-ttu-id="6c969-157">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="6c969-157">Fetch data</span></span>

<span data-ttu-id="6c969-158">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="6c969-158">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="6c969-159">Per incrementare un contatore in una pagina Web è in genere necessario scrivere JavaScript, ma con blazer C#è possibile usare.</span><span class="sxs-lookup"><span data-stu-id="6c969-159">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="6c969-160">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="6c969-160">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="6c969-161">Una richiesta per `/counter` nel browser, come specificato dalla direttiva `@page` nella parte superiore, causa il rendering del contenuto da parte del componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="6c969-161">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="6c969-162">I componenti eseguono il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="6c969-162">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="6c969-163">Ogni volta che viene selezionato il pulsante **Click me** :</span><span class="sxs-lookup"><span data-stu-id="6c969-163">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="6c969-164">Viene generato l'evento `onclick`.</span><span class="sxs-lookup"><span data-stu-id="6c969-164">The `onclick` event is fired.</span></span>
* <span data-ttu-id="6c969-165">Viene chiamato il metodo `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="6c969-165">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="6c969-166">Il `currentCount` viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="6c969-166">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="6c969-167">Il rendering del componente viene eseguito nuovamente.</span><span class="sxs-lookup"><span data-stu-id="6c969-167">The component is rendered again.</span></span>

<span data-ttu-id="6c969-168">Il runtime confronta il nuovo contenuto con il contenuto precedente e applica solo il contenuto modificato al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="6c969-168">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="6c969-169">Aggiungere un componente a un altro componente usando la sintassi HTML.</span><span class="sxs-lookup"><span data-stu-id="6c969-169">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="6c969-170">Ad esempio, aggiungere il componente `Counter` alla Home page dell'app aggiungendo un elemento `<Counter />` al componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="6c969-170">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="6c969-171">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="6c969-171">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="6c969-172">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="6c969-172">Run the app.</span></span> <span data-ttu-id="6c969-173">Il contatore della Home page è fornito dal componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="6c969-173">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="6c969-174">I parametri del componente vengono specificati utilizzando attributi o [contenuto figlio](xref:blazor/components#child-content), che consentono di impostare le proprietà per il componente figlio.</span><span class="sxs-lookup"><span data-stu-id="6c969-174">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="6c969-175">Per aggiungere un parametro al componente `Counter`, aggiornare il blocco `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="6c969-175">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="6c969-176">Aggiungere una proprietà pubblica per `IncrementAmount` con un attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="6c969-176">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="6c969-177">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="6c969-177">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="6c969-178">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="6c969-178">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="6c969-179">Specificare il `IncrementAmount` nell'elemento `<Counter>` del componente `Index` usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="6c969-179">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="6c969-180">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="6c969-180">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="6c969-181">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="6c969-181">Run the app.</span></span> <span data-ttu-id="6c969-182">Il componente `Index` ha il proprio contatore che viene incrementato di dieci ogni volta che viene selezionato il pulsante **Click me** .</span><span class="sxs-lookup"><span data-stu-id="6c969-182">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="6c969-183">Il componente `Counter` (*Counter. Razor*) in `/counter` continua a incrementare di uno.</span><span class="sxs-lookup"><span data-stu-id="6c969-183">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c969-184">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6c969-184">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="6c969-185">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6c969-185">Additional resources</span></span>

* <xref:signalr/introduction>
