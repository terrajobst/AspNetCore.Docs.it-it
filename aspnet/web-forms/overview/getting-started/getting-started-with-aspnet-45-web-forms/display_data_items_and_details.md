---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Visualizzare i dati degli elementi e i dettagli | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.7 e Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: acc2f8e78375ef0455d467e2af750ecbee623224
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396225"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="584c0-103">Elementi di dati visualizzati e dettagli</span><span class="sxs-lookup"><span data-stu-id="584c0-103">Display data items and details</span></span>
====================
<span data-ttu-id="584c0-104">da [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="584c0-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="584c0-105">Questa serie di esercitazioni illustra le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.7 e Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="584c0-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="584c0-106">In questa esercitazione si apprenderà come visualizzare gli elementi di dati e i dettagli dell'elemento dati con Entity Framework Code First e Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="584c0-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="584c0-107">Questa esercitazione si basa sull'esercitazione precedente "Navigazione dell'interfaccia utente e" come parte della serie di esercitazioni di Wingtip giocattoli Store.</span><span class="sxs-lookup"><span data-stu-id="584c0-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="584c0-108">Dopo aver completato questa esercitazione, si noterà i prodotti nel *ProductsList.aspx* pagina e i dettagli di un prodotto nel *ProductDetails.aspx* pagina.</span><span class="sxs-lookup"><span data-stu-id="584c0-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="584c0-109">Si imparerà a eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="584c0-109">You'll learn how to:</span></span>

- <span data-ttu-id="584c0-110">Aggiungere un controllo dati per visualizzare i prodotti dal database</span><span class="sxs-lookup"><span data-stu-id="584c0-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="584c0-111">Connettere un controllo dei dati per i dati selezionati</span><span class="sxs-lookup"><span data-stu-id="584c0-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="584c0-112">Aggiungere un controllo dati per visualizzare i dettagli sul prodotto dal database</span><span class="sxs-lookup"><span data-stu-id="584c0-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="584c0-113">Recuperare un valore dalla stringa di query e usare tale valore per limitare i dati recuperati dal database</span><span class="sxs-lookup"><span data-stu-id="584c0-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="584c0-114">Funzionalità introdotte in questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="584c0-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="584c0-115">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="584c0-115">Model binding</span></span>
- <span data-ttu-id="584c0-116">Provider di valori</span><span class="sxs-lookup"><span data-stu-id="584c0-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="584c0-117">Aggiungere un controllo dei dati</span><span class="sxs-lookup"><span data-stu-id="584c0-117">Add a data control</span></span>

<span data-ttu-id="584c0-118">È possibile usare alcune opzioni diverse per associare dati a un controllo server.</span><span class="sxs-lookup"><span data-stu-id="584c0-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="584c0-119">I più comuni includono:</span><span class="sxs-lookup"><span data-stu-id="584c0-119">The most common include:</span></span>

 * <span data-ttu-id="584c0-120">Aggiunta di un controllo origine dati</span><span class="sxs-lookup"><span data-stu-id="584c0-120">Adding a data source control</span></span>
 * <span data-ttu-id="584c0-121">Aggiunta manuale di codice</span><span class="sxs-lookup"><span data-stu-id="584c0-121">Adding code by hand</span></span>
 * <span data-ttu-id="584c0-122">Tramite l'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="584c0-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="584c0-123">Usare un controllo origine dati per associare i dati</span><span class="sxs-lookup"><span data-stu-id="584c0-123">Use a data source control to bind data</span></span>

<span data-ttu-id="584c0-124">Aggiunta di un controllo origine dati consente di collegare il controllo origine dati al controllo che visualizza i dati.</span><span class="sxs-lookup"><span data-stu-id="584c0-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="584c0-125">Con questo approccio, è possibile in modo dichiarativo, anziché connettersi a livello di codice i controlli lato server alle origini dati.</span><span class="sxs-lookup"><span data-stu-id="584c0-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="584c0-126">Codice manualmente per associare i dati</span><span class="sxs-lookup"><span data-stu-id="584c0-126">Code by hand to bind data</span></span>

<span data-ttu-id="584c0-127">Codifica manuale prevede:</span><span class="sxs-lookup"><span data-stu-id="584c0-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="584c0-128">Lettura di un valore</span><span class="sxs-lookup"><span data-stu-id="584c0-128">Reading a value</span></span>
2. <span data-ttu-id="584c0-129">Controllare se è null</span><span class="sxs-lookup"><span data-stu-id="584c0-129">Checking if it's null</span></span>
3. <span data-ttu-id="584c0-130">Conversione in un tipo appropriato</span><span class="sxs-lookup"><span data-stu-id="584c0-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="584c0-131">Controllo operazioni riuscite di conversione</span><span class="sxs-lookup"><span data-stu-id="584c0-131">Checking conversion success</span></span>
5. <span data-ttu-id="584c0-132">Utilizzando il valore nella query</span><span class="sxs-lookup"><span data-stu-id="584c0-132">Using the value in the query</span></span> 

<span data-ttu-id="584c0-133">Questo approccio consente di avere il controllo completo per la logica di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="584c0-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="584c0-134">Usare l'associazione di modelli per associare i dati</span><span class="sxs-lookup"><span data-stu-id="584c0-134">Use model binding to bind data</span></span>

<span data-ttu-id="584c0-135">Associazione di modelli consente di associare i risultati con molto meno codice e ti offre la possibilità di riusare le funzionalità in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="584c0-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="584c0-136">Semplifica l'utilizzo con la logica di accesso ai dati incentrato sul codice garantendo comunque un framework completo e associazione dati.</span><span class="sxs-lookup"><span data-stu-id="584c0-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="584c0-137">Visualizzare i prodotti</span><span class="sxs-lookup"><span data-stu-id="584c0-137">Display products</span></span>

<span data-ttu-id="584c0-138">In questa esercitazione si userà l'associazione di modelli per associare i dati.</span><span class="sxs-lookup"><span data-stu-id="584c0-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="584c0-139">Per configurare un controllo dati per utilizzare l'associazione di modelli per selezionare i dati, si imposta il controllo `SelectMethod` proprietà a un nome di metodo nel codice della pagina.</span><span class="sxs-lookup"><span data-stu-id="584c0-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="584c0-140">Il controllo dei dati chiama il metodo momento adeguato del ciclo di vita della pagina e associa automaticamente i dati restituiti.</span><span class="sxs-lookup"><span data-stu-id="584c0-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="584c0-141">Non è necessario chiamare in modo esplicito il `DataBind` (metodo).</span><span class="sxs-lookup"><span data-stu-id="584c0-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="584c0-142">Nelle **Esplora soluzioni**aprire *ProductList. aspx*.</span><span class="sxs-lookup"><span data-stu-id="584c0-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="584c0-143">Sostituire il markup esistente con questo codice:</span><span class="sxs-lookup"><span data-stu-id="584c0-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="584c0-144">Questo codice Usa un **ListView** controllo denominato `productList` per visualizzare i prodotti.</span><span class="sxs-lookup"><span data-stu-id="584c0-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="584c0-145">Con stili e modelli, è possibile definire come il **ListView** controllo Visualizza i dati.</span><span class="sxs-lookup"><span data-stu-id="584c0-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="584c0-146">È utile per i dati in una struttura ripetuta.</span><span class="sxs-lookup"><span data-stu-id="584c0-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="584c0-147">Anche se ciò **ListView** viene visualizzata semplicemente i dati del database, è possibile anche, senza codice, consentire agli utenti di modificare, inserire ed eliminare dati e di ordinamento e paging dei dati.</span><span class="sxs-lookup"><span data-stu-id="584c0-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="584c0-148">Impostando il `ItemType` proprietà nel **ListView** controllare, l'espressione di associazione dati `Item` è disponibile e il controllo diventa fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="584c0-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="584c0-149">Come indicato nell'esercitazione precedente, è possibile selezionare i dettagli dell'oggetto elemento con la tecnologia IntelliSense, ad esempio specificando la `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="584c0-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Visualizzare i dati gli elementi e i dettagli - IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="584c0-151">Si usa anche l'associazione di modelli per specificare un `SelectMethod` valore.</span><span class="sxs-lookup"><span data-stu-id="584c0-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="584c0-152">Questo valore (`GetProducts`) corrisponde al metodo si aggiungeranno al codice indietro per visualizzare i prodotti nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="584c0-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="584c0-153">Aggiungere codice per visualizzare i prodotti</span><span class="sxs-lookup"><span data-stu-id="584c0-153">Add code to display products</span></span>

<span data-ttu-id="584c0-154">In questo passaggio si aggiungerà codice per popolare la **ListView** controllo con i dati prodotti dal database.</span><span class="sxs-lookup"><span data-stu-id="584c0-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="584c0-155">Il codice supporta che mostra tutti i prodotti e singola categoria.</span><span class="sxs-lookup"><span data-stu-id="584c0-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="584c0-156">Nelle **Esplora soluzioni**, fare doppio clic su *ProductList. aspx* e quindi selezionare **Visualizza codice**.</span><span class="sxs-lookup"><span data-stu-id="584c0-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="584c0-157">Sostituire il codice esistente nel *ProductList.aspx.cs* file con questo:</span><span class="sxs-lookup"><span data-stu-id="584c0-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="584c0-158">Questo codice viene illustrato il `GetProducts` metodo che il **ListView** del controllo `ItemType` riferimenti alla proprietà nella *ProductList. aspx* pagina.</span><span class="sxs-lookup"><span data-stu-id="584c0-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="584c0-159">Per limitare i risultati a una categoria di database specifico, il codice imposta la `categoryId` valore del valore di stringa di query passato al *ProductList. aspx* pagina quando la *ProductList. aspx* pagina è ci si sposta su.</span><span class="sxs-lookup"><span data-stu-id="584c0-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="584c0-160">Il `QueryStringAttribute` classe la `System.Web.ModelBinding` dello spazio dei nomi viene usato per recuperare il valore della variabile di stringa della query `id`.</span><span class="sxs-lookup"><span data-stu-id="584c0-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="584c0-161">Ciò indica l'associazione di modelli per tentare di associare un valore di stringa di query di `categoryId` parametro in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="584c0-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="584c0-162">Quando una categoria valida viene passata come stringa di query alla pagina, i risultati della query sono limitati a tali prodotti nel database che corrispondono al `categoryId` valore.</span><span class="sxs-lookup"><span data-stu-id="584c0-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="584c0-163">Ad esempio, se il *ProductsList.aspx* URL della pagina è questo:</span><span class="sxs-lookup"><span data-stu-id="584c0-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="584c0-164">Nella pagina vengono visualizzati solo i prodotti in cui il `categoryId` è uguale a `1`.</span><span class="sxs-lookup"><span data-stu-id="584c0-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="584c0-165">Tutti i prodotti vengono visualizzati se nessuna stringa di query viene incluso durante la *ProductList. aspx* pagina viene definita.</span><span class="sxs-lookup"><span data-stu-id="584c0-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="584c0-166">Le origini dei valori per questi metodi sono dette *valore provider* (ad esempio *QueryString*), e gli attributi di parametro che indicano quali provider di valori da utilizzare sono dette *attributi di provider di valore* (ad esempio `id`).</span><span class="sxs-lookup"><span data-stu-id="584c0-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="584c0-167">ASP.NET include i provider di valori e gli attributi corrispondenti per tutte le origini tipiche dell'input utente in un'applicazione Web Form, ad esempio la stringa di query, cookie, i valori del form, controlli, lo stato di visualizzazione, lo stato della sessione e le proprietà del profilo.</span><span class="sxs-lookup"><span data-stu-id="584c0-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="584c0-168">È anche possibile scrivere provider di valori personalizzati.</span><span class="sxs-lookup"><span data-stu-id="584c0-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="584c0-169">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="584c0-169">Run the application</span></span>

<span data-ttu-id="584c0-170">Eseguire ora l'applicazione per visualizzare tutti i prodotti o i prodotti della categoria.</span><span class="sxs-lookup"><span data-stu-id="584c0-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="584c0-171">Premere **F5** all'interno di Visual Studio per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="584c0-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="584c0-172">Il browser si apre e Mostra le *default. aspx* pagina.</span><span class="sxs-lookup"><span data-stu-id="584c0-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="584c0-173">Selezionare **automobili** dal menu di navigazione di categoria prodotto.</span><span class="sxs-lookup"><span data-stu-id="584c0-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="584c0-174">Il *ProductList. aspx* pagina viene visualizzato che mostra solo **automobili** prodotti di categoria.</span><span class="sxs-lookup"><span data-stu-id="584c0-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="584c0-175">Più avanti in questa esercitazione, verranno visualizzati i dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="584c0-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Visualizzare i dati gli elementi e i dettagli - automobili](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="584c0-177">Selezionare **prodotti** dal menu di navigazione nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="584c0-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="584c0-178">Anche in questo caso, il *ProductList. aspx* pagina viene visualizzata, ma questa volta Mostra l'elenco completo di prodotti.</span><span class="sxs-lookup"><span data-stu-id="584c0-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Visualizzare i dati gli elementi e i dettagli - prodotti](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="584c0-180">Chiudere il browser e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="584c0-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="584c0-181">Aggiungere un controllo dei dati per visualizzare i dettagli sul prodotto</span><span class="sxs-lookup"><span data-stu-id="584c0-181">Add a data control to display product details</span></span>

<span data-ttu-id="584c0-182">Successivamente, si modificherà il markup nel *ProductDetails.aspx* pagina che è stato aggiunto nell'esercitazione precedente per visualizzare le informazioni di prodotto specifico.</span><span class="sxs-lookup"><span data-stu-id="584c0-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="584c0-183">Nelle **Esplora soluzioni**aprire *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="584c0-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="584c0-184">Sostituire il markup esistente con questo codice:</span><span class="sxs-lookup"><span data-stu-id="584c0-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="584c0-185">Questo codice Usa un **FormView** controllo per visualizzare i dettagli sul prodotto specifico.</span><span class="sxs-lookup"><span data-stu-id="584c0-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="584c0-186">Questo markup Usa i metodi, ad esempio i metodi utilizzati per visualizzare i dati nella *ProductList. aspx* pagina.</span><span class="sxs-lookup"><span data-stu-id="584c0-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="584c0-187">Il **FormView** controllo viene usato per visualizzare un singolo record alla volta da un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="584c0-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="584c0-188">Quando si usa la **FormView** (controllo), è creare modelli per visualizzare e modificare i valori associati a dati.</span><span class="sxs-lookup"><span data-stu-id="584c0-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="584c0-189">Questi modelli contengono controlli, espressioni di associazione, e formattazione che definiscono l'aspetto e funzionalità del modulo.</span><span class="sxs-lookup"><span data-stu-id="584c0-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="584c0-190">Il markup precedente la connessione al database richiede codice aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="584c0-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="584c0-191">Nelle **Esplora soluzioni**, fare doppio clic su *ProductDetails.aspx* e quindi fare clic su **Visualizza codice**.</span><span class="sxs-lookup"><span data-stu-id="584c0-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="584c0-192">Il *ProductDetails.aspx.cs* file viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="584c0-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="584c0-193">Sostituire il codice esistente con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="584c0-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="584c0-194">Questo codice cerca un "`productID`" valore di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="584c0-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="584c0-195">Se viene trovato un valore di stringa di query valida, viene visualizzato il prodotto corrisponda.</span><span class="sxs-lookup"><span data-stu-id="584c0-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="584c0-196">Se non viene trovata la stringa di query o il relativo valore non è valido, non viene visualizzato alcun prodotto.</span><span class="sxs-lookup"><span data-stu-id="584c0-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="584c0-197">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="584c0-197">Run the application</span></span>

<span data-ttu-id="584c0-198">A questo punto è possibile eseguire l'applicazione per visualizzare un singolo prodotto visualizzato basato su ID prodotto.</span><span class="sxs-lookup"><span data-stu-id="584c0-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="584c0-199">Premere **F5** all'interno di Visual Studio per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="584c0-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="584c0-200">Il browser si apre e Mostra le *default. aspx* pagina.</span><span class="sxs-lookup"><span data-stu-id="584c0-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="584c0-201">Selezionare **evolveranno** dal menu di navigazione di categoria.</span><span class="sxs-lookup"><span data-stu-id="584c0-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="584c0-202">Il *ProductList. aspx* viene visualizzata la pagina.</span><span class="sxs-lookup"><span data-stu-id="584c0-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="584c0-203">Selezionare **barca Paper** dall'elenco dei prodotti.</span><span class="sxs-lookup"><span data-stu-id="584c0-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="584c0-204">Il *ProductDetails.aspx* viene visualizzata la pagina.</span><span class="sxs-lookup"><span data-stu-id="584c0-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Visualizzare i dati gli elementi e i dettagli - prodotti](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="584c0-206">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="584c0-206">Close the browser.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="584c0-207">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="584c0-207">Additional resources</span></span>

[<span data-ttu-id="584c0-208">Il recupero e visualizzazione dei dati con l'associazione di modelli e web form</span><span class="sxs-lookup"><span data-stu-id="584c0-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="584c0-209">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="584c0-209">Next steps</span></span>

<span data-ttu-id="584c0-210">In questa esercitazione è stato aggiunto di markup e codice per visualizzare i prodotti e i dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="584c0-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="584c0-211">Sono stati illustrati i controlli dati fortemente tipizzato, l'associazione di modelli e i provider di valori.</span><span class="sxs-lookup"><span data-stu-id="584c0-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="584c0-212">Nella prossima esercitazione, si aggiungerà un carrello della spesa per l'applicazione di esempio Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="584c0-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="584c0-213">[Precedente](ui_and_navigation.md)
> [Successivo](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="584c0-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
