---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controller | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 2 illustra i controller.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841095"
---
<a name="part-2-controllers"></a><span data-ttu-id="b20ee-104">Parte 2: controller</span><span class="sxs-lookup"><span data-stu-id="b20ee-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="b20ee-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="b20ee-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="b20ee-106">Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="b20ee-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="b20ee-107">La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="b20ee-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="b20ee-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="b20ee-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="b20ee-109">Parte 2 illustra i controller.</span><span class="sxs-lookup"><span data-stu-id="b20ee-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="b20ee-110">Framework web tradizionali, gli URL in ingresso vengono in genere mappati ai file su disco.</span><span class="sxs-lookup"><span data-stu-id="b20ee-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="b20ee-111">Ad esempio: una richiesta per un URL, ad esempio "/ Products. aspx" o "/ Products" potrebbe essere elaborata da un file "Products" o "Products".</span><span class="sxs-lookup"><span data-stu-id="b20ee-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="b20ee-112">Framework MVC basati su Web eseguire il mapping degli URL al codice lato server in modo leggermente diverso.</span><span class="sxs-lookup"><span data-stu-id="b20ee-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="b20ee-113">Anziché eseguire il mapping degli URL in ingresso ai file, ma eseguono il mapping degli URL ai metodi nelle classi.</span><span class="sxs-lookup"><span data-stu-id="b20ee-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="b20ee-114">Queste classi vengono chiamate "Controller" e sono responsabili per l'elaborazione delle richieste HTTP in ingresso, gestione dell'input dell'utente, eseguire il recupero e salvataggio dei dati e determinare la risposta da inviare al client (visualizzazione del contenuto HTML, scaricare un file, il reindirizzamento a un altro URL e così via).</span><span class="sxs-lookup"><span data-stu-id="b20ee-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="b20ee-115">Aggiunta di una classe HomeController</span><span class="sxs-lookup"><span data-stu-id="b20ee-115">Adding a HomeController</span></span>

<span data-ttu-id="b20ee-116">Inizieremo la nostra applicazione MVC Music Store mediante l'aggiunta di una classe Controller che gestirà l'URL alla Home page del sito.</span><span class="sxs-lookup"><span data-stu-id="b20ee-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="b20ee-117">Illustreremo seguono le convenzioni di denominazione predefinito di MVC ASP.NET e chiamarla HomeController.</span><span class="sxs-lookup"><span data-stu-id="b20ee-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="b20ee-118">Fare doppio clic nella cartella "Controller" all'interno di Esplora soluzioni e selezionare "Aggiungi", quindi il comando "Controller in corso":</span><span class="sxs-lookup"><span data-stu-id="b20ee-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="b20ee-119">Verrà visualizzata la finestra di dialogo "Aggiungi Controller".</span><span class="sxs-lookup"><span data-stu-id="b20ee-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="b20ee-120">Denominare il controller "HomeController" e fare clic su Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="b20ee-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="b20ee-121">Si creerà un nuovo file HomeController.cs, con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b20ee-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="b20ee-122">Per avviare semplicemente possibile, è possibile sostituire il metodo di indice con un metodo semplice che restituisce solo una stringa.</span><span class="sxs-lookup"><span data-stu-id="b20ee-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="b20ee-123">Saranno apportate due modifiche:</span><span class="sxs-lookup"><span data-stu-id="b20ee-123">We'll make two changes:</span></span>

- <span data-ttu-id="b20ee-124">Modificare il metodo per restituire una stringa anziché un ActionResult</span><span class="sxs-lookup"><span data-stu-id="b20ee-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="b20ee-125">Modificare l'istruzione return per restituire "Hello da casa"</span><span class="sxs-lookup"><span data-stu-id="b20ee-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="b20ee-126">Il metodo dovrebbe ora essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b20ee-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="b20ee-127">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="b20ee-127">Running the Application</span></span>

