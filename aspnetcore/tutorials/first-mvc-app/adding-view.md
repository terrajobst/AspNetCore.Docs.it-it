---
title: Aggiungere una vista a un'app ASP.NET Core MVC
author: rick-anderson
description: Aggiunta di una vista a una semplice app ASP.NET Core MVC
ms.author: riande
ms.date: 03/04/2017
uid: tutorials/first-mvc-app/adding-view
ms.openlocfilehash: 5267e5a49bb6ecdd4cef671989f111eae7a64ec4
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53381816"
---
# <a name="add-a-view-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="ebacb-103">Aggiungere una vista a un'app ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="ebacb-103">Add a view to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="ebacb-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ebacb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ebacb-105">In questa sezione si modifica la classe `HelloWorldController` in modo da usare i file della vista [Razor](xref:mvc/views/razor) per incapsulare correttamente il processo di generazione di risposte HTML a un client.</span><span class="sxs-lookup"><span data-stu-id="ebacb-105">In this section you modify the `HelloWorldController` class to use [Razor](xref:mvc/views/razor) view files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="ebacb-106">Si crea un file di modello di vista usando Razor.</span><span class="sxs-lookup"><span data-stu-id="ebacb-106">You create a view template file using Razor.</span></span> <span data-ttu-id="ebacb-107">I modelli di vista basati su Razor hanno l'estensione di file *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ebacb-107">Razor-based view templates have a *.cshtml* file extension.</span></span> <span data-ttu-id="ebacb-108">Consentono di creare l'output HTML in modo accurato con C#.</span><span class="sxs-lookup"><span data-stu-id="ebacb-108">They provide an elegant way to create HTML output with C#.</span></span>

<span data-ttu-id="ebacb-109">Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller.</span><span class="sxs-lookup"><span data-stu-id="ebacb-109">Currently the `Index` method returns a string with a message that's hard-coded in the controller class.</span></span> <span data-ttu-id="ebacb-110">Nella classe `HelloWorldController` sostituire il metodo `Index` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ebacb-110">In the `HelloWorldController` class, replace the `Index` method with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

<span data-ttu-id="ebacb-111">Il codice precedente restituisce un oggetto `View`.</span><span class="sxs-lookup"><span data-stu-id="ebacb-111">The preceding code returns a `View` object.</span></span> <span data-ttu-id="ebacb-112">Usa un modello di vista per generare una risposta HTML al browser.</span><span class="sxs-lookup"><span data-stu-id="ebacb-112">It uses a view template to generate an HTML response to the browser.</span></span> <span data-ttu-id="ebacb-113">I metodi controller, noti anche come metodi di azione, ad esempio il metodo `Index` descritto in precedenza, restituiscono in genere un [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) (o una classe derivata da `ActionResult`) e non un tipo come una stringa.</span><span class="sxs-lookup"><span data-stu-id="ebacb-113">Controller methods (also known as action methods) such as the `Index` method above, generally return an [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) (or a class derived from `ActionResult`), not a type like string.</span></span>

## <a name="add-a-view"></a><span data-ttu-id="ebacb-114">Aggiungere una vista</span><span class="sxs-lookup"><span data-stu-id="ebacb-114">Add a view</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ebacb-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ebacb-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ebacb-116">Fare clic con il pulsante destro del mouse sulla cartella *Views*, quindi su **Aggiungi > Nuova cartella** e denominare la cartella *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="ebacb-116">Right click on the *Views* folder, and then **Add > New Folder** and name the folder *HelloWorld*.</span></span>

* <span data-ttu-id="ebacb-117">Fare clic con il pulsante destro del mouse sulla cartella *Views/HelloWorld*, quindi su **Aggiungi > Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="ebacb-117">Right click on the *Views/HelloWorld* folder, and then **Add > New Item**.</span></span>

* <span data-ttu-id="ebacb-118">Nella finestra di dialogo **Aggiungi nuovo elemento - MvcMovie**</span><span class="sxs-lookup"><span data-stu-id="ebacb-118">In the **Add New Item - MvcMovie** dialog</span></span>

  * <span data-ttu-id="ebacb-119">Nella casella di ricerca in alto a destra immettere *view* (vista)</span><span class="sxs-lookup"><span data-stu-id="ebacb-119">In the search box in the upper-right, enter *view*</span></span>

  * <span data-ttu-id="ebacb-120">Selezionare **Visualizzazione Razor**</span><span class="sxs-lookup"><span data-stu-id="ebacb-120">Select **Razor View**</span></span>

  * <span data-ttu-id="ebacb-121">Mantenere il valore della casella **Nome**, *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ebacb-121">Keep the **Name** box value, *Index.cshtml*.</span></span>

  * <span data-ttu-id="ebacb-122">Selezionare **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="ebacb-122">Select **Add**</span></span>

