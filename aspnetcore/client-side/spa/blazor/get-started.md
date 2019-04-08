---
title: Introduzione a Blazor
author: guardrex
description: Informazioni su come iniziare a utilizzare Blazor creando e modificando un progetto Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: spa/blazor/get-started
ms.openlocfilehash: b3928c2812be6f34cdf2f17295a1251106f651e5
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068235"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="1f0b3-103">Introduzione a Blazor</span><span class="sxs-lookup"><span data-stu-id="1f0b3-103">Get started with Blazor</span></span>

<span data-ttu-id="1f0b3-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1f0b3-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# [<a name="visual-studio"></a><span data-ttu-id="1f0b3-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f0b3-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1f0b3-106">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="1f0b3-107">Per creare il primo progetto Blazor in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="1f0b3-108">Installare la versione più recente [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="1f0b3-109">Consentire a Visual Studio usare l'anteprima di SDK:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="1f0b3-110">Aprire **degli strumenti** > **opzioni** nella barra dei menu.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="1f0b3-111">Aprire il **progetti e soluzioni** nodo.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="1f0b3-112">Aprire il **.NET Core** scheda.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="1f0b3-113">Selezionare la casella **utilizzano le anteprime di .NET Core SDK**.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="1f0b3-114">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-114">Select **OK**.</span></span>
1. <span data-ttu-id="1f0b3-115">Installare la versione più recente [Blazor estensione](https://go.microsoft.com/fwlink/?linkid=870389) da Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-115">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="1f0b3-116">Questo passaggio rende disponibili i modelli Blazor a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-116">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="1f0b3-117">Rendere disponibili per l'uso con la CLI di .NET Core i modelli Blazor eseguendo il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-117">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```
1. <span data-ttu-id="1f0b3-118">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-118">Create a new project.</span></span>
1. <span data-ttu-id="1f0b3-119">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="1f0b3-120">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-120">Select **Next**.</span></span>
1. <span data-ttu-id="1f0b3-121">Specificare un nome nella **nome progetto** campo.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-121">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="1f0b3-122">Verificare i **posizione** voce sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="1f0b3-123">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-123">Select **Create**.</span></span>
1. <span data-ttu-id="1f0b3-124">Assicurarsi che **.NET Core** e **ASP.NET Core 3.0** sono selezionate nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-124">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="1f0b3-125">Selezionare il **Blazor** modello e selezionare **crea**.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-125">Select the **Blazor** template and select **Create**.</span></span>
1. <span data-ttu-id="1f0b3-126">Premere **F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-126">Press **F5** to run the app.</span></span>

<span data-ttu-id="1f0b3-127">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-127">Congratulations!</span></span> <span data-ttu-id="1f0b3-128">Sono stati eseguiti la prima app Blazor!</span><span class="sxs-lookup"><span data-stu-id="1f0b3-128">You just ran your first Blazor app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="1f0b3-129">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="1f0b3-129">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="1f0b3-130">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-130">Prerequisites:</span></span>

* [<span data-ttu-id="1f0b3-131">.NET Core SDK 3.0 Preview</span><span class="sxs-lookup"><span data-stu-id="1f0b3-131">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="1f0b3-132">Aggiungere i modelli Blazor eseguendo il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-132">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="1f0b3-133">Creare il primo progetto Blazor in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-133">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="1f0b3-134">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-134">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="1f0b3-135">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-135">Congratulations!</span></span> <span data-ttu-id="1f0b3-136">Sono stati eseguiti la prima app Blazor!</span><span class="sxs-lookup"><span data-stu-id="1f0b3-136">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="1f0b3-137">Progetto Blazor</span><span class="sxs-lookup"><span data-stu-id="1f0b3-137">Blazor project</span></span>

<span data-ttu-id="1f0b3-138">Quando si esegue l'app, più pagine sono disponibili le schede nella barra laterale:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-138">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="1f0b3-139">Home</span><span class="sxs-lookup"><span data-stu-id="1f0b3-139">Home</span></span>
* <span data-ttu-id="1f0b3-140">Counter</span><span class="sxs-lookup"><span data-stu-id="1f0b3-140">Counter</span></span>
* <span data-ttu-id="1f0b3-141">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="1f0b3-141">Fetch data</span></span>

<span data-ttu-id="1f0b3-142">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-142">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="1f0b3-143">Incrementare un contatore in una pagina Web in genere richiede la scrittura di JavaScript, ma Blazor offre un approccio migliore usando C#.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-143">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="1f0b3-144">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-144">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="1f0b3-145">Una richiesta per `/counter` nel browser, come specificato da di `@page` direttiva all'inizio, fa sì che il componente di contatore per il rendering del relativo contenuto.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-145">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="1f0b3-146">Componenti di eseguire il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-146">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="1f0b3-147">Ogni volta che il **Click me** pulsante è selezionato:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-147">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="1f0b3-148">Il `onclick` evento.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-148">The `onclick` event is fired.</span></span>
* <span data-ttu-id="1f0b3-149">Viene chiamato il metodo `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="1f0b3-149">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="1f0b3-150">Il `currentCount` viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-150">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="1f0b3-151">Il componente viene eseguito il rendering nuovamente.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-151">The component is rendered again.</span></span>

<span data-ttu-id="1f0b3-152">Il runtime consente di confrontare il nuovo contenuto per il contenuto precedente e si applica solo il contenuto modificato per il provider di servizi Internet (DOM, Document Object Model).</span><span class="sxs-lookup"><span data-stu-id="1f0b3-152">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="1f0b3-153">Aggiungere un componente a un altro componente usando una sintassi simile all'HTML.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-153">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="1f0b3-154">Parametri del componente vengono specificati utilizzando gli attributi o contenuto figlio.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-154">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="1f0b3-155">Ad esempio, un componente del contatore può essere aggiunto alla home page dell'app aggiungendo un `<Counter />` elemento per il componente dell'indice.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-155">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="1f0b3-156">Nelle *cshtml*, sostituire il messaggio di richiesta di sondaggio componente con un componente del contatore:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-156">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="1f0b3-157">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-157">Run the app.</span></span> <span data-ttu-id="1f0b3-158">Home page ha un proprio contatore.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-158">The homepage has its own counter.</span></span>

<span data-ttu-id="1f0b3-159">Per aggiungere un parametro per il componente del contatore, aggiornare il componente `@functions` blocco:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-159">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="1f0b3-160">Aggiungere una proprietà per `IncrementAmount` decorata con il `[Parameter]` attributo.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-160">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="1f0b3-161">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-161">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="1f0b3-162">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-162">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="1f0b3-163">Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Home usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-163">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="1f0b3-164">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1f0b3-164">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="1f0b3-165">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-165">Run the app.</span></span> <span data-ttu-id="1f0b3-166">Home page ha un proprio contatore che viene incrementato di dieci ogni volta che il **Click me** pulsante è selezionato.</span><span class="sxs-lookup"><span data-stu-id="1f0b3-166">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f0b3-167">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="1f0b3-167">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
