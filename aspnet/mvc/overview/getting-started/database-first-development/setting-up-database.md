---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Guida introduttiva a 6 Database First di Entity Framework con MVC 5 | Microsoft Docs
author: tfitzmac
description: Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 98deeb91dc2b9a1bad535be1bf1e2ec85dfe4028
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371713"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="cd4ba-104">Guida introduttiva a 6 Database First di Entity Framework con MVC 5</span><span class="sxs-lookup"><span data-stu-id="cd4ba-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="cd4ba-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cd4ba-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cd4ba-106">Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="cd4ba-107">Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="cd4ba-108">Il codice generato corrispondente alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="cd4ba-109">Nell'ultima parte della serie, si distribuirà il sito e il database di Azure.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="cd4ba-110">Questa parte della serie è incentrato sulla creazione di un database e popolarlo con i dati.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="cd4ba-111">Questa serie è stato scritto con contributi Tom Dykstra e Rick Anderson.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="cd4ba-112">È stato migliorato in base al feedback degli utenti nella sezione dei commenti.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="cd4ba-113">Introduzione</span><span class="sxs-lookup"><span data-stu-id="cd4ba-113">Introduction</span></span>

<span data-ttu-id="cd4ba-114">Questo argomento illustra come iniziare con un oggetto esistente del database e creare rapidamente un'applicazione web che consente agli utenti di interagire con i dati.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="cd4ba-115">Per compilare l'applicazione web Usa l'Entity Framework 6 e MVC 5.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="cd4ba-116">La funzionalità di Scaffolding di ASP.NET consente di generare automaticamente codice per la visualizzazione, l'aggiornamento, la creazione e l'eliminazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="cd4ba-117">Usa gli strumenti di pubblicazione all'interno di Visual Studio, è possibile distribuire facilmente il sito e il database in Azure.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="cd4ba-118">Questo argomento illustra la situazione in cui si dispone di un database e si desidera generare il codice per un'applicazione web in base ai campi di tale database.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="cd4ba-119">Questo approccio è definito lo sviluppo di Database First.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-119">This approach is called Database First development.</span></span> <span data-ttu-id="cd4ba-120">Se si dispone già di un database esistente, è possibile usare invece un approccio definito lo sviluppo Code First che prevede la definizione delle classi di dati e generazione del database dalle proprietà della classe.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="cd4ba-121">Per un esempio introduttivo di sviluppo con Code First, vedere [Introduzione a ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="cd4ba-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="cd4ba-122">Per un esempio più avanzato, vedere [creazione di un modello di dati di Entity Framework per un'App ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="cd4ba-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="cd4ba-123">Per indicazioni sulla selezione l'approccio Entity Framework da utilizzare, vedere [approcci di sviluppo di Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="cd4ba-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd4ba-124">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="cd4ba-124">Prerequisites</span></span>

<span data-ttu-id="cd4ba-125">Visual Studio 2013 o Visual Studio Express 2013 per Web</span><span class="sxs-lookup"><span data-stu-id="cd4ba-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="cd4ba-126">Configurare il database</span><span class="sxs-lookup"><span data-stu-id="cd4ba-126">Set up the database</span></span>

<span data-ttu-id="cd4ba-127">Per simulare l'ambiente di disporre di un database esistente, è prima di tutto creare un database con alcuni dati pre-popolata e quindi creare l'applicazione web che si connette al database.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="cd4ba-128">Questa esercitazione è stata sviluppata l'utilizzo di LocalDB con Visual Studio 2013 o Visual Studio Express 2013 per Web.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="cd4ba-129">È possibile usare un server di database esistente anziché LocalDB, ma a seconda della versione di Visual Studio e il tipo di database, tutti gli strumenti dati in Visual Studio potrebbe non essere supportati.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="cd4ba-130">Se gli strumenti non sono disponibili per il database, si potrebbe essere necessario eseguire alcuni passaggi specifici del database all'interno della suite di gestione per il database.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="cd4ba-131">Se si dispone di un problema con gli strumenti di database nella versione di Visual Studio, assicurarsi che sia installata la versione più recente degli strumenti di database.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="cd4ba-132">Per informazioni sull'aggiornamento o installazione degli strumenti di database, vedere [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span><span class="sxs-lookup"><span data-stu-id="cd4ba-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="cd4ba-133">Avviare Visual Studio e creare un **progetto di Database di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="cd4ba-134">Denominare il progetto **ContosoUniversityData**.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-134">Name the project **ContosoUniversityData**.</span></span>

![creare il progetto di database](setting-up-database/_static/image1.png)

<span data-ttu-id="cd4ba-136">È ora disponibile un progetto di database vuoto.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-136">You now have an empty database project.</span></span> <span data-ttu-id="cd4ba-137">Si distribuirà questo database di Azure più avanti in questa esercitazione, quindi è necessario impostare il Database SQL di Azure come piattaforma di destinazione per il progetto.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="cd4ba-138">Impostare la piattaforma di destinazione non distribuisce effettivamente il database. Ciò significa semplicemente che il progetto di database verrà verificato che la progettazione del database è compatibile con la piattaforma di destinazione.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="cd4ba-139">Per impostare la piattaforma di destinazione, aprire il **delle proprietà** per il progetto e selezionare **Database SQL di Microsoft Azure** per la piattaforma di destinazione.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![impostare la destinazione piattaforma](setting-up-database/_static/image2.png)

