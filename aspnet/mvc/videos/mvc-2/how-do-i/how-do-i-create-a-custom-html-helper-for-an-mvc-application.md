---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: "Procedura: creazione di un Helper HTML personalizzati per un'applicazione MVC? | Microsoft Docs"
author: rick-anderson
description: In questo video Chris Pels viene illustrato come creare un HtmlHelper personalizzato che non è disponibile nel set di standard in un'applicazione MVC. Primo, un applica MVC di esempio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/11/2009
ms.topic: article
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 92faa04e1eefec0852604d51987ddaa9ee58838a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="78e0f-105">Procedura: creazione di un Helper HTML personalizzati per un'applicazione MVC?</span><span class="sxs-lookup"><span data-stu-id="78e0f-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="78e0f-106">da [Chris PEL](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="78e0f-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="78e0f-107">In questo video Chris Pels viene illustrato come creare un HtmlHelper personalizzato che non è disponibile nel set di standard in un'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="78e0f-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="78e0f-108">In primo luogo, un'applicazione MVC di esempio viene creata con un controller demo e visualizzazione per testare il HtmlHelper personalizzato.</span><span class="sxs-lookup"><span data-stu-id="78e0f-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="78e0f-109">Successivamente, creazione di un modulo con una funzione pubblica di un metodo di estensione che rappresenta l'implementazione del HtmlHelper personalizzato.</span><span class="sxs-lookup"><span data-stu-id="78e0f-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="78e0f-110">L'helper personalizzato è per la creazione di `<img>` tag in una pagina e riceve i diversi parametri in entrata, compresi l'id, l'url e il testo alternativo per il tag di immagine.</span><span class="sxs-lookup"><span data-stu-id="78e0f-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="78e0f-111">La logica viene quindi aggiunto alla funzione per la restituzione completato `<img>` tag con le informazioni specificate.</span><span class="sxs-lookup"><span data-stu-id="78e0f-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="78e0f-112">Il HtmlHelper personalizzata viene utilizzata nella pagina demo per visualizzare un'immagine.</span><span class="sxs-lookup"><span data-stu-id="78e0f-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="78e0f-113">Infine, il HtmlHelper personalizzato viene espanso per includere più sostituzioni costruttore che forniscono flessibilità per più facilmente la creazione di diversi `<img>` tag.</span><span class="sxs-lookup"><span data-stu-id="78e0f-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="78e0f-114">&#9654;Guardare video (18 minuti)</span><span class="sxs-lookup"><span data-stu-id="78e0f-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="78e0f-115">[Precedente](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Successivo](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="78e0f-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
