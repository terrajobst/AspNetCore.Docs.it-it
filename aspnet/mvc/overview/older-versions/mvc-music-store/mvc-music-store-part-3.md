---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: Viste e ViewModel | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 3 vengono descritte le visualizzazioni e ViewModel.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 497c2898db2e03b0650982c3ad1e6b5ac5ca639d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="11f75-104">Parte 3: Viste e ViewModel</span><span class="sxs-lookup"><span data-stu-id="11f75-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="11f75-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="11f75-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="11f75-106">L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="11f75-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="11f75-107">L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="11f75-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="11f75-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio.</span><span class="sxs-lookup"><span data-stu-id="11f75-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="11f75-109">Parte 3 vengono descritte le visualizzazioni e ViewModel.</span><span class="sxs-lookup"><span data-stu-id="11f75-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="11f75-110">Finora è stato semplicemente stato restituisce stringhe da azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="11f75-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="11f75-111">Che è un modo utile per avere un'idea del funzionamento del controller, ma è non come si desidera compilare un'applicazione web reale.</span><span class="sxs-lookup"><span data-stu-id="11f75-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="11f75-112">È possibile passare a un metodo migliore per generare HTML al browser che visitano il sito: uno in cui è possibile utilizzare i file di modello per personalizzare più facilmente il contenuto HTML invia nuovamente.</span><span class="sxs-lookup"><span data-stu-id="11f75-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="11f75-113">Questo è esattamente cosa viste.</span><span class="sxs-lookup"><span data-stu-id="11f75-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="11f75-114">Aggiunta di un modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="11f75-114">Adding a View template</span></span>

