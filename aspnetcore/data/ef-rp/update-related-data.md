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
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a>Razor Pages con EF Core in ASP.NET Core - Aggiornare dati correlati - 7 di 8

Di [Tom Dykstra](https://github.com/tdykstra), e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Questa esercitazione illustra l'aggiornamento di dati correlati. Se si verificano problemi che non si è in grado di risolvere, [scaricare o visualizzare l'app completa](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). [Istruzioni per il download](xref:index#how-to-download-a-sample).

Le illustrazioni seguenti mostrano alcune pagine completate.

![Pagina di modifica del corso](update-related-data/_static/course-edit.png)
![Pagina di modifica dell'insegnante](update-related-data/_static/instructor-edit-courses.png)

Esaminare e testare le pagine di creazione e modifica del corso. Creare un nuovo corso. Il dipartimento viene selezionato in base alla chiave primaria (numero intero), non in base al nome. Modificare il nuovo corso. Al termine del test, eliminare il nuovo corso.

## <a name="create-a-base-class-to-share-common-code"></a>Creare una classe di base per condividere il codice comune

La pagina di creazione e quella di modifica del corso hanno bisogno dell'elenco dei nomi dei dipartimenti. Creare la classe di base *Pages/Courses/DepartmentNamePageModel.cshtml.cs* per le pagine Create (Crea) ed Edit (Modifica):

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

Il codice precedente crea un oggetto [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) in cui inserire l'elenco dei nomi dei dipartimenti. Se `selectedDepartment` è specificato, il dipartimento corrispondente viene selezionato in `SelectList`.

Le classi modello delle pagine Create (Crea) ed Edit (Modifica) derivano da `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Personalizzare le pagine dei corsi

Quando viene creata, una nuova entità corso deve essere in relazione con un dipartimento esistente. Per consentire l'aggiunta di un dipartimento durante la creazione di un corso, la classe di base per Create (Crea) ed Edit (Modifica) contiene un elenco a discesa per la selezione del dipartimento. L'elenco a discesa imposta la proprietà di chiave esterna `Course.DepartmentID`. Core EF usa la chiave esterna `Course.DepartmentID` per caricare la proprietà di navigazione `Department`.

![Create course (Crea corso)](update-related-data/_static/ddl.png)

Aggiornare il modello della pagina Create (Crea) con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

Il codice precedente:

* Classe derivata da `DepartmentNamePageModel`.
* Usa `TryUpdateModelAsync` per evitare l'[overposting](xref:data/ef-rp/crud#overposting).
* Sostituisce `ViewData["DepartmentID"]` con `DepartmentNameSL` (dalla classe di base).

`ViewData["DepartmentID"]` viene sostituito con `DepartmentNameSL` fortemente tipizzato. I modelli fortemente tipizzati sono preferibili a quelli scarsamente tipizzati. Per altre informazioni, vedere [Dati con tipizzazione debole (ViewData e ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Aggiornare la pagina di creazione dei corsi

Aggiornare *Pages/Courses/Create.cshtml* con il markup seguente:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

Il markup precedente apporta le modifiche seguenti:

* Modifica la didascalia da **DepartmentID** a **Department**.
* Sostituisce `"ViewBag.DepartmentID"` con `DepartmentNameSL` (dalla classe di base).
* Aggiunge l'opzione "Select Department" (Selezionare il dipartimento). Questa modifica esegue il rendering di "Select Department" (Selezionare il dipartimento) anziché del primo dipartimento.
* Aggiunge un messaggio di convalida quando il dipartimento non è selezionato.

La pagina Razor usa l'[helper tag di selezione](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Testare la pagina Create (Crea). La pagina Create (Crea) visualizza il nome, anziché l'ID, del dipartimento.

### <a name="update-the-courses-edit-page"></a>Aggiornare la pagina di modifica dei corsi.

Aggiornare il modello della pagina Edit (Modifica) con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

Le modifiche sono simili a quelle apportate nel modello della pagina Create (Crea). Nel codice precedente, `PopulateDepartmentsDropDownList` passa l'ID del dipartimento, che seleziona il dipartimento specificato nell'elenco a discesa.

Aggiornare *Pages/Courses/Edit.cshtml* con il markup seguente:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Il markup precedente apporta le modifiche seguenti:

* Visualizza l'ID del corso. In genere, la chiave primaria di un'entità non viene visualizzata. Le chiavi primarie di solito non sono significative per gli utenti. In questo caso, la chiave primaria corrisponde al numero del corso.
* Modifica la didascalia da **DepartmentID** a **Department**.
* Sostituisce `"ViewBag.DepartmentID"` con `DepartmentNameSL` (dalla classe di base).

La pagina contiene un campo nascosto (`<input type="hidden">`) per il numero del corso. L'aggiunta di un helper tag `<label>` con `asp-for="Course.CourseID"` non elimina la necessità del campo nascosto. `<input type="hidden">` è necessario per includere il numero del corso tra i dati inviati quando l'utente fa clic su **Save** (Salva).

Testare il codice aggiornato. Creare, modificare ed eliminare un corso.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Aggiungere AsNoTracking ai modelli delle pagine Details (Dettagli) e Delete (Elimina)

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) può migliorare le prestazioni quando la registrazione non è necessaria. Aggiungere `AsNoTracking` alle pagine Delete (Elimina) e Details (Dettagli). Il codice seguente illustra il modello della pagina Delete (Elimina) aggiornato:

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Aggiornare il metodo `OnGetAsync` nel file *Pages/Courses/Details.cshtml.cs*:

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Modificare le pagine Delete (Elimina) e Details (Dettagli)

Aggiornare la pagina Razor Delete (Elimina) con il markup seguente:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Apportare le stesse modifiche alla pagina Details (Dettagli).

### <a name="test-the-course-pages"></a>Testare le pagine del corso

Testare le pagine di creazione, modifica, dettagli ed eliminazione.

## <a name="update-the-instructor-pages"></a>Aggiornare le pagine dell'insegnante

Le sezioni seguenti aggiornano le pagine dell'insegnante.

### <a name="add-office-location"></a>Aggiungere la posizione dell'ufficio

Quando si modifica il record di un insegnante, può essere necessario aggiornare l'assegnazione dell'ufficio. L'entità `Instructor` ha una relazione uno-a-zero-o-uno con l'entità `OfficeAssignment`. Il codice relativo all'insegnante deve gestire i casi seguenti:

* Se l'utente cancella l'assegnazione dell'ufficio, eliminare l'entità `OfficeAssignment`.
* Se l'utente immette l'assegnazione dell'ufficio e questa era vuota, creare una nuova entità `OfficeAssignment`.
* Se l'utente modifica l'assegnazione dell'ufficio, aggiornare l'entità `OfficeAssignment`.

Aggiornare il modello della pagina Edit (Modifica) dell'insegnante con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

Il codice precedente:

- Ottiene l'entità `Instructor` corrente dal database tramite il caricamento eager per la proprietà di navigazione `OfficeAssignment`.
- Aggiorna l'entità `Instructor` recuperata con valori dallo strumento di associazione di modelli. `TryUpdateModel` impedisce l'[overposting](xref:data/ef-rp/crud#overposting).
- Se la posizione dell'ufficio è vuota, imposta `Instructor.OfficeAssignment` su Null. Se `Instructor.OfficeAssignment` è Null, la riga correlata nella tabella `OfficeAssignment` viene eliminata.

### <a name="update-the-instructor-edit-page"></a>Aggiornare la pagina di modifica dell'insegnante

Aggiornare *Pages/Instructors/Edit.cshtml* con la posizione dell'ufficio:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Verificare che sia possibile modificare la posizione dell'ufficio di un insegnante.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Aggiungere assegnazioni di corso nella pagina di modifica dell'insegnante

Gli insegnanti possono tenere un numero qualsiasi di corsi. In questa sezione, viene aggiunta la possibilità di modificare le assegnazioni dei corsi. La figura seguente illustra la pagina Edit (Modifica) dell'insegnante aggiornata:

![Pagina di modifica dell'insegnante con corsi](update-related-data/_static/instructor-edit-courses.png)

`Course` e `Instructor` hanno una relazione molti-a-molti. Per aggiungere e rimuovere relazioni, aggiungere e rimuovere entità dal set di entità di join `CourseAssignments`.

Per consentire la modifica dei corsi assegnati a un insegnante, si usano caselle di controllo. Per ogni corso nel database è visualizzata una casella di controllo. I corsi assegnati all'insegnante corrente sono selezionati. L'utente può selezionare e deselezionare le caselle di controllo per modificare le assegnazioni dei corsi. Se il numero di corsi è molto più elevato:

* Per visualizzare i corsi è consigliabile usare un'interfaccia utente diversa.
* Il metodo di modifica di un'entità di join per creare o eliminare relazioni non cambia.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Aggiungere classi per supportare le pagine di creazione e modifica dell'insegnante

Creare *SchoolViewModels/AssignedCourseData.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

La classe `AssignedCourseData` contiene i dati per la creazione delle caselle di controllo per i corsi assegnati a un insegnante.

Creare la classe di base *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*:

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` è la classe di base che verrà usata per i modelli delle pagine Edit (Modifica) e Create (Crea). `PopulateAssignedCourseData` legge tutte le entità `Course` per popolare `AssignedCourseDataList`. Per ogni corso, il codice imposta `CourseID` e titolo, e stabilisce se l'insegnante è assegnato al corso. Per creare ricerche efficienti, si usa un [HashSet](/dotnet/api/system.collections.generic.hashset-1).

### <a name="instructors-edit-page-model"></a>Modello della pagina di modifica dell'insegnante

Aggiornare il modello della pagina Edit (Modifica) dell'insegnante con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

Il codice precedente gestisce le modifiche alle assegnazioni di ufficio.

Aggiornare la visualizzazione Razor degli insegnanti:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Quando si incolla il codice in Visual Studio, le interruzioni di riga vengono modificate in modo tale da danneggiare il codice. Premere Ctrl + Z una volta per annullare la formattazione automatica. Premendo CTRL + Z si correggono le interruzioni di riga, che vengono visualizzate come illustrato qui. Il rientro non deve necessariamente essere perfetto, ma le righe `@</tr><tr>`, `@:<td>`, `@:</td>` e `@:</tr>` devono trovarsi in una sola riga, come illustrato. Dopo aver selezionato il blocco di nuovo codice, premere Tab tre volte per allineare il nuovo codice con il codice esistente. Esprimere un voto o vedere lo stato di questo bug [con questo collegamento](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

Il codice precedente crea una tabella HTML con tre colonne. Ogni colonna ha una casella di controllo e una didascalia che contiene il numero e il titolo del corso. Le caselle di controllo hanno tutte lo stesso nome ("selectedCourses"). L'uso dello stesso nome informa lo strumento di associazione di modelli che deve considerarle come un gruppo. L'attributo value di ogni casella di controllo è impostato su `CourseID`. Quando la pagina viene pubblicata, lo strumento di associazione di modelli passa una matrice costituita dai valori `CourseID` delle sole caselle di controllo selezionate.

Quando viene eseguito il rendering iniziale delle caselle di controllo, i corsi assegnati all'insegnante hanno attributi selezionati.

Eseguire l'app e testare la pagina Edit (Modifica) dell'insegnante aggiornata. Modificare alcune assegnazioni di corsi. Le modifiche si riflettono nella pagina di indice.

Nota: l'approccio qui adottato per la modifica dei dati dei corsi degli insegnanti funziona bene quando è presente un numero limitato di corsi. Per raccolte molto più grandi, un'interfaccia utente diversa e un altro metodo di aggiornamento sono più efficienti e pratici.

### <a name="update-the-instructors-create-page"></a>Aggiornare la pagina di creazione dell'insegnante

Aggiornare il modello della pagina Create (Crea) dell'insegnante con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

Il codice precedente è simile al codice *Pages/Instructors/Edit.cshtml.cs*.

Aggiornare la pagina Razor Create (Crea) dell'insegnante con il markup seguente:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Testare la pagina Create (Crea) dell'insegnante.

## <a name="update-the-delete-page"></a>Aggiornare la pagina Delete (Elimina)

Aggiornare il modello di pagina Delete (Elimina) con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

Il codice precedente apporta le modifiche seguenti:

* Usare il caricamento eager per la proprietà di navigazione `CourseAssignments`. È necessario includere `CourseAssignments`, in caso contrario le assegnazioni non vengono eliminate quando viene eliminato l'insegnante. Per evitare la necessità di leggerle, configurare l'eliminazione a catena nel database.

* Se l'insegnante da eliminare è assegnato come responsabile di un dipartimento, tale assegnazione viene rimossa dal dipartimento.

> [!div class="step-by-step"]
> [Precedente](xref:data/ef-rp/read-related-data)
> [Successivo](xref:data/ef-rp/concurrency)
