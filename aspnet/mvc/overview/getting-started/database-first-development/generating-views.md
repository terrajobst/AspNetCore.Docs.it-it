---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Esercitazione: Generare viste per Entity Framework Database First con app ASP.NET MVC'
description: Questo articolo è incentrato sull'uso di Scaffolding di ASP.NET per generare il controller e visualizzazioni.
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e1f6646cdf10d293268b92f44b018709e70c0f86
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889782"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="394e6-103">Esercitazione: Generare viste per Entity Framework Database First con app ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="394e6-103">Tutorial: Generate views for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="394e6-104">Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="394e6-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="394e6-105">Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="394e6-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="394e6-106">Il codice generato corrispondente alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="394e6-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="394e6-107">Questo articolo è incentrato sull'uso di Scaffolding di ASP.NET per generare il controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="394e6-107">This article focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>

<span data-ttu-id="394e6-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="394e6-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="394e6-109">Aggiungi scaffolding</span><span class="sxs-lookup"><span data-stu-id="394e6-109">Add scaffold</span></span>
> * <span data-ttu-id="394e6-110">Aggiungere collegamenti per le nuove visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="394e6-110">Add links to new views</span></span>
> * <span data-ttu-id="394e6-111">Visualizzare le visualizzazioni per studenti</span><span class="sxs-lookup"><span data-stu-id="394e6-111">Display student views</span></span>
> * <span data-ttu-id="394e6-112">Visualizzare le visualizzazioni di registrazione</span><span class="sxs-lookup"><span data-stu-id="394e6-112">Display enrollment views</span></span>

## <a name="prerequisite"></a><span data-ttu-id="394e6-113">Prerequisito</span><span class="sxs-lookup"><span data-stu-id="394e6-113">Prerequisite</span></span>

* [<span data-ttu-id="394e6-114">Creare modelli di dati e applicazioni web</span><span class="sxs-lookup"><span data-stu-id="394e6-114">Create the web application and data models</span></span>](creating-the-web-application.md)

## <a name="add-scaffold"></a><span data-ttu-id="394e6-115">Aggiungi scaffolding</span><span class="sxs-lookup"><span data-stu-id="394e6-115">Add scaffold</span></span>

<span data-ttu-id="394e6-116">Si è pronti per generare il codice che fornisce operazioni di dati standard per le classi del modello.</span><span class="sxs-lookup"><span data-stu-id="394e6-116">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="394e6-117">Aggiungere il codice aggiungendo un elemento di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="394e6-117">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="394e6-118">Sono disponibili molte opzioni per il tipo di scaffolding che è possibile aggiungere; in questa esercitazione, lo scaffold includerà un controller e visualizzazioni che corrispondono ai modelli per studenti e di registrazione che è stato creato nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="394e6-118">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="394e6-119">Per mantenere la coerenza del progetto, si aggiungerà il nuovo controller esistente **controller** cartella.</span><span class="sxs-lookup"><span data-stu-id="394e6-119">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="394e6-120">Fare doppio clic il **controller** cartella e selezionare **Add** > **nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="394e6-120">Right-click the **Controllers** folder, and select **Add** > **New Scaffolded Item**.</span></span>

<span data-ttu-id="394e6-121">Selezionare il **Controller MVC 5 con visualizzazioni, mediante Entity Framework** opzione.</span><span class="sxs-lookup"><span data-stu-id="394e6-121">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="394e6-122">Questa opzione genererà il controller e visualizzazioni per l'aggiornamento, eliminazione, la creazione e visualizzazione dei dati nel modello.</span><span class="sxs-lookup"><span data-stu-id="394e6-122">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Aggiungi controller mvc](generating-views/_static/image2.png)

<span data-ttu-id="394e6-124">Selezionare **Student (ContosoSite.Models)** per la classe di modello e selezionare il **ContosoUniversityDataEntities (ContosoSite.Models)** per la classe del contesto.</span><span class="sxs-lookup"><span data-stu-id="394e6-124">Select **Student (ContosoSite.Models)** for the model class and select the **ContosoUniversityDataEntities (ContosoSite.Models)** for the context class.</span></span> <span data-ttu-id="394e6-125">Mantenere il nome del controller come **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="394e6-125">Keep the controller name as **StudentsController**.</span></span>

<span data-ttu-id="394e6-126">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="394e6-126">Click **Add**.</span></span>

<span data-ttu-id="394e6-127">Se si riceve un errore, è possibile che il progetto nella sezione precedente non è stata compilata.</span><span class="sxs-lookup"><span data-stu-id="394e6-127">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="394e6-128">In questo caso, provare a compilare il progetto e quindi aggiungere di nuovo l'elemento di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="394e6-128">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="394e6-129">Al termine il processo di generazione di codice, verrà visualizzato un nuovo controller e visualizzazioni del progetto **controller** e **viste** > **studenti** cartelle .</span><span class="sxs-lookup"><span data-stu-id="394e6-129">After the code generation process is complete, you will see a new controller and views in your project's **Controllers** and **Views** > **Students** folders.</span></span>


