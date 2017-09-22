---
title: Dati - 7 di 10 correlati alla base di ASP.NET MVC con Entity Framework Core - aggiornamento
author: tdykstra
description: "In questa esercitazione si aggiornerà i dati correlati mediante l'aggiornamento di campi di chiave esterni e le proprietà di navigazione."
keywords: ASP.NET Core, Entity Framework Core, i dati correlati, join
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 67bd162b-bfb7-4750-9e7f-705228b5288c
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: daf6dd8024863e02e40ad002a0a7da388f5a2ec7
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a><span data-ttu-id="582ad-104">L'aggiornamento dei dati correlati - EF Core con l'esercitazione di base di ASP.NET MVC (7 di 10)</span><span class="sxs-lookup"><span data-stu-id="582ad-104">Updating related data - EF Core with ASP.NET Core MVC tutorial (7 of 10)</span></span>

<span data-ttu-id="582ad-105">Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="582ad-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="582ad-106">L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="582ad-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="582ad-107">Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="582ad-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="582ad-108">Nell'esercitazione precedente si visualizzati dati correlati. in questa esercitazione si aggiornerà i dati correlati mediante l'aggiornamento di campi di chiave esterni e le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="582ad-108">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="582ad-109">Le illustrazioni seguenti mostrano alcune delle pagine che verranno utilizzate.</span><span class="sxs-lookup"><span data-stu-id="582ad-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Pagina Modifica corso](update-related-data/_static/course-edit.png)

