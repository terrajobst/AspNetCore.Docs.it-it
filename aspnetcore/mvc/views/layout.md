---
title: Layout
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 29f12d1f-9734-48bd-bf1a-cee53a8ab700
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: 064621d8756b007c5b8859111bf3a03a0d7dda81
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="layout"></a><span data-ttu-id="c120a-103">Layout</span><span class="sxs-lookup"><span data-stu-id="c120a-103">Layout</span></span>

<span data-ttu-id="c120a-104">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c120a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c120a-105">Viste spesso condividono elementi visivi e a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="c120a-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="c120a-106">In questo articolo si apprenderà come usare layout comuni, condividere direttive ed eseguire codice comune prima di rendering delle visualizzazioni nell'applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c120a-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="c120a-107">Che cos'è un Layout</span><span class="sxs-lookup"><span data-stu-id="c120a-107">What is a Layout</span></span>

<span data-ttu-id="c120a-108">La maggior parte delle applicazioni web presentano un layout comune che fornisce all'utente un'esperienza coerente, come si spostano da una pagina.</span><span class="sxs-lookup"><span data-stu-id="c120a-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="c120a-109">In genere, il layout include elementi dell'interfaccia utente comune, ad esempio l'intestazione di app, la navigazione o elementi di menu e piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="c120a-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Esempio di Layout di pagina](layout/_static/page-layout.png)

