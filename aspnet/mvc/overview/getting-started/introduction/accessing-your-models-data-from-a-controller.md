---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un Controller | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d3dfa079c334e04f368531456ec2ec4e9728f893
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873557"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="182ca-102">Accesso ai dati del modello da un Controller</span><span class="sxs-lookup"><span data-stu-id="182ca-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="182ca-103">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="182ca-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="182ca-104">In questa sezione si creerà un nuovo `MoviesController` classe e scrivere codice che recupera i dati dei film e lo visualizza in browser utilizzando un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="182ca-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="182ca-105">**Compilare l'applicazione** prima di procedere al passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="182ca-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="182ca-106">Se l'applicazione, si otterrà un errore durante l'aggiunta di un controller.</span><span class="sxs-lookup"><span data-stu-id="182ca-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="182ca-107">In Esplora soluzioni fare doppio clic su di *controller* cartella e quindi fare clic su **Aggiungi**, quindi **Controller**.</span><span class="sxs-lookup"><span data-stu-id="182ca-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="182ca-108">Nel **aggiungere lo scaffolding** nella finestra di dialogo fare clic su **Controller MVC 5 con visualizzazioni, mediante Entity Framework**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="182ca-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="182ca-109">Selezionare **filmato (MvcMovie.Models)** per la classe di modello.</span><span class="sxs-lookup"><span data-stu-id="182ca-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="182ca-110">Selezionare **MovieDBContext (MvcMovie.Models)** per la classe di contesto dati.</span><span class="sxs-lookup"><span data-stu-id="182ca-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="182ca-111">Immettere il nome del Controller **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="182ca-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="182ca-112">L'immagine seguente mostra la finestra di dialogo completata.</span><span class="sxs-lookup"><span data-stu-id="182ca-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="182ca-113">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="182ca-113">Click **Add**.</span></span> <span data-ttu-id="182ca-114">(Se si verifica un errore, probabile che non è stata compilare l'applicazione prima di avviare l'aggiunta del controller.) Visual Studio crea i file e cartelle seguenti:</span><span class="sxs-lookup"><span data-stu-id="182ca-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="182ca-115">*Un MoviesController.cs* del file nel *controller* cartella.</span><span class="sxs-lookup"><span data-stu-id="182ca-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="182ca-116">Oggetto *Views\Movies* cartella.</span><span class="sxs-lookup"><span data-stu-id="182ca-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="182ca-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, e *cshtml* nella nuova *Views\Movies* cartella.</span><span class="sxs-lookup"><span data-stu-id="182ca-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="182ca-118">Visual Studio creato automaticamente il [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (creare, leggere, aggiornare ed eliminare) i metodi di azione e le viste per l'utente (la creazione automatica delle viste e i metodi di azione CRUD è noto come scaffolding).</span><span class="sxs-lookup"><span data-stu-id="182ca-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="182ca-119">Ora è un'applicazione web completamente funzionale che consente di creare, elencare, modificare ed eliminare le voci di film.</span><span class="sxs-lookup"><span data-stu-id="182ca-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="182ca-120">Eseguire l'applicazione e fare clic sul **MVC film** collegamento (o selezionare il `Movies` controller aggiungendo */Movies* per l'URL nella barra degli indirizzi del browser).</span><span class="sxs-lookup"><span data-stu-id="182ca-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="182ca-121">Poiché l'applicazione si basa il routing predefinito (definito nel *App\_Start\RouteConfig.cs* file), la richiesta del browser `http://localhost:xxxxx/Movies` viene indirizzato per il valore predefinito `Index` metodo di azione del `Movies` controller.</span><span class="sxs-lookup"><span data-stu-id="182ca-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="182ca-122">In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` corrisponde in modo efficace la richiesta del browser `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="182ca-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="182ca-123">Il risultato è un elenco vuoto di film, perché ancora non sono stati aggiunti.</span><span class="sxs-lookup"><span data-stu-id="182ca-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="182ca-124">Creazione di un film</span><span class="sxs-lookup"><span data-stu-id="182ca-124">Creating a Movie</span></span>

<span data-ttu-id="182ca-125">Selezionare il collegamento **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="182ca-125">Select the **Create New** link.</span></span> <span data-ttu-id="182ca-126">Immettere alcune informazioni dettagliate su un filmato, quindi scegliere il **crea** pulsante.</span><span class="sxs-lookup"><span data-stu-id="182ca-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="182ca-127">Potrebbe non essere in grado di immettere decimali o virgole nel campo Prezzo.</span><span class="sxs-lookup"><span data-stu-id="182ca-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="182ca-128">per supportare la convalida jQuery per inglesi che utilizzano una virgola (&quot;,&quot;) per un separatore decimale e formati di data non in lingua inglese Stati Uniti, è necessario includere *globalize.js* specifici e  *Cultures/globalize.Cultures.js* file (da [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) e JavaScript per utilizzare `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="182ca-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="182ca-129">Illustrato come eseguire questa operazione nella prossima esercitazione.</span><span class="sxs-lookup"><span data-stu-id="182ca-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="182ca-130">Per il momento, immettere solo numeri interi come 10.</span><span class="sxs-lookup"><span data-stu-id="182ca-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="182ca-131">Fare clic su di **crea** pulsante fa sì che il modulo di essere inviata al server, in cui le informazioni di film vengono salvate nel database.</span><span class="sxs-lookup"><span data-stu-id="182ca-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="182ca-132">Quindi si viene reindirizzati al */Movies* URL, in cui è possibile visualizzare il film appena creato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="182ca-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="182ca-133">Creare altre due voci di filmato.</span><span class="sxs-lookup"><span data-stu-id="182ca-133">Create a couple more movie entries.</span></span> <span data-ttu-id="182ca-134">Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.</span><span class="sxs-lookup"><span data-stu-id="182ca-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="182ca-135">Analisi del codice generato</span><span class="sxs-lookup"><span data-stu-id="182ca-135">Examining the Generated Code</span></span>

<span data-ttu-id="182ca-136">Aprire il *Controllers\MoviesController.cs* file ed esaminare generato `Index` metodo.</span><span class="sxs-lookup"><span data-stu-id="182ca-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="182ca-137">Una parte del controller di film con il `Index` metodo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="182ca-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="182ca-138">Una richiesta per il `Movies` controller restituisce tutte le voci la `Movies` tabella e quindi passa i risultati per il `Index` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="182ca-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="182ca-139">La riga seguente dalla `MoviesController` classe crea un'istanza di un contesto di database, film, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="182ca-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="182ca-140">È possibile utilizzare il contesto del database film per eseguire una query, modificare ed eliminare i film.</span><span class="sxs-lookup"><span data-stu-id="182ca-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="182ca-141">Fortemente tipizzate modelli e @model (parola chiave)</span><span class="sxs-lookup"><span data-stu-id="182ca-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="182ca-142">In precedenza in questa esercitazione, si è visto come un controller può passare oggetti o dati a un modello di visualizzazione utilizzando il `ViewBag` oggetto.</span><span class="sxs-lookup"><span data-stu-id="182ca-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="182ca-143">Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico ad associazione tardiva per passare informazioni a una vista.</span><span class="sxs-lookup"><span data-stu-id="182ca-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="182ca-144">MVC fornisce inoltre la possibilità di passare *fortemente* un modello di visualizzazione di oggetti tipizzati.</span><span class="sxs-lookup"><span data-stu-id="182ca-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="182ca-145">Questo approccio fortemente tipizzato consente una migliore in fase di compilazione più ricco e controllo del codice [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) nell'editor di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="182ca-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="182ca-146">Il meccanismo di scaffolding in Visual Studio utilizzato questo approccio (vale a dire, passando un *fortemente* modello tipizzato) con il `MoviesController` modelli di classe e di visualizzazione quando creato i metodi e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="182ca-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="182ca-147">Nel *Controllers\MoviesController.cs* file esaminare generato `Details` metodo.</span><span class="sxs-lookup"><span data-stu-id="182ca-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="182ca-148">Il `Details` metodo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="182ca-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="182ca-149">Il `id` parametro viene in genere passato come dati di route, ad esempio `http://localhost:1234/movies/details/1` imposterà il controller al controller di film, l'azione da `details` e `id` su 1.</span><span class="sxs-lookup"><span data-stu-id="182ca-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="182ca-150">È inoltre possibile passare l'id con una stringa di query come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="182ca-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="182ca-151">Se un `Movie` viene trovato, un'istanza del `Movie` modello viene passato per il `Details` Vista:</span><span class="sxs-lookup"><span data-stu-id="182ca-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="182ca-152">Esaminare il contenuto del *Views\Movies\Details.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="182ca-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="182ca-153">Includendo un `@model` istruzione all'inizio del file di modello di visualizzazione, è possibile specificare il tipo di oggetto che prevede la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="182ca-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="182ca-154">Al momento della creazione del controller di film, l'istruzione `@model` è stata inclusa automaticamente in Visual Studio all'inizio del file *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="182ca-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="182ca-155">Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="182ca-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="182ca-156">Ad esempio, nel *Details.cshtml* modello, il codice passa tutti i campi film il `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) helper HTML con l'oggetto fortemente tipizzato `Model` oggetto.</span><span class="sxs-lookup"><span data-stu-id="182ca-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="182ca-157">Il `Create` e `Edit` metodi e i modelli di visualizzazione anche passano un oggetto modello film.</span><span class="sxs-lookup"><span data-stu-id="182ca-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="182ca-158">Esaminare il *cshtml* modello di visualizzazione e la `Index` metodo il *MoviesController.cs* file.</span><span class="sxs-lookup"><span data-stu-id="182ca-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="182ca-159">Si noti come il codice crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) oggetto quando viene chiamata la `View` il metodo di supporto nel `Index` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="182ca-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="182ca-160">Il codice passa quindi questo `Movies` elenco dal `Index` il metodo di azione alla visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="182ca-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="182ca-161">Al momento della creazione del controller di film, Visual Studio automaticamente includeva `@model` istruzione all'inizio del *cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="182ca-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="182ca-162">Questo `@model` direttiva consente di accedere all'elenco di film che passato alla visualizzazione per il controller utilizzando un `Model` oggetto fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="182ca-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="182ca-163">Ad esempio, nel *cshtml* modello, il codice scorre i film in questo modo un `foreach` istruzione tramite l'oggetto fortemente tipizzato `Model` oggetto:</span><span class="sxs-lookup"><span data-stu-id="182ca-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="182ca-164">Poiché il `Model` sono fortemente tipizzato di oggetti (come un `IEnumerable<Movie>` oggetto), ogni `item` oggetto nel ciclo è tipizzato come `Movie`.</span><span class="sxs-lookup"><span data-stu-id="182ca-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="182ca-165">Tra gli altri vantaggi, ciò significa che ottenere il controllo del codice in fase di compilazione e di supporto di IntelliSense nell'editor di codice completo:</span><span class="sxs-lookup"><span data-stu-id="182ca-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="182ca-167">Utilizzo di SQL Server Local DB</span><span class="sxs-lookup"><span data-stu-id="182ca-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="182ca-168">Code First di Entity Framework ha rilevato che a cui punta la stringa di connessione di database che è stata fornita un `Movies` database che non esiste ancora, pertanto il primo codice creato il database automaticamente.</span><span class="sxs-lookup"><span data-stu-id="182ca-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="182ca-169">È possibile verificare che sia stato creato esaminando il *App\_dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="182ca-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="182ca-170">Se non viene visualizzato il *Movies.mdf* file, fare clic sul **Mostra tutti i file** pulsante il **Esplora** sulla barra degli strumenti, fare clic sul **aggiornare** pulsante e quindi espandere il *App\_dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="182ca-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="182ca-171">Fare doppio clic su *Movies.mdf* per aprire **Esplora SERVER**, quindi espandere il **tabelle** cartella per visualizzare la tabella di film.</span><span class="sxs-lookup"><span data-stu-id="182ca-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="182ca-172">Si noti l'icona accanto a ID chiave</span><span class="sxs-lookup"><span data-stu-id="182ca-172">Note the key icon next to ID.</span></span> <span data-ttu-id="182ca-173">Per impostazione predefinita, EF consentirà una proprietà denominata ID chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="182ca-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="182ca-174">Per ulteriori informazioni su EF and MVC, vedere ottima esercitazione di Tom Dykstra in [MVC ed EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="182ca-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="182ca-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="182ca-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="182ca-176">Fare doppio clic su di `Movies` tabella e selezionare **Mostra dati tabella** per visualizzare i dati è stato creato.</span><span class="sxs-lookup"><span data-stu-id="182ca-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="182ca-177">Fare doppio clic su di `Movies` tabella e selezionare **Apri definizione tabella** per visualizzare la tabella di struttura che Entity Framework Code First creato.</span><span class="sxs-lookup"><span data-stu-id="182ca-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="182ca-178">Si noti come lo schema del `Movies` esegue il mapping alla tabella il `Movie` classe creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="182ca-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="182ca-179">Code First di Entity Framework automaticamente questo schema creato in base al `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="182ca-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="182ca-180">Al termine, chiudere la connessione facendo clic *MovieDBContext* e selezionando **Chiudi connessione**.</span><span class="sxs-lookup"><span data-stu-id="182ca-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="182ca-181">(Se si non chiude la connessione, si verifichi un errore alla successiva che esecuzione del progetto).</span><span class="sxs-lookup"><span data-stu-id="182ca-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="182ca-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="182ca-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="182ca-183">È ora disponibile un database e alcune pagine per visualizzare, modificare, aggiornare ed eliminare dati.</span><span class="sxs-lookup"><span data-stu-id="182ca-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="182ca-184">Nella prossima esercitazione, si sarà il resto del codice scaffolding di esaminare e aggiungere un `SearchIndex` (metodo) e un `SearchIndex` visualizzazione che consente di eseguire la ricerca di filmati in questo database.</span><span class="sxs-lookup"><span data-stu-id="182ca-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="182ca-185">Per ulteriori informazioni sull'utilizzo di Entity Framework con MVC, vedere [la creazione di un modello di dati di Entity Framework per un'applicazione MVC ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="182ca-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="182ca-186">[Precedente](creating-a-connection-string.md)
> [Successivo](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="182ca-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
