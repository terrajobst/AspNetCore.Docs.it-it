---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Procedura:] Usare la proprietà Reponse. Filter per sostituire il codice HTML in una pagina ASP.NET | Microsoft Docs'
author: rick-anderson
description: In questo video Chris Pels illustra come usare la proprietà Reponse. Filter per intercettare e modificare il codice HTML inviati a una pagina. Prima di tutto una pagina di esempio viene creata w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: f871a3969da21a22bfe10e3a85f43db3e0e522e8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382361"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="14a4a-104">[Procedura:] Usare la proprietà Reponse. Filter per sostituire il codice HTML in una pagina ASP.NET</span><span class="sxs-lookup"><span data-stu-id="14a4a-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="14a4a-105">da [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="14a4a-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="14a4a-106">In questo video Chris Pels illustra come usare la proprietà Reponse. Filter per intercettare e modificare il codice HTML inviati a una pagina.</span><span class="sxs-lookup"><span data-stu-id="14a4a-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="14a4a-107">Prima di tutto una pagina di esempio viene creata con un semplice testo.</span><span class="sxs-lookup"><span data-stu-id="14a4a-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="14a4a-108">Quindi, viene creata una classe di Stream personalizzata che agisce come il flusso di sostituzione per il flusso corrente inviato al browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="14a4a-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="14a4a-109">In tale classe di flusso personalizzata il contenuto della pagina viene recuperato dal flusso, modificato e quindi vengono scritti nel flusso di risposta.</span><span class="sxs-lookup"><span data-stu-id="14a4a-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="14a4a-110">In questa classe Stream personalizzata del metodo Write viene personalizzato per sostituire il codice HTML nel flusso di risposta di base, in tal modo la modifica di ciò che viene inviato al browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="14a4a-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="14a4a-111">Infine, la nuova classe di flusso viene assegnata alla proprietà Response. Filter nella pagina\_evento di caricamento, in tal modo, che fornisce il meccanismo per la modifica del contenuto della pagina.</span><span class="sxs-lookup"><span data-stu-id="14a4a-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="14a4a-112">&#9654;Guarda il video (13 minuti)</span><span class="sxs-lookup"><span data-stu-id="14a4a-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
