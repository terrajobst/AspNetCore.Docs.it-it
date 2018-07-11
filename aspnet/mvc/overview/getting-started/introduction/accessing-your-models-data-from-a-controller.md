---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un Controller | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a3f3f4a030650ff65b070528c5efa1605be764a0
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38120153"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="086db-102">Accesso ai dati del modello da un Controller</span><span class="sxs-lookup"><span data-stu-id="086db-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="086db-103">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="086db-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="086db-104">In questa sezione si creerà un nuovo `MoviesController` classe e scrivere il codice che recupera i dati dei film e lo visualizza nel browser usando un modello di vista.</span><span class="sxs-lookup"><span data-stu-id="086db-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="086db-105">**Compilare l'applicazione** prima di procedere al passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="086db-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="086db-106">Se si non compila l'applicazione, si otterrà un errore durante l'aggiunta di un controller.</span><span class="sxs-lookup"><span data-stu-id="086db-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="086db-107">In Esplora soluzioni fare doppio clic il *controller* cartella e quindi fare clic su **Add**, quindi **Controller**.</span><span class="sxs-lookup"><span data-stu-id="086db-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="086db-108">Nel **Add Scaffold** della finestra di dialogo fare clic su **Controller MVC 5 con visualizzazioni, mediante Entity Framework**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="086db-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="086db-109">Selezionare **Movie (mvcmovie. Models)** per la classe di modello.</span><span class="sxs-lookup"><span data-stu-id="086db-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="086db-110">Selezionare **MovieDBContext (mvcmovie. Models)** per la classe del contesto dati.</span><span class="sxs-lookup"><span data-stu-id="086db-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="086db-111">Immettere il nome Controller **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="086db-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="086db-112">L'immagine seguente mostra la finestra di dialogo completato.</span><span class="sxs-lookup"><span data-stu-id="086db-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="086db-113">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="086db-113">Click **Add**.</span></span> <span data-ttu-id="086db-114">(Se si verifica un errore, probabilmente non è stata creata l'applicazione prima di avviare l'aggiunta del controller.) Visual Studio crea il file e cartelle seguenti:</span><span class="sxs-lookup"><span data-stu-id="086db-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="086db-115">*Un MoviesController.cs* del file nei *controller* cartella.</span><span class="sxs-lookup"><span data-stu-id="086db-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="086db-116">Oggetto *viste\filmati* cartella.</span><span class="sxs-lookup"><span data-stu-id="086db-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="086db-117">*Create. cshtml, cshtml, Details. cshtml, Edit. cshtml*, e *index. cshtml* nella nuova *viste\filmati* cartella.</span><span class="sxs-lookup"><span data-stu-id="086db-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="086db-118">Visual Studio creato automaticamente il [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (creare, leggere, aggiornare ed eliminare) i metodi di azione e visualizzazioni per l'utente (la creazione automatica di metodi di azione CRUD e le visualizzazioni è nota come scaffolding).</span><span class="sxs-lookup"><span data-stu-id="086db-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="086db-119">Ora disponibile un'applicazione web completamente funzionali che consente di creare, elencare, modificare ed eliminare le voci di filmato.</span><span class="sxs-lookup"><span data-stu-id="086db-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="086db-120">Eseguire l'applicazione e fare clic sulla **MVC Movie** collegamento (o selezionare il `Movies` controller mediante l'aggiunta */Movies* all'URL nella barra degli indirizzi del browser).</span><span class="sxs-lookup"><span data-stu-id="086db-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="086db-121">Poiché l'applicazione si basa il routing predefinito (definito nel *App\_Start\RouteConfig.cs* file), la richiesta del browser `http://localhost:xxxxx/Movies` viene instradato sul valore predefinito `Index` metodo di azione del `Movies` controller.</span><span class="sxs-lookup"><span data-stu-id="086db-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="086db-122">In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` corrisponde effettivamente alla richiesta del browser `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="086db-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="086db-123">Il risultato è un elenco vuoto di film, perché ancora stato aggiunto uno.</span><span class="sxs-lookup"><span data-stu-id="086db-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="086db-124">Creazione di un film</span><span class="sxs-lookup"><span data-stu-id="086db-124">Creating a Movie</span></span>

