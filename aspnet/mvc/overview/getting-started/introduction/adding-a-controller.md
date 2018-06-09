---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Aggiunta di un Controller | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 3864bab284661b0c44f9e4cb363c2d60eccc7c66
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "30869621"
---
<a name="adding-a-controller"></a><span data-ttu-id="e89b9-102">Aggiunta di un controller</span><span class="sxs-lookup"><span data-stu-id="e89b9-102">Adding a Controller</span></span>
====================
<span data-ttu-id="e89b9-103">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e89b9-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="e89b9-104">MVC è l'acronimo di *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="e89b9-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="e89b9-105">MVC è un modello per lo sviluppo di applicazioni che sono ben strutturato, testabile e facili da gestire.</span><span class="sxs-lookup"><span data-stu-id="e89b9-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="e89b9-106">Applicazioni basate su MVC contengono:</span><span class="sxs-lookup"><span data-stu-id="e89b9-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="e89b9-107">**M** odels: le classi che rappresentano i dati dell'applicazione e che usano la logica di convalida per applicare le regole di business per tali dati.</span><span class="sxs-lookup"><span data-stu-id="e89b9-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="e89b9-108">**V** iews: file di modello che l'applicazione usa per generare in modo dinamico le risposte HTML.</span><span class="sxs-lookup"><span data-stu-id="e89b9-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="e89b9-109">**C** ontrollers: le classi che gestiscono le richieste del browser in ingresso, recuperare i dati del modello e quindi specificare i modelli di visualizzazione che restituiscono una risposta nel browser.</span><span class="sxs-lookup"><span data-stu-id="e89b9-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="e89b9-110">Si verrà tutti questi concetti in questa serie di esercitazioni di copertura e viene illustrato come usarle per compilare un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e89b9-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="e89b9-111">Il modello è stato selezionato nel passaggio precedente il MVC predefinito.</span><span class="sxs-lookup"><span data-stu-id="e89b9-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="e89b9-112">Crea Home, Account e gestire i controller per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e89b9-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="e89b9-113">Si inizierà creando una classe controller.</span><span class="sxs-lookup"><span data-stu-id="e89b9-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="e89b9-114">In **Esplora**, fare doppio clic su di *controller* cartella e quindi fare clic su **Aggiungi**, quindi **Controller**.</span><span class="sxs-lookup"><span data-stu-id="e89b9-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="e89b9-115">Nel **aggiungere lo scaffolding** nella finestra di dialogo fare clic su **Controller MVC 5 - vuoto**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="e89b9-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="e89b9-116">Il nuovo controller "HelloWorldController" e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="e89b9-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Aggiungi controller](adding-a-controller/_static/image3.png)

