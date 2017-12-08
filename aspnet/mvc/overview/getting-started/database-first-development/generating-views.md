---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Database di Entity Framework prima con ASP.NET MVC: generazione di visualizzazioni | Documenti Microsoft'
author: tfitzmac
description: "Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 5fccb3c56af0945ec448becff777a3e92dc160d7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="eea43-104">Database di Entity Framework prima con ASP.NET MVC: generazione di visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="eea43-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="eea43-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="eea43-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="eea43-106">Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="eea43-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="eea43-107">Questa serie di esercitazioni viene illustrato come automaticamente generare il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="eea43-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="eea43-108">Il codice generato corrisponde alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="eea43-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="eea43-109">Questa parte della serie si concentra sull'uso di Scaffolding di ASP.NET per generare il controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="eea43-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="eea43-110">Aggiungere lo scaffolding</span><span class="sxs-lookup"><span data-stu-id="eea43-110">Add scaffold</span></span>

<span data-ttu-id="eea43-111">Si è pronti per generare il codice che fornisce le operazioni di dati standard per le classi di modello.</span><span class="sxs-lookup"><span data-stu-id="eea43-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="eea43-112">Aggiungere il codice aggiungendo un elemento lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="eea43-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="eea43-113">Sono disponibili molte opzioni per il tipo di scaffolding che è possibile aggiungere; in questa esercitazione il scaffold includerà un controller e visualizzazioni che corrispondono ai modelli di studenti e la registrazione che è stato creato nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="eea43-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="eea43-114">Per mantenere la coerenza del progetto, si aggiungerà il nuovo controller esistente **controller** cartella.</span><span class="sxs-lookup"><span data-stu-id="eea43-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="eea43-115">Fare doppio clic su di **controller** cartella e selezionare **Aggiungi** – **nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="eea43-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![aggiungere lo scaffolding](generating-views/_static/image1.png)

<span data-ttu-id="eea43-117">Selezionare il **Controller MVC 5 con visualizzazioni, mediante Entity Framework** opzione.</span><span class="sxs-lookup"><span data-stu-id="eea43-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="eea43-118">Questa opzione genererà il controller e visualizzazioni per l'aggiornamento, eliminazione, la creazione e visualizzazione dei dati nel modello.</span><span class="sxs-lookup"><span data-stu-id="eea43-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Aggiungi controller mvc](generating-views/_static/image2.png)

<span data-ttu-id="eea43-120">Selezionare **Student** per la classe di modello e selezionare il **ContosoUniversityEntities** per la classe di contesto.</span><span class="sxs-lookup"><span data-stu-id="eea43-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="eea43-121">Mantenere il nome del controller come **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="eea43-121">Keep the controller name as **StudentsController**,</span></span>

![specificare controller](generating-views/_static/image3.png)

<span data-ttu-id="eea43-123">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="eea43-123">Click **Add**.</span></span>

<span data-ttu-id="eea43-124">Se si riceve un errore, è possibile che si non compila il progetto nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="eea43-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="eea43-125">In questo caso, provare a compilare il progetto e quindi aggiungere di nuovo l'elemento di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="eea43-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="eea43-126">Al termine del processo di generazione del codice, si noterà un nuovo controller e visualizzazioni nel progetto.</span><span class="sxs-lookup"><span data-stu-id="eea43-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![Mostra visualizzazioni](generating-views/_static/image4.png)

