---
title: Introduzione a Blazor
author: guardrex
description: Informazioni su come iniziare a utilizzare Blazor creando e modificando un progetto Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: 667c57d536450fa2f8ae1cabc7c5a76a16d38a55
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665587"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="39de2-103">Introduzione a Blazor</span><span class="sxs-lookup"><span data-stu-id="39de2-103">Get started with Blazor</span></span>

<span data-ttu-id="39de2-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="39de2-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="39de2-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39de2-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="39de2-106">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="39de2-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="39de2-107">Per creare il primo progetto Blazor in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="39de2-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="39de2-108">Installare la versione più recente [estensione servizi di linguaggio Blazor](https://go.microsoft.com/fwlink/?linkid=870389) da Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="39de2-108">Install the latest [Blazor Language Services extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="39de2-109">Questo passaggio rende disponibili i modelli Blazor a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39de2-109">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="39de2-110">Rendere disponibili per l'uso con la CLI di .NET Core i modelli Blazor eseguendo il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="39de2-110">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="39de2-111">Selezionare **File** > **nuovo progetto** > **Web** > **applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="39de2-111">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="39de2-112">Assicurarsi che **.NET Core** e **ASP.NET Core 3.0** sono selezionate nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="39de2-112">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="39de2-113">Scegliere il modello **Blazor** e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="39de2-113">Choose the **Blazor** template and select **OK**.</span></span>
1. <span data-ttu-id="39de2-114">Premere **F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="39de2-114">Press **F5** to run the app.</span></span>

<span data-ttu-id="39de2-115">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="39de2-115">Congratulations!</span></span> <span data-ttu-id="39de2-116">Sono stati eseguiti la prima app Blazor!</span><span class="sxs-lookup"><span data-stu-id="39de2-116">You just ran your first Blazor app!</span></span>

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="39de2-117">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="39de2-117">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="39de2-118">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="39de2-118">Prerequisites:</span></span>

* [<span data-ttu-id="39de2-119">.NET Core SDK 3.0 Preview</span><span class="sxs-lookup"><span data-stu-id="39de2-119">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="39de2-120">Aggiungere i modelli Blazor eseguendo il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="39de2-120">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="39de2-121">Creare il primo progetto Blazor in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="39de2-121">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="39de2-122">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="39de2-122">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="39de2-123">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="39de2-123">Congratulations!</span></span> <span data-ttu-id="39de2-124">Sono stati eseguiti la prima app Blazor!</span><span class="sxs-lookup"><span data-stu-id="39de2-124">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="39de2-125">Progetto Blazor</span><span class="sxs-lookup"><span data-stu-id="39de2-125">Blazor project</span></span>

<span data-ttu-id="39de2-126">Quando si esegue l'app, più pagine sono disponibili le schede nella barra laterale:</span><span class="sxs-lookup"><span data-stu-id="39de2-126">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="39de2-127">Home</span><span class="sxs-lookup"><span data-stu-id="39de2-127">Home</span></span>
* <span data-ttu-id="39de2-128">Counter</span><span class="sxs-lookup"><span data-stu-id="39de2-128">Counter</span></span>
* <span data-ttu-id="39de2-129">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="39de2-129">Fetch data</span></span>

<span data-ttu-id="39de2-130">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="39de2-130">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="39de2-131">Incrementare un contatore in una pagina Web in genere richiede la scrittura di JavaScript, ma Blazor offre un approccio migliore usando C#.</span><span class="sxs-lookup"><span data-stu-id="39de2-131">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="39de2-132">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="39de2-132">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="39de2-133">Una richiesta per `/counter` nel browser, come specificato da di `@page` direttiva all'inizio, fa sì che il componente di contatore per il rendering del relativo contenuto.</span><span class="sxs-lookup"><span data-stu-id="39de2-133">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="39de2-134">Componenti di eseguire il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="39de2-134">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="39de2-135">Ogni volta che il **Click me** pulsante è selezionato:</span><span class="sxs-lookup"><span data-stu-id="39de2-135">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="39de2-136">Il `onclick` evento.</span><span class="sxs-lookup"><span data-stu-id="39de2-136">The `onclick` event is fired.</span></span>
* <span data-ttu-id="39de2-137">Viene chiamato il metodo `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="39de2-137">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="39de2-138">Il `currentCount` viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="39de2-138">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="39de2-139">Il componente viene eseguito il rendering nuovamente.</span><span class="sxs-lookup"><span data-stu-id="39de2-139">The component is rendered again.</span></span>

<span data-ttu-id="39de2-140">Il runtime consente di confrontare il nuovo contenuto per il contenuto precedente e si applica solo il contenuto modificato per il provider di servizi Internet (DOM, Document Object Model).</span><span class="sxs-lookup"><span data-stu-id="39de2-140">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="39de2-141">Aggiungere un componente a un altro componente usando una sintassi simile all'HTML.</span><span class="sxs-lookup"><span data-stu-id="39de2-141">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="39de2-142">Parametri del componente vengono specificati utilizzando gli attributi o contenuto figlio.</span><span class="sxs-lookup"><span data-stu-id="39de2-142">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="39de2-143">Ad esempio, un componente del contatore può essere aggiunto alla home page dell'app aggiungendo un `<Counter />` elemento per il componente dell'indice.</span><span class="sxs-lookup"><span data-stu-id="39de2-143">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="39de2-144">Nelle *cshtml*, sostituire il messaggio di richiesta di sondaggio componente con un componente del contatore:</span><span class="sxs-lookup"><span data-stu-id="39de2-144">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="39de2-145">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="39de2-145">Run the app.</span></span> <span data-ttu-id="39de2-146">Home page ha un proprio contatore.</span><span class="sxs-lookup"><span data-stu-id="39de2-146">The homepage has its own counter.</span></span>

<span data-ttu-id="39de2-147">Per aggiungere un parametro per il componente del contatore, aggiornare il componente `@functions` blocco:</span><span class="sxs-lookup"><span data-stu-id="39de2-147">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="39de2-148">Aggiungere una proprietà per `IncrementAmount` decorata con il `[Parameter]` attributo.</span><span class="sxs-lookup"><span data-stu-id="39de2-148">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="39de2-149">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="39de2-149">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="39de2-150">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="39de2-150">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="39de2-151">Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Home usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="39de2-151">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="39de2-152">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="39de2-152">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="39de2-153">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="39de2-153">Run the app.</span></span> <span data-ttu-id="39de2-154">Home page ha un proprio contatore che viene incrementato di dieci ogni volta che il **Click me** pulsante è selezionato.</span><span class="sxs-lookup"><span data-stu-id="39de2-154">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39de2-155">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="39de2-155">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