<span data-ttu-id="c120a-111">Strutture HTML comuni, ad esempio script e fogli di stile vengono spesso utilizzate anche dal numero di pagine all'interno di un'app.</span><span class="sxs-lookup"><span data-stu-id="c120a-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="c120a-112">Tutti questi elementi di lavoro condivisa può essere definiti un *layout* file, è possibile fare riferimento da qualsiasi visualizzazione utilizzata all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="c120a-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="c120a-113">Layout di ridurre il codice duplicato in viste, consentendo loro seguire il [principio non ripetere manualmente (sorgente)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="c120a-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="c120a-114">Per convenzione, il layout predefinito per un'app ASP.NET denominato `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="c120a-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="c120a-115">Il modello di progetto di Visual Studio ASP.NET Core MVC include il file di layout nel `Views/Shared` cartella:</span><span class="sxs-lookup"><span data-stu-id="c120a-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![cartella Views in Esplora soluzioni](layout/_static/web-project-views.png)

<span data-ttu-id="c120a-117">Questo layout definisce un modello di livello superiore per le viste nell'app.</span><span class="sxs-lookup"><span data-stu-id="c120a-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="c120a-118">App non richiedono un layout e le applicazioni è possono definire più di un layout, con diverse visualizzazioni specificare layout diversi.</span><span class="sxs-lookup"><span data-stu-id="c120a-118">Apps do not require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="c120a-119">Un esempio `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="c120a-119">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="c120a-120">Specificare un Layout</span><span class="sxs-lookup"><span data-stu-id="c120a-120">Specifying a Layout</span></span>

<span data-ttu-id="c120a-121">Visualizzazioni Razor hanno un `Layout` proprietà.</span><span class="sxs-lookup"><span data-stu-id="c120a-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="c120a-122">Singole visualizzazioni per specificare un layout, impostazione di questa proprietà:</span><span class="sxs-lookup"><span data-stu-id="c120a-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="c120a-123">Il layout specificato è possibile utilizzare un percorso completo (esempio: `/Views/Shared/_Layout.cshtml`) o un nome parziale (esempio: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="c120a-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="c120a-124">Quando viene fornito un nome parziale, il motore di visualizzazione Razor cercherà il file di layout tramite il processo di individuazione standard.</span><span class="sxs-lookup"><span data-stu-id="c120a-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="c120a-125">La cartella associata al controller viene eseguita la ricerca in primo luogo, aggiungendo il `Shared` cartella.</span><span class="sxs-lookup"><span data-stu-id="c120a-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="c120a-126">Questo processo di individuazione è identico a quello utilizzato per individuare [visualizzazioni parziali](partial.md).</span><span class="sxs-lookup"><span data-stu-id="c120a-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="c120a-127">Per impostazione predefinita, è necessario chiamare ogni layout `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="c120a-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="c120a-128">Quando la chiamata a `RenderBody` è inserito, verrà visualizzato il contenuto della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c120a-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="c120a-129">Sezioni</span><span class="sxs-lookup"><span data-stu-id="c120a-129">Sections</span></span>

<span data-ttu-id="c120a-130">Un layout può fare riferimento facoltativamente uno o più *sezioni*, chiamando `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="c120a-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="c120a-131">Che forniscono un modo per organizzare in cui determinati elementi di pagina devono essere inseriti.</span><span class="sxs-lookup"><span data-stu-id="c120a-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="c120a-132">Ogni chiamata a `RenderSection` possibile specificare se tale sezione è obbligatoria o facoltativa.</span><span class="sxs-lookup"><span data-stu-id="c120a-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="c120a-133">Se una sezione richiesta non viene trovata, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="c120a-133">If a required section is not found, an exception will be thrown.</span></span> <span data-ttu-id="c120a-134">Viste singole specificare il contenuto da sottoporre a rendering all'interno di una sezione con il `@section` sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="c120a-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="c120a-135">Se una vista definisce una sezione, è necessario eseguire il rendering o si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="c120a-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="c120a-136">Un esempio `@section` definizione in una vista:</span><span class="sxs-lookup"><span data-stu-id="c120a-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="c120a-137">Nel codice precedente, vengono aggiunti gli script di convalida per il `scripts` sezione in una vista che include un modulo.</span><span class="sxs-lookup"><span data-stu-id="c120a-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="c120a-138">Altre visualizzazioni nella stessa applicazione possono non richiedere eventuali altri script e pertanto non sarà necessario definire una sezione di script.</span><span class="sxs-lookup"><span data-stu-id="c120a-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="c120a-139">Nelle sezioni definite in una vista sono disponibili solo nella relativa pagina di layout immediato.</span><span class="sxs-lookup"><span data-stu-id="c120a-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="c120a-140">È possibile farvi riferimento da parziali, Visualizza i componenti o da altre parti del sistema di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c120a-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="c120a-141">Ignorare le sezioni</span><span class="sxs-lookup"><span data-stu-id="c120a-141">Ignoring sections</span></span>

<span data-ttu-id="c120a-142">Per impostazione predefinita, il corpo e tutte le sezioni in una pagina di contenuto devono tutti essere rendering pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="c120a-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="c120a-143">Il motore di visualizzazione Razor questo comportamento viene imposto da verificare se il corpo e ogni sezione è stato eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="c120a-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="c120a-144">Per indicare al motore di visualizzazione per ignorare il corpo o sezioni, chiamare il `IgnoreBody` e `IgnoreSection` metodi.</span><span class="sxs-lookup"><span data-stu-id="c120a-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="c120a-145">Il corpo e tutte le sezioni in una pagina Razor deve essere eseguito il rendering o ignorati.</span><span class="sxs-lookup"><span data-stu-id="c120a-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="c120a-146">L'importazione delle direttive condivise</span><span class="sxs-lookup"><span data-stu-id="c120a-146">Importing Shared Directives</span></span>

<span data-ttu-id="c120a-147">Viste è possono utilizzare le direttive Razor per eseguire molte operazioni, ad esempio l'importazione di spazi dei nomi o l'esecuzione di [inserimento di dipendenze](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="c120a-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="c120a-148">Direttive condivise da numerose visualizzazioni possono essere specificate in una comune `_ViewImports.cshtml` file.</span><span class="sxs-lookup"><span data-stu-id="c120a-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="c120a-149">Il `_ViewImports` file supporta le direttive seguenti:</span><span class="sxs-lookup"><span data-stu-id="c120a-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="c120a-150">Il file non supporta altre funzionalità di Razor, ad esempio le funzioni e le definizioni di sezione.</span><span class="sxs-lookup"><span data-stu-id="c120a-150">The file does not support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="c120a-151">Un esempio `_ViewImports.cshtml` file:</span><span class="sxs-lookup"><span data-stu-id="c120a-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="c120a-152">Il `_ViewImports.cshtml` file per un'applicazione ASP.NET MVC di base in genere viene posizionata nel `Views` cartella.</span><span class="sxs-lookup"><span data-stu-id="c120a-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="c120a-153">Oggetto `_ViewImports.cshtml` file può essere posizionato in qualsiasi cartella, nel quale caso verranno applicata solo a viste all'interno di tale cartella e nelle relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="c120a-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="c120a-154">`_ViewImports`i file vengono elaborati a partire a livello di radice, e quindi per ogni cartella iniziali fino al percorso della visualizzazione stessa, che le impostazioni specificate a livello di radice può essere sottoposto a override a livello di cartella.</span><span class="sxs-lookup"><span data-stu-id="c120a-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="c120a-155">Ad esempio, se un livello di radice `_ViewImports.cshtml` file specifica `@model` e `@addTagHelper`e un altro `_ViewImports.cshtml` specifica un altro file nella cartella della visualizzazione associata al controller `@model` e aggiunge un altro `@addTagHelper`, la visualizzazione sarà possibile accedere a entrambi gli helper di tag e utilizzerà il secondo `@model`.</span><span class="sxs-lookup"><span data-stu-id="c120a-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="c120a-156">Se più `_ViewImports.cshtml` i file vengono eseguiti per una vista, combinati comportamento delle direttive di cui è incluso nel `ViewImports.cshtml` file saranno come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c120a-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="c120a-157">`@addTagHelper`, `@removeTagHelper`: eseguiti nell'ordine</span><span class="sxs-lookup"><span data-stu-id="c120a-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="c120a-158">`@tagHelperPrefix`: il più vicino alla vista esegue l'override di tutte le altre</span><span class="sxs-lookup"><span data-stu-id="c120a-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="c120a-159">`@model`: il più vicino alla vista esegue l'override di tutte le altre</span><span class="sxs-lookup"><span data-stu-id="c120a-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="c120a-160">`@inherits`: il più vicino alla vista esegue l'override di tutte le altre</span><span class="sxs-lookup"><span data-stu-id="c120a-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="c120a-161">`@using`: tutti sono inclusi. i duplicati vengono ignorati</span><span class="sxs-lookup"><span data-stu-id="c120a-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="c120a-162">`@inject`: per ogni proprietà, il più vicino alla vista esegue l'override di tutti gli altri con lo stesso nome di proprietà</span><span class="sxs-lookup"><span data-stu-id="c120a-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="c120a-163">Esecuzione di codice prima di ogni visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c120a-163">Running Code Before Each View</span></span>

<span data-ttu-id="c120a-164">Se si dispone di codice è necessario eseguire prima di ogni visualizzazione, questo deve essere inserito nel `_ViewStart.cshtml` file.</span><span class="sxs-lookup"><span data-stu-id="c120a-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="c120a-165">Per convenzione, il `_ViewStart.cshtml` file si trova nella `Views` cartella.</span><span class="sxs-lookup"><span data-stu-id="c120a-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="c120a-166">Le istruzioni nella `_ViewStart.cshtml` vengono eseguiti prima di ogni visualizzazione completa (non di layout e le visualizzazioni parziali non).</span><span class="sxs-lookup"><span data-stu-id="c120a-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="c120a-167">Ad esempio [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` è di tipo gerarchico.</span><span class="sxs-lookup"><span data-stu-id="c120a-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="c120a-168">Se un `_ViewStart.cshtml` file è definito nella cartella di visualizzazione associata al controller, verrà eseguito dopo quello definito nella radice di `Views` cartella (se presente).</span><span class="sxs-lookup"><span data-stu-id="c120a-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="c120a-169">Un esempio `_ViewStart.cshtml` file:</span><span class="sxs-lookup"><span data-stu-id="c120a-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="c120a-170">Il file precedente specifica che tutte le viste useranno il `_Layout.cshtml` layout.</span><span class="sxs-lookup"><span data-stu-id="c120a-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="c120a-171">Né `_ViewStart.cshtml` né `_ViewImports.cshtml` vengono generalmente posizionati il `/Views/Shared` cartella.</span><span class="sxs-lookup"><span data-stu-id="c120a-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="c120a-172">Le versioni a livello di applicazione di questi file devono essere inserite direttamente nella `/Views` cartella.</span><span class="sxs-lookup"><span data-stu-id="c120a-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
