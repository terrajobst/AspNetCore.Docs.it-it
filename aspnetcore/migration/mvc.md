---
title: Eseguire la migrazione da ASP.NET MVC ad ASP.NET Core MVC
author: ardalis
description: Informazioni su come iniziare a usare la migrazione di un progetto ASP.NET MVC ad ASP.NET Core MVC.
ms.author: riande
ms.date: 02/13/2019
uid: migration/mvc
ms.openlocfilehash: 2ca51a145243444722ad8081fd8cdbb65d72b53a
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248043"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="638bf-103">Eseguire la migrazione da ASP.NET MVC ad ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="638bf-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="638bf-104">Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="638bf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="638bf-105">Questo articolo illustra come iniziare a usare la migrazione di un progetto ASP.NET MVC [ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="638bf-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="638bf-106">Nel processo, verrà evidenziato gran parte delle operazioni che sono stati modificati da ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="638bf-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="638bf-107">La migrazione da ASP.NET MVC è un processo in più passaggi e questo articolo illustra la configurazione iniziale, i controller di base e viste, contenuto statico e le dipendenze dal lato client.</span><span class="sxs-lookup"><span data-stu-id="638bf-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="638bf-108">Articoli aggiuntivi illustrano migrazione della configurazione e il codice di identità disponibili in molti progetti ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="638bf-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="638bf-109">I numeri di versione indicato negli esempi potrebbero non essere aggiornati.</span><span class="sxs-lookup"><span data-stu-id="638bf-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="638bf-110">Si potrebbe essere necessario aggiornare di conseguenza i progetti.</span><span class="sxs-lookup"><span data-stu-id="638bf-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="638bf-111">Creare il progetto ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="638bf-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="638bf-112">Per illustrare l'aggiornamento, si inizierà creando un'app ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="638bf-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="638bf-113">Crearlo con il nome *App Web 1* in modo che lo spazio dei nomi corrisponde al progetto di ASP.NET Core verrà creata nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="638bf-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Finestra di dialogo di Visual Studio nuovo progetto](mvc/_static/new-project.png)

![Nuova finestra di dialogo dell'applicazione Web: Modello di progetto MVC selezionato nel Pannello di modelli ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="638bf-116">*Facoltativo:* Modificare il nome della soluzione da *App Web 1* al *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="638bf-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="638bf-117">Visual Studio visualizza il nuovo nome della soluzione (*Mvc5*), che rende più semplice indicare il progetto dal progetto successivo.</span><span class="sxs-lookup"><span data-stu-id="638bf-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="638bf-118">Creare il progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="638bf-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="638bf-119">Creare una nuova *vuote* app web ASP.NET Core con lo stesso nome del progetto precedente (*App Web 1*) in modo che corrispondano agli spazi dei nomi dei due progetti.</span><span class="sxs-lookup"><span data-stu-id="638bf-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="638bf-120">Lo stesso spazio dei nomi rende più semplice copiare codice tra i due progetti.</span><span class="sxs-lookup"><span data-stu-id="638bf-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="638bf-121">È possibile creare il progetto in una directory diversa da quella del progetto precedente per usare lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="638bf-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Finestra di dialogo Nuovo progetto](mvc/_static/new_core.png)

![Nuova finestra di dialogo dell'applicazione Web ASP.NET: Modello di progetto vuoto selezionato nel Pannello di modelli di ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="638bf-124">*Facoltativo:* Creare una nuova app ASP.NET Core usando il *applicazione Web* modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="638bf-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="638bf-125">Denominare il progetto *App Web 1*, quindi selezionare un'opzione di autenticazione di **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="638bf-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="638bf-126">Rinomina l'app per *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="638bf-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="638bf-127">Creazione di questo consente di risparmiare progetto tempo durante la conversione.</span><span class="sxs-lookup"><span data-stu-id="638bf-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="638bf-128">È possibile esaminare il codice modello generato per visualizzare il risultato finale o copiare codice al progetto di conversione.</span><span class="sxs-lookup"><span data-stu-id="638bf-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="638bf-129">È inoltre utile quando bloccarsi su un'istruzione di conversione da confrontare con il progetto di modello generato.</span><span class="sxs-lookup"><span data-stu-id="638bf-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="638bf-130">Configurare il sito per l'uso di MVC</span><span class="sxs-lookup"><span data-stu-id="638bf-130">Configure the site to use MVC</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="638bf-131">Quando la destinazione è .NET Core, il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) viene fatto riferimento per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="638bf-131">When targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) is referenced by default.</span></span> <span data-ttu-id="638bf-132">Questo pacchetto contiene i pacchetti di pacchetti comunemente usati dalle App MVC.</span><span class="sxs-lookup"><span data-stu-id="638bf-132">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="638bf-133">Se la destinazione è .NET Framework, riferimenti ai pacchetti deve essere elencate singolarmente nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="638bf-133">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="638bf-134">Quando la destinazione è .NET Core, il [metapacchetto Microsoft. aspnetcore](xref:fundamentals/metapackage) viene fatto riferimento per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="638bf-134">When targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) is referenced by default.</span></span> <span data-ttu-id="638bf-135">Questo pacchetto contiene i pacchetti di pacchetti comunemente usati dalle App MVC.</span><span class="sxs-lookup"><span data-stu-id="638bf-135">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="638bf-136">Se la destinazione è .NET Framework, riferimenti ai pacchetti deve essere elencate singolarmente nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="638bf-136">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="638bf-137">Quando la destinazione è .NET Core o .NET Framework, i pacchetti di pacchetti comunemente usati dalle App MVC sono elencati singolarmente nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="638bf-137">When targeting .NET Core or .NET Framework, packages commonly used packages by MVC apps are listed individually in the project file.</span></span>

