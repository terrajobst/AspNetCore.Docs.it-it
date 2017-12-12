---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratorio pratico: Un ASP.NET: l''integrazione di Web Form ASP.NET, MVC e Web API | Documenti Microsoft'
author: rick-anderson
description: "ASP.NET è un framework per la compilazione di siti Web, applicazioni e servizi con le tecnologie specializzate, ad esempio MVC, API Web e altri. Con l'espansione h ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="f9153-104">Laboratorio pratico: Un ASP.NET: l'integrazione di Web Form ASP.NET, MVC e Web API</span><span class="sxs-lookup"><span data-stu-id="f9153-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="f9153-105">da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="f9153-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="f9153-106">Download Web categorie Kit di formazione</span><span class="sxs-lookup"><span data-stu-id="f9153-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="f9153-107">ASP.NET è un framework per la compilazione di siti Web, applicazioni e servizi con le tecnologie specializzate, ad esempio MVC, API Web e altri.</span><span class="sxs-lookup"><span data-stu-id="f9153-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="f9153-108">Con l'espansione ASP.NET ha rilevato dopo la creazione e l'espresso necessario disporre di queste tecnologie integrate, sono state eseguite attività di recente in favore di **ASP.NET uno**.</span><span class="sxs-lookup"><span data-stu-id="f9153-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="f9153-109">Visual Studio 2013 introduce un nuovo sistema di progetto unificato che consente di compilare un'applicazione e utilizzare tutte le tecnologie ASP.NET in un unico progetto.</span><span class="sxs-lookup"><span data-stu-id="f9153-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="f9153-110">Questa funzionalità Elimina la necessità di scegliere una tecnologia all'inizio di un progetto e stick con esso e invece promuove l'uso di diversi framework ASP.NET all'interno di un progetto.</span><span class="sxs-lookup"><span data-stu-id="f9153-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="f9153-111">Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="f9153-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="f9153-112">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f9153-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="f9153-113">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="f9153-113">Objectives</span></span>

<span data-ttu-id="f9153-114">In questa esercitazione pratica, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="f9153-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="f9153-115">Creare un sito Web basato sul **ASP.NET uno** tipo di progetto</span><span class="sxs-lookup"><span data-stu-id="f9153-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="f9153-116">Utilizzare diversi **ASP.NET** Framework come **MVC** e **API Web** nello stesso progetto</span><span class="sxs-lookup"><span data-stu-id="f9153-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="f9153-117">Identificare i componenti principali di un **ASP.NET** applicazione</span><span class="sxs-lookup"><span data-stu-id="f9153-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="f9153-118">Sfruttare il **lo Scaffolding di ASP.NET** framework per creare automaticamente i controller e visualizzazioni per eseguire operazioni CRUD basato su classi modello</span><span class="sxs-lookup"><span data-stu-id="f9153-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="f9153-119">Esporre lo stesso set di informazioni in formati macchina e leggibile utilizzando lo strumento appropriato per ogni processo</span><span class="sxs-lookup"><span data-stu-id="f9153-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="f9153-120">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f9153-120">Prerequisites</span></span>

<span data-ttu-id="f9153-121">Di seguito è necessario per completare questa esercitazione pratica:</span><span class="sxs-lookup"><span data-stu-id="f9153-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="f9153-122">[Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="f9153-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="f9153-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="f9153-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="f9153-124">Configurazione</span><span class="sxs-lookup"><span data-stu-id="f9153-124">Setup</span></span>

<span data-ttu-id="f9153-125">Per eseguire gli esercizi di questa esercitazione pratica, è necessario configurare prima di tutto l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="f9153-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="f9153-126">Aprire Esplora risorse e passare nel lab **origine** cartella.</span><span class="sxs-lookup"><span data-stu-id="f9153-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="f9153-127">Fare clic su **Setup.cmd** e selezionare **Esegui come amministratore** per avviare il processo di installazione che la configurazione dell'ambiente e installare i frammenti di codice di Visual Studio per questo ambiente lab.</span><span class="sxs-lookup"><span data-stu-id="f9153-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="f9153-128">Se viene visualizzata la finestra di dialogo controllo dell'Account utente, confermare l'azione per continuare.</span><span class="sxs-lookup"><span data-stu-id="f9153-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="f9153-129">Assicurarsi di avere selezionato tutte le dipendenze per questa esercitazione prima di eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="f9153-130">Utilizzo dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="f9153-130">Using the Code Snippets</span></span>

<span data-ttu-id="f9153-131">In tutto il documento lab, verrà invitati a inserire i blocchi di codice.</span><span class="sxs-lookup"><span data-stu-id="f9153-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="f9153-132">Per praticità, la maggior parte di questo codice viene fornito come Visual Studio frammenti di codice che è possibile accedere da all'interno di Visual Studio 2013 per evitare di dover aggiungerla manualmente.</span><span class="sxs-lookup"><span data-stu-id="f9153-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="f9153-133">Ogni esercizio è accompagnata da una soluzione inizia all'interno di **iniziare** cartella dell'esercizio che consente di seguire ogni esercizio indipendentemente dalle altre.</span><span class="sxs-lookup"><span data-stu-id="f9153-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="f9153-134">Tenere presente che i frammenti di codice aggiunti durante un esercizio non sono presenti queste soluzioni di avvio e potrebbero non funzionare fino a completare l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="f9153-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="f9153-135">All'interno del codice sorgente per un esercizio, si avrà inoltre un **fine** cartella che contiene una soluzione di Visual Studio con il codice generato da completare i passaggi nell'esercizio corrispondente.</span><span class="sxs-lookup"><span data-stu-id="f9153-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="f9153-136">Se è necessario ulteriore assistenza durante l'utilizzo tramite questa esercitazione pratica, è possibile usare queste soluzioni come guida.</span><span class="sxs-lookup"><span data-stu-id="f9153-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="f9153-137">Esercizi</span><span class="sxs-lookup"><span data-stu-id="f9153-137">Exercises</span></span>

