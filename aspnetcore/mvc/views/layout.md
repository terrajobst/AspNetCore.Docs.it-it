---
title: Layout
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 3e9e5949d8940a33508e24f0da015b49b7ba468c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="layout"></a><span data-ttu-id="432a3-102">Layout</span><span class="sxs-lookup"><span data-stu-id="432a3-102">Layout</span></span>

<span data-ttu-id="432a3-103">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="432a3-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="432a3-104">Le visualizzazioni condividono spesso elementi visivi e a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="432a3-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="432a3-105">In questo articolo si apprenderà come usare layout comuni, condividere direttive ed eseguire codice comune prima di eseguire il rendering delle visualizzazioni nell'applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="432a3-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="432a3-106">Che cos'è il layout?</span><span class="sxs-lookup"><span data-stu-id="432a3-106">What is a Layout</span></span>

<span data-ttu-id="432a3-107">La maggior parte delle applicazioni web presenta un layout comune che fornisce all'utente un'esperienza omogenea, nel passare da una pagina a un'altra.</span><span class="sxs-lookup"><span data-stu-id="432a3-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="432a3-108">In genere, il layout comprende elementi dell'interfaccia utente comune, ad esempio l'intestazione dell'app, la navigazione o elementi di menu e piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="432a3-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Esempio di layout di pagina](layout/_static/page-layout.png)