<span data-ttu-id="cd4ba-141">È possibile creare le tabelle necessarie per questa esercitazione aggiungendo gli script SQL che definiscono le tabelle.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="cd4ba-142">Mouse sul progetto e aggiungere un nuovo elemento.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-142">Right-click your project and add a new item.</span></span>

![Aggiungi nuovo elemento](setting-up-database/_static/image3.png)

<span data-ttu-id="cd4ba-144">Aggiungere una nuova tabella denominata studente.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-144">Add a new table named Student.</span></span>

![Aggiungi tabella student](setting-up-database/_static/image4.png)

<span data-ttu-id="cd4ba-146">Nel file della tabella, sostituire il comando T-SQL con il codice seguente per creare la tabella.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="cd4ba-147">Si noti che la finestra di progettazione si sincronizza automaticamente con il codice.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="cd4ba-148">È possibile lavorare con il codice o la finestra di progettazione.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-148">You can work with either the code or designer.</span></span>

![Mostra codice e progettazione](setting-up-database/_static/image5.png)

<span data-ttu-id="cd4ba-150">Aggiungere un'altra tabella.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-150">Add another table.</span></span> <span data-ttu-id="cd4ba-151">Questa volta denominarlo Course e usare il comando T-SQL seguente.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="cd4ba-152">E, ancora una volta per creare una tabella denominata registrazione ripetere.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="cd4ba-153">È possibile popolare il database con i dati tramite uno script che viene eseguito dopo che il database viene distribuito.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="cd4ba-154">Aggiungere uno Script di post-distribuzione per il progetto.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="cd4ba-155">È possibile usare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-155">You can use the default name.</span></span>

![aggiungere uno script di post-distribuzione](setting-up-database/_static/image6.png)

<span data-ttu-id="cd4ba-157">Aggiungere il codice T-SQL seguente allo script di post-distribuzione.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="cd4ba-158">Questo script aggiunge semplicemente i dati al database quando non viene trovato alcun record corrispondente.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="cd4ba-159">Non sovrascrivere o eliminare i dati che immessi nel database.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="cd4ba-160">È importante notare che lo script di post-distribuzione viene eseguito ogni volta che si distribuisce il progetto di database.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="cd4ba-161">Pertanto, è necessario considerare attentamente i requisiti durante la scrittura di questo script.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="cd4ba-162">In alcuni casi, si desidera iniziare da capo da un set di dati noto ogni volta che viene distribuito il progetto.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="cd4ba-163">In altri casi, è possibile non modificare i dati esistenti in alcun modo.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="cd4ba-164">In base alle esigenze, è possibile decidere se è necessario uno script di post-distribuzione o quello che devi includere nello script.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="cd4ba-165">Per altre informazioni sul popolamento del database con uno script di post-distribuzione, vedere [tra cui i dati in un progetto di Database di SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span><span class="sxs-lookup"><span data-stu-id="cd4ba-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="cd4ba-166">È ora possibile 4 file script SQL, ma non le tabelle effettive.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="cd4ba-167">Si è pronti per distribuire il progetto di database in localdb.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="cd4ba-168">In Visual Studio, fare clic sul pulsante Start (o F5) per compilare e distribuire il progetto di database.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="cd4ba-169">Controllare la scheda di Output per verificare che la compilazione e la distribuzione ha avuto esito positivo.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![Mostra output](setting-up-database/_static/image7.png)

<span data-ttu-id="cd4ba-171">Per verificare che il nuovo database è stato creato, aprire **Esplora oggetti di SQL Server** e cercare il nome del progetto nel server di database locale corretto (in questo caso **\ProjectsV12 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="cd4ba-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![visualizzare di nuovo database](setting-up-database/_static/image8.png)

<span data-ttu-id="cd4ba-173">Per verificare che le tabelle vengono popolate con i dati, fare doppio clic su una tabella e selezionare **dati della visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![Mostra dati tabella](setting-up-database/_static/image9.png)

<span data-ttu-id="cd4ba-175">Viene mostrata una visualizzazione modificabile dei dati della tabella.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-175">An editable view of the table data is displayed.</span></span>

![visualizzare i risultati dei dati di tabella](setting-up-database/_static/image10.png)

<span data-ttu-id="cd4ba-177">Il database è ora configurato e popolato con i dati.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="cd4ba-178">Nella prossima esercitazione, si creerà un'applicazione web per il database.</span><span class="sxs-lookup"><span data-stu-id="cd4ba-178">In the next tutorial, you will create a web application for the database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cd4ba-179">avanti</span><span class="sxs-lookup"><span data-stu-id="cd4ba-179">Next</span></span>](creating-the-web-application.md)