<span data-ttu-id="f9153-138">Questa esercitazione include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f9153-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="f9153-139">Creazione di un nuovo progetto di Web Form</span><span class="sxs-lookup"><span data-stu-id="f9153-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="f9153-140">Creazione di un Controller MVC con Scaffolding</span><span class="sxs-lookup"><span data-stu-id="f9153-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="f9153-141">Creazione di un Controller API Web mediante Scaffolding</span><span class="sxs-lookup"><span data-stu-id="f9153-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="f9153-142">Tempo stimato per completare questa esercitazione: **60 minuti**</span><span class="sxs-lookup"><span data-stu-id="f9153-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="f9153-143">Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="f9153-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="f9153-144">Ogni raccolta predefinita è progettato in modo che corrisponda a un particolare stile di sviluppo e determina il layout di finestra, il comportamento dell'editor, frammenti di codice IntelliSense e finestre di dialogo.</span><span class="sxs-lookup"><span data-stu-id="f9153-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="f9153-145">Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si utilizza il **impostazioni generali per lo sviluppo** insieme.</span><span class="sxs-lookup"><span data-stu-id="f9153-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="f9153-146">Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario prendere in considerazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="f9153-147">Esercizio 1: Creazione di un nuovo progetto di Web Form</span><span class="sxs-lookup"><span data-stu-id="f9153-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="f9153-148">In questo esercizio si creerà un nuovo sito Web Form in Visual Studio 2013 con il **ASP.NET uno** unificata esperienza di progetto, che consente di integrare facilmente i componenti di Web Form, MVC e Web API nella stessa applicazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="f9153-149">Verrà quindi esplorare la soluzione generata e identificare le parti e infine viene visualizzato il sito Web in azione.</span><span class="sxs-lookup"><span data-stu-id="f9153-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="f9153-150">Attività 1: creazione di un nuovo sito usando un'esperienza di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f9153-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="f9153-151">In questa attività si inizierà creando un nuovo sito Web in Visual Studio in base il **ASP.NET uno** tipo di progetto.</span><span class="sxs-lookup"><span data-stu-id="f9153-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="f9153-152">**Uno ASP.NET** unifica tutte le tecnologie di ASP.NET e offre la possibilità di combinare e associarli in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="f9153-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="f9153-153">È quindi riconoscerà i vari componenti di Web Form, MVC e Web API live side-by-side all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="f9153-154">Aprire **Visual Studio Express 2013 per Web** e selezionare **File | Nuovo progetto...**  per avviare una nuova soluzione.</span><span class="sxs-lookup"><span data-stu-id="f9153-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Creazione di un nuovo progetto](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="f9153-156">*Crea un nuovo progetto*</span><span class="sxs-lookup"><span data-stu-id="f9153-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="f9153-157">Nel **nuovo progetto** nella finestra di dialogo **applicazione Web ASP.NET** sotto il **Visual c# | Web** scheda e accertarsi che **.NET Framework 4.5** è selezionata.</span><span class="sxs-lookup"><span data-stu-id="f9153-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="f9153-158">Denominare il progetto *MyHybridSite*, scegliere un **percorso** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9153-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Nuovo progetto applicazione Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="f9153-160">*Crea un nuovo progetto applicazione Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="f9153-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="f9153-161">Nel **nuovo progetto ASP.NET** la finestra di dialogo, seleziona il **Web Form** modello e selezionare il **MVC** e **API Web** opzioni.</span><span class="sxs-lookup"><span data-stu-id="f9153-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="f9153-162">Inoltre, assicurarsi che il **autenticazione** opzione è impostata su **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="f9153-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="f9153-163">Fare clic su **OK** per continuare.</span><span class="sxs-lookup"><span data-stu-id="f9153-163">Click **OK** to continue.</span></span>

    ![Crea un nuovo progetto con il modello Web Form, inclusi i componenti Web API e MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="f9153-165">*Crea un nuovo progetto con il modello Web Form, inclusi i componenti Web API e MVC*</span><span class="sxs-lookup"><span data-stu-id="f9153-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="f9153-166">È ora possibile esplorare la struttura della soluzione generata.</span><span class="sxs-lookup"><span data-stu-id="f9153-166">You can now explore the structure of the generated solution.</span></span>

    ![Esplorazione della soluzione generata](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="f9153-168">*Esplorazione della soluzione generata*</span><span class="sxs-lookup"><span data-stu-id="f9153-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="f9153-169">**Account:** questa cartella contiene le pagine Web Form per registrare, accedere a e gestire gli account utente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="f9153-170">Questa cartella viene aggiunta quando le **singoli account utente di** è selezionata l'opzione di autenticazione durante la configurazione del modello di progetto di Web Form.</span><span class="sxs-lookup"><span data-stu-id="f9153-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="f9153-171">**Modelli:** questa cartella contiene le classi che rappresentano i dati dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="f9153-172">**Controller** e **viste**: sono necessarie per queste cartelle di **ASP.NET MVC** e **ASP.NET Web API** componenti.</span><span class="sxs-lookup"><span data-stu-id="f9153-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="f9153-173">Analizzerà le tecnologie MVC e Web API negli esercizi.</span><span class="sxs-lookup"><span data-stu-id="f9153-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="f9153-174">Il **Default.aspx**, **Contact.aspx** e **About** file sono pagine Web Form predefinite che è possibile utilizzare come punto di partenza per compilare le pagine specifiche per il applicazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="f9153-175">La logica di programmazione di tali file si trova in un file separato, detto il &quot;codice&quot; file, con un &quot;. aspx&quot; o &quot;. aspx.cs&quot; estensione (a seconda di linguaggio utilizzato).</span><span class="sxs-lookup"><span data-stu-id="f9153-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="f9153-176">La logica del codice viene eseguito nel server e in modo dinamico produce l'output HTML per la pagina.</span><span class="sxs-lookup"><span data-stu-id="f9153-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="f9153-177">Il **Site. master** e **Site.Mobile.Master** pagine definiscono l'aspetto e il comportamento standard di tutte le pagine nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="f9153-178">Fare doppio clic su di **Default.aspx** file per esplorare il contenuto della pagina.</span><span class="sxs-lookup"><span data-stu-id="f9153-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Esplorazione della pagina default.](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="f9153-180">*Esplorazione della pagina default.*</span><span class="sxs-lookup"><span data-stu-id="f9153-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f9153-181">Il **pagina** direttiva all'inizio del file definisce gli attributi della pagina Web Form.</span><span class="sxs-lookup"><span data-stu-id="f9153-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="f9153-182">Ad esempio, il **MasterPageFile** attributo specifica il percorso al master pagina - in questo caso, il *Site. master* pagina e **Inherits** attributo definisce il classe code-behind per la pagina da cui ereditare.</span><span class="sxs-lookup"><span data-stu-id="f9153-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="f9153-183">Questa classe si trova nel file di base di **CodeBehind** attributo.</span><span class="sxs-lookup"><span data-stu-id="f9153-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="f9153-184">Il **asp: Content** controllo contiene il contenuto effettivo della pagina (testo, markup e controlli) e viene eseguito il mapping a un **asp: ContentPlaceHolder** controllo nella pagina master.</span><span class="sxs-lookup"><span data-stu-id="f9153-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="f9153-185">In questo caso, il contenuto della pagina eseguito all'interno di *MainContent* controllo definite nel *Site. master* pagina.</span><span class="sxs-lookup"><span data-stu-id="f9153-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="f9153-186">Espandere il **App\_avviare** cartella e notare il **WebApiConfig.cs** file.</span><span class="sxs-lookup"><span data-stu-id="f9153-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="f9153-187">Visual Studio inclusi i file nella soluzione generata perché inclusa l'API Web quando si configura il progetto con il modello ASP.NET uno.</span><span class="sxs-lookup"><span data-stu-id="f9153-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="f9153-188">Aprire il **WebApiConfig.cs** file.</span><span class="sxs-lookup"><span data-stu-id="f9153-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="f9153-189">Nel *WebApiConfig* si noterà che la configurazione associata all'API Web, che esegue il mapping HTTP classe indirizza a **controller API Web**.</span><span class="sxs-lookup"><span data-stu-id="f9153-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="f9153-190">Aprire il **RouteConfig.cs** file.</span><span class="sxs-lookup"><span data-stu-id="f9153-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="f9153-191">All'interno di *RegisterRoutes* metodo si noterà che la configurazione associata a MVC, che esegue il mapping delle route HTTP a **controller MVC**.</span><span class="sxs-lookup"><span data-stu-id="f9153-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="f9153-192">Attività 2: esecuzione della soluzione</span><span class="sxs-lookup"><span data-stu-id="f9153-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="f9153-193">In questa attività verranno eseguire la soluzione generata, esplorare l'app e alcune caratteristiche, ad esempio la riscrittura degli URL e l'autenticazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f9153-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="f9153-194">Per eseguire la soluzione, premere **F5** oppure fare clic su di **avviare** pulsante Trova sulla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="f9153-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="f9153-195">La home page dell'applicazione deve aprire nel browser.</span><span class="sxs-lookup"><span data-stu-id="f9153-195">The application home page should open in the browser.</span></span>

    ![Esecuzione della soluzione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="f9153-197">Verificare che le pagine Web Form vengano richiamate.</span><span class="sxs-lookup"><span data-stu-id="f9153-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="f9153-198">A tale scopo, aggiungere **/contact.aspx** per l'URL nella barra degli indirizzi e premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="f9153-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![URL brevi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="f9153-200">*URL brevi*</span><span class="sxs-lookup"><span data-stu-id="f9153-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f9153-201">Come si può notare, l'URL viene modificato per **o contattare**.</span><span class="sxs-lookup"><span data-stu-id="f9153-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="f9153-202">A partire da **ASP.NET 4**, funzionalità di routing di URL sono stati aggiunti a un Web Form, pertanto è possibile scrivere come URL  *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)*  anziché  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="f9153-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="f9153-203">Per ulteriori informazioni fare riferimento a [Routing degli URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="f9153-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="f9153-204">Verrà ora illustrato il flusso di autenticazione integrato nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="f9153-205">A tale scopo, fare clic su **registrare** nell'angolo superiore destro della pagina.</span><span class="sxs-lookup"><span data-stu-id="f9153-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Registrazione di un nuovo utente](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="f9153-207">*Registrazione di un nuovo utente*</span><span class="sxs-lookup"><span data-stu-id="f9153-207">*Registering a new user*</span></span>
4. <span data-ttu-id="f9153-208">Nel **registrare** pagina, immettere un **nome utente** e **Password**, quindi fare clic su **registrare**.</span><span class="sxs-lookup"><span data-stu-id="f9153-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Pagina di registrazione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="f9153-210">*Pagina di registrazione*</span><span class="sxs-lookup"><span data-stu-id="f9153-210">*Register page*</span></span>
5. <span data-ttu-id="f9153-211">L'applicazione Registra nuovo account e l'autenticazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="f9153-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Utente autenticato](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="f9153-213">*Utente autenticato*</span><span class="sxs-lookup"><span data-stu-id="f9153-213">*User authenticated*</span></span>
6. <span data-ttu-id="f9153-214">Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="f9153-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="f9153-215">Esercizio 2: Creazione di un Controller MVC con Scaffolding</span><span class="sxs-lookup"><span data-stu-id="f9153-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="f9153-216">In questo esercizio si sfrutterà il framework di ASP.NET Scaffolding fornito da Visual Studio per creare un controller di ASP.NET MVC 5 con azioni e visualizzazioni Razor per eseguire operazioni CRUD, senza scrivere una singola riga di codice.</span><span class="sxs-lookup"><span data-stu-id="f9153-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="f9153-217">Il processo di scaffolding userà Code First di Entity Framework per generare il contesto dei dati e lo schema del database nel database SQL.</span><span class="sxs-lookup"><span data-stu-id="f9153-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="f9153-218">**Su Entity Framework Code prima**</span><span class="sxs-lookup"><span data-stu-id="f9153-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="f9153-219">Entity Framework (EF) è un mapping relazionale a oggetti (ORM) che consente di creare applicazioni di accesso ai dati tramite programmazione con un modello di applicazione concettuale anziché programmando direttamente con uno schema di archiviazione relazionale.</span><span class="sxs-lookup"><span data-stu-id="f9153-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="f9153-220">Il flusso di lavoro modello Code First di Entity Framework consente di utilizzare le classi di dominio per rappresentare il modello di Entity Framework si basa su quando si esegue la query, le funzioni di rilevamento delle modifiche e l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="f9153-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="f9153-221">Utilizza il flusso di lavoro di sviluppo Code First, non necessario avviare l'applicazione mediante la creazione di un database o specificare uno schema.</span><span class="sxs-lookup"><span data-stu-id="f9153-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="f9153-222">In alternativa, è possibile scrivere standard .NET le classi che definiscono gli oggetti modello di dominio più appropriati per l'applicazione ed Entity Framework consente di creare il database per l'utente.</span><span class="sxs-lookup"><span data-stu-id="f9153-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="f9153-223">Maggiori informazioni su Entity Framework [qui](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="f9153-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="f9153-224">Attività 1: creazione di un nuovo modello</span><span class="sxs-lookup"><span data-stu-id="f9153-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="f9153-225">Ora si definirà un **persona** (classe), che sarà il modello utilizzato dal processo di scaffolding per creare il controller MVC e le viste.</span><span class="sxs-lookup"><span data-stu-id="f9153-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="f9153-226">Iniziare creando un **persona** classe di modello e le operazioni CRUD nel controller di verranno automaticamente creata utilizzando le funzionalità di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f9153-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="f9153-227">Aprire **Visual Studio Express 2013 per Web** e **MyHybridSite.sln** soluzione si trova nel **origine/Ex2-MvcScaffolding/inizio** cartella.</span><span class="sxs-lookup"><span data-stu-id="f9153-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="f9153-228">In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="f9153-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="f9153-229">In **Esplora**, fare doppio clic su di **modelli** cartella del **MyHybridSite** del progetto e selezionare **Aggiungi | Classe...** .</span><span class="sxs-lookup"><span data-stu-id="f9153-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Aggiunta di una classe modello persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="f9153-231">*Aggiunta di una classe modello persona*</span><span class="sxs-lookup"><span data-stu-id="f9153-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="f9153-232">Nel **Aggiungi nuovo elemento** al file il nome nella finestra di dialogo *Person* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f9153-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Creazione della classe di modello di persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="f9153-234">*Creazione della classe di modello di persona*</span><span class="sxs-lookup"><span data-stu-id="f9153-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="f9153-235">Sostituire il contenuto del **Person** file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="f9153-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="f9153-236">Premere **CTRL + S** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="f9153-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="f9153-237">(- Frammento di codice *PersonClass BringingTogetherOneAspNet - Ex2 -*)</span><span class="sxs-lookup"><span data-stu-id="f9153-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="f9153-238">In **Esplora**, fare doppio clic su di **MyHybridSite** progetto e selezionare **compilare**, oppure premere **CTRL + MAIUSC + B** per compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="f9153-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="f9153-239">Attività 2: creazione di un Controller MVC</span><span class="sxs-lookup"><span data-stu-id="f9153-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="f9153-240">Ora che il **persona** creazione del modello, si utilizzerà lo scaffolding di ASP.NET MVC con Entity Framework per creare le azioni del controller CRUD e le viste per **persona**.</span><span class="sxs-lookup"><span data-stu-id="f9153-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="f9153-241">In **Esplora**, fare doppio clic sul **controller** cartella del **MyHybridSite** del progetto e selezionare **Aggiungi | Nuovo elemento di scaffolding...** .</span><span class="sxs-lookup"><span data-stu-id="f9153-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Creare un nuovo Controller di scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="f9153-243">*Creare un nuovo Controller di scaffolding*</span><span class="sxs-lookup"><span data-stu-id="f9153-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="f9153-244">Nel **aggiungere lo scaffolding** nella finestra di dialogo **Controller MVC 5 con visualizzazioni, mediante Entity Framework** e quindi fare clic su **Aggiungi.**</span><span class="sxs-lookup"><span data-stu-id="f9153-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Selezione di Controller MVC 5 con visualizzazioni ed Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="f9153-246">*Selezione di Controller MVC 5 con visualizzazioni ed Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="f9153-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="f9153-247">Impostare *MvcPersonController* come il **nome Controller**, selezionare il **utilizzare azioni asincrone del controller** opzione e selezionare **persona (MyHybridSite.Models)**  come il **classe modello**.</span><span class="sxs-lookup"><span data-stu-id="f9153-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Aggiunta di un controller MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="f9153-249">*Aggiunta di un controller MVC con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="f9153-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="f9153-250">In **classe contesto dati**, fare clic su **nuovo contesto dati...** .</span><span class="sxs-lookup"><span data-stu-id="f9153-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Creazione di un nuovo contesto dati](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="f9153-252">*Creazione di un nuovo contesto dati*</span><span class="sxs-lookup"><span data-stu-id="f9153-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="f9153-253">Nel **nuovo contesto dati** nella finestra di dialogo Nome nuovo contesto dati *PersonContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f9153-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Creare il nuovo PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="f9153-255">*La creazione del nuovo tipo PersonContext*</span><span class="sxs-lookup"><span data-stu-id="f9153-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="f9153-256">Fare clic su **Aggiungi** per creare il nuovo controller per **persona** con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f9153-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="f9153-257">Visual Studio genera quindi le azioni del controller, il contesto dei dati utente e le visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="f9153-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Dopo la creazione di controller MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="f9153-259">*Dopo la creazione di controller MVC con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="f9153-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="f9153-260">Aprire il **MvcPersonController.cs** file nel **controller** cartella.</span><span class="sxs-lookup"><span data-stu-id="f9153-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="f9153-261">Si noti che i metodi di azione CRUD siano stati generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f9153-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="f9153-262">Selezionando il **utilizzare azioni asincrone del controller** casella di controllo dallo scaffolding opzioni nei passaggi precedenti, Visual Studio genera metodi di azione asincroni per tutte le azioni che comportano l'accesso per il contesto dei dati utente.</span><span class="sxs-lookup"><span data-stu-id="f9153-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="f9153-263">Si consiglia di utilizzare metodi di azione asincroni per l'esecuzione prolungata, non associate alla CPU richieste per evitare di bloccare il server Web di esecuzione del lavoro durante l'elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="f9153-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="f9153-264">Attività 3: esecuzione della soluzione</span><span class="sxs-lookup"><span data-stu-id="f9153-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="f9153-265">In questa attività verrà eseguita la soluzione per verificare che le viste per **persona** funzionino come previsto.</span><span class="sxs-lookup"><span data-stu-id="f9153-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="f9153-266">Si aggiungerà un nuovo utente per verificare che è stata salvata nel database.</span><span class="sxs-lookup"><span data-stu-id="f9153-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="f9153-267">Premere **F5** per eseguire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="f9153-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="f9153-268">Passare a **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="f9153-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="f9153-269">La visualizzazione di scaffolding che mostra l'elenco degli utenti dovrebbe essere visualizzato.</span><span class="sxs-lookup"><span data-stu-id="f9153-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="f9153-270">Fare clic su **Crea nuovo** per aggiungere una nuova persona.</span><span class="sxs-lookup"><span data-stu-id="f9153-270">Click **Create New** to add a new person.</span></span>

    ![Passare alle visualizzazioni MVC scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="f9153-272">*Passare alle visualizzazioni MVC scaffolding*</span><span class="sxs-lookup"><span data-stu-id="f9153-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="f9153-273">Nel **crea** visualizzare, fornire un **nome** e un **Age** per utente, fare clic su **crea**.</span><span class="sxs-lookup"><span data-stu-id="f9153-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Aggiunta di una nuova persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="f9153-275">*Aggiunta di una nuova persona*</span><span class="sxs-lookup"><span data-stu-id="f9153-275">*Adding a new person*</span></span>
5. <span data-ttu-id="f9153-276">Il nuovo utente viene aggiunto all'elenco.</span><span class="sxs-lookup"><span data-stu-id="f9153-276">The new person is added to the list.</span></span> <span data-ttu-id="f9153-277">Nell'elenco degli elementi, fare clic su **dettagli** per visualizzare i dettagli della persona.</span><span class="sxs-lookup"><span data-stu-id="f9153-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="f9153-278">Quindi, nel **dettagli** consente di visualizzare, fare clic su **elenco** per tornare alla visualizzazione elenco.</span><span class="sxs-lookup"><span data-stu-id="f9153-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Visualizzazione dei dettagli dell'utente](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="f9153-280">*Visualizzazione dei dettagli dell'utente*</span><span class="sxs-lookup"><span data-stu-id="f9153-280">*Person's details view*</span></span>
6. <span data-ttu-id="f9153-281">Fare clic su di **eliminare** collegamento per eliminare l'utente.</span><span class="sxs-lookup"><span data-stu-id="f9153-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="f9153-282">Nel **eliminare** consente di visualizzare, fare clic su **eliminare** per confermare l'operazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![L'eliminazione di un utente](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="f9153-284">*L'eliminazione di un utente*</span><span class="sxs-lookup"><span data-stu-id="f9153-284">*Deleting a person*</span></span>
7. <span data-ttu-id="f9153-285">Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="f9153-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="f9153-286">Esercizio 3: Creazione di un Controller API Web mediante Scaffolding</span><span class="sxs-lookup"><span data-stu-id="f9153-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="f9153-287">Framework Web API fa parte dello Stack di ASP.NET e progettato per semplificare l'implementazione di servizi HTTP, in genere l'invio e ricezione di dati in formato JSON o XML tramite un'API RESTful.</span><span class="sxs-lookup"><span data-stu-id="f9153-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="f9153-288">In questo esercizio, si utilizzerà lo Scaffolding di ASP.NET nuovamente per generare un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="f9153-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="f9153-289">Si utilizzerà lo stesso **persona** e **PersonContext** classi nell'esercizio precedente per fornire gli stessi dati utente in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="f9153-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="f9153-290">Verrà visualizzato come è possibile esporre le stesse risorse in modi diversi nella stessa applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f9153-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="f9153-291">Attività 1: creazione di un Controller API Web</span><span class="sxs-lookup"><span data-stu-id="f9153-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="f9153-292">In questa attività verrà creato un nuovo **Controller API Web** che esporrà i dati utente in un formato macchina utilizzabili come JSON.</span><span class="sxs-lookup"><span data-stu-id="f9153-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="f9153-293">Se non è già aperto, aprirlo **Visual Studio Express 2013 per Web** e aprire il **MyHybridSite.sln** soluzione si trova nel **origine/Ex3-WebAPI/inizio** cartella.</span><span class="sxs-lookup"><span data-stu-id="f9153-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="f9153-294">In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="f9153-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f9153-295">Se si inizia con la soluzione Begin da esercizio 3, premere **CTRL + MAIUSC + B** per compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="f9153-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="f9153-296">In **Esplora**, fare doppio clic sul **controller** cartella del **MyHybridSite** del progetto e selezionare **Aggiungi | Nuovo elemento di scaffolding...** .</span><span class="sxs-lookup"><span data-stu-id="f9153-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Creare un nuovo Controller di scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="f9153-298">*Creare un nuovo Controller di scaffolding*</span><span class="sxs-lookup"><span data-stu-id="f9153-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="f9153-299">Nel **aggiungere lo scaffolding** nella finestra di dialogo **API Web** nel riquadro a sinistra, quindi **Web 2 Controller API con azioni, mediante Entity Framework** nel riquadro centrale e quindi fare clic su  **Aggiungere.**</span><span class="sxs-lookup"><span data-stu-id="f9153-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="f9153-300">![Selezione di Controller di Web API 2 con azioni ed Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "la selezione di Controller API Web 2 con azioni ed Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="f9153-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="f9153-301">*Selezione di Controller di Web API 2 con azioni ed Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="f9153-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="f9153-302">Impostare *ApiPersonController* come il **nome Controller**, selezionare il **utilizzare azioni asincrone del controller** opzione e selezionare **persona (MyHybridSite.Models)**  e **PersonContext (MyHybridSite.Models)** come il **modello** e **contesto dati** classi rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="f9153-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="f9153-303">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f9153-303">Then click **Add**.</span></span>

    <span data-ttu-id="f9153-304">![Aggiunta di un Controller API Web con lo scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "aggiunta di un controller API Web con lo scaffolding")</span><span class="sxs-lookup"><span data-stu-id="f9153-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="f9153-305">*Aggiunta di un controller API Web con lo scaffolding*</span><span class="sxs-lookup"><span data-stu-id="f9153-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="f9153-306">Visual Studio genera quindi il **ApiPersonController** classe con le quattro azioni CRUD per lavorare con i dati.</span><span class="sxs-lookup"><span data-stu-id="f9153-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="f9153-307">![Dopo aver creato il controller API Web con lo scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "dopo aver creato il controller API Web con lo scaffolding")</span><span class="sxs-lookup"><span data-stu-id="f9153-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="f9153-308">*Dopo aver creato il controller API Web con lo scaffolding*</span><span class="sxs-lookup"><span data-stu-id="f9153-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="f9153-309">Aprire il **ApiPersonController.cs** file ed esaminare il *GetPeople* metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="f9153-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="f9153-310">Questo metodo esegue una query il campo di database di **PersonContext** tipo per ottenere le persone di dati.</span><span class="sxs-lookup"><span data-stu-id="f9153-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="f9153-311">Osservare ora il commento sopra la definizione di metodo.</span><span class="sxs-lookup"><span data-stu-id="f9153-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="f9153-312">Fornisce l'URI che espone questa azione che verrà utilizzato nell'attività successiva.</span><span class="sxs-lookup"><span data-stu-id="f9153-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="f9153-313">Per impostazione predefinita, l'API Web è configurata per rilevare le query per il */api* percorso per evitare conflitti con i controller MVC.</span><span class="sxs-lookup"><span data-stu-id="f9153-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="f9153-314">Se è necessario modificare questa impostazione, fare riferimento a [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f9153-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="f9153-315">Attività 2: esecuzione della soluzione</span><span class="sxs-lookup"><span data-stu-id="f9153-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="f9153-316">In questa attività verrà utilizzato Internet Explorer **strumenti di sviluppo F12** per controllare l'intera risposta dal controller API Web.</span><span class="sxs-lookup"><span data-stu-id="f9153-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="f9153-317">Verrà visualizzato come è possibile acquisire il traffico di rete per ottenere informazioni più dettagliate sui dati dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="f9153-318">Assicurarsi che **Internet Explorer** è selezionata nel **avviare** pulsante Trova sulla barra degli strumenti di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f9153-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Opzione di Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="f9153-320">Il **strumenti di sviluppo F12** dispone di una vasta gamma di funzionalità non trattate in questa esercitazione pratica.</span><span class="sxs-lookup"><span data-stu-id="f9153-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="f9153-321">Se si desidera ottenere altre informazioni, vedere [utilizzando gli strumenti di sviluppo F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="f9153-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="f9153-322">Premere **F5** per eseguire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="f9153-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f9153-323">Per eseguire correttamente questa attività, l'applicazione deve disporre di dati.</span><span class="sxs-lookup"><span data-stu-id="f9153-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="f9153-324">Se il database è vuoto, è possibile tornare all'attività 3 nell'esercizio 2 e seguire i passaggi su come creare un nuovo utente utilizzando le visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="f9153-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="f9153-325">Nel browser, premere **F12** per aprire la **gli strumenti di sviluppo** pannello.</span><span class="sxs-lookup"><span data-stu-id="f9153-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="f9153-326">Premere **CTRL** + **4** oppure fare clic su di **rete** icona e quindi fare clic su pulsante freccia verde per avviare l'acquisizione del traffico di rete.</span><span class="sxs-lookup"><span data-stu-id="f9153-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="f9153-327">![Avvio di acquisizione di rete di API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "acquisizione di rete di avvio di Web API")</span><span class="sxs-lookup"><span data-stu-id="f9153-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="f9153-328">*Avvio di acquisizione di rete di API Web*</span><span class="sxs-lookup"><span data-stu-id="f9153-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="f9153-329">Aggiungere **api/ApiPerson** per l'URL nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="f9153-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="f9153-330">È ora esamina i dettagli della risposta di **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="f9153-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="f9153-331">![Il recupero dei dati utente tramite l'API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "il recupero dei dati utente tramite l'API Web")</span><span class="sxs-lookup"><span data-stu-id="f9153-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="f9153-332">*Il recupero dei dati utente tramite l'API Web*</span><span class="sxs-lookup"><span data-stu-id="f9153-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f9153-333">Una volta completato il download, verrà richiesto di effettuare un'azione con il file scaricato.</span><span class="sxs-lookup"><span data-stu-id="f9153-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="f9153-334">Lasciare la finestra di dialogo aperta per poter visualizzare il contenuto della risposta tramite la finestra degli strumenti sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="f9153-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="f9153-335">Ora si esamina il corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="f9153-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="f9153-336">A tale scopo, fare clic su di **dettagli** scheda e quindi fare clic su **corpo della risposta**.</span><span class="sxs-lookup"><span data-stu-id="f9153-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="f9153-337">È possibile verificare che i dati scaricati sono un elenco di oggetti con le proprietà **Id**, **nome** e **Age** che corrispondono al **persona** classe.</span><span class="sxs-lookup"><span data-stu-id="f9153-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="f9153-338">![La visualizzazione Web corpo della risposta API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "visualizzazione Web corpo della risposta API")</span><span class="sxs-lookup"><span data-stu-id="f9153-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="f9153-339">*Corpo della risposta di API Web visualizzazione*</span><span class="sxs-lookup"><span data-stu-id="f9153-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="f9153-340">Attività 3: aggiunta di pagine della Guida di API Web</span><span class="sxs-lookup"><span data-stu-id="f9153-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="f9153-341">Quando si crea un'API Web, è utile creare una pagina della Guida in modo che altri sviluppatori saranno in grado di chiamare l'API.</span><span class="sxs-lookup"><span data-stu-id="f9153-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="f9153-342">È possibile creare e aggiornare manualmente le pagine della documentazione, ma è preferibile generare automaticamente in modo da evitare di dover eseguire operazioni di manutenzione.</span><span class="sxs-lookup"><span data-stu-id="f9153-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="f9153-343">In questa attività si utilizzerà un pacchetto Nuget per generare automaticamente le pagine della Guida di API Web alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="f9153-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="f9153-344">Dal **strumenti** menu in Visual Studio, selezionare **Gestione pacchetti libreria**, quindi fare clic su **Console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="f9153-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="f9153-345">Nel **Package Manager Console** finestra, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f9153-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="f9153-346">Il **Microsoft.AspNet.WebApi.HelpPage** pacchetto installa gli assembly necessari e aggiunge le visualizzazioni MVC per le pagine della Guida nel **aree/HelpPage** cartella.</span><span class="sxs-lookup"><span data-stu-id="f9153-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="f9153-347">![Area HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span><span class="sxs-lookup"><span data-stu-id="f9153-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="f9153-348">*Area HelpPage*</span><span class="sxs-lookup"><span data-stu-id="f9153-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="f9153-349">Per impostazione predefinita, la Guida pagine sono stringhe di segnaposto per la documentazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="f9153-350">È possibile utilizzare i commenti della documentazione XML per creare la documentazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="f9153-351">Per abilitare questa funzionalità, aprire il **HelpPageConfig.cs** file si trova nella **HelpPage/aree/App\_avviare** cartella e rimuovere il commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="f9153-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="f9153-352">In **Esplora**, fare clic sul progetto **MyHybridSite**selezionare **proprietà** e fare clic su di **compilare** scheda.</span><span class="sxs-lookup"><span data-stu-id="f9153-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="f9153-353">![Scheda Compila](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "compilare sezione")</span><span class="sxs-lookup"><span data-stu-id="f9153-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="f9153-354">*Scheda Compila*</span><span class="sxs-lookup"><span data-stu-id="f9153-354">*Build tab*</span></span>
5. <span data-ttu-id="f9153-355">In **Output**selezionare **file di documentazione XML**.</span><span class="sxs-lookup"><span data-stu-id="f9153-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="f9153-356">Nella casella di modifica, digitare **App\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="f9153-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="f9153-357">![Output sezione nella scheda compilazione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "sezione nella scheda compilazione di Output")</span><span class="sxs-lookup"><span data-stu-id="f9153-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="f9153-358">*Sezione di output nella scheda compilazione*</span><span class="sxs-lookup"><span data-stu-id="f9153-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="f9153-359">Premere **CTRL** + **S** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="f9153-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="f9153-360">Aprire il **ApiPersonController.cs** file dal **controller** cartella.</span><span class="sxs-lookup"><span data-stu-id="f9153-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="f9153-361">Immettere una nuova riga tra il *GetPeople* firma del metodo e */ / ottenere api/ApiPerson* impostare come commento e quindi digitare tre barre.</span><span class="sxs-lookup"><span data-stu-id="f9153-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f9153-362">Visual Studio inserisce automaticamente gli elementi XML che definiscono la documentazione relativa al metodo.</span><span class="sxs-lookup"><span data-stu-id="f9153-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="f9153-363">Aggiungere un testo di riepilogo e il valore restituito per il *GetPeople* metodo.</span><span class="sxs-lookup"><span data-stu-id="f9153-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="f9153-364">Dovrebbe essere simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="f9153-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="f9153-365">Premere **F5** per eseguire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="f9153-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="f9153-366">Aggiungere **/Help** per l'URL nella barra degli indirizzi per passare alla pagina della Guida.</span><span class="sxs-lookup"><span data-stu-id="f9153-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="f9153-367">![Pagina della Guida ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "pagina della Guida ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="f9153-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="f9153-368">*Pagina della Guida ASP.NET Web API*</span><span class="sxs-lookup"><span data-stu-id="f9153-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f9153-369">Il principale contenuto della pagina è una tabella delle API, raggruppate dal controller.</span><span class="sxs-lookup"><span data-stu-id="f9153-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="f9153-370">Le voci della tabella vengono generate in modo dinamico, utilizzando il **IApiExplorer** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="f9153-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="f9153-371">Se si aggiunge o aggiorna un controller API, la tabella verrà automaticamente aggiornata la volta successiva che si compila l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f9153-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="f9153-372">Il **API** colonna elenca il metodo HTTP e un URI relativo.</span><span class="sxs-lookup"><span data-stu-id="f9153-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="f9153-373">Il **descrizione** colonna contiene informazioni che sono stato estratto dalla documentazione del metodo.</span><span class="sxs-lookup"><span data-stu-id="f9153-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="f9153-374">Si noti che la descrizione che aggiunti sopra la definizione di metodo viene visualizzata nella colonna Descrizione.</span><span class="sxs-lookup"><span data-stu-id="f9153-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="f9153-375">![Descrizione del metodo API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "descrizione del metodo API")</span><span class="sxs-lookup"><span data-stu-id="f9153-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="f9153-376">*Descrizione del metodo API*</span><span class="sxs-lookup"><span data-stu-id="f9153-376">*API method description*</span></span>
13. <span data-ttu-id="f9153-377">Fare clic su uno dei metodi API per passare a una pagina con informazioni più dettagliate, tra cui corpi di risposta di esempio.</span><span class="sxs-lookup"><span data-stu-id="f9153-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="f9153-378">![Pagina informazioni di dettaglio](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "pagina informazioni di dettaglio")</span><span class="sxs-lookup"><span data-stu-id="f9153-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="f9153-379">*Pagina informazioni dettagliate*</span><span class="sxs-lookup"><span data-stu-id="f9153-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="f9153-380">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f9153-380">Summary</span></span>

<span data-ttu-id="f9153-381">Completando questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="f9153-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="f9153-382">Creare una nuova applicazione Web utilizzando un'esperienza di ASP.NET in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f9153-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="f9153-383">Integrare più tecnologie ASP.NET in un unico progetto</span><span class="sxs-lookup"><span data-stu-id="f9153-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="f9153-384">Generare visualizzazioni di controller MVC e dalle classi modello mediante Scaffolding di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f9153-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="f9153-385">Generare controller API Web, utilizzare le funzionalità, ad esempio di programmazione asincrona e l'accesso ai dati tramite Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f9153-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="f9153-386">Generare automaticamente le pagine della Guida di API Web per i controller</span><span class="sxs-lookup"><span data-stu-id="f9153-386">Automatically generate Web API Help Pages for your controllers</span></span>
