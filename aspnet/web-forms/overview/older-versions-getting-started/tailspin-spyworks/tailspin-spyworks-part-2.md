---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: Livello di accesso ai dati | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 2 viene illustrata l'aggiunta del livello di accesso ai dati.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f734b04a0f4cec3c33bc5b42ef283ea64cdb463
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890489"
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="ba1d4-104">Parte 2: Livello di accesso ai dati</span><span class="sxs-lookup"><span data-stu-id="ba1d4-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="ba1d4-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="ba1d4-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="ba1d4-106">Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="ba1d4-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="ba1d4-107">Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="ba1d4-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="ba1d4-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="ba1d4-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="ba1d4-109">Parte 2 viene illustrata l'aggiunta del livello di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="ba1d4-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a>  <span data-ttu-id="ba1d4-110">Aggiunta di livello di accesso ai dati</span><span class="sxs-lookup"><span data-stu-id="ba1d4-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="ba1d4-111">L'applicazione di e-commerce dipenderà da due database.</span><span class="sxs-lookup"><span data-stu-id="ba1d4-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="ba1d4-112">Per informazioni sul cliente verrà usato il database di sistema di appartenenze ASP.NET standard.</span><span class="sxs-lookup"><span data-stu-id="ba1d4-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="ba1d4-113">Per il nostro carrello catalogo carrello e il prodotto è un database di SQL Express viene implementato come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="ba1d4-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="ba1d4-114">Dopo aver creato il database (Commerce.mdf) nell'App dell'applicazione\_cartella dati è possibile procedere per creare il livello di accesso ai dati tramite Entity Framework .NET.</span><span class="sxs-lookup"><span data-stu-id="ba1d4-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="ba1d4-115">Verrà creata una cartella denominata "dati\_accesso" e fare clic con il pulsante destro per tale cartella e scegliere "Aggiungi nuovo elemento".</span><span class="sxs-lookup"><span data-stu-id="ba1d4-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="ba1d4-116">In "Modelli installati" elemento e quindi selezionare "ADO.NET Entity Data Model" Immettere EDM\_Commerce.edmx come il nome e fare clic sul pulsante "Aggiungi".</span><span class="sxs-lookup"><span data-stu-id="ba1d4-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="ba1d4-117">Scegliere "Genera da Database".</span><span class="sxs-lookup"><span data-stu-id="ba1d4-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="ba1d4-118">Salvare e compilare.</span><span class="sxs-lookup"><span data-stu-id="ba1d4-118">Save and build.</span></span>

<span data-ttu-id="ba1d4-119">Si è ora pronti per aggiungere la funzionalità: prima di un menu di categoria di prodotto.</span><span class="sxs-lookup"><span data-stu-id="ba1d4-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ba1d4-120">[Precedente](tailspin-spyworks-part-1.md)
> [Successivo](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="ba1d4-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