![Finestra di dialogo Aggiungi nuovo elemento](adding-view/_static/add_view.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ebacb-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ebacb-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ebacb-125">Aggiungere una vista `Index` per `HelloWorldController`.</span><span class="sxs-lookup"><span data-stu-id="ebacb-125">Add an `Index` view for the `HelloWorldController`.</span></span>

* <span data-ttu-id="ebacb-126">Aggiungere una nuova cartella denominata *Views/HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="ebacb-126">Add a new folder named *Views/HelloWorld*.</span></span>
* <span data-ttu-id="ebacb-127">Aggiungere un nuovo file al nome di cartella *Views/HelloWorld* *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ebacb-127">Add a new file to the *Views/HelloWorld* folder name *Index.cshtml*.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ebacb-128">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="ebacb-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ebacb-129">Fare clic con il pulsante destro del mouse sulla cartella *Views*, quindi su **Aggiungi > Nuova cartella** e denominare la cartella *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="ebacb-129">Right click on the *Views* folder, and then **Add > New Folder** and name the folder *HelloWorld*.</span></span>
* <span data-ttu-id="ebacb-130">Fare clic con il pulsante destro del mouse sulla cartella *Views/HelloWorld*, quindi su **Aggiungi > Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="ebacb-130">Right click on the *Views/HelloWorld* folder, and then **Add > New File**.</span></span>
* <span data-ttu-id="ebacb-131">Nel finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="ebacb-131">In the **New File** dialog:</span></span>

  * <span data-ttu-id="ebacb-132">Selezionare **Web** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="ebacb-132">Select **Web** in the left pane.</span></span>
  * <span data-ttu-id="ebacb-133">Selezionare **File HTML vuoto** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="ebacb-133">Select **Empty HTML file** in the center pane.</span></span>
  * <span data-ttu-id="ebacb-134">Digitare *Index.cshtml* nella casella **Nome**.</span><span class="sxs-lookup"><span data-stu-id="ebacb-134">Type *Index.cshtml* in the **Name** box.</span></span>
  * <span data-ttu-id="ebacb-135">Selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="ebacb-135">Select **New**.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento](adding-view/_static/add_view.png)

---  
<!-- End of VS tabs -->

<span data-ttu-id="ebacb-137">Sostituire il contenuto del file di vista Razor *Views/HelloWorld/Index.cshtml* con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="ebacb-137">Replace the contents of the *Views/HelloWorld/Index.cshtml* Razor view file with the following:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]

<span data-ttu-id="ebacb-138">Passare a `https://localhost:xxxx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="ebacb-138">Navigate to `https://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="ebacb-139">Il metodo `Index` in `HelloWorldController` non ha eseguito molte operazioni; ha eseguito l'istruzione `return View();` che ha specificato che il metodo deve usare un file di modello della vista per eseguire il rendering di una risposta al browser.</span><span class="sxs-lookup"><span data-stu-id="ebacb-139">The `Index` method in the `HelloWorldController` didn't do much; it ran the statement `return View();`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="ebacb-140">Poiché non è stato specificato in modo esplicito il nome del file di modello della vista, MVC imposta come predefinito il file della vista *Index.cshtml* nella cartella */Views/HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="ebacb-140">Because you didn't explicitly specify the name of the view template file, MVC defaulted to using the *Index.cshtml* view file in the */Views/HelloWorld* folder.</span></span> <span data-ttu-id="ebacb-141">L'immagine seguente mostra la stringa "Hello from our View Template!"</span><span class="sxs-lookup"><span data-stu-id="ebacb-141">The image below shows the string "Hello from our View Template!"</span></span> <span data-ttu-id="ebacb-142">hardcoded nella vista.</span><span class="sxs-lookup"><span data-stu-id="ebacb-142">hard-coded in the view.</span></span>

