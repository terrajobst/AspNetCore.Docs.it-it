---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Nozioni fondamentali su ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Questa pratica è basata su MVC (Model View Controller) Music Store, un'applicazione di esercitazione che presenta e illustra come usare ASP.NET MV dettagliate...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: d8e837a5d56871d271590859c2e82336111cc87a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825893"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="f8692-103">Nozioni fondamentali su ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="f8692-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="f8692-104">da [Camp Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="f8692-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="f8692-105">Download Web Camp Kit di formazione</span><span class="sxs-lookup"><span data-stu-id="f8692-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="f8692-106">Questa pratica si basa su MVC (Model View Controller) Music Store, un'applicazione di esercitazione che introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8692-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="f8692-107">Nell'intero laboratorio si apprenderà la semplicità, ancora alimentazione dell'utilizzo congiunto di queste tecnologie.</span><span class="sxs-lookup"><span data-stu-id="f8692-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="f8692-108">Si inizierà con una semplice applicazione e si farà finché non possiedi un completamente funzionale applicazione Web ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f8692-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="f8692-109">Questo Lab è compatibile con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f8692-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="f8692-110">Se si vuole esplorare la versione di ASP.NET MVC 3 dell'applicazione dell'esercitazione, è possibile trovarlo nel [MVC-musica-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="f8692-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="f8692-111">In questo laboratorio pratico presuppone che lo sviluppatore abbia esperienza nelle tecnologie di sviluppo Web, ad esempio HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f8692-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="f8692-112">Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="f8692-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="f8692-113">Il progetto specifico per questo lab è disponibile all'indirizzo [nozioni di base di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="f8692-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="f8692-114">L'applicazione Music Store</span><span class="sxs-lookup"><span data-stu-id="f8692-114">The Music Store application</span></span>

<span data-ttu-id="f8692-115">L'applicazione web di Music Store che sarà compilato in tutto questo Lab è costituito da tre parti principali: market, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="f8692-116">I visitatori sarà in grado di esplorare gli album in base al genere, aggiungere gli album al carrello acquisti, esaminare la selezione e infine procedono alla cassa per eseguire l'accesso e completare l'ordine.</span><span class="sxs-lookup"><span data-stu-id="f8692-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="f8692-117">Inoltre, gli amministratori di store saranno in grado di gestire gli album disponibili, nonché le relative proprietà principali.</span><span class="sxs-lookup"><span data-stu-id="f8692-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="f8692-118">![Le schermate di Music Store](aspnet-mvc-4-fundamentals/_static/image1.png "screening di Music Store")</span><span class="sxs-lookup"><span data-stu-id="f8692-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="f8692-119">*Schermate di Music Store*</span><span class="sxs-lookup"><span data-stu-id="f8692-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="f8692-120">Essentials di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="f8692-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="f8692-121">Applicazione Music Store verrà compilata usando **Model View Controller (MVC)**, uno schema architetturale che separa un'applicazione in tre componenti principali:</span><span class="sxs-lookup"><span data-stu-id="f8692-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="f8692-122">**I modelli**: gli oggetti modello sono le parti dell'applicazione che implementano la logica di dominio.</span><span class="sxs-lookup"><span data-stu-id="f8692-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="f8692-123">Spesso, anche gli oggetti modello recupero e archiviano lo stato del modello in un database.</span><span class="sxs-lookup"><span data-stu-id="f8692-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="f8692-124">**Visualizzazioni:** viste sono elencati i componenti che consentono di visualizzare l'interfaccia utente (UI).</span><span class="sxs-lookup"><span data-stu-id="f8692-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="f8692-125">In genere, questa interfaccia utente viene creata da dati del modello.</span><span class="sxs-lookup"><span data-stu-id="f8692-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="f8692-126">Un esempio sarebbe visualizzazione di modifica che consente di visualizzare le caselle di testo e un elenco di riepilogo a discesa basato sullo stato corrente di un oggetto di Album di album.</span><span class="sxs-lookup"><span data-stu-id="f8692-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="f8692-127">**I controller:** controller sono i componenti che gestiscono l'interazione dell'utente, modificano il modello e selezionano infine una visualizzazione per eseguire il rendering dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="f8692-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="f8692-128">In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="f8692-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="f8692-129">Il pattern MVC consente di creare applicazioni che separano i diversi aspetti dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), fornendo un accoppiamento libero tra questi elementi.</span><span class="sxs-lookup"><span data-stu-id="f8692-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="f8692-130">Questa separazione consente di gestire la complessità quando si compila un'applicazione, in quanto consente di concentrarsi su un aspetto dell'implementazione alla volta.</span><span class="sxs-lookup"><span data-stu-id="f8692-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="f8692-131">Inoltre, il modello MVC rende semplice testare le applicazioni, incoraggiando anche l'utilizzo dello sviluppo basato su test (TDD) per la creazione di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="f8692-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="f8692-132">Il **ASP.NET MVC** framework offre un'alternativa al modello Web Form ASP.NET per la creazione di applicazioni Web basate su ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f8692-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="f8692-133">Il **ASP.NET MVC** è un framework di presentazione leggero e facile da testare che (come con le applicazioni basate su form Web) integrato con funzionalità ASP.NET esistenti, ad esempio le pagine master e basata sull'appartenenza autenticazione in modo che si ottiene tutta la potenza di .NET core.</span><span class="sxs-lookup"><span data-stu-id="f8692-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="f8692-134">Ciò è utile se si ha già familiarità con Web Form ASP.NET perché tutte le librerie già in uso sono anche disponibili in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f8692-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="f8692-135">Inoltre, l'accoppiamento libero tra i tre componenti principali di un'applicazione MVC favorisce anche lo sviluppo parallelo.</span><span class="sxs-lookup"><span data-stu-id="f8692-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="f8692-136">Ad esempio, uno sviluppatore può lavorare sulla visualizzazione, uno sviluppatore secondo gli strumenti per la logica del controller e un terzo può concentrarsi sulla logica di business nel modello.</span><span class="sxs-lookup"><span data-stu-id="f8692-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="f8692-137">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="f8692-137">Objectives</span></span>

<span data-ttu-id="f8692-138">In questo laboratorio pratico, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="f8692-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="f8692-139">Creare un'applicazione ASP.NET MVC da zero basata su Esercitazione l'applicazione Music Store</span><span class="sxs-lookup"><span data-stu-id="f8692-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="f8692-140">Aggiungere controller per gestire gli URL per la Home page del sito e per esplorare le funzionalità principali</span><span class="sxs-lookup"><span data-stu-id="f8692-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="f8692-141">Aggiungere una visualizzazione per personalizzare il contenuto visualizzato insieme ai relativi stile</span><span class="sxs-lookup"><span data-stu-id="f8692-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="f8692-142">Aggiungere le classi di modello per contenere e la gestione dei dati e dominio per la logica</span><span class="sxs-lookup"><span data-stu-id="f8692-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="f8692-143">Utilizzare il modello di vista per passare informazioni dalle azioni del controller per i modelli di vista</span><span class="sxs-lookup"><span data-stu-id="f8692-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="f8692-144">Esplorare il nuovo modello di ASP.NET MVC 4 per applicazioni internet</span><span class="sxs-lookup"><span data-stu-id="f8692-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="f8692-145">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f8692-145">Prerequisites</span></span>

<span data-ttu-id="f8692-146">Sono necessari gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="f8692-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="f8692-147">[Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leggere [appendice A](#AppendixA) per istruzioni su come installarla)</span><span class="sxs-lookup"><span data-stu-id="f8692-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="f8692-148">Configurazione</span><span class="sxs-lookup"><span data-stu-id="f8692-148">Setup</span></span>

<span data-ttu-id="f8692-149">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="f8692-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="f8692-150">Per praticità, gran parte del codice che vengono gestiti insieme questo lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8692-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="f8692-151">Per installare i frammenti di codice eseguiti **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="f8692-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="f8692-152">Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: Using Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="f8692-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="f8692-153">Esercizi</span><span class="sxs-lookup"><span data-stu-id="f8692-153">Exercises</span></span>

