---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Accesso ai dati e i modelli ASP.NET MVC 4 | Documenti Microsoft
author: rick-anderson
description: 'Nota: Questa pratica presuppone che la knowledge base di ASP.NET MVC. Se si utilizza ASP.NET MVC prima, ti consigliamo di andare su ASP.NET MVC 4...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 353419077422516761df56f730352b19b5db5ff2
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="e4bbb-104">Accesso ai dati e i modelli ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="e4bbb-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="e4bbb-105">Da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e4bbb-106">Download Web categorie Kit di formazione</span><span class="sxs-lookup"><span data-stu-id="e4bbb-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="e4bbb-107">Questa pratica presuppone avere conoscenze di base **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="e4bbb-108">Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **nozioni di base di ASP.NET MVC 4** le esercitazioni pratiche.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="e4bbb-109">Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza tramite l'applicazione di modifiche di lieve entità a un'applicazione Web di esempio fornita nella cartella di origine.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="e4bbb-110">Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [versioni Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="e4bbb-111">Il progetto specifico per questa esercitazione è disponibile all'indirizzo [accesso ai dati e i modelli di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="e4bbb-112">In **nozioni fondamentali su MVC ASP.NET** pratica, si hanno stato passando dati hardcoded verso i controller per i modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="e4bbb-113">Ma, per compilare un'applicazione Web reale, è possibile utilizzare un database reale.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="e4bbb-114">Questa pratica verrà descritto come utilizzare un motore di database per archiviare e recuperare i dati necessari per l'applicazione del Negozio.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="e4bbb-115">Per raggiungere questo obiettivo, si avvia con un database esistente e creare il modello di dati di entità da esso.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="e4bbb-116">In questo laboratorio soddisferà la **Database First** approccio, nonché **Code First** approccio.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="e4bbb-117">Tuttavia, è inoltre possibile utilizzare il **Model First** approccio, creare lo stesso modello utilizzando gli strumenti e quindi generare il database da esso.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="e4bbb-118">![Visual Studio prima di database. Prima del modello](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First Visual Studio. Primo modello")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="e4bbb-119">*Visual Studio prima di database. Primo modello*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="e4bbb-120">Dopo aver generato il modello, verranno apportate le modifiche appropriate nella StoreController per fornire le viste di archivio con i dati acquisiti dal database, invece di usare dati hardcoded.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="e4bbb-121">Non occorre di apportare qualsiasi modifica ai modelli di visualizzazione perché il StoreController verrà restituito il ViewModel stessa per i modelli di visualizzazione, anche se questo tempo i dati provengono dal database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="e4bbb-122">**Il primo approccio di codice**</span><span class="sxs-lookup"><span data-stu-id="e4bbb-122">**The Code First Approach**</span></span>

