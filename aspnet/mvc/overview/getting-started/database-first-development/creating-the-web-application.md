---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: "Esercitazione: Creare il l'applicazione Web e i modelli di dati per Entity Framework Database First con ASP.NET MVC"
description: Questa esercitazione è incentrata sulla creazione dell'applicazione web e la generazione di modelli di dati basati su tabelle di database.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: dced55386c3f810e406c5c2b3f0071b45e3b2dbd
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236367"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="1365a-103">Esercitazione: Creare il l'applicazione Web e i modelli di dati per Entity Framework Database First con ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="1365a-103">Tutorial: Create the the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="1365a-104">Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="1365a-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="1365a-105">Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="1365a-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="1365a-106">Il codice generato corrispondente alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="1365a-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="1365a-107">Questa esercitazione è incentrata sulla creazione dell'applicazione web e la generazione di modelli di dati basati su tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="1365a-107">This tutorial focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="1365a-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="1365a-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1365a-109">Creare un'app Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1365a-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="1365a-110">Genera i modelli</span><span class="sxs-lookup"><span data-stu-id="1365a-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1365a-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="1365a-111">Prerequisites</span></span>

* [<span data-ttu-id="1365a-112">Guida introduttiva a 6 Database First di Entity Framework con MVC 5</span><span class="sxs-lookup"><span data-stu-id="1365a-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="1365a-113">Creare un'app Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1365a-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="1365a-114">In una nuova soluzione o la stessa soluzione come progetto di database, creare un nuovo progetto in Visual Studio e selezionare il **applicazione Web ASP.NET** modello.</span><span class="sxs-lookup"><span data-stu-id="1365a-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="1365a-115">Denominare il progetto **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="1365a-115">Name the project **ContosoSite**.</span></span>

![Crea progetto](creating-the-web-application/_static/image1.png)

<span data-ttu-id="1365a-117">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="1365a-117">Click **OK**.</span></span>

<span data-ttu-id="1365a-118">Nella finestra Nuovo progetto ASP.NET, selezionare la **MVC** modello.</span><span class="sxs-lookup"><span data-stu-id="1365a-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="1365a-119">È possibile cancellare il **ospita nel cloud** opzione per il momento, perché si distribuirà l'applicazione nel cloud in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="1365a-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="1365a-120">Fare clic su **OK** per creare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1365a-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="1365a-121">Il progetto viene creato con i file predefiniti e le cartelle.</span><span class="sxs-lookup"><span data-stu-id="1365a-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="1365a-122">In questa esercitazione si userà Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="1365a-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="1365a-123">È possibile verificare la versione di Entity Framework nel progetto tramite la finestra Gestisci pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="1365a-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="1365a-124">Se necessario, aggiornare la versione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1365a-124">If necessary, update your version of Entity Framework.</span></span>

![Mostra versione](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="1365a-126">Genera i modelli</span><span class="sxs-lookup"><span data-stu-id="1365a-126">Generate the models</span></span>

<span data-ttu-id="1365a-127">Ora si creerà i modelli di Entity Framework dalle tabelle del database.</span><span class="sxs-lookup"><span data-stu-id="1365a-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="1365a-128">Questi modelli sono classi che si userà per lavorare con i dati.</span><span class="sxs-lookup"><span data-stu-id="1365a-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="1365a-129">Ogni modello riflette una tabella nel database e contiene proprietà che corrispondono alle colonne nella tabella.</span><span class="sxs-lookup"><span data-stu-id="1365a-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="1365a-130">Fare doppio clic il **modelli** cartella e selezionare **Add** e **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="1365a-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="1365a-131">Nella finestra Aggiungi nuovo elemento, selezionare **Data** nel riquadro sinistro e **ADO.NET Entity Data Model** tra le opzioni nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="1365a-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="1365a-132">Denominare il nuovo file di modello **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="1365a-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="1365a-133">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="1365a-133">Click **Add**.</span></span>

<span data-ttu-id="1365a-134">Nella procedura guidata Entity Data Model, selezionare **Entity Framework Designer da database**.</span><span class="sxs-lookup"><span data-stu-id="1365a-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="1365a-135">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1365a-135">Click **Next**.</span></span>

<span data-ttu-id="1365a-136">Se si dispone di connessioni di database definite all'interno dell'ambiente di sviluppo, si potrebbe vedere una di queste connessioni pre-selezionate.</span><span class="sxs-lookup"><span data-stu-id="1365a-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="1365a-137">Tuttavia, si desidera creare una nuova connessione al database creato nella prima parte di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="1365a-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="1365a-138">Scegliere il **nuova connessione** pulsante.</span><span class="sxs-lookup"><span data-stu-id="1365a-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="1365a-139">Nella finestra proprietà di connessione, specificare il nome del server locale in cui è stato creato il database (in questo caso **\Projects13 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="1365a-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\Projects13**).</span></span> <span data-ttu-id="1365a-140">Dopo aver specificato il nome del server, selezionare il ContosoUniversityData dai database di disponibilità.</span><span class="sxs-lookup"><span data-stu-id="1365a-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![impostare le proprietà di connessione](creating-the-web-application/_static/image8.png)

<span data-ttu-id="1365a-142">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="1365a-142">Click **OK**.</span></span>

<span data-ttu-id="1365a-143">Sono ora visualizzate le proprietà di connessione corretta.</span><span class="sxs-lookup"><span data-stu-id="1365a-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="1365a-144">È possibile usare il nome predefinito per la connessione nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="1365a-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="1365a-145">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1365a-145">Click **Next**.</span></span>

<span data-ttu-id="1365a-146">Selezionare la versione più recente di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1365a-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="1365a-147">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1365a-147">Click **Next**.</span></span>

<span data-ttu-id="1365a-148">Selezionare **tabelle** per generare modelli per tutte le tre tabelle.</span><span class="sxs-lookup"><span data-stu-id="1365a-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="1365a-149">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="1365a-149">Click **Finish**.</span></span>

<span data-ttu-id="1365a-150">Se si riceve un avviso di sicurezza, selezionare **OK** per continuare l'esecuzione del modello.</span><span class="sxs-lookup"><span data-stu-id="1365a-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="1365a-151">I modelli vengono generati dalle tabelle del database e viene visualizzato un diagramma che mostra le proprietà e relazioni tra le tabelle.</span><span class="sxs-lookup"><span data-stu-id="1365a-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![diagramma del modello](creating-the-web-application/_static/image11.png)

<span data-ttu-id="1365a-153">La cartella Models ora include molti nuovi file correlati ai modelli generati dal database.</span><span class="sxs-lookup"><span data-stu-id="1365a-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="1365a-154">Il **ContosoModel.Context.cs** file contiene una classe che deriva dalle **DbContext** classe e fornisce una proprietà per ogni classe di modello che corrisponde a una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="1365a-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="1365a-155">Il **Course.cs**, **Enrollment.cs**, e **Student.cs** file contengono le classi del modello che rappresentano le tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="1365a-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="1365a-156">Si utilizzerà la classe del contesto sia le classi del modello quando si lavora con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="1365a-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="1365a-157">Prima di procedere con questa esercitazione, compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="1365a-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="1365a-158">Nella sezione successiva, si genererà codice basato su modelli di data, ma tale sezione non funzionerà se non è stato compilato il progetto.</span><span class="sxs-lookup"><span data-stu-id="1365a-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1365a-159">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="1365a-159">Next steps</span></span>

<span data-ttu-id="1365a-160">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="1365a-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1365a-161">Creazione di un'app web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1365a-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="1365a-162">I modelli generati</span><span class="sxs-lookup"><span data-stu-id="1365a-162">Generated the models</span></span>

<span data-ttu-id="1365a-163">Passare all'esercitazione successiva per imparare a creare generano codice basato sui modelli di dati.</span><span class="sxs-lookup"><span data-stu-id="1365a-163">Advance to the next tutorial to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="1365a-164">La generazione di visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="1365a-164">Generating views</span></span>](generating-views.md)