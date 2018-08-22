---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Rendering Web ASP.NET (Razor) siti con pagine per i dispositivi mobili | Microsoft Docs
author: tfitzmac
description: "Questo articolo descrive come creare le pagine in un sito di ASP.NET Web Pages (Razor) che verrà eseguito il rendering in modo appropriato nei dispositivi mobili. Contenuto dell'esercitazione: come è..."
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 7159283efcd4a82d5dff68f2d9e315f804e7cdcb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823957"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="1171e-104">Il rendering di ASP.NET Web Pages (Razor) siti per dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="1171e-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="1171e-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1171e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1171e-106">Questo articolo descrive come creare le pagine in un sito di ASP.NET Web Pages (Razor) che verrà eseguito il rendering in modo appropriato nei dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="1171e-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="1171e-107">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="1171e-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="1171e-108">Come usare una convenzione di denominazione per specificare che una pagina è progettato specificamente per i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="1171e-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1171e-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="1171e-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1171e-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="1171e-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="1171e-111">Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="1171e-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="1171e-112">ASP.NET Web Pages consente di creare visualizzazioni personalizzate per il rendering del contenuto in altri dispositivi o per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="1171e-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="1171e-113">Il modo più semplice per creare specifiche del dispositivo pagina in un sito di ASP.NET Web Pages consiste nell'usare un modello di denominazione file simile al seguente: <em>nomefile.</em> <em>Mobile</em><em>cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="1171e-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: <em>FileName.</em><em>Mobile</em><em>.cshtml</em>.</span></span> <span data-ttu-id="1171e-114">È possibile creare due versioni di una pagina (ad esempio, una denominata <em>MyFile.cshtml</em> e uno denominato <em>MyFile.Mobile.cshtml</em>).</span><span class="sxs-lookup"><span data-stu-id="1171e-114">You can create two versions of a page (for example, one named <em>MyFile.cshtml</em> and one named <em>MyFile.Mobile.cshtml</em>).</span></span> <span data-ttu-id="1171e-115">In fase di esecuzione, quando un dispositivo mobile richiede <em>MyFile.cshtml</em>, ASP.NET esegue il rendering del contenuto dalla <em>MyFile.Mobile.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="1171e-115">At run time, when a mobile device requests <em>MyFile.cshtml</em>, ASP.NET renders the content from <em>MyFile.Mobile.cshtml</em>.</span></span> <span data-ttu-id="1171e-116">In caso contrario, <em>MyFile.cshtml</em> viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="1171e-116">Otherwise, <em>MyFile.cshtml</em> is rendered.</span></span>

<span data-ttu-id="1171e-117">Nell'esempio seguente viene illustrato come abilitare il rendering per dispositivi mobili tramite l'aggiunta di una pagina di contenuto per i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="1171e-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="1171e-118">*Page1.cshtml* contiene contenuto oltre a una barra laterale di navigazione.</span><span class="sxs-lookup"><span data-stu-id="1171e-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="1171e-119">*Page1.Mobile.cshtml* lo stesso contenuto, ma omette l'intestazione laterale.</span><span class="sxs-lookup"><span data-stu-id="1171e-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="1171e-120">In un sito di ASP.NET Web Pages, creare un file denominato *Page1.cshtml* e sostituire il contenuto corrente con il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="1171e-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="1171e-121">Creare un file denominato *Page1.Mobile.cshtml* e sostituire il contenuto esistente con il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="1171e-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="1171e-122">Si noti che la versione mobile della pagina omette la sezione di spostamento per una migliore riproduzione su uno schermo piccolo.</span><span class="sxs-lookup"><span data-stu-id="1171e-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="1171e-123">Eseguire un browser desktop e passare alla *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1171e-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="1171e-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1171e-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="1171e-125">Eseguire un browser per dispositivi mobili (o un emulatore di dispositivi mobili) e passare alla *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1171e-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="1171e-126">(Si noti che non si include *. Mobile.*</span><span class="sxs-lookup"><span data-stu-id="1171e-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="1171e-127">come parte dell'URL.) Anche se la richiesta è su *Page1.cshtml*, ASP.NET esegue il rendering *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1171e-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="1171e-129">Per testare le pagine per dispositivi mobili, è possibile usare un simulatore di dispositivi mobili che viene eseguito in un computer desktop.</span><span class="sxs-lookup"><span data-stu-id="1171e-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="1171e-130">Questo strumento consente di testare le pagine web come appaiono nei dispositivi mobili (vale a dire, in genere con un notevolmente ridotta visualizzare l'area).</span><span class="sxs-lookup"><span data-stu-id="1171e-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="1171e-131">Un esempio di un simulatore è il [componente aggiuntivo di selezione dell'agente utente](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) per Mozilla Firefox, che consente di emulare diversi browser per dispositivi mobili da una versione desktop di Firefox.</span><span class="sxs-lookup"><span data-stu-id="1171e-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="1171e-132">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1171e-132">Additional Resources</span></span>


<span data-ttu-id="1171e-133">[Emulatore Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="1171e-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
