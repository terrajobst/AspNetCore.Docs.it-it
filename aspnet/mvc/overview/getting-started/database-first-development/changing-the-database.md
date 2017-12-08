---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Database di Entity Framework prima con ASP.NET MVC: modifica del Database | Documenti Microsoft'
author: tfitzmac
description: "Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 1ffe753812e5eef817f03ab488a28ae5fcefd41e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="ff59b-104">Database di Entity Framework prima con ASP.NET MVC: modifica del Database</span><span class="sxs-lookup"><span data-stu-id="ff59b-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="ff59b-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ff59b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ff59b-106">Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="ff59b-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="ff59b-107">Questa serie di esercitazioni viene illustrato come automaticamente generare il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="ff59b-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="ff59b-108">Il codice generato corrisponde alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="ff59b-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="ff59b-109">Questa parte della serie è incentrata su effettua un aggiornamento per la struttura del database e propagare la modifica dell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="ff59b-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="ff59b-110">Aggiungere una colonna</span><span class="sxs-lookup"><span data-stu-id="ff59b-110">Add a column</span></span>

<span data-ttu-id="ff59b-111">Se si aggiorna la struttura di una tabella nel database, è necessario assicurarsi che la modifica viene propagata per il modello di dati, visualizzazioni e controller.</span><span class="sxs-lookup"><span data-stu-id="ff59b-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="ff59b-112">Per questa esercitazione si aggiungerà una nuova colonna alla tabella di studenti per registrare il cognome dello studente.</span><span class="sxs-lookup"><span data-stu-id="ff59b-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="ff59b-113">Per aggiungere questa colonna, aprire il progetto di database, aprire il file Student.sql.</span><span class="sxs-lookup"><span data-stu-id="ff59b-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="ff59b-114">Tramite la finestra di progettazione o il codice T-SQL, aggiungere una colonna denominata **MiddleName** che è un nvarchar (50) e consente valori NULL.</span><span class="sxs-lookup"><span data-stu-id="ff59b-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![aggiungere secondo nome](changing-the-database/_static/image1.png)

<span data-ttu-id="ff59b-116">Distribuire questa modifica nel database locale avviando il progetto di database (o F5).</span><span class="sxs-lookup"><span data-stu-id="ff59b-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="ff59b-117">Il nuovo campo viene aggiunto alla tabella.</span><span class="sxs-lookup"><span data-stu-id="ff59b-117">The new field is added to the table.</span></span> <span data-ttu-id="ff59b-118">Se non è presente in Esplora oggetti di SQL Server, fare clic sul pulsante Aggiorna nel riquadro.</span><span class="sxs-lookup"><span data-stu-id="ff59b-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Mostra nuova colonna](changing-the-database/_static/image2.png)

<span data-ttu-id="ff59b-120">La nuova colonna esiste nella tabella di database, ma non è attualmente esiste nella classe di modello di dati.</span><span class="sxs-lookup"><span data-stu-id="ff59b-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="ff59b-121">È necessario aggiornare il modello per includere la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="ff59b-121">You must update the model to include your new column.</span></span> <span data-ttu-id="ff59b-122">Nel **modelli** cartella, aprire il **ContosoModel.edmx** file per la visualizzazione del diagramma del modello.</span><span class="sxs-lookup"><span data-stu-id="ff59b-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="ff59b-123">Si noti che il modello di Student non contiene la proprietà MiddleName.</span><span class="sxs-lookup"><span data-stu-id="ff59b-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="ff59b-124">In un punto qualsiasi nell'area di progettazione e scegliere **il modello di aggiornamento dal Database**.</span><span class="sxs-lookup"><span data-stu-id="ff59b-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![modello di aggiornamento](changing-the-database/_static/image3.png)

<span data-ttu-id="ff59b-126">Nell'aggiornamento guidato, selezionare il **aggiornamento** scheda e **Student** tabella.</span><span class="sxs-lookup"><span data-stu-id="ff59b-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![aggiornamento guidato](changing-the-database/_static/image4.png)

<span data-ttu-id="ff59b-128">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="ff59b-128">Click **Finish**.</span></span>

<span data-ttu-id="ff59b-129">Al termine del processo di aggiornamento, il diagramma di database include il nuovo **MiddleName** proprietà.</span><span class="sxs-lookup"><span data-stu-id="ff59b-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="ff59b-130">Salvare il **ContosoModel.edmx** file.</span><span class="sxs-lookup"><span data-stu-id="ff59b-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="ff59b-131">È necessario salvare questo file per la nuova proprietà di propagazione per il **Student.cs** classe.</span><span class="sxs-lookup"><span data-stu-id="ff59b-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="ff59b-132">A questo punto, è stato aggiornato il database e il modello.</span><span class="sxs-lookup"><span data-stu-id="ff59b-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="ff59b-133">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="ff59b-133">Build the solution.</span></span>

<span data-ttu-id="ff59b-134">Purtroppo, le viste ancora non contengono la nuova proprietà.</span><span class="sxs-lookup"><span data-stu-id="ff59b-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="ff59b-135">Per aggiornare le viste sono disponibili due opzioni: è possibile nuovamente generare le visualizzazioni aggiungendo ancora una volta lo scaffolding per la classe Student o è possibile aggiungere manualmente la nuova proprietà per le visualizzazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="ff59b-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="ff59b-136">In questa esercitazione si aggiungeranno lo scaffolding nuovamente perché non sono state modifiche personalizzate per le visualizzazioni generate automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ff59b-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="ff59b-137">È possibile aggiungere manualmente la proprietà quando si sono apportate modifiche alle viste e non si desidera perdere tali modifiche.</span><span class="sxs-lookup"><span data-stu-id="ff59b-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="ff59b-138">Per assicurarsi che le viste vengono ricreate, eliminare il **studenti** cartella **viste**ed eliminare il **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="ff59b-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="ff59b-139">Quindi, fare doppio clic sul **controller** cartella e aggiungere lo scaffolding per il **Student** modello.</span><span class="sxs-lookup"><span data-stu-id="ff59b-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="ff59b-140">Nuovamente, denominare il controller **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="ff59b-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="ff59b-141">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff59b-141">Select **OK**.</span></span>

<span data-ttu-id="ff59b-142">Le viste contengono ora la proprietà MiddleName.</span><span class="sxs-lookup"><span data-stu-id="ff59b-142">The views now contain the MiddleName property.</span></span>

![visualizzare cognome](changing-the-database/_static/image5.png)

<span data-ttu-id="ff59b-144">Nella sezione successiva, si aggiungerà codice per personalizzare la visualizzazione per la visualizzazione dei dettagli di un record di studenti.</span><span class="sxs-lookup"><span data-stu-id="ff59b-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ff59b-145">[Precedente](generating-views.md)
[Successivo](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="ff59b-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
