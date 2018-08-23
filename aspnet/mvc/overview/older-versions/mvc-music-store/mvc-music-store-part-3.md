---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: Visualizzazioni e ViewModel | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 3 riguarda le visualizzazioni e ViewModel.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 828ff18abcc5932f82be71a45ebde589eeb051fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832291"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="d7db9-104">Parte 3: Visualizzazioni e ViewModel</span><span class="sxs-lookup"><span data-stu-id="d7db9-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="d7db9-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="d7db9-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="d7db9-106">Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="d7db9-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="d7db9-107">La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="d7db9-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="d7db9-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="d7db9-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="d7db9-109">Parte 3 riguarda le visualizzazioni e ViewModel.</span><span class="sxs-lookup"><span data-stu-id="d7db9-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="d7db9-110">Finora è stato semplicemente stato restituisce stringhe dalle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="d7db9-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="d7db9-111">Che è una buona soluzione per avere un'idea di come funzionano i controller, ma è non modo in cui è preferibile creare un'applicazione web reale.</span><span class="sxs-lookup"><span data-stu-id="d7db9-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="d7db9-112">Si intende trovare un modo migliore per generare codice HTML al browser che visitano il sito: uno in cui è possibile usare i file di modello per personalizzare più facilmente il contenuto HTML invia nuovamente.</span><span class="sxs-lookup"><span data-stu-id="d7db9-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="d7db9-113">Questo è esattamente cosa viste.</span><span class="sxs-lookup"><span data-stu-id="d7db9-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="d7db9-114">Aggiunta di un modello di vista</span><span class="sxs-lookup"><span data-stu-id="d7db9-114">Adding a View template</span></span>

