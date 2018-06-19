---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: Moduli di modifica e i modelli | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 5 vengono illustrati i moduli di modifica e modelli.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: d584e614b5a4124044cd9decd2272192ca164643
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874912"
---
<a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="b4caf-104">Parte 5: Modifica moduli e modelli</span><span class="sxs-lookup"><span data-stu-id="b4caf-104">Part 5: Edit Forms and Templating</span></span>
====================
<span data-ttu-id="b4caf-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="b4caf-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="b4caf-106">L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="b4caf-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="b4caf-107">L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="b4caf-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="b4caf-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio.</span><span class="sxs-lookup"><span data-stu-id="b4caf-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="b4caf-109">Parte 5 vengono illustrati i moduli di modifica e modelli.</span><span class="sxs-lookup"><span data-stu-id="b4caf-109">Part 5 covers Edit Forms and Templating.</span></span>


<span data-ttu-id="b4caf-110">Nel capitolo precedente, è durante il caricamento dei dati dal database e visualizzarlo.</span><span class="sxs-lookup"><span data-stu-id="b4caf-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="b4caf-111">In questo capitolo, si verrà abilitata la modifica dei dati.</span><span class="sxs-lookup"><span data-stu-id="b4caf-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="b4caf-112">Creazione di StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="b4caf-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="b4caf-113">Iniziare creando un nuovo controller chiamato **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="b4caf-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="b4caf-114">Per questo controller, si verrà essere sfruttano le funzionalità di Scaffolding disponibili in ASP.NET MVC 3 Tools Update.</span><span class="sxs-lookup"><span data-stu-id="b4caf-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="b4caf-115">Impostare le opzioni per la finestra di dialogo Aggiungi Controller, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b4caf-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="b4caf-116">Quando si fa clic sul pulsante Aggiungi, si noterà che il meccanismo di scaffolding di ASP.NET MVC 3 non è una discreta quantità di lavoro per l'utente:</span><span class="sxs-lookup"><span data-stu-id="b4caf-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="b4caf-117">Crea il nuovo StoreManagerController con una variabile locale di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b4caf-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="b4caf-118">Aggiunge una cartella StoreManager cartella visualizzazioni del progetto</span><span class="sxs-lookup"><span data-stu-id="b4caf-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="b4caf-119">Aggiunge Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml e cshtml visualizzazione fortemente tipizzata per la classe Album</span><span class="sxs-lookup"><span data-stu-id="b4caf-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="b4caf-120">La nuova classe controller StoreManager include CRUD (create, leggere, aggiornare ed eliminare) le azioni del controller che sono in grado di utilizzare Album classe del modello e utilizzare il contesto di Entity Framework per l'accesso al database.</span><span class="sxs-lookup"><span data-stu-id="b4caf-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="b4caf-121">Modifica di una vista scaffolding</span><span class="sxs-lookup"><span data-stu-id="b4caf-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="b4caf-122">È importante tenere presente che, sebbene questo codice è stato generato automaticamente, è codice standard di ASP.NET MVC, esattamente come è stato della scrittura in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="b4caf-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="b4caf-123">È progettato per risparmiare il tempo che sarebbero necessari sulla scrittura di codice boilerplate del controller e la creazione di visualizzazioni fortemente tipizzate manualmente, ma non il tipo di codice generato si potrebbe aver notato preceduto da avvertimenti nei commenti sulle non deve cambiare la codice.</span><span class="sxs-lookup"><span data-stu-id="b4caf-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="b4caf-124">Questo è il codice e previsto per modificarlo.</span><span class="sxs-lookup"><span data-stu-id="b4caf-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="b4caf-125">In tal caso, iniziamo con una modifica rapida per la visualizzazione dell'indice StoreManager (/ Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="b4caf-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="b4caf-126">Questa vista viene visualizzata una tabella che elenca gli album Negozio con Modifica / dettagli / eliminare collegamenti e include le proprietà pubbliche dell'Album.</span><span class="sxs-lookup"><span data-stu-id="b4caf-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="b4caf-127">Il campo AlbumArtUrl, rimuoverò perché non è molto utile in questa visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b4caf-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="b4caf-128">In &lt;tabella&gt; sezione del codice di visualizzazione, rimuovere il &lt;th&gt; e &lt;td&gt; elementi circostanti AlbumArtUrl riferimenti, come indicato dalle righe evidenziate riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b4caf-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="b4caf-129">Il codice di visualizzazione modificato verrà visualizzato come segue:</span><span class="sxs-lookup"><span data-stu-id="b4caf-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="b4caf-130">Un primo sguardo al gestore del Negozio</span><span class="sxs-lookup"><span data-stu-id="b4caf-130">A first look at the Store Manager</span></span>

<span data-ttu-id="b4caf-131">Esegui l'applicazione e passare a/StoreManager /.</span><span class="sxs-lookup"><span data-stu-id="b4caf-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="b4caf-132">Consente di visualizzare l'indice di gestione archiviazione appena modificata, che mostra un elenco di quelli nell'archivio con collegamenti a informazioni dettagliate, modifica ed Elimina.</span><span class="sxs-lookup"><span data-stu-id="b4caf-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="b4caf-133">Il collegamento di modifica un modulo di modifica con i campi per visualizzare Album, inclusi elenchi a discesa per Genre e artista.</span><span class="sxs-lookup"><span data-stu-id="b4caf-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="b4caf-134">Fare clic sul collegamento "Torna all'elenco" nella parte inferiore, quindi fare clic sul collegamento dettagli per un Album.</span><span class="sxs-lookup"><span data-stu-id="b4caf-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="b4caf-135">Consente di visualizzare le informazioni dettagliate per un singolo Album.</span><span class="sxs-lookup"><span data-stu-id="b4caf-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="b4caf-136">Nuovamente, fare clic su nella parte posteriore al collegamento di elenco, quindi fare clic sul collegamento Elimina.</span><span class="sxs-lookup"><span data-stu-id="b4caf-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="b4caf-137">Verrà visualizzata una finestra di dialogo di conferma, che mostra i dettagli dell'album e viene chiesto se si è certi di voler eliminare.</span><span class="sxs-lookup"><span data-stu-id="b4caf-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="b4caf-138">Fare clic sul pulsante Elimina nella parte inferiore verrà eliminare album e tornare alla pagina di indice, che mostra album eliminato.</span><span class="sxs-lookup"><span data-stu-id="b4caf-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="b4caf-139">Non è stata completata con la gestione di Store, ma non è disponibile l'utilizzo di controller e visualizza il codice per le operazioni CRUD avviare da.</span><span class="sxs-lookup"><span data-stu-id="b4caf-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="b4caf-140">Esaminando il codice del Controller di gestione di Store</span><span class="sxs-lookup"><span data-stu-id="b4caf-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="b4caf-141">Il Controller di gestione di Store contiene una discreta quantità di codice.</span><span class="sxs-lookup"><span data-stu-id="b4caf-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="b4caf-142">Verrà ora analizzato questo dall'alto verso il basso.</span><span class="sxs-lookup"><span data-stu-id="b4caf-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="b4caf-143">Il controller include alcuni spazi dei nomi standard per un controller MVC, nonché un riferimento a questo spazio dei nomi modelli.</span><span class="sxs-lookup"><span data-stu-id="b4caf-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="b4caf-144">Il controller ha un'istanza privata della MusicStoreEntities, ognuna delle azioni controller utilizzato per accedere ai dati.</span><span class="sxs-lookup"><span data-stu-id="b4caf-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="b4caf-145">Registrare azioni dell'indice di gestione e i dettagli</span><span class="sxs-lookup"><span data-stu-id="b4caf-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="b4caf-146">La visualizzazione dell'indice recupera un elenco di album, tra cui Genre e artista informazioni di riferimento ogni album, come illustrato in precedenza quando si utilizza il metodo Store Sfoglia.</span><span class="sxs-lookup"><span data-stu-id="b4caf-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="b4caf-147">La visualizzazione dell'indice è dopo i riferimenti a oggetti collegati in modo da poter visualizzare ogni album Genre e artista nome, in modo efficiente e l'esecuzione di query per ottenere questa informazione nella richiesta originale, è in corso il controller.</span><span class="sxs-lookup"><span data-stu-id="b4caf-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="b4caf-148">Azione del controller i dettagli del StoreManager Controller funziona esattamente come l'azione di dettagli sui Controller di archiviazione è scritto in precedenza, viene eseguita una query per Album dall'ID utilizzando il metodo Find(), lo restituisce quindi alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b4caf-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="b4caf-149">La creazione di metodi di azione</span><span class="sxs-lookup"><span data-stu-id="b4caf-149">The Create Action Methods</span></span>

<span data-ttu-id="b4caf-150">I metodi di azione Create sono leggermente diversi rispetto a quella che abbiamo visto finora, perché gestiscono l'input del form.</span><span class="sxs-lookup"><span data-stu-id="b4caf-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="b4caf-151">Quando un utente visita prima /StoreManager/creazione/essi verrà visualizzati un form vuoto.</span><span class="sxs-lookup"><span data-stu-id="b4caf-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="b4caf-152">La pagina HTML conterrà un &lt;modulo&gt; elemento che contiene l'elenco a discesa e casella di testo in cui possono inserire i dettagli dell'album di elementi di input.</span><span class="sxs-lookup"><span data-stu-id="b4caf-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="b4caf-153">Dopo che l'utente immette i valori di form Album, è possibile fare clic sul pulsante "Salva" per inviare che le modifiche all'applicazione per salvare nel database.</span><span class="sxs-lookup"><span data-stu-id="b4caf-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="b4caf-154">Quando l'utente preme il pulsante "Salva" il &lt;modulo&gt; eseguirà un POST HTTP al /StoreManager/creazione/URL e invia il &lt;modulo&gt; valori come parte di HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="b4caf-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="b4caf-155">ASP.NET MVC consente di dividere costituisce la logica di questi due scenari di chiamata URL consentono di implementare due metodi di azione "Crea" distinti all'interno di classe StoreManagerController: gestire Sfoglia HTTP-GET iniziale per il /StoreManager/Create / URL e l'altra di gestire HTTP-POST delle modifiche inviate.</span><span class="sxs-lookup"><span data-stu-id="b4caf-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="b4caf-156">Passaggio di informazioni in una vista utilizzando ViewBag</span><span class="sxs-lookup"><span data-stu-id="b4caf-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="b4caf-157">È stato usato ViewBag precedentemente in questa esercitazione, ma non hai parlato molto su di esso.</span><span class="sxs-lookup"><span data-stu-id="b4caf-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="b4caf-158">ViewBag consente di passare alla visualizzazione informazioni senza utilizzare un oggetto modello fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="b4caf-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="b4caf-159">In questo caso, l'azione del controller modifica HTTP-GET deve passare un elenco di generi e artisti di modulo per popolare i menu a discesa e il modo più semplice per eseguire questa operazione è per restituirli come elementi ViewBag.</span><span class="sxs-lookup"><span data-stu-id="b4caf-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="b4caf-160">ViewBag è un oggetto dinamico, vale a dire che è possibile digitare ViewBag.Foo o ViewBag.YourNameHere senza scrivere codice per definire tali proprietà.</span><span class="sxs-lookup"><span data-stu-id="b4caf-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="b4caf-161">In questo caso, il codice del controller utilizza ViewBag.GenreId e ViewBag.ArtistId in modo che i valori di elenco a discesa inviati con il modulo GenreId e ArtistId, ovvero si prevede di impostare le proprietà di Album.</span><span class="sxs-lookup"><span data-stu-id="b4caf-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="b4caf-162">I valori di elenco a discesa vengono restituiti al form utilizzando l'oggetto SelectList, che viene compilato solo a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="b4caf-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="b4caf-163">Questa operazione viene eseguita utilizzando codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b4caf-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="b4caf-164">Come si può vedere dal codice del metodo di azione, tre parametri vengono utilizzati per creare questo oggetto:</span><span class="sxs-lookup"><span data-stu-id="b4caf-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="b4caf-165">L'elenco di elementi che prevede di visualizzare l'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="b4caf-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="b4caf-166">Si noti che questo non è una stringa, passaggio di un elenco di generi.</span><span class="sxs-lookup"><span data-stu-id="b4caf-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="b4caf-167">Il parametro successivo da passare il SelectList è il valore selezionato.</span><span class="sxs-lookup"><span data-stu-id="b4caf-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="b4caf-168">In questo modo il SelectList sia in grado di pre-selezionare un elemento nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="b4caf-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="b4caf-169">Questo sarà più facile da comprendere quando si esamina il modulo di modifica, è piuttosto simile.</span><span class="sxs-lookup"><span data-stu-id="b4caf-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="b4caf-170">Il parametro finale è la proprietà da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="b4caf-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="b4caf-171">In questo caso, ciò indica che la proprietà Genre.Name è ciò che verrà visualizzato all'utente.</span><span class="sxs-lookup"><span data-stu-id="b4caf-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="b4caf-172">Tenendo presente, quindi, l'azione Create HTTP-GET è piuttosto semplice - due SelectLists vengono aggiunti al ViewBag e nessun oggetto modello viene passato al form (dal momento che non è stato ancora creato).</span><span class="sxs-lookup"><span data-stu-id="b4caf-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="b4caf-173">Helper HTML per visualizzare il Drop down nell'istruzione Create View</span><span class="sxs-lookup"><span data-stu-id="b4caf-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="b4caf-174">Poiché sono stati trattati come elenco a discesa di valori vengono passati alla visualizzazione, è opportuno un rapido controllo di visualizzazione per vedere come vengono visualizzati i valori.</span><span class="sxs-lookup"><span data-stu-id="b4caf-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="b4caf-175">Nel codice di visualizzazione (/ Views/StoreManager/Create.cshtml), si noterà che viene effettuata la chiamata seguente per visualizzare il genere verso il basso.</span><span class="sxs-lookup"><span data-stu-id="b4caf-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="b4caf-176">Questo è noto come un HTML Helper, un metodo di utilità che consente di eseguire un'attività comune di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b4caf-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="b4caf-177">Helper HTML sono molto utili per mantenere il codice visualizza concise e leggibili.</span><span class="sxs-lookup"><span data-stu-id="b4caf-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="b4caf-178">L'helper Html.DropDownList viene fornito da ASP.NET MVC, ma come vedremo in un secondo momento è possibile creare un helper per il codice di visualizzazione che verrà riutilizzato nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b4caf-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="b4caf-179">La chiamata di Html.DropDownList deve semplicemente indicato due cose - in di ottenere l'elenco per visualizzare e valore (se presente) deve essere preselezionato.</span><span class="sxs-lookup"><span data-stu-id="b4caf-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="b4caf-180">Il primo parametro, GenreId, indica DropDownList per cercare un valore denominato GenreId nel modello o ViewBag.</span><span class="sxs-lookup"><span data-stu-id="b4caf-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="b4caf-181">Il secondo parametro è utilizzato per indicare il valore per visualizzare inizialmente selezionato nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="b4caf-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="b4caf-182">Poiché questo modulo è un modulo di creazione, è presente alcun valore per essere preselezionate e viene passato String. Empty.</span><span class="sxs-lookup"><span data-stu-id="b4caf-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="b4caf-183">La gestione dei valori di modulo registrato</span><span class="sxs-lookup"><span data-stu-id="b4caf-183">Handling the Posted Form values</span></span>

<span data-ttu-id="b4caf-184">Come accennato prima, esistono due metodi di azione associati a ogni modulo.</span><span class="sxs-lookup"><span data-stu-id="b4caf-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="b4caf-185">Il primo gestisce la richiesta HTTP GET e visualizza il form.</span><span class="sxs-lookup"><span data-stu-id="b4caf-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="b4caf-186">Il secondo gestisce la richiesta HTTP POST, che contiene i valori del modulo inviato.</span><span class="sxs-lookup"><span data-stu-id="b4caf-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="b4caf-187">Si noti che l'azione del controller è un attributo [HttpPost], che indica ad ASP.NET MVC che deve rispondere solo alle richieste HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="b4caf-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="b4caf-188">Questa azione ha quattro responsabilità:</span><span class="sxs-lookup"><span data-stu-id="b4caf-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="b4caf-189">Leggere i valori del form</span><span class="sxs-lookup"><span data-stu-id="b4caf-189">Read the form values</span></span>
- 2. <span data-ttu-id="b4caf-190">Controllare se i valori del form passare qualsiasi regola di convalida</span><span class="sxs-lookup"><span data-stu-id="b4caf-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="b4caf-191">Se l'invio del modulo è valido, salvare i dati e visualizzare l'elenco aggiornato</span><span class="sxs-lookup"><span data-stu-id="b4caf-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="b4caf-192">Se l'invio del modulo non è valido, rivisualizzare il form con errori di convalida</span><span class="sxs-lookup"><span data-stu-id="b4caf-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="b4caf-193">Lettura di valori di Form con associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="b4caf-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="b4caf-194">Invio di un form che contiene i valori per GenreId e ArtistId (dall'elenco a discesa) e i valori di casella di testo per titolo, prezzo e AlbumArtUrl elaborazione azione del controller.</span><span class="sxs-lookup"><span data-stu-id="b4caf-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="b4caf-195">Sebbene sia possibile accedere direttamente ai valori del form, un approccio migliore è di utilizzare le funzionalità di associazione di modelli integrate in MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b4caf-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="b4caf-196">Quando un'azione del controller richiede un tipo di modello come parametro, verrà effettuato un tentativo di popolare un oggetto di quel tipo utilizzando formano l'input (così come i valori di route e querystring) ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b4caf-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="b4caf-197">A tale scopo, cercando valori i cui nomi corrispondano alle proprietà dell'oggetto modello, ad esempio, quando si imposta il nuovo Album valore dell'oggetto GenreId, la ricerca di un input con il nome GenreId.</span><span class="sxs-lookup"><span data-stu-id="b4caf-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="b4caf-198">Quando si creano visualizzazioni utilizzando i metodi standard in ASP.NET MVC, il form verranno visualizzati sempre nomi delle proprietà come nomi di campo di input, in modo i nomi dei campi verrà semplicemente trovata corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="b4caf-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="b4caf-199">La convalida del modello</span><span class="sxs-lookup"><span data-stu-id="b4caf-199">Validating the Model</span></span>