<span data-ttu-id="432a3-110">Molte pagine all'interno di un'app utilizzano anche strutture HTML comuni, come script e fogli di stile.</span><span class="sxs-lookup"><span data-stu-id="432a3-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="432a3-111">Tutti questi elementi condivisi possono essere definiti in un file di *layout*, cui è possibile fare riferimento da qualsiasi visualizzazione utilizzata all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="432a3-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="432a3-112">I layout riducono il codice duplicato nelle visualizzazioni, consentendo di seguire il [principio Don't Repeat Yourself (DRY)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="432a3-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="432a3-113">Per convenzione, il layout predefinito per un'app ASP.NET è denominato `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="432a3-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="432a3-114">Il modello di progetto di Visual Studio ASP.NET Core MVC include questo file di layout nella cartella `Views/Shared`:</span><span class="sxs-lookup"><span data-stu-id="432a3-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![cartella visualizzazioni in Esplora soluzioni](layout/_static/web-project-views.png)

<span data-ttu-id="432a3-116">Questo layout definisce un modello di livello superiore per le visualizzazioni incluse nell'app.</span><span class="sxs-lookup"><span data-stu-id="432a3-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="432a3-117">Le app non richiedono un layout e possono definire più di un layout, con diverse visualizzazioni che specificano layout diversi.</span><span class="sxs-lookup"><span data-stu-id="432a3-117">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="432a3-118">Esempio di `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="432a3-118">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="432a3-119">Definizione di un layout</span><span class="sxs-lookup"><span data-stu-id="432a3-119">Specifying a Layout</span></span>

<span data-ttu-id="432a3-120">Le visualizzazioni Razor hanno una proprietà `Layout`.</span><span class="sxs-lookup"><span data-stu-id="432a3-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="432a3-121">Le visualizzazioni singole specificano un layout impostando la seguente proprietà:</span><span class="sxs-lookup"><span data-stu-id="432a3-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="432a3-122">Il layout specificato può utilizzare un percorso completo (esempio: `/Views/Shared/_Layout.cshtml`) o un nome parziale (esempio: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="432a3-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="432a3-123">Quando viene fornito un nome parziale, il motore di visualizzazione Razor cerca il file di layout tramite il processo di individuazione standard.</span><span class="sxs-lookup"><span data-stu-id="432a3-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="432a3-124">Viene innanzitutto cercata la cartella associata al controller, seguita dalla cartella `Shared`.</span><span class="sxs-lookup"><span data-stu-id="432a3-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="432a3-125">Questo processo di individuazione è identico a quello utilizzato per individuare le [visualizzazioni parziali](partial.md).</span><span class="sxs-lookup"><span data-stu-id="432a3-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="432a3-126">Per impostazione predefinita, ogni layout deve chiamare `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="432a3-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="432a3-127">Quando viene eseguita la chiamata a `RenderBody`, viene eseguito il rendering del contenuto della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="432a3-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="432a3-128">Sezioni</span><span class="sxs-lookup"><span data-stu-id="432a3-128">Sections</span></span>

<span data-ttu-id="432a3-129">Un layout può facoltativamente fare riferimento a una o più *sezioni*, chiamando `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="432a3-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="432a3-130">Le sezioni forniscono un modo per organizzare la posizione in cui devono essere inseriti determinati elementi di pagina.</span><span class="sxs-lookup"><span data-stu-id="432a3-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="432a3-131">Ogni chiamata a `RenderSection` può specificare se tale sezione è obbligatoria o facoltativa.</span><span class="sxs-lookup"><span data-stu-id="432a3-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="432a3-132">Se una sezione richiesta non viene trovata, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="432a3-132">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="432a3-133">Le viste singole specificano il contenuto da sottoporre a rendering all'interno di una sezione tramite la sintassi Razor `@section`.</span><span class="sxs-lookup"><span data-stu-id="432a3-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="432a3-134">Se una visualizzazione definisce una sezione, è necessario eseguirne il rendering (o si verifica un errore).</span><span class="sxs-lookup"><span data-stu-id="432a3-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="432a3-135">Esempio di definizione `@section` in una visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="432a3-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="432a3-136">Nel codice precedente vengono aggiunti script di convalida alla sezione `scripts` in una visualizzazione che include un modulo.</span><span class="sxs-lookup"><span data-stu-id="432a3-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="432a3-137">Altre visualizzazioni nella stessa applicazione potrebbero non richiedere altri script, pertanto non sarà necessario definire una sezione di script.</span><span class="sxs-lookup"><span data-stu-id="432a3-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="432a3-138">Le sezioni definite in una visualizzazione sono disponibili solo nella relativa pagina di layout immediato.</span><span class="sxs-lookup"><span data-stu-id="432a3-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="432a3-139">Non è possibile farvi riferimento da righe parzialmente eseguite, componenti di visualizzazione o altre parti del sistema di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="432a3-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="432a3-140">Esclusione di sezioni</span><span class="sxs-lookup"><span data-stu-id="432a3-140">Ignoring sections</span></span>

<span data-ttu-id="432a3-141">Per impostazione predefinita, la pagina di layout deve eseguire il rendering della parte principale e di tutte le sezioni di una pagina di contenuto.</span><span class="sxs-lookup"><span data-stu-id="432a3-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="432a3-142">Il motore di visualizzazione Razor impone questa operazione verificando se è stato eseguito il rendering della parte principale e di ogni sezione.</span><span class="sxs-lookup"><span data-stu-id="432a3-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="432a3-143">Per indicare al motore di visualizzazione di escludere la parte principale o le sezioni, chiamare i metodi `IgnoreBody` e `IgnoreSection`.</span><span class="sxs-lookup"><span data-stu-id="432a3-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="432a3-144">Deve essere eseguito il rendering della parte principale e di tutte le sezioni di una pagina Razor oppure devono essere escluse.</span><span class="sxs-lookup"><span data-stu-id="432a3-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="432a3-145">Importazione delle direttive condivise</span><span class="sxs-lookup"><span data-stu-id="432a3-145">Importing Shared Directives</span></span>

<span data-ttu-id="432a3-146">Le visualizzazioni possono utilizzare le direttive Razor per eseguire molte operazioni, ad esempio l'importazione di spazi dei nomi o l'esecuzione dell'[inserimento di dipendenze](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="432a3-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="432a3-147">Le direttive condivise da numerose visualizzazioni possono essere specificate in un file `_ViewImports.cshtml` comune.</span><span class="sxs-lookup"><span data-stu-id="432a3-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="432a3-148">Il file `_ViewImports` supporta le direttive seguenti:</span><span class="sxs-lookup"><span data-stu-id="432a3-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="432a3-149">Il file non supporta altre funzionalità di Razor, come le funzioni e le definizioni di sezione.</span><span class="sxs-lookup"><span data-stu-id="432a3-149">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="432a3-150">Esempio di file `_ViewImports.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="432a3-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="432a3-151">Il file `_ViewImports.cshtml` per un'applicazione ASP.NET Core MVC viene generalmente posizionato nella cartella `Views`.</span><span class="sxs-lookup"><span data-stu-id="432a3-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="432a3-152">È possibile collocare un file `_ViewImports.cshtml` in qualsiasi cartella, nel qual caso verrà applicato unicamente alle visualizzazioni all'interno di tale cartella e nelle relative sottocartelle.</span><span class="sxs-lookup"><span data-stu-id="432a3-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="432a3-153">I file `_ViewImports` vengono elaborati a partire dal livello radice e successivamente per ogni cartella collegata al percorso della visualizzazione stessa, pertanto le impostazioni specificate al livello radice possono essere sottoposte a override a livello di cartella.</span><span class="sxs-lookup"><span data-stu-id="432a3-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="432a3-154">Ad esempio, se un file `_ViewImports.cshtml` a livello radice specifica `@model` e `@addTagHelper`, e un altro file `_ViewImports.cshtml` nella cartella associata al controller della visualizzazione specifica un file `@model` diverso e aggiunge un altro `@addTagHelper`, la visualizzazione avrà accesso a entrambi gli helper di tag e userà il secondo `@model`.</span><span class="sxs-lookup"><span data-stu-id="432a3-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="432a3-155">Se vengono eseguiti più file `_ViewImports.cshtml` per una visualizzazione, il comportamento combinato delle direttive incluse nei file `ViewImports.cshtml` sarà:</span><span class="sxs-lookup"><span data-stu-id="432a3-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="432a3-156">`@addTagHelper`, `@removeTagHelper`: eseguiti nell'ordine</span><span class="sxs-lookup"><span data-stu-id="432a3-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="432a3-157">`@tagHelperPrefix`: il file più vicino alla visualizzazione esegue l'override di tutti gli altri</span><span class="sxs-lookup"><span data-stu-id="432a3-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="432a3-158">`@model`: il file più vicino alla visualizzazione esegue l'override di tutti gli altri</span><span class="sxs-lookup"><span data-stu-id="432a3-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="432a3-159">`@inherits`: il file più vicino alla visualizzazione esegue l'override di tutti gli altri</span><span class="sxs-lookup"><span data-stu-id="432a3-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="432a3-160">`@using`: vengono inclusi tutti; i duplicati vengono esclusi</span><span class="sxs-lookup"><span data-stu-id="432a3-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="432a3-161">`@inject`: per ogni proprietà, quella più vicina alla visualizzazione esegue l'override di tutte le altre con lo stesso nome di proprietà</span><span class="sxs-lookup"><span data-stu-id="432a3-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="432a3-162">Esecuzione di codice prima di ogni visualizzazione</span><span class="sxs-lookup"><span data-stu-id="432a3-162">Running Code Before Each View</span></span>

<span data-ttu-id="432a3-163">Se si dispone di un codice che è necessario eseguire prima di ogni visualizzazione, è necessario inserirlo nel file `_ViewStart.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="432a3-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="432a3-164">Per convenzione, il file `_ViewStart.cshtml` si trova nella cartella `Views`.</span><span class="sxs-lookup"><span data-stu-id="432a3-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="432a3-165">Le istruzioni elencate in `_ViewStart.cshtml` vengono eseguite prima di ogni visualizzazione completa (non dei layout e delle visualizzazioni parziali).</span><span class="sxs-lookup"><span data-stu-id="432a3-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="432a3-166">Come [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` è di tipo gerarchico.</span><span class="sxs-lookup"><span data-stu-id="432a3-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="432a3-167">Se un file `_ViewStart.cshtml` è definito nella cartella di visualizzazione associata al controller, verrà eseguito dopo quello definito nella radice della cartella `Views` (se presente).</span><span class="sxs-lookup"><span data-stu-id="432a3-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="432a3-168">Esempio di file `_ViewStart.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="432a3-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="432a3-169">Il file precedente specifica che tutte le visualizzazioni useranno il layout `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="432a3-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="432a3-170">Né `_ViewStart.cshtml` né `_ViewImports.cshtml` vengono generalmente posizionati nella cartella `/Views/Shared`.</span><span class="sxs-lookup"><span data-stu-id="432a3-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="432a3-171">Le versioni a livello di app di questi file devono essere inserite direttamente nella cartella `/Views`.</span><span class="sxs-lookup"><span data-stu-id="432a3-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
