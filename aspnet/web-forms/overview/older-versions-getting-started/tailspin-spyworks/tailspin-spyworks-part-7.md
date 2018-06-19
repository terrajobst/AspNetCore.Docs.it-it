---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: Aggiunta di funzionalità | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 7 aggiunge funzionalità aggiuntive, ad esempio account revie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888393"
---
<a name="part-7-adding-features"></a><span data-ttu-id="a064f-104">Parte 7: Aggiunta di funzionalità</span><span class="sxs-lookup"><span data-stu-id="a064f-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="a064f-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="a064f-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="a064f-106">Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="a064f-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="a064f-107">Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="a064f-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="a064f-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="a064f-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="a064f-109">Parte 7 aggiunge funzionalità aggiuntive, come la revisione di account, recensioni dei prodotti e "elementi comuni" e controlli utente "anche acquistato".</span><span class="sxs-lookup"><span data-stu-id="a064f-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="a064f-110">Aggiunta di funzionalità</span><span class="sxs-lookup"><span data-stu-id="a064f-110">Adding Features</span></span>

<span data-ttu-id="a064f-111">Se gli utenti possono cercare il catalogo, inserire gli elementi nel carrello acquisti e completare il processo di estrazione, sono disponibili che funzionalità di un numero di supporto che verrà incluso per migliorare il sito.</span><span class="sxs-lookup"><span data-stu-id="a064f-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="a064f-112">Revisione di account (elenco di ordini inseriti e visualizzano i dettagli.)</span><span class="sxs-lookup"><span data-stu-id="a064f-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="a064f-113">Aggiungere alcuni contenuti specifici di contesto nella prima pagina.</span><span class="sxs-lookup"><span data-stu-id="a064f-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="a064f-114">Aggiungere una funzionalità per consentire agli utenti di esaminare i prodotti nel catalogo.</span><span class="sxs-lookup"><span data-stu-id="a064f-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="a064f-115">Creare un controllo utente per visualizzare gli elementi comuni e sul posto che controllano sulla prima pagina.</span><span class="sxs-lookup"><span data-stu-id="a064f-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="a064f-116">Creare un controllo utente "Anche acquistato" e aggiungerlo alla pagina dei dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="a064f-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="a064f-117">Aggiungere un contatto pagina.</span><span class="sxs-lookup"><span data-stu-id="a064f-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="a064f-118">Aggiungere una pagina.</span><span class="sxs-lookup"><span data-stu-id="a064f-118">Add an About Page.</span></span>
8. <span data-ttu-id="a064f-119">Errore globale</span><span class="sxs-lookup"><span data-stu-id="a064f-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="a064f-120">Revisione di account</span><span class="sxs-lookup"><span data-stu-id="a064f-120">Account Review</span></span>

<span data-ttu-id="a064f-121">Nella cartella "Account", creare due pagine aspx OrderList.aspx denominata uno e l'altra denominata OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="a064f-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="a064f-122">OrderList.aspx verranno utilizzati i controlli GridView ed EntityDataSoure così come è stato precedentemente definito.</span><span class="sxs-lookup"><span data-stu-id="a064f-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="a064f-123">Il EntityDataSoure seleziona i record della tabella Orders filtrati al nome utente (vedere il WhereParameter) che è impostato in una variabile di sessione quando all'utente di accedere.</span><span class="sxs-lookup"><span data-stu-id="a064f-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="a064f-124">Si noti inoltre questi parametri, HyperlinkField di GridView:</span><span class="sxs-lookup"><span data-stu-id="a064f-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="a064f-125">Il collegamento alla visualizzazione Dettagli ordine per ogni prodotto specificando il campo OrderID come parametro QueryString per la pagina OrderDetails.aspx specificano.</span><span class="sxs-lookup"><span data-stu-id="a064f-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="a064f-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="a064f-126">OrderDetails.aspx</span></span>