<span data-ttu-id="086db-125">Selezionare il collegamento **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="086db-125">Select the **Create New** link.</span></span> <span data-ttu-id="086db-126">Immettere alcuni dettagli sui filmati, quindi scegliere il **Create** pulsante.</span><span class="sxs-lookup"><span data-stu-id="086db-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="086db-127">Potrebbe non essere in grado di immettere i separatori decimali o virgole nel campo di prezzo.</span><span class="sxs-lookup"><span data-stu-id="086db-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="086db-128">Per supportare la convalida di jQuery per impostazioni locali di lingua diversa dall'inglese che usano la virgola (&quot;,&quot;) per un separatore decimale e formati di data non in lingua inglese Stati Uniti, è necessario includere *globalize.js* specifiche e  *Cultures/globalize.Cultures.js* file (da [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) e JavaScript usare `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="086db-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="086db-129">Verrà illustrato come eseguire questa operazione nella prossima esercitazione.</span><span class="sxs-lookup"><span data-stu-id="086db-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="086db-130">Per il momento, immettere solo numeri interi come 10.</span><span class="sxs-lookup"><span data-stu-id="086db-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="086db-131">Facendo clic sui **Create** pulsante fa sì che il modulo viene inviato al server, in cui le informazioni sui film viene salvati nel database.</span><span class="sxs-lookup"><span data-stu-id="086db-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="086db-132">Si verrà quindi reindirizzati per il */Movies* URL, in cui è possibile visualizzare i film appena creato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="086db-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="086db-133">Creare altre due voci di filmato.</span><span class="sxs-lookup"><span data-stu-id="086db-133">Create a couple more movie entries.</span></span> <span data-ttu-id="086db-134">Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.</span><span class="sxs-lookup"><span data-stu-id="086db-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="086db-135">Analisi del codice generato</span><span class="sxs-lookup"><span data-stu-id="086db-135">Examining the Generated Code</span></span>