<span data-ttu-id="d7db9-115">Per usare un modello di vista, si sarà modificare il metodo indice HomeController per restituire un ActionResult e restituisse View(), come di seguito:</span><span class="sxs-lookup"><span data-stu-id="d7db9-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="d7db9-116">La modifica precedente indica che invece di restituire una stringa, ma invece di utilizzare una "visualizzazione" per generare un risultato.</span><span class="sxs-lookup"><span data-stu-id="d7db9-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="d7db9-117">A questo punto si aggiungerà un modello di visualizzazione appropriato per il progetto.</span><span class="sxs-lookup"><span data-stu-id="d7db9-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="d7db9-118">A tale scopo è sarà posizionare il cursore del testo all'interno del metodo di azione Index, quindi fare doppio clic e selezionare "Aggiungi visualizzazione".</span><span class="sxs-lookup"><span data-stu-id="d7db9-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="d7db9-119">Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="d7db9-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="d7db9-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d7db9-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="d7db9-121">La finestra di dialogo "Aggiungi visualizzazione" consente di generare rapidamente e facilmente i file di modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d7db9-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="d7db9-122">Per impostazione predefinita "Aggiungi visualizzazione" finestra di dialogo popola preventivamente il nome del modello di visualizzazione da creare in modo che corrisponda al metodo di azione che verrà usato.</span><span class="sxs-lookup"><span data-stu-id="d7db9-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="d7db9-123">Poiché è stato usato il menu di scelta rapida "Aggiungi visualizzazione" all'interno del metodo di azione Index () del nostro HomeController, finestra di dialogo "Aggiungi visualizzazione" precedente è "Index" come nome della vista prepopolato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="d7db9-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="d7db9-124">Non è necessario modificare le opzioni in questa finestra di dialogo, fare clic sul pulsante Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="d7db9-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="d7db9-125">Quando si fa clic sul pulsante Aggiungi, Visual Web Developer creerà un index. cshtml nuovo modello di vista automaticamente nella directory \Views\Home, creando la cartella se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="d7db9-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="d7db9-126">Il nome e cartella la posizione del file "Index. cshtml" è importante e segue le convenzioni di denominazione di ASP.NET MVC predefinita.</span><span class="sxs-lookup"><span data-stu-id="d7db9-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="d7db9-127">Il nome della directory, \Views\Home, corrisponde al controller - denominata HomeController.</span><span class="sxs-lookup"><span data-stu-id="d7db9-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="d7db9-128">Il nome del modello di visualizzazione, indice, corrisponde al metodo di azione del controller che prevede di visualizzare la vista.</span><span class="sxs-lookup"><span data-stu-id="d7db9-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="d7db9-129">ASP.NET MVC consente di evitare di dover specificare esplicitamente il nome o il percorso di un modello di visualizzazione quando si usa questa convenzione di denominazione per restituire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d7db9-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="d7db9-130">Eseguirà per impostazione predefinita il rendering il modello di vista \Views\Home\Index.cshtml quando si scrive codice simile al seguente nel nostro HomeController:</span><span class="sxs-lookup"><span data-stu-id="d7db9-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="d7db9-131">Visual Web Developer creato e aperto il modello di vista "Index. cshtml" dopo che è stato fatto clic sul pulsante "Aggiungi" nella finestra di dialogo "Aggiungi visualizzazione".</span><span class="sxs-lookup"><span data-stu-id="d7db9-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="d7db9-132">Il contenuto di index. cshtml è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d7db9-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="d7db9-133">In questa vista è tramite la sintassi Razor, che è più concisa rispetto al motore di visualizzazione Web Form utilizzato in Web Form ASP.NET e le versioni precedenti di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d7db9-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="d7db9-134">Il motore di visualizzazione Web Form è ancora disponibile in ASP.NET MVC 3, ma molti sviluppatori trovano che il motore di visualizzazione Razor sia adeguato particolarmente bene lo sviluppo di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d7db9-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="d7db9-135">Le prime tre righe impostano il titolo della pagina usando ViewBag.</span><span class="sxs-lookup"><span data-stu-id="d7db9-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="d7db9-136">Verrà osservare il funzionamento in modo più dettagliato a breve, ma prima di tutto è possibile aggiornare il testo di intestazione di testo e visualizzare la pagina.</span><span class="sxs-lookup"><span data-stu-id="d7db9-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="d7db9-137">Aggiorna il &lt;h2&gt; tag scrivo "Questa è la Home Page", come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d7db9-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="d7db9-138">È in esecuzione nell'applicazione viene illustrato che il nuovo testo sia visibile nella home page.</span><span class="sxs-lookup"><span data-stu-id="d7db9-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="d7db9-139">Utilizzo di un Layout per gli elementi comuni del sito</span><span class="sxs-lookup"><span data-stu-id="d7db9-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="d7db9-140">La maggior parte dei siti Web con contenuto che verrà condivise tra molte pagine: esplorazione, piè di pagina, immagini del logo, i riferimenti di foglio di stile e così via. Il motore di visualizzazione Razor semplifica questa attività gestire utilizzando una pagina denominata \_layout. cshtml che è stato creato automaticamente per noi all'interno della cartella condivisa/viste /.</span><span class="sxs-lookup"><span data-stu-id="d7db9-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="d7db9-141">Fare doppio clic su questa cartella per visualizzare il contenuto, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d7db9-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="d7db9-142">Verrà visualizzato il contenuto dai nostri singole visualizzazioni per il @RenderBodycomando () e qualsiasi contenuto comune che si desidera visualizzare all'esterno che possono essere aggiunti al \_markup di layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="d7db9-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="d7db9-143">Sarebbe auspicabile nostro Store musica MVC per avere un'intestazione comune con collegamenti per l'area di Store e la Home page in tutte le pagine del sito, in modo che verrà aggiunto al modello direttamente sopra che @RenderBodyistruzione ().</span><span class="sxs-lookup"><span data-stu-id="d7db9-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="d7db9-144">L'aggiornamento del foglio di stile</span><span class="sxs-lookup"><span data-stu-id="d7db9-144">Updating the StyleSheet</span></span>

