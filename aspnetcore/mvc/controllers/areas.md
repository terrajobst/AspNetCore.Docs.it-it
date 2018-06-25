---
title: Aree in ASP.NET Core
author: rick-anderson
description: Informazioni sulle aree, una funzionalità di ASP.NET MVC che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni).
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: 3e998af42cd6209271495dd8dd97a8aed35717a4
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274827"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="55c2e-103">Aree in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55c2e-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="55c2e-104">Di [Dhananjay Kumar](https://twitter.com/debug_mode) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="55c2e-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="55c2e-105">Le aree sono una funzionalità di ASP.NET MVC che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni).</span><span class="sxs-lookup"><span data-stu-id="55c2e-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="55c2e-106">Usando le aree si crea una gerarchia per il routing aggiungendo un altro parametro di route, `area`, a `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="55c2e-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="55c2e-107">Le aree consentono di suddividere un'app Web ASP.NET Core MVC di grandi dimensioni in raggruppamenti funzionali più piccoli.</span><span class="sxs-lookup"><span data-stu-id="55c2e-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="55c2e-108">Un'area è in effetti una struttura MVC all'interno di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="55c2e-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="55c2e-109">In un progetto MVC i componenti logici come modello, controller e visualizzazione si trovano in cartelle diverse e MVC crea una relazione tra questi componenti tramite convenzioni di denominazione.</span><span class="sxs-lookup"><span data-stu-id="55c2e-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="55c2e-110">Per un'app di grandi dimensioni può risultare utile suddividere l'app in aree di funzionalità di alto livello distinte.</span><span class="sxs-lookup"><span data-stu-id="55c2e-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="55c2e-111">È il caso, ad esempio, di un'app di e-commerce con più business unit, ad esempio per il completamento della transazione, la fatturazione, la ricerca e così via. Ognuna di queste business unit avrà le proprie visualizzazioni componenti, i propri controller e i propri modelli logici.</span><span class="sxs-lookup"><span data-stu-id="55c2e-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="55c2e-112">In questo scenario, è possibile usare le aree per suddividere fisicamente i componenti business all'interno dello stesso progetto.</span><span class="sxs-lookup"><span data-stu-id="55c2e-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="55c2e-113">Un'area può essere definita come un insieme di unità funzionali più piccole in un progetto ASP.NET Core MVC con un proprio set di controller, visualizzazioni e modelli.</span><span class="sxs-lookup"><span data-stu-id="55c2e-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="55c2e-114">In un progetto MVC è consigliabile usare le aree quando:</span><span class="sxs-lookup"><span data-stu-id="55c2e-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="55c2e-115">L'applicazione è costituita da più componenti funzionali di alto livello che devono rimanere logicamente separati</span><span class="sxs-lookup"><span data-stu-id="55c2e-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="55c2e-116">Si vuole suddividere il progetto MVC in modo da poter lavorare su ogni area funzionale in modo indipendente</span><span class="sxs-lookup"><span data-stu-id="55c2e-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="55c2e-117">Caratteristiche delle aree:</span><span class="sxs-lookup"><span data-stu-id="55c2e-117">Area features:</span></span>

