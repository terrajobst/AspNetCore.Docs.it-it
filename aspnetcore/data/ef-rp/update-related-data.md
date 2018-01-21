---
title: Pagine Razor con Entity Framework Core - aggiornare i dati correlati - 7 di 8
author: rick-anderson
description: "In questa esercitazione si aggiornerà i dati correlati mediante l'aggiornamento di campi di chiave esterni e le proprietà di navigazione."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 817bfd48dce94e7dbad96cb6f822494e3adfae1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="a906f-103">L'aggiornamento dei dati correlati - EF base Razor pagine (7 8)</span><span class="sxs-lookup"><span data-stu-id="a906f-103">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="a906f-104">Da [Tom Dykstra](https://github.com/tdykstra), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a906f-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="a906f-105">Questa esercitazione viene illustrato l'aggiornamento dei dati correlati.</span><span class="sxs-lookup"><span data-stu-id="a906f-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="a906f-106">Se si verificano problemi, è possibile risolvere, scaricare il [app completata per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="a906f-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="a906f-107">Le illustrazioni seguenti vengono illustrate alcune delle pagine completate.</span><span class="sxs-lookup"><span data-stu-id="a906f-107">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="a906f-108">![Pagina di modifica del corso](update-related-data/_static/course-edit.png)
![istruttore Modifica pagina](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="a906f-108">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="a906f-109">Esaminare e testare le pagine di corso di creazione e modifica.</span><span class="sxs-lookup"><span data-stu-id="a906f-109">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="a906f-110">Creare un nuovo corso.</span><span class="sxs-lookup"><span data-stu-id="a906f-110">Create a new course.</span></span> <span data-ttu-id="a906f-111">Il reparto è selezionato per la chiave primaria (integer), non il relativo nome.</span><span class="sxs-lookup"><span data-stu-id="a906f-111">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="a906f-112">Modificare il nuovo corso.</span><span class="sxs-lookup"><span data-stu-id="a906f-112">Edit the new course.</span></span> <span data-ttu-id="a906f-113">Al termine del test, eliminare il corso di nuovo.</span><span class="sxs-lookup"><span data-stu-id="a906f-113">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="a906f-114">Creare una classe base per condividere il codice</span><span class="sxs-lookup"><span data-stu-id="a906f-114">Create a base class to share common code</span></span>

<span data-ttu-id="a906f-115">Le pagine corsi/Create e corsi/modifica è necessario un elenco di nomi di reparto.</span><span class="sxs-lookup"><span data-stu-id="a906f-115">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="a906f-116">Crea il *Pages/Courses/DepartmentNamePageModel.cshtml.cs* classe di base per le pagine di creare e modificare:</span><span class="sxs-lookup"><span data-stu-id="a906f-116">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="a906f-117">Il codice precedente crea un [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) per contenere l'elenco di nomi di reparto.</span><span class="sxs-lookup"><span data-stu-id="a906f-117">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="a906f-118">Se `selectedDepartment` è specificato, tale reparto viene selezionato il `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="a906f-118">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="a906f-119">Le classi modello di pagina di creazione e modifica verranno derivare da `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="a906f-119">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="a906f-120">Personalizzare le pagine di corsi</span><span class="sxs-lookup"><span data-stu-id="a906f-120">Customize the Courses Pages</span></span>

<span data-ttu-id="a906f-121">Quando viene creata una nuova entità course, deve avere una relazione a una categoria esistente.</span><span class="sxs-lookup"><span data-stu-id="a906f-121">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="a906f-122">Per aggiungere un reparto durante la creazione di un corso, la classe base per creare e modificare contiene un elenco di riepilogo a discesa per la selezione del reparto.</span><span class="sxs-lookup"><span data-stu-id="a906f-122">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="a906f-123">Imposta l'elenco a discesa il `Course.DepartmentID` proprietà di chiave esterna (FK).</span><span class="sxs-lookup"><span data-stu-id="a906f-123">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="a906f-124">Core EF utilizza il `Course.DepartmentID` FK per caricare il `Department` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a906f-124">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Creare corsi](update-related-data/_static/ddl.png)

<span data-ttu-id="a906f-126">Aggiornare il modello di pagina Create con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a906f-126">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="a906f-127">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="a906f-127">The preceding code:</span></span>

* <span data-ttu-id="a906f-128">Classe derivata da `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="a906f-128">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="a906f-129">Usa `TryUpdateModelAsync` per impedire [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="a906f-129">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="a906f-130">Sostituisce `ViewData["DepartmentID"]` con `DepartmentNameSL` (dalla classe di base).</span><span class="sxs-lookup"><span data-stu-id="a906f-130">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="a906f-131">`ViewData["DepartmentID"]`viene sostituito con l'oggetto fortemente tipizzato `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="a906f-131">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="a906f-132">Modelli fortemente tipizzati sono consigliabili su scarsamente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="a906f-132">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="a906f-133">Per ulteriori informazioni, vedere [scarsamente tipizzato dati (ViewData e ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="a906f-133">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="a906f-134">Aggiornare la pagina creare corsi</span><span class="sxs-lookup"><span data-stu-id="a906f-134">Update the Courses Create page</span></span>

<span data-ttu-id="a906f-135">Aggiornamento *Pages/Courses/Create.cshtml* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="a906f-135">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="a906f-136">Il markup precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="a906f-136">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="a906f-137">Modifica l'etichetta da **DepartmentID** a **reparto**.</span><span class="sxs-lookup"><span data-stu-id="a906f-137">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="a906f-138">Sostituisce `"ViewBag.DepartmentID"` con `DepartmentNameSL` (dalla classe di base).</span><span class="sxs-lookup"><span data-stu-id="a906f-138">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="a906f-139">Aggiunge l'opzione "Selezionare Department".</span><span class="sxs-lookup"><span data-stu-id="a906f-139">Adds the "Select Department" option.</span></span> <span data-ttu-id="a906f-140">Questa modifica esegue il rendering di "Selezionare Department" anziché del primo reparto.</span><span class="sxs-lookup"><span data-stu-id="a906f-140">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="a906f-141">Aggiunge un messaggio di convalida quando il reparto non è selezionato.</span><span class="sxs-lookup"><span data-stu-id="a906f-141">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="a906f-142">La pagina Razor utilizza il [selezionare Helper Tag](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="a906f-142">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="a906f-143">La pagina di creazione di test.</span><span class="sxs-lookup"><span data-stu-id="a906f-143">Test the Create page.</span></span> <span data-ttu-id="a906f-144">Nella pagina di creazione viene visualizzato il nome di reparto, anziché l'ID del reparto.</span><span class="sxs-lookup"><span data-stu-id="a906f-144">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="a906f-145">Aggiornare la pagina Modifica corsi.</span><span class="sxs-lookup"><span data-stu-id="a906f-145">Update the Courses Edit page.</span></span>

<span data-ttu-id="a906f-146">Aggiornare il modello di pagina di modifica con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a906f-146">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="a906f-147">Le modifiche sono simili a quelle apportate nel modello di pagina di creazione.</span><span class="sxs-lookup"><span data-stu-id="a906f-147">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="a906f-148">Nel codice precedente, `PopulateDepartmentsDropDownList` passa l'ID del reparto, selezionare il reparto specificato nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="a906f-148">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="a906f-149">Aggiornamento *Pages/Courses/Edit.cshtml* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="a906f-149">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="a906f-150">Il markup precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="a906f-150">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="a906f-151">Visualizza l'ID del corso.</span><span class="sxs-lookup"><span data-stu-id="a906f-151">Displays the course ID.</span></span> <span data-ttu-id="a906f-152">In genere non viene visualizzata la chiave primaria (PK) di un'entità.</span><span class="sxs-lookup"><span data-stu-id="a906f-152">Generally the Primary Key (PK) of an entity is not displayed.</span></span> <span data-ttu-id="a906f-153">Usata è in genere utilizzati per gli utenti.</span><span class="sxs-lookup"><span data-stu-id="a906f-153">PKs are usually meaningless to users.</span></span> <span data-ttu-id="a906f-154">In questo caso, la chiave pubblica è il numero di corso.</span><span class="sxs-lookup"><span data-stu-id="a906f-154">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="a906f-155">Modifica l'etichetta da **DepartmentID** a **reparto**.</span><span class="sxs-lookup"><span data-stu-id="a906f-155">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="a906f-156">Sostituisce `"ViewBag.DepartmentID"` con `DepartmentNameSL` (dalla classe di base).</span><span class="sxs-lookup"><span data-stu-id="a906f-156">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="a906f-157">Aggiunge l'opzione "Selezionare Department".</span><span class="sxs-lookup"><span data-stu-id="a906f-157">Adds the "Select Department" option.</span></span> <span data-ttu-id="a906f-158">Questa modifica esegue il rendering di "Selezionare Department" anziché del primo reparto.</span><span class="sxs-lookup"><span data-stu-id="a906f-158">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="a906f-159">Aggiunge un messaggio di convalida quando il reparto non è selezionato.</span><span class="sxs-lookup"><span data-stu-id="a906f-159">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="a906f-160">La pagina contiene un campo nascosto (`<input type="hidden">`) per il numero di corso.</span><span class="sxs-lookup"><span data-stu-id="a906f-160">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="a906f-161">Aggiunta di un `<label>` tag helper con `asp-for="Course.CourseID"` non eliminano la necessità per il campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="a906f-161">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="a906f-162">`<input type="hidden">`è necessario per il numero del corso da includere nei dati inviati quando l'utente fa clic **salvare**.</span><span class="sxs-lookup"><span data-stu-id="a906f-162">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="a906f-163">Testare il codice aggiornato.</span><span class="sxs-lookup"><span data-stu-id="a906f-163">Test the updated code.</span></span> <span data-ttu-id="a906f-164">Creare, modificare ed eliminare un corso.</span><span class="sxs-lookup"><span data-stu-id="a906f-164">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="a906f-165">Aggiungere i dettagli di AsNoTracking ed eliminare i modelli di pagina</span><span class="sxs-lookup"><span data-stu-id="a906f-165">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="a906f-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) può migliorare le prestazioni quando il rilevamento non è necessario.</span><span class="sxs-lookup"><span data-stu-id="a906f-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking is not required.</span></span> <span data-ttu-id="a906f-167">Aggiungere `AsNoTracking` al modello di pagina di eliminazione e i dettagli.</span><span class="sxs-lookup"><span data-stu-id="a906f-167">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="a906f-168">Il codice seguente viene illustrato il modello di pagina Delete aggiornato:</span><span class="sxs-lookup"><span data-stu-id="a906f-168">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="a906f-169">Aggiornamento di `OnGetAsync` metodo il *Pages/Courses/Details.cshtml.cs* file:</span><span class="sxs-lookup"><span data-stu-id="a906f-169">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="a906f-170">Modificare le pagine di eliminazione e dettagli</span><span class="sxs-lookup"><span data-stu-id="a906f-170">Modify the Delete and Details pages</span></span>

<span data-ttu-id="a906f-171">Aggiornare la pagina Razor eliminare con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="a906f-171">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="a906f-172">Apportare le stesse modifiche alla pagina dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="a906f-172">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="a906f-173">Le pagine del corso di test</span><span class="sxs-lookup"><span data-stu-id="a906f-173">Test the Course pages</span></span>

<span data-ttu-id="a906f-174">Test di creare, modificare, informazioni dettagliate ed eliminare.</span><span class="sxs-lookup"><span data-stu-id="a906f-174">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="a906f-175">Aggiornare le pagine instructor</span><span class="sxs-lookup"><span data-stu-id="a906f-175">Update the instructor pages</span></span>

<span data-ttu-id="a906f-176">Nelle sezioni seguenti di aggiornare le pagine di istruttore.</span><span class="sxs-lookup"><span data-stu-id="a906f-176">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="a906f-177">Aggiungere il percorso di office</span><span class="sxs-lookup"><span data-stu-id="a906f-177">Add office location</span></span>

<span data-ttu-id="a906f-178">Quando si modifica un record istruttore, si desidera aggiornare l'assegnazione dell'ufficio dell'insegnante.</span><span class="sxs-lookup"><span data-stu-id="a906f-178">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="a906f-179">Il `Instructor` entità dispone di una relazione uno-a-zero-o-uno con il `OfficeAssignment` entità.</span><span class="sxs-lookup"><span data-stu-id="a906f-179">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="a906f-180">Il codice istruttore deve gestire:</span><span class="sxs-lookup"><span data-stu-id="a906f-180">The instructor code must handle:</span></span>

* <span data-ttu-id="a906f-181">Se l'utente cancella l'assegnazione di office, eliminare il `OfficeAssignment` entità.</span><span class="sxs-lookup"><span data-stu-id="a906f-181">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="a906f-182">Se l'utente immette l'assegnazione dell'ufficio ed era vuoto, creare un nuovo `OfficeAssignment` entità.</span><span class="sxs-lookup"><span data-stu-id="a906f-182">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="a906f-183">Se l'utente modifica l'assegnazione di office, aggiornare il `OfficeAssignment` entità.</span><span class="sxs-lookup"><span data-stu-id="a906f-183">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="a906f-184">Aggiornare il modello di pagina di modifica i docenti con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a906f-184">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="a906f-185">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="a906f-185">The preceding code:</span></span>

- <span data-ttu-id="a906f-186">Ottiene l'oggetto corrente `Instructor` entità dal database tramite il caricamento immediato per la `OfficeAssignment` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a906f-186">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="a906f-187">Aggiorna l'oggetto recuperato `Instructor` entità con valori dallo strumento di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="a906f-187">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="a906f-188">`TryUpdateModel`impedisce [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="a906f-188">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="a906f-189">Se il percorso di office è vuoto, imposta `Instructor.OfficeAssignment` su null.</span><span class="sxs-lookup"><span data-stu-id="a906f-189">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="a906f-190">Quando `Instructor.OfficeAssignment` è null, la riga correlata nella `OfficeAssignment` tabella viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="a906f-190">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="a906f-191">Aggiornare la pagina Modifica instructor</span><span class="sxs-lookup"><span data-stu-id="a906f-191">Update the instructor Edit page</span></span>

<span data-ttu-id="a906f-192">Aggiornamento *Pages/Instructors/Edit.cshtml* con il percorso di office:</span><span class="sxs-lookup"><span data-stu-id="a906f-192">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="a906f-193">Verificare che è possibile modificare la posizione di un ufficio i docenti.</span><span class="sxs-lookup"><span data-stu-id="a906f-193">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="a906f-194">Aggiungere esercitazioni per la pagina Modifica instructor</span><span class="sxs-lookup"><span data-stu-id="a906f-194">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="a906f-195">I docenti potrebbero indicare un numero qualsiasi di corsi.</span><span class="sxs-lookup"><span data-stu-id="a906f-195">Instructors may teach any number of courses.</span></span> <span data-ttu-id="a906f-196">In questa sezione, aggiungere la possibilità di modificare le assegnazioni dei corsi.</span><span class="sxs-lookup"><span data-stu-id="a906f-196">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="a906f-197">La figura seguente mostra l'istruttore aggiornato pagina Modifica:</span><span class="sxs-lookup"><span data-stu-id="a906f-197">The following image shows the updated instructor Edit page:</span></span>

![Pagina Modifica istruttore corsi](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="a906f-199">`Course`e `Instructor` ha una relazione molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="a906f-199">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="a906f-200">Per aggiungere e rimuovere le relazioni, aggiungere e rimuovere le entità dal `CourseAssignments` aggiungere set di entità.</span><span class="sxs-lookup"><span data-stu-id="a906f-200">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="a906f-201">Caselle di controllo abilitare modifiche ai corsi assegnato a un docente.</span><span class="sxs-lookup"><span data-stu-id="a906f-201">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="a906f-202">Una casella di controllo viene visualizzata per ogni corso nel database.</span><span class="sxs-lookup"><span data-stu-id="a906f-202">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="a906f-203">Vengono controllati i corsi istruttore assegnato a.</span><span class="sxs-lookup"><span data-stu-id="a906f-203">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="a906f-204">L'utente può selezionare o deselezionare le caselle di controllo per modificare le assegnazioni dei corsi.</span><span class="sxs-lookup"><span data-stu-id="a906f-204">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="a906f-205">Se il numero di corsi è molto più elevato:</span><span class="sxs-lookup"><span data-stu-id="a906f-205">If the number of courses were much greater:</span></span>

* <span data-ttu-id="a906f-206">Utilizzare probabilmente un'interfaccia utente differente per visualizzare i corsi.</span><span class="sxs-lookup"><span data-stu-id="a906f-206">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="a906f-207">Il metodo di manipolazione di un'entità di join per creare o eliminare le relazioni non modificherebbe.</span><span class="sxs-lookup"><span data-stu-id="a906f-207">The method of manipulating a join entity to create or delete relationships would not change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="a906f-208">Aggiungere le classi per supportare creare e modificare pagine instructor</span><span class="sxs-lookup"><span data-stu-id="a906f-208">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="a906f-209">Creare *SchoolViewModels/AssignedCourseData.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a906f-209">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="a906f-210">La `AssignedCourseData` classe contiene i dati per creare le caselle di controllo per i corsi assegnati da un docente.</span><span class="sxs-lookup"><span data-stu-id="a906f-210">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="a906f-211">Creare il *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* classe di base:</span><span class="sxs-lookup"><span data-stu-id="a906f-211">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="a906f-212">Il `InstructorCoursesPageModel` è la classe base, verrà utilizzato per la modifica e creare modelli di pagina.</span><span class="sxs-lookup"><span data-stu-id="a906f-212">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="a906f-213">`PopulateAssignedCourseData`legge tutti `Course` entità per popolare `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="a906f-213">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="a906f-214">Per ogni corso, imposta il codice di `CourseID`, titolo e assegnato o meno istruttore al corso.</span><span class="sxs-lookup"><span data-stu-id="a906f-214">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="a906f-215">Oggetto [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) viene utilizzato per creare ricerche efficienti.</span><span class="sxs-lookup"><span data-stu-id="a906f-215">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="a906f-216">Modello di pagina Modifica i docenti</span><span class="sxs-lookup"><span data-stu-id="a906f-216">Instructors Edit page model</span></span>

<span data-ttu-id="a906f-217">Aggiornare il modello di pagina di modifica istruttore con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a906f-217">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="a906f-218">Il codice precedente gestisce le modifiche di assegnazione di office.</span><span class="sxs-lookup"><span data-stu-id="a906f-218">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="a906f-219">Aggiornare la visualizzazione Razor istruttore:</span><span class="sxs-lookup"><span data-stu-id="a906f-219">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="a906f-220">Quando si incolla il codice in Visual Studio, le interruzioni di riga vengono modificate in modo che interrompe il codice.</span><span class="sxs-lookup"><span data-stu-id="a906f-220">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="a906f-221">Premere Ctrl + Z di una volta per annullare la formattazione automatica.</span><span class="sxs-lookup"><span data-stu-id="a906f-221">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="a906f-222">CTRL + Z corregge le interruzioni di riga in modo che vengano visualizzate come gli elementi visualizzati.</span><span class="sxs-lookup"><span data-stu-id="a906f-222">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="a906f-223">Il rientro non deve necessariamente essere perfetto, ma la `@</tr><tr>`, `@:<td>`, `@:</td>`, e `@:</tr>` righe devono essere su una singola riga come illustrato.</span><span class="sxs-lookup"><span data-stu-id="a906f-223">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="a906f-224">Con il blocco di nuovo codice selezionato, premere Tab tre volte per allineare il nuovo codice con il codice esistente.</span><span class="sxs-lookup"><span data-stu-id="a906f-224">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="a906f-225">Votare o esaminare lo stato del bug [con questo collegamento](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="a906f-225">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="a906f-226">Il codice precedente crea una tabella HTML che ha tre colonne.</span><span class="sxs-lookup"><span data-stu-id="a906f-226">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="a906f-227">Ogni colonna ha una casella di controllo e una didascalia che contiene il numero di corso e il titolo.</span><span class="sxs-lookup"><span data-stu-id="a906f-227">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="a906f-228">Tutte le caselle di controllo con lo stesso nome ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="a906f-228">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="a906f-229">Utilizzando lo stesso nome informa il raccoglitore di modelli per li considera come un gruppo.</span><span class="sxs-lookup"><span data-stu-id="a906f-229">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="a906f-230">Il valore di attributo di ogni casella di controllo è impostato su `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="a906f-230">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="a906f-231">Quando viene eseguito il postback della pagina, il gestore di associazione del modello passa una matrice che include il `CourseID` i valori per solo le caselle di controllo selezionate.</span><span class="sxs-lookup"><span data-stu-id="a906f-231">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="a906f-232">Quando le caselle di controllo sono inizialmente eseguito il rendering, corsi assegnati a istruttore sono controllati gli attributi.</span><span class="sxs-lookup"><span data-stu-id="a906f-232">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="a906f-233">Eseguire l'applicazione e verificare la pagina di modifica i docenti aggiornato.</span><span class="sxs-lookup"><span data-stu-id="a906f-233">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="a906f-234">Modificare alcune esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="a906f-234">Change some course assignments.</span></span> <span data-ttu-id="a906f-235">Le modifiche vengono riflesse nella pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="a906f-235">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="a906f-236">Nota: L'approccio adottato qui per modificare i dati del corso istruttore funziona anche quando è presente un numero limitato di corsi.</span><span class="sxs-lookup"><span data-stu-id="a906f-236">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="a906f-237">Per le raccolte che sono più grandi, un'interfaccia utente differente e un altro metodo di aggiornamento sono più utilizzabili ed efficienti.</span><span class="sxs-lookup"><span data-stu-id="a906f-237">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="a906f-238">Aggiornare la pagina Crea i docenti</span><span class="sxs-lookup"><span data-stu-id="a906f-238">Update the instructors Create page</span></span>

<span data-ttu-id="a906f-239">Aggiornare il modello di pagina istruttore Create con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a906f-239">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="a906f-240">Il codice precedente è simile al *Pages/Instructors/Edit.cshtml.cs* codice.</span><span class="sxs-lookup"><span data-stu-id="a906f-240">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="a906f-241">Aggiornare la pagina di creare Razor istruttore con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="a906f-241">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="a906f-242">Pagina Crea insegnante di test.</span><span class="sxs-lookup"><span data-stu-id="a906f-242">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="a906f-243">Aggiornare la pagina di eliminazione</span><span class="sxs-lookup"><span data-stu-id="a906f-243">Update the Delete page</span></span>

<span data-ttu-id="a906f-244">Aggiornare il modello di pagina di Delete con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a906f-244">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="a906f-245">Il codice precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="a906f-245">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="a906f-246">Utilizza il caricamento immediato per la `CourseAssignments` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a906f-246">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="a906f-247">`CourseAssignments`deve essere inclusa o non vengano eliminati quando viene eliminato l'istruttore.</span><span class="sxs-lookup"><span data-stu-id="a906f-247">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="a906f-248">Per evitare la necessità di leggerli, configurare eliminazione a catena nel database.</span><span class="sxs-lookup"><span data-stu-id="a906f-248">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="a906f-249">Se istruttore da eliminare viene assegnato come responsabile per i reparti, rimuove l'assegnazione istruttore i reparti.</span><span class="sxs-lookup"><span data-stu-id="a906f-249">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a906f-250">[Precedente](xref:data/ef-rp/read-related-data)
[Successivo](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="a906f-250">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