<span data-ttu-id="11f75-115">Per utilizzare un modello di visualizzazione, è modificherai il metodo HomeController indice per restituire un ActionResult e restituire View(), come di seguito:</span><span class="sxs-lookup"><span data-stu-id="11f75-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="11f75-116">La modifica precedente indica che invece che ha restituito una stringa, invece di voler utilizzare una "visualizzazione" per generare un risultato.</span><span class="sxs-lookup"><span data-stu-id="11f75-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="11f75-117">A questo punto verrà aggiunto un modello di visualizzazione appropriato al progetto.</span><span class="sxs-lookup"><span data-stu-id="11f75-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="11f75-118">A tale scopo si sarà posizionare il cursore di testo all'interno del metodo di azione di indice, quindi fare doppio clic e selezionare "Aggiungi visualizzazione".</span><span class="sxs-lookup"><span data-stu-id="11f75-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="11f75-119">Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="11f75-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="11f75-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="11f75-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="11f75-121">La finestra di dialogo "Aggiungi visualizzazione" consente di visualizzare i file di modello di generare rapidamente e facilmente.</span><span class="sxs-lookup"><span data-stu-id="11f75-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="11f75-122">Per impostazione predefinita "Aggiungi visualizzazione" finestra di dialogo pre-popola il nome del modello di visualizzazione da creare in modo che corrisponda il metodo di azione che verrà utilizzato.</span><span class="sxs-lookup"><span data-stu-id="11f75-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="11f75-123">Poiché è utilizzato il menu di scelta rapida "Aggiungi visualizzazione" all'interno del metodo di azione Index () del nostro HomeController, la finestra di dialogo "Aggiungi visualizzazione" sopra è "Index" come nome della vista pre-popolato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="11f75-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="11f75-124">Non è necessario modificare le opzioni in questa finestra di dialogo, quindi fare clic sul pulsante Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="11f75-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="11f75-125">Quando si fa clic sul pulsante Aggiungi, Visual Web Developer verrà creato un nuovo cshtml modello di visualizzazione per noi nella directory \Views\Home, la creazione della cartella se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="11f75-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="11f75-126">Il percorso di nome e la cartella del file "Cshtml" è importante e segue le convenzioni di denominazione predefinito MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="11f75-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="11f75-127">Il nome della directory, \Views\Home, il controller - denominato HomeController corrispondente.</span><span class="sxs-lookup"><span data-stu-id="11f75-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="11f75-128">Il nome del modello di visualizzazione, indice, corrisponde a metodo di azione del controller che prevede di visualizzare la vista.</span><span class="sxs-lookup"><span data-stu-id="11f75-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="11f75-129">ASP.NET MVC consente di evitare di dover specificare in modo esplicito il nome o il percorso di un modello di visualizzazione quando si usa questa convenzione di denominazione per restituire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="11f75-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="11f75-130">Per impostazione predefinita restituirà il modello di visualizzazione \Views\Home\Index.cshtml quando si scrive codice come riportato di seguito nel nostro HomeController:</span><span class="sxs-lookup"><span data-stu-id="11f75-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="11f75-131">Visual Web Developer creato e aperto il modello di visualizzazione "Cshtml" dopo che è stato fatto clic sul pulsante "Aggiungi" nella finestra di dialogo "Aggiungi visualizzazione".</span><span class="sxs-lookup"><span data-stu-id="11f75-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="11f75-132">Il contenuto di cshtml è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="11f75-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="11f75-133">Questa vista utilizza la sintassi Razor, che è più concisa rispetto al motore di visualizzazione Web Form utilizzato in versioni precedenti di ASP.NET MVC e Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="11f75-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="11f75-134">Il motore di visualizzazione Web Form è ancora disponibile in ASP.NET MVC 3, ma molti sviluppatori trovano che il motore di visualizzazione Razor rientri di sviluppo ASP.NET MVC benissimo.</span><span class="sxs-lookup"><span data-stu-id="11f75-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="11f75-135">Le prime tre righe impostano il titolo della pagina utilizzando ViewBag.</span><span class="sxs-lookup"><span data-stu-id="11f75-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="11f75-136">Viene osservare il funzionamento in modo più dettagliato breve, ma prima si aggiorna il testo di intestazione di testo e visualizzare la pagina.</span><span class="sxs-lookup"><span data-stu-id="11f75-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="11f75-137">Aggiornamento di &lt;h2&gt; tag, ad esempio "Questa è la Home Page", come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="11f75-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="11f75-138">Viene illustrata l'applicazione che il nuovo testo sia visibile nella home page è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="11f75-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="11f75-139">Utilizzo di un Layout per gli elementi comuni del sito</span><span class="sxs-lookup"><span data-stu-id="11f75-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="11f75-140">La maggior parte dei siti Web con contenuto che viene condiviso tra più pagine: navigazione, piè di pagina, immagini di logo, i riferimenti di foglio di stile e così via. Il motore di visualizzazione Razor è semplice da gestire con una pagina denominata \_cshtml che è stato creato automaticamente per noi all'interno della cartella condivisa/visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="11f75-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="11f75-141">Fare doppio clic su questa cartella per visualizzare il contenuto, che vengono mostrate di seguito.</span><span class="sxs-lookup"><span data-stu-id="11f75-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="11f75-142">Verrà visualizzato il contenuto dai nostri singole visualizzazioni per il @RenderBodycomando () e qualsiasi contenuto comune che si desidera venga visualizzato all'esterno che possono essere aggiunti al \_cshtml markup.</span><span class="sxs-lookup"><span data-stu-id="11f75-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="11f75-143">È opportuno negozio musica MVC di un'intestazione comune con i collegamenti per l'area Home page e l'archivio in tutte le pagine del sito, in modo che verrà aggiunto al modello direttamente sopra che @RenderBodyistruzione ().</span><span class="sxs-lookup"><span data-stu-id="11f75-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="11f75-144">L'aggiornamento del foglio di stile</span><span class="sxs-lookup"><span data-stu-id="11f75-144">Updating the StyleSheet</span></span>

