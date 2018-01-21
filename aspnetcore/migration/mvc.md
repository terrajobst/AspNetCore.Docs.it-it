---
title: Migrazione da MVC ASP.NET ad ASP.NET MVC di base
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: 88e5b7575930434e291a7aa4daef429306c653e0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="a4cd7-102">Migrazione da MVC ASP.NET ad ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="a4cd7-102">Migrating From ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="a4cd7-103">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="a4cd7-103">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="a4cd7-104">Questo articolo illustra come iniziare la migrazione di un progetto ASP.NET MVC per [ASP.NET MVC Core](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-104">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="a4cd7-105">Nel processo, viene illustrata la gran parte delle operazioni che sono stati modificati da ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-105">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="a4cd7-106">La migrazione da ASP.NET MVC è un processo in più passaggi e questo articolo vengono illustrate la configurazione iniziale, i controller di base e viste, contenuto statico e dipendenze sul lato client.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-106">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="a4cd7-107">Articoli aggiuntivi coprono la migrazione della configurazione e il codice di identità presente in molti progetti ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-107">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="a4cd7-108">I numeri di versione campioni potrebbero non essere aggiornati.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-108">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="a4cd7-109">Si potrebbe essere necessario aggiornare di conseguenza i progetti.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-109">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="a4cd7-110">Creare il progetto ASP.NET MVC starter</span><span class="sxs-lookup"><span data-stu-id="a4cd7-110">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="a4cd7-111">Per dimostrare l'aggiornamento, si inizierà creando un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-111">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="a4cd7-112">Crearla con il nome *WebApp1* in modo lo spazio dei nomi corrisponderà il progetto ASP.NET Core creata nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-112">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Finestra di dialogo di Visual Studio nuovo progetto](mvc/_static/new-project.png)

![Finestra di dialogo nuova applicazione Web: il modello di progetto MVC selezionato nel riquadro modelli ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="a4cd7-115">*Facoltativo:* modificare il nome della soluzione da *WebApp1* a *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-115">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="a4cd7-116">Verrà visualizzato il nome della nuova soluzione in Visual Studio (*Mvc5*), che renderà più facile indicare il progetto dal progetto successivo.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-116">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="a4cd7-117">Creare il progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4cd7-117">Create the ASP.NET Core project</span></span>

<span data-ttu-id="a4cd7-118">Creare un nuovo *vuoto* app web ASP.NET Core con lo stesso nome del progetto precedente (*WebApp1*) in modo da corrispondere gli spazi dei nomi in due progetti.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-118">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="a4cd7-119">Lo stesso spazio dei nomi rende più semplice copiare il codice tra i due progetti.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-119">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="a4cd7-120">È necessario creare il progetto in una directory diversa da quella del progetto precedente per utilizzare lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-120">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Finestra di dialogo Nuovo progetto](mvc/_static/new_core.png)

![Finestra di dialogo nuova applicazione Web ASP.NET: modello di progetto vuoto selezionato nel Pannello di ASP.NET Core modelli](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="a4cd7-123">*Facoltativo:* crea una nuova applicazione ASP.NET Core usando il *applicazione Web* modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-123">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="a4cd7-124">Denominare il progetto *WebApp1*, quindi selezionare un'opzione di autenticazione di **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-124">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="a4cd7-125">Rinominare questa app e *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-125">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="a4cd7-126">Creazione di questo progetto verranno risparmiare tempo durante la conversione.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-126">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="a4cd7-127">È possibile esaminare il codice modello generato per visualizzare il risultato finale o copiare il codice per il progetto di conversione.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-127">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="a4cd7-128">È inoltre utile quando è bloccato nel passaggio conversione da confrontare con il progetto di modello generato.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-128">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="a4cd7-129">Configurare il sito per l'uso di MVC</span><span class="sxs-lookup"><span data-stu-id="a4cd7-129">Configure the site to use MVC</span></span>

* <span data-ttu-id="a4cd7-130">Installare il `Microsoft.AspNetCore.Mvc` e `Microsoft.AspNetCore.StaticFiles` pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-130">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="a4cd7-131">`Microsoft.AspNetCore.Mvc`è il framework ASP.NET MVC di base.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-131">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="a4cd7-132">`Microsoft.AspNetCore.StaticFiles`è il gestore di file statici.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-132">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="a4cd7-133">Il runtime di ASP.NET è modulare e deve esplicitamente ad utilizzati file statici (vedere [utilizzo di file statici](../fundamentals/static-files.md)).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-133">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="a4cd7-134">Aprire il *csproj* file (pulsante destro del mouse sul progetto in **Esplora** e selezionare **WebApp1.csproj modifica**) e aggiungere un `PrepareForPublish` destinazione:</span><span class="sxs-lookup"><span data-stu-id="a4cd7-134">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  <span data-ttu-id="a4cd7-135">Il `PrepareForPublish` destinazione è necessaria per l'acquisizione delle librerie sul lato client tramite Bower.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-135">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="a4cd7-136">Parleremo che in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-136">We'll talk about that later.</span></span>

* <span data-ttu-id="a4cd7-137">Aprire il *Startup.cs* file e modificare il codice in modo che corrisponda a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="a4cd7-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  <span data-ttu-id="a4cd7-138">Il `UseStaticFiles` metodo di estensione viene aggiunto il gestore di file statici.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="a4cd7-139">Come accennato in precedenza, il runtime di ASP.NET è modulare e deve esplicitamente ad utilizzati file statici.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="a4cd7-140">Il `UseMvc` consente di aggiungere il metodo di estensione di routing.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="a4cd7-141">Per ulteriori informazioni, vedere [avvio dell'applicazione](../fundamentals/startup.md) e [Routing](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-141">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="a4cd7-142">Aggiungere un controller e una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="a4cd7-142">Add a controller and view</span></span>

<span data-ttu-id="a4cd7-143">In questa sezione si aggiungerà un controller di minima e una visualizzazione per essere utilizzati come segnaposti per il controller MVC ASP.NET e le viste che è possibile eseguire la migrazione nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="a4cd7-144">Aggiungere un *controller* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="a4cd7-145">Aggiungere un **classe controller MVC** con il nome *HomeController.cs* per il *controller* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-145">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="a4cd7-147">Aggiungere un *viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="a4cd7-148">Aggiungere un *Views/Home* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="a4cd7-149">Aggiungere un *cshtml* pagina visualizzazione MVC per il *Views/Home* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-149">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento](mvc/_static/view.png)

<span data-ttu-id="a4cd7-151">La struttura del progetto è illustrata di seguito:</span><span class="sxs-lookup"><span data-stu-id="a4cd7-151">The project structure is shown below:</span></span>

![Esplora soluzioni con file e cartelle di WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="a4cd7-153">Sostituire il contenuto del *Views/Home/Index.cshtml* file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a4cd7-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="a4cd7-154">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-154">Run the app.</span></span>

![Applicazione Web aperto in Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="a4cd7-156">Vedere [controller](../mvc/controllers/index.md) e [viste](../mvc/views/index.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-156">See [Controllers](../mvc/controllers/index.md) and [Views](../mvc/views/index.md) for more information.</span></span>

<span data-ttu-id="a4cd7-157">Ora che è disponibile un progetto ASP.NET Core lavoro minimo, è possibile avviare la migrazione di funzionalità dal progetto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="a4cd7-158">È necessario spostare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a4cd7-158">We will need to move the following:</span></span>

* <span data-ttu-id="a4cd7-159">contenuto sul lato client (CSS, i tipi di carattere e script)</span><span class="sxs-lookup"><span data-stu-id="a4cd7-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="a4cd7-160">controller</span><span class="sxs-lookup"><span data-stu-id="a4cd7-160">controllers</span></span>

* <span data-ttu-id="a4cd7-161">visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="a4cd7-161">views</span></span>

* <span data-ttu-id="a4cd7-162">modelli</span><span class="sxs-lookup"><span data-stu-id="a4cd7-162">models</span></span>

* <span data-ttu-id="a4cd7-163">Creazione di bundle</span><span class="sxs-lookup"><span data-stu-id="a4cd7-163">bundling</span></span>

* <span data-ttu-id="a4cd7-164">filtri</span><span class="sxs-lookup"><span data-stu-id="a4cd7-164">filters</span></span>

* <span data-ttu-id="a4cd7-165">Log in/out, identità (questa operazione verrà eseguita nella prossima esercitazione.)</span><span class="sxs-lookup"><span data-stu-id="a4cd7-165">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="a4cd7-166">Controller e visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="a4cd7-166">Controllers and views</span></span>

* <span data-ttu-id="a4cd7-167">Copia di ciascuno dei metodi di ASP.NET MVC `HomeController` al nuovo `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="a4cd7-168">Si noti che in ASP.NET MVC, tipo restituito del metodo del modello predefinito controller azione è [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET MVC di base, i metodi di azione restituiti `IActionResult` invece.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="a4cd7-169">`ActionResult`implementa `IActionResult`, pertanto non è necessario modificare il tipo restituito dei metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-169">`ActionResult` implements `IActionResult`, so there is no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="a4cd7-170">Copia il *About.cshtml*, *Contact.cshtml*, e *cshtml* Visualizza file dal progetto ASP.NET MVC Razor per il progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="a4cd7-171">Eseguire l'applicazione ASP.NET di base e ogni metodo di test.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="a4cd7-172">È non ancora eseguita la migrazione di file di layout o gli stili, in modo le viste visualizzabili conterrà solo il contenuto nel file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-172">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="a4cd7-173">Non sarà possibile i collegamenti generati file di layout per il `About` e `Contact` viste, pertanto è necessario essere richiamati dal browser (sostituire **4492** con il numero di porta usato nel progetto).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Pagina contatto](mvc/_static/contact-page.png)

<span data-ttu-id="a4cd7-175">Si noti la mancanza di stile e voci di menu.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="a4cd7-176">Che verrà risolto nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="a4cd7-177">Contenuto statico</span><span class="sxs-lookup"><span data-stu-id="a4cd7-177">Static content</span></span>

<span data-ttu-id="a4cd7-178">Nelle versioni precedenti di ASP.NET MVC, contenuto statico è stato ospitato dalla radice del progetto web e stato combinato con i file sul lato server.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-178">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="a4cd7-179">In ASP.NET Core, contenuto statico è ospitato nel *wwwroot* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="a4cd7-180">Sarà necessario copiare il contenuto statico dall'app ASP.NET MVC precedente per il *wwwroot* cartella del progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="a4cd7-181">In questa conversione di esempio:</span><span class="sxs-lookup"><span data-stu-id="a4cd7-181">In this sample conversion:</span></span>

* <span data-ttu-id="a4cd7-182">Copia il *favicon.ico* file dal progetto MVC precedente per il *wwwroot* cartella del progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="a4cd7-183">ASP.NET MVC il vecchio progetto usa [Bootstrap](http://getbootstrap.com/) lo stile e archivia il programma di avvio file nel *contenuto* e *script* cartelle.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-183">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="a4cd7-184">Il modello, che ha generato il vecchio progetto ASP.NET MVC, fa riferimento nel file di layout Bootstrap (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="a4cd7-185">È possibile copiare il *bootstrap.js* e *bootstrap.css* di ASP.NET MVC i file di progetto per il *wwwroot* cartella in cui il nuovo progetto, ma tale approccio non utilizza il meccanismo migliore per la gestione delle dipendenze sul lato client in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="a4cd7-186">Nel nuovo progetto, verrà aggiunto il supporto per l'avvio e altre librerie sul lato client tramite [Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="a4cd7-186">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="a4cd7-187">Aggiungi un [Bower](https://bower.io/) file di configurazione denominato *bower. JSON* alla radice del progetto (pulsante destro del mouse sul progetto, quindi **Aggiungi > Nuovo elemento > File di configurazione Bower**).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-187">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="a4cd7-188">Aggiungere [Bootstrap](http://getbootstrap.com/) e [jQuery](https://jquery.com/) al file (vedere le righe evidenziate riportato di seguito).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-188">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

<span data-ttu-id="a4cd7-189">Dopo il salvataggio del file, Bower scaricherà automaticamente le dipendenze per il *wwwroot/lib* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-189">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="a4cd7-190">È possibile utilizzare il **Cerca in Esplora soluzioni** per trovare il percorso delle risorse:</span><span class="sxs-lookup"><span data-stu-id="a4cd7-190">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![Asset jQuery visualizzati nei risultati della ricerca di Esplora soluzioni](mvc/_static/search.png)

<span data-ttu-id="a4cd7-192">Vedere [Gestisci pacchetti sul lato Client con Bower](../client-side/bower.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-192">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="a4cd7-193">La migrazione del file di layout</span><span class="sxs-lookup"><span data-stu-id="a4cd7-193">Migrate the layout file</span></span>

* <span data-ttu-id="a4cd7-194">Copia il *viewstart* file dal progetto ASP.NET MVC precedente *viste* nella cartella del progetto ASP.NET Core *viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-194">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="a4cd7-195">Il *viewstart* file non è stato modificato in ASP.NET MVC di base.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-195">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="a4cd7-196">Creare un *Views/Shared* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-196">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="a4cd7-197">*Facoltativo:* copia *viewimports. cshtml* dal *FullAspNetCore* del progetto MVC *viste* nella cartella del progetto ASP.NET Core *Viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-197">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="a4cd7-198">Rimuovere le dichiarazioni dello spazio dei nomi nella *viewimports. cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-198">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="a4cd7-199">Il *viewimports. cshtml* fornisce gli spazi dei nomi per tutti i file di visualizzazione e la porta file [gli helper di Tag](../mvc/views/tag-helpers/index.md).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-199">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](../mvc/views/tag-helpers/index.md).</span></span> <span data-ttu-id="a4cd7-200">Gli helper di tag vengono utilizzati nel nuovo file di layout.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-200">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="a4cd7-201">Il *viewimports. cshtml* file è una novità di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-201">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="a4cd7-202">Copia il *layout. cshtml* file dal progetto ASP.NET MVC precedente *Views/Shared* nella cartella del progetto ASP.NET Core *Views/Shared* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-202">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="a4cd7-203">Aprire *layout. cshtml* file e apportare le modifiche seguenti (il codice completo è illustrato di seguito):</span><span class="sxs-lookup"><span data-stu-id="a4cd7-203">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="a4cd7-204">Sostituire `@Styles.Render("~/Content/css")` con un `<link>` elemento da cui caricare *bootstrap.css* (vedere sotto).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-204">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="a4cd7-205">Rimuovere `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-205">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="a4cd7-206">Impostare come commento il `@Html.Partial("_LoginPartial")` riga (racchiudono la riga con `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-206">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="a4cd7-207">Verrà restituiamo ad esso in un'esercitazione future.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-207">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="a4cd7-208">Sostituire `@Scripts.Render("~/bundles/jquery")` con un `<script>` elemento (vedere sotto).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-208">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="a4cd7-209">Sostituire `@Scripts.Render("~/bundles/bootstrap")` con un `<script>` elemento (vedere sotto)...</span><span class="sxs-lookup"><span data-stu-id="a4cd7-209">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="a4cd7-210">Il collegamento CSS sostituzione:</span><span class="sxs-lookup"><span data-stu-id="a4cd7-210">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="a4cd7-211">I tag di script di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="a4cd7-211">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="a4cd7-212">L'aggiornamento *layout. cshtml* file è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="a4cd7-212">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

<span data-ttu-id="a4cd7-213">Visualizzare il sito nel browser.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-213">View the site in the browser.</span></span> <span data-ttu-id="a4cd7-214">Ora debba caricare correttamente, con gli stili previsto sul posto.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-214">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="a4cd7-215">*Facoltativo:* è possibile provare a usare il nuovo file di layout.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-215">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="a4cd7-216">Per questo progetto è possibile copiare il file di layout dal *FullAspNetCore* progetto.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-216">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="a4cd7-217">Il nuovo file di layout Usa [gli helper di Tag](../mvc/views/tag-helpers/index.md) e dispone di altri miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-217">The new layout file uses [Tag Helpers](../mvc/views/tag-helpers/index.md) and has other improvements.</span></span>

## <a name="configure-bundling--minification"></a><span data-ttu-id="a4cd7-218">Configurare l'aggregazione & minimizzazione</span><span class="sxs-lookup"><span data-stu-id="a4cd7-218">Configure Bundling & Minification</span></span>

<span data-ttu-id="a4cd7-219">Per informazioni su come configurare l'aggregazione e riduzione, vedere [Bundling and Minification](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="a4cd7-219">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="a4cd7-220">Risolvere gli errori HTTP 500</span><span class="sxs-lookup"><span data-stu-id="a4cd7-220">Solving HTTP 500 errors</span></span>

<span data-ttu-id="a4cd7-221">Esistono molti problemi che possono causare un messaggio di errore HTTP 500 che non contengono informazioni sull'origine del problema.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-221">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="a4cd7-222">Ad esempio, se il *Views/_ViewImports.cshtml* file contiene uno spazio dei nomi che non esiste nel progetto, si otterrà un errore HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-222">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="a4cd7-223">Per ottenere un messaggio di errore dettagliato, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a4cd7-223">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="a4cd7-224">Vedere **utilizzando la pagina di eccezione Developer** in [Error Handling](../fundamentals/error-handling.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="a4cd7-224">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a4cd7-225">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a4cd7-225">Additional Resources</span></span>

* [<span data-ttu-id="a4cd7-226">Sviluppo lato client</span><span class="sxs-lookup"><span data-stu-id="a4cd7-226">Client-Side Development</span></span>](../client-side/index.md)

* [<span data-ttu-id="a4cd7-227">Helper tag</span><span class="sxs-lookup"><span data-stu-id="a4cd7-227">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