<span data-ttu-id="086db-136">Aprire il *Controllers\MoviesController.cs* del file ed esaminare il generato `Index` (metodo).</span><span class="sxs-lookup"><span data-stu-id="086db-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="086db-137">Una parte del controller di film con la `Index` metodo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="086db-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="086db-138">Una richiesta per il `Movies` controller restituisce tutte le voci la `Movies` tabella e quindi passa i risultati per il `Index` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="086db-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="086db-139">La riga seguente dal `MoviesController` classe crea un'istanza di un contesto di database di film, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="086db-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="086db-140">È possibile utilizzare il contesto di database di film per eseguire una query, modificare ed eliminare i film.</span><span class="sxs-lookup"><span data-stu-id="086db-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="086db-141">Modelli fortemente tipizzati e @model (parola chiave)</span><span class="sxs-lookup"><span data-stu-id="086db-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="086db-142">In precedenza in questa esercitazione si è visto come un controller può passare oggetti o dati a un modello di visualizzazione usando la `ViewBag` oggetto.</span><span class="sxs-lookup"><span data-stu-id="086db-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="086db-143">Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico con associazione tardiva per passare informazioni a una vista.</span><span class="sxs-lookup"><span data-stu-id="086db-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="086db-144">MVC offre anche la possibilità di passare *fortemente* tipizzata di oggetti da un modello di vista.</span><span class="sxs-lookup"><span data-stu-id="086db-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="086db-145">Questo approccio fortemente tipizzato consente una migliore in fase di compilazione più ricco e controllo del codice [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) nell'editor di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="086db-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="086db-146">Questo approccio utilizzato il meccanismo di scaffolding in Visual Studio (vale a dire, passando un *fortemente* modello tipizzato) con il `MoviesController` i modelli di classe e visualizzazione della creazione di metodi e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="086db-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="086db-147">Nel *Controllers\MoviesController.cs* file esaminare generato `Details` (metodo).</span><span class="sxs-lookup"><span data-stu-id="086db-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="086db-148">Il `Details` metodo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="086db-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="086db-149">Il `id` parametro viene in genere passato come dati della route, ad esempio `http://localhost:1234/movies/details/1` imposterà il controller al controller di film, l'azione che deve `details` e il `id` su 1.</span><span class="sxs-lookup"><span data-stu-id="086db-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="086db-150">È inoltre possibile passare l'id con una stringa di query come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="086db-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="086db-151">Se un `Movie` viene trovato, un'istanza del `Movie` modello viene passato al `Details` Vista:</span><span class="sxs-lookup"><span data-stu-id="086db-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="086db-152">Esaminare il contenuto del *Views\Movies\Details.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="086db-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="086db-153">Includendo un `@model` informativa nella parte superiore del file di modello della vista, è possibile specificare il tipo di oggetto previsto dalla vista.</span><span class="sxs-lookup"><span data-stu-id="086db-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="086db-154">Al momento della creazione del controller di film, l'istruzione `@model` è stata inclusa automaticamente in Visual Studio all'inizio del file *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="086db-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="086db-155">Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="086db-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="086db-156">Ad esempio, nelle *Details. cshtml* modello, il codice passa ogni campo di film per il `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) helper HTML con l'oggetto fortemente tipizzato `Model` oggetto.</span><span class="sxs-lookup"><span data-stu-id="086db-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="086db-157">Il `Create` e `Edit` metodi e modelli di visualizzazione anche passano un oggetto modello di film.</span><span class="sxs-lookup"><span data-stu-id="086db-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="086db-158">Esaminare i *index. cshtml* modello di visualizzazione e il `Index` metodo nel *MoviesController.cs* file.</span><span class="sxs-lookup"><span data-stu-id="086db-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="086db-159">Si noti che il codice crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) dell'oggetto quando viene chiamato il `View` metodo helper nel `Index` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="086db-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="086db-160">Il codice passa quindi questo `Movies` dall'elenco il `Index` metodo di azione per la visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="086db-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="086db-161">Quando si crea il controller di film, Visual Studio inclusa automaticamente quanto segue `@model` istruzione all'inizio del *index. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="086db-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="086db-162">Ciò `@model` direttiva consente di accedere all'elenco di film che il controller ha passato alla vista usando un `Model` oggetto fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="086db-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="086db-163">Ad esempio, nelle *index. cshtml* modello, il codice scorre i film in questo modo un `foreach` istruzione sull'oggetto fortemente tipizzato `Model` oggetto:</span><span class="sxs-lookup"><span data-stu-id="086db-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="086db-164">Poiché il `Model` oggetto è fortemente tipizzato (come un `IEnumerable<Movie>` oggetto), ognuno `item` oggetto nel ciclo viene tipizzato come `Movie`.</span><span class="sxs-lookup"><span data-stu-id="086db-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="086db-165">Tra gli altri vantaggi, ciò significa che si ottiene controllo in fase di compilazione del codice e supporto di IntelliSense nell'editor di codice completo:</span><span class="sxs-lookup"><span data-stu-id="086db-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="086db-167">Utilizzo di SQL Server Local DB</span><span class="sxs-lookup"><span data-stu-id="086db-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="086db-168">Code First di Entity Framework ha rilevato che la stringa di connessione di database che è stata fornita puntata un `Movies` database che non esiste ancora, in modo da Code First ha creato il database automaticamente.</span><span class="sxs-lookup"><span data-stu-id="086db-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="086db-169">È possibile verificare che sia stata creata, eseguire una ricerca nel *App\_dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="086db-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="086db-170">Se non viene visualizzato il *Movies.mdf* file, fare clic sul **Mostra tutti i file** pulsante il **Esplora soluzioni** sulla barra degli strumenti, fare clic sul **Aggiorna** pulsante e quindi espandere la *App\_dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="086db-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="086db-171">Fare doppio clic su *Movies.mdf* per aprire **Esplora SERVER**, quindi espandere il **tabelle** cartella per visualizzare la tabella di film.</span><span class="sxs-lookup"><span data-stu-id="086db-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="086db-172">Si noti l'icona della chiave accanto a ID.</span><span class="sxs-lookup"><span data-stu-id="086db-172">Note the key icon next to ID.</span></span> <span data-ttu-id="086db-173">Per impostazione predefinita, Entity Framework userà una proprietà denominata ID della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="086db-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="086db-174">Per altre informazioni su Entity Framework e MVC, vedere ottima esercitazione di Tom Dykstra sul [MVC ed Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="086db-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="086db-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="086db-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="086db-176">Fare doppio clic il `Movies` tabella e selezionare **Mostra dati tabella** per visualizzare i dati è stato creato.</span><span class="sxs-lookup"><span data-stu-id="086db-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="086db-177">Fare doppio clic il `Movies` tabella e selezionare **Apri definizione tabella** per vedere la tabella di struttura che Entity Framework Code First creato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="086db-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="086db-178">Si noti come lo schema del `Movies` esegue il mapping alla tabella il `Movie` classe creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="086db-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="086db-179">Code First di Entity Framework creato automaticamente questo schema di base di `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="086db-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="086db-180">Al termine, chiudere la connessione facendo clic con il pulsante destro *MovieDBContext* e selezionando **Chiudi connessione**.</span><span class="sxs-lookup"><span data-stu-id="086db-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="086db-181">(Se si non chiude la connessione, è possibile ottenere un errore alla successiva che esecuzione del progetto).</span><span class="sxs-lookup"><span data-stu-id="086db-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="086db-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="086db-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="086db-183">È ora disponibile un database e alcune pagine per visualizzare, modificare, aggiornare ed eliminare dati.</span><span class="sxs-lookup"><span data-stu-id="086db-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="086db-184">Nella prossima esercitazione, verrà esaminare il resto del codice con scaffolding e aggiunta una `SearchIndex` metodo e un `SearchIndex` vista che consente di eseguire la ricerca di film nel database.</span><span class="sxs-lookup"><span data-stu-id="086db-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="086db-185">Per altre informazioni sull'uso di Entity Framework con MVC, vedere [creazione di un modello di dati di Entity Framework per un'applicazione ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="086db-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="086db-186">[Precedente](creating-a-connection-string.md)
> [Successivo](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="086db-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
