---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: Appartenenza ASP.NET | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 6 aggiunge le appartenenze di ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: c847db058bc03115210f12eeb0c3c76fecc8a31e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834312"
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="bb118-104">Parte 6: Appartenenza ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bb118-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="bb118-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="bb118-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="bb118-106">Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="bb118-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="bb118-107">Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="bb118-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="bb118-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="bb118-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="bb118-109">Parte 6 aggiunge le appartenenze di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bb118-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="bb118-110">Utilizzo di appartenenza ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bb118-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="bb118-111">Fare clic su sicurezza</span><span class="sxs-lookup"><span data-stu-id="bb118-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="bb118-112">Assicurarsi che si sta usando l'autenticazione basata su form.</span><span class="sxs-lookup"><span data-stu-id="bb118-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="bb118-113">Usare il collegamento "Create User" per creare un paio di utenti.</span><span class="sxs-lookup"><span data-stu-id="bb118-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="bb118-114">Al termine, fare riferimento alla finestra Esplora soluzioni e aggiornare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="bb118-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="bb118-115">Si noti che il file ASPNETDB. È stato creato correttamente MDF.</span><span class="sxs-lookup"><span data-stu-id="bb118-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="bb118-116">Questo file contiene le tabelle per supportare i servizi ASP.NET core, ad esempio l'appartenenza.</span><span class="sxs-lookup"><span data-stu-id="bb118-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="bb118-117">A questo punto è possibile iniziare a implementare il processo di estrazione.</span><span class="sxs-lookup"><span data-stu-id="bb118-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="bb118-118">Iniziare creando una pagina CheckOut.aspx.</span><span class="sxs-lookup"><span data-stu-id="bb118-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="bb118-119">La pagina CheckOut.aspx solo deve essere disponibile per gli utenti che sono registrati in modo che Microsoft limiterà l'accesso a effettuato l'accesso degli utenti e reindirizzerà gli utenti che non sono connessi alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="bb118-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="bb118-120">A questo scopo si aggiungerà quanto segue alla sezione di configurazione del file Web. config.</span><span class="sxs-lookup"><span data-stu-id="bb118-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="bb118-121">Il modello per le applicazioni Web Form ASP.NET automaticamente aggiunta una sezione di autenticazione al file Web. config e stabilito la pagina di accesso predefinito.</span><span class="sxs-lookup"><span data-stu-id="bb118-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="bb118-122">È necessario modificare il codice aspx file per eseguire la migrazione di un carrello acquisti anonimo quando l'utente accede code-behind.</span><span class="sxs-lookup"><span data-stu-id="bb118-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="bb118-123">La pagina Cambia\_evento di caricamento come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="bb118-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="bb118-124">Aggiungere quindi un gestore dell'evento "LoggedIn" simile al seguente per impostare il nome della sessione per l'utente appena connesso e modificare l'id sessione temporaneo nel carrello acquisti in quello dell'utente chiamando il metodo MigrateCart nella nostra classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="bb118-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="bb118-125">(Implementata nel file con estensione cs)</span><span class="sxs-lookup"><span data-stu-id="bb118-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="bb118-126">Implementare il metodo MigrateCart() simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="bb118-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="bb118-127">In checkout.aspx useremo un controllo EntityDataSource e un controllo GridView in, vedere la pagina così come è stato fatto nella nostra pagina carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="bb118-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="bb118-128">Si noti che il controllo GridView specifica un gestore di evento "ondatabound" denominato MyList\_RowDataBound così è possibile implementare tale gestore eventi simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="bb118-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="bb118-129">Questo metodo modo, viene mantenuto un totale parziale del carrello come ogni riga è associata e aggiorni la riga nella parte inferiore di GridView.</span><span class="sxs-lookup"><span data-stu-id="bb118-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="bb118-130">A questo punto sono state implementate una presentazione di "recensione" dell'ordine da inserire.</span><span class="sxs-lookup"><span data-stu-id="bb118-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="bb118-131">È possibile gestire un scenario del carrello vuoto mediante l'aggiunta di poche righe di codice alla nostra pagina\_evento di caricamento:</span><span class="sxs-lookup"><span data-stu-id="bb118-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="bb118-132">Quando l'utente fa clic sul pulsante "Invia" si eseguirà il codice seguente nel gestore dell'evento fare clic sul pulsante Submit.</span><span class="sxs-lookup"><span data-stu-id="bb118-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="bb118-133">La sostanza"" del processo di invio dell'ordine è per essere implementata nel metodo SubmitOrder() della nostra classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="bb118-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="bb118-134">SubmitOrder sarà:</span><span class="sxs-lookup"><span data-stu-id="bb118-134">SubmitOrder will:</span></span>

- <span data-ttu-id="bb118-135">Eseguire tutte le voci nel carrello acquisti e usarli per creare un nuovo Record di ordine e i record OrderDetails associati.</span><span class="sxs-lookup"><span data-stu-id="bb118-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="bb118-136">Calcola la data di spedizione.</span><span class="sxs-lookup"><span data-stu-id="bb118-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="bb118-137">Deselezionare il carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="bb118-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="bb118-138">Ai fini di questa applicazione di esempio sarà calcoliamo una data di spedizione semplicemente aggiungendo due giorni alla data corrente.</span><span class="sxs-lookup"><span data-stu-id="bb118-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="bb118-139">Esecuzione dell'applicazione ora consentirà per testare il processo di acquisto dall'inizio alla fine.</span><span class="sxs-lookup"><span data-stu-id="bb118-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bb118-140">[Precedente](tailspin-spyworks-part-5.md)
> [Successivo](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="bb118-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
