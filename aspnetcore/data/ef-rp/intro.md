---
title: 'Razor Pages con Entity Framework Core in ASP.NET Core: esercitazione 1 di 8'
author: rick-anderson
description: Viene illustrato come creare un'app Razor Pages con Entity Framework Core
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 99a8d158c896566c2f6e6c22e4b37b1956e21cbf
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="579a1-103">Razor Pages con Entity Framework Core in ASP.NET Core: esercitazione 1 di 8</span><span class="sxs-lookup"><span data-stu-id="579a1-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="579a1-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="579a1-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="579a1-105">L'app Web di esempio di Contoso University illustra come creare applicazioni Web ASP.NET Core 2.0 MVC con Entity Framework Core 2.0 e Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="579a1-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="579a1-106">L'app di esempio è un sito Web per una fittizia Contoso University.</span><span class="sxs-lookup"><span data-stu-id="579a1-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="579a1-107">Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati.</span><span class="sxs-lookup"><span data-stu-id="579a1-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="579a1-108">Questa pagina è il prima di una serie di esercitazioni che illustrano come compilare l'app di esempio di Contoso University.</span><span class="sxs-lookup"><span data-stu-id="579a1-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="579a1-109">Scaricare o visualizzare l'app completata.</span><span class="sxs-lookup"><span data-stu-id="579a1-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="579a1-110">[Istruzioni per il download](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="579a1-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="579a1-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="579a1-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<span data-ttu-id="579a1-112">Conoscenza di [Razor Pages](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="579a1-112">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="579a1-113">Prima di iniziare questa serie, i programmatori non esperti dovranno completare l'[introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="579a1-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="579a1-114">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="579a1-114">Troubleshooting</span></span>

<span data-ttu-id="579a1-115">Se si verifica un problema che non si sa come risolvere, è generalmente possibile trovare la soluzione confrontando il codice con la [fase completata](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots).</span><span class="sxs-lookup"><span data-stu-id="579a1-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots).</span></span> <span data-ttu-id="579a1-116">Per un elenco degli errori comuni e delle relative soluzioni, vedere [la sezione relativa alla risoluzione dei problemi nell'ultima esercitazione della serie](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="579a1-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="579a1-117">Se non si trova la soluzione, è possibile inviare una domanda a [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) per [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [Entity Framework Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="579a1-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="579a1-118">Questa serie di esercitazioni si basa sulle attività eseguite nelle esercitazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="579a1-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="579a1-119">È consigliabile salvare una copia del progetto dopo aver completato ogni esercitazione.</span><span class="sxs-lookup"><span data-stu-id="579a1-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="579a1-120">Se si verificano problemi, è possibile ricominciare dall'esercitazione precedente anziché tornare all'inizio.</span><span class="sxs-lookup"><span data-stu-id="579a1-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="579a1-121">In alternativa, è possibile scaricare una [ fase completa](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) e ricominciare usando questa fase.</span><span class="sxs-lookup"><span data-stu-id="579a1-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="579a1-122">App Web di Contoso University</span><span class="sxs-lookup"><span data-stu-id="579a1-122">The Contoso University web app</span></span>

<span data-ttu-id="579a1-123">L'applicazione compilata in queste esercitazioni è il sito Web di base di un'università.</span><span class="sxs-lookup"><span data-stu-id="579a1-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="579a1-124">Gli utenti possono visualizzare e aggiornare le informazioni che riguardano studenti, corsi e insegnanti.</span><span class="sxs-lookup"><span data-stu-id="579a1-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="579a1-125">Di seguito sono disponibili alcune schermate dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="579a1-125">Here are a few of the screens created in the tutorial.</span></span>

![Pagina Student Index (Indice degli studenti)](intro/_static/students-index.png)

![Pagina Students Edit (Modifica studenti)](intro/_static/student-edit.png)

<span data-ttu-id="579a1-128">Lo stile dell'interfaccia utente del sito è simile a quanto è stato generato tramite i modelli predefiniti.</span><span class="sxs-lookup"><span data-stu-id="579a1-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="579a1-129">L'esercitazione è incentrata su Entity Framework Core con Razor Pages e non sull'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="579a1-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="579a1-130">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="579a1-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="579a1-131">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="579a1-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="579a1-132">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="579a1-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="579a1-133">Denominare il progetto **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="579a1-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="579a1-134">È importante denominare il progetto *ContosoUniversity* in modo che gli spazi dei nomi corrispondano quando il codice viene copiato/incollato.</span><span class="sxs-lookup"><span data-stu-id="579a1-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="579a1-135">![nuova applicazione Web ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="579a1-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="579a1-136">Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="579a1-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="579a1-137">![Applicazione Web (pagine Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="579a1-137">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="579a1-138">Premere **F5** per eseguire l'app in modalità di debug o **Ctrl-F5** per l'esecuzione senza collegamento del debugger</span><span class="sxs-lookup"><span data-stu-id="579a1-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="579a1-139">Impostare lo stile del sito</span><span class="sxs-lookup"><span data-stu-id="579a1-139">Set up the site style</span></span>

<span data-ttu-id="579a1-140">Con alcune modifiche è possibile impostare il menu del sito, il layout e la home page.</span><span class="sxs-lookup"><span data-stu-id="579a1-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="579a1-141">Aprire *Pages/_Layout.cshtml* e apportare le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="579a1-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="579a1-142">Modificare tutte le occorrenze di "ContosoUniversity" in "Contoso University."</span><span class="sxs-lookup"><span data-stu-id="579a1-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="579a1-143">Le occorrenze sono tre.</span><span class="sxs-lookup"><span data-stu-id="579a1-143">There are three occurrences.</span></span>

* <span data-ttu-id="579a1-144">Aggiungere le voci di menu per **Students** (Studenti), **Courses** (Corsi), **Instructors** (Insegnanti) e **Departments** (Dipartimenti) ed eliminare la voce di menu **Contact** (Contatto).</span><span class="sxs-lookup"><span data-stu-id="579a1-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="579a1-145">Le modifiche vengono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="579a1-145">The changes are highlighted.</span></span> <span data-ttu-id="579a1-146">*Non* viene visualizzato l'intero markup.</span><span class="sxs-lookup"><span data-stu-id="579a1-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="579a1-147">In *Pages/Index.cshtml* sostituire il contenuto del file con il codice seguente. In questo modo il testo su ASP.NET e MVC sarà sostituito dal testo relativo a questa app:</span><span class="sxs-lookup"><span data-stu-id="579a1-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="579a1-148">Premere CTRL+F5 per eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="579a1-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="579a1-149">La home page sarà visualizzata con le schede create nelle esercitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="579a1-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Home page di Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="579a1-151">Creare il modello di dati</span><span class="sxs-lookup"><span data-stu-id="579a1-151">Create the data model</span></span>

<span data-ttu-id="579a1-152">Creare le classi delle entità per l'app di Contoso University.</span><span class="sxs-lookup"><span data-stu-id="579a1-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="579a1-153">Iniziare con le tre entità seguenti:</span><span class="sxs-lookup"><span data-stu-id="579a1-153">Start with the following three entities:</span></span>

![Diagramma modello di dati Course/Enrollment/Student (Corso/Iscrizione/Studente)](intro/_static/data-model-diagram.png)

<span data-ttu-id="579a1-155">Esiste una relazione uno-a-molti tra le entità `Student` e `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="579a1-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="579a1-156">Esiste una relazione uno-a-molti tra le entità `Course` e `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="579a1-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="579a1-157">Uno studente può iscriversi a un numero qualsiasi di corsi.</span><span class="sxs-lookup"><span data-stu-id="579a1-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="579a1-158">Un corso può avere un numero qualsiasi di studenti iscritti.</span><span class="sxs-lookup"><span data-stu-id="579a1-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="579a1-159">Nelle sezioni seguenti viene creata una classe per ognuna di queste entità.</span><span class="sxs-lookup"><span data-stu-id="579a1-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="579a1-160">Entità Student (Studente)</span><span class="sxs-lookup"><span data-stu-id="579a1-160">The Student entity</span></span>

![Diagramma entità Student (Studente)](intro/_static/student-entity.png)

<span data-ttu-id="579a1-162">Creare una cartella *Models* (Modelli).</span><span class="sxs-lookup"><span data-stu-id="579a1-162">Create a *Models* folder.</span></span> <span data-ttu-id="579a1-163">Nella cartella *Models* (Modelli) creare un file di classe denominato *Student.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="579a1-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="579a1-164">La proprietà `ID` diventa la colonna di chiave primaria della tabella di database (DB) che corrisponde a questa classe.</span><span class="sxs-lookup"><span data-stu-id="579a1-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="579a1-165">Per impostazione predefinita, Entity Framework Core interpreta una proprietà denominata `ID` o `classnameID` come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="579a1-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="579a1-166">In `classnameID` `classname` è il nome della classe, ad esempio `Student` nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="579a1-166">In `classnameID`, `classname` is the name of the class, such as `Student` in the preceding example.</span></span>

<span data-ttu-id="579a1-167">La proprietà `Enrollments` rappresenta una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="579a1-167">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="579a1-168">Le proprietà di navigazione si collegano ad altre entità correlate a questa entità.</span><span class="sxs-lookup"><span data-stu-id="579a1-168">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="579a1-169">In questo caso la proprietà `Enrollments` di `Student entity` contiene tutte le entità `Enrollment` correlate a `Student`.</span><span class="sxs-lookup"><span data-stu-id="579a1-169">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="579a1-170">Ad esempio, se una riga Student (Studente) nella database presenta due righe Enrollment (Iscrizione) correlate, la proprietà di navigazione `Enrollments` contiene questi due entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="579a1-170">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="579a1-171">Una riga `Enrollment` correlata è una riga che contiene il valore della chiave primaria dello studente nella colonna `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="579a1-171">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="579a1-172">Si supponga ad esempio che per lo studente con ID = 1 siano presenti due righe nella tabella `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="579a1-172">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="579a1-173">La tabella `Enrollment` contiene due righe con `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="579a1-173">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="579a1-174">`StudentID` è una chiave esterna nella tabella `Enrollment` che specifica lo studente nella tabella `Student`.</span><span class="sxs-lookup"><span data-stu-id="579a1-174">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="579a1-175">Se una proprietà di navigazione può contenere più entità, deve essere un tipo elenco, ad esempio `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="579a1-175">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="579a1-176">È possibile specificare `ICollection<T>` o un tipo, ad esempio `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="579a1-176">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="579a1-177">Se si specifica `ICollection<T>`, per impostazione predefinita Entity Framework Core crea una raccolta `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="579a1-177">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="579a1-178">Le proprietà di navigazione che contengono più entità provengono da relazioni molti-a-molti e uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="579a1-178">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="579a1-179">Entità Enrollment (Iscrizione)</span><span class="sxs-lookup"><span data-stu-id="579a1-179">The Enrollment entity</span></span>

![Diagramma entità Enrollment (Iscrizione)](intro/_static/enrollment-entity.png)

<span data-ttu-id="579a1-181">Nella cartella *Models* (Modelli) creare un file *Enrollment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="579a1-181">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="579a1-182">La proprietà `EnrollmentID` è la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="579a1-182">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="579a1-183">Questa entità usa il criterio `classnameID` anziché `ID` come l'entità `Student`.</span><span class="sxs-lookup"><span data-stu-id="579a1-183">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="579a1-184">In genere gli sviluppatori scelgono un criterio che usano poi in tutto il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="579a1-184">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="579a1-185">In un'esercitazione successiva viene illustrato l'uso di ID senza classname. In questo modo si semplifica l'implementazione dell'ereditarietà nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="579a1-185">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="579a1-186">La proprietà `Grade` è un oggetto`enum`.</span><span class="sxs-lookup"><span data-stu-id="579a1-186">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="579a1-187">Il punto interrogativo dopo la dichiarazione del tipo `Grade` indica che la proprietà `Grade` ammette i valori Null.</span><span class="sxs-lookup"><span data-stu-id="579a1-187">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="579a1-188">Un voto il cui valore è Null è diverso da un voto con valore zero. Null significa che un voto è sconosciuto o non è stato ancora assegnato.</span><span class="sxs-lookup"><span data-stu-id="579a1-188">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="579a1-189">La proprietà `StudentID` è una chiave esterna e la proprietà di navigazione corrispondente è `Student`.</span><span class="sxs-lookup"><span data-stu-id="579a1-189">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="579a1-190">Un'entità `Enrollment` è associata a un'entità `Student`, pertanto la proprietà contiene un'unica entità `Student`.</span><span class="sxs-lookup"><span data-stu-id="579a1-190">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="579a1-191">L'entità `Student` si differenzia dalla proprietà di navigazione `Student.Enrollments` che contiene più entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="579a1-191">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="579a1-192">La proprietà `CourseID` è una chiave esterna e la proprietà di navigazione corrispondente è `Course`.</span><span class="sxs-lookup"><span data-stu-id="579a1-192">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="579a1-193">Un'entità `Enrollment` è associata a un'entità `Course`.</span><span class="sxs-lookup"><span data-stu-id="579a1-193">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="579a1-194">Entity Framework Core interpreta una proprietà come chiave esterna se è denominata `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="579a1-194">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="579a1-195">Ad esempio, `StudentID` per la proprietà di navigazione `Student`, poiché la chiave primaria `Student` dell'entità è `ID`.</span><span class="sxs-lookup"><span data-stu-id="579a1-195">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="579a1-196">Le proprietà di chiave esterna possono anche essere denominate `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="579a1-196">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="579a1-197">Ad esempio, `CourseID` poiché la chiave primaria `Course` dell'entità è `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="579a1-197">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="579a1-198">Entità Course (Corso)</span><span class="sxs-lookup"><span data-stu-id="579a1-198">The Course entity</span></span>

![Diagramma entità Course (Corso)](intro/_static/course-entity.png)

<span data-ttu-id="579a1-200">Nella cartella *Models* (Modelli) creare un file *Course.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="579a1-200">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="579a1-201">La proprietà `Enrollments` rappresenta una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="579a1-201">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="579a1-202">È possibile correlare un'entità `Course` a un numero qualsiasi di entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="579a1-202">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="579a1-203">L'attributo `DatabaseGenerated` consente all'app di specificare la chiave primaria anziché chiedere al database di generarla.</span><span class="sxs-lookup"><span data-stu-id="579a1-203">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="579a1-204">Creare il contesto di database SchoolContext</span><span class="sxs-lookup"><span data-stu-id="579a1-204">Create the SchoolContext DB context</span></span>

<span data-ttu-id="579a1-205">La classe principale che coordina le funzionalità di Entity Framework Core per un determinato modello di dati è la classe del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="579a1-205">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="579a1-206">Il contesto dei dati viene derivato da `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="579a1-206">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="579a1-207">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="579a1-207">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="579a1-208">In questo progetto la classe è denominata `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="579a1-208">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="579a1-209">Creare una cartella denominata *Data* (Dati) nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="579a1-209">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="579a1-210">Nella cartella *Data* (Dati) creare *SchoolContext.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="579a1-210">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="579a1-211">Questo codice crea una proprietà `DbSet` per ogni set di entità.</span><span class="sxs-lookup"><span data-stu-id="579a1-211">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="579a1-212">Nella terminologia di Entity Framework Core:</span><span class="sxs-lookup"><span data-stu-id="579a1-212">In EF Core terminology:</span></span>

* <span data-ttu-id="579a1-213">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="579a1-213">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="579a1-214">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="579a1-214">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="579a1-215">Le istruzioni `DbSet<Enrollment>` e `DbSet<Course>` possono essere omesse.</span><span class="sxs-lookup"><span data-stu-id="579a1-215">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="579a1-216">Core Entity Framework le include in modo implicito perché l'entità `Student` fa riferimento all'entità `Enrollment` e l'entità `Enrollment` fa riferimento all'entità `Course`.</span><span class="sxs-lookup"><span data-stu-id="579a1-216">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="579a1-217">Per questa esercitazione, mantenere `DbSet<Enrollment>` e `DbSet<Course>` in `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="579a1-217">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="579a1-218">Quando viene creato il database, Entity Framework Core crea tabelle i cui nomi sono gli stessi della proprietà `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="579a1-218">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="579a1-219">I nomi di proprietà per le raccolte sono generalmente usati al plurale (Students anziché Student).</span><span class="sxs-lookup"><span data-stu-id="579a1-219">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="579a1-220">Gli sviluppatori non hanno un'opinione unanime sul fatto che i nomi di tabella debbano essere usati al plurale.</span><span class="sxs-lookup"><span data-stu-id="579a1-220">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="579a1-221">Per queste esercitazioni, viene eseguito l'override del comportamento predefinito specificando nomi di tabella al singolare in DbContext.</span><span class="sxs-lookup"><span data-stu-id="579a1-221">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="579a1-222">Per specificare nomi di tabella al singolare, aggiungere il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="579a1-222">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="579a1-223">Registrare il contesto con l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="579a1-223">Register the context with dependency injection</span></span>

<span data-ttu-id="579a1-224">ASP.NET Core include l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="579a1-224">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="579a1-225">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="579a1-225">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="579a1-226">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="579a1-226">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="579a1-227">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="579a1-227">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="579a1-228">Per registrare `SchoolContext` come servizio, aprire *Startup.cs* e aggiungere le righe evidenziate al metodo `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="579a1-228">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="579a1-229">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="579a1-229">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="579a1-230">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="579a1-230">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="579a1-231">Aggiungere le istruzioni `using` per gli spazi dei nomi `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="579a1-231">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="579a1-232">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="579a1-232">Build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="579a1-233">Aprire il file *appsettings.json* e aggiungere una stringa di connessione come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="579a1-233">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="579a1-234">La stringa di connessione precedente usa `ConnectRetryCount=0` per impedire l'interruzione di [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework).</span><span class="sxs-lookup"><span data-stu-id="579a1-234">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="579a1-235">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="579a1-235">SQL Server Express LocalDB</span></span>

<span data-ttu-id="579a1-236">La stringa di connessione specifica un database Local DB di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="579a1-236">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="579a1-237">Local DB è una versione leggera del motore di database di SQL Server Express appositamente pensato per lo sviluppo di app e non per la produzione.</span><span class="sxs-lookup"><span data-stu-id="579a1-237">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="579a1-238">Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa.</span><span class="sxs-lookup"><span data-stu-id="579a1-238">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="579a1-239">Per impostazione predefinita, Local DB crea file di database con estensione *mdf* nella directory `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="579a1-239">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="579a1-240">Aggiungere il codice per inizializzare il database con dati di test</span><span class="sxs-lookup"><span data-stu-id="579a1-240">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="579a1-241">Entity Framework Core crea un database vuoto.</span><span class="sxs-lookup"><span data-stu-id="579a1-241">EF Core creates an empty DB.</span></span> <span data-ttu-id="579a1-242">In questa sezione viene scritto un metodo di *inizializzazione* per popolare il database con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="579a1-242">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="579a1-243">Nella cartella *Data* (Dati) creare un nuovo file di classe denominato *DbInitializer.cs* e aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="579a1-243">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="579a1-244">Il codice controlla se esistono studenti nel database.</span><span class="sxs-lookup"><span data-stu-id="579a1-244">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="579a1-245">Se il database non contiene studenti, il database viene inizializzato con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="579a1-245">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="579a1-246">I dati di test vengono caricati in matrici anziché in raccolte `List<T>` per ottimizzare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="579a1-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="579a1-247">Il metodo `EnsureCreated` crea automaticamente il database per il contesto di database.</span><span class="sxs-lookup"><span data-stu-id="579a1-247">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="579a1-248">Se il database esiste, `EnsureCreated` restituisce senza modificare il database.</span><span class="sxs-lookup"><span data-stu-id="579a1-248">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="579a1-249">In *Program.cs* modificare il metodo `Main` per eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="579a1-249">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="579a1-250">Ottenere un'istanza del contesto di database dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="579a1-250">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="579a1-251">Chiamare il metodo di inizializzazione, passandolo al contesto.</span><span class="sxs-lookup"><span data-stu-id="579a1-251">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="579a1-252">Eliminare il contesto dopo che il metodo di inizializzazione è stato completato.</span><span class="sxs-lookup"><span data-stu-id="579a1-252">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="579a1-253">Il codice seguente illustra il file *Program.cs* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="579a1-253">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="579a1-254">La prima volta che si esegue l'app, il database viene creato e inizializzato con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="579a1-254">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="579a1-255">Quando il modello di dati è aggiornato, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="579a1-255">When the data model is updated:</span></span>
* <span data-ttu-id="579a1-256">Eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="579a1-256">Delete the DB.</span></span>
* <span data-ttu-id="579a1-257">Aggiornare il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="579a1-257">Update the seed method.</span></span>
* <span data-ttu-id="579a1-258">Eseguire l'app. Viene creato un nuovo database inizializzato.</span><span class="sxs-lookup"><span data-stu-id="579a1-258">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="579a1-259">Nelle esercitazioni successive il database viene aggiornato alla modifica del modello di dati, senza eliminare e ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="579a1-259">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="579a1-260">Aggiungere gli strumenti di scaffolding</span><span class="sxs-lookup"><span data-stu-id="579a1-260">Add scaffold tooling</span></span>

<span data-ttu-id="579a1-261">In questa sezione viene usata la console di Gestione pacchetti per aggiungere il pacchetto di generazione del codice Web di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="579a1-261">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="579a1-262">Questo pacchetto è necessario per eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="579a1-262">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="579a1-263">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="579a1-263">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="579a1-264">Nella console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="579a1-264">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="579a1-265">Con il comando precedente i pacchetti NuGet vengono aggiunti al file \* con estensione csproj:</span><span class="sxs-lookup"><span data-stu-id="579a1-265">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-csharp[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="579a1-266">Eseguire lo scaffolding del modello</span><span class="sxs-lookup"><span data-stu-id="579a1-266">Scaffold the model</span></span>

* <span data-ttu-id="579a1-267">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="579a1-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="579a1-268">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="579a1-268">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

<span data-ttu-id="579a1-269">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="579a1-269">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="579a1-270">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="579a1-270">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="579a1-271">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="579a1-271">Build the project.</span></span> <span data-ttu-id="579a1-272">La compilazione genera errori simili al seguente:</span><span class="sxs-lookup"><span data-stu-id="579a1-272">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="579a1-273">Convertire a livello globale `_context.Student` in `_context.Students` (ovvero aggiungere una "s" a `Student`).</span><span class="sxs-lookup"><span data-stu-id="579a1-273">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="579a1-274">Vengono trovate e aggiornate sette occorrenze.</span><span class="sxs-lookup"><span data-stu-id="579a1-274">7 occurrences are found and updated.</span></span> <span data-ttu-id="579a1-275">Si spera di risolvere [questo bug](https://github.com/aspnet/Scaffolding/issues/633) nella prossima versione.</span><span class="sxs-lookup"><span data-stu-id="579a1-275">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE [model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="579a1-276">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="579a1-276">Test the app</span></span>

<span data-ttu-id="579a1-277">Eseguire l'app e selezionare il collegamento **Students** (Studenti).</span><span class="sxs-lookup"><span data-stu-id="579a1-277">Run the app and select the **Students** link.</span></span> <span data-ttu-id="579a1-278">A seconda delle dimensione del browser, il collegamento **Students** (Studenti) viene visualizzato in alto alla pagina.</span><span class="sxs-lookup"><span data-stu-id="579a1-278">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="579a1-279">Se il collegamento **Students** (Studenti) non è visibile, fare clic sull'icona di spostamento in alto a destra.</span><span class="sxs-lookup"><span data-stu-id="579a1-279">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Home page di Contoso University ristretta](intro/_static/home-page-narrow.png)

<span data-ttu-id="579a1-281">Eseguire il test dei collegamenti **Create** (Crea), **Edit** (Modifica) e **Details** (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="579a1-281">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="579a1-282">Visualizzare il database</span><span class="sxs-lookup"><span data-stu-id="579a1-282">View the DB</span></span>

<span data-ttu-id="579a1-283">Dopo aver avviato l'app, `DbInitializer.Initialize` chiama `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="579a1-283">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="579a1-284">`EnsureCreated` controlla l'esistenza del database e se necessario ne crea uno.</span><span class="sxs-lookup"><span data-stu-id="579a1-284">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="579a1-285">Se il database non contiene studenti, il metodo `Initialize` aggiunge gli studenti.</span><span class="sxs-lookup"><span data-stu-id="579a1-285">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="579a1-286">Aprire **Esplora oggetti di SQL Server** dal menu **Visualizza** in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="579a1-286">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="579a1-287">In Esplora oggetti di SQL Server fare clic su **(localdb)\MSSQLLocalDB > Database > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="579a1-287">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="579a1-288">Espandere il nodo **Tabelle**.</span><span class="sxs-lookup"><span data-stu-id="579a1-288">Expand the **Tables** node.</span></span>

<span data-ttu-id="579a1-289">Fare clic con il pulsante destro del mouse sulla tabella **Student** (Studente) e fare clic su **Visualizza dati** per visualizzare le colonne create e le righe inserite nella tabella.</span><span class="sxs-lookup"><span data-stu-id="579a1-289">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="579a1-290">I file di database con estensione <em>mdf</em> e <em>ldf</em> sono contenuti nella cartella <em>C:\Utenti\\<yourusername></em>.</span><span class="sxs-lookup"><span data-stu-id="579a1-290">The <em>.mdf</em> and <em>.ldf</em> DB files are in the <em>C:\Users\\<yourusername></em> folder.</span></span>

<span data-ttu-id="579a1-291">`EnsureCreated` viene chiamato all'avvio dell'app e consente il flusso di lavoro seguente:</span><span class="sxs-lookup"><span data-stu-id="579a1-291">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="579a1-292">Eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="579a1-292">Delete the DB.</span></span>
* <span data-ttu-id="579a1-293">Modificare lo schema di database, ad esempio, aggiungere un campo `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="579a1-293">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="579a1-294">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="579a1-294">Run the app.</span></span>

<span data-ttu-id="579a1-295">`EnsureCreated` crea un database con la colonna `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="579a1-295">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="579a1-296">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="579a1-296">Conventions</span></span>

<span data-ttu-id="579a1-297">Grazie all'uso di convenzioni o di ipotesi di Entity Framework Core, la quantità di codice scritto che è necessario a Entity Framework Core per creare un database completo è minino.</span><span class="sxs-lookup"><span data-stu-id="579a1-297">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="579a1-298">I nomi delle proprietà `DbSet` vengono usate come nomi di tabella.</span><span class="sxs-lookup"><span data-stu-id="579a1-298">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="579a1-299">Per le entità a cui non fa riferimento una proprietà `DbSet`, i nomi della classe di entità vengono usati come nome di tabella.</span><span class="sxs-lookup"><span data-stu-id="579a1-299">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="579a1-300">I nomi della proprietà di entità vengono usati come nomi di colonna.</span><span class="sxs-lookup"><span data-stu-id="579a1-300">Entity property names are used for column names.</span></span>

* <span data-ttu-id="579a1-301">Le proprietà dell'entità vengono denominate ID o classnameID e vengono riconosciute come proprietà della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="579a1-301">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="579a1-302">Una proprietà viene interpretata come proprietà di una chiave esterna se è denominata *<navigation property name><primary key property name>*, ad esempio `StudentID` per la proprietà di navigazione `Student` poiché la chiave primaria dell'entità `Student` è `ID`.</span><span class="sxs-lookup"><span data-stu-id="579a1-302">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="579a1-303">Le proprietà di una chiave esterna possono essere denominate *<primary key property name>*, ad esempio `EnrollmentID` poiché la chiave primaria dell'entità `Enrollment` è `EnrollmentID`.</span><span class="sxs-lookup"><span data-stu-id="579a1-303">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="579a1-304">È possibile eseguire l'override del comportamento convenzionale.</span><span class="sxs-lookup"><span data-stu-id="579a1-304">Conventional behavior can be overridden.</span></span> <span data-ttu-id="579a1-305">Ad esempio, i nomi di tabella possono essere specificati in modo esplicito, come illustrato in precedenza in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="579a1-305">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="579a1-306">I nomi delle colonne possono essere impostati in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="579a1-306">The column names can be explicitly set.</span></span> <span data-ttu-id="579a1-307">Le chiavi primarie e le chiavi esterne possono essere impostate in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="579a1-307">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="579a1-308">Codice asincrono</span><span class="sxs-lookup"><span data-stu-id="579a1-308">Asynchronous code</span></span>

<span data-ttu-id="579a1-309">La programmazione asincrona è la modalità predefinita per ASP.NET Core ed Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="579a1-309">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="579a1-310">Per un server Web è disponibile un numero limitato di thread e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso.</span><span class="sxs-lookup"><span data-stu-id="579a1-310">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="579a1-311">In queste circostanze il server non può elaborare nuove richieste finché i thread non saranno liberi.</span><span class="sxs-lookup"><span data-stu-id="579a1-311">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="579a1-312">Con il codice sincrono, può succedere che molti thread siano vincolati nonostante in quel momento non stiano eseguendo alcuna operazione. Rimangono tuttavia in attesa che l'operazione I/O sia completata.</span><span class="sxs-lookup"><span data-stu-id="579a1-312">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="579a1-313">Con il codice asincrono, se un processo è in attesa del completamento dell'operazione I/O, il thread viene liberato e il server lo può usare per l'elaborazione di altre richieste.</span><span class="sxs-lookup"><span data-stu-id="579a1-313">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="579a1-314">Di conseguenza, il codice asincrono consente un uso più efficiente delle risorse e il server è abilitato per gestire una maggiore quantità di traffico senza ritardi.</span><span class="sxs-lookup"><span data-stu-id="579a1-314">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="579a1-315">Il codice asincrono comporta un minimo sovraccarico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="579a1-315">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="579a1-316">In caso di traffico ridotto, il calo delle prestazioni è irrilevante, mentre nelle situazioni di traffico elevato, è essenziale ottimizzare le prestazioni potenziali.</span><span class="sxs-lookup"><span data-stu-id="579a1-316">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="579a1-317">Nel codice seguente la parola chiave `async`, il valore restituito `Task<T>`, la parola chiave `await` e il metodo `ToListAsync` consentono di eseguire il codice in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="579a1-317">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="579a1-318">La parola chiave `async` indica al compilatore di eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="579a1-318">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="579a1-319">Generare callback per parti del corpo del metodo.</span><span class="sxs-lookup"><span data-stu-id="579a1-319">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="579a1-320">Creare automaticamente l' oggetto [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) restituito.</span><span class="sxs-lookup"><span data-stu-id="579a1-320">Automatically create the [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="579a1-321">Per altre informazioni, vedere [Tipo restituito Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="579a1-321">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="579a1-322">Il tipo restituito implicito `Task` rappresenta il lavoro in corso.</span><span class="sxs-lookup"><span data-stu-id="579a1-322">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="579a1-323">La parola chiave `await` indica al compilatore di suddividere il metodo in due parti.</span><span class="sxs-lookup"><span data-stu-id="579a1-323">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="579a1-324">La prima parte termina con l'operazione avviata in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="579a1-324">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="579a1-325">La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="579a1-325">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="579a1-326">`ToListAsync` è la versione asincrona del metodo di estensione `ToList`.</span><span class="sxs-lookup"><span data-stu-id="579a1-326">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="579a1-327">Alcuni aspetti da considerare quando si scrive codice asincrono usato da Entity Framework Core:</span><span class="sxs-lookup"><span data-stu-id="579a1-327">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="579a1-328">Solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="579a1-328">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="579a1-329">Sono incluse `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` e `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="579a1-329">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="579a1-330">Non sono incluse le istruzioni che modificano solo un oggetto `IQueryable`, ad esempio `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="579a1-330">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="579a1-331">Un contesto di Entity Framework Core non è thread-safe. Non tentare di eseguire più operazioni parallelamente.</span><span class="sxs-lookup"><span data-stu-id="579a1-331">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="579a1-332">Per sfruttare i vantaggi del codice asincrono in termini di prestazioni, verificare che i pacchetti della libreria impiegati, ad esempio per il paging, usino la modalità asincrona per chiamare i metodi di Entity Framework Core che generano query da inviare al database.</span><span class="sxs-lookup"><span data-stu-id="579a1-332">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="579a1-333">Per altre informazioni sulla programmazione asincrona in .NET, vedere [Panoramica della programmazione asincrona](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="579a1-333">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="579a1-334">Nella prossima esercitazione, vengono esaminate le operazioni CRUD per creare, leggere, aggiornare, eliminare ed elencare.</span><span class="sxs-lookup"><span data-stu-id="579a1-334">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="579a1-335">avanti</span><span class="sxs-lookup"><span data-stu-id="579a1-335">Next</span></span>](xref:data/ef-rp/crud)
