---
title: ASP.NET Core MVC con EF Core - Aggiornare dati correlati - 7 di 10
author: rick-anderson
description: In questa esercitazione verrà effettuato l'aggiornamento di dati correlati tramite l'aggiornamento di campi di chiave esterna e proprietà di navigazione.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 37985c945f2e4b15cfcefb0c126c3209e0bdeac4
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090732"
---
# <a name="aspnet-core-mvc-with-ef-core---update-related-data---7-of-10"></a><span data-ttu-id="53ad4-103">ASP.NET Core MVC con EF Core - Aggiornare dati correlati - 7 di 10</span><span class="sxs-lookup"><span data-stu-id="53ad4-103">ASP.NET Core MVC with EF Core - Update Related Data - 7 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="53ad4-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="53ad4-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="53ad4-105">L'applicazione Web di esempio Contoso University illustra come creare applicazioni Web ASP.NET Core MVC con Entity Framework Core e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="53ad4-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="53ad4-106">Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="53ad4-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="53ad4-107">Nell'esercitazione precedente sono stati visualizzati dati correlati. In questa esercitazione i dati correlati verranno aggiornati tramite l'aggiornamento di campi di chiave esterna e proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="53ad4-107">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="53ad4-108">Le figure seguenti illustrano alcune delle pagine che verranno usate.</span><span class="sxs-lookup"><span data-stu-id="53ad4-108">The following illustrations show some of the pages that you'll work with.</span></span>

![Pagina di modifica del corso](update-related-data/_static/course-edit.png)

