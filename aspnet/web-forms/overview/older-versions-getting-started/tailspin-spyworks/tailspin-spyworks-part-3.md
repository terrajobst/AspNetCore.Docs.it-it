---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: Layout e i Menu categoria | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 57c0342efb67b94a0d8c8b06dc13a727e7184db8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="a9e76-104">Parte 3: Layout e i Menu categoria</span><span class="sxs-lookup"><span data-stu-id="a9e76-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="a9e76-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="a9e76-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="a9e76-106">Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="a9e76-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="a9e76-107">Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="a9e76-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="a9e76-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="a9e76-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="a9e76-109">Parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.</span><span class="sxs-lookup"><span data-stu-id="a9e76-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a><span data-ttu-id="a9e76-110">Aggiunta di un Layout e un Menu di categoria</span><span class="sxs-lookup"><span data-stu-id="a9e76-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="a9e76-111">In questa pagina master sito verrà aggiunto un elemento div per la colonna di sinistra che conterrà il menu di categoria di prodotto.</span><span class="sxs-lookup"><span data-stu-id="a9e76-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="a9e76-112">Si noti che l'allineamento desiderato e altri elementi di formattazione vengano fornite dalla classe CSS che abbiamo aggiunto al file Style.css.</span><span class="sxs-lookup"><span data-stu-id="a9e76-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="a9e76-113">Il menu di categoria di prodotto verrà creato in modo dinamico in fase di esecuzione da una query sul database di Commerce per i collegamenti esistenti categorie di prodotti e la creazione di voci di menu e corrispondente.</span><span class="sxs-lookup"><span data-stu-id="a9e76-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="a9e76-114">A tale scopo si utilizzeranno due di ASP. Controlli di dati potenti della rete.</span><span class="sxs-lookup"><span data-stu-id="a9e76-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="a9e76-115">Il controllo di "Origine dati di entità" e il controllo "ListView di".</span><span class="sxs-lookup"><span data-stu-id="a9e76-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="a9e76-116">Si passa a "Visualizzazione di progettazione" e utilizzare gli helper per configurare i controlli.</span><span class="sxs-lookup"><span data-stu-id="a9e76-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="a9e76-117">Consente di impostare la proprietà di EntityDataSource ID su ripartizione\_categoria\_Menu e fare clic su "Configura origine dati".</span><span class="sxs-lookup"><span data-stu-id="a9e76-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="a9e76-118">Selezionare la connessione CommerceEntities creato automaticamente quando abbiamo creato il modello di origine dati di entità per il Database Commerce e fare clic su "Avanti".</span><span class="sxs-lookup"><span data-stu-id="a9e76-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="a9e76-119">Selezionare il nome del set di entità "Categorie" e lasciare il resto delle opzioni come predefinito.</span><span class="sxs-lookup"><span data-stu-id="a9e76-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="a9e76-120">Fare clic su "Fine".</span><span class="sxs-lookup"><span data-stu-id="a9e76-120">Click "Finish".</span></span>

<span data-ttu-id="a9e76-121">A questo punto è necessario impostare la proprietà ID dell'istanza del controllo ListView che è stato inserito nella nostra pagina ListView\_ProductsMenu e attivare il supporto.</span><span class="sxs-lookup"><span data-stu-id="a9e76-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="a9e76-122">Tuttavia è possibile usare le opzioni di controllo per la formattazione della visualizzazione dell'elemento dati e formattazione, la creazione di menu sarà necessari solo il markup semplice verrà immettere il codice nella visualizzazione origine.</span><span class="sxs-lookup"><span data-stu-id="a9e76-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="a9e76-123">Si noti l'istruzione "Eval": &lt;% # % Eval("CategoryName")&gt;</span><span class="sxs-lookup"><span data-stu-id="a9e76-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="a9e76-124">La sintassi ASP.NET &lt;% # %&gt; è una convenzione a sintassi abbreviata che indica al runtime di eseguire il contenuto è contenuto all'interno e i risultati "nella riga".</span><span class="sxs-lookup"><span data-stu-id="a9e76-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="a9e76-125">L'istruzione Eval("CategoryName") indica che, per la voce corrente nella raccolta associata di elementi di dati, recuperare il valore dei nomi di elemento di modello di entità "CatagoryName".</span><span class="sxs-lookup"><span data-stu-id="a9e76-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="a9e76-126">Questa è una sintassi concisa per una funzionalità molto potente.</span><span class="sxs-lookup"><span data-stu-id="a9e76-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="a9e76-127">Consente di eseguire l'applicazione adesso.</span><span class="sxs-lookup"><span data-stu-id="a9e76-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="a9e76-128">Si noti che verrà visualizzato il menu di categoria di prodotto e quando si posiziona su una delle voci di menu categoria, è possibile osservare i punti di collegamento elemento di menu a una pagina è ancora necessario implementare denominato ProductsList.aspx e che è stato compilato un argomento di stringa di query dinamica che contiene il  id della categoria.</span><span class="sxs-lookup"><span data-stu-id="a9e76-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a9e76-129">[Precedente](tailspin-spyworks-part-2.md)
[Successivo](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="a9e76-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
