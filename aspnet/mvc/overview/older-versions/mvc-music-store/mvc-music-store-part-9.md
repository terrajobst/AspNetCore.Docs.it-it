---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: Estrazione e registrazione | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 9 riguarda la registrazione e estrazione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e7e83b70f2508b6dfc0c078b992747a76e4d0ff2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="e4276-104">Parte 9: Estrazione e registrazione</span><span class="sxs-lookup"><span data-stu-id="e4276-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="e4276-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="e4276-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="e4276-106">L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="e4276-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="e4276-107">L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="e4276-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="e4276-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio.</span><span class="sxs-lookup"><span data-stu-id="e4276-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="e4276-109">Parte 9 riguarda la registrazione e estrazione.</span><span class="sxs-lookup"><span data-stu-id="e4276-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="e4276-110">In questa sezione, che verrà creata una CheckoutController che raccoglierà informazioni di pagamento e indirizzo dell'acquirente.</span><span class="sxs-lookup"><span data-stu-id="e4276-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="e4276-111">Si richiederà agli utenti di registrare il sito prima dell'estrazione, in modo che il controller richiederà l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="e4276-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="e4276-112">Gli utenti si sposterà il processo di estrazione dal carrello acquisti facendo clic sul pulsante "Checkpoint".</span><span class="sxs-lookup"><span data-stu-id="e4276-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="e4276-113">Se l'utente non è connesso, verrà richiesto di.</span><span class="sxs-lookup"><span data-stu-id="e4276-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="e4276-114">Dopo l'accesso ha esito positivo, l'utente viene visualizzata la vista indirizzo e pagamento.</span><span class="sxs-lookup"><span data-stu-id="e4276-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="e4276-115">Dopo che il modulo compilato e inviato l'ordine, essi verranno visualizzati la schermata di conferma dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="e4276-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="e4276-116">Tenta di visualizzare un ordine non esistente o un ordine che non appartiene all'utente verrà visualizzato l'errore, vedere.</span><span class="sxs-lookup"><span data-stu-id="e4276-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="e4276-117">La migrazione il carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="e4276-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="e4276-118">Durante il processo di acquisto è anonimo, quando l'utente fa clic sul pulsante di estrazione, sarà necessario per registrare e account di accesso.</span><span class="sxs-lookup"><span data-stu-id="e4276-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="e4276-119">Gli utenti si aspetteranno che si manterrà le informazioni relative al carrello acquisti tra visite, pertanto è necessario associare le informazioni relative al carrello acquisti un utente al momento della compilazione di registrazione o l'account di accesso.</span><span class="sxs-lookup"><span data-stu-id="e4276-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="e4276-120">Questo è effettivamente molto semplice, come la classe ShoppingCart dispone già di un metodo quale assocerà tutti gli elementi nel carrello corrente con un nome utente.</span><span class="sxs-lookup"><span data-stu-id="e4276-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="e4276-121">È semplicemente necessario chiamare questo metodo quando un utente ha completato la registrazione o l'account di accesso.</span><span class="sxs-lookup"><span data-stu-id="e4276-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="e4276-122">Aprire il **AccountController** classe che viene aggiunto quando è durante la configurazione di appartenenza e l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="e4276-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="e4276-123">Aggiungere un utilizzando l'istruzione che fa riferimento MvcMusicStore.Models, quindi aggiungere il metodo MigrateShoppingCart seguente:</span><span class="sxs-lookup"><span data-stu-id="e4276-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="e4276-124">Successivamente, modificare l'azione di post di accesso per chiamare MigrateShoppingCart dopo che l'utente è stato convalidato, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="e4276-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="e4276-125">Apportare la stessa modifica al Registro di post-azione, immediatamente dopo che l'account utente è stato creato:</span><span class="sxs-lookup"><span data-stu-id="e4276-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="e4276-126">È tutto - ora un carrello acquisti anonimo verrà trasferito automaticamente a un account utente al momento dopo la corretta registrazione o l'account di accesso.</span><span class="sxs-lookup"><span data-stu-id="e4276-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="e4276-127">Creazione di CheckoutController</span><span class="sxs-lookup"><span data-stu-id="e4276-127">Creating the CheckoutController</span></span>

