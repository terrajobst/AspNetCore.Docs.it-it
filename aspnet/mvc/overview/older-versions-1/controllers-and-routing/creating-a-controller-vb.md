---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Creazione di un Controller (VB) | Documenti Microsoft
author: StephenWalther
description: In questa esercitazione, Stephen Walther viene illustrato come aggiungere un controller per un'applicazione MVC ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: e9a2bbcb09672f5247429064908cd4d2ef67f518
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879010"
---
<a name="creating-a-controller-vb"></a><span data-ttu-id="c429d-103">Creazione di un Controller (VB)</span><span class="sxs-lookup"><span data-stu-id="c429d-103">Creating a Controller (VB)</span></span>
====================
<span data-ttu-id="c429d-104">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="c429d-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="c429d-105">In questa esercitazione, Stephen Walther viene illustrato come aggiungere un controller per un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c429d-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="c429d-106">L'obiettivo di questa esercitazione è illustrare come è possibile creare nuovi ASP.NET MVC controller.</span><span class="sxs-lookup"><span data-stu-id="c429d-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="c429d-107">Informazioni su come creare controller utilizzando l'opzione di menu di Visual Studio aggiungere Controller e creando manualmente un file di classe.</span><span class="sxs-lookup"><span data-stu-id="c429d-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="c429d-108">Tramite l'aggiunta di Controller di menu</span><span class="sxs-lookup"><span data-stu-id="c429d-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="c429d-109">Il modo più semplice per creare un nuovo controller consiste nella cartella controller nella finestra Esplora soluzioni di Visual Studio e scegliere il **Aggiungi, Controller** opzione di menu (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="c429d-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="c429d-110">Selezionando questa opzione di menu viene visualizzata la **Aggiungi Controller** finestra di dialogo (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="c429d-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="c429d-111">[![La finestra di dialogo Nuovo progetto](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c429d-111">[![The New Project dialog box](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span></span>

<span data-ttu-id="c429d-112">**Figura 01**: aggiunge un nuovo controller ([fare clic per visualizzare l'immagine ingrandita](creating-a-controller-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="c429d-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-vb/_static/image2.png))</span></span>


<span data-ttu-id="c429d-113">[![La finestra di dialogo Nuovo progetto](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c429d-113">[![The New Project dialog box](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span></span>

<span data-ttu-id="c429d-114">**Figura 02**: finestra di dialogo di Aggiungi Controller ([fare clic per visualizzare l'immagine ingrandita](creating-a-controller-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="c429d-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-vb/_static/image4.png))</span></span>


<span data-ttu-id="c429d-115">Si noti che la prima parte del nome del controller è evidenziata nel **Aggiungi Controller** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="c429d-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="c429d-116">Ogni nome del controller deve terminare con il suffisso *Controller*.</span><span class="sxs-lookup"><span data-stu-id="c429d-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="c429d-117">Ad esempio, è possibile creare un controller denominato *ProductController* ma non un controller denominato *prodotto*.</span><span class="sxs-lookup"><span data-stu-id="c429d-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="c429d-118">Se si crea un controller che manca il *Controller* suffisso quindi non sarà in grado di richiamare il controller.</span><span class="sxs-lookup"><span data-stu-id="c429d-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="c429d-119">Non eseguire questa operazione, ovvero ho sprecato moltissimo tempo la scadenza dopo questo errore.</span><span class="sxs-lookup"><span data-stu-id="c429d-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="c429d-120">**Elenco 1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="c429d-120">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

<span data-ttu-id="c429d-121">Nella cartella Controllers, è consigliabile creare sempre i controller.</span><span class="sxs-lookup"><span data-stu-id="c429d-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="c429d-122">In caso contrario, si saranno violano le convenzioni di MVC ASP.NET e altri sviluppatori avrà un'ora più difficile la comprensione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c429d-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="c429d-123">Lo scaffolding di metodi di azione</span><span class="sxs-lookup"><span data-stu-id="c429d-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="c429d-124">Quando si crea un controller, è possibile generare automaticamente i metodi di azione Create, Update e dettagli (vedere la figura 3).</span><span class="sxs-lookup"><span data-stu-id="c429d-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="c429d-125">Se si seleziona questa opzione viene generata la classe di controller nel listato 2.</span><span class="sxs-lookup"><span data-stu-id="c429d-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="c429d-126">[![La creazione automatica di metodi di azione](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c429d-126">[![Creating action methods automatically](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span></span>

<span data-ttu-id="c429d-127">**Figura 03**: la creazione automatica di metodi di azione ([fare clic per visualizzare l'immagine ingrandita](creating-a-controller-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c429d-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-vb/_static/image6.png))</span></span>


<span data-ttu-id="c429d-128">**Listing 2 - Controllers\CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="c429d-128">**Listing 2 - Controllers\CustomerController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

<span data-ttu-id="c429d-129">Questi generati sono i metodi stub.</span><span class="sxs-lookup"><span data-stu-id="c429d-129">These generated methods are stub methods.</span></span> <span data-ttu-id="c429d-130">È necessario aggiungere la logica effettiva per la creazione, aggiornamento e visualizzazione dei dettagli per un cliente manualmente.</span><span class="sxs-lookup"><span data-stu-id="c429d-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="c429d-131">Tuttavia, i metodi stub offrono un punto di partenza nice.</span><span class="sxs-lookup"><span data-stu-id="c429d-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="c429d-132">Creazione di una classe Controller</span><span class="sxs-lookup"><span data-stu-id="c429d-132">Creating a Controller Class</span></span>

<span data-ttu-id="c429d-133">Controller MVC ASP.NET è semplicemente una classe.</span><span class="sxs-lookup"><span data-stu-id="c429d-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="c429d-134">Se si preferisce, è possibile ignorare lo scaffolding di controller pratico di Visual Studio e creare manualmente una classe controller.</span><span class="sxs-lookup"><span data-stu-id="c429d-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="c429d-135">Attenersi ai passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="c429d-135">Follow these steps:</span></span>

1. <span data-ttu-id="c429d-136">Fare clic sulla cartella controller e selezionare l'opzione di menu **Aggiungi, elemento nuovo** e selezionare il **classe** modello (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="c429d-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="c429d-137">Denominare la nuova classe PersonController.vb e fare clic su di **Aggiungi** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c429d-137">Name the new class PersonController.vb and click the **Add** button.</span></span>
3. <span data-ttu-id="c429d-138">Modificare il file di classe risultante in modo che la classe eredita dalla classe base MVC (vedere Listato 3).</span><span class="sxs-lookup"><span data-stu-id="c429d-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="c429d-139">[![Creazione di una nuova classe](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c429d-139">[![Creating a new class](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span></span>

<span data-ttu-id="c429d-140">**Figura 04**: creare una nuova classe ([fare clic per visualizzare l'immagine ingrandita](creating-a-controller-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="c429d-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-vb/_static/image8.png))</span></span>


<span data-ttu-id="c429d-141">**Listing 3 - Controllers\PersonController.vb**</span><span class="sxs-lookup"><span data-stu-id="c429d-141">**Listing 3 - Controllers\PersonController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

<span data-ttu-id="c429d-142">Il controller nel listato 3 espone un'azione denominata index () che restituisce la stringa "Hello World!".</span><span class="sxs-lookup"><span data-stu-id="c429d-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="c429d-143">È possibile richiamare l'azione del controller, che esegue l'applicazione e richiedere un URL simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c429d-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="c429d-144">Il Server di sviluppo ASP.NET utilizza un numero di porta casuale (ad esempio, 40071).</span><span class="sxs-lookup"><span data-stu-id="c429d-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="c429d-145">Quando si immette un URL per richiamare un controller, sarà necessario fornire il numero della porta destra.</span><span class="sxs-lookup"><span data-stu-id="c429d-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="c429d-146">È possibile determinare il numero di porta passando il mouse sull'icona per il Server di sviluppo ASP.NET nell'Area di notifica di Windows (in basso a destra dello schermo).</span><span class="sxs-lookup"><span data-stu-id="c429d-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="c429d-147">[Precedente](adding-dynamic-content-to-a-cached-page-vb.md)
> [Successivo](creating-an-action-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c429d-147">[Previous](adding-dynamic-content-to-a-cached-page-vb.md)
[Next](creating-an-action-vb.md)</span></span>
