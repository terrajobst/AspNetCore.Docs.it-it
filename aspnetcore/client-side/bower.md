---
title: Gestire i pacchetti lato client con Bower in ASP.NET Core
author: rick-anderson
description: La gestione dei pacchetti lato client con Bower.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 06edf7ee791aac0984ff71c2f243f61093f0d503
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570022"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="a96fb-103">Gestire i pacchetti lato client con Bower in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a96fb-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="a96fb-104">Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel riso](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="a96fb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a96fb-105">Mentre viene mantenuta Bower, i relativi gestori consigliabile usare una soluzione diversa.</span><span class="sxs-lookup"><span data-stu-id="a96fb-105">While Bower is maintained, its maintainers recommend using a different solution.</span></span> <span data-ttu-id="a96fb-106">[Gestione librerie](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan per brevità) è nuova libreria lato client acquisizione degli strumenti Visual Studio (Visual Studio 15,8 o versione successiva).</span><span class="sxs-lookup"><span data-stu-id="a96fb-106">[Library Manager](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan for short) is Visual Studio's new client-side library acquisition tool (Visual Studio 15.8 or later).</span></span> <span data-ttu-id="a96fb-107">Per altre informazioni, vedere <xref:client-side/libman/index>.</span><span class="sxs-lookup"><span data-stu-id="a96fb-107">For more information, see <xref:client-side/libman/index>.</span></span> <span data-ttu-id="a96fb-108">Bower è supportata in Visual Studio fino alla versione 15.5.</span><span class="sxs-lookup"><span data-stu-id="a96fb-108">Bower is supported in Visual Studio through version 15.5.</span></span>
>
> <span data-ttu-id="a96fb-109">Yarn con Webpack è una diffusa alternativa per il quale [istruzioni relative alla migrazione](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="a96fb-109">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="a96fb-110">[Bower](https://bower.io/) chiama se stessa "Gestione pacchetti per il web".</span><span class="sxs-lookup"><span data-stu-id="a96fb-110">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="a96fb-111">All'interno dell'ecosistema .NET, riempie il vuoto a sinistra di impossibilità di NuGet per recapitare i file di contenuto statici.</span><span class="sxs-lookup"><span data-stu-id="a96fb-111">Within the .NET ecosystem, it fills the void left by NuGet's inability to deliver static content files.</span></span> <span data-ttu-id="a96fb-112">Per i progetti ASP.NET Core, sono inerenti alle librerie lato client, ad esempio i file statici [jQuery](http://jquery.com/) e [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="a96fb-112">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="a96fb-113">Per le librerie .NET, è comunque usare [NuGet](https://www.nuget.org/) Gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="a96fb-113">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="a96fb-114">Processo di compilazione di nuovi progetti creati con i modelli di progetto ASP.NET Core configurare lato client.</span><span class="sxs-lookup"><span data-stu-id="a96fb-114">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="a96fb-115">[jQuery](http://jquery.com/) e [Bootstrap](http://getbootstrap.com/) sono installati, e Bower è supportata.</span><span class="sxs-lookup"><span data-stu-id="a96fb-115">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="a96fb-116">Vengono elencati i pacchetti lato client nel *bower. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="a96fb-116">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="a96fb-117">Consente di configurare i modelli di progetto ASP.NET Core *bower. JSON* con jQuery, la convalida di jQuery e Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="a96fb-117">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="a96fb-118">In questa esercitazione, verrà aggiunto il supporto per [Font Awesome](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="a96fb-118">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="a96fb-119">I pacchetti bower possono essere installati con il **Gestisci pacchetti Bower** dell'interfaccia utente o manualmente nel *bower. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="a96fb-119">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="a96fb-120">Installazione tramite pacchetti Bower Gestisci dell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="a96fb-120">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="a96fb-121">Creare una nuova app Web ASP.NET Core con il **applicazione Web ASP.NET Core (.NET Core)** modello.</span><span class="sxs-lookup"><span data-stu-id="a96fb-121">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="a96fb-122">Selezionare **applicazione Web** e **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="a96fb-122">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="a96fb-123">Fare clic sul progetto in Esplora soluzioni e selezionare **Gestisci pacchetti Bower** (in alternativa dal menu principale **Project** > **Gestisci pacchetti Bower**).</span><span class="sxs-lookup"><span data-stu-id="a96fb-123">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="a96fb-124">Nel **Bower: \<nome progetto\>**  finestra, fare clic sulla scheda "Sfoglia" e quindi filtrare l'elenco di pacchetti tramite l'immissione di `font-awesome` nella casella di ricerca:</span><span class="sxs-lookup"><span data-stu-id="a96fb-124">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

  ![Gestisci pacchetti bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="a96fb-126">Verificare che il "salvare le modifiche *bower. JSON*" casella di controllo è selezionata.</span><span class="sxs-lookup"><span data-stu-id="a96fb-126">Confirm that the "Save changes to *bower.json*" check box is checked.</span></span> <span data-ttu-id="a96fb-127">Selezionare una versione dall'elenco a discesa elenco e scegliere il **installare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="a96fb-127">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="a96fb-128">Il **Output** finestra Mostra i dettagli di installazione.</span><span class="sxs-lookup"><span data-stu-id="a96fb-128">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="a96fb-129">Installazione manuale in bower. JSON</span><span class="sxs-lookup"><span data-stu-id="a96fb-129">Manual installation in bower.json</span></span>

<span data-ttu-id="a96fb-130">Aprire il *bower. JSON* file e aggiungere "font awesome" alle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="a96fb-130">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="a96fb-131">IntelliSense mostra i pacchetti disponibili.</span><span class="sxs-lookup"><span data-stu-id="a96fb-131">IntelliSense shows the available packages.</span></span> <span data-ttu-id="a96fb-132">Quando viene selezionato un pacchetto, vengono visualizzate le versioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="a96fb-132">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="a96fb-133">Le immagini seguenti sono meno recenti e non corrisponderanno a ciò che viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="a96fb-133">The images below are older and won't match what you see.</span></span>

![IntelliSense di Esplora pacchetti bower](bower/_static/add-package.png)

![versione bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="a96fb-136">Viene utilizzato per bower [versionamento semantico](http://semver.org/) per organizzare le dipendenze.</span><span class="sxs-lookup"><span data-stu-id="a96fb-136">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="a96fb-137">Versionamento semantico, noto anche come SemVer, identifica i pacchetti con lo schema di numerazione \<principale >.\< secondaria >. \<patch >.</span><span class="sxs-lookup"><span data-stu-id="a96fb-137">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="a96fb-138">IntelliSense semplifica versionamento semantico mostrando solo alcune opzioni comuni.</span><span class="sxs-lookup"><span data-stu-id="a96fb-138">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="a96fb-139">Il primo elemento nell'elenco di IntelliSense (4.6.3 nell'esempio precedente) viene considerato la versione stabile più recente del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="a96fb-139">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="a96fb-140">Il simbolo di accento circonflesso (^) corrisponde alla versione principale più recente e la tilde (~) corrisponde alla versione secondaria più recente.</span><span class="sxs-lookup"><span data-stu-id="a96fb-140">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="a96fb-141">Salvare il *bower. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="a96fb-141">Save the *bower.json* file.</span></span> <span data-ttu-id="a96fb-142">Visual Studio controlla le *bower. JSON* file per le modifiche.</span><span class="sxs-lookup"><span data-stu-id="a96fb-142">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="a96fb-143">Al momento del salvataggio, il *bower install* comando viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="a96fb-143">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="a96fb-144">Vedere la finestra di Output **npm/Bower** visualizzazione per il comando esatto eseguito.</span><span class="sxs-lookup"><span data-stu-id="a96fb-144">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="a96fb-145">Aprire il *. bowerrc* del file sotto *bower. JSON*.</span><span class="sxs-lookup"><span data-stu-id="a96fb-145">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="a96fb-146">Il `directory` è impostata su *wwwroot/lib* che indica la posizione Bower installerà gli asset di pacchetti.</span><span class="sxs-lookup"><span data-stu-id="a96fb-146">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="a96fb-147">Per individuare e visualizzare il pacchetto font awesome, è possibile utilizzare la casella di ricerca in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="a96fb-147">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="a96fb-148">Aprire il *Views\Shared\_layout. cshtml* file e aggiungere il file CSS font awesome all'ambiente [Helper Tag](xref:mvc/views/tag-helpers/intro) per `Development`.</span><span class="sxs-lookup"><span data-stu-id="a96fb-148">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="a96fb-149">Da Esplora soluzioni, trascinare e rilasciare *font awesome.css* all'interno di `<environment names="Development">` elemento.</span><span class="sxs-lookup"><span data-stu-id="a96fb-149">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="a96fb-150">In un'app di produzione aggiungerebbe *font awesome.min.css* per l'helper tag di ambiente per `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="a96fb-150">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="a96fb-151">Sostituire il contenuto del *Views\Home\About.cshtml* file Razor con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="a96fb-151">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[](bower/sample/About.cshtml)]

<span data-ttu-id="a96fb-152">Eseguire l'app e passare alla visualizzazione About per verificare il funzionamento del pacchetto font awesome.</span><span class="sxs-lookup"><span data-stu-id="a96fb-152">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="a96fb-153">Esplorare il processo di compilazione dal lato client</span><span class="sxs-lookup"><span data-stu-id="a96fb-153">Exploring the client-side build process</span></span>

<span data-ttu-id="a96fb-154">La maggior parte dei modelli di progetto ASP.NET Core sono già configurati per usare Bower.</span><span class="sxs-lookup"><span data-stu-id="a96fb-154">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="a96fb-155">Questa procedura dettagliata successiva inizia con un progetto ASP.NET Core vuoto e aggiunge ogni pezzo manualmente, in modo che è possibile avere un'idea di come Bower viene utilizzato in un progetto.</span><span class="sxs-lookup"><span data-stu-id="a96fb-155">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="a96fb-156">È possibile visualizzare cosa accade alla struttura di progetto e il runtime perché viene effettuata ogni modifica della configurazione di output.</span><span class="sxs-lookup"><span data-stu-id="a96fb-156">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="a96fb-157">I passaggi generali per usare il processo di compilazione dal lato client con Bower sono:</span><span class="sxs-lookup"><span data-stu-id="a96fb-157">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="a96fb-158">Definire pacchetti usati nel progetto.</span><span class="sxs-lookup"><span data-stu-id="a96fb-158">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="a96fb-159">Pacchetti di riferimento dalle pagine web.</span><span class="sxs-lookup"><span data-stu-id="a96fb-159">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="a96fb-160">Definire pacchetti</span><span class="sxs-lookup"><span data-stu-id="a96fb-160">Define packages</span></span>

<span data-ttu-id="a96fb-161">Quando si elencano i pacchetti nel *bower. JSON* file, Visual Studio li scaricheranno.</span><span class="sxs-lookup"><span data-stu-id="a96fb-161">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="a96fb-162">L'esempio seguente usa Bower caricare jQuery e Bootstrap per i *wwwroot* cartella.</span><span class="sxs-lookup"><span data-stu-id="a96fb-162">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="a96fb-163">Creare una nuova app Web ASP.NET Core con il **applicazione Web ASP.NET Core (.NET Core)** modello.</span><span class="sxs-lookup"><span data-stu-id="a96fb-163">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="a96fb-164">Selezionare il **vuote** modello di progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a96fb-164">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="a96fb-165">In Esplora soluzioni fare clic sul progetto > **Aggiungi nuovo elemento** e selezionare **File di configurazione Bower**.</span><span class="sxs-lookup"><span data-stu-id="a96fb-165">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="a96fb-166">Nota: Una *. bowerrc* verrà inoltre aggiunto file.</span><span class="sxs-lookup"><span data-stu-id="a96fb-166">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="a96fb-167">Aprire *bower. JSON*, quindi aggiungere jquery e bootstrap per il `dependencies` sezione.</span><span class="sxs-lookup"><span data-stu-id="a96fb-167">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="a96fb-168">L'oggetto risultante *bower. JSON* file avrà un aspetto simile all'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="a96fb-168">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="a96fb-169">Le versioni cambieranno nel corso del tempo e potrebbero non corrispondere a quella riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="a96fb-169">The versions will change over time and may not match the image below.</span></span>

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="a96fb-170">Salvare il *bower. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="a96fb-170">Save the *bower.json* file.</span></span>

  <span data-ttu-id="a96fb-171">Verificare che il progetto includa il *bootstrap* e *jQuery* directory nel *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="a96fb-171">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="a96fb-172">Bower Usa il *. bowerrc* per installare gli asset di file *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="a96fb-172">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

  <span data-ttu-id="a96fb-173">Nota: L'interfaccia utente "Gestisci pacchetti Bower" fornisce un'alternativa alla modifica del file manuale.</span><span class="sxs-lookup"><span data-stu-id="a96fb-173">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="a96fb-174">Abilitare i file statici</span><span class="sxs-lookup"><span data-stu-id="a96fb-174">Enable static files</span></span>

* <span data-ttu-id="a96fb-175">Aggiungere il `Microsoft.AspNetCore.StaticFiles` pacchetto NuGet al progetto.</span><span class="sxs-lookup"><span data-stu-id="a96fb-175">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="a96fb-176">Abilitare i file statici essere serviti con la [middleware dei file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="a96fb-176">Enable static files to be served with the [Static file middleware](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="a96fb-177">Aggiungere una chiamata a [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) per il `Configure` metodo `Startup`.</span><span class="sxs-lookup"><span data-stu-id="a96fb-177">Add a call to [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="a96fb-178">Pacchetti di riferimento</span><span class="sxs-lookup"><span data-stu-id="a96fb-178">Reference packages</span></span>

<span data-ttu-id="a96fb-179">In questa sezione si creerà una pagina HTML per verificare che possa accedere i pacchetti distribuiti.</span><span class="sxs-lookup"><span data-stu-id="a96fb-179">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="a96fb-180">Aggiungere una nuova pagina HTML denominata *index. HTML* per il *wwwroot* cartella.</span><span class="sxs-lookup"><span data-stu-id="a96fb-180">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="a96fb-181">Nota: È necessario aggiungere il file HTML per il *wwwroot* cartella.</span><span class="sxs-lookup"><span data-stu-id="a96fb-181">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="a96fb-182">Per impostazione predefinita, il contenuto statico non può essere visualizzato di fuori *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="a96fb-182">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="a96fb-183">Visualizzare [i file statici](xref:fundamentals/static-files) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="a96fb-183">See [Static files](xref:fundamentals/static-files) for more information.</span></span>

  <span data-ttu-id="a96fb-184">Sostituire il contenuto del *index. HTML* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="a96fb-184">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[](bower/sample/Index.html)]

* <span data-ttu-id="a96fb-185">Eseguire l'app e passare a `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="a96fb-185">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="a96fb-186">In alternativa, con *index. HTML* aperta, premere `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="a96fb-186">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="a96fb-187">Verificare che l'applicazione di stili jumbotron venga applicato, il codice jQuery risponde quando si fa clic sul pulsante e che il pulsante Bootstrap viene modificato lo stato.</span><span class="sxs-lookup"><span data-stu-id="a96fb-187">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

  ![stile Jumbotron applicato](bower/_static/jumbotron.png)
