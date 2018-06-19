---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Eseguire la ricerca per categorie:] Utilizzare la proprietà Reponse.Filter per sostituire HTML in una pagina ASP.NET | Documenti Microsoft'
author: rick-anderson
description: In questo video di Chris Pels viene illustrato come utilizzare la proprietà Reponse.Filter per intercettare e modificare il codice HTML inviati a una pagina. Una pagina di esempio viene innanzitutto creata w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525530"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="42bfe-104">[Eseguire la ricerca per categorie:] Utilizzare la proprietà Reponse.Filter per sostituire HTML in una pagina ASP.NET</span><span class="sxs-lookup"><span data-stu-id="42bfe-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="42bfe-105">da [Chris PEL](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="42bfe-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="42bfe-106">In questo video di Chris Pels viene illustrato come utilizzare la proprietà Reponse.Filter per intercettare e modificare il codice HTML inviati a una pagina.</span><span class="sxs-lookup"><span data-stu-id="42bfe-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="42bfe-107">In primo luogo, una pagina di esempio viene creata con un testo semplice.</span><span class="sxs-lookup"><span data-stu-id="42bfe-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="42bfe-108">Quindi, viene creata una classe di flusso personalizzata che serve come flusso di sostituzione per il flusso corrente viene inviato al browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="42bfe-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="42bfe-109">In tale classe di flusso personalizzato il contenuto della pagina viene recuperato dal flusso, modificato e quindi vengono scritti nel flusso di risposta.</span><span class="sxs-lookup"><span data-stu-id="42bfe-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="42bfe-110">In questa classe di flusso personalizzata al metodo Write è stato personalizzato per sostituire il codice HTML nel flusso di risposta di base, in tal modo la modifica viene inviato ai browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="42bfe-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="42bfe-111">Infine, la nuova classe di flusso viene assegnata per la proprietà Response nella pagina\_evento di caricamento, in tal modo, che fornisce il meccanismo per la modifica del contenuto della pagina.</span><span class="sxs-lookup"><span data-stu-id="42bfe-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="42bfe-112">&#9654; Guardare video (13 minuti)</span><span class="sxs-lookup"><span data-stu-id="42bfe-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
