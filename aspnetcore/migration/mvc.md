---
title: Eseguire la migrazione di ASP.NET MVC per Core ASP.NET MVC
author: ardalis
description: Informazioni su come iniziare a eseguire la migrazione di un progetto MVC ASP.NET ad ASP.NET MVC di base.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851027"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="f1ef5-103">Eseguire la migrazione di ASP.NET MVC per Core ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f1ef5-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="f1ef5-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="f1ef5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="f1ef5-105">Questo articolo illustra come iniziare la migrazione di un progetto ASP.NET MVC per [ASP.NET MVC Core](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="f1ef5-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="f1ef5-106">Nel processo, viene illustrata la gran parte delle operazioni che sono stati modificati da ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="f1ef5-107">La migrazione da ASP.NET MVC è un processo in più passaggi e questo articolo vengono illustrate la configurazione iniziale, i controller di base e viste, contenuto statico e dipendenze sul lato client.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="f1ef5-108">Articoli aggiuntivi coprono la migrazione della configurazione e il codice di identità presente in molti progetti ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="f1ef5-109">I numeri di versione campioni potrebbero non essere aggiornati.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="f1ef5-110">Si potrebbe essere necessario aggiornare di conseguenza i progetti.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="f1ef5-111">Creare il progetto ASP.NET MVC starter</span><span class="sxs-lookup"><span data-stu-id="f1ef5-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="f1ef5-112">Per dimostrare l'aggiornamento, si inizierà creando un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="f1ef5-113">Creare con il nome *WebApp1* in modo che lo spazio dei nomi corrisponde al progetto ASP.NET Core creata nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Finestra di dialogo di Visual Studio nuovo progetto](mvc/_static/new-project.png)

![Finestra di dialogo nuova applicazione Web: il modello di progetto MVC selezionato nel riquadro modelli ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="f1ef5-116">*Facoltativo:* modificare il nome della soluzione da *WebApp1* a *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="f1ef5-117">Visual Studio consente di visualizzare il nuovo nome della soluzione (*Mvc5*), che rende più semplice indicare questo progetto dal progetto successivo.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="f1ef5-118">Creare il progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1ef5-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="f1ef5-119">Creare un nuovo *vuoto* app web ASP.NET Core con lo stesso nome del progetto precedente (*WebApp1*) in modo da corrispondere gli spazi dei nomi in due progetti.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="f1ef5-120">Lo stesso spazio dei nomi rende più semplice copiare il codice tra i due progetti.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="f1ef5-121">È necessario creare il progetto in una directory diversa da quella del progetto precedente per utilizzare lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Finestra di dialogo Nuovo progetto](mvc/_static/new_core.png)

