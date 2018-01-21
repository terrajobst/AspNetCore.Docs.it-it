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
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a>L'aggiornamento dei dati correlati - EF base Razor pagine (7 8)

Da [Tom Dykstra](https://github.com/tdykstra), e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Questa esercitazione viene illustrato l'aggiornamento dei dati correlati. Se si verificano problemi, è possibile risolvere, scaricare il [app completata per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).

Le illustrazioni seguenti vengono illustrate alcune delle pagine completate.

![Pagina di modifica del corso](update-related-data/_static/course-edit.png)
![istruttore Modifica pagina](update-related-data/_static/instructor-edit-courses.png)

Esaminare e testare le pagine di corso di creazione e modifica. Creare un nuovo corso. Il reparto è selezionato per la chiave primaria (integer), non il relativo nome. Modificare il nuovo corso. Al termine del test, eliminare il corso di nuovo.

## <a name="create-a-base-class-to-share-common-code"></a>Creare una classe base per condividere il codice

Le pagine corsi/Create e corsi/modifica è necessario un elenco di nomi di reparto. Crea il *Pages/Courses/DepartmentNamePageModel.cshtml.cs* classe di base per le pagine di creare e modificare:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

Il codice precedente crea un [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) per contenere l'elenco di nomi di reparto. Se `selectedDepartment` è specificato, tale reparto viene selezionato il `SelectList`.

Le classi modello di pagina di creazione e modifica verranno derivare da `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Personalizzare le pagine di corsi

Quando viene creata una nuova entità course, deve avere una relazione a una categoria esistente. Per aggiungere un reparto durante la creazione di un corso, la classe base per creare e modificare contiene un elenco di riepilogo a discesa per la selezione del reparto. Imposta l'elenco a discesa il `Course.DepartmentID` proprietà di chiave esterna (FK). Core EF utilizza il `Course.DepartmentID` FK per caricare il `Department` proprietà di navigazione.

![Creare corsi](update-related-data/_static/ddl.png)

Aggiornare il modello di pagina Create con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

Il codice precedente:

* Classe derivata da `DepartmentNamePageModel`.
* Usa `TryUpdateModelAsync` per impedire [overposting](xref:data/ef-rp/crud#overposting).
* Sostituisce `ViewData["DepartmentID"]` con `DepartmentNameSL` (dalla classe di base).

`ViewData["DepartmentID"]`viene sostituito con l'oggetto fortemente tipizzato `DepartmentNameSL`. Modelli fortemente tipizzati sono consigliabili su scarsamente tipizzato. Per ulteriori informazioni, vedere [scarsamente tipizzato dati (ViewData e ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Aggiornare la pagina creare corsi

Aggiornamento *Pages/Courses/Create.cshtml* con il markup seguente:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

Il markup precedente apporta le modifiche seguenti:

* Modifica l'etichetta da **DepartmentID** a **reparto**.
* Sostituisce `"ViewBag.DepartmentID"` con `DepartmentNameSL` (dalla classe di base).
* Aggiunge l'opzione "Selezionare Department". Questa modifica esegue il rendering di "Selezionare Department" anziché del primo reparto.
* Aggiunge un messaggio di convalida quando il reparto non è selezionato.

La pagina Razor utilizza il [selezionare Helper Tag](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

La pagina di creazione di test. Nella pagina di creazione viene visualizzato il nome di reparto, anziché l'ID del reparto.

### <a name="update-the-courses-edit-page"></a>Aggiornare la pagina Modifica corsi.

Aggiornare il modello di pagina di modifica con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

Le modifiche sono simili a quelle apportate nel modello di pagina di creazione. Nel codice precedente, `PopulateDepartmentsDropDownList` passa l'ID del reparto, selezionare il reparto specificato nell'elenco a discesa.

Aggiornamento *Pages/Courses/Edit.cshtml* con il markup seguente:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Il markup precedente apporta le modifiche seguenti:

* Visualizza l'ID del corso. In genere non viene visualizzata la chiave primaria (PK) di un'entità. Usata è in genere utilizzati per gli utenti. In questo caso, la chiave pubblica è il numero di corso.
* Modifica l'etichetta da **DepartmentID** a **reparto**.
* Sostituisce `"ViewBag.DepartmentID"` con `DepartmentNameSL` (dalla classe di base).
* Aggiunge l'opzione "Selezionare Department". Questa modifica esegue il rendering di "Selezionare Department" anziché del primo reparto.
* Aggiunge un messaggio di convalida quando il reparto non è selezionato.

La pagina contiene un campo nascosto (`<input type="hidden">`) per il numero di corso. Aggiunta di un `<label>` tag helper con `asp-for="Course.CourseID"` non eliminano la necessità per il campo nascosto. `<input type="hidden">`è necessario per il numero del corso da includere nei dati inviati quando l'utente fa clic **salvare**.

Testare il codice aggiornato. Creare, modificare ed eliminare un corso.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Aggiungere i dettagli di AsNoTracking ed eliminare i modelli di pagina

[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) può migliorare le prestazioni quando il rilevamento non è necessario. Aggiungere `AsNoTracking` al modello di pagina di eliminazione e i dettagli. Il codice seguente viene illustrato il modello di pagina Delete aggiornato:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Aggiornamento di `OnGetAsync` metodo il *Pages/Courses/Details.cshtml.cs* file:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Modificare le pagine di eliminazione e dettagli

Aggiornare la pagina Razor eliminare con il markup seguente:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Apportare le stesse modifiche alla pagina dei dettagli.

### <a name="test-the-course-pages"></a>Le pagine del corso di test

Test di creare, modificare, informazioni dettagliate ed eliminare.

## <a name="update-the-instructor-pages"></a>Aggiornare le pagine instructor

Nelle sezioni seguenti di aggiornare le pagine di istruttore.

### <a name="add-office-location"></a>Aggiungere il percorso di office

Quando si modifica un record istruttore, si desidera aggiornare l'assegnazione dell'ufficio dell'insegnante. Il `Instructor` entità dispone di una relazione uno-a-zero-o-uno con il `OfficeAssignment` entità. Il codice istruttore deve gestire:

* Se l'utente cancella l'assegnazione di office, eliminare il `OfficeAssignment` entità.
* Se l'utente immette l'assegnazione dell'ufficio ed era vuoto, creare un nuovo `OfficeAssignment` entità.
* Se l'utente modifica l'assegnazione di office, aggiornare il `OfficeAssignment` entità.

Aggiornare il modello di pagina di modifica i docenti con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

Il codice precedente:

- Ottiene l'oggetto corrente `Instructor` entità dal database tramite il caricamento immediato per la `OfficeAssignment` proprietà di navigazione.
- Aggiorna l'oggetto recuperato `Instructor` entità con valori dallo strumento di associazione del modello. `TryUpdateModel`impedisce [overposting](xref:data/ef-rp/crud#overposting).
- Se il percorso di office è vuoto, imposta `Instructor.OfficeAssignment` su null. Quando `Instructor.OfficeAssignment` è null, la riga correlata nella `OfficeAssignment` tabella viene eliminata.

### <a name="update-the-instructor-edit-page"></a>Aggiornare la pagina Modifica instructor

Aggiornamento *Pages/Instructors/Edit.cshtml* con il percorso di office:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Verificare che è possibile modificare la posizione di un ufficio i docenti.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Aggiungere esercitazioni per la pagina Modifica instructor

I docenti potrebbero indicare un numero qualsiasi di corsi. In questa sezione, aggiungere la possibilità di modificare le assegnazioni dei corsi. La figura seguente mostra l'istruttore aggiornato pagina Modifica:

![Pagina Modifica istruttore corsi](update-related-data/_static/instructor-edit-courses.png)

`Course`e `Instructor` ha una relazione molti-a-molti. Per aggiungere e rimuovere le relazioni, aggiungere e rimuovere le entità dal `CourseAssignments` aggiungere set di entità.

Caselle di controllo abilitare modifiche ai corsi assegnato a un docente. Una casella di controllo viene visualizzata per ogni corso nel database. Vengono controllati i corsi istruttore assegnato a. L'utente può selezionare o deselezionare le caselle di controllo per modificare le assegnazioni dei corsi. Se il numero di corsi è molto più elevato:

* Utilizzare probabilmente un'interfaccia utente differente per visualizzare i corsi.
* Il metodo di manipolazione di un'entità di join per creare o eliminare le relazioni non modificherebbe.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Aggiungere le classi per supportare creare e modificare pagine instructor

Creare *SchoolViewModels/AssignedCourseData.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

La `AssignedCourseData` classe contiene i dati per creare le caselle di controllo per i corsi assegnati da un docente.

Creare il *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* classe di base:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

Il `InstructorCoursesPageModel` è la classe base, verrà utilizzato per la modifica e creare modelli di pagina. `PopulateAssignedCourseData`legge tutti `Course` entità per popolare `AssignedCourseDataList`. Per ogni corso, imposta il codice di `CourseID`, titolo e assegnato o meno istruttore al corso. Oggetto [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) viene utilizzato per creare ricerche efficienti.

### <a name="instructors-edit-page-model"></a>Modello di pagina Modifica i docenti

Aggiornare il modello di pagina di modifica istruttore con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

Il codice precedente gestisce le modifiche di assegnazione di office.

Aggiornare la visualizzazione Razor istruttore:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Quando si incolla il codice in Visual Studio, le interruzioni di riga vengono modificate in modo che interrompe il codice. Premere Ctrl + Z di una volta per annullare la formattazione automatica. CTRL + Z corregge le interruzioni di riga in modo che vengano visualizzate come gli elementi visualizzati. Il rientro non deve necessariamente essere perfetto, ma la `@</tr><tr>`, `@:<td>`, `@:</td>`, e `@:</tr>` righe devono essere su una singola riga come illustrato. Con il blocco di nuovo codice selezionato, premere Tab tre volte per allineare il nuovo codice con il codice esistente. Votare o esaminare lo stato del bug [con questo collegamento](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

Il codice precedente crea una tabella HTML che ha tre colonne. Ogni colonna ha una casella di controllo e una didascalia che contiene il numero di corso e il titolo. Tutte le caselle di controllo con lo stesso nome ("selectedCourses"). Utilizzando lo stesso nome informa il raccoglitore di modelli per li considera come un gruppo. Il valore di attributo di ogni casella di controllo è impostato su `CourseID`. Quando viene eseguito il postback della pagina, il gestore di associazione del modello passa una matrice che include il `CourseID` i valori per solo le caselle di controllo selezionate.

Quando le caselle di controllo sono inizialmente eseguito il rendering, corsi assegnati a istruttore sono controllati gli attributi.

Eseguire l'applicazione e verificare la pagina di modifica i docenti aggiornato. Modificare alcune esercitazioni. Le modifiche vengono riflesse nella pagina di indice.

Nota: L'approccio adottato qui per modificare i dati del corso istruttore funziona anche quando è presente un numero limitato di corsi. Per le raccolte che sono più grandi, un'interfaccia utente differente e un altro metodo di aggiornamento sono più utilizzabili ed efficienti.

### <a name="update-the-instructors-create-page"></a>Aggiornare la pagina Crea i docenti

Aggiornare il modello di pagina istruttore Create con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

Il codice precedente è simile al *Pages/Instructors/Edit.cshtml.cs* codice.

Aggiornare la pagina di creare Razor istruttore con il markup seguente:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Pagina Crea insegnante di test.

## <a name="update-the-delete-page"></a>Aggiornare la pagina di eliminazione

Aggiornare il modello di pagina di Delete con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

Il codice precedente apporta le modifiche seguenti:

* Utilizza il caricamento immediato per la `CourseAssignments` proprietà di navigazione. `CourseAssignments`deve essere inclusa o non vengano eliminati quando viene eliminato l'istruttore. Per evitare la necessità di leggerli, configurare eliminazione a catena nel database.

* Se istruttore da eliminare viene assegnato come responsabile per i reparti, rimuove l'assegnazione istruttore i reparti.

>[!div class="step-by-step"]
[Precedente](xref:data/ef-rp/read-related-data)
[Successivo](xref:data/ef-rp/concurrency)