<span data-ttu-id="394e6-130">Eseguire di nuovo gli stessi passaggi, ma aggiungere lo scaffolding per il **registrazione** classe.</span><span class="sxs-lookup"><span data-stu-id="394e6-130">Perform the same steps again, but add a scaffold for the **Enrollment** class.</span></span> <span data-ttu-id="394e6-131">Al termine, disponibile un' **EnrollmentsController.cs** file e una cartella sotto **viste** denominato **registrazioni** con le visualizzazioni Create, Delete, i dettagli, modifica e indice.</span><span class="sxs-lookup"><span data-stu-id="394e6-131">When finished, you have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

## <a name="add-links-to-new-views"></a><span data-ttu-id="394e6-132">Aggiungere collegamenti per le nuove visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="394e6-132">Add links to new views</span></span>

<span data-ttu-id="394e6-133">Per renderne più semplice per passare alle nuove visualizzazioni, è possibile aggiungere un paio di collegamenti ipertestuali alle viste di indice per gli studenti e le registrazioni.</span><span class="sxs-lookup"><span data-stu-id="394e6-133">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="394e6-134">Aprire il file dal **viste** > **Home** > *index. cshtml*, che è la home page del sito.</span><span class="sxs-lookup"><span data-stu-id="394e6-134">Open the file at **Views** > **Home** > *Index.cshtml*, which is the home page for your site.</span></span> <span data-ttu-id="394e6-135">Aggiungere il codice seguente sotto il jumbotron.</span><span class="sxs-lookup"><span data-stu-id="394e6-135">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="394e6-136">Per il metodo ActionLink, il primo parametro è il testo da visualizzare nel collegamento.</span><span class="sxs-lookup"><span data-stu-id="394e6-136">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="394e6-137">Il secondo parametro è l'azione e il terzo parametro è il nome del controller.</span><span class="sxs-lookup"><span data-stu-id="394e6-137">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="394e6-138">Ad esempio, il primo collegamento punta all'azione Index in StudentsController.</span><span class="sxs-lookup"><span data-stu-id="394e6-138">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="394e6-139">Il collegamento effettivo viene costruito da questi valori.</span><span class="sxs-lookup"><span data-stu-id="394e6-139">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="394e6-140">Il primo collegamento in definitiva richiede agli utenti delle **index. cshtml** all'interno del file il **viste/Students** cartella.</span><span class="sxs-lookup"><span data-stu-id="394e6-140">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="394e6-141">Visualizzare le visualizzazioni per studenti</span><span class="sxs-lookup"><span data-stu-id="394e6-141">Display student views</span></span>

<span data-ttu-id="394e6-142">Si verificherà che il codice aggiunto correttamente al progetto viene visualizzato un elenco degli studenti e consente agli utenti di modificare, creare o eliminare i record di studenti nel database.</span><span class="sxs-lookup"><span data-stu-id="394e6-142">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="394e6-143">Fare doppio clic il **viste** > **Home** > *index. cshtml* file e scegliere **Visualizza nel Browser**.</span><span class="sxs-lookup"><span data-stu-id="394e6-143">Right-click the **Views** > **Home** > *Index.cshtml* file, and select **View in Browser**.</span></span> <span data-ttu-id="394e6-144">Nella home page dell'applicazione, selezionare **elenco degli studenti**.</span><span class="sxs-lookup"><span data-stu-id="394e6-144">On the application home page, select **List of students**.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="394e6-145">Nel **indice** pagina, viene visualizzato l'elenco degli studenti e i collegamenti per modificare i dati.</span><span class="sxs-lookup"><span data-stu-id="394e6-145">On the **Index** page, notice the list of the students and links to modify this data.</span></span> <span data-ttu-id="394e6-146">Selezionare il **Crea nuovo** collegare e fornire valori per un nuovo studente.</span><span class="sxs-lookup"><span data-stu-id="394e6-146">Select the **Create New** link and provide some values for a new student.</span></span> <span data-ttu-id="394e6-147">Fare clic su **Create**e notare che il nuovo studente viene aggiunto all'elenco.</span><span class="sxs-lookup"><span data-stu-id="394e6-147">Click **Create**, and notice the new student is added to your list.</span></span>

