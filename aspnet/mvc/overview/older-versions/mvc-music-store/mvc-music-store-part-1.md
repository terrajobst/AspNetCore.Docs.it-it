---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: Panoramica e File -> Nuovo progetto | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Viene illustrato come parte 1 Panoramica e File -> Nuovo progetto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="039d1-104">Parte 1: Panoramica e File -> Nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="039d1-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="039d1-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="039d1-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="039d1-106">L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="039d1-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="039d1-107">L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="039d1-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="039d1-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio.</span><span class="sxs-lookup"><span data-stu-id="039d1-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="039d1-109">Parte 1 copre panoramica e File -&gt;nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="039d1-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="039d1-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="039d1-110">Overview</span></span>

<span data-ttu-id="039d1-111">L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Web Developer per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="039d1-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="039d1-112">Si verrà avviata lenta, pertanto l'esperienza di sviluppo web di livello per principianti è accettabile.</span><span class="sxs-lookup"><span data-stu-id="039d1-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="039d1-113">L'applicazione che è da generare è un archivio di musica semplice.</span><span class="sxs-lookup"><span data-stu-id="039d1-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="039d1-114">Esistono tre parti principali per l'applicazione: market, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="039d1-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="039d1-115">I visitatori possono passare album per genere:</span><span class="sxs-lookup"><span data-stu-id="039d1-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="039d1-116">È possibile visualizzare album di un singolo e aggiungerlo al carrello:</span><span class="sxs-lookup"><span data-stu-id="039d1-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="039d1-117">È possibile esaminarne carrello, la rimozione di tutti gli elementi che non si desidera più:</span><span class="sxs-lookup"><span data-stu-id="039d1-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="039d1-118">Procedere con l'acquisto verrà richiesto loro di accedere o registrare un account utente.</span><span class="sxs-lookup"><span data-stu-id="039d1-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="039d1-119">Dopo aver creato un account, completano l'ordine inserendo le informazioni di pagamento e di spedizione.</span><span class="sxs-lookup"><span data-stu-id="039d1-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="039d1-120">Per semplicità, in corso una promozione straordinaria: tutto ciò che è disponibile se si immette il codice di innalzamento di livello "FREE".</span><span class="sxs-lookup"><span data-stu-id="039d1-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="039d1-121">Dopo aver ordinato, viene visualizzato una schermata di conferma semplice:</span><span class="sxs-lookup"><span data-stu-id="039d1-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="039d1-122">Oltre alle pagine faceing di clienti, è inoltre sarà compilare una sezione di amministratore che mostra un elenco di album da cui gli amministratori possono creare, modificare ed eliminare album:</span><span class="sxs-lookup"><span data-stu-id="039d1-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="039d1-123">1. File -&gt; nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="039d1-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="039d1-124">L'installazione del software</span><span class="sxs-lookup"><span data-stu-id="039d1-124">Installing the software</span></span>

