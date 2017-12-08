---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un Controller | Documenti Microsoft
author: Rick-Anderson
description: "Nota: Una versione aggiornata di questa esercitazione è disponibile qui che utilizza ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice seguire e demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 82b4521279dcd9b9dc5a8e81b3a0d87ab26d46ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="7460a-104">Accesso ai dati del modello da un Controller</span><span class="sxs-lookup"><span data-stu-id="7460a-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="7460a-105">Da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7460a-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="7460a-106">È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="7460a-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="7460a-107">È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="7460a-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="7460a-108">In questa sezione si creerà un nuovo `MoviesController` classe e scrivere codice che recupera i dati dei film e lo visualizza in browser utilizzando un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="7460a-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="7460a-109">**Compilare l'applicazione** prima di procedere al passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="7460a-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="7460a-110">Fare doppio clic su di *controller* cartella e creare un nuovo `MoviesController` controller.</span><span class="sxs-lookup"><span data-stu-id="7460a-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="7460a-111">Le opzioni seguenti non verranno visualizzati fino a quando non si compila l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7460a-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="7460a-112">Selezionare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7460a-112">Select the following options:</span></span>

- <span data-ttu-id="7460a-113">Nome del controller: **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="7460a-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="7460a-114">(Questo è il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="7460a-114">(This is the default.</span></span> <span data-ttu-id="7460a-115">)</span><span class="sxs-lookup"><span data-stu-id="7460a-115">)</span></span>
- <span data-ttu-id="7460a-116">Modello: **Controller MVC con azioni di lettura/scrittura e visualizzazioni, mediante Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="7460a-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="7460a-117">Classe del modello: **filmato (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="7460a-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="7460a-118">Classe del contesto dati: **MovieDBContext (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="7460a-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="7460a-119">Visualizzazioni: **Razor (CSHTML)**.</span><span class="sxs-lookup"><span data-stu-id="7460a-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="7460a-120">(Predefinito).</span><span class="sxs-lookup"><span data-stu-id="7460a-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="7460a-122">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="7460a-122">Click **Add**.</span></span> <span data-ttu-id="7460a-123">Visual Studio Express crea i file e cartelle seguenti:</span><span class="sxs-lookup"><span data-stu-id="7460a-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="7460a-124">*Un MoviesController.cs* nel file del progetto *controller* cartella.</span><span class="sxs-lookup"><span data-stu-id="7460a-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="7460a-125">Oggetto *filmati* cartella del progetto *viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="7460a-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="7460a-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, e *cshtml* nel nuovo *Views\Movies* cartella.</span><span class="sxs-lookup"><span data-stu-id="7460a-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="7460a-127">ASP.NET MVC 4 creato automaticamente il CRUD (creare, leggere, aggiornare ed eliminare) i metodi di azione e le viste per l'utente (la creazione automatica delle viste e i metodi di azione CRUD è noto come scaffolding).</span><span class="sxs-lookup"><span data-stu-id="7460a-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="7460a-128">Ora è un'applicazione web completamente funzionale che consente di creare, elencare, modificare ed eliminare le voci di film.</span><span class="sxs-lookup"><span data-stu-id="7460a-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="7460a-129">Eseguire l'applicazione e individuare il `Movies` controller aggiungendo */Movies* per l'URL nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="7460a-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="7460a-130">Poiché l'applicazione si basa il routing predefinito (definito nel *Global. asax* file), la richiesta del browser `http://localhost:xxxxx/Movies` viene indirizzato per il valore predefinito `Index` metodo di azione del `Movies` controller.</span><span class="sxs-lookup"><span data-stu-id="7460a-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="7460a-131">In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` corrisponde in modo efficace la richiesta del browser `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="7460a-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="7460a-132">Il risultato è un elenco vuoto di film, perché ancora non sono stati aggiunti.</span><span class="sxs-lookup"><span data-stu-id="7460a-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="7460a-133">Creazione di un film</span><span class="sxs-lookup"><span data-stu-id="7460a-133">Creating a Movie</span></span>

<span data-ttu-id="7460a-134">Selezionare il collegamento **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="7460a-134">Select the **Create New** link.</span></span> <span data-ttu-id="7460a-135">Immettere alcune informazioni dettagliate su un filmato, quindi scegliere il **crea** pulsante.</span><span class="sxs-lookup"><span data-stu-id="7460a-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="7460a-136">Fare clic su di **crea** pulsante fa sì che il modulo di essere inviata al server, in cui le informazioni di film vengono salvate nel database.</span><span class="sxs-lookup"><span data-stu-id="7460a-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="7460a-137">Quindi si viene reindirizzati al */Movies* URL, in cui è possibile visualizzare il film appena creato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="7460a-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="7460a-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="7460a-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="7460a-139">Creare altre due voci di filmato.</span><span class="sxs-lookup"><span data-stu-id="7460a-139">Create a couple more movie entries.</span></span> <span data-ttu-id="7460a-140">Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.</span><span class="sxs-lookup"><span data-stu-id="7460a-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="7460a-141">Analisi del codice generato</span><span class="sxs-lookup"><span data-stu-id="7460a-141">Examining the Generated Code</span></span>

<span data-ttu-id="7460a-142">Aprire il *Controllers\MoviesController.cs* file ed esaminare generato `Index` metodo.</span><span class="sxs-lookup"><span data-stu-id="7460a-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="7460a-143">Una parte del controller di film con il `Index` metodo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7460a-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="7460a-144">La riga seguente dalla `MoviesController` classe crea un'istanza di un contesto di database, film, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="7460a-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="7460a-145">È possibile utilizzare il contesto del database film per eseguire una query, modificare ed eliminare i film.</span><span class="sxs-lookup"><span data-stu-id="7460a-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="7460a-146">Una richiesta per il `Movies` controller restituisce tutte le voci la `Movies` tabella del database film e quindi passa i risultati per il `Index` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="7460a-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="7460a-147">Fortemente tipizzate modelli e @model (parola chiave)</span><span class="sxs-lookup"><span data-stu-id="7460a-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="7460a-148">In precedenza in questa esercitazione, si è visto come un controller può passare oggetti o dati a un modello di visualizzazione utilizzando il `ViewBag` oggetto.</span><span class="sxs-lookup"><span data-stu-id="7460a-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="7460a-149">Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico ad associazione tardiva per passare informazioni a una vista.</span><span class="sxs-lookup"><span data-stu-id="7460a-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="7460a-150">ASP.NET MVC fornisce inoltre la possibilità di passare fortemente tipizzata di dati o oggetti da un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="7460a-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="7460a-151">Questo approccio consente una migliore in fase di compilazione il controllo del codice e più completa IntelliSense nell'editor di Visual Studio è fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="7460a-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="7460a-152">Il meccanismo di scaffolding in Visual Studio utilizzato questo approccio che prevede il `MoviesController` modelli di classe e di visualizzazione quando creato i metodi e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="7460a-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="7460a-153">Nel *Controllers\MoviesController.cs* file esaminare generato `Details` metodo.</span><span class="sxs-lookup"><span data-stu-id="7460a-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="7460a-154">Una parte del controller di film con il `Details` metodo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7460a-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="7460a-155">Se un `Movie` viene trovato, un'istanza di `Movie` modello viene passato alla visualizzazione dettagli.</span><span class="sxs-lookup"><span data-stu-id="7460a-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="7460a-156">Esaminare il contenuto del *Views\Movies\Details.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="7460a-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="7460a-157">Includendo un `@model` istruzione all'inizio del file di modello di visualizzazione, è possibile specificare il tipo di oggetto che prevede la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="7460a-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="7460a-158">Al momento della creazione del controller di film, l'istruzione `@model` è stata inclusa automaticamente in Visual Studio all'inizio del file *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7460a-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="7460a-159">Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="7460a-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="7460a-160">Ad esempio, nel *Details.cshtml* modello, il codice passa tutti i campi film il `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) helper HTML con l'oggetto fortemente tipizzato `Model` oggetto.</span><span class="sxs-lookup"><span data-stu-id="7460a-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="7460a-161">I metodi di creazione e modifica e i modelli di visualizzazione anche passano un oggetto modello film.</span><span class="sxs-lookup"><span data-stu-id="7460a-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="7460a-162">Esaminare il *cshtml* modello di visualizzazione e la `Index` metodo il *MoviesController.cs* file.</span><span class="sxs-lookup"><span data-stu-id="7460a-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="7460a-163">Si noti come il codice crea un [ `List` ](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) oggetto quando viene chiamata la `View` il metodo di supporto nel `Index` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7460a-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="7460a-164">Il codice passa quindi questo `Movies` elenco dal controller alla visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="7460a-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="7460a-165">Al momento della creazione del controller di film, Visual Studio Express automaticamente includeva `@model` istruzione all'inizio del *cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="7460a-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="7460a-166">Questo `@model` direttiva consente di accedere all'elenco di film che passato alla visualizzazione per il controller utilizzando un `Model` oggetto fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="7460a-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="7460a-167">Ad esempio, nel *cshtml* modello, il codice scorre i film in questo modo un `foreach` istruzione tramite l'oggetto fortemente tipizzato `Model` oggetto:</span><span class="sxs-lookup"><span data-stu-id="7460a-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="7460a-168">Poiché il `Model` sono fortemente tipizzato di oggetti (come un `IEnumerable<Movie>` oggetto), ogni `item` oggetto nel ciclo è tipizzato come `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7460a-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="7460a-169">Tra gli altri vantaggi, ciò significa che ottenere il controllo del codice in fase di compilazione e di supporto di IntelliSense nell'editor di codice completo:</span><span class="sxs-lookup"><span data-stu-id="7460a-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="7460a-171">Utilizzo di SQL Server Local DB</span><span class="sxs-lookup"><span data-stu-id="7460a-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="7460a-172">Code First di Entity Framework ha rilevato che a cui punta la stringa di connessione di database che è stata fornita un `Movies` database che non esiste ancora, pertanto il primo codice creato il database automaticamente.</span><span class="sxs-lookup"><span data-stu-id="7460a-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="7460a-173">È possibile verificare che sia stato creato esaminando il *App\_dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="7460a-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="7460a-174">Se non viene visualizzato il *Movies.mdf* file, fare clic sul **Mostra tutti i file** pulsante il **Esplora** sulla barra degli strumenti, fare clic sul **aggiornare** pulsante e quindi espandere il *App\_dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="7460a-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="7460a-175">Fare doppio clic su *Movies.mdf* per aprire **Esplora DATABASE**, quindi espandere il **tabelle** cartella per visualizzare la tabella di film.</span><span class="sxs-lookup"><span data-stu-id="7460a-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="7460a-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="7460a-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="7460a-177">Se non viene visualizzato Esplora database, dal **strumenti** dal menu **Connetti al Database**, quindi annullare il **Scegli origine dati** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="7460a-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="7460a-178">Questa operazione forzerà aprire Esplora database.</span><span class="sxs-lookup"><span data-stu-id="7460a-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="7460a-179">Se si utilizza VWD o Visual Studio 2010 e si verifica un errore simile a uno dei seguenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="7460a-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="7460a-180">Il database ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF' non può essere aperto perché la versione 706 è.</span><span class="sxs-lookup"><span data-stu-id="7460a-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="7460a-181">Questo server supporta 655 e nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="7460a-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="7460a-182">Un percorso per il downgrade non è supportato.</span><span class="sxs-lookup"><span data-stu-id="7460a-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="7460a-183">&quot;Non è stata gestita dal codice utente eccezione InvalidOperation&quot; nella stringa SqlConnection fornita non specifica un catalogo iniziale.</span><span class="sxs-lookup"><span data-stu-id="7460a-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="7460a-184">È necessario installare il [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) e [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span><span class="sxs-lookup"><span data-stu-id="7460a-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="7460a-185">Verificare il `MovieDBContext` stringa di connessione specificata nella pagina precedente.</span><span class="sxs-lookup"><span data-stu-id="7460a-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="7460a-186">Fare doppio clic su di `Movies` tabella e selezionare **Mostra dati tabella** per visualizzare i dati è stato creato.</span><span class="sxs-lookup"><span data-stu-id="7460a-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="7460a-187">Fare doppio clic su di `Movies` tabella e selezionare **Apri definizione tabella** per visualizzare la tabella di struttura che Entity Framework Code First creato.</span><span class="sxs-lookup"><span data-stu-id="7460a-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="7460a-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="7460a-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="7460a-189">Si noti come lo schema del `Movies` esegue il mapping alla tabella il `Movie` classe creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="7460a-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="7460a-190">Code First di Entity Framework automaticamente questo schema creato in base al `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="7460a-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="7460a-191">Al termine, chiudere la connessione facendo clic *MovieDBContext* e selezionando **Chiudi connessione**.</span><span class="sxs-lookup"><span data-stu-id="7460a-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="7460a-192">(Se si non chiude la connessione, si verifichi un errore alla successiva che esecuzione del progetto).</span><span class="sxs-lookup"><span data-stu-id="7460a-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="7460a-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="7460a-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="7460a-194">È ora disponibile il database e una pagina di elenco semplice per visualizzare il contenuto da esso.</span><span class="sxs-lookup"><span data-stu-id="7460a-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="7460a-195">Nella prossima esercitazione, si sarà il resto del codice scaffolding di esaminare e aggiungere un `SearchIndex` (metodo) e un `SearchIndex` visualizzazione che consente di eseguire la ricerca di filmati in questo database.</span><span class="sxs-lookup"><span data-stu-id="7460a-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7460a-196">[Precedente](adding-a-model.md)
[Successivo](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="7460a-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
