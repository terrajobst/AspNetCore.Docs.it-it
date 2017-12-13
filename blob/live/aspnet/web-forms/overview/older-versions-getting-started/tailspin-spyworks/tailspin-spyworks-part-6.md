---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: L''appartenenza ASP.NET | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 6 aggiunge le appartenenze di ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: efb0e2bed1172f42c7f1539f016fba305c47e3eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="2cf72-104">Parte 6: L'appartenenza ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2cf72-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="2cf72-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="2cf72-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="2cf72-106">Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="2cf72-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="2cf72-107">Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="2cf72-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="2cf72-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="2cf72-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="2cf72-109">Parte 6 aggiunge le appartenenze di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2cf72-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a><span data-ttu-id="2cf72-110">Utilizzo di appartenenza ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2cf72-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="2cf72-111">Fare clic su protezione</span><span class="sxs-lookup"><span data-stu-id="2cf72-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="2cf72-112">Assicurarsi che si sta usando l'autenticazione basata su form.</span><span class="sxs-lookup"><span data-stu-id="2cf72-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="2cf72-113">Utilizzare il collegamento "Create User" per creare un paio di utenti.</span><span class="sxs-lookup"><span data-stu-id="2cf72-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="2cf72-114">Al termine, fare riferimento alla finestra di Esplora soluzioni e aggiornare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="2cf72-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="2cf72-115">Si noti che il file ASPNETDB. Fine file MDF è stato creato.</span><span class="sxs-lookup"><span data-stu-id="2cf72-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="2cf72-116">Questo file contiene le tabelle per supportare i servizi ASP.NET di base come l'appartenenza.</span><span class="sxs-lookup"><span data-stu-id="2cf72-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="2cf72-117">Ora è possibile iniziare l'implementazione del processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="2cf72-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="2cf72-118">Iniziare creando una pagina CheckOut.aspx.</span><span class="sxs-lookup"><span data-stu-id="2cf72-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="2cf72-119">La pagina CheckOut.aspx solo deve essere disponibile per gli utenti connessi in modo si verrà limitato l'accesso ai registrato in utenti e reindirizzerà gli utenti che non sono connessi alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="2cf72-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="2cf72-120">A tale scopo seguente verrà aggiunto alla sezione di configurazione del file Web. config.</span><span class="sxs-lookup"><span data-stu-id="2cf72-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="2cf72-121">Il modello per le applicazioni Web Form ASP.NET viene automaticamente aggiunta una sezione di autenticazione per il file Web. config e stabilita pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="2cf72-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="2cf72-122">È necessario modificare il login file code-behind per eseguire la migrazione di un carrello acquisti anonimo quando l'utente effettua l'accesso.</span><span class="sxs-lookup"><span data-stu-id="2cf72-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="2cf72-123">Modificare la pagina\_evento di caricamento come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="2cf72-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="2cf72-124">Quindi aggiungere un gestore dell'evento simile al seguente per impostare il nome della sessione per l'utente appena connesso e modificare l'id sessione temporaneo nel carrello acquisti a quella dell'utente chiamando il metodo MigrateCart della classe MyShoppingCart "LoggedIn".</span><span class="sxs-lookup"><span data-stu-id="2cf72-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="2cf72-125">(Implementato nel file con estensione cs)</span><span class="sxs-lookup"><span data-stu-id="2cf72-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="2cf72-126">Implementare il metodo MigrateCart() come segue.</span><span class="sxs-lookup"><span data-stu-id="2cf72-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="2cf72-127">In checkout.aspx utilizzeremo un controllo EntityDataSource e un controllo GridView in, vedere la pagina quanto abbiamo nella nostra pagina carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="2cf72-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="2cf72-128">Si noti che il controllo GridView specifica un gestore di evento "ondatabound" denominato MyList\_RowDataBound questo punto implementare il gestore dell'evento simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="2cf72-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="2cf72-129">Mantenere questo metodo un totale parziale del Market carrello come ogni riga è associata e aggiorna la riga inferiore di GridView.</span><span class="sxs-lookup"><span data-stu-id="2cf72-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="2cf72-130">In questa fase è stata implementata una presentazione di "verifica" nell'ordine da inserire.</span><span class="sxs-lookup"><span data-stu-id="2cf72-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="2cf72-131">Verrà gestito un scenario del carrello vuoto mediante l'aggiunta di alcune righe di codice alla pagina dei\_evento di caricamento:</span><span class="sxs-lookup"><span data-stu-id="2cf72-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="2cf72-132">Quando l'utente fa clic sul pulsante "Invia" eseguiamo il codice seguente nel gestore di evento di fare clic su pulsante Submit.</span><span class="sxs-lookup"><span data-stu-id="2cf72-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="2cf72-133">"Carne" del processo di invio dell'ordine è per essere implementato nel metodo della classe organizzazione MyShoppingCart SubmitOrder().</span><span class="sxs-lookup"><span data-stu-id="2cf72-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="2cf72-134">SubmitOrder sarà:</span><span class="sxs-lookup"><span data-stu-id="2cf72-134">SubmitOrder will:</span></span>

- <span data-ttu-id="2cf72-135">Eseguire tutti gli articoli nel carrello acquisti e utilizzarli per creare un nuovo Record di ordine e i record OrderDetails associati.</span><span class="sxs-lookup"><span data-stu-id="2cf72-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="2cf72-136">Calcolare la data di spedizione.</span><span class="sxs-lookup"><span data-stu-id="2cf72-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="2cf72-137">Deselezionare il carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="2cf72-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="2cf72-138">Ai fini di questa applicazione di esempio è sarà calcolare una data di spedizione semplicemente aggiungendo due giorni alla data corrente.</span><span class="sxs-lookup"><span data-stu-id="2cf72-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="2cf72-139">Esegue l'applicazione ora consentirà per testare il processo di acquisto dall'inizio alla fine.</span><span class="sxs-lookup"><span data-stu-id="2cf72-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2cf72-140">[Precedente](tailspin-spyworks-part-5.md)
[Successivo](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="2cf72-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