<span data-ttu-id="11f75-145">Il modello di progetto vuoto include un file CSS molto semplice che include solo gli stili utilizzati per visualizzare i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="11f75-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="11f75-146">La finestra di progettazione fornisce alcuni CSS e immagini aggiuntive per definire l'aspetto per il sito, in modo che quelli ora verrà aggiunto.</span><span class="sxs-lookup"><span data-stu-id="11f75-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="11f75-147">I file CSS e immagini aggiornati sono inclusi nella directory del contenuto di Assets.zip MvcMusicStore disponibile all'indirizzo [negozio di musica MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="11f75-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="11f75-148">Verrà selezionare entrambi gli elementi in Esplora risorse e trascinarli nella cartella del contenuto della soluzione in Visual Web Developer, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="11f75-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="11f75-149">Verrà chiesto di confermare se si desidera sovrascrivere il file Site.css esistente.</span><span class="sxs-lookup"><span data-stu-id="11f75-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="11f75-150">Fare clic su Sì.</span><span class="sxs-lookup"><span data-stu-id="11f75-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="11f75-151">La cartella del contenuto dell'applicazione verrà visualizzato come segue:</span><span class="sxs-lookup"><span data-stu-id="11f75-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="11f75-152">Ora consente di eseguire l'applicazione e visualizzare le modifiche nella Home page.</span><span class="sxs-lookup"><span data-stu-id="11f75-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="11f75-153">Si esamini cosa è cambiato: metodo di azione del HomeController l'indice individuato visualizzati e il modello \Views\Home\Index.cshtmlView, anche se il codice chiamato "View() restituito", poiché il modello di visualizzazione di seguito la convenzione di denominazione standard.</span><span class="sxs-lookup"><span data-stu-id="11f75-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="11f75-154">Home Page di visualizzazione di un messaggio di benvenuto semplice che è definito all'interno del modello di visualizzazione \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="11f75-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="11f75-155">Utilizza la Home Page il nostro \_cshtml modello, quindi il messaggio di benvenuto è contenuto all'interno del layout standard sito HTML.</span><span class="sxs-lookup"><span data-stu-id="11f75-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="11f75-156">Utilizzo di un modello per passare le informazioni per la visualizzazione</span><span class="sxs-lookup"><span data-stu-id="11f75-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="11f75-157">Per rendere un sito web molto interessanti, non sarà un modello di visualizzazione che visualizza solo hardcoded HTML.</span><span class="sxs-lookup"><span data-stu-id="11f75-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="11f75-158">Per creare un sito web dynamic, è opportuno invece passare le informazioni dalle azioni del controller per i modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="11f75-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="11f75-159">Nel modello Model-View-Controller, il termine A che modello fa riferimento oggetti che rappresentano i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="11f75-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="11f75-160">Spesso gli oggetti del modello corrispondono alle tabelle del database, ma non necessario.</span><span class="sxs-lookup"><span data-stu-id="11f75-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="11f75-161">Metodi di azione del controller che restituiscono un ActionResult possono passare un oggetto modello nella vista.</span><span class="sxs-lookup"><span data-stu-id="11f75-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="11f75-162">In questo modo un Controller per il pacchetto correttamente tutte le informazioni necessarie per generare una risposta e quindi passare tali informazioni a un modello di visualizzazione da utilizzare per generare la risposta HTML appropriata.</span><span class="sxs-lookup"><span data-stu-id="11f75-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="11f75-163">Questo è più facile da comprendere per visualizzarlo in azione, pertanto è possibile iniziare subito.</span><span class="sxs-lookup"><span data-stu-id="11f75-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="11f75-164">Verranno innanzitutto create alcune classi di modello per rappresentare generi e album all'interno di questo archivio.</span><span class="sxs-lookup"><span data-stu-id="11f75-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="11f75-165">Iniziamo creando una classe di genere.</span><span class="sxs-lookup"><span data-stu-id="11f75-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="11f75-166">Fare clic sulla cartella di "Modelli" all'interno del progetto, scegliere l'opzione "Aggiungi classe" e denominare il file "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="11f75-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="11f75-167">Aggiungere quindi una stringa pubblica proprietà nome alla classe che è stata creata:</span><span class="sxs-lookup"><span data-stu-id="11f75-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="11f75-168">*Nota: nel caso dovrai più chiederti, il {get; impostare;} effettua notazione uso della funzionalità di proprietà implementate automaticamente #. Ciò offre i vantaggi di una proprietà senza richiedere di dichiarare un campo sottostante.*</span><span class="sxs-lookup"><span data-stu-id="11f75-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="11f75-169">Successivamente, la stessa procedura per creare una classe Album (denominata Album.cs) che dispone di un titolo e una proprietà di genere:</span><span class="sxs-lookup"><span data-stu-id="11f75-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="11f75-170">Ora che potremo modificare il StoreController per l'utilizzo di viste che consentono di visualizzare informazioni dinamiche dal modello in esame.</span><span class="sxs-lookup"><span data-stu-id="11f75-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="11f75-171">Se - per scopi dimostrativi - ora è denominato nostri album in base all'ID di richiesta, è possibile visualizzare tali informazioni come la visualizzazione seguente.</span><span class="sxs-lookup"><span data-stu-id="11f75-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="11f75-172">Si inizierà modificando l'azione di dettagli archivio in modo che visualizzi le informazioni per un singolo album.</span><span class="sxs-lookup"><span data-stu-id="11f75-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="11f75-173">Aggiungere un'istruzione "using" in cima al **StoreControllers** classe per includere lo spazio dei nomi MvcMusicStore.Models, in modo che non è necessario digitare MvcMusicStore.Models.Album ogni volta che si desidera utilizzare la classe album.</span><span class="sxs-lookup"><span data-stu-id="11f75-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="11f75-174">La sezione "using" di tale classe verranno visualizzati come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="11f75-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="11f75-175">Successivamente, verrà aggiornata l'azione del controller dettagli in modo che restituisca un ActionResult anziché una stringa, come è stato fatto con il metodo di indice del HomeController.</span><span class="sxs-lookup"><span data-stu-id="11f75-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="11f75-176">A questo punto è possibile modificare la logica per restituire un oggetto Album alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="11f75-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="11f75-177">Più avanti in questa esercitazione verranno recuperati i dati da un database, ma per ora verrà utilizzato "fittizio dati" per iniziare.</span><span class="sxs-lookup"><span data-stu-id="11f75-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="11f75-178">*Nota: Se non si ha familiarità con c#, si può presupporre che utilizzando var significa che la variabile album sia con associazione tardiva. Che non è corretto, il compilatore c# è utilizzando l'inferenza del tipo in base a ciò che è in corso assegnazione alla variabile per determinare che tale album è di tipo Album e la compilazione la variabile locale album come un tipo di Album, in modo che i risultati ottenuti controllo in fase di compilazione e l'editor di Visual Studio code supporto.*</span><span class="sxs-lookup"><span data-stu-id="11f75-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="11f75-179">Creare ora un modello di visualizzazione che utilizza il nostro Album per generare una risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="11f75-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="11f75-180">Prima che è necessario compilare il progetto in modo che la finestra di dialogo Aggiungi visualizzazione conosce la classe Album appena creata.</span><span class="sxs-lookup"><span data-stu-id="11f75-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="11f75-181">È possibile compilare il progetto selezionando il Debug⇨Build MvcMusicStore voce di menu (per supplementare, è possibile utilizzare il collegamento Ctrl-MAIUSC + B per compilare il progetto).</span><span class="sxs-lookup"><span data-stu-id="11f75-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="11f75-182">Ora che è stato configurato il nostro classi di supporto, si è pronti a compilare il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="11f75-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="11f75-183">Fare clic all'interno del metodo di dettagli e selezionare "Aggiungi visualizzazione..."</span><span class="sxs-lookup"><span data-stu-id="11f75-183">Right-click within the Details method and select "Add View…"</span></span> <span data-ttu-id="11f75-184">dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="11f75-184">from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="11f75-185">Verrà per creare un nuovo modello di visualizzazione, come abbiamo visto prima con la classe HomeController.</span><span class="sxs-lookup"><span data-stu-id="11f75-185">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="11f75-186">Poiché si sta creando, dal StoreController verrà per impostazione predefinita generato in un file \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="11f75-186">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="11f75-187">A differenza precedente, verrà per controllare la casella di controllo di visualizzazione "Crea oggetto fortemente tipizzato".</span><span class="sxs-lookup"><span data-stu-id="11f75-187">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="11f75-188">Verrà quindi selezionare la classe "Album" all'interno di rilascio-downlist "Classe dati visualizzazione".</span><span class="sxs-lookup"><span data-stu-id="11f75-188">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="11f75-189">In questo modo, la finestra di dialogo "Aggiungi visualizzazione" creare un modello di visualizzazione che prevede che un Album oggetto verrà passato in modo da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="11f75-189">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="11f75-190">Quando si fa clic sul pulsante "Aggiungi" nostro modello di visualizzazione \Views\Store\Details.cshtml verrà creato, contenente il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="11f75-190">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="11f75-191">Si noti la prima riga, che indica che questa vista è fortemente tipizzata per la classe Album.</span><span class="sxs-lookup"><span data-stu-id="11f75-191">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="11f75-192">Il motore di visualizzazione Razor consapevole del fatto che è stato superato un oggetto Album, in modo da poter accedere facilmente le proprietà del modello e disporre anche il vantaggio di IntelliSense nell'editor di Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="11f75-192">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="11f75-193">Aggiornamento di &lt;h2&gt; tag in modo da visualizzare proprietà del titolo dell'Album modificando la riga come segue.</span><span class="sxs-lookup"><span data-stu-id="11f75-193">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="11f75-194">Si noti che IntelliSense viene attivato quando si immette il periodo dopo il @Model (parola chiave), che mostra le proprietà e metodi supportati dalla classe Album.</span><span class="sxs-lookup"><span data-stu-id="11f75-194">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="11f75-195">Verrà ora eseguire nuovamente il progetto e visitare l'URL di archivio/dettagli/5.</span><span class="sxs-lookup"><span data-stu-id="11f75-195">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="11f75-196">Verranno esaminati i dettagli di un Album come di seguito.</span><span class="sxs-lookup"><span data-stu-id="11f75-196">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="11f75-197">Ora ci accerteremo che un aggiornamento analogo per il metodo di azione archivio Sfoglia.</span><span class="sxs-lookup"><span data-stu-id="11f75-197">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="11f75-198">Il metodo di aggiornamento in modo che restituisca un ActionResult e modificare la logica del metodo in modo che crea un nuovo oggetto Genre e viene ripristinata la vista.</span><span class="sxs-lookup"><span data-stu-id="11f75-198">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="11f75-199">Fare clic nel metodo Sfoglia e selezionare "Aggiungi visualizzazione..."</span><span class="sxs-lookup"><span data-stu-id="11f75-199">Right-click in the Browse method and select "Add View…"</span></span> <span data-ttu-id="11f75-200">dal menu di scelta rapida, quindi aggiungere una visualizzazione fortemente tipizzata aggiungere una classe fortemente tipizzata per la classe Genre.</span><span class="sxs-lookup"><span data-stu-id="11f75-200">from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="11f75-201">Aggiornamento di &lt;h2&gt; elemento nella visualizzazione codice (/Views/Store/Browse.cshtml) per visualizzare le informazioni di genere.</span><span class="sxs-lookup"><span data-stu-id="11f75-201">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="11f75-202">Ora consente di eseguire nuovamente il progetto e passare a/archivio/Sfoglia? Genre = URL di individuazione.</span><span class="sxs-lookup"><span data-stu-id="11f75-202">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="11f75-203">Verranno esaminati nella pagina Sfoglia visualizzata nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="11f75-203">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="11f75-204">Infine, questo punto, eseguire un aggiornamento leggermente più complesso per il **archiviare indice** metodo di azione e visualizzazione per visualizzare un elenco di tutti i generi il Negozio.</span><span class="sxs-lookup"><span data-stu-id="11f75-204">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="11f75-205">Che faremo utilizzando un elenco di generi come l'oggetto modello, anziché solo un singolo genere.</span><span class="sxs-lookup"><span data-stu-id="11f75-205">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="11f75-206">Fare clic nel metodo di azione di indice dell'archivio e selezionare Aggiungi visualizzazione, come prima, selezionare il genere come classe di modello e fare clic sul pulsante Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="11f75-206">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="11f75-207">Innanzitutto si modificherà il @model dichiarazione per indicare che la vista prevede Genre diversi oggetti anziché uno solo.</span><span class="sxs-lookup"><span data-stu-id="11f75-207">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="11f75-208">Modifica la prima riga del /Store/Index.cshtml come segue:</span><span class="sxs-lookup"><span data-stu-id="11f75-208">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="11f75-209">In questo modo il motore di visualizzazione Razor che utilizzeranno con un oggetto modello che può contenere più oggetti di genere.</span><span class="sxs-lookup"><span data-stu-id="11f75-209">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="11f75-210">Verrà usato un oggetto IEnumerable&lt;Genre&gt; anziché un elenco&lt;Genre&gt; perché è più generico, che consente di modificare il tipo di modello in un secondo momento per qualsiasi tipo di oggetto che supporta l'interfaccia IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="11f75-210">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="11f75-211">Successivamente, verrà scorrere gli oggetti di genere nel modello, come illustrato nel seguente codice completate.</span><span class="sxs-lookup"><span data-stu-id="11f75-211">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="11f75-212">Si noti che è disponibile supporto IntelliSense completo quando si immette questo codice, in modo che quando si digita "@Model."</span><span class="sxs-lookup"><span data-stu-id="11f75-212">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="11f75-213">Vediamo tutti i metodi e proprietà supportate da un oggetto IEnumerable di tipo genere.</span><span class="sxs-lookup"><span data-stu-id="11f75-213">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="11f75-214">Il ciclo "foreach", all'interno di Visual Web Developer sa che ogni elemento è di tipo genere, pertanto vediamo IntelliSense per ogni tipo di genere.</span><span class="sxs-lookup"><span data-stu-id="11f75-214">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="11f75-215">Successivamente, la funzionalità di scaffolding esaminato l'oggetto Genre e determinata che ogni avrà una proprietà Name, pertanto consente di scorrere e li scrive. Genera inoltre i collegamenti di modifica, dettagli e Delete per ogni singolo elemento.</span><span class="sxs-lookup"><span data-stu-id="11f75-215">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="11f75-216">È possibile sfruttare che più avanti in questo gestore dell'archivio, ma per ora desideriamo dispone invece di un elenco semplice.</span><span class="sxs-lookup"><span data-stu-id="11f75-216">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="11f75-217">Quando si esegue l'applicazione e passare a /Store, che il numero e l'elenco di generi è visualizzato.</span><span class="sxs-lookup"><span data-stu-id="11f75-217">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="11f75-218">Aggiunta di collegamenti tra le pagine</span><span class="sxs-lookup"><span data-stu-id="11f75-218">Adding Links between pages</span></span>

