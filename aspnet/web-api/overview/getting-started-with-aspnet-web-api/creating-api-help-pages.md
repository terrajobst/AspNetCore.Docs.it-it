---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Creazione di pagine della Guida per ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 37fd26ebaea192cb540c443eff8a07343ab8c15b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="c7ec7-102">Creazione di pagine della Guida per ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c7ec7-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="c7ec7-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c7ec7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c7ec7-104">Quando si crea un'API web, è spesso utile creare una pagina della Guida, in modo che altri sviluppatori saranno in grado di chiamare l'API.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="c7ec7-105">È possibile creare tutta la documentazione manualmente, ma è preferibile per la generazione automatica il più possibile.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="c7ec7-106">Per semplificare questa attività, ASP.NET Web API fornisce una libreria per la generazione automatica di pagine della Guida in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="c7ec7-107">Creazione di pagine della Guida di API</span><span class="sxs-lookup"><span data-stu-id="c7ec7-107">Creating API Help Pages</span></span>

<span data-ttu-id="c7ec7-108">Installare [Web ASP.NET e strumenti di aggiornamento 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="c7ec7-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="c7ec7-109">Questo aggiornamento si integra il modello di progetto API Web pagine della Guida.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="c7ec7-110">Successivamente, creare un nuovo progetto ASP.NET MVC 4 e selezionare il modello di progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="c7ec7-111">Il modello di progetto crea un controller di API di esempio denominato `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="c7ec7-112">Il modello crea anche le pagine della Guida di API.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-112">The template also creates the API help pages.</span></span> <span data-ttu-id="c7ec7-113">Tutti i file di codice per la pagina della Guida vengono inseriti nella cartella di aree del progetto.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="c7ec7-114">Quando si esegue l'applicazione, la home page contiene un collegamento alla pagina della Guida di API.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="c7ec7-115">Dalla home page, il percorso relativo è /Help.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="c7ec7-116">Questo collegamento consente di visualizzare una pagina di riepilogo di API.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="c7ec7-117">La visualizzazione MVC questa pagina è definita in Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="c7ec7-118">È possibile modificare questa pagina per modificare il layout, introduzione, titolo, stili e così via.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="c7ec7-119">La parte principale della pagina è una tabella delle API, raggruppate dal controller.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="c7ec7-120">Le voci della tabella vengono generate in modo dinamico, utilizzando il **IApiExplorer** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="c7ec7-121">(Più avanti parlerò su questa interfaccia in un secondo momento.) Se si aggiunge un nuovo controller API, la tabella viene aggiornata automaticamente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="c7ec7-122">La colonna "API" Elenca l'URI relativo e il metodo HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="c7ec7-123">La colonna "Descrizione" contiene la documentazione per ogni API.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="c7ec7-124">La documentazione è inizialmente, solo il testo segnaposto.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="c7ec7-125">Nella sezione successiva verrà illustrato aggiungere documentazione da commenti XML.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="c7ec7-126">Ogni API dispone di un collegamento a una pagina con informazioni più dettagliate, tra cui i testi di richiesta e risposta di esempio.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="c7ec7-127">Aggiunta di pagine della Guida per un progetto esistente</span><span class="sxs-lookup"><span data-stu-id="c7ec7-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="c7ec7-128">È possibile aggiungere le pagine della Guida per un progetto di API Web esistente tramite Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="c7ec7-129">Questa opzione è utile che iniziare da un modello di progetto diverso rispetto al modello "API Web".</span><span class="sxs-lookup"><span data-stu-id="c7ec7-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="c7ec7-130">Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="c7ec7-131">Nel [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) finestra, digitare uno dei seguenti comandi:</span><span class="sxs-lookup"><span data-stu-id="c7ec7-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="c7ec7-132">Per un **c#** applicazione:`Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="c7ec7-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="c7ec7-133">Per un **Visual Basic** applicazione:`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="c7ec7-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="c7ec7-134">Esistono due pacchetti, uno per c# e uno per Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="c7ec7-135">Assicurarsi di usare quello che corrisponde al progetto.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="c7ec7-136">Questo comando Installa gli assembly necessari e aggiunge le visualizzazioni MVC per le pagine della Guida (che si trova nella cartella aree/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="c7ec7-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="c7ec7-137">È necessario aggiungere manualmente un collegamento alla pagina della Guida.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="c7ec7-138">L'URI è /Help.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-138">The URI is /Help.</span></span> <span data-ttu-id="c7ec7-139">Per creare un collegamento in una visualizzazione razor, aggiungere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c7ec7-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="c7ec7-140">Assicurarsi inoltre che registrare le aree.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-140">Also, make sure to register areas.</span></span> <span data-ttu-id="c7ec7-141">Nel file Global. asax, aggiungere il codice seguente per il **applicazione\_avviare** metodo, se non esiste già:</span><span class="sxs-lookup"><span data-stu-id="c7ec7-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="c7ec7-142">Aggiunta di documentazione dell'API</span><span class="sxs-lookup"><span data-stu-id="c7ec7-142">Adding API Documentation</span></span>

<span data-ttu-id="c7ec7-143">Per impostazione predefinita, la Guida pagine sono stringhe di segnaposto per la documentazione.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="c7ec7-144">È possibile utilizzare [commenti della documentazione XML](https://msdn.microsoft.com/library/b2s063f7.aspx) per creare la documentazione.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="c7ec7-145">Per abilitare questa funzionalità, aprire il file HelpPage/aree/App\_Start/HelpPageConfig.cs e rimuovere il commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="c7ec7-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="c7ec7-146">Ora la documentazione XML.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-146">Now enable XML documentation.</span></span> <span data-ttu-id="c7ec7-147">In Esplora soluzioni, fare clic sul progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="c7ec7-148">Selezionare il **compilare** pagina.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="c7ec7-149">In **Output**, controllare **file di documentazione XML**.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="c7ec7-150">Nella casella di modifica, digitare "App\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="c7ec7-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="c7ec7-151">Aprire quindi il codice per il `ValuesController` controller API che è definito in /Controllers/ValuesControler.cs.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="c7ec7-152">Aggiungere alcuni commenti di documentazione per i metodi del controller.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="c7ec7-153">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7ec7-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="c7ec7-154">Suggerimento: Se si posiziona il punto di inserimento nella riga sopra il metodo e digitare tre barre, Visual Studio automaticamente inserisce gli elementi XML.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="c7ec7-155">È quindi possibile inserire gli spazi vuoti.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="c7ec7-156">Ora di compilazione ed eseguire nuovamente l'applicazione e individuare le pagine della Guida.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="c7ec7-157">Le stringhe di documentazione dovrebbero essere visualizzato nella tabella API.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="c7ec7-158">La pagina della Guida legge le stringhe dal file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="c7ec7-159">(Quando si distribuisce l'applicazione, assicurarsi di distribuire il file XML.)</span><span class="sxs-lookup"><span data-stu-id="c7ec7-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="c7ec7-160">Dietro le quinte</span><span class="sxs-lookup"><span data-stu-id="c7ec7-160">Under the Hood</span></span>

<span data-ttu-id="c7ec7-161">Le pagine della Guida si basano sul **ApiExplorer** (classe), che fa parte del framework Web API.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="c7ec7-162">Il **ApiExplorer** classe fornisce le materie prime per la creazione di una pagina della Guida.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="c7ec7-163">Per ogni API, **ApiExplorer** contiene un **ApiDescription** che descrive l'API.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="c7ec7-164">A tale scopo, un'API"" è definita come la combinazione di URI relativo e il metodo HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="c7ec7-165">Ad esempio, ecco alcune API distinti:</span><span class="sxs-lookup"><span data-stu-id="c7ec7-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="c7ec7-166">OTTENERE /api/Products</span><span class="sxs-lookup"><span data-stu-id="c7ec7-166">GET /api/Products</span></span>
- <span data-ttu-id="c7ec7-167">OTTENERE /api/prodotti / {id}</span><span class="sxs-lookup"><span data-stu-id="c7ec7-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="c7ec7-168">Registra/api/prodotti</span><span class="sxs-lookup"><span data-stu-id="c7ec7-168">POST /api/Products</span></span>

<span data-ttu-id="c7ec7-169">Se un'azione del controller supporta più metodi HTTP, il **ApiExplorer** considera ogni metodo di un'API distinta.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="c7ec7-170">Per nascondere un'API del **ApiExplorer**, aggiungere il **ApiExplorerSettings** attributo per l'azione e set *IgnoreApi* su true.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="c7ec7-171">È anche possibile aggiungere questo attributo nel controller, escludere il controller di intero.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="c7ec7-172">La classe ApiExplorer Ottiene le stringhe di documentazione dal **IDocumentationProvider** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="c7ec7-173">Come illustrato in precedenza, la libreria di pagine della Guida fornisce un **IDocumentationProvider** che ottiene documentazione da stringhe di documentazione XML.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="c7ec7-174">Il codice si trova in /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="c7ec7-175">È possibile ottenere la documentazione di un'altra origine scrivendo proprio **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="c7ec7-176">Per associare il, chiamare il **SetDocumentationProvider** metodo di estensione, definito **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="c7ec7-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="c7ec7-177">**ApiExplorer** chiama automaticamente il **IDocumentationProvider** interfaccia da ottenere stringhe della documentazione per ogni API.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="c7ec7-178">Li archivia nel **documentazione** proprietà del **ApiDescription** e **ApiParameterDescription** oggetti.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c7ec7-179">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c7ec7-179">Next Steps</span></span>

<span data-ttu-id="c7ec7-180">Non è per le pagine della guida indicate di seguito.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="c7ec7-181">In effetti, **ApiExplorer** non è limitata alla creazione di pagine della Guida.</span><span class="sxs-lookup"><span data-stu-id="c7ec7-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="c7ec7-182">Yao Huang Lin ha scritto che alcuni grande post di blog per iniziare a pensare all'esterno della casella:</span><span class="sxs-lookup"><span data-stu-id="c7ec7-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="c7ec7-183">Aggiunta di un semplice Client di Test a una pagina della Guida di ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c7ec7-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="c7ec7-184">Funzionamento nei servizi self-hosted di pagina della Guida di ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c7ec7-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="c7ec7-185">Generazione in fase di progettazione di Guida pagina (o client) per l'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c7ec7-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="c7ec7-186">Personalizzazioni avanzate di pagina della Guida</span><span class="sxs-lookup"><span data-stu-id="c7ec7-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
