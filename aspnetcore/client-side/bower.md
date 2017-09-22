---
title: Usando Bower in ASP.NET Core
author: rick-anderson
description: La gestione dei pacchetti sul lato client con Bower.
keywords: ASP.NET Core, bower
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: df7c43da-280e-4df6-86cb-eecec8f12bfc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bower
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f5d9acc59b6f40dc8a336f59925d639fff007c4e
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="1b7f9-104">Gestire i pacchetti sul lato client con Bower in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1b7f9-104">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="1b7f9-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [riso Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="1b7f9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="1b7f9-106">[Bower](https://bower.io/) chiama se stessa "Gestione pacchetti per il web."</span><span class="sxs-lookup"><span data-stu-id="1b7f9-106">[Bower](https://bower.io/) calls itself "A package manager for the web."</span></span> <span data-ttu-id="1b7f9-107">All'interno dell'ecosistema di .NET, questa si riempie il vuoto lasciato dall'impossibilità di NuGet per recapitare i file di contenuto statico.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-107">Within the .NET ecosystem, it fills the void left by NuGet’s inability to deliver static content files.</span></span> <span data-ttu-id="1b7f9-108">Per i progetti ASP.NET Core, i file statici vengono svolte da librerie sul lato client come [jQuery](http://jquery.com/) e [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="1b7f9-108">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="1b7f9-109">Per le librerie .NET, è comunque usare [NuGet](https://www.nuget.org/) Gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-109">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="1b7f9-110">Processo di compilazione di nuovi progetti creati con i modelli di progetto ASP.NET Core impostare sul lato client.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-110">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="1b7f9-111">[jQuery](http://jquery.com/) e [Bootstrap](http://getbootstrap.com/) vengono installati e Bower è supportato.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-111">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="1b7f9-112">Pacchetti sul lato client sono elencati nel *bower. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-112">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="1b7f9-113">Consente di configurare i modelli di progetto ASP.NET Core *bower. JSON* con Bootstrap, convalida jQuery e jQuery.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-113">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="1b7f9-114">In questa esercitazione, verrà aggiunto il supporto per [carattere straordinario](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="1b7f9-114">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="1b7f9-115">Pacchetti bower possono essere installati con il **Gestisci pacchetti Bower** dell'interfaccia utente o manualmente nel *bower. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-115">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="1b7f9-116">Installazione tramite pacchetti Bower gestione dell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="1b7f9-116">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="1b7f9-117">Creare una nuova app Web di ASP.NET Core con il **applicazione Web di ASP.NET Core (Core .NET)** modello.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-117">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="1b7f9-118">Selezionare **applicazione Web** e **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-118">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="1b7f9-119">Fare clic sul progetto in Esplora soluzioni e selezionare **Gestisci pacchetti Bower** (in alternativa dal menu principale, **progetto** > **Gestisci pacchetti Bower**).</span><span class="sxs-lookup"><span data-stu-id="1b7f9-119">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="1b7f9-120">Nel **Bower: \<nome progetto\> ** finestra, fare clic sulla scheda "Sfoglia" e quindi filtrare l'elenco di pacchetti immettendo `font-awesome` nella casella di ricerca:</span><span class="sxs-lookup"><span data-stu-id="1b7f9-120">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

 ![gestire i pacchetti bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="1b7f9-122">Verificare che il "salvare le modifiche a *bower. JSON*" casella di controllo è selezionata.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-122">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="1b7f9-123">Selezionare una versione dall'elenco a discesa e fare clic su di **installare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-123">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="1b7f9-124">Il **Output** finestra Visualizza i dettagli di installazione.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-124">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="1b7f9-125">Installazione manuale in bower. JSON</span><span class="sxs-lookup"><span data-stu-id="1b7f9-125">Manual installation in bower.json</span></span>

<span data-ttu-id="1b7f9-126">Aprire il *bower. JSON* "tipo di carattere straordinario" aggiungere le dipendenze e dei file.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-126">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="1b7f9-127">IntelliSense mostra i pacchetti disponibili.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-127">IntelliSense shows the available packages.</span></span> <span data-ttu-id="1b7f9-128">Quando si seleziona un pacchetto, vengono visualizzate le versioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-128">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="1b7f9-129">Le immagini seguenti sono precedenti e non corrisponderà a ciò che viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-129">The images below are older and will not match what you see.</span></span>

![IntelliSense di Esplora pacchetti bower](bower/_static/add-package.png)

![versione bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="1b7f9-132">Bower utilizza [controllo delle versioni semantico](http://semver.org/) per organizzare le dipendenze.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-132">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="1b7f9-133">Controllo delle versioni semantico, noto anche come SemVer, identifica i pacchetti con lo schema di numerazione \<principale >.\< secondaria >. \<patch >.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-133">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="1b7f9-134">IntelliSense semplifica il controllo delle versioni semantico mostrando solo alcune opzioni comuni.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-134">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="1b7f9-135">Il primo elemento nell'elenco di IntelliSense (4.6.3 nell'esempio precedente) viene considerato l'ultima versione stabile del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-135">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="1b7f9-136">Il simbolo di accento circonflesso (^) corrisponda alla versione principale più recente e la tilde (~) corrisponda alla versione secondaria più recente.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-136">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="1b7f9-137">Salvare il *bower. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-137">Save the *bower.json* file.</span></span> <span data-ttu-id="1b7f9-138">Visual Studio controlla il *bower. JSON* file per le modifiche.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-138">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="1b7f9-139">Al momento del salvataggio, il *installazione bower* comando viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-139">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="1b7f9-140">Vedere la finestra di Output **Bower o npm** vista per eseguire il comando esatto.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-140">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="1b7f9-141">Aprire il *. bowerrc* file *bower. JSON*.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-141">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="1b7f9-142">Il `directory` è impostata su *wwwroot/lib* che indica la posizione Bower installerà le risorse del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-142">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="1b7f9-143">È possibile utilizzare la casella di ricerca in Esplora soluzioni per trovare e visualizzare il pacchetto straordinario con tipo di carattere.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-143">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="1b7f9-144">Aprire il *Views\Shared\_cshtml* file e aggiungere il file CSS straordinario con tipo di carattere ambiente [Helper di Tag](xref:mvc/views/tag-helpers/intro) per `Development`.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-144">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="1b7f9-145">Da Esplora soluzioni, trascinare e rilasciare *carattere awesome.css* all'interno di `<environment names="Development">` elemento.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-145">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="1b7f9-146">In un'app di produzione si aggiungerebbe *carattere awesome.min.css* per l'helper di tag di ambiente per `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-146">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="1b7f9-147">Sostituire il contenuto del *Views\Home\About.cshtml* file Razor con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="1b7f9-147">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[Main](bower/sample/About.cshtml)]

<span data-ttu-id="1b7f9-148">Eseguire l'app e passare alla visualizzazione di informazioni per verificare il funzionamento del pacchetto straordinario con tipo di carattere.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-148">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="1b7f9-149">Esplorare il processo di compilazione sul lato client</span><span class="sxs-lookup"><span data-stu-id="1b7f9-149">Exploring the client-side build process</span></span>

<span data-ttu-id="1b7f9-150">La maggior parte dei modelli di progetto ASP.NET Core sono già configurati per utilizzare Bower.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-150">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="1b7f9-151">In questa procedura dettagliata successiva inizia con un progetto vuoto di ASP.NET Core e aggiunge ogni elemento manualmente, è possibile ottenere un aspetto della modalità di utilizzo Bower in un progetto.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-151">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="1b7f9-152">Vedrai possibile cosa accade alla struttura del progetto e viene eseguito il runtime di output come ogni modifica della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-152">You see can what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="1b7f9-153">I passaggi generali per utilizzare il processo di compilazione sul lato client con Bower sono:</span><span class="sxs-lookup"><span data-stu-id="1b7f9-153">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="1b7f9-154">Definire pacchetti utilizzati nel progetto.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-154">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="1b7f9-155">Pacchetti di riferimento dalle pagine web.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-155">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="1b7f9-156">Definire pacchetti</span><span class="sxs-lookup"><span data-stu-id="1b7f9-156">Define packages</span></span>

<span data-ttu-id="1b7f9-157">Una volta elencare i pacchetti nel *bower. JSON* file, Visual Studio li scaricheranno.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-157">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="1b7f9-158">L'esempio seguente usa Bower caricare jQuery e Bootstrap per il *wwwroot* cartella.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-158">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="1b7f9-159">Creare una nuova app Web di ASP.NET Core con il **applicazione Web di ASP.NET Core (Core .NET)** modello.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-159">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="1b7f9-160">Selezionare il **vuoto** modello di progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-160">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="1b7f9-161">In Esplora soluzioni, fare clic sul progetto > **Aggiungi nuovo elemento** e selezionare **File di configurazione Bower**.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-161">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="1b7f9-162">Nota: A *. bowerrc* verrà inoltre aggiunto file.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-162">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="1b7f9-163">Aprire *bower. JSON*e aggiungere jquery e bootstrap per il `dependencies` sezione.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-163">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="1b7f9-164">Il valore risultante *bower. JSON* file sarà simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-164">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="1b7f9-165">Le versioni cambiano nel tempo e potrebbero non corrispondere nell'immagine seguente.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-165">The versions will change over time and may not match the image below.</span></span>

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="1b7f9-166">Salvare il *bower. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-166">Save the *bower.json* file.</span></span>

 <span data-ttu-id="1b7f9-167">Verificare che il progetto includa il *bootstrap* e *jQuery* directory *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-167">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="1b7f9-168">Bower utilizza il *. bowerrc* file per installare gli asset in *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-168">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

 <span data-ttu-id="1b7f9-169">Nota: L'interfaccia utente di "Gestione dei pacchetti Bower" fornisce un'alternativa alla modifica dei file manuale.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-169">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="1b7f9-170">Abilitare i file statici</span><span class="sxs-lookup"><span data-stu-id="1b7f9-170">Enable static files</span></span>

* <span data-ttu-id="1b7f9-171">Aggiungere il `Microsoft.AspNetCore.StaticFiles` pacchetto NuGet al progetto.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-171">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="1b7f9-172">Abilitare i file statici da gestire con il [middleware file statici](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="1b7f9-172">Enable static files to be served with the [Static file middleware](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="1b7f9-173">Aggiungere una chiamata a [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) per il `Configure` metodo `Startup`.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-173">Add a call to [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="1b7f9-174">Pacchetti di riferimento</span><span class="sxs-lookup"><span data-stu-id="1b7f9-174">Reference packages</span></span>

<span data-ttu-id="1b7f9-175">In questa sezione si creerà una pagina HTML per verificare che i pacchetti distribuiti può accedervi.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-175">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="1b7f9-176">Aggiungere una nuova pagina HTML denominata *Index.html* per il *wwwroot* cartella.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-176">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="1b7f9-177">Nota: È necessario aggiungere il file HTML per il *wwwroot* cartella.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-177">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="1b7f9-178">Per impostazione predefinita, il contenuto statico non può essere visualizzato all'esterno di *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-178">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="1b7f9-179">Vedere [utilizzo di file statici](xref:fundamentals/static-files) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-179">See [Working with static files](xref:fundamentals/static-files) for more information.</span></span>

 <span data-ttu-id="1b7f9-180">Sostituire il contenuto di *Index.html* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="1b7f9-180">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[Main](bower/sample/Index.html)]

* <span data-ttu-id="1b7f9-181">Eseguire l'app e passare a `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-181">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="1b7f9-182">In alternativa, con *Index.html* aperta, premere `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-182">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="1b7f9-183">Verificare che venga applicato lo stile jumbotron, il codice jQuery risponde quando viene scelto il pulsante e che il Bootstrap cambia lo stato.</span><span class="sxs-lookup"><span data-stu-id="1b7f9-183">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

 ![stile Jumbotron applicato](bower/_static/jumbotron.png)
