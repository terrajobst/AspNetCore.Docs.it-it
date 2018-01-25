---
title: Layout
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: e268f045e39188e9cc1e759ff7e6c553662dd669
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="layout"></a><span data-ttu-id="b530c-102">Layout</span><span class="sxs-lookup"><span data-stu-id="b530c-102">Layout</span></span>

<span data-ttu-id="b530c-103">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b530c-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b530c-104">Viste spesso condividono elementi visivi e a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="b530c-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="b530c-105">In questo articolo si apprenderà come usare layout comuni, condividere direttive ed eseguire codice comune prima di rendering delle visualizzazioni nell'applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b530c-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="b530c-106">Che cos'è un Layout</span><span class="sxs-lookup"><span data-stu-id="b530c-106">What is a Layout</span></span>

<span data-ttu-id="b530c-107">La maggior parte delle applicazioni web presentano un layout comune che fornisce all'utente un'esperienza coerente, come si spostano da una pagina.</span><span class="sxs-lookup"><span data-stu-id="b530c-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="b530c-108">In genere, il layout include elementi dell'interfaccia utente comune, ad esempio l'intestazione di app, la navigazione o elementi di menu e piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="b530c-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Esempio di Layout di pagina](layout/_static/page-layout.png)

<span data-ttu-id="b530c-110">Strutture HTML comuni, ad esempio script e fogli di stile vengono spesso utilizzate anche dal numero di pagine all'interno di un'app.</span><span class="sxs-lookup"><span data-stu-id="b530c-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="b530c-111">Tutti questi elementi di lavoro condivisa può essere definiti un *layout* file, è possibile fare riferimento da qualsiasi visualizzazione utilizzata all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="b530c-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="b530c-112">Layout di ridurre il codice duplicato in viste, consentendo loro seguire il [principio non ripetere manualmente (sorgente)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="b530c-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="b530c-113">Per convenzione, il layout predefinito per un'app ASP.NET denominato `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="b530c-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="b530c-114">Il modello di progetto di Visual Studio ASP.NET Core MVC include il file di layout nel `Views/Shared` cartella:</span><span class="sxs-lookup"><span data-stu-id="b530c-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![cartella Views in Esplora soluzioni](layout/_static/web-project-views.png)

<span data-ttu-id="b530c-116">Questo layout definisce un modello di livello superiore per le viste nell'app.</span><span class="sxs-lookup"><span data-stu-id="b530c-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="b530c-117">Le app non richiedono un layout e le applicazioni è possono definire più di un layout, con diverse visualizzazioni specificare layout diversi.</span><span class="sxs-lookup"><span data-stu-id="b530c-117">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="b530c-118">Un esempio `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="b530c-118">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="b530c-119">Specificare un Layout</span><span class="sxs-lookup"><span data-stu-id="b530c-119">Specifying a Layout</span></span>

<span data-ttu-id="b530c-120">Visualizzazioni Razor hanno un `Layout` proprietà.</span><span class="sxs-lookup"><span data-stu-id="b530c-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="b530c-121">Singole visualizzazioni per specificare un layout, impostazione di questa proprietà:</span><span class="sxs-lookup"><span data-stu-id="b530c-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="b530c-122">Il layout specificato è possibile utilizzare un percorso completo (esempio: `/Views/Shared/_Layout.cshtml`) o un nome parziale (esempio: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="b530c-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="b530c-123">Quando viene fornito un nome parziale, il motore di visualizzazione Razor cercherà il file di layout tramite il processo di individuazione standard.</span><span class="sxs-lookup"><span data-stu-id="b530c-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="b530c-124">La cartella associata al controller viene eseguita la ricerca in primo luogo, aggiungendo il `Shared` cartella.</span><span class="sxs-lookup"><span data-stu-id="b530c-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="b530c-125">Questo processo di individuazione è identico a quello utilizzato per individuare [visualizzazioni parziali](partial.md).</span><span class="sxs-lookup"><span data-stu-id="b530c-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="b530c-126">Per impostazione predefinita, è necessario chiamare ogni layout `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="b530c-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="b530c-127">Quando la chiamata a `RenderBody` è inserito, verrà visualizzato il contenuto della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b530c-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="b530c-128">Sezioni</span><span class="sxs-lookup"><span data-stu-id="b530c-128">Sections</span></span>

<span data-ttu-id="b530c-129">Un layout può fare riferimento facoltativamente uno o più *sezioni*, chiamando `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="b530c-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="b530c-130">Che forniscono un modo per organizzare in cui determinati elementi di pagina devono essere inseriti.</span><span class="sxs-lookup"><span data-stu-id="b530c-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="b530c-131">Ogni chiamata a `RenderSection` possibile specificare se tale sezione è obbligatoria o facoltativa.</span><span class="sxs-lookup"><span data-stu-id="b530c-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="b530c-132">Se una sezione richiesta non viene trovata, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="b530c-132">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="b530c-133">Viste singole specificare il contenuto da sottoporre a rendering all'interno di una sezione con il `@section` sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="b530c-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="b530c-134">Se una vista definisce una sezione, è necessario eseguire il rendering o si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="b530c-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="b530c-135">Un esempio `@section` definizione in una vista:</span><span class="sxs-lookup"><span data-stu-id="b530c-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="b530c-136">Nel codice precedente, vengono aggiunti gli script di convalida per il `scripts` sezione in una vista che include un modulo.</span><span class="sxs-lookup"><span data-stu-id="b530c-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="b530c-137">Altre visualizzazioni nella stessa applicazione possono non richiedere eventuali altri script e pertanto non sarà necessario definire una sezione di script.</span><span class="sxs-lookup"><span data-stu-id="b530c-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="b530c-138">Nelle sezioni definite in una vista sono disponibili solo nella relativa pagina di layout immediato.</span><span class="sxs-lookup"><span data-stu-id="b530c-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="b530c-139">È possibile farvi riferimento da parziali, Visualizza i componenti o da altre parti del sistema di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b530c-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="b530c-140">Ignorare le sezioni</span><span class="sxs-lookup"><span data-stu-id="b530c-140">Ignoring sections</span></span>

<span data-ttu-id="b530c-141">Per impostazione predefinita, il corpo e tutte le sezioni in una pagina di contenuto devono tutti essere rendering pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="b530c-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="b530c-142">Il motore di visualizzazione Razor questo comportamento viene imposto da verificare se il corpo e ogni sezione è stato eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="b530c-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="b530c-143">Per indicare al motore di visualizzazione per ignorare il corpo o sezioni, chiamare il `IgnoreBody` e `IgnoreSection` metodi.</span><span class="sxs-lookup"><span data-stu-id="b530c-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="b530c-144">Il corpo e tutte le sezioni in una pagina Razor deve essere eseguito il rendering o ignorati.</span><span class="sxs-lookup"><span data-stu-id="b530c-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="b530c-145">L'importazione delle direttive condivise</span><span class="sxs-lookup"><span data-stu-id="b530c-145">Importing Shared Directives</span></span>

<span data-ttu-id="b530c-146">Viste è possono utilizzare le direttive Razor per eseguire molte operazioni, ad esempio l'importazione di spazi dei nomi o l'esecuzione di [inserimento di dipendenze](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="b530c-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="b530c-147">Direttive condivise da numerose visualizzazioni possono essere specificate in una comune `_ViewImports.cshtml` file.</span><span class="sxs-lookup"><span data-stu-id="b530c-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="b530c-148">Il `_ViewImports` file supporta le direttive seguenti:</span><span class="sxs-lookup"><span data-stu-id="b530c-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="b530c-149">Il file non supporta altre funzionalità di Razor, ad esempio le funzioni e le definizioni di sezione.</span><span class="sxs-lookup"><span data-stu-id="b530c-149">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="b530c-150">Un esempio `_ViewImports.cshtml` file:</span><span class="sxs-lookup"><span data-stu-id="b530c-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="b530c-151">Il `_ViewImports.cshtml` file per un'applicazione ASP.NET MVC di base in genere viene posizionata nel `Views` cartella.</span><span class="sxs-lookup"><span data-stu-id="b530c-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="b530c-152">Oggetto `_ViewImports.cshtml` file può essere posizionato in qualsiasi cartella, nel quale caso verranno applicata solo a viste all'interno di tale cartella e nelle relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="b530c-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="b530c-153">`_ViewImports`i file vengono elaborati a partire a livello di radice, e quindi per ogni cartella iniziali fino al percorso della visualizzazione stessa, che le impostazioni specificate a livello di radice può essere sottoposto a override a livello di cartella.</span><span class="sxs-lookup"><span data-stu-id="b530c-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="b530c-154">Ad esempio, se un livello di radice `_ViewImports.cshtml` file specifica `@model` e `@addTagHelper`e un altro `_ViewImports.cshtml` specifica un altro file nella cartella della visualizzazione associata al controller `@model` e aggiunge un altro `@addTagHelper`, la visualizzazione sarà possibile accedere a entrambi gli helper di tag e utilizzerà il secondo `@model`.</span><span class="sxs-lookup"><span data-stu-id="b530c-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="b530c-155">Se più `_ViewImports.cshtml` i file vengono eseguiti per una vista, combinati comportamento delle direttive di cui è incluso nel `ViewImports.cshtml` file saranno come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b530c-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="b530c-156">`@addTagHelper`, `@removeTagHelper`: eseguiti nell'ordine</span><span class="sxs-lookup"><span data-stu-id="b530c-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="b530c-157">`@tagHelperPrefix`: il più vicino alla vista esegue l'override di tutte le altre</span><span class="sxs-lookup"><span data-stu-id="b530c-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="b530c-158">`@model`: il più vicino alla vista esegue l'override di tutte le altre</span><span class="sxs-lookup"><span data-stu-id="b530c-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="b530c-159">`@inherits`: il più vicino alla vista esegue l'override di tutte le altre</span><span class="sxs-lookup"><span data-stu-id="b530c-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="b530c-160">`@using`: tutti sono inclusi. i duplicati vengono ignorati</span><span class="sxs-lookup"><span data-stu-id="b530c-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="b530c-161">`@inject`: per ogni proprietà, il più vicino alla vista esegue l'override di tutti gli altri con lo stesso nome di proprietà</span><span class="sxs-lookup"><span data-stu-id="b530c-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="b530c-162">Esecuzione di codice prima di ogni visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b530c-162">Running Code Before Each View</span></span>

<span data-ttu-id="b530c-163">Se si dispone di codice è necessario eseguire prima di ogni visualizzazione, questo deve essere inserito nel `_ViewStart.cshtml` file.</span><span class="sxs-lookup"><span data-stu-id="b530c-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="b530c-164">Per convenzione, il `_ViewStart.cshtml` file si trova nella `Views` cartella.</span><span class="sxs-lookup"><span data-stu-id="b530c-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="b530c-165">Le istruzioni nella `_ViewStart.cshtml` vengono eseguiti prima di ogni visualizzazione completa (non di layout e le visualizzazioni parziali non).</span><span class="sxs-lookup"><span data-stu-id="b530c-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="b530c-166">Ad esempio [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` è di tipo gerarchico.</span><span class="sxs-lookup"><span data-stu-id="b530c-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="b530c-167">Se un `_ViewStart.cshtml` file è definito nella cartella di visualizzazione associata al controller, verrà eseguito dopo quello definito nella radice di `Views` cartella (se presente).</span><span class="sxs-lookup"><span data-stu-id="b530c-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="b530c-168">Un esempio `_ViewStart.cshtml` file:</span><span class="sxs-lookup"><span data-stu-id="b530c-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="b530c-169">Il file precedente specifica che tutte le viste useranno il `_Layout.cshtml` layout.</span><span class="sxs-lookup"><span data-stu-id="b530c-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="b530c-170">Né `_ViewStart.cshtml` né `_ViewImports.cshtml` vengono generalmente posizionati il `/Views/Shared` cartella.</span><span class="sxs-lookup"><span data-stu-id="b530c-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="b530c-171">Le versioni a livello di applicazione di questi file devono essere inserite direttamente nella `/Views` cartella.</span><span class="sxs-lookup"><span data-stu-id="b530c-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