<span data-ttu-id="eea43-128">Eseguire di nuovo gli stessi passaggi, ma è aggiungere lo scaffolding per la classe di registrazione.</span><span class="sxs-lookup"><span data-stu-id="eea43-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="eea43-129">Al termine, è necessario un **EnrollmentsController.cs** file e una cartella sotto **viste** denominato **le registrazioni** con Create, Delete, i dettagli, modifica e indice Visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="eea43-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![Mostra visualizzazioni](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="eea43-131">Aggiungere collegamenti alle nuove visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="eea43-131">Add links to new views</span></span>

<span data-ttu-id="eea43-132">Per rendere più semplice per passare alle nuove visualizzazioni, è possibile aggiungere un paio di collegamenti ipertestuali alle viste di indice per studenti e le registrazioni.</span><span class="sxs-lookup"><span data-stu-id="eea43-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="eea43-133">Aprire il file in **Views/Home/Index.cshtml**, che è la home page del sito.</span><span class="sxs-lookup"><span data-stu-id="eea43-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="eea43-134">Aggiungere il codice seguente sotto il jumbotron.</span><span class="sxs-lookup"><span data-stu-id="eea43-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="eea43-135">Per il metodo ActionLink, il primo parametro è il testo da visualizzare nel collegamento.</span><span class="sxs-lookup"><span data-stu-id="eea43-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="eea43-136">Il secondo parametro è l'azione e il terzo parametro è il nome del controller.</span><span class="sxs-lookup"><span data-stu-id="eea43-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="eea43-137">Ad esempio, il primo collegamento punta all'azione indice StudentsController.</span><span class="sxs-lookup"><span data-stu-id="eea43-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="eea43-138">Il collegamento effettivo viene costruito da questi valori.</span><span class="sxs-lookup"><span data-stu-id="eea43-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="eea43-139">Il primo collegamento accetta infine agli utenti del **cshtml** file all'interno di **viste e gli studenti** cartella.</span><span class="sxs-lookup"><span data-stu-id="eea43-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="eea43-140">Visualizzare le visualizzazioni di student</span><span class="sxs-lookup"><span data-stu-id="eea43-140">Display student views</span></span>

<span data-ttu-id="eea43-141">Si verificherà che il codice aggiunto al progetto correttamente Visualizza un elenco di studenti e consente agli utenti di modificare, creare o eliminare i record di studente nel database.</span><span class="sxs-lookup"><span data-stu-id="eea43-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="eea43-142">Fare doppio clic su di **Views/Home/Index.cshtml** file e selezionare **Visualizza nel Browser**.</span><span class="sxs-lookup"><span data-stu-id="eea43-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="eea43-143">In questa pagina, fare clic sul collegamento per l'elenco di studenti.</span><span class="sxs-lookup"><span data-stu-id="eea43-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="eea43-144">In questa pagina viene visualizzato l'elenco di studenti e collegamenti per modificare questi dati.</span><span class="sxs-lookup"><span data-stu-id="eea43-144">On this page, notice the list of the students and links to modify this data.</span></span>

![elenco di studenti](generating-views/_static/image7.png)

<span data-ttu-id="eea43-146">Fare clic su di **Crea nuovo** collegare e fornire valori per un nuovo studente.</span><span class="sxs-lookup"><span data-stu-id="eea43-146">Click the **Create New** link and provide some values for a new student.</span></span>

![creare nuovo studente](generating-views/_static/image8.png)

<span data-ttu-id="eea43-148">Fare clic su **crea**e il nuovo studente viene aggiunto all'elenco.</span><span class="sxs-lookup"><span data-stu-id="eea43-148">Click **Create**, and notice the new student is added to your list.</span></span>

![elenco con nuovo studente](generating-views/_static/image9.png)

<span data-ttu-id="eea43-150">Selezionare il **modifica** collegamento e modificare alcuni dei valori per uno studente.</span><span class="sxs-lookup"><span data-stu-id="eea43-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![modifica di student](generating-views/_static/image10.png)

<span data-ttu-id="eea43-152">Fare clic su **salvare**e il record studente è stato modificato.</span><span class="sxs-lookup"><span data-stu-id="eea43-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="eea43-153">Infine, selezionare il **eliminare** collegare e confermare che si desidera eliminare il record, fare clic su di **eliminare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="eea43-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![eliminare student](generating-views/_static/image11.png)

<span data-ttu-id="eea43-155">Senza scrivere alcun codice, sono state aggiunte le viste che eseguono operazioni comuni sui dati nella tabella di studenti.</span><span class="sxs-lookup"><span data-stu-id="eea43-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="eea43-156">Si può notare che l'etichetta di testo per un campo è in base alla proprietà di database (ad esempio **LastName**) che non è necessariamente ciò che si desidera visualizzare nella pagina web.</span><span class="sxs-lookup"><span data-stu-id="eea43-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="eea43-157">Ad esempio, è preferibile all'etichetta **cognome**.</span><span class="sxs-lookup"><span data-stu-id="eea43-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="eea43-158">Per correggere il problema di visualizzazione è più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="eea43-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="eea43-159">Visualizzare le visualizzazioni di registrazione</span><span class="sxs-lookup"><span data-stu-id="eea43-159">Display enrollment views</span></span>

<span data-ttu-id="eea43-160">Il database include una relazione uno-a-molti tra le tabelle Student e registrazione e una relazione uno-a-molti tra le tabelle in corso e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="eea43-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="eea43-161">Le viste per la registrazione di gestire correttamente tali relazioni.</span><span class="sxs-lookup"><span data-stu-id="eea43-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="eea43-162">Passare alla home page per il sito e selezionare il **elenco le registrazioni** collegamento e quindi la **Crea nuovo** collegamento.</span><span class="sxs-lookup"><span data-stu-id="eea43-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="eea43-163">Nella visualizzazione di un form per la creazione di un nuovo record di registrazione.</span><span class="sxs-lookup"><span data-stu-id="eea43-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="eea43-164">In particolare, si noti che il form contiene due elenchi a discesa vengono popolati con i valori dalle tabelle correlate.</span><span class="sxs-lookup"><span data-stu-id="eea43-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![creare una registrazione](generating-views/_static/image12.png)

<span data-ttu-id="eea43-166">Inoltre, convalida dei valori forniti viene applicata automaticamente in base al tipo di dati del campo.</span><span class="sxs-lookup"><span data-stu-id="eea43-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="eea43-167">Livello richiede un numero, pertanto viene visualizzato un messaggio di errore se si tenta di fornire un valore non compatibile.</span><span class="sxs-lookup"><span data-stu-id="eea43-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![messaggio di convalida](generating-views/_static/image13.png)

<span data-ttu-id="eea43-169">Significa che le visualizzazioni generate automaticamente consentono agli utenti di lavorare con i dati nel database.</span><span class="sxs-lookup"><span data-stu-id="eea43-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="eea43-170">Nella prossima esercitazione di questa serie, si aggiorna il database e apportare le modifiche corrispondenti nell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="eea43-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="eea43-171">[Precedente](creating-the-web-application.md)
[Successivo](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="eea43-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