<span data-ttu-id="11f75-219">L'URL /Store che elenca generi attualmente Elenca i nomi di genere semplicemente come testo normale.</span><span class="sxs-lookup"><span data-stu-id="11f75-219">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="11f75-220">È necessario modificare questo, in modo che anziché il testo normale è invece disponibile il collegamento di nomi di genere all'URL di archivio/Sfoglia appropriato, in modo che facendo clic su un genere musica, ad esempio "Disco" si sposterà/archivio/Sfoglia? genre = Disco URL.</span><span class="sxs-lookup"><span data-stu-id="11f75-220">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="11f75-221">È possibile aggiornare il modello di visualizzazione \Views\Store\Index.cshtml all'output di questi collegamenti utilizzando codice simile di sotto **(non di tipo in - si intende migliorare su di esso)**:</span><span class="sxs-lookup"><span data-stu-id="11f75-221">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="11f75-222">Che funziona, ma può causare dei problemi in un secondo momento, poiché si basa su una stringa hardcoded.</span><span class="sxs-lookup"><span data-stu-id="11f75-222">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="11f75-223">Ad esempio, se si desidera rinominare il Controller, è sarebbe necessario cercare il codice alla ricerca di collegamenti che devono essere aggiornati.</span><span class="sxs-lookup"><span data-stu-id="11f75-223">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="11f75-224">Un approccio alternativo, che è possibile utilizzare sia per sfruttare i vantaggi di un metodo HTML Helper.</span><span class="sxs-lookup"><span data-stu-id="11f75-224">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="11f75-225">ASP.NET MVC include i metodi HTML Helper disponibili mediante il codice del modello di visualizzazione per eseguire una serie di attività comuni come segue.</span><span class="sxs-lookup"><span data-stu-id="11f75-225">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="11f75-226">Il metodo di supporto Html.ActionLink() è particolarmente utile e semplice generare HTML &lt;un&gt; collega e si occupa di dettagli fastidiosi come assicurandosi che i percorsi URL siano URL correttamente codificati.</span><span class="sxs-lookup"><span data-stu-id="11f75-226">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="11f75-227">Html.ActionLink() dispone di diversi overload diversi per specificare le informazioni necessarie per i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="11f75-227">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="11f75-228">Nel caso più semplice, si dovranno fornire solo il testo del collegamento e il metodo di azione a cui passare quando si fa clic sul collegamento ipertestuale nel client.</span><span class="sxs-lookup"><span data-stu-id="11f75-228">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="11f75-229">Ad esempio, è possibile collegare "Archivio /" metodo Index () nella pagina dei dettagli di archivio con il testo del collegamento "Vai per l'archivio indice" tramite la chiamata seguente:</span><span class="sxs-lookup"><span data-stu-id="11f75-229">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="11f75-230">*Nota: In questo caso, non occorre specificare il nome del controller in quanto è in corso solo il collegamento a un'altra azione all'interno del controller stesso che esegue il rendering di visualizzazione corrente.*</span><span class="sxs-lookup"><span data-stu-id="11f75-230">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="11f75-231">I collegamenti alla pagina Sfoglia saranno necessario passare un parametro, tuttavia, utilizzeremo un altro overload del metodo HTML. ActionLink che accetta tre parametri:</span><span class="sxs-lookup"><span data-stu-id="11f75-231">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="11f75-232">Testo del collegamento, che verrà visualizzato il nome di genere</span><span class="sxs-lookup"><span data-stu-id="11f75-232">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="11f75-233">Nome dell'azione controller (Sfoglia)</span><span class="sxs-lookup"><span data-stu-id="11f75-233">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="11f75-234">Valori dei parametri di route, che specifica il nome (Genre) e il valore (nome genere)</span><span class="sxs-lookup"><span data-stu-id="11f75-234">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="11f75-235">Inserimento che tutti insieme, ecco come è necessario scrivere tali collegamenti per la visualizzazione dell'indice di archivio:</span><span class="sxs-lookup"><span data-stu-id="11f75-235">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="11f75-236">Eseguire nuovamente il progetto e accedere all'URL /Store/ è verrà visualizzato un elenco di generi.</span><span class="sxs-lookup"><span data-stu-id="11f75-236">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="11f75-237">Ogni genere è un collegamento ipertestuale: quando si fa clic potrebbero essere necessari per l'archivio/Sfoglia? genre =*[genre]* URL.</span><span class="sxs-lookup"><span data-stu-id="11f75-237">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="11f75-238">Il codice HTML per l'elenco di genere è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="11f75-238">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="11f75-239">[Precedente](mvc-music-store-part-2.md)
> [Successivo](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="11f75-239">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