<span data-ttu-id="b4caf-200">Il modello viene convalidato con una semplice chiamata a ModelState.IsValid.</span><span class="sxs-lookup"><span data-stu-id="b4caf-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="b4caf-201">È ancora stato aggiunto a qualsiasi regola di convalida per la classe Album ancora - faremo contenenti un bit - momento questo controllo non è possibile eseguire.</span><span class="sxs-lookup"><span data-stu-id="b4caf-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="b4caf-202">L'aspetto importante è che questo controllo ModelStat.IsValid adatterà alle regole di convalida che è stato inserito nel modello in esame, pertanto le future modifiche alle regole di convalida non richiedono aggiornamenti per il codice di azione del controller.</span><span class="sxs-lookup"><span data-stu-id="b4caf-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="b4caf-203">Il salvataggio dei valori inviati</span><span class="sxs-lookup"><span data-stu-id="b4caf-203">Saving the submitted values</span></span>

<span data-ttu-id="b4caf-204">Se l'invio del modulo passa la convalida, è necessario salvare i valori nel database.</span><span class="sxs-lookup"><span data-stu-id="b4caf-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="b4caf-205">Con Entity Framework, che richiede semplicemente aggiungere il modello di raccolta album e chiamare SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="b4caf-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="b4caf-206">Entity Framework genera comandi SQL corretti per mantenere il valore.</span><span class="sxs-lookup"><span data-stu-id="b4caf-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="b4caf-207">Dopo aver salvato i dati, si reindirizza l'elenco di album come possiamo vedere l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="b4caf-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="b4caf-208">Questa operazione viene eseguita tramite la restituzione di RedirectToAction con il nome dell'azione del controller a cui che si desidera visualizzare.</span><span class="sxs-lookup"><span data-stu-id="b4caf-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="b4caf-209">In questo caso, si tratta del metodo di indice.</span><span class="sxs-lookup"><span data-stu-id="b4caf-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="b4caf-210">Visualizzazione di invii di form non valido con errori di convalida</span><span class="sxs-lookup"><span data-stu-id="b4caf-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="b4caf-211">Nel caso di input del form non valido, i valori di elenco a discesa vengono aggiunti a ViewBag (come nel caso di HTTP GET) e vengono passati i valori del modello associato alla visualizzazione per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b4caf-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="b4caf-212">Errori di convalida vengono visualizzati automaticamente utilizzando il @Html.ValidationMessageFor HTML Helper.</span><span class="sxs-lookup"><span data-stu-id="b4caf-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="b4caf-213">Modulo di creazione di test</span><span class="sxs-lookup"><span data-stu-id="b4caf-213">Testing the Create Form</span></span>

