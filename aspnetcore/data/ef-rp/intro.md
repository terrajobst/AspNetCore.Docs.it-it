---
title: 'Razor Pages con Entity Framework Core: esercitazione 1 di 8'
author: rick-anderson
description: Viene illustrato come creare un'app Razor Pages con Entity Framework Core
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 091f34da347d52ba8e3e87779ddc4aeb790c2800
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="7b51b-103">Introduzione a Razor Pages ed Entity Framework Core con Visual Studio (1 di 8)</span><span class="sxs-lookup"><span data-stu-id="7b51b-103">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="7b51b-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7b51b-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7b51b-105">L'app Web di esempio di Contoso University illustra come creare applicazioni Web ASP.NET Core 2.0 MVC con Entity Framework Core 2.0 e Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="7b51b-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="7b51b-106">L'app di esempio è un sito Web per una fittizia Contoso University.</span><span class="sxs-lookup"><span data-stu-id="7b51b-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="7b51b-107">Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati.</span><span class="sxs-lookup"><span data-stu-id="7b51b-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="7b51b-108">Questa pagina è il prima di una serie di esercitazioni che illustrano come compilare l'app di esempio di Contoso University.</span><span class="sxs-lookup"><span data-stu-id="7b51b-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="7b51b-109">Scaricare o visualizzare l'app completata.</span><span class="sxs-lookup"><span data-stu-id="7b51b-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="7b51b-110">[Istruzioni per il download](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="7b51b-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b51b-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="7b51b-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

