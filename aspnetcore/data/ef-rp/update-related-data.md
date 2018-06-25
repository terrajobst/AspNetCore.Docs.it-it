---
title: Razor Pages con EF Core in ASP.NET Core - Aggiornare dati correlati - 7 di 8
author: rick-anderson
description: In questa esercitazione verrà effettuato l'aggiornamento di dati correlati tramite l'aggiornamento di campi di chiave esterna e proprietà di navigazione.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: e987971f60e5c5a9fb79e30440c7c986df64447e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275294"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="7e9dd-103">Razor Pages con EF Core in ASP.NET Core - Aggiornare dati correlati - 7 di 8</span><span class="sxs-lookup"><span data-stu-id="7e9dd-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="7e9dd-104">Di [Tom Dykstra](https://github.com/tdykstra), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7e9dd-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="7e9dd-105">Questa esercitazione illustra l'aggiornamento di dati correlati.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="7e9dd-106">Se si verificano problemi che non si è in grado di risolvere, scaricare l'[app completa per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="7e9dd-107">Le illustrazioni seguenti mostrano alcune pagine completate.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-107">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="7e9dd-108">![Pagina di modifica del corso](update-related-data/_static/course-edit.png)
![Pagina di modifica dell'insegnante](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="7e9dd-108">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="7e9dd-109">Esaminare e testare le pagine di creazione e modifica del corso.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-109">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="7e9dd-110">Creare un nuovo corso.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-110">Create a new course.</span></span> <span data-ttu-id="7e9dd-111">Il dipartimento viene selezionato in base alla chiave primaria (numero intero), non in base al nome.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-111">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="7e9dd-112">Modificare il nuovo corso.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-112">Edit the new course.</span></span> <span data-ttu-id="7e9dd-113">Al termine del test, eliminare il nuovo corso.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-113">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="7e9dd-114">Creare una classe di base per condividere il codice comune</span><span class="sxs-lookup"><span data-stu-id="7e9dd-114">Create a base class to share common code</span></span>

<span data-ttu-id="7e9dd-115">La pagina di creazione e quella di modifica del corso hanno bisogno dell'elenco dei nomi dei dipartimenti.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-115">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="7e9dd-116">Creare la classe di base *Pages/Courses/DepartmentNamePageModel.cshtml.cs* per le pagine Create (Crea) ed Edit (Modifica):</span><span class="sxs-lookup"><span data-stu-id="7e9dd-116">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="7e9dd-117">Il codice precedente crea un oggetto [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) in cui inserire l'elenco dei nomi dei dipartimenti.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-117">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="7e9dd-118">Se `selectedDepartment` è specificato, il dipartimento corrispondente viene selezionato in `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-118">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="7e9dd-119">Le classi modello delle pagine Create (Crea) ed Edit (Modifica) derivano da `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-119">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="7e9dd-120">Personalizzare le pagine dei corsi</span><span class="sxs-lookup"><span data-stu-id="7e9dd-120">Customize the Courses Pages</span></span>

<span data-ttu-id="7e9dd-121">Quando viene creata, una nuova entità corso deve essere in relazione con un dipartimento esistente.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-121">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="7e9dd-122">Per consentire l'aggiunta di un dipartimento durante la creazione di un corso, la classe di base per Create (Crea) ed Edit (Modifica) contiene un elenco a discesa per la selezione del dipartimento.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-122">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="7e9dd-123">L'elenco a discesa imposta la proprietà di chiave esterna `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-123">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="7e9dd-124">Core EF usa la chiave esterna `Course.DepartmentID` per caricare la proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-124">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Create course (Crea corso)](update-related-data/_static/ddl.png)

<span data-ttu-id="7e9dd-126">Aggiornare il modello della pagina Create (Crea) con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-126">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="7e9dd-127">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-127">The preceding code:</span></span>

* <span data-ttu-id="7e9dd-128">Classe derivata da `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-128">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="7e9dd-129">Usa `TryUpdateModelAsync` per evitare l'[overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-129">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="7e9dd-130">Sostituisce `ViewData["DepartmentID"]` con `DepartmentNameSL` (dalla classe di base).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-130">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="7e9dd-131">`ViewData["DepartmentID"]` viene sostituito con `DepartmentNameSL` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-131">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="7e9dd-132">I modelli fortemente tipizzati sono preferibili a quelli scarsamente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-132">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="7e9dd-133">Per altre informazioni, vedere [Dati con tipizzazione debole (ViewData e ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-133">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="7e9dd-134">Aggiornare la pagina di creazione dei corsi</span><span class="sxs-lookup"><span data-stu-id="7e9dd-134">Update the Courses Create page</span></span>

<span data-ttu-id="7e9dd-135">Aggiornare *Pages/Courses/Create.cshtml* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-135">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="7e9dd-136">Il markup precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-136">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="7e9dd-137">Modifica la didascalia da **DepartmentID** a **Department**.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-137">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="7e9dd-138">Sostituisce `"ViewBag.DepartmentID"` con `DepartmentNameSL` (dalla classe di base).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-138">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="7e9dd-139">Aggiunge l'opzione "Select Department" (Selezionare il dipartimento).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-139">Adds the "Select Department" option.</span></span> <span data-ttu-id="7e9dd-140">Questa modifica esegue il rendering di "Select Department" (Selezionare il dipartimento) anziché del primo dipartimento.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-140">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="7e9dd-141">Aggiunge un messaggio di convalida quando il dipartimento non è selezionato.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-141">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="7e9dd-142">La pagina Razor usa l'[helper tag di selezione](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="7e9dd-142">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="7e9dd-143">Testare la pagina Create (Crea).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-143">Test the Create page.</span></span> <span data-ttu-id="7e9dd-144">La pagina Create (Crea) visualizza il nome, anziché l'ID, del dipartimento.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-144">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="7e9dd-145">Aggiornare la pagina di modifica dei corsi.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-145">Update the Courses Edit page.</span></span>

<span data-ttu-id="7e9dd-146">Aggiornare il modello della pagina Edit (Modifica) con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-146">Update the edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="7e9dd-147">Le modifiche sono simili a quelle apportate nel modello della pagina Create (Crea).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-147">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="7e9dd-148">Nel codice precedente, `PopulateDepartmentsDropDownList` passa l'ID del dipartimento, che seleziona il dipartimento specificato nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-148">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="7e9dd-149">Aggiornare *Pages/Courses/Edit.cshtml* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-149">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="7e9dd-150">Il markup precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-150">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="7e9dd-151">Visualizza l'ID del corso.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-151">Displays the course ID.</span></span> <span data-ttu-id="7e9dd-152">In genere, la chiave primaria di un'entità non viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-152">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="7e9dd-153">Le chiavi primarie di solito non sono significative per gli utenti.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-153">PKs are usually meaningless to users.</span></span> <span data-ttu-id="7e9dd-154">In questo caso, la chiave primaria corrisponde al numero del corso.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-154">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="7e9dd-155">Modifica la didascalia da **DepartmentID** a **Department**.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-155">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="7e9dd-156">Sostituisce `"ViewBag.DepartmentID"` con `DepartmentNameSL` (dalla classe di base).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-156">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="7e9dd-157">La pagina contiene un campo nascosto (`<input type="hidden">`) per il numero del corso.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-157">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="7e9dd-158">L'aggiunta di un helper tag `<label>` con `asp-for="Course.CourseID"` non elimina la necessità del campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-158">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="7e9dd-159">`<input type="hidden">` è necessario per includere il numero del corso tra i dati inviati quando l'utente fa clic su **Save** (Salva).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-159">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="7e9dd-160">Testare il codice aggiornato.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-160">Test the updated code.</span></span> <span data-ttu-id="7e9dd-161">Creare, modificare ed eliminare un corso.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-161">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="7e9dd-162">Aggiungere AsNoTracking ai modelli delle pagine Details (Dettagli) e Delete (Elimina)</span><span class="sxs-lookup"><span data-stu-id="7e9dd-162">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="7e9dd-163">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) può migliorare le prestazioni quando la registrazione non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-163">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="7e9dd-164">Aggiungere `AsNoTracking` alle pagine Delete (Elimina) e Details (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-164">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="7e9dd-165">Il codice seguente illustra il modello della pagina Delete (Elimina) aggiornato:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-165">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="7e9dd-166">Aggiornare il metodo `OnGetAsync` nel file *Pages/Courses/Details.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-166">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="7e9dd-167">Modificare le pagine Delete (Elimina) e Details (Dettagli)</span><span class="sxs-lookup"><span data-stu-id="7e9dd-167">Modify the Delete and Details pages</span></span>

<span data-ttu-id="7e9dd-168">Aggiornare la pagina Razor Delete (Elimina) con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-168">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="7e9dd-169">Apportare le stesse modifiche alla pagina Details (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-169">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="7e9dd-170">Testare le pagine del corso</span><span class="sxs-lookup"><span data-stu-id="7e9dd-170">Test the Course pages</span></span>

<span data-ttu-id="7e9dd-171">Testare le pagine di creazione, modifica, dettagli ed eliminazione.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-171">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="7e9dd-172">Aggiornare le pagine dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="7e9dd-172">Update the instructor pages</span></span>

<span data-ttu-id="7e9dd-173">Le sezioni seguenti aggiornano le pagine dell'insegnante.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-173">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="7e9dd-174">Aggiungere la posizione dell'ufficio</span><span class="sxs-lookup"><span data-stu-id="7e9dd-174">Add office location</span></span>

<span data-ttu-id="7e9dd-175">Quando si modifica il record di un insegnante, può essere necessario aggiornare l'assegnazione dell'ufficio.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-175">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="7e9dd-176">L'entità `Instructor` ha una relazione uno-a-zero-o-uno con l'entità `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-176">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="7e9dd-177">Il codice relativo all'insegnante deve gestire i casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-177">The instructor code must handle:</span></span>

* <span data-ttu-id="7e9dd-178">Se l'utente cancella l'assegnazione dell'ufficio, eliminare l'entità `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-178">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="7e9dd-179">Se l'utente immette l'assegnazione dell'ufficio e questa era vuota, creare una nuova entità `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-179">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="7e9dd-180">Se l'utente modifica l'assegnazione dell'ufficio, aggiornare l'entità `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-180">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="7e9dd-181">Aggiornare il modello della pagina Edit (Modifica) dell'insegnante con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-181">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="7e9dd-182">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-182">The preceding code:</span></span>

- <span data-ttu-id="7e9dd-183">Ottiene l'entità `Instructor` corrente dal database tramite il caricamento eager per la proprietà di navigazione `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-183">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="7e9dd-184">Aggiorna l'entità `Instructor` recuperata con valori dallo strumento di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-184">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="7e9dd-185">`TryUpdateModel` impedisce l'[overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-185">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="7e9dd-186">Se la posizione dell'ufficio è vuota, imposta `Instructor.OfficeAssignment` su Null.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-186">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="7e9dd-187">Se `Instructor.OfficeAssignment` è Null, la riga correlata nella tabella `OfficeAssignment` viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-187">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="7e9dd-188">Aggiornare la pagina di modifica dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="7e9dd-188">Update the instructor Edit page</span></span>

<span data-ttu-id="7e9dd-189">Aggiornare *Pages/Instructors/Edit.cshtml* con la posizione dell'ufficio:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-189">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="7e9dd-190">Verificare che sia possibile modificare la posizione dell'ufficio di un insegnante.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-190">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="7e9dd-191">Aggiungere assegnazioni di corso nella pagina di modifica dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="7e9dd-191">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="7e9dd-192">Gli insegnanti possono tenere un numero qualsiasi di corsi.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-192">Instructors may teach any number of courses.</span></span> <span data-ttu-id="7e9dd-193">In questa sezione, viene aggiunta la possibilità di modificare le assegnazioni dei corsi.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-193">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="7e9dd-194">La figura seguente illustra la pagina Edit (Modifica) dell'insegnante aggiornata:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-194">The following image shows the updated instructor Edit page:</span></span>

![Pagina di modifica dell'insegnante con corsi](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="7e9dd-196">`Course` e `Instructor` hanno una relazione molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-196">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="7e9dd-197">Per aggiungere e rimuovere relazioni, aggiungere e rimuovere entità dal set di entità di join `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-197">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="7e9dd-198">Per consentire la modifica dei corsi assegnati a un insegnante, si usano caselle di controllo.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-198">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="7e9dd-199">Per ogni corso nel database è visualizzata una casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-199">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="7e9dd-200">I corsi assegnati all'insegnante corrente sono selezionati.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-200">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="7e9dd-201">L'utente può selezionare e deselezionare le caselle di controllo per modificare le assegnazioni dei corsi.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-201">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="7e9dd-202">Se il numero di corsi è molto più elevato:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-202">If the number of courses were much greater:</span></span>

* <span data-ttu-id="7e9dd-203">Per visualizzare i corsi è consigliabile usare un'interfaccia utente diversa.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-203">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="7e9dd-204">Il metodo di modifica di un'entità di join per creare o eliminare relazioni non cambia.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-204">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="7e9dd-205">Aggiungere classi per supportare le pagine di creazione e modifica dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="7e9dd-205">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="7e9dd-206">Creare *SchoolViewModels/AssignedCourseData.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-206">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="7e9dd-207">La classe `AssignedCourseData` contiene i dati per la creazione delle caselle di controllo per i corsi assegnati a un insegnante.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-207">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="7e9dd-208">Creare la classe di base *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-208">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="7e9dd-209">`InstructorCoursesPageModel` è la classe di base che verrà usata per i modelli delle pagine Edit (Modifica) e Create (Crea).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-209">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="7e9dd-210">`PopulateAssignedCourseData` legge tutte le entità `Course` per popolare `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-210">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="7e9dd-211">Per ogni corso, il codice imposta `CourseID` e titolo, e stabilisce se l'insegnante è assegnato al corso.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-211">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="7e9dd-212">Per creare ricerche efficienti, si usa un [HashSet](/dotnet/api/system.collections.generic.hashset-1).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-212">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="7e9dd-213">Modello della pagina di modifica dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="7e9dd-213">Instructors Edit page model</span></span>

<span data-ttu-id="7e9dd-214">Aggiornare il modello della pagina Edit (Modifica) dell'insegnante con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-214">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="7e9dd-215">Il codice precedente gestisce le modifiche alle assegnazioni di ufficio.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-215">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="7e9dd-216">Aggiornare la visualizzazione Razor degli insegnanti:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-216">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="7e9dd-217">Quando si incolla il codice in Visual Studio, le interruzioni di riga vengono modificate in modo tale da danneggiare il codice.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-217">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="7e9dd-218">Premere Ctrl + Z una volta per annullare la formattazione automatica.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-218">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="7e9dd-219">Premendo CTRL + Z si correggono le interruzioni di riga, che vengono visualizzate come illustrato qui.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-219">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="7e9dd-220">Il rientro non deve necessariamente essere perfetto, ma le righe `@</tr><tr>`, `@:<td>`, `@:</td>` e `@:</tr>` devono trovarsi in una sola riga, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-220">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="7e9dd-221">Dopo aver selezionato il blocco di nuovo codice, premere Tab tre volte per allineare il nuovo codice con il codice esistente.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-221">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="7e9dd-222">Esprimere un voto o vedere lo stato di questo bug [con questo collegamento](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="7e9dd-222">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="7e9dd-223">Il codice precedente crea una tabella HTML con tre colonne.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-223">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="7e9dd-224">Ogni colonna ha una casella di controllo e una didascalia che contiene il numero e il titolo del corso.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-224">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="7e9dd-225">Le caselle di controllo hanno tutte lo stesso nome ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="7e9dd-225">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="7e9dd-226">L'uso dello stesso nome informa lo strumento di associazione di modelli che deve considerarle come un gruppo.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-226">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="7e9dd-227">L'attributo value di ogni casella di controllo è impostato su `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-227">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="7e9dd-228">Quando la pagina viene pubblicata, lo strumento di associazione di modelli passa una matrice costituita dai valori `CourseID` delle sole caselle di controllo selezionate.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-228">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="7e9dd-229">Quando viene eseguito il rendering iniziale delle caselle di controllo, i corsi assegnati all'insegnante hanno attributi selezionati.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-229">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="7e9dd-230">Eseguire l'app e testare la pagina Edit (Modifica) dell'insegnante aggiornata.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-230">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="7e9dd-231">Modificare alcune assegnazioni di corsi.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-231">Change some course assignments.</span></span> <span data-ttu-id="7e9dd-232">Le modifiche si riflettono nella pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-232">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="7e9dd-233">Nota: l'approccio qui adottato per la modifica dei dati dei corsi degli insegnanti funziona bene quando è presente un numero limitato di corsi.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-233">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="7e9dd-234">Per raccolte molto più grandi, un'interfaccia utente diversa e un altro metodo di aggiornamento sono più efficienti e pratici.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-234">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="7e9dd-235">Aggiornare la pagina di creazione dell'insegnante</span><span class="sxs-lookup"><span data-stu-id="7e9dd-235">Update the instructors Create page</span></span>

<span data-ttu-id="7e9dd-236">Aggiornare il modello della pagina Create (Crea) dell'insegnante con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-236">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="7e9dd-237">Il codice precedente è simile al codice *Pages/Instructors/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-237">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="7e9dd-238">Aggiornare la pagina Razor Create (Crea) dell'insegnante con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-238">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="7e9dd-239">Testare la pagina Create (Crea) dell'insegnante.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-239">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="7e9dd-240">Aggiornare la pagina Delete (Elimina)</span><span class="sxs-lookup"><span data-stu-id="7e9dd-240">Update the Delete page</span></span>

<span data-ttu-id="7e9dd-241">Aggiornare il modello di pagina Delete (Elimina) con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-241">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="7e9dd-242">Il codice precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="7e9dd-242">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="7e9dd-243">Usare il caricamento eager per la proprietà di navigazione `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-243">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="7e9dd-244">È necessario includere `CourseAssignments`, in caso contrario le assegnazioni non vengono eliminate quando viene eliminato l'insegnante.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-244">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="7e9dd-245">Per evitare la necessità di leggerle, configurare l'eliminazione a catena nel database.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-245">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="7e9dd-246">Se l'insegnante da eliminare è assegnato come responsabile di un dipartimento, tale assegnazione viene rimossa dal dipartimento.</span><span class="sxs-lookup"><span data-stu-id="7e9dd-246">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7e9dd-247">[Precedente](xref:data/ef-rp/read-related-data)
> [Successivo](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="7e9dd-247">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
