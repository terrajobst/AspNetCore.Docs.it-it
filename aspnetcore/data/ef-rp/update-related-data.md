---
title: Razor Pages con EF Core in ASP.NET Core - Aggiornare dati correlati - 7 di 8
author: rick-anderson
description: In questa esercitazione verrà effettuato l'aggiornamento di dati correlati tramite l'aggiornamento di campi di chiave esterna e proprietà di navigazione.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 4306118240c052585a5c2eeb2053ce03534b547c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207543"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="db5cd-103">Razor Pages con EF Core in ASP.NET Core - Aggiornare dati correlati - 7 di 8</span><span class="sxs-lookup"><span data-stu-id="db5cd-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="db5cd-104">Di [Tom Dykstra](https://github.com/tdykstra), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="db5cd-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="db5cd-105">Questa esercitazione illustra l'aggiornamento di dati correlati.</span><span class="sxs-lookup"><span data-stu-id="db5cd-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="db5cd-106">Se si verificano problemi che non si è in grado di risolvere, [scaricare o visualizzare l'app completa](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="db5cd-106">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="db5cd-107">[Istruzioni per il download](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="db5cd-107">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="db5cd-108">Le illustrazioni seguenti mostrano alcune pagine completate.</span><span class="sxs-lookup"><span data-stu-id="db5cd-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="db5cd-109">![Pagina di modifica del corso](update-related-data/_static/course-edit.png)
![Pagina di modifica dell'insegnante](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="db5cd-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="db5cd-110">Esaminare e testare le pagine di creazione e modifica del corso.</span><span class="sxs-lookup"><span data-stu-id="db5cd-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="db5cd-111">Creare un nuovo corso.</span><span class="sxs-lookup"><span data-stu-id="db5cd-111">Create a new course.</span></span> <span data-ttu-id="db5cd-112">Il dipartimento viene selezionato in base alla chiave primaria (numero intero), non in base al nome.</span><span class="sxs-lookup"><span data-stu-id="db5cd-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="db5cd-113">Modificare il nuovo corso.</span><span class="sxs-lookup"><span data-stu-id="db5cd-113">Edit the new course.</span></span> <span data-ttu-id="db5cd-114">Al termine del test, eliminare il nuovo corso.</span><span class="sxs-lookup"><span data-stu-id="db5cd-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="db5cd-115">Creare una classe di base per condividere il codice comune</span><span class="sxs-lookup"><span data-stu-id="db5cd-115">Create a base class to share common code</span></span>

<span data-ttu-id="db5cd-116">La pagina di creazione e quella di modifica del corso hanno bisogno dell'elenco dei nomi dei dipartimenti.</span><span class="sxs-lookup"><span data-stu-id="db5cd-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="db5cd-117">Creare la classe di base *Pages/Courses/DepartmentNamePageModel.cshtml.cs* per le pagine Create (Crea) ed Edit (Modifica):</span><span class="sxs-lookup"><span data-stu-id="db5cd-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="db5cd-118">Il codice precedente crea un oggetto [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) in cui inserire l'elenco dei nomi dei dipartimenti.</span><span class="sxs-lookup"><span data-stu-id="db5cd-118">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="db5cd-119">Se `selectedDepartment` è specificato, il dipartimento corrispondente viene selezionato in `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="db5cd-120">Le classi modello delle pagine Create (Crea) ed Edit (Modifica) derivano da `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="db5cd-121">Personalizzare le pagine dei corsi</span><span class="sxs-lookup"><span data-stu-id="db5cd-121">Customize the Courses Pages</span></span>

<span data-ttu-id="db5cd-122">Quando viene creata, una nuova entità corso deve essere in relazione con un dipartimento esistente.</span><span class="sxs-lookup"><span data-stu-id="db5cd-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="db5cd-123">Per consentire l'aggiunta di un dipartimento durante la creazione di un corso, la classe di base per Create (Crea) ed Edit (Modifica) contiene un elenco a discesa per la selezione del dipartimento.</span><span class="sxs-lookup"><span data-stu-id="db5cd-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="db5cd-124">L'elenco a discesa imposta la proprietà di chiave esterna `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="db5cd-125">Core EF usa la chiave esterna `Course.DepartmentID` per caricare la proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Create course (Crea corso)](update-related-data/_static/ddl.png)

<span data-ttu-id="db5cd-127">Aggiornare il modello della pagina Create (Crea) con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-127">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="db5cd-128">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-128">The preceding code:</span></span>

* <span data-ttu-id="db5cd-129">Classe derivata da `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="db5cd-130">Usa `TryUpdateModelAsync` per evitare l'[overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="db5cd-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="db5cd-131">Sostituisce `ViewData["DepartmentID"]` con `DepartmentNameSL` (dalla classe di base).</span><span class="sxs-lookup"><span data-stu-id="db5cd-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="db5cd-132">`ViewData["DepartmentID"]` viene sostituito con `DepartmentNameSL` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="db5cd-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="db5cd-133">I modelli fortemente tipizzati sono preferibili a quelli scarsamente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="db5cd-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="db5cd-134">Per altre informazioni, vedere [Dati con tipizzazione debole (ViewData e ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="db5cd-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="db5cd-135">Aggiornare la pagina di creazione dei corsi</span><span class="sxs-lookup"><span data-stu-id="db5cd-135">Update the Courses Create page</span></span>

<span data-ttu-id="db5cd-136">Aggiornare *Pages/Courses/Create.cshtml* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="db5cd-137">Il markup precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="db5cd-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="db5cd-138">Modifica la didascalia da **DepartmentID** a **Department**.</span><span class="sxs-lookup"><span data-stu-id="db5cd-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="db5cd-139">Sostituisce `"ViewBag.DepartmentID"` con `DepartmentNameSL` (dalla classe di base).</span><span class="sxs-lookup"><span data-stu-id="db5cd-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="db5cd-140">Aggiunge l'opzione "Select Department" (Selezionare il dipartimento).</span><span class="sxs-lookup"><span data-stu-id="db5cd-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="db5cd-141">Questa modifica esegue il rendering di "Select Department" (Selezionare il dipartimento) anziché del primo dipartimento.</span><span class="sxs-lookup"><span data-stu-id="db5cd-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="db5cd-142">Aggiunge un messaggio di convalida quando il dipartimento non è selezionato.</span><span class="sxs-lookup"><span data-stu-id="db5cd-142">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="db5cd-143">La pagina Razor usa l'[helper tag di selezione](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="db5cd-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="db5cd-144">Testare la pagina Create (Crea).</span><span class="sxs-lookup"><span data-stu-id="db5cd-144">Test the Create page.</span></span> <span data-ttu-id="db5cd-145">La pagina Create (Crea) visualizza il nome, anziché l'ID, del dipartimento.</span><span class="sxs-lookup"><span data-stu-id="db5cd-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="db5cd-146">Aggiornare la pagina di modifica dei corsi.</span><span class="sxs-lookup"><span data-stu-id="db5cd-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="db5cd-147">Aggiornare il modello della pagina Edit (Modifica) con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-147">Update the edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="db5cd-148">Le modifiche sono simili a quelle apportate nel modello della pagina Create (Crea).</span><span class="sxs-lookup"><span data-stu-id="db5cd-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="db5cd-149">Nel codice precedente, `PopulateDepartmentsDropDownList` passa l'ID del dipartimento, che seleziona il dipartimento specificato nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="db5cd-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="db5cd-150">Aggiornare *Pages/Courses/Edit.cshtml* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="db5cd-151">Il markup precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="db5cd-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="db5cd-152">Visualizza l'ID del corso.</span><span class="sxs-lookup"><span data-stu-id="db5cd-152">Displays the course ID.</span></span> <span data-ttu-id="db5cd-153">In genere, la chiave primaria di un'entità non viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="db5cd-153">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="db5cd-154">Le chiavi primarie di solito non sono significative per gli utenti.</span><span class="sxs-lookup"><span data-stu-id="db5cd-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="db5cd-155">In questo caso, la chiave primaria corrisponde al numero del corso.</span><span class="sxs-lookup"><span data-stu-id="db5cd-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="db5cd-156">Modifica la didascalia da **DepartmentID** a **Department**.</span><span class="sxs-lookup"><span data-stu-id="db5cd-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="db5cd-157">Sostituisce `"ViewBag.DepartmentID"` con `DepartmentNameSL` (dalla classe di base).</span><span class="sxs-lookup"><span data-stu-id="db5cd-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="db5cd-158">La pagina contiene un campo nascosto (`<input type="hidden">`) per il numero del corso.</span><span class="sxs-lookup"><span data-stu-id="db5cd-158">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="db5cd-159">L'aggiunta di un helper tag `<label>` con `asp-for="Course.CourseID"` non elimina la necessità del campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="db5cd-159">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="db5cd-160">`<input type="hidden">` è necessario per includere il numero del corso tra i dati inviati quando l'utente fa clic su **Save** (Salva).</span><span class="sxs-lookup"><span data-stu-id="db5cd-160">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="db5cd-161">Testare il codice aggiornato.</span><span class="sxs-lookup"><span data-stu-id="db5cd-161">Test the updated code.</span></span> <span data-ttu-id="db5cd-162">Creare, modificare ed eliminare un corso.</span><span class="sxs-lookup"><span data-stu-id="db5cd-162">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="db5cd-163">Aggiungere AsNoTracking ai modelli delle pagine Details (Dettagli) e Delete (Elimina)</span><span class="sxs-lookup"><span data-stu-id="db5cd-163">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="db5cd-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) può migliorare le prestazioni quando la registrazione non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="db5cd-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="db5cd-165">Aggiungere `AsNoTracking` alle pagine Delete (Elimina) e Details (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="db5cd-165">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="db5cd-166">Il codice seguente illustra il modello della pagina Delete (Elimina) aggiornato:</span><span class="sxs-lookup"><span data-stu-id="db5cd-166">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="db5cd-167">Aggiornare il metodo `OnGetAsync` nel file *Pages/Courses/Details.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="db5cd-167">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="db5cd-168">Modificare le pagine Delete (Elimina) e Details (Dettagli)</span><span class="sxs-lookup"><span data-stu-id="db5cd-168">Modify the Delete and Details pages</span></span>

<span data-ttu-id="db5cd-169">Aggiornare la pagina Razor Delete (Elimina) con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-169">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="db5cd-170">Apportare le stesse modifiche alla pagina Details (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="db5cd-170">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="db5cd-171">Testare le pagine del corso</span><span class="sxs-lookup"><span data-stu-id="db5cd-171">Test the Course pages</span></span>

<span data-ttu-id="db5cd-172">Testare le pagine di creazione, modifica, dettagli ed eliminazione.</span><span class="sxs-lookup"><span data-stu-id="db5cd-172">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="db5cd-173">Aggiornare le pagine dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="db5cd-173">Update the instructor pages</span></span>

<span data-ttu-id="db5cd-174">Le sezioni seguenti aggiornano le pagine dell'insegnante.</span><span class="sxs-lookup"><span data-stu-id="db5cd-174">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="db5cd-175">Aggiungere la posizione dell'ufficio</span><span class="sxs-lookup"><span data-stu-id="db5cd-175">Add office location</span></span>

<span data-ttu-id="db5cd-176">Quando si modifica il record di un insegnante, può essere necessario aggiornare l'assegnazione dell'ufficio.</span><span class="sxs-lookup"><span data-stu-id="db5cd-176">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="db5cd-177">L'entità `Instructor` ha una relazione uno-a-zero-o-uno con l'entità `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-177">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="db5cd-178">Il codice relativo all'insegnante deve gestire i casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="db5cd-178">The instructor code must handle:</span></span>

* <span data-ttu-id="db5cd-179">Se l'utente cancella l'assegnazione dell'ufficio, eliminare l'entità `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-179">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="db5cd-180">Se l'utente immette l'assegnazione dell'ufficio e questa era vuota, creare una nuova entità `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-180">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="db5cd-181">Se l'utente modifica l'assegnazione dell'ufficio, aggiornare l'entità `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-181">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="db5cd-182">Aggiornare il modello della pagina Edit (Modifica) dell'insegnante con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-182">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="db5cd-183">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-183">The preceding code:</span></span>

- <span data-ttu-id="db5cd-184">Ottiene l'entità `Instructor` corrente dal database tramite il caricamento eager per la proprietà di navigazione `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-184">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="db5cd-185">Aggiorna l'entità `Instructor` recuperata con valori dallo strumento di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="db5cd-185">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="db5cd-186">`TryUpdateModel` impedisce l'[overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="db5cd-186">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="db5cd-187">Se la posizione dell'ufficio è vuota, imposta `Instructor.OfficeAssignment` su Null.</span><span class="sxs-lookup"><span data-stu-id="db5cd-187">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="db5cd-188">Se `Instructor.OfficeAssignment` è Null, la riga correlata nella tabella `OfficeAssignment` viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="db5cd-188">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="db5cd-189">Aggiornare la pagina di modifica dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="db5cd-189">Update the instructor Edit page</span></span>

<span data-ttu-id="db5cd-190">Aggiornare *Pages/Instructors/Edit.cshtml* con la posizione dell'ufficio:</span><span class="sxs-lookup"><span data-stu-id="db5cd-190">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="db5cd-191">Verificare che sia possibile modificare la posizione dell'ufficio di un insegnante.</span><span class="sxs-lookup"><span data-stu-id="db5cd-191">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="db5cd-192">Aggiungere assegnazioni di corso nella pagina di modifica dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="db5cd-192">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="db5cd-193">Gli insegnanti possono tenere un numero qualsiasi di corsi.</span><span class="sxs-lookup"><span data-stu-id="db5cd-193">Instructors may teach any number of courses.</span></span> <span data-ttu-id="db5cd-194">In questa sezione, viene aggiunta la possibilità di modificare le assegnazioni dei corsi.</span><span class="sxs-lookup"><span data-stu-id="db5cd-194">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="db5cd-195">La figura seguente illustra la pagina Edit (Modifica) dell'insegnante aggiornata:</span><span class="sxs-lookup"><span data-stu-id="db5cd-195">The following image shows the updated instructor Edit page:</span></span>

![Pagina di modifica dell'insegnante con corsi](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="db5cd-197">`Course` e `Instructor` hanno una relazione molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="db5cd-197">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="db5cd-198">Per aggiungere e rimuovere relazioni, aggiungere e rimuovere entità dal set di entità di join `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-198">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="db5cd-199">Per consentire la modifica dei corsi assegnati a un insegnante, si usano caselle di controllo.</span><span class="sxs-lookup"><span data-stu-id="db5cd-199">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="db5cd-200">Per ogni corso nel database è visualizzata una casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="db5cd-200">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="db5cd-201">I corsi assegnati all'insegnante corrente sono selezionati.</span><span class="sxs-lookup"><span data-stu-id="db5cd-201">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="db5cd-202">L'utente può selezionare e deselezionare le caselle di controllo per modificare le assegnazioni dei corsi.</span><span class="sxs-lookup"><span data-stu-id="db5cd-202">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="db5cd-203">Se il numero di corsi è molto più elevato:</span><span class="sxs-lookup"><span data-stu-id="db5cd-203">If the number of courses were much greater:</span></span>

* <span data-ttu-id="db5cd-204">Per visualizzare i corsi è consigliabile usare un'interfaccia utente diversa.</span><span class="sxs-lookup"><span data-stu-id="db5cd-204">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="db5cd-205">Il metodo di modifica di un'entità di join per creare o eliminare relazioni non cambia.</span><span class="sxs-lookup"><span data-stu-id="db5cd-205">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="db5cd-206">Aggiungere classi per supportare le pagine di creazione e modifica dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="db5cd-206">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="db5cd-207">Creare *SchoolViewModels/AssignedCourseData.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-207">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="db5cd-208">La classe `AssignedCourseData` contiene i dati per la creazione delle caselle di controllo per i corsi assegnati a un insegnante.</span><span class="sxs-lookup"><span data-stu-id="db5cd-208">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="db5cd-209">Creare la classe di base *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="db5cd-209">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="db5cd-210">`InstructorCoursesPageModel` è la classe di base che verrà usata per i modelli delle pagine Edit (Modifica) e Create (Crea).</span><span class="sxs-lookup"><span data-stu-id="db5cd-210">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="db5cd-211">`PopulateAssignedCourseData` legge tutte le entità `Course` per popolare `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-211">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="db5cd-212">Per ogni corso, il codice imposta `CourseID` e titolo, e stabilisce se l'insegnante è assegnato al corso.</span><span class="sxs-lookup"><span data-stu-id="db5cd-212">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="db5cd-213">Per creare ricerche efficienti, si usa un [HashSet](/dotnet/api/system.collections.generic.hashset-1).</span><span class="sxs-lookup"><span data-stu-id="db5cd-213">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="db5cd-214">Modello della pagina di modifica dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="db5cd-214">Instructors Edit page model</span></span>

<span data-ttu-id="db5cd-215">Aggiornare il modello della pagina Edit (Modifica) dell'insegnante con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-215">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="db5cd-216">Il codice precedente gestisce le modifiche alle assegnazioni di ufficio.</span><span class="sxs-lookup"><span data-stu-id="db5cd-216">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="db5cd-217">Aggiornare la visualizzazione Razor degli insegnanti:</span><span class="sxs-lookup"><span data-stu-id="db5cd-217">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="db5cd-218">Quando si incolla il codice in Visual Studio, le interruzioni di riga vengono modificate in modo tale da danneggiare il codice.</span><span class="sxs-lookup"><span data-stu-id="db5cd-218">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="db5cd-219">Premere Ctrl + Z una volta per annullare la formattazione automatica.</span><span class="sxs-lookup"><span data-stu-id="db5cd-219">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="db5cd-220">Premendo CTRL + Z si correggono le interruzioni di riga, che vengono visualizzate come illustrato qui.</span><span class="sxs-lookup"><span data-stu-id="db5cd-220">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="db5cd-221">Il rientro non deve necessariamente essere perfetto, ma le righe `@</tr><tr>`, `@:<td>`, `@:</td>` e `@:</tr>` devono trovarsi in una sola riga, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="db5cd-221">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="db5cd-222">Dopo aver selezionato il blocco di nuovo codice, premere Tab tre volte per allineare il nuovo codice con il codice esistente.</span><span class="sxs-lookup"><span data-stu-id="db5cd-222">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="db5cd-223">Esprimere un voto o vedere lo stato di questo bug [con questo collegamento](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="db5cd-223">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="db5cd-224">Il codice precedente crea una tabella HTML con tre colonne.</span><span class="sxs-lookup"><span data-stu-id="db5cd-224">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="db5cd-225">Ogni colonna ha una casella di controllo e una didascalia che contiene il numero e il titolo del corso.</span><span class="sxs-lookup"><span data-stu-id="db5cd-225">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="db5cd-226">Le caselle di controllo hanno tutte lo stesso nome ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="db5cd-226">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="db5cd-227">L'uso dello stesso nome informa lo strumento di associazione di modelli che deve considerarle come un gruppo.</span><span class="sxs-lookup"><span data-stu-id="db5cd-227">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="db5cd-228">L'attributo value di ogni casella di controllo è impostato su `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-228">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="db5cd-229">Quando la pagina viene pubblicata, lo strumento di associazione di modelli passa una matrice costituita dai valori `CourseID` delle sole caselle di controllo selezionate.</span><span class="sxs-lookup"><span data-stu-id="db5cd-229">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="db5cd-230">Quando viene eseguito il rendering iniziale delle caselle di controllo, i corsi assegnati all'insegnante hanno attributi selezionati.</span><span class="sxs-lookup"><span data-stu-id="db5cd-230">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="db5cd-231">Eseguire l'app e testare la pagina Edit (Modifica) dell'insegnante aggiornata.</span><span class="sxs-lookup"><span data-stu-id="db5cd-231">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="db5cd-232">Modificare alcune assegnazioni di corsi.</span><span class="sxs-lookup"><span data-stu-id="db5cd-232">Change some course assignments.</span></span> <span data-ttu-id="db5cd-233">Le modifiche si riflettono nella pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="db5cd-233">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="db5cd-234">Nota: l'approccio qui adottato per la modifica dei dati dei corsi degli insegnanti funziona bene quando è presente un numero limitato di corsi.</span><span class="sxs-lookup"><span data-stu-id="db5cd-234">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="db5cd-235">Per raccolte molto più grandi, un'interfaccia utente diversa e un altro metodo di aggiornamento sono più efficienti e pratici.</span><span class="sxs-lookup"><span data-stu-id="db5cd-235">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="db5cd-236">Aggiornare la pagina di creazione dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="db5cd-236">Update the instructors Create page</span></span>

<span data-ttu-id="db5cd-237">Aggiornare il modello della pagina Create (Crea) dell'insegnante con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-237">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="db5cd-238">Il codice precedente è simile al codice *Pages/Instructors/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="db5cd-238">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="db5cd-239">Aggiornare la pagina Razor Create (Crea) dell'insegnante con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-239">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="db5cd-240">Testare la pagina Create (Crea) dell'insegnante.</span><span class="sxs-lookup"><span data-stu-id="db5cd-240">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="db5cd-241">Aggiornare la pagina Delete (Elimina)</span><span class="sxs-lookup"><span data-stu-id="db5cd-241">Update the Delete page</span></span>

<span data-ttu-id="db5cd-242">Aggiornare il modello di pagina Delete (Elimina) con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="db5cd-242">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="db5cd-243">Il codice precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="db5cd-243">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="db5cd-244">Usare il caricamento eager per la proprietà di navigazione `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="db5cd-244">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="db5cd-245">È necessario includere `CourseAssignments`, in caso contrario le assegnazioni non vengono eliminate quando viene eliminato l'insegnante.</span><span class="sxs-lookup"><span data-stu-id="db5cd-245">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="db5cd-246">Per evitare la necessità di leggerle, configurare l'eliminazione a catena nel database.</span><span class="sxs-lookup"><span data-stu-id="db5cd-246">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="db5cd-247">Se l'insegnante da eliminare è assegnato come responsabile di un dipartimento, tale assegnazione viene rimossa dal dipartimento.</span><span class="sxs-lookup"><span data-stu-id="db5cd-247">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="db5cd-248">[Precedente](xref:data/ef-rp/read-related-data)
> [Successivo](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="db5cd-248">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