![Finestra del browser](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a><span data-ttu-id="ebacb-144">Modificare le viste e le pagine di layout</span><span class="sxs-lookup"><span data-stu-id="ebacb-144">Change views and layout pages</span></span>

<span data-ttu-id="ebacb-145">Selezionare i collegamenti di menu: **MvcMovie**, **Home** e **Privacy**.</span><span class="sxs-lookup"><span data-stu-id="ebacb-145">Select the menu links (**MvcMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="ebacb-146">Ogni pagina mostra lo stesso layout di menu.</span><span class="sxs-lookup"><span data-stu-id="ebacb-146">Each page shows the same menu layout.</span></span> <span data-ttu-id="ebacb-147">Il layout di menu viene implementato nel file *Views/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ebacb-147">The menu layout is implemented in the *Views/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="ebacb-148">Aprire il file *Views/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ebacb-148">Open the *Views/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="ebacb-149">I modelli di [layout](xref:mvc/views/layout) consentono di specificare il layout del contenitore HTML del sito in un'unica posizione e quindi di applicarlo in più pagine del sito.</span><span class="sxs-lookup"><span data-stu-id="ebacb-149">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="ebacb-150">Trovare la riga `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="ebacb-150">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="ebacb-151">`RenderBody` è un segnaposto dove tutte le pagine specifiche della vista vengono presentate, *incapsulate* nella pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="ebacb-151">`RenderBody` is a placeholder where all the view-specific pages you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="ebacb-152">Ad esempio, se si seleziona il collegamento **Privacy**, il rendering della vista **Views/Home/Privacy.cshtml** viene eseguito all'interno del metodo `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="ebacb-152">For example, if you select the **Privacy** link, the **Views/Home/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a><span data-ttu-id="ebacb-153">Modificare il titolo e il collegamento di menu nel file di layout</span><span class="sxs-lookup"><span data-stu-id="ebacb-153">Change the title and menu link in the layout file</span></span>

* <span data-ttu-id="ebacb-154">Nell'elemento del titolo cambiare `MvcMovie` in `Movie App`.</span><span class="sxs-lookup"><span data-stu-id="ebacb-154">In the title element, change `MvcMovie` to `Movie App`.</span></span>
* <span data-ttu-id="ebacb-155">Modificare l'elemento ancora `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` in `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.</span><span class="sxs-lookup"><span data-stu-id="ebacb-155">Change the anchor element `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` to `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.</span></span>

<span data-ttu-id="ebacb-156">Il markup seguente visualizza le modifiche evidenziate:</span><span class="sxs-lookup"><span data-stu-id="ebacb-156">The following markup shows the highlighted changes:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Shared/_Layout.cshtml?highlight=6,24)]

<span data-ttu-id="ebacb-157">Nel markup precedente, l'attributo `asp-area` [Helper Tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) è stato omesso perché questa app non usa le [Aree](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="ebacb-157">In the preceding markup, the `asp-area` [anchor Tag Helper attribute](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) was omitted because this app is not using [Areas](xref:mvc/controllers/areas).</span></span>

<!-- Routing has changed in 2.2, it's going to the last route.
>[!WARNING]
> We haven't implemented the `Movies` controller yet, so if you click the `Movie App` link, you get a 404 (Not found) error.
-->

<span data-ttu-id="ebacb-158">**Nota**: Il controller `Movies` non è stato implementato.</span><span class="sxs-lookup"><span data-stu-id="ebacb-158">**Note**: The `Movies` controller has not been implemented.</span></span> <span data-ttu-id="ebacb-159">A questo punto, il collegamento `Movie App` non è funzionale.</span><span class="sxs-lookup"><span data-stu-id="ebacb-159">At this point, the `Movie App` link is not functional.</span></span>

<span data-ttu-id="ebacb-160">Salvare le modifiche e selezionare il collegamento **Privacy**.</span><span class="sxs-lookup"><span data-stu-id="ebacb-160">Save your changes and select the **Privacy** link.</span></span> <span data-ttu-id="ebacb-161">Si noti come il titolo sulla scheda del browser visualizzi **Privacy - Movie App** anziché **Privacy - Mvc Movie**:</span><span class="sxs-lookup"><span data-stu-id="ebacb-161">Notice how the title on the browser tab displays **Privacy - Movie App** instead of **Privacy - Mvc Movie**:</span></span>

![Scheda Privacy](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

<span data-ttu-id="ebacb-163">Toccare il collegamento **Home** e notare che anche il titolo e il testo di ancoraggio visualizzano **Movie App**.</span><span class="sxs-lookup"><span data-stu-id="ebacb-163">Select the **Home** link and notice that the title and anchor text also display **Movie App**.</span></span> <span data-ttu-id="ebacb-164">La modifica è stata apportata una volta nel modello di layout e tutte le pagine nel sito riflettono il nuovo testo del collegamento e il nuovo titolo.</span><span class="sxs-lookup"><span data-stu-id="ebacb-164">We were able to make the change once in the layout template and have all pages on the site reflect the new link text and new title.</span></span>

<span data-ttu-id="ebacb-165">Esaminare il file *Views/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ebacb-165">Examine the *Views/_ViewStart.cshtml* file:</span></span>

```HTML
@{
    Layout = "_Layout";
}
```

<span data-ttu-id="ebacb-166">Il file *Views/_ViewStart.cshtml* attiva il file *Views/Shared/_Layout.cshtml* per ogni vista.</span><span class="sxs-lookup"><span data-stu-id="ebacb-166">The *Views/_ViewStart.cshtml* file brings in the *Views/Shared/_Layout.cshtml* file to each view.</span></span> <span data-ttu-id="ebacb-167">È possibile usare la proprietà `Layout` per impostare una vista di layout differente oppure impostarla su `null` e quindi non verrà usato alcun file di layout.</span><span class="sxs-lookup"><span data-stu-id="ebacb-167">The `Layout` property can be used to set a different layout view, or set it to `null` so no layout file will be used.</span></span>

<span data-ttu-id="ebacb-168">Modificare il titolo e l'elemento `<h2>` del file *Views/HelloWorld/Index.cshtml* di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="ebacb-168">Change the title and `<h2>` element of the *Views/HelloWorld/Index.cshtml* view file:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

<span data-ttu-id="ebacb-169">Il titolo e l'elemento `<h2>`sono leggermente diversi in modo da poter esaminare la parte specifica di codice che modifica la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ebacb-169">The title and `<h2>` element are slightly different so you can see which bit of code changes the display.</span></span>

<span data-ttu-id="ebacb-170">`ViewData["Title"] = "Movie List";` nel codice precedente imposta la proprietà `Title` del dizionario `ViewData` su "Movie List".</span><span class="sxs-lookup"><span data-stu-id="ebacb-170">`ViewData["Title"] = "Movie List";` in the code above sets the `Title` property of the `ViewData` dictionary to "Movie List".</span></span> <span data-ttu-id="ebacb-171">La proprietà `Title` viene usata nell'elemento HTML `<title>` nella pagina di layout:</span><span class="sxs-lookup"><span data-stu-id="ebacb-171">The `Title` property is used in the `<title>` HTML element in the layout page:</span></span>

```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

<span data-ttu-id="ebacb-172">Salvare la modifica e passare a `https://localhost:xxxx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="ebacb-172">Save the change and navigate to `https://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="ebacb-173">Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati.</span><span class="sxs-lookup"><span data-stu-id="ebacb-173">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="ebacb-174">Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="ebacb-174">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="ebacb-175">Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server. Il titolo del browser viene creato con il valore `ViewData["Title"]` impostato nel modello di vista *Index.cshtml* e la stringa "- Movie App" aggiuntiva viene aggiunta nel file di layout.</span><span class="sxs-lookup"><span data-stu-id="ebacb-175">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with `ViewData["Title"]` we set in the *Index.cshtml* view template and the additional "- Movie App" added in the layout file.</span></span>

<span data-ttu-id="ebacb-176">Si noti anche come il contenuto del modello di vista *Index.cshtml* sia stato unito con il modello di vista *Views/Shared/_Layout.cshtml* e sia stata inviata una singola risposta HTML al browser.</span><span class="sxs-lookup"><span data-stu-id="ebacb-176">Also notice how the content in the *Index.cshtml* view template was merged with the *Views/Shared/_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="ebacb-177">I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ebacb-177">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span> <span data-ttu-id="ebacb-178">Per altre informazioni, vedere [Layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="ebacb-178">To learn more see [Layout](xref:mvc/views/layout).</span></span>

![Vista dell'elenco di film](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

<span data-ttu-id="ebacb-180">Il breve testo di "dati", in questo caso il messaggio "Hello from our View Template!",</span><span class="sxs-lookup"><span data-stu-id="ebacb-180">Our little bit of "data" (in this case the "Hello from our View Template!"</span></span> <span data-ttu-id="ebacb-181">è tuttavia hardcoded.</span><span class="sxs-lookup"><span data-stu-id="ebacb-181">message) is hard-coded, though.</span></span> <span data-ttu-id="ebacb-182">L'applicazione MVC ha un elemento "V" (vista) ed è stato ottenuto un elemento "C" (controller), ma non ancora un elemento "M" (modello).</span><span class="sxs-lookup"><span data-stu-id="ebacb-182">The MVC application has a "V" (view) and you've got a "C" (controller), but no "M" (model) yet.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="ebacb-183">Passaggio di dati dal controller alla vista</span><span class="sxs-lookup"><span data-stu-id="ebacb-183">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="ebacb-184">Le azioni del controller vengono richiamate in risposta a una richiesta URL in ingresso.</span><span class="sxs-lookup"><span data-stu-id="ebacb-184">Controller actions are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="ebacb-185">Una classe controller è il punto in cui viene scritto il codice che gestisce le richieste in ingresso del browser.</span><span class="sxs-lookup"><span data-stu-id="ebacb-185">A controller class is where the code is written that handles the incoming browser requests.</span></span> <span data-ttu-id="ebacb-186">Il controller recupera i dati da un'origine dati e determina il tipo di risposta da inviare al browser.</span><span class="sxs-lookup"><span data-stu-id="ebacb-186">The controller retrieves data from a data source and decides what type of response to send back to the browser.</span></span> <span data-ttu-id="ebacb-187">I modelli di vista possono essere usati da un controller per generare e formattare una risposta HTML al browser.</span><span class="sxs-lookup"><span data-stu-id="ebacb-187">View templates can be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="ebacb-188">Il compito dei controller è fornire i dati necessari affinché un modello di vista possa eseguire il rendering di una risposta.</span><span class="sxs-lookup"><span data-stu-id="ebacb-188">Controllers are responsible for providing the data required in order for a view template to render a response.</span></span> <span data-ttu-id="ebacb-189">Procedura consigliata: i modelli di vista **non** devono eseguire la logica di business o interagire direttamente con un database.</span><span class="sxs-lookup"><span data-stu-id="ebacb-189">A best practice: View templates should **not** perform business logic or interact with a database directly.</span></span> <span data-ttu-id="ebacb-190">Un modello di vista deve invece usare solo i dati forniti dal controller.</span><span class="sxs-lookup"><span data-stu-id="ebacb-190">Rather, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="ebacb-191">Questa "separazione delle problematiche" consente di mantenere il codice pulito e semplifica la gestione e i test.</span><span class="sxs-lookup"><span data-stu-id="ebacb-191">Maintaining this "separation of concerns" helps keep the code clean, testable, and maintainable.</span></span>

<span data-ttu-id="ebacb-192">Attualmente il metodo `Welcome` nella classe `HelloWorldController` accetta un parametro `name` e `ID` e quindi genera i valori direttamente al browser.</span><span class="sxs-lookup"><span data-stu-id="ebacb-192">Currently, the `Welcome` method in the `HelloWorldController` class takes a `name` and a `ID` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="ebacb-193">Invece di ottenere il rendering di questa risposta come stringa, cambiare il controller in modo che usi un modello di vista.</span><span class="sxs-lookup"><span data-stu-id="ebacb-193">Rather than have the controller render this response as a string, change the controller to use a view template instead.</span></span> <span data-ttu-id="ebacb-194">Il modello di vista genera una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta.</span><span class="sxs-lookup"><span data-stu-id="ebacb-194">The view template generates a dynamic response, which means that appropriate bits of data must be passed from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="ebacb-195">A tale scopo, fare in modo che il controller inserisca i dati dinamici (parametri) di cui il modello di vista necessita in un dizionario `ViewData` a cui il modello di vista può accedere.</span><span class="sxs-lookup"><span data-stu-id="ebacb-195">Do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewData` dictionary that the view template can then access.</span></span>

<span data-ttu-id="ebacb-196">In *HelloWorldController.cs*, modificare il metodo `Welcome` in modo da aggiungere un valore `Message` e `NumTimes` al dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="ebacb-196">In *HelloWorldController.cs*, change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewData` dictionary.</span></span> <span data-ttu-id="ebacb-197">Il dizionario `ViewData` è un oggetto dinamico, ovvero è possibile usare qualunque tipo; l'oggetto `ViewData` non dispone di proprietà definite fino a quando non si inserisce un elemento al suo interno.</span><span class="sxs-lookup"><span data-stu-id="ebacb-197">The `ViewData` dictionary is a dynamic object, which means any type can be used; the `ViewData` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="ebacb-198">Il sistema di [associazione di modelli MVC](xref:mvc/models/model-binding) esegue il mapping dei parametri denominati (`name` e `numTimes`) dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.</span><span class="sxs-lookup"><span data-stu-id="ebacb-198">The [MVC model binding system](xref:mvc/models/model-binding) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="ebacb-199">Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ebacb-199">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

<span data-ttu-id="ebacb-200">L'oggetto di dizionario `ViewData` contiene i dati che verranno passati alla vista.</span><span class="sxs-lookup"><span data-stu-id="ebacb-200">The `ViewData` dictionary object contains data that will be passed to the view.</span></span> 

<span data-ttu-id="ebacb-201">Creare un modello di vista Welcome denominato *Views/HelloWorld/Welcome.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ebacb-201">Create a Welcome view template named *Views/HelloWorld/Welcome.cshtml*.</span></span>

<span data-ttu-id="ebacb-202">Si creerà un ciclo nel modello di vista *Welcome.cshtml* che visualizza la stringa "Hello" `NumTimes`.</span><span class="sxs-lookup"><span data-stu-id="ebacb-202">You'll create a loop in the *Welcome.cshtml* view template that displays "Hello" `NumTimes`.</span></span> <span data-ttu-id="ebacb-203">Sostituire il contenuto di *Views/HelloWorld/Welcome.cshtml* con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="ebacb-203">Replace the contents of *Views/HelloWorld/Welcome.cshtml* with the following:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

<span data-ttu-id="ebacb-204">Salvare le modifiche e passare all'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="ebacb-204">Save your changes and browse to the following URL:</span></span>

`https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

<span data-ttu-id="ebacb-205">I dati vengono prelevati dall'URL e passati al controller usando lo [strumento di associazione di modelli MVC](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="ebacb-205">Data is taken from the URL and passed to the controller using the [MVC model binder](xref:mvc/models/model-binding) .</span></span> <span data-ttu-id="ebacb-206">Il controller crea un pacchetto di dati in un dizionario `ViewData` e passa tale oggetto alla vista.</span><span class="sxs-lookup"><span data-stu-id="ebacb-206">The controller packages the data into a `ViewData` dictionary and passes that object to the view.</span></span> <span data-ttu-id="ebacb-207">La vista esegue quindi il rendering dei dati in formato HTML al browser.</span><span class="sxs-lookup"><span data-stu-id="ebacb-207">The view then renders the data as HTML to the browser.</span></span>

![Vista Privacy con un'etichetta di benvenuto e la frase Hello Rick riportata quattro volte](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

<span data-ttu-id="ebacb-209">Nell'esempio precedente è stato usato il dizionario `ViewData` per passare i dati dal controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="ebacb-209">In the sample above, the `ViewData` dictionary was used to pass data from the controller to a view.</span></span> <span data-ttu-id="ebacb-210">Più avanti nell'esercitazione si userà un modello di vista per passare i dati da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="ebacb-210">Later in the tutorial, a view model is used to pass data from a controller to a view.</span></span> <span data-ttu-id="ebacb-211">In genere l'approccio basato sul modello di vista per passare i dati è preferito rispetto all'approccio basato sul dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="ebacb-211">The view model approach to passing data is generally much preferred over the `ViewData` dictionary approach.</span></span> <span data-ttu-id="ebacb-212">Per altre informazioni, vedere [When to use ViewBag, ViewData, or TempData](http://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) (Quando usare use ViewBag, ViewData o TempData).</span><span class="sxs-lookup"><span data-stu-id="ebacb-212">See [When to use ViewBag, ViewData, or TempData ](http://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) for more information.</span></span>

<span data-ttu-id="ebacb-213">Nella prossima esercitazione, viene creato un database di film.</span><span class="sxs-lookup"><span data-stu-id="ebacb-213">In the next tutorial, a database of movies is created.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ebacb-214">[Precedente](adding-controller.md)
> [Successivo](adding-model.md)</span><span class="sxs-lookup"><span data-stu-id="ebacb-214">[Previous](adding-controller.md)
[Next](adding-model.md)</span></span>
