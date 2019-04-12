---
title: Introduzione a Razor componenti
author: guardrex
description: Informazioni su come iniziare con i componenti di Razor creando e modificando un progetto Razor componenti.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/get-started
ms.openlocfilehash: 151e58497b0f22fa7c5a9bde1f665eeb73fd5dc3
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515530"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="cba87-103">Introduzione a Razor componenti</span><span class="sxs-lookup"><span data-stu-id="cba87-103">Get started with Razor Components</span></span>

<span data-ttu-id="cba87-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cba87-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="cba87-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cba87-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cba87-106">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="cba87-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="cba87-107">Per creare il primo progetto Razor componenti in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="cba87-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="cba87-108">Installare la versione più recente [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span><span class="sxs-lookup"><span data-stu-id="cba87-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="cba87-109">Consentire a Visual Studio usare l'anteprima di SDK:</span><span class="sxs-lookup"><span data-stu-id="cba87-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="cba87-110">Aprire **degli strumenti** > **opzioni** nella barra dei menu.</span><span class="sxs-lookup"><span data-stu-id="cba87-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="cba87-111">Aprire il **progetti e soluzioni** nodo.</span><span class="sxs-lookup"><span data-stu-id="cba87-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="cba87-112">Aprire il **.NET Core** scheda.</span><span class="sxs-lookup"><span data-stu-id="cba87-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="cba87-113">Selezionare la casella **utilizzano le anteprime di .NET Core SDK**.</span><span class="sxs-lookup"><span data-stu-id="cba87-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="cba87-114">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="cba87-114">Select **OK**.</span></span>
1. <span data-ttu-id="cba87-115">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="cba87-115">Create a new project.</span></span>
1. <span data-ttu-id="cba87-116">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="cba87-116">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="cba87-117">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="cba87-117">Select **Next**.</span></span>
1. <span data-ttu-id="cba87-118">Specificare un nome nella **nome progetto** campo.</span><span class="sxs-lookup"><span data-stu-id="cba87-118">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="cba87-119">Verificare i **posizione** voce sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="cba87-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="cba87-120">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="cba87-120">Select **Create**.</span></span>
1. <span data-ttu-id="cba87-121">Assicurarsi che **.NET Core** e **ASP.NET Core 3.0** sono selezionate nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="cba87-121">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="cba87-122">Scegliere il **componenti Razor** modello e selezionare **crea**.</span><span class="sxs-lookup"><span data-stu-id="cba87-122">Choose the **Razor Components** template and select **Create**.</span></span>
1. <span data-ttu-id="cba87-123">Premere **F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="cba87-123">Press **F5** to run the app.</span></span>

<span data-ttu-id="cba87-124">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="cba87-124">Congratulations!</span></span> <span data-ttu-id="cba87-125">Sono stati eseguiti la prima app Razor componenti!</span><span class="sxs-lookup"><span data-stu-id="cba87-125">You just ran your first Razor Components app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="cba87-126">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="cba87-126">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="cba87-127">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="cba87-127">Prerequisites:</span></span>

