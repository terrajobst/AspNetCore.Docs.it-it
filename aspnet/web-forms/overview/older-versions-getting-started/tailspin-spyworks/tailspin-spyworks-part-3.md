---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: Layout e Menu per la categoria | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 72642610c203127b431e03214b2f1bc85a85b5fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366647"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="2a87c-104">Parte 3: Layout e Menu per la categoria</span><span class="sxs-lookup"><span data-stu-id="2a87c-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="2a87c-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="2a87c-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="2a87c-106">Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="2a87c-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="2a87c-107">Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="2a87c-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="2a87c-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="2a87c-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="2a87c-109">Parte 3 viene illustrata l'aggiunta di layout e un menu di categoria.</span><span class="sxs-lookup"><span data-stu-id="2a87c-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="2a87c-110">Aggiunta di alcuni Layout e un Menu di categoria</span><span class="sxs-lookup"><span data-stu-id="2a87c-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="2a87c-111">In questa pagina master del sito verrà aggiunto un elemento div per la colonna di sinistra che conterrà il menu di scelta di categoria di prodotto.</span><span class="sxs-lookup"><span data-stu-id="2a87c-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="2a87c-112">Si noti che l'allineamento desiderato e altre opzioni di formattazione saranno disponibili per la classe CSS che abbiamo aggiunto al file Style. CSS.</span><span class="sxs-lookup"><span data-stu-id="2a87c-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="2a87c-113">Nel menu di categoria di prodotto verrà creato in modo dinamico in fase di esecuzione da una query sul database di e-Commerce per i collegamenti esistenti categorie di prodotti e la creazione di voci di menu e corrispondente.</span><span class="sxs-lookup"><span data-stu-id="2a87c-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="2a87c-114">A tale scopo si userà due di ASP. Controlli avanzati per i dati di NET.</span><span class="sxs-lookup"><span data-stu-id="2a87c-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="2a87c-115">Il controllo "Entità origine di dati" e il controllo "ListView".</span><span class="sxs-lookup"><span data-stu-id="2a87c-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="2a87c-116">È possibile passare alla "Visualizzazione di progettazione" e usare gli helper per configurare i controlli.</span><span class="sxs-lookup"><span data-stu-id="2a87c-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="2a87c-117">È possibile impostare la proprietà ID di EntityDataSource per EDS\_categoria\_Menu e fare clic su "Configura origine dati".</span><span class="sxs-lookup"><span data-stu-id="2a87c-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="2a87c-118">Selezionare la connessione CommerceEntities che è stato creato automaticamente quando abbiamo creato il modello di origine dati di entità per il Database di e-Commerce e fare clic su "Avanti".</span><span class="sxs-lookup"><span data-stu-id="2a87c-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="2a87c-119">Selezionare il nome del set di entità "Categorie" e lasciare le altre opzioni come predefinito.</span><span class="sxs-lookup"><span data-stu-id="2a87c-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="2a87c-120">Fare clic su "Fine".</span><span class="sxs-lookup"><span data-stu-id="2a87c-120">Click "Finish".</span></span>

<span data-ttu-id="2a87c-121">A questo punto è possibile impostare la proprietà ID dell'istanza del controllo ListView che è stato inserito nella nostra pagina ListView\_ProductsMenu e attivare il supporto.</span><span class="sxs-lookup"><span data-stu-id="2a87c-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="2a87c-122">Tuttavia è possibile utilizzare le opzioni di controllo per formattare la visualizzazione di elementi di dati e formattazione, la creazione di menu sarà necessari solo markup semplice verrà immettere il codice nella visualizzazione origine.</span><span class="sxs-lookup"><span data-stu-id="2a87c-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="2a87c-123">Si noti l'istruzione "Eval": &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="2a87c-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="2a87c-124">La sintassi ASP.NET &lt;% # %&gt; è una convenzione a sintassi abbreviata che indica al runtime di eseguire qualsiasi elemento è contenuto all'interno e restituire i risultati "nella riga".</span><span class="sxs-lookup"><span data-stu-id="2a87c-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="2a87c-125">L'istruzione Eval("CategoryName") indica che, per la voce corrente nella raccolta associata di elementi di dati, recuperare il valore dei nomi di elemento Entity Model "CatagoryName".</span><span class="sxs-lookup"><span data-stu-id="2a87c-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="2a87c-126">Si tratta di una sintassi concisa per una funzionalità molto potente.</span><span class="sxs-lookup"><span data-stu-id="2a87c-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="2a87c-127">Consente di eseguire ora l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2a87c-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="2a87c-128">Si noti che viene ora visualizzato il menu di scelta di categoria di prodotto e quando si al passaggio del mouse su una delle voci di menu categoria noteremo i punti di collegamento elemento menu a una pagina si è ancora necessario implementare denominato ProductsList.aspx e che è stato creato un argomento di stringa di query dinamica che contiene il  id della categoria.</span><span class="sxs-lookup"><span data-stu-id="2a87c-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2a87c-129">[Precedente](tailspin-spyworks-part-2.md)
> [Successivo](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="2a87c-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