![Pagina di modifica dell'insegnante](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="53ad4-111">Personalizzare le pagine di creazione e di modifica per i corsi</span><span class="sxs-lookup"><span data-stu-id="53ad4-111">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="53ad4-112">Quando viene creata, una nuova entità corso deve essere in relazione con un dipartimento esistente.</span><span class="sxs-lookup"><span data-stu-id="53ad4-112">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="53ad4-113">Per semplificare il raggiungimento di questo obiettivo, il codice con scaffolding include i metodi del controller e le visualizzazioni di creazione e modifica includono un elenco a discesa per la selezione del dipartimento.</span><span class="sxs-lookup"><span data-stu-id="53ad4-113">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="53ad4-114">L'elenco a discesa imposta la proprietà di chiave esterna `Course.DepartmentID`. Questo è tutto ciò che serve a Entity Framework per caricare la proprietà di navigazione `Department` con l'entità Department appropriata.</span><span class="sxs-lookup"><span data-stu-id="53ad4-114">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="53ad4-115">Verrà usato il codice con scaffolding, che però verrà modificato leggermente per aggiungere la gestione degli errori e l'ordinamento dell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="53ad4-115">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="53ad4-116">In *CoursesController.cs* eliminare i quattro metodi Create ed Edit e sostituirli con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="53ad4-116">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="53ad4-117">Dopo il metodo `Edit` HttpPost, creare un nuovo metodo che carichi le informazioni di dipartimento per l'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="53ad4-117">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="53ad4-118">Il metodo `PopulateDepartmentsDropDownList` ottiene un elenco di tutti i dipartimenti ordinato per nome, crea una raccolta `SelectList` per un elenco a discesa e passa tale raccolta alla visualizzazione in `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="53ad4-118">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="53ad4-119">Il metodo accetta il parametro facoltativo `selectedDepartment`, che consente al codice chiamante di specificare l'elemento che deve essere selezionato quando viene eseguito il rendering dell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="53ad4-119">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="53ad4-120">La visualizzazione passerà il nome "DepartmentID" all'helper tag `<select>`, che quindi saprà di dover cercare nell'oggetto `ViewBag` una raccolta `SelectList` denominata "DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="53ad4-120">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="53ad4-121">Il metodo `Create` HttpGet chiama il metodo `PopulateDepartmentsDropDownList` senza impostare l'elemento selezionato, perché per un nuovo corso il dipartimento non è ancora stabilito:</span><span class="sxs-lookup"><span data-stu-id="53ad4-121">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department isn't established yet:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="53ad4-122">Il metodo `Edit` HttpGet imposta l'elemento selezionato, in base all'ID del dipartimento già assegnato al corso in fase di modifica:</span><span class="sxs-lookup"><span data-stu-id="53ad4-122">The HttpGet `Edit` method sets the selected item, based on the ID of the department that's already assigned to the course being edited:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="53ad4-123">I metodi HttpPost sia per `Create` che per `Edit` includono anche codice che imposta l'elemento selezionato quando la pagina viene visualizzata di nuovo dopo un errore.</span><span class="sxs-lookup"><span data-stu-id="53ad4-123">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="53ad4-124">Ciò garantisce che, quando la pagina viene visualizzata di nuovo per mostrare il messaggio di errore, il dipartimento selezionato rimane selezionato.</span><span class="sxs-lookup"><span data-stu-id="53ad4-124">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="53ad4-125">Aggiungere .AsNoTracking ai metodi Details e Delete</span><span class="sxs-lookup"><span data-stu-id="53ad4-125">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="53ad4-126">Per ottimizzare le prestazioni delle pagine dei dettagli e di eliminazione del corso, aggiungere chiamate a `AsNoTracking` nei metodi `Details` e `Delete` HttpGet.</span><span class="sxs-lookup"><span data-stu-id="53ad4-126">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="53ad4-127">Modificare le visualizzazioni dei corsi</span><span class="sxs-lookup"><span data-stu-id="53ad4-127">Modify the Course views</span></span>

<span data-ttu-id="53ad4-128">In *Views/Courses/Create.cshtml* aggiungere l'opzione "Select Department" (Selezionare il dipartimento) all'elenco a discesa **Department** (Dipartimento), modificare la didascalia da **DepartmentID** (ID dipartimento) a **Department** (Dipartimento) e aggiungere un messaggio di convalida.</span><span class="sxs-lookup"><span data-stu-id="53ad4-128">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="53ad4-129">In *Views/Courses/Edit.cshtml* apportare al campo Department (Dipartimento) la stessa modifica effettuata in *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="53ad4-129">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="53ad4-130">In *Views/Courses/Edit.cshtml* aggiungere anche il campo per il numero di corso prima del campo **Title** (Titolo).</span><span class="sxs-lookup"><span data-stu-id="53ad4-130">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="53ad4-131">Poiché il numero di corso è la chiave primaria, viene visualizzato, ma non può essere modificato.</span><span class="sxs-lookup"><span data-stu-id="53ad4-131">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="53ad4-132">Per il numero di corso è già presente un campo nascosto (`<input type="hidden">`) nella visualizzazione di modifica.</span><span class="sxs-lookup"><span data-stu-id="53ad4-132">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="53ad4-133">L'aggiunta di un helper tag `<label>` non elimina la necessità del campo nascosto, poiché senza di questo il numero di corso non viene incluso nei dati inviati quando l'utente fa clic su **Save** (Salva) nella pagina **Edit** (Modifica).</span><span class="sxs-lookup"><span data-stu-id="53ad4-133">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="53ad4-134">In *Views/Courses/Delete.cshtml* aggiungere il campo del numero di corso nella parte superiore e modificare l'ID del dipartimento nel nome del dipartimento.</span><span class="sxs-lookup"><span data-stu-id="53ad4-134">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="53ad4-135">In *Views/Courses/Details.cshtml* apportare la stessa modifica appena effettuata in *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="53ad4-135">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="53ad4-136">Testare le pagine del corso</span><span class="sxs-lookup"><span data-stu-id="53ad4-136">Test the Course pages</span></span>

<span data-ttu-id="53ad4-137">Eseguire l'app, selezionare la scheda **Courses** (Corsi), fare clic su **Create New** (Crea nuovo) e immettere i dati per un nuovo corso:</span><span class="sxs-lookup"><span data-stu-id="53ad4-137">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Pagina di creazione del corso](update-related-data/_static/course-create.png)

<span data-ttu-id="53ad4-139">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="53ad4-139">Click **Create**.</span></span> <span data-ttu-id="53ad4-140">La pagina di indice dei corsi viene visualizzata con il nuovo corso aggiunto all'elenco.</span><span class="sxs-lookup"><span data-stu-id="53ad4-140">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="53ad4-141">Il nome del dipartimento nell'elenco della pagina di indice deriva dalla proprietà di navigazione, che mostra che la relazione è stata stabilita correttamente.</span><span class="sxs-lookup"><span data-stu-id="53ad4-141">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="53ad4-142">Fare clic su **Edit** (Modifica) per un corso nella pagina di indice dei corsi.</span><span class="sxs-lookup"><span data-stu-id="53ad4-142">Click **Edit** on a course in the Courses Index page.</span></span>

![Pagina di modifica del corso](update-related-data/_static/course-edit.png)

<span data-ttu-id="53ad4-144">Modificare i dati nella pagina e fare clic su **Save** (Salva).</span><span class="sxs-lookup"><span data-stu-id="53ad4-144">Change data on the page and click **Save**.</span></span> <span data-ttu-id="53ad4-145">La pagina di indice dei corsi verrà visualizzata con i dati del corso aggiornati.</span><span class="sxs-lookup"><span data-stu-id="53ad4-145">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="53ad4-146">Aggiungere una pagina di modifica per gli insegnanti</span><span class="sxs-lookup"><span data-stu-id="53ad4-146">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="53ad4-147">Quando si modifica il record di un insegnante, è necessario essere in grado di aggiornare l'assegnazione dell'ufficio.</span><span class="sxs-lookup"><span data-stu-id="53ad4-147">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="53ad4-148">L'entità Instructor ha una relazione uno-a-zero-o-uno con l'entità OfficeAssignment. Ciò significa che il codice deve gestire le situazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="53ad4-148">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="53ad4-149">Se l'utente cancella l'assegnazione di un ufficio e questa originariamente rappresentava l'unico valore, eliminare l'entità OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="53ad4-149">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="53ad4-150">Se l'utente immette l'assegnazione di un ufficio e prima non c'era alcun valore, creare una nuova entità OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="53ad4-150">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="53ad4-151">Se l'utente modifica il valore di un'assegnazione di ufficio, modificare il valore in un'entità OfficeAssignment esistente.</span><span class="sxs-lookup"><span data-stu-id="53ad4-151">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="53ad4-152">Aggiornare il controller Instructors</span><span class="sxs-lookup"><span data-stu-id="53ad4-152">Update the Instructors controller</span></span>

<span data-ttu-id="53ad4-153">In *InstructorsController.cs* modificare il codice nel metodo `Edit` HttpGet in modo che carichi la proprietà di navigazione `OfficeAssignment` dell'entità Instructor e chiami `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="53ad4-153">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="53ad4-154">Sostituire il metodo `Edit` HttpPost con il codice seguente per gestire gli aggiornamenti delle assegnazioni di ufficio:</span><span class="sxs-lookup"><span data-stu-id="53ad4-154">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="53ad4-155">Il codice effettua queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="53ad4-155">The code does the following:</span></span>

-  <span data-ttu-id="53ad4-156">Modifica il nome del metodo in `EditPost` perché ora la firma è la stessa del metodo `Edit` HttpGet (l'attributo `ActionName` specifica che l'URL `/Edit/` viene ancora usato).</span><span class="sxs-lookup"><span data-stu-id="53ad4-156">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="53ad4-157">Ottiene l'entità Instructor corrente dal database tramite il caricamento eager per la proprietà di navigazione `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="53ad4-157">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="53ad4-158">Ciò corrisponde a quanto effettuato nel metodo `Edit` HttpGet.</span><span class="sxs-lookup"><span data-stu-id="53ad4-158">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="53ad4-159">Aggiorna l'entità Instructor recuperata con valori dallo strumento di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="53ad4-159">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="53ad4-160">L'overload `TryUpdateModel` consente di creare un elenco elementi consentiti per le proprietà da includere.</span><span class="sxs-lookup"><span data-stu-id="53ad4-160">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="53ad4-161">In questo modo è possibile evitare l'overposting, come illustrato nella [seconda esercitazione](crud.md).</span><span class="sxs-lookup"><span data-stu-id="53ad4-161">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```

-   <span data-ttu-id="53ad4-162">Se la posizione dell'ufficio è vuota, impostare la proprietà Instructor.OfficeAssignment su Null, in modo che la riga correlata nella tabella OfficeAssignment venga eliminata.</span><span class="sxs-lookup"><span data-stu-id="53ad4-162">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="53ad4-163">Salva le modifiche nel database.</span><span class="sxs-lookup"><span data-stu-id="53ad4-163">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="53ad4-164">Aggiornare la visualizzazione di modifica dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="53ad4-164">Update the Instructor Edit view</span></span>

<span data-ttu-id="53ad4-165">In *Views/Instructors/Edit.cshtml* aggiungere un nuovo campo per la modifica della posizione dell'ufficio alla fine, prima del pulsante **Save** (Salva):</span><span class="sxs-lookup"><span data-stu-id="53ad4-165">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="53ad4-166">Eseguire l'app, selezionare la scheda **Instructors** (Insegnanti) e quindi fare clic su **Edit** (Modifica) per un insegnante.</span><span class="sxs-lookup"><span data-stu-id="53ad4-166">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="53ad4-167">Modificare **Office Location** (Posizione ufficio) e fare clic su **Save** (Salva).</span><span class="sxs-lookup"><span data-stu-id="53ad4-167">Change the **Office Location** and click **Save**.</span></span>

![Pagina di modifica dell'insegnante](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="53ad4-169">Aggiungere assegnazioni di corso nella pagina di modifica dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="53ad4-169">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="53ad4-170">Gli insegnanti possono tenere un numero qualsiasi di corsi.</span><span class="sxs-lookup"><span data-stu-id="53ad4-170">Instructors may teach any number of courses.</span></span> <span data-ttu-id="53ad4-171">A questo punto la pagina di modifica dell'insegnante verrà migliorata aggiungendo la possibilità di modificare le assegnazioni di corso tramite un gruppo di caselle di controllo, come illustrato nello screenshot seguente:</span><span class="sxs-lookup"><span data-stu-id="53ad4-171">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Pagina di modifica dell'insegnante con corsi](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="53ad4-173">Tra le entità Course e Instructor c'è una relazione molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="53ad4-173">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="53ad4-174">Per aggiungere e rimuovere relazioni, aggiungere e rimuovere entità dal set di entità di join CourseAssignments.</span><span class="sxs-lookup"><span data-stu-id="53ad4-174">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="53ad4-175">L'interfaccia utente che consente di modificare i corsi assegnati a un insegnante è costituita da un gruppo di caselle di controllo.</span><span class="sxs-lookup"><span data-stu-id="53ad4-175">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="53ad4-176">È visualizzata una casella di controllo per ogni corso nel database e le caselle corrispondenti ai corsi attualmente assegnati all'insegnante sono selezionate.</span><span class="sxs-lookup"><span data-stu-id="53ad4-176">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="53ad4-177">L'utente può selezionare e deselezionare le caselle di controllo per modificare le assegnazioni dei corsi.</span><span class="sxs-lookup"><span data-stu-id="53ad4-177">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="53ad4-178">Se il numero di corsi fosse molto superiore, sarebbe probabilmente consigliabile usare un altro metodo di presentazione dei dati nella visualizzazione, ma si userebbe lo stesso metodo di modifica di un'entità di join per creare o eliminare relazioni.</span><span class="sxs-lookup"><span data-stu-id="53ad4-178">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="53ad4-179">Aggiornare il controller Instructors</span><span class="sxs-lookup"><span data-stu-id="53ad4-179">Update the Instructors controller</span></span>

<span data-ttu-id="53ad4-180">Per fornire i dati alla visualizzazione dell'elenco di caselle di controllo, si userà una classe modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="53ad4-180">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="53ad4-181">Creare *AssignedCourseData.cs* nella cartella *SchoolViewModels* e sostituire il codice esistente con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="53ad4-181">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="53ad4-182">In *InstructorsController.cs* sostituire il metodo `Edit` HttpGet con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="53ad4-182">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="53ad4-183">Le modifiche sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="53ad4-183">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="53ad4-184">Il codice aggiunge il caricamento eager per la proprietà di navigazione `Courses` e chiama il nuovo metodo `PopulateAssignedCourseData` per fornire informazioni per la matrice di caselle di controllo tramite la classe modello di visualizzazione `AssignedCourseData`.</span><span class="sxs-lookup"><span data-stu-id="53ad4-184">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="53ad4-185">Il codice nel metodo `PopulateAssignedCourseData` legge tutte le entità Course per caricare un elenco di corsi tramite la classe modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="53ad4-185">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="53ad4-186">Per ogni corso, il codice verifica se è presente nella proprietà di navigazione `Courses` dell'insegnante.</span><span class="sxs-lookup"><span data-stu-id="53ad4-186">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="53ad4-187">Per creare un ricerca efficiente per la verifica dell'assegnazione di un corso all'insegnante, i corsi assegnati all'insegnante vengono inseriti in una raccolta `HashSet`.</span><span class="sxs-lookup"><span data-stu-id="53ad4-187">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="53ad4-188">La proprietà `Assigned` è impostata su true per i corsi assegnati all'insegnante.</span><span class="sxs-lookup"><span data-stu-id="53ad4-188">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="53ad4-189">La visualizzazione usa questa proprietà per determinare quali caselle di controllo devono essere visualizzate come selezionate.</span><span class="sxs-lookup"><span data-stu-id="53ad4-189">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="53ad4-190">L'elenco, infine, viene passato alla visualizzazione in `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="53ad4-190">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="53ad4-191">Aggiungere quindi il codice che viene eseguito quando l'utente fa clic su **Save** (Salva).</span><span class="sxs-lookup"><span data-stu-id="53ad4-191">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="53ad4-192">Sostituire il metodo `EditPost` con il codice seguente e aggiungere un nuovo metodo che aggiorni la proprietà di navigazione `Courses` dell'entità Instructor.</span><span class="sxs-lookup"><span data-stu-id="53ad4-192">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="53ad4-193">La firma del metodo è ora diversa dal metodo `Edit` HttpGet, quindi il nome del metodo cambia di nuovo da `EditPost` a `Edit`.</span><span class="sxs-lookup"><span data-stu-id="53ad4-193">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="53ad4-194">Poiché la visualizzazione non ha una raccolta di entità Course, lo strumento di associazione di modelli non può aggiornare automaticamente la proprietà di navigazione `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="53ad4-194">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="53ad4-195">Anziché usare lo strumento di associazione di modelli per aggiornare la proprietà di navigazione `CourseAssignments`, questa operazione viene eseguita nel nuovo metodo `UpdateInstructorCourses`.</span><span class="sxs-lookup"><span data-stu-id="53ad4-195">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="53ad4-196">È pertanto necessario escludere la proprietà `CourseAssignments` dall'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="53ad4-196">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="53ad4-197">Ciò non richiede modifiche al codice che chiama `TryUpdateModel`, poiché si sta usando l'overload dell'elenco elementi consentiti e `CourseAssignments` non è presente nell'elenco di inclusione.</span><span class="sxs-lookup"><span data-stu-id="53ad4-197">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="53ad4-198">Se nessuna casella di controllo è selezionata, il codice in `UpdateInstructorCourses` inizializza la proprietà di navigazione `CourseAssignments` con una raccolta vuota e torna:</span><span class="sxs-lookup"><span data-stu-id="53ad4-198">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="53ad4-199">Il codice quindi esegue il ciclo di tutti i corsi nel database e controlla ogni corso a fronte di quelli assegnati all'insegnante rispetto a quelli selezionati nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="53ad4-199">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="53ad4-200">Per facilitare l'esecuzione di ricerche efficienti, le ultime due raccolte sono archiviate all'interno di oggetti `HashSet`.</span><span class="sxs-lookup"><span data-stu-id="53ad4-200">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="53ad4-201">Se la casella di controllo di un corso è selezionata ma il corso non è presente nella proprietà di navigazione `Instructor.CourseAssignments`, il corso viene aggiunto alla raccolta nella proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="53ad4-201">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="53ad4-202">Se la casella di controllo di un corso non è selezionata ma il corso è presente nella proprietà di navigazione `Instructor.CourseAssignments`, il corso viene rimosso dalla proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="53ad4-202">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="53ad4-203">Aggiornare le visualizzazioni dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="53ad4-203">Update the Instructor views</span></span>

<span data-ttu-id="53ad4-204">In *Views/Instructors/Edit.cshtml* aggiungere un campo **Courses** (Corsi) con una matrice di caselle di controllo aggiungendo il codice seguente immediatamente dopo gli elementi `div` per il campo **Office**  (Ufficio) e prima dell'elemento `div` per il pulsante **Save** (Salva).</span><span class="sxs-lookup"><span data-stu-id="53ad4-204">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="53ad4-205">Quando si incolla il codice in Visual Studio, le interruzioni di riga vengono modificate in modo tale da danneggiare il codice.</span><span class="sxs-lookup"><span data-stu-id="53ad4-205">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="53ad4-206">Premere Ctrl + Z una volta per annullare la formattazione automatica.</span><span class="sxs-lookup"><span data-stu-id="53ad4-206">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="53ad4-207">Ciò corregge le interruzioni di riga, che vengono visualizzate come illustrato qui.</span><span class="sxs-lookup"><span data-stu-id="53ad4-207">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="53ad4-208">Il rientro non deve necessariamente essere perfetto, ma le righe `@</tr><tr>`, `@:<td>`, `@:</td>` e `@:</tr>` devono trovarsi in una sola riga, come illustrato. In caso contrario, viene visualizzato un errore di runtime.</span><span class="sxs-lookup"><span data-stu-id="53ad4-208">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="53ad4-209">Dopo aver selezionato il blocco di nuovo codice, premere Tab tre volte per allineare il nuovo codice con il codice esistente.</span><span class="sxs-lookup"><span data-stu-id="53ad4-209">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="53ad4-210">È possibile controllare lo stato del problema [qui](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="53ad4-210">You can check the status of this problem [here](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="53ad4-211">Questo codice crea una tabella HTML con tre colonne.</span><span class="sxs-lookup"><span data-stu-id="53ad4-211">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="53ad4-212">In ogni colonna è presente una casella di controllo seguita da una didascalia costituita dal numero di corso e dal titolo.</span><span class="sxs-lookup"><span data-stu-id="53ad4-212">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="53ad4-213">Le caselle di controllo hanno tutte lo stesso nome ("selectedCourses"). Questo informa lo strumento di associazione di modelli che devono essere considerate come un gruppo.</span><span class="sxs-lookup"><span data-stu-id="53ad4-213">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they're to be treated as a group.</span></span> <span data-ttu-id="53ad4-214">L'attributo value di ogni casella di controllo è impostato sul valore di `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="53ad4-214">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="53ad4-215">Quando la pagina viene pubblicata, lo strumento di associazione di modelli passa al controller una matrice costituita dai valori `CourseID` delle sole caselle di controllo selezionate.</span><span class="sxs-lookup"><span data-stu-id="53ad4-215">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="53ad4-216">Quando viene eseguito il rendering iniziale delle caselle di controllo, quelle corrispondenti ai corsi assegnati all'insegnante hanno attributi checked, che le rendono selezionate (visualizzate come selezionate).</span><span class="sxs-lookup"><span data-stu-id="53ad4-216">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="53ad4-217">Eseguire l'app, selezionare la scheda **Instructors** (Insegnanti) e fare clic su **Edit** (Modifica) per un insegnante per visualizzare la pagina **Edit** (Modifica).</span><span class="sxs-lookup"><span data-stu-id="53ad4-217">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Pagina di modifica dell'insegnante con corsi](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="53ad4-219">Modificare alcune assegnazioni di corsi e fare clic su Save (Salva).</span><span class="sxs-lookup"><span data-stu-id="53ad4-219">Change some course assignments and click Save.</span></span> <span data-ttu-id="53ad4-220">Le modifiche effettuate si riflettono nella pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="53ad4-220">The changes you make are reflected on the Index page.</span></span>

> [!NOTE]
> <span data-ttu-id="53ad4-221">L'approccio qui adottato per la modifica dei dati dei corsi degli insegnanti funziona bene quando è presente un numero limitato di corsi.</span><span class="sxs-lookup"><span data-stu-id="53ad4-221">The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="53ad4-222">Per raccolte molto più grandi, sarebbero necessari un'interfaccia utente diversa e un altro metodo di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="53ad4-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="53ad4-223">Aggiornare la pagina Delete (Elimina)</span><span class="sxs-lookup"><span data-stu-id="53ad4-223">Update the Delete page</span></span>

<span data-ttu-id="53ad4-224">In *InstructorsController.cs* eliminare il metodo `DeleteConfirmed` e sostituirlo con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="53ad4-224">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="53ad4-225">Questo codice determina le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="53ad4-225">This code makes the following changes:</span></span>

* <span data-ttu-id="53ad4-226">Esegue il caricamento eager per la proprietà di navigazione `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="53ad4-226">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="53ad4-227">È necessario includere questa proprietà o EF non sarà a conoscenza delle entità `CourseAssignment` correlate e non le eliminerà.</span><span class="sxs-lookup"><span data-stu-id="53ad4-227">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="53ad4-228">Per evitare la necessità di leggerle qui, è possibile configurare l'eliminazione a catena nel database.</span><span class="sxs-lookup"><span data-stu-id="53ad4-228">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="53ad4-229">Se l'insegnante da eliminare è assegnato come responsabile di un dipartimento, tale assegnazione viene rimossa dal dipartimento.</span><span class="sxs-lookup"><span data-stu-id="53ad4-229">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="53ad4-230">Aggiungere posizione dell'ufficio e corsi alla pagina Create (Crea)</span><span class="sxs-lookup"><span data-stu-id="53ad4-230">Add office location and courses to the Create page</span></span>

<span data-ttu-id="53ad4-231">In *InstructorsController.cs*, eliminare i metodi `Create` HttpGet e HttpPost e quindi sostituirli con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="53ad4-231">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="53ad4-232">Questo codice è simile a quello dei metodi `Edit`, ad eccezione del fatto che inizialmente non è selezionato alcun corso.</span><span class="sxs-lookup"><span data-stu-id="53ad4-232">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="53ad4-233">Il metodo `Create` HttpGet chiama il metodo `PopulateAssignedCourseData` non perché possono essere presenti corsi selezionati, ma per fornire una raccolta vuota per il ciclo `foreach` nella visualizzazioni (in caso contrario il codice di visualizzazione genera un'eccezione di riferimento Null).</span><span class="sxs-lookup"><span data-stu-id="53ad4-233">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="53ad4-234">Il metodo `Create` HttpPost aggiunge i corsi selezionati alla proprietà di navigazione `CourseAssignments` prima di controllare la presenza di errori di convalida e aggiungere il nuovo insegnante al database.</span><span class="sxs-lookup"><span data-stu-id="53ad4-234">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="53ad4-235">I corsi vengono aggiunti anche in caso di errori di modello. Quindi se si verificano errori di questo tipo (ad esempio se l'utente digita una data non valida) e la pagina viene visualizzata di nuovo con un messaggio di errore, tutte le selezioni relative ai corsi effettuate vengono ripristinate automaticamente.</span><span class="sxs-lookup"><span data-stu-id="53ad4-235">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="53ad4-236">Si noti che, perché sia possibile aggiungere corsi alla proprietà di navigazione `CourseAssignments`, è necessario inizializzare la proprietà come raccolta vuota:</span><span class="sxs-lookup"><span data-stu-id="53ad4-236">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="53ad4-237">Anziché all'interno di codice di controllo, è possibile eseguire questa operazione nel modello Instructor modificando il getter della proprietà in modo da creare automaticamente la raccolta, se questa non esiste, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="53ad4-237">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

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

<span data-ttu-id="53ad4-238">Se si modifica la proprietà `CourseAssignments` in questo modo, è possibile rimuovere il codice di inizializzazione esplicita della proprietà nel controller.</span><span class="sxs-lookup"><span data-stu-id="53ad4-238">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="53ad4-239">In *Views/Instructor/Create.cshtml* aggiungere una casella di testo per la posizione dell'ufficio per i corsi prima del pulsante Submit (Invia).</span><span class="sxs-lookup"><span data-stu-id="53ad4-239">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="53ad4-240">Come nel caso della pagina Edit (Modifica), [correggere la formattazione se Visual Studio riformatta il codice quando lo si incolla](#notepad).</span><span class="sxs-lookup"><span data-stu-id="53ad4-240">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="53ad4-241">Eseguire il test eseguendo l'app e creando un insegnante.</span><span class="sxs-lookup"><span data-stu-id="53ad4-241">Test by running the app and creating an instructor.</span></span>

## <a name="handling-transactions"></a><span data-ttu-id="53ad4-242">Gestione delle transazioni</span><span class="sxs-lookup"><span data-stu-id="53ad4-242">Handling Transactions</span></span>

<span data-ttu-id="53ad4-243">Come spiegato nell' [esercitazione su CRUD](crud.md), per impostazione predefinita Entity Framework implementa in modo implicito le transazioni.</span><span class="sxs-lookup"><span data-stu-id="53ad4-243">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="53ad4-244">Per gli scenari in cui è necessario un maggior controllo, ad esempio per includere le operazioni eseguite all'esterno di Entity Framework in una transazione, vedere [Transazioni](/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="53ad4-244">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="53ad4-245">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="53ad4-245">Summary</span></span>

<span data-ttu-id="53ad4-246">L'esercitazione di introduzione all'uso di dati correlati è stata completata.</span><span class="sxs-lookup"><span data-stu-id="53ad4-246">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="53ad4-247">Nella prossima esercitazione verrà illustrato come gestire i conflitti di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="53ad4-247">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="53ad4-248">[Precedente](read-related-data.md)
> [Successivo](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="53ad4-248">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>
