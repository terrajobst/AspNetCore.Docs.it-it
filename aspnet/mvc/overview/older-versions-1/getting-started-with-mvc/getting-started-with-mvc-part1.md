---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introduzione a ASP.NET MVC | Documenti Microsoft
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice la lettura e scrittura da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="f20f9-104">Intro to ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f20f9-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="f20f9-105">da [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f20f9-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="f20f9-106">Una versione aggiornata se è disponibile in questa esercitazione [qui](../../getting-started/introduction/getting-started.md) utilizzando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="f20f9-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="f20f9-107">Nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti introdotti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f20f9-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="f20f9-108">Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f20f9-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="f20f9-109">Si creerà un'applicazione web semplice la lettura e scrittura da un database.</span><span class="sxs-lookup"><span data-stu-id="f20f9-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="f20f9-110">Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.</span><span class="sxs-lookup"><span data-stu-id="f20f9-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="f20f9-111">Verifichiamo la prima applicazione Web ASP.NET MVC utilizzando [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="f20f9-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="f20f9-112">Verrà effettuata una piccola applicazione elenco di film che creerà segnalare il problema e l'elenco di film.</span><span class="sxs-lookup"><span data-stu-id="f20f9-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="f20f9-113">Cosa Compilerai</span><span class="sxs-lookup"><span data-stu-id="f20f9-113">What You'll Build</span></span>

<span data-ttu-id="f20f9-114">Di seguito sono riportate due schermate dell'applicazione, sarà necessario creare.</span><span class="sxs-lookup"><span data-stu-id="f20f9-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="f20f9-115">È una tabella semplice dei filmati con diverse colonne.</span><span class="sxs-lookup"><span data-stu-id="f20f9-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="f20f9-116">[![Elenco di film - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f20f9-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="f20f9-117">E sarà necessario un modulo di creazione in modo possiamo aggiungere all'elenco di film.</span><span class="sxs-lookup"><span data-stu-id="f20f9-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="f20f9-118">[![Creare un filmato - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f20f9-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="f20f9-119">Si apprenderà competenze</span><span class="sxs-lookup"><span data-stu-id="f20f9-119">Skills You'll Learn</span></span>

<span data-ttu-id="f20f9-120">In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web di MVC ASP.NET utilizzando Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f20f9-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="f20f9-121">Si apprenderà:</span><span class="sxs-lookup"><span data-stu-id="f20f9-121">You'll learn:</span></span>

- <span data-ttu-id="f20f9-122">Come creare un nuovo progetto MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f20f9-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="f20f9-123">Come creare un nuovo Database con SQL Server</span><span class="sxs-lookup"><span data-stu-id="f20f9-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="f20f9-124">Come creare ASP.NET MVC controller e visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="f20f9-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="f20f9-125">Come recuperare e visualizzare i dati</span><span class="sxs-lookup"><span data-stu-id="f20f9-125">How to retrieve and display data</span></span>
- <span data-ttu-id="f20f9-126">Come modificare i dati e abilitare la convalida dei dati</span><span class="sxs-lookup"><span data-stu-id="f20f9-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="f20f9-127">Come aggiornare lo schema del database</span><span class="sxs-lookup"><span data-stu-id="f20f9-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="f20f9-128">Introduzione</span><span class="sxs-lookup"><span data-stu-id="f20f9-128">Get Started</span></span>

<span data-ttu-id="f20f9-129">Iniziare eseguendo Visual Web Developer 2010 Express (che chiamerò "VWD" d'ora in avanti) e selezionare Nuovo progetto, dalla schermata Start.</span><span class="sxs-lookup"><span data-stu-id="f20f9-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="f20f9-130">Visual Web Developer è un IDE, o un ambiente di sviluppo integrato.</span><span class="sxs-lookup"><span data-stu-id="f20f9-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="f20f9-131">Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni.</span><span class="sxs-lookup"><span data-stu-id="f20f9-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="f20f9-132">È una barra degli strumenti nella parte superiore che mostra le diverse opzioni disponibili si, nonché i menu potrebbe anche avere utilizzato per selezionare File | Nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="f20f9-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="f20f9-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f20f9-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="f20f9-134">Creazione di un'applicazione</span><span class="sxs-lookup"><span data-stu-id="f20f9-134">Creating Your First Application</span></span>

<span data-ttu-id="f20f9-135">È possibile creare applicazioni con Visual Basic o Visual c#.</span><span class="sxs-lookup"><span data-stu-id="f20f9-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="f20f9-136">Per il momento, selezionare Visual c# a sinistra, quindi selezionare "Applicazione Web ASP.NET MVC 2".</span><span class="sxs-lookup"><span data-stu-id="f20f9-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="f20f9-137">Denominare il progetto "Filmati" e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="f20f9-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="f20f9-138">[![Nuovo progetto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f20f9-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="f20f9-139">Sul lato destro è Esplora soluzioni che mostra tutti i file e cartelle nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f20f9-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="f20f9-140">La finestra grande al centro è possibile modificare il codice e dedicare del tempo.</span><span class="sxs-lookup"><span data-stu-id="f20f9-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="f20f9-141">Visual Studio utilizzato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia un'applicazione funzionante ora senza eseguire alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="f20f9-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="f20f9-142">Si tratta di un semplice "Hello World!</span><span class="sxs-lookup"><span data-stu-id="f20f9-142">This is a simple "Hello World!</span></span> <span data-ttu-id="f20f9-143">progetto e è un buon punto di partenza per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f20f9-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="f20f9-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="f20f9-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="f20f9-145">Selezionare il pulsante "play" alla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="f20f9-145">Select the "play" button to the toolbar.</span></span>

![Avvia debug](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="f20f9-147">È una freccia verde che punta a destra che compila il programma e avviare l'applicazione in un web browser.</span><span class="sxs-lookup"><span data-stu-id="f20f9-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="f20f9-148">*Nota: È possibile in alternativa, premere F5 o selezionare Debug -&gt;Avvia debug dal menu "Debug".*</span><span class="sxs-lookup"><span data-stu-id="f20f9-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="f20f9-149">In questo modo Visual Web Developer avviare un server web di sviluppo ed eseguire l'applicazione web (non sono presenti configurazione o una procedura manuale necessaria per abilitare questa opzione).</span><span class="sxs-lookup"><span data-stu-id="f20f9-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="f20f9-150">Verrà quindi avviato un browser e configurarlo per esaminare l'home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f20f9-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="f20f9-151">Notare che la barra degli indirizzi del browser viene visualizzato "localhost" e non un risultato simile example.com sotto. Ciò avviene perché localhost punta sempre al computer locale, che in questo caso è in esecuzione l'applicazione che abbiamo appena creato.</span><span class="sxs-lookup"><span data-stu-id="f20f9-151">Notice below that the address bar of the browser says "localhost", and not something like example.com. That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="f20f9-152">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="f20f9-152">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="f20f9-153">All'esterno della casella tale modello predefinito offre è due pagine da visitare e una pagina di accesso di base.</span><span class="sxs-lookup"><span data-stu-id="f20f9-153">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="f20f9-154">Si modifica il funzionamento di questa applicazione e un po' informazioni su MVC ASP.NET nel processo.</span><span class="sxs-lookup"><span data-stu-id="f20f9-154">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="f20f9-155">Chiudere il browser e consente di modificare il codice.</span><span class="sxs-lookup"><span data-stu-id="f20f9-155">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f20f9-156">avanti</span><span class="sxs-lookup"><span data-stu-id="f20f9-156">Next</span></span>](getting-started-with-mvc-part2.md)