<span data-ttu-id="f8692-154">In questo laboratorio pratico include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f8692-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="f8692-155">Esercizio 1: Creazione progetto di applicazione Web ASP.NET MVC MusicStore</span><span class="sxs-lookup"><span data-stu-id="f8692-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="f8692-156">Esercizio 2: Creazione di un Controller</span><span class="sxs-lookup"><span data-stu-id="f8692-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="f8692-157">Esercizio 3: Il passaggio di parametri a un Controller</span><span class="sxs-lookup"><span data-stu-id="f8692-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="f8692-158">Esercizio 4: Creazione di una vista</span><span class="sxs-lookup"><span data-stu-id="f8692-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="f8692-159">Esercizio 5: Creazione di un modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f8692-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="f8692-160">Esercizio 6: Utilizzo di parametri nella visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f8692-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="f8692-161">Esercizio 7: Una visione generale di nuovo modello di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="f8692-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="f8692-162">Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="f8692-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="f8692-163">Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="f8692-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="f8692-164">Tempo stimato per completare questa esercitazione: **60 minuti**.</span><span class="sxs-lookup"><span data-stu-id="f8692-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="f8692-165">Esercizio 1: Creazione progetto di applicazione Web ASP.NET MVC MusicStore</span><span class="sxs-lookup"><span data-stu-id="f8692-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="f8692-166">In questo esercizio, si apprenderà come creare un'applicazione ASP.NET MVC in Visual Studio Express 2012 per Web nonché relativa organizzazione della cartella principale.</span><span class="sxs-lookup"><span data-stu-id="f8692-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="f8692-167">Inoltre, si apprenderà come aggiungere un nuovo Controller e renderla di visualizzare una semplice stringa nella home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="f8692-168">Attività 1: creazione del progetto di applicazione Web ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f8692-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="f8692-169">In questa attività si creerà un progetto di applicazione MVC ASP.NET vuoto usando il modello MVC Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8692-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="f8692-170">Avviare **Visual Studio Express per Web**.</span><span class="sxs-lookup"><span data-stu-id="f8692-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="f8692-171">Scegliere **Nuovo progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="f8692-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="f8692-172">Nel **nuovo progetto** finestra di dialogo per selezionare il **applicazione Web ASP.NET MVC 4** tipo, che si trova nel progetto **Visual c#** **Web** modello elenco.</span><span class="sxs-lookup"><span data-stu-id="f8692-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="f8692-173">Modifica il **Name** al *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="f8692-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="f8692-174">Impostare il percorso della soluzione all'interno di una nuova **Begin** nella cartella di origine di questo esercizio, ad esempio **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-PASQUA-PATH]**.</span><span class="sxs-lookup"><span data-stu-id="f8692-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="f8692-175">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8692-175">Click **OK**.</span></span>

    <span data-ttu-id="f8692-176">![Creare una finestra di dialogo Nuovo progetto](aspnet-mvc-4-fundamentals/_static/image2.png "Crea finestra di dialogo Nuovo progetto")</span><span class="sxs-lookup"><span data-stu-id="f8692-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="f8692-177">*Creare una finestra di dialogo Nuovo progetto*</span><span class="sxs-lookup"><span data-stu-id="f8692-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="f8692-178">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo per selezionare la **base** modello e assicurarsi che il **motore di visualizzazione** selezionata sia **Razor**.</span><span class="sxs-lookup"><span data-stu-id="f8692-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="f8692-179">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8692-179">Click **OK**.</span></span>

    <span data-ttu-id="f8692-180">![Finestra di dialogo Nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "finestra di dialogo Nuovo progetto ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="f8692-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="f8692-181">*Finestra di dialogo Nuovo progetto ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="f8692-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="f8692-182">Attività 2: esplorare la struttura della soluzione</span><span class="sxs-lookup"><span data-stu-id="f8692-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="f8692-183">Il framework ASP.NET MVC include un modello di progetto di Visual Studio che consente di creare applicazioni Web che supportano il modello MVC.</span><span class="sxs-lookup"><span data-stu-id="f8692-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="f8692-184">Questo modello crea una nuova applicazione Web MVC ASP.NET con le cartelle necessarie, modelli di elementi e le voci di file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="f8692-185">In questa attività, si esaminerà la struttura della soluzione per comprendere gli elementi che sono coinvolti e le relative relazioni.</span><span class="sxs-lookup"><span data-stu-id="f8692-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="f8692-186">Le cartelle seguenti sono inclusi in tutto l'applicazione ASP.NET MVC perché il framework ASP.NET MVC per impostazione predefinita Usa una &quot;convenzione rispetto alle configurazioni&quot; approccio e la imposta alcuni presupposti predefiniti in base alla denominazione di cartella convenzioni.</span><span class="sxs-lookup"><span data-stu-id="f8692-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="f8692-187">Una volta creato il progetto, esaminare la struttura di cartelle che è stata creata in Esplora soluzioni sul lato destro:</span><span class="sxs-lookup"><span data-stu-id="f8692-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="f8692-188">![Struttura di cartelle MVC ASP.NET in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image4.png "struttura di cartelle MVC ASP.NET in Esplora soluzioni")</span><span class="sxs-lookup"><span data-stu-id="f8692-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="f8692-189">*Struttura di cartelle MVC ASP.NET in Esplora soluzioni*</span><span class="sxs-lookup"><span data-stu-id="f8692-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="f8692-190">**I controller**.</span><span class="sxs-lookup"><span data-stu-id="f8692-190">**Controllers**.</span></span> <span data-ttu-id="f8692-191">Questa cartella contiene le classi controller.</span><span class="sxs-lookup"><span data-stu-id="f8692-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="f8692-192">In un'applicazione MVC di base, i controller sono tenuti a gestisce l'interazione dell'utente finale, la modifica del modello e infine scegliendo una vista per eseguire il rendering dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="f8692-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="f8692-193">Il framework MVC richiede i nomi di tutti i controller terminino con &quot;Controller&quot;-ad esempio HomeController, LoginController o ProductController.</span><span class="sxs-lookup"><span data-stu-id="f8692-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="f8692-194">**I modelli**.</span><span class="sxs-lookup"><span data-stu-id="f8692-194">**Models**.</span></span> <span data-ttu-id="f8692-195">Questa cartella è disponibile per le classi che rappresentano il modello di applicazione per l'applicazione Web MVC.</span><span class="sxs-lookup"><span data-stu-id="f8692-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="f8692-196">Ciò in genere include codice che definisce gli oggetti e la logica per l'interazione con l'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="f8692-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="f8692-197">In genere, gli oggetti modello effettivo sarà nelle librerie di classi separata.</span><span class="sxs-lookup"><span data-stu-id="f8692-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="f8692-198">Tuttavia, quando si crea una nuova applicazione, si includono le classi e quindi spostarle in librerie di classi separate in un secondo momento nel ciclo di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f8692-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="f8692-199">**Viste**.</span><span class="sxs-lookup"><span data-stu-id="f8692-199">**Views**.</span></span> <span data-ttu-id="f8692-200">Questa cartella è il percorso consigliato per viste, i componenti responsabile della visualizzazione dell'interfaccia utente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="f8692-201">Le visualizzazioni utilizzano i file con estensione aspx, ascx, cshtml e. master, oltre a eventuali altri file correlati al rendering delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="f8692-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="f8692-202">Cartella Views contiene una cartella per ogni controller. la cartella è denominata con il prefisso nome del controller.</span><span class="sxs-lookup"><span data-stu-id="f8692-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="f8692-203">Ad esempio, se si dispone di un controller denominato **HomeController**, la cartella Views conterrà una cartella Home.</span><span class="sxs-lookup"><span data-stu-id="f8692-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="f8692-204">Per impostazione predefinita, quando il framework di MVC ASP.NET carica una visualizzazione, Cerca un file con estensione aspx con il nome della visualizzazione richiesta nella cartella Views\controllerName (**viste [ControllerName] [Action]. aspx**) o (**viste [ControllerName] [Action]. cshtml**) per le visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="f8692-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f8692-205">Oltre alle cartelle elencate in precedenza, un'applicazione Web MVC utilizza il **Global. asax** per impostare il routing degli URL globale per impostazione predefinita e Usa le **Web. config** file per configurare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="f8692-206">Attività 3: aggiunta di una classe HomeController</span><span class="sxs-lookup"><span data-stu-id="f8692-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="f8692-207">Nelle applicazioni ASP.NET che non usano il framework di MVC l'interazione dell'utente è organizzata intorno alle pagine e intorno alla generazione e gestione di eventi da tali pagine.</span><span class="sxs-lookup"><span data-stu-id="f8692-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="f8692-208">Al contrario, l'interazione dell'utente con le applicazioni MVC ASP.NET è organizzata intorno a controller e i relativi metodi di azione.</span><span class="sxs-lookup"><span data-stu-id="f8692-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="f8692-209">D'altra parte, framework di MVC ASP.NET esegue il mapping degli URL alle classi che vengono definite i controller.</span><span class="sxs-lookup"><span data-stu-id="f8692-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="f8692-210">I controller di elaborare le richieste in ingresso, gestiscono input dell'utente e le interazioni, eseguire la logica dell'applicazione appropriati e determinare la risposta da inviare al client (visualizzazione del contenuto HTML, scaricare un file, il reindirizzamento a un URL diverso e così via).</span><span class="sxs-lookup"><span data-stu-id="f8692-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="f8692-211">Nel caso di visualizzazione HTML, una classe controller chiama in genere un componente di visualizzazione separato per generare il markup HTML per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="f8692-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="f8692-212">In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="f8692-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="f8692-213">In questa attività si aggiungerà una classe Controller che gestirà l'URL alla Home page del sito Music Store.</span><span class="sxs-lookup"><span data-stu-id="f8692-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="f8692-214">Fare doppio clic su **controller** cartella all'interno di Esplora soluzioni, scegliere **Add** e quindi **Controller** comando:</span><span class="sxs-lookup"><span data-stu-id="f8692-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="f8692-215">![Aggiungere un comando del Controller](aspnet-mvc-4-fundamentals/_static/image5.png "aggiungere un comando del Controller")</span><span class="sxs-lookup"><span data-stu-id="f8692-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="f8692-216">*Aggiungi comando Controller*</span><span class="sxs-lookup"><span data-stu-id="f8692-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="f8692-217">Il **Aggiungi Controller** viene visualizzata la finestra.</span><span class="sxs-lookup"><span data-stu-id="f8692-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="f8692-218">Denominare il controller *HomeController* , quindi premere **Add**.</span><span class="sxs-lookup"><span data-stu-id="f8692-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="f8692-219">![Aggiungi finestra di dialogo Controller](aspnet-mvc-4-fundamentals/_static/image6.png "Controller finestra di dialogo Aggiungi")</span><span class="sxs-lookup"><span data-stu-id="f8692-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="f8692-220">*Aggiungi finestra di dialogo Controller*</span><span class="sxs-lookup"><span data-stu-id="f8692-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="f8692-221">Il file **HomeController.cs** viene creato nel **controller** cartella.</span><span class="sxs-lookup"><span data-stu-id="f8692-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="f8692-222">Per avere la **HomeController** restituiscono una stringa nella relativa operazione sull'indice, sostituire il **indice** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="f8692-223">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - indice di HomeController Ex1*)</span><span class="sxs-lookup"><span data-stu-id="f8692-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="f8692-224">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f8692-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="f8692-225">In questa attività proverà orizzontalmente l'applicazione in un web browser.</span><span class="sxs-lookup"><span data-stu-id="f8692-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="f8692-226">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="f8692-227">La compilazione del progetto e avvia Server Web IIS locale.</span><span class="sxs-lookup"><span data-stu-id="f8692-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="f8692-228">Locale Server Web IIS verranno automaticamente aprire un web browser puntando all'URL del server Web.</span><span class="sxs-lookup"><span data-stu-id="f8692-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="f8692-229">![Applicazione in esecuzione in un web browser](aspnet-mvc-4-fundamentals/_static/image7.png "applicazione in esecuzione in un web browser")</span><span class="sxs-lookup"><span data-stu-id="f8692-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="f8692-230">*Applicazione in esecuzione in un web browser*</span><span class="sxs-lookup"><span data-stu-id="f8692-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8692-231">Il Server Web IIS locale verrà eseguito il sito Web in un numero di porta disponibile casuale.</span><span class="sxs-lookup"><span data-stu-id="f8692-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="f8692-232">Nella figura precedente, il sito viene eseguito in `http://localhost:50103/`, pertanto utilizza porta 50103.</span><span class="sxs-lookup"><span data-stu-id="f8692-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="f8692-233">Il numero di porta può variare.</span><span class="sxs-lookup"><span data-stu-id="f8692-233">Your port number may vary.</span></span>
2. <span data-ttu-id="f8692-234">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="f8692-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="f8692-235">Esercizio 2: Creazione di un Controller</span><span class="sxs-lookup"><span data-stu-id="f8692-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="f8692-236">In questo esercizio, si apprenderà come aggiornare il controller per implementare la funzionalità semplice dell'applicazione Music Store.</span><span class="sxs-lookup"><span data-stu-id="f8692-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="f8692-237">Tale controller definirà i metodi di azione per gestire ognuna delle richieste specifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="f8692-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="f8692-238">Una pagina di presentazione di generi musica in di Music Store</span><span class="sxs-lookup"><span data-stu-id="f8692-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="f8692-239">Una pagina di esplorazione che elenca tutti i album musicali per un particolare genere</span><span class="sxs-lookup"><span data-stu-id="f8692-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="f8692-240">Una pagina che contiene informazioni su un album musicali specifici</span><span class="sxs-lookup"><span data-stu-id="f8692-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="f8692-241">Per l'ambito di questo esercizio, tali azioni restituirà semplicemente una stringa a questo punto.</span><span class="sxs-lookup"><span data-stu-id="f8692-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="f8692-242">Attività 1: aggiunta di una nuova classe StoreController</span><span class="sxs-lookup"><span data-stu-id="f8692-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="f8692-243">In questa attività si aggiungerà un nuovo Controller.</span><span class="sxs-lookup"><span data-stu-id="f8692-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="f8692-244">Se non è già aperta, avviare **Visual Studio Express per Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="f8692-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="f8692-245">Nel **File** menu, scegliere **aprire il progetto**.</span><span class="sxs-lookup"><span data-stu-id="f8692-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="f8692-246">Nella finestra di dialogo Apri progetto individuare **Source\Ex02 CreatingAController\Begin**, selezionare **Begin.sln** e fare clic su **Open**.</span><span class="sxs-lookup"><span data-stu-id="f8692-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="f8692-247">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="f8692-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="f8692-248">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="f8692-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f8692-249">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f8692-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f8692-250">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="f8692-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f8692-251">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f8692-252">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8692-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f8692-253">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8692-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f8692-254">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f8692-255">Aggiungere il nuovo controller.</span><span class="sxs-lookup"><span data-stu-id="f8692-255">Add the new controller.</span></span> <span data-ttu-id="f8692-256">A tale scopo, fare doppio clic sui **controller** cartella all'interno di Esplora soluzioni, scegliere **Add** e quindi il **Controller** comando.</span><span class="sxs-lookup"><span data-stu-id="f8692-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="f8692-257">Modifica il **nome del Controller** a *StoreController*, fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="f8692-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="f8692-258">![Aggiungi finestra di dialogo Controller](aspnet-mvc-4-fundamentals/_static/image8.png "Controller finestra di dialogo Aggiungi")</span><span class="sxs-lookup"><span data-stu-id="f8692-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="f8692-259">*Aggiungi finestra di dialogo Controller*</span><span class="sxs-lookup"><span data-stu-id="f8692-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="f8692-260">Attività 2 - modificare le azioni del StoreController</span><span class="sxs-lookup"><span data-stu-id="f8692-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="f8692-261">In questa attività si modificherà i metodi del Controller che vengono chiamati **azioni**.</span><span class="sxs-lookup"><span data-stu-id="f8692-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="f8692-262">Le azioni sono responsabili per la gestione delle richieste di URL e determinare il contenuto che deve essere inviato nuovamente al browser o all'utente che ha richiamato l'URL.</span><span class="sxs-lookup"><span data-stu-id="f8692-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="f8692-263">Il **StoreController** classe esiste già un' **indice** (metodo).</span><span class="sxs-lookup"><span data-stu-id="f8692-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="f8692-264">Si utilizzerà più avanti in questo laboratorio per implementare la pagina che elenca tutti i generi dell'archivio di file musicali.</span><span class="sxs-lookup"><span data-stu-id="f8692-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="f8692-265">Per il momento, è sufficiente sostituire il **indice** con il codice seguente che restituisce una stringa &quot;Hello from Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="f8692-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="f8692-266">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - indice StoreController Ex2*)</span><span class="sxs-lookup"><span data-stu-id="f8692-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="f8692-267">Aggiungere **esplorare** e **dettagli** metodi.</span><span class="sxs-lookup"><span data-stu-id="f8692-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="f8692-268">A tale scopo, aggiungere il codice seguente per il **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="f8692-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="f8692-269">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="f8692-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="f8692-270">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f8692-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="f8692-271">In questa attività proverà orizzontalmente l'applicazione in un web browser.</span><span class="sxs-lookup"><span data-stu-id="f8692-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="f8692-272">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="f8692-273">Il progetto viene avviato nel **Home** pagina.</span><span class="sxs-lookup"><span data-stu-id="f8692-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="f8692-274">Modificare l'URL per verificare l'implementazione di ogni azione.</span><span class="sxs-lookup"><span data-stu-id="f8692-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="f8692-275">**/ Store**.</span><span class="sxs-lookup"><span data-stu-id="f8692-275">**/Store**.</span></span> <span data-ttu-id="f8692-276">Si noterà  **&quot;Hello from Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="f8692-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="f8692-277">**/ Store/esplorazione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-277">**/Store/Browse**.</span></span> <span data-ttu-id="f8692-278">Si noterà  **&quot;Hello from Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="f8692-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="f8692-279">**/ Store/Details**.</span><span class="sxs-lookup"><span data-stu-id="f8692-279">**/Store/Details**.</span></span> <span data-ttu-id="f8692-280">Si noterà  **&quot;Hello from Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="f8692-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="f8692-281">![Esplorazione StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "esplorazione StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="f8692-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="f8692-282">*Esplorazione /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="f8692-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="f8692-283">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="f8692-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="f8692-284">Esercizio 3: Il passaggio di parametri a un Controller</span><span class="sxs-lookup"><span data-stu-id="f8692-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="f8692-285">Fino ad ora, si hanno restituiva le stringhe costanti verso i controller.</span><span class="sxs-lookup"><span data-stu-id="f8692-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="f8692-286">In questa esercitazione si apprenderà come passare i parametri a un Controller usando l'URL e la stringa di query e apportando quindi le azioni di metodo rispondere con il testo nel browser.</span><span class="sxs-lookup"><span data-stu-id="f8692-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="f8692-287">Attività 1: aggiunta parametro Genre StoreController</span><span class="sxs-lookup"><span data-stu-id="f8692-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="f8692-288">In questa attività si userà il **querystring** inviare parametri per il **Sfoglia** metodo di azione il **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="f8692-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="f8692-289">Se non è già aperta, avviare **Visual Studio Express per Web**.</span><span class="sxs-lookup"><span data-stu-id="f8692-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="f8692-290">Nel **File** menu, scegliere **aprire il progetto**.</span><span class="sxs-lookup"><span data-stu-id="f8692-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="f8692-291">Nella finestra di dialogo Apri progetto individuare **Source\Ex03 PassingParametersToAController\Begin**, selezionare **Begin.sln** e fare clic su **Open**.</span><span class="sxs-lookup"><span data-stu-id="f8692-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="f8692-292">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="f8692-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="f8692-293">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="f8692-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f8692-294">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f8692-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f8692-295">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="f8692-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f8692-296">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f8692-297">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8692-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f8692-298">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8692-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f8692-299">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f8692-300">Aprire **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-300">Open **StoreController** class.</span></span> <span data-ttu-id="f8692-301">A questo scopo, nella **Esplora soluzioni**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="f8692-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="f8692-302">Modifica il **esplorare** metodo, aggiunta di un parametro di stringa per richiedere per un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="f8692-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="f8692-303">ASP.NET MVC verranno automaticamente passare qualsiasi stringa di query o parametri denominati invio del form **genre** a questo metodo di azione quando viene richiamato.</span><span class="sxs-lookup"><span data-stu-id="f8692-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="f8692-304">A tale scopo, sostituire il **esplorare** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="f8692-305">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="f8692-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="f8692-306">Si usa la **HttpUtility** metodo di utilità per impedisce agli utenti di inserimento di Javascript in vista con un collegamento, ad esempio   **/Store/Sfoglia? Genre =&lt;script&gt;Window. Location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="f8692-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="f8692-307">Per altre informazioni, visita [questo articolo di msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="f8692-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="f8692-308">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f8692-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="f8692-309">In questa attività, provare l'applicazione in un web browser e verrà utilizzato il **genre** parametro.</span><span class="sxs-lookup"><span data-stu-id="f8692-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="f8692-310">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="f8692-311">Il progetto viene avviato nel **Home** pagina.</span><span class="sxs-lookup"><span data-stu-id="f8692-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="f8692-312">Modificare l'URL in   */Store/Sfoglia? Genre = Disco* per verificare che l'azione riceve il parametro genre.</span><span class="sxs-lookup"><span data-stu-id="f8692-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="f8692-313">![Esplorazione StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "esplorazione StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="f8692-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="f8692-314">*Esplorazione /Store/Browse? Genre = Disco*</span><span class="sxs-lookup"><span data-stu-id="f8692-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="f8692-315">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="f8692-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="f8692-316">Attività 3: aggiunta di un parametro Id incorporato nell'URL</span><span class="sxs-lookup"><span data-stu-id="f8692-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="f8692-317">In questa attività si userà il **URL** per passare un **Id** parametro per il **dettagli** il metodo di azione del **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="f8692-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="f8692-318">Impostazione predefinita di ASP.NET MVC convenzione di routing consiste nel considerare il segmento di URL dopo il nome del metodo di azione come un parametro denominato **Id**. Se il metodo di azione presenta un parametro denominato Id, quindi ASP.NET MVC automaticamente passerà il segmento URL all'utente come parametro.</span><span class="sxs-lookup"><span data-stu-id="f8692-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="f8692-319">Nell'URL **Store/dettagli/5**, **Id** viene interpretato come **5**.</span><span class="sxs-lookup"><span data-stu-id="f8692-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="f8692-320">Modifica il **dettagli** metodo per il **StoreController**, l'aggiunta un' **int** parametro denominato **id**. A tale scopo, sostituire il **dettagli** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="f8692-321">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="f8692-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="f8692-322">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f8692-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="f8692-323">In questa attività, provare l'applicazione in un web browser e verrà utilizzato il **Id** parametro.</span><span class="sxs-lookup"><span data-stu-id="f8692-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="f8692-324">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="f8692-325">Il progetto viene avviato nel **Home** pagina.</span><span class="sxs-lookup"><span data-stu-id="f8692-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="f8692-326">Modificare l'URL */Store/Details/5* per verificare che l'azione riceve il parametro id.</span><span class="sxs-lookup"><span data-stu-id="f8692-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="f8692-327">![Esplorazione StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "esplorazione StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="f8692-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="f8692-328">*Esplorazione /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="f8692-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="f8692-329">Esercizio 4: Creazione di una vista</span><span class="sxs-lookup"><span data-stu-id="f8692-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="f8692-330">Finora si hanno restituiva le stringhe dalle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="f8692-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="f8692-331">Sebbene questo sia un modo utile per comprendere come funzionano i controller, non è modo in cui le applicazioni Web reali vengono compilate.</span><span class="sxs-lookup"><span data-stu-id="f8692-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="f8692-332">Le visualizzazioni sono componenti che forniscono un approccio migliore per la generazione di codice HTML al browser con l'uso di file di modello.</span><span class="sxs-lookup"><span data-stu-id="f8692-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="f8692-333">In questa esercitazione si apprenderà come aggiungere una pagina master layout per la configurazione di un modello per il contenuto HTML comune, un foglio di stile per migliorare l'aspetto del sito e, infine, un modello di vista per abilitare HomeController restituire HTML.</span><span class="sxs-lookup"><span data-stu-id="f8692-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="f8692-334">Attività 1: modifica del file \_layout. cshtml</span><span class="sxs-lookup"><span data-stu-id="f8692-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="f8692-335">Il file **~/Views/Shared/\_layout. cshtml** consente di configurare un modello per il codice HTML comune da usare tra l'intero sito Web.</span><span class="sxs-lookup"><span data-stu-id="f8692-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="f8692-336">In questa attività si aggiungerà una pagina master layout con un'intestazione comune con i collegamenti all'area di Store e la Home page.</span><span class="sxs-lookup"><span data-stu-id="f8692-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="f8692-337">Se non è già aperta, avviare **Visual Studio Express per Web**.</span><span class="sxs-lookup"><span data-stu-id="f8692-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="f8692-338">Nel **File** menu, scegliere **aprire il progetto**.</span><span class="sxs-lookup"><span data-stu-id="f8692-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="f8692-339">Nella finestra di dialogo Apri progetto individuare **Source\Ex04 CreatingAView\Begin**, selezionare **Begin.sln** e fare clic su **Open**.</span><span class="sxs-lookup"><span data-stu-id="f8692-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="f8692-340">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="f8692-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="f8692-341">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="f8692-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f8692-342">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f8692-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f8692-343">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="f8692-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f8692-344">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f8692-345">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8692-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f8692-346">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8692-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f8692-347">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f8692-348">Il file  <strong>\_layout. cshtml</strong> contiene il layout del contenitore HTML per tutte le pagine nel sito.</span><span class="sxs-lookup"><span data-stu-id="f8692-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="f8692-349">Include il <strong>&lt;html&gt;</strong> (elemento) per la risposta HTML, così come il <strong>&lt;head&gt;</strong> e <strong>&lt;corpo&gt;</strong> elementi.</span><span class="sxs-lookup"><span data-stu-id="f8692-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="f8692-350"><strong>@RenderBody()</strong> all'interno dell'HTML body identificare le aree di tale vista modelli saranno in grado di compilare con contenuto dinamico.</span><span class="sxs-lookup"><span data-stu-id="f8692-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="f8692-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="f8692-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="f8692-352">Aggiungere un'intestazione comune con i collegamenti nell'area di Store e la Home page in tutte le pagine nel sito.</span><span class="sxs-lookup"><span data-stu-id="f8692-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="f8692-353">Per farlo, aggiungere il codice seguente sotto &lt;corpo&gt; istruzione.</span><span class="sxs-lookup"><span data-stu-id="f8692-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="f8692-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="f8692-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="f8692-355">Includere un elemento div per eseguire il rendering di sezione del corpo di ogni pagina.</span><span class="sxs-lookup"><span data-stu-id="f8692-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="f8692-356">Sostituire  <strong>@RenderBody()</strong> con il seguente codice higlighted: (c#)</span><span class="sxs-lookup"><span data-stu-id="f8692-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="f8692-357">Lo sapevi?</span><span class="sxs-lookup"><span data-stu-id="f8692-357">Did you know?</span></span> <span data-ttu-id="f8692-358">Visual Studio 2012 include frammenti di codice che rendono più semplice aggiungere codice di uso comune in HTML, file di codice e altro ancora!</span><span class="sxs-lookup"><span data-stu-id="f8692-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="f8692-359">Provarlo digitando **&lt;div&gt;** e premendo **scheda** due volte per inserire un completo **div** tag.</span><span class="sxs-lookup"><span data-stu-id="f8692-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="f8692-360">Attività 2 - aggiunta foglio di stile CSS</span><span class="sxs-lookup"><span data-stu-id="f8692-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="f8692-361">Il modello di progetto vuoto include un file CSS molto semplice che include solo gli stili utilizzati per visualizzare forme di base e i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="f8692-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="f8692-362">Si userà per migliorare l'aspetto del sito aggiuntivi CSS e immagini (potenzialmente fornite da una finestra di progettazione).</span><span class="sxs-lookup"><span data-stu-id="f8692-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="f8692-363">In questa attività si aggiungerà un foglio di stile CSS per definire gli stili del sito.</span><span class="sxs-lookup"><span data-stu-id="f8692-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="f8692-364">I file CSS e immagini sono inclusi nel **Source\Assets\Content** cartella della presente esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="f8692-365">Per aggiungerle all'applicazione, trascinare i propri contenuti da un **Windows Explorer** finestra all'interno di **Esplora soluzioni** in Visual Studio Express per Web, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="f8692-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="f8692-366">![Trascinamento di contenuto di stile](aspnet-mvc-4-fundamentals/_static/image12.png "il trascinamento di contenuto stile")</span><span class="sxs-lookup"><span data-stu-id="f8692-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="f8692-367">*Trascinamento di contenuto stile*</span><span class="sxs-lookup"><span data-stu-id="f8692-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="f8692-368">Verrà visualizzata una finestra di dialogo di avviso, in cui viene chiesto di confermare se sostituire **CSS** file e alcune immagini esistente.</span><span class="sxs-lookup"><span data-stu-id="f8692-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="f8692-369">Controllare **applica a tutti gli elementi** e fare clic su **Yes**.</span><span class="sxs-lookup"><span data-stu-id="f8692-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="f8692-370">Attività 3: aggiunta di un modello di vista</span><span class="sxs-lookup"><span data-stu-id="f8692-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="f8692-371">In questa attività si aggiungerà un modello di vista per generare la risposta HTML che utilizzerà la pagina master layout e CSS aggiunti in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="f8692-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="f8692-372">Per usare un modello di vista durante l'esplorazione delle home page del sito, è necessario innanzitutto indicare che invece di restituire una stringa, il **indice HomeController** metodo restituirà una **visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="f8692-373">Aprire **HomeController** classe e modificare relativo **indice** metodo restituisca un' **ActionResult**, e restituisse **View()**.</span><span class="sxs-lookup"><span data-stu-id="f8692-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="f8692-374">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - indice di HomeController Ex4*)</span><span class="sxs-lookup"><span data-stu-id="f8692-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="f8692-375">A questo punto, è necessario aggiungere un modello di visualizzazione appropriato.</span><span class="sxs-lookup"><span data-stu-id="f8692-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="f8692-376">A tale scopo **fare doppio clic su** all'interno di **indice** metodo di azione e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="f8692-377">Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="f8692-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="f8692-378">![Aggiunta di una vista all'interno del metodo di indice](aspnet-mvc-4-fundamentals/_static/image13.png "aggiunta di una vista all'interno del metodo Index")</span><span class="sxs-lookup"><span data-stu-id="f8692-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="f8692-379">*Aggiunta di una vista all'interno del metodo Index*</span><span class="sxs-lookup"><span data-stu-id="f8692-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="f8692-380">Il **Aggiungi visualizzazione** verrà visualizzata finestra di dialogo per generare un file di modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="f8692-381">Per impostazione predefinita, questa finestra di dialogo popola preventivamente il nome del modello di visualizzazione in modo che corrisponda al metodo di azione che verrà usato.</span><span class="sxs-lookup"><span data-stu-id="f8692-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="f8692-382">Poiché è stato utilizzato il **Aggiungi visualizzazione** menu di scelta rapida all'interno del **indice** metodo di azione all'interno di HomeController, il **Aggiungi visualizzazione** dialogo ha indice come il nome della visualizzazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f8692-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="f8692-383">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8692-383">Click **Add**.</span></span>

    <span data-ttu-id="f8692-384">![Aggiungi finestra di dialogo di visualizzazione](aspnet-mvc-4-fundamentals/_static/image14.png "Visualizza finestra di dialogo Aggiungi")</span><span class="sxs-lookup"><span data-stu-id="f8692-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="f8692-385">*Visualizza finestra di dialogo Aggiungi*</span><span class="sxs-lookup"><span data-stu-id="f8692-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="f8692-386">Visual Studio genera una **index. cshtml** modello di visualizzazione all'interno di **Views\Home** cartella e quindi lo apre.</span><span class="sxs-lookup"><span data-stu-id="f8692-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="f8692-387">![Visualizzazione dell'indice creato-Home](aspnet-mvc-4-fundamentals/_static/image15.png "visualizzazione pagina iniziale dell'indice creato")</span><span class="sxs-lookup"><span data-stu-id="f8692-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="f8692-388">*Visualizzazione dell'indice iniziale creato*</span><span class="sxs-lookup"><span data-stu-id="f8692-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8692-389">nome e il percorso dei **index. cshtml** file pertinente e segue le convenzioni di denominazione di ASP.NET MVC predefinita.</span><span class="sxs-lookup"><span data-stu-id="f8692-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="f8692-390">La cartella \Views\**casa** corrisponde al nome del controller (**Home** Controller).</span><span class="sxs-lookup"><span data-stu-id="f8692-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="f8692-391">Il nome del modello di visualizzazione (**indice**), corrisponde al metodo di azione del controller che prevede di visualizzare la vista.</span><span class="sxs-lookup"><span data-stu-id="f8692-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="f8692-392">In questo modo, ASP.NET MVC evita di dover specificare esplicitamente il nome o il percorso di un modello di visualizzazione quando si usa questa convenzione di denominazione per restituire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="f8692-393">Il modello di vista generato si basa sul  **\_layout. cshtml** modello definito in precedenza.</span><span class="sxs-lookup"><span data-stu-id="f8692-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="f8692-394">Aggiornare la proprietà ViewBag **casa**e modificare il contenuto principale per **questa è la Home Page**, come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="f8692-395">Selezionare **MvcMusicStore** progetto in Esplora soluzioni e premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="f8692-396">Attività 4: verifica</span><span class="sxs-lookup"><span data-stu-id="f8692-396">Task 4: Verification</span></span>

<span data-ttu-id="f8692-397">Per verificare che siano state completate correttamente tutti i passaggi nell'esercizio precedente, procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="f8692-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="f8692-398">Con l'applicazione in un browser, si noti che:</span><span class="sxs-lookup"><span data-stu-id="f8692-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="f8692-399">Metodo di azione Index del HomeController trovare e visualizzare il **\Views\Home\Index.cshtml** visualizzare modelli, anche se il codice chiamato **restituire View()**, poiché il modello di vista seguito il convenzione di denominazione standard.</span><span class="sxs-lookup"><span data-stu-id="f8692-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="f8692-400">La Home Page viene visualizzato il messaggio di benvenuto definito all'interno di **\Views\Home\Index.cshtml** modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="f8692-401">Home Page è tramite il  **\_layout. cshtml** modello, e in modo che il messaggio di benvenuto è contenuto all'interno del layout del sito standard HTML.</span><span class="sxs-lookup"><span data-stu-id="f8692-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="f8692-402">![Visualizzazione dell'indice utilizzando il LayoutPage e lo stile definito-Home](aspnet-mvc-4-fundamentals/_static/image16.png "Home visualizzazione dell'indice utilizzando il LayoutPage e lo stile definito")</span><span class="sxs-lookup"><span data-stu-id="f8692-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="f8692-403">*Visualizzazione dell'indice iniziale usando il LayoutPage e lo stile definito*</span><span class="sxs-lookup"><span data-stu-id="f8692-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="f8692-404">Esercizio 5: Creazione di un modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f8692-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="f8692-405">Finora, apportate alle visualizzazioni visualizzare HTML hardcoded, ma, per poter creare applicazioni web dinamiche, il modello di vista deve ricevere informazioni dal Controller.</span><span class="sxs-lookup"><span data-stu-id="f8692-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="f8692-406">Una tecnica comune da utilizzare a tale scopo è il **ViewModel** modello, che consente a un Controller per creare un pacchetto di tutte le informazioni necessarie per generare la risposta appropriata in HTML.</span><span class="sxs-lookup"><span data-stu-id="f8692-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="f8692-407">In questo esercizio, si apprenderà come creare una classe ViewModel e aggiungere le proprietà obbligatorie: il numero di generi nell'archivio e un elenco di tali generi.</span><span class="sxs-lookup"><span data-stu-id="f8692-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="f8692-408">Anche se si aggiornerà il StoreController per l'uso di ViewModel creati e infine, si creerà un nuovo modello di visualizzazione che verrà visualizzate le proprietà indicate nella pagina.</span><span class="sxs-lookup"><span data-stu-id="f8692-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="f8692-409">Attività 1: creazione di una classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="f8692-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="f8692-410">In questa attività si creerà una classe di ViewModel che consente di implementare lo scenario di inserzione genre Store.</span><span class="sxs-lookup"><span data-stu-id="f8692-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="f8692-411">Se non è già aperta, avviare **Visual Studio Express per Web**.</span><span class="sxs-lookup"><span data-stu-id="f8692-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="f8692-412">Nel **File** menu, scegliere **aprire il progetto**.</span><span class="sxs-lookup"><span data-stu-id="f8692-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="f8692-413">Nella finestra di dialogo Apri progetto individuare **Source\Ex05 CreatingAViewModel\Begin**, selezionare **Begin.sln** e fare clic su **Open**.</span><span class="sxs-lookup"><span data-stu-id="f8692-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="f8692-414">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="f8692-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="f8692-415">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="f8692-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f8692-416">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f8692-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f8692-417">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="f8692-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f8692-418">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f8692-419">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8692-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f8692-420">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8692-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f8692-421">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f8692-422">Creare un **ViewModel** cartella per contenere l'elemento ViewModel.</span><span class="sxs-lookup"><span data-stu-id="f8692-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="f8692-423">A tale scopo, fare doppio clic su di primo livello **MvcMusicStore** progetto, selezionare **Add** e quindi **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f8692-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="f8692-424">![Aggiunta di una nuova cartella](aspnet-mvc-4-fundamentals/_static/image17.png "aggiunta una nuova cartella")</span><span class="sxs-lookup"><span data-stu-id="f8692-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="f8692-425">*Aggiunta di una nuova cartella*</span><span class="sxs-lookup"><span data-stu-id="f8692-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="f8692-426">Denominare la cartella *ViewModel*.</span><span class="sxs-lookup"><span data-stu-id="f8692-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="f8692-427">![Cartella ViewModel in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image18.png "cartella ViewModel in Esplora soluzioni")</span><span class="sxs-lookup"><span data-stu-id="f8692-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="f8692-428">*Cartella ViewModel in Esplora soluzioni*</span><span class="sxs-lookup"><span data-stu-id="f8692-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="f8692-429">Creare un **ViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="f8692-430">A tale scopo, fare clic sui **ViewModel** cartella creato di recente, selezionare **Add** e quindi **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="f8692-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="f8692-431">Sotto **codice**, scegliere il **classe** item e denominare il file *StoreIndexViewModel.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8692-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="f8692-432">![Aggiunta di una nuova classe](aspnet-mvc-4-fundamentals/_static/image19.png "aggiunta una nuova classe")</span><span class="sxs-lookup"><span data-stu-id="f8692-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="f8692-433">*Aggiunta di una nuova classe*</span><span class="sxs-lookup"><span data-stu-id="f8692-433">*Adding a new Class*</span></span>

    <span data-ttu-id="f8692-434">![Creazione classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel Crea classe")</span><span class="sxs-lookup"><span data-stu-id="f8692-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="f8692-435">*Creazione classe StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="f8692-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="f8692-436">Attività 2: aggiunta di proprietà alla classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="f8692-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="f8692-437">Esistono due parametri deve essere passato dal StoreController il modello di vista per generare la risposta HTML previsto: il numero di generi nell'archivio e un elenco di tali generi.</span><span class="sxs-lookup"><span data-stu-id="f8692-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="f8692-438">In questa attività si aggiungerà le 2 proprietà per il **StoreIndexViewModel** classe: **NumberOfGenres** (integer) e **generi** (un elenco di stringhe).</span><span class="sxs-lookup"><span data-stu-id="f8692-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="f8692-439">Aggiungere **NumberOfGenres** e **generi** delle proprietà per il **StoreIndexViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="f8692-440">A tale scopo, aggiungere le 2 righe seguente alla definizione della classe:</span><span class="sxs-lookup"><span data-stu-id="f8692-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="f8692-441">(Code - Snippet *ASP.NET MVC 4 Fundamentals - proprietà StoreIndexViewModel Ex5*)</span><span class="sxs-lookup"><span data-stu-id="f8692-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="f8692-442">Il **{get; set;}**  notazione si avvale di # funzionalità di proprietà implementate automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f8692-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="f8692-443">Che offre i vantaggi di una proprietà senza la necessità di dichiarare un campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="f8692-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="f8692-444">Attività 3: aggiornamento StoreController usare il StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="f8692-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="f8692-445">Il **StoreIndexViewModel** classe incapsula le informazioni necessarie per passare dal **StoreController**del **indice** metodo da un modello di vista per generare una risposta .</span><span class="sxs-lookup"><span data-stu-id="f8692-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="f8692-446">In questa attività si aggiornerà il **StoreController** da utilizzare il **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="f8692-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="f8692-447">Aprire **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="f8692-448">![Apertura di classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "classe StoreController apertura")</span><span class="sxs-lookup"><span data-stu-id="f8692-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="f8692-449">*Classe StoreController apertura*</span><span class="sxs-lookup"><span data-stu-id="f8692-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="f8692-450">Per usare la **StoreIndexViewModel** classe la **StoreController**, aggiungere lo spazio dei nomi seguente all'inizio del **StoreController** codice:</span><span class="sxs-lookup"><span data-stu-id="f8692-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="f8692-451">(Code - Snippet *ASP.NET MVC 4 Fundamentals - StoreIndexViewModel Ex5 usando ViewModel*)</span><span class="sxs-lookup"><span data-stu-id="f8692-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="f8692-452">Modifica il **StoreController**del **indice** metodo di azione in modo che si crea e popola una **StoreIndexViewModel** dell'oggetto e quindi li passa a un modello di vista generare una risposta HTML con esso.</span><span class="sxs-lookup"><span data-stu-id="f8692-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8692-453">Nel laboratorio &quot;l'accesso ai dati e modelli di MVC ASP.NET&quot; si scriverà il codice che recupera l'elenco dei generi store da un database.</span><span class="sxs-lookup"><span data-stu-id="f8692-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="f8692-454">Nel codice seguente, si creerà una **elenco** di generi dati fittizi in grado di popolare il **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="f8692-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="f8692-455">Dopo la creazione e configurazione del **StoreIndexViewModel** dell'oggetto, verrà passato come argomento per il **visualizzazione** (metodo).</span><span class="sxs-lookup"><span data-stu-id="f8692-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="f8692-456">Ciò indica che il modello di vista utilizzerà tale oggetto per generare una risposta HTML con esso.</span><span class="sxs-lookup"><span data-stu-id="f8692-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="f8692-457">Sostituire il **indice** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="f8692-458">(Code - Snippet *Fundamentals di ASP.NET MVC 4 - metodo Ex5 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="f8692-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="f8692-459">Se non si ha familiarità con c#, si può presupporre che l'utilizzo **var** significa che le **viewModel** variabile è l'associazione tardiva.</span><span class="sxs-lookup"><span data-stu-id="f8692-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="f8692-460">Non è corretto, il compilatore c# Usa basato su ciò che si assegna alla variabile di inferenza del tipo per determinare che **viewModel** JE typu **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="f8692-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="f8692-461">Inoltre, per la compilazione locale **viewModel** variabile come una **StoreIndexViewModel** tipo di controllo della fase di compilazione di get e il supporto di editor di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8692-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="f8692-462">Attività 4: creazione di un modello di visualizzazione che utilizza StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="f8692-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="f8692-463">In questa attività si creerà un modello di visualizzazione che verrà utilizzato un oggetto StoreIndexViewModel passato dal Controller per visualizzare un elenco dei generi.</span><span class="sxs-lookup"><span data-stu-id="f8692-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="f8692-464">Prima di creare il nuovo modello di vista, è possibile compilare il progetto in modo che il **finestra di dialogo Aggiungi visualizzazione** conosce le **StoreIndexViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="f8692-465">Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilazione MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="f8692-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="f8692-466">![Compilazione del progetto](aspnet-mvc-4-fundamentals/_static/image22.png "compilando il progetto")</span><span class="sxs-lookup"><span data-stu-id="f8692-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="f8692-467">*Compilazione del progetto*</span><span class="sxs-lookup"><span data-stu-id="f8692-467">*Building the project*</span></span>
2. <span data-ttu-id="f8692-468">Creare un nuovo modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-468">Create a new View template.</span></span> <span data-ttu-id="f8692-469">A tale scopo, fare doppio clic all'interno di **indice** (metodo) e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="f8692-470">![Aggiunta di una vista](aspnet-mvc-4-fundamentals/_static/image23.png "aggiunta di una vista")</span><span class="sxs-lookup"><span data-stu-id="f8692-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="f8692-471">*Aggiunta di una visualizzazione*</span><span class="sxs-lookup"><span data-stu-id="f8692-471">*Adding a View*</span></span>
3. <span data-ttu-id="f8692-472">Poiché il **finestra di dialogo Aggiungi visualizzazione** da cui è stato richiamato il **StoreController**, aggiungerà il modello di vista per impostazione predefinita in un **\Views\Store\Index.cshtml** file.</span><span class="sxs-lookup"><span data-stu-id="f8692-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="f8692-473">Verificare i **creare una fortemente tipizzate-visualizzazione** casella di controllo e quindi selezionare **StoreIndexViewModel** come il **classe modello**.</span><span class="sxs-lookup"><span data-stu-id="f8692-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="f8692-474">Inoltre, assicurarsi che il motore di visualizzazione selezionato sia **Razor**.</span><span class="sxs-lookup"><span data-stu-id="f8692-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="f8692-475">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8692-475">Click **Add**.</span></span>

    <span data-ttu-id="f8692-476">![Aggiungi finestra di dialogo di visualizzazione](aspnet-mvc-4-fundamentals/_static/image24.png "Visualizza finestra di dialogo Aggiungi")</span><span class="sxs-lookup"><span data-stu-id="f8692-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="f8692-477">*Visualizza finestra di dialogo Aggiungi*</span><span class="sxs-lookup"><span data-stu-id="f8692-477">*Add View Dialog*</span></span>

    <span data-ttu-id="f8692-478">Il **\Views\Store\Index.cshtml** viene creato e aperto il file di modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="f8692-479">Basato sulle informazioni fornite per il **Aggiungi visualizzazione** finestra di dialogo nell'ultimo passaggio, la visualizzazione modello si aspetta di ricevere un **StoreIndexViewModel** istanza come i dati da utilizzare per generare una risposta HTML.</span><span class="sxs-lookup"><span data-stu-id="f8692-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="f8692-480">Si noterà che il modello eredita un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` nel linguaggio c#.</span><span class="sxs-lookup"><span data-stu-id="f8692-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="f8692-481">Attività 5: aggiornare il modello di vista</span><span class="sxs-lookup"><span data-stu-id="f8692-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="f8692-482">In questa attività si aggiornerà il modello di vista creato nell'ultima attività per recuperare il numero di generi e i relativi nomi all'interno della pagina.</span><span class="sxs-lookup"><span data-stu-id="f8692-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="f8692-483">Si userà sintassi @ (anche detti &quot;nugget di codice&quot;) per eseguire il codice all'interno del modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="f8692-484">Nel **index. cshtml** nel file, all'interno di **Store** cartella, sostituire il codice con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="f8692-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

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
2. <span data-ttu-id="f8692-485">Scorrere l'elenco genre nella **StoreIndexViewModel** e creare un elemento HTML **&lt;ul&gt;** elencate mediante un **foreach** ciclo.</span><span class="sxs-lookup"><span data-stu-id="f8692-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="f8692-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="f8692-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="f8692-487">Premere **F5** per eseguire l'applicazione ed esplorare **/Store**.</span><span class="sxs-lookup"><span data-stu-id="f8692-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="f8692-488">Verrà visualizzato l'elenco di generi passato il **StoreIndexViewModel** dell'oggetto dal **StoreController** al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="f8692-489">![Visualizzazione con un elenco dei generi](aspnet-mvc-4-fundamentals/_static/image26.png "visualizzazione mostra un elenco dei generi")</span><span class="sxs-lookup"><span data-stu-id="f8692-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="f8692-490">*Visualizzazione con un elenco dei generi*</span><span class="sxs-lookup"><span data-stu-id="f8692-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="f8692-491">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="f8692-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="f8692-492">Esercizio 6: Utilizzo di parametri nella visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f8692-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="f8692-493">Nell'esercizio 3 è stato descritto come passare parametri al Controller.</span><span class="sxs-lookup"><span data-stu-id="f8692-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="f8692-494">In questo esercizio, si apprenderà come usare questi parametri nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="f8692-495">A tale scopo, verranno presentate innanzitutto alle classi di modello che consentono di gestire la logica di dominio e i dati.</span><span class="sxs-lookup"><span data-stu-id="f8692-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="f8692-496">Inoltre, si apprenderà come creare collegamenti a pagine all'interno dell'applicazione ASP.NET MVC senza doversi preoccupare di elementi quali i percorsi URL codifica.</span><span class="sxs-lookup"><span data-stu-id="f8692-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="f8692-497">Attività 1: aggiunta di classi del modello</span><span class="sxs-lookup"><span data-stu-id="f8692-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="f8692-498">A differenza di ViewModel, che vengono creati solo per passare le informazioni dal Controller alla visualizzazione, le classi del modello sono create per includono e gestiscono i dati e dominio per la logica.</span><span class="sxs-lookup"><span data-stu-id="f8692-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="f8692-499">In questa attività si aggiungeranno due classi di modello per rappresentare questi concetti: **Genre** e **Album**.</span><span class="sxs-lookup"><span data-stu-id="f8692-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="f8692-500">Se non è già aperta, avviare **Visual Studio Express per Web**</span><span class="sxs-lookup"><span data-stu-id="f8692-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="f8692-501">Nel **File** menu, scegliere **aprire il progetto**.</span><span class="sxs-lookup"><span data-stu-id="f8692-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="f8692-502">Nella finestra di dialogo Apri progetto individuare **Source\Ex06 UsingParametersInView\Begin**, selezionare **Begin.sln** e fare clic su **Open**.</span><span class="sxs-lookup"><span data-stu-id="f8692-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="f8692-503">In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="f8692-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="f8692-504">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="f8692-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f8692-505">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f8692-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f8692-506">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="f8692-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f8692-507">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f8692-508">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8692-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f8692-509">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8692-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f8692-510">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f8692-511">Aggiungere un **Genre** classe del modello.</span><span class="sxs-lookup"><span data-stu-id="f8692-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="f8692-512">A tale scopo, fare doppio clic sui **modelli** cartella nel **Esplora soluzioni**, selezionare **Add** e quindi il **nuovo elemento** opzione.</span><span class="sxs-lookup"><span data-stu-id="f8692-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="f8692-513">Sotto **codice**, scegliere il **classe** item e denominare il file *Genre.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8692-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="f8692-514">![Aggiunta di una classe](aspnet-mvc-4-fundamentals/_static/image27.png "aggiunta di una classe")</span><span class="sxs-lookup"><span data-stu-id="f8692-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="f8692-515">*Aggiunta di un nuovo elemento*</span><span class="sxs-lookup"><span data-stu-id="f8692-515">*Adding a new item*</span></span>

    <span data-ttu-id="f8692-516">![Aggiungere classe di modello Genre](aspnet-mvc-4-fundamentals/_static/image28.png "aggiungere classe di modello al genere")</span><span class="sxs-lookup"><span data-stu-id="f8692-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="f8692-517">*Aggiungere classe di modello al genere*</span><span class="sxs-lookup"><span data-stu-id="f8692-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="f8692-518">Aggiungere un **nome** proprietà alla classe del genere.</span><span class="sxs-lookup"><span data-stu-id="f8692-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="f8692-519">A tale scopo, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-519">To do this, add the following code:</span></span>

    <span data-ttu-id="f8692-520">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Genre Ex6*)</span><span class="sxs-lookup"><span data-stu-id="f8692-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="f8692-521">Attenersi alla procedura descritta in precedenza, aggiungere un' **Album** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="f8692-522">A tale scopo, fare doppio clic sui **modelli** cartella nel **Esplora soluzioni**, selezionare **Add** e quindi il **nuovo elemento** opzione.</span><span class="sxs-lookup"><span data-stu-id="f8692-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="f8692-523">Sotto **codice**, scegliere il **classe** item e denominare il file *Album.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8692-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="f8692-524">Aggiungere due proprietà alla classe Album: **Genre** e **titolo**.</span><span class="sxs-lookup"><span data-stu-id="f8692-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="f8692-525">A tale scopo, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-525">To do this, add the following code:</span></span>

    <span data-ttu-id="f8692-526">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Album Ex6*)</span><span class="sxs-lookup"><span data-stu-id="f8692-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="f8692-527">Attività 2: aggiunta di un StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="f8692-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="f8692-528">Oggetto **StoreBrowseViewModel** verrà usato in questa attività per mostrare gli album che corrispondono a un genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="f8692-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="f8692-529">In questa attività verrà creare questa classe e quindi aggiungere due proprietà che consentono di gestire il **Genre** e il relativo **Album**dell'elenco.</span><span class="sxs-lookup"><span data-stu-id="f8692-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="f8692-530">Aggiungere un **StoreBrowseViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="f8692-531">A tale scopo, fare doppio clic sul **ViewModel** cartella il **Esplora soluzioni**, selezionare **Add** e quindi il **nuovo elemento** opzione.</span><span class="sxs-lookup"><span data-stu-id="f8692-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="f8692-532">Sotto **codice**, scegliere il **classe** item e denominare il file *StoreBrowseViewModel.cs*, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8692-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="f8692-533">Aggiungere un riferimento ai modelli nel **StoreBrowseViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="f8692-534">A tale scopo, aggiungere il codice seguente utilizza spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="f8692-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="f8692-535">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="f8692-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="f8692-536">Aggiungere due proprietà consentono **StoreBrowseViewModel** classe: **Genre** e **album**.</span><span class="sxs-lookup"><span data-stu-id="f8692-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="f8692-537">A tale scopo, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-537">To do this, add the following code:</span></span>

    <span data-ttu-id="f8692-538">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="f8692-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="f8692-539">What ' s **elenco&lt;Album&gt;**  ?: questa definizione Usa i **elenco&lt;T&gt;**  tipo, dove **T** vincola la tipo da cui ottenere elementi di questo **elenco** appartiene, in questo caso **Album** (o uno qualsiasi dei relativi discendenti).</span><span class="sxs-lookup"><span data-stu-id="f8692-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="f8692-540">La possibilità di progettare classi e metodi che rinviano la specifica di uno o più tipi finché non la classe o il metodo viene dichiarato e creare un'istanza dal codice client è una funzionalità del linguaggio c# denominato **Generics**.</span><span class="sxs-lookup"><span data-stu-id="f8692-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="f8692-541">**Elenco&lt;T&gt;**  equivale a generico di **ArrayList** digitare ed è disponibile nel **System.Collections.Generic** dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="f8692-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="f8692-542">Uno dei vantaggi dell'utilizzo **generics** è che poiché il tipo è specificato, non occorre occuparsi di operazioni, ad esempio il cast degli elementi nel controllo del tipo **Album** come si farebbe con un' **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="f8692-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="f8692-543">Attività 3: usando l'elemento ViewModel di nuovo nel StoreController</span><span class="sxs-lookup"><span data-stu-id="f8692-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="f8692-544">In questa attività si modificherà il **StoreController**del **Sfoglia** e **dettagli** metodi di azione per usare il nuovo **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="f8692-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="f8692-545">Aggiungere un riferimento alla cartella modelli in **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="f8692-546">A tale scopo, espandere la **controller** cartella nel **Esplora soluzioni** e aprire il **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="f8692-547">Aggiungere quindi il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-547">Then add the following code:</span></span>

    <span data-ttu-id="f8692-548">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="f8692-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="f8692-549">Sostituire il **esplorare** metodo di azione per utilizzare il **StoreViewBrowseController** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="f8692-550">Si creerà un genere e due nuovi oggetti di album con dati fittizi (nel laboratorio pratico successivo si utilizzeranno dati reali da un database).</span><span class="sxs-lookup"><span data-stu-id="f8692-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="f8692-551">A tale scopo, sostituire il **esplorare** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="f8692-552">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="f8692-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="f8692-553">Sostituire il **informazioni dettagliate** metodo di azione per utilizzare il **StoreViewBrowseController** classe.</span><span class="sxs-lookup"><span data-stu-id="f8692-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="f8692-554">Si creerà una nuova **Album** oggetto da restituire per il **visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="f8692-555">A tale scopo, sostituire il **dettagli** metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="f8692-556">(Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="f8692-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="f8692-557">Attività 4: aggiunta di un modello di vista di esplorazione</span><span class="sxs-lookup"><span data-stu-id="f8692-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="f8692-558">In questa attività si aggiungerà un **esplorare** visualizzazione per mostrare gli album trovati per un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="f8692-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="f8692-559">Prima di creare il nuovo modello di vista, è necessario compilare il progetto in modo che il **Aggiungi visualizzazione** finestra di dialogo è a conoscenza di **ViewModel** classe da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="f8692-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="f8692-560">Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilazione MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="f8692-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="f8692-561">Aggiungere un **esplorare** visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-561">Add a **Browse** View.</span></span> <span data-ttu-id="f8692-562">A tale scopo, fare doppio clic nella **esplorare** metodo di azione del **StoreController** e fare clic su **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="f8692-563">Nel **Aggiungi visualizzazione** finestra di dialogo, verificare che il nome della vista sia **Sfoglia**.</span><span class="sxs-lookup"><span data-stu-id="f8692-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="f8692-564">Verificare i **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **StoreBrowseViewModel** dal **classe modello** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="f8692-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="f8692-565">Lasciare gli altri campi con il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="f8692-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="f8692-566">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8692-566">Then click **Add**.</span></span>

    <span data-ttu-id="f8692-567">![Aggiunta di una visualizzazione esplorazione](aspnet-mvc-4-fundamentals/_static/image29.png "aggiunta di una vista di esplorazione")</span><span class="sxs-lookup"><span data-stu-id="f8692-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="f8692-568">*Aggiunta di una vista di esplorazione*</span><span class="sxs-lookup"><span data-stu-id="f8692-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="f8692-569">Modificare il **Browse.cshtml** per visualizzare le informazioni del genere, l'accesso di **StoreBrowseViewModel** oggetto passato al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="f8692-570">A tale scopo, sostituire il contenuto con quanto segue: (c#)</span><span class="sxs-lookup"><span data-stu-id="f8692-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="f8692-571">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f8692-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="f8692-572">In questa attività si testerà che il **esplorare** che consente di recuperare gli album dal **Sfoglia** azione del metodo.</span><span class="sxs-lookup"><span data-stu-id="f8692-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="f8692-573">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="f8692-574">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="f8692-574">The project starts in the Home page.</span></span> <span data-ttu-id="f8692-575">Modificare l'URL in   **/Store/Sfoglia? Genre = Disco** per verificare che l'azione restituisce due album.</span><span class="sxs-lookup"><span data-stu-id="f8692-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="f8692-576">![Esplorazione Store discoteca album](aspnet-mvc-4-fundamentals/_static/image30.png "esplorazione album discoteca Store")</span><span class="sxs-lookup"><span data-stu-id="f8692-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="f8692-577">*Esplorazione di album discoteca Store*</span><span class="sxs-lookup"><span data-stu-id="f8692-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="f8692-578">Attività 6 - visualizzazione di informazioni su una specifica Album</span><span class="sxs-lookup"><span data-stu-id="f8692-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="f8692-579">In questa attività si implementerà il **Store/dettagli** visualizzazione per visualizzare informazioni su un album specifico.</span><span class="sxs-lookup"><span data-stu-id="f8692-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="f8692-580">In questo laboratorio pratico, tutto ciò che visualizzerà relative all'album è già contenuto nel **vista** modello.</span><span class="sxs-lookup"><span data-stu-id="f8692-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="f8692-581">In questo caso, invece di crearne una **StoreDetailsViewModel** (classe), si userà corrente **StoreBrowseViewModel** passando all'Album a tale modello.</span><span class="sxs-lookup"><span data-stu-id="f8692-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="f8692-582">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8692-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="f8692-583">Aggiungere un nuovo **informazioni dettagliate** visualizzare per il **StoreController**del **dettagli** metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f8692-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="f8692-584">A tale scopo, fare doppio clic sul **informazioni dettagliate** metodo nella **StoreController** classe e fare clic su **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="f8692-585">Nel **Aggiungi visualizzazione** finestra di dialogo verificare che il **nome della vista** viene **dettagli**.</span><span class="sxs-lookup"><span data-stu-id="f8692-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="f8692-586">Verificare i **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **Album** dal **classe modello** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="f8692-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="f8692-587">Lasciare gli altri campi con il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="f8692-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="f8692-588">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8692-588">Then click **Add**.</span></span> <span data-ttu-id="f8692-589">Verrà creata e aprire una **\Views\Store\Details.cshtml** file.</span><span class="sxs-lookup"><span data-stu-id="f8692-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="f8692-590">![Aggiunta di una visualizzazione dettagli](aspnet-mvc-4-fundamentals/_static/image31.png "aggiunta di una visualizzazione dettagli")</span><span class="sxs-lookup"><span data-stu-id="f8692-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="f8692-591">*Aggiunta di una visualizzazione dettagli*</span><span class="sxs-lookup"><span data-stu-id="f8692-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="f8692-592">Modificare il **Details. cshtml** file per visualizzare le informazioni di un album trae, l'accesso alla **Album** oggetto passato al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="f8692-593">A tale scopo, sostituire il contenuto con quanto segue: (c#)</span><span class="sxs-lookup"><span data-stu-id="f8692-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="f8692-594">Attività 7: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f8692-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="f8692-595">In questa attività si testerà che il **informazioni dettagliate** vista recupera le informazioni di Album dalle **azione in dettaglio** (metodo).</span><span class="sxs-lookup"><span data-stu-id="f8692-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="f8692-596">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="f8692-597">Il progetto viene avviato nel **Home** pagina.</span><span class="sxs-lookup"><span data-stu-id="f8692-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="f8692-598">Modificare l'URL **/Store/Details/5** per verificare le informazioni dell'album.</span><span class="sxs-lookup"><span data-stu-id="f8692-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="f8692-599">![Esplorazione dettaglio album](aspnet-mvc-4-fundamentals/_static/image32.png "dettaglio album di esplorazione")</span><span class="sxs-lookup"><span data-stu-id="f8692-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="f8692-600">*Esplorazione dettaglio dell'Album*</span><span class="sxs-lookup"><span data-stu-id="f8692-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="f8692-601">Attività 8: aggiunta di collegamenti tra pagine</span><span class="sxs-lookup"><span data-stu-id="f8692-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="f8692-602">In questa attività si aggiungerà un collegamento degli Store per avere un collegamento in nome ogni genere appropriati **/Store/Sfoglia** URL.</span><span class="sxs-lookup"><span data-stu-id="f8692-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="f8692-603">In questo modo, quando si fa clic in genere, ad esempio **Disco**, passerà a **/Store/Sfoglia? genre = Disco** URL.</span><span class="sxs-lookup"><span data-stu-id="f8692-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="f8692-604">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8692-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="f8692-605">Aggiornamento di **indice** pagina per aggiungere un collegamento al **Sfoglia** pagina.</span><span class="sxs-lookup"><span data-stu-id="f8692-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="f8692-606">A questo scopo, nella **Esplora soluzioni** espandere il **viste** cartella, il **Store** cartella e fare doppio clic il **index. cshtml** pagina.</span><span class="sxs-lookup"><span data-stu-id="f8692-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="f8692-607">Aggiungere un collegamento alla visualizzazione di esplorazione che indica il genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="f8692-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="f8692-608">A tale scopo, sostituire il codice evidenziato seguente all'interno di **&lt;li&gt;** tag: (c#)</span><span class="sxs-lookup"><span data-stu-id="f8692-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="f8692-609">un altro approccio potrebbe essere collegato direttamente alla pagina, con un codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f8692-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="f8692-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="f8692-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="f8692-611">Sebbene questo approccio funzioni, a seconda stringa hardcoded.</span><span class="sxs-lookup"><span data-stu-id="f8692-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="f8692-612">Se in un secondo momento si rinomina il Controller, sarà necessario modificare manualmente questa istruzione.</span><span class="sxs-lookup"><span data-stu-id="f8692-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="f8692-613">Un'alternativa migliore consiste nell'utilizzare un **HTML Helper** (metodo).</span><span class="sxs-lookup"><span data-stu-id="f8692-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="f8692-614">ASP.NET MVC include un metodo HTML Helper che è disponibile per tali attività.</span><span class="sxs-lookup"><span data-stu-id="f8692-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="f8692-615">Il **Html.ActionLink()** metodo helper rende più semplice compilare HTML **&lt;una&gt;** collegamenti, assicurandosi che i percorsi URL siano URL correttamente codificati.</span><span class="sxs-lookup"><span data-stu-id="f8692-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="f8692-616">Htlm.ActionLink dispone di diversi overload.</span><span class="sxs-lookup"><span data-stu-id="f8692-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="f8692-617">In questo esercizio si userà uno che accetta tre parametri:</span><span class="sxs-lookup"><span data-stu-id="f8692-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="f8692-618">Testo del collegamento, che verrà visualizzato il nome genere</span><span class="sxs-lookup"><span data-stu-id="f8692-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="f8692-619">Nome azione del controller (**esplorare**)</span><span class="sxs-lookup"><span data-stu-id="f8692-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="f8692-620">I valori di parametro, che specifica il nome di route (**Genre**) e il valore (**nome genere**)</span><span class="sxs-lookup"><span data-stu-id="f8692-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="f8692-621">Attività 9: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f8692-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="f8692-622">In questa attività si testerà che ogni genere viene visualizzato con un collegamento a appropriato **/Store/Sfoglia** URL.</span><span class="sxs-lookup"><span data-stu-id="f8692-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="f8692-623">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="f8692-624">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="f8692-624">The project starts in the Home page.</span></span> <span data-ttu-id="f8692-625">Modificare l'URL **/Store** per verificare che ogni genere collegamenti appropriati **/Store/Sfoglia** URL.</span><span class="sxs-lookup"><span data-stu-id="f8692-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="f8692-626">![Esplorazione generi con collegamenti alla pagina di esplorazione](aspnet-mvc-4-fundamentals/_static/image33.png "generi di esplorazione con i collegamenti alla pagina di esplorazione")</span><span class="sxs-lookup"><span data-stu-id="f8692-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="f8692-627">*Esplorazione generi con collegamenti alla pagina di esplorazione*</span><span class="sxs-lookup"><span data-stu-id="f8692-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="f8692-628">Attività 10 - usando ViewModel dinamici raccolta per passare i valori</span><span class="sxs-lookup"><span data-stu-id="f8692-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="f8692-629">In questa attività si apprenderà un metodo semplice e potente per passare i valori tra il Controller e la visualizzazione senza apportare alcuna modifica nel modello.</span><span class="sxs-lookup"><span data-stu-id="f8692-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="f8692-630">ASP.NET MVC 4 fornisce la raccolta &quot;ViewModel&quot;, che può essere assegnato a qualsiasi valore dinamico e controller e visualizzazioni nonché accedere direttamente.</span><span class="sxs-lookup"><span data-stu-id="f8692-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="f8692-631">Verrà utilizzata per passare un elenco di raccolta dinamica ViewBag &quot; **contrassegnata con stelle generi** &quot; dal controller alla vista.</span><span class="sxs-lookup"><span data-stu-id="f8692-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="f8692-632">La visualizzazione dell'indice Store avranno accesso a **ViewModel** e visualizzare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="f8692-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="f8692-633">Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8692-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="f8692-634">Aprire **StoreController.cs** e modificare **indice** metodo per creare un elenco di contrassegnata con stelle generi nella raccolta di ViewModel:</span><span class="sxs-lookup"><span data-stu-id="f8692-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="f8692-635">È anche possibile usare la sintassi **ViewBag [&quot;Starred&quot;]** per accedere alle proprietà.</span><span class="sxs-lookup"><span data-stu-id="f8692-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="f8692-636">L'icona a stella **&quot;starred.png&quot;** è incluso nel **Source\Assets\Images** cartella della presente esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="f8692-637">Per aggiungerla all'applicazione, trascinare i propri contenuti da un **Windows Explorer** finestra all'interno di **Esplora soluzioni** in Visual Web Developer Express, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="f8692-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="f8692-638">![Immagine star aggiunta alla soluzione](aspnet-mvc-4-fundamentals/_static/image34.png "immagine star aggiunta alla soluzione")</span><span class="sxs-lookup"><span data-stu-id="f8692-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="f8692-639">*Aggiunta di immagine star alla soluzione*</span><span class="sxs-lookup"><span data-stu-id="f8692-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="f8692-640">Aprire la visualizzazione **Store/Index.cshtml** e modificare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="f8692-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="f8692-641">Leggeranno i &quot;contrassegnata con stelle&quot; proprietà nel **ViewBag** insieme e chiedere se il nome del genere corrente è nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="f8692-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="f8692-642">In questo caso si mostrerà un'icona a stella a destra per il collegamento al genere.</span><span class="sxs-lookup"><span data-stu-id="f8692-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="f8692-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="f8692-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="f8692-644">Attività 11: eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="f8692-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="f8692-645">In questa attività si testerà che generi con stelle visualizzino un'icona a stella.</span><span class="sxs-lookup"><span data-stu-id="f8692-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="f8692-646">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="f8692-647">Il progetto viene avviato nel **Home** pagina.</span><span class="sxs-lookup"><span data-stu-id="f8692-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="f8692-648">Modificare l'URL **/Store** per verificare che ogni genere in primo piano è associata l'etichetta respecting:</span><span class="sxs-lookup"><span data-stu-id="f8692-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="f8692-649">![Esplorazione generi con elementi con stelle](aspnet-mvc-4-fundamentals/_static/image35.png "generi di esplorazione con gli elementi con stelle")</span><span class="sxs-lookup"><span data-stu-id="f8692-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="f8692-650">*Esplorazione generi con elementi con stelle*</span><span class="sxs-lookup"><span data-stu-id="f8692-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="f8692-651">Esercizio 7: Una visione generale di nuovo modello di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="f8692-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="f8692-652">In questo esercizio si esploreranno i miglioramenti nei modelli di progetto ASP.NET MVC 4, come ad esempio funzioni rilevanti al massimo del nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="f8692-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="f8692-653">Attività 1: Esplorare il modello di applicazione Internet ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="f8692-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="f8692-654">Se non è già aperta, avviare **Visual Studio Express per Web**</span><span class="sxs-lookup"><span data-stu-id="f8692-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="f8692-655">Selezionare il **File | New | Progetto** comando di menu.</span><span class="sxs-lookup"><span data-stu-id="f8692-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="f8692-656">Nel **nuovo progetto** finestra di dialogo, seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere il **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="f8692-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="f8692-657">**Nome** il progetto *MusicStore* e aggiornare il **Nome soluzione** alla *Begin*, quindi selezionare una località (o lasciare il valore predefinito) e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="f8692-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="f8692-658">![Creazione di un nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "creando un nuovo progetto ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="f8692-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="f8692-659">*Creazione di un nuovo progetto ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="f8692-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="f8692-660">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona la **applicazione Internet** modello di progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8692-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="f8692-661">Si noti che è possibile selezionare Razor o ASPX come il motore di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="f8692-662">![Crea una nuova applicazione Internet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "crea una nuova applicazione Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="f8692-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="f8692-663">*Crea una nuova applicazione Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="f8692-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8692-664">Sintassi Razor è stato introdotto in ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="f8692-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="f8692-665">L'obiettivo consiste nel ridurre al minimo il numero di caratteri e sequenze di tasti richieste in un file, abilitando un veloce e fluido codifica del flusso di lavoro.</span><span class="sxs-lookup"><span data-stu-id="f8692-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="f8692-666">Razor Usa esistente C# /VB (o altro) competenze linguistiche e offre una sintassi di markup di modello che consente un'incredibile HTML costruzione del flusso di lavoro.</span><span class="sxs-lookup"><span data-stu-id="f8692-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="f8692-667">Premere **F5** per eseguire la soluzione e visualizzare il modello rinnovato.</span><span class="sxs-lookup"><span data-stu-id="f8692-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="f8692-668">È possibile estrarre le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="f8692-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="f8692-669">**Modelli di stile moderno**</span><span class="sxs-lookup"><span data-stu-id="f8692-669">**Modern-style templates**</span></span>

        <span data-ttu-id="f8692-670">I modelli sono stati rinnovati, che fornisce altri stili dall'aspetto moderno.</span><span class="sxs-lookup"><span data-stu-id="f8692-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="f8692-671">![I modelli ASP.NET MVC 4 modificato nello stile](aspnet-mvc-4-fundamentals/_static/image38.png "modificato nello stile modelli ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="f8692-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="f8692-672">*Modelli ASP.NET MVC 4 modificato nello stile*</span><span class="sxs-lookup"><span data-stu-id="f8692-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="f8692-673">**Rendering adattivo**</span><span class="sxs-lookup"><span data-stu-id="f8692-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="f8692-674">Consulta il ridimensionamento della finestra del browser e notare come il layout di pagina si adatta dinamicamente per le nuove dimensioni della finestra.</span><span class="sxs-lookup"><span data-stu-id="f8692-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="f8692-675">Questi modelli utilizzano la tecnica di rendering adattivo per eseguire il rendering correttamente in piattaforme mobili e desktop senza eseguire alcuna personalizzazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="f8692-676">![Modello di progetto ASP.NET MVC 4 in dimensioni di browser differenti](aspnet-mvc-4-fundamentals/_static/image39.png "modello di progetto ASP.NET MVC 4 in dimensioni diversa del browser")</span><span class="sxs-lookup"><span data-stu-id="f8692-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="f8692-677">*Modello di progetto ASP.NET MVC 4 in dimensioni diversa del browser*</span><span class="sxs-lookup"><span data-stu-id="f8692-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="f8692-678">Chiudere il browser per arrestare il debugger, tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8692-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="f8692-679">A questo punto si è in grado di esplorare la soluzione e Scopri alcune delle nuove funzionalità introdotte da ASP.NET MVC 4 nel modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="f8692-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="f8692-680">![ASP.NET MVC4-internet---modello di progetto applicazione](aspnet-mvc-4-fundamentals/_static/image40.png "il modello di progetto applicazione Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="f8692-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="f8692-681">*Il modello di progetto applicazione Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="f8692-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="f8692-682">**Tag HTML5**</span><span class="sxs-lookup"><span data-stu-id="f8692-682">**HTML5 markup**</span></span>

       <span data-ttu-id="f8692-683">Esplorare le viste del modello per individuare il nuovo tag di tema, ad esempio open **About. cshtml** visualizzare entro **Home** cartella.</span><span class="sxs-lookup"><span data-stu-id="f8692-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="f8692-684">![Nuovo modello, utilizzando il markup Razor e HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nuovo modello, utilizzando il markup Razor e HTML5")</span><span class="sxs-lookup"><span data-stu-id="f8692-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="f8692-685">*Nuovo modello, utilizzando il markup Razor e HTML5*</span><span class="sxs-lookup"><span data-stu-id="f8692-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="f8692-686">**Librerie JavaScript inclusione**</span><span class="sxs-lookup"><span data-stu-id="f8692-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="f8692-687">**jQuery**: jQuery semplifica l'attraversamento dei documenti HTML, la gestione degli eventi, l'animazione e interazioni di Ajax.</span><span class="sxs-lookup"><span data-stu-id="f8692-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="f8692-688">**interfaccia utente di jQuery**: questa libreria fornisce astrazioni per themeable widget basato la libreria JavaScript jQuery e animazione, effetti avanzati e interazione di basso livello.</span><span class="sxs-lookup"><span data-stu-id="f8692-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="f8692-689">Sono disponibili informazioni jQuery UI e jQuery nelle [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="f8692-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="f8692-690">**Knockout. js**: modello predefinito di ASP.NET MVC 4 include ora **Knockout. js**, un framework MVVM di JavaScript che consente di creare applicazioni web avanzate e particolarmente efficienti con JavaScript e HTML.</span><span class="sxs-lookup"><span data-stu-id="f8692-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="f8692-691">Come in ASP.NET MVC 3, jQuery e librerie dell'interfaccia utente di jQuery sono inoltre inclusi in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f8692-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="f8692-692">È possibile ottenere altre informazioni sulla libreria Knockout. js in questo collegamento: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="f8692-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="f8692-693">**Modernizr**: questa libreria viene eseguito automaticamente, rendere un sito Web compatibile con i browser meno recenti quando si usano le tecnologie HTML5 e CSS3.</span><span class="sxs-lookup"><span data-stu-id="f8692-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="f8692-694">È possibile ottenere altre informazioni sulla libreria Modernizr in questo collegamento: [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="f8692-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="f8692-695">**SimpleMembership incluso nella soluzione**</span><span class="sxs-lookup"><span data-stu-id="f8692-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="f8692-696">SimpleMembership è stata progettata come una sostituzione per il sistema di provider di appartenenze e ruoli ASP.NET precedente.</span><span class="sxs-lookup"><span data-stu-id="f8692-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="f8692-697">Include numerose nuove funzionalità che rendono più semplice per gli sviluppatori per proteggere le pagine web in modo più flessibile.</span><span class="sxs-lookup"><span data-stu-id="f8692-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="f8692-698">Il modello Internet già dispone di configurare alcuni aspetti da integrare SimpleMembership, ad esempio, il AccountController sono pronti per usare OAuthWebSecurity (per la registrazione dell'account OAuth, account di accesso, gestione e così via) e la sicurezza Web.</span><span class="sxs-lookup"><span data-stu-id="f8692-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="f8692-699">![SimpleMembership incluso nella soluzione](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluso nella soluzione")</span><span class="sxs-lookup"><span data-stu-id="f8692-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="f8692-700">*SimpleMembership incluso nella soluzione*</span><span class="sxs-lookup"><span data-stu-id="f8692-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="f8692-701">Trovare altre informazioni sulle [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span><span class="sxs-lookup"><span data-stu-id="f8692-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="f8692-702">Inoltre, è possibile distribuire questa applicazione per siti Web di Azure seguenti [appendice b: pubblicazione un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="f8692-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="f8692-703">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f8692-703">Summary</span></span>

<span data-ttu-id="f8692-704">Completando questa pratica si è appreso le nozioni di base di ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="f8692-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="f8692-705">Elementi principali di un'applicazione MVC e come interagiscono</span><span class="sxs-lookup"><span data-stu-id="f8692-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="f8692-706">Come creare un'applicazione ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f8692-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="f8692-707">Come aggiungere e configurare i controller per gestire i parametri passati tramite l'URL e la stringa di query</span><span class="sxs-lookup"><span data-stu-id="f8692-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="f8692-708">Come aggiungere una pagina master layout per la configurazione di un modello per il contenuto HTML comune, un foglio di stile per migliorare l'aspetto e un modello di vista per visualizzare il contenuto HTML</span><span class="sxs-lookup"><span data-stu-id="f8692-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="f8692-709">Come usare il modello a ViewModel per passare le proprietà per il modello di vista per visualizzare informazioni dinamiche</span><span class="sxs-lookup"><span data-stu-id="f8692-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="f8692-710">Come usare i parametri passati ai controller nel modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="f8692-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="f8692-711">Come aggiungere collegamenti a pagine all'interno dell'applicazione ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f8692-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="f8692-712">Come aggiungere e usare le proprietà dinamiche in una vista</span><span class="sxs-lookup"><span data-stu-id="f8692-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="f8692-713">I miglioramenti nei modelli di progetto ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="f8692-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="f8692-714">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="f8692-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="f8692-715">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="f8692-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="f8692-716">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="f8692-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="f8692-717">Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="f8692-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="f8692-718">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="f8692-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="f8692-719">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="f8692-719">Click on **Install Now**.</span></span> <span data-ttu-id="f8692-720">Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.</span><span class="sxs-lookup"><span data-stu-id="f8692-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="f8692-721">Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="f8692-722">![Installa Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="f8692-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="f8692-723">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="f8692-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="f8692-724">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="f8692-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="f8692-726">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="f8692-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="f8692-727">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-727">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="f8692-729">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="f8692-729">*Installation progress*</span></span>
6. <span data-ttu-id="f8692-730">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="f8692-730">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="f8692-732">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="f8692-732">*Installation completed*</span></span>
7. <span data-ttu-id="f8692-733">Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="f8692-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="f8692-734">Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="f8692-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express per il riquadro Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="f8692-736">*Visual Studio Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="f8692-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f8692-737">Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="f8692-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="f8692-738">In questa appendice spiega come creare un nuovo sito web dal portale di gestione di Azure e pubblicare l'applicazione che è stato ottenuto seguendo il lab, sfruttando la funzionalità di pubblicazione di distribuzione Web fornita da Azure.</span><span class="sxs-lookup"><span data-stu-id="f8692-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="f8692-739">Attività 1: creazione di un nuovo sito Web di Windows portale di Azure</span><span class="sxs-lookup"><span data-stu-id="f8692-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="f8692-740">Andare alla [portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere usando le credenziali di Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="f8692-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8692-741">Con Windows Azure Puoi ospitare gratuitamente 10 siti Web ASP.NET e la scalabilità orizzontale man mano che aumenta il traffico.</span><span class="sxs-lookup"><span data-stu-id="f8692-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="f8692-742">È possibile effettuare l'iscrizione [qui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="f8692-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="f8692-743">![Accedere al portale di Azure](aspnet-mvc-4-fundamentals/_static/image48.png "accedere al portale di Azure")</span><span class="sxs-lookup"><span data-stu-id="f8692-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="f8692-744">*Accedere al portale di gestione di Azure*</span><span class="sxs-lookup"><span data-stu-id="f8692-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="f8692-745">Fare clic su **New** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="f8692-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="f8692-746">![Creazione di un nuovo sito Web](aspnet-mvc-4-fundamentals/_static/image49.png "creando un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="f8692-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="f8692-747">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="f8692-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="f8692-748">Fare clic su **Compute** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="f8692-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="f8692-749">Quindi selezionare **creazione rapida** opzione.</span><span class="sxs-lookup"><span data-stu-id="f8692-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="f8692-750">Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="f8692-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8692-751">Un sito Web di Microsoft Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="f8692-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="f8692-752">L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure all'esterno del portale.</span><span class="sxs-lookup"><span data-stu-id="f8692-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="f8692-753">Non include i passaggi per la configurazione di un database.</span><span class="sxs-lookup"><span data-stu-id="f8692-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="f8692-754">![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-fundamentals/_static/image50.png "creando un nuovo sito Web mediante Creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="f8692-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="f8692-755">*Creazione di un nuovo sito Web utilizzando Creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="f8692-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="f8692-756">Attendere finché il nuovo **sito Web** viene creato.</span><span class="sxs-lookup"><span data-stu-id="f8692-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="f8692-757">Dopo aver creato il sito Web fare clic sul collegamento sotto i **URL** colonna.</span><span class="sxs-lookup"><span data-stu-id="f8692-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="f8692-758">Verificare che il nuovo sito Web sia in funzione.</span><span class="sxs-lookup"><span data-stu-id="f8692-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="f8692-759">![Passare al nuovo sito web](aspnet-mvc-4-fundamentals/_static/image51.png "passare al nuovo sito web")</span><span class="sxs-lookup"><span data-stu-id="f8692-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="f8692-760">*Passare al nuovo sito web*</span><span class="sxs-lookup"><span data-stu-id="f8692-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="f8692-761">![Sito Web in esecuzione](aspnet-mvc-4-fundamentals/_static/image52.png "sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="f8692-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="f8692-762">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="f8692-762">*Web site running*</span></span>
6. <span data-ttu-id="f8692-763">Tornare al portale e fare clic sul nome del sito web con il **nome** colonna per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="f8692-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="f8692-764">![Aprire le pagine di gestione sito web](aspnet-mvc-4-fundamentals/_static/image53.png "aprire le pagine di gestione sito web")</span><span class="sxs-lookup"><span data-stu-id="f8692-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="f8692-765">*Aprire le pagine di gestione sito Web*</span><span class="sxs-lookup"><span data-stu-id="f8692-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="f8692-766">Nel **Dashboard** nella pagina il **riepilogo rapido** fare clic sui **Scarica profilo di pubblicazione** collegamento.</span><span class="sxs-lookup"><span data-stu-id="f8692-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8692-767">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="f8692-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="f8692-768">Il profilo di pubblicazione contiene l'URL, le credenziali dell'utente e stringhe di database necessarie per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="f8692-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="f8692-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="f8692-770">![Download del sito web di profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image54.png "scaricando il sito web di profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="f8692-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="f8692-771">*Profilo di pubblicazione scaricato il sito Web*</span><span class="sxs-lookup"><span data-stu-id="f8692-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="f8692-772">Scaricare il file di profilo di pubblicazione in una posizione nota.</span><span class="sxs-lookup"><span data-stu-id="f8692-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="f8692-773">In questo esercizio ulteriormente si vedrà come usare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8692-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="f8692-774">![Salvare il file di profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image55.png "salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="f8692-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="f8692-775">*Salvare il file di profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="f8692-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="f8692-776">Attività 2: configurare il Server di Database</span><span class="sxs-lookup"><span data-stu-id="f8692-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="f8692-777">Se l'applicazione Usa SQL Server database è necessario creare un Database di SQL server.</span><span class="sxs-lookup"><span data-stu-id="f8692-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="f8692-778">Se si vuole distribuire una semplice applicazione che non Usa SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="f8692-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="f8692-779">È necessario un server di Database SQL per archiviare il database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f8692-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="f8692-780">È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="f8692-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="f8692-781">Se non si dispone di un server creato, è possibile crearne una usando il **Add** pulsante sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="f8692-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="f8692-782">Annotare il **nome server e URL, nome dell'account di accesso amministratore e la password**, come verranno usati nelle attività successiva.</span><span class="sxs-lookup"><span data-stu-id="f8692-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="f8692-783">Non creare il database, come verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="f8692-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="f8692-784">![Dashboard di Server di Database SQL](aspnet-mvc-4-fundamentals/_static/image56.png "Dashboard di Server di Database SQL")</span><span class="sxs-lookup"><span data-stu-id="f8692-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="f8692-785">*Dashboard di Server di Database SQL*</span><span class="sxs-lookup"><span data-stu-id="f8692-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="f8692-786">Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server del **indirizzi IP consentiti**.</span><span class="sxs-lookup"><span data-stu-id="f8692-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="f8692-787">A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP dal **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e scegliere il ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) pulsante.</span><span class="sxs-lookup"><span data-stu-id="f8692-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="f8692-789">*Aggiunta indirizzo IP del Client*</span><span class="sxs-lookup"><span data-stu-id="f8692-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="f8692-790">Una volta il **indirizzo IP del Client** viene aggiunto a indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="f8692-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confermare le modifiche](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="f8692-792">*Confermare le modifiche*</span><span class="sxs-lookup"><span data-stu-id="f8692-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f8692-793">Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="f8692-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="f8692-794">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f8692-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="f8692-795">Nel **Esplora soluzioni**, fare clic sul progetto sito web e selezionare **Publish**.</span><span class="sxs-lookup"><span data-stu-id="f8692-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="f8692-796">![Pubblicazione dell'applicazione](aspnet-mvc-4-fundamentals/_static/image60.png "pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="f8692-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="f8692-797">*Pubblicazione del sito web*</span><span class="sxs-lookup"><span data-stu-id="f8692-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="f8692-798">Importare il profilo di pubblicazione che è stato salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="f8692-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="f8692-799">![L'importazione del profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image61.png "l'importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="f8692-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="f8692-800">*Importazione del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="f8692-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="f8692-801">Fare clic su **convalidare la connessione**.</span><span class="sxs-lookup"><span data-stu-id="f8692-801">Click **Validate Connection**.</span></span> <span data-ttu-id="f8692-802">Dopo aver completata la convalida fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="f8692-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8692-803">La convalida è stata completata una volta che visualizzato un segno di spunta verde accanto al pulsante convalida connessione.</span><span class="sxs-lookup"><span data-stu-id="f8692-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="f8692-804">![La convalida connessione](aspnet-mvc-4-fundamentals/_static/image62.png "convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="f8692-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="f8692-805">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="f8692-805">*Validating connection*</span></span>
4. <span data-ttu-id="f8692-806">Nel **impostazioni** nella pagina il **database** sezione, fare clic sul pulsante accanto alla casella di testo della connessione di database (vale a dire **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="f8692-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="f8692-807">![Configurazione della distribuzione Web](aspnet-mvc-4-fundamentals/_static/image63.png "configurazione della distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="f8692-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="f8692-808">*Configurazione della distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="f8692-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="f8692-809">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="f8692-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="f8692-810">Nel **nome Server** digitare l'URL server di Database SQL tramite il *tcp:* prefisso.</span><span class="sxs-lookup"><span data-stu-id="f8692-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="f8692-811">Nelle **nome utente** digitare il nome dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="f8692-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="f8692-812">Nelle **Password** digitare la password dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="f8692-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="f8692-813">Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="f8692-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="f8692-814">![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-fundamentals/_static/image64.png "configurazione stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="f8692-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="f8692-815">*Configurazione di stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="f8692-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="f8692-816">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8692-816">Then click **OK**.</span></span> <span data-ttu-id="f8692-817">Quando viene richiesto di creare il database, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f8692-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="f8692-818">![Creazione del database](aspnet-mvc-4-fundamentals/_static/image65.png "creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="f8692-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="f8692-819">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="f8692-819">*Creating the database*</span></span>
7. <span data-ttu-id="f8692-820">La stringa di connessione che si userà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di connessione predefinita nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="f8692-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="f8692-821">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f8692-821">Then click **Next**.</span></span>

    <span data-ttu-id="f8692-822">![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-fundamentals/_static/image66.png "stringa di connessione che punta al Database SQL")</span><span class="sxs-lookup"><span data-stu-id="f8692-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="f8692-823">*Stringa di connessione che punta al Database SQL*</span><span class="sxs-lookup"><span data-stu-id="f8692-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="f8692-824">Nel **Preview** pagina, fare clic su **Publish**.</span><span class="sxs-lookup"><span data-stu-id="f8692-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="f8692-825">![Pubblicazione dell'applicazione web](aspnet-mvc-4-fundamentals/_static/image67.png "pubblicazione dell'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="f8692-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="f8692-826">*Pubblicazione dell'applicazione web*</span><span class="sxs-lookup"><span data-stu-id="f8692-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="f8692-827">Al termine del processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="f8692-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="f8692-828">![Applicazione pubblicata in Azure](aspnet-mvc-4-fundamentals/_static/image68.png "applicazione pubblicata in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="f8692-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="f8692-829">*Pubblicata dell'applicazione in Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="f8692-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="f8692-830">Appendice c: uso dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="f8692-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="f8692-831">Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione.</span><span class="sxs-lookup"><span data-stu-id="f8692-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="f8692-832">Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="f8692-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="f8692-833">![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-fundamentals/_static/image69.png "frammenti di codice con Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="f8692-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="f8692-834">*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="f8692-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="f8692-835">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="f8692-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="f8692-836">Posizionare il cursore in cui si vuole inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="f8692-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="f8692-837">Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="f8692-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="f8692-838">Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="f8692-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="f8692-839">Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="f8692-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="f8692-840">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="f8692-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="f8692-841">![Iniziare a digitare il nome di frammento](aspnet-mvc-4-fundamentals/_static/image70.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="f8692-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="f8692-842">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="f8692-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="f8692-843">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-fundamentals/_static/image71.png "premere Tab per selezionare il frammento di codice evidenziata")</span><span class="sxs-lookup"><span data-stu-id="f8692-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="f8692-844">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="f8692-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="f8692-845">![Il frammento di codice e premere nuovamente Tab espanderà](aspnet-mvc-4-fundamentals/_static/image72.png "si espanderà il frammento di codice e premere nuovamente Tab")</span><span class="sxs-lookup"><span data-stu-id="f8692-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="f8692-846">*Il frammento di codice e premere nuovamente Tab espanderà*</span><span class="sxs-lookup"><span data-stu-id="f8692-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="f8692-847">***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="f8692-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="f8692-848">Pulsante destro del mouse in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="f8692-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="f8692-849">Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="f8692-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="f8692-850">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="f8692-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="f8692-851">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](aspnet-mvc-4-fundamentals/_static/image73.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="f8692-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="f8692-852">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="f8692-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="f8692-853">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-fundamentals/_static/image74.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")</span><span class="sxs-lookup"><span data-stu-id="f8692-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="f8692-854">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*</span><span class="sxs-lookup"><span data-stu-id="f8692-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