<span data-ttu-id="b4caf-214">Per testare questo, Esegui l'applicazione e passare al /StoreManager/creazione / - verranno visualizzati è un modulo vuoto in cui è stato restituito dal metodo StoreController creare HTTP-GET.</span><span class="sxs-lookup"><span data-stu-id="b4caf-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="b4caf-215">Specificare alcuni valori e fare clic sul pulsante Crea per inviare il form.</span><span class="sxs-lookup"><span data-stu-id="b4caf-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="b4caf-216">Gestione delle modifiche</span><span class="sxs-lookup"><span data-stu-id="b4caf-216">Handling Edits</span></span>

<span data-ttu-id="b4caf-217">La coppia di azione di modifica (HTTP-GET e HTTP-POST) è molto simile ai metodi di azione Crea che abbiamo appena esaminato.</span><span class="sxs-lookup"><span data-stu-id="b4caf-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="b4caf-218">Poiché lo scenario di modifica implica l'utilizzo di un album esistente, la modifica HTTP-GET metodo carica Album in base al parametro "id", passato tramite la route.</span><span class="sxs-lookup"><span data-stu-id="b4caf-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="b4caf-219">Questo codice per il recupero di un album di payload è lo stesso come in precedenza abbiamo esaminato nell'azione del controller di dettagli.</span><span class="sxs-lookup"><span data-stu-id="b4caf-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="b4caf-220">Come con la creazione / HTTP-GET (metodo), nell'elenco a discesa di valori vengono restituiti tramite ViewBag.</span><span class="sxs-lookup"><span data-stu-id="b4caf-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="b4caf-221">Ciò consente di restituire un Album come l'oggetto modello per la visualizzazione (che deve essere fortemente tipizzata per la classe di Album) durante il passaggio di dati aggiuntivi (ad esempio, un elenco di generi) tramite ViewBag.</span><span class="sxs-lookup"><span data-stu-id="b4caf-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="b4caf-222">L'azione di modifica HTTP-POST è molto simile all'azione di creazione HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="b4caf-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="b4caf-223">L'unica differenza è che, anziché aggiungere un nuovo album al database. Raccolta album, ci stiamo ricerca l'istanza corrente dell'Album tramite db. Entry(album) e impostare lo stato modificato.</span><span class="sxs-lookup"><span data-stu-id="b4caf-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="b4caf-224">Ciò indica a Entity Framework che si sta modificando un album esistente anziché crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="b4caf-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="b4caf-225">È possibile testarlo in esecuzione l'applicazione e la selezione di StoreManger/e scegliendo il collegamento di modifica di un album.</span><span class="sxs-lookup"><span data-stu-id="b4caf-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="b4caf-226">Consente di visualizzare il modulo di modifica indicato dal metodo di modifica HTTP-GET.</span><span class="sxs-lookup"><span data-stu-id="b4caf-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="b4caf-227">Specificare alcuni valori e fare clic sul pulsante Salva.</span><span class="sxs-lookup"><span data-stu-id="b4caf-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="b4caf-228">Invia il form, Salva i valori e restituisce ci all'elenco di Album, che mostra che i valori siano stati aggiornati.</span><span class="sxs-lookup"><span data-stu-id="b4caf-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="b4caf-229">Eliminazione di gestione</span><span class="sxs-lookup"><span data-stu-id="b4caf-229">Handling Deletion</span></span>