<span data-ttu-id="b20ee-128">A questo punto è possibile eseguire il sito.</span><span class="sxs-lookup"><span data-stu-id="b20ee-128">Now let's run the site.</span></span> <span data-ttu-id="b20ee-129">È possibile avviare il server web e provare a utilizzare il sito usando quanto segue:</span><span class="sxs-lookup"><span data-stu-id="b20ee-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="b20ee-130">Scegliere la voce di menu Avvia debug ⇨ di Debug</span><span class="sxs-lookup"><span data-stu-id="b20ee-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="b20ee-131">Fare clic sul pulsante freccia verde sulla barra degli strumenti ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="b20ee-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="b20ee-132">Usare i tasti di scelta rapida F5.</span><span class="sxs-lookup"><span data-stu-id="b20ee-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="b20ee-133">Usando uno dei passaggi precedenti verrà compilare il progetto e quindi causare il Server di sviluppo ASP.NET che viene incorporato in Visual Web Developer per iniziare.</span><span class="sxs-lookup"><span data-stu-id="b20ee-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="b20ee-134">Una notifica verrà visualizzata nell'angolo inferiore della schermata per indicare che ha avviato il Server di sviluppo ASP.NET e verrà indicato il numero di porta che viene eseguito sotto.</span><span class="sxs-lookup"><span data-stu-id="b20ee-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="b20ee-135">Visual Web Developer quindi aprirà automaticamente una finestra del browser con URL che punta al nostro server web.</span><span class="sxs-lookup"><span data-stu-id="b20ee-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="b20ee-136">Questo ci consentirà di provare rapidamente l'applicazione web:</span><span class="sxs-lookup"><span data-stu-id="b20ee-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="b20ee-137">Bene, questa è stata abbastanza rapida: è stato creato un nuovo sito Web, aggiunta una funzione tre inline e abbiamo testo in un browser.</span><span class="sxs-lookup"><span data-stu-id="b20ee-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="b20ee-138">Non stare scienza, ma è un inizio.</span><span class="sxs-lookup"><span data-stu-id="b20ee-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="b20ee-139">*Nota: Visual Web Developer include il Server di sviluppo ASP.NET, che verrà eseguito il sito Web su un numero casuale gratuito "port". Nella schermata precedente, il sito viene eseguito in `http://localhost:26641/`, pertanto utilizza porta 26641. Il numero di porta sarà diverso. Quando si parla /Store/Browse like dell'URL in questa esercitazione, che risulteranno dopo il numero di porta. Supponendo che un numero di porta di 26641, passare a/Store/Sfoglia implica esplorando `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="b20ee-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="b20ee-140">Aggiunta di un StoreController</span><span class="sxs-lookup"><span data-stu-id="b20ee-140">Adding a StoreController</span></span>

<span data-ttu-id="b20ee-141">È stato aggiunto un oggetto semplice che implementa la Home Page del sito.</span><span class="sxs-lookup"><span data-stu-id="b20ee-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="b20ee-142">Questo punto, aggiungere un altro controller che verranno utilizzate per implementare la funzionalità di esplorazione dell'archivio file musicali.</span><span class="sxs-lookup"><span data-stu-id="b20ee-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="b20ee-143">Il controller di archiviazione offrirà supporto per tre scenari:</span><span class="sxs-lookup"><span data-stu-id="b20ee-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="b20ee-144">Una pagina di presentazione di generi musica nel nostro store musica</span><span class="sxs-lookup"><span data-stu-id="b20ee-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="b20ee-145">Una pagina di esplorazione che elenca tutti i album musicali in genere particolare</span><span class="sxs-lookup"><span data-stu-id="b20ee-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="b20ee-146">Una pagina che contiene informazioni su un album musicali specifici</span><span class="sxs-lookup"><span data-stu-id="b20ee-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="b20ee-147">Si inizierà aggiungendo una nuova classe StoreController..</span><span class="sxs-lookup"><span data-stu-id="b20ee-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="b20ee-148">Se hai già fatto, interrompere l'esecuzione dell'applicazione la chiusura del browser oppure selezionando la voce di menu arresta debug ⇨ di Debug.</span><span class="sxs-lookup"><span data-stu-id="b20ee-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="b20ee-149">A questo punto aggiungere un nuovo StoreController.</span><span class="sxs-lookup"><span data-stu-id="b20ee-149">Now add a new StoreController.</span></span> <span data-ttu-id="b20ee-150">Come abbiamo fatto con HomeController, faremo questo facendo clic sulla cartella "Controller" all'interno di Esplora soluzioni e scegliendo Aggiungi -&gt;voce di menu Controller</span><span class="sxs-lookup"><span data-stu-id="b20ee-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="b20ee-151">Nostro nuovo StoreController dispone già di un metodo "Index".</span><span class="sxs-lookup"><span data-stu-id="b20ee-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="b20ee-152">Si userà questo metodo "Index" per implementare la pagina di presentazione che elenca tutti i generi nel nostro store musica.</span><span class="sxs-lookup"><span data-stu-id="b20ee-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="b20ee-153">Viene anche aggiunto altri due metodi per implementare i due altri scenari vogliamo nostro StoreController gestire: esplorazione e i dettagli.</span><span class="sxs-lookup"><span data-stu-id="b20ee-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="b20ee-154">Questi metodi (Index, Sfoglia e Details) nel Controller vengono chiamati "Azioni del Controller" e come abbiamo già visto con il metodo di azione HomeController.Index (), il loro lavoro consiste nel rispondere alle richieste di URL e (in genere) determinare il tipo di contenuto devono essere inviati nuovamente al browser o all'utente che ha richiamato l'URL.</span><span class="sxs-lookup"><span data-stu-id="b20ee-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="b20ee-155">Si inizierà la nostra implementazione StoreController modificando theIndex() metodo per restituire la stringa "Hello da Store.Index()" e aggiungeremo metodi simili per Browse e Details():</span><span class="sxs-lookup"><span data-stu-id="b20ee-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="b20ee-156">Eseguire nuovamente il progetto e passare gli URL seguenti:</span><span class="sxs-lookup"><span data-stu-id="b20ee-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="b20ee-157">/ Store</span><span class="sxs-lookup"><span data-stu-id="b20ee-157">/Store</span></span>
- <span data-ttu-id="b20ee-158">/ Store/esplorazione</span><span class="sxs-lookup"><span data-stu-id="b20ee-158">/Store/Browse</span></span>
- <span data-ttu-id="b20ee-159">/ Store/dettagli</span><span class="sxs-lookup"><span data-stu-id="b20ee-159">/Store/Details</span></span>

<span data-ttu-id="b20ee-160">L'accesso a questi URL verrà richiamare i metodi di azione all'interno di Controller e restituire le risposte di stringa:</span><span class="sxs-lookup"><span data-stu-id="b20ee-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="b20ee-161">Bene, ma si tratta di stringhe solo costanti.</span><span class="sxs-lookup"><span data-stu-id="b20ee-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="b20ee-162">È possibile renderli dinamico, in modo accettare informazioni dall'URL e li visualizzi nell'output della pagina.</span><span class="sxs-lookup"><span data-stu-id="b20ee-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="b20ee-163">Prima di tutto si modificherà il metodo di azione di esplorazione per recuperare un valore di stringa di query dall'URL.</span><span class="sxs-lookup"><span data-stu-id="b20ee-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="b20ee-164">È possibile farlo mediante l'aggiunta di un parametro "genre" per il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="b20ee-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="b20ee-165">Quando si esegue questa operazione, ASP.NET MVC passerà automaticamente i parametri post qualsiasi stringa di query o un modulo denominati "genre" per il metodo di azione quando viene richiamato.</span><span class="sxs-lookup"><span data-stu-id="b20ee-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="b20ee-166">*Nota: Viene usato il metodo di utilità HttpUtility per purificare l'input dell'utente. In questo modo si impedisce agli utenti di inserimento di Javascript in visualizzazione con un collegamento, ad esempio /Store/Browse? Genre =&lt;script&gt;Window. Location = 'http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="b20ee-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="b20ee-167">A questo punto è possibile passare a/Store/Sfoglia? Genre = Disco</span><span class="sxs-lookup"><span data-stu-id="b20ee-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="b20ee-168">Modifichiamo quindi l'azione Details per leggere e visualizzare un parametro di input denominato ID.</span><span class="sxs-lookup"><span data-stu-id="b20ee-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="b20ee-169">A differenza dei nostri metodo precedente, abbiamo non incorporamento il valore ID come parametro di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="b20ee-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="b20ee-170">È invece sarà incorporarlo direttamente all'interno dell'URL stesso.</span><span class="sxs-lookup"><span data-stu-id="b20ee-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="b20ee-171">Ad esempio: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="b20ee-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="b20ee-172">ASP.NET MVC consente di eseguire facilmente questa operazione senza la necessità di alcuna configurazione.</span><span class="sxs-lookup"><span data-stu-id="b20ee-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="b20ee-173">Convenzione di routing predefinito di ASP.NET MVC consiste nel considerare il segmento di URL dopo il nome del metodo di azione come un parametro denominato "ID".</span><span class="sxs-lookup"><span data-stu-id="b20ee-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="b20ee-174">Se il metodo di azione ha un parametro denominato ID quindi ASP.NET MVC automaticamente passerà il segmento URL all'utente come parametro.</span><span class="sxs-lookup"><span data-stu-id="b20ee-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="b20ee-175">Eseguire l'applicazione e passare a /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="b20ee-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="b20ee-176">È utile esaminare quanto è stato fatto finora:</span><span class="sxs-lookup"><span data-stu-id="b20ee-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="b20ee-177">Abbiamo creato un nuovo progetto ASP.NET MVC in Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="b20ee-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="b20ee-178">È stata illustrata la struttura di cartelle di base di un'applicazione ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b20ee-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="b20ee-179">È stato appreso come eseguire il nostro sito Web utilizzando ASP.NET Development Server</span><span class="sxs-lookup"><span data-stu-id="b20ee-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="b20ee-180">Abbiamo creato due classi Controller: una classe HomeController e un StoreController</span><span class="sxs-lookup"><span data-stu-id="b20ee-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="b20ee-181">Sono stati aggiunti i metodi di azione per il controller che rispondono alle richieste di URL e restituiscono testo al browser</span><span class="sxs-lookup"><span data-stu-id="b20ee-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="b20ee-182">[Precedente](mvc-music-store-part-1.md)
> [Successivo](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="b20ee-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
