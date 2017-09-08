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
ms.openlocfilehash: 655fefc0f9d884300bea670795c39a7a9aa10bb8
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2017
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a><span data-ttu-id="ebd8c-104">L'aggiornamento dei dati correlati - EF Core con l'esercitazione di base di ASP.NET MVC (7 di 10)</span><span class="sxs-lookup"><span data-stu-id="ebd8c-104">Updating related data - EF Core with ASP.NET Core MVC tutorial (7 of 10)</span></span>

<span data-ttu-id="ebd8c-105">Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ebd8c-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ebd8c-106">L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="ebd8c-107">Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="ebd8c-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="ebd8c-108">Nell'esercitazione precedente si visualizzati dati correlati. in questa esercitazione si aggiornerà i dati correlati mediante l'aggiornamento di campi di chiave esterni e le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-108">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="ebd8c-109">Le illustrazioni seguenti mostrano alcune delle pagine che verranno utilizzate.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Pagina Modifica corso](update-related-data/_static/course-edit.png)

![Pagina Modifica Instructor](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="ebd8c-112">Personalizzare la creazione e le pagine di modifica per i corsi</span><span class="sxs-lookup"><span data-stu-id="ebd8c-112">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="ebd8c-113">Quando viene creata una nuova entità course, deve avere una relazione a una categoria esistente.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-113">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="ebd8c-114">A tale scopo, il codice di scaffolding include i metodi del controller e creare e modificare le viste che includono un elenco a discesa per la selezione del reparto.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-114">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="ebd8c-115">Imposta l'elenco a discesa il `Course.DepartmentID` le proprietà di chiave esterna, e che tutte le esigenze di Entity Framework per caricare il `Department` proprietà di navigazione con l'entità Department appropriato.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-115">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="ebd8c-116">Verranno utilizzare il codice di supporto temporaneo, ma modificare leggermente per aggiungere la gestione degli errori e ordinare l'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-116">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="ebd8c-117">In *CoursesController.cs*, eliminare i quattro metodi di creazione e modifica e sostituirli con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-117">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

<span data-ttu-id="ebd8c-118">[!code-csharp[Principale](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-118">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]</span></span>

<span data-ttu-id="ebd8c-119">[!code-csharp[Principale](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-119">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]</span></span>

<span data-ttu-id="ebd8c-120">[!code-csharp[Principale](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-120">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]</span></span>

<span data-ttu-id="ebd8c-121">[!code-csharp[Principale](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-121">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]</span></span>

<span data-ttu-id="ebd8c-122">Dopo il `Edit` metodo HttpPost, creare un nuovo metodo che consente di caricare le informazioni di reparto per l'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-122">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

<span data-ttu-id="ebd8c-123">[!code-csharp[Principale](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-123">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]</span></span>

<span data-ttu-id="ebd8c-124">Il `PopulateDepartmentsDropDownList` metodo ottiene un elenco di tutti i reparti ordinati per nome, crea un `SelectList` raccolta per un elenco a discesa e passa l'insieme alla vista `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-124">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="ebd8c-125">Il metodo accetta il parametro facoltativo `selectedDepartment` parametro che consente al codice chiamante specificare l'elemento selezionato quando l'elenco a discesa viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-125">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="ebd8c-126">La vista verrà passare il nome "DepartmentID" al `<select>` helper di tag e il supporto in grado di conoscere la ricerca di `ViewBag` dell'oggetto per un `SelectList` denominato "DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="ebd8c-126">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="ebd8c-127">Il HttpGet `Create` chiamate al metodo di `PopulateDepartmentsDropDownList` metodo senza impostare l'elemento selezionato, perché per un nuovo corso il reparto non è ancora stabilito:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-127">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

<span data-ttu-id="ebd8c-128">[!code-csharp[Principale](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-128">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]</span></span>

<span data-ttu-id="ebd8c-129">Il HttpGet `Edit` metodo imposta l'elemento selezionato, in base all'ID del reparto che è già assegnato al corso modificato:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-129">The HttpGet `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

<span data-ttu-id="ebd8c-130">[!code-csharp[Principale](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-130">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]</span></span>

<span data-ttu-id="ebd8c-131">Per entrambi i metodi di HttpPost `Create` e `Edit` inoltre includere codice che imposta l'elemento selezionato quando vengono nuovamente la pagina dopo un errore.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-131">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="ebd8c-132">Ciò garantisce che quando la pagina viene visualizzata per mostrare il messaggio di errore, è stato selezionato il reparto rimane selezionato.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-132">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="ebd8c-133">Aggiungere. AsNoTracking dettagli ed eliminare i metodi</span><span class="sxs-lookup"><span data-stu-id="ebd8c-133">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="ebd8c-134">Per ottimizzare le prestazioni del corso dettagli e pagine di eliminazione, aggiungere `AsNoTracking` chiama il `Details` e HttpGet `Delete` metodi.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-134">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

<span data-ttu-id="ebd8c-135">[!code-csharp[Principale](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-135">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]</span></span>

<span data-ttu-id="ebd8c-136">[!code-csharp[Principale](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-136">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]</span></span>

### <a name="modify-the-course-views"></a><span data-ttu-id="ebd8c-137">Modificare le viste del corso</span><span class="sxs-lookup"><span data-stu-id="ebd8c-137">Modify the Course views</span></span>

<span data-ttu-id="ebd8c-138">In *Views/Courses/Create.cshtml*, aggiungere un'opzione "Selezionare reparto" per il **reparto** elenco a discesa elenco, modificare la didascalia da **DepartmentID** a  **Reparto**e aggiungere un messaggio di convalida.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-138">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

<span data-ttu-id="ebd8c-139">[!code-html[Principale](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-139">[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]</span></span>

<span data-ttu-id="ebd8c-140">In *Views/Courses/Edit.cshtml*, apportare la stessa modifica per il campo reparto consente di verificare in *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-140">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="ebd8c-141">Anche in *Views/Courses/Edit.cshtml*, aggiungere un campo numerico corso prima di **titolo** campo.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-141">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="ebd8c-142">Poiché il numero di corso è la chiave primaria, viene visualizzato, ma non può essere modificata.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-142">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

<span data-ttu-id="ebd8c-143">[!code-html[Principale](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-143">[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]</span></span>

<span data-ttu-id="ebd8c-144">È già presente un campo nascosto (`<input type="hidden">`) per il numero di corso nella visualizzazione di modifica.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-144">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="ebd8c-145">Aggiunta di un `<label>` helper di tag non di eliminare la necessità per il campo nascosto poiché non causare il numero del corso da includere nei dati inviati quando l'utente fa clic **salvare** sul **modifica** pagina.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-145">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="ebd8c-146">In *Views/Courses/Delete.cshtml*, aggiungere un campo del numero di corso nella parte superiore e modificare l'ID del reparto al nome del reparto.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-146">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

<span data-ttu-id="ebd8c-147">[!code-html[Principale](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-147">[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]</span></span>

<span data-ttu-id="ebd8c-148">In *Views/Courses/Details.cshtml*, apportare la stessa modifica effettuati per *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-148">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="ebd8c-149">Le pagine del corso di test</span><span class="sxs-lookup"><span data-stu-id="ebd8c-149">Test the Course pages</span></span>

<span data-ttu-id="ebd8c-150">Eseguire il **crea** pagina (visualizzare la pagina di indice corso, quindi fare clic su **Crea nuovo**) e immettere i dati per un nuovo corso:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-150">Run the **Create** page (display the Course Index page and click **Create New**) and enter data for a new course:</span></span>

![Pagina Crea corso](update-related-data/_static/course-create.png)

<span data-ttu-id="ebd8c-152">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-152">Click **Create**.</span></span> <span data-ttu-id="ebd8c-153">Verrà visualizzata la pagina di indice corsi con la nuova linea aggiunto all'elenco.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-153">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="ebd8c-154">Il nome di reparto nell'elenco di pagina di indice deriva dalla proprietà di navigazione, che mostra che la relazione è stata stabilita correttamente.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-154">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="ebd8c-155">Eseguire il **modifica** pagina (fare clic su **modifica** su un corso nella pagina di indice corso).</span><span class="sxs-lookup"><span data-stu-id="ebd8c-155">Run the **Edit** page (click **Edit** on a course in the Course Index page ).</span></span>

![Pagina Modifica corso](update-related-data/_static/course-edit.png)

<span data-ttu-id="ebd8c-157">Modificare i dati nella pagina e fare clic su **salvare**.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-157">Change data on the page and click **Save**.</span></span> <span data-ttu-id="ebd8c-158">Verrà visualizzata la pagina di indice corsi con i dati di corso aggiornato.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-158">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="ebd8c-159">Aggiungere una pagina di modifica per i docenti</span><span class="sxs-lookup"><span data-stu-id="ebd8c-159">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="ebd8c-160">Quando si modifica un record istruttore, si desidera essere in grado di aggiornare l'assegnazione dell'ufficio dell'insegnante.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-160">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="ebd8c-161">Entità Instructor ha una relazione uno-a-zero-o-uno con l'entità OfficeAssignment, ovvero che il codice deve gestire le situazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-161">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="ebd8c-162">Se l'utente cancella l'assegnazione di office e che inizialmente aveva un valore, eliminare l'entità OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-162">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="ebd8c-163">Se l'utente immette un valore di assegnazione di office ed era originariamente vuoto, creare una nuova entità OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-163">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="ebd8c-164">Se l'utente modifica il valore di un'assegnazione di office, modificare il valore in un'entità OfficeAssignment esistente.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-164">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="ebd8c-165">Aggiornare il controller istruttori</span><span class="sxs-lookup"><span data-stu-id="ebd8c-165">Update the Instructors controller</span></span>

<span data-ttu-id="ebd8c-166">In *InstructorsController.cs*, modificare il codice di HttpGet `Edit` metodo in modo che carica l'entità Instructor `OfficeAssignment` proprietà di navigazione e chiamate `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-166">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

<span data-ttu-id="ebd8c-167">[!code-csharp[Principale](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-167">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]</span></span>

<span data-ttu-id="ebd8c-168">Sostituire il HttpPost `Edit` metodo con il codice seguente per gestire gli aggiornamenti di assegnazione di office:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-168">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

<span data-ttu-id="ebd8c-169">[!code-csharp[Principale](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-169">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]</span></span>

<span data-ttu-id="ebd8c-170">Il codice esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-170">The code does the following:</span></span>

-  <span data-ttu-id="ebd8c-171">Modifica il nome del metodo per `EditPost` perché la firma è ora come il HttpGet `Edit` metodo (il `ActionName` attributo specifica che il `/Edit/` URL viene ancora utilizzato).</span><span class="sxs-lookup"><span data-stu-id="ebd8c-171">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="ebd8c-172">Ottiene l'entità Instructor corrente dal database utilizzando eager caricamento per il `OfficeAssignment` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-172">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="ebd8c-173">Ciò equivale il il HttpGet `Edit` metodo.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-173">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="ebd8c-174">Aggiorna l'entità Instructor recuperata con valori dallo strumento di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-174">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="ebd8c-175">Il `TryUpdateModel` overload consente all'elenco elementi consentiti le proprietà che si desidera includere.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-175">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="ebd8c-176">In questo modo la registrazione, come illustrato nel [esercitazione secondo](crud.md).</span><span class="sxs-lookup"><span data-stu-id="ebd8c-176">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   <span data-ttu-id="ebd8c-177">Se il percorso di office è vuoto, imposta la proprietà Instructor.OfficeAssignment su null in modo che la riga correlata nella tabella OfficeAssignment verrà eliminata.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-177">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="ebd8c-178">Salva le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-178">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="ebd8c-179">Aggiornare la visualizzazione di modifica Instructor</span><span class="sxs-lookup"><span data-stu-id="ebd8c-179">Update the Instructor Edit view</span></span>

<span data-ttu-id="ebd8c-180">In *Views/Instructors/Edit.cshtml*, aggiungere un nuovo campo per modificare il percorso di office, al termine prima di **salvare** pulsante:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-180">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

<span data-ttu-id="ebd8c-181">[!code-html[Principale](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-181">[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]</span></span>

<span data-ttu-id="ebd8c-182">Eseguire la pagina (selezionare il **i docenti** scheda e quindi fare clic su **modifica** su un docente).</span><span class="sxs-lookup"><span data-stu-id="ebd8c-182">Run the page (select the **Instructors** tab and then click **Edit** on an instructor).</span></span> <span data-ttu-id="ebd8c-183">Modifica il **ufficio** e fare clic su **salvare**.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-183">Change the **Office Location** and click **Save**.</span></span>

![Pagina Modifica Instructor](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="ebd8c-185">Aggiungere esercitazioni per la pagina Modifica Instructor</span><span class="sxs-lookup"><span data-stu-id="ebd8c-185">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="ebd8c-186">I docenti potrebbero indicare un numero qualsiasi di corsi.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-186">Instructors may teach any number of courses.</span></span> <span data-ttu-id="ebd8c-187">A questo punto sarà possibile migliorare la pagina Modifica istruttore aggiungendo la possibilità di modificare le assegnazioni di corso usando un gruppo di caselle di controllo, come illustrato nella schermata seguente:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-187">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Pagina Modifica istruttore corsi](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="ebd8c-189">La relazione tra le entità Course e istruttore è molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-189">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="ebd8c-190">Per aggiungere e rimuovere le relazioni, aggiungere e rimuovere le entità da e verso il set di entità CourseAssignments join.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-190">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="ebd8c-191">L'interfaccia utente che consente di modificare quali corsi un docente è assegnato a è un gruppo di caselle di controllo.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-191">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="ebd8c-192">Viene visualizzata una casella di controllo per ogni corso nel database e quelle che è attualmente assegnata istruttore selezionati.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-192">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="ebd8c-193">L'utente può selezionare o deselezionare le caselle di controllo per modificare le assegnazioni dei corsi.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-193">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="ebd8c-194">Se il numero di corsi è molto superiore, probabilmente si desidera utilizzare un altro metodo di presentazione dei dati nella visualizzazione, ma utilizzare lo stesso metodo di manipolazione di un'entità di join per creare o eliminare le relazioni.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-194">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="ebd8c-195">Aggiornare il controller istruttori</span><span class="sxs-lookup"><span data-stu-id="ebd8c-195">Update the Instructors controller</span></span>

<span data-ttu-id="ebd8c-196">Per fornire dati per la visualizzazione per un elenco di caselle di controllo, si utilizzerà una classe di modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-196">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="ebd8c-197">Creare *AssignedCourseData.cs* nel *SchoolViewModels* cartella e sostituire il codice esistente con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-197">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

<span data-ttu-id="ebd8c-198">[!code-csharp[Principale](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-198">[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]</span></span>

<span data-ttu-id="ebd8c-199">In *InstructorsController.cs*, sostituire il HttpGet `Edit` metodo con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-199">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="ebd8c-200">Le modifiche sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-200">The changes are highlighted.</span></span>

<span data-ttu-id="ebd8c-201">[!code-csharp[Principale](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-201">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]</span></span>

<span data-ttu-id="ebd8c-202">Il codice aggiunge il caricamento immediato per la `Courses` proprietà di navigazione e chiama il nuovo `PopulateAssignedCourseData` metodo per fornire informazioni per la matrice di casella di controllo utilizzando il `AssignedCourseData` visualizzare una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-202">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="ebd8c-203">Il codice di `PopulateAssignedCourseData` metodo legge tutte le entità Course per caricare un elenco di corsi utilizzando la classe di modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-203">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="ebd8c-204">Per ogni corso, il codice verifica se il corso è presente nell'istruttore `Courses` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-204">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="ebd8c-205">Per creare ricerca efficiente quando si verifica se un corso è assegnato all'istruttore, corsi assegnati all'istruttore vengono inseriti in un `HashSet` insieme.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-205">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="ebd8c-206">Il `Assigned` è impostata su true per i corsi istruttore viene assegnato a.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-206">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="ebd8c-207">La vista utilizzerà questa proprietà per determinare quale controllo caselle devono essere visualizzate come selezionato.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-207">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="ebd8c-208">Infine, tale elenco viene passato alla visualizzazione in `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-208">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="ebd8c-209">Successivamente, aggiungere il codice che viene eseguito quando l'utente fa clic **salvare**.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-209">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="ebd8c-210">Sostituire il `EditPost` (metodo) con il seguente codice, quindi aggiungere un nuovo metodo che aggiorna il `Courses` proprietà di navigazione di entità Instructor.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-210">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

<span data-ttu-id="ebd8c-211">[!code-csharp[Principale](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-211">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]</span></span>

<span data-ttu-id="ebd8c-212">[!code-csharp[Principale](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-212">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]</span></span>

<span data-ttu-id="ebd8c-213">La firma del metodo è diversa dal HttpGet `Edit` metodo, pertanto, il nome del metodo di modifica da `EditPost` al `Edit`.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-213">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="ebd8c-214">Poiché la vista non dispone di una raccolta di entità corso, lo strumento di associazione del modello non può aggiornare automaticamente il `CourseAssignments` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-214">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="ebd8c-215">Anziché utilizzare lo strumento di associazione del modello per aggiornare il `CourseAssignments` proprietà di navigazione è eseguire questa operazione nella nuova `UpdateInstructorCourses` metodo.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-215">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="ebd8c-216">È pertanto necessario escludere il `CourseAssignments` proprietà dall'associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-216">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="ebd8c-217">Questo non richiede modifiche al codice che chiama `TryUpdateModel` poiché si utilizza l'overload whitelist e `CourseAssignments` non è presente nell'elenco di inclusione.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-217">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="ebd8c-218">Se nessun controllo caselle sono state selezionate, il codice in `UpdateInstructorCourses` Inizializza il `CourseAssignments` proprietà di navigazione e restituisce una raccolta vuota:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-218">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

<span data-ttu-id="ebd8c-219">[!code-csharp[Principale](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-219">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]</span></span>

<span data-ttu-id="ebd8c-220">Il codice quindi esegue il ciclo di tutti i corsi nel database e controlla ogni corso rispetto a quelli attualmente assegnati all'istruttore rispetto a quelli che sono stati selezionati nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-220">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="ebd8c-221">Per facilitare le ricerche efficienti, quest'ultime due raccolte vengono archiviate in `HashSet` oggetti.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-221">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="ebd8c-222">Se è stata selezionata la casella di controllo per un corso ma non è corso la `Instructor.CourseAssignments` proprietà di navigazione, la linea viene aggiunto alla raccolta nella proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-222">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

<span data-ttu-id="ebd8c-223">[!code-csharp[Principale](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-223">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]</span></span>

<span data-ttu-id="ebd8c-224">Se non è stata selezionata la casella di controllo per un corso, ma il corso è il `Instructor.CourseAssignments` proprietà di navigazione, la linea viene rimossa dalla proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-224">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

<span data-ttu-id="ebd8c-225">[!code-csharp[Principale](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-225">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]</span></span>

### <a name="update-the-instructor-views"></a><span data-ttu-id="ebd8c-226">Aggiornare le visualizzazioni Instructor</span><span class="sxs-lookup"><span data-stu-id="ebd8c-226">Update the Instructor views</span></span>

<span data-ttu-id="ebd8c-227">In *Views/Instructors/Edit.cshtml*, aggiungere un **corsi** campo con una matrice di caselle di controllo aggiungendo il seguente codice immediatamente dopo il `div` elementi per il **Office**  campo e prima di `div` elemento per il **salvare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-227">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE] 
> <span data-ttu-id="ebd8c-228">Quando si incolla il codice in Visual Studio, le interruzioni di riga verranno modificate in modo che interrompe il codice.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-228">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="ebd8c-229">Premere Ctrl + Z di una volta per annullare la formattazione automatica.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-229">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="ebd8c-230">Ciò consente di risolvere le interruzioni di riga in modo che vengano visualizzate come gli elementi visualizzati.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-230">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="ebd8c-231">Il rientro non deve necessariamente essere perfetto, ma la `@</tr><tr>`, `@:<td>`, `@:</td>`, e `@:</tr>` righe devono essere su una singola riga come illustrato o si otterrà un errore di runtime.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-231">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="ebd8c-232">Con il blocco di nuovo codice selezionato, premere Tab tre volte per allineare il nuovo codice con il codice esistente.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-232">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span>

<span data-ttu-id="ebd8c-233">[!code-html[Principale](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-233">[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]</span></span>

<span data-ttu-id="ebd8c-234">Questo codice crea una tabella HTML che ha tre colonne.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-234">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="ebd8c-235">In ogni colonna è seguita da una didascalia che include il numero di corso e il titolo di una casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-235">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="ebd8c-236">Tutte le caselle di controllo con lo stesso nome ("selectedCourses"), che informa il gestore di associazione del modello in cui devono essere considerate come un gruppo.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-236">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="ebd8c-237">L'attributo value di ogni casella di controllo è impostata sul valore di `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-237">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="ebd8c-238">Quando viene eseguito il postback della pagina, il gestore di associazione del modello passa una matrice per il controller che include il `CourseID` i valori per solo le caselle di controllo selezionate.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-238">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="ebd8c-239">Quando le caselle di controllo sono inizialmente eseguito il rendering, quelli per i corsi assegnati all'istruttore sono controllati gli attributi, che seleziona (Display li selezionate).</span><span class="sxs-lookup"><span data-stu-id="ebd8c-239">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="ebd8c-240">Eseguire la pagina di indice Instructor e fare clic su **modifica** su un docente per visualizzare il **modifica** pagina.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-240">Run the Instructor Index page, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Pagina Modifica istruttore corsi](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="ebd8c-242">Modificare alcune esercitazioni e fare clic su Salva.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-242">Change some course assignments and click Save.</span></span> <span data-ttu-id="ebd8c-243">Le modifiche apportate vengono riflesse nella pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-243">The changes you make are reflected on the Index page.</span></span>

> [!NOTE] 
> <span data-ttu-id="ebd8c-244">L'approccio adottato qui per modificare i dati del corso istruttore funziona anche quando è presente un numero limitato di corsi.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-244">The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="ebd8c-245">Per le raccolte che sono più grandi, un'interfaccia utente differente e un altro metodo di aggiornamento sono necessarie.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-245">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="ebd8c-246">Aggiornare la pagina di eliminazione</span><span class="sxs-lookup"><span data-stu-id="ebd8c-246">Update the Delete page</span></span>

<span data-ttu-id="ebd8c-247">In *InstructorsController.cs*, eliminare il `DeleteConfirmed` (metodo) e l'inserimento di codice seguente al suo posto.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-247">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

<span data-ttu-id="ebd8c-248">[!code-csharp[Principale](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-248">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]</span></span>

<span data-ttu-id="ebd8c-249">Questo codice rende le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-249">This code makes the following changes:</span></span>

* <span data-ttu-id="ebd8c-250">Eager fa caricamento per il `CourseAssignments` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-250">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="ebd8c-251">È necessario includere questa o EF non rileveranno correlati `CourseAssignment` entità e non di eliminarli.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-251">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="ebd8c-252">Per evitare la necessità di leggerli qui è possibile configurare eliminazione a catena nel database.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-252">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="ebd8c-253">Se istruttore da eliminare viene assegnato come responsabile per i reparti, rimuove l'assegnazione istruttore i reparti.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-253">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="ebd8c-254">Aggiungere la pagina Crea ufficio e corsi</span><span class="sxs-lookup"><span data-stu-id="ebd8c-254">Add office location and courses to the Create page</span></span>

<span data-ttu-id="ebd8c-255">In *InstructorsController.cs*, eliminare il HttpGet e HttpPost `Create` metodi e quindi aggiungere il codice seguente al loro posto:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-255">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

<span data-ttu-id="ebd8c-256">[!code-csharp[Principale](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-256">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]</span></span>

<span data-ttu-id="ebd8c-257">Questo codice è simile a quello visualizzato per il `Edit` metodi, ad eccezione che inizialmente non corsi selezionati.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-257">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="ebd8c-258">Il HttpGet `Create` chiamate al metodo di `PopulateAssignedCourseData` metodo non perché potrebbero essere presenti corsi selezionati ma in ordine per fornire una raccolta vuota per il `foreach` ciclo nella vista (in caso contrario Visualizza codice genera un'eccezione di riferimento null).</span><span class="sxs-lookup"><span data-stu-id="ebd8c-258">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="ebd8c-259">HttpPost `Create` metodo aggiunge ogni corso selezionato per il `CourseAssignments` proprietà di navigazione prima che verifica la presenza di errori di convalida e aggiunge il nuovo docente al database.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-259">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="ebd8c-260">Corsi vengono aggiunti anche se sono presenti errori del modello in modo che quando sono presenti errori del modello (per un esempio, l'utente con chiave in una data non valida) e la pagina viene visualizzata con un messaggio di errore, le selezioni corso apportate vengono automaticamente ripristinate.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-260">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="ebd8c-261">Si noti che per poter essere in grado di aggiungere i corsi per il `CourseAssignments` proprietà di navigazione, è necessario inizializzare la proprietà su una raccolta vuota:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-261">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="ebd8c-262">Come alternativa a questa operazione nel codice del controller, è possibile usare il modello istruttore modificando il metodo Get della proprietà per creare automaticamente la raccolta se non esiste, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="ebd8c-262">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

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

<span data-ttu-id="ebd8c-263">Se si modifica il `CourseAssignments` proprietà in questo modo, è possibile rimuovere il codice di inizializzazione di proprietà espliciti nel controller.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-263">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="ebd8c-264">In *Views/Instructor/Create.cshtml*, aggiungere una casella di testo percorso office caselle di controllo e per i corsi prima pulsante Invia.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-264">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="ebd8c-265">Come nel caso di modifica pagina [correggere la formattazione se Visual Studio riformatta il codice quando lo si incolla](#notepad).</span><span class="sxs-lookup"><span data-stu-id="ebd8c-265">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

<span data-ttu-id="ebd8c-266">[!code-html[Principale](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]</span><span class="sxs-lookup"><span data-stu-id="ebd8c-266">[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]</span></span>

<span data-ttu-id="ebd8c-267">Test eseguendo il **crea** pagina e l'aggiunta di un docente.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-267">Test by running the **Create** page and adding an instructor.</span></span> 

## <a name="handling-transactions"></a><span data-ttu-id="ebd8c-268">Gestione delle transazioni</span><span class="sxs-lookup"><span data-stu-id="ebd8c-268">Handling Transactions</span></span>

<span data-ttu-id="ebd8c-269">Come spiegato nel [esercitazione CRUD](crud.md), Entity Framework implementa in modo implicito le transazioni.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-269">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="ebd8c-270">Per scenari in cui è necessario più controllare, ad esempio, se si desidera includere operazioni eseguite all'esterno di Entity Framework in una transazione, vedere [transazioni](https://docs.microsoft.com/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="ebd8c-270">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="ebd8c-271">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="ebd8c-271">Summary</span></span>

<span data-ttu-id="ebd8c-272">Introduzione all'utilizzo di dati correlati sono stati completati.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-272">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="ebd8c-273">Nella prossima esercitazione verrà visualizzato come gestire i conflitti di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="ebd8c-273">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ebd8c-274">[Precedente](read-related-data.md)
[Successivo](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="ebd8c-274">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>  