* <span data-ttu-id="55c2e-118">Un'app ASP.NET Core MVC può avere un numero qualsiasi di aree</span><span class="sxs-lookup"><span data-stu-id="55c2e-118">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="55c2e-119">Ogni area ha un proprio controller, nonché modelli e visualizzazioni propri</span><span class="sxs-lookup"><span data-stu-id="55c2e-119">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="55c2e-120">Consente di organizzare progetti MVC di grandi dimensioni in più componenti di alto livello che è possibile usare in modo indipendente</span><span class="sxs-lookup"><span data-stu-id="55c2e-120">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="55c2e-121">Supporta più controller con lo stesso nome, purché abbiano *aree* diverse</span><span class="sxs-lookup"><span data-stu-id="55c2e-121">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="55c2e-122">Di seguito viene presentato un esempio di creazione e di uso delle aree.</span><span class="sxs-lookup"><span data-stu-id="55c2e-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="55c2e-123">Si supponga di avere un'app di vendita con due raggruppamenti distinti di controller e visualizzazioni: Products e Services.</span><span class="sxs-lookup"><span data-stu-id="55c2e-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="55c2e-124">Una struttura di cartelle tipica per questo tipo di uso delle MVC è la seguente:</span><span class="sxs-lookup"><span data-stu-id="55c2e-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="55c2e-125">Nome progetto</span><span class="sxs-lookup"><span data-stu-id="55c2e-125">Project name</span></span>

  * <span data-ttu-id="55c2e-126">Aree</span><span class="sxs-lookup"><span data-stu-id="55c2e-126">Areas</span></span>

    * <span data-ttu-id="55c2e-127">Prodotti</span><span class="sxs-lookup"><span data-stu-id="55c2e-127">Products</span></span>

      * <span data-ttu-id="55c2e-128">Controllers</span><span class="sxs-lookup"><span data-stu-id="55c2e-128">Controllers</span></span>

        * <span data-ttu-id="55c2e-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="55c2e-129">HomeController.cs</span></span>

        * <span data-ttu-id="55c2e-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="55c2e-130">ManageController.cs</span></span>

      * <span data-ttu-id="55c2e-131">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="55c2e-131">Views</span></span>

        * <span data-ttu-id="55c2e-132">Home</span><span class="sxs-lookup"><span data-stu-id="55c2e-132">Home</span></span>

          * <span data-ttu-id="55c2e-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="55c2e-133">Index.cshtml</span></span>

        * <span data-ttu-id="55c2e-134">Gestisci</span><span class="sxs-lookup"><span data-stu-id="55c2e-134">Manage</span></span>

          * <span data-ttu-id="55c2e-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="55c2e-135">Index.cshtml</span></span>

    * <span data-ttu-id="55c2e-136">Servizi</span><span class="sxs-lookup"><span data-stu-id="55c2e-136">Services</span></span>

      * <span data-ttu-id="55c2e-137">Controllers</span><span class="sxs-lookup"><span data-stu-id="55c2e-137">Controllers</span></span>

        * <span data-ttu-id="55c2e-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="55c2e-138">HomeController.cs</span></span>

      * <span data-ttu-id="55c2e-139">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="55c2e-139">Views</span></span>

        * <span data-ttu-id="55c2e-140">Home</span><span class="sxs-lookup"><span data-stu-id="55c2e-140">Home</span></span>

          * <span data-ttu-id="55c2e-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="55c2e-141">Index.cshtml</span></span>

<span data-ttu-id="55c2e-142">Quando MVC tenta di eseguire il rendering di una visualizzazione in un'area, per impostazione predefinita cerca nei percorsi seguenti:</span><span class="sxs-lookup"><span data-stu-id="55c2e-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="55c2e-143">Questi sono i percorsi predefiniti, che possono essere modificati tramite `AreaViewLocationFormats` in `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="55c2e-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="55c2e-144">Nel codice seguente, ad esempio, il nome della cartella 'Areas' è stato modificato in 'Categories'.</span><span class="sxs-lookup"><span data-stu-id="55c2e-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="55c2e-145">Si noti che in questo esempio la struttura della cartella *Views* è l'unica considerata importante. Il contenuto delle altre cartelle, ad esempio *Controllers* e *Models* **non** è rilevante.</span><span class="sxs-lookup"><span data-stu-id="55c2e-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="55c2e-146">Non è assolutamente necessario, ad esempio, che esistano le cartelle *Controllers* e *Models*.</span><span class="sxs-lookup"><span data-stu-id="55c2e-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="55c2e-147">Questo procedimento funziona perché il contenuto di *Controllers* e *Models* è solo codice che viene compilato in un file con estensione dll, mentre il contenuto di *Views* non lo è finché non viene effettuata una richiesta a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="55c2e-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="55c2e-148">Dopo aver definito la gerarchia di cartelle, è necessario informare MVC che ogni controller è associato a un'area.</span><span class="sxs-lookup"><span data-stu-id="55c2e-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="55c2e-149">A tale scopo, si decora il nome del controller con l'attributo `[Area]`.</span><span class="sxs-lookup"><span data-stu-id="55c2e-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

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