<span data-ttu-id="a064f-127">Un controllo EntityDataSource utilizzeremo per accedere gli ordini e un controllo FormView per visualizzare i dati dell'ordine e un altro EntityDataSource con un controllo GridView per visualizzare le voci dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="a064f-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="a064f-128">Nel file Code-Behind (OrderDetails.aspx.cs) sono disponibili due bit minimo di manutenzione.</span><span class="sxs-lookup"><span data-stu-id="a064f-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="a064f-129">È innanzitutto necessario assicurarsi che OrderDetails ottiene sempre un OrderId.</span><span class="sxs-lookup"><span data-stu-id="a064f-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="a064f-130">È anche necessario calcolare e visualizzare il totale di voci dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="a064f-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="a064f-131">Home Page</span><span class="sxs-lookup"><span data-stu-id="a064f-131">The Home Page</span></span>

<span data-ttu-id="a064f-132">Aggiungere contenuto statico alla pagina Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="a064f-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="a064f-133">Prima di tutto creare una cartella "Contenuta" e al suo interno una cartella immagini (e possibile includere un'immagine da utilizzare nella home page).</span><span class="sxs-lookup"><span data-stu-id="a064f-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="a064f-134">Nel segnaposto nella parte inferiore della pagina aspx, aggiungere il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="a064f-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="a064f-135">Recensioni dei prodotti</span><span class="sxs-lookup"><span data-stu-id="a064f-135">Product Reviews</span></span>