<span data-ttu-id="e89b9-118">Si noti che nel **Esplora** che un nuovo file è stato creato denominato *HelloWorldController.cs* e una nuova cartella *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="e89b9-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="e89b9-119">Il controller è aperto nell'IDE.</span><span class="sxs-lookup"><span data-stu-id="e89b9-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="e89b9-120">Sostituire il contenuto del file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="e89b9-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="e89b9-121">I metodi di controller verranno restituita una stringa HTML come esempio.</span><span class="sxs-lookup"><span data-stu-id="e89b9-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="e89b9-122">Il controller è denominato `HelloWorldController` e il primo metodo è denominato `Index`.</span><span class="sxs-lookup"><span data-stu-id="e89b9-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="e89b9-123">Verrà richiamato da un browser.</span><span class="sxs-lookup"><span data-stu-id="e89b9-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="e89b9-124">Eseguire l'applicazione (premere F5 o Ctrl + F5).</span><span class="sxs-lookup"><span data-stu-id="e89b9-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="e89b9-125">Nel browser, aggiungere &quot;HelloWorld&quot; per il percorso nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="e89b9-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="e89b9-126">(Ad esempio, nella figura seguente, la `http://localhost:1234/HelloWorld.`) la pagina nel browser apparirà la schermata seguente.</span><span class="sxs-lookup"><span data-stu-id="e89b9-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="e89b9-127">Nel metodo, il codice restituito direttamente una stringa.</span><span class="sxs-lookup"><span data-stu-id="e89b9-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="e89b9-128">È indicato il sistema da restituire solo il codice HTML e aveva!</span><span class="sxs-lookup"><span data-stu-id="e89b9-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="e89b9-129">ASP.NET MVC richiama classi controller diverso (e i metodi di azione diversi all'interno di essi) a seconda dell'URL in ingresso.</span><span class="sxs-lookup"><span data-stu-id="e89b9-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="e89b9-130">La logica di routing URL predefinito utilizzata da MVC ASP.NET utilizza un formato simile al seguente per determinare quale codice per richiamare:</span><span class="sxs-lookup"><span data-stu-id="e89b9-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="e89b9-131">Impostare il formato per il routing di *App\_Start/RouteConfig.cs* file.</span><span class="sxs-lookup"><span data-stu-id="e89b9-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="e89b9-132">Quando si esegue l'applicazione e non forniscono eventuali segmenti di URL, il valore predefinito è il controller "Home" e il metodo di azione "Index" specificato nella sezione Impostazioni predefinite del codice precedente.</span><span class="sxs-lookup"><span data-stu-id="e89b9-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="e89b9-133">La prima parte dell'URL determina la classe controller da eseguire.</span><span class="sxs-lookup"><span data-stu-id="e89b9-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="e89b9-134">Pertanto, */HelloWorld* esegue il mapping alla `HelloWorldController` classe.</span><span class="sxs-lookup"><span data-stu-id="e89b9-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="e89b9-135">La seconda parte dell'URL determina il metodo di azione della classe per l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="e89b9-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="e89b9-136">Così */HelloWorld/indice* causerebbe il `Index` metodo il `HelloWorldController` classe da eseguire.</span><span class="sxs-lookup"><span data-stu-id="e89b9-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="e89b9-137">Si noti che abbiamo solo passare a */HelloWorld* e `Index` metodo utilizzato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e89b9-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="e89b9-138">In questo modo un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller non è specificata in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="e89b9-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="e89b9-139">La terza parte del segmento di URL ( `Parameters`) è relativa ai dati di route.</span><span class="sxs-lookup"><span data-stu-id="e89b9-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="e89b9-140">Dati della route verrà illustrata più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e89b9-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="e89b9-141">Passare a `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="e89b9-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="e89b9-142">Il `Welcome` metodo viene eseguito e restituisce la stringa &quot;si tratta del metodo di azione di completamento dell'installazione... &quot;.</span><span class="sxs-lookup"><span data-stu-id="e89b9-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="e89b9-143">Il mapping di MVC predefinita è `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="e89b9-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="e89b9-144">Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`.</span><span class="sxs-lookup"><span data-stu-id="e89b9-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="e89b9-145">Non è stata ancora usata la parte `[Parameters]` dell'URL.</span><span class="sxs-lookup"><span data-stu-id="e89b9-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="e89b9-146">Modifichiamo l'esempio leggermente, in modo che è possibile passare alcune informazioni sui parametri dall'URL per il controller (ad esempio, */HelloWorld/iniziale? name = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="e89b9-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="e89b9-147">Modifica il `Welcome` metodo includere due parametri, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e89b9-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="e89b9-148">Si noti che il codice Usa la funzionalità di c# parametro facoltativo per indicare che il `numTimes` parametro deve essere predefinito su 1 se per tale parametro viene passato alcun valore.</span><span class="sxs-lookup"><span data-stu-id="e89b9-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="e89b9-149">Nota sulla sicurezza: Il codice precedente viene [HttpUtility](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) per proteggere l'applicazione da input dannosi (vale a dire JavaScript).</span><span class="sxs-lookup"><span data-stu-id="e89b9-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="e89b9-150">Per ulteriori informazioni vedere [procedura: proteggere dagli attacchi tramite Script in un'applicazione Web da applicare la codifica HTML alle stringhe](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="e89b9-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="e89b9-151">Eseguire l'applicazione e passare all'URL di esempio (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="e89b9-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="e89b9-152">È possibile provare diversi valori per `name` e `numtimes` nell'URL.</span><span class="sxs-lookup"><span data-stu-id="e89b9-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="e89b9-153">Il [sistema di associazione del modello MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) associa automaticamente i parametri denominati dalla stringa di query nella barra degli indirizzi per i parametri del metodo.</span><span class="sxs-lookup"><span data-stu-id="e89b9-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="e89b9-154">Nell'esempio precedente, il segmento URL ( `Parameters`) non viene utilizzato il `name` e `numTimes` i parametri vengono passati come [delle stringhe di query](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="e89b9-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="e89b9-155">Il carattere jolly ?</span><span class="sxs-lookup"><span data-stu-id="e89b9-155">The ?</span></span> <span data-ttu-id="e89b9-156">(punto interrogativo) nell'URL precedente è un separatore e seguono le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="e89b9-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="e89b9-157">Il carattere &amp; separa le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="e89b9-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="e89b9-158">Sostituire il metodo di completamento dell'installazione con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e89b9-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="e89b9-159">Eseguire l'applicazione e immettere l'URL seguente: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="e89b9-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="e89b9-160">Questa volta il terzo segmento di URL corrispondente parametro di route `ID.` il `Welcome` il metodo di azione contiene un parametro (`ID`) corrispondenti della specifica URL di `RegisterRoutes` (metodo).</span><span class="sxs-lookup"><span data-stu-id="e89b9-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="e89b9-161">Nelle applicazioni ASP.NET MVC, è più comune di comunicazione per passare i parametri come dati di route (come abbiamo visto con ID precedente) di essere passate come stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="e89b9-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="e89b9-162">È inoltre possibile aggiungere una route per soddisfare entrambi i `name` e `numtimes` nei parametri come dati di route nell'URL.</span><span class="sxs-lookup"><span data-stu-id="e89b9-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="e89b9-163">Nel *App\_Start\RouteConfig.cs* file, aggiungere la route "Hello":</span><span class="sxs-lookup"><span data-stu-id="e89b9-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="e89b9-164">Eseguire l'applicazione e passare a `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="e89b9-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="e89b9-165">Per molte applicazioni MVC, la route predefinita funziona correttamente.</span><span class="sxs-lookup"><span data-stu-id="e89b9-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="e89b9-166">Verrà illustrato più avanti in questa esercitazione per passare i dati utilizzando lo strumento di associazione del modello e non sarà necessario modificare la route predefinita per tale.</span><span class="sxs-lookup"><span data-stu-id="e89b9-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="e89b9-167">In questi esempi di operazioni il controller del &quot;VC&quot; parte di MVC, vale a dire, il lavoro di visualizzazione e controller.</span><span class="sxs-lookup"><span data-stu-id="e89b9-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="e89b9-168">Il controller ha restituito HTML direttamente.</span><span class="sxs-lookup"><span data-stu-id="e89b9-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="e89b9-169">In genere non si desidera controller restituzione HTML direttamente, dal momento che diventa molto complessa al codice.</span><span class="sxs-lookup"><span data-stu-id="e89b9-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="e89b9-170">Invece in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="e89b9-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="e89b9-171">Verrà ora esaminato il successivo come possiamo farlo.</span><span class="sxs-lookup"><span data-stu-id="e89b9-171">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e89b9-172">[Precedente](getting-started.md)
> [Successivo](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="e89b9-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