<span data-ttu-id="e4bbb-123">L'approccio Code First consente di definire il modello dal codice senza generare le classi che sono in genere associate con il framework.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="e4bbb-124">Nel codice in primo luogo, gli oggetti del modello sono definiti con POCOs, &quot;Plain Old CLR Object&quot;.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="e4bbb-125">POCOs sono semplici classi normale che non dispone di alcun tipo di ereditarietà e non implementano le interfacce.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="e4bbb-126">È possibile generare automaticamente il database da essi oppure è possibile utilizzare un database esistente e generare il mapping della classe dal codice.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="e4bbb-127">I vantaggi dell'utilizzo di questo approccio è che il modello rimane indipendente dal framework di persistenza (in questo caso, Entity Framework), come le classi POCOs non sono collegate con il framework di mapping.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="e4bbb-128">Questo Lab è basato su ASP.NET MVC 4 e una versione dell'applicazione di esempio musica archivio personalizzato e adatta solo le funzionalità illustrate in questo laboratorio pratico ridotta a icona.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="e4bbb-129">Se si desidera esplorare l'intero **negozio** applicazione di esercitazione sarà possibile trovarlo in [negozio di musica MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e4bbb-130">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e4bbb-130">Prerequisites</span></span>

<span data-ttu-id="e4bbb-131">È necessario che gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="e4bbb-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="e4bbb-132">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e4bbb-133">Configurazione</span><span class="sxs-lookup"><span data-stu-id="e4bbb-133">Setup</span></span>

<span data-ttu-id="e4bbb-134">**L'installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="e4bbb-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="e4bbb-135">Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="e4bbb-136">Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="e4bbb-137">Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: con i frammenti di codice](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e4bbb-138">Esercizi</span><span class="sxs-lookup"><span data-stu-id="e4bbb-138">Exercises</span></span>

<span data-ttu-id="e4bbb-139">Questa pratica include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e4bbb-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="e4bbb-140">Esercizio 1: Aggiunta di un Database</span><span class="sxs-lookup"><span data-stu-id="e4bbb-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="e4bbb-141">Esercizio 2: Creazione di un Database mediante Code First</span><span class="sxs-lookup"><span data-stu-id="e4bbb-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="e4bbb-142">Esercizio 3: Eseguire query sul Database con parametri</span><span class="sxs-lookup"><span data-stu-id="e4bbb-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="e4bbb-143">Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="e4bbb-144">Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="e4bbb-145">Tempo stimato per completare questa esercitazione: **35 minuti**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="e4bbb-146">Esercizio 1: Aggiunta di un Database</span><span class="sxs-lookup"><span data-stu-id="e4bbb-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="e4bbb-147">In questo esercizio, si apprenderà come aggiungere un database con le tabelle dell'applicazione MusicStore alla soluzione per poter utilizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="e4bbb-148">Una volta che il database viene generato con il modello e aggiunto alla soluzione, si modificherà la classe StoreController per fornire il modello di visualizzazione con i dati acquisiti dal database, anziché utilizzare valori hardcoded.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="e4bbb-149">Attività 1: aggiunta di un Database</span><span class="sxs-lookup"><span data-stu-id="e4bbb-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="e4bbb-150">In questa attività si aggiungerà un database già creato con le tabelle principale dell'applicazione MusicStore alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="e4bbb-151">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex1-AddingADatabaseDBFirst/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

    1. <span data-ttu-id="e4bbb-152">È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e4bbb-153">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="e4bbb-154">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="e4bbb-155">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4bbb-156">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e4bbb-157">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e4bbb-158">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e4bbb-159">Aggiungere **MvcMusicStore** file di database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="e4bbb-160">In questo laboratorio pratico, utilizzare un database già creato denominato **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="e4bbb-161">A tale scopo, fare doppio clic su **App\_dati** cartella, scegliere **Aggiungi** e quindi fare clic su **elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="e4bbb-162">Passare a **\Source\Assets** e selezionare il **MvcMusicStore.mdf** file.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="e4bbb-163">![Aggiunta di un elemento esistente](aspnet-mvc-4-models-and-data-access/_static/image2.png "aggiunta di un elemento esistente")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="e4bbb-164">*Aggiunta di un elemento esistente*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="e4bbb-165">![File di database MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf file di database")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="e4bbb-166">*File di database MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="e4bbb-167">Il database è stato aggiunto al progetto.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-167">The database has been added to the project.</span></span> <span data-ttu-id="e4bbb-168">Anche quando il database si trova all'interno della soluzione, è possibile eseguire una query e aggiornarli quando è stato ospitato in un server database diverso.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="e4bbb-169">![Database MvcMusicStore in Esplora soluzioni](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Esplora soluzioni")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="e4bbb-170">*Database MvcMusicStore in Esplora soluzioni*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="e4bbb-171">Verificare la connessione al database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-171">Verify the connection to the database.</span></span> <span data-ttu-id="e4bbb-172">A tale scopo, fare doppio clic su **MvcMusicStore.mdf** per stabilire una connessione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="e4bbb-173">![Connessione a MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "la connessione a MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="e4bbb-174">*Connessione a MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="e4bbb-175">Attività 2: creazione di un modello di dati</span><span class="sxs-lookup"><span data-stu-id="e4bbb-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="e4bbb-176">In questa attività si creerà un modello di dati per interagire con il database aggiunto nell'attività precedente.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="e4bbb-177">Creare un modello di data che rappresenta il database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="e4bbb-178">A tale scopo, in rapida di Esplora soluzioni di **modelli** cartella, scegliere **Aggiungi** e quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="e4bbb-179">Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona il **dati** modello e quindi la **ADO.NET Entity Data Model** elemento.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="e4bbb-180">Modificare il nome del modello di dati in **StoreDB.edmx** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="e4bbb-181">![Aggiunta di StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "aggiunta StoreDB ADO.NET Entity Data Model")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="e4bbb-182">*Aggiunta di StoreDB ADO.NET Entity Data Model*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="e4bbb-183">Il **procedura guidata Entity Data Model** verranno visualizzati.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="e4bbb-184">Questa procedura guidata vi assisterà tramite la creazione del livello di modello.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="e4bbb-185">Poiché il modello deve essere creato in base il recentyl database esistenti aggiunti selezionare **genera da database** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="e4bbb-186">![Scegliere il contenuto del modello](aspnet-mvc-4-models-and-data-access/_static/image7.png "scegliendo il contenuto del modello")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="e4bbb-187">*Scegliere il contenuto del modello*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="e4bbb-188">Poiché si desidera generare un modello da un database, è necessario specificare la connessione da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="e4bbb-189">Fare clic su **nuova connessione**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="e4bbb-190">Selezionare **File di Database di Microsoft SQL Server** e fare clic su **continua**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="e4bbb-191">![Scegli origine dati](aspnet-mvc-4-models-and-data-access/_static/image8.png "Scegli origine dati")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="e4bbb-192">*Scegliere una finestra di dialogo origine dati*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="e4bbb-193">Fare clic su **Sfoglia** e selezionare il database **MvcMusicStore.mdf** trova nella **App\_dati** cartella e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="e4bbb-194">![Le proprietà di connessione](aspnet-mvc-4-models-and-data-access/_static/image9.png "le proprietà di connessione")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="e4bbb-195">*Proprietà di connessione*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-195">*Connection properties*</span></span>