<span data-ttu-id="039d1-125">In questa esercitazione si inizierà creando un nuovo progetto ASP.NET MVC 3 con il libero Visual Web Developer 2010 Express (che è gratuito) e quindi verrà aggiunto in modo incrementale le funzionalità per creare un'applicazione funzionante completezza.</span><span class="sxs-lookup"><span data-stu-id="039d1-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="039d1-126">Inoltre, ci occuperemo accesso al database, gli scenari di registrazione di modulo, la convalida dei dati, utilizzando le pagine master per il layout di pagina coerente, AJAX per gli aggiornamenti della pagina e la convalida, account di accesso utente e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="039d1-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="039d1-127">È possibile seguire la procedura dettagliata, oppure è possibile scaricare l'applicazione finita da [negozio di musica MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="039d1-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="039d1-128">Per compilare l'applicazione, è possibile utilizzare Visual Studio 2010 SP1 o Visual Web Developer 2010 Express SP1 (versione gratuita di Visual Studio 2010).</span><span class="sxs-lookup"><span data-stu-id="039d1-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="039d1-129">Verrà usato SQL Server Compact (anche gratuito) per ospitare il database.</span><span class="sxs-lookup"><span data-stu-id="039d1-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="039d1-130">Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="039d1-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="039d1-131">[Prerequisiti di visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="039d1-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="039d1-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="039d1-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="039d1-133">[SQL Server Compact 4.0 -] incluso il supporto degli strumenti e runtime</span><span class="sxs-lookup"><span data-stu-id="039d1-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="039d1-134">Crea un nuovo progetto ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="039d1-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="039d1-135">Si inizierà selezionando "Nuovo progetto" dal menu File in Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="039d1-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="039d1-136">Verrà visualizzata la finestra di dialogo Nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="039d1-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="039d1-137">Si selezionerà il Visual c# -&gt; modelli Web gruppo a sinistra, quindi scegliere il modello "Applicazione Web di ASP.NET MVC 3" nella colonna centrale.</span><span class="sxs-lookup"><span data-stu-id="039d1-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="039d1-138">Denominare il progetto MvcMusicStore e fare clic sul pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="039d1-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="039d1-139">Verrà visualizzata una finestra di dialogo secondario che consente di verificare alcune impostazioni specifiche per il progetto MVC in.</span><span class="sxs-lookup"><span data-stu-id="039d1-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="039d1-140">Selezionare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="039d1-140">Select the following:</span></span>

<span data-ttu-id="039d1-141">Modello di progetto - selezionare vuoto</span><span class="sxs-lookup"><span data-stu-id="039d1-141">Project Template - select Empty</span></span>

<span data-ttu-id="039d1-142">Motore di visualizzazione: selezionare Razor</span><span class="sxs-lookup"><span data-stu-id="039d1-142">View Engine - select Razor</span></span>

<span data-ttu-id="039d1-143">Utilizza markup semantico HTML5 - selezionata</span><span class="sxs-lookup"><span data-stu-id="039d1-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="039d1-144">Verificare che le impostazioni sono come illustrato di seguito, quindi fare clic sul pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="039d1-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="039d1-145">Verrà creato il progetto.</span><span class="sxs-lookup"><span data-stu-id="039d1-145">This will create our project.</span></span> <span data-ttu-id="039d1-146">Esaminiamo le cartelle che sono stati aggiunti all'applicazione in Esplora soluzioni sul lato destro.</span><span class="sxs-lookup"><span data-stu-id="039d1-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="039d1-147">Il modello vuoto MVC 3 non è completamente vuoto e aggiunge una struttura di cartelle di base:</span><span class="sxs-lookup"><span data-stu-id="039d1-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="039d1-148">ASP.NET MVC si avvalgono di alcune convenzioni di denominazione di base per i nomi delle cartelle:</span><span class="sxs-lookup"><span data-stu-id="039d1-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="039d1-149">**Cartella**</span><span class="sxs-lookup"><span data-stu-id="039d1-149">**Folder**</span></span> | <span data-ttu-id="039d1-150">**Scopo**</span><span class="sxs-lookup"><span data-stu-id="039d1-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="039d1-151">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="039d1-151">**/Controllers**</span></span> | <span data-ttu-id="039d1-152">Controller rispondono a input dal browser, decidere cosa fare con esso e restituire una risposta all'utente.</span><span class="sxs-lookup"><span data-stu-id="039d1-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="039d1-153">**/Views**</span><span class="sxs-lookup"><span data-stu-id="039d1-153">**/Views**</span></span> | <span data-ttu-id="039d1-154">Viste contengono i nostri modelli dell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="039d1-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="039d1-155">**O i modelli**</span><span class="sxs-lookup"><span data-stu-id="039d1-155">**/Models**</span></span> | <span data-ttu-id="039d1-156">I modelli contengono e modificare i dati</span><span class="sxs-lookup"><span data-stu-id="039d1-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="039d1-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="039d1-157">**/Content**</span></span> | <span data-ttu-id="039d1-158">Questa cartella contiene il nostro immagini, CSS e qualsiasi altro contenuto statico</span><span class="sxs-lookup"><span data-stu-id="039d1-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="039d1-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="039d1-159">**/Scripts**</span></span> | <span data-ttu-id="039d1-160">Questa cartella contiene i file JavaScript</span><span class="sxs-lookup"><span data-stu-id="039d1-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="039d1-161">Queste cartelle sono inclusi anche in un'applicazione MVC ASP.NET vuoto poiché il framework ASP.NET MVC per impostazione predefinita viene utilizzato un approccio "convenzione sulla configurazione" e rende alcuni presupposti in base alle convenzioni di denominazione di cartella.</span><span class="sxs-lookup"><span data-stu-id="039d1-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="039d1-162">Ad esempio, i controller di cercano le viste nella cartella Views per impostazione predefinita senza dover specificare in modo esplicito nel codice.</span><span class="sxs-lookup"><span data-stu-id="039d1-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="039d1-163">Trovarsi bene con le convenzioni predefinite riduce la quantità di codice da scrivere, e può rendere più semplice per gli altri sviluppatori comprendere il progetto.</span><span class="sxs-lookup"><span data-stu-id="039d1-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="039d1-164">Verrà illustrato queste convenzioni ulteriori mentre si compila l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="039d1-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="039d1-165">avanti</span><span class="sxs-lookup"><span data-stu-id="039d1-165">Next</span></span>](mvc-music-store-part-2.md)
