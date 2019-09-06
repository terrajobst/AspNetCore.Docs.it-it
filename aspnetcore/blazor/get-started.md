---
title: Introduzione a ASP.NET Core Blazer
author: guardrex
description: Inizia a usare Blazer compilando un'app Blazer con gli strumenti che preferisci.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/get-started
ms.openlocfilehash: 030c289e072efad43d4f6cdd63cba07cc623a090
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310430"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="7021b-103">Introduzione a ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="7021b-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="7021b-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7021b-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7021b-105">Introduzione a Blazer:</span><span class="sxs-lookup"><span data-stu-id="7021b-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="7021b-106">Installare la versione più recente di [.NET Core 3,0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) .</span><span class="sxs-lookup"><span data-stu-id="7021b-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="7021b-107">Installare i modelli Blazer eseguendo il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="7021b-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview9.19424.4
   ```

1. <span data-ttu-id="7021b-108">Seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="7021b-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7021b-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7021b-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="7021b-110">1 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-110">1\.</span></span> <span data-ttu-id="7021b-111">Installare la versione più recente di [Visual Studio Preview](https://visualstudio.com/vs/preview) con il carico di lavoro **sviluppo di ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="7021b-111">Install the latest [Visual Studio preview](https://visualstudio.com/vs/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="7021b-112">2 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-112">2\.</span></span> <span data-ttu-id="7021b-113">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="7021b-113">Create a new project.</span></span>

   <span data-ttu-id="7021b-114">3 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-114">3\.</span></span> <span data-ttu-id="7021b-115">Selezionare **app Blazer**.</span><span class="sxs-lookup"><span data-stu-id="7021b-115">Select **Blazor App**.</span></span> <span data-ttu-id="7021b-116">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="7021b-116">Select **Next**.</span></span>

   <span data-ttu-id="7021b-117">4 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-117">4\.</span></span> <span data-ttu-id="7021b-118">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="7021b-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="7021b-119">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="7021b-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="7021b-120">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="7021b-120">Select **Create**.</span></span>

   <span data-ttu-id="7021b-121">5 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-121">5\.</span></span> <span data-ttu-id="7021b-122">Per un'esperienza del lato client blazer, scegliere il modello **app Webassembly Blazer** .</span><span class="sxs-lookup"><span data-stu-id="7021b-122">For a Blazor client-side experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="7021b-123">Per un'esperienza sul lato server di Blazer, scegliere il modello di **app del server Blazer** .</span><span class="sxs-lookup"><span data-stu-id="7021b-123">For a Blazor server-side experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="7021b-124">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="7021b-124">Select **Create**.</span></span> <span data-ttu-id="7021b-125">Per informazioni sui due modelli di hosting blazer, lato server e lato client, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="7021b-125">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="7021b-126">6 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-126">6\.</span></span> <span data-ttu-id="7021b-127">Premere **F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="7021b-127">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="7021b-128">Se è stata installata l'estensione di Visual Studio blazer per una versione di anteprima precedente di ASP.NET Core blazer (anteprima 6 o precedente), è possibile disinstallare l'estensione.</span><span class="sxs-lookup"><span data-stu-id="7021b-128">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="7021b-129">L'installazione dei modelli di Blazer in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7021b-129">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7021b-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7021b-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="7021b-131">1 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-131">1\.</span></span> <span data-ttu-id="7021b-132">Installare [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="7021b-132">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="7021b-133">2 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-133">2\.</span></span> <span data-ttu-id="7021b-134">Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="7021b-134">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="7021b-135">3 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-135">3\.</span></span> <span data-ttu-id="7021b-136">Per un'esperienza del lato client di Blazer, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="7021b-136">For a Blazor client-side experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="7021b-137">Per un'esperienza sul lato server di Blaze, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="7021b-137">For a Blazor server-side experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="7021b-138">Per informazioni sui due modelli di hosting blazer, lato server e lato client, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="7021b-138">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="7021b-139">4 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-139">4\.</span></span> <span data-ttu-id="7021b-140">Aprire la cartella *WebApplication1* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7021b-140">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="7021b-141">5 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-141">5\.</span></span> <span data-ttu-id="7021b-142">Per un progetto Blazer sul lato server, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="7021b-142">For a Blazor server-side project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="7021b-143">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="7021b-143">Select **Yes**.</span></span>

   <span data-ttu-id="7021b-144">6 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-144">6\.</span></span> <span data-ttu-id="7021b-145">Se si usa un'app del lato server blazer, eseguire l'app usando il debugger Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7021b-145">If using a Blazor server-side app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="7021b-146">Se si usa un'app del lato client blazer, `dotnet run` eseguire dalla cartella del progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="7021b-146">If using a Blazor client-side app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="7021b-147">7 \.</span><span class="sxs-lookup"><span data-stu-id="7021b-147">7\.</span></span> <span data-ttu-id="7021b-148">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7021b-148">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **Blazor Server App** template. For a Blazor client-side experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7021b-149">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="7021b-149">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="7021b-150">Per un'esperienza del lato client di Blazer, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="7021b-150">For a Blazor client-side experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="7021b-151">Per un'esperienza sul lato server di Blazer, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="7021b-151">For a Blazor server-side experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="7021b-152">Per informazioni sui due modelli di hosting blazer, lato server e lato client, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="7021b-152">For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="7021b-153">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7021b-153">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="7021b-154">Nelle schede della barra laterale sono disponibili più pagine:</span><span class="sxs-lookup"><span data-stu-id="7021b-154">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="7021b-155">Home</span><span class="sxs-lookup"><span data-stu-id="7021b-155">Home</span></span>
* <span data-ttu-id="7021b-156">Counter</span><span class="sxs-lookup"><span data-stu-id="7021b-156">Counter</span></span>
* <span data-ttu-id="7021b-157">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="7021b-157">Fetch data</span></span>

<span data-ttu-id="7021b-158">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="7021b-158">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="7021b-159">Per incrementare un contatore in una pagina Web è in genere necessario scrivere JavaScript, ma i componenti Razor offrono C#un approccio migliore con.</span><span class="sxs-lookup"><span data-stu-id="7021b-159">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="7021b-160">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="7021b-160">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="7021b-161">Una richiesta `/counter` di nel browser, come specificato `@page` dalla direttiva nella parte superiore, determina il rendering del `Counter` contenuto da parte del componente.</span><span class="sxs-lookup"><span data-stu-id="7021b-161">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="7021b-162">I componenti eseguono il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="7021b-162">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="7021b-163">Ogni volta che viene selezionato il pulsante **Click me** :</span><span class="sxs-lookup"><span data-stu-id="7021b-163">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="7021b-164">L' `onclick` evento viene generato.</span><span class="sxs-lookup"><span data-stu-id="7021b-164">The `onclick` event is fired.</span></span>
* <span data-ttu-id="7021b-165">Viene chiamato il metodo `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="7021b-165">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="7021b-166">`currentCount` Viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="7021b-166">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="7021b-167">Il rendering del componente viene eseguito nuovamente.</span><span class="sxs-lookup"><span data-stu-id="7021b-167">The component is rendered again.</span></span>

