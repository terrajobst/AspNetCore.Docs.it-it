---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installazione di un Helper in un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: Questo articolo descrive come installare un helper in un sito Web ASP.NET Web Pages (Razor). Un helper è un componente riutilizzabile che include il codice e markup per...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 38290fd47355e7893eddd1f867f47b113b54ca7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361806"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="270cf-104">Installazione di un Helper in un sito di ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="270cf-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="270cf-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="270cf-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="270cf-106">Questo articolo descrive come installare un helper in un sito Web ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="270cf-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="270cf-107">Oggetto *helper* è un componente riutilizzabile che include il codice e markup per eseguire un'attività che potrebbe essere noioso o complessi.</span><span class="sxs-lookup"><span data-stu-id="270cf-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="270cf-108">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="270cf-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="270cf-109">Come installare un helper in un sito Web creato utilizzando WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="270cf-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="270cf-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="270cf-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="270cf-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="270cf-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="270cf-112">Panoramica degli helper</span><span class="sxs-lookup"><span data-stu-id="270cf-112">Overview of Helpers</span></span>

<span data-ttu-id="270cf-113">Alcune attività che spesso gli utenti vogliono eseguire operazioni sulle pagine web richiedono una grande quantità di codice o della Knowledge Base aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="270cf-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="270cf-114">Gli esempi includono la visualizzazione di un grafico per i dati. inserimento di un pulsante "Segui" Twitter in una pagina. Invio messaggio di posta elettronica tramite il sito Web; ritaglio o ridimensionamento delle immagini; utilizzo di PayPal per il sito.</span><span class="sxs-lookup"><span data-stu-id="270cf-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="270cf-115">Per renderlo semplice eseguire questo tipo di attività, ASP.NET Web Pages ti permette di usare *helper*.</span><span class="sxs-lookup"><span data-stu-id="270cf-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="270cf-116">Gli helper sono componenti installare per un sito e che consentono di eseguono attività tipiche usando solo una o due righe di codice Razor.</span><span class="sxs-lookup"><span data-stu-id="270cf-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="270cf-117">ASP.NET Web Pages ha alcuni helper incorporato.</span><span class="sxs-lookup"><span data-stu-id="270cf-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="270cf-118">Tuttavia, molti helper sono disponibili nei pacchetti forniti tramite Gestione pacchetti NuGet (componenti aggiuntivi).</span><span class="sxs-lookup"><span data-stu-id="270cf-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="270cf-119">NuGet consente di selezionare un pacchetto da installare e quindi si occupa di tutti i dettagli dell'installazione.</span><span class="sxs-lookup"><span data-stu-id="270cf-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="270cf-120">Installazione di un Helper di WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="270cf-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="270cf-121">In WebMatrix 3, fare clic sui **NuGet** pulsante.</span><span class="sxs-lookup"><span data-stu-id="270cf-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Finestra di dialogo NuGet Gallery in WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="270cf-123">Verrà avviato Gestione pacchetti NuGet e visualizza i pacchetti disponibili.</span><span class="sxs-lookup"><span data-stu-id="270cf-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="270cf-124">Nella casella di ricerca, immettere una parola chiave per l'helper in cui che si vuole installare.</span><span class="sxs-lookup"><span data-stu-id="270cf-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Finestra di dialogo NuGet Gallery in WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="270cf-126">Selezionare il pacchetto e quindi fare clic su **installare**.</span><span class="sxs-lookup"><span data-stu-id="270cf-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="270cf-127">Fare clic su **Sì** quando viene richiesto se si desidera installare il pacchetto e indicare che si accettano le condizioni.</span><span class="sxs-lookup"><span data-stu-id="270cf-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="270cf-128">Se questa è la prima volta che è stato installato un file di supporto, NuGet crea le cartelle nel sito Web per il codice che costituisce l'helper.</span><span class="sxs-lookup"><span data-stu-id="270cf-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="270cf-129">Per disinstallare un helper, fare clic sui **Gallery** e fare clic il **installato** scheda e selezionare il pacchetto da disinstallare.</span><span class="sxs-lookup"><span data-stu-id="270cf-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="270cf-130">Installare l'helper di Twitter</span><span class="sxs-lookup"><span data-stu-id="270cf-130">Installing the Twitter helper</span></span>

<span data-ttu-id="270cf-131">La versione più recente dell'API di Twitter non è compatibile con l'helper di Twitter che si installa tramite NuGet.</span><span class="sxs-lookup"><span data-stu-id="270cf-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="270cf-132">Al contrario, vedere la [Helper di Twitter con WebMatrix](twitter-helper.md) argomento per informazioni su come configurare l'helper di Twitter nel progetto.</span><span class="sxs-lookup"><span data-stu-id="270cf-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="270cf-133">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="270cf-133">Additional Resources</span></span>


[<span data-ttu-id="270cf-134">Introduzione ad ASP.NET Web Pages 2 - nozioni fondamentali di programmazione</span><span class="sxs-lookup"><span data-stu-id="270cf-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="270cf-135">Helper di Twitter con WebMatrix</span><span class="sxs-lookup"><span data-stu-id="270cf-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