<span data-ttu-id="55c2e-150">Impostare la definizione di una route che funzioni con le aree appena create.</span><span class="sxs-lookup"><span data-stu-id="55c2e-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="55c2e-151">L'articolo [Route ad azioni del controller](routing.md) descrive in dettaglio come creare definizioni di route, nonché come usare route convenzionali rispetto a route di attributi.</span><span class="sxs-lookup"><span data-stu-id="55c2e-151">The [Route to controller actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="55c2e-152">In questo esempio verrà usata una route convenzionale.</span><span class="sxs-lookup"><span data-stu-id="55c2e-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="55c2e-153">A tale scopo, aprire il file *Startup.cs* e modificarlo aggiungendo la definizione di route denominata `areaRoute` riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="55c2e-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

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

<span data-ttu-id="55c2e-154">Se si passa a `http://<yourApp>/products`, viene richiamato il metodo di azione `Index` di `HomeController` nell'area `Products`.</span><span class="sxs-lookup"><span data-stu-id="55c2e-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="55c2e-155">Generazione di collegamenti</span><span class="sxs-lookup"><span data-stu-id="55c2e-155">Link Generation</span></span>

* <span data-ttu-id="55c2e-156">Generazione di collegamenti da un'azione all'interno di un controller basato su un'area a un'altra azione all'interno dello stesso controller.</span><span class="sxs-lookup"><span data-stu-id="55c2e-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="55c2e-157">Si supponga che il percorso della richiesta corrente sia `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="55c2e-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="55c2e-158">Sintassi di HtmlHelper: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="55c2e-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="55c2e-159">Sintassi di TagHelper: `<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="55c2e-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="55c2e-160">Si noti che non è necessario specificare qui i valori 'area' e 'controller', perché sono già disponibili nel contesto della richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="55c2e-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="55c2e-161">Valori di questo tipo sono detti valori `ambient`.</span><span class="sxs-lookup"><span data-stu-id="55c2e-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="55c2e-162">Generazione di collegamenti da un'azione all'interno di un controller basato su un'area a un'altra azione in un controller diverso</span><span class="sxs-lookup"><span data-stu-id="55c2e-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="55c2e-163">Si supponga che il percorso della richiesta corrente sia `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="55c2e-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="55c2e-164">Sintassi di HtmlHelper: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="55c2e-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="55c2e-165">Sintassi di TagHelper: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="55c2e-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="55c2e-166">Si noti che qui è usato un valore ambient 'area', mentre il valore 'controller' è specificato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="55c2e-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="55c2e-167">Generazione di collegamenti da un'azione all'interno di un controller basato su un'area a un'altra azione in un altro controller e in un'altra area.</span><span class="sxs-lookup"><span data-stu-id="55c2e-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="55c2e-168">Si supponga che il percorso della richiesta corrente sia `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="55c2e-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="55c2e-169">Sintassi di HtmlHelper: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="55c2e-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="55c2e-170">Sintassi di TagHelper: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="55c2e-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span></span>

  <span data-ttu-id="55c2e-171">Si noti che in questo caso non vengono usati valori ambient.</span><span class="sxs-lookup"><span data-stu-id="55c2e-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="55c2e-172">Generazione di collegamenti da un'azione all'interno di un controller basato su un'area a un'altra azione in un altro controller e **non** in un'area.</span><span class="sxs-lookup"><span data-stu-id="55c2e-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="55c2e-173">Sintassi di HtmlHelper: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="55c2e-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="55c2e-174">Sintassi di TagHelper: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="55c2e-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="55c2e-175">Poiché si vogliono generare collegamenti a un'azione del controller non basato su un'area, in questo caso il valore ambient di 'area' viene svuotato.</span><span class="sxs-lookup"><span data-stu-id="55c2e-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="55c2e-176">Pubblicazione di aree</span><span class="sxs-lookup"><span data-stu-id="55c2e-176">Publishing Areas</span></span>

<span data-ttu-id="55c2e-177">Tutti i file `*.cshtml` e `wwwroot/**` vengono pubblicati per l'output quando `<Project Sdk="Microsoft.NET.Sdk.Web">` viene incluso nel file con estensione *csproj*.</span><span class="sxs-lookup"><span data-stu-id="55c2e-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