<span data-ttu-id="d7db9-145">Il modello di progetto vuoto include un file CSS molto semplice che include solo gli stili utilizzati per visualizzare i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="d7db9-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="d7db9-146">La finestra di progettazione ha fornito alcuni CSS e immagini aggiuntive per definire l'aspetto per il sito, quindi si aggiungerà quelle nell'ora.</span><span class="sxs-lookup"><span data-stu-id="d7db9-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="d7db9-147">Il file CSS e immagini aggiornati sono inclusi nella directory del contenuto di Assets.zip MvcMusicStore disponibile all'indirizzo [MVC-musica-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="d7db9-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="d7db9-148">Abbiamo selezionare entrambi i sistemi operativi in Windows Explorer e trascinarli nella cartella del contenuto della nostra soluzione in Visual Web Developer, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="d7db9-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="d7db9-149">Verrà chiesto di confermare se si desidera sovrascrivere il file CSS esistente.</span><span class="sxs-lookup"><span data-stu-id="d7db9-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="d7db9-150">Fare clic su Sì.</span><span class="sxs-lookup"><span data-stu-id="d7db9-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="d7db9-151">La cartella del contenuto dell'applicazione verrà ora visualizzato come segue:</span><span class="sxs-lookup"><span data-stu-id="d7db9-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="d7db9-152">A questo punto è possibile eseguire l'applicazione e visualizzare l'aspetto di tali modifiche nella Home page.</span><span class="sxs-lookup"><span data-stu-id="d7db9-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="d7db9-153">È opportuno esaminare che cosa è cambiato: metodo di azione Index del HomeController The trovato e visualizzato il modello \Views\Home\Index.cshtmlView, anche se il codice chiamato "View() restituito", perché il modello di visualizzazione seguita la convenzione di denominazione standard.</span><span class="sxs-lookup"><span data-stu-id="d7db9-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="d7db9-154">Home Page Visualizza un messaggio di benvenuto semplice che è definito all'interno del modello di visualizzazione \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="d7db9-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="d7db9-155">Usa la Home Page nostro \_modello di layout. cshtml, e in modo che il messaggio di benvenuto è contenuto all'interno del layout del sito standard HTML.</span><span class="sxs-lookup"><span data-stu-id="d7db9-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="d7db9-156">Utilizzo di un modello per passare le informazioni di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="d7db9-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="d7db9-157">Un modello di visualizzazione che visualizza solo HTML hardcoded non dovrà creare un sito web molto interessante.</span><span class="sxs-lookup"><span data-stu-id="d7db9-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="d7db9-158">Per creare un sito web dinamico, è opportuno invece passare le informazioni dalle azioni del controller per i modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d7db9-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="d7db9-159">Nel modello Model-View-Controller, il termine A che modello fa riferimento oggetti che rappresentano i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d7db9-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="d7db9-160">Spesso, gli oggetti del modello corrispondono alle tabelle del database, ma sono privi di.</span><span class="sxs-lookup"><span data-stu-id="d7db9-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="d7db9-161">Metodi di azione del controller che restituiscono un ActionResult possono passare un oggetto del modello alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d7db9-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="d7db9-162">In questo modo un Controller per il pacchetto correttamente tutte le informazioni necessarie per generare una risposta e quindi passare tali informazioni a un modello di vista da utilizzare per generare la risposta appropriata in HTML.</span><span class="sxs-lookup"><span data-stu-id="d7db9-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="d7db9-163">Questo è più facile da comprendere per vederla in azione, bene, cominciamo.</span><span class="sxs-lookup"><span data-stu-id="d7db9-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="d7db9-164">Prima di tutto si creerà alcune classi di modello per rappresentare gli album e generi entro lo store.</span><span class="sxs-lookup"><span data-stu-id="d7db9-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="d7db9-165">Iniziamo creando una classe al genere.</span><span class="sxs-lookup"><span data-stu-id="d7db9-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="d7db9-166">Fare doppio clic su cartella "Models" all'interno del progetto, scegliere l'opzione "Aggiungi classe" e denominare il file "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="d7db9-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="d7db9-167">Quindi aggiungere una proprietà nome di stringa pubblica alla classe che è stata creata:</span><span class="sxs-lookup"><span data-stu-id="d7db9-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="d7db9-168">*Nota: nel caso ve lo chiediate, il {get; set;} effettua notazione uso della funzionalità di proprietà implementate automaticamente di #. Questo ci offre i vantaggi di una proprietà senza la necessità di dichiarare un campo sottostante.*</span><span class="sxs-lookup"><span data-stu-id="d7db9-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="d7db9-169">Successivamente, seguire gli stessi passaggi per creare una classe di Album (denominata Album.cs) che presenta un titolo e una proprietà di genere:</span><span class="sxs-lookup"><span data-stu-id="d7db9-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="d7db9-170">A questo punto è possibile modificare il StoreController per utilizzare le viste che mostrano informazioni dinamiche dal modello.</span><span class="sxs-lookup"><span data-stu-id="d7db9-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="d7db9-171">Se - a scopo dimostrativo subito - denominato nostro album in base all'ID richiesta, si sono visualizzare tali informazioni come la visualizzazione seguente.</span><span class="sxs-lookup"><span data-stu-id="d7db9-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="d7db9-172">Si inizierà modificando l'azione Details di Store in modo che visualizzi le informazioni per un singolo album.</span><span class="sxs-lookup"><span data-stu-id="d7db9-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="d7db9-173">Aggiungere un'istruzione "using" in cima il **StoreControllers** classe per includere lo spazio dei nomi MvcMusicStore.Models, in modo che non è necessario digitare MvcMusicStore.Models.Album ogni volta che si vuole usare la classe di album.</span><span class="sxs-lookup"><span data-stu-id="d7db9-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="d7db9-174">La sezione "using" di tale classe dovrebbe ora essere visualizzato come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d7db9-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="d7db9-175">Successivamente, verrà aggiornata l'azione del controller Details in modo che restituisca un ActionResult anziché una stringa, come è stato fatto con il metodo di indice di HomeController.</span><span class="sxs-lookup"><span data-stu-id="d7db9-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="d7db9-176">A questo punto è possibile modificare la logica per restituire un oggetto di Album alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d7db9-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="d7db9-177">Più avanti in questa esercitazione verranno recuperati i dati da un database, ma per ora si userà "fittizio di dati" per iniziare.</span><span class="sxs-lookup"><span data-stu-id="d7db9-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="d7db9-178">*Nota: Se non si ha familiarità con c#, si potrebbe supporre che utilizzo di var significa che la variabile di album sia con associazione tardiva. Non è corretto: il compilatore c# usa l'inferenza del tipo basata su ciò che abbiamo stiamo assegnazione alla variabile per determinare che tale album JE typu Album e la compilazione alla variabile locale album come un tipo di Album così ottenere controllo in fase di compilazione e l'editor di Visual Studio code supporto tecnico.*</span><span class="sxs-lookup"><span data-stu-id="d7db9-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="d7db9-179">Creare ora un modello di vista che usa i nostri Album per generare una risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="d7db9-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="d7db9-180">Prima di procedere è necessario compilare il progetto in modo che la finestra di dialogo Aggiungi visualizzazione sappia sulla nostra classe Album appena creata.</span><span class="sxs-lookup"><span data-stu-id="d7db9-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="d7db9-181">È possibile compilare il progetto selezionando il Debug⇨Build MvcMusicStore voce di menu (per supplementare, è possibile usare lo scelta rapida Ctrl + MAIUSC + B per compilare il progetto).</span><span class="sxs-lookup"><span data-stu-id="d7db9-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="d7db9-182">Ora che è stato configurato le classi di supporta, siamo pronti a compilare il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d7db9-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="d7db9-183">Fare doppio clic all'interno del metodo di dettagli e scegliere "Aggiungi visualizzazione" dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="d7db9-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="d7db9-184">Si intende creare un nuovo modello di visualizzazione come fatto in precedenza con la classe HomeController.</span><span class="sxs-lookup"><span data-stu-id="d7db9-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="d7db9-185">Poiché stiamo creando si dal StoreController verrà per impostazione predefinita generato in un file \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="d7db9-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="d7db9-186">A differenza di prima, si intende controllare la casella di controllo di visualizzazione "Crea oggetto fortemente tipizzato".</span><span class="sxs-lookup"><span data-stu-id="d7db9-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="d7db9-187">Verrà quindi selezionare la classe "Album" all'interno di "Data-classe di visualizzazione" drop-downlist.</span><span class="sxs-lookup"><span data-stu-id="d7db9-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="d7db9-188">In questo modo la finestra di dialogo "Aggiungi visualizzazione" creare un modello di visualizzazione che prevede che un Album oggetto verrà passato in modo da usare.</span><span class="sxs-lookup"><span data-stu-id="d7db9-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="d7db9-189">Quando si fa clic sul pulsante "Aggiungi" nostro modello di vista \Views\Store\Details.cshtml verrà creato, che contiene il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="d7db9-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="d7db9-190">Si noti che la prima riga, che indica che questa vista è fortemente tipizzata per la classe di Album.</span><span class="sxs-lookup"><span data-stu-id="d7db9-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="d7db9-191">Il motore di visualizzazione Razor riconosce che si è stato passato un oggetto di Album, poter accedere facilmente alle proprietà dei modelli e ha anche il vantaggio di IntelliSense nell'editor di Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="d7db9-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="d7db9-192">Aggiorna il &lt;h2&gt; contrassegnare in modo che visualizzi proprietà titolo dell'Album modificando tale riga deve essere visualizzato come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d7db9-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="d7db9-193">Si noti che IntelliSense viene attivata quando si immette il periodo dopo il @Model (parola chiave), che mostra le proprietà e metodi che supporta la classe di Album.</span><span class="sxs-lookup"><span data-stu-id="d7db9-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="d7db9-194">È possibile ora eseguire nuovamente il progetto e visitare l'URL di Store/dettagli/5.</span><span class="sxs-lookup"><span data-stu-id="d7db9-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="d7db9-195">Vedremo i dettagli di un Album come di seguito.</span><span class="sxs-lookup"><span data-stu-id="d7db9-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="d7db9-196">A questo punto ci assicureremo che un aggiornamento analogo per il metodo di azione Store Sfoglia.</span><span class="sxs-lookup"><span data-stu-id="d7db9-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="d7db9-197">Aggiornare il metodo in modo che restituisca un ActionResult e modificare la logica del metodo in modo che lo crea un nuovo oggetto Genre e lo restituisce alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d7db9-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="d7db9-198">Fare doppio clic nel metodo Sfoglia e selezionare "Aggiungi in corso di visualizzazione" dal menu di scelta rapida, quindi aggiungere una visualizzazione fortemente tipizzata aggiungere una classe fortemente tipizzata per la classe Genre.</span><span class="sxs-lookup"><span data-stu-id="d7db9-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="d7db9-199">Aggiorna il &lt;h2&gt; elemento nella visualizzazione del codice (in /Views/Store/Browse.cshtml) per visualizzare le informazioni al genere.</span><span class="sxs-lookup"><span data-stu-id="d7db9-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="d7db9-200">A questo punto è possibile eseguire nuovamente il progetto e passare a/Store/Sfoglia? Genre = URL del Disco.</span><span class="sxs-lookup"><span data-stu-id="d7db9-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="d7db9-201">Si vedrà nella pagina Sfoglia visualizzata nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="d7db9-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="d7db9-202">Infine, creiamo un aggiornamento leggermente più complesso per la **Store indice** metodo di azione e visualizzazione per visualizzare un elenco di tutti i generi nel nostro store.</span><span class="sxs-lookup"><span data-stu-id="d7db9-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="d7db9-203">Che faremo usando un elenco di generi come l'oggetto modello, anziché semplicemente un genere singolo.</span><span class="sxs-lookup"><span data-stu-id="d7db9-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="d7db9-204">Nel metodo di azione Index Store e scegliere Aggiungi visualizzazione come prima, selezionare il genere come classe di modello e fare clic sul pulsante Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="d7db9-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="d7db9-205">Prima di tutto si modificherà il @model dichiarazione per indicare che la vista si aspettano Genre diversi oggetti anziché uno solo.</span><span class="sxs-lookup"><span data-stu-id="d7db9-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="d7db9-206">Modificare la prima riga del /Store/Index.cshtml come segue:</span><span class="sxs-lookup"><span data-stu-id="d7db9-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="d7db9-207">Ciò indica al motore di visualizzazione Razor che verranno usate con un oggetto modello che può contenere più oggetti al genere.</span><span class="sxs-lookup"><span data-stu-id="d7db9-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="d7db9-208">Utilizziamo un oggetto IEnumerable&lt;Genre&gt; anziché un elenco&lt;Genre&gt; poiché è più generico, che consente di modificare il tipo di modello in un secondo momento per qualsiasi tipo di oggetto che supporta l'interfaccia IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="d7db9-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="d7db9-209">Successivamente, verranno esaminati in ciclo gli oggetti di genere del modello come illustrato nel seguente codice completate.</span><span class="sxs-lookup"><span data-stu-id="d7db9-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="d7db9-210">Si noti che è presente il supporto IntelliSense completo quando si immette questo codice, in modo che quando si digita "@Model."</span><span class="sxs-lookup"><span data-stu-id="d7db9-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="d7db9-211">Noteremo tutti i metodi e proprietà supportate da un oggetto IEnumerable di tipo genere.</span><span class="sxs-lookup"><span data-stu-id="d7db9-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="d7db9-212">Nel nostro ciclo "foreach", Visual Web Developer sa che ogni elemento è di tipo Genre, noterete IntelliSense per ogni tipo di genere.</span><span class="sxs-lookup"><span data-stu-id="d7db9-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSense for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="d7db9-213">Successivamente, la funzionalità di scaffolding esaminato l'oggetto Genre e determinata che ognuna avrà una proprietà Name, scorre in ciclo e li scrive in modo. Genera anche i collegamenti di modifica, dettagli e Delete per ogni singolo elemento.</span><span class="sxs-lookup"><span data-stu-id="d7db9-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="d7db9-214">Prenderemo in cui sfruttare in un secondo momento nel nostro gestore dell'archivio, ma per ora ci piacerebbe avere invece un elenco semplice.</span><span class="sxs-lookup"><span data-stu-id="d7db9-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="d7db9-215">Quando si esegue l'applicazione e passare a /Store, vediamo che viene visualizzato il numero e l'elenco dei generi.</span><span class="sxs-lookup"><span data-stu-id="d7db9-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="d7db9-216">Aggiunta di collegamenti tra pagine</span><span class="sxs-lookup"><span data-stu-id="d7db9-216">Adding Links between pages</span></span>

