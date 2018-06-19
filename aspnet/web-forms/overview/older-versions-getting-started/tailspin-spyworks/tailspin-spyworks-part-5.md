---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: Logica di Business | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 5 aggiunge una logica di business.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885081"
---
<a name="part-5-business-logic"></a><span data-ttu-id="a4a14-104">Parte 5: Logica di Business</span><span class="sxs-lookup"><span data-stu-id="a4a14-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="a4a14-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="a4a14-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="a4a14-106">Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="a4a14-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="a4a14-107">Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="a4a14-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="a4a14-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="a4a14-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="a4a14-109">Parte 5 aggiunge una logica di business.</span><span class="sxs-lookup"><span data-stu-id="a4a14-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="a4a14-110">Aggiunta di una logica di Business</span><span class="sxs-lookup"><span data-stu-id="a4a14-110">Adding Some Business Logic</span></span>

<span data-ttu-id="a4a14-111">Si vuole che la nostra esperienza acquisto sia disponibile ogni volta che un utente visita il sito web.</span><span class="sxs-lookup"><span data-stu-id="a4a14-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="a4a14-112">Visitatori sarà in grado di cercare e aggiungere elementi al carrello, anche se non sono registrati o registrati.</span><span class="sxs-lookup"><span data-stu-id="a4a14-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="a4a14-113">Quando sono pronti per l'estrazione essi verrà assegnato l'opzione di autenticazione e se non sono ancora membri saranno in grado di creare un account.</span><span class="sxs-lookup"><span data-stu-id="a4a14-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="a4a14-114">Ciò significa che è necessario implementare la logica per convertire il carrello acquisti da uno stato anonimo in uno stato "Registered User".</span><span class="sxs-lookup"><span data-stu-id="a4a14-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="a4a14-115">Si crea una directory denominata "Classi" quindi pulsante destro del mouse sulla cartella e creare un nuovo file "Class" denominato MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="a4a14-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="a4a14-116">Come accennato in precedenza si sarà l'estensione della classe che implementa la pagina MyShoppingCart.aspx lo faremo questo utilizzo. Costrutto potente "classe parziale" di NET.</span><span class="sxs-lookup"><span data-stu-id="a4a14-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="a4a14-117">La chiamata per il file MyShoppingCart.aspx.cf generata è simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="a4a14-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="a4a14-118">Si noti l'uso della parola chiave "parziale".</span><span class="sxs-lookup"><span data-stu-id="a4a14-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="a4a14-119">Il file di classe generata è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a4a14-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="a4a14-120">Si consentirà di unire il nostro implementazioni aggiungendo la parola chiave partial anche questo file.</span><span class="sxs-lookup"><span data-stu-id="a4a14-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="a4a14-121">Il nuovo file di classe è ora simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="a4a14-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="a4a14-122">Il primo metodo che saranno aggiunti alla classe il metodo "AddItem".</span><span class="sxs-lookup"><span data-stu-id="a4a14-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="a4a14-123">Si tratta del metodo che, in definitiva, verrà chiamato quando l'utente fa clic sui collegamenti di "Aggiunta di immagini" nelle pagine di elenco di prodotti e i dettagli sul prodotto.</span><span class="sxs-lookup"><span data-stu-id="a4a14-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="a4a14-124">Aggiungere la seguente utilizzando le istruzioni nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="a4a14-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="a4a14-125">E aggiungere questo metodo alla classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="a4a14-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="a4a14-126">Si sta usando LINQ to Entities per vedere se l'elemento è già nel carrello.</span><span class="sxs-lookup"><span data-stu-id="a4a14-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="a4a14-127">Se in tal caso, si aggiorna la quantità dell'ordine dell'elemento, in caso contrario è creare una nuova voce per l'elemento selezionato</span><span class="sxs-lookup"><span data-stu-id="a4a14-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="a4a14-128">Per chiamare questo metodo verrà implementata una pagina AddToCart.aspx che non solo il metodo della classe ma quindi visualizzati carrello acquisti un = dopo aver aggiunto l'elemento corrente.</span><span class="sxs-lookup"><span data-stu-id="a4a14-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="a4a14-129">Fare clic sul nome della soluzione in Esplora soluzioni e aggiungere e nuova pagina denominata AddToCart.aspx come è stato fatto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="a4a14-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="a4a14-130">Mentre è possibile utilizzare questa pagina per visualizzare i risultati intermedi come bassi problemi scorte e così via, nell'implementazione, la pagina verrà effettivamente non esegue il rendering, ma piuttosto chiamare la logica di "Aggiungi" e reindirizzare.</span><span class="sxs-lookup"><span data-stu-id="a4a14-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="a4a14-131">A questo scopo verrà aggiunto il codice seguente alla pagina\_evento di caricamento.</span><span class="sxs-lookup"><span data-stu-id="a4a14-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="a4a14-132">Si noti che stiamo cercando il prodotto da aggiungere al carrello acquisti di un parametro di stringa di query e la chiamata al metodo AddItem la classe.</span><span class="sxs-lookup"><span data-stu-id="a4a14-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="a4a14-133">Supponendo che non gli errori vengono rilevati il controllo viene passato alla pagina SHoppingCart.aspx verrà completamente implementato successivamente.</span><span class="sxs-lookup"><span data-stu-id="a4a14-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="a4a14-134">Se deve essere un errore, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="a4a14-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="a4a14-135">Attualmente non è stata ancora implementata un gestore errori globale diventano non gestita per l'applicazione di questa eccezione, ma ci consentirà di risolvere questo a breve.</span><span class="sxs-lookup"><span data-stu-id="a4a14-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="a4a14-136">Si noti inoltre l'utilizzo dell'istruzione Debug.Fail() (disponibili tramite `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="a4a14-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="a4a14-137">È l'applicazione è in esecuzione nel debugger, questo metodo verrà visualizzata una finestra di dialogo dettagliata con informazioni sullo stato delle applicazioni insieme al messaggio di errore che si specifica.</span><span class="sxs-lookup"><span data-stu-id="a4a14-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="a4a14-138">Quando in esecuzione nell'ambiente di produzione l'istruzione Debug.Fail() viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="a4a14-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="a4a14-139">Si noterà nel codice sopra una chiamata a un metodo in ai nomi di classe di carrello acquisti "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="a4a14-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="a4a14-140">Aggiungere il codice per implementare il metodo come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a4a14-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="a4a14-141">Si noti che è stato aggiunto anche pulsanti di aggiornamento e l'estrazione e un'etichetta in cui è possibile visualizzare il carrello "totale".</span><span class="sxs-lookup"><span data-stu-id="a4a14-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="a4a14-142">È ora possibile aggiungere elementi al nostro carrello acquisti ma non è stata implementata la logica per visualizzare il carrello dopo l'aggiunta di un prodotto.</span><span class="sxs-lookup"><span data-stu-id="a4a14-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="a4a14-143">In tal caso, nella pagina MyShoppingCart.aspx verrà aggiunto un controllo EntityDataSource e un controllo GridVire come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a4a14-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="a4a14-144">Chiamare il form nella finestra di progettazione in modo che è possibile fare doppio clic sul pulsante di aggiornamento del carrello e generare il gestore eventi click che viene specificato nella dichiarazione nel markup.</span><span class="sxs-lookup"><span data-stu-id="a4a14-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="a4a14-145">I dettagli verrà implementato in un secondo momento, ma in questo modo verrà compilata ed eseguita l'applicazione senza errori di segnalare il problema.</span><span class="sxs-lookup"><span data-stu-id="a4a14-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="a4a14-146">Quando si esegue l'applicazione e aggiungere un elemento al carrello acquisti verrà visualizzato.</span><span class="sxs-lookup"><span data-stu-id="a4a14-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="a4a14-147">Si noti che è stato deviato dalla visualizzazione griglia "default" mediante l'implementazione di tre colonne personalizzate.</span><span class="sxs-lookup"><span data-stu-id="a4a14-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="a4a14-148">Il primo è un oggetto modificabile, il campo "Associato" per la quantità:</span><span class="sxs-lookup"><span data-stu-id="a4a14-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="a4a14-149">Alla successiva è una colonna "calcolata" che visualizza il totale della voce (l'elemento di costo per la quantità da ordinare):</span><span class="sxs-lookup"><span data-stu-id="a4a14-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="a4a14-150">Infine, è disponibile una colonna personalizzata che contiene un controllo casella di controllo che l'utente utilizzerà per indicare che l'elemento deve essere rimosso dal grafico acquisto.</span><span class="sxs-lookup"><span data-stu-id="a4a14-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="a4a14-151">Come si può notare, l'ordine di riga totale è vuoto a questo punto aggiungere logica per calcolare il totale di ordini.</span><span class="sxs-lookup"><span data-stu-id="a4a14-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="a4a14-152">È prima viene implementato un metodo "GetTotal" alla classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="a4a14-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="a4a14-153">Nel file MyShoppingCart.cs aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="a4a14-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="a4a14-154">Quindi, nella pagina\_gestore di eventi di caricamento si sarà possibile chiamare il metodo GetTotal.</span><span class="sxs-lookup"><span data-stu-id="a4a14-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="a4a14-155">Un test per vedere se il carrello acquisti è vuoto e regolare la visualizzazione di conseguenza se verrà aggiunto allo stesso tempo.</span><span class="sxs-lookup"><span data-stu-id="a4a14-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="a4a14-156">A questo punto il carrello acquisti è vuoto si ottiene questo:</span><span class="sxs-lookup"><span data-stu-id="a4a14-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="a4a14-157">In caso contrario, è e il totale.</span><span class="sxs-lookup"><span data-stu-id="a4a14-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="a4a14-158">Tuttavia, questa pagina non è ancora completa.</span><span class="sxs-lookup"><span data-stu-id="a4a14-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="a4a14-159">È necessario logica aggiuntiva per ricalcolare il carrello acquisti, rimuovendo gli elementi contrassegnati per la rimozione e determinando i nuovi valori quantità come alcune sia stato modificato nella griglia dall'utente.</span><span class="sxs-lookup"><span data-stu-id="a4a14-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="a4a14-160">Consente di aggiungere un metodo "RemoveItem" per la classe di carrello in MyShoppingCart.cs per gestire il caso, quando si contrassegna un elemento per la rimozione.</span><span class="sxs-lookup"><span data-stu-id="a4a14-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="a4a14-161">Ora si ad un metodo per gestire il caso, quando un utente cambia semplicemente la qualità verrà ordinata in GridView.</span><span class="sxs-lookup"><span data-stu-id="a4a14-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="a4a14-162">Con le funzionalità base di installazione e aggiornamento sul posto è possibile implementare la logica che effettivamente Aggiorna carrello nel database.</span><span class="sxs-lookup"><span data-stu-id="a4a14-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="a4a14-163">(In MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="a4a14-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="a4a14-164">È possibile notare che questo metodo prevede due parametri.</span><span class="sxs-lookup"><span data-stu-id="a4a14-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="a4a14-165">Uno è il carrello acquisti Id e l'altro è una matrice di oggetti di tipo definito dall'utente.</span><span class="sxs-lookup"><span data-stu-id="a4a14-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="a4a14-166">In modo da ridurre al minimo la dipendenza del nostro logica sulle specifiche di interfaccia utente, viene definito una struttura di dati che è possibile utilizzare per passare gli elementi del carrello acquisti al codice senza il necessità di accedere direttamente ai controllo GridView (metodo).</span><span class="sxs-lookup"><span data-stu-id="a4a14-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="a4a14-167">Nel file MyShoppingCart.aspx.cs è possibile usare questa struttura in questo gestore dell'evento fare clic su pulsante di aggiornamento come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a4a14-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="a4a14-168">Si noti che oltre ad aggiornare il carrello verrà aggiornato anche il totale di carrello.</span><span class="sxs-lookup"><span data-stu-id="a4a14-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="a4a14-169">Si noti con particolare interesse questa riga di codice:</span><span class="sxs-lookup"><span data-stu-id="a4a14-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="a4a14-170">GetValues() è una funzione di supporto speciale che verrà implementato in MyShoppingCart.aspx.cs come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a4a14-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="a4a14-171">Ciò fornisce un modo pulito per accedere ai valori degli elementi associati in del controllo GridView.</span><span class="sxs-lookup"><span data-stu-id="a4a14-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="a4a14-172">Poiché non è associato il controllo casella di controllo "Rimuovi elemento" sarà accedere tramite il metodo FindControl().</span><span class="sxs-lookup"><span data-stu-id="a4a14-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="a4a14-173">In questa fase di sviluppo del progetto è stiamo preparativi per l'implementazione del processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="a4a14-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="a4a14-174">Prima di procedere si utilizzare Visual Studio per generare il database delle appartenenze e aggiungere un utente nel repository di appartenenza.</span><span class="sxs-lookup"><span data-stu-id="a4a14-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a4a14-175">[Precedente](tailspin-spyworks-part-4.md)
> [Successivo](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="a4a14-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
