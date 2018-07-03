---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[Procedura:] Scegliere tra i metodi di AJAX pagina degli aggiornamenti? | Microsoft Docs'
author: JoeStagner
description: In questo video Joe Stagner confronta i due metodi principali di esecuzione degli aggiornamenti di pagina in stile AJAX in un'applicazione ASP.NET. Il primo metodo consiste nell'usare un Upd...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2007
ms.topic: article
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: c092a7dcaeeeec53e899bb1cdf12f9bb02a99597
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392052"
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="1dc8d-105">[Procedura:] Scegliere tra i metodi di AJAX pagina degli aggiornamenti?</span><span class="sxs-lookup"><span data-stu-id="1dc8d-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>
====================
<span data-ttu-id="1dc8d-106">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="1dc8d-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="1dc8d-107">In questo video Joe Stagner confronta i due metodi principali di esecuzione degli aggiornamenti di pagina in stile AJAX in un'applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1dc8d-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="1dc8d-108">Il primo metodo consiste nell'utilizzare un UpdatePanel, in cui non deve essere scritto sul lato client o lato server richiedere codice aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="1dc8d-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="1dc8d-109">Il vantaggio di utilizzare UpdatePanel è che tutto funziona automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1dc8d-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="1dc8d-110">Lo svantaggio è che il client che richiede una grande quantità di dati da includere nella richiesta AJAX e nella risposta e il server che richiede un ciclo di vita completo della pagina deve essere eseguito.</span><span class="sxs-lookup"><span data-stu-id="1dc8d-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="1dc8d-111">Il secondo metodo consiste nell'utilizzare i callback di rete, in cui deve essere scritto sul lato client sia sul lato server codice aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="1dc8d-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="1dc8d-112">Il vantaggio dell'utilizzo dei callback di rete è che il client richiede pochi dati da includere nella richiesta AJAX e nella risposta e richiede solo il metodo di servizio chiamato deve essere eseguito nel server.</span><span class="sxs-lookup"><span data-stu-id="1dc8d-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="1dc8d-113">Il significativo è il tempo e impegno che necessario per scrivere il codice necessario.</span><span class="sxs-lookup"><span data-stu-id="1dc8d-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="1dc8d-114">Joe conclude il video, illustrando cosa deve considerare nella scelta tra i due metodi principali degli aggiornamenti a pagina stile AJAX.</span><span class="sxs-lookup"><span data-stu-id="1dc8d-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="1dc8d-115">(In questo video viene usato il codice dal [come iniziare con ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video e la [ricerca per categorie verificare i callback di rete Client-Side con ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span><span class="sxs-lookup"><span data-stu-id="1dc8d-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="1dc8d-116">&#9654;Guarda il video (11 minuti)</span><span class="sxs-lookup"><span data-stu-id="1dc8d-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="1dc8d-117">[Precedente](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [Successivo](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="1dc8d-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
