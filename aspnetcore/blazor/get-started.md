---
title: Introduzione a ASP.NET Core Blazor
author: guardrex
description: Inizia a usare Blazor compilando un'app Blazor con gli strumenti che preferisci.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 9ebeb57d2fad7e4c288d61a46911f2bf64cac2fb
ms.sourcegitcommit: f3b1bcfd108e5d53f73abc0bf2555890869d953b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80320936"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="4092e-103">Introduzione a ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="4092e-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="4092e-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4092e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="4092e-105">Per iniziare a usare blazer, seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="4092e-105">To get started with Blazor, follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4092e-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4092e-106">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="4092e-107">Per creare app del server blazer, installare [Visual Studio 2019 versione 16,4 o successiva](https://visualstudio.microsoft.com/vs/preview/) con il carico di lavoro di **sviluppo ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="4092e-107">To create Blazor Server apps, install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="4092e-108">Per creare app di webassembly per server e blazer, installare Visual Studio 2019 16,6 Preview 2 o versione successiva con il carico di lavoro **sviluppo di ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="4092e-108">To create Blazor Server and Blazor WebAssembly apps, install Visual Studio 2019 16.6 Preview 2 or later with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="4092e-109">Per informazioni sui due modelli di hosting blazer, *Blazer webassembly* e *Blazer Server*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4092e-109">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="4092e-110">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="4092e-110">Create a new project.</span></span>

1. <span data-ttu-id="4092e-111">Selezionare **app Blazer**.</span><span class="sxs-lookup"><span data-stu-id="4092e-111">Select **Blazor App**.</span></span> <span data-ttu-id="4092e-112">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="4092e-112">Select **Next**.</span></span>

1. <span data-ttu-id="4092e-113">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="4092e-113">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="4092e-114">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="4092e-114">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="4092e-115">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4092e-115">Select **Create**.</span></span>

1. <span data-ttu-id="4092e-116">Per un'esperienza di webassembly blazer (Visual Studio 16,6 Preview 2 o versione successiva), scegliere il modello di **app Webassembly Blazer** .</span><span class="sxs-lookup"><span data-stu-id="4092e-116">For a Blazor WebAssembly experience (Visual Studio 16.6 Preview 2 or later), choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="4092e-117">Per un'esperienza del server blazer (Visual Studio 16,4 o versione successiva), scegliere il modello di **app del server Blazer** .</span><span class="sxs-lookup"><span data-stu-id="4092e-117">For a Blazor Server experience (Visual Studio 16.4 or later), choose the **Blazor Server App** template.</span></span> <span data-ttu-id="4092e-118">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4092e-118">Select **Create**.</span></span>

1. <span data-ttu-id="4092e-119">Premere <kbd>CTRL</kbd>+<kbd>F5</kbd> per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4092e-119">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="4092e-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4092e-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="4092e-121">Installare [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="4092e-121">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="4092e-122">Facoltativamente, installare il modello di anteprima di [Webassembly Blazer](xref:blazor/hosting-models#blazor-webassembly) eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="4092e-122">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="4092e-123">Per usare il modello di assembly webassembly 3,2 Preview 3 blazer è **necessario** il [.NET Core SDK versione 3.1.201 o successiva](https://dotnet.microsoft.com/download/dotnet-core/3.1) .</span><span class="sxs-lookup"><span data-stu-id="4092e-123">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="4092e-124">Verificare la versione di .NET Core SDK installata eseguendo `dotnet --version` in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="4092e-124">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="4092e-125">Installare [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="4092e-125">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="4092e-126">Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) e il [debugger JavaScript (notturno)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) con `debug.javascript.usePreview` impostato su `true`.</span><span class="sxs-lookup"><span data-stu-id="4092e-126">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

1. <span data-ttu-id="4092e-127">Per un'esperienza del server Blazor, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="4092e-127">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="4092e-128">Per un'esperienza di webassembly Blazor, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="4092e-128">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="4092e-129">Per informazioni sui due modelli di hosting blazer, *Server Blazer* e *webassembly Blazer*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4092e-129">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="4092e-130">Aprire la cartella *WebApplication1* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4092e-130">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="4092e-131">Le richieste dell'IDE aggiungono risorse per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="4092e-131">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="4092e-132">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="4092e-132">Select **Yes**.</span></span>

1. <span data-ttu-id="4092e-133">Eseguire l'app usando il debugger Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4092e-133">Run the app using the Visual Studio Code debugger.</span></span>

1. <span data-ttu-id="4092e-134">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4092e-134">In a browser, navigate to `https://localhost:5001`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="4092e-135">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="4092e-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="4092e-136">Il server blazer è supportato in Visual Studio per Mac.</span><span class="sxs-lookup"><span data-stu-id="4092e-136">Blazor Server is supported in Visual Studio for Mac.</span></span> <span data-ttu-id="4092e-137">Il webassembly Blazer non è supportato in questo momento.</span><span class="sxs-lookup"><span data-stu-id="4092e-137">Blazor WebAssembly isn't supported at this time.</span></span> <span data-ttu-id="4092e-138">Per compilare app webassembly di Blazer in macOS, seguire le istruzioni nella scheda **interfaccia della riga di comando di .NET Core** .</span><span class="sxs-lookup"><span data-stu-id="4092e-138">To build Blazor WebAssembly apps on macOS, follow the guidance on the **.NET Core CLI** tab.</span></span>

1. <span data-ttu-id="4092e-139">Installare [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="4092e-139">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="4092e-140">Selezionare **File** > **nuova soluzione** o creare un **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="4092e-140">Select **File** > **New Solution** or create a **New Project**.</span></span>

1. <span data-ttu-id="4092e-141">Nella barra laterale selezionare **.NET Core** > **app**.</span><span class="sxs-lookup"><span data-stu-id="4092e-141">In the sidebar, select **.NET Core** > **App**.</span></span>

1. <span data-ttu-id="4092e-142">Selezionare il modello **applicazione server Blazer** .</span><span class="sxs-lookup"><span data-stu-id="4092e-142">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="4092e-143">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4092e-143">Select **Create**.</span></span>

   <span data-ttu-id="4092e-144">Per informazioni sul modello di hosting del server blazer, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4092e-144">For information on the Blazor Server hosting model, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="4092e-145">Impostare il **Framework di destinazione** su **.NET Core 3,1** e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="4092e-145">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

1. <span data-ttu-id="4092e-146">Nel campo **nome progetto** assegnare un nome all'app `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="4092e-146">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="4092e-147">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4092e-147">Select **Create**.</span></span>

1. <span data-ttu-id="4092e-148">Selezionare **esegui** > **Esegui senza eseguire debug** per eseguire l'app *senza il debugger*.</span><span class="sxs-lookup"><span data-stu-id="4092e-148">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="4092e-149">Eseguire l'app con **Avvia debug** per eseguire l'app *con il debugger*.</span><span class="sxs-lookup"><span data-stu-id="4092e-149">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

<span data-ttu-id="4092e-150">Se viene visualizzato un messaggio per considerare attendibile il certificato di sviluppo, considerare attendibile il certificato e continuare.</span><span class="sxs-lookup"><span data-stu-id="4092e-150">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="4092e-151">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="4092e-151">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="4092e-152">Installare [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="4092e-152">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="4092e-153">Facoltativamente, installare il modello di anteprima di [Webassembly Blazer](xref:blazor/hosting-models#blazor-webassembly) eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="4092e-153">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="4092e-154">Per usare il modello di assembly webassembly 3,2 Preview 3 blazer è **necessario** il [.NET Core SDK versione 3.1.201 o successiva](https://dotnet.microsoft.com/download/dotnet-core/3.1) .</span><span class="sxs-lookup"><span data-stu-id="4092e-154">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="4092e-155">Verificare la versione di .NET Core SDK installata eseguendo `dotnet --version` in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="4092e-155">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="4092e-156">Per un'esperienza del server Blazor, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="4092e-156">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="4092e-157">Per un'esperienza di webassembly Blazor, eseguire i comandi seguenti in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="4092e-157">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="4092e-158">Per informazioni sui due modelli di hosting blazer, *Server Blazer* e *webassembly Blazer*, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4092e-158">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="4092e-159">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4092e-159">In a browser, navigate to `https://localhost:5001`.</span></span>

---

<span data-ttu-id="4092e-160">Nelle schede della barra laterale sono disponibili più pagine:</span><span class="sxs-lookup"><span data-stu-id="4092e-160">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="4092e-161">Home</span><span class="sxs-lookup"><span data-stu-id="4092e-161">Home</span></span>
* <span data-ttu-id="4092e-162">Contatore</span><span class="sxs-lookup"><span data-stu-id="4092e-162">Counter</span></span>
* <span data-ttu-id="4092e-163">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="4092e-163">Fetch data</span></span>

<span data-ttu-id="4092e-164">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="4092e-164">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="4092e-165">Per incrementare un contatore in una pagina Web è in genere necessario scrivere JavaScript, ma con Blazor C#è possibile utilizzare.</span><span class="sxs-lookup"><span data-stu-id="4092e-165">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="4092e-166">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="4092e-166">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="4092e-167">Una richiesta di `/counter` nel browser, come specificato dalla direttiva `@page` nella parte superiore, fa sì che il componente `Counter` esegua il rendering del contenuto.</span><span class="sxs-lookup"><span data-stu-id="4092e-167">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="4092e-168">I componenti eseguono il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="4092e-168">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="4092e-169">Ogni volta che viene selezionato il pulsante **Click me** :</span><span class="sxs-lookup"><span data-stu-id="4092e-169">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="4092e-170">Viene generato l'evento `onclick`.</span><span class="sxs-lookup"><span data-stu-id="4092e-170">The `onclick` event is fired.</span></span>
* <span data-ttu-id="4092e-171">Viene chiamato il metodo `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="4092e-171">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="4092e-172">Il `currentCount` viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="4092e-172">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="4092e-173">Il rendering del componente viene eseguito nuovamente.</span><span class="sxs-lookup"><span data-stu-id="4092e-173">The component is rendered again.</span></span>

<span data-ttu-id="4092e-174">Il runtime confronta il nuovo contenuto con il contenuto precedente e applica solo il contenuto modificato al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="4092e-174">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="4092e-175">Aggiungere un componente a un altro componente usando la sintassi HTML.</span><span class="sxs-lookup"><span data-stu-id="4092e-175">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="4092e-176">Ad esempio, aggiungere il componente `Counter` alla Home page dell'app aggiungendo un elemento `<Counter />` al componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="4092e-176">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="4092e-177">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="4092e-177">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="4092e-178">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4092e-178">Run the app.</span></span> <span data-ttu-id="4092e-179">La Home page presenta il proprio contatore fornito dal componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="4092e-179">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="4092e-180">I parametri del componente vengono specificati utilizzando attributi o [contenuto figlio](xref:blazor/components#child-content), che consentono di impostare le proprietà per il componente figlio.</span><span class="sxs-lookup"><span data-stu-id="4092e-180">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="4092e-181">Per aggiungere un parametro al componente `Counter`, aggiornare il blocco `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="4092e-181">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="4092e-182">Aggiungere una proprietà pubblica per `IncrementAmount` con un attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="4092e-182">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="4092e-183">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="4092e-183">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="4092e-184">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="4092e-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="4092e-185">Specificare il `IncrementAmount` nell'elemento `<Counter>` del componente di `Index` utilizzando un attributo.</span><span class="sxs-lookup"><span data-stu-id="4092e-185">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="4092e-186">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="4092e-186">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="4092e-187">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4092e-187">Run the app.</span></span> <span data-ttu-id="4092e-188">Il componente `Index` dispone di un contatore che aumenta di dieci ogni volta che viene selezionato il pulsante **Click me** .</span><span class="sxs-lookup"><span data-stu-id="4092e-188">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="4092e-189">Il componente `Counter` (*Counter. Razor*) in `/counter` continua a incrementare di uno.</span><span class="sxs-lookup"><span data-stu-id="4092e-189">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4092e-190">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="4092e-190">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="4092e-191">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4092e-191">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