6. <span data-ttu-id="e4bbb-196">La classe generata deve avere lo stesso nome come stringa di connessione entity, modificare il nome su **MusicStoreEntities** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="e4bbb-197">![Scegliere la connessione dati](aspnet-mvc-4-models-and-data-access/_static/image10.png "scelta la connessione dati")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="e4bbb-198">*Scegliere la connessione dati*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="e4bbb-199">Scegliere gli oggetti di database da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-199">Choose the database objects to use.</span></span> <span data-ttu-id="e4bbb-200">Il modello di entità utilizzerà solo le tabelle del database, selezionare il **tabelle** opzione e verificare che il **Includi colonne chiavi esterne nel modello** e **Rendi plurali o singolari generato i nomi di oggetto** risulteranno selezionate anche le opzioni.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="e4bbb-201">Modificare il modello Namespace per **MvcMusicStore.Model** e fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="e4bbb-202">![Scegliere gli oggetti di database](aspnet-mvc-4-models-and-data-access/_static/image11.png "scegliendo gli oggetti di database")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="e4bbb-203">*Scegliere gli oggetti di database*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4bbb-204">Se viene visualizzata una finestra di dialogo Avviso di sicurezza, fare clic su **OK** per eseguire il modello e generare le classi per l'entità del modello.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="e4bbb-205">Verrà visualizzato un diagramma di entità per il database, mentre verrà creata una classe separata che esegue il mapping di ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="e4bbb-206">Ad esempio, il **album** tabella sarà rappresentata da un **Album** (classe), ogni colonna della tabella in cui verrà eseguito il mapping a una proprietà di classe.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="e4bbb-207">Ciò consentirà di eseguire query e utilizzare gli oggetti che rappresentano le righe nel database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="e4bbb-208">![Diagramma di entità](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagramma entità")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="e4bbb-209">*Diagramma di entità*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-209">*Entity diagram*</span></span>