<span data-ttu-id="7021b-168">Il runtime confronta il nuovo contenuto con il contenuto precedente e applica solo il contenuto modificato al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="7021b-168">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="7021b-169">Aggiungere un componente a un altro componente usando la sintassi HTML.</span><span class="sxs-lookup"><span data-stu-id="7021b-169">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="7021b-170">Ad esempio, aggiungere il `Counter` componente alla Home page dell'app aggiungendo un `<Counter />` elemento al `Index` componente.</span><span class="sxs-lookup"><span data-stu-id="7021b-170">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="7021b-171">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="7021b-171">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="7021b-172">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="7021b-172">Run the app.</span></span> <span data-ttu-id="7021b-173">La Home page presenta il proprio contatore fornito dal `Counter` componente.</span><span class="sxs-lookup"><span data-stu-id="7021b-173">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="7021b-174">I parametri del componente vengono specificati utilizzando attributi o [contenuto figlio](xref:blazor/components#child-content), che consentono di impostare le proprietà per il componente figlio.</span><span class="sxs-lookup"><span data-stu-id="7021b-174">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="7021b-175">Per aggiungere un parametro al `Counter` componente, aggiornare il `@code` blocco del componente:</span><span class="sxs-lookup"><span data-stu-id="7021b-175">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="7021b-176">Aggiungere una proprietà pubblica per `IncrementAmount` con un `[Parameter]` attributo.</span><span class="sxs-lookup"><span data-stu-id="7021b-176">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="7021b-177">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="7021b-177">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="7021b-178">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="7021b-178">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="7021b-179">`IncrementAmount` Specificare `<Counter>` nell'elemento del componente usando un attributo. `Index`</span><span class="sxs-lookup"><span data-stu-id="7021b-179">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="7021b-180">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="7021b-180">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="7021b-181">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="7021b-181">Run the app.</span></span> <span data-ttu-id="7021b-182">Il `Index` componente dispone di un contatore specifico che aumenta di dieci ogni volta che viene selezionato il pulsante **Click me** .</span><span class="sxs-lookup"><span data-stu-id="7021b-182">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="7021b-183">Il `Counter` componente (*Counter. Razor*) a `/counter` continua a incrementare di uno.</span><span class="sxs-lookup"><span data-stu-id="7021b-183">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7021b-184">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="7021b-184">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="7021b-185">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7021b-185">Additional resources</span></span>

* <xref:signalr/introduction>