::: moniker-end

<span data-ttu-id="638bf-138">`Microsoft.AspNetCore.Mvc` è il framework di ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="638bf-138">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="638bf-139">`Microsoft.AspNetCore.StaticFiles` è il gestore di file statici.</span><span class="sxs-lookup"><span data-stu-id="638bf-139">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="638bf-140">Il runtime di ASP.NET Core è modulare e deve esplicitamente acconsentire esplicitamente a usare i file statici (vedere [i file statici](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="638bf-140">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="638bf-141">Aprire il *Startup.cs* di file e modificare il codice in base a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="638bf-141">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="638bf-142">Il `UseStaticFiles` metodo di estensione aggiunge il gestore di file statici.</span><span class="sxs-lookup"><span data-stu-id="638bf-142">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="638bf-143">Come accennato in precedenza, il runtime di ASP.NET è modulare e deve esplicitamente acconsentire esplicitamente a usare i file statici.</span><span class="sxs-lookup"><span data-stu-id="638bf-143">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="638bf-144">Il `UseMvc` aggiunge il metodo di estensione routing.</span><span class="sxs-lookup"><span data-stu-id="638bf-144">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="638bf-145">Per altre informazioni, vedere [avvio dell'applicazione](xref:fundamentals/startup) e [Routing](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="638bf-145">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="638bf-146">Aggiungere un controller e visualizzazione</span><span class="sxs-lookup"><span data-stu-id="638bf-146">Add a controller and view</span></span>

<span data-ttu-id="638bf-147">In questa sezione si aggiungerà un controller di minima e una visualizzazione a essere utilizzati come segnaposti per i controller ASP.NET MVC e le viste che è possibile eseguire la migrazione nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="638bf-147">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="638bf-148">Aggiungere un *controller* cartella.</span><span class="sxs-lookup"><span data-stu-id="638bf-148">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="638bf-149">Aggiungere un **classe Controller** denominate *HomeController.cs* per il *controller* cartella.</span><span class="sxs-lookup"><span data-stu-id="638bf-149">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="638bf-151">Aggiungere un *viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="638bf-151">Add a *Views* folder.</span></span>

* <span data-ttu-id="638bf-152">Aggiungere un *Views/Home* cartella.</span><span class="sxs-lookup"><span data-stu-id="638bf-152">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="638bf-153">Aggiungere un **visualizzazione Razor** denominate *index. cshtml* per il *Views/Home* cartella.</span><span class="sxs-lookup"><span data-stu-id="638bf-153">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento](mvc/_static/view.png)

<span data-ttu-id="638bf-155">La struttura del progetto è la seguente:</span><span class="sxs-lookup"><span data-stu-id="638bf-155">The project structure is shown below:</span></span>

![Esplora soluzioni con file e cartelle dell'App Web 1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="638bf-157">Sostituire il contenuto del *Views/Home/Index.cshtml* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="638bf-157">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="638bf-158">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="638bf-158">Run the app.</span></span>

![App Web aperta in Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="638bf-160">Visualizzare [controller](xref:mvc/controllers/actions) e [viste](xref:mvc/views/overview) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="638bf-160">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="638bf-161">Ora che abbiamo un progetto ASP.NET Core lavoro minimo, è possibile iniziare la migrazione delle funzionalità dal progetto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="638bf-161">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="638bf-162">Sarà necessario passare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="638bf-162">We need to move the following:</span></span>

* <span data-ttu-id="638bf-163">contenuto sul lato client (CSS, i tipi di carattere e gli script)</span><span class="sxs-lookup"><span data-stu-id="638bf-163">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="638bf-164">controller</span><span class="sxs-lookup"><span data-stu-id="638bf-164">controllers</span></span>

* <span data-ttu-id="638bf-165">visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="638bf-165">views</span></span>

* <span data-ttu-id="638bf-166">modelli</span><span class="sxs-lookup"><span data-stu-id="638bf-166">models</span></span>

* <span data-ttu-id="638bf-167">Creazione di bundle</span><span class="sxs-lookup"><span data-stu-id="638bf-167">bundling</span></span>

* <span data-ttu-id="638bf-168">filtri</span><span class="sxs-lookup"><span data-stu-id="638bf-168">filters</span></span>

* <span data-ttu-id="638bf-169">Il log in ingresso/uscita, Identity (ciò viene eseguito nella prossima esercitazione.)</span><span class="sxs-lookup"><span data-stu-id="638bf-169">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="638bf-170">Controller e visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="638bf-170">Controllers and views</span></span>

* <span data-ttu-id="638bf-171">Ognuno dei metodi di copia da ASP.NET MVC `HomeController` alla nuova `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="638bf-171">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="638bf-172">Si noti che in ASP.NET MVC, tipo restituito del metodo del modello incorporato controller azione sia [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, i metodi di azione restituiti `IActionResult` invece.</span><span class="sxs-lookup"><span data-stu-id="638bf-172">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="638bf-173">`ActionResult` implementa `IActionResult`, pertanto non è necessario modificare il tipo restituito dei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="638bf-173">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="638bf-174">Copia il *About. cshtml*, *Contact. cshtml*, e *index. cshtml* Visualizza file dal progetto ASP.NET MVC Razor per il progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="638bf-174">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="638bf-175">Eseguire l'app ASP.NET Core e ogni metodo di test.</span><span class="sxs-lookup"><span data-stu-id="638bf-175">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="638bf-176">È ancora stato ancora eseguito la migrazione il file di layout o gli stili, in modo che le viste visualizzabili contengono solo il contenuto nei file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="638bf-176">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="638bf-177">Non sarà necessario i collegamenti generati file di layout per la `About` e `Contact` viste, pertanto è necessario invece richiamarle dal browser (sostituire **4492** con il numero di porta utilizzato nel progetto).</span><span class="sxs-lookup"><span data-stu-id="638bf-177">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Pagina contatto](mvc/_static/contact-page.png)

<span data-ttu-id="638bf-179">Si noti la mancanza di applicazione di stili e voci di menu.</span><span class="sxs-lookup"><span data-stu-id="638bf-179">Note the lack of styling and menu items.</span></span> <span data-ttu-id="638bf-180">Questo problema verrà corretto nella prossima sezione.</span><span class="sxs-lookup"><span data-stu-id="638bf-180">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="638bf-181">Contenuto statico</span><span class="sxs-lookup"><span data-stu-id="638bf-181">Static content</span></span>

<span data-ttu-id="638bf-182">Nelle versioni precedenti di ASP.NET MVC, contenuto statico è stato ospitato dalla radice del progetto web ed è stato combinato con un file lato server.</span><span class="sxs-lookup"><span data-stu-id="638bf-182">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="638bf-183">In ASP.NET Core, il contenuto statico è ospitato nel *wwwroot* cartella.</span><span class="sxs-lookup"><span data-stu-id="638bf-183">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="638bf-184">È opportuno copiare il contenuto statico dalla propria app ASP.NET MVC precedente per il *wwwroot* cartella nel progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="638bf-184">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="638bf-185">In questa conversione di esempio:</span><span class="sxs-lookup"><span data-stu-id="638bf-185">In this sample conversion:</span></span>

* <span data-ttu-id="638bf-186">Copia il */favicon.ico* file dal progetto MVC precedente per il *wwwroot* cartella nel progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="638bf-186">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="638bf-187">ASP.NET MVC il vecchio progetto viene utilizzato [Bootstrap](https://getbootstrap.com/) per la relativa applicazione di stili e archivi di bootstrap relativo ai file nei *contenuto* e *script* cartelle.</span><span class="sxs-lookup"><span data-stu-id="638bf-187">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="638bf-188">Il modello, che ha generato il vecchio progetto ASP.NET MVC, fa riferimento a Bootstrap nel file di layout (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="638bf-188">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="638bf-189">È possibile copiare il *bootstrap. js* e *bootstrap* da MVC ASP.NET i file di progetto per il *wwwroot* cartella nel nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="638bf-189">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="638bf-190">Al contrario, si aggiungerà il supporto per Bootstrap e altre librerie lato client tramite le reti CDN nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="638bf-190">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="638bf-191">Eseguire la migrazione di file di layout</span><span class="sxs-lookup"><span data-stu-id="638bf-191">Migrate the layout file</span></span>

* <span data-ttu-id="638bf-192">Copia il *viewstart. cshtml* file dal progetto ASP.NET MVC precedente *viste* nella cartella del progetto ASP.NET Core *viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="638bf-192">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="638bf-193">Il *viewstart. cshtml* file non è stato modificato in ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="638bf-193">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="638bf-194">Creare un *Views/Shared* cartella.</span><span class="sxs-lookup"><span data-stu-id="638bf-194">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="638bf-195">*Facoltativo:* Copia *viewimports. cshtml* dalle *FullAspNetCore* del progetto MVC *viste* nella cartella del progetto ASP.NET Core *viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="638bf-195">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="638bf-196">Rimuovere le dichiarazioni dello spazio dei nomi nella *viewimports. cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="638bf-196">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="638bf-197">Il *viewimports. cshtml* file fornisce gli spazi dei nomi per tutti i file di visualizzazione e riporta [gli helper Tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="638bf-197">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="638bf-198">Gli helper tag vengono usati nel nuovo file di layout.</span><span class="sxs-lookup"><span data-stu-id="638bf-198">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="638bf-199">Il *viewimports. cshtml* file è una novità di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="638bf-199">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="638bf-200">Copia il *layout. cshtml* file dal progetto ASP.NET MVC precedente *Views/Shared* nella cartella del progetto ASP.NET Core *Views/Shared* cartella.</span><span class="sxs-lookup"><span data-stu-id="638bf-200">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="638bf-201">Aprire *layout. cshtml* file e apportare le modifiche seguenti (il codice completo è illustrato di seguito):</span><span class="sxs-lookup"><span data-stu-id="638bf-201">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="638bf-202">Sostituire `@Styles.Render("~/Content/css")` con un `<link>` elemento caricare *bootstrap* (vedere sotto).</span><span class="sxs-lookup"><span data-stu-id="638bf-202">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="638bf-203">Rimuovere `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="638bf-203">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="638bf-204">Impostare come commento il `@Html.Partial("_LoginPartial")` line (racchiudere la riga con `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="638bf-204">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="638bf-205">Per altre informazioni vedere [eseguire la migrazione di autenticazione e identità ad ASP.NET Core](xref:migration/identity)</span><span class="sxs-lookup"><span data-stu-id="638bf-205">For more information see [Migrate Authentication and Identity to ASP.NET Core](xref:migration/identity)</span></span>

* <span data-ttu-id="638bf-206">Sostituire `@Scripts.Render("~/bundles/jquery")` con un `<script>` elemento (vedere sotto).</span><span class="sxs-lookup"><span data-stu-id="638bf-206">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="638bf-207">Sostituire `@Scripts.Render("~/bundles/bootstrap")` con un `<script>` elemento (vedere sotto).</span><span class="sxs-lookup"><span data-stu-id="638bf-207">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="638bf-208">Il markup di sostituzione per l'inclusione di CSS Bootstrap:</span><span class="sxs-lookup"><span data-stu-id="638bf-208">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="638bf-209">Il markup di sostituzione per jQuery e JavaScript Bootstrap inclusione:</span><span class="sxs-lookup"><span data-stu-id="638bf-209">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="638bf-210">Aggiornato *layout. cshtml* file è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="638bf-210">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="638bf-211">Visualizzare il sito nel browser.</span><span class="sxs-lookup"><span data-stu-id="638bf-211">View the site in the browser.</span></span> <span data-ttu-id="638bf-212">A questo punto dovrebbero essere caricati correttamente, con gli stili previsto posto.</span><span class="sxs-lookup"><span data-stu-id="638bf-212">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="638bf-213">*Facoltativo:* È possibile provare a usare il nuovo file di layout.</span><span class="sxs-lookup"><span data-stu-id="638bf-213">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="638bf-214">Per questo progetto è possibile copiare il file di layout dal *FullAspNetCore* progetto.</span><span class="sxs-lookup"><span data-stu-id="638bf-214">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="638bf-215">Usa il nuovo file di layout [helper Tag](xref:mvc/views/tag-helpers/intro) e sono stati apportati altri miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="638bf-215">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="638bf-216">Configurare la creazione di bundle e minimizzazione</span><span class="sxs-lookup"><span data-stu-id="638bf-216">Configure bundling and minification</span></span>

<span data-ttu-id="638bf-217">Per informazioni su come configurare la creazione di bundle e minimizzazione, vedere [Bundling and Minification](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="638bf-217">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="638bf-218">Risolvere gli errori HTTP 500</span><span class="sxs-lookup"><span data-stu-id="638bf-218">Solve HTTP 500 errors</span></span>

<span data-ttu-id="638bf-219">Esistono molti problemi che possono causare un messaggio di errore HTTP 500 che non contengono informazioni sull'origine del problema.</span><span class="sxs-lookup"><span data-stu-id="638bf-219">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="638bf-220">Ad esempio, se il *viewimports* file contiene uno spazio dei nomi che non esiste nel progetto, si otterrà un errore HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="638bf-220">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="638bf-221">Per impostazione predefinita nelle App ASP.NET Core, il `UseDeveloperExceptionPage` viene aggiunta l'estensione per il `IApplicationBuilder` eseguiti quando la configurazione è *sviluppo*.</span><span class="sxs-lookup"><span data-stu-id="638bf-221">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="638bf-222">Ciò è descritta in dettaglio il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="638bf-222">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="638bf-223">ASP.NET Core converte le eccezioni non gestite in un'app web nelle risposte di errore HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="638bf-223">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="638bf-224">In genere, i dettagli dell'errore non sono inclusi in queste risposte per impedire la divulgazione di informazioni potenzialmente riservate sul server.</span><span class="sxs-lookup"><span data-stu-id="638bf-224">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="638bf-225">Vedere **usando la pagina delle eccezioni per gli sviluppatori** nelle [gestione degli errori](../fundamentals/error-handling.md) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="638bf-225">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="638bf-226">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="638bf-226">Additional resources</span></span>

* <xref:razor-components/index>
* <xref:mvc/views/tag-helpers/intro>