<span data-ttu-id="394e6-148">Nella **indice** pagina, selezionare la **modificare** collegamento e modificare alcuni dei valori di uno studente.</span><span class="sxs-lookup"><span data-stu-id="394e6-148">Back on the **Index** page, select the **Edit** link, and change some of the values for a student.</span></span> <span data-ttu-id="394e6-149">Fare clic su **salvare**e notare che è stato modificato il record degli studenti.</span><span class="sxs-lookup"><span data-stu-id="394e6-149">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="394e6-150">Infine, selezionare il **eliminare** collegare e confermare che si desidera eliminare i record facendo il **eliminare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="394e6-150">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

<span data-ttu-id="394e6-151">Senza scrivere alcun codice, si sono aggiunte le viste che eseguono operazioni comuni sui dati nella tabella studenti.</span><span class="sxs-lookup"><span data-stu-id="394e6-151">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="394e6-152">Si è notato che l'etichetta di testo per un campo è basata sulla proprietà database (ad esempio **LastName**) che non è necessariamente ciò che si desidera visualizzare nella pagina web.</span><span class="sxs-lookup"><span data-stu-id="394e6-152">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="394e6-153">Ad esempio, è preferibile all'etichetta **cognome**.</span><span class="sxs-lookup"><span data-stu-id="394e6-153">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="394e6-154">Si correggerà questo problema di visualizzazione in un secondo momento nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="394e6-154">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="394e6-155">Visualizzare le visualizzazioni di registrazione</span><span class="sxs-lookup"><span data-stu-id="394e6-155">Display enrollment views</span></span>

<span data-ttu-id="394e6-156">Il database include una relazione uno-a-molti tra le tabelle Student e registrazione e una relazione uno-a-molti tra le tabelle Course ed Enrollment esiste.</span><span class="sxs-lookup"><span data-stu-id="394e6-156">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="394e6-157">Le viste per la registrazione di gestiscono correttamente tali relazioni.</span><span class="sxs-lookup"><span data-stu-id="394e6-157">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="394e6-158">Passare alla home page per il sito e selezionare il **elenco di registrazioni** collegamento e quindi la **Crea nuovo** collegamento.</span><span class="sxs-lookup"><span data-stu-id="394e6-158">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span>

<span data-ttu-id="394e6-159">Nella visualizzazione di un form per la creazione di un nuovo record di registrazione.</span><span class="sxs-lookup"><span data-stu-id="394e6-159">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="394e6-160">In particolare, si noti che il modulo contiene un **CourseID** elenco a discesa elenco e una **StudentID** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="394e6-160">In particular, notice that the form contains a **CourseID** drop-down list and a **StudentID** drop-down list.</span></span> <span data-ttu-id="394e6-161">Entrambi vengono popolate con i valori dalle tabelle correlate.</span><span class="sxs-lookup"><span data-stu-id="394e6-161">Both are populated with values from the related tables.</span></span>

<span data-ttu-id="394e6-162">Inoltre, convalida dei valori forniti viene applicata automaticamente in base sul tipo di dati del campo.</span><span class="sxs-lookup"><span data-stu-id="394e6-162">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="394e6-163">**Livello** richiede un numero, pertanto viene visualizzato un messaggio di errore se si tenta di fornire un valore incompatibile: *Il livello di campo deve essere un numero.*</span><span class="sxs-lookup"><span data-stu-id="394e6-163">**Grade** requires a number, so an error message is displayed if you try to provide an incompatible value: *The field Grade must be a number.*</span></span>

<span data-ttu-id="394e6-164">Si è verificato che le visualizzazioni generate automaticamente consentono agli utenti di lavorare con i dati nel database.</span><span class="sxs-lookup"><span data-stu-id="394e6-164">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="394e6-165">Nella prossima esercitazione della serie, si sarà l'aggiornamento del database e apportare le modifiche corrispondenti nell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="394e6-165">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="394e6-166">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="394e6-166">Next steps</span></span>

<span data-ttu-id="394e6-167">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="394e6-167">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="394e6-168">Aggiunta di scaffolding</span><span class="sxs-lookup"><span data-stu-id="394e6-168">Added scaffold</span></span>
> * <span data-ttu-id="394e6-169">Sono stati aggiunti collegamenti per le nuove visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="394e6-169">Added links to new views</span></span>
> * <span data-ttu-id="394e6-170">Viste visualizzate per studenti</span><span class="sxs-lookup"><span data-stu-id="394e6-170">Displayed student views</span></span>
> * <span data-ttu-id="394e6-171">Viste di registrazione visualizzata</span><span class="sxs-lookup"><span data-stu-id="394e6-171">Displayed enrollment views</span></span>

<span data-ttu-id="394e6-172">Passare all'articolo successivo per informazioni su come modificare il database.</span><span class="sxs-lookup"><span data-stu-id="394e6-172">Advance to the next article to learn how to change the database.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="394e6-173">Modificare il database</span><span class="sxs-lookup"><span data-stu-id="394e6-173">Change the database</span></span>](changing-the-database.md)