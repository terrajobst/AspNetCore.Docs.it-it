---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Esercitazione: Modificare il database per Entity Framework Database First con app ASP.NET MVC'
description: Questa esercitazione è incentrata su rilasciato un aggiornamento per la struttura del database e propagare tale modifica in tutta l'applicazione web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667635"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="4bb6a-103">Esercitazione: Modificare il database per Entity Framework Database First con app ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4bb6a-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="4bb6a-104">Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="4bb6a-105">Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="4bb6a-106">Il codice generato corrispondente alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="4bb6a-107">Questa esercitazione è incentrata su rilasciato un aggiornamento per la struttura del database e propagare tale modifica in tutta l'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="4bb6a-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="4bb6a-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4bb6a-109">Aggiungere una colonna</span><span class="sxs-lookup"><span data-stu-id="4bb6a-109">Add a column</span></span>
> * <span data-ttu-id="4bb6a-110">Aggiungere la proprietà alle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="4bb6a-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4bb6a-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4bb6a-111">Prerequisites</span></span>

* [<span data-ttu-id="4bb6a-112">La generazione di visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="4bb6a-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="4bb6a-113">Aggiungere una colonna</span><span class="sxs-lookup"><span data-stu-id="4bb6a-113">Add a column</span></span>

<span data-ttu-id="4bb6a-114">Se si aggiorna la struttura di una tabella nel database, è necessario assicurarsi che la modifica viene propagata per il modello di dati, viste e controller.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="4bb6a-115">Per questa esercitazione, si aggiungerà una nuova colonna alla tabella Student per registrare il cognome dello studente.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="4bb6a-116">Per aggiungere questa colonna, aprire il progetto di database e aprire il file Student.sql.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="4bb6a-117">Tramite la finestra di progettazione o il codice T-SQL, aggiungere una colonna denominata **MiddleName** che è un nvarchar (50) e consente valori NULL.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="4bb6a-118">Distribuire questa modifica nel database locale avviando il progetto di database (o F5).</span><span class="sxs-lookup"><span data-stu-id="4bb6a-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="4bb6a-119">Il nuovo campo viene aggiunto alla tabella.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-119">The new field is added to the table.</span></span> <span data-ttu-id="4bb6a-120">Se non è presente in Esplora oggetti di SQL Server, fare clic sul pulsante Aggiorna nel riquadro.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Mostra la nuova colonna](changing-the-database/_static/image2.png)

<span data-ttu-id="4bb6a-122">La nuova colonna esiste nella tabella di database, ma non esiste attualmente nella classe di modello di dati.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="4bb6a-123">È necessario aggiornare il modello per includere la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-123">You must update the model to include your new column.</span></span> <span data-ttu-id="4bb6a-124">Nel **modelli** cartella, aprire il **ContosoModel.edmx** file per visualizzare il diagramma del modello.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="4bb6a-125">Si noti che il modello per studenti non contiene la proprietà MiddleName.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="4bb6a-126">Fare doppio clic su un punto qualsiasi nell'area di progettazione e selezionare **Aggiorna modello da Database**.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="4bb6a-127">Nell'aggiornamento guidato, selezionare la **Refresh** scheda e quindi selezionare **tabelle** > **dbo** > **studente**.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="4bb6a-128">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-128">Click **Finish**.</span></span>

<span data-ttu-id="4bb6a-129">Una volta completato il processo di aggiornamento, il diagramma di database include le nuove **MiddleName** proprietà.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="4bb6a-130">Salvare il **ContosoModel.edmx** file.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="4bb6a-131">È necessario salvare questo file per la nuova proprietà di propagazione per la **Student.cs** classe.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="4bb6a-132">A questo punto è stato aggiornato il database e il modello.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="4bb6a-133">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="4bb6a-134">Aggiungere la proprietà alle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="4bb6a-134">Add the property to the views</span></span>

<span data-ttu-id="4bb6a-135">Sfortunatamente, le viste ancora non contengono la nuova proprietà.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="4bb6a-136">Per aggiornare le viste sono disponibili due opzioni: è possibile nuovamente generare le visualizzazioni, ancora una volta aggiungere lo scaffolding per la classe Student, o è possibile aggiungere manualmente la nuova proprietà a visualizzazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="4bb6a-137">In questa esercitazione si aggiungerà lo scaffolding nuovamente perché non si sono eseguite le modifiche personalizzate alle visualizzazioni generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="4bb6a-138">È possibile aggiungere manualmente la proprietà quando si sono apportate modifiche alle visualizzazioni e non si desidera perdere tali modifiche.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="4bb6a-139">Per garantire le viste vengono ricreate, eliminare il **studenti** cartella sotto **viste**ed eliminare i **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="4bb6a-140">Quindi, fare doppio clic sui **controller** cartella e aggiungere lo scaffolding per il **studente** modello.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="4bb6a-141">Anche in questo caso, denominare il controller **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="4bb6a-142">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-142">Select **Add**.</span></span>

<span data-ttu-id="4bb6a-143">Compilare di nuovo la soluzione.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-143">Build the solution again.</span></span> <span data-ttu-id="4bb6a-144">Le visualizzazioni contengono ora la proprietà MiddleName.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-144">The views now contain the MiddleName property.</span></span>

![mostrano middle name](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="4bb6a-146">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="4bb6a-146">Next steps</span></span>

<span data-ttu-id="4bb6a-147">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="4bb6a-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4bb6a-148">Aggiunta di una colonna</span><span class="sxs-lookup"><span data-stu-id="4bb6a-148">Added a column</span></span>
> * <span data-ttu-id="4bb6a-149">Aggiunta la proprietà alle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="4bb6a-149">Added the property to the views</span></span>

<span data-ttu-id="4bb6a-150">Passare all'esercitazione successiva per informazioni su come personalizzare la visualizzazione per la visualizzazione dei dettagli di un record di studenti.</span><span class="sxs-lookup"><span data-stu-id="4bb6a-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="4bb6a-151">Personalizzare una vista</span><span class="sxs-lookup"><span data-stu-id="4bb6a-151">Customize a view</span></span>](customizing-a-view.md)