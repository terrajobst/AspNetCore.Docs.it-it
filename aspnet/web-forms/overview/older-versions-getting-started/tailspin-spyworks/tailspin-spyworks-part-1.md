---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: File -> Nuovo progetto | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 1 copre panoramica e File/nuovo progetto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-1-file--new-project"></a><span data-ttu-id="4d94a-104">Parte 1: File -> Nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="4d94a-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="4d94a-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4d94a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4d94a-106">Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="4d94a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4d94a-107">Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="4d94a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4d94a-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="4d94a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4d94a-109">Parte 1 copre panoramica e File/nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="4d94a-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="4d94a-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="4d94a-110">Overview</span></span>

<span data-ttu-id="4d94a-111">In questa esercitazione viene fornita un'introduzione a ASP.NET WebForms.</span><span class="sxs-lookup"><span data-stu-id="4d94a-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="4d94a-112">Si verrà avviata lenta, pertanto l'esperienza di sviluppo web di livello per principianti è accettabile.</span><span class="sxs-lookup"><span data-stu-id="4d94a-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="4d94a-113">L'applicazione che è da generare è un semplice negozio online.</span><span class="sxs-lookup"><span data-stu-id="4d94a-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="4d94a-114">I visitatori possono sfogliare i prodotti per categoria:</span><span class="sxs-lookup"><span data-stu-id="4d94a-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="4d94a-115">È possibile visualizzare un singolo prodotto e aggiungerlo al carrello:</span><span class="sxs-lookup"><span data-stu-id="4d94a-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="4d94a-116">È possibile esaminarne carrello, la rimozione di tutti gli elementi che non si desidera più:</span><span class="sxs-lookup"><span data-stu-id="4d94a-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="4d94a-117">Procedere con l'acquisto verrà visualizzato un messaggio a</span><span class="sxs-lookup"><span data-stu-id="4d94a-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="4d94a-118">Dopo aver ordinato, viene visualizzato una schermata di conferma semplice:</span><span class="sxs-lookup"><span data-stu-id="4d94a-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="4d94a-119">Iniziare creando un nuovo progetto di Web Form ASP.NET in Visual Studio 2010 e funzionalità per creare un'applicazione funzionante completezza in modo incrementale verrà aggiunto.</span><span class="sxs-lookup"><span data-stu-id="4d94a-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="4d94a-120">Inoltre, ci occuperemo accesso al database, le visualizzazioni elenco e griglia, pagine di aggiornamento dati, la convalida dei dati, utilizzando le pagine master per layout di pagina coerente, AJAX, convalida, l'appartenenza dell'utente e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="4d94a-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="4d94a-121">È possibile seguire la procedura dettagliata, oppure è possibile scaricare l'applicazione finita da [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="4d94a-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="4d94a-122">È possibile utilizzare Visual Studio 2010 o il liberi Visual Web Developer 2010 da [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="4d94a-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="4d94a-123">Per compilare l'applicazione, è possibile utilizzare SQL Server o il disponibile SQL Server Express per ospitare il database.</span><span class="sxs-lookup"><span data-stu-id="4d94a-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="4d94a-124">File / nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="4d94a-124">File / New Project</span></span>

<span data-ttu-id="4d94a-125">Si inizierà selezionando il nuovo progetto dal menu File in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4d94a-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="4d94a-126">Verrà visualizzata la finestra di dialogo Nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="4d94a-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="4d94a-127">È possibile selezionare Visual c# / modelli Web gruppo a sinistra e quindi scegliere il modello "Applicazione Web ASP.NET" nella colonna centrale.</span><span class="sxs-lookup"><span data-stu-id="4d94a-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="4d94a-128">Denominare il progetto TailspinSpyworks e fare clic sul pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="4d94a-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="4d94a-129">Verrà creato il progetto.</span><span class="sxs-lookup"><span data-stu-id="4d94a-129">This will create our project.</span></span> <span data-ttu-id="4d94a-130">Esaminiamo le cartelle in cui sono inclusi nell'applicazione in Esplora soluzioni sul lato destro.</span><span class="sxs-lookup"><span data-stu-id="4d94a-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="4d94a-131">La soluzione vuota non è completamente vuota e aggiunge una struttura di cartelle di base:</span><span class="sxs-lookup"><span data-stu-id="4d94a-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="4d94a-132">Tenere presente le convenzioni implementate dal modello di progetto predefinito di ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="4d94a-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="4d94a-133">La cartella "Account" implementa un'interfaccia utente di base per ASP. Sottosistema di appartenenza della rete.</span><span class="sxs-lookup"><span data-stu-id="4d94a-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="4d94a-134">La cartella "Scripts" funge da repository per il file JavaScript sul lato client e i file con estensione js jQuery core vengono resi disponibili per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4d94a-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="4d94a-135">La cartella "Stili" viene usata per organizzare elementi visivi del sito web (fogli di stile CSS)</span><span class="sxs-lookup"><span data-stu-id="4d94a-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="4d94a-136">Quando si preme F5 per eseguire l'applicazione e il rendering della pagina aspx viene visualizzato quanto segue.</span><span class="sxs-lookup"><span data-stu-id="4d94a-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="4d94a-137">Il miglioramento dell'applicazione prima è necessario sostituire il file Style.css dal modello Web Form predefinito con i file di immagine associata che verranno eseguito il rendering di asthetics visual desiderati per l'applicazione Tailspin Spyworks e di classi CSS.</span><span class="sxs-lookup"><span data-stu-id="4d94a-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="4d94a-138">Al termine dell'operazione nostra pagina aspx esegue il rendering simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="4d94a-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="4d94a-139">Notare i collegamenti di immagine nella parte superiore destra della pagina e le voci di menu che sono stati aggiunti alla pagina master.</span><span class="sxs-lookup"><span data-stu-id="4d94a-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="4d94a-140">Solo i collegamenti di "Accedi" e "Account" puntano per le pagine esistenti (generate tramite il modello predefinito) e il resto delle pagine che verrà implementata mentre si compila l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4d94a-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="4d94a-141">Verrà inoltre spostare la pagina Master per la directory di stili.</span><span class="sxs-lookup"><span data-stu-id="4d94a-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="4d94a-142">Anche se questo è solo una preferenza può avere elementi un po' più semplice se si decide di eseguire l'applicazione "skinable" in futuro.</span><span class="sxs-lookup"><span data-stu-id="4d94a-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="4d94a-143">Dopo questa operazione che è necessario modificare la pagina master i riferimenti in tutti i file con estensione aspx generato per l'impostazione predefinita le pagine Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4d94a-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4d94a-144">avanti</span><span class="sxs-lookup"><span data-stu-id="4d94a-144">Next</span></span>](tailspin-spyworks-part-2.md)
