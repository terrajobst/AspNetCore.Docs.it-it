---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controller | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 2 vengono illustrati i controller.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-2-controllers"></a><span data-ttu-id="79bd3-104">Parte 2: controller</span><span class="sxs-lookup"><span data-stu-id="79bd3-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="79bd3-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="79bd3-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="79bd3-106">L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="79bd3-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="79bd3-107">L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="79bd3-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="79bd3-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio.</span><span class="sxs-lookup"><span data-stu-id="79bd3-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="79bd3-109">Parte 2 vengono illustrati i controller.</span><span class="sxs-lookup"><span data-stu-id="79bd3-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="79bd3-110">Con il framework di web tradizionali, gli URL in ingresso vengono in genere associati ai file su disco.</span><span class="sxs-lookup"><span data-stu-id="79bd3-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="79bd3-111">Ad esempio: una richiesta per un URL come "/ Products" o "/ Products.php" possono essere elaborate da un file "Products" o "Products.php".</span><span class="sxs-lookup"><span data-stu-id="79bd3-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="79bd3-112">Framework MVC basata sul Web di eseguire il mapping degli URL al codice server in modo leggermente diverso.</span><span class="sxs-lookup"><span data-stu-id="79bd3-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="79bd3-113">Anziché eseguire il mapping degli URL in ingresso ai file, gli URL sono invece associati ai metodi nelle classi.</span><span class="sxs-lookup"><span data-stu-id="79bd3-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="79bd3-114">Queste classi vengono chiamate "Controller" e sono responsabili per l'elaborazione delle richieste HTTP in ingresso, gestione dell'input dell'utente, il recupero e il salvataggio dei dati e determinare la risposta da inviare al client (visualizzazione del contenuto HTML, scaricare un file, il reindirizzamento a un altro URL e così via).</span><span class="sxs-lookup"><span data-stu-id="79bd3-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="79bd3-115">Aggiunta di un HomeController</span><span class="sxs-lookup"><span data-stu-id="79bd3-115">Adding a HomeController</span></span>

<span data-ttu-id="79bd3-116">L'applicazione MVC negozio inizieremo aggiungendo una classe Controller che gestirà gli URL per la Home page del sito.</span><span class="sxs-lookup"><span data-stu-id="79bd3-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="79bd3-117">Viene seguono le convenzioni di denominazione predefinito di ASP.NET MVC e chiamarlo HomeController.</span><span class="sxs-lookup"><span data-stu-id="79bd3-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="79bd3-118">Nella cartella "Controller" all'interno di Esplora soluzioni e scegliere "Aggiungi", quindi il "Controller..."</span><span class="sxs-lookup"><span data-stu-id="79bd3-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…"</span></span> <span data-ttu-id="79bd3-119">comando:</span><span class="sxs-lookup"><span data-stu-id="79bd3-119">command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="79bd3-120">Verrà visualizzata la finestra di dialogo "Aggiungi Controller".</span><span class="sxs-lookup"><span data-stu-id="79bd3-120">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="79bd3-121">Denominare il controller "HomeController" e fare clic sul pulsante Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="79bd3-121">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="79bd3-122">Si creerà un nuovo file HomeController. cs, con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="79bd3-122">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="79bd3-123">Per avviare semplicemente come possibile, sostituire il metodo di indice con un metodo semplice che restituisce una stringa.</span><span class="sxs-lookup"><span data-stu-id="79bd3-123">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="79bd3-124">Due modifiche da apportare:</span><span class="sxs-lookup"><span data-stu-id="79bd3-124">We'll make two changes:</span></span>

- <span data-ttu-id="79bd3-125">Modificare il metodo per restituire una stringa anziché un ActionResult</span><span class="sxs-lookup"><span data-stu-id="79bd3-125">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="79bd3-126">Modificare l'istruzione return per restituire "Hello dalla pagina iniziale"</span><span class="sxs-lookup"><span data-stu-id="79bd3-126">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="79bd3-127">Il metodo dovrebbe risultare simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="79bd3-127">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="79bd3-128">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="79bd3-128">Running the Application</span></span>

