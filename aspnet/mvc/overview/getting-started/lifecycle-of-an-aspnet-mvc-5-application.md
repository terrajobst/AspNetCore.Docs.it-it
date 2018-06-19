---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Ciclo di vita di un'applicazione ASP.NET MVC 5 | Documenti Microsoft
author: cephalin
description: Scaricare un documento PDF che illustra il ciclo di vita di un'applicazione ASP.NET MVC 5. Questo ciclo di vita documento viene fornita una panoramica del ciclo di vita MVC un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036492"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="80870-104">Ciclo di vita di un'applicazione MVC ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="80870-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="80870-105">da [Cephas Lin](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="80870-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="80870-106">Scaricare il documento PDF</span><span class="sxs-lookup"><span data-stu-id="80870-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="80870-107">Qui è possibile scaricare un documento PDF che grafici il ciclo di vita di ogni applicazione ASP.NET MVC 5, di ricezione HTTP richiesta per l'invio della risposta HTTP al client.</span><span class="sxs-lookup"><span data-stu-id="80870-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="80870-108">È progettato come un strumento di apprendimento per gli utenti che hanno familiarità con MVC ASP.NET e anche come riferimento per gli utenti che necessitano di approfondire gli aspetti specifici dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="80870-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="80870-109">Il documento PDF ha le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="80870-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="80870-110">Rilevanti [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) fasi per comprendere in MVC integra il [ciclo di vita dell'applicazione ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).</span><span class="sxs-lookup"><span data-stu-id="80870-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="80870-111">Panoramica del processo di applicazione MVC, in cui è possibile comprendere le fasi principali che ogni applicazione MVC di passa attraverso la pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="80870-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="80870-112">Una visualizzazione dettagliata che illustra drill nei dettagli della pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="80870-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="80870-113">È possibile confrontare la visualizzazione di alto livello e la visualizzazione di dettaglio per vedere come vengono raccolti i dettagli di cicli di vita in varie fasi.</span><span class="sxs-lookup"><span data-stu-id="80870-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="80870-114">[Scarica il PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) per ottenere una visualizzazione più grande.</span><span class="sxs-lookup"><span data-stu-id="80870-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="80870-115">La selezione e lo scopo di tutti i metodi sottoponibili a override sul [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) oggetto nella pipeline di elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="80870-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="80870-116">È possibile o non abbia la necessità di eseguire l'override di qualsiasi metodo di uno, ma è importante comprendere il proprio ruolo nel ciclo di vita dell'applicazione in modo che sia possibile scrivere codice fase del ciclo di vita appropriato per ottenere l'effetto desiderato.</span><span class="sxs-lookup"><span data-stu-id="80870-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="80870-117">Diagrammi di essere sostituito-up che mostra come ognuno dei tipi di filtro (autenticazione, autorizzazione, azione e risultato) viene richiamata.</span><span class="sxs-lookup"><span data-stu-id="80870-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="80870-118">Per collegarsi a un articolo utile o un blog da ogni punto di interesse nella vista di dettaglio.</span><span class="sxs-lookup"><span data-stu-id="80870-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="80870-119">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="80870-119">Next Steps</span></span>

<span data-ttu-id="80870-120">Questo documento soddisfa l'esigenza?</span><span class="sxs-lookup"><span data-stu-id="80870-120">Does this document meet your need?</span></span> <span data-ttu-id="80870-121">Si si apprezza l'invio di commenti e suggerimenti.</span><span class="sxs-lookup"><span data-stu-id="80870-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="80870-122">Se hai domande sul ciclo di vita di ASP.NET MVC nell'applicazione, [Stackoverflow](http://stackoverflow.com/help) e [forum di ASP.NET MVC](https://forums.asp.net/1146.aspx) sono i modi per chiedere.</span><span class="sxs-lookup"><span data-stu-id="80870-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="80870-123">Seguire [me](https://twitter.com/Cephas_MSFT) su twitter, pertanto è possibile ottenere gli aggiornamenti in my esercitazioni più recente.</span><span class="sxs-lookup"><span data-stu-id="80870-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
