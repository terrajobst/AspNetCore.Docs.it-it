---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introduzione all'esercitazione NerdDinner | Microsoft Docs
author: shanselman
description: Il modo migliore per imparare un nuovo framework consiste nella compilazione di un elemento con esso. Questa esercitazione illustra in dettaglio come compilare un'applicazione di piccole dimensioni, ma completa, tramite ASP. ne...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 9188df1eca7f488502640bc17d5034f93f4ac82c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805110"
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="af30a-104">Introduzione all'esercitazione NerdDinner</span><span class="sxs-lookup"><span data-stu-id="af30a-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="af30a-105">da [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="af30a-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="af30a-106">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="af30a-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="af30a-107">Il modo migliore per imparare un nuovo framework consiste nella compilazione di un elemento con esso.</span><span class="sxs-lookup"><span data-stu-id="af30a-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="af30a-108">Questa esercitazione illustra come compilare una piccola, ma completa, applicazione che usa ASP.NET MVC 1 e introduce alcuni concetti di base sottostante.</span><span class="sxs-lookup"><span data-stu-id="af30a-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="af30a-109">Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="af30a-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="af30a-110">Esercitazione NerdDinner</span><span class="sxs-lookup"><span data-stu-id="af30a-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="af30a-111">Il modo migliore per imparare un nuovo framework consiste nella compilazione di un elemento con esso.</span><span class="sxs-lookup"><span data-stu-id="af30a-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="af30a-112">Questa esercitazione illustra come compilare una piccola, ma completa, dell'applicazione tramite ASP.NET MVC e introduce alcuni concetti di base sottostante.</span><span class="sxs-lookup"><span data-stu-id="af30a-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="af30a-113">L'applicazione che si intende compilare è chiamato "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="af30a-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="af30a-114">NerdDinner offre un modo semplice per gli utenti individuare e organizzare dinners online:</span><span class="sxs-lookup"><span data-stu-id="af30a-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="af30a-115">NerdDinner consente agli utenti registrati di creare, modificare ed eliminare dinners.</span><span class="sxs-lookup"><span data-stu-id="af30a-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="af30a-116">Viene utilizzato per applicare un set coerente di convalida e le regole business all'interno dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="af30a-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="af30a-117">I visitatori possono utilizzare una mappa basata su AJAX per la ricerca dinners imminenti mantenuto quasi li:</span><span class="sxs-lookup"><span data-stu-id="af30a-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="af30a-118">Facendo clic su un dinner li indirizzerà a una pagina in cui è possibile imparare informazioni su di essa:</span><span class="sxs-lookup"><span data-stu-id="af30a-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="af30a-119">Se si è interessati a frequentate dinner potranno accedere o registrarsi nel sito:</span><span class="sxs-lookup"><span data-stu-id="af30a-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="af30a-120">È quindi possibile fare clic su un collegamento RSVP basate su AJAX per partecipare all'evento:</span><span class="sxs-lookup"><span data-stu-id="af30a-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="af30a-121">Implementazione di NerdDinner</span><span class="sxs-lookup"><span data-stu-id="af30a-121">Implementing NerdDinner</span></span>

