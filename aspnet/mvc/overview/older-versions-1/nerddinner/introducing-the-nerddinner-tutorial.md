---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introduzione dell'esercitazione NerdDinner | Documenti Microsoft
author: shanselman
description: Il modo migliore per imparare un nuovo framework consiste nel compilare un elemento con esso. In questa esercitazione illustra in dettaglio come compilare un'applicazione di piccola ma completa, utilizzando ASP.NE...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="c85eb-104">Introduzione dell'esercitazione NerdDinner</span><span class="sxs-lookup"><span data-stu-id="c85eb-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="c85eb-105">da [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="c85eb-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="c85eb-106">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="c85eb-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="c85eb-107">Il modo migliore per imparare un nuovo framework consiste nel compilare un elemento con esso.</span><span class="sxs-lookup"><span data-stu-id="c85eb-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="c85eb-108">In questa esercitazione illustra in dettaglio come compilare una piccola ma completa, l'applicazione mediante ASP.NET MVC 1 e vengono presentati alcuni dei concetti di base.</span><span class="sxs-lookup"><span data-stu-id="c85eb-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="c85eb-109">Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="c85eb-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="c85eb-110">NerdDinner esercitazione</span><span class="sxs-lookup"><span data-stu-id="c85eb-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="c85eb-111">Il modo migliore per imparare un nuovo framework consiste nel compilare un elemento con esso.</span><span class="sxs-lookup"><span data-stu-id="c85eb-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="c85eb-112">In questa esercitazione illustra in dettaglio come compilare una piccola ma completa, l'applicazione mediante ASP.NET MVC e vengono presentati alcuni dei concetti di base.</span><span class="sxs-lookup"><span data-stu-id="c85eb-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="c85eb-113">L'applicazione che verrà compilazione viene chiamato "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="c85eb-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="c85eb-114">NerdDinner fornisce un modo semplice per gli utenti individuare e organizzare dinners online:</span><span class="sxs-lookup"><span data-stu-id="c85eb-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="c85eb-115">NerdDinner consente agli utenti registrati creare, modificare ed eliminare dinners.</span><span class="sxs-lookup"><span data-stu-id="c85eb-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="c85eb-116">Applica un set coerente di convalida e regole business all'interno dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="c85eb-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="c85eb-117">I visitatori possono utilizzare una mappa basata su AJAX per la ricerca dinners future mantenuti vicini:</span><span class="sxs-lookup"><span data-stu-id="c85eb-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="c85eb-118">Facendo clic su una cena giungeranno a una pagina di dettagli in cui possono apprendere altre informazioni:</span><span class="sxs-lookup"><span data-stu-id="c85eb-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="c85eb-119">Se sono interessati che frequentano dinner potranno accedere o registrare nel sito:</span><span class="sxs-lookup"><span data-stu-id="c85eb-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="c85eb-120">È quindi possibile fare clic su un collegamento RSVP basate su AJAX per partecipare all'evento:</span><span class="sxs-lookup"><span data-stu-id="c85eb-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="c85eb-121">Implementazione NerdDinner</span><span class="sxs-lookup"><span data-stu-id="c85eb-121">Implementing NerdDinner</span></span>

