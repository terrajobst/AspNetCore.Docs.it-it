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
ms.openlocfilehash: 076fa87eff140a3e7ff6855e4876abac40419c57
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="5b8f0-104">Accesso ai dati e i modelli ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="5b8f0-104">ASP.NET MVC 4 Models and Data Access</span></span>
====================
<span data-ttu-id="5b8f0-105">da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="5b8f0-106">Questa pratica presuppone avere conoscenze di base **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="5b8f0-107">Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **nozioni di base di ASP.NET MVC 4** le esercitazioni pratiche.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-107">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="5b8f0-108">Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza tramite l'applicazione di modifiche di lieve entità a un'applicazione Web di esempio fornita nella cartella di origine.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-108">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="5b8f0-109">Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-109">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<span data-ttu-id="5b8f0-110">In **nozioni fondamentali su MVC ASP.NET** pratica, si hanno stato passando dati hardcoded verso i controller per i modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-110">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="5b8f0-111">Ma, per compilare un'applicazione Web reale, è possibile utilizzare un database reale.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-111">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="5b8f0-112">Questa pratica verrà descritto come utilizzare un motore di database per archiviare e recuperare i dati necessari per l'applicazione del Negozio.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-112">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="5b8f0-113">Per raggiungere questo obiettivo, si avvia con un database esistente e creare il modello di dati di entità da esso.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-113">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="5b8f0-114">In questo laboratorio soddisferà la **Database First** approccio, nonché **Code First** approccio.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-114">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="5b8f0-115">Tuttavia, è inoltre possibile utilizzare il **Model First** approccio, creare lo stesso modello utilizzando gli strumenti e quindi generare il database da esso.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-115">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="5b8f0-116">![Visual Studio prima di database. Prima del modello](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First Visual Studio. Primo modello")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-116">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="5b8f0-117">*Visual Studio prima di database. Primo modello*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-117">*Database First vs. Model First*</span></span>

<span data-ttu-id="5b8f0-118">Dopo aver generato il modello, verranno apportate le modifiche appropriate nella StoreController per fornire le viste di archivio con i dati acquisiti dal database, invece di usare dati hardcoded.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-118">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="5b8f0-119">Non occorre di apportare qualsiasi modifica ai modelli di visualizzazione perché il StoreController verrà restituito il ViewModel stessa per i modelli di visualizzazione, anche se questo tempo i dati provengono dal database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-119">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="5b8f0-120">**Il primo approccio di codice**</span><span class="sxs-lookup"><span data-stu-id="5b8f0-120">**The Code First Approach**</span></span>

<span data-ttu-id="5b8f0-121">L'approccio Code First consente di definire il modello dal codice senza generare le classi che sono in genere associate con il framework.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-121">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="5b8f0-122">Nel codice in primo luogo, gli oggetti del modello sono definiti con POCOs, &quot;Plain Old CLR Object&quot;.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-122">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="5b8f0-123">POCOs sono semplici classi normale che non dispone di alcun tipo di ereditarietà e non implementano le interfacce.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-123">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="5b8f0-124">È possibile generare automaticamente il database da essi oppure è possibile utilizzare un database esistente e generare il mapping della classe dal codice.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-124">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="5b8f0-125">I vantaggi dell'utilizzo di questo approccio è che il modello rimane indipendente dal framework di persistenza (in questo caso, Entity Framework), come le classi POCOs non sono collegate con il framework di mapping.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-125">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="5b8f0-126">Questo Lab è basato su ASP.NET MVC 4 e una versione dell'applicazione di esempio musica archivio personalizzato e adatta solo le funzionalità illustrate in questo laboratorio pratico ridotta a icona.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-126">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="5b8f0-127">Se si desidera esplorare l'intero **negozio** applicazione di esercitazione sarà possibile trovarlo in [negozio di musica MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-127">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5b8f0-128">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="5b8f0-128">Prerequisites</span></span>

<span data-ttu-id="5b8f0-129">È necessario che gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="5b8f0-129">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="5b8f0-130">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5b8f0-131">Configurazione</span><span class="sxs-lookup"><span data-stu-id="5b8f0-131">Setup</span></span>

<span data-ttu-id="5b8f0-132">**L'installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="5b8f0-132">**Installing Code Snippets**</span></span>

<span data-ttu-id="5b8f0-133">Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-133">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="5b8f0-134">Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-134">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="5b8f0-135">Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: con i frammenti di codice](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-135">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5b8f0-136">Esercizi</span><span class="sxs-lookup"><span data-stu-id="5b8f0-136">Exercises</span></span>

<span data-ttu-id="5b8f0-137">Questa pratica include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="5b8f0-137">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="5b8f0-138">Esercizio 1: Aggiunta di un Database</span><span class="sxs-lookup"><span data-stu-id="5b8f0-138">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="5b8f0-139">Esercizio 2: Creazione di un Database mediante Code First</span><span class="sxs-lookup"><span data-stu-id="5b8f0-139">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="5b8f0-140">Esercizio 3: Eseguire query sul Database con parametri</span><span class="sxs-lookup"><span data-stu-id="5b8f0-140">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="5b8f0-141">Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-141">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="5b8f0-142">Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-142">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="5b8f0-143">Tempo stimato per completare questa esercitazione: **35 minuti**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-143">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="5b8f0-144">Esercizio 1: Aggiunta di un Database</span><span class="sxs-lookup"><span data-stu-id="5b8f0-144">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="5b8f0-145">In questo esercizio, si apprenderà come aggiungere un database con le tabelle dell'applicazione MusicStore alla soluzione per poter utilizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-145">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="5b8f0-146">Una volta che il database viene generato con il modello e aggiunto alla soluzione, si modificherà la classe StoreController per fornire il modello di visualizzazione con i dati acquisiti dal database, anziché utilizzare valori hardcoded.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-146">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="5b8f0-147">Attività 1: aggiunta di un Database</span><span class="sxs-lookup"><span data-stu-id="5b8f0-147">Task 1 - Adding a Database</span></span>

<span data-ttu-id="5b8f0-148">In questa attività si aggiungerà un database già creato con le tabelle principale dell'applicazione MusicStore alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-148">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="5b8f0-149">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex1-AddingADatabaseDBFirst/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-149">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

    1. <span data-ttu-id="5b8f0-150">È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-150">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5b8f0-151">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-151">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5b8f0-152">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-152">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5b8f0-153">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-153">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b8f0-154">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-154">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5b8f0-155">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-155">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5b8f0-156">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-156">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5b8f0-157">Aggiungere **MvcMusicStore** file di database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-157">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="5b8f0-158">In questo laboratorio pratico, utilizzare un database già creato denominato **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-158">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="5b8f0-159">A tale scopo, fare doppio clic su **App\_dati** cartella, scegliere **Aggiungi** e quindi fare clic su **elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-159">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="5b8f0-160">Passare a **\Source\Assets** e selezionare il **MvcMusicStore.mdf** file.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-160">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="5b8f0-161">![Aggiunta di un elemento esistente](aspnet-mvc-4-models-and-data-access/_static/image2.png "aggiunta di un elemento esistente")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-161">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="5b8f0-162">*Aggiunta di un elemento esistente*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-162">*Adding an Existing Item*</span></span>

    <span data-ttu-id="5b8f0-163">![File di database MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf file di database")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-163">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="5b8f0-164">*File di database MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-164">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="5b8f0-165">Il database è stato aggiunto al progetto.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-165">The database has been added to the project.</span></span> <span data-ttu-id="5b8f0-166">Anche quando il database si trova all'interno della soluzione, è possibile eseguire una query e aggiornarli quando è stato ospitato in un server database diverso.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-166">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="5b8f0-167">![Database MvcMusicStore in Esplora soluzioni](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Esplora soluzioni")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-167">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="5b8f0-168">*Database MvcMusicStore in Esplora soluzioni*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-168">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="5b8f0-169">Verificare la connessione al database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-169">Verify the connection to the database.</span></span> <span data-ttu-id="5b8f0-170">A tale scopo, fare doppio clic su **MvcMusicStore.mdf** per stabilire una connessione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-170">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="5b8f0-171">![Connessione a MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "la connessione a MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-171">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="5b8f0-172">*Connessione a MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-172">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="5b8f0-173">Attività 2: creazione di un modello di dati</span><span class="sxs-lookup"><span data-stu-id="5b8f0-173">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="5b8f0-174">In questa attività si creerà un modello di dati per interagire con il database aggiunto nell'attività precedente.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-174">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="5b8f0-175">Creare un modello di data che rappresenta il database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-175">Create a data model that will represent the database.</span></span> <span data-ttu-id="5b8f0-176">A tale scopo, in rapida di Esplora soluzioni di **modelli** cartella, scegliere **Aggiungi** e quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-176">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="5b8f0-177">Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona il **dati** modello e quindi la **ADO.NET Entity Data Model** elemento.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-177">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="5b8f0-178">Modificare il nome del modello di dati in **StoreDB.edmx** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-178">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="5b8f0-179">![Aggiunta di StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "aggiunta StoreDB ADO.NET Entity Data Model")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-179">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="5b8f0-180">*Aggiunta di StoreDB ADO.NET Entity Data Model*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-180">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="5b8f0-181">Il **procedura guidata Entity Data Model** verranno visualizzati.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-181">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="5b8f0-182">Questa procedura guidata vi assisterà tramite la creazione del livello di modello.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-182">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="5b8f0-183">Poiché il modello deve essere creato in base il recentyl database esistenti aggiunti selezionare **genera da database** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-183">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="5b8f0-184">![Scegliere il contenuto del modello](aspnet-mvc-4-models-and-data-access/_static/image7.png "scegliendo il contenuto del modello")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-184">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="5b8f0-185">*Scegliere il contenuto del modello*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-185">*Choosing the model content*</span></span>
3. <span data-ttu-id="5b8f0-186">Poiché si desidera generare un modello da un database, è necessario specificare la connessione da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-186">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="5b8f0-187">Fare clic su **nuova connessione**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-187">Click **New Connection**.</span></span>
4. <span data-ttu-id="5b8f0-188">Selezionare **File di Database di Microsoft SQL Server** e fare clic su **continua**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-188">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="5b8f0-189">![Scegli origine dati](aspnet-mvc-4-models-and-data-access/_static/image8.png "Scegli origine dati")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-189">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="5b8f0-190">*Scegliere una finestra di dialogo origine dati*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-190">*Choose data source dialog*</span></span>
5. <span data-ttu-id="5b8f0-191">Fare clic su **Sfoglia** e selezionare il database **MvcMusicStore.mdf** trova nella **App\_dati** cartella e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-191">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="5b8f0-192">![Le proprietà di connessione](aspnet-mvc-4-models-and-data-access/_static/image9.png "le proprietà di connessione")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-192">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="5b8f0-193">*Proprietà di connessione*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-193">*Connection properties*</span></span>
6. <span data-ttu-id="5b8f0-194">La classe generata deve avere lo stesso nome come stringa di connessione entity, modificare il nome su **MusicStoreEntities** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-194">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="5b8f0-195">![Scegliere la connessione dati](aspnet-mvc-4-models-and-data-access/_static/image10.png "scelta la connessione dati")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-195">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="5b8f0-196">*Scegliere la connessione dati*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-196">*Choosing the data connection*</span></span>
7. <span data-ttu-id="5b8f0-197">Scegliere gli oggetti di database da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-197">Choose the database objects to use.</span></span> <span data-ttu-id="5b8f0-198">Il modello di entità utilizzerà solo le tabelle del database, selezionare il **tabelle** opzione e verificare che il **Includi colonne chiavi esterne nel modello** e **Rendi plurali o singolari generato i nomi di oggetto** risulteranno selezionate anche le opzioni.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-198">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="5b8f0-199">Modificare il modello Namespace per **MvcMusicStore.Model** e fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-199">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="5b8f0-200">![Scegliere gli oggetti di database](aspnet-mvc-4-models-and-data-access/_static/image11.png "scegliendo gli oggetti di database")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-200">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="5b8f0-201">*Scegliere gli oggetti di database*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-201">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b8f0-202">Se viene visualizzata una finestra di dialogo Avviso di sicurezza, fare clic su **OK** per eseguire il modello e generare le classi per l'entità del modello.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-202">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="5b8f0-203">Verrà visualizzato un diagramma di entità per il database, mentre verrà creata una classe separata che esegue il mapping di ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-203">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="5b8f0-204">Ad esempio, il **album** tabella sarà rappresentata da un **Album** (classe), ogni colonna della tabella in cui verrà eseguito il mapping a una proprietà di classe.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-204">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="5b8f0-205">Ciò consentirà di eseguire query e utilizzare gli oggetti che rappresentano le righe nel database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-205">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="5b8f0-206">![Diagramma di entità](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagramma entità")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-206">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="5b8f0-207">*Diagramma di entità*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-207">*Entity diagram*</span></span>

> [!NOTE]
> <span data-ttu-id="5b8f0-208">I modelli T4 (con estensione TT) eseguire codice per generare le classi di entità e sovrascriveranno le classi esistenti con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-208">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="5b8f0-209">In questo esempio, le classi &quot;Album&quot;, &quot;Genre&quot; e &quot;artista&quot; sono stati sovrascritti con il codice generato.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-209">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="5b8f0-210">Attività 3: compilazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="5b8f0-210">Task 3 - Building the Application</span></span>

<span data-ttu-id="5b8f0-211">In questa attività, si verificherà che, anche se la generazione del modello è stato rimosso il **Album**, **Genre** e **artista** classi del modello, il progetto venga compilato correttamente tramite le nuove classi di modello di dati.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-211">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="5b8f0-212">Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilare MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-212">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="5b8f0-213">![Compilazione del progetto](aspnet-mvc-4-models-and-data-access/_static/image13.png "la compilazione del progetto")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-213">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="5b8f0-214">*Compilazione del progetto*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-214">*Building the project*</span></span>
2. <span data-ttu-id="5b8f0-215">Il progetto venga compilato correttamente.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-215">The project builds successfully.</span></span> <span data-ttu-id="5b8f0-216">Perché ancora funziona?</span><span class="sxs-lookup"><span data-stu-id="5b8f0-216">Why does it still work?</span></span> <span data-ttu-id="5b8f0-217">Funziona perché le tabelle di database dispone di campi che includono le proprietà che si sono utilizzato nelle classi rimosse **Album** e **Genre**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-217">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="5b8f0-218">![Compilazioni ha avuto esito positivo](aspnet-mvc-4-models-and-data-access/_static/image14.png "compilazioni ha avuto esito positivo")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-218">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="5b8f0-219">*Compilazioni completate*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-219">*Builds succeeded*</span></span>
3. <span data-ttu-id="5b8f0-220">Mentre la finestra di progettazione consente di visualizzare le entità in un formato di diagramma, sono in effetti classi c#.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-220">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="5b8f0-221">Espandere il **StoreDB.edmx** nodo in Esplora soluzioni e quindi **StoreDB.tt**, si noterà che la nuova entità generate.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-221">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="5b8f0-222">![I file generati](aspnet-mvc-4-models-and-data-access/_static/image15.png "i file generati")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-222">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="5b8f0-223">*File generati*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-223">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="5b8f0-224">Attività 4: eseguire query sul Database</span><span class="sxs-lookup"><span data-stu-id="5b8f0-224">Task 4 - Querying the Database</span></span>

<span data-ttu-id="5b8f0-225">In questa attività si aggiornerà la classe StoreController in modo che, invece di usare dati hardcoded, invierà una query del database per recuperare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-225">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="5b8f0-226">Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per contenere un'istanza del **MusicStoreEntities** classe, denominata **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="5b8f0-226">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="5b8f0-227">(- Frammento di codice *modelli e accesso ai dati - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-227">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="5b8f0-228">Il **MusicStoreEntities** classe espone una raccolta di proprietà per ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-228">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="5b8f0-229">Aggiornamento **Sfoglia** il metodo di azione per recuperare un genere con tutte le **album**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-229">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="5b8f0-230">(- Frammento di codice *modelli e l'accesso ai dati - Sfoglia archivio Ex1*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-230">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b8f0-231">Si utilizza una funzionalità di .NET denominata **LINQ** (language integrated query) per scrivere espressioni di query fortemente tipizzata in base a queste raccolte - di cui eseguire il codice nel database e verranno restituiti gli oggetti che è possibile programmare nei confronti.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-231">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="5b8f0-232">Per ulteriori informazioni su LINQ, visitare il [sito msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-232">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="5b8f0-233">Aggiornamento **indice** per recuperare tutti i generi i metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-233">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="5b8f0-234">(- Frammento di codice *modelli e l'accesso ai dati - indice archivio Ex1*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-234">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="5b8f0-235">Aggiornamento **indice** metodo di azione per recuperare tutti i generi e trasformare la raccolta a un elenco.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-235">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="5b8f0-236">(- Frammento di codice *modelli e l'accesso ai dati - Ex1 archivio GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-236">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="5b8f0-237">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="5b8f0-237">Task 5 - Running the Application</span></span>

<span data-ttu-id="5b8f0-238">In questa attività si verifica che la pagina di indice dell'archivio visualizzerà i generi archiviati nel database anziché hardcoded quelli.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-238">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="5b8f0-239">Non è necessario modificare il modello di visualizzazione perché il **StoreController** restituisce le stesse entità come in precedenza, anche se questo tempo i dati provengono dal database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-239">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="5b8f0-240">Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-240">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5b8f0-241">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-241">The project starts in the Home page.</span></span> <span data-ttu-id="5b8f0-242">Verificare che il menu di **generi** non è più un elenco hardcoded e i dati vengono recuperati direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-242">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="5b8f0-244">*Esplorazione generi dal database*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-244">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="5b8f0-245">Ora individuare qualsiasi genere e verificare che gli album vengono popolati dal database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-245">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="5b8f0-246">![Esplorazione album dal database](aspnet-mvc-4-models-and-data-access/_static/image17.png "esplorazione album dal database")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-246">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="5b8f0-247">*Esplorazione album dal database*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-247">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="5b8f0-248">Esercizio 2: Creazione di un Database tramite codice prima di tutto</span><span class="sxs-lookup"><span data-stu-id="5b8f0-248">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="5b8f0-249">In questo esercizio, si apprenderà come usare l'approccio Code First per creare un database con le tabelle dell'applicazione MusicStore e come accedere ai dati.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-249">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="5b8f0-250">Dopo la generazione del modello, si modificherà il StoreController per fornire il modello di visualizzazione con i dati acquisiti dal database, anziché utilizzare valori hardcoded.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-250">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="5b8f0-251">Se si hanno completato l'esercizio 1 e già stato utilizzato il Database primo approccio, ora si apprenderà come ottenere gli stessi risultati con un altro processo.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-251">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="5b8f0-252">Le attività in comune con esercizio 1 sono state contrassegnate per semplificare la lettura.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-252">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="5b8f0-253">Se non sono state completate esercizio 1, ma si desidera acquisire il codice primo approccio, è possibile iniziare da questo esercizio e ottenere una copertura completa dell'argomento.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-253">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="5b8f0-254">Attività 1: il popolamento dei dati di esempio</span><span class="sxs-lookup"><span data-stu-id="5b8f0-254">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="5b8f0-255">In questa attività verrà compilato il database con dati di esempio, quando viene inizialmente creato utilizzando Code First.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-255">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="5b8f0-256">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex2-CreatingADatabaseCodeFirst/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-256">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="5b8f0-257">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-257">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5b8f0-258">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-258">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5b8f0-259">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-259">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5b8f0-260">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-260">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5b8f0-261">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-261">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b8f0-262">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-262">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5b8f0-263">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-263">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5b8f0-264">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-264">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5b8f0-265">Aggiungere il **SampleData.cs** file per il **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-265">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="5b8f0-266">A tale scopo, fare doppio clic su **modelli** cartella, scegliere **Aggiungi** e quindi fare clic su **elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-266">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="5b8f0-267">Passare a **\Source\Assets** e selezionare il **SampleData.cs** file.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-267">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="5b8f0-268">![Dati di esempio popolano codice](aspnet-mvc-4-models-and-data-access/_static/image18.png "codice di popolare i dati di esempio")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-268">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="5b8f0-269">*Codice di popolare i dati di esempio*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-269">*Sample data populate code*</span></span>
3. <span data-ttu-id="5b8f0-270">Aprire il **Global.asax.cs** file e aggiungere le seguenti *utilizzando* istruzioni.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-270">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="5b8f0-271">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 globale Asax using*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-271">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="5b8f0-272">Nel **applicazione\_Start** metodo aggiungere la riga seguente per impostare l'inizializzatore del database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-272">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="5b8f0-273">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 globale Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="5b8f0-274">Attività 2: configurare la connessione al Database</span><span class="sxs-lookup"><span data-stu-id="5b8f0-274">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="5b8f0-275">Ora che già stato aggiunto un database al progetto, viene scritto nel **Web. config** file la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-275">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="5b8f0-276">Aggiungere una stringa di connessione in **Web. config**. A tale scopo, aprire **Web. config** alla radice del progetto e sostituire la stringa di connessione denominata DefaultConnection con la riga nel  **&lt;connectionStrings&gt;**  sezione:</span><span class="sxs-lookup"><span data-stu-id="5b8f0-276">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="5b8f0-277">![Percorso del file Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "percorso del file Web. config")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-277">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="5b8f0-278">*Percorso del file Web. config*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-278">*Web.config file location*</span></span>


    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="5b8f0-279">Attività 3: utilizzo del modello</span><span class="sxs-lookup"><span data-stu-id="5b8f0-279">Task 3 - Working with the Model</span></span>

<span data-ttu-id="5b8f0-280">Ora che già stato configurato la connessione al database, si procederà al collegamento del modello con le tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-280">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="5b8f0-281">In questa attività si creerà una classe che verrà collegata al database con Code First.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-281">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="5b8f0-282">Tenere presente che esiste una classe di modello POCO esistente che deve essere modificata.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-282">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="5b8f0-283">Se è stato completato esercizio 1, si noterà che questo passaggio è stato eseguito da una procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-283">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="5b8f0-284">In questo modo Code First, si creerà manualmente le classi che verranno collegate alle entità di dati.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-284">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>


1. <span data-ttu-id="5b8f0-285">Aprire la classe modello POCO **Genre** da **modelli** cartella del progetto e includere un ID.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-285">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="5b8f0-286">Utilizzare una proprietà int con il nome **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-286">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="5b8f0-287">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice prima Genre*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-287">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b8f0-288">Per utilizzare le convenzioni di Code First, la classe Genre deve avere una proprietà di chiave primaria che verrà rilevata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-288">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="5b8f0-289">Altre informazioni sulle convenzioni prima del codice in questo [articolo msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-289">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="5b8f0-290">A questo punto, aprire la classe di modello POCO **Album** da **modelli** cartella del progetto e includere le chiavi esterne, creare proprietà con i nomi **GenreId** e  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-290">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="5b8f0-291">Questa classe dispone già di **GenreId** per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-291">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="5b8f0-292">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice primo Album*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-292">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="5b8f0-293">Aprire la classe modello POCO **artista** e includere il **ArtistId** proprietà.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-293">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="5b8f0-294">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice prima artista*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-294">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="5b8f0-295">Fare doppio clic su di **modelli** cartella del progetto e selezionare **Aggiungi | Classe**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-295">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="5b8f0-296">Nome del file **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-296">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="5b8f0-297">Quindi, fare clic su **Aggiungi.**</span><span class="sxs-lookup"><span data-stu-id="5b8f0-297">Then, click **Add.**</span></span>

    <span data-ttu-id="5b8f0-298">![Aggiunta di una classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "aggiunta di una classe")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-298">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="5b8f0-299">*Aggiunta di un nuovo elemento*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-299">*Adding a new item*</span></span>

    <span data-ttu-id="5b8f0-300">![Aggiunta di class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "aggiunta class2")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-300">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="5b8f0-301">*Aggiunta di una classe*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-301">*Adding a class*</span></span>
5. <span data-ttu-id="5b8f0-302">Aprire la classe appena creato, **MusicStoreEntities.cs**e includere gli spazi dei nomi **System.Data.Entity** e **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-302">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="5b8f0-303">Sostituire la dichiarazione di classe per estendere il **DbContext** classe: dichiarare un pubblico **DBSet** ed eseguire l'override **OnModelCreating** metodo.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-303">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="5b8f0-304">Dopo questo passaggio si otterrà una classe di dominio che si collegherà il modello con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-304">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="5b8f0-305">A tale scopo, sostituire il codice della classe con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5b8f0-305">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="5b8f0-306">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice prima MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-306">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b8f0-307">Con Entity Framework **DbContext** e **DBSet** sarà in grado di eseguire query sulla classe POCO genere.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-307">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="5b8f0-308">Estendendo **OnModelCreating** metodo, si specifica nel **codice** come genere verrà mappato a una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-308">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="5b8f0-309">È possibile trovare ulteriori informazioni su DBContext e DBSet in questo articolo di msdn: [collegamento](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-309">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="5b8f0-310">Attività 4: eseguire query sul Database</span><span class="sxs-lookup"><span data-stu-id="5b8f0-310">Task 4 - Querying the Database</span></span>

<span data-ttu-id="5b8f0-311">In questa attività si aggiornerà la classe StoreController in modo che, invece di usare dati hardcoded, verrà recuperato dal database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-311">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="5b8f0-312">Questa attività è in comune con esercizio 1.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-312">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="5b8f0-313">Se è stato completato l'esercizio 1, si noterà questi passaggi sono gli stessi in entrambi gli approcci (prima del Database o prima del codice).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-313">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="5b8f0-314">Sono diversi in modalità con cui i dati sono collegati con il modello, ma l'accesso alle entità di dati è ancora visibile dal controller.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-314">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="5b8f0-315">Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per contenere un'istanza del **MusicStoreEntities** classe, denominata **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="5b8f0-315">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="5b8f0-316">(- Frammento di codice *modelli e accesso ai dati - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-316">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="5b8f0-317">Il **MusicStoreEntities** classe espone una raccolta di proprietà per ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-317">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="5b8f0-318">Aggiornamento **Sfoglia** il metodo di azione per recuperare un genere con tutte le **album**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-318">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="5b8f0-319">(- Frammento di codice *modelli e l'accesso ai dati - Sfoglia archivio Ex2*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-319">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b8f0-320">Si utilizza una funzionalità di .NET denominata **LINQ** (language integrated query) per scrivere espressioni di query fortemente tipizzata in base a queste raccolte - di cui eseguire il codice nel database e verranno restituiti gli oggetti che è possibile programmare nei confronti.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-320">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="5b8f0-321">Per ulteriori informazioni su LINQ, visitare il [sito msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-321">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="5b8f0-322">Aggiornamento **indice** per recuperare tutti i generi i metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-322">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="5b8f0-323">(- Frammento di codice *modelli e l'accesso ai dati - indice archivio Ex2*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-323">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="5b8f0-324">Aggiornamento **indice** metodo di azione per recuperare tutti i generi e trasformare la raccolta a un elenco.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-324">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="5b8f0-325">(- Frammento di codice *modelli e l'accesso ai dati - Ex2 archivio GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-325">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="5b8f0-326">Attività 5: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="5b8f0-326">Task 5 - Running the Application</span></span>

<span data-ttu-id="5b8f0-327">In questa attività si verifica che la pagina di indice dell'archivio visualizzerà i generi archiviati nel database anziché hardcoded quelli.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-327">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="5b8f0-328">Non è necessario modificare il modello di visualizzazione perché il **StoreController** restituisce lo stesso **StoreIndexViewModel** come in precedenza, ma questa volta i dati provengono dal database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-328">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="5b8f0-329">Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-329">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5b8f0-330">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-330">The project starts in the Home page.</span></span> <span data-ttu-id="5b8f0-331">Verificare che il menu di **generi** non è più un elenco hardcoded e i dati vengono recuperati direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-331">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="5b8f0-333">*Esplorazione generi dal database*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-333">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="5b8f0-334">Ora individuare qualsiasi genere e verificare che gli album vengono popolati dal database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-334">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="5b8f0-335">![Esplorazione album dal database](aspnet-mvc-4-models-and-data-access/_static/image23.png "esplorazione album dal database")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-335">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="5b8f0-336">*Esplorazione album dal database*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-336">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="5b8f0-337">Esercizio 3: Eseguire query sul Database con parametri</span><span class="sxs-lookup"><span data-stu-id="5b8f0-337">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="5b8f0-338">In questo esercizio, si apprenderà come eseguire query sul database utilizzando i parametri e come utilizzare la forma del risultato di Query, una funzionalità che riduce il numero database accede il recupero dei dati in modo più efficiente.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-338">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="5b8f0-339">Per ulteriori informazioni sulla forma del risultato di Query, visitare il seguente [articolo msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-339">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="5b8f0-340">Attività 1 - modifica StoreController per recuperare gli album da Database</span><span class="sxs-lookup"><span data-stu-id="5b8f0-340">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="5b8f0-341">In questa attività si modificherà il **StoreController** classe per accedere al database per recuperare gli album da un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-341">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="5b8f0-342">Aprire il **iniziare** soluzione si trova in corrispondenza di **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** cartella se si desidera utilizzare Code First approccio o **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** cartella se si desidera utilizzare l'approccio di Database-First.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-342">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="5b8f0-343">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-343">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5b8f0-344">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-344">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5b8f0-345">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-345">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5b8f0-346">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-346">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5b8f0-347">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-347">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b8f0-348">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-348">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5b8f0-349">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-349">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5b8f0-350">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-350">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5b8f0-351">Aprire il **StoreController** classe per modificare il **Sfoglia** metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-351">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="5b8f0-352">A tale scopo, nel **Esplora**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-352">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="5b8f0-353">Modifica il **Sfoglia** metodo di azione per recuperare gli album per un genere specifico.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-353">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="5b8f0-354">A tale scopo, sostituire il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5b8f0-354">To do this, replace the following code:</span></span>

    <span data-ttu-id="5b8f0-355">(- Frammento di codice *modelli e l'accesso ai dati - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-355">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b8f0-356">Per popolare una raccolta di entità, è necessario utilizzare il **Include** per specificare che si desidera recuperare gli album troppo.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-356">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="5b8f0-357">È possibile utilizzare il. **Single()** estensione in LINQ perché in questo caso in cui è previsto il genere di un solo per un album.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-357">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="5b8f0-358">Il **Single()** metodo accetta un'espressione Lambda come un parametro che specifica in questo caso un singolo oggetto Genre in modo che il nome corrisponde al valore definito.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-358">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
    > 
    > <span data-ttu-id="5b8f0-359">Si avvale di una funzionalità che consente di indicare altre entità correlate che si desidera caricare anche quando l'oggetto Genre viene recuperato.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-359">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="5b8f0-360">Questa funzionalità è denominata **forma del risultato di Query**e consente di ridurre il numero di volte necessario per accedere al database per recuperare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-360">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="5b8f0-361">In questo scenario, è possibile recuperare preventivamente gli album per il genere recuperate.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-361">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
    > 
    > <span data-ttu-id="5b8f0-362">La query include **Genres.Include (&quot;album&quot;)** per indicare che si desidera anche album correlati.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-362">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="5b8f0-363">Verrà creato in un'applicazione più efficiente, poiché consente di recuperare dati sia Genre e Album nella richiesta singolo database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-363">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="5b8f0-364">Attività 2: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="5b8f0-364">Task 2 - Running the Application</span></span>

<span data-ttu-id="5b8f0-365">In questa attività, verranno eseguire l'applicazione e saranno recuperate album di un genere specifico dal database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-365">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="5b8f0-366">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-366">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5b8f0-367">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-367">The project starts in the Home page.</span></span> <span data-ttu-id="5b8f0-368">Modificare l'URL in **archivio/Sfoglia? genre = Pop** per verificare che i risultati vengono recuperati dal database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-368">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="5b8f0-369">![Esplorazione per genere](aspnet-mvc-4-models-and-data-access/_static/image24.png "esplorazione per genere")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-369">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="5b8f0-370">*Esplorazione archivio/Sfoglia? genre = Pop*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-370">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="5b8f0-371">Attività 3 - accesso album da Id</span><span class="sxs-lookup"><span data-stu-id="5b8f0-371">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="5b8f0-372">In questa attività si ripeterà la procedura precedente per ottenere gli album base all'ID.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-372">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="5b8f0-373">Chiudere il browser, se necessario, per tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-373">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="5b8f0-374">Aprire il **StoreController** classe per modificare il **dettagli** metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-374">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="5b8f0-375">A tale scopo, nel **Esplora**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-375">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="5b8f0-376">Modifica il **dettagli** recuperare album Dettagli metodo di azione in base alle loro **Id**. A tale scopo, sostituire il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5b8f0-376">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="5b8f0-377">(- Frammento di codice *modelli e l'accesso ai dati - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="5b8f0-377">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="5b8f0-378">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="5b8f0-378">Task 4 - Running the Application</span></span>

<span data-ttu-id="5b8f0-379">In questa attività verrà eseguire l'applicazione in un web browser e ottenere i dettagli di album in base al relativo ID.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-379">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="5b8f0-380">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-380">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5b8f0-381">Il progetto viene avviato nella Home page.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-381">The project starts in the Home page.</span></span> <span data-ttu-id="5b8f0-382">Modificare l'URL in **/Store/Details/51** o individuare i generi e selezionare un album per verificare che i risultati vengono recuperati dal database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-382">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="5b8f0-383">![Visualizzazione dettagli](aspnet-mvc-4-models-and-data-access/_static/image25.png "visualizzazione dettagli")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-383">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="5b8f0-384">*Esplorazione /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-384">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="5b8f0-385">Inoltre, è possibile distribuire l'applicazione per siti Web di Azure seguenti [pubblicazione appendice b: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-385">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5b8f0-386">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="5b8f0-386">Summary</span></span>

<span data-ttu-id="5b8f0-387">Per il completamento di questa pratica si è appreso le nozioni di base di accesso ai dati e i modelli di MVC ASP.NET, usando il **Database First** approccio, nonché **Code First** approccio:</span><span class="sxs-lookup"><span data-stu-id="5b8f0-387">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="5b8f0-388">Come aggiungere un database alla soluzione per poter utilizzare i relativi dati</span><span class="sxs-lookup"><span data-stu-id="5b8f0-388">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="5b8f0-389">Come aggiornare i controller per fornire i modelli di visualizzazione con i dati acquisiti dal database anziché a livello di codice</span><span class="sxs-lookup"><span data-stu-id="5b8f0-389">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="5b8f0-390">Come eseguire query sul database utilizzando i parametri</span><span class="sxs-lookup"><span data-stu-id="5b8f0-390">How to query the database using parameters</span></span>
- <span data-ttu-id="5b8f0-391">Come utilizzare la Query risultato Data Shaping, una funzionalità che riduce il numero di accessi al database, il recupero dei dati in modo più efficiente</span><span class="sxs-lookup"><span data-stu-id="5b8f0-391">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="5b8f0-392">Come utilizzare il primo Database e di Code First in Entity Framework di Microsoft per collegare il database con il modello</span><span class="sxs-lookup"><span data-stu-id="5b8f0-392">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="5b8f0-393">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="5b8f0-393">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="5b8f0-394">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il  **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="5b8f0-394">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="5b8f0-395">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-395">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="5b8f0-396">Passare a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-396">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="5b8f0-397">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *Visual Studio Express 2012 per Web con Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-397">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="5b8f0-398">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-398">Click on **Install Now**.</span></span> <span data-ttu-id="5b8f0-399">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-399">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="5b8f0-400">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-400">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="5b8f0-401">![Installa Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-401">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="5b8f0-402">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-402">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="5b8f0-403">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-403">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="5b8f0-405">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-405">*Accepting the license terms*</span></span>
5. <span data-ttu-id="5b8f0-406">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-406">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="5b8f0-408">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-408">*Installation progress*</span></span>
6. <span data-ttu-id="5b8f0-409">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-409">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="5b8f0-411">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-411">*Installation completed*</span></span>
7. <span data-ttu-id="5b8f0-412">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-412">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="5b8f0-413">Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-413">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express per il riquadro Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="5b8f0-415">*VS Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-415">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="5b8f0-416">Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="5b8f0-416">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="5b8f0-417">Questa appendice mostra come creare un nuovo sito web dal portale di gestione di Windows Azure e pubblicare l'applicazione, ottenute seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-417">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="5b8f0-418">Attività 1 - Creazione di un nuovo sito Web da Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5b8f0-418">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="5b8f0-419">Passare al [il portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-419">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b8f0-420">Con Windows Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-420">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="5b8f0-421">È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-421">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="5b8f0-422">![Accedere al portale Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "accedere al portale Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-422">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="5b8f0-423">*Accedere al portale di gestione di Azure*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-423">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="5b8f0-424">Fare clic su **nuovo** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-424">Click **New** on the command bar.</span></span>

    <span data-ttu-id="5b8f0-425">![Creazione di un nuovo sito Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "creando un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-425">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="5b8f0-426">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-426">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="5b8f0-427">Fare clic su **calcolo** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-427">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="5b8f0-428">Selezionare quindi **creazione rapida** opzione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-428">Then select **Quick Create** option.</span></span> <span data-ttu-id="5b8f0-429">Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-429">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b8f0-430">Un sito Web di Windows Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-430">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="5b8f0-431">L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure dall'esterno al portale.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-431">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="5b8f0-432">Non include i passaggi per configurare un database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-432">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="5b8f0-433">![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-models-and-data-access/_static/image33.png "creando un nuovo sito Web utilizzando Creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-433">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="5b8f0-434">*Creazione di un nuovo sito Web utilizzando Creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-434">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="5b8f0-435">Attendere finché il nuovo **sito Web** viene creato.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-435">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="5b8f0-436">Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-436">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="5b8f0-437">Verificare il funzionamento del nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-437">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="5b8f0-438">![Esplorazione per il nuovo sito web](aspnet-mvc-4-models-and-data-access/_static/image34.png "esplorazione per il nuovo sito web")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-438">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="5b8f0-439">*Esplorazione per il nuovo sito web*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-439">*Browsing to the new web site*</span></span>

    <span data-ttu-id="5b8f0-440">![Sito Web in esecuzione](aspnet-mvc-4-models-and-data-access/_static/image35.png "sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-440">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="5b8f0-441">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-441">*Web site running*</span></span>
6. <span data-ttu-id="5b8f0-442">Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-442">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="5b8f0-443">![Aprire le pagine di gestione del sito web](aspnet-mvc-4-models-and-data-access/_static/image36.png "apertura delle pagine di gestione del sito web")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-443">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="5b8f0-444">*Aprire le pagine di gestione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-444">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="5b8f0-445">Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-445">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b8f0-446">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-446">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="5b8f0-447">Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-447">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="5b8f0-448">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web per siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-448">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="5b8f0-449">![Profilo di pubblicazione del sito web di download](aspnet-mvc-4-models-and-data-access/_static/image37.png "profilo di pubblicazione del sito web di download")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-449">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="5b8f0-450">*Profilo di pubblicazione del sito Web di download*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-450">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="5b8f0-451">Scaricare il file del profilo di pubblicazione in un percorso noto.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-451">Download the publish profile file to a known location.</span></span> <span data-ttu-id="5b8f0-452">In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-452">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="5b8f0-453">![Salvare il file del profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image38.png "il salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-453">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="5b8f0-454">*Salvare il file del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-454">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="5b8f0-455">Attività 2: configurare il Server di Database</span><span class="sxs-lookup"><span data-stu-id="5b8f0-455">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="5b8f0-456">Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-456">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="5b8f0-457">Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-457">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="5b8f0-458">È necessario un server di Database SQL per archiviare il database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-458">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="5b8f0-459">È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-459">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="5b8f0-460">Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-460">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="5b8f0-461">Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-461">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="5b8f0-462">Non creare il database, come verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-462">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="5b8f0-463">![Dashboard del Server Database SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "Dashboard del Server Database SQL")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-463">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="5b8f0-464">*Dashboard del Server Database SQL*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-464">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="5b8f0-465">Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-465">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="5b8f0-466">A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) pulsante.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-466">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="5b8f0-468">*Aggiunta indirizzo IP del Client*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-468">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="5b8f0-469">Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-469">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confermare le modifiche](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="5b8f0-471">*Confermare le modifiche*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-471">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="5b8f0-472">Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="5b8f0-472">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="5b8f0-473">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-473">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="5b8f0-474">Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-474">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="5b8f0-475">![La pubblicazione dell'applicazione](aspnet-mvc-4-models-and-data-access/_static/image43.png "la pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-475">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="5b8f0-476">*Pubblicazione del sito web*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-476">*Publishing the web site*</span></span>
2. <span data-ttu-id="5b8f0-477">Importare il profilo di pubblicazione che è stato salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-477">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="5b8f0-478">![L'importazione del profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image44.png "l'importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-478">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="5b8f0-479">*L'importazione di profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-479">*Importing publish profile*</span></span>
3. <span data-ttu-id="5b8f0-480">Fare clic su **convalidare la connessione**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-480">Click **Validate Connection**.</span></span> <span data-ttu-id="5b8f0-481">Una volta completata la convalida, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-481">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b8f0-482">Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-482">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="5b8f0-483">![Convalida della connessione](aspnet-mvc-4-models-and-data-access/_static/image45.png "convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-483">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="5b8f0-484">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-484">*Validating connection*</span></span>
4. <span data-ttu-id="5b8f0-485">Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-485">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="5b8f0-486">![Configurazione della distribuzione Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "configurazione della distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-486">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="5b8f0-487">*Configurazione della distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-487">*Web deploy configuration*</span></span>
5. <span data-ttu-id="5b8f0-488">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="5b8f0-488">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="5b8f0-489">Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-489">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="5b8f0-490">In **nome utente** digitare il nome di accesso di amministratore di server.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-490">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="5b8f0-491">In **Password** digitare la password dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-491">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="5b8f0-492">Digitare un nuovo nome di database.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-492">Type a new database name.</span></span>

    <span data-ttu-id="5b8f0-493">![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-models-and-data-access/_static/image47.png "configurazione stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-493">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="5b8f0-494">*Configurazione di stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-494">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="5b8f0-495">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-495">Then click **OK**.</span></span> <span data-ttu-id="5b8f0-496">Quando viene richiesto di creare il database fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-496">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="5b8f0-497">![Creazione del database](aspnet-mvc-4-models-and-data-access/_static/image48.png "creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-497">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="5b8f0-498">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-498">*Creating the database*</span></span>
7. <span data-ttu-id="5b8f0-499">La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-499">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="5b8f0-500">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-500">Then click **Next**.</span></span>

    <span data-ttu-id="5b8f0-501">![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "stringa di connessione che punta al Database SQL")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-501">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="5b8f0-502">*Stringa di connessione che punta al Database SQL*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-502">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="5b8f0-503">Nel **anteprima** pagina, fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-503">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="5b8f0-504">![Pubblicare l'applicazione web](aspnet-mvc-4-models-and-data-access/_static/image50.png "pubblicare l'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-504">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="5b8f0-505">*Pubblicare l'applicazione web*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-505">*Publishing the web application*</span></span>
9. <span data-ttu-id="5b8f0-506">Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-506">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="5b8f0-507">Appendice c: utilizzo dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="5b8f0-507">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="5b8f0-508">Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-508">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="5b8f0-509">Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-509">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="5b8f0-510">![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-models-and-data-access/_static/image51.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-510">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="5b8f0-511">*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-511">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="5b8f0-512">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="5b8f0-512">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="5b8f0-513">Posizionare il cursore dove si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-513">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="5b8f0-514">Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-514">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="5b8f0-515">Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-515">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="5b8f0-516">Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="5b8f0-516">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="5b8f0-517">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-517">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="5b8f0-518">![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-models-and-data-access/_static/image52.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-518">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="5b8f0-519">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-519">*Start typing the snippet name*</span></span>

<span data-ttu-id="5b8f0-520">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-models-and-data-access/_static/image53.png "premere Tab per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-520">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="5b8f0-521">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-521">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="5b8f0-522">![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-models-and-data-access/_static/image54.png "espanderà premere Tab nuovamente e il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-522">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="5b8f0-523">*Premere Tab nuovamente e il frammento di codice verranno espansi*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-523">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="5b8f0-524">***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-524">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="5b8f0-525">Fare clic in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-525">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="5b8f0-526">Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-526">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="5b8f0-527">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="5b8f0-527">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="5b8f0-528">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-models-and-data-access/_static/image55.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-528">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="5b8f0-529">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-529">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="5b8f0-530">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-models-and-data-access/_static/image56.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="5b8f0-530">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="5b8f0-531">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="5b8f0-531">*Pick the relevant snippet from the list, by clicking on it*</span></span>
