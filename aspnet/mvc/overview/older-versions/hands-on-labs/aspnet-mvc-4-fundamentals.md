---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Nozioni fondamentali su ASP.NET MVC 4 | Documenti Microsoft
author: rick-anderson
description: Questa pratica è basato sull'archivio musica MVC (Model View Controller), un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: a0dd32280321938aba84a2aed5273d80750ed774
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="34c2c-103">Nozioni fondamentali su ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="34c2c-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="34c2c-104">Da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="34c2c-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="34c2c-105">Download Web categorie Kit di formazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="34c2c-106">Questa pratica è basato su archivio musica MVC (Model View Controller), un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="34c2c-107">All'interno del laboratorio si apprenderà semplicità, ancora potenza di utilizzo congiunto di queste tecnologie.</span><span class="sxs-lookup"><span data-stu-id="34c2c-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="34c2c-108">Si inizierà con una semplice applicazione e verrà compilato finché non si dispone di un completamente funzionale applicazione Web ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="34c2c-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="34c2c-109">In questo laboratorio funziona con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="34c2c-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="34c2c-110">Se si desidera esplorare la versione di ASP.NET MVC 3 dell'esercitazione applicazione, sarà possibile trovarlo in [negozio di musica MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="34c2c-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="34c2c-111">Questa pratica, si presuppone che lo sviluppatore ha esperienza nelle tecnologie di sviluppo Web, ad esempio HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="34c2c-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="34c2c-112">Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [versioni Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="34c2c-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="34c2c-113">Il progetto specifico per questa esercitazione è disponibile all'indirizzo [nozioni di base di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="34c2c-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="34c2c-114">L'applicazione del Negozio</span><span class="sxs-lookup"><span data-stu-id="34c2c-114">The Music Store application</span></span>

<span data-ttu-id="34c2c-115">L'applicazione web di negozio che verrà compilata in tutto questo Lab è costituito da tre parti principali: market, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="34c2c-116">Visitatori sarà in grado di individuare album per genere, aggiungere album al carrello, esaminare la selezione e infine procedere con l'estrazione di accesso e completare l'ordine.</span><span class="sxs-lookup"><span data-stu-id="34c2c-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="34c2c-117">Inoltre, gli amministratori di archivio sarà in grado di gestire gli album di disponibili, nonché le relative proprietà principale.</span><span class="sxs-lookup"><span data-stu-id="34c2c-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="34c2c-118">![Schermate di negozio](aspnet-mvc-4-fundamentals/_static/image1.png "schermate del Negozio")</span><span class="sxs-lookup"><span data-stu-id="34c2c-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="34c2c-119">*Schermate del Negozio*</span><span class="sxs-lookup"><span data-stu-id="34c2c-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="34c2c-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="34c2c-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="34c2c-121">Applicazione di archiviazione di file musicali sarà creati usando **Controller MVC (Model View)**, un modello architettonico che suddivide un'applicazione in tre componenti principali:</span><span class="sxs-lookup"><span data-stu-id="34c2c-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="34c2c-122">**Modelli**: gli oggetti modello rappresentano le parti dell'applicazione che implementano la logica di dominio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="34c2c-123">Spesso, anche gli oggetti del modello recupero e archiviano lo stato del modello in un database.</span><span class="sxs-lookup"><span data-stu-id="34c2c-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="34c2c-124">**Visualizzazioni:** visualizzazioni sono i componenti che consentono di visualizzare l'interfaccia utente (UI).</span><span class="sxs-lookup"><span data-stu-id="34c2c-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="34c2c-125">In genere, questa interfaccia viene creata da dati del modello.</span><span class="sxs-lookup"><span data-stu-id="34c2c-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="34c2c-126">Un esempio potrebbe essere la visualizzazione di modifica dell'album che consente di visualizzare le caselle di testo e un elenco a discesa in base allo stato corrente di un oggetto Album.</span><span class="sxs-lookup"><span data-stu-id="34c2c-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="34c2c-127">**Controller:** controller sono i componenti che gestiscono l'interazione dell'utente, modificare il modello e selezionano infine una visualizzazione per il rendering dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="34c2c-128">In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="34c2c-129">Il modello MVC consente di creare applicazioni che separano i diversi aspetti dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), fornendo un accoppiamento libero tra questi elementi.</span><span class="sxs-lookup"><span data-stu-id="34c2c-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="34c2c-130">Questa separazione consente di gestire la complessità quando si compila un'applicazione, in quanto consente di concentrarsi su un aspetto dell'implementazione alla volta.</span><span class="sxs-lookup"><span data-stu-id="34c2c-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="34c2c-131">Inoltre, il modello MVC rende semplice testare le applicazioni, incoraggiando anche l'utilizzo di sviluppo basato su test (TDD) per la creazione di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="34c2c-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="34c2c-132">Il **ASP.NET MVC** framework offre un'alternativa al modello Web Form ASP.NET per la creazione di applicazioni Web basate su ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="34c2c-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="34c2c-133">Il **ASP.NET MVC** framework è un framework di presentazione leggero e facile da testare che (come con applicazioni basate sul Web Form) è integrato con le funzionalità ASP.NET esistenti, ad esempio pagine master e basata sull'appartenenza autenticazione in modo da tutte le potenzialità di .NET framework core.</span><span class="sxs-lookup"><span data-stu-id="34c2c-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="34c2c-134">Ciò è utile se si ha già familiarità con Web Form ASP.NET perché tutte le raccolte già in uso sono anche disponibili in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="34c2c-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="34c2c-135">Inoltre, l'accoppiamento debole tra i tre componenti principali di un'applicazione MVC Alza di livello anche lo sviluppo parallelo.</span><span class="sxs-lookup"><span data-stu-id="34c2c-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="34c2c-136">Ad esempio, uno sviluppatore può utilizzare la visualizzazione, uno sviluppatore secondo gli strumenti per la logica di controller e un terzo può concentrarsi sulla logica di business nel modello.</span><span class="sxs-lookup"><span data-stu-id="34c2c-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="34c2c-137">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="34c2c-137">Objectives</span></span>

<span data-ttu-id="34c2c-138">In questa pratica, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="34c2c-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="34c2c-139">Creare un'applicazione ASP.NET MVC da zero in base l'esercitazione musica archivio applicazioni</span><span class="sxs-lookup"><span data-stu-id="34c2c-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="34c2c-140">Aggiungere controller per gestire gli URL per la Home page del sito e per esplorare le funzionalità principali</span><span class="sxs-lookup"><span data-stu-id="34c2c-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="34c2c-141">Aggiungere una visualizzazione per personalizzare il contenuto visualizzato unitamente al proprio stile</span><span class="sxs-lookup"><span data-stu-id="34c2c-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="34c2c-142">Aggiungere le classi di modello per contenere e gestire la logica di dati e di dominio</span><span class="sxs-lookup"><span data-stu-id="34c2c-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="34c2c-143">Utilizzare il modello di modello di visualizzazione per passare le informazioni da azioni del controller per i modelli di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="34c2c-144">Esplorare il nuovo modello di ASP.NET MVC 4 per le applicazioni internet</span><span class="sxs-lookup"><span data-stu-id="34c2c-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="34c2c-145">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="34c2c-145">Prerequisites</span></span>

<span data-ttu-id="34c2c-146">È necessario che gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="34c2c-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="34c2c-147">[Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (lettura [appendice](#AppendixA) per istruzioni sulla modalità di installazione)</span><span class="sxs-lookup"><span data-stu-id="34c2c-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="34c2c-148">Configurazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-148">Setup</span></span>

<span data-ttu-id="34c2c-149">**L'installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="34c2c-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="34c2c-150">Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="34c2c-151">Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="34c2c-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="34c2c-152">Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: con i frammenti di codice](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="34c2c-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="34c2c-153">Esercizi</span><span class="sxs-lookup"><span data-stu-id="34c2c-153">Exercises</span></span>

<span data-ttu-id="34c2c-154">Questa pratica include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="34c2c-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="34c2c-155">Esercizio 1: Creazione progetto di applicazione Web ASP.NET MVC MusicStore</span><span class="sxs-lookup"><span data-stu-id="34c2c-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="34c2c-156">Esercizio 2: Creazione di un Controller</span><span class="sxs-lookup"><span data-stu-id="34c2c-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="34c2c-157">Esercizio 3: Passaggio di parametri a un Controller</span><span class="sxs-lookup"><span data-stu-id="34c2c-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="34c2c-158">Esercizio 4: Creazione di una vista</span><span class="sxs-lookup"><span data-stu-id="34c2c-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="34c2c-159">Esercizio 5: Creazione di un modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="34c2c-160">Esercizio 6: Utilizzo di parametri nella visualizzazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="34c2c-161">Esercizio 7: Un'introduzione ad ASP.NET MVC 4 nuovo modello</span><span class="sxs-lookup"><span data-stu-id="34c2c-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="34c2c-162">Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="34c2c-163">Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="34c2c-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="34c2c-164">Tempo stimato per completare questa esercitazione: **60 minuti**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="34c2c-165">Esercizio 1: Creazione progetto di applicazione Web ASP.NET MVC MusicStore</span><span class="sxs-lookup"><span data-stu-id="34c2c-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="34c2c-166">In questo esercizio, si apprenderà come creare un'applicazione ASP.NET MVC in Visual Studio Express 2012 per Web, nonché l'organizzazione della cartella principale.</span><span class="sxs-lookup"><span data-stu-id="34c2c-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="34c2c-167">Inoltre, si apprenderà come aggiungere un nuovo Controller e una stringa semplice di visualizzarlo nella home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="34c2c-168">Attività 1 - Creazione del progetto di applicazione Web ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="34c2c-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="34c2c-169">In questa attività si creerà un progetto di applicazione MVC ASP.NET vuoto utilizzando il modello MVC Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="34c2c-170">Avviare **Visual Studio Express per Web**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="34c2c-171">Scegliere **Nuovo progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="34c2c-172">Nel **nuovo progetto** finestra di dialogo selezionare il **applicazione Web ASP.NET MVC 4** progetto di tipo, che si trova in **Visual c#,** **Web** modello elenco.</span><span class="sxs-lookup"><span data-stu-id="34c2c-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="34c2c-173">Modifica il **nome** a *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="34c2c-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="34c2c-174">Impostare il percorso della soluzione all'interno di un nuovo **iniziare** cartella nella cartella di origine di questo esercizio, ad esempio **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-HOL-PATH]**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="34c2c-175">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-175">Click **OK**.</span></span>

    <span data-ttu-id="34c2c-176">![Creare una finestra di dialogo Nuovo progetto](aspnet-mvc-4-fundamentals/_static/image2.png "creare una finestra di dialogo Nuovo progetto")</span><span class="sxs-lookup"><span data-stu-id="34c2c-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="34c2c-177">*Creare una finestra di dialogo Nuovo progetto*</span><span class="sxs-lookup"><span data-stu-id="34c2c-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="34c2c-178">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo selezionare il **base** modello e assicurarsi che il **motore di visualizzazione** selezionato è **Razor**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="34c2c-179">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-179">Click **OK**.</span></span>

    <span data-ttu-id="34c2c-180">![Finestra di dialogo Nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "finestra di dialogo Nuovo progetto ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="34c2c-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="34c2c-181">*Finestra di dialogo Nuovo progetto ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="34c2c-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="34c2c-182">Attività 2: esplorare la struttura della soluzione</span><span class="sxs-lookup"><span data-stu-id="34c2c-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="34c2c-183">Il framework di MVC ASP.NET include un modello di progetto di Visual Studio che consente di creare applicazioni Web che supporta il modello MVC.</span><span class="sxs-lookup"><span data-stu-id="34c2c-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="34c2c-184">Questo modello consente di creare una nuova applicazione Web MVC ASP.NET con le cartelle necessarie, modelli di elementi e le voci di file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="34c2c-185">In questa attività si esaminerà la struttura della soluzione per comprendere gli elementi coinvolti e le relative relazioni.</span><span class="sxs-lookup"><span data-stu-id="34c2c-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="34c2c-186">Le cartelle seguenti vengono inclusi nel tutti l'applicazione ASP.NET MVC, perché per impostazione predefinita il framework di MVC ASP.NET utilizza un &quot;convenzione configurazione&quot; approccio e rende alcuni presupposti in base a nomi di cartella convenzioni.</span><span class="sxs-lookup"><span data-stu-id="34c2c-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="34c2c-187">Una volta creato il progetto, esaminare la struttura di cartelle che è stata creata in Esplora soluzioni sul lato destro:</span><span class="sxs-lookup"><span data-stu-id="34c2c-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="34c2c-188">![Struttura di cartelle di MVC ASP.NET in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image4.png "struttura di cartelle di MVC ASP.NET in Esplora soluzioni")</span><span class="sxs-lookup"><span data-stu-id="34c2c-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="34c2c-189">*Struttura di cartelle di MVC ASP.NET in Esplora soluzioni*</span><span class="sxs-lookup"><span data-stu-id="34c2c-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="34c2c-190">**Controller**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-190">**Controllers**.</span></span> <span data-ttu-id="34c2c-191">Questa cartella contiene le classi controller.</span><span class="sxs-lookup"><span data-stu-id="34c2c-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="34c2c-192">In un'applicazione MVC di base, i controller sono responsabili per la gestione di interazione dell'utente finale, la modifica del modello e infine scegliendo una visualizzazione per il rendering dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="34c2c-193">Il framework MVC richiede che i nomi di tutti i controller terminino con &quot;Controller&quot;-ad esempio HomeController, LoginController o ProductController.</span><span class="sxs-lookup"><span data-stu-id="34c2c-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="34c2c-194">**Modelli**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-194">**Models**.</span></span> <span data-ttu-id="34c2c-195">Questa cartella viene fornita per le classi che rappresentano il modello di applicazione per l'applicazione Web MVC.</span><span class="sxs-lookup"><span data-stu-id="34c2c-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="34c2c-196">Questo include in genere il codice che definisce gli oggetti e la logica per l'interazione con l'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="34c2c-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="34c2c-197">In genere, gli oggetti modello effettivo sarà nelle librerie di classi separato.</span><span class="sxs-lookup"><span data-stu-id="34c2c-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="34c2c-198">Tuttavia, quando si crea una nuova applicazione, si includono le classi e spostarle in librerie di classi separate in un momento successivo nel ciclo di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="34c2c-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="34c2c-199">**Viste**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-199">**Views**.</span></span> <span data-ttu-id="34c2c-200">Questa cartella è la posizione consigliata per le viste, i componenti responsabili per la visualizzazione dell'interfaccia utente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="34c2c-201">Le visualizzazioni utilizzano file con estensione aspx, ascx, cshtml e master, oltre a qualsiasi altro file che sono correlato al rendering delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="34c2c-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="34c2c-202">Cartella Views contiene una cartella per ogni controller. la cartella è denominata con il prefisso del nome del controller.</span><span class="sxs-lookup"><span data-stu-id="34c2c-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="34c2c-203">Ad esempio, se si dispone di un controller denominato **HomeController**, la cartella Views conterrà una cartella Home.</span><span class="sxs-lookup"><span data-stu-id="34c2c-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="34c2c-204">Per impostazione predefinita, quando il framework di MVC ASP.NET carica una visualizzazione, la ricerca di un file con estensione aspx con il nome della visualizzazione richiesta nella cartella Views\controllerName (**viste [ControllerName] [azione] aspx**) o (**viste [ControllerName] [Azione] cshtml**) per le visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="34c2c-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="34c2c-205">Oltre alle cartelle elencate in precedenza, un'applicazione Web MVC utilizza il **Global. asax** impostazioni predefinite di file per impostare il routing degli URL globale e viene utilizzato il **Web. config** file per configurare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="34c2c-206">Attività 3: aggiunta di un HomeController</span><span class="sxs-lookup"><span data-stu-id="34c2c-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="34c2c-207">Nelle applicazioni ASP.NET che non utilizzano il framework di MVC, interazione dell'utente è organizzata intorno alle pagine e intorno alla generazione e la gestione degli eventi da tali pagine.</span><span class="sxs-lookup"><span data-stu-id="34c2c-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="34c2c-208">Al contrario, interazione dell'utente con le applicazioni ASP.NET MVC è organizzata intorno a controller e i relativi metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="34c2c-209">D'altra parte, framework di MVC ASP.NET esegue il mapping degli URL alle classi che vengono definite i controller.</span><span class="sxs-lookup"><span data-stu-id="34c2c-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="34c2c-210">I controller di elaborare le richieste in ingresso, gestire l'input dell'utente e le interazioni, eseguire la logica di applicazione appropriata e determinare la risposta da inviare al client (visualizzazione del contenuto HTML, scaricare un file, reindirizzare a un altro URL e così via).</span><span class="sxs-lookup"><span data-stu-id="34c2c-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="34c2c-211">Nel caso di visualizzazione HTML, una classe controller chiama in genere un componente di visualizzazione separato per generare il markup HTML per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="34c2c-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="34c2c-212">In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="34c2c-213">In questa attività si aggiungerà una classe Controller che gestirà gli URL per la Home page del sito Negozio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="34c2c-214">Fare doppio clic su **controller** cartella in Esplora soluzioni, selezionare **Aggiungi** e quindi **Controller** comando:</span><span class="sxs-lookup"><span data-stu-id="34c2c-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="34c2c-215">![Aggiungere un comando Controller](aspnet-mvc-4-fundamentals/_static/image5.png "aggiungere un comando Controller")</span><span class="sxs-lookup"><span data-stu-id="34c2c-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="34c2c-216">*Aggiungere il comando Controller*</span><span class="sxs-lookup"><span data-stu-id="34c2c-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="34c2c-217">Il **Aggiungi Controller** viene visualizzata la finestra.</span><span class="sxs-lookup"><span data-stu-id="34c2c-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="34c2c-218">Denominare il controller *HomeController* e premere **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="34c2c-219">![Finestra di dialogo Controller Aggiungi](aspnet-mvc-4-fundamentals/_static/image6.png "Controller finestra di dialogo Aggiungi")</span><span class="sxs-lookup"><span data-stu-id="34c2c-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="34c2c-220">*Aggiungere una finestra di dialogo Controller*</span><span class="sxs-lookup"><span data-stu-id="34c2c-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="34c2c-221">Il file **HomeController.cs** viene creato nel **controller** cartella.</span><span class="sxs-lookup"><span data-stu-id="34c2c-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="34c2c-222">Per avere la **HomeController** restituire una stringa nella relativa operazione sull'indice, sostituire il **indice** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="34c2c-223">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex1 HomeController indice*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]
~~~

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="34c2c-224">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="34c2c-225">In questa attività verrà provare l'applicazione in un web browser.</span><span class="sxs-lookup"><span data-stu-id="34c2c-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="34c2c-226">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="34c2c-227">La compilazione del progetto e viene avviato il Server Web IIS locale.</span><span class="sxs-lookup"><span data-stu-id="34c2c-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="34c2c-228">Locale Server Web IIS verrà aperto automaticamente un web browser che punta all'URL del server Web.</span><span class="sxs-lookup"><span data-stu-id="34c2c-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="34c2c-229">![Applicazione in esecuzione in un web browser](aspnet-mvc-4-fundamentals/_static/image7.png "applicazione in esecuzione in un web browser")</span><span class="sxs-lookup"><span data-stu-id="34c2c-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="34c2c-230">*Applicazione in esecuzione in un web browser*</span><span class="sxs-lookup"><span data-stu-id="34c2c-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="34c2c-231">Il Server Web IIS locale verrà eseguito il sito Web su un numero casuale porta libera.</span><span class="sxs-lookup"><span data-stu-id="34c2c-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="34c2c-232">Nella figura precedente, il sito è in esecuzione in `http://localhost:50103/`, pertanto è usando porta 50103.</span><span class="sxs-lookup"><span data-stu-id="34c2c-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="34c2c-233">Il numero di porta può variare.</span><span class="sxs-lookup"><span data-stu-id="34c2c-233">Your port number may vary.</span></span>
2. <span data-ttu-id="34c2c-234">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="34c2c-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="34c2c-235">Esercizio 2: Creazione di un Controller</span><span class="sxs-lookup"><span data-stu-id="34c2c-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="34c2c-236">In questo esercizio, si apprenderà come aggiornare il controller per implementare la funzionalità semplice dell'applicazione Negozio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="34c2c-237">Tale controller verrà definiti i metodi di azione per gestire le richieste specifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="34c2c-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="34c2c-238">Una pagina di elenco di generi di musica nell'archivio di musica</span><span class="sxs-lookup"><span data-stu-id="34c2c-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="34c2c-239">Una pagina di esplorazione che elenca tutti gli album musica per un particolare genere</span><span class="sxs-lookup"><span data-stu-id="34c2c-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="34c2c-240">Una pagina di dettagli che contiene informazioni su un album musicali specifici</span><span class="sxs-lookup"><span data-stu-id="34c2c-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="34c2c-241">Per l'ambito di questo esercizio, tali azioni restituirà semplicemente una stringa a questo punto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="34c2c-242">Attività 1 - aggiunta di una nuova classe StoreController</span><span class="sxs-lookup"><span data-stu-id="34c2c-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="34c2c-243">In questa attività si aggiungerà un nuovo Controller.</span><span class="sxs-lookup"><span data-stu-id="34c2c-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="34c2c-244">Se non è già aperta, avviare **Visual Studio Express per Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="34c2c-245">Nel **File** menu, scegliere **Apri progetto**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="34c2c-246">Nella finestra di dialogo Apri progetto individuare **Source\Ex02 CreatingAController\Begin**selezionare **Begin.sln** e fare clic su **aprire**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="34c2c-247">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="34c2c-248">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="34c2c-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="34c2c-249">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="34c2c-250">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="34c2c-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="34c2c-251">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="34c2c-252">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="34c2c-253">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="34c2c-254">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="34c2c-255">Aggiungere il nuovo controller.</span><span class="sxs-lookup"><span data-stu-id="34c2c-255">Add the new controller.</span></span> <span data-ttu-id="34c2c-256">A tale scopo, fare doppio clic su di **controller** cartella in Esplora soluzioni, selezionare **Aggiungi** e quindi la **Controller** comando.</span><span class="sxs-lookup"><span data-stu-id="34c2c-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="34c2c-257">Modifica il **nome Controller** a *StoreController*, fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="34c2c-258">![Finestra di dialogo Controller Aggiungi](aspnet-mvc-4-fundamentals/_static/image8.png "Controller finestra di dialogo Aggiungi")</span><span class="sxs-lookup"><span data-stu-id="34c2c-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="34c2c-259">*Aggiungere una finestra di dialogo Controller*</span><span class="sxs-lookup"><span data-stu-id="34c2c-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="34c2c-260">Attività 2: Modifica azioni del StoreController</span><span class="sxs-lookup"><span data-stu-id="34c2c-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="34c2c-261">In questa attività si modificherà i Controller i metodi chiamati **azioni**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="34c2c-262">Le azioni sono responsabili per la gestione delle richieste di URL e determinare il contenuto che deve essere inviato nuovamente il browser o l'utente che ha richiamato l'URL.</span><span class="sxs-lookup"><span data-stu-id="34c2c-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="34c2c-263">Il **StoreController** classe esiste già un **indice** metodo.</span><span class="sxs-lookup"><span data-stu-id="34c2c-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="34c2c-264">Si utilizzerà più avanti in questa esercitazione per implementare la pagina che elenca tutti i generi dell'archivio di musica.</span><span class="sxs-lookup"><span data-stu-id="34c2c-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="34c2c-265">Per il momento, è sufficiente sostituire il **indice** (metodo) con il codice seguente che restituisce una stringa &quot;Hello da Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="34c2c-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="34c2c-266">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - indice StoreController Ex2*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
~~~
2. <span data-ttu-id="34c2c-267">Aggiungere **Sfoglia** e **dettagli** metodi.</span><span class="sxs-lookup"><span data-stu-id="34c2c-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="34c2c-268">A tale scopo, aggiungere il codice seguente per il **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="34c2c-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="34c2c-269">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="34c2c-270">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="34c2c-271">In questa attività verrà provare l'applicazione in un web browser.</span><span class="sxs-lookup"><span data-stu-id="34c2c-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="34c2c-272">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="34c2c-273">Avvia il progetto nel **Home** pagina.</span><span class="sxs-lookup"><span data-stu-id="34c2c-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="34c2c-274">Modificare l'URL per verificare l'implementazione di ogni azione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="34c2c-275">**/ Archiviare**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-275">**/Store**.</span></span> <span data-ttu-id="34c2c-276">Si noterà  **&quot;Hello da Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="34c2c-277">**/Store/Browse**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-277">**/Store/Browse**.</span></span> <span data-ttu-id="34c2c-278">Si noterà  **&quot;Hello da Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="34c2c-279">**/ / Dettagli archivio**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-279">**/Store/Details**.</span></span> <span data-ttu-id="34c2c-280">Si noterà  **&quot;Hello da Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="34c2c-281">![Esplorazione StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "esplorazione StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="34c2c-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="34c2c-282">*Esplorazione /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="34c2c-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="34c2c-283">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="34c2c-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="34c2c-284">Esercizio 3: Passaggio di parametri a un Controller</span><span class="sxs-lookup"><span data-stu-id="34c2c-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="34c2c-285">Fino ad ora, si hanno stato restituzione stringhe costanti verso i controller.</span><span class="sxs-lookup"><span data-stu-id="34c2c-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="34c2c-286">In questo esercizio si apprenderà come passare i parametri a un Controller utilizzando l'URL e la stringa di query e apportando quindi le azioni di metodo rispondere con il testo nel browser.</span><span class="sxs-lookup"><span data-stu-id="34c2c-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="34c2c-287">Attività 1 - aggiunta parametro Genre StoreController</span><span class="sxs-lookup"><span data-stu-id="34c2c-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="34c2c-288">In questa attività, si utilizzerà il **querystring** per l'invio di parametri per il **Sfoglia** metodo di azione di **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="34c2c-289">Se non è già aperta, avviare **Visual Studio Express per Web**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="34c2c-290">Nel **File** menu, scegliere **Apri progetto**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="34c2c-291">Nella finestra di dialogo Apri progetto individuare **Source\Ex03 PassingParametersToAController\Begin**selezionare **Begin.sln** e fare clic su **aprire**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="34c2c-292">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="34c2c-293">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="34c2c-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="34c2c-294">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="34c2c-295">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="34c2c-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="34c2c-296">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="34c2c-297">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="34c2c-298">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="34c2c-299">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="34c2c-300">Aprire **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-300">Open **StoreController** class.</span></span> <span data-ttu-id="34c2c-301">A tale scopo, nel **Esplora**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="34c2c-302">Modifica il **Sfoglia** (metodo), aggiunta di un parametro di stringa per richiedere per un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="34c2c-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="34c2c-303">ASP.NET MVC verranno automaticamente passare qualsiasi stringa di query o parametri denominati invio del form **genere** a questo metodo di azione quando viene richiamato.</span><span class="sxs-lookup"><span data-stu-id="34c2c-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="34c2c-304">A tale scopo, sostituire il **Sfoglia** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="34c2c-305">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.
> 
> For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="34c2c-306">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-306">Task 2 - Running the Application</span></span>

<span data-ttu-id="34c2c-307">In questa attività si provare l'applicazione in un web browser e si utilizzerà il **genre** parametro.</span><span class="sxs-lookup"><span data-stu-id="34c2c-307">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="34c2c-308">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-308">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="34c2c-309">Avvia il progetto nel **Home** pagina.</span><span class="sxs-lookup"><span data-stu-id="34c2c-309">The project starts in the **Home** page.</span></span> <span data-ttu-id="34c2c-310">Modificare l'URL in   */archiviare/Sfoglia? Genre = Disco* per verificare che l'azione riceve il parametro genere.</span><span class="sxs-lookup"><span data-stu-id="34c2c-310">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="34c2c-311">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span><span class="sxs-lookup"><span data-stu-id="34c2c-311">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="34c2c-312">*Esplorazione /Store/Browse? Genre = Disco*</span><span class="sxs-lookup"><span data-stu-id="34c2c-312">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="34c2c-313">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="34c2c-313">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="34c2c-314">Attività 3: aggiunta di un parametro Id incorporato nell'URL</span><span class="sxs-lookup"><span data-stu-id="34c2c-314">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="34c2c-315">In questa attività, si utilizzerà il **URL** per passare un **Id** parametro per il **dettagli** il metodo di azione del **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-315">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="34c2c-316">Il valore predefinito di ASP.NET MVC convenzione di routing consiste nel considerare il segmento di un URL dopo il nome del metodo di azione come un parametro denominato **Id**. Se il metodo di azione presenta un parametro denominato Id, quindi ASP.NET MVC automaticamente passerà il segmento URL all'utente come parametro.</span><span class="sxs-lookup"><span data-stu-id="34c2c-316">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="34c2c-317">Nell'URL **archivio/dettagli/5**, **Id** verrà interpretato come **5**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-317">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="34c2c-318">Modifica il **dettagli** metodo il **StoreController**, aggiungendo un **int** parametro denominato **id**. A tale scopo, sostituire il **dettagli** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-318">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="34c2c-319">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-319">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="34c2c-320">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-320">Task 4 - Running the Application</span></span>

<span data-ttu-id="34c2c-321">In questa attività si provare l'applicazione in un web browser e si utilizzerà il **Id** parametro.</span><span class="sxs-lookup"><span data-stu-id="34c2c-321">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="34c2c-322">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-322">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="34c2c-323">Avvia il progetto nel **Home** pagina.</span><span class="sxs-lookup"><span data-stu-id="34c2c-323">The project starts in the **Home** page.</span></span> <span data-ttu-id="34c2c-324">Modificare l'URL in */Store/Details/5* per verificare che l'azione riceve il parametro id.</span><span class="sxs-lookup"><span data-stu-id="34c2c-324">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="34c2c-325">![Esplorazione StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "esplorazione StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="34c2c-325">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="34c2c-326">*Esplorazione /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="34c2c-326">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="34c2c-327">Esercizio 4: Creazione di una vista</span><span class="sxs-lookup"><span data-stu-id="34c2c-327">Exercise 4: Creating a View</span></span>

<span data-ttu-id="34c2c-328">Hanno finora è stato restituire stringhe da azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="34c2c-328">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="34c2c-329">Sebbene questo sia un modo utile per conoscere il funzionamento di controller, non è come vengono compilate le applicazioni Web reali.</span><span class="sxs-lookup"><span data-stu-id="34c2c-329">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="34c2c-330">Le visualizzazioni sono componenti che forniscono un approccio migliore per generare codice HTML al browser con l'utilizzo di file di modello.</span><span class="sxs-lookup"><span data-stu-id="34c2c-330">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="34c2c-331">In questo esercizio si apprenderà come aggiungere una pagina master layout per la configurazione di un modello di contenuto HTML comune, un foglio di stile per migliorare l'aspetto del sito e, infine, un modello di visualizzazione per abilitare HomeController restituire l'HTML.</span><span class="sxs-lookup"><span data-stu-id="34c2c-331">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="34c2c-332">Attività 1: modificare il file \_cshtml</span><span class="sxs-lookup"><span data-stu-id="34c2c-332">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="34c2c-333">Il file **~/Views/Shared/\_cshtml** consente di configurare un modello per HTML comuni da utilizzare per l'intero sito Web.</span><span class="sxs-lookup"><span data-stu-id="34c2c-333">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="34c2c-334">In questa attività si aggiungerà una pagina master layout con un'intestazione comune con i collegamenti all'area Home page e l'archivio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-334">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="34c2c-335">Se non è già aperta, avviare **Visual Studio Express per Web**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-335">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="34c2c-336">Nel **File** menu, scegliere **Apri progetto**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-336">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="34c2c-337">Nella finestra di dialogo Apri progetto individuare **Source\Ex04 CreatingAView\Begin**selezionare **Begin.sln** e fare clic su **aprire**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-337">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="34c2c-338">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-338">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="34c2c-339">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="34c2c-339">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="34c2c-340">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-340">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="34c2c-341">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="34c2c-341">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="34c2c-342">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-342">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="34c2c-343">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-343">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="34c2c-344">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-344">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="34c2c-345">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-345">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="34c2c-346">Il file  <strong>\_cshtml</strong> contiene il layout di contenitore HTML per tutte le pagine nel sito.</span><span class="sxs-lookup"><span data-stu-id="34c2c-346">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="34c2c-347">Include il <strong>&lt;html&gt;</strong> elemento per la risposta HTML, nonché il <strong>&lt;head&gt;</strong> e <strong>&lt;corpo&gt;</strong> elementi.</span><span class="sxs-lookup"><span data-stu-id="34c2c-347">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="34c2c-348"><strong>@RenderBody()</strong> all'interno dell'HTML corpo identificare le aree di visualizzazione modelli saranno in grado di inserire il contenuto dinamico.</span><span class="sxs-lookup"><span data-stu-id="34c2c-348"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="34c2c-349">(C#)</span><span class="sxs-lookup"><span data-stu-id="34c2c-349">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="34c2c-350">Aggiungere un'intestazione comune con i collegamenti all'area Home page e l'archivio in tutte le pagine nel sito.</span><span class="sxs-lookup"><span data-stu-id="34c2c-350">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="34c2c-351">A tale scopo, aggiungere il codice seguente sotto &lt;corpo&gt; istruzione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-351">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="34c2c-352">(C#)</span><span class="sxs-lookup"><span data-stu-id="34c2c-352">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="34c2c-353">Includere un elemento div per eseguire il rendering nella sezione corpo di ogni pagina.</span><span class="sxs-lookup"><span data-stu-id="34c2c-353">Include a div to render the body section of each page.</span></span> <span data-ttu-id="34c2c-354">Sostituire  <strong>@RenderBody()</strong> con il seguente codice higlighted: (c#)</span><span class="sxs-lookup"><span data-stu-id="34c2c-354">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="34c2c-355">Non tutti sanno.</span><span class="sxs-lookup"><span data-stu-id="34c2c-355">Did you know?</span></span> <span data-ttu-id="34c2c-356">Visual Studio 2012 include frammenti di codice che rendono più semplice aggiungere codice di uso comune in HTML, file di codice e altro ancora!</span><span class="sxs-lookup"><span data-stu-id="34c2c-356">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="34c2c-357">Provarlo digitando **&lt;div&gt;** e premendo **scheda** due volte per inserire un completo **div** tag.</span><span class="sxs-lookup"><span data-stu-id="34c2c-357">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="34c2c-358">Attività 2: aggiunta foglio di stile CSS</span><span class="sxs-lookup"><span data-stu-id="34c2c-358">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="34c2c-359">Il modello di progetto vuoto include un file CSS molto semplice che include solo gli stili utilizzati per visualizzare i form di base e i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="34c2c-359">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="34c2c-360">Si utilizzerà per migliorare l'aspetto del sito aggiuntivi CSS e immagini (potenzialmente fornite da una finestra di progettazione).</span><span class="sxs-lookup"><span data-stu-id="34c2c-360">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="34c2c-361">In questa attività si aggiungerà un foglio di stile CSS per definire gli stili del sito.</span><span class="sxs-lookup"><span data-stu-id="34c2c-361">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="34c2c-362">Il file CSS e le immagini sono inclusi nel **Source\Assets\Content** cartella di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-362">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="34c2c-363">Per aggiungerle all'applicazione, trascinare i propri contenuti da un **Esplora** finestra all'interno di **Esplora soluzioni** in Visual Studio Express per Web, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="34c2c-363">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="34c2c-364">![Trascinare il contenuto di stile](aspnet-mvc-4-fundamentals/_static/image12.png "trascinando il contenuto di stile")</span><span class="sxs-lookup"><span data-stu-id="34c2c-364">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="34c2c-365">*Trascinare il contenuto di stile*</span><span class="sxs-lookup"><span data-stu-id="34c2c-365">*Dragging style contents*</span></span>
2. <span data-ttu-id="34c2c-366">Verrà visualizzata una finestra di dialogo di avviso, in cui viene chiesto di confermare se sostituire **Site** file e alcune immagini esistente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-366">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="34c2c-367">Controllare **applica a tutti gli elementi** e fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-367">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="34c2c-368">Attività 3: aggiunta di un modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-368">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="34c2c-369">In questa attività si aggiungerà un modello di visualizzazione per generare la risposta HTML che verrà utilizzata la pagina master layout e CSS aggiunti in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-369">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="34c2c-370">Per utilizzare un modello di visualizzazione durante l'esplorazione delle home page del sito, è necessario innanzitutto indicare che invece di restituire una stringa, il **HomeController indice** metodo restituirà un **vista**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-370">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="34c2c-371">Aprire **HomeController** classe e modificare il relativo **indice** per restituire un **ActionResult**, e restituire **View()**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-371">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="34c2c-372">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex4 HomeController indice*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-372">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
~~~
2. <span data-ttu-id="34c2c-373">A questo punto, è necessario aggiungere un modello di visualizzazione appropriato.</span><span class="sxs-lookup"><span data-stu-id="34c2c-373">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="34c2c-374">A tale scopo, **destro** all'interno di **indice** metodo di azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-374">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="34c2c-375">Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="34c2c-375">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="34c2c-376">![Aggiunta di una vista all'interno del metodo indice](aspnet-mvc-4-fundamentals/_static/image13.png "aggiunta di una vista all'interno del metodo di indice")</span><span class="sxs-lookup"><span data-stu-id="34c2c-376">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="34c2c-377">*Aggiunta di una vista all'interno del metodo di indice*</span><span class="sxs-lookup"><span data-stu-id="34c2c-377">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="34c2c-378">Il **Aggiungi visualizzazione** finestra di dialogo verrà visualizzata per generare un file di modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-378">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="34c2c-379">Per impostazione predefinita, questa finestra di dialogo pre-consente di popolare il nome del modello di visualizzazione in modo che corrisponda il metodo di azione che verrà utilizzato.</span><span class="sxs-lookup"><span data-stu-id="34c2c-379">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="34c2c-380">Perché è stata utilizzata il **Aggiungi visualizzazione** menu di scelta rapida all'interno di **indice** il metodo di azione all'interno di HomeController, il **Aggiungi visualizzazione** finestra di dialogo ha indice come il nome della visualizzazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="34c2c-380">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="34c2c-381">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-381">Click **Add**.</span></span>

    <span data-ttu-id="34c2c-382">![Visualizza finestra di dialogo Aggiungi](aspnet-mvc-4-fundamentals/_static/image14.png "Visualizza finestra di dialogo Aggiungi")</span><span class="sxs-lookup"><span data-stu-id="34c2c-382">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="34c2c-383">*Visualizza finestra di dialogo Aggiungi*</span><span class="sxs-lookup"><span data-stu-id="34c2c-383">*Add View Dialog*</span></span>
4. <span data-ttu-id="34c2c-384">Visual Studio genera un **cshtml** il modello di visualizzazione all'interno di **Views\Home** cartella e quindi lo apre.</span><span class="sxs-lookup"><span data-stu-id="34c2c-384">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="34c2c-385">![Visualizzazione dell'indice creato Home](aspnet-mvc-4-fundamentals/_static/image15.png "Vista Home indice creato")</span><span class="sxs-lookup"><span data-stu-id="34c2c-385">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="34c2c-386">*Visualizzazione dell'indice iniziale creato*</span><span class="sxs-lookup"><span data-stu-id="34c2c-386">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="34c2c-387">nome e percorso del **cshtml** file è rilevante e segue le convenzioni di denominazione predefinito MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="34c2c-387">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="34c2c-388">La cartella \Views\**Home** corrisponde al nome del controller (**Home** Controller).</span><span class="sxs-lookup"><span data-stu-id="34c2c-388">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="34c2c-389">Il nome del modello di visualizzazione (**indice**), il metodo di azione di controller che prevede di visualizzare la vista corrispondente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-389">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="34c2c-390">In questo modo, ASP.NET MVC consente di evitare la necessità di specificare in modo esplicito il nome o il percorso di un modello di visualizzazione quando si utilizza questa convenzione di denominazione per restituire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-390">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="34c2c-391">Il modello di visualizzazione generato si basa sul  **\_cshtml** modello definito in precedenza.</span><span class="sxs-lookup"><span data-stu-id="34c2c-391">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="34c2c-392">Aggiornare la proprietà ViewBag **Home**e modificare il contenuto principale per **questa è la Home Page**, come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-392">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>