<span data-ttu-id="c85eb-122">Per avviare l'applicazione NerdDinner tramite File - verrà&gt;comando nuovo progetto in Visual Studio per creare un nuovo progetto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c85eb-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="c85eb-123">Verranno quindi aggiunti in modo incrementale e delle funzionalità.</span><span class="sxs-lookup"><span data-stu-id="c85eb-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="c85eb-124">Lungo il percorso ci occuperemo:</span><span class="sxs-lookup"><span data-stu-id="c85eb-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="c85eb-125">Come creare un nuovo progetto ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c85eb-125">How to create a new ASP.NET MVC Project</span></span>](# "crea un nuovo progetto MVC ASP.NET")
2. [<span data-ttu-id="c85eb-126">Come creare un database</span><span class="sxs-lookup"><span data-stu-id="c85eb-126">How to create a database</span></span>](# "creare un Database")
3. [<span data-ttu-id="c85eb-127">Come creare un modello con le convalide di regola business</span><span class="sxs-lookup"><span data-stu-id="c85eb-127">How to build a model with business rule validations</span></span>](# "compilare un modello con le convalide di regola Business")
4. [<span data-ttu-id="c85eb-128">Come usare controller e visualizzazioni per implementare un listato/dettagli UI</span><span class="sxs-lookup"><span data-stu-id="c85eb-128">How to use controllers and views to implement a listing/details UI</span></span>](# "utilizzano controller e visualizzazioni per implementare un'interfaccia utente/dettagli")
5. <span data-ttu-id="c85eb-129">[Come fornire CRUD (create, leggere, aggiornare ed eliminare) dati formano supporto voce](# "forniscono CRUD (Create, Read, Update, Delete) dati Form voce supporta")</span><span class="sxs-lookup"><span data-stu-id="c85eb-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="c85eb-130">Come utilizzare ViewData e implementare le classi ViewModel</span><span class="sxs-lookup"><span data-stu-id="c85eb-130">How to use ViewData and implement ViewModel classes</span></span>](# "ViewData usare e implementare le classi ViewModel")
7. [<span data-ttu-id="c85eb-131">Come per il riutilizzo dell'interfaccia utente utilizzando le pagine master e parziali</span><span class="sxs-lookup"><span data-stu-id="c85eb-131">How to re-use UI using master pages and partials</span></span>](# "riutilizzo di interfaccia utente utilizzando pagine Master e parziali")
8. [<span data-ttu-id="c85eb-132">Come implementare il paging dei dati efficiente</span><span class="sxs-lookup"><span data-stu-id="c85eb-132">How to implement efficient data paging</span></span>](# "implementare Paging dei dati efficiente")
9. [<span data-ttu-id="c85eb-133">Come proteggere le applicazioni tramite l'autenticazione e autorizzazione</span><span class="sxs-lookup"><span data-stu-id="c85eb-133">How to secure applications using authentication and authorization</span></span>](# "sicuro delle applicazioni usando autenticazione e autorizzazione")
10. [<span data-ttu-id="c85eb-134">Come utilizzare AJAX per distribuire gli aggiornamenti dinamici</span><span class="sxs-lookup"><span data-stu-id="c85eb-134">How to use AJAX to deliver dynamic updates</span></span>](# "utilizzare AJAX per inviare gli aggiornamenti dinamici")
11. [<span data-ttu-id="c85eb-135">Come utilizzare AJAX per implementare scenari di mapping</span><span class="sxs-lookup"><span data-stu-id="c85eb-135">How to use AJAX to implement mapping scenarios</span></span>](# "utilizzare AJAX per implementare scenari di Mapping")
12. [<span data-ttu-id="c85eb-136">Come abilitare unit test automatici</span><span class="sxs-lookup"><span data-stu-id="c85eb-136">How to enable automated unit testing</span></span>](# "attiva Testing unità automatizzati")

<span data-ttu-id="c85eb-137">È possibile compilare la propria copia di NerdDinner da zero, completare ogni passaggio è procedura dettagliata in questo capitolo.</span><span class="sxs-lookup"><span data-stu-id="c85eb-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="c85eb-138">In alternativa, è possibile scaricare una versione completa del codice sorgente qui: [NerdDinner su GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="c85eb-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="c85eb-139">Facoltativamente è possibile inoltre anche [scaricare una versione gratuita PDF di questa esercitazione](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) se si desidera leggere l'esercitazione non in linea.</span><span class="sxs-lookup"><span data-stu-id="c85eb-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="c85eb-140">Per compilare l'applicazione, è possibile utilizzare Visual Studio 2008 o il libero Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="c85eb-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="c85eb-141">È possibile utilizzare SQL Server o il disponibile SQL Server Express per il database.</span><span class="sxs-lookup"><span data-stu-id="c85eb-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="c85eb-142">È possibile installare ASP.NET MVC, Visual Web Developer 2008 Express e SQL Server Express (gratuitamente) tramite V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="c85eb-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="c85eb-143">Ora è possibile iniziare subito...</span><span class="sxs-lookup"><span data-stu-id="c85eb-143">Now let's get started....</span></span>

<span data-ttu-id="c85eb-144">Ora che abbiamo trattato novità NerdDinner, si riporta il nostro manicotti e scrivere codice.</span><span class="sxs-lookup"><span data-stu-id="c85eb-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="c85eb-145">Inizieremo con File -&gt;nuovo progetto di Visual Studio per creare l'applicazione NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="c85eb-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c85eb-146">avanti</span><span class="sxs-lookup"><span data-stu-id="c85eb-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
