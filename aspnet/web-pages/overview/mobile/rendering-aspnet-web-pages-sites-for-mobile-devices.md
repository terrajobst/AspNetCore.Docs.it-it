---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Il rendering di ASP.NET Web Pages siti (Razor) per i dispositivi mobili | Documenti Microsoft
author: tfitzmac
description: "In questo articolo viene descritto come creare pagine in un sito di pagine Web ASP.NET (Razor) che verrà eseguito il rendering in modo appropriato nei dispositivi mobili. Si apprenderà: come è..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 899bbdef82d689be81cd77ea6805e0484fb614aa
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="05dd9-104">Il rendering di siti (Razor) le pagine Web ASP.NET per dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="05dd9-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="05dd9-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="05dd9-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="05dd9-106">In questo articolo viene descritto come creare pagine in un sito di pagine Web ASP.NET (Razor) che verrà eseguito il rendering in modo appropriato nei dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="05dd9-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="05dd9-107">Illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="05dd9-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="05dd9-108">Come utilizzare una convenzione di denominazione per specificare che una pagina è progettato specificamente per i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="05dd9-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="05dd9-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="05dd9-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="05dd9-110">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="05dd9-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="05dd9-111">In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="05dd9-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="05dd9-112">Le pagine Web ASP.NET consente di creare visualizzazioni personalizzate per il rendering del contenuto su dispositivi mobili o altri dispositivi.</span><span class="sxs-lookup"><span data-stu-id="05dd9-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="05dd9-113">Il modo più semplice per creare la pagina specifico del dispositivo in un sito ASP.NET Web Pages è utilizzando un modello di denominazione dei file simile al seguente: *FileName. **Mobile**. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="05dd9-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.**Mobile**.cshtml*.</span></span> <span data-ttu-id="05dd9-114">È possibile creare due versioni di una pagina (ad esempio, uno denominato *MyFile.cshtml* e uno denominato *MyFile.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="05dd9-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="05dd9-115">In fase di esecuzione, quando un dispositivo mobile richiede *MyFile.cshtml*, ASP.NET esegue il rendering del contenuto dalla *MyFile.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="05dd9-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="05dd9-116">In caso contrario, *MyFile.cshtml* viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="05dd9-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="05dd9-117">Nell'esempio seguente viene illustrato come attivare il rendering di dispositivi mobili mediante l'aggiunta di una pagina contenuto per i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="05dd9-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="05dd9-118">*Page1.cshtml* contiene contenuto più di una barra laterale di navigazione.</span><span class="sxs-lookup"><span data-stu-id="05dd9-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="05dd9-119">*Page1.Mobile.cshtml* contiene lo stesso contenuto, ma omette l'intestazione laterale.</span><span class="sxs-lookup"><span data-stu-id="05dd9-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="05dd9-120">In un sito ASP.NET Web Pages, creare un file denominato *Page1.cshtml* e sostituire il contenuto corrente con il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="05dd9-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="05dd9-121">Creare un file denominato *Page1.Mobile.cshtml* e sostituire il contenuto esistente con il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="05dd9-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="05dd9-122">Si noti che la versione per dispositivi mobili della pagina omette la sezione di spostamento per una migliore riproduzione su uno schermo piccolo.</span><span class="sxs-lookup"><span data-stu-id="05dd9-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="05dd9-123">Eseguire un browser per desktop e passare a *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="05dd9-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="05dd9-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="05dd9-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="05dd9-125">Eseguire un browser per dispositivi mobili (o un emulatore di dispositivo mobile) e passare a *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="05dd9-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="05dd9-126">(Si noti che non si include *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="05dd9-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="05dd9-127">come parte dell'URL.) Anche se la richiesta è su *Page1.cshtml*, ASP.NET esegue il rendering *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="05dd9-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="05dd9-129">Per testare pagine per dispositivi mobili, è possibile utilizzare un simulatore di dispositivi mobili che viene eseguito in un computer desktop.</span><span class="sxs-lookup"><span data-stu-id="05dd9-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="05dd9-130">Questo strumento consente di testare le pagine web come appaiono nei dispositivi mobili (ovvero, in genere con molto piccola Visualizza area).</span><span class="sxs-lookup"><span data-stu-id="05dd9-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="05dd9-131">È un esempio di un simulatore di [componente aggiuntivo di selezione dell'agente utente](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) per Mozilla Firefox, che consente di emulare diversi browser per dispositivi mobili da una versione desktop di Firefox.</span><span class="sxs-lookup"><span data-stu-id="05dd9-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="05dd9-132">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="05dd9-132">Additional Resources</span></span>


<span data-ttu-id="05dd9-133">[Emulatore Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="05dd9-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
