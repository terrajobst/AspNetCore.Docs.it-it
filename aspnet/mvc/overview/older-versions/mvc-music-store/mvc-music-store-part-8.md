---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: Carrello acquisti con gli aggiornamenti Ajax | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 8 copre un carrello acquisti con gli aggiornamenti Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 75e1dff96f8b56d74c28ff9d522f4766fbad669f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="0521a-104">Parte 8: Shopping Cart con gli aggiornamenti Ajax</span><span class="sxs-lookup"><span data-stu-id="0521a-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="0521a-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="0521a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="0521a-106">L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="0521a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="0521a-107">L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="0521a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="0521a-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio.</span><span class="sxs-lookup"><span data-stu-id="0521a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="0521a-109">Parte 8 copre un carrello acquisti con gli aggiornamenti Ajax.</span><span class="sxs-lookup"><span data-stu-id="0521a-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="0521a-110">Faremo in modo agli utenti di effettuare album nel carrello senza registrazione, ma sarà necessario registrare come guest a completare il checkpoint.</span><span class="sxs-lookup"><span data-stu-id="0521a-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="0521a-111">Il processo di acquisto e l'estrazione verrà separato in due controller: un ShoppingCart Controller che consente l'aggiunta di elementi in modo anonimo a un carrello e un Controller di estrazione che gestisce il processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="0521a-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="0521a-112">Si sarà iniziare con il carrello in questa sezione, quindi compilare il processo di estrazione nella sezione seguente.</span><span class="sxs-lookup"><span data-stu-id="0521a-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="0521a-113">Aggiunta di classi di modello carrello, ordine e OrderDetail</span><span class="sxs-lookup"><span data-stu-id="0521a-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="0521a-114">I processi del carrello e l'estrazione renderà utilizzare di alcune nuove classi.</span><span class="sxs-lookup"><span data-stu-id="0521a-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="0521a-115">Fare clic sulla cartella di modelli e aggiungere una classe di carrello (Cart.cs) con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="0521a-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="0521a-116">Questa classe è molto simile ad altri utenti che è stata utilizzata fino a questo punto, fatta eccezione per l'attributo [Key] per la proprietà RecordId.</span><span class="sxs-lookup"><span data-stu-id="0521a-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="0521a-117">L'elemento di carrello disporrà di un identificatore di stringa denominato CartID per consentire di acquisti anonimi, ma la tabella include una chiave primaria di tipo integer denominata RecordId.</span><span class="sxs-lookup"><span data-stu-id="0521a-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="0521a-118">Per convenzione, Code-First di Entity Framework prevede che la chiave primaria per una tabella denominata carrello sarà CartId o ID, ma è possibile eseguire facilmente l'override che tramite le annotazioni o codice se lo si desidera.</span><span class="sxs-lookup"><span data-stu-id="0521a-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="0521a-119">Questo è un esempio di come è possibile utilizzare le convenzioni semplice in Entity Framework Code-First quando sono in base alle us, ma ci non stiamo è vincolati da li quando questo non avviene.</span><span class="sxs-lookup"><span data-stu-id="0521a-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="0521a-120">Successivamente, aggiungere una classe Order (cs) con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="0521a-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="0521a-121">Questa classe tiene traccia delle informazioni di riepilogo e il recapito di un ordine.</span><span class="sxs-lookup"><span data-stu-id="0521a-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="0521a-122">**Non verrà compilato ancora**perché è una proprietà di navigazione OrderDetails che dipende da una classe è non ancora creato.</span><span class="sxs-lookup"><span data-stu-id="0521a-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="0521a-123">Si correggerà che ora aggiungendo una classe denominata OrderDetail.cs, aggiungendo il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="0521a-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="0521a-124">Ultimo aggiornamento da apportare alla classe MusicStoreEntities includere DbSets che espongono le nuove classi di modello, incluso anche un elemento DbSet&lt;artista&gt;.</span><span class="sxs-lookup"><span data-stu-id="0521a-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="0521a-125">La classe MusicStoreEntities aggiornata viene visualizzato come di seguito.</span><span class="sxs-lookup"><span data-stu-id="0521a-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="0521a-126">Gestire la logica di business Shopping Cart</span><span class="sxs-lookup"><span data-stu-id="0521a-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="0521a-127">Successivamente, si creerà la classe ShoppingCart nella cartella Models.</span><span class="sxs-lookup"><span data-stu-id="0521a-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="0521a-128">Il modello ShoppingCart gestisce l'accesso ai dati alla tabella carrello.</span><span class="sxs-lookup"><span data-stu-id="0521a-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="0521a-129">Inoltre, gestirà la logica di business per l'aggiunta e rimozione di elementi dal carrello.</span><span class="sxs-lookup"><span data-stu-id="0521a-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="0521a-130">Poiché non si desidera richiedere agli utenti di iscriversi a un account solo aggiungere elementi al carrello acquisti, si assegnerà agli utenti un identificatore univoco temporaneo (utilizzando un GUID o un identificatore univoco globale) quando accedono il carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="0521a-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="0521a-131">Questo ID utilizzando la classe di sessione ASP.NET verranno archiviate.</span><span class="sxs-lookup"><span data-stu-id="0521a-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="0521a-132">*Nota: La sessione ASP.NET è una posizione comoda in cui archiviare informazioni specifiche dell'utente che scadranno dopo di lasciare il sito. Durante l'utilizzo improprio dello stato sessione può avere implicazioni sulle prestazioni siti di grandi dimensioni, l'uso di luce funzionerà anche a scopo dimostrativo.*</span><span class="sxs-lookup"><span data-stu-id="0521a-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="0521a-133">La classe ShoppingCart espone i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="0521a-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="0521a-134">**AddToCart** accetta un Album come parametro e lo aggiunge al carrello acquisti dell'utente.</span><span class="sxs-lookup"><span data-stu-id="0521a-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="0521a-135">Poiché la tabella di carrello tiene traccia della quantità per ogni album, include la logica per creare una nuova riga, se necessario o semplicemente incrementa la quantità se l'utente ha già ordinato una copia dell'album.</span><span class="sxs-lookup"><span data-stu-id="0521a-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="0521a-136">**RemoveFromCart** accetta un ID di Album e lo rimuove dal carrello dell'utente.</span><span class="sxs-lookup"><span data-stu-id="0521a-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="0521a-137">Se l'utente dispone solo di una copia dell'album nel carrello, la riga viene rimossa.</span><span class="sxs-lookup"><span data-stu-id="0521a-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="0521a-138">**EmptyCart** rimuove tutti gli elementi dal carrello acquisti dell'utente.</span><span class="sxs-lookup"><span data-stu-id="0521a-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="0521a-139">**GetCartItems** recupera un elenco di CartItems per la visualizzazione o l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="0521a-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="0521a-140">**GetCount** recupera un numero totale di album dispone di un utente nel carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="0521a-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="0521a-141">**GetTotal** calcola il costo totale di tutti gli elementi nel carrello.</span><span class="sxs-lookup"><span data-stu-id="0521a-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="0521a-142">**CreateOrder** converte il carrello acquisti in un ordine durante la fase di estrazione.</span><span class="sxs-lookup"><span data-stu-id="0521a-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="0521a-143">**GetCart** è un metodo statico che consente il controller ottenere un oggetto del carrello.</span><span class="sxs-lookup"><span data-stu-id="0521a-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="0521a-144">Usa il **GetCartId** metodo per gestire la lettura di CartId dalla sessione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="0521a-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="0521a-145">Il metodo GetCartId richiede HttpContextBase in modo che è possibile leggere CartId dell'utente dalla sessione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="0521a-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="0521a-146">Ecco l'intero **classe ShoppingCart**:</span><span class="sxs-lookup"><span data-stu-id="0521a-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="0521a-147">ViewModel</span><span class="sxs-lookup"><span data-stu-id="0521a-147">ViewModels</span></span>