> [!NOTE]
> <span data-ttu-id="e4bbb-210">I modelli T4 (con estensione TT) eseguire codice per generare le classi di entità e sovrascriveranno le classi esistenti con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="e4bbb-211">In questo esempio, le classi &quot;Album&quot;, &quot;Genre&quot; e &quot;artista&quot; sono stati sovrascritti con il codice generato.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="e4bbb-212">Attività 3: compilazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="e4bbb-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="e4bbb-213">In questa attività, si verificherà che, anche se la generazione del modello è stato rimosso il **Album**, **Genre** e **artista** classi del modello, il progetto venga compilato correttamente tramite le nuove classi di modello di dati.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="e4bbb-214">Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilare MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="e4bbb-215">![Compilazione del progetto](aspnet-mvc-4-models-and-data-access/_static/image13.png "la compilazione del progetto")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="e4bbb-216">*Compilazione del progetto*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-216">*Building the project*</span></span>
2. <span data-ttu-id="e4bbb-217">Il progetto venga compilato correttamente.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-217">The project builds successfully.</span></span> <span data-ttu-id="e4bbb-218">Perché ancora funziona?</span><span class="sxs-lookup"><span data-stu-id="e4bbb-218">Why does it still work?</span></span> <span data-ttu-id="e4bbb-219">Funziona perché le tabelle di database dispone di campi che includono le proprietà che si sono utilizzato nelle classi rimosse **Album** e **Genre**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="e4bbb-220">![Compilazioni ha avuto esito positivo](aspnet-mvc-4-models-and-data-access/_static/image14.png "compilazioni ha avuto esito positivo")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="e4bbb-221">*Compilazioni completate*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="e4bbb-222">Mentre la finestra di progettazione consente di visualizzare le entità in un formato di diagramma, sono in effetti classi c#.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="e4bbb-223">Espandere il **StoreDB.edmx** nodo in Esplora soluzioni e quindi **StoreDB.tt**, si noterà che la nuova entità generate.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="e4bbb-224">![I file generati](aspnet-mvc-4-models-and-data-access/_static/image15.png "i file generati")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="e4bbb-225">*File generati*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="e4bbb-226">Attività 4: eseguire query sul Database</span><span class="sxs-lookup"><span data-stu-id="e4bbb-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="e4bbb-227">In questa attività si aggiornerà la classe StoreController in modo che, invece di usare dati hardcoded, invierà una query del database per recuperare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="e4bbb-228">Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per contenere un'istanza del **MusicStoreEntities** classe, denominata **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="e4bbb-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="e4bbb-229">(- Frammento di codice *modelli e accesso ai dati - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="e4bbb-230">Il **MusicStoreEntities** classe espone una raccolta di proprietà per ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="e4bbb-231">Aggiornamento **Sfoglia** il metodo di azione per recuperare un genere con tutte le **album**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="e4bbb-232">(- Frammento di codice *modelli e l'accesso ai dati - Sfoglia archivio Ex1*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="e4bbb-233">Si utilizza una funzionalità di .NET denominata **LINQ** (language integrated query) per scrivere espressioni di query fortemente tipizzata in base a queste raccolte - di cui eseguire il codice nel database e verranno restituiti gli oggetti che è possibile programmare nei confronti.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="e4bbb-234">Per ulteriori informazioni su LINQ, visitare il [sito msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="e4bbb-235">Aggiornamento **indice** per recuperare tutti i generi i metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="e4bbb-236">(- Frammento di codice *modelli e l'accesso ai dati - indice archivio Ex1*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="e4bbb-237">Aggiornamento **indice** metodo di azione per recuperare tutti i generi e trasformare la raccolta a un elenco.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="e4bbb-238">(- Frammento di codice *modelli e l'accesso ai dati - Ex1 archivio GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="e4bbb-239">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="e4bbb-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="e4bbb-240">In questa attività si verifica che la pagina di indice dell'archivio visualizzerà i generi archiviati nel database anziché hardcoded quelli.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="e4bbb-241">Non è necessario modificare il modello di visualizzazione perché il **StoreController** restituisce le stesse entità come in precedenza, anche se questo tempo i dati provengono dal database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="e4bbb-242">Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4bbb-243">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-243">The project starts in the Home page.</span></span> <span data-ttu-id="e4bbb-244">Verificare che il menu di **generi** non è più un elenco hardcoded e i dati vengono recuperati direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="e4bbb-246">*Esplorazione generi dal database*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="e4bbb-247">Ora individuare qualsiasi genere e verificare che gli album vengono popolati dal database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="e4bbb-248">![Esplorazione album dal database](aspnet-mvc-4-models-and-data-access/_static/image17.png "esplorazione album dal database")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="e4bbb-249">*Esplorazione album dal database*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="e4bbb-250">Esercizio 2: Creazione di un Database tramite codice prima di tutto</span><span class="sxs-lookup"><span data-stu-id="e4bbb-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="e4bbb-251">In questo esercizio, si apprenderà come usare l'approccio Code First per creare un database con le tabelle dell'applicazione MusicStore e come accedere ai dati.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="e4bbb-252">Dopo la generazione del modello, si modificherà il StoreController per fornire il modello di visualizzazione con i dati acquisiti dal database, anziché utilizzare valori hardcoded.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="e4bbb-253">Se si hanno completato l'esercizio 1 e già stato utilizzato il Database primo approccio, ora si apprenderà come ottenere gli stessi risultati con un altro processo.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="e4bbb-254">Le attività in comune con esercizio 1 sono state contrassegnate per semplificare la lettura.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="e4bbb-255">Se non sono state completate esercizio 1, ma si desidera acquisire il codice primo approccio, è possibile iniziare da questo esercizio e ottenere una copertura completa dell'argomento.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="e4bbb-256">Attività 1: il popolamento dei dati di esempio</span><span class="sxs-lookup"><span data-stu-id="e4bbb-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="e4bbb-257">In questa attività verrà compilato il database con dati di esempio, quando viene inizialmente creato utilizzando Code First.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="e4bbb-258">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex2-CreatingADatabaseCodeFirst/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="e4bbb-259">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="e4bbb-260">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e4bbb-261">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="e4bbb-262">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="e4bbb-263">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4bbb-264">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e4bbb-265">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e4bbb-266">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e4bbb-267">Aggiungere il **SampleData.cs** file per il **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="e4bbb-268">A tale scopo, fare doppio clic su **modelli** cartella, scegliere **Aggiungi** e quindi fare clic su **elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="e4bbb-269">Passare a **\Source\Assets** e selezionare il **SampleData.cs** file.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="e4bbb-270">![Dati di esempio popolano codice](aspnet-mvc-4-models-and-data-access/_static/image18.png "codice di popolare i dati di esempio")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="e4bbb-271">*Codice di popolare i dati di esempio*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="e4bbb-272">Aprire il **Global.asax.cs** file e aggiungere le seguenti *utilizzando* istruzioni.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="e4bbb-273">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 globale Asax using*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="e4bbb-274">Nel **applicazione\_Start** metodo aggiungere la riga seguente per impostare l'inizializzatore del database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="e4bbb-275">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 globale Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="e4bbb-276">Attività 2: configurare la connessione al Database</span><span class="sxs-lookup"><span data-stu-id="e4bbb-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="e4bbb-277">Ora che già stato aggiunto un database al progetto, viene scritto nel **Web. config** file la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="e4bbb-278">Aggiungere una stringa di connessione in **Web. config**. A tale scopo, aprire **Web. config** alla radice del progetto e sostituire la stringa di connessione denominata DefaultConnection con la riga nel  **&lt;connectionStrings&gt;**  sezione:</span><span class="sxs-lookup"><span data-stu-id="e4bbb-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="e4bbb-279">![Percorso del file Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "percorso del file Web. config")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="e4bbb-280">*percorso del file Web. config*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-280">*Web.config file location*</span></span>


    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="e4bbb-281">Attività 3: utilizzo del modello</span><span class="sxs-lookup"><span data-stu-id="e4bbb-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="e4bbb-282">Ora che già stato configurato la connessione al database, si procederà al collegamento del modello con le tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="e4bbb-283">In questa attività si creerà una classe che verrà collegata al database con Code First.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="e4bbb-284">Tenere presente che esiste una classe di modello POCO esistente che deve essere modificata.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="e4bbb-285">Se è stato completato esercizio 1, si noterà che questo passaggio è stato eseguito da una procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="e4bbb-286">In questo modo Code First, si creerà manualmente le classi che verranno collegate alle entità di dati.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>


1. <span data-ttu-id="e4bbb-287">Aprire la classe modello POCO **Genre** da **modelli** cartella del progetto e includere un ID.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="e4bbb-288">Utilizzare una proprietà int con il nome **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="e4bbb-289">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice prima Genre*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="e4bbb-290">Per utilizzare le convenzioni di Code First, la classe Genre deve avere una proprietà di chiave primaria che verrà rilevata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="e4bbb-291">Altre informazioni sulle convenzioni prima del codice in questo [articolo msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="e4bbb-292">A questo punto, aprire la classe di modello POCO **Album** da **modelli** cartella del progetto e includere le chiavi esterne, creare proprietà con i nomi **GenreId** e  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="e4bbb-293">Questa classe dispone già di **GenreId** per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="e4bbb-294">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice primo Album*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="e4bbb-295">Aprire la classe modello POCO **artista** e includere il **ArtistId** proprietà.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="e4bbb-296">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice prima artista*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="e4bbb-297">Fare doppio clic su di **modelli** cartella del progetto e selezionare **Aggiungi | Classe**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="e4bbb-298">Nome del file **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="e4bbb-299">Quindi, fare clic su **Aggiungi.**</span><span class="sxs-lookup"><span data-stu-id="e4bbb-299">Then, click **Add.**</span></span>

    <span data-ttu-id="e4bbb-300">![Aggiunta di una classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "aggiunta di una classe")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="e4bbb-301">*Aggiunta di un nuovo elemento*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-301">*Adding a new item*</span></span>

    <span data-ttu-id="e4bbb-302">![Aggiunta di class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "aggiunta class2")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="e4bbb-303">*Aggiunta di una classe*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-303">*Adding a class*</span></span>
5. <span data-ttu-id="e4bbb-304">Aprire la classe appena creato, **MusicStoreEntities.cs**e includere gli spazi dei nomi **System.Data.Entity** e **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="e4bbb-305">Sostituire la dichiarazione di classe per estendere il **DbContext** classe: dichiarare un pubblico **DBSet** ed eseguire l'override **OnModelCreating** metodo.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="e4bbb-306">Dopo questo passaggio si otterrà una classe di dominio che si collegherà il modello con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="e4bbb-307">A tale scopo, sostituire il codice della classe con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="e4bbb-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="e4bbb-308">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice prima MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="e4bbb-309">Con Entity Framework **DbContext** e **DBSet** sarà in grado di eseguire query sulla classe POCO genere.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="e4bbb-310">Estendendo **OnModelCreating** metodo, si specifica nel **codice** come genere verrà mappato a una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="e4bbb-311">È possibile trovare ulteriori informazioni su DBContext e DBSet in questo articolo di msdn: [collegamento](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="e4bbb-312">Attività 4: eseguire query sul Database</span><span class="sxs-lookup"><span data-stu-id="e4bbb-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="e4bbb-313">In questa attività si aggiornerà la classe StoreController in modo che, invece di usare dati hardcoded, verrà recuperato dal database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="e4bbb-314">Questa attività è in comune con esercizio 1.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="e4bbb-315">Se è stato completato l'esercizio 1, si noterà questi passaggi sono gli stessi in entrambi gli approcci (prima del Database o prima del codice).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="e4bbb-316">Sono diversi in modalità con cui i dati sono collegati con il modello, ma l'accesso alle entità di dati è ancora visibile dal controller.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="e4bbb-317">Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per contenere un'istanza del **MusicStoreEntities** classe, denominata **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="e4bbb-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="e4bbb-318">(- Frammento di codice *modelli e accesso ai dati - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="e4bbb-319">Il **MusicStoreEntities** classe espone una raccolta di proprietà per ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="e4bbb-320">Aggiornamento **Sfoglia** il metodo di azione per recuperare un genere con tutte le **album**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="e4bbb-321">(- Frammento di codice *modelli e l'accesso ai dati - Sfoglia archivio Ex2*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="e4bbb-322">Si utilizza una funzionalità di .NET denominata **LINQ** (language integrated query) per scrivere espressioni di query fortemente tipizzata in base a queste raccolte - di cui eseguire il codice nel database e verranno restituiti gli oggetti che è possibile programmare nei confronti.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="e4bbb-323">Per ulteriori informazioni su LINQ, visitare il [sito msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="e4bbb-324">Aggiornamento **indice** per recuperare tutti i generi i metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="e4bbb-325">(- Frammento di codice *modelli e l'accesso ai dati - indice archivio Ex2*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="e4bbb-326">Aggiornamento **indice** metodo di azione per recuperare tutti i generi e trasformare la raccolta a un elenco.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="e4bbb-327">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 archivio GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="e4bbb-328">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="e4bbb-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="e4bbb-329">In questa attività si verifica che la pagina di indice dell'archivio visualizzerà i generi archiviati nel database anziché hardcoded quelli.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="e4bbb-330">Non è necessario modificare il modello di visualizzazione perché il **StoreController** restituisce lo stesso **StoreIndexViewModel** come in precedenza, ma questa volta i dati provengono dal database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="e4bbb-331">Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4bbb-332">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-332">The project starts in the Home page.</span></span> <span data-ttu-id="e4bbb-333">Verificare che il menu di **generi** non è più un elenco hardcoded e i dati vengono recuperati direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="e4bbb-335">*Esplorazione generi dal database*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="e4bbb-336">Ora individuare qualsiasi genere e verificare che gli album vengono popolati dal database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="e4bbb-337">![Esplorazione album dal database](aspnet-mvc-4-models-and-data-access/_static/image23.png "esplorazione album dal database")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="e4bbb-338">*Esplorazione album dal database*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="e4bbb-339">Esercizio 3: Eseguire query sul Database con parametri</span><span class="sxs-lookup"><span data-stu-id="e4bbb-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="e4bbb-340">In questo esercizio, si apprenderà come eseguire query sul database utilizzando i parametri e come utilizzare la forma del risultato di Query, una funzionalità che riduce il numero database accede il recupero dei dati in modo più efficiente.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="e4bbb-341">Per ulteriori informazioni sulla forma del risultato di Query, visitare il seguente [articolo msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="e4bbb-342">Attività 1 - modifica StoreController per recuperare gli album da Database</span><span class="sxs-lookup"><span data-stu-id="e4bbb-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="e4bbb-343">In questa attività si modificherà il **StoreController** classe per accedere al database per recuperare gli album da un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="e4bbb-344">Aprire il **iniziare** soluzione si trova in corrispondenza di **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** cartella se si desidera utilizzare Code First approccio o **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** cartella se si desidera utilizzare l'approccio di Database-First.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="e4bbb-345">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="e4bbb-346">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e4bbb-347">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="e4bbb-348">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="e4bbb-349">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4bbb-350">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e4bbb-351">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e4bbb-352">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e4bbb-353">Aprire il **StoreController** classe per modificare il **Sfoglia** metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="e4bbb-354">A tale scopo, nel **Esplora**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="e4bbb-355">Modifica il **Sfoglia** metodo di azione per recuperare gli album per un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="e4bbb-356">A tale scopo, sostituire il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e4bbb-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="e4bbb-357">(- Frammento di codice *modelli e l'accesso ai dati - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="e4bbb-358">Per popolare una raccolta di entità, è necessario utilizzare il **Include** per specificare che si desidera recuperare gli album troppo.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="e4bbb-359">È possibile utilizzare il. **Single()** estensione in LINQ perché in questo caso in cui è previsto il genere di un solo per un album.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="e4bbb-360">Il **Single()** metodo accetta un'espressione Lambda come un parametro che specifica in questo caso un singolo oggetto Genre in modo che il nome corrisponde al valore definito.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
    > 
    > <span data-ttu-id="e4bbb-361">Si avvale di una funzionalità che consente di indicare altre entità correlate che si desidera caricare anche quando l'oggetto Genre viene recuperato.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="e4bbb-362">Questa funzionalità è denominata **forma del risultato di Query**e consente di ridurre il numero di volte necessario per accedere al database per recuperare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="e4bbb-363">In questo scenario, è possibile recuperare preventivamente gli album per il genere recuperate.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
    > 
    > <span data-ttu-id="e4bbb-364">La query include **Genres.Include (&quot;album&quot;)** per indicare che si desidera anche album correlati.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="e4bbb-365">Verrà creato in un'applicazione più efficiente, poiché consente di recuperare dati sia Genre e Album nella richiesta singolo database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="e4bbb-366">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="e4bbb-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="e4bbb-367">In questa attività, verranno eseguire l'applicazione e saranno recuperate album di un genere specifico dal database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="e4bbb-368">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4bbb-369">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-369">The project starts in the Home page.</span></span> <span data-ttu-id="e4bbb-370">Modificare l'URL in **archivio/Sfoglia? genre = Pop** per verificare che i risultati vengono recuperati dal database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="e4bbb-371">![Esplorazione per genere](aspnet-mvc-4-models-and-data-access/_static/image24.png "esplorazione per genere")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="e4bbb-372">*Esplorazione archivio/Sfoglia? genre = Pop*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="e4bbb-373">Attività 3 - accesso album da Id</span><span class="sxs-lookup"><span data-stu-id="e4bbb-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="e4bbb-374">In questa attività si ripeterà la procedura precedente per ottenere gli album base all'ID.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="e4bbb-375">Chiudere il browser, se necessario, per tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="e4bbb-376">Aprire il **StoreController** classe per modificare il **dettagli** metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="e4bbb-377">A tale scopo, nel **Esplora**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="e4bbb-378">Modifica il **dettagli** recuperare album Dettagli metodo di azione in base alle loro **Id**. A tale scopo, sostituire il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e4bbb-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="e4bbb-379">(- Frammento di codice *modelli e l'accesso ai dati - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="e4bbb-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="e4bbb-380">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="e4bbb-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="e4bbb-381">In questa attività verrà eseguire l'applicazione in un web browser e ottenere i dettagli di album in base al relativo ID.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="e4bbb-382">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4bbb-383">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-383">The project starts in the Home page.</span></span> <span data-ttu-id="e4bbb-384">Modificare l'URL in **/Store/Details/51** o individuare i generi e selezionare un album per verificare che i risultati vengono recuperati dal database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="e4bbb-385">![Visualizzazione dettagli](aspnet-mvc-4-models-and-data-access/_static/image25.png "visualizzazione dettagli")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="e4bbb-386">*Esplorazione /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="e4bbb-387">Inoltre, è possibile distribuire l'applicazione per siti Web di Azure seguenti [pubblicazione appendice b: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e4bbb-388">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e4bbb-388">Summary</span></span>

<span data-ttu-id="e4bbb-389">Per il completamento di questa pratica si è appreso le nozioni di base di accesso ai dati e i modelli di MVC ASP.NET, usando il **Database First** approccio, nonché **Code First** approccio:</span><span class="sxs-lookup"><span data-stu-id="e4bbb-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="e4bbb-390">Come aggiungere un database alla soluzione per poter utilizzare i relativi dati</span><span class="sxs-lookup"><span data-stu-id="e4bbb-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="e4bbb-391">Come aggiornare i controller per fornire i modelli di visualizzazione con i dati acquisiti dal database anziché a livello di codice</span><span class="sxs-lookup"><span data-stu-id="e4bbb-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="e4bbb-392">Come eseguire query sul database utilizzando i parametri</span><span class="sxs-lookup"><span data-stu-id="e4bbb-392">How to query the database using parameters</span></span>
- <span data-ttu-id="e4bbb-393">Come utilizzare la Query risultato Data Shaping, una funzionalità che riduce il numero di accessi al database, il recupero dei dati in modo più efficiente</span><span class="sxs-lookup"><span data-stu-id="e4bbb-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="e4bbb-394">Come utilizzare il primo Database e di Code First in Entity Framework di Microsoft per collegare il database con il modello</span><span class="sxs-lookup"><span data-stu-id="e4bbb-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e4bbb-395">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="e4bbb-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e4bbb-396">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il  **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="e4bbb-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e4bbb-397">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e4bbb-398">Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e4bbb-399">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *Visual Studio Express 2012 per Web con Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="e4bbb-400">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-400">Click on **Install Now**.</span></span> <span data-ttu-id="e4bbb-401">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e4bbb-402">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e4bbb-403">![Installa Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e4bbb-404">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e4bbb-405">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="e4bbb-407">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e4bbb-408">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-408">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="e4bbb-410">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-410">*Installation progress*</span></span>
6. <span data-ttu-id="e4bbb-411">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-411">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="e4bbb-413">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-413">*Installation completed*</span></span>
7. <span data-ttu-id="e4bbb-414">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e4bbb-415">Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express per il riquadro Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="e4bbb-417">*VS Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e4bbb-418">Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="e4bbb-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="e4bbb-419">Questa appendice mostra come creare un nuovo sito web dal portale di gestione di Windows Azure e pubblicare l'applicazione, ottenute seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="e4bbb-420">Attività 1 - Creazione di un nuovo sito Web da Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e4bbb-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="e4bbb-421">Passare al [il portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4bbb-422">Con Windows Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="e4bbb-423">È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-423">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="e4bbb-424">![Accedere al portale Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "accedere al portale Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="e4bbb-425">*Accedere al portale di gestione di Azure*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="e4bbb-426">Fare clic su **nuovo** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="e4bbb-427">![Creazione di un nuovo sito Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "creando un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="e4bbb-428">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="e4bbb-429">Fare clic su **calcolo** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="e4bbb-430">Selezionare quindi **creazione rapida** opzione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="e4bbb-431">Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4bbb-432">Un sito Web di Windows Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="e4bbb-433">L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure dall'esterno al portale.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="e4bbb-434">Non include i passaggi per configurare un database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="e4bbb-435">![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-models-and-data-access/_static/image33.png "creando un nuovo sito Web utilizzando Creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="e4bbb-436">*Creazione di un nuovo sito Web utilizzando Creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="e4bbb-437">Attendere finché il nuovo **sito Web** viene creato.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="e4bbb-438">Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="e4bbb-439">Verificare il funzionamento del nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="e4bbb-440">![Esplorazione per il nuovo sito web](aspnet-mvc-4-models-and-data-access/_static/image34.png "esplorazione per il nuovo sito web")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="e4bbb-441">*Esplorazione per il nuovo sito web*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="e4bbb-442">![Sito Web in esecuzione](aspnet-mvc-4-models-and-data-access/_static/image35.png "sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="e4bbb-443">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-443">*Web site running*</span></span>
6. <span data-ttu-id="e4bbb-444">Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="e4bbb-445">![Aprire le pagine di gestione del sito web](aspnet-mvc-4-models-and-data-access/_static/image36.png "apertura delle pagine di gestione del sito web")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="e4bbb-446">*Aprire le pagine di gestione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="e4bbb-447">Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4bbb-448">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="e4bbb-449">Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="e4bbb-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web per siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="e4bbb-451">![Profilo di pubblicazione del sito web di download](aspnet-mvc-4-models-and-data-access/_static/image37.png "profilo di pubblicazione del sito web di download")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="e4bbb-452">*Profilo di pubblicazione del sito Web di download*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="e4bbb-453">Scaricare il file del profilo di pubblicazione in un percorso noto.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="e4bbb-454">In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="e4bbb-455">![Salvare il file del profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image38.png "il salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="e4bbb-456">*Salvare il file del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="e4bbb-457">Attività 2: configurare il Server di Database</span><span class="sxs-lookup"><span data-stu-id="e4bbb-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="e4bbb-458">Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="e4bbb-459">Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="e4bbb-460">È necessario un server di Database SQL per archiviare il database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="e4bbb-461">È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="e4bbb-462">Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="e4bbb-463">Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="e4bbb-464">Non creare il database, come verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="e4bbb-465">![Dashboard del Server Database SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "Dashboard del Server Database SQL")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="e4bbb-466">*Dashboard del Server Database SQL*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="e4bbb-467">Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="e4bbb-468">A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) pulsante.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="e4bbb-470">*Aggiunta indirizzo IP del Client*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="e4bbb-471">Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confermare le modifiche](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="e4bbb-473">*Confermare le modifiche*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="e4bbb-474">Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="e4bbb-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="e4bbb-475">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="e4bbb-476">Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="e4bbb-477">![La pubblicazione dell'applicazione](aspnet-mvc-4-models-and-data-access/_static/image43.png "la pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="e4bbb-478">*Pubblicazione del sito web*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="e4bbb-479">Importare il profilo di pubblicazione che è stato salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="e4bbb-480">![L'importazione del profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image44.png "l'importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="e4bbb-481">*L'importazione di profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="e4bbb-482">Fare clic su **convalidare la connessione**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-482">Click **Validate Connection**.</span></span> <span data-ttu-id="e4bbb-483">Una volta completata la convalida, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4bbb-484">Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="e4bbb-485">![Convalida della connessione](aspnet-mvc-4-models-and-data-access/_static/image45.png "convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="e4bbb-486">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-486">*Validating connection*</span></span>
4. <span data-ttu-id="e4bbb-487">Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="e4bbb-488">![Configurazione della distribuzione Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "configurazione della distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="e4bbb-489">*Configurazione della distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="e4bbb-490">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="e4bbb-490">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="e4bbb-491">Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="e4bbb-492">In **nome utente** digitare il nome di accesso di amministratore di server.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-492">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="e4bbb-493">In **Password** digitare la password dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-493">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="e4bbb-494">Digitare un nuovo nome di database.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-494">Type a new database name.</span></span>

    <span data-ttu-id="e4bbb-495">![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-models-and-data-access/_static/image47.png "configurazione stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="e4bbb-496">*Configurazione di stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="e4bbb-497">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-497">Then click **OK**.</span></span> <span data-ttu-id="e4bbb-498">Quando viene richiesto di creare il database fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="e4bbb-499">![Creazione del database](aspnet-mvc-4-models-and-data-access/_static/image48.png "creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="e4bbb-500">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-500">*Creating the database*</span></span>
7. <span data-ttu-id="e4bbb-501">La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="e4bbb-502">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-502">Then click **Next**.</span></span>

    <span data-ttu-id="e4bbb-503">![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "stringa di connessione che punta al Database SQL")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="e4bbb-504">*Stringa di connessione che punta al Database SQL*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="e4bbb-505">Nel **anteprima** pagina, fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="e4bbb-506">![Pubblicare l'applicazione web](aspnet-mvc-4-models-and-data-access/_static/image50.png "pubblicare l'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="e4bbb-507">*Pubblicare l'applicazione web*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="e4bbb-508">Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="e4bbb-509">Appendice c: utilizzo dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="e4bbb-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="e4bbb-510">Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="e4bbb-511">Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="e4bbb-512">![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-models-and-data-access/_static/image51.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="e4bbb-513">*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="e4bbb-514">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="e4bbb-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="e4bbb-515">Posizionare il cursore dove si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="e4bbb-516">Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="e4bbb-517">Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="e4bbb-518">Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="e4bbb-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="e4bbb-519">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="e4bbb-520">![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-models-and-data-access/_static/image52.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="e4bbb-521">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="e4bbb-522">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-models-and-data-access/_static/image53.png "premere Tab per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="e4bbb-523">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="e4bbb-524">![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-models-and-data-access/_static/image54.png "espanderà premere Tab nuovamente e il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="e4bbb-525">*Premere Tab nuovamente e il frammento di codice verranno espansi*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="e4bbb-526">***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="e4bbb-527">Fare clic in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="e4bbb-528">Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="e4bbb-529">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="e4bbb-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="e4bbb-530">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-models-and-data-access/_static/image55.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="e4bbb-531">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="e4bbb-532">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-models-and-data-access/_static/image56.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="e4bbb-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="e4bbb-533">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="e4bbb-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
