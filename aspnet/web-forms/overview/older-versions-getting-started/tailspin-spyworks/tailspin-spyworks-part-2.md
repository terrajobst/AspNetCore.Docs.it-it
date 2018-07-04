---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Nella parte 2: Livello di accesso ai dati | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 2 illustra l'aggiunta di livello di accesso ai dati.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 13abb02e76b3af80aa11d09e75dc223403917804
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382569"
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="f6ddd-104">Parte 2: Livello di accesso ai dati</span><span class="sxs-lookup"><span data-stu-id="f6ddd-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="f6ddd-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="f6ddd-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="f6ddd-106">Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="f6ddd-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="f6ddd-107">Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="f6ddd-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="f6ddd-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="f6ddd-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="f6ddd-109">Parte 2 illustra l'aggiunta di livello di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="f6ddd-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a>  <span data-ttu-id="f6ddd-110">Aggiunta di livello di accesso ai dati</span><span class="sxs-lookup"><span data-stu-id="f6ddd-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="f6ddd-111">L'applicazione di e-commerce dipenderà da due database.</span><span class="sxs-lookup"><span data-stu-id="f6ddd-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="f6ddd-112">Per informazioni sui clienti useremo database di appartenenze ASP.NET standard.</span><span class="sxs-lookup"><span data-stu-id="f6ddd-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="f6ddd-113">Per il nostro catalogo di prodotto e carrello acquisti implementeremo un database SQL Express come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f6ddd-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="f6ddd-114">Dopo aver creato il database (Commerce.mdf) nell'App dell'applicazione\_cartella dati è possibile procedere per creare il livello di accesso ai dati usando .NET Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f6ddd-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="f6ddd-115">Si creerà una cartella denominata "dati\_accesso" e fare clic con il pulsante destro su tale cartella e scegliere "Aggiungi nuovo elemento".</span><span class="sxs-lookup"><span data-stu-id="f6ddd-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="f6ddd-116">In "Modelli installati" elemento e quindi selezionare "ADO.NET Entity Data Model" Immettere EDM\_Commerce.edmx come il nome e fare clic sul pulsante "Aggiungi".</span><span class="sxs-lookup"><span data-stu-id="f6ddd-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="f6ddd-117">Scegliere "Genera da Database".</span><span class="sxs-lookup"><span data-stu-id="f6ddd-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="f6ddd-118">Salvare e compilare.</span><span class="sxs-lookup"><span data-stu-id="f6ddd-118">Save and build.</span></span>

<span data-ttu-id="f6ddd-119">A questo punto siamo pronti aggiungere la prima funzionalità – un menu di categoria di prodotto.</span><span class="sxs-lookup"><span data-stu-id="f6ddd-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f6ddd-120">[Precedente](tailspin-spyworks-part-1.md)
> [Successivo](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="f6ddd-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