* [<span data-ttu-id="cba87-128">.NET Core SDK 3.0 Preview</span><span class="sxs-lookup"><span data-stu-id="cba87-128">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="cba87-129">Per creare il primo progetto di componenti Razor da una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="cba87-129">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="cba87-130">In un browser passare a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="cba87-130">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="cba87-131">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="cba87-131">Congratulations!</span></span> <span data-ttu-id="cba87-132">Sono stati eseguiti la prima app Razor componenti!</span><span class="sxs-lookup"><span data-stu-id="cba87-132">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="cba87-133">Progetti di componenti Razor</span><span class="sxs-lookup"><span data-stu-id="cba87-133">Razor Components project</span></span>

<span data-ttu-id="cba87-134">Componenti di Razor vengono creati utilizzando la sintassi Razor, ma vengono compilati in modo diverso da visualizzazioni MVC e Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cba87-134">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="cba87-135">Il *.razor* estensione di file viene usato per specificare un componente di Razor.</span><span class="sxs-lookup"><span data-stu-id="cba87-135">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="cba87-136">Razor Pages ed MVC viste continuano a usare il *cshtml* estensione di file.</span><span class="sxs-lookup"><span data-stu-id="cba87-136">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="cba87-137">Razor componenti possono essere creati con la *. cshtml* estensione di file, purché tali file vengono identificati come i file Razor componente utilizzando il `_RazorComponentInclude` proprietà MSBuild.</span><span class="sxs-lookup"><span data-stu-id="cba87-137">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="cba87-138">Ad esempio, un'app creata usando il modello del componente Razor specifica che tutti i *. cshtml* file sotto il *componenti* cartella deve essere trattata come componenti Razor:</span><span class="sxs-lookup"><span data-stu-id="cba87-138">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="cba87-139">Quando si esegue l'app, più pagine sono disponibili le schede nella barra laterale:</span><span class="sxs-lookup"><span data-stu-id="cba87-139">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="cba87-140">Home</span><span class="sxs-lookup"><span data-stu-id="cba87-140">Home</span></span>
* <span data-ttu-id="cba87-141">Counter</span><span class="sxs-lookup"><span data-stu-id="cba87-141">Counter</span></span>
* <span data-ttu-id="cba87-142">Recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="cba87-142">Fetch data</span></span>

<span data-ttu-id="cba87-143">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="cba87-143">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="cba87-144">Per incrementare un contatore in una pagina Web è in genere richiesta la scrittura di codice JavaScript, ma Razor Components offre un approccio migliore usando C#.</span><span class="sxs-lookup"><span data-stu-id="cba87-144">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="cba87-145">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="cba87-145">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="cba87-146">Una richiesta per `/counter` nel browser, come specificato da di `@page` direttiva all'inizio, fa sì che il componente di contatore per il rendering del relativo contenuto.</span><span class="sxs-lookup"><span data-stu-id="cba87-146">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="cba87-147">Componenti di eseguire il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="cba87-147">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="cba87-148">Ogni volta che il **Click me** pulsante è selezionato:</span><span class="sxs-lookup"><span data-stu-id="cba87-148">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="cba87-149">Il `onclick` evento.</span><span class="sxs-lookup"><span data-stu-id="cba87-149">The `onclick` event is fired.</span></span>
* <span data-ttu-id="cba87-150">Viene chiamato il metodo `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="cba87-150">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="cba87-151">Il `currentCount` viene incrementato.</span><span class="sxs-lookup"><span data-stu-id="cba87-151">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="cba87-152">Il componente viene eseguito il rendering nuovamente.</span><span class="sxs-lookup"><span data-stu-id="cba87-152">The component is rendered again.</span></span>

<span data-ttu-id="cba87-153">Il runtime consente di confrontare il nuovo contenuto per il contenuto precedente e si applica solo il contenuto modificato per il provider di servizi Internet (DOM, Document Object Model).</span><span class="sxs-lookup"><span data-stu-id="cba87-153">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="cba87-154">Aggiungere un componente a un altro componente usando una sintassi simile all'HTML.</span><span class="sxs-lookup"><span data-stu-id="cba87-154">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="cba87-155">Parametri del componente vengono specificati utilizzando gli attributi o contenuto figlio.</span><span class="sxs-lookup"><span data-stu-id="cba87-155">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="cba87-156">Ad esempio, un componente del contatore può essere aggiunto alla home page dell'app aggiungendo un `<Counter />` elemento per il componente dell'indice.</span><span class="sxs-lookup"><span data-stu-id="cba87-156">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="cba87-157">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="cba87-157">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="cba87-158">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="cba87-158">Run the app.</span></span> <span data-ttu-id="cba87-159">Home page ha un proprio contatore.</span><span class="sxs-lookup"><span data-stu-id="cba87-159">The homepage has its own counter.</span></span>

<span data-ttu-id="cba87-160">Per aggiungere un parametro per il componente del contatore, aggiornare il componente `@functions` blocco:</span><span class="sxs-lookup"><span data-stu-id="cba87-160">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="cba87-161">Aggiungere una proprietà per `IncrementAmount` decorata con il `[Parameter]` attributo.</span><span class="sxs-lookup"><span data-stu-id="cba87-161">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="cba87-162">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="cba87-162">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="cba87-163">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="cba87-163">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

<span data-ttu-id="cba87-164">Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Home usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="cba87-164">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="cba87-165">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="cba87-165">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="cba87-166">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="cba87-166">Run the app.</span></span> <span data-ttu-id="cba87-167">Home page ha un proprio contatore che viene incrementato di dieci ogni volta che il **Click me** pulsante è selezionato.</span><span class="sxs-lookup"><span data-stu-id="cba87-167">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cba87-168">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="cba87-168">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
