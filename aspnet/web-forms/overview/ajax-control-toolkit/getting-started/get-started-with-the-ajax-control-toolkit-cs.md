---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Introduzione a AJAX Control Toolkit (c#) | Microsoft Docs
author: microsoft
description: Scopri tutto che devi sapere per iniziare a usare AJAX Control Toolkit.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 120213ce27858e2f0650b71c82f689ae4493d8a8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399597"
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="9969b-103">Introduzione a AJAX Control Toolkit (c#)</span><span class="sxs-lookup"><span data-stu-id="9969b-103">Get Started with the AJAX Control Toolkit (C#)</span></span>
====================
<span data-ttu-id="9969b-104">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9969b-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="9969b-105">Scopri tutto che devi sapere per iniziare a usare AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="9969b-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="9969b-106">AJAX Control Toolkit contiene più di 30 controlli gratuiti che è possibile usare nelle applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9969b-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="9969b-107">In questa esercitazione descrive come scaricare AJAX Control Toolkit e aggiungere i controlli toolkit per la casella degli strumenti di Visual Studio e Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="9969b-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="9969b-108">Download di AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="9969b-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="9969b-109">Il [AJAX Control Toolkit](http://devexpress.com/act) è un progetto open source sviluppato dai membri della community ASP.NET e il team di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9969b-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 


<span data-ttu-id="9969b-110">[![Download di AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9969b-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="9969b-111">**Figura 01**: download di AJAX Control Toolkit ([fare clic per visualizzare l'immagine con dimensioni normali](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="9969b-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>


<span data-ttu-id="9969b-112">Dopo aver scaricato il file, è necessario sbloccare il file.</span><span class="sxs-lookup"><span data-stu-id="9969b-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="9969b-113">Fare doppio clic su file, selezionare le proprietà e scegliere il **Unblock** pulsante (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="9969b-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="9969b-114">[![Il file ZIP Toolkit controllo AJAX di sblocco](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="9969b-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="9969b-115">**Figura 02**: il file ZIP Toolkit controllo AJAX di sblocco ([fare clic per visualizzare l'immagine con dimensioni normali](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="9969b-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>


<span data-ttu-id="9969b-116">Dopo aver sbloccato il file, è possibile decomprimere il file: il pulsante destro e selezionare il **Estrai tutto** l'opzione di menu.</span><span class="sxs-lookup"><span data-stu-id="9969b-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="9969b-117">A questo punto, siamo pronti aggiungere il toolkit alla casella degli strumenti di Visual Studio e Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="9969b-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="9969b-118">Aggiunta di AJAX Control Toolkit alla casella degli strumenti</span><span class="sxs-lookup"><span data-stu-id="9969b-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="9969b-119">Il modo più semplice usare AJAX Control Toolkit consiste nell'aggiungere il toolkit per la casella degli strumenti di Visual Studio e Visual Web Developer (vedere la figura 3).</span><span class="sxs-lookup"><span data-stu-id="9969b-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="9969b-120">In questo modo, è possibile semplicemente trascinare un controllo toolkit in una pagina quando si desidera utilizzarla.</span><span class="sxs-lookup"><span data-stu-id="9969b-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="9969b-121">[![AJAX Control Toolkit viene visualizzato nella casella degli strumenti](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="9969b-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="9969b-122">**Figura 03**: AJAX Control Toolkit viene visualizzato nella casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni normali](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9969b-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>


<span data-ttu-id="9969b-123">In primo luogo, è necessario aggiungere una scheda di AJAX Control Toolkit alla casella degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="9969b-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="9969b-124">Seguire questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="9969b-124">Follow these steps.</span></span>

1. <span data-ttu-id="9969b-125">Creare un nuovo sito Web ASP.NET selezionando l'opzione di menu File, nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="9969b-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="9969b-126">Fare doppio clic su default. aspx nella finestra Esplora soluzioni per aprire il file nell'editor.</span><span class="sxs-lookup"><span data-stu-id="9969b-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="9969b-127">Fare doppio clic su casella degli strumenti sotto la scheda Impostazioni generali e selezionare l'opzione di menu **Aggiungi scheda** (vedere la figura 4).</span><span class="sxs-lookup"><span data-stu-id="9969b-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="9969b-128">Immettere una nuova scheda denominata AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="9969b-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="9969b-129">[![Aggiunta di una nuova scheda](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="9969b-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="9969b-130">**Figura 04**: aggiungere una nuova scheda ([fare clic per visualizzare l'immagine con dimensioni normali](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="9969b-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>


<span data-ttu-id="9969b-131">Successivamente, è necessario aggiungere i controlli di AJAX Control Toolkit per la nuova scheda. Attenersi ai passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="9969b-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="9969b-132">Fare clic sotto la scheda di AJAX Control Toolkit e selezionare l'opzione di menu **Scegli elementi (vedere la figura 5)**.</span><span class="sxs-lookup"><span data-stu-id="9969b-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="9969b-133">Passare al percorso in cui è stata decompressa AJAX Control Toolkit e selezionare l'assembly di AjaxControlToolkit. dll.</span><span class="sxs-lookup"><span data-stu-id="9969b-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="9969b-134">[![Scegliere gli elementi da aggiungere alla casella degli strumenti](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="9969b-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="9969b-135">**Figura 05**: scegliere gli elementi da aggiungere alla casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni normali](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="9969b-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>


<span data-ttu-id="9969b-136">Dopo aver completato questi passaggi, tutti i controlli toolkit verranno visualizzati nella casella degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="9969b-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="9969b-137">L'aggiornamento a una nuova versione del Toolkit</span><span class="sxs-lookup"><span data-stu-id="9969b-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="9969b-138">Se si usa una versione precedente del Toolkit, è possibile spostare in una versione più recente di seguito sono riportati i passaggi consigliati:</span><span class="sxs-lookup"><span data-stu-id="9969b-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="9969b-139">I file binari - eliminare la versione precedente dell'assembly AjaxControlToolkit. dll dalla cartella Bin del sito Web.</span><span class="sxs-lookup"><span data-stu-id="9969b-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="9969b-140">Gli elementi della casella degli strumenti - Elimina la scheda di AJAX Control Toolkit e seguire i passaggi precedenti per creare di nuovo la scheda con la nuova versione dell'assembly AjaxControlToolkit. dll.</span><span class="sxs-lookup"><span data-stu-id="9969b-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9969b-141">avanti</span><span class="sxs-lookup"><span data-stu-id="9969b-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