<span data-ttu-id="0521a-148">Il Controller del carrello acquisti sarà necessario comunicare alcune informazioni complesse alle visualizzazioni che non eseguono correttamente il mapping per gli oggetti modello.</span><span class="sxs-lookup"><span data-stu-id="0521a-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="0521a-149">Non si desidera modificare i modelli per soddisfare il nostro viste. Le classi del modello devono rappresentare il dominio, non l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="0521a-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="0521a-150">Una soluzione, è possibile passare le informazioni per il nostro viste utilizzando la classe ViewBag, come è stato eseguito con le informazioni di gestione di Store elenco a discesa, ma il passaggio di molte informazioni tramite ViewBag Ottiene difficile da gestire.</span><span class="sxs-lookup"><span data-stu-id="0521a-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="0521a-151">Una soluzione consiste nell'usare il *ViewModel* modello.</span><span class="sxs-lookup"><span data-stu-id="0521a-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="0521a-152">Quando si utilizza questo modello che è creare classi fortemente tipizzate che sono ottimizzati per gli scenari di visualizzazione ed ed espongono le proprietà per i valori o contenuto dinamico necessari per i modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="0521a-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="0521a-153">La classi controller quindi compilare e passare queste classi con ottimizzazione per la visualizzazione per il modello di visualizzazione da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="0521a-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="0521a-154">In questo modo l'indipendenza dai tipi, il controllo in fase di compilazione ed editor IntelliSense all'interno di modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="0521a-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="0521a-155">Si creerà due modelli di visualizzazione da utilizzare nel controller nostro carrello acquisti: il ShoppingCartViewModel conterrà il contenuto del carrello acquisti dell'utente e la ShoppingCartRemoveViewModel verrà utilizzato per visualizzare le informazioni di conferma quando l'utente rimuove un elemento dal carrello.</span><span class="sxs-lookup"><span data-stu-id="0521a-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="0521a-156">Creare una nuova cartella ViewModel nella radice del progetto per organizzare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="0521a-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="0521a-157">Fare clic sul progetto, selezionare Aggiungi / nuova cartella.</span><span class="sxs-lookup"><span data-stu-id="0521a-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="0521a-158">Nome della cartella ViewModel.</span><span class="sxs-lookup"><span data-stu-id="0521a-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="0521a-159">Successivamente, aggiungere la classe ShoppingCartViewModel nella cartella ViewModel.</span><span class="sxs-lookup"><span data-stu-id="0521a-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="0521a-160">Ha due proprietà: un elenco di articoli del carrello e un valore decimale per contenere il prezzo totale per tutti gli elementi nel carrello.</span><span class="sxs-lookup"><span data-stu-id="0521a-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="0521a-161">Aggiungere ora la ShoppingCartRemoveViewModel alla cartella ViewModel, con le seguenti quattro proprietà.</span><span class="sxs-lookup"><span data-stu-id="0521a-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="0521a-162">Il Controller del carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="0521a-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="0521a-163">Il controller Shopping Cart ha tre scopi principali: aggiunta di elementi a un carrello, la rimozione di elementi dal carrello e visualizzazione degli elementi nel carrello.</span><span class="sxs-lookup"><span data-stu-id="0521a-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="0521a-164">Apporterà utilizzare delle tre classi a cui è appena creato: ShoppingCartViewModel ShoppingCartRemoveViewModel e ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="0521a-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="0521a-165">Come nel StoreController e StoreManagerController, verrà aggiunto un campo deve contenere un'istanza di MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="0521a-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="0521a-166">Aggiungere un nuovo controller Shopping Cart al progetto utilizzando il modello di controller vuoto.</span><span class="sxs-lookup"><span data-stu-id="0521a-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="0521a-167">Di seguito è riportato il ShoppingCart Controller completo.</span><span class="sxs-lookup"><span data-stu-id="0521a-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="0521a-168">Le azioni Aggiungi Controller e di indice dovrebbero essere molto familiare.</span><span class="sxs-lookup"><span data-stu-id="0521a-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="0521a-169">Le azioni del controller Remove e CartSummary gestiscono due casi speciali, che verranno discussi nella sezione seguente.</span><span class="sxs-lookup"><span data-stu-id="0521a-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="0521a-170">Aggiornamenti AJAX con jQuery</span><span class="sxs-lookup"><span data-stu-id="0521a-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="0521a-171">Successivamente, si creerà una pagina di indice di carrello acquisti che deve essere fortemente tipizzata per il ShoppingCartViewModel e utilizza il modello di visualizzazione elenco utilizzando lo stesso metodo.</span><span class="sxs-lookup"><span data-stu-id="0521a-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="0521a-172">Tuttavia, anziché utilizzare un HTML. ActionLink per rimuovere gli elementi dal carrello, useremo jQuery per "associare" l'evento click per tutti i collegamenti in questa vista con la classe HTML RemoveLink.</span><span class="sxs-lookup"><span data-stu-id="0521a-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="0521a-173">Anziché il modulo di registrazione, questo gestore dell'evento click consentirà solo un callback AJAX per l'azione del controller RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="0521a-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="0521a-174">Il RemoveFromCart restituisce un risultato serializzato in formato JSON, che il callback di jQuery quindi analizza ed esegue quattro aggiornamenti rapidi della pagina tramite jQuery:</span><span class="sxs-lookup"><span data-stu-id="0521a-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="0521a-175">Rimuove il album eliminato dall'elenco</span><span class="sxs-lookup"><span data-stu-id="0521a-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="0521a-176">Aggiorna il conteggio di carrello nell'intestazione</span><span class="sxs-lookup"><span data-stu-id="0521a-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="0521a-177">Visualizza un messaggio di aggiornamento all'utente</span><span class="sxs-lookup"><span data-stu-id="0521a-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="0521a-178">Aggiorna il prezzo totale carrello</span><span class="sxs-lookup"><span data-stu-id="0521a-178">Updates the cart total price</span></span>

