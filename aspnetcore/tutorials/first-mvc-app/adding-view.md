---
title: Aggiungere una vista a un'app ASP.NET Core MVC
author: rick-anderson
description: Aggiunta di una vista a una semplice app ASP.NET Core MVC
ms.author: riande
ms.date: 03/04/2017
uid: tutorials/first-mvc-app/adding-view
ms.openlocfilehash: 70c96b4838b37ea2c193c328f00a410642338369
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "67167073"
---
# <a name="add-a-view-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="dfb45-103">Aggiungere una vista a un'app ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="dfb45-103">Add a view to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="dfb45-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dfb45-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dfb45-105">In questa sezione si modifica la classe `HelloWorldController` in modo da usare i file della vista [Razor](xref:mvc/views/razor) per incapsulare correttamente il processo di generazione di risposte HTML a un client.</span><span class="sxs-lookup"><span data-stu-id="dfb45-105">In this section you modify the `HelloWorldController` class to use [Razor](xref:mvc/views/razor) view files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="dfb45-106">Si crea un file di modello di vista usando Razor.</span><span class="sxs-lookup"><span data-stu-id="dfb45-106">You create a view template file using Razor.</span></span> <span data-ttu-id="dfb45-107">I modelli di vista basati su Razor hanno l'estensione di file *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dfb45-107">Razor-based view templates have a *.cshtml* file extension.</span></span> <span data-ttu-id="dfb45-108">Consentono di creare l'output HTML in modo accurato con C#.</span><span class="sxs-lookup"><span data-stu-id="dfb45-108">They provide an elegant way to create HTML output with C#.</span></span>

<span data-ttu-id="dfb45-109">Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller.</span><span class="sxs-lookup"><span data-stu-id="dfb45-109">Currently the `Index` method returns a string with a message that's hard-coded in the controller class.</span></span> <span data-ttu-id="dfb45-110">Nella classe `HelloWorldController` sostituire il metodo `Index` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dfb45-110">In the `HelloWorldController` class, replace the `Index` method with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

<span data-ttu-id="dfb45-111">Il codice precedente chiama il metodo <xref:Microsoft.AspNetCore.Mvc.Controller.View*> del controller.</span><span class="sxs-lookup"><span data-stu-id="dfb45-111">The preceding code calls the controller's <xref:Microsoft.AspNetCore.Mvc.Controller.View*> method.</span></span> <span data-ttu-id="dfb45-112">Usa un modello di visualizzazione per generare una risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="dfb45-112">It uses a view template to generate an HTML response.</span></span> <span data-ttu-id="dfb45-113">I metodi del controller, noti anche come *metodi di azione*, ad esempio il metodo `Index` descritto in precedenza, restituiscono in genere un <xref:Microsoft.AspNetCore.Mvc.IActionResult> (o una classe derivata da <xref:Microsoft.AspNetCore.Mvc.ActionResult>) e non un tipo come `string`.</span><span class="sxs-lookup"><span data-stu-id="dfb45-113">Controller methods (also known as *action methods*), such as the `Index` method above, generally return an <xref:Microsoft.AspNetCore.Mvc.IActionResult> (or a class derived from <xref:Microsoft.AspNetCore.Mvc.ActionResult>), not a type like `string`.</span></span>

## <a name="add-a-view"></a><span data-ttu-id="dfb45-114">Aggiungere una vista</span><span class="sxs-lookup"><span data-stu-id="dfb45-114">Add a view</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dfb45-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dfb45-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dfb45-116">Fare clic con il pulsante destro del mouse sulla cartella *Views*, quindi su **Aggiungi > Nuova cartella** e denominare la cartella *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="dfb45-116">Right click on the *Views* folder, and then **Add > New Folder** and name the folder *HelloWorld*.</span></span>

* <span data-ttu-id="dfb45-117">Fare clic con il pulsante destro del mouse sulla cartella *Views/HelloWorld*, quindi su **Aggiungi > Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="dfb45-117">Right click on the *Views/HelloWorld* folder, and then **Add > New Item**.</span></span>

