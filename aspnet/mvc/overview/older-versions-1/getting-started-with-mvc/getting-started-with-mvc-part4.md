---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Creazione di un Database | Microsoft Docs
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice che legge e scrive da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 0e72359095e4c40ef7e56f1290a45ded257143c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362356"
---
<a name="creating-a-database"></a><span data-ttu-id="2427e-104">Creazione di un Database</span><span class="sxs-lookup"><span data-stu-id="2427e-104">Creating a Database</span></span>
====================
<span data-ttu-id="2427e-105">da [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="2427e-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="2427e-106">Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2427e-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="2427e-107">Si creerà una semplice applicazione web che legge e scrive da un database.</span><span class="sxs-lookup"><span data-stu-id="2427e-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="2427e-108">Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.</span><span class="sxs-lookup"><span data-stu-id="2427e-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="2427e-109">In questa sezione si intende creare un nuovo database di SQL Express che verranno utilizzate per archiviare e recuperare i dati dei film.</span><span class="sxs-lookup"><span data-stu-id="2427e-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="2427e-110">IDE di Visual Web Developer, selezionare Visualizza | Esplora server.</span><span class="sxs-lookup"><span data-stu-id="2427e-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="2427e-111">Fare clic con il pulsante destro su connessioni di dati e fare clic su Aggiungi connessione...</span><span class="sxs-lookup"><span data-stu-id="2427e-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="2427e-113">Nella finestra di dialogo Scegli origine dati, selezionare Microsoft SQL Server e selezionare continua.</span><span class="sxs-lookup"><span data-stu-id="2427e-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="2427e-114">Nella finestra di dialogo Aggiungi connessione, immettere ". \SQLEXPRESS" per il nome del Server, quindi immettere "Movies" come nome per il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="2427e-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="2427e-115">[![Aggiungi finestra di dialogo di connessione](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="2427e-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="2427e-116">Fare clic su OK e viene chiesto se si desidera creare tale database.</span><span class="sxs-lookup"><span data-stu-id="2427e-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="2427e-117">Selezionare Sì.</span><span class="sxs-lookup"><span data-stu-id="2427e-117">Select yes.</span></span>

<span data-ttu-id="2427e-118">[![Creazione di film?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="2427e-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="2427e-119">A questo punto è disponibile un database vuoto in Esplora Server.</span><span class="sxs-lookup"><span data-stu-id="2427e-119">Now you've got an empty database in Server Explorer.</span></span>

![Aggiungi nuova tabella](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="2427e-121">Fare clic con il pulsante destro su tabelle e fare clic su Aggiungi tabella.</span><span class="sxs-lookup"><span data-stu-id="2427e-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="2427e-122">Verrà visualizzata la finestra di progettazione di tabella.</span><span class="sxs-lookup"><span data-stu-id="2427e-122">The Table Designer will appear.</span></span> <span data-ttu-id="2427e-123">Aggiungere colonne per Id, Title, ReleaseDate, Genre e prezzo.</span><span class="sxs-lookup"><span data-stu-id="2427e-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="2427e-124">Fare clic con il pulsante destro sulla colonna ID e fare clic su Imposta chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="2427e-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="2427e-125">Ecco quali my aree di progettazione dell'aspetto.</span><span class="sxs-lookup"><span data-stu-id="2427e-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="2427e-126">[![Editor di tabella di database](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="2427e-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="2427e-127">Inoltre, selezionare la colonna Id e nella proprietà della colonna seguito modificare "Specifica Identity" su "Sì".</span><span class="sxs-lookup"><span data-stu-id="2427e-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="2427e-128">[![IsIdentity - proprietà colonna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="2427e-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="2427e-129">Quando è stato fatto, fare clic sull'icona Salva nella barra degli strumenti o scegliere File | Salva dal menu di scelta e assegnare un nome di tabella "**film**" (singolare).</span><span class="sxs-lookup"><span data-stu-id="2427e-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="2427e-130">Abbiamo un database e una tabella.</span><span class="sxs-lookup"><span data-stu-id="2427e-130">We've got a database and a table!</span></span>

<span data-ttu-id="2427e-131">[![Scegli nome](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="2427e-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="2427e-132">Tornare a Esplora Server e fare clic con il pulsante destro tabella Movie, quindi selezionare "Mostra dati tabella".</span><span class="sxs-lookup"><span data-stu-id="2427e-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="2427e-133">Immettere alcuni filmati in modo che il database dispone di alcuni dati.</span><span class="sxs-lookup"><span data-stu-id="2427e-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="2427e-134">[![Modifica delle tabelle di database](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="2427e-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="2427e-135">Creazione di un modello</span><span class="sxs-lookup"><span data-stu-id="2427e-135">Creating a Model</span></span>

<span data-ttu-id="2427e-136">A questo punto, tornare a Esplora soluzioni sul lato destro dell'IDE e del mouse sulla cartella Models e scegliere Aggiungi | Nuovo elemento.</span><span class="sxs-lookup"><span data-stu-id="2427e-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="2427e-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="2427e-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="2427e-138">Dobbiamo creare un modello di entità dal nuovo database.</span><span class="sxs-lookup"><span data-stu-id="2427e-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="2427e-139">Si aggiungerà un set di classi per il progetto che rende più semplice per la query e modificare i dati all'interno di database.</span><span class="sxs-lookup"><span data-stu-id="2427e-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="2427e-140">Selezionare il nodo di dati sul lato sinistro della finestra di dialogo e quindi selezionare il modello di elemento ADO.NET Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="2427e-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="2427e-141">Denominarla Movies.edmx.</span><span class="sxs-lookup"><span data-stu-id="2427e-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="2427e-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="2427e-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="2427e-143">Fare clic sul pulsante "Aggiungi".</span><span class="sxs-lookup"><span data-stu-id="2427e-143">Click the "Add" button.</span></span> <span data-ttu-id="2427e-144">Verrà quindi avviata "Entity Data Model Wizard".</span><span class="sxs-lookup"><span data-stu-id="2427e-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="2427e-145">Nella finestra di dialogo Nuovo che viene visualizzata, selezionare Generate dal Database.</span><span class="sxs-lookup"><span data-stu-id="2427e-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="2427e-146">Poiché abbiamo appena creato un database, è necessario solo in merito a Entity Framework il nuovo database e la relativa tabella.</span><span class="sxs-lookup"><span data-stu-id="2427e-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="2427e-147">Scegliere Avanti per salvare la connessione al database nella configurazione dell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="2427e-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="2427e-148">A questo punto, controllare le tabelle e i film e fare clic su Fine.</span><span class="sxs-lookup"><span data-stu-id="2427e-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="2427e-149">[![Procedura guidata Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="2427e-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="2427e-150">A questo punto è possibile visualizzare la nuova tabella di film in Entity Framework Designer e accedervi dal codice.</span><span class="sxs-lookup"><span data-stu-id="2427e-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="2427e-151">[![Film - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="2427e-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="2427e-152">Nell'area di progettazione è possibile visualizzare una classe "Movie".</span><span class="sxs-lookup"><span data-stu-id="2427e-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="2427e-153">Questa classe esegue il mapping alla tabella "Movie" nel nostro database, e ogni proprietà all'interno di esso viene eseguito il mapping a una colonna con la tabella.</span><span class="sxs-lookup"><span data-stu-id="2427e-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="2427e-154">Ogni istanza di una classe "Movie" corrisponderà a una riga all'interno della tabella "Movie".</span><span class="sxs-lookup"><span data-stu-id="2427e-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="2427e-155">Se non sono quelli di denominazione predefinito e le convenzioni usate da Entity Framework di mapping, è possibile utilizzare la finestra di progettazione di Entity Framework per modificare o personalizzarli.</span><span class="sxs-lookup"><span data-stu-id="2427e-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="2427e-156">Per questa applicazione verrà usata le impostazioni predefinite e salvare solo il file come-è.</span><span class="sxs-lookup"><span data-stu-id="2427e-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="2427e-157">A questo punto, è possibile usare alcuni dati reali.</span><span class="sxs-lookup"><span data-stu-id="2427e-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2427e-158">[Precedente](getting-started-with-mvc-part3.md)
> [Successivo](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="2427e-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
