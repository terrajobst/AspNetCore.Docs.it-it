---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Visualizzare i dati degli elementi e i dettagli | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.7 e Microsoft Visual Studio Community 2017 per il Web
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207434"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="4b559-103">Elementi di dati visualizzati e dettagli</span><span class="sxs-lookup"><span data-stu-id="4b559-103">Display data items and details</span></span>
====================
<span data-ttu-id="4b559-104">da [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="4b559-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="4b559-105">Questa serie di esercitazioni illustra le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.7 e Microsoft Visual Studio Community 2017 per il Web.</span><span class="sxs-lookup"><span data-stu-id="4b559-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio Community 2017 for the Web.</span></span>

<span data-ttu-id="4b559-106">In questa esercitazione descrive come visualizzare gli elementi di dati e i dettagli dell'elemento dati con Entity Framework Code First e Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4b559-106">In this tutorial, you learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="4b559-107">Questa esercitazione si basa sull'esercitazione precedente "Navigazione dell'interfaccia utente e" come parte della serie di esercitazioni di Wingtip giocattoli Store.</span><span class="sxs-lookup"><span data-stu-id="4b559-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="4b559-108">Nell'esercitazione completata, i prodotti nel *ProductsList.aspx* pagina e i dettagli di un prodotto nel *ProductDetails.aspx* pagina vengono visualizzati.</span><span class="sxs-lookup"><span data-stu-id="4b559-108">In the completed tutorial, products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page are displayed.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="4b559-109">Tutto ciò che Apprendi</span><span class="sxs-lookup"><span data-stu-id="4b559-109">What you learn</span></span>

- <span data-ttu-id="4b559-110">Aggiungere un controllo dei dati per visualizzare i prodotti di database.</span><span class="sxs-lookup"><span data-stu-id="4b559-110">Add a data control to display database products.</span></span>
- <span data-ttu-id="4b559-111">Connettere un controllo dei dati per i dati selezionati.</span><span class="sxs-lookup"><span data-stu-id="4b559-111">Connect a data control to selected data.</span></span>
- <span data-ttu-id="4b559-112">Aggiungere un controllo dei dati per visualizzare i dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="4b559-112">Add a data control to display product details.</span></span>
- <span data-ttu-id="4b559-113">Analizzare un valore di stringa di query e usarlo per filtrare i dati del database recuperato.</span><span class="sxs-lookup"><span data-stu-id="4b559-113">Parse a query string value and use it to filter retrieved database data.</span></span>

<span data-ttu-id="4b559-114">Le funzionalità introdotte in questa esercitazione includono l'associazione di modelli e i provider di valori.</span><span class="sxs-lookup"><span data-stu-id="4b559-114">Features introduced in this tutorial include model binding and value providers.</span></span>

## <a name="add-a-data-control-to-display-products"></a><span data-ttu-id="4b559-115">Aggiungere un controllo dati per visualizzare i prodotti</span><span class="sxs-lookup"><span data-stu-id="4b559-115">Add a data control to display products</span></span>
 
<span data-ttu-id="4b559-116">Sono disponibili alcune opzioni per associare dati a un controllo server.</span><span class="sxs-lookup"><span data-stu-id="4b559-116">You have a few options to bind data to a server control.</span></span> <span data-ttu-id="4b559-117">I più comuni includono:</span><span class="sxs-lookup"><span data-stu-id="4b559-117">The most common include:</span></span>

 * <span data-ttu-id="4b559-118">Aggiunta di un controllo origine dati</span><span class="sxs-lookup"><span data-stu-id="4b559-118">Adding a data source control</span></span>
 * <span data-ttu-id="4b559-119">Aggiunta manuale di codice</span><span class="sxs-lookup"><span data-stu-id="4b559-119">Adding code by hand</span></span>
 * <span data-ttu-id="4b559-120">Implementazione di associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="4b559-120">Implementing model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="4b559-121">Usare un controllo origine dati per associare i dati</span><span class="sxs-lookup"><span data-stu-id="4b559-121">Use a data source control to bind data</span></span>

<span data-ttu-id="4b559-122">Aggiunta di un controllo origine dati, i collegamenti al controllo origine dati al controllo che visualizza i dati.</span><span class="sxs-lookup"><span data-stu-id="4b559-122">Adding a data source control links the data source control to the control that displays the data.</span></span> <span data-ttu-id="4b559-123">Con questo approccio, è possibile in modo dichiarativo, anziché connettersi a livello di codice i controlli lato server alle origini dati.</span><span class="sxs-lookup"><span data-stu-id="4b559-123">With this approach, you can declaratively, rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="4b559-124">Codice manualmente per associare i dati</span><span class="sxs-lookup"><span data-stu-id="4b559-124">Code by hand to bind data</span></span>

<span data-ttu-id="4b559-125">Codifica manuale prevede:</span><span class="sxs-lookup"><span data-stu-id="4b559-125">Coding by hand involves:</span></span>

1. <span data-ttu-id="4b559-126">Lettura di un valore</span><span class="sxs-lookup"><span data-stu-id="4b559-126">Reading a value</span></span>
2. <span data-ttu-id="4b559-127">Controllare se è null</span><span class="sxs-lookup"><span data-stu-id="4b559-127">Checking if it's null</span></span>
3. <span data-ttu-id="4b559-128">Conversione in un tipo appropriato</span><span class="sxs-lookup"><span data-stu-id="4b559-128">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="4b559-129">Controllo operazioni riuscite di conversione</span><span class="sxs-lookup"><span data-stu-id="4b559-129">Checking conversion success</span></span>
5. <span data-ttu-id="4b559-130">Effettua una query con il valore convertito</span><span class="sxs-lookup"><span data-stu-id="4b559-130">Making a query with the converted value</span></span> 

<span data-ttu-id="4b559-131">Con questo approccio, è necessario il controllo completo per la logica di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="4b559-131">With this approach, you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="4b559-132">Usare l'associazione di modelli per associare i dati</span><span class="sxs-lookup"><span data-stu-id="4b559-132">Use model binding to bind data</span></span>

<span data-ttu-id="4b559-133">Con l'associazione di modelli, associare i risultati con molto meno codice e offre la possibilità di riusare le funzionalità in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4b559-133">With model binding, you bind results with far less code and it gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="4b559-134">Semplifica l'utilizzo con la logica di accesso ai dati incentrato sul codice garantendo comunque un framework completo e associazione dati.</span><span class="sxs-lookup"><span data-stu-id="4b559-134">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="4b559-135">Visualizzare i prodotti</span><span class="sxs-lookup"><span data-stu-id="4b559-135">Display products</span></span>

<span data-ttu-id="4b559-136">In questa esercitazione si usa l'associazione di modelli per associare i dati.</span><span class="sxs-lookup"><span data-stu-id="4b559-136">In this tutorial, you use model binding to bind data.</span></span> <span data-ttu-id="4b559-137">Per configurare un controllo dati per utilizzare l'associazione di modelli per selezionare i dati, si imposta il controllo `SelectMethod` proprietà a un metodo nel codice della pagina.</span><span class="sxs-lookup"><span data-stu-id="4b559-137">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method in the page's code.</span></span> <span data-ttu-id="4b559-138">Il controllo dei dati chiama il metodo momento adeguato del ciclo di vita della pagina e associa automaticamente i dati restituiti.</span><span class="sxs-lookup"><span data-stu-id="4b559-138">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="4b559-139">Non è necessario chiamare in modo esplicito il `DataBind` (metodo).</span><span class="sxs-lookup"><span data-stu-id="4b559-139">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="4b559-140">Usa i passaggi seguenti, si modificano *ProductList. aspx* markup per visualizzare i prodotti.</span><span class="sxs-lookup"><span data-stu-id="4b559-140">Working through the following steps, you modify *ProductList.aspx* markup to display products.</span></span>

1. <span data-ttu-id="4b559-141">Nelle **Esplora soluzioni**aprire *ProductList. aspx*.</span><span class="sxs-lookup"><span data-stu-id="4b559-141">In **Solution Explorer**, open *ProductList.aspx*.</span></span>

2. <span data-ttu-id="4b559-142">Sostituire il markup esistente con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="4b559-142">Replace the existing markup with the following markup:</span></span> 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="4b559-143">Il markup precedente Usa un **ListView** controllo denominato `productList` per visualizzare i prodotti.</span><span class="sxs-lookup"><span data-stu-id="4b559-143">The preceding markup uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="4b559-144">Con stili e modelli, è possibile definire come il **ListView** controllo Visualizza i dati.</span><span class="sxs-lookup"><span data-stu-id="4b559-144">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="4b559-145">È utile per i dati in una struttura ripetuta.</span><span class="sxs-lookup"><span data-stu-id="4b559-145">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="4b559-146">Anche se ciò **ListView** viene visualizzata semplicemente i dati del database, è possibile anche, senza codice, consentire agli utenti di modificare, inserire ed eliminare dati e di ordinamento e paging dei dati.</span><span class="sxs-lookup"><span data-stu-id="4b559-146">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="4b559-147">Quando si imposta la `ItemType` proprietà nel **ListView** controllare, l'espressione di associazione dati `Item` è disponibile e il controllo diventa fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="4b559-147">When you set the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="4b559-148">Come indicato nell'esercitazione precedente, è possibile selezionare i dettagli dell'oggetto elemento con la tecnologia IntelliSense, ad esempio specificando la `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="4b559-148">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Visualizzare i dati gli elementi e i dettagli - IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="4b559-150">Con l'associazione di modelli, si specifica un `SelectMethod` valore (`GetProducts`).</span><span class="sxs-lookup"><span data-stu-id="4b559-150">With model binding, you're specifying a `SelectMethod` value (`GetProducts`).</span></span> <span data-ttu-id="4b559-151">Questo è il metodo da aggiungere al codice indietro per visualizzare i prodotti nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="4b559-151">This is the method you add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="4b559-152">Aggiungere codice per visualizzare i prodotti</span><span class="sxs-lookup"><span data-stu-id="4b559-152">Add code to display products</span></span>

<span data-ttu-id="4b559-153">In questo passaggio si aggiunge codice per popolare la **ListView** controllo con i dati del prodotto di database.</span><span class="sxs-lookup"><span data-stu-id="4b559-153">In this step, you add code to populate the **ListView** control with database product data.</span></span> <span data-ttu-id="4b559-154">Il codice supporta che mostra tutti i prodotti e singola categoria.</span><span class="sxs-lookup"><span data-stu-id="4b559-154">The code supports showing all products and individual category products.</span></span>

1. <span data-ttu-id="4b559-155">Nelle **Esplora soluzioni**, fare doppio clic su *ProductList. aspx* e quindi selezionare **Visualizza codice**.</span><span class="sxs-lookup"><span data-stu-id="4b559-155">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="4b559-156">Sostituire il codice esistente nel *ProductList.aspx.cs* file con questo:</span><span class="sxs-lookup"><span data-stu-id="4b559-156">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="4b559-157">Questo codice viene illustrato il `GetProducts` metodo che i **ListView** del controllo `ItemType` riferimenti alla proprietà nella *ProductList. aspx*.</span><span class="sxs-lookup"><span data-stu-id="4b559-157">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in *ProductList.aspx*.</span></span> <span data-ttu-id="4b559-158">Per limitare i risultati a una categoria di database specifico, il codice imposta la `categoryId` valore dalla stringa di query passata a *ProductList. aspx*.</span><span class="sxs-lookup"><span data-stu-id="4b559-158">To limit the results to a specific database category, the code sets the `categoryId` value from the query string passed to *ProductList.aspx*.</span></span> <span data-ttu-id="4b559-159">Il `QueryStringAttribute` classe la `System.Web.ModelBinding` dello spazio dei nomi viene usato per recuperare la variabile di stringa di query `id`del valore.</span><span class="sxs-lookup"><span data-stu-id="4b559-159">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the query string variable `id`'s value.</span></span> <span data-ttu-id="4b559-160">Ciò indica l'associazione di modelli per, in fase di esecuzione, associare un valore di stringa di query per il `categoryId` parametro.</span><span class="sxs-lookup"><span data-stu-id="4b559-160">This instructs model binding to, at run time, bind a query string value to the `categoryId` parameter.</span></span>

<span data-ttu-id="4b559-161">Quando una categoria valida (`categoryId`) viene passato, i risultati sono limitati ai prodotti del database della categoria.</span><span class="sxs-lookup"><span data-stu-id="4b559-161">When a valid category (`categoryId`) is passed, the results are limited to that category's database products.</span></span> <span data-ttu-id="4b559-162">Ad esempio, se il *ProductsList.aspx* URL della pagina è questo:</span><span class="sxs-lookup"><span data-stu-id="4b559-162">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="4b559-163">Nella pagina vengono visualizzati solo i prodotti in cui il `categoryId` è uguale a `1`.</span><span class="sxs-lookup"><span data-stu-id="4b559-163">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="4b559-164">Se non viene passata alcuna stringa di query, verranno visualizzati tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="4b559-164">All products are displayed if no query string is passed.</span></span>

<span data-ttu-id="4b559-165">Le origini dei valori per questi metodi vengono chiamate *valore provider* (ad esempio `QueryString`), e gli attributi di parametro che indicano quali provider di valori da utilizzare vengono chiamati *attributi di provider di valore* ( ad esempio `id`).</span><span class="sxs-lookup"><span data-stu-id="4b559-165">The value sources for these methods are called *value providers* (such as `QueryString`), and the parameter attributes that indicate which value provider to use are called *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="4b559-166">ASP.NET include i provider di valori e gli attributi per tutti i Web Form dell'applicazione utente inpue origini tipiche.</span><span class="sxs-lookup"><span data-stu-id="4b559-166">ASP.NET includes value providers and attributes for all typical Web Forms application user input sources.</span></span> <span data-ttu-id="4b559-167">Queste includono la stringa di query, cookie, i valori del form, controlli, lo stato di visualizzazione, lo stato della sessione e le proprietà del profilo.</span><span class="sxs-lookup"><span data-stu-id="4b559-167">These include the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="4b559-168">È anche possibile scrivere provider di valori personalizzati.</span><span class="sxs-lookup"><span data-stu-id="4b559-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="4b559-169">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="4b559-169">Run the application</span></span>

<span data-ttu-id="4b559-170">Eseguire ora l'applicazione per visualizzare tutti i prodotti o i prodotti della categoria.</span><span class="sxs-lookup"><span data-stu-id="4b559-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="4b559-171">In Visual Studio, premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4b559-171">In Visual Studio, press **F5** to run the application.</span></span>
 <span data-ttu-id="4b559-172">Il browser si apre e Mostra le *default. aspx* pagina.</span><span class="sxs-lookup"><span data-stu-id="4b559-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="4b559-173">Nel menu di categoria di prodotto, selezionare **automobili**.</span><span class="sxs-lookup"><span data-stu-id="4b559-173">From the product category menu, select **Cars**.</span></span>

   <span data-ttu-id="4b559-174">Il *ProductList. aspx* verrà visualizzata la pagina, che mostra solo i prodotti dalle **automobili** categoria.</span><span class="sxs-lookup"><span data-stu-id="4b559-174">The *ProductList.aspx* page appears, showing only products from the **Cars** category.</span></span> <span data-ttu-id="4b559-175">Più avanti in questa esercitazione è visualizzare i dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="4b559-175">Later in this tutorial, you display product details.</span></span>

    ![Visualizzare i dati gli elementi e i dettagli - automobili](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="4b559-177">Selezionare **prodotti** dal menu in alto.</span><span class="sxs-lookup"><span data-stu-id="4b559-177">Select **Products** from the top menu.</span></span>
 <span data-ttu-id="4b559-178">Il *ProductList. aspx* nella pagina verranno visualizzati tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="4b559-178">The *ProductList.aspx* page now displays all products.</span></span> 

    ![Visualizzare i dati gli elementi e i dettagli - prodotti](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="4b559-180">Chiudere il browser e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b559-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="4b559-181">Aggiungere un controllo dei dati per visualizzare i dettagli sul prodotto</span><span class="sxs-lookup"><span data-stu-id="4b559-181">Add a Data Control to display product details</span></span>

<span data-ttu-id="4b559-182">Modificare il *ProductDetails.aspx* markup di aggiunto nell'esercitazione precedente per visualizzare le informazioni di prodotto specifico:</span><span class="sxs-lookup"><span data-stu-id="4b559-182">Modify the *ProductDetails.aspx* markup that you added in the previous tutorial to display specific product information:</span></span>

1. <span data-ttu-id="4b559-183">Nelle **Esplora soluzioni**aprire *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="4b559-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="4b559-184">Sostituire il markup esistente con questo codice:</span><span class="sxs-lookup"><span data-stu-id="4b559-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

<span data-ttu-id="4b559-185">Questo markup Usa un' **FormView** controllo per visualizzare i dettagli sul prodotto specifico.</span><span class="sxs-lookup"><span data-stu-id="4b559-185">This markup uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="4b559-186">Usa i metodi, ad esempio quelle usate per visualizzare i dati in *ProductList. aspx*.</span><span class="sxs-lookup"><span data-stu-id="4b559-186">It uses methods like those used to display data in *ProductList.aspx*.</span></span> <span data-ttu-id="4b559-187">Il **FormView** controllo viene usato per visualizzare un singolo record alla volta da un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="4b559-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="4b559-188">Quando si usa la **FormView** (controllo), è creare modelli per visualizzare e modificare i valori associati a dati.</span><span class="sxs-lookup"><span data-stu-id="4b559-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="4b559-189">Questi modelli contengono controlli, espressioni di associazione, e formattazione che definiscono l'aspetto e funzionalità del modulo.</span><span class="sxs-lookup"><span data-stu-id="4b559-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="4b559-190">Il markup precedente la connessione al database richiede codice aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="4b559-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="4b559-191">Nelle **Esplora soluzioni**, fare doppio clic su *ProductDetails.aspx* e quindi selezionare **Visualizza codice**.</span><span class="sxs-lookup"><span data-stu-id="4b559-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then select **View Code**.</span></span>  
   <span data-ttu-id="4b559-192">Il *ProductDetails.aspx.cs* file viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="4b559-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="4b559-193">Sostituire il codice esistente con questo:</span><span class="sxs-lookup"><span data-stu-id="4b559-193">Replace the existing code with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="4b559-194">Questo codice cerca un "`productID`" valore di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="4b559-194">This code checks for a "`productID`" query string value.</span></span> <span data-ttu-id="4b559-195">Se viene trovato un valore valido, viene visualizzato il prodotto corrisponda.</span><span class="sxs-lookup"><span data-stu-id="4b559-195">If a valid value is found, the matching product is displayed.</span></span> <span data-ttu-id="4b559-196">Se non viene trovata la stringa di query o il relativo valore non è valido, non viene visualizzato alcun prodotto.</span><span class="sxs-lookup"><span data-stu-id="4b559-196">If the query string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="4b559-197">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="4b559-197">Run the application</span></span>

<span data-ttu-id="4b559-198">A questo punto è possibile eseguire l'applicazione per vedere i dettagli sul prodotto specifico basati su ID prodotto.</span><span class="sxs-lookup"><span data-stu-id="4b559-198">Now you can run the application to see specific product details based on product ID.</span></span>

1. <span data-ttu-id="4b559-199">In Visual Studio, premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4b559-199">In Visual Studio, press **F5** to run the application.</span></span>  
 <span data-ttu-id="4b559-200">Nel browser verrà aperta *default. aspx*.</span><span class="sxs-lookup"><span data-stu-id="4b559-200">The browser opens to *Default.aspx*.</span></span>

2. <span data-ttu-id="4b559-201">Nel menu di categoria, selezionare **evolveranno**.</span><span class="sxs-lookup"><span data-stu-id="4b559-201">From the category menu, select **Boats**.</span></span>  
 <span data-ttu-id="4b559-202">Il *ProductList. aspx* viene visualizzata la pagina.</span><span class="sxs-lookup"><span data-stu-id="4b559-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="4b559-203">Selezionare **carta barca**.</span><span class="sxs-lookup"><span data-stu-id="4b559-203">Select **Paper Boat**.</span></span>  
 <span data-ttu-id="4b559-204">Il *ProductDetails.aspx* viene visualizzata la pagina.</span><span class="sxs-lookup"><span data-stu-id="4b559-204">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Visualizzare i dati gli elementi e i dettagli - prodotti](display_data_items_and_details/_static/image4.png)

<span data-ttu-id="4b559-206">Nella prossima esercitazione, un carrello acquisti è aggiungere l'applicazione Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="4b559-206">In the next tutorial, you add a shopping cart to the Wingtip Toys application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4b559-207">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4b559-207">Additional resources</span></span>

[<span data-ttu-id="4b559-208">Il recupero e visualizzazione dei dati con l'associazione di modelli e web form</span><span class="sxs-lookup"><span data-stu-id="4b559-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="4b559-209">[Precedente](ui_and_navigation.md)
> [Successivo](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="4b559-209">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