<span data-ttu-id="0521a-179">Poiché lo scenario di installazione viene gestito da un callback Ajax all'interno della visualizzazione dell'indice, non è necessario un'ulteriore visualizzazione disponibile per l'azione RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="0521a-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="0521a-180">Ecco il codice completo per la visualizzazione /ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="0521a-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="0521a-181">Per testarlo, è necessario essere in grado di aggiungere elementi al nostro carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="0521a-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="0521a-182">Verrà aggiornata la **dettagli archivio** visualizzazione per includere un pulsante "Aggiungi al carrello".</span><span class="sxs-lookup"><span data-stu-id="0521a-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="0521a-183">Mentre è attiva la, possiamo includere alcune delle informazioni aggiuntive che è stato aggiunto dopo è ultimo aggiornamento di questa visualizzazione: Genre, artista, prezzo e dell'album.</span><span class="sxs-lookup"><span data-stu-id="0521a-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="0521a-184">Il codice di visualizzazione Dettagli archivio aggiornato viene visualizzato come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="0521a-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="0521a-185">Ora è possibile fare clic tramite lo store e test aggiunta e rimozione di album da e verso il nostro carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="0521a-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="0521a-186">Eseguire l'applicazione e individuare l'indice dell'archivio.</span><span class="sxs-lookup"><span data-stu-id="0521a-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="0521a-187">Fare clic su un genere per visualizzare un elenco di album.</span><span class="sxs-lookup"><span data-stu-id="0521a-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="0521a-188">Facendo clic sul titolo di un Album ora mostra la visualizzazione dettagli Album aggiornata, incluso il pulsante "Aggiungi al carrello".</span><span class="sxs-lookup"><span data-stu-id="0521a-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="0521a-189">Fare clic sul pulsante "Aggiungi al carrello" Mostra la visualizzazione di indice carrello acquisti con l'elenco di riepilogo del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="0521a-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="0521a-190">Dopo il caricamento di carrello, è possibile fare clic sul pulsante Rimuovi dal relativo collegamento per visualizzare l'aggiornamento di Ajax al carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="0521a-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="0521a-191">È stata realizzata un lavoro carrello che consente agli utenti di annullare la registrazione aggiungere elementi al carrello degli acquisti.</span><span class="sxs-lookup"><span data-stu-id="0521a-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="0521a-192">Nella sezione seguente, in modo per registrare e completare il processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="0521a-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="0521a-193">[Precedente](mvc-music-store-part-7.md)
[Successivo](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="0521a-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