<span data-ttu-id="79bd3-129">Ora si esegue il sito.</span><span class="sxs-lookup"><span data-stu-id="79bd3-129">Now let's run the site.</span></span> <span data-ttu-id="79bd3-130">È possibile avviare il server web e provare a utilizzare il sito usando uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="79bd3-130">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="79bd3-131">Scegliere la voce di menu Avvia debug ⇨ di Debug</span><span class="sxs-lookup"><span data-stu-id="79bd3-131">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="79bd3-132">Fare clic sul pulsante freccia verde sulla barra degli strumenti ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="79bd3-132">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="79bd3-133">Utilizzare i tasti di scelta rapida F5.</span><span class="sxs-lookup"><span data-stu-id="79bd3-133">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="79bd3-134">Utilizzando uno dei passaggi precedenti verrà compilare il progetto e causare quindi il Server di sviluppo ASP.NET che viene compilato in Visual Web Developer per avviare.</span><span class="sxs-lookup"><span data-stu-id="79bd3-134">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="79bd3-135">Una notifica verrà visualizzato nell'angolo inferiore della schermata per indicare che ha avviato il Server di sviluppo ASP.NET e verrà visualizzato il numero di porta che viene eseguito con.</span><span class="sxs-lookup"><span data-stu-id="79bd3-135">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="79bd3-136">Visual Web Developer verrà quindi aperto automaticamente una finestra del browser con URL punta al server web.</span><span class="sxs-lookup"><span data-stu-id="79bd3-136">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="79bd3-137">Ciò permetterà di provare rapidamente l'applicazione web:</span><span class="sxs-lookup"><span data-stu-id="79bd3-137">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="79bd3-138">OK, che è stata abbastanza rapida: è stato creato un nuovo sito Web, aggiungere una funzione grafico three line e abbiamo testo in un browser.</span><span class="sxs-lookup"><span data-stu-id="79bd3-138">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="79bd3-139">Non stare informatica, ma è un modo per iniziare.</span><span class="sxs-lookup"><span data-stu-id="79bd3-139">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="79bd3-140">*Nota: Visual Web Developer comprende il Server di sviluppo ASP.NET che eseguiranno il sito Web in un numero casuale libero "porta". Nella schermata precedente, il sito è in esecuzione in `http://localhost:26641/`, pertanto è usando porta 26641. Il numero di porta è diverso. Quando si parlerà like /Store/Browse dell'URL in questa esercitazione, che entra dopo il numero di porta. Presupponendo un numero di porta del 26641, la selezione di archivio/Sfoglia implica la selezione di `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="79bd3-140">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="79bd3-141">Aggiunta di un StoreController</span><span class="sxs-lookup"><span data-stu-id="79bd3-141">Adding a StoreController</span></span>

<span data-ttu-id="79bd3-142">È stato aggiunto un semplice HomeController che implementa la Home Page del sito.</span><span class="sxs-lookup"><span data-stu-id="79bd3-142">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="79bd3-143">A questo punto aggiungere un altro controller che verranno utilizzate per implementare la funzionalità di esplorazione del Negozio musica.</span><span class="sxs-lookup"><span data-stu-id="79bd3-143">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="79bd3-144">Il controller di archivio verrà supportano tre scenari:</span><span class="sxs-lookup"><span data-stu-id="79bd3-144">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="79bd3-145">Una pagina di elenco di generi di musica nel negozio musica</span><span class="sxs-lookup"><span data-stu-id="79bd3-145">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="79bd3-146">Una pagina di esplorazione che elenca tutti gli album musica in un particolare genere</span><span class="sxs-lookup"><span data-stu-id="79bd3-146">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="79bd3-147">Una pagina di dettagli che contiene informazioni su un album musicali specifici</span><span class="sxs-lookup"><span data-stu-id="79bd3-147">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="79bd3-148">Inizieremo aggiungendo una nuova classe StoreController...</span><span class="sxs-lookup"><span data-stu-id="79bd3-148">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="79bd3-149">Se hai già fatto, arrestare l'esecuzione dell'applicazione mediante la chiusura del browser o selezionando il ⇨ Termina debug dal menu Debug.</span><span class="sxs-lookup"><span data-stu-id="79bd3-149">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="79bd3-150">Aggiungere un nuovo StoreController.</span><span class="sxs-lookup"><span data-stu-id="79bd3-150">Now add a new StoreController.</span></span> <span data-ttu-id="79bd3-151">Proprio come abbiamo fatto con HomeController, faremo questo facendo clic sulla cartella "Controller" all'interno di Esplora soluzioni e scegliendo Aggiungi -&gt;voce di menu Controller</span><span class="sxs-lookup"><span data-stu-id="79bd3-151">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="79bd3-152">Il nuovo StoreController dispone già di un metodo "Index".</span><span class="sxs-lookup"><span data-stu-id="79bd3-152">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="79bd3-153">Questo metodo "Index" useremo per implementare la pagina di elenco in cui sono elencati tutti i generi negozio musica.</span><span class="sxs-lookup"><span data-stu-id="79bd3-153">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="79bd3-154">Inoltre, verranno aggiunte due metodi aggiuntivi per implementare i due altri scenari vogliamo nostri StoreController di gestire: Sfoglia e dettagli.</span><span class="sxs-lookup"><span data-stu-id="79bd3-154">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="79bd3-155">Questi metodi (indice, esplorazione e i dettagli) all'interno di questo Controller vengono chiamati "Azioni del Controller" e come è stato già illustrato con il metodo di azione HomeController.Index (), il processo è per rispondere alle richieste di URL e (in genere) determinare il tipo di contenuto deve essere inviato nuovamente il browser o l'utente che ha richiamato l'URL.</span><span class="sxs-lookup"><span data-stu-id="79bd3-155">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="79bd3-156">Si inizierà l'implementazione di StoreController modificando theIndex() metodo per restituire la stringa "Hello da Store.Index()" e verrà aggiunto metodi simili per Browse () e Details():</span><span class="sxs-lookup"><span data-stu-id="79bd3-156">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="79bd3-157">Eseguire nuovamente il progetto e passare i seguenti URL:</span><span class="sxs-lookup"><span data-stu-id="79bd3-157">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="79bd3-158">/ Store</span><span class="sxs-lookup"><span data-stu-id="79bd3-158">/Store</span></span>
- <span data-ttu-id="79bd3-159">/ Archivio/Sfoglia</span><span class="sxs-lookup"><span data-stu-id="79bd3-159">/Store/Browse</span></span>
- <span data-ttu-id="79bd3-160">/ / Dettagli archivio</span><span class="sxs-lookup"><span data-stu-id="79bd3-160">/Store/Details</span></span>

<span data-ttu-id="79bd3-161">L'accesso a questi URL verrà richiamare i metodi di azione all'interno di questo Controller e restituire risposte stringa:</span><span class="sxs-lookup"><span data-stu-id="79bd3-161">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="79bd3-162">Che è molto utile, ma questi sono stringhe solo costanti.</span><span class="sxs-lookup"><span data-stu-id="79bd3-162">That's great, but these are just constant strings.</span></span> <span data-ttu-id="79bd3-163">Di seguito renderli dinamici, in modo che ricevono informazioni dall'URL e visualizzarla nell'output della pagina.</span><span class="sxs-lookup"><span data-stu-id="79bd3-163">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="79bd3-164">Innanzitutto si modificherà il metodo di azione di esplorazione per recuperare un valore di stringa di query dell'URL.</span><span class="sxs-lookup"><span data-stu-id="79bd3-164">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="79bd3-165">È possibile farlo mediante l'aggiunta di un parametro "genre" per il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="79bd3-165">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="79bd3-166">Quando si esegue questa operazione, ASP.NET MVC passerà automaticamente eventuali parametri post querystring o modulo denominati "genre" per il metodo di azione quando viene richiamata.</span><span class="sxs-lookup"><span data-stu-id="79bd3-166">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="79bd3-167">*Nota: Si usa il metodo di utilità HttpUtility per purificare all'input dell'utente. Ciò impedisce agli utenti di inserendo Javascript la vista con un collegamento come /Store/Browse? Genre =&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="79bd3-167">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="79bd3-168">Ora consente di passare a archivio/Sfoglia? Genre = Disco</span><span class="sxs-lookup"><span data-stu-id="79bd3-168">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="79bd3-169">Modificare quindi l'azione di dettagli per leggere e visualizzare un parametro di input denominato ID.</span><span class="sxs-lookup"><span data-stu-id="79bd3-169">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="79bd3-170">A differenza di questo metodo precedente, è non incorporare il valore ID come parametro di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="79bd3-170">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="79bd3-171">È invece sarà incorporarlo direttamente nell'URL stesso.</span><span class="sxs-lookup"><span data-stu-id="79bd3-171">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="79bd3-172">Ad esempio: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="79bd3-172">For example: /Store/Details/5.</span></span>

<span data-ttu-id="79bd3-173">ASP.NET MVC consente di farlo facilmente senza dover configurare alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="79bd3-173">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="79bd3-174">Convenzione di routing predefinita di ASP.NET MVC consiste nel considerare il segmento di un URL dopo il nome del metodo di azione come un parametro denominato "ID".</span><span class="sxs-lookup"><span data-stu-id="79bd3-174">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="79bd3-175">Se il metodo di azione ha un parametro denominato ID quindi ASP.NET MVC automaticamente passerà il segmento URL all'utente come parametro.</span><span class="sxs-lookup"><span data-stu-id="79bd3-175">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="79bd3-176">Eseguire l'applicazione e passare a /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="79bd3-176">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="79bd3-177">Riepilogo si abbiamo finora:</span><span class="sxs-lookup"><span data-stu-id="79bd3-177">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="79bd3-178">Abbiamo creato un nuovo progetto ASP.NET MVC in Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="79bd3-178">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="79bd3-179">È stata illustrata la struttura di cartelle di base di un'applicazione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="79bd3-179">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="79bd3-180">È stato spiegato come eseguire il sito Web utilizzando il Server di sviluppo ASP.NET</span><span class="sxs-lookup"><span data-stu-id="79bd3-180">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="79bd3-181">Abbiamo creato due classi Controller: un HomeController e un StoreController</span><span class="sxs-lookup"><span data-stu-id="79bd3-181">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="79bd3-182">Sono stati aggiunti i metodi di azione per il controller che risponde alle richieste di URL e restituisce testo nel browser</span><span class="sxs-lookup"><span data-stu-id="79bd3-182">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="79bd3-183">[Precedente](mvc-music-store-part-1.md)
> [Successivo](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="79bd3-183">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