<span data-ttu-id="d7db9-217">L'URL /Store che elenca generi attualmente Elenca i nomi di Sottogeneri semplicemente come testo normale.</span><span class="sxs-lookup"><span data-stu-id="d7db9-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="d7db9-218">È possibile modificare questo, in modo che invece che testo normale è invece disponibile il collegamento di nomi di genere sull'URL appropriato Store/esplorazione, in modo che facendo clic su un genere musica, ad esempio "Disco" per passare alla/Store/Sfoglia? genre = URL del Disco.</span><span class="sxs-lookup"><span data-stu-id="d7db9-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="d7db9-219">È possibile aggiornare il modello di vista \Views\Store\Index.cshtml all'output di questi collegamenti tramite il codice come di seguito **(non digitare quanto segue in, dobbiamo migliorare su di esso)**:</span><span class="sxs-lookup"><span data-stu-id="d7db9-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="d7db9-220">Funziona, ma ciò potrebbe causare problemi in un secondo momento, poiché si basa su una stringa hardcoded.</span><span class="sxs-lookup"><span data-stu-id="d7db9-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="d7db9-221">Ad esempio, se si vuole rinominare il Controller, è necessario eseguire la ricerca tramite il codice alla ricerca di collegamenti che devono essere aggiornati.</span><span class="sxs-lookup"><span data-stu-id="d7db9-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="d7db9-222">Un approccio alternativo, che è possibile usare sia per sfruttare i vantaggi di un metodo di HTML Helper.</span><span class="sxs-lookup"><span data-stu-id="d7db9-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="d7db9-223">ASP.NET MVC include metodi HTML Helper disponibili mediante il codice del modello di visualizzazione per eseguire una serie di attività comuni come segue.</span><span class="sxs-lookup"><span data-stu-id="d7db9-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="d7db9-224">Il metodo helper Html.ActionLink() è particolarmente utile e rende più semplice creare HTML &lt;un&gt; collega e si occupa di dettagli fastidiosi, ad esempio assicurandosi che i percorsi URL siano URL correttamente codificati.</span><span class="sxs-lookup"><span data-stu-id="d7db9-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="d7db9-225">Html.ActionLink() dispone di diversi overload diversi per specificare quante più informazioni in base alle esigenze per i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="d7db9-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="d7db9-226">Nel caso più semplice, si dovranno fornire solo il testo del collegamento e il metodo di azione a cui passare quando si fa clic sul collegamento ipertestuale nel client.</span><span class="sxs-lookup"><span data-stu-id="d7db9-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="d7db9-227">Ad esempio, è possibile collegare a "/ Store /" metodo Index () nella pagina dei dettagli di Store con il testo del collegamento "Vai al Store Index" tramite la chiamata seguente:</span><span class="sxs-lookup"><span data-stu-id="d7db9-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="d7db9-228">*Nota: In questo caso, non dobbiamo specificare il nome del controller perché si sta semplicemente il collegamento a un'altra azione all'interno del controller stesso che esegue il rendering della visualizzazione corrente.*</span><span class="sxs-lookup"><span data-stu-id="d7db9-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="d7db9-229">I collegamenti alla pagina di esplorazione saranno necessario passare un parametro, tuttavia, quindi verrà usato un altro overload del metodo HTML. ActionLink che accetta tre parametri:</span><span class="sxs-lookup"><span data-stu-id="d7db9-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="d7db9-230">Testo del collegamento, che verrà visualizzato il nome genere</span><span class="sxs-lookup"><span data-stu-id="d7db9-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="d7db9-231">Nome azione del controller (Sfoglia)</span><span class="sxs-lookup"><span data-stu-id="d7db9-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="d7db9-232">Valori dei parametri di route, che specifica il nome (genere) e il valore (nome genere)</span><span class="sxs-lookup"><span data-stu-id="d7db9-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="d7db9-233">L'inserimento che tutti gli elementi, ecco come verranno scritti i collegamenti alla vista Index Store:</span><span class="sxs-lookup"><span data-stu-id="d7db9-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="d7db9-234">A questo punto quando si esegue nuovamente il progetto e accedere all'URL /Store/ si verrà visualizzato un elenco dei generi.</span><span class="sxs-lookup"><span data-stu-id="d7db9-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="d7db9-235">Ogni genere è un collegamento ipertestuale: quando si fa clic potrebbero essere necessari al nostro/Store/Sfoglia? genre =*[genre]* URL.</span><span class="sxs-lookup"><span data-stu-id="d7db9-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="d7db9-236">Il codice HTML per l'elenco di genere è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="d7db9-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="d7db9-237">[Precedente](mvc-music-store-part-2.md)
> [Successivo](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="d7db9-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