<span data-ttu-id="b4caf-230">L'eliminazione segue dello stesso modello di modifica e creare, tramite l'azione di un controller per visualizzare il modulo di conferma e un'altra azione del controller per gestire l'invio del modulo.</span><span class="sxs-lookup"><span data-stu-id="b4caf-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="b4caf-231">Azione del controller HTTP-GET Delete corrisponde esattamente come l'azione del controller precedente dettagli di gestione di Store.</span><span class="sxs-lookup"><span data-stu-id="b4caf-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="b4caf-232">A un tipo di Album, utilizzando il modello di contenuto di visualizzazione di eliminazione, viene visualizzato un form che deve essere fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="b4caf-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="b4caf-233">Il modello di eliminazione Mostra tutti i campi per il modello, ma è possibile semplificare abbastanza tale verso il basso.</span><span class="sxs-lookup"><span data-stu-id="b4caf-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="b4caf-234">Modificare la visualizzazione del codice /Views/StoreManager/Delete.cshtml al seguente.</span><span class="sxs-lookup"><span data-stu-id="b4caf-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="b4caf-235">Verrà visualizzato un messaggio di conferma eliminazione semplificata.</span><span class="sxs-lookup"><span data-stu-id="b4caf-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="b4caf-236">Facendo clic sul pulsante Elimina, il modulo eseguire il postback al server, che esegue l'azione DeleteConfirmed.</span><span class="sxs-lookup"><span data-stu-id="b4caf-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="b4caf-237">L'azione del Controller Delete HTTP-POST esegue le azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b4caf-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="b4caf-238">Carica Album da ID</span><span class="sxs-lookup"><span data-stu-id="b4caf-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="b4caf-239">Elimina album e salvare le modifiche</span><span class="sxs-lookup"><span data-stu-id="b4caf-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="b4caf-240">Effettua il reindirizzamento all'indice, che mostra che Album è stato rimosso dall'elenco</span><span class="sxs-lookup"><span data-stu-id="b4caf-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="b4caf-241">Per eseguire questa verifica, eseguire l'applicazione e passare a /StoreManager.</span><span class="sxs-lookup"><span data-stu-id="b4caf-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="b4caf-242">Selezionare un album dall'elenco e fare clic sul collegamento Elimina.</span><span class="sxs-lookup"><span data-stu-id="b4caf-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="b4caf-243">Verrà visualizzata la schermata di conferma eliminazione.</span><span class="sxs-lookup"><span data-stu-id="b4caf-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="b4caf-244">Fare clic sul pulsante Delete rimuove album e noi restituisce alla pagina di indice di gestione di Store, indicante che è stato eliminato album.</span><span class="sxs-lookup"><span data-stu-id="b4caf-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="b4caf-245">Utilizzo di un HTML Helper personalizzati per troncare il testo</span><span class="sxs-lookup"><span data-stu-id="b4caf-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="b4caf-246">Abbiamo un potenziale problema con la pagina di indice di gestione di Store.</span><span class="sxs-lookup"><span data-stu-id="b4caf-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="b4caf-247">La proprietà titolo dell'Album e artista possono essere entrambi sufficientemente lunga che potrebbe potenzialmente generare disattivata la formattazione della tabella.</span><span class="sxs-lookup"><span data-stu-id="b4caf-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="b4caf-248">Si creerà un HTML Helper personalizzati per consentire di troncare facilmente queste e altre proprietà nel nostro viste.</span><span class="sxs-lookup"><span data-stu-id="b4caf-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="b4caf-249">Del Razor @helper sintassi ha reso piuttosto semplice creare funzioni di supporto per l'utilizzo nelle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="b4caf-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="b4caf-250">Aprire la vista /Views/StoreManager/Index.cshtml e aggiungere il codice seguente immediatamente dopo il @model riga.</span><span class="sxs-lookup"><span data-stu-id="b4caf-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="b4caf-251">Questo metodo di supporto accetta una stringa e la lunghezza massima per consentire.</span><span class="sxs-lookup"><span data-stu-id="b4caf-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="b4caf-252">Se il testo specificato è inferiore alla lunghezza specificata, l'helper genera come-è.</span><span class="sxs-lookup"><span data-stu-id="b4caf-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="b4caf-253">Se più lungo, quindi tronca il testo e viene eseguito il rendering "…" per il resto.</span><span class="sxs-lookup"><span data-stu-id="b4caf-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="b4caf-254">Ora è possibile utilizzare l'helper di troncamento per assicurarsi che il titolo dell'Album e il nome artista proprietà sono meno di 25 caratteri.</span><span class="sxs-lookup"><span data-stu-id="b4caf-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="b4caf-255">Il codice di visualizzazione completa utilizzando l'helper di troncamento nuovo viene visualizzato sotto.</span><span class="sxs-lookup"><span data-stu-id="b4caf-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="b4caf-256">A questo punto quando si passa l'URL /StoreManager/, vengono mantenuti sotto la lunghezza massima di album e titoli.</span><span class="sxs-lookup"><span data-stu-id="b4caf-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="b4caf-257">Nota: Viene illustrato il caso di creazione e utilizzo di un helper in una visualizzazione semplice.</span><span class="sxs-lookup"><span data-stu-id="b4caf-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="b4caf-258">Per ulteriori informazioni sulla creazione di helper che è possibile utilizzare in tutto il sito, vedere il blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="b4caf-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="b4caf-259">[Precedente](mvc-music-store-part-4.md)
> [Successivo](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="b4caf-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>