* <span data-ttu-id="dfb45-118">Nella finestra di dialogo **Aggiungi nuovo elemento - MvcMovie**</span><span class="sxs-lookup"><span data-stu-id="dfb45-118">In the **Add New Item - MvcMovie** dialog</span></span>

  * <span data-ttu-id="dfb45-119">Nella casella di ricerca in alto a destra immettere *view* (vista)</span><span class="sxs-lookup"><span data-stu-id="dfb45-119">In the search box in the upper-right, enter *view*</span></span>

  * <span data-ttu-id="dfb45-120">Selezionare **Visualizzazione Razor**</span><span class="sxs-lookup"><span data-stu-id="dfb45-120">Select **Razor View**</span></span>

  * <span data-ttu-id="dfb45-121">Mantenere il valore della casella **Nome**, *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dfb45-121">Keep the **Name** box value, *Index.cshtml*.</span></span>

  * <span data-ttu-id="dfb45-122">Selezionare **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="dfb45-122">Select **Add**</span></span>

![Finestra di dialogo Aggiungi nuovo elemento](adding-view/_static/add_view.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dfb45-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dfb45-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dfb45-125">Aggiungere una vista `Index` per `HelloWorldController`.</span><span class="sxs-lookup"><span data-stu-id="dfb45-125">Add an `Index` view for the `HelloWorldController`.</span></span>

* <span data-ttu-id="dfb45-126">Aggiungere una nuova cartella denominata *Views/HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="dfb45-126">Add a new folder named *Views/HelloWorld*.</span></span>
* <span data-ttu-id="dfb45-127">Aggiungere un nuovo file al nome di cartella *Views/HelloWorld* *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dfb45-127">Add a new file to the *Views/HelloWorld* folder name *Index.cshtml*.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dfb45-128">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="dfb45-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="dfb45-129">Fare clic con il pulsante destro del mouse sulla cartella *Views*, quindi su **Aggiungi > Nuova cartella** e denominare la cartella *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="dfb45-129">Right click on the *Views* folder, and then **Add > New Folder** and name the folder *HelloWorld*.</span></span>
* <span data-ttu-id="dfb45-130">Fare clic con il pulsante destro del mouse sulla cartella *Views/HelloWorld*, quindi su **Aggiungi > Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="dfb45-130">Right click on the *Views/HelloWorld* folder, and then **Add > New File**.</span></span>
* <span data-ttu-id="dfb45-131">Nel finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="dfb45-131">In the **New File** dialog:</span></span>

  * <span data-ttu-id="dfb45-132">Selezionare **Web** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="dfb45-132">Select **Web** in the left pane.</span></span>
  * <span data-ttu-id="dfb45-133">Selezionare **File HTML vuoto** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="dfb45-133">Select **Empty HTML file** in the center pane.</span></span>
  * <span data-ttu-id="dfb45-134">Digitare *Index.cshtml* nella casella **Nome**.</span><span class="sxs-lookup"><span data-stu-id="dfb45-134">Type *Index.cshtml* in the **Name** box.</span></span>
  * <span data-ttu-id="dfb45-135">Selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="dfb45-135">Select **New**.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento](adding-view/_static/add_view.png)

---

<span data-ttu-id="dfb45-137">Sostituire il contenuto del file di vista Razor *Views/HelloWorld/Index.cshtml* con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="dfb45-137">Replace the contents of the *Views/HelloWorld/Index.cshtml* Razor view file with the following:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]

<span data-ttu-id="dfb45-138">Passare a `https://localhost:xxxx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="dfb45-138">Navigate to `https://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="dfb45-139">Il metodo `Index` in `HelloWorldController` non ha eseguito molte operazioni; ha eseguito l'istruzione `return View();` che ha specificato che il metodo deve usare un file di modello della vista per eseguire il rendering di una risposta al browser.</span><span class="sxs-lookup"><span data-stu-id="dfb45-139">The `Index` method in the `HelloWorldController` didn't do much; it ran the statement `return View();`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="dfb45-140">Poiché non è stato specificato un nome di file di modello di visualizzazione, per impostazione predefinita MVC usa il file di visualizzazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="dfb45-140">Because a view template file name wasn't specified, MVC defaulted to using the default view file.</span></span> <span data-ttu-id="dfb45-141">Il file di visualizzazione predefinito ha lo stesso nome del metodo (`Index`), quindi viene usato */Views/HelloWorld/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dfb45-141">The default view file has the same name as the method (`Index`), so in the */Views/HelloWorld/Index.cshtml* is used.</span></span> <span data-ttu-id="dfb45-142">L'immagine seguente mostra la stringa "Hello from our View Template!"</span><span class="sxs-lookup"><span data-stu-id="dfb45-142">The image below shows the string "Hello from our View Template!"</span></span> <span data-ttu-id="dfb45-143">hardcoded nella vista.</span><span class="sxs-lookup"><span data-stu-id="dfb45-143">hard-coded in the view.</span></span>

