---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introduzione ad ASP.NET MVC 3 (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: fe98ad8a9f7891b3bf17d2da840d0c333d96a210
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826748"
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="912be-103">Introduzione ad ASP.NET MVC 3 (c#)</span><span class="sxs-lookup"><span data-stu-id="912be-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="912be-104">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="912be-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="912be-105">È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="912be-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="912be-106">È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="912be-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="912be-107">Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="912be-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="912be-108">Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="912be-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="912be-109">È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="912be-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="912be-110">In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="912be-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="912be-111">Prerequisiti di Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="912be-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="912be-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="912be-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="912be-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)</span><span class="sxs-lookup"><span data-stu-id="912be-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="912be-114">Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="912be-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="912be-115">Un progetto di Visual Web Developer con codice sorgente c# è disponibile a complemento di questo argomento.</span><span class="sxs-lookup"><span data-stu-id="912be-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="912be-116">[Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="912be-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="912be-117">Se si preferisce Visual Basic, passare al [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="912be-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="912be-118">Scopo dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="912be-118">What You'll Build</span></span>

<span data-ttu-id="912be-119">Si sarà implementare una semplice applicazione di elenco di film che supporta la creazione, modifica e l'elenco di film da un database.</span><span class="sxs-lookup"><span data-stu-id="912be-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="912be-120">Di seguito sono riportate due schermate dell'applicazione che verrà compilata.</span><span class="sxs-lookup"><span data-stu-id="912be-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="912be-121">Include una pagina che visualizza un elenco di film da un database:</span><span class="sxs-lookup"><span data-stu-id="912be-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="912be-123">L'applicazione consente anche di aggiungere, modificare ed eliminare filmati, nonché le informazioni relative a quelli singoli.</span><span class="sxs-lookup"><span data-stu-id="912be-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="912be-124">Tutti gli scenari di immissione di dati includono la convalida per garantire che i dati archiviati nel database siano corretti.</span><span class="sxs-lookup"><span data-stu-id="912be-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="912be-125">Competenze</span><span class="sxs-lookup"><span data-stu-id="912be-125">Skills You'll Learn</span></span>

<span data-ttu-id="912be-126">Ecco cosa si apprenderà:</span><span class="sxs-lookup"><span data-stu-id="912be-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="912be-127">Come creare un nuovo progetto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="912be-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="912be-128">Come creare ASP.NET MVC controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="912be-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="912be-129">Come creare un nuovo database usando il paradigma Code First di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="912be-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="912be-130">Come recuperare e visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="912be-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="912be-131">Come modificare i dati e abilitare la convalida dei dati.</span><span class="sxs-lookup"><span data-stu-id="912be-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="912be-132">Introduzione</span><span class="sxs-lookup"><span data-stu-id="912be-132">Getting Started</span></span>

<span data-ttu-id="912be-133">Iniziare eseguendo Visual Web Developer 2010 Express ("Visual Web Developer" breve) e selezionare **nuovo progetto** dalle **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="912be-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="912be-134">Visual Web Developer è un IDE, o ambiente di sviluppo integrato.</span><span class="sxs-lookup"><span data-stu-id="912be-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="912be-135">Come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni.</span><span class="sxs-lookup"><span data-stu-id="912be-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="912be-136">In Visual Web Developer è presente una barra degli strumenti nella parte superiore che mostra le diverse opzioni disponibili per l'utente.</span><span class="sxs-lookup"><span data-stu-id="912be-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="912be-137">È inoltre disponibile un menu che fornisce un altro modo per eseguire attività nell'IDE.</span><span class="sxs-lookup"><span data-stu-id="912be-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="912be-138">(Ad esempio, invece di selezionare **nuovo progetto** dal **avviare** pagina, è possibile usare il menu e selezionare **File** &gt; **nuovo progetto**.)</span><span class="sxs-lookup"><span data-stu-id="912be-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="912be-139">Creazione della prima applicazione</span><span class="sxs-lookup"><span data-stu-id="912be-139">Creating Your First Application</span></span>

<span data-ttu-id="912be-140">È possibile creare applicazioni che usano Visual Basic o Visual c# come linguaggio di programmazione.</span><span class="sxs-lookup"><span data-stu-id="912be-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="912be-141">Selezionare Visual c# a sinistra e quindi selezionare **applicazione Web ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="912be-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="912be-142">Denominare il progetto "MvcMovie" e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="912be-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="912be-143">(Se si preferisce Visual Basic, passare al [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.)</span><span class="sxs-lookup"><span data-stu-id="912be-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="912be-144">Nel **nuovo progetto ASP.NET MVC 3** finestra di dialogo **applicazione Internet**.</span><span class="sxs-lookup"><span data-stu-id="912be-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="912be-145">Controllare **markup usare HTML5** lasciando **Razor** come motore di visualizzazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="912be-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="912be-146">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="912be-146">Click **OK**.</span></span> <span data-ttu-id="912be-147">Visual Web Developer usato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia subito un'applicazione funzionante non esegue alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="912be-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="912be-148">Si tratta di un semplice "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="912be-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="912be-149">progetto che 's un buon punto di partenza dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="912be-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="912be-150">Scegliere **Avvia debug** dal menu **Debug**.</span><span class="sxs-lookup"><span data-stu-id="912be-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="912be-151">Si noti che il tasto di scelta rapida per avviare il debug F5.</span><span class="sxs-lookup"><span data-stu-id="912be-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="912be-152">F5 fa in modo che Visual Web Developer avviare un server web di sviluppo ed eseguire l'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="912be-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="912be-153">Quindi, Visual Web Developer viene avviato un browser e verrà visualizzata la home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="912be-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="912be-154">Si noti che la barra degli indirizzi del browser afferma `localhost` e non o simili `example.com`.</span><span class="sxs-lookup"><span data-stu-id="912be-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="912be-155">Infatti, `localhost` fa sempre riferimento al proprio computer locale, consentendo in questo caso è in esecuzione l'applicazione appena compilato.</span><span class="sxs-lookup"><span data-stu-id="912be-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="912be-156">Quando Visual Web Developer viene eseguito un progetto web, viene usata una porta casuale per il server web.</span><span class="sxs-lookup"><span data-stu-id="912be-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="912be-157">Nell'immagine seguente, il numero di porta casuale è 43246.</span><span class="sxs-lookup"><span data-stu-id="912be-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="912be-158">Quando si esegue l'applicazione, si noterà probabilmente un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="912be-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="912be-159">Impostazione predefinita questo modello predefinito offre due pagine di visitare e una pagina di accesso basic.</span><span class="sxs-lookup"><span data-stu-id="912be-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="912be-160">Il passaggio successivo è modificare il comportamento dell'applicazione e imparare un po' su ASP.NET MVC nel processo.</span><span class="sxs-lookup"><span data-stu-id="912be-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="912be-161">Chiudere il browser e modificare codice.</span><span class="sxs-lookup"><span data-stu-id="912be-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="912be-162">avanti</span><span class="sxs-lookup"><span data-stu-id="912be-162">Next</span></span>](adding-a-controller.md)