~~~
[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
~~~
6. <span data-ttu-id="34c2c-393">Selezionare **MvcMusicStore** progetto in Esplora soluzioni e premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-393">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="34c2c-394">Attività 4: verifica</span><span class="sxs-lookup"><span data-stu-id="34c2c-394">Task 4: Verification</span></span>

<span data-ttu-id="34c2c-395">Per verificare di avere eseguito correttamente tutti i passaggi nell'esercizio precedente, procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="34c2c-395">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="34c2c-396">Con l'applicazione in un browser, si noti che:</span><span class="sxs-lookup"><span data-stu-id="34c2c-396">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="34c2c-397">Metodo di azione Index del HomeController trovare e visualizzare il **\Views\Home\Index.cshtml** visualizzare modello, anche se il codice chiamato **restituire View()**, poiché il modello di visualizzazione seguito il convenzione di denominazione standard.</span><span class="sxs-lookup"><span data-stu-id="34c2c-397">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="34c2c-398">Home Page Visualizza il messaggio di benvenuto definito all'interno di **\Views\Home\Index.cshtml** modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-398">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="34c2c-399">Utilizza la Home Page di  **\_cshtml** modello, quindi il messaggio di benvenuto è contenuto all'interno del layout standard sito HTML.</span><span class="sxs-lookup"><span data-stu-id="34c2c-399">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="34c2c-400">![Home visualizzazione dell'indice utilizzando il LayoutPage e lo stile definito](aspnet-mvc-4-fundamentals/_static/image16.png "Home visualizzazione dell'indice utilizzando il LayoutPage e lo stile definito")</span><span class="sxs-lookup"><span data-stu-id="34c2c-400">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="34c2c-401">*Visualizzazione dell'indice iniziale utilizzando il LayoutPage e lo stile definito*</span><span class="sxs-lookup"><span data-stu-id="34c2c-401">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="34c2c-402">Esercizio 5: Creazione di un modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-402">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="34c2c-403">Finora, apportate alle visualizzazioni visualizzare hardcoded HTML, ma, per creare applicazioni web dinamiche, il modello di visualizzazione deve ricevere informazioni dal Controller.</span><span class="sxs-lookup"><span data-stu-id="34c2c-403">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="34c2c-404">È una tecnica comune da utilizzare a tale scopo la **ViewModel** modello, che consente a un Controller di tutte le informazioni necessarie per generare la risposta HTML appropriata del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-404">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="34c2c-405">In questo esercizio, si apprenderà come creare una classe ViewModel e aggiungere le proprietà obbligatorie: il numero di generi un elenco di tali generi nell'archivio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-405">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="34c2c-406">Si verrà aggiornato anche il StoreController per utilizzare l'elemento creato ViewModel e, infine, si creerà un nuovo modello di visualizzazione che consente di visualizzare le proprietà menzionate nella pagina.</span><span class="sxs-lookup"><span data-stu-id="34c2c-406">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="34c2c-407">Attività 1 - Creazione di una classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="34c2c-407">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="34c2c-408">In questa attività si creerà una classe ViewModel che consente di implementare lo scenario di elenco genre archivio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-408">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="34c2c-409">Se non è già aperta, avviare **Visual Studio Express per Web**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-409">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="34c2c-410">Nel **File** menu, scegliere **Apri progetto**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-410">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="34c2c-411">Nella finestra di dialogo Apri progetto individuare **Source\Ex05 CreatingAViewModel\Begin**selezionare **Begin.sln** e fare clic su **aprire**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-411">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="34c2c-412">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-412">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="34c2c-413">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="34c2c-413">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="34c2c-414">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-414">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="34c2c-415">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="34c2c-415">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="34c2c-416">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-416">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="34c2c-417">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-417">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="34c2c-418">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-418">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="34c2c-419">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-419">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="34c2c-420">Creare un **ViewModel** cartella per contenere l'elemento ViewModel.</span><span class="sxs-lookup"><span data-stu-id="34c2c-420">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="34c2c-421">A tale scopo, fare doppio clic su di primo livello **MvcMusicStore** progetto, selezionare **Aggiungi** e quindi **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-421">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="34c2c-422">![Aggiunta di una nuova cartella](aspnet-mvc-4-fundamentals/_static/image17.png "aggiunta una nuova cartella")</span><span class="sxs-lookup"><span data-stu-id="34c2c-422">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="34c2c-423">*Aggiunta di una nuova cartella*</span><span class="sxs-lookup"><span data-stu-id="34c2c-423">*Adding a new folder*</span></span>
4. <span data-ttu-id="34c2c-424">Denominare la cartella *ViewModel*.</span><span class="sxs-lookup"><span data-stu-id="34c2c-424">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="34c2c-425">![Cartella ViewModel in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModel cartella in Esplora soluzioni")</span><span class="sxs-lookup"><span data-stu-id="34c2c-425">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="34c2c-426">*Cartella ViewModel in Esplora soluzioni*</span><span class="sxs-lookup"><span data-stu-id="34c2c-426">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="34c2c-427">Creare un **ViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-427">Create a **ViewModel** class.</span></span> <span data-ttu-id="34c2c-428">A tale scopo, fare clic su di **ViewModel** cartella creato di recente, selezionare **Aggiungi** e quindi **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-428">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="34c2c-429">In **codice**, scegliere il **classe** elemento e denominare il file *StoreIndexViewModel.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-429">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="34c2c-430">![Aggiunta di una nuova classe](aspnet-mvc-4-fundamentals/_static/image19.png "aggiunta una nuova classe")</span><span class="sxs-lookup"><span data-stu-id="34c2c-430">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="34c2c-431">*Aggiunta di una nuova classe*</span><span class="sxs-lookup"><span data-stu-id="34c2c-431">*Adding a new Class*</span></span>

    <span data-ttu-id="34c2c-432">![Creazione classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel creazione classe")</span><span class="sxs-lookup"><span data-stu-id="34c2c-432">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="34c2c-433">*Creazione classe StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="34c2c-433">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="34c2c-434">Attività 2: aggiunta di proprietà alla classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="34c2c-434">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="34c2c-435">Esistono due parametri deve essere passato dal StoreController il modello di visualizzazione per generare la risposta HTML previsto: il numero di generi un elenco di tali generi nell'archivio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-435">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="34c2c-436">In questa attività si aggiungeranno tali 2 proprietà per il **StoreIndexViewModel** classe: **NumberOfGenres** (integer) e **generi** (un elenco di stringhe).</span><span class="sxs-lookup"><span data-stu-id="34c2c-436">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="34c2c-437">Aggiungere **NumberOfGenres** e **generi** proprietà per il **StoreIndexViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-437">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="34c2c-438">A tale scopo, aggiungere le seguenti 2 righe alla definizione di classe:</span><span class="sxs-lookup"><span data-stu-id="34c2c-438">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="34c2c-439">(- Frammento di codice *ASP.NET MVC 4 nozioni di base - proprietà Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-439">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature. It provides the benefits of a property without requiring us to declare a backing field.
~~~

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="34c2c-440">Attività 3: aggiornamento StoreController per utilizzare il StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="34c2c-440">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="34c2c-441">Il **StoreIndexViewModel** classe incapsula le informazioni necessarie per passare da **StoreController**del **indice** a un modello di visualizzazione per generare una risposta (metodo) .</span><span class="sxs-lookup"><span data-stu-id="34c2c-441">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="34c2c-442">In questa attività si aggiornerà il **StoreController** per utilizzare il **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-442">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="34c2c-443">Aprire **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-443">Open **StoreController** class.</span></span>

    <span data-ttu-id="34c2c-444">![Apertura classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController apertura classe")</span><span class="sxs-lookup"><span data-stu-id="34c2c-444">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="34c2c-445">*Classe StoreController apertura*</span><span class="sxs-lookup"><span data-stu-id="34c2c-445">*Opening StoreController class*</span></span>
2. <span data-ttu-id="34c2c-446">Per utilizzare il **StoreIndexViewModel** classe il **StoreController**, aggiungere lo spazio dei nomi seguente all'inizio del **StoreController** codice:</span><span class="sxs-lookup"><span data-stu-id="34c2c-446">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="34c2c-447">(- Frammento di codice *ASP.NET MVC 4 nozioni di base - StoreIndexViewModel Ex5 utilizzando ViewModel*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-447">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
~~~
3. <span data-ttu-id="34c2c-448">Modifica il **StoreController**del **indice** il metodo di azione in modo che crea e popola un **StoreIndexViewModel** dell'oggetto e quindi li passa a un modello di visualizzazione per generare una risposta HTML con esso.</span><span class="sxs-lookup"><span data-stu-id="34c2c-448">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="34c2c-449">In Lab &quot;accesso ai dati e i modelli di MVC ASP.NET&quot; si scriverà il codice che recupera l'elenco di generi di archivio da un database.</span><span class="sxs-lookup"><span data-stu-id="34c2c-449">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="34c2c-450">Nel codice seguente, si creerà un **elenco** di generi dati fittizi in grado di popolare il **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-450">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="34c2c-451">Dopo la creazione e configurazione del **StoreIndexViewModel** dell'oggetto, verrà passato come argomento per il **vista** metodo.</span><span class="sxs-lookup"><span data-stu-id="34c2c-451">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="34c2c-452">Ciò indica che il modello di visualizzazione verrà utilizzata tale oggetto per generare una risposta HTML con esso.</span><span class="sxs-lookup"><span data-stu-id="34c2c-452">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="34c2c-453">Sostituire il **indice** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-453">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="34c2c-454">(- Frammento di codice *ASP.NET MVC 4 nozioni di base - metodo Ex5 StoreController indice*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-454">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound. That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**. Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.
~~~

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="34c2c-455">Attività 4: creazione di un modello di visualizzazione che utilizza StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="34c2c-455">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="34c2c-456">In questa attività si creerà un modello di visualizzazione che verrà utilizzato un oggetto StoreIndexViewModel passato dal Controller per visualizzare un elenco di generi.</span><span class="sxs-lookup"><span data-stu-id="34c2c-456">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="34c2c-457">Prima di creare il nuovo modello di visualizzazione, è possibile compilare il progetto in modo che il **finestra di dialogo Aggiungi visualizzazione** viene a conoscenza di **StoreIndexViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-457">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="34c2c-458">Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilare MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-458">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="34c2c-459">![Compilazione del progetto](aspnet-mvc-4-fundamentals/_static/image22.png "la compilazione del progetto")</span><span class="sxs-lookup"><span data-stu-id="34c2c-459">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="34c2c-460">*Compilazione del progetto*</span><span class="sxs-lookup"><span data-stu-id="34c2c-460">*Building the project*</span></span>
2. <span data-ttu-id="34c2c-461">Creare un nuovo modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-461">Create a new View template.</span></span> <span data-ttu-id="34c2c-462">A tale scopo, fare doppio clic all'interno di **indice** (metodo) e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-462">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="34c2c-463">![Aggiunta di una vista](aspnet-mvc-4-fundamentals/_static/image23.png "aggiunta di una vista")</span><span class="sxs-lookup"><span data-stu-id="34c2c-463">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="34c2c-464">*Aggiunta di una visualizzazione*</span><span class="sxs-lookup"><span data-stu-id="34c2c-464">*Adding a View*</span></span>
3. <span data-ttu-id="34c2c-465">Poiché il **finestra di dialogo Aggiungi visualizzazione** da cui è stato richiamato il **StoreController**, verranno aggiunti il modello di visualizzazione per impostazione predefinita in un **\Views\Store\Index.cshtml** file.</span><span class="sxs-lookup"><span data-stu-id="34c2c-465">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="34c2c-466">Controllare il **creare una fortemente tipizzato-visualizzazione** casella di controllo e quindi selezionare **StoreIndexViewModel** come il **classe modello**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-466">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="34c2c-467">Inoltre, assicurarsi che il motore di visualizzazione selezionato sia **Razor**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-467">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="34c2c-468">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-468">Click **Add**.</span></span>

    <span data-ttu-id="34c2c-469">![Visualizza finestra di dialogo Aggiungi](aspnet-mvc-4-fundamentals/_static/image24.png "Visualizza finestra di dialogo Aggiungi")</span><span class="sxs-lookup"><span data-stu-id="34c2c-469">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="34c2c-470">*Visualizza finestra di dialogo Aggiungi*</span><span class="sxs-lookup"><span data-stu-id="34c2c-470">*Add View Dialog*</span></span>

    <span data-ttu-id="34c2c-471">Il **\Views\Store\Index.cshtml** Visualizza file di modello viene creato e aperto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-471">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="34c2c-472">In base alle informazioni disponibili per il **Aggiungi visualizzazione** finestra di dialogo nell'ultimo passaggio, la vista modello si aspetta di ricevere un **StoreIndexViewModel** istanza dei dati da utilizzare per generare una risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="34c2c-472">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="34c2c-473">Si noterà che il modello eredita un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in c#.</span><span class="sxs-lookup"><span data-stu-id="34c2c-473">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="34c2c-474">Attività 5: aggiornamento dei modelli di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-474">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="34c2c-475">In questa attività, è possibile aggiornare il modello di visualizzazione creato nell'ultima attività per recuperare il numero di generi e i relativi nomi all'interno della pagina.</span><span class="sxs-lookup"><span data-stu-id="34c2c-475">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="34c2c-476">Si utilizzerà sintassi @ (noto anche come &quot;un elevato numero di codice&quot;) per eseguire codice all'interno del modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-476">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>


1. <span data-ttu-id="34c2c-477">Nel **cshtml** file, all'interno di **archivio** cartella, sostituire il codice con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="34c2c-477">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>


~~~
[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

> [!NOTE]
> As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
> 
> ![](aspnet-mvc-4-fundamentals/_static/image25.png)
> 
> *Getting Model properties and methods with Visual Studio's IntelliSense*
> 
> The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
> 
> You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
~~~
2. <span data-ttu-id="34c2c-478">Scorrere l'elenco in genere **StoreIndexViewModel** e creare un elemento HTML **&lt;ul&gt;** elenco tramite un **foreach** ciclo.</span><span class="sxs-lookup"><span data-stu-id="34c2c-478">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="34c2c-479">(C#)</span><span class="sxs-lookup"><span data-stu-id="34c2c-479">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="34c2c-480">Premere **F5** per eseguire l'applicazione e selezionare **e delle archiviazioni**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-480">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="34c2c-481">Verrà visualizzato l'elenco di generi passato il **StoreIndexViewModel** dall'oggetto di **StoreController** per il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-481">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="34c2c-482">![Vista che visualizza un elenco di generi](aspnet-mvc-4-fundamentals/_static/image26.png "vista che visualizza un elenco di generi")</span><span class="sxs-lookup"><span data-stu-id="34c2c-482">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="34c2c-483">*Visualizza un elenco di generi di visualizzazione*</span><span class="sxs-lookup"><span data-stu-id="34c2c-483">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="34c2c-484">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="34c2c-484">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="34c2c-485">Esercizio 6: Utilizzo di parametri nella visualizzazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-485">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="34c2c-486">Nell'esercizio 3 è stato descritto come passare i parametri per il Controller.</span><span class="sxs-lookup"><span data-stu-id="34c2c-486">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="34c2c-487">In questo esercizio, si apprenderà come usare questi parametri nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-487">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="34c2c-488">A tale scopo, verranno presentate prima alle classi di modello che consentiranno di gestire la logica di dati e di dominio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-488">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="34c2c-489">Inoltre, si apprenderà come creare i collegamenti a pagine all'interno dell'applicazione ASP.NET MVC senza doversi preoccupare di elementi come i percorsi URL codifica.</span><span class="sxs-lookup"><span data-stu-id="34c2c-489">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="34c2c-490">Attività 1: aggiunta di classi modello</span><span class="sxs-lookup"><span data-stu-id="34c2c-490">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="34c2c-491">A differenza di ViewModel, che vengono creati solo per passare le informazioni dal Controller alla visualizzazione, le classi del modello vengono compilate per contenere e gestire la logica di dati e di dominio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-491">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="34c2c-492">In questa attività si aggiungeranno due classi di modello per rappresentare questi concetti: **Genre** e **Album**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-492">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="34c2c-493">Se non è già aperta, avviare **Visual Studio Express per il Web**</span><span class="sxs-lookup"><span data-stu-id="34c2c-493">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="34c2c-494">Nel **File** menu, scegliere **Apri progetto**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-494">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="34c2c-495">Nella finestra di dialogo Apri progetto individuare **Source\Ex06 UsingParametersInView\Begin**selezionare **Begin.sln** e fare clic su **aprire**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-495">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="34c2c-496">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-496">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="34c2c-497">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="34c2c-497">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="34c2c-498">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-498">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="34c2c-499">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="34c2c-499">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="34c2c-500">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-500">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="34c2c-501">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-501">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="34c2c-502">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-502">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="34c2c-503">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-503">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="34c2c-504">Aggiungere un **Genre** classe del modello.</span><span class="sxs-lookup"><span data-stu-id="34c2c-504">Add a **Genre** Model class.</span></span> <span data-ttu-id="34c2c-505">A tale scopo, fare doppio clic sul **modelli** cartella il **Esplora**, selezionare **Aggiungi** e quindi il **nuovo elemento** opzione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-505">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="34c2c-506">In **codice**, scegliere il **classe** elemento e denominare il file *Genre.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-506">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="34c2c-507">![Aggiunta di una classe](aspnet-mvc-4-fundamentals/_static/image27.png "aggiunta di una classe")</span><span class="sxs-lookup"><span data-stu-id="34c2c-507">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="34c2c-508">*Aggiunta di un nuovo elemento*</span><span class="sxs-lookup"><span data-stu-id="34c2c-508">*Adding a new item*</span></span>

    <span data-ttu-id="34c2c-509">![Aggiungere la classe di modello Genre](aspnet-mvc-4-fundamentals/_static/image28.png "aggiungere classe di modello Genre")</span><span class="sxs-lookup"><span data-stu-id="34c2c-509">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="34c2c-510">*Aggiungere la classe di modello Genre*</span><span class="sxs-lookup"><span data-stu-id="34c2c-510">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="34c2c-511">Aggiungere un **nome** proprietà alla classe genere.</span><span class="sxs-lookup"><span data-stu-id="34c2c-511">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="34c2c-512">A tale scopo, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-512">To do this, add the following code:</span></span>

    <span data-ttu-id="34c2c-513">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Genre Ex6*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-513">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
~~~
5. <span data-ttu-id="34c2c-514">Seguire la stessa procedura come prima, aggiungere un **Album** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-514">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="34c2c-515">A tale scopo, fare doppio clic sul **modelli** cartella il **Esplora**, selezionare **Aggiungi** e quindi il **nuovo elemento** opzione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-515">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="34c2c-516">In **codice**, scegliere il **classe** elemento e denominare il file *Album.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-516">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="34c2c-517">Aggiungere due proprietà alla classe Album: **Genre** e **titolo**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-517">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="34c2c-518">A tale scopo, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-518">To do this, add the following code:</span></span>

    <span data-ttu-id="34c2c-519">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Album Ex6*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-519">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]
~~~

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="34c2c-520">Attività 2: aggiunta di un StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="34c2c-520">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="34c2c-521">Oggetto **StoreBrowseViewModel** verrà utilizzato in questa attività per visualizzare album che corrispondono a un genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="34c2c-521">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="34c2c-522">In questa attività verrà creare questa classe e quindi aggiungere due proprietà consentono di gestire il **Genre** e il relativo **Album**dell'elenco.</span><span class="sxs-lookup"><span data-stu-id="34c2c-522">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="34c2c-523">Aggiungere un **StoreBrowseViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-523">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="34c2c-524">A tale scopo, fare doppio clic sul **ViewModel** cartella la **Esplora**, selezionare **Aggiungi** e quindi il **nuovo elemento** opzione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-524">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="34c2c-525">In **codice**, scegliere il **classe** elemento e denominare il file *StoreBrowseViewModel.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-525">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="34c2c-526">Aggiungere un riferimento ai modelli in **StoreBrowseViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-526">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="34c2c-527">A tale scopo, aggiungere il seguente utilizzo dello spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="34c2c-527">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="34c2c-528">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-528">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
~~~
3. <span data-ttu-id="34c2c-529">Aggiungere le due proprietà **StoreBrowseViewModel** classe: **Genre** e **album**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-529">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="34c2c-530">A tale scopo, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-530">To do this, add the following code:</span></span>

    <span data-ttu-id="34c2c-531">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-531">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).
> 
> This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.
> 
> **List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace. One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.
~~~

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="34c2c-532">Attività 3: utilizzando il nuovo ViewModel di StoreController</span><span class="sxs-lookup"><span data-stu-id="34c2c-532">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="34c2c-533">In questa attività si modificherà il **StoreController**del **Sfoglia** e **dettagli** per utilizzare i nuovi metodi di azione **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="34c2c-533">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="34c2c-534">Aggiungere un riferimento alla cartella modelli in **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-534">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="34c2c-535">A tale scopo, espandere il **controller** cartella il **Esplora** e aprire il **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-535">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="34c2c-536">Quindi aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-536">Then add the following code:</span></span>

    <span data-ttu-id="34c2c-537">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-537">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
~~~
2. <span data-ttu-id="34c2c-538">Sostituire il **Sfoglia** il metodo di azione per utilizzare il **StoreViewBrowseController** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-538">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="34c2c-539">Si creerà un genere e due nuovi oggetti album con dati fittizi (nel laboratorio pratico successiva si utilizzerà dati reali da un database).</span><span class="sxs-lookup"><span data-stu-id="34c2c-539">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="34c2c-540">A tale scopo, sostituire il **Sfoglia** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-540">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="34c2c-541">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-541">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
~~~
3. <span data-ttu-id="34c2c-542">Sostituire il **dettagli** il metodo di azione per utilizzare il **StoreViewBrowseController** classe.</span><span class="sxs-lookup"><span data-stu-id="34c2c-542">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="34c2c-543">Verrà creato un nuovo **Album** oggetto da restituire per il **vista**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-543">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="34c2c-544">A tale scopo, sostituire il **dettagli** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-544">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="34c2c-545">(- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="34c2c-545">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]
~~~

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="34c2c-546">Attività 4: aggiunta di un modello di visualizzazione esplorazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-546">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="34c2c-547">In questa attività si aggiungerà un **Sfoglia** visualizzazione per mostrare gli album trovati per un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="34c2c-547">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="34c2c-548">Prima di creare il nuovo modello di visualizzazione, è necessario compilare il progetto in modo che il **Aggiungi visualizzazione** finestra di dialogo viene a conoscenza di **ViewModel** classe da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="34c2c-548">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="34c2c-549">Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilare MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-549">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="34c2c-550">Aggiungere un **Sfoglia** visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-550">Add a **Browse** View.</span></span> <span data-ttu-id="34c2c-551">A tale scopo, fare clic del mouse il **Sfoglia** il metodo di azione del **StoreController** e fare clic su **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-551">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="34c2c-552">Nel **Aggiungi visualizzazione** finestra di dialogo, verificare che il nome della visualizzazione è **Sfoglia**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-552">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="34c2c-553">Controllare il **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **StoreBrowseViewModel** dal **classe modello** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="34c2c-553">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="34c2c-554">Lasciare gli altri campi con il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="34c2c-554">Leave the other fields with their default value.</span></span> <span data-ttu-id="34c2c-555">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-555">Then click **Add**.</span></span>

    <span data-ttu-id="34c2c-556">![Aggiunta di una vista di esplorazione](aspnet-mvc-4-fundamentals/_static/image29.png "aggiunta di una vista di esplorazione")</span><span class="sxs-lookup"><span data-stu-id="34c2c-556">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="34c2c-557">*Aggiunta di una vista di esplorazione*</span><span class="sxs-lookup"><span data-stu-id="34c2c-557">*Adding a Browse View*</span></span>
4. <span data-ttu-id="34c2c-558">Modificare il **Browse.cshtml** per visualizzare le informazioni del genere, l'accesso di **StoreBrowseViewModel** oggetto passato per il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-558">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="34c2c-559">A tale scopo, sostituire il contenuto con quanto segue: (c#)</span><span class="sxs-lookup"><span data-stu-id="34c2c-559">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="34c2c-560">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-560">Task 5 - Running the Application</span></span>

<span data-ttu-id="34c2c-561">In questa attività, si verificherà che il **Sfoglia** che consente di recuperare gli album dal **Sfoglia** azione del metodo.</span><span class="sxs-lookup"><span data-stu-id="34c2c-561">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="34c2c-562">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-562">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="34c2c-563">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="34c2c-563">The project starts in the Home page.</span></span> <span data-ttu-id="34c2c-564">Modificare l'URL in   **/archiviare/Sfoglia? Genre = Disco** per verificare che l'azione restituisce due album.</span><span class="sxs-lookup"><span data-stu-id="34c2c-564">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="34c2c-565">![Esplorazione album Disco archivio](aspnet-mvc-4-fundamentals/_static/image30.png "esplorazione album Disco archivio")</span><span class="sxs-lookup"><span data-stu-id="34c2c-565">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="34c2c-566">*Esplorazione album Disco archivio*</span><span class="sxs-lookup"><span data-stu-id="34c2c-566">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="34c2c-567">Attività 6 - visualizzazione di informazioni su una specifica Album</span><span class="sxs-lookup"><span data-stu-id="34c2c-567">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="34c2c-568">In questa attività verrà implementata la **/dettagli archivio** visualizzazione per visualizzare informazioni su un album specifico.</span><span class="sxs-lookup"><span data-stu-id="34c2c-568">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="34c2c-569">In questo laboratorio pratico, tutti gli elementi verranno visualizzati su album già inclusa nel **vista** modello.</span><span class="sxs-lookup"><span data-stu-id="34c2c-569">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="34c2c-570">In questo caso, anziché creare un **StoreDetailsViewModel** (classe), si utilizzerà corrente **StoreBrowseViewModel** passando Album a tale modello.</span><span class="sxs-lookup"><span data-stu-id="34c2c-570">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="34c2c-571">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-571">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="34c2c-572">Aggiungere un nuovo **dettagli** visualizzare per il **StoreController**del **dettagli** metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-572">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="34c2c-573">A tale scopo, fare doppio clic su di **dettagli** metodo il **StoreController** classe e fare clic su **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-573">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="34c2c-574">Nel **Aggiungi visualizzazione** finestra di dialogo, verificare che il **nome vista** è **dettagli**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-574">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="34c2c-575">Controllare il **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **Album** dal **classe modello** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="34c2c-575">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="34c2c-576">Lasciare gli altri campi con il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="34c2c-576">Leave the other fields with their default value.</span></span> <span data-ttu-id="34c2c-577">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-577">Then click **Add**.</span></span> <span data-ttu-id="34c2c-578">Verrà creata e aprire un **\Views\Store\Details.cshtml** file.</span><span class="sxs-lookup"><span data-stu-id="34c2c-578">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="34c2c-579">![Aggiunta di una visualizzazione dettagli](aspnet-mvc-4-fundamentals/_static/image31.png "aggiunta di una visualizzazione dettagli")</span><span class="sxs-lookup"><span data-stu-id="34c2c-579">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="34c2c-580">*Aggiunta di una visualizzazione dettagli*</span><span class="sxs-lookup"><span data-stu-id="34c2c-580">*Adding a Details View*</span></span>
3. <span data-ttu-id="34c2c-581">Modificare il **Details.cshtml** file per visualizzare le informazioni dell'Album, l'accesso di **Album** oggetto passato per il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-581">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="34c2c-582">A tale scopo, sostituire il contenuto con quanto segue: (c#)</span><span class="sxs-lookup"><span data-stu-id="34c2c-582">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="34c2c-583">Attività 7: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-583">Task 7 - Running the Application</span></span>

<span data-ttu-id="34c2c-584">In questa attività, si verificherà che il **dettagli** vista recupera le informazioni di Album dal **dettagli azione** metodo.</span><span class="sxs-lookup"><span data-stu-id="34c2c-584">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="34c2c-585">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-585">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="34c2c-586">Avvia il progetto nel **Home** pagina.</span><span class="sxs-lookup"><span data-stu-id="34c2c-586">The project starts in the **Home** page.</span></span> <span data-ttu-id="34c2c-587">Modificare l'URL in **/Store/Details/5** per verificare le informazioni dell'album.</span><span class="sxs-lookup"><span data-stu-id="34c2c-587">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="34c2c-588">![Visualizzazione Dettagli album](aspnet-mvc-4-fundamentals/_static/image32.png "esplorazione album dettaglio")</span><span class="sxs-lookup"><span data-stu-id="34c2c-588">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="34c2c-589">*Esplorazione dettaglio dell'Album*</span><span class="sxs-lookup"><span data-stu-id="34c2c-589">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="34c2c-590">Attività 8: aggiunta di collegamenti tra le pagine</span><span class="sxs-lookup"><span data-stu-id="34c2c-590">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="34c2c-591">In questa attività si aggiungerà un collegamento nella visualizzazione archivio per disporre di un collegamento in nome di ogni genere appropriati **archivio/Sfoglia** URL.</span><span class="sxs-lookup"><span data-stu-id="34c2c-591">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="34c2c-592">In questo modo, quando si fa clic su un genere, ad esempio **Disco**, verrà visualizzata **archivio/Sfoglia? genre = Disco** URL.</span><span class="sxs-lookup"><span data-stu-id="34c2c-592">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="34c2c-593">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-593">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="34c2c-594">Aggiornamento di **indice** pagina per aggiungere un collegamento per il **Sfoglia** pagina.</span><span class="sxs-lookup"><span data-stu-id="34c2c-594">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="34c2c-595">A tale scopo, nel **Esplora** espandere la **viste** cartella, il **archivio** cartella e fare doppio clic il **cshtml** pagina.</span><span class="sxs-lookup"><span data-stu-id="34c2c-595">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="34c2c-596">Aggiungere un collegamento alla visualizzazione Sfoglia che indica il genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="34c2c-596">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="34c2c-597">A tale scopo, sostituire il codice evidenziato di seguito all'interno di **&lt;li&gt;** tag: (c#)</span><span class="sxs-lookup"><span data-stu-id="34c2c-597">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="34c2c-598">un altro approccio potrebbe essere il collegamento direttamente alla pagina, con un codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="34c2c-598">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="34c2c-599">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="34c2c-599">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="34c2c-600">Sebbene questo approccio funziona, dipende dalla stringa hardcoded.</span><span class="sxs-lookup"><span data-stu-id="34c2c-600">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="34c2c-601">Se in un secondo momento, si rinomina il Controller, sarà necessario modificare manualmente questa istruzione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-601">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="34c2c-602">Un'alternativa migliore consiste nell'usare un **HTML Helper** metodo.</span><span class="sxs-lookup"><span data-stu-id="34c2c-602">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="34c2c-603">ASP.NET MVC include un metodo HTML Helper che è disponibile per tali attività.</span><span class="sxs-lookup"><span data-stu-id="34c2c-603">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="34c2c-604">Il **Html.ActionLink()** metodo helper semplifica la compilazione HTML **&lt;un&gt;** collegamenti, assicurandosi che i percorsi URL siano URL correttamente codificati.</span><span class="sxs-lookup"><span data-stu-id="34c2c-604">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="34c2c-605">Htlm.ActionLink dispone di diversi overload.</span><span class="sxs-lookup"><span data-stu-id="34c2c-605">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="34c2c-606">In questo esercizio si utilizzerà uno che accetta tre parametri:</span><span class="sxs-lookup"><span data-stu-id="34c2c-606">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="34c2c-607">Testo del collegamento, che verrà visualizzato il nome di genere</span><span class="sxs-lookup"><span data-stu-id="34c2c-607">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="34c2c-608">Nome dell'azione controller (**Sfoglia**)</span><span class="sxs-lookup"><span data-stu-id="34c2c-608">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="34c2c-609">I valori di parametro, che specifica il nome di route (**Genre**) e il valore (**nome genere**)</span><span class="sxs-lookup"><span data-stu-id="34c2c-609">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="34c2c-610">Attività 9: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-610">Task 9 - Running the Application</span></span>

<span data-ttu-id="34c2c-611">In questa attività si eseguirà il test con un collegamento a appropriato, verrà visualizzata ogni genere **archivio/Sfoglia** URL.</span><span class="sxs-lookup"><span data-stu-id="34c2c-611">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="34c2c-612">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-612">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="34c2c-613">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="34c2c-613">The project starts in the Home page.</span></span> <span data-ttu-id="34c2c-614">Modificare l'URL in **e delle archiviazioni** per verificare che ogni genere collegamenti appropriati **archivio/Sfoglia** URL.</span><span class="sxs-lookup"><span data-stu-id="34c2c-614">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="34c2c-615">![Esplorazione generi con collegamenti a pagina Sfoglia](aspnet-mvc-4-fundamentals/_static/image33.png "generi esplorazione con collegamenti a pagina Sfoglia")</span><span class="sxs-lookup"><span data-stu-id="34c2c-615">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="34c2c-616">*Esplorazione generi con collegamenti a pagina Sfoglia*</span><span class="sxs-lookup"><span data-stu-id="34c2c-616">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="34c2c-617">Attività 10 - tramite Raccolta ViewModel dinamica per passare i valori</span><span class="sxs-lookup"><span data-stu-id="34c2c-617">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="34c2c-618">In questa attività si apprenderà un metodo semplice e potente per passare i valori tra il Controller e la visualizzazione senza apportare modifiche nel modello.</span><span class="sxs-lookup"><span data-stu-id="34c2c-618">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="34c2c-619">ASP.NET MVC 4 viene fornita la raccolta &quot;ViewModel&quot;, che può essere assegnato qualsiasi valore dinamico e si accede all'interno di controller e visualizzazioni nonché.</span><span class="sxs-lookup"><span data-stu-id="34c2c-619">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="34c2c-620">Verrà utilizzata la raccolta dinamica ViewBag per passare un elenco di &quot; **stella generi** &quot; dal controller alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-620">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="34c2c-621">La visualizzazione dell'indice di archivio avrà accesso a **ViewModel** e visualizzare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="34c2c-621">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="34c2c-622">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-622">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="34c2c-623">Aprire **StoreController.cs** e modificare **indice** metodo per creare un elenco di generi di stella in ViewModel raccolta:</span><span class="sxs-lookup"><span data-stu-id="34c2c-623">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

> [!NOTE]
> You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.
~~~
2. <span data-ttu-id="34c2c-624">L'icona a stella **&quot;starred.png&quot;** è incluso nel **Source\Assets\Images** cartella di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-624">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="34c2c-625">Per aggiungerlo all'applicazione, trascinare i propri contenuti da un **Esplora** finestra all'interno di **Esplora** in Visual Web Developer Express, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="34c2c-625">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="34c2c-626">![Immagine di stella di aggiunta alla soluzione](aspnet-mvc-4-fundamentals/_static/image34.png "immagine di stella di aggiunta alla soluzione")</span><span class="sxs-lookup"><span data-stu-id="34c2c-626">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="34c2c-627">*Immagine di stella aggiunta alla soluzione*</span><span class="sxs-lookup"><span data-stu-id="34c2c-627">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="34c2c-628">Aprire la visualizzazione **Store/Index.cshtml** e modificare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-628">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="34c2c-629">È di lettura di &quot;stella&quot; proprietà il **ViewBag** insieme per verificare se il nome del genere corrente è nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="34c2c-629">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="34c2c-630">In questo caso saranno visualizzati un'icona a stella destra al collegamento di genere.</span><span class="sxs-lookup"><span data-stu-id="34c2c-630">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="34c2c-631">(C#)</span><span class="sxs-lookup"><span data-stu-id="34c2c-631">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="34c2c-632">Attività 11 - esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-632">Task 11 - Running the Application</span></span>

<span data-ttu-id="34c2c-633">In questa attività si testerà che generi i contrassegnata da indica visualizzino un'icona a stella.</span><span class="sxs-lookup"><span data-stu-id="34c2c-633">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="34c2c-634">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-634">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="34c2c-635">Avvia il progetto nel **Home** pagina.</span><span class="sxs-lookup"><span data-stu-id="34c2c-635">The project starts in the **Home** page.</span></span> <span data-ttu-id="34c2c-636">Modificare l'URL in **e delle archiviazioni** per verificare che ogni genere in evidenza è l'etichetta respecting:</span><span class="sxs-lookup"><span data-stu-id="34c2c-636">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="34c2c-637">![Esplorazione generi con elementi contrassegnata da indica](aspnet-mvc-4-fundamentals/_static/image35.png "esplorazione generi con contrassegnata da indica elementi")</span><span class="sxs-lookup"><span data-stu-id="34c2c-637">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="34c2c-638">*Esplorazione generi con contrassegnata da indica elementi*</span><span class="sxs-lookup"><span data-stu-id="34c2c-638">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="34c2c-639">Esercizio 7: Un'introduzione ad ASP.NET MVC 4 nuovo modello</span><span class="sxs-lookup"><span data-stu-id="34c2c-639">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="34c2c-640">In questo esercizio verranno esplorati i miglioramenti nei modelli di progetto ASP.NET MVC 4, osservare le funzionalità più rilevanti del nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="34c2c-640">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="34c2c-641">Attività 1: Esplorare il modello di applicazione ASP.NET MVC 4 Internet</span><span class="sxs-lookup"><span data-stu-id="34c2c-641">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="34c2c-642">Se non è già aperta, avviare **Visual Studio Express per il Web**</span><span class="sxs-lookup"><span data-stu-id="34c2c-642">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="34c2c-643">Selezionare il **File | New | Progetto** comando di menu.</span><span class="sxs-lookup"><span data-stu-id="34c2c-643">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="34c2c-644">Nel **nuovo progetto** finestra di dialogo Seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere il **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-644">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="34c2c-645">**Nome** il progetto *MusicStore* e aggiornare il **Nome soluzione** a *iniziare*, quindi selezionare un percorso (o lasciare il valore predefinito) e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="34c2c-645">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="34c2c-646">![Creazione di un nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "la creazione di un nuovo progetto ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="34c2c-646">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="34c2c-647">*Creazione di un nuovo progetto ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="34c2c-647">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="34c2c-648">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo Seleziona il **applicazione Internet** modello di progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-648">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="34c2c-649">Si noti che come il motore di visualizzazione, è possibile selezionare Razor o ASPX.</span><span class="sxs-lookup"><span data-stu-id="34c2c-649">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="34c2c-650">![Crea una nuova applicazione Internet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "crea una nuova applicazione Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="34c2c-650">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="34c2c-651">*Crea una nuova applicazione Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="34c2c-651">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="34c2c-652">La sintassi Razor è stato introdotto in ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="34c2c-652">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="34c2c-653">L'obiettivo consiste nel ridurre al minimo il numero di caratteri e sequenze di tasti richieste in un file, consentendo un rapido e fluido codifica del flusso di lavoro.</span><span class="sxs-lookup"><span data-stu-id="34c2c-653">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="34c2c-654">Razor sfrutta esistente C# /VB (o altro) linguistiche e offre una sintassi di modello che consente un straordinario HTML costruzione del flusso di lavoro.</span><span class="sxs-lookup"><span data-stu-id="34c2c-654">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="34c2c-655">Premere **F5** per eseguire la soluzione e visualizzare il modello rinnovato.</span><span class="sxs-lookup"><span data-stu-id="34c2c-655">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="34c2c-656">È possibile controllare le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="34c2c-656">You can check out the following features:</span></span>

    1. <span data-ttu-id="34c2c-657">**Modelli di stile moderno**</span><span class="sxs-lookup"><span data-stu-id="34c2c-657">**Modern-style templates**</span></span>

        <span data-ttu-id="34c2c-658">I modelli sono stati rinnovati, fornendo più stili dall'aspetto moderno.</span><span class="sxs-lookup"><span data-stu-id="34c2c-658">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="34c2c-659">![Modelli ASP.NET MVC 4 modificato nello stile](aspnet-mvc-4-fundamentals/_static/image38.png "modificato nello stile modelli ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="34c2c-659">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="34c2c-660">*Modelli ASP.NET MVC 4 modificato nello stile*</span><span class="sxs-lookup"><span data-stu-id="34c2c-660">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="34c2c-661">**Rendering adattivo**</span><span class="sxs-lookup"><span data-stu-id="34c2c-661">**Adaptive Rendering**</span></span>

        <span data-ttu-id="34c2c-662">Estrarre il ridimensionamento della finestra del browser e notare come il layout di pagina si adatta dinamicamente per le nuove dimensioni della finestra.</span><span class="sxs-lookup"><span data-stu-id="34c2c-662">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="34c2c-663">Questi modelli utilizzano la tecnica di rendering adattivo per eseguire correttamente il rendering in piattaforme mobili e desktop senza personalizzazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-663">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="34c2c-664">![Modello di progetto ASP.NET MVC 4 in formati diversi browser](aspnet-mvc-4-fundamentals/_static/image39.png "il modello di progetto ASP.NET MVC 4 in formati diversi browser")</span><span class="sxs-lookup"><span data-stu-id="34c2c-664">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="34c2c-665">*Modello di progetto ASP.NET MVC 4 in formati diversi browser*</span><span class="sxs-lookup"><span data-stu-id="34c2c-665">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="34c2c-666">Chiudere il browser per arrestare il debugger e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-666">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="34c2c-667">Si è ora possibile esplorare la soluzione e verificare alcune delle nuove funzionalità introdotte da ASP.NET MVC 4 nel modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-667">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="34c2c-668">![ASP.NET MVC4-internet---modello di progetto applicazione](aspnet-mvc-4-fundamentals/_static/image40.png "il modello di progetto ASP.NET MVC 4 applicazione Internet")</span><span class="sxs-lookup"><span data-stu-id="34c2c-668">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="34c2c-669">*Il modello di progetto ASP.NET MVC 4 applicazione Internet*</span><span class="sxs-lookup"><span data-stu-id="34c2c-669">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="34c2c-670">**Tag HTML5**</span><span class="sxs-lookup"><span data-stu-id="34c2c-670">**HTML5 markup**</span></span>

       <span data-ttu-id="34c2c-671">Sfogliare le viste di modello per individuare il markup nuovo tema, ad esempio aprire **About.cshtml** visualizzare all'interno di **Home** cartella.</span><span class="sxs-lookup"><span data-stu-id="34c2c-671">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="34c2c-672">![Nuovo modello, utilizzando il markup Razor e HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nuovo modello, utilizzando il markup Razor e HTML5")</span><span class="sxs-lookup"><span data-stu-id="34c2c-672">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="34c2c-673">*Nuovo modello, utilizzando il markup Razor e HTML5*</span><span class="sxs-lookup"><span data-stu-id="34c2c-673">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="34c2c-674">**Librerie JavaScript inclusione**</span><span class="sxs-lookup"><span data-stu-id="34c2c-674">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="34c2c-675">**jQuery**: jQuery semplifica l'attraversamento documento HTML, la gestione degli eventi, l'animazione e interazioni Ajax.</span><span class="sxs-lookup"><span data-stu-id="34c2c-675">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="34c2c-676">**jQuery UI**: questa libreria fornisce astrazioni per applicazione widget basato sulla libreria JavaScript jQuery e animazione, effetti avanzate e interazione di basso livello.</span><span class="sxs-lookup"><span data-stu-id="34c2c-676">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="34c2c-677">È possibile acquisire informazioni jQuery e jQuery UI in [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="34c2c-677">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="34c2c-678">**Knockout.js**: modello predefinito di ASP.NET MVC 4 include ora **Knockout.js**, un framework JavaScript MVVM che consente di creare applicazioni web avanzate e ad alte scritte in JavaScript e HTML.</span><span class="sxs-lookup"><span data-stu-id="34c2c-678">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="34c2c-679">Ad esempio in ASP.NET MVC 3, jQuery e jQuery librerie di interfaccia utente sono incluse anche in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="34c2c-679">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="34c2c-680">È possibile ottenere ulteriori informazioni sulla libreria Knockout. js in questo collegamento: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="34c2c-680">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="34c2c-681">**Modernizr**: questa libreria viene eseguito automaticamente, rendendo il sito compatibile con browser meno recenti quando si utilizzano le tecnologie di HTML5 e CSS3.</span><span class="sxs-lookup"><span data-stu-id="34c2c-681">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="34c2c-682">È possibile ottenere ulteriori informazioni sulla libreria Modernizr in questo collegamento: [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="34c2c-682">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="34c2c-683">**SimpleMembership inclusi nella soluzione**</span><span class="sxs-lookup"><span data-stu-id="34c2c-683">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="34c2c-684">SimpleMembership è stato progettato come sostituzione per il sistema di provider di ruoli ASP.NET e appartenenza precedente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-684">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="34c2c-685">Include molte nuove funzionalità che rendono più semplice per lo sviluppatore a pagine web protette in modo più flessibile.</span><span class="sxs-lookup"><span data-stu-id="34c2c-685">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="34c2c-686">Il modello Internet già dispone di configurare alcuni aspetti da integrare SimpleMembership, ad esempio, il AccountController è pronto per l'utilizzo OAuthWebSecurity (per la registrazione dell'account OAuth, account di accesso, gestione e così via) e la sicurezza del Web.</span><span class="sxs-lookup"><span data-stu-id="34c2c-686">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="34c2c-687">![SimpleMembership inclusi nella soluzione](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership inclusi nella soluzione")</span><span class="sxs-lookup"><span data-stu-id="34c2c-687">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="34c2c-688">*SimpleMembership inclusi nella soluzione*</span><span class="sxs-lookup"><span data-stu-id="34c2c-688">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="34c2c-689">Ulteriori informazioni su [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span><span class="sxs-lookup"><span data-stu-id="34c2c-689">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="34c2c-690">Inoltre, è possibile distribuire l'applicazione per siti Web di Azure seguenti [pubblicazione appendice b: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="34c2c-690">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="34c2c-691">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="34c2c-691">Summary</span></span>

<span data-ttu-id="34c2c-692">Completando questa pratica si è appreso le nozioni di base di ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="34c2c-692">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="34c2c-693">Elementi di base di un'applicazione MVC e come interagiscono</span><span class="sxs-lookup"><span data-stu-id="34c2c-693">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="34c2c-694">Come creare un'applicazione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="34c2c-694">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="34c2c-695">Come aggiungere e configurare i controller per gestire i parametri passati tramite l'URL e la stringa di query</span><span class="sxs-lookup"><span data-stu-id="34c2c-695">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="34c2c-696">Come aggiungere una pagina master layout per la configurazione di un modello di contenuto HTML comune, un foglio di stile per migliorare l'aspetto e un modello di visualizzazione per visualizzare il contenuto HTML</span><span class="sxs-lookup"><span data-stu-id="34c2c-696">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="34c2c-697">Come utilizzare il modello ViewModel per passare le proprietà per il modello di visualizzazione per visualizzare informazioni dinamiche</span><span class="sxs-lookup"><span data-stu-id="34c2c-697">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="34c2c-698">Come utilizzare i parametri passati ai controller nel modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="34c2c-698">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="34c2c-699">Come aggiungere collegamenti a pagine all'interno dell'applicazione ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="34c2c-699">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="34c2c-700">Come aggiungere e utilizzare le proprietà dinamiche in una vista</span><span class="sxs-lookup"><span data-stu-id="34c2c-700">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="34c2c-701">I miglioramenti nei modelli di progetto ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="34c2c-701">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="34c2c-702">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="34c2c-702">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="34c2c-703">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-703">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="34c2c-704">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="34c2c-704">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="34c2c-705">Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="34c2c-705">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="34c2c-706">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="34c2c-706">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="34c2c-707">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-707">Click on **Install Now**.</span></span> <span data-ttu-id="34c2c-708">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="34c2c-708">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="34c2c-709">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-709">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="34c2c-710">![Installa Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="34c2c-710">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="34c2c-711">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="34c2c-711">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="34c2c-712">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="34c2c-712">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="34c2c-714">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="34c2c-714">*Accepting the license terms*</span></span>
5. <span data-ttu-id="34c2c-715">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-715">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="34c2c-717">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="34c2c-717">*Installation progress*</span></span>
6. <span data-ttu-id="34c2c-718">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-718">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="34c2c-720">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="34c2c-720">*Installation completed*</span></span>
7. <span data-ttu-id="34c2c-721">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="34c2c-721">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="34c2c-722">Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="34c2c-722">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express per il riquadro Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="34c2c-724">*VS Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="34c2c-724">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="34c2c-725">Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="34c2c-725">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="34c2c-726">Questa appendice mostra come creare un nuovo sito web dal portale di gestione di Windows Azure e pubblicare l'applicazione, ottenute seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="34c2c-726">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="34c2c-727">Attività 1 - Creazione di un nuovo sito Web da Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="34c2c-727">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="34c2c-728">Passare al [il portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-728">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="34c2c-729">Con Windows Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico.</span><span class="sxs-lookup"><span data-stu-id="34c2c-729">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="34c2c-730">È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="34c2c-730">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="34c2c-731">![Accedere al portale Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "accedere al portale Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="34c2c-731">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="34c2c-732">*Accedere al portale di gestione di Azure*</span><span class="sxs-lookup"><span data-stu-id="34c2c-732">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="34c2c-733">Fare clic su **nuovo** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="34c2c-733">Click **New** on the command bar.</span></span>

    <span data-ttu-id="34c2c-734">![Creazione di un nuovo sito Web](aspnet-mvc-4-fundamentals/_static/image49.png "creando un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="34c2c-734">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="34c2c-735">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="34c2c-735">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="34c2c-736">Fare clic su **calcolo** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-736">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="34c2c-737">Selezionare quindi **creazione rapida** opzione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-737">Then select **Quick Create** option.</span></span> <span data-ttu-id="34c2c-738">Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-738">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="34c2c-739">Un sito Web di Windows Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="34c2c-739">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="34c2c-740">L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure dall'esterno al portale.</span><span class="sxs-lookup"><span data-stu-id="34c2c-740">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="34c2c-741">Non include i passaggi per configurare un database.</span><span class="sxs-lookup"><span data-stu-id="34c2c-741">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="34c2c-742">![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-fundamentals/_static/image50.png "creando un nuovo sito Web utilizzando Creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="34c2c-742">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="34c2c-743">*Creazione di un nuovo sito Web utilizzando Creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="34c2c-743">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="34c2c-744">Attendere finché il nuovo **sito Web** viene creato.</span><span class="sxs-lookup"><span data-stu-id="34c2c-744">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="34c2c-745">Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna.</span><span class="sxs-lookup"><span data-stu-id="34c2c-745">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="34c2c-746">Verificare il funzionamento del nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="34c2c-746">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="34c2c-747">![Esplorazione per il nuovo sito web](aspnet-mvc-4-fundamentals/_static/image51.png "esplorazione per il nuovo sito web")</span><span class="sxs-lookup"><span data-stu-id="34c2c-747">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="34c2c-748">*Esplorazione per il nuovo sito web*</span><span class="sxs-lookup"><span data-stu-id="34c2c-748">*Browsing to the new web site*</span></span>

    <span data-ttu-id="34c2c-749">![Sito Web in esecuzione](aspnet-mvc-4-fundamentals/_static/image52.png "sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="34c2c-749">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="34c2c-750">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="34c2c-750">*Web site running*</span></span>
6. <span data-ttu-id="34c2c-751">Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-751">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="34c2c-752">![Aprire le pagine di gestione del sito web](aspnet-mvc-4-fundamentals/_static/image53.png "apertura delle pagine di gestione del sito web")</span><span class="sxs-lookup"><span data-stu-id="34c2c-752">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="34c2c-753">*Aprire le pagine di gestione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="34c2c-753">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="34c2c-754">Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.</span><span class="sxs-lookup"><span data-stu-id="34c2c-754">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="34c2c-755">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="34c2c-755">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="34c2c-756">Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-756">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="34c2c-757">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web per siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="34c2c-757">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="34c2c-758">![Profilo di pubblicazione del sito web di download](aspnet-mvc-4-fundamentals/_static/image54.png "profilo di pubblicazione del sito web di download")</span><span class="sxs-lookup"><span data-stu-id="34c2c-758">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="34c2c-759">*Profilo di pubblicazione del sito Web di download*</span><span class="sxs-lookup"><span data-stu-id="34c2c-759">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="34c2c-760">Scaricare il file del profilo di pubblicazione in un percorso noto.</span><span class="sxs-lookup"><span data-stu-id="34c2c-760">Download the publish profile file to a known location.</span></span> <span data-ttu-id="34c2c-761">In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34c2c-761">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="34c2c-762">![Salvare il file del profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image55.png "il salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="34c2c-762">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="34c2c-763">*Salvare il file del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="34c2c-763">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="34c2c-764">Attività 2: configurare il Server di Database</span><span class="sxs-lookup"><span data-stu-id="34c2c-764">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="34c2c-765">Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL.</span><span class="sxs-lookup"><span data-stu-id="34c2c-765">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="34c2c-766">Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="34c2c-766">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="34c2c-767">È necessario un server di Database SQL per archiviare il database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="34c2c-767">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="34c2c-768">È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-768">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="34c2c-769">Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="34c2c-769">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="34c2c-770">Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive.</span><span class="sxs-lookup"><span data-stu-id="34c2c-770">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="34c2c-771">Non creare il database, come verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="34c2c-771">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="34c2c-772">![Dashboard del Server Database SQL](aspnet-mvc-4-fundamentals/_static/image56.png "Dashboard del Server Database SQL")</span><span class="sxs-lookup"><span data-stu-id="34c2c-772">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="34c2c-773">*Dashboard del Server Database SQL*</span><span class="sxs-lookup"><span data-stu-id="34c2c-773">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="34c2c-774">Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-774">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="34c2c-775">A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) pulsante.</span><span class="sxs-lookup"><span data-stu-id="34c2c-775">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="34c2c-777">*Aggiunta indirizzo IP del Client*</span><span class="sxs-lookup"><span data-stu-id="34c2c-777">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="34c2c-778">Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="34c2c-778">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confermare le modifiche](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="34c2c-780">*Confermare le modifiche*</span><span class="sxs-lookup"><span data-stu-id="34c2c-780">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="34c2c-781">Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="34c2c-781">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="34c2c-782">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="34c2c-782">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="34c2c-783">Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-783">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="34c2c-784">![La pubblicazione dell'applicazione](aspnet-mvc-4-fundamentals/_static/image60.png "la pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="34c2c-784">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="34c2c-785">*Pubblicazione del sito web*</span><span class="sxs-lookup"><span data-stu-id="34c2c-785">*Publishing the web site*</span></span>
2. <span data-ttu-id="34c2c-786">Importare il profilo di pubblicazione che è stato salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="34c2c-786">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="34c2c-787">![L'importazione del profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image61.png "l'importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="34c2c-787">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="34c2c-788">*L'importazione di profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="34c2c-788">*Importing publish profile*</span></span>
3. <span data-ttu-id="34c2c-789">Fare clic su **convalidare la connessione**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-789">Click **Validate Connection**.</span></span> <span data-ttu-id="34c2c-790">Una volta completata la convalida, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-790">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="34c2c-791">Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.</span><span class="sxs-lookup"><span data-stu-id="34c2c-791">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="34c2c-792">![Convalida della connessione](aspnet-mvc-4-fundamentals/_static/image62.png "convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="34c2c-792">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="34c2c-793">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="34c2c-793">*Validating connection*</span></span>
4. <span data-ttu-id="34c2c-794">Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="34c2c-794">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="34c2c-795">![Configurazione della distribuzione Web](aspnet-mvc-4-fundamentals/_static/image63.png "configurazione della distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="34c2c-795">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="34c2c-796">*Configurazione della distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="34c2c-796">*Web deploy configuration*</span></span>
5. <span data-ttu-id="34c2c-797">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="34c2c-797">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="34c2c-798">Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.</span><span class="sxs-lookup"><span data-stu-id="34c2c-798">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="34c2c-799">In **nome utente** digitare il nome di accesso di amministratore di server.</span><span class="sxs-lookup"><span data-stu-id="34c2c-799">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="34c2c-800">In **Password** digitare la password dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="34c2c-800">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="34c2c-801">Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="34c2c-801">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="34c2c-802">![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-fundamentals/_static/image64.png "configurazione stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="34c2c-802">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="34c2c-803">*Configurazione di stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="34c2c-803">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="34c2c-804">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-804">Then click **OK**.</span></span> <span data-ttu-id="34c2c-805">Quando viene richiesto di creare il database fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-805">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="34c2c-806">![Creazione del database](aspnet-mvc-4-fundamentals/_static/image65.png "creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="34c2c-806">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="34c2c-807">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="34c2c-807">*Creating the database*</span></span>
7. <span data-ttu-id="34c2c-808">La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito.</span><span class="sxs-lookup"><span data-stu-id="34c2c-808">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="34c2c-809">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-809">Then click **Next**.</span></span>

    <span data-ttu-id="34c2c-810">![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-fundamentals/_static/image66.png "stringa di connessione che punta al Database SQL")</span><span class="sxs-lookup"><span data-stu-id="34c2c-810">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="34c2c-811">*Stringa di connessione che punta al Database SQL*</span><span class="sxs-lookup"><span data-stu-id="34c2c-811">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="34c2c-812">Nel **anteprima** pagina, fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-812">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="34c2c-813">![Pubblicare l'applicazione web](aspnet-mvc-4-fundamentals/_static/image67.png "pubblicare l'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="34c2c-813">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="34c2c-814">*Pubblicare l'applicazione web*</span><span class="sxs-lookup"><span data-stu-id="34c2c-814">*Publishing the web application*</span></span>
9. <span data-ttu-id="34c2c-815">Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="34c2c-815">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="34c2c-816">![Applicazione pubblicata in Azure](aspnet-mvc-4-fundamentals/_static/image68.png "applicazione pubblicata in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="34c2c-816">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="34c2c-817">*Applicazione pubblicata in Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="34c2c-817">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="34c2c-818">Appendice c: utilizzo dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="34c2c-818">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="34c2c-819">Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="34c2c-819">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="34c2c-820">Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="34c2c-820">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="34c2c-821">![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-fundamentals/_static/image69.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="34c2c-821">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="34c2c-822">*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="34c2c-822">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="34c2c-823">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="34c2c-823">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="34c2c-824">Posizionare il cursore dove si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="34c2c-824">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="34c2c-825">Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).</span><span class="sxs-lookup"><span data-stu-id="34c2c-825">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="34c2c-826">Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="34c2c-826">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="34c2c-827">Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="34c2c-827">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="34c2c-828">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="34c2c-828">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="34c2c-829">![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-fundamentals/_static/image70.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="34c2c-829">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="34c2c-830">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="34c2c-830">*Start typing the snippet name*</span></span>

<span data-ttu-id="34c2c-831">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-fundamentals/_static/image71.png "premere Tab per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="34c2c-831">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="34c2c-832">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="34c2c-832">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="34c2c-833">![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-fundamentals/_static/image72.png "espanderà premere Tab nuovamente e il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="34c2c-833">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="34c2c-834">*Premere Tab nuovamente e il frammento di codice verranno espansi*</span><span class="sxs-lookup"><span data-stu-id="34c2c-834">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="34c2c-835">***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="34c2c-835">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="34c2c-836">Fare clic in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="34c2c-836">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="34c2c-837">Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="34c2c-837">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="34c2c-838">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="34c2c-838">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="34c2c-839">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-fundamentals/_static/image73.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="34c2c-839">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="34c2c-840">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="34c2c-840">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="34c2c-841">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-fundamentals/_static/image74.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="34c2c-841">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="34c2c-842">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="34c2c-842">*Pick the relevant snippet from the list, by clicking on it*</span></span>
