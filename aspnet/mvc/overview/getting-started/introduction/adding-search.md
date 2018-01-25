---
uid: mvc/overview/getting-started/introduction/adding-search
title: Ricerca | Documenti Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 116f681e14af0a09a4eb1502ef9f057c5db2f97d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="search"></a><span data-ttu-id="5540e-102">Cerca</span><span class="sxs-lookup"><span data-stu-id="5540e-102">Search</span></span>
====================
<span data-ttu-id="5540e-103">Da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="5540e-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="5540e-104">Aggiunta di un metodo di ricerca e visualizzazione di ricerca</span><span class="sxs-lookup"><span data-stu-id="5540e-104">Adding a Search Method and Search View</span></span>

<span data-ttu-id="5540e-105">In questa sezione viene aggiunta la funzionalità di ricerca per il `Index` metodo di azione che consente di eseguire la ricerca filmati genre o nome.</span><span class="sxs-lookup"><span data-stu-id="5540e-105">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="updating-the-index-form"></a><span data-ttu-id="5540e-106">Aggiornamenti del modulo di indice</span><span class="sxs-lookup"><span data-stu-id="5540e-106">Updating the Index Form</span></span>

<span data-ttu-id="5540e-107">Avviare aggiornando il `Index` il metodo di azione esistente `MoviesController` classe.</span><span class="sxs-lookup"><span data-stu-id="5540e-107">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="5540e-108">Ecco il codice:</span><span class="sxs-lookup"><span data-stu-id="5540e-108">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="5540e-109">La prima riga del `Index` metodo vengono creati i seguenti [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query per selezionare i film:</span><span class="sxs-lookup"><span data-stu-id="5540e-109">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="5540e-110">La query viene definita a questo punto, ma non è ancora stata eseguita sul database.</span><span class="sxs-lookup"><span data-stu-id="5540e-110">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="5540e-111">Se il `searchString` parametro contiene una stringa, viene modificata la query di filmati per filtrare il valore della stringa di ricerca, tramite il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5540e-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="5540e-112">Il codice `s => s.Title` precedente è un'[espressione lambda](https://msdn.microsoft.com/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="5540e-112">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="5540e-113">Le espressioni lambda vengono utilizzate in base al metodo [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) esegue una query come argomenti dei metodi degli operatori query standard, ad esempio il [dove](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) utilizzato nel codice precedente.</span><span class="sxs-lookup"><span data-stu-id="5540e-113">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="5540e-114">Le query LINQ non vengono eseguite quando vengono definite o quando vengono modificati chiamando un metodo, ad esempio `Where` o `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="5540e-114">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="5540e-115">In alternativa, esecuzione della query è rinviata, il che significa che la valutazione di un'espressione viene ritardata fino a quando il relativo valore realizzato viene effettivamente eseguita un'iterazione o [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metodo viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="5540e-115">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="5540e-116">Nel `Search` esempio, in cui viene eseguita la query di *cshtml* visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="5540e-116">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="5540e-117">Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="5540e-117">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5540e-118">Il [Contains](https://msdn.microsoft.com/library/bb155125.aspx) metodo viene eseguito sul database, non il codice c# sopra riportato.</span><span class="sxs-lookup"><span data-stu-id="5540e-118">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="5540e-119">Il database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) esegue il mapping a [SQL come](https://msdn.microsoft.com/library/ms179859.aspx), che viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="5540e-119">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="5540e-120">Ora è possibile aggiornare il `Index` visualizzazione che consente di visualizzare il form per l'utente.</span><span class="sxs-lookup"><span data-stu-id="5540e-120">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="5540e-121">Eseguire l'applicazione e passare a *filmati/indice*.</span><span class="sxs-lookup"><span data-stu-id="5540e-121">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="5540e-122">Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL.</span><span class="sxs-lookup"><span data-stu-id="5540e-122">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="5540e-123">Vengono visualizzati i film filtrati.</span><span class="sxs-lookup"><span data-stu-id="5540e-123">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="5540e-125">Se si modifica la firma del `Index` metodo contiene un parametro denominato `id`, `id` parametro corrisponderà il `{id}` indirizza i segnaposto per il valore predefinito impostato nel *App\_Start RouteConfig.cs* file.</span><span class="sxs-lookup"><span data-stu-id="5540e-125">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="5540e-126">Originale `Index` metodo è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="5540e-126">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="5540e-127">Modificato `Index` metodo si presenterebbe come segue:</span><span class="sxs-lookup"><span data-stu-id="5540e-127">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="5540e-128">È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="5540e-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="5540e-129">Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film.</span><span class="sxs-lookup"><span data-stu-id="5540e-129">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="5540e-130">Quindi, ora è aggiungerai dell'interfaccia utente per consentire loro filtrare film.</span><span class="sxs-lookup"><span data-stu-id="5540e-130">So now you you'll add UI to help them filter movies.</span></span> <span data-ttu-id="5540e-131">Se è stata modificata la firma del `Index` metodo per verificare come passare il parametro ID di associazione di route, modificarlo in modo che il `Index` metodo accetta un parametro di stringa denominato `searchString`:</span><span class="sxs-lookup"><span data-stu-id="5540e-131">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="5540e-132">Aprire il *Views\Movies\Index.cshtml* file e subito dopo `@Html.ActionLink("Create New", "Create")`, aggiungere il markup modulo evidenziato di seguito:</span><span class="sxs-lookup"><span data-stu-id="5540e-132">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="5540e-133">Il `Html.BeginForm` helper crea un'apertura `<form>` tag.</span><span class="sxs-lookup"><span data-stu-id="5540e-133">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="5540e-134">Il `Html.BeginForm` helper, il form inviare a se stessa quando l'utente invia il form scegliendo il **filtro** pulsante.</span><span class="sxs-lookup"><span data-stu-id="5540e-134">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="5540e-135">Visual Studio 2013 è un miglioramento utile per la visualizzazione e modifica dei file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="5540e-135">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="5540e-136">Quando si esegue l'applicazione con un file della vista aperta, Visual Studio 2013 richiama il metodo di azione del controller corretto per visualizzare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="5540e-136">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="5540e-137">Con la visualizzazione dell'indice aperto in Visual Studio (come illustrato nella figura precedente), toccare Ctr F5 o F5 per eseguire l'applicazione, quindi ripetere la ricerca di un film.</span><span class="sxs-lookup"><span data-stu-id="5540e-137">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="5540e-138">È presente alcun `HttpPost` overload di `Index` metodo.</span><span class="sxs-lookup"><span data-stu-id="5540e-138">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="5540e-139">Non è necessaria, poiché il metodo non modifica lo stato dell'applicazione, solo il filtraggio dei dati.</span><span class="sxs-lookup"><span data-stu-id="5540e-139">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="5540e-140">È possibile aggiungere il metodo `HttpPost Index` seguente.</span><span class="sxs-lookup"><span data-stu-id="5540e-140">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="5540e-141">In tal caso, corrisponderà a invoker dell'azione di `HttpPost Index` (metodo) e `HttpPost Index` metodo verrebbe eseguito come illustrato nell'immagine seguente.</span><span class="sxs-lookup"><span data-stu-id="5540e-141">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="5540e-143">Tuttavia, anche se si aggiunge questa versione `HttpPost` del metodo `Index`, esiste una limitazione sul modo sulla relativa implementazione.</span><span class="sxs-lookup"><span data-stu-id="5540e-143">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="5540e-144">Si supponga che si desideri usare come segnalibro una ricerca specifica o inviare un collegamento agli amici su cui possono fare clic per visualizzare lo stesso elenco filtrato di film.</span><span class="sxs-lookup"><span data-stu-id="5540e-144">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="5540e-145">Si noti che l'URL per la richiesta HTTP POST è lo stesso come URL per la richiesta di recupero (localhost:xxxxx filmati/indice)--non sono disponibili informazioni di ricerca nell'URL stesso.</span><span class="sxs-lookup"><span data-stu-id="5540e-145">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="5540e-146">Diritto a questo punto, le informazioni sulla stringa di ricerca viene inviati al server come un valore del campo modulo.</span><span class="sxs-lookup"><span data-stu-id="5540e-146">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="5540e-147">Ciò significa che non è possibile acquisire le informazioni di ricerca per aggiungere un segnalibro o inviare agli amici in un URL.</span><span class="sxs-lookup"><span data-stu-id="5540e-147">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="5540e-148">La soluzione consiste nell'usare un overload di `BeginForm` che specifica che la richiesta POST è necessario aggiungere le informazioni di ricerca per l'URL e che devono essere instradato al `HttpGet` versione il `Index` (metodo).</span><span class="sxs-lookup"><span data-stu-id="5540e-148">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="5540e-149">Sostituire le funzioni senza parametri `BeginForm` (metodo) con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="5540e-149">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="5540e-151">A questo punto quando si invia una ricerca, l'URL contiene una stringa di query di ricerca.</span><span class="sxs-lookup"><span data-stu-id="5540e-151">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="5540e-152">La ricerca passerà anche al metodo di azione `HttpGet Index`, anche se si dispone di un metodo `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="5540e-152">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="5540e-154">Aggiunta di ricerca per genere</span><span class="sxs-lookup"><span data-stu-id="5540e-154">Adding Search by Genre</span></span>

<span data-ttu-id="5540e-155">Se è stato aggiunto il `HttpPost` versione il `Index` metodo elimina ora.</span><span class="sxs-lookup"><span data-stu-id="5540e-155">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="5540e-156">Successivamente, aggiungere una funzionalità per consentire agli utenti di cercare film per genere.</span><span class="sxs-lookup"><span data-stu-id="5540e-156">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="5540e-157">Sostituire il metodo `Index` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5540e-157">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="5540e-158">Questa versione di `Index` metodo accetta un parametro aggiuntivo, vale a dire `movieGenre`.</span><span class="sxs-lookup"><span data-stu-id="5540e-158">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="5540e-159">Creano le prime righe di codice un `List` oggetto che contenga generi film dal database.</span><span class="sxs-lookup"><span data-stu-id="5540e-159">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="5540e-160">Il codice seguente è una query LINQ che recupera tutti i generi dal database.</span><span class="sxs-lookup"><span data-stu-id="5540e-160">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="5540e-161">Il codice Usa il `AddRange` metodo del tipo generico `List` raccolta da aggiungere all'elenco di tutti i generi distinti.</span><span class="sxs-lookup"><span data-stu-id="5540e-161">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="5540e-162">(Senza il `Distinct` modificatore, generi duplicati verrebbero aggiunto, ad esempio, verrebbero aggiunto comici due volte nel nostro esempio).</span><span class="sxs-lookup"><span data-stu-id="5540e-162">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="5540e-163">Il codice quindi archivia l'elenco di generi nel `ViewBag.MovieGenre` oggetto.</span><span class="sxs-lookup"><span data-stu-id="5540e-163">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="5540e-164">L'archiviazione dei dati della categoria (del tali un genere) come un [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) dell'oggetto un `ViewBag`, quindi l'accesso ai dati di categoria in una casella di riepilogo a discesa è un approccio tipico per le applicazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="5540e-164">Storing category data (such a movie genre's) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="5540e-165">Il codice seguente viene illustrato come controllare il `movieGenre` parametro.</span><span class="sxs-lookup"><span data-stu-id="5540e-165">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="5540e-166">Se non è vuota, ulteriormente il codice vincola la query di filmati per limitare i film selezionati per il genere specificato.</span><span class="sxs-lookup"><span data-stu-id="5540e-166">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="5540e-167">Come indicato in precedenza, la query non viene eseguita sulla base dei dati fino a quando non viene scorso l'elenco di film (situazione che si verifica nella visualizzazione, dopo il `Index` metodo di azione restituisce).</span><span class="sxs-lookup"><span data-stu-id="5540e-167">As stated previously, the query is not run on the data base until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="5540e-168">Aggiunta di Markup per la visualizzazione dell'indice per supportare la ricerca per genere</span><span class="sxs-lookup"><span data-stu-id="5540e-168">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="5540e-169">Aggiungere un `Html.DropDownList` helper per il *Views\Movies\Index.cshtml* file, appena prima di `TextBox` helper.</span><span class="sxs-lookup"><span data-stu-id="5540e-169">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="5540e-170">Di seguito è riportato il markup completato:</span><span class="sxs-lookup"><span data-stu-id="5540e-170">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="5540e-171">Nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5540e-171">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="5540e-172">Il parametro "MovieGenre" fornisce la chiave per la `DropDownList` helper per trovare un `IEnumerable<SelectListItem>` nel `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="5540e-172">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="5540e-173">Il `ViewBag` è stato popolato nel metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="5540e-173">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="5540e-174">Il parametro "All" fornisce un'etichetta di opzione.</span><span class="sxs-lookup"><span data-stu-id="5540e-174">The parameter "All" provides an option label.</span></span> <span data-ttu-id="5540e-175">Se si analizza l'opzione nel browser, si noterà che l'attributo "value" è vuota.</span><span class="sxs-lookup"><span data-stu-id="5540e-175">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="5540e-176">Poiché il controller filtra solo `if` la stringa non è `null` o empty, invio di un valore vuoto per `movieGenre` Mostra tutti i generi.</span><span class="sxs-lookup"><span data-stu-id="5540e-176">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="5540e-177">È inoltre possibile impostare un'opzione per essere selezionata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5540e-177">You can also set an option to be selected by default.</span></span> <span data-ttu-id="5540e-178">Se si desidera "Commedia" come opzione di impostazione predefinita, si modificherebbe il codice nel Controller come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="5540e-178">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="5540e-179">Eseguire l'applicazione e passare a *filmati/indice*.</span><span class="sxs-lookup"><span data-stu-id="5540e-179">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="5540e-180">Eseguire una ricerca per genere, in base al nome di film e da entrambi i criteri.</span><span class="sxs-lookup"><span data-stu-id="5540e-180">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="5540e-181">In questa sezione è creato un metodo di azione di ricerca e visualizzazione che consentono agli utenti di ricerca in base al genere e titolo del film.</span><span class="sxs-lookup"><span data-stu-id="5540e-181">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="5540e-182">Nella sezione successiva verrà esaminato come aggiungere una proprietà per il `Movie` modello e come aggiungere un inizializzatore che verrà creato automaticamente un database di test.</span><span class="sxs-lookup"><span data-stu-id="5540e-182">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5540e-183">[Precedente](examining-the-edit-methods-and-edit-view.md)
[Successivo](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="5540e-183">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