![Finestra di dialogo nuova applicazione Web ASP.NET: modello di progetto vuoto selezionato nel Pannello di ASP.NET Core modelli](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="f1ef5-124">*Facoltativo:* crea una nuova applicazione ASP.NET Core usando il *applicazione Web* modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="f1ef5-125">Denominare il progetto *WebApp1*, quindi selezionare un'opzione di autenticazione di **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="f1ef5-126">Rinominare questa app e *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="f1ef5-127">Creazione in questo modo progetto durante la conversione.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="f1ef5-128">È possibile esaminare il codice modello generato per visualizzare il risultato finale o copiare il codice per il progetto di conversione.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="f1ef5-129">È inoltre utile quando è bloccato nel passaggio conversione da confrontare con il progetto di modello generato.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="f1ef5-130">Configurare il sito per l'uso di MVC</span><span class="sxs-lookup"><span data-stu-id="f1ef5-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="f1ef5-131">Quando la destinazione è .NET Core, il metapackage ASP.NET Core viene aggiunto al progetto, denominato `Microsoft.AspNetCore.All` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-131">When targeting .NET Core, the ASP.NET Core metapackage is added to the project, called `Microsoft.AspNetCore.All` by default.</span></span> <span data-ttu-id="f1ef5-132">Questo pacchetto contiene i pacchetti, ad esempio `Microsoft.AspNetCore.Mvc` e `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-132">This package contains packages like `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="f1ef5-133">Se la destinazione è .NET Framework, pacchetto fa riferimento desidera essere elencati singolarmente nel file csproj.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-133">If targeting .NET Framework, package references need to be listed individually in the \*.csproj file.</span></span>

<span data-ttu-id="f1ef5-134">`Microsoft.AspNetCore.Mvc` è il framework ASP.NET MVC di base.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-134">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="f1ef5-135">`Microsoft.AspNetCore.StaticFiles` è il gestore di file statici.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-135">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="f1ef5-136">Il runtime di ASP.NET Core è modulare e in modo esplicito è necessario optare per servire file statici (vedere [file statici](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="f1ef5-136">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="f1ef5-137">Aprire il *Startup.cs* file e modificare il codice in modo che corrisponda a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="f1ef5-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="f1ef5-138">Il `UseStaticFiles` metodo di estensione viene aggiunto il gestore di file statici.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="f1ef5-139">Come accennato in precedenza, il runtime di ASP.NET è modulare e deve esplicitamente ad utilizzati file statici.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="f1ef5-140">Il `UseMvc` consente di aggiungere il metodo di estensione di routing.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="f1ef5-141">Per ulteriori informazioni, vedere [avvio dell'applicazione](xref:fundamentals/startup) e [Routing](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="f1ef5-141">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="f1ef5-142">Aggiungere un controller e una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f1ef5-142">Add a controller and view</span></span>

<span data-ttu-id="f1ef5-143">In questa sezione si aggiungerà un controller di minima e una visualizzazione per essere utilizzati come segnaposti per il controller MVC ASP.NET e le viste che è possibile eseguire la migrazione nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="f1ef5-144">Aggiungere un *controller* cartella.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="f1ef5-145">Aggiungere un **classe Controller** denominata *HomeController.cs* per il *controller* cartella.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-145">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="f1ef5-147">Aggiungere un *viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="f1ef5-148">Aggiungere un *Views/Home* cartella.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="f1ef5-149">Aggiungere un **visualizzazione Razor** denominata *cshtml* per il *Views/Home* cartella.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-149">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento](mvc/_static/view.png)

<span data-ttu-id="f1ef5-151">La struttura del progetto è illustrata di seguito:</span><span class="sxs-lookup"><span data-stu-id="f1ef5-151">The project structure is shown below:</span></span>

![Esplora soluzioni con file e cartelle di WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="f1ef5-153">Sostituire il contenuto del *Views/Home/Index.cshtml* file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="f1ef5-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="f1ef5-154">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-154">Run the app.</span></span>

![App Web aperta in Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="f1ef5-156">Vedere [controller](xref:mvc/controllers/actions) e [viste](xref:mvc/views/overview) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-156">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="f1ef5-157">Ora che è disponibile un progetto ASP.NET Core lavoro minimo, è possibile avviare la migrazione di funzionalità dal progetto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="f1ef5-158">È necessario spostare il seguente:</span><span class="sxs-lookup"><span data-stu-id="f1ef5-158">We need to move the following:</span></span>

* <span data-ttu-id="f1ef5-159">contenuto sul lato client (CSS, i tipi di carattere e script)</span><span class="sxs-lookup"><span data-stu-id="f1ef5-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="f1ef5-160">controller</span><span class="sxs-lookup"><span data-stu-id="f1ef5-160">controllers</span></span>

* <span data-ttu-id="f1ef5-161">visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="f1ef5-161">views</span></span>

* <span data-ttu-id="f1ef5-162">modelli</span><span class="sxs-lookup"><span data-stu-id="f1ef5-162">models</span></span>

* <span data-ttu-id="f1ef5-163">Creazione di bundle</span><span class="sxs-lookup"><span data-stu-id="f1ef5-163">bundling</span></span>

* <span data-ttu-id="f1ef5-164">filtri</span><span class="sxs-lookup"><span data-stu-id="f1ef5-164">filters</span></span>

* <span data-ttu-id="f1ef5-165">Log in/out, identità (questa operazione viene eseguita nella prossima esercitazione.)</span><span class="sxs-lookup"><span data-stu-id="f1ef5-165">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="f1ef5-166">Controller e visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="f1ef5-166">Controllers and views</span></span>

* <span data-ttu-id="f1ef5-167">Copia di ciascuno dei metodi di ASP.NET MVC `HomeController` al nuovo `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="f1ef5-168">Si noti che in ASP.NET MVC, tipo restituito del metodo del modello predefinito controller azione è [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET MVC di base, i metodi di azione restituiti `IActionResult` invece.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="f1ef5-169">`ActionResult` implementa `IActionResult`, pertanto non è necessario modificare il tipo restituito dei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="f1ef5-170">Copia il *About.cshtml*, *Contact.cshtml*, e *cshtml* Visualizza file dal progetto ASP.NET MVC Razor per il progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="f1ef5-171">Eseguire l'applicazione ASP.NET di base e ogni metodo di test.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="f1ef5-172">È non ancora eseguita la migrazione di file di layout o gli stili, in modo che le viste visualizzabili contengano solo il contenuto nei file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-172">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="f1ef5-173">Non sarà possibile i collegamenti generati file di layout per il `About` e `Contact` viste, pertanto è necessario essere richiamati dal browser (sostituire **4492** con il numero di porta usato nel progetto).</span><span class="sxs-lookup"><span data-stu-id="f1ef5-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Pagina contatto](mvc/_static/contact-page.png)

<span data-ttu-id="f1ef5-175">Si noti la mancanza di stile e voci di menu.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="f1ef5-176">Questo problema verrà corretto nella prossima sezione.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="f1ef5-177">Contenuto statico</span><span class="sxs-lookup"><span data-stu-id="f1ef5-177">Static content</span></span>

<span data-ttu-id="f1ef5-178">Nelle versioni precedenti di ASP.NET MVC, contenuto statico è stato ospitato dalla radice del progetto web e stato combinato con i file sul lato server.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-178">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="f1ef5-179">In ASP.NET Core, contenuto statico è ospitato nel *wwwroot* cartella.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="f1ef5-180">Sarà necessario copiare il contenuto statico dall'app ASP.NET MVC precedente per il *wwwroot* cartella del progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="f1ef5-181">In questa conversione di esempio:</span><span class="sxs-lookup"><span data-stu-id="f1ef5-181">In this sample conversion:</span></span>

* <span data-ttu-id="f1ef5-182">Copia il *favicon.ico* file dal progetto MVC precedente per il *wwwroot* cartella del progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="f1ef5-183">ASP.NET MVC il vecchio progetto usa [Bootstrap](https://getbootstrap.com/) lo stile e archivia il programma di avvio file nel *contenuto* e *script* cartelle.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-183">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="f1ef5-184">Il modello, che ha generato il vecchio progetto ASP.NET MVC, fa riferimento nel file di layout Bootstrap (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="f1ef5-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="f1ef5-185">È possibile copiare il *bootstrap.js* e *bootstrap.css* di ASP.NET MVC i file di progetto per il *wwwroot* cartella nel nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="f1ef5-186">Al contrario, verrà aggiunto il supporto per il Bootstratp e altre librerie client-side tramite reti CDN nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-186">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="f1ef5-187">La migrazione del file di layout</span><span class="sxs-lookup"><span data-stu-id="f1ef5-187">Migrate the layout file</span></span>

* <span data-ttu-id="f1ef5-188">Copia il *viewstart* file dal progetto ASP.NET MVC precedente *viste* nella cartella del progetto ASP.NET Core *viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-188">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="f1ef5-189">Il *viewstart* file non è stato modificato in ASP.NET MVC di base.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-189">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="f1ef5-190">Creare un *Views/Shared* cartella.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-190">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="f1ef5-191">*Facoltativo:* copia *viewimports. cshtml* dal *FullAspNetCore* del progetto MVC *viste* nella cartella del progetto ASP.NET Core  *Viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-191">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="f1ef5-192">Rimuovere le dichiarazioni dello spazio dei nomi nella *viewimports. cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-192">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f1ef5-193">Il *viewimports. cshtml* fornisce gli spazi dei nomi per tutti i file di visualizzazione e la porta file [gli helper di Tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="f1ef5-193">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="f1ef5-194">Gli helper di tag vengono utilizzati nel nuovo file di layout.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-194">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="f1ef5-195">Il *viewimports. cshtml* file è una novità di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-195">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="f1ef5-196">Copia il *layout. cshtml* file dal progetto ASP.NET MVC precedente *Views/Shared* nella cartella del progetto ASP.NET Core *Views/Shared* cartella.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-196">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="f1ef5-197">Aprire *layout. cshtml* file e apportare le modifiche seguenti (il codice completo è illustrato di seguito):</span><span class="sxs-lookup"><span data-stu-id="f1ef5-197">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="f1ef5-198">Sostituire `@Styles.Render("~/Content/css")` con un `<link>` elemento da cui caricare *bootstrap.css* (vedere sotto).</span><span class="sxs-lookup"><span data-stu-id="f1ef5-198">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="f1ef5-199">Rimuovere `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-199">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="f1ef5-200">Impostare come commento il `@Html.Partial("_LoginPartial")` riga (racchiudono la riga con `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="f1ef5-200">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="f1ef5-201">Verrà restituiamo ad esso in un'esercitazione future.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-201">We'll return to it in a future tutorial.</span></span>

* <span data-ttu-id="f1ef5-202">Sostituire `@Scripts.Render("~/bundles/jquery")` con un `<script>` elemento (vedere sotto).</span><span class="sxs-lookup"><span data-stu-id="f1ef5-202">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="f1ef5-203">Sostituire `@Scripts.Render("~/bundles/bootstrap")` con un `<script>` elemento (vedere sotto).</span><span class="sxs-lookup"><span data-stu-id="f1ef5-203">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="f1ef5-204">Il markup di sostituzione per l'inclusione di Bootstrap CSS:</span><span class="sxs-lookup"><span data-stu-id="f1ef5-204">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="f1ef5-205">Il markup di sostituzione per l'inclusione di Bootstrap JavaScript e jQuery:</span><span class="sxs-lookup"><span data-stu-id="f1ef5-205">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="f1ef5-206">L'aggiornamento *layout. cshtml* file è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="f1ef5-206">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="f1ef5-207">Visualizzare il sito nel browser.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-207">View the site in the browser.</span></span> <span data-ttu-id="f1ef5-208">Ora debba caricare correttamente, con gli stili previsto sul posto.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-208">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="f1ef5-209">*Facoltativo:* è possibile provare a usare il nuovo file di layout.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-209">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="f1ef5-210">Per questo progetto è possibile copiare il file di layout dal *FullAspNetCore* progetto.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-210">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="f1ef5-211">Il nuovo file di layout Usa [gli helper di Tag](xref:mvc/views/tag-helpers/intro) e dispone di altri miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-211">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="f1ef5-212">Configurare l'aggregazione e riduzione</span><span class="sxs-lookup"><span data-stu-id="f1ef5-212">Configure bundling and minification</span></span>

<span data-ttu-id="f1ef5-213">Per informazioni su come configurare l'aggregazione e riduzione, vedere [Bundling and Minification](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="f1ef5-213">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="f1ef5-214">Risolvere gli errori HTTP 500</span><span class="sxs-lookup"><span data-stu-id="f1ef5-214">Solve HTTP 500 errors</span></span>

<span data-ttu-id="f1ef5-215">Esistono molti problemi che possono causare un messaggio di errore HTTP 500 che non contengono informazioni sull'origine del problema.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-215">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="f1ef5-216">Ad esempio, se il *Views/_ViewImports.cshtml* file contiene uno spazio dei nomi che non esiste nel progetto, si otterrà un errore HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-216">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="f1ef5-217">Per impostazione predefinita in applicazioni ASP.NET Core, il `UseDeveloperExceptionPage` viene aggiunta l'estensione per il `IApplicationBuilder` ed eseguito dopo la configurazione *sviluppo*.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-217">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="f1ef5-218">Informazioni dettagliate, vedere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f1ef5-218">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="f1ef5-219">ASP.NET Core converte le eccezioni non gestite in un'app web in risposte di errore HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-219">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="f1ef5-220">In genere, i dettagli dell'errore non sono inclusi in queste risposte per impedire la divulgazione di informazioni potenzialmente riservate sul server.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-220">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="f1ef5-221">Vedere **utilizzando la pagina di eccezione Developer** in [gestire gli errori](../fundamentals/error-handling.md) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="f1ef5-221">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1ef5-222">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f1ef5-222">Additional resources</span></span>

* [<span data-ttu-id="f1ef5-223">Sviluppo sul lato client</span><span class="sxs-lookup"><span data-stu-id="f1ef5-223">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="f1ef5-224">Helper tag</span><span class="sxs-lookup"><span data-stu-id="f1ef5-224">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