<span data-ttu-id="e4276-128">Pulsante destro del mouse sulla cartella controller e aggiungere un nuovo Controller al progetto denominato CheckoutController utilizzando il modello di controller vuoto.</span><span class="sxs-lookup"><span data-stu-id="e4276-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="e4276-129">In primo luogo, aggiungere l'attributo Authorize sopra la dichiarazione di classe Controller richiedere agli utenti di registrare prima dell'estrazione:</span><span class="sxs-lookup"><span data-stu-id="e4276-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="e4276-130">*Nota: Questa operazione è simile alla modifica che è apportate in precedenza il StoreManagerController, ma in tal caso l'attributo Authorize necessario che l'utente appartenga a un ruolo di amministratore. Nel Controller di estrazione, è in corso richiedendo all'utente di connettersi ma non richiedono che sono amministratori.*</span><span class="sxs-lookup"><span data-stu-id="e4276-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="e4276-131">Per ragioni di semplicità, è non essere gestiscono le informazioni di pagamento in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e4276-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="e4276-132">Al contrario, si consente agli utenti di estrarre utilizzando un codice promozionale.</span><span class="sxs-lookup"><span data-stu-id="e4276-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="e4276-133">Verrà archiviato il codice promozionale utilizza una costante denominata codice promozione.</span><span class="sxs-lookup"><span data-stu-id="e4276-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="e4276-134">Come StoreController, si dichiara un campo per contenere un'istanza della classe MusicStoreEntities, denominata storeDB.</span><span class="sxs-lookup"><span data-stu-id="e4276-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="e4276-135">Per rendere usano la classe MusicStoreEntities, sarà necessario aggiungere un tramite l'istruzione per lo spazio dei nomi MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="e4276-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="e4276-136">Di seguito è riportata nella parte superiore di questo controller di estrazione.</span><span class="sxs-lookup"><span data-stu-id="e4276-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="e4276-137">Il CheckoutController saranno disponibili le seguenti azioni controller:</span><span class="sxs-lookup"><span data-stu-id="e4276-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="e4276-138">**AddressAndPayment (GET method)** verrà visualizzato un form per consentire all'utente di immettere le informazioni.</span><span class="sxs-lookup"><span data-stu-id="e4276-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="e4276-139">**AddressAndPayment (metodo POST)** verrà convalidare l'input ed elaborare l'ordine.</span><span class="sxs-lookup"><span data-stu-id="e4276-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="e4276-140">**Completa** verrà visualizzato dopo che un utente ha completato correttamente il processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="e4276-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="e4276-141">Questa visualizzazione includerà il numero di ordine dell'utente, conferma.</span><span class="sxs-lookup"><span data-stu-id="e4276-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="e4276-142">In primo luogo, rinominare l'azione del controller indice (che è stato generato quando abbiamo creato il controller) per AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="e4276-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="e4276-143">Questa azione del controller Visualizza solo il modulo di estrazione, e pertanto non richiede le informazioni sul modello.</span><span class="sxs-lookup"><span data-stu-id="e4276-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="e4276-144">Il metodo POST AddressAndPayment seguiranno lo stesso modello, abbiamo utilizzato il StoreManagerController: tenterà di accettare l'invio del modulo e completare l'ordine e verrà nuovamente visualizzato il form in caso di errore.</span><span class="sxs-lookup"><span data-stu-id="e4276-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="e4276-145">Dopo la convalida dell'input del form soddisfa i requisiti di convalida per un ordine, si verificherà direttamente il valore di modulo di codice promozione.</span><span class="sxs-lookup"><span data-stu-id="e4276-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="e4276-146">Presupponendo che le informazioni sono corrette, che si salverà le informazioni aggiornate con l'ordine, indicare l'oggetto ShoppingCart per completare il processo dell'ordine e il reindirizzamento all'azione di completamento.</span><span class="sxs-lookup"><span data-stu-id="e4276-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="e4276-147">Al completamento del processo di estrazione, gli utenti verranno reindirizzati all'azione del controller completo.</span><span class="sxs-lookup"><span data-stu-id="e4276-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="e4276-148">Questa azione esegue un semplice controllo per verificare che l'ordine effettivamente appartengono all'utente connesso prima di visualizzare il numero dell'ordine come una conferma.</span><span class="sxs-lookup"><span data-stu-id="e4276-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="e4276-149">*Nota: La visualizzazione di errore creata automaticamente per noi nella cartella /Views/Shared quando abbiamo iniziato il progetto.*</span><span class="sxs-lookup"><span data-stu-id="e4276-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="e4276-150">Il codice CheckoutController completo è il seguente:</span><span class="sxs-lookup"><span data-stu-id="e4276-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="e4276-151">Aggiunta della visualizzazione AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="e4276-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="e4276-152">A questo punto, creare la vista AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="e4276-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="e4276-153">Fare clic su una delle azioni AddressAndPayment controller e aggiungere una visualizzazione denominata AddressAndPayment è fortemente tipizzata come un ordine che utilizza il modello di modifica, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e4276-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="e4276-154">Questa vista renderà utilizzare due delle tecniche di cui è stato esaminato durante la creazione della visualizzazione StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="e4276-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="e4276-155">Si utilizzerà Html.EditorForModel() per visualizzare i campi del form per il modello di ordine</span><span class="sxs-lookup"><span data-stu-id="e4276-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="e4276-156">Verranno utilizzate le regole di convalida utilizzando una classe Order con gli attributi di convalida</span><span class="sxs-lookup"><span data-stu-id="e4276-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="e4276-157">Si inizierà aggiornando il codice del modulo per l'utilizzo di Html.EditorForModel(), seguito da una casella di testo aggiuntiva per il codice promozionale.</span><span class="sxs-lookup"><span data-stu-id="e4276-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="e4276-158">Il codice completo per la visualizzazione AddressAndPayment è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e4276-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="e4276-159">Definizione delle regole di convalida per l'ordine</span><span class="sxs-lookup"><span data-stu-id="e4276-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="e4276-160">Dopo che la vista è impostato, si imposterà le regole di convalida per il modello di ordine come è stato fatto in precedenza per il modello di Album.</span><span class="sxs-lookup"><span data-stu-id="e4276-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="e4276-161">Fare doppio clic sulla cartella modelli e aggiungere una classe denominata ordine.</span><span class="sxs-lookup"><span data-stu-id="e4276-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="e4276-162">Oltre agli attributi di convalida che è stata utilizzata in precedenza per Album, anche utilizzeremo un'espressione regolare per convalidare l'indirizzo di posta elettronica dell'utente.</span><span class="sxs-lookup"><span data-stu-id="e4276-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="e4276-163">Il tentativo di inviare il form con mancante o informazioni non valide verranno visualizzati il messaggio di errore utilizzando la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="e4276-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="e4276-164">Passare alla procedura guidata che abbiamo realizzato la maggior parte del lavoro disco rigido per il processo di estrazione. sono disponibili solo pochi termina in e probabilità per completare.</span><span class="sxs-lookup"><span data-stu-id="e4276-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="e4276-165">È necessario aggiungere due visualizzazioni semplici, e sono necessarie svolgere il passaggio di informazioni relative al carrello durante il processo di accesso.</span><span class="sxs-lookup"><span data-stu-id="e4276-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="e4276-166">Aggiunta della visualizzazione completa di estrazione</span><span class="sxs-lookup"><span data-stu-id="e4276-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="e4276-167">La visualizzazione completa di estrazione è piuttosto semplice, ma è sufficiente visualizzare l'ID di ordine.</span><span class="sxs-lookup"><span data-stu-id="e4276-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="e4276-168">Fare doppio clic sull'azione di completamento controller e aggiungere una visualizzazione denominata Complete fortemente tipizzato come valore int.</span><span class="sxs-lookup"><span data-stu-id="e4276-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="e4276-169">Ora verrà aggiornato il codice di visualizzazione per visualizzare l'ID dell'ordine, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e4276-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="e4276-170">Aggiornare la visualizzazione dell'errore</span><span class="sxs-lookup"><span data-stu-id="e4276-170">Updating The Error view</span></span>

<span data-ttu-id="e4276-171">Il modello predefinito include una visualizzazione di errore nella cartella views condiviso in modo che possa essere riutilizzato altrove nel sito.</span><span class="sxs-lookup"><span data-stu-id="e4276-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="e4276-172">Questa vista di errore contiene un errore molto semplice e non usa il Layout, il sito in modo che verrà aggiornata.</span><span class="sxs-lookup"><span data-stu-id="e4276-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="e4276-173">Poiché si tratta di una pagina di errore generico, il contenuto è molto semplice.</span><span class="sxs-lookup"><span data-stu-id="e4276-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="e4276-174">Verrà incluso un messaggio e un collegamento per passare alla pagina precedente nella cronologia se l'utente desidera riprovare l'azione.</span><span class="sxs-lookup"><span data-stu-id="e4276-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="e4276-175">[Precedente](mvc-music-store-part-8.md)
> [Successivo](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="e4276-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