![Finestra del browser](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a><span data-ttu-id="dfb45-145">Modificare le viste e le pagine di layout</span><span class="sxs-lookup"><span data-stu-id="dfb45-145">Change views and layout pages</span></span>

<span data-ttu-id="dfb45-146">Selezionare i collegamenti di menu: **MvcMovie**, **Home** e **Privacy**.</span><span class="sxs-lookup"><span data-stu-id="dfb45-146">Select the menu links (**MvcMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="dfb45-147">Ogni pagina mostra lo stesso layout di menu.</span><span class="sxs-lookup"><span data-stu-id="dfb45-147">Each page shows the same menu layout.</span></span> <span data-ttu-id="dfb45-148">Il layout di menu viene implementato nel file *Views/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dfb45-148">The menu layout is implemented in the *Views/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="dfb45-149">Aprire il file *Views/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dfb45-149">Open the *Views/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="dfb45-150">I modelli di [layout](xref:mvc/views/layout) consentono di specificare il layout del contenitore HTML del sito in un'unica posizione e quindi di applicarlo in più pagine del sito.</span><span class="sxs-lookup"><span data-stu-id="dfb45-150">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="dfb45-151">Trovare la riga `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="dfb45-151">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="dfb45-152">`RenderBody` è un segnaposto dove tutte le pagine specifiche della vista vengono presentate, *incapsulate* nella pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="dfb45-152">`RenderBody` is a placeholder where all the view-specific pages you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="dfb45-153">Ad esempio, se si seleziona il collegamento **Privacy**, il rendering della vista **Views/Home/Privacy.cshtml** viene eseguito all'interno del metodo `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="dfb45-153">For example, if you select the **Privacy** link, the **Views/Home/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

## <a name="change-the-title-footer-and-menu-link-in-the-layout-file"></a><span data-ttu-id="dfb45-154">Modificare il titolo, il piè di pagina e il collegamento di menu nel file di layout</span><span class="sxs-lookup"><span data-stu-id="dfb45-154">Change the title, footer, and menu link in the layout file</span></span>

* <span data-ttu-id="dfb45-155">Negli elementi del titolo e del piè di pagina modificare `MvcMovie` in `Movie App`.</span><span class="sxs-lookup"><span data-stu-id="dfb45-155">In the title and footer elements, change `MvcMovie` to `Movie App`.</span></span>
* <span data-ttu-id="dfb45-156">Modificare l'elemento ancora `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` in `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.</span><span class="sxs-lookup"><span data-stu-id="dfb45-156">Change the anchor element `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` to `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.</span></span>

<span data-ttu-id="dfb45-157">Il markup seguente visualizza le modifiche evidenziate:</span><span class="sxs-lookup"><span data-stu-id="dfb45-157">The following markup shows the highlighted changes:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Shared/_Layout.cshtml?highlight=6,24,51)]

<span data-ttu-id="dfb45-158">Nel markup precedente, l'attributo `asp-area` [Helper Tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) è stato omesso perché questa app non usa le [Aree](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="dfb45-158">In the preceding markup, the `asp-area` [anchor Tag Helper attribute](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) was omitted because this app is not using [Areas](xref:mvc/controllers/areas).</span></span>

<!-- Routing has changed in 2.2, it's going to the last route.
>[!WARNING]
> We haven't implemented the `Movies` controller yet, so if you click the `Movie App` link, you get a 404 (Not found) error.
-->

<span data-ttu-id="dfb45-159">**Nota**: Il controller `Movies` non è stato implementato.</span><span class="sxs-lookup"><span data-stu-id="dfb45-159">**Note**: The `Movies` controller has not been implemented.</span></span> <span data-ttu-id="dfb45-160">A questo punto, il collegamento `Movie App` non è funzionale.</span><span class="sxs-lookup"><span data-stu-id="dfb45-160">At this point, the `Movie App` link is not functional.</span></span>

<span data-ttu-id="dfb45-161">Salvare le modifiche e selezionare il collegamento **Privacy**.</span><span class="sxs-lookup"><span data-stu-id="dfb45-161">Save your changes and select the **Privacy** link.</span></span> <span data-ttu-id="dfb45-162">Si noti come il titolo sulla scheda del browser visualizzi **Privacy Policy - Movie App** anziché **Privacy Policy - Mvc Movie**:</span><span class="sxs-lookup"><span data-stu-id="dfb45-162">Notice how the title on the browser tab displays **Privacy Policy - Movie App** instead of **Privacy Policy - Mvc Movie**:</span></span>

![Scheda Privacy](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

<span data-ttu-id="dfb45-164">Toccare il collegamento **Home** e notare che anche il titolo e il testo di ancoraggio visualizzano **Movie App**.</span><span class="sxs-lookup"><span data-stu-id="dfb45-164">Select the **Home** link and notice that the title and anchor text also display **Movie App**.</span></span> <span data-ttu-id="dfb45-165">La modifica è stata apportata una volta nel modello di layout e tutte le pagine nel sito riflettono il nuovo testo del collegamento e il nuovo titolo.</span><span class="sxs-lookup"><span data-stu-id="dfb45-165">We were able to make the change once in the layout template and have all pages on the site reflect the new link text and new title.</span></span>

<span data-ttu-id="dfb45-166">Esaminare il file *Views/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="dfb45-166">Examine the *Views/_ViewStart.cshtml* file:</span></span>

```HTML
@{
    Layout = "_Layout";
}
```

<span data-ttu-id="dfb45-167">Il file *Views/_ViewStart.cshtml* attiva il file *Views/Shared/_Layout.cshtml* per ogni vista.</span><span class="sxs-lookup"><span data-stu-id="dfb45-167">The *Views/_ViewStart.cshtml* file brings in the *Views/Shared/_Layout.cshtml* file to each view.</span></span> <span data-ttu-id="dfb45-168">È possibile usare la proprietà `Layout` per impostare una vista di layout differente oppure impostarla su `null` e quindi non verrà usato alcun file di layout.</span><span class="sxs-lookup"><span data-stu-id="dfb45-168">The `Layout` property can be used to set a different layout view, or set it to `null` so no layout file will be used.</span></span>

<span data-ttu-id="dfb45-169">Modificare il titolo e l'elemento `<h2>` del file *Views/HelloWorld/Index.cshtml* di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="dfb45-169">Change the title and `<h2>` element of the *Views/HelloWorld/Index.cshtml* view file:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

<span data-ttu-id="dfb45-170">Il titolo e l'elemento `<h2>`sono leggermente diversi in modo da poter esaminare la parte specifica di codice che modifica la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="dfb45-170">The title and `<h2>` element are slightly different so you can see which bit of code changes the display.</span></span>

<span data-ttu-id="dfb45-171">`ViewData["Title"] = "Movie List";` nel codice precedente imposta la proprietà `Title` del dizionario `ViewData` su "Movie List".</span><span class="sxs-lookup"><span data-stu-id="dfb45-171">`ViewData["Title"] = "Movie List";` in the code above sets the `Title` property of the `ViewData` dictionary to "Movie List".</span></span> <span data-ttu-id="dfb45-172">La proprietà `Title` viene usata nell'elemento HTML `<title>` nella pagina di layout:</span><span class="sxs-lookup"><span data-stu-id="dfb45-172">The `Title` property is used in the `<title>` HTML element in the layout page:</span></span>

```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

<span data-ttu-id="dfb45-173">Salvare la modifica e passare a `https://localhost:xxxx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="dfb45-173">Save the change and navigate to `https://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="dfb45-174">Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati.</span><span class="sxs-lookup"><span data-stu-id="dfb45-174">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="dfb45-175">Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="dfb45-175">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="dfb45-176">Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server. Il titolo del browser viene creato con il valore `ViewData["Title"]` impostato nel modello di vista *Index.cshtml* e la stringa "- Movie App" aggiuntiva viene aggiunta nel file di layout.</span><span class="sxs-lookup"><span data-stu-id="dfb45-176">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with `ViewData["Title"]` we set in the *Index.cshtml* view template and the additional "- Movie App" added in the layout file.</span></span>

<span data-ttu-id="dfb45-177">Si noti anche come il contenuto del modello di vista *Index.cshtml* sia stato unito con il modello di vista *Views/Shared/_Layout.cshtml* e sia stata inviata una singola risposta HTML al browser.</span><span class="sxs-lookup"><span data-stu-id="dfb45-177">Also notice how the content in the *Index.cshtml* view template was merged with the *Views/Shared/_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="dfb45-178">I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dfb45-178">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span> <span data-ttu-id="dfb45-179">Per altre informazioni, vedere [Layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="dfb45-179">To learn more see [Layout](xref:mvc/views/layout).</span></span>

![Vista dell'elenco di film](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

<span data-ttu-id="dfb45-181">Il breve testo di "dati", in questo caso il messaggio "Hello from our View Template!",</span><span class="sxs-lookup"><span data-stu-id="dfb45-181">Our little bit of "data" (in this case the "Hello from our View Template!"</span></span> <span data-ttu-id="dfb45-182">è tuttavia hardcoded.</span><span class="sxs-lookup"><span data-stu-id="dfb45-182">message) is hard-coded, though.</span></span> <span data-ttu-id="dfb45-183">L'applicazione MVC ha un elemento "V" (vista) ed è stato ottenuto un elemento "C" (controller), ma non ancora un elemento "M" (modello).</span><span class="sxs-lookup"><span data-stu-id="dfb45-183">The MVC application has a "V" (view) and you've got a "C" (controller), but no "M" (model) yet.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="dfb45-184">Passaggio di dati dal controller alla vista</span><span class="sxs-lookup"><span data-stu-id="dfb45-184">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="dfb45-185">Le azioni del controller vengono richiamate in risposta a una richiesta URL in ingresso.</span><span class="sxs-lookup"><span data-stu-id="dfb45-185">Controller actions are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="dfb45-186">Una classe controller è il punto in cui viene scritto il codice che gestisce le richieste in ingresso del browser.</span><span class="sxs-lookup"><span data-stu-id="dfb45-186">A controller class is where the code is written that handles the incoming browser requests.</span></span> <span data-ttu-id="dfb45-187">Il controller recupera i dati da un'origine dati e determina il tipo di risposta da inviare al browser.</span><span class="sxs-lookup"><span data-stu-id="dfb45-187">The controller retrieves data from a data source and decides what type of response to send back to the browser.</span></span> <span data-ttu-id="dfb45-188">I modelli di vista possono essere usati da un controller per generare e formattare una risposta HTML al browser.</span><span class="sxs-lookup"><span data-stu-id="dfb45-188">View templates can be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="dfb45-189">Il compito dei controller è fornire i dati necessari affinché un modello di vista possa eseguire il rendering di una risposta.</span><span class="sxs-lookup"><span data-stu-id="dfb45-189">Controllers are responsible for providing the data required in order for a view template to render a response.</span></span> <span data-ttu-id="dfb45-190">Procedura consigliata: i modelli di vista **non** devono eseguire la logica di business o interagire direttamente con un database.</span><span class="sxs-lookup"><span data-stu-id="dfb45-190">A best practice: View templates should **not** perform business logic or interact with a database directly.</span></span> <span data-ttu-id="dfb45-191">Un modello di vista deve invece usare solo i dati forniti dal controller.</span><span class="sxs-lookup"><span data-stu-id="dfb45-191">Rather, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="dfb45-192">Questa "separazione delle problematiche" consente di mantenere il codice pulito e semplifica la gestione e i test.</span><span class="sxs-lookup"><span data-stu-id="dfb45-192">Maintaining this "separation of concerns" helps keep the code clean, testable, and maintainable.</span></span>

<span data-ttu-id="dfb45-193">Attualmente il metodo `Welcome` nella classe `HelloWorldController` accetta un parametro `name` e `ID` e quindi genera i valori direttamente al browser.</span><span class="sxs-lookup"><span data-stu-id="dfb45-193">Currently, the `Welcome` method in the `HelloWorldController` class takes a `name` and a `ID` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="dfb45-194">Invece di ottenere il rendering di questa risposta come stringa, cambiare il controller in modo che usi un modello di vista.</span><span class="sxs-lookup"><span data-stu-id="dfb45-194">Rather than have the controller render this response as a string, change the controller to use a view template instead.</span></span> <span data-ttu-id="dfb45-195">Il modello di vista genera una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta.</span><span class="sxs-lookup"><span data-stu-id="dfb45-195">The view template generates a dynamic response, which means that appropriate bits of data must be passed from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="dfb45-196">A tale scopo, fare in modo che il controller inserisca i dati dinamici (parametri) di cui il modello di vista necessita in un dizionario `ViewData` a cui il modello di vista può accedere.</span><span class="sxs-lookup"><span data-stu-id="dfb45-196">Do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewData` dictionary that the view template can then access.</span></span>

<span data-ttu-id="dfb45-197">In *HelloWorldController.cs*, modificare il metodo `Welcome` in modo da aggiungere un valore `Message` e `NumTimes` al dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="dfb45-197">In *HelloWorldController.cs*, change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewData` dictionary.</span></span> <span data-ttu-id="dfb45-198">Il dizionario `ViewData` è un oggetto dinamico, ovvero è possibile usare qualunque tipo; l'oggetto `ViewData` non dispone di proprietà definite fino a quando non si inserisce un elemento al suo interno.</span><span class="sxs-lookup"><span data-stu-id="dfb45-198">The `ViewData` dictionary is a dynamic object, which means any type can be used; the `ViewData` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="dfb45-199">Il sistema di [associazione di modelli MVC](xref:mvc/models/model-binding) esegue il mapping dei parametri denominati (`name` e `numTimes`) dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.</span><span class="sxs-lookup"><span data-stu-id="dfb45-199">The [MVC model binding system](xref:mvc/models/model-binding) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="dfb45-200">Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="dfb45-200">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

<span data-ttu-id="dfb45-201">L'oggetto di dizionario `ViewData` contiene i dati che verranno passati alla vista.</span><span class="sxs-lookup"><span data-stu-id="dfb45-201">The `ViewData` dictionary object contains data that will be passed to the view.</span></span>

<span data-ttu-id="dfb45-202">Creare un modello di vista Welcome denominato *Views/HelloWorld/Welcome.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dfb45-202">Create a Welcome view template named *Views/HelloWorld/Welcome.cshtml*.</span></span>

<span data-ttu-id="dfb45-203">Si creerà un ciclo nel modello di vista *Welcome.cshtml* che visualizza la stringa "Hello" `NumTimes`.</span><span class="sxs-lookup"><span data-stu-id="dfb45-203">You'll create a loop in the *Welcome.cshtml* view template that displays "Hello" `NumTimes`.</span></span> <span data-ttu-id="dfb45-204">Sostituire il contenuto di *Views/HelloWorld/Welcome.cshtml* con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="dfb45-204">Replace the contents of *Views/HelloWorld/Welcome.cshtml* with the following:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

<span data-ttu-id="dfb45-205">Salvare le modifiche e passare all'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="dfb45-205">Save your changes and browse to the following URL:</span></span>

`https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

<span data-ttu-id="dfb45-206">I dati vengono prelevati dall'URL e passati al controller usando lo [strumento di associazione di modelli MVC](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="dfb45-206">Data is taken from the URL and passed to the controller using the [MVC model binder](xref:mvc/models/model-binding) .</span></span> <span data-ttu-id="dfb45-207">Il controller crea un pacchetto di dati in un dizionario `ViewData` e passa tale oggetto alla vista.</span><span class="sxs-lookup"><span data-stu-id="dfb45-207">The controller packages the data into a `ViewData` dictionary and passes that object to the view.</span></span> <span data-ttu-id="dfb45-208">La vista esegue quindi il rendering dei dati in formato HTML al browser.</span><span class="sxs-lookup"><span data-stu-id="dfb45-208">The view then renders the data as HTML to the browser.</span></span>

![Vista Privacy con un'etichetta di benvenuto e la frase Hello Rick riportata quattro volte](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

<span data-ttu-id="dfb45-210">Nell'esempio precedente è stato usato il dizionario `ViewData` per passare i dati dal controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="dfb45-210">In the sample above, the `ViewData` dictionary was used to pass data from the controller to a view.</span></span> <span data-ttu-id="dfb45-211">Più avanti nell'esercitazione si userà un modello di vista per passare i dati da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="dfb45-211">Later in the tutorial, a view model is used to pass data from a controller to a view.</span></span> <span data-ttu-id="dfb45-212">In genere l'approccio basato sul modello di vista per passare i dati è preferito rispetto all'approccio basato sul dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="dfb45-212">The view model approach to passing data is generally much preferred over the `ViewData` dictionary approach.</span></span> <span data-ttu-id="dfb45-213">Per altre informazioni, vedere [When to use ViewBag, ViewData, or TempData](http://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) (Quando usare ViewBag, ViewData o TempData).</span><span class="sxs-lookup"><span data-stu-id="dfb45-213">See [When to use ViewBag, ViewData, or TempData](http://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) for more information.</span></span>

<span data-ttu-id="dfb45-214">Nella prossima esercitazione, viene creato un database di film.</span><span class="sxs-lookup"><span data-stu-id="dfb45-214">In the next tutorial, a database of movies is created.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dfb45-215">[Precedente](adding-controller.md)
> [Successivo](adding-model.md)</span><span class="sxs-lookup"><span data-stu-id="dfb45-215">[Previous](adding-controller.md)
[Next](adding-model.md)</span></span>