![Pagina Modifica Instructor](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="582ad-112">Personalizzare la creazione e le pagine di modifica per i corsi</span><span class="sxs-lookup"><span data-stu-id="582ad-112">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="582ad-113">Quando viene creata una nuova entità course, deve avere una relazione a una categoria esistente.</span><span class="sxs-lookup"><span data-stu-id="582ad-113">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="582ad-114">A tale scopo, il codice di scaffolding include i metodi del controller e creare e modificare le viste che includono un elenco a discesa per la selezione del reparto.</span><span class="sxs-lookup"><span data-stu-id="582ad-114">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="582ad-115">Imposta l'elenco a discesa il `Course.DepartmentID` le proprietà di chiave esterna, e che tutte le esigenze di Entity Framework per caricare il `Department` proprietà di navigazione con l'entità Department appropriato.</span><span class="sxs-lookup"><span data-stu-id="582ad-115">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="582ad-116">Verranno utilizzare il codice di supporto temporaneo, ma modificare leggermente per aggiungere la gestione degli errori e ordinare l'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="582ad-116">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="582ad-117">In *CoursesController.cs*, eliminare i quattro metodi di creazione e modifica e sostituirli con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="582ad-117">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="582ad-118">Dopo il `Edit` metodo HttpPost, creare un nuovo metodo che consente di caricare le informazioni di reparto per l'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="582ad-118">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="582ad-119">Il `PopulateDepartmentsDropDownList` metodo ottiene un elenco di tutti i reparti ordinati per nome, crea un `SelectList` raccolta per un elenco a discesa e passa l'insieme alla vista `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="582ad-119">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="582ad-120">Il metodo accetta il parametro facoltativo `selectedDepartment` parametro che consente al codice chiamante specificare l'elemento selezionato quando l'elenco a discesa viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="582ad-120">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="582ad-121">La vista verrà passare il nome "DepartmentID" al `<select>` helper di tag e il supporto in grado di conoscere la ricerca di `ViewBag` dell'oggetto per un `SelectList` denominato "DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="582ad-121">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="582ad-122">Il HttpGet `Create` chiamate al metodo di `PopulateDepartmentsDropDownList` metodo senza impostare l'elemento selezionato, perché per un nuovo corso il reparto non è ancora stabilito:</span><span class="sxs-lookup"><span data-stu-id="582ad-122">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="582ad-123">Il HttpGet `Edit` metodo imposta l'elemento selezionato, in base all'ID del reparto che è già assegnato al corso modificato:</span><span class="sxs-lookup"><span data-stu-id="582ad-123">The HttpGet `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="582ad-124">Per entrambi i metodi di HttpPost `Create` e `Edit` inoltre includere codice che imposta l'elemento selezionato quando vengono nuovamente la pagina dopo un errore.</span><span class="sxs-lookup"><span data-stu-id="582ad-124">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="582ad-125">Ciò garantisce che quando la pagina viene visualizzata per mostrare il messaggio di errore, è stato selezionato il reparto rimane selezionato.</span><span class="sxs-lookup"><span data-stu-id="582ad-125">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="582ad-126">Aggiungere. AsNoTracking dettagli ed eliminare i metodi</span><span class="sxs-lookup"><span data-stu-id="582ad-126">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="582ad-127">Per ottimizzare le prestazioni del corso dettagli e pagine di eliminazione, aggiungere `AsNoTracking` chiama il `Details` e HttpGet `Delete` metodi.</span><span class="sxs-lookup"><span data-stu-id="582ad-127">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="582ad-128">Modificare le viste del corso</span><span class="sxs-lookup"><span data-stu-id="582ad-128">Modify the Course views</span></span>

<span data-ttu-id="582ad-129">In *Views/Courses/Create.cshtml*, aggiungere un'opzione "Selezionare reparto" per il **reparto** elenco a discesa elenco, modificare la didascalia da **DepartmentID** a ** Reparto**e aggiungere un messaggio di convalida.</span><span class="sxs-lookup"><span data-stu-id="582ad-129">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="582ad-130">In *Views/Courses/Edit.cshtml*, apportare la stessa modifica per il campo reparto consente di verificare in *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="582ad-130">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="582ad-131">Anche in *Views/Courses/Edit.cshtml*, aggiungere un campo numerico corso prima di **titolo** campo.</span><span class="sxs-lookup"><span data-stu-id="582ad-131">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="582ad-132">Poiché il numero di corso è la chiave primaria, viene visualizzato, ma non può essere modificata.</span><span class="sxs-lookup"><span data-stu-id="582ad-132">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="582ad-133">È già presente un campo nascosto (`<input type="hidden">`) per il numero di corso nella visualizzazione di modifica.</span><span class="sxs-lookup"><span data-stu-id="582ad-133">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="582ad-134">Aggiunta di un `<label>` helper di tag non di eliminare la necessità per il campo nascosto poiché non causare il numero del corso da includere nei dati inviati quando l'utente fa clic **salvare** sul **modifica** pagina.</span><span class="sxs-lookup"><span data-stu-id="582ad-134">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="582ad-135">In *Views/Courses/Delete.cshtml*, aggiungere un campo del numero di corso nella parte superiore e modificare l'ID del reparto al nome del reparto.</span><span class="sxs-lookup"><span data-stu-id="582ad-135">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="582ad-136">In *Views/Courses/Details.cshtml*, apportare la stessa modifica effettuati per *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="582ad-136">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="582ad-137">Le pagine del corso di test</span><span class="sxs-lookup"><span data-stu-id="582ad-137">Test the Course pages</span></span>

<span data-ttu-id="582ad-138">Eseguire l'app, selezionare il **corsi** scheda, fare clic su **Crea nuovo**e immettere i dati per un nuovo corso:</span><span class="sxs-lookup"><span data-stu-id="582ad-138">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Pagina Crea corso](update-related-data/_static/course-create.png)

<span data-ttu-id="582ad-140">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="582ad-140">Click **Create**.</span></span> <span data-ttu-id="582ad-141">Verrà visualizzata la pagina di indice corsi con la nuova linea aggiunto all'elenco.</span><span class="sxs-lookup"><span data-stu-id="582ad-141">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="582ad-142">Il nome di reparto nell'elenco di pagina di indice deriva dalla proprietà di navigazione, che mostra che la relazione è stata stabilita correttamente.</span><span class="sxs-lookup"><span data-stu-id="582ad-142">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="582ad-143">Fare clic su **modifica** su un corso nella pagina di indice corsi.</span><span class="sxs-lookup"><span data-stu-id="582ad-143">Click **Edit** on a course in the Courses Index page.</span></span>

![Pagina Modifica corso](update-related-data/_static/course-edit.png)

<span data-ttu-id="582ad-145">Modificare i dati nella pagina e fare clic su **salvare**.</span><span class="sxs-lookup"><span data-stu-id="582ad-145">Change data on the page and click **Save**.</span></span> <span data-ttu-id="582ad-146">Verrà visualizzata la pagina di indice corsi con i dati di corso aggiornato.</span><span class="sxs-lookup"><span data-stu-id="582ad-146">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="582ad-147">Aggiungere una pagina di modifica per i docenti</span><span class="sxs-lookup"><span data-stu-id="582ad-147">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="582ad-148">Quando si modifica un record istruttore, si desidera essere in grado di aggiornare l'assegnazione dell'ufficio dell'insegnante.</span><span class="sxs-lookup"><span data-stu-id="582ad-148">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="582ad-149">Entità Instructor ha una relazione uno-a-zero-o-uno con l'entità OfficeAssignment, ovvero che il codice deve gestire le situazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="582ad-149">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="582ad-150">Se l'utente cancella l'assegnazione di office e che inizialmente aveva un valore, eliminare l'entità OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="582ad-150">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="582ad-151">Se l'utente immette un valore di assegnazione di office ed era originariamente vuoto, creare una nuova entità OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="582ad-151">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="582ad-152">Se l'utente modifica il valore di un'assegnazione di office, modificare il valore in un'entità OfficeAssignment esistente.</span><span class="sxs-lookup"><span data-stu-id="582ad-152">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="582ad-153">Aggiornare il controller istruttori</span><span class="sxs-lookup"><span data-stu-id="582ad-153">Update the Instructors controller</span></span>

<span data-ttu-id="582ad-154">In *InstructorsController.cs*, modificare il codice di HttpGet `Edit` metodo in modo che carica l'entità Instructor `OfficeAssignment` proprietà di navigazione e chiamate `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="582ad-154">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="582ad-155">Sostituire il HttpPost `Edit` metodo con il codice seguente per gestire gli aggiornamenti di assegnazione di office:</span><span class="sxs-lookup"><span data-stu-id="582ad-155">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="582ad-156">Il codice esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="582ad-156">The code does the following:</span></span>

-  <span data-ttu-id="582ad-157">Modifica il nome del metodo per `EditPost` perché la firma è ora come il HttpGet `Edit` metodo (il `ActionName` attributo specifica che il `/Edit/` URL viene ancora utilizzato).</span><span class="sxs-lookup"><span data-stu-id="582ad-157">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="582ad-158">Ottiene l'entità Instructor corrente dal database utilizzando eager caricamento per il `OfficeAssignment` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="582ad-158">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="582ad-159">Ciò equivale il il HttpGet `Edit` metodo.</span><span class="sxs-lookup"><span data-stu-id="582ad-159">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="582ad-160">Aggiorna l'entità Instructor recuperata con valori dallo strumento di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="582ad-160">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="582ad-161">Il `TryUpdateModel` overload consente all'elenco elementi consentiti le proprietà che si desidera includere.</span><span class="sxs-lookup"><span data-stu-id="582ad-161">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="582ad-162">In questo modo la registrazione, come illustrato nel [esercitazione secondo](crud.md).</span><span class="sxs-lookup"><span data-stu-id="582ad-162">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   <span data-ttu-id="582ad-163">Se il percorso di office è vuoto, imposta la proprietà Instructor.OfficeAssignment su null in modo che la riga correlata nella tabella OfficeAssignment verrà eliminata.</span><span class="sxs-lookup"><span data-stu-id="582ad-163">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="582ad-164">Salva le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="582ad-164">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="582ad-165">Aggiornare la visualizzazione di modifica Instructor</span><span class="sxs-lookup"><span data-stu-id="582ad-165">Update the Instructor Edit view</span></span>

<span data-ttu-id="582ad-166">In *Views/Instructors/Edit.cshtml*, aggiungere un nuovo campo per modificare il percorso di office, al termine prima di **salvare** pulsante:</span><span class="sxs-lookup"><span data-stu-id="582ad-166">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="582ad-167">Eseguire l'app, selezionare il **i docenti** scheda e quindi fare clic su **modifica** su un docente.</span><span class="sxs-lookup"><span data-stu-id="582ad-167">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="582ad-168">Modifica il **ufficio** e fare clic su **salvare**.</span><span class="sxs-lookup"><span data-stu-id="582ad-168">Change the **Office Location** and click **Save**.</span></span>

![Pagina Modifica Instructor](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="582ad-170">Aggiungere esercitazioni per la pagina Modifica Instructor</span><span class="sxs-lookup"><span data-stu-id="582ad-170">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="582ad-171">I docenti potrebbero indicare un numero qualsiasi di corsi.</span><span class="sxs-lookup"><span data-stu-id="582ad-171">Instructors may teach any number of courses.</span></span> <span data-ttu-id="582ad-172">A questo punto sarà possibile migliorare la pagina Modifica istruttore aggiungendo la possibilità di modificare le assegnazioni di corso usando un gruppo di caselle di controllo, come illustrato nella schermata seguente:</span><span class="sxs-lookup"><span data-stu-id="582ad-172">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Pagina Modifica istruttore corsi](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="582ad-174">La relazione tra le entità Course e istruttore è molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="582ad-174">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="582ad-175">Per aggiungere e rimuovere le relazioni, aggiungere e rimuovere le entità da e verso il set di entità CourseAssignments join.</span><span class="sxs-lookup"><span data-stu-id="582ad-175">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="582ad-176">L'interfaccia utente che consente di modificare quali corsi un docente è assegnato a è un gruppo di caselle di controllo.</span><span class="sxs-lookup"><span data-stu-id="582ad-176">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="582ad-177">Viene visualizzata una casella di controllo per ogni corso nel database e quelle che è attualmente assegnata istruttore selezionati.</span><span class="sxs-lookup"><span data-stu-id="582ad-177">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="582ad-178">L'utente può selezionare o deselezionare le caselle di controllo per modificare le assegnazioni dei corsi.</span><span class="sxs-lookup"><span data-stu-id="582ad-178">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="582ad-179">Se il numero di corsi è molto superiore, probabilmente si desidera utilizzare un altro metodo di presentazione dei dati nella visualizzazione, ma utilizzare lo stesso metodo di manipolazione di un'entità di join per creare o eliminare le relazioni.</span><span class="sxs-lookup"><span data-stu-id="582ad-179">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="582ad-180">Aggiornare il controller istruttori</span><span class="sxs-lookup"><span data-stu-id="582ad-180">Update the Instructors controller</span></span>

<span data-ttu-id="582ad-181">Per fornire dati per la visualizzazione per un elenco di caselle di controllo, si utilizzerà una classe di modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="582ad-181">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="582ad-182">Creare *AssignedCourseData.cs* nel *SchoolViewModels* cartella e sostituire il codice esistente con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="582ad-182">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="582ad-183">In *InstructorsController.cs*, sostituire il HttpGet `Edit` metodo con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="582ad-183">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="582ad-184">Le modifiche sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="582ad-184">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="582ad-185">Il codice aggiunge il caricamento immediato per la `Courses` proprietà di navigazione e chiama il nuovo `PopulateAssignedCourseData` metodo per fornire informazioni per la matrice di casella di controllo utilizzando il `AssignedCourseData` visualizzare una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="582ad-185">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="582ad-186">Il codice di `PopulateAssignedCourseData` metodo legge tutte le entità Course per caricare un elenco di corsi utilizzando la classe di modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="582ad-186">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="582ad-187">Per ogni corso, il codice verifica se il corso è presente nell'istruttore `Courses` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="582ad-187">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="582ad-188">Per creare ricerca efficiente quando si verifica se un corso è assegnato all'istruttore, corsi assegnati all'istruttore vengono inseriti in un `HashSet` insieme.</span><span class="sxs-lookup"><span data-stu-id="582ad-188">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="582ad-189">Il `Assigned` è impostata su true per i corsi istruttore viene assegnato a.</span><span class="sxs-lookup"><span data-stu-id="582ad-189">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="582ad-190">La vista utilizzerà questa proprietà per determinare quale controllo caselle devono essere visualizzate come selezionato.</span><span class="sxs-lookup"><span data-stu-id="582ad-190">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="582ad-191">Infine, tale elenco viene passato alla visualizzazione in `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="582ad-191">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="582ad-192">Successivamente, aggiungere il codice che viene eseguito quando l'utente fa clic **salvare**.</span><span class="sxs-lookup"><span data-stu-id="582ad-192">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="582ad-193">Sostituire il `EditPost` (metodo) con il seguente codice, quindi aggiungere un nuovo metodo che aggiorna il `Courses` proprietà di navigazione di entità Instructor.</span><span class="sxs-lookup"><span data-stu-id="582ad-193">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="582ad-194">La firma del metodo è diversa dal HttpGet `Edit` metodo, pertanto, il nome del metodo di modifica da `EditPost` al `Edit`.</span><span class="sxs-lookup"><span data-stu-id="582ad-194">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="582ad-195">Poiché la vista non dispone di una raccolta di entità corso, lo strumento di associazione del modello non può aggiornare automaticamente il `CourseAssignments` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="582ad-195">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="582ad-196">Anziché utilizzare lo strumento di associazione del modello per aggiornare il `CourseAssignments` proprietà di navigazione è eseguire questa operazione nella nuova `UpdateInstructorCourses` metodo.</span><span class="sxs-lookup"><span data-stu-id="582ad-196">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="582ad-197">È pertanto necessario escludere il `CourseAssignments` proprietà dall'associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="582ad-197">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="582ad-198">Questo non richiede modifiche al codice che chiama `TryUpdateModel` poiché si utilizza l'overload whitelist e `CourseAssignments` non è presente nell'elenco di inclusione.</span><span class="sxs-lookup"><span data-stu-id="582ad-198">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="582ad-199">Se nessun controllo caselle sono state selezionate, il codice in `UpdateInstructorCourses` Inizializza il `CourseAssignments` proprietà di navigazione e restituisce una raccolta vuota:</span><span class="sxs-lookup"><span data-stu-id="582ad-199">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="582ad-200">Il codice quindi esegue il ciclo di tutti i corsi nel database e controlla ogni corso rispetto a quelli attualmente assegnati all'istruttore rispetto a quelli che sono stati selezionati nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="582ad-200">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="582ad-201">Per facilitare le ricerche efficienti, quest'ultime due raccolte vengono archiviate in `HashSet` oggetti.</span><span class="sxs-lookup"><span data-stu-id="582ad-201">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="582ad-202">Se è stata selezionata la casella di controllo per un corso ma non è corso la `Instructor.CourseAssignments` proprietà di navigazione, la linea viene aggiunto alla raccolta nella proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="582ad-202">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="582ad-203">Se non è stata selezionata la casella di controllo per un corso, ma il corso è il `Instructor.CourseAssignments` proprietà di navigazione, la linea viene rimossa dalla proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="582ad-203">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="582ad-204">Aggiornare le visualizzazioni Instructor</span><span class="sxs-lookup"><span data-stu-id="582ad-204">Update the Instructor views</span></span>

<span data-ttu-id="582ad-205">In *Views/Instructors/Edit.cshtml*, aggiungere un **corsi** campo con una matrice di caselle di controllo aggiungendo il seguente codice immediatamente dopo il `div` elementi per il **Office ** campo e prima di `div` elemento per il **salvare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="582ad-205">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE] 
> <span data-ttu-id="582ad-206">Quando si incolla il codice in Visual Studio, le interruzioni di riga verranno modificate in modo che interrompe il codice.</span><span class="sxs-lookup"><span data-stu-id="582ad-206">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="582ad-207">Premere Ctrl + Z di una volta per annullare la formattazione automatica.</span><span class="sxs-lookup"><span data-stu-id="582ad-207">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="582ad-208">Ciò consente di risolvere le interruzioni di riga in modo che vengano visualizzate come gli elementi visualizzati.</span><span class="sxs-lookup"><span data-stu-id="582ad-208">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="582ad-209">Il rientro non deve necessariamente essere perfetto, ma la `@</tr><tr>`, `@:<td>`, `@:</td>`, e `@:</tr>` righe devono essere su una singola riga come illustrato o si otterrà un errore di runtime.</span><span class="sxs-lookup"><span data-stu-id="582ad-209">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="582ad-210">Con il blocco di nuovo codice selezionato, premere Tab tre volte per allineare il nuovo codice con il codice esistente.</span><span class="sxs-lookup"><span data-stu-id="582ad-210">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="582ad-211">Questo codice crea una tabella HTML che ha tre colonne.</span><span class="sxs-lookup"><span data-stu-id="582ad-211">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="582ad-212">In ogni colonna è seguita da una didascalia che include il numero di corso e il titolo di una casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="582ad-212">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="582ad-213">Tutte le caselle di controllo con lo stesso nome ("selectedCourses"), che informa il gestore di associazione del modello in cui devono essere considerate come un gruppo.</span><span class="sxs-lookup"><span data-stu-id="582ad-213">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="582ad-214">L'attributo value di ogni casella di controllo è impostata sul valore di `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="582ad-214">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="582ad-215">Quando viene eseguito il postback della pagina, il gestore di associazione del modello passa una matrice per il controller che include il `CourseID` i valori per solo le caselle di controllo selezionate.</span><span class="sxs-lookup"><span data-stu-id="582ad-215">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="582ad-216">Quando le caselle di controllo sono inizialmente eseguito il rendering, quelli per i corsi assegnati all'istruttore sono controllati gli attributi, che seleziona (Display li selezionate).</span><span class="sxs-lookup"><span data-stu-id="582ad-216">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="582ad-217">Eseguire l'app, selezionare il **i docenti** scheda e fare clic su **modifica** su un docente per visualizzare il **modifica** pagina.</span><span class="sxs-lookup"><span data-stu-id="582ad-217">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Pagina Modifica istruttore corsi](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="582ad-219">Modificare alcune esercitazioni e fare clic su Salva.</span><span class="sxs-lookup"><span data-stu-id="582ad-219">Change some course assignments and click Save.</span></span> <span data-ttu-id="582ad-220">Le modifiche apportate vengono riflesse nella pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="582ad-220">The changes you make are reflected on the Index page.</span></span>

> [!NOTE] 
> <span data-ttu-id="582ad-221">L'approccio adottato qui per modificare i dati del corso istruttore funziona anche quando è presente un numero limitato di corsi.</span><span class="sxs-lookup"><span data-stu-id="582ad-221">The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="582ad-222">Per le raccolte che sono più grandi, un'interfaccia utente differente e un altro metodo di aggiornamento sono necessarie.</span><span class="sxs-lookup"><span data-stu-id="582ad-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="582ad-223">Aggiornare la pagina di eliminazione</span><span class="sxs-lookup"><span data-stu-id="582ad-223">Update the Delete page</span></span>

<span data-ttu-id="582ad-224">In *InstructorsController.cs*, eliminare il `DeleteConfirmed` (metodo) e l'inserimento di codice seguente al suo posto.</span><span class="sxs-lookup"><span data-stu-id="582ad-224">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="582ad-225">Questo codice rende le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="582ad-225">This code makes the following changes:</span></span>

* <span data-ttu-id="582ad-226">Eager fa caricamento per il `CourseAssignments` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="582ad-226">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="582ad-227">È necessario includere questa o EF non rileveranno correlati `CourseAssignment` entità e non di eliminarli.</span><span class="sxs-lookup"><span data-stu-id="582ad-227">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="582ad-228">Per evitare la necessità di leggerli qui è possibile configurare eliminazione a catena nel database.</span><span class="sxs-lookup"><span data-stu-id="582ad-228">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="582ad-229">Se istruttore da eliminare viene assegnato come responsabile per i reparti, rimuove l'assegnazione istruttore i reparti.</span><span class="sxs-lookup"><span data-stu-id="582ad-229">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="582ad-230">Aggiungere la pagina Crea ufficio e corsi</span><span class="sxs-lookup"><span data-stu-id="582ad-230">Add office location and courses to the Create page</span></span>

<span data-ttu-id="582ad-231">In *InstructorsController.cs*, eliminare il HttpGet e HttpPost `Create` metodi e quindi aggiungere il codice seguente al loro posto:</span><span class="sxs-lookup"><span data-stu-id="582ad-231">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="582ad-232">Questo codice è simile a quello visualizzato per il `Edit` metodi, ad eccezione che inizialmente non corsi selezionati.</span><span class="sxs-lookup"><span data-stu-id="582ad-232">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="582ad-233">Il HttpGet `Create` chiamate al metodo di `PopulateAssignedCourseData` metodo non perché potrebbero essere presenti corsi selezionati ma in ordine per fornire una raccolta vuota per il `foreach` ciclo nella vista (in caso contrario Visualizza codice genera un'eccezione di riferimento null).</span><span class="sxs-lookup"><span data-stu-id="582ad-233">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="582ad-234">HttpPost `Create` metodo aggiunge ogni corso selezionato per il `CourseAssignments` proprietà di navigazione prima che verifica la presenza di errori di convalida e aggiunge il nuovo docente al database.</span><span class="sxs-lookup"><span data-stu-id="582ad-234">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="582ad-235">Corsi vengono aggiunti anche se sono presenti errori del modello in modo che quando sono presenti errori del modello (per un esempio, l'utente con chiave in una data non valida) e la pagina viene visualizzata con un messaggio di errore, le selezioni corso apportate vengono automaticamente ripristinate.</span><span class="sxs-lookup"><span data-stu-id="582ad-235">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="582ad-236">Si noti che per poter essere in grado di aggiungere i corsi per il `CourseAssignments` proprietà di navigazione, è necessario inizializzare la proprietà su una raccolta vuota:</span><span class="sxs-lookup"><span data-stu-id="582ad-236">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="582ad-237">Come alternativa a questa operazione nel codice del controller, è possibile usare il modello istruttore modificando il metodo Get della proprietà per creare automaticamente la raccolta se non esiste, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="582ad-237">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

<span data-ttu-id="582ad-238">Se si modifica il `CourseAssignments` proprietà in questo modo, è possibile rimuovere il codice di inizializzazione di proprietà espliciti nel controller.</span><span class="sxs-lookup"><span data-stu-id="582ad-238">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="582ad-239">In *Views/Instructor/Create.cshtml*, aggiungere una casella di testo percorso office caselle di controllo e per i corsi prima pulsante Invia.</span><span class="sxs-lookup"><span data-stu-id="582ad-239">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="582ad-240">Come nel caso di modifica pagina [correggere la formattazione se Visual Studio riformatta il codice quando lo si incolla](#notepad).</span><span class="sxs-lookup"><span data-stu-id="582ad-240">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="582ad-241">Verificare l'esecuzione dell'app e la creazione di un docente.</span><span class="sxs-lookup"><span data-stu-id="582ad-241">Test by running the app and creating an instructor.</span></span> 

## <a name="handling-transactions"></a><span data-ttu-id="582ad-242">Gestione delle transazioni</span><span class="sxs-lookup"><span data-stu-id="582ad-242">Handling Transactions</span></span>

<span data-ttu-id="582ad-243">Come spiegato nel [esercitazione CRUD](crud.md), Entity Framework implementa in modo implicito le transazioni.</span><span class="sxs-lookup"><span data-stu-id="582ad-243">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="582ad-244">Per scenari in cui è necessario più controllare, ad esempio, se si desidera includere operazioni eseguite all'esterno di Entity Framework in una transazione, vedere [transazioni](https://docs.microsoft.com/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="582ad-244">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="582ad-245">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="582ad-245">Summary</span></span>

<span data-ttu-id="582ad-246">Introduzione all'utilizzo di dati correlati sono stati completati.</span><span class="sxs-lookup"><span data-stu-id="582ad-246">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="582ad-247">Nella prossima esercitazione verrà visualizzato come gestire i conflitti di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="582ad-247">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="582ad-248">[Precedente](read-related-data.md)
[Successivo](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="582ad-248">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>  