<span data-ttu-id="a064f-136">Innanzitutto verrà aggiunto un pulsante con un collegamento a un form che è possibile utilizzare per immettere una revisione del prodotto.</span><span class="sxs-lookup"><span data-stu-id="a064f-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="a064f-137">Si noti che viene passato il ProductID nella stringa di query</span><span class="sxs-lookup"><span data-stu-id="a064f-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="a064f-138">Avanti aggiungere pagina denominata ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="a064f-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="a064f-139">Questa pagina verrà utilizza ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="a064f-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="a064f-140">Se non è già fatto in modo è possibile scaricarlo dal [DevExpress](http://devexpress.com/act) e sono presenti informazioni aggiuntive su come configurare il toolkit per l'uso con Visual Studio qui [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="a064f-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="a064f-141">Nella modalità progettazione, trascinare i controlli e validator dalla casella degli strumenti e creare un form simile a quello riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a064f-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="a064f-142">Il codice sarà simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="a064f-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="a064f-143">Ora che è possibile immettere le revisioni, consente di visualizzare le revisioni nella pagina del prodotto.</span><span class="sxs-lookup"><span data-stu-id="a064f-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="a064f-144">Aggiungere il markup alla pagina ProductDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="a064f-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="a064f-145">In esecuzione l'applicazione ora e passare a un prodotto Mostra le informazioni sul prodotto, incluse le recensioni dei clienti.</span><span class="sxs-lookup"><span data-stu-id="a064f-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="a064f-146">Controllo di elementi comuni (creazione di controlli utente)</span><span class="sxs-lookup"><span data-stu-id="a064f-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="a064f-147">Per aumentare le vendite nel sito web verrà aggiunto un paio di funzionalità per i prodotti più diffusi o correlati "vendere suggerite".</span><span class="sxs-lookup"><span data-stu-id="a064f-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="a064f-148">Il primo di queste funzionalità saranno un elenco del prodotto più popolare il catalogo dei prodotti.</span><span class="sxs-lookup"><span data-stu-id="a064f-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="a064f-149">Si creerà un "controllo utente" per visualizzare gli elementi nella home page dell'applicazione più venduti.</span><span class="sxs-lookup"><span data-stu-id="a064f-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="a064f-150">Poiché questo sarà un controllo, è possibile utilizzare in qualsiasi pagina semplicemente trascinando e rilasciando il controllo nella finestra di progettazione di Visual Studio in qualsiasi pagina che si desidera.</span><span class="sxs-lookup"><span data-stu-id="a064f-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="a064f-151">In Esplora soluzioni di Visual Studio, fare clic sul nome della soluzione e creare una nuova directory denominata "Controlli".</span><span class="sxs-lookup"><span data-stu-id="a064f-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="a064f-152">Mentre non è necessario eseguire questa operazione, si consente di mantenere il progetto organizzato mediante la creazione di tutti i controlli utente nella directory "Controlli".</span><span class="sxs-lookup"><span data-stu-id="a064f-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="a064f-153">Pulsante destro del mouse sulla cartella controlli e scegliere "New Item":</span><span class="sxs-lookup"><span data-stu-id="a064f-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="a064f-154">Specificare un nome per il controllo di "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="a064f-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="a064f-155">Si noti che l'estensione di file per i controlli utente ascx non aspx.</span><span class="sxs-lookup"><span data-stu-id="a064f-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="a064f-156">Il controllo utente comune di elementi verrà definito come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a064f-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="a064f-157">In questo caso si usa un metodo che è stato non ancora usato in questa applicazione.</span><span class="sxs-lookup"><span data-stu-id="a064f-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="a064f-158">Utilizziamo controllo repeater e invece di usare un controllo origine dati si sta associando il controllo Repeater ai risultati di una query LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="a064f-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="a064f-159">Nel code-behind di questo controllo è eseguire questa operazione come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a064f-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="a064f-160">Si noti inoltre questa riga importante nella parte superiore del markup del controllo.</span><span class="sxs-lookup"><span data-stu-id="a064f-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="a064f-161">Poiché gli elementi più diffusi verrà modificato in una base di un minuto a altro è possibile aggiungere una direttiva aching per migliorare le prestazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a064f-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="a064f-162">Questa direttiva causerà il codice di controlli per essere eseguita solo quando l'output del controllo memorizzati nella cache scade.</span><span class="sxs-lookup"><span data-stu-id="a064f-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="a064f-163">In caso contrario, verrà utilizzata la versione memorizzata nella cache di output del controllo.</span><span class="sxs-lookup"><span data-stu-id="a064f-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="a064f-164">A questo punto è sufficiente includere il nuovo controllo nella nostra pagina Default.aspc.</span><span class="sxs-lookup"><span data-stu-id="a064f-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="a064f-165">Utilizzare trascinamento della selezione per inserire un'istanza del controllo nella colonna aperta il modulo predefinito.</span><span class="sxs-lookup"><span data-stu-id="a064f-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="a064f-166">A questo punto quando si esegue l'applicazione della home page Visualizza gli elementi più comuni.</span><span class="sxs-lookup"><span data-stu-id="a064f-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="a064f-167">"Anche acquistato" controllare (controlli utente con i parametri)</span><span class="sxs-lookup"><span data-stu-id="a064f-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="a064f-168">Il secondo controllo utente che verrà creata richiederà suggerito aggiungendo specificità contesto delle vendite al livello successivo.</span><span class="sxs-lookup"><span data-stu-id="a064f-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="a064f-169">La logica per calcolare i primi elementi "Anche acquistato" è non semplice.</span><span class="sxs-lookup"><span data-stu-id="a064f-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="a064f-170">Del controllo "Anche acquistato" verrà selezionare i record OrderDetails (acquistati in precedenza) per il ProductID attualmente selezionato e agganciare i OrderIDs per ogni ordine univoco che viene trovato.</span><span class="sxs-lookup"><span data-stu-id="a064f-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="a064f-171">Quindi si selezionerà tutti i prodotti da tutti i ordini e somma le quantità acquistate.</span><span class="sxs-lookup"><span data-stu-id="a064f-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="a064f-172">Viene ordinare i prodotti per la somma delle quantità e visualizzare i primi cinque elementi.</span><span class="sxs-lookup"><span data-stu-id="a064f-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="a064f-173">Data la complessità di questa logica, come una stored procedure verrà implementato questo algoritmo.</span><span class="sxs-lookup"><span data-stu-id="a064f-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="a064f-174">L'istruzione T-SQL per la stored procedure è come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a064f-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="a064f-175">Si noti che questa stored procedure (SelectPurchasedWithProducts) devono essere presenti nel database, quando sono inclusi nell'applicazione e quando viene generato è specificato che, oltre alle tabelle e viste che è necessario, Entity Data Model Entity Data Model deve includere questa stored procedure.</span><span class="sxs-lookup"><span data-stu-id="a064f-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="a064f-176">Per accedere alla stored procedure da Entity Data Model, è necessario importare la funzione.</span><span class="sxs-lookup"><span data-stu-id="a064f-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="a064f-177">Fare doppio clic su Entity Data Model in Esplora soluzioni per aprirlo nella finestra di progettazione e aprire il Browser modello, quindi fare doppio clic nella finestra di progettazione e selezionare "Aggiungi funzione importazione".</span><span class="sxs-lookup"><span data-stu-id="a064f-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="a064f-178">In questo modo verrà aperta la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="a064f-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="a064f-179">Compilare i campi come visualizzato in precedenza, la selezione di "SelectPurchasedWithProducts" e utilizzare il nome della routine per il nome del nostro funzione importata.</span><span class="sxs-lookup"><span data-stu-id="a064f-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="a064f-180">Fare clic su "Ok".</span><span class="sxs-lookup"><span data-stu-id="a064f-180">Click "Ok".</span></span>

<span data-ttu-id="a064f-181">Al termine che è semplicemente possibile programmare la stored procedure come si farebbe qualsiasi altro elemento del modello.</span><span class="sxs-lookup"><span data-stu-id="a064f-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="a064f-182">In tal caso, la cartella "Controlli" creare un nuovo controllo utente denominato AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="a064f-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="a064f-183">Il markup per il controllo sarà molto familiare per il controllo PopularItems.</span><span class="sxs-lookup"><span data-stu-id="a064f-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="a064f-184">La differenza evidente è che non memorizza nella cache l'output dopo l'esecuzione del rendering dell'elemento sarà diversa dal prodotto.</span><span class="sxs-lookup"><span data-stu-id="a064f-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="a064f-185">ProductId sarà "property" per il controllo.</span><span class="sxs-lookup"><span data-stu-id="a064f-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="a064f-186">Nel gestore di evento PreRender del controllo è eed per eseguire tre operazioni.</span><span class="sxs-lookup"><span data-stu-id="a064f-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="a064f-187">Verificare che sia impostato il ProductID.</span><span class="sxs-lookup"><span data-stu-id="a064f-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="a064f-188">Verificare se sono presenti tutti i prodotti che sono stati acquistati con quella corrente.</span><span class="sxs-lookup"><span data-stu-id="a064f-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="a064f-189">L'output di alcuni elementi, come determinato # 2.</span><span class="sxs-lookup"><span data-stu-id="a064f-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="a064f-190">Si noti quanto sia facile per chiamare la stored procedure tramite il modello.</span><span class="sxs-lookup"><span data-stu-id="a064f-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="a064f-191">Dopo aver determinato che non esiste "inoltre acquistati" Ripetitore semplicemente possibile associare ai risultati restituiti dalla query.</span><span class="sxs-lookup"><span data-stu-id="a064f-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="a064f-192">Se non erano disponibili in tutti gli elementi "anche acquistato" è semplicemente verranno visualizzati altri elementi comuni dal nostro catalogo.</span><span class="sxs-lookup"><span data-stu-id="a064f-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="a064f-193">Per visualizzare gli elementi "Anche acquistato", aprire la pagina ProductDetails.aspx e trascinare il controllo AlsoPurchased da Esplora soluzioni, in modo che venga visualizzato in questa posizione nel markup.</span><span class="sxs-lookup"><span data-stu-id="a064f-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="a064f-194">In questo modo verrà creato un riferimento al controllo nella parte superiore della pagina Dettagli prodotto.</span><span class="sxs-lookup"><span data-stu-id="a064f-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="a064f-195">Poiché il controllo utente AlsoPurchased richiede un numero di ID prodotto si imposterà la proprietà ProductID del controllo utilizzando un'istruzione Eval sull'elemento del modello di dati corrente della pagina.</span><span class="sxs-lookup"><span data-stu-id="a064f-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="a064f-196">Quando si compila ed Esegui ora e passare a un prodotto è visualizzare gli elementi "Anche acquistato".</span><span class="sxs-lookup"><span data-stu-id="a064f-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="a064f-197">[Precedente](tailspin-spyworks-part-6.md)
> [Successivo](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="a064f-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