<span data-ttu-id="7b51b-112">Conoscenza di [Razor Pages](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="7b51b-112">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="7b51b-113">Prima di iniziare questa serie, i programmatori non esperti dovranno completare l'[introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="7b51b-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7b51b-114">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="7b51b-114">Troubleshooting</span></span>

<span data-ttu-id="7b51b-115">Se si verifica un problema che non si sa come risolvere, è generalmente possibile trovare la soluzione confrontando il codice con la [fase completa](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) o il [progetto completato](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="7b51b-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="7b51b-116">Per un elenco degli errori comuni e delle relative soluzioni, vedere [la sezione relativa alla risoluzione dei problemi nell'ultima esercitazione della serie](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="7b51b-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="7b51b-117">Se non si trova la soluzione, è possibile inviare una domanda a [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) per [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [Entity Framework Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="7b51b-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="7b51b-118">Questa serie di esercitazioni si basa sulle attività eseguite nelle esercitazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="7b51b-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="7b51b-119">È consigliabile salvare una copia del progetto dopo aver completato ogni esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7b51b-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="7b51b-120">Se si verificano problemi, è possibile ricominciare dall'esercitazione precedente anziché tornare all'inizio.</span><span class="sxs-lookup"><span data-stu-id="7b51b-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="7b51b-121">In alternativa, è possibile scaricare una [ fase completa](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) e ricominciare usando questa fase.</span><span class="sxs-lookup"><span data-stu-id="7b51b-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="7b51b-122">App Web di Contoso University</span><span class="sxs-lookup"><span data-stu-id="7b51b-122">The Contoso University web app</span></span>

<span data-ttu-id="7b51b-123">L'applicazione compilata in queste esercitazioni è il sito Web di base di un'università.</span><span class="sxs-lookup"><span data-stu-id="7b51b-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="7b51b-124">Gli utenti possono visualizzare e aggiornare le informazioni che riguardano studenti, corsi e insegnanti.</span><span class="sxs-lookup"><span data-stu-id="7b51b-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="7b51b-125">Di seguito sono disponibili alcune schermate dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7b51b-125">Here are a few of the screens created in the tutorial.</span></span>

![Pagina Student Index (Indice degli studenti)](intro/_static/students-index.png)

![Pagina Students Edit (Modifica studenti)](intro/_static/student-edit.png)

<span data-ttu-id="7b51b-128">Lo stile dell'interfaccia utente del sito è simile a quanto è stato generato tramite i modelli predefiniti.</span><span class="sxs-lookup"><span data-stu-id="7b51b-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="7b51b-129">L'esercitazione è incentrata su Entity Framework Core con Razor Pages e non sull'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="7b51b-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="7b51b-130">Creare un'app Web di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7b51b-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="7b51b-131">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="7b51b-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7b51b-132">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b51b-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="7b51b-133">Denominare il progetto **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="7b51b-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="7b51b-134">È importante denominare il progetto *ContosoUniversity* in modo che gli spazi dei nomi corrispondano quando il codice viene copiato/incollato.</span><span class="sxs-lookup"><span data-stu-id="7b51b-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="7b51b-135">![nuova applicazione Web ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="7b51b-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="7b51b-136">Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="7b51b-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="7b51b-137">![Applicazione Web (pagine Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="7b51b-137">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="7b51b-138">Premere **F5** per eseguire l'app in modalità di debug o **Ctrl-F5** per l'esecuzione senza collegamento del debugger</span><span class="sxs-lookup"><span data-stu-id="7b51b-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="7b51b-139">Impostare lo stile del sito</span><span class="sxs-lookup"><span data-stu-id="7b51b-139">Set up the site style</span></span>

<span data-ttu-id="7b51b-140">Con alcune modifiche è possibile impostare il menu del sito, il layout e la home page.</span><span class="sxs-lookup"><span data-stu-id="7b51b-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="7b51b-141">Aprire *Pages/_Layout.cshtml* e apportare le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b51b-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="7b51b-142">Modificare tutte le occorrenze di "ContosoUniversity" in "Contoso University."</span><span class="sxs-lookup"><span data-stu-id="7b51b-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="7b51b-143">Le occorrenze sono tre.</span><span class="sxs-lookup"><span data-stu-id="7b51b-143">There are three occurrences.</span></span>

* <span data-ttu-id="7b51b-144">Aggiungere le voci di menu per **Students** (Studenti), **Courses** (Corsi), **Instructors** (Insegnanti) e **Departments** (Dipartimenti) ed eliminare la voce di menu **Contact** (Contatto).</span><span class="sxs-lookup"><span data-stu-id="7b51b-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="7b51b-145">Le modifiche vengono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="7b51b-145">The changes are highlighted.</span></span> <span data-ttu-id="7b51b-146">*Non* viene visualizzato l'intero markup.</span><span class="sxs-lookup"><span data-stu-id="7b51b-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="7b51b-147">In *Pages/Index.cshtml* sostituire il contenuto del file con il codice seguente. In questo modo il testo su ASP.NET e MVC sarà sostituito dal testo relativo a questa app:</span><span class="sxs-lookup"><span data-stu-id="7b51b-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="7b51b-148">Premere CTRL+F5 per eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="7b51b-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="7b51b-149">La home page sarà visualizzata con le schede create nelle esercitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b51b-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Home page di Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="7b51b-151">Creare il modello di dati</span><span class="sxs-lookup"><span data-stu-id="7b51b-151">Create the data model</span></span>

<span data-ttu-id="7b51b-152">Creare le classi delle entità per l'app di Contoso University.</span><span class="sxs-lookup"><span data-stu-id="7b51b-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="7b51b-153">Iniziare con le tre entità seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b51b-153">Start with the following three entities:</span></span>

![Diagramma modello di dati Course/Enrollment/Student (Corso/Iscrizione/Studente)](intro/_static/data-model-diagram.png)

<span data-ttu-id="7b51b-155">Esiste una relazione uno-a-molti tra le entità `Student` e `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="7b51b-156">Esiste una relazione uno-a-molti tra le entità `Course` e `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="7b51b-157">Uno studente può iscriversi a un numero qualsiasi di corsi.</span><span class="sxs-lookup"><span data-stu-id="7b51b-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="7b51b-158">Un corso può avere un numero qualsiasi di studenti iscritti.</span><span class="sxs-lookup"><span data-stu-id="7b51b-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="7b51b-159">Nelle sezioni seguenti viene creata una classe per ognuna di queste entità.</span><span class="sxs-lookup"><span data-stu-id="7b51b-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="7b51b-160">Entità Student (Studente)</span><span class="sxs-lookup"><span data-stu-id="7b51b-160">The Student entity</span></span>

![Diagramma entità Student (Studente)](intro/_static/student-entity.png)

<span data-ttu-id="7b51b-162">Creare una cartella *Models* (Modelli).</span><span class="sxs-lookup"><span data-stu-id="7b51b-162">Create a *Models* folder.</span></span> <span data-ttu-id="7b51b-163">Nella cartella *Models* (Modelli) creare un file di classe denominato *Student.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7b51b-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="7b51b-164">La proprietà `ID` diventa la colonna di chiave primaria della tabella di database (DB) che corrisponde a questa classe.</span><span class="sxs-lookup"><span data-stu-id="7b51b-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="7b51b-165">Per impostazione predefinita, Entity Framework Core interpreta una proprietà denominata `ID` o `classnameID` come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="7b51b-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="7b51b-166">La proprietà `Enrollments` rappresenta una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="7b51b-166">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="7b51b-167">Le proprietà di navigazione si collegano ad altre entità correlate a questa entità.</span><span class="sxs-lookup"><span data-stu-id="7b51b-167">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="7b51b-168">In questo caso la proprietà `Enrollments` di `Student entity` contiene tutte le entità `Enrollment` correlate a `Student`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-168">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="7b51b-169">Ad esempio, se una riga Student (Studente) nella database presenta due righe Enrollment (Iscrizione) correlate, la proprietà di navigazione `Enrollments` contiene questi due entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-169">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="7b51b-170">Una riga `Enrollment` correlata è una riga che contiene il valore della chiave primaria dello studente nella colonna `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-170">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="7b51b-171">Si supponga ad esempio che per lo studente con ID = 1 siano presenti due righe nella tabella `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-171">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="7b51b-172">La tabella `Enrollment` contiene due righe con `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="7b51b-172">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="7b51b-173">`StudentID` è una chiave esterna nella tabella `Enrollment` che specifica lo studente nella tabella `Student`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-173">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="7b51b-174">Se una proprietà di navigazione può contenere più entità, deve essere un tipo elenco, ad esempio `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-174">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="7b51b-175">È possibile specificare `ICollection<T>` o un tipo, ad esempio `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-175">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="7b51b-176">Se si specifica `ICollection<T>`, per impostazione predefinita Entity Framework Core crea una raccolta `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-176">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="7b51b-177">Le proprietà di navigazione che contengono più entità provengono da relazioni molti-a-molti e uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="7b51b-177">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="7b51b-178">Entità Enrollment (Iscrizione)</span><span class="sxs-lookup"><span data-stu-id="7b51b-178">The Enrollment entity</span></span>

![Diagramma entità Enrollment (Iscrizione)](intro/_static/enrollment-entity.png)

<span data-ttu-id="7b51b-180">Nella cartella *Models* (Modelli) creare un file *Enrollment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7b51b-180">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="7b51b-181">La proprietà `EnrollmentID` è la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="7b51b-181">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="7b51b-182">Questa entità usa il criterio `classnameID` anziché `ID` come l'entità `Student`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-182">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="7b51b-183">In genere gli sviluppatori scelgono un criterio che usano poi in tutto il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="7b51b-183">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="7b51b-184">In un'esercitazione successiva viene illustrato l'uso di ID senza classname. In questo modo si semplifica l'implementazione dell'ereditarietà nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="7b51b-184">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="7b51b-185">La proprietà `Grade` è un oggetto`enum`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-185">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="7b51b-186">Il punto interrogativo dopo la dichiarazione del tipo `Grade` indica che la proprietà `Grade` ammette i valori Null.</span><span class="sxs-lookup"><span data-stu-id="7b51b-186">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="7b51b-187">Un voto il cui valore è Null è diverso da un voto con valore zero. Null significa che un voto è sconosciuto o non è stato ancora assegnato.</span><span class="sxs-lookup"><span data-stu-id="7b51b-187">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="7b51b-188">La proprietà `StudentID` è una chiave esterna e la proprietà di navigazione corrispondente è `Student`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-188">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="7b51b-189">Un'entità `Enrollment` è associata a un'entità `Student`, pertanto la proprietà contiene un'unica entità `Student`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-189">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="7b51b-190">L'entità `Student` si differenzia dalla proprietà di navigazione `Student.Enrollments` che contiene più entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-190">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="7b51b-191">La proprietà `CourseID` è una chiave esterna e la proprietà di navigazione corrispondente è `Course`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-191">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="7b51b-192">Un'entità `Enrollment` è associata a un'entità `Course`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-192">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="7b51b-193">Entity Framework Core interpreta una proprietà come chiave esterna se è denominata `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-193">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="7b51b-194">Ad esempio, `StudentID` per la proprietà di navigazione `Student`, poiché la chiave primaria `Student` dell'entità è `ID`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-194">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="7b51b-195">Le proprietà di chiave esterna possono anche essere denominate `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-195">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="7b51b-196">Ad esempio, `CourseID` poiché la chiave primaria `Course` dell'entità è `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-196">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="7b51b-197">Entità Course (Corso)</span><span class="sxs-lookup"><span data-stu-id="7b51b-197">The Course entity</span></span>

![Diagramma entità Course (Corso)](intro/_static/course-entity.png)

<span data-ttu-id="7b51b-199">Nella cartella *Models* (Modelli) creare un file *Course.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7b51b-199">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="7b51b-200">La proprietà `Enrollments` rappresenta una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="7b51b-200">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="7b51b-201">È possibile correlare un'entità `Course` a un numero qualsiasi di entità `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-201">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="7b51b-202">L'attributo `DatabaseGenerated` consente all'app di specificare la chiave primaria anziché chiedere al database di generarla.</span><span class="sxs-lookup"><span data-stu-id="7b51b-202">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="7b51b-203">Creare il contesto di database SchoolContext</span><span class="sxs-lookup"><span data-stu-id="7b51b-203">Create the SchoolContext DB context</span></span>

<span data-ttu-id="7b51b-204">La classe principale che coordina le funzionalità di Entity Framework Core per un determinato modello di dati è la classe del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="7b51b-204">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="7b51b-205">Il contesto dei dati viene derivato da `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-205">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="7b51b-206">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="7b51b-206">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="7b51b-207">In questo progetto la classe è denominata `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-207">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="7b51b-208">Creare una cartella denominata *Data* (Dati) nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="7b51b-208">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="7b51b-209">Nella cartella *Data* (Dati) creare *SchoolContext.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7b51b-209">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="7b51b-210">Questo codice crea una proprietà `DbSet` per ogni set di entità.</span><span class="sxs-lookup"><span data-stu-id="7b51b-210">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="7b51b-211">Nella terminologia di Entity Framework Core:</span><span class="sxs-lookup"><span data-stu-id="7b51b-211">In EF Core terminology:</span></span>

* <span data-ttu-id="7b51b-212">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="7b51b-212">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="7b51b-213">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="7b51b-213">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7b51b-214">Le istruzioni `DbSet<Enrollment>` e `DbSet<Course>` possono essere omesse.</span><span class="sxs-lookup"><span data-stu-id="7b51b-214">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="7b51b-215">Core Entity Framework le include in modo implicito perché l'entità `Student` fa riferimento all'entità `Enrollment` e l'entità `Enrollment` fa riferimento all'entità `Course`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-215">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="7b51b-216">Per questa esercitazione, mantenere `DbSet<Enrollment>` e `DbSet<Course>` in `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-216">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="7b51b-217">Quando viene creato il database, Entity Framework Core crea tabelle i cui nomi sono gli stessi della proprietà `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-217">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="7b51b-218">I nomi di proprietà per le raccolte sono generalmente usati al plurale (Students anziché Student).</span><span class="sxs-lookup"><span data-stu-id="7b51b-218">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="7b51b-219">Gli sviluppatori non hanno un'opinione unanime sul fatto che i nomi di tabella debbano essere usati al plurale.</span><span class="sxs-lookup"><span data-stu-id="7b51b-219">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="7b51b-220">Per queste esercitazioni, viene eseguito l'override del comportamento predefinito specificando nomi di tabella al singolare in DbContext.</span><span class="sxs-lookup"><span data-stu-id="7b51b-220">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="7b51b-221">Per specificare nomi di tabella al singolare, aggiungere il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="7b51b-221">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="7b51b-222">Registrare il contesto con l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="7b51b-222">Register the context with dependency injection</span></span>

<span data-ttu-id="7b51b-223">ASP.NET Core include l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7b51b-223">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7b51b-224">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7b51b-224">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7b51b-225">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="7b51b-225">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="7b51b-226">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="7b51b-226">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="7b51b-227">Per registrare `SchoolContext` come servizio, aprire *Startup.cs* e aggiungere le righe evidenziate al metodo `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-227">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="7b51b-228">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-228">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="7b51b-229">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7b51b-229">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="7b51b-230">Aggiungere le istruzioni `using` per gli spazi dei nomi `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-230">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="7b51b-231">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="7b51b-231">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="7b51b-232">Aprire il file *appsettings.json* e aggiungere una stringa di connessione come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7b51b-232">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="7b51b-233">La stringa di connessione precedente usa `ConnectRetryCount=0` per impedire l'interruzione di [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework).</span><span class="sxs-lookup"><span data-stu-id="7b51b-233">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="7b51b-234">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="7b51b-234">SQL Server Express LocalDB</span></span>

<span data-ttu-id="7b51b-235">La stringa di connessione specifica un database Local DB di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7b51b-235">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="7b51b-236">Local DB è una versione leggera del motore di database di SQL Server Express appositamente pensato per lo sviluppo di app e non per la produzione.</span><span class="sxs-lookup"><span data-stu-id="7b51b-236">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="7b51b-237">Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa.</span><span class="sxs-lookup"><span data-stu-id="7b51b-237">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="7b51b-238">Per impostazione predefinita, Local DB crea file di database con estensione *mdf* nella directory `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-238">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="7b51b-239">Aggiungere il codice per inizializzare il database con dati di test</span><span class="sxs-lookup"><span data-stu-id="7b51b-239">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="7b51b-240">Entity Framework Core crea un database vuoto.</span><span class="sxs-lookup"><span data-stu-id="7b51b-240">EF Core creates an empty DB.</span></span> <span data-ttu-id="7b51b-241">In questa sezione viene scritto un metodo di *inizializzazione* per popolare il database con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="7b51b-241">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="7b51b-242">Nella cartella *Data* (Dati) creare un nuovo file di classe denominato *DbInitializer.cs* e aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7b51b-242">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="7b51b-243">Il codice controlla se esistono studenti nel database.</span><span class="sxs-lookup"><span data-stu-id="7b51b-243">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="7b51b-244">Se il database non contiene studenti, il database viene inizializzato con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="7b51b-244">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="7b51b-245">I dati di test vengono caricati in matrici anziché in raccolte `List<T>` per ottimizzare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="7b51b-245">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="7b51b-246">Il metodo `EnsureCreated` crea automaticamente il database per il contesto di database.</span><span class="sxs-lookup"><span data-stu-id="7b51b-246">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="7b51b-247">Se il database esiste, `EnsureCreated` restituisce senza modificare il database.</span><span class="sxs-lookup"><span data-stu-id="7b51b-247">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="7b51b-248">In *Program.cs* modificare il metodo `Main` per eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b51b-248">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="7b51b-249">Ottenere un'istanza del contesto di database dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7b51b-249">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="7b51b-250">Chiamare il metodo di inizializzazione, passandolo al contesto.</span><span class="sxs-lookup"><span data-stu-id="7b51b-250">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="7b51b-251">Eliminare il contesto dopo che il metodo di inizializzazione è stato completato.</span><span class="sxs-lookup"><span data-stu-id="7b51b-251">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="7b51b-252">Il codice seguente illustra il file *Program.cs* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="7b51b-252">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="7b51b-253">La prima volta che si esegue l'app, il database viene creato e inizializzato con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="7b51b-253">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="7b51b-254">Quando il modello di dati è aggiornato, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b51b-254">When the data model is updated:</span></span>
* <span data-ttu-id="7b51b-255">Eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="7b51b-255">Delete the DB.</span></span>
* <span data-ttu-id="7b51b-256">Aggiornare il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="7b51b-256">Update the seed method.</span></span>
* <span data-ttu-id="7b51b-257">Eseguire l'app. Viene creato un nuovo database inizializzato.</span><span class="sxs-lookup"><span data-stu-id="7b51b-257">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="7b51b-258">Nelle esercitazioni successive il database viene aggiornato alla modifica del modello di dati, senza eliminare e ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="7b51b-258">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="7b51b-259">Aggiungere gli strumenti di scaffolding</span><span class="sxs-lookup"><span data-stu-id="7b51b-259">Add scaffold tooling</span></span>

<span data-ttu-id="7b51b-260">In questa sezione viene usata la console di Gestione pacchetti per aggiungere il pacchetto di generazione del codice Web di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7b51b-260">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="7b51b-261">Questo pacchetto è necessario per eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="7b51b-261">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="7b51b-262">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="7b51b-262">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="7b51b-263">Nella console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b51b-263">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="7b51b-264">Con il comando precedente i pacchetti NuGet vengono aggiunti al file \* con estensione csproj:</span><span class="sxs-lookup"><span data-stu-id="7b51b-264">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="7b51b-265">Eseguire lo scaffolding del modello</span><span class="sxs-lookup"><span data-stu-id="7b51b-265">Scaffold the model</span></span>

* <span data-ttu-id="7b51b-266">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="7b51b-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7b51b-267">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b51b-267">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="7b51b-268">Se viene generato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="7b51b-268">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="7b51b-269">Eseguire nuovamente il comando e aggiungere un commento alla fine della pagina.</span><span class="sxs-lookup"><span data-stu-id="7b51b-269">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="7b51b-270">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="7b51b-270">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="7b51b-271">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="7b51b-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="7b51b-272">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="7b51b-272">Build the project.</span></span> <span data-ttu-id="7b51b-273">La compilazione genera errori simili al seguente:</span><span class="sxs-lookup"><span data-stu-id="7b51b-273">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="7b51b-274">Convertire a livello globale `_context.Student` in `_context.Students` (ovvero aggiungere una "s" a `Student`).</span><span class="sxs-lookup"><span data-stu-id="7b51b-274">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="7b51b-275">Vengono trovate e aggiornate sette occorrenze.</span><span class="sxs-lookup"><span data-stu-id="7b51b-275">7 occurrences are found and updated.</span></span> <span data-ttu-id="7b51b-276">Si spera di risolvere [questo bug](https://github.com/aspnet/Scaffolding/issues/633) nella prossima versione.</span><span class="sxs-lookup"><span data-stu-id="7b51b-276">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="7b51b-277">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="7b51b-277">Test the app</span></span>

<span data-ttu-id="7b51b-278">Eseguire l'app e selezionare il collegamento **Students** (Studenti).</span><span class="sxs-lookup"><span data-stu-id="7b51b-278">Run the app and select the **Students** link.</span></span> <span data-ttu-id="7b51b-279">A seconda delle dimensione del browser, il collegamento **Students** (Studenti) viene visualizzato in alto alla pagina.</span><span class="sxs-lookup"><span data-stu-id="7b51b-279">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="7b51b-280">Se il collegamento **Students** (Studenti) non è visibile, fare clic sull'icona di spostamento in alto a destra.</span><span class="sxs-lookup"><span data-stu-id="7b51b-280">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Home page di Contoso University ristretta](intro/_static/home-page-narrow.png)

<span data-ttu-id="7b51b-282">Eseguire il test dei collegamenti **Create** (Crea), **Edit** (Modifica) e **Details** (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="7b51b-282">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="7b51b-283">Visualizzare il database</span><span class="sxs-lookup"><span data-stu-id="7b51b-283">View the DB</span></span>

<span data-ttu-id="7b51b-284">Dopo aver avviato l'app, `DbInitializer.Initialize` chiama `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-284">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="7b51b-285">`EnsureCreated` controlla l'esistenza del database e se necessario ne crea uno.</span><span class="sxs-lookup"><span data-stu-id="7b51b-285">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="7b51b-286">Se il database non contiene studenti, il metodo `Initialize` aggiunge gli studenti.</span><span class="sxs-lookup"><span data-stu-id="7b51b-286">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="7b51b-287">Aprire **Esplora oggetti di SQL Server** dal menu **Visualizza** in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7b51b-287">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="7b51b-288">In Esplora oggetti di SQL Server fare clic su **(localdb)\MSSQLLocalDB > Database > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="7b51b-288">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="7b51b-289">Espandere il nodo **Tabelle**.</span><span class="sxs-lookup"><span data-stu-id="7b51b-289">Expand the **Tables** node.</span></span>

<span data-ttu-id="7b51b-290">Fare clic con il pulsante destro del mouse sulla tabella **Student** (Studente) e fare clic su **Visualizza dati** per visualizzare le colonne create e le righe inserite nella tabella.</span><span class="sxs-lookup"><span data-stu-id="7b51b-290">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="7b51b-291">I file di database con estensione *mdf* e *ldf* sono contenuti nella cartella *C:\Utenti\\<yourusername>*.</span><span class="sxs-lookup"><span data-stu-id="7b51b-291">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="7b51b-292">`EnsureCreated` viene chiamato all'avvio dell'app e consente il flusso di lavoro seguente:</span><span class="sxs-lookup"><span data-stu-id="7b51b-292">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="7b51b-293">Eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="7b51b-293">Delete the DB.</span></span>
* <span data-ttu-id="7b51b-294">Modificare lo schema di database, ad esempio, aggiungere un campo `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-294">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="7b51b-295">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="7b51b-295">Run the app.</span></span>

<span data-ttu-id="7b51b-296">`EnsureCreated` crea un database con la colonna `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-296">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="7b51b-297">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="7b51b-297">Conventions</span></span>

<span data-ttu-id="7b51b-298">Grazie all'uso di convenzioni o di ipotesi di Entity Framework Core, la quantità di codice scritto che è necessario a Entity Framework Core per creare un database completo è minino.</span><span class="sxs-lookup"><span data-stu-id="7b51b-298">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="7b51b-299">I nomi delle proprietà `DbSet` vengono usate come nomi di tabella.</span><span class="sxs-lookup"><span data-stu-id="7b51b-299">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="7b51b-300">Per le entità a cui non fa riferimento una proprietà `DbSet`, i nomi della classe di entità vengono usati come nome di tabella.</span><span class="sxs-lookup"><span data-stu-id="7b51b-300">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="7b51b-301">I nomi della proprietà di entità vengono usati come nomi di colonna.</span><span class="sxs-lookup"><span data-stu-id="7b51b-301">Entity property names are used for column names.</span></span>

* <span data-ttu-id="7b51b-302">Le proprietà dell'entità vengono denominate ID o classnameID e vengono riconosciute come proprietà della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="7b51b-302">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="7b51b-303">Una proprietà viene interpretata come proprietà di una chiave esterna se è denominata *<navigation property name><primary key property name>*, ad esempio `StudentID` per la proprietà di navigazione `Student` poiché la chiave primaria dell'entità `Student` è `ID`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-303">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="7b51b-304">Le proprietà di una chiave esterna possono essere denominate *<primary key property name>*, ad esempio `EnrollmentID` poiché la chiave primaria dell'entità `Enrollment` è `EnrollmentID`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-304">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="7b51b-305">È possibile eseguire l'override del comportamento convenzionale.</span><span class="sxs-lookup"><span data-stu-id="7b51b-305">Conventional behavior can be overridden.</span></span> <span data-ttu-id="7b51b-306">Ad esempio, i nomi di tabella possono essere specificati in modo esplicito, come illustrato in precedenza in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7b51b-306">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="7b51b-307">I nomi delle colonne possono essere impostati in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="7b51b-307">The column names can be explicitly set.</span></span> <span data-ttu-id="7b51b-308">Le chiavi primarie e le chiavi esterne possono essere impostate in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="7b51b-308">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="7b51b-309">Codice asincrono</span><span class="sxs-lookup"><span data-stu-id="7b51b-309">Asynchronous code</span></span>

<span data-ttu-id="7b51b-310">La programmazione asincrona è la modalità predefinita per ASP.NET Core ed Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7b51b-310">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="7b51b-311">Per un server Web è disponibile un numero limitato di thread e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso.</span><span class="sxs-lookup"><span data-stu-id="7b51b-311">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="7b51b-312">In queste circostanze il server non può elaborare nuove richieste finché i thread non saranno liberi.</span><span class="sxs-lookup"><span data-stu-id="7b51b-312">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="7b51b-313">Con il codice sincrono, può succedere che molti thread siano vincolati nonostante in quel momento non stiano eseguendo alcuna operazione. Rimangono tuttavia in attesa che l'operazione I/O sia completata.</span><span class="sxs-lookup"><span data-stu-id="7b51b-313">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="7b51b-314">Con il codice asincrono, se un processo è in attesa del completamento dell'operazione I/O, il thread viene liberato e il server lo può usare per l'elaborazione di altre richieste.</span><span class="sxs-lookup"><span data-stu-id="7b51b-314">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="7b51b-315">Di conseguenza, il codice asincrono consente un uso più efficiente delle risorse e il server è abilitato per gestire una maggiore quantità di traffico senza ritardi.</span><span class="sxs-lookup"><span data-stu-id="7b51b-315">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="7b51b-316">Il codice asincrono comporta un minimo sovraccarico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7b51b-316">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="7b51b-317">In caso di traffico ridotto, il calo delle prestazioni è irrilevante, mentre nelle situazioni di traffico elevato, è essenziale ottimizzare le prestazioni potenziali.</span><span class="sxs-lookup"><span data-stu-id="7b51b-317">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="7b51b-318">Nel codice seguente la parola chiave `async`, il valore restituito `Task<T>`, la parola chiave `await` e il metodo `ToListAsync` consentono di eseguire il codice in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="7b51b-318">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="7b51b-319">La parola chiave `async` indica al compilatore di eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b51b-319">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="7b51b-320">Generare callback per parti del corpo del metodo.</span><span class="sxs-lookup"><span data-stu-id="7b51b-320">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="7b51b-321">Creare automaticamente l' oggetto [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) restituito.</span><span class="sxs-lookup"><span data-stu-id="7b51b-321">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="7b51b-322">Per altre informazioni, vedere [Tipo restituito Task](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="7b51b-322">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="7b51b-323">Il tipo restituito implicito `Task` rappresenta il lavoro in corso.</span><span class="sxs-lookup"><span data-stu-id="7b51b-323">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="7b51b-324">La parola chiave `await` indica al compilatore di suddividere il metodo in due parti.</span><span class="sxs-lookup"><span data-stu-id="7b51b-324">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="7b51b-325">La prima parte termina con l'operazione avviata in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="7b51b-325">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="7b51b-326">La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="7b51b-326">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="7b51b-327">`ToListAsync` è la versione asincrona del metodo di estensione `ToList`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-327">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="7b51b-328">Alcuni aspetti da considerare quando si scrive codice asincrono usato da Entity Framework Core:</span><span class="sxs-lookup"><span data-stu-id="7b51b-328">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="7b51b-329">Solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="7b51b-329">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="7b51b-330">Sono incluse `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` e `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-330">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="7b51b-331">Non sono incluse le istruzioni che modificano solo un oggetto `IQueryable`, ad esempio `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="7b51b-331">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="7b51b-332">Un contesto di Entity Framework Core non è thread-safe. Non tentare di eseguire più operazioni parallelamente.</span><span class="sxs-lookup"><span data-stu-id="7b51b-332">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="7b51b-333">Per sfruttare i vantaggi del codice asincrono in termini di prestazioni, verificare che i pacchetti della libreria impiegati, ad esempio per il paging, usino la modalità asincrona per chiamare i metodi di Entity Framework Core che generano query da inviare al database.</span><span class="sxs-lookup"><span data-stu-id="7b51b-333">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="7b51b-334">Per altre informazioni sulla programmazione asincrona in .NET, vedere [Panoramica della programmazione asincrona](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="7b51b-334">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="7b51b-335">Nella prossima esercitazione, vengono esaminate le operazioni CRUD per creare, leggere, aggiornare, eliminare ed elencare.</span><span class="sxs-lookup"><span data-stu-id="7b51b-335">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7b51b-336">avanti</span><span class="sxs-lookup"><span data-stu-id="7b51b-336">Next</span></span>](xref:data/ef-rp/crud)