<span data-ttu-id="af30a-122">Si userà per avviare l'applicazione di NerdDinner usando il File -&gt;comandi nuovo progetto di Visual Studio per creare un nuovo progetto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="af30a-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="af30a-123">Verrà quindi aggiunto in modo incrementale le funzionalità.</span><span class="sxs-lookup"><span data-stu-id="af30a-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="af30a-124">Corso della trattazione trattati:</span><span class="sxs-lookup"><span data-stu-id="af30a-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="af30a-125">Come creare un nuovo progetto ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="af30a-125">How to create a new ASP.NET MVC Project</span></span>](# "crea un nuovo progetto MVC ASP.NET")
2. [<span data-ttu-id="af30a-126">Come creare un database</span><span class="sxs-lookup"><span data-stu-id="af30a-126">How to create a database</span></span>](# "creare un Database")
3. [<span data-ttu-id="af30a-127">Come compilare un modello con convalide delle regole business</span><span class="sxs-lookup"><span data-stu-id="af30a-127">How to build a model with business rule validations</span></span>](# "compilare un modello con convalide delle regole Business")
4. [<span data-ttu-id="af30a-128">Come usare controller e visualizzazioni per implementare un'interfaccia utente elenco/dettagli</span><span class="sxs-lookup"><span data-stu-id="af30a-128">How to use controllers and views to implement a listing/details UI</span></span>](# "usare controller e visualizzazioni per implementare un'interfaccia utente elenco/dettagli")
5. <span data-ttu-id="af30a-129">[Come fornire CRUD (create, leggere, aggiornare ed eliminare) dati formano il supporto di ingresso](# "forniscono CRUD (Create, Read, Update, Delete) dati Form voce supporta")</span><span class="sxs-lookup"><span data-stu-id="af30a-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="af30a-130">Come usare ViewData e implementare classi ViewModel</span><span class="sxs-lookup"><span data-stu-id="af30a-130">How to use ViewData and implement ViewModel classes</span></span>](# "usare ViewData e implementare classi ViewModel")
7. [<span data-ttu-id="af30a-131">Come usare nuovamente l'interfaccia utente usando le pagine master e visualizzazioni parziali</span><span class="sxs-lookup"><span data-stu-id="af30a-131">How to re-use UI using master pages and partials</span></span>](# "riutilizzo di interfaccia utente tramite pagine Master e visualizzazioni parziali")
8. [<span data-ttu-id="af30a-132">Come implementare il paging dei dati efficiente</span><span class="sxs-lookup"><span data-stu-id="af30a-132">How to implement efficient data paging</span></span>](# "implementare Paging dei dati efficiente")
9. [<span data-ttu-id="af30a-133">Come proteggere le applicazioni usando l'autenticazione e autorizzazione</span><span class="sxs-lookup"><span data-stu-id="af30a-133">How to secure applications using authentication and authorization</span></span>](# "sicure le applicazioni usando autenticazione e autorizzazione")
10. [<span data-ttu-id="af30a-134">Come usare AJAX per distribuire gli aggiornamenti dinamici</span><span class="sxs-lookup"><span data-stu-id="af30a-134">How to use AJAX to deliver dynamic updates</span></span>](# "usare AJAX per inviare gli aggiornamenti dinamici")
11. [<span data-ttu-id="af30a-135">Come usare AJAX per implementare scenari di mapping</span><span class="sxs-lookup"><span data-stu-id="af30a-135">How to use AJAX to implement mapping scenarios</span></span>](# "usare AJAX per implementare scenari di Mapping")
12. [<span data-ttu-id="af30a-136">Come abilitare il testing unità automatizzato</span><span class="sxs-lookup"><span data-stu-id="af30a-136">How to enable automated unit testing</span></span>](# "Abilita Testing unità automatizzato")

<span data-ttu-id="af30a-137">È possibile compilare la propria copia di NerdDinner da zero, completare ogni passaggio sono procedura dettagliata in questo capitolo.</span><span class="sxs-lookup"><span data-stu-id="af30a-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="af30a-138">In alternativa, è possibile scaricare una versione completa del codice sorgente di seguito: [NerdDinner su GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="af30a-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="af30a-139">È anche possibile anche possibile [scaricare una versione PDF gratuita di questa esercitazione](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) se si desidera leggere l'esercitazione offline.</span><span class="sxs-lookup"><span data-stu-id="af30a-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="af30a-140">Per compilare l'applicazione, è possibile usare Visual Studio 2008 o il gratuito Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="af30a-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="af30a-141">È possibile usare SQL Server o gratuita SQL Server Express per il database.</span><span class="sxs-lookup"><span data-stu-id="af30a-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="af30a-142">È possibile installare ASP.NET MVC, Visual Web Developer 2008 Express e SQL Server Express (tutto gratuito) con V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="af30a-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="af30a-143">A questo punto possiamo iniziare...</span><span class="sxs-lookup"><span data-stu-id="af30a-143">Now let's get started....</span></span>

<span data-ttu-id="af30a-144">Ora che abbiamo trattato NerdDinner What ' s, verrà ora nostro maniche e scrivere il codice.</span><span class="sxs-lookup"><span data-stu-id="af30a-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="af30a-145">Inizieremo con File -&gt;nuovo progetto in Visual Studio per creare l'applicazione di NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="af30a-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="af30a-146">avanti</span><span class="sxs-lookup"><span data-stu-id="af30a-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
