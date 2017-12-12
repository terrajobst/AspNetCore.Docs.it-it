---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: L'installazione di un Helper in un Web ASP.NET di pagine del sito (Razor) | Documenti Microsoft
author: tfitzmac
description: "In questo articolo viene descritto come installare un helper in un sito Web ASP.NET Web Pages (Razor). Un helper è un componente riutilizzabile che include codice e markup per..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 842c5a56d14314217c1e6ad6d48ded28d3cc5b4e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="85712-104">L'installazione di un file di supporto nel sito Web ASP.NET (Razor) pagine</span><span class="sxs-lookup"><span data-stu-id="85712-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="85712-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="85712-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="85712-106">In questo articolo viene descritto come installare un helper in un sito Web ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="85712-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="85712-107">Oggetto *helper* è un componente riutilizzabile che include codice e markup per eseguire un'attività che potrebbe essere noiosa o complessi.</span><span class="sxs-lookup"><span data-stu-id="85712-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="85712-108">Illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="85712-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="85712-109">Come installare un helper in un sito Web creato utilizzando WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="85712-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="85712-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="85712-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="85712-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="85712-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="85712-112">Panoramica di helper</span><span class="sxs-lookup"><span data-stu-id="85712-112">Overview of Helpers</span></span>

<span data-ttu-id="85712-113">Alcune attività che gli utenti desiderano spesso nelle pagine web richiedono una grande quantità di codice o richiedono la conoscenza aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="85712-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="85712-114">Esempi includono la visualizzazione di un grafico per i dati. inserimento di un pulsante di "Seguire" Twitter in una pagina. Invio messaggio di posta elettronica dal sito Web; il ritaglio e ridimensionamento di immagini. utilizzo di PayPal per il sito.</span><span class="sxs-lookup"><span data-stu-id="85712-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="85712-115">Per consentire un facile eseguire questi tipi di elementi, le pagine Web ASP.NET consente di utilizzare *helper*.</span><span class="sxs-lookup"><span data-stu-id="85712-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="85712-116">Gli helper sono componenti che si installa per un sito e che consentono di eseguono attività comuni usando solo una riga o codice Razor.</span><span class="sxs-lookup"><span data-stu-id="85712-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="85712-117">Le pagine Web ASP.NET dispone di alcuni helper compilato.</span><span class="sxs-lookup"><span data-stu-id="85712-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="85712-118">Tuttavia, molte funzioni di supporto sono disponibili nei pacchetti che sono forniti usando Gestione pacchetti NuGet (componenti aggiuntivi).</span><span class="sxs-lookup"><span data-stu-id="85712-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="85712-119">NuGet consente di selezionare un pacchetto da installare e quindi si occupa di tutti i dettagli dell'installazione.</span><span class="sxs-lookup"><span data-stu-id="85712-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="85712-120">L'installazione di un Helper in WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="85712-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="85712-121">In WebMatrix 3, fare clic su di **NuGet** pulsante.</span><span class="sxs-lookup"><span data-stu-id="85712-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Nella finestra di dialogo Raccolta NuGet WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="85712-123">Verrà avviato Gestione pacchetti NuGet e visualizza i pacchetti disponibili.</span><span class="sxs-lookup"><span data-stu-id="85712-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="85712-124">Nella casella di ricerca, immettere una parola chiave per il supporto di cui che si desidera installare.</span><span class="sxs-lookup"><span data-stu-id="85712-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Nella finestra di dialogo Raccolta NuGet WebMatrix](installing-helpers/_static/image2.png)
- <span data-ttu-id="85712-126">Selezionare il pacchetto e quindi fare clic su **installare**.</span><span class="sxs-lookup"><span data-stu-id="85712-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="85712-127">Fare clic su **Sì** quando viene richiesto se si desidera installare il pacchetto e indicare che si accettano le condizioni.</span><span class="sxs-lookup"><span data-stu-id="85712-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

    <span data-ttu-id="85712-128">Se questa è la prima volta che è stato installato un helper, NuGet consente di creare cartelle nel sito Web per il codice che costituisce l'helper.</span><span class="sxs-lookup"><span data-stu-id="85712-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
- <span data-ttu-id="85712-129">Per disinstallare un helper, fare clic su di **raccolta** fare clic su di **installato** scheda e selezionare il pacchetto che si desidera disinstallare.</span><span class="sxs-lookup"><span data-stu-id="85712-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="85712-130">L'installazione l'helper di Twitter</span><span class="sxs-lookup"><span data-stu-id="85712-130">Installing the Twitter helper</span></span>

<span data-ttu-id="85712-131">La versione più recente dell'API di Twitter non è compatibile con l'helper di Twitter che si installa tramite NuGet.</span><span class="sxs-lookup"><span data-stu-id="85712-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="85712-132">In alternativa, vedere il [Helper di Twitter con WebMatrix](twitter-helper.md) argomento per informazioni su come configurare l'helper di Twitter nel progetto.</span><span class="sxs-lookup"><span data-stu-id="85712-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="85712-133">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="85712-133">Additional Resources</span></span>


[<span data-ttu-id="85712-134">Introduzione di ASP.NET Web Pages 2 - nozioni di base sulla programmazione</span><span class="sxs-lookup"><span data-stu-id="85712-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="85712-135">Helper di Twitter con WebMatrix</span><span class="sxs-lookup"><span data-stu-id="85712-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
