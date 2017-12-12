---
title: Aree
author: rick-anderson
description: Viene illustrato come utilizzare le aree.
keywords: ASP.NET Core, aree, routing, le visualizzazioni
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 5e16d5e8-5696-4cb2-8ec7-d36be305c922
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: cd0302fa1668979df9bbd6cb36f82742d325c5e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="areas"></a><span data-ttu-id="725a8-104">Aree</span><span class="sxs-lookup"><span data-stu-id="725a8-104">Areas</span></span>

<span data-ttu-id="725a8-105">Da [Dhananjay Kumar](https://twitter.com/debug_mode) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="725a8-105">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="725a8-106">Le aree sono una funzionalità di ASP.NET MVC consentono di organizzare la funzionalità correlata in un gruppo come una struttura di cartelle (per le viste) e un spazio dei nomi separato (per il routing).</span><span class="sxs-lookup"><span data-stu-id="725a8-106">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="725a8-107">Utilizzo di aree consente di creare una gerarchia allo scopo di routing mediante l'aggiunta di un altro parametro di route, `area`a `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="725a8-107">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="725a8-108">Le aree consentono di suddividere un'app Web ASP.NET MVC di base di grandi dimensioni in raggruppamenti funzionali più piccoli.</span><span class="sxs-lookup"><span data-stu-id="725a8-108">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="725a8-109">Un'area è in effetti una struttura MVC all'interno di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="725a8-109">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="725a8-110">In un progetto MVC, vengono mantenuti i componenti logici come modello, Controller e visualizza in cartelle diverse e MVC utilizza le convenzioni di denominazione per creare la relazione tra questi componenti.</span><span class="sxs-lookup"><span data-stu-id="725a8-110">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="725a8-111">Per un'app di grandi dimensioni, può risultare utile per partizionare l'app in varie aree di livello elevate di funzionalità.</span><span class="sxs-lookup"><span data-stu-id="725a8-111">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="725a8-112">Ad esempio, un'applicazione di e-commerce con più unità aziendali, ad esempio l'estrazione, fatturazione e la ricerca e così via. Ognuna di queste unità sono le proprie viste componenti logici, controller e i modelli.</span><span class="sxs-lookup"><span data-stu-id="725a8-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="725a8-113">In questo scenario, è possibile utilizzare aree per suddividere fisicamente i componenti business nello stesso progetto.</span><span class="sxs-lookup"><span data-stu-id="725a8-113">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="725a8-114">Un'area può essere definita come unità funzionale più piccole in un progetto ASP.NET MVC di base con un proprio set di modelli, visualizzazioni e controller.</span><span class="sxs-lookup"><span data-stu-id="725a8-114">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="725a8-115">È consigliabile utilizzare le aree in MVC progetto quando:</span><span class="sxs-lookup"><span data-stu-id="725a8-115">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="725a8-116">L'applicazione è costituita da più componenti funzionali di alto livello che devono essere separati in modo logico</span><span class="sxs-lookup"><span data-stu-id="725a8-116">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="725a8-117">Si desidera partizionare il progetto MVC in modo che ogni area funzionale possa essere elaborata in modo indipendente</span><span class="sxs-lookup"><span data-stu-id="725a8-117">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="725a8-118">Funzionalità area:</span><span class="sxs-lookup"><span data-stu-id="725a8-118">Area features:</span></span>

* <span data-ttu-id="725a8-119">Un'applicazione ASP.NET MVC di base può avere qualsiasi numero di aree</span><span class="sxs-lookup"><span data-stu-id="725a8-119">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="725a8-120">Ogni area dispone di un proprio controller, modelli e visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="725a8-120">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="725a8-121">Consente di organizzare i progetti MVC di grandi dimensioni in più componenti di alto livello che possono essere svolte in modo indipendente</span><span class="sxs-lookup"><span data-stu-id="725a8-121">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="725a8-122">Supporta più controller con lo stesso nome, purché hanno diversi *aree*</span><span class="sxs-lookup"><span data-stu-id="725a8-122">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="725a8-123">Esaminiamo un esempio per illustrare come aree vengono create e utilizzate.</span><span class="sxs-lookup"><span data-stu-id="725a8-123">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="725a8-124">Si potrebbe avere una app store con due gruppi distinti di controller e visualizzazioni: prodotti e servizi.</span><span class="sxs-lookup"><span data-stu-id="725a8-124">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="725a8-125">Una tipico cartella struttura che utilizza aree MVC è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="725a8-125">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="725a8-126">Nome progetto</span><span class="sxs-lookup"><span data-stu-id="725a8-126">Project name</span></span>

  * <span data-ttu-id="725a8-127">Aree</span><span class="sxs-lookup"><span data-stu-id="725a8-127">Areas</span></span>

    * <span data-ttu-id="725a8-128">Prodotti</span><span class="sxs-lookup"><span data-stu-id="725a8-128">Products</span></span>

      * <span data-ttu-id="725a8-129">Controller</span><span class="sxs-lookup"><span data-stu-id="725a8-129">Controllers</span></span>

        * <span data-ttu-id="725a8-130">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="725a8-130">HomeController.cs</span></span>

        * <span data-ttu-id="725a8-131">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="725a8-131">ManageController.cs</span></span>

      * <span data-ttu-id="725a8-132">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="725a8-132">Views</span></span>

        * <span data-ttu-id="725a8-133">Home</span><span class="sxs-lookup"><span data-stu-id="725a8-133">Home</span></span>

          * <span data-ttu-id="725a8-134">Cshtml</span><span class="sxs-lookup"><span data-stu-id="725a8-134">Index.cshtml</span></span>

        * <span data-ttu-id="725a8-135">Gestisci</span><span class="sxs-lookup"><span data-stu-id="725a8-135">Manage</span></span>

          * <span data-ttu-id="725a8-136">Cshtml</span><span class="sxs-lookup"><span data-stu-id="725a8-136">Index.cshtml</span></span>

    * <span data-ttu-id="725a8-137">Servizi</span><span class="sxs-lookup"><span data-stu-id="725a8-137">Services</span></span>

      * <span data-ttu-id="725a8-138">Controller</span><span class="sxs-lookup"><span data-stu-id="725a8-138">Controllers</span></span>

        * <span data-ttu-id="725a8-139">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="725a8-139">HomeController.cs</span></span>

      * <span data-ttu-id="725a8-140">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="725a8-140">Views</span></span>

        * <span data-ttu-id="725a8-141">Home</span><span class="sxs-lookup"><span data-stu-id="725a8-141">Home</span></span>

          * <span data-ttu-id="725a8-142">Cshtml</span><span class="sxs-lookup"><span data-stu-id="725a8-142">Index.cshtml</span></span>

<span data-ttu-id="725a8-143">Quando MVC tenta di eseguire il rendering di una vista in un'Area, per impostazione predefinita, tenta di ricerca nei percorsi seguenti:</span><span class="sxs-lookup"><span data-stu-id="725a8-143">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="725a8-144">Questi sono i percorsi predefiniti che possono essere modificati tramite il `AreaViewLocationFormats` sul `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="725a8-144">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="725a8-145">Ad esempio, nel seguente codice anziché il nome della cartella come 'Aree', che è stato modificato per 'Categorie'.</span><span class="sxs-lookup"><span data-stu-id="725a8-145">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="725a8-146">Si noti che sono la struttura del *viste* cartella è l'unico che è considerata importante qui e il contenuto della parte restante delle cartelle come *controller* e *modelli* does **non** è rilevante.</span><span class="sxs-lookup"><span data-stu-id="725a8-146">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="725a8-147">Ad esempio, è necessario non è un *controller* e *modelli* cartella affatto.</span><span class="sxs-lookup"><span data-stu-id="725a8-147">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="725a8-148">Questo procedimento funziona perché il contenuto di *controller* e *modelli* è solo il codice che viene compilato in una DLL in cui come contenuto del *viste* non è presente fino a quando una richiesta a quella visualizzazione è stata trovata.</span><span class="sxs-lookup"><span data-stu-id="725a8-148">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* is not until a request to that view has been made.</span></span>

<span data-ttu-id="725a8-149">Dopo aver definito la gerarchia di cartelle, è necessario che ogni controller è associata a un'area per MVC.</span><span class="sxs-lookup"><span data-stu-id="725a8-149">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="725a8-150">A tale scopo, aggiungendo il nome del controller con la `[Area]` attributo.</span><span class="sxs-lookup"><span data-stu-id="725a8-150">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

<span data-ttu-id="725a8-151">Consente di impostare una definizione della route che funziona con le aree appena create.</span><span class="sxs-lookup"><span data-stu-id="725a8-151">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="725a8-152">Il [Routing alle azioni del Controller](routing.md) articolo si analizzano in dettaglio come creare definizioni di route, incluso l'utilizzo convenzionale route rispetto route di attributi.</span><span class="sxs-lookup"><span data-stu-id="725a8-152">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="725a8-153">In questo esempio, si userà una route convenzionale.</span><span class="sxs-lookup"><span data-stu-id="725a8-153">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="725a8-154">A tale scopo, aprire il *Startup.cs* file e modificarlo aggiungendovi il `areaRoute` denominato definizione route riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="725a8-154">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

<span data-ttu-id="725a8-155">La selezione di `http://<yourApp>/products`, `Index` il metodo di azione del `HomeController` nel `Products` area verrà richiamata.</span><span class="sxs-lookup"><span data-stu-id="725a8-155">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="725a8-156">Generazione di collegamento</span><span class="sxs-lookup"><span data-stu-id="725a8-156">Link Generation</span></span>

* <span data-ttu-id="725a8-157">Generazione di collegamenti da un'azione all'interno di un'area in base a un'altra azione all'interno del controller stesso controller di.</span><span class="sxs-lookup"><span data-stu-id="725a8-157">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="725a8-158">Si supponga che il percorso della richiesta corrente è simile a`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="725a8-158">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="725a8-159">Sintassi HtmlHelper:`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="725a8-159">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="725a8-160">Sintassi di helper tag:`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="725a8-160">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="725a8-161">Si noti che non sia base fornire i valori "area" e "controller" di seguito sono già disponibili nel contesto della richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="725a8-161">Note that we need not supply the 'area' and 'controller' values here as they are already available in the context of the current request.</span></span> <span data-ttu-id="725a8-162">Questo tipo di valori denominati `ambient` valori.</span><span class="sxs-lookup"><span data-stu-id="725a8-162">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="725a8-163">Generazione di collegamenti da un'azione all'interno di un'area in base a un'altra azione su un controller diverso controller di</span><span class="sxs-lookup"><span data-stu-id="725a8-163">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="725a8-164">Si supponga che il percorso della richiesta corrente è simile a`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="725a8-164">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="725a8-165">Sintassi HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="725a8-165">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="725a8-166">Sintassi di helper tag:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="725a8-166">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="725a8-167">Si noti che questo ambito viene usato il valore di ambiente di un'area' ', ma è specificato in modo esplicito il valore "controller" sopra.</span><span class="sxs-lookup"><span data-stu-id="725a8-167">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="725a8-168">Generazione di collegamenti da un'azione all'interno di un'area in base all'azione di un altro controller di un altro controller e un'area diversa.</span><span class="sxs-lookup"><span data-stu-id="725a8-168">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="725a8-169">Si supponga che il percorso della richiesta corrente è simile a`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="725a8-169">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="725a8-170">Sintassi HtmlHelper:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="725a8-170">HtmlHelper syntax: `@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="725a8-171">Sintassi di helper tag:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="725a8-171">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span></span>

  <span data-ttu-id="725a8-172">Si noti che in questo caso in cui non vengono utilizzati valori di ambiente.</span><span class="sxs-lookup"><span data-stu-id="725a8-172">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="725a8-173">Generazione di collegamenti da un'azione all'interno di un controller di area in base a un'altra azione su un altro controller e **non** in un'area.</span><span class="sxs-lookup"><span data-stu-id="725a8-173">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="725a8-174">Sintassi HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="725a8-174">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="725a8-175">Sintassi di helper tag:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="725a8-175">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="725a8-176">Poiché si desidera generare collegamenti a un'area di non-azione del controller, è vuoto in base al valore di ambiente 'area' qui.</span><span class="sxs-lookup"><span data-stu-id="725a8-176">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="725a8-177">Aree di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="725a8-177">Publishing Areas</span></span>

<span data-ttu-id="725a8-178">Tutti `*.cshtml` e `wwwroot/**` i file vengono pubblicati di output quando `<Project Sdk="Microsoft.NET.Sdk.Web">` è incluso nel *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="725a8-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
