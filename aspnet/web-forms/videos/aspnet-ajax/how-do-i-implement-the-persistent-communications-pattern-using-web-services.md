---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
title: '[Procedura:] Implementare il modello di comunicazione persistente tramite servizi Web? | Microsoft Docs'
author: JoeStagner
description: Il browser e il server in un sito Web tradizionali, non mantengono una comunicazione continua, ma comunicare solo in risposta all'utente di eseguire una determinata azione...
ms.author: aspnetcontent
ms.date: 08/22/2007
ms.assetid: 424c06cd-6d61-43cd-a1f2-d1a6b62e47b1
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
msc.type: video
ms.openlocfilehash: b1cc0ad7ba9ce608a54bcf158bd71cf8a65c51ed
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826468"
---
<a name="how-do-i-implement-the-persistent-communications-pattern-using-web-services"></a><span data-ttu-id="aad8b-104">[Procedura:] Implementare il modello di comunicazione persistente tramite servizi Web?</span><span class="sxs-lookup"><span data-stu-id="aad8b-104">[How Do I:] Implement the Persistent Communications Pattern using Web Services?</span></span>
====================
<span data-ttu-id="aad8b-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="aad8b-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="aad8b-106">Il browser e il server in un sito Web tradizionali, non mantengono una comunicazione in corso, ma comunicare solo in risposta all'utente che esegue un'azione.</span><span class="sxs-lookup"><span data-stu-id="aad8b-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="aad8b-107">In un sito Web moderne in cui la pagina diventa un contenitore di applicazione, può essere vantaggioso per il browser e il server per mantenere una comunicazione continua in modo che gli aggiornamenti a pagina possono essere eseguita senza l'utente che esegue un'azione.</span><span class="sxs-lookup"><span data-stu-id="aad8b-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="aad8b-108">Ciò è noto come il modello di comunicazione persistente per AJAX.</span><span class="sxs-lookup"><span data-stu-id="aad8b-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="aad8b-109">ASP.NET AJAX fornisce due modi principali per gli sviluppatori Web implementare il modello di comunicazione persistente.</span><span class="sxs-lookup"><span data-stu-id="aad8b-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="aad8b-110">In un video precedenti abbiamo visto come utilizzare UpdatePanel di ASP.NET AJAX come base per l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="aad8b-110">In an earlier video we saw how to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="aad8b-111">In questo video abbiamo informazioni su come implementare lo stesso modello con una chiamata JavaScrpt a un servizio Web, che elimina la necessità di un UpdatePanel di ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="aad8b-111">In this video we learn how to implement the same pattern using a JavaScrpt call to a Web service, which removes the need for an ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="aad8b-112">&#9654;Guarda il video (16 minuti)</span><span class="sxs-lookup"><span data-stu-id="aad8b-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-using-web-services)

> [!div class="step-by-step"]
> <span data-ttu-id="aad8b-113">[Precedente](how-do-i-localize-an-aspnet-ajax-application.md)
> [Successivo](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span><span class="sxs-lookup"><span data-stu-id="aad8b-113">[Previous](how-do-i-localize-an-aspnet-ajax-application.md)
[Next](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span></span>
