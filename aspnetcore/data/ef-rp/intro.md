---
title: Pagine Razor con Entity Framework Core - esercitazione 1 di 8
author: rick-anderson
description: Viene illustrato come creare un'app di pagine Razor usando Entity Framework Core
keywords: Esercitazione di ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: d3bcf9aaf7fa809825a0ba8631ee52d3860b090d
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/05/2017
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="5491c-104">Introduzione a Entity Framework Core con Visual Studio (1 di 8) e di pagine Razor</span><span class="sxs-lookup"><span data-stu-id="5491c-104">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="5491c-105">Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5491c-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5491c-106">L'app web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC 2.0 di base utilizzando componenti di base di Entity Framework (EF) 2.0 e Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="5491c-106">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="5491c-107">L'app di esempio è un sito web per un'università Contoso fittizio.</span><span class="sxs-lookup"><span data-stu-id="5491c-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="5491c-108">Include funzionalità, ad esempio l'ammissione di studenti, la creazione di corsi e assegnazioni istruttore.</span><span class="sxs-lookup"><span data-stu-id="5491c-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="5491c-109">Questa pagina è il primo in una serie di esercitazioni che illustrano come compilare l'applicazione di esempio Contoso University.</span><span class="sxs-lookup"><span data-stu-id="5491c-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="5491c-110">Scaricare o visualizzare app completata.</span><span class="sxs-lookup"><span data-stu-id="5491c-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="5491c-111">[Istruzioni di download](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="5491c-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5491c-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="5491c-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a><span data-ttu-id="5491c-113">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="5491c-113">Troubleshooting</span></span>

<span data-ttu-id="5491c-114">Se si esegue un problema non è possibile risolvere, è possibile trovare la soluzione in genere confrontando il codice per il [completato fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) o [progetto completato](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="5491c-114">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="5491c-115">Per un elenco di errori comuni e come risolverli, vedere [la sezione Risoluzione dell'ultima esercitazione nella serie](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="5491c-115">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="5491c-116">Se non si trova ciò che è necessario non esiste, è possibile pubblicare una domanda StackOverflow.com per [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="5491c-116">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="5491c-117">Questa serie di esercitazioni si basa su ciò che viene eseguita nelle esercitazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="5491c-117">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="5491c-118">Si consiglia di salvare una copia del progetto dopo ogni completamento dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5491c-118">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="5491c-119">Se si verificano problemi, è possibile ricominciare dall'esercitazione precedente anziché tornando all'inizio.</span><span class="sxs-lookup"><span data-stu-id="5491c-119">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="5491c-120">In alternativa, è possibile scaricare un [completato fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) e ricominciare usando la fase completata.</span><span class="sxs-lookup"><span data-stu-id="5491c-120">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="5491c-121">L'app web di Contoso University</span><span class="sxs-lookup"><span data-stu-id="5491c-121">The Contoso University web app</span></span>

<span data-ttu-id="5491c-122">L'applicazione compilata in queste esercitazioni è un sito web di base university.</span><span class="sxs-lookup"><span data-stu-id="5491c-122">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="5491c-123">Gli utenti possono visualizzare e aggiornare studente, corsi e istruttore informazioni.</span><span class="sxs-lookup"><span data-stu-id="5491c-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="5491c-124">Ecco alcune delle schermate create nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5491c-124">Here are a few of the screens created in the tutorial.</span></span>

![Pagina di indice di studenti](intro/_static/students-index.png)

![Pagina Modifica studenti](intro/_static/student-edit.png)

<span data-ttu-id="5491c-127">Lo stile dell'interfaccia utente del sito è vicino a ciò che viene generato tramite i modelli predefiniti.</span><span class="sxs-lookup"><span data-stu-id="5491c-127">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="5491c-128">L'esercitazione è attiva EF Core con pagine Razor, non l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="5491c-128">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="5491c-129">Creare un'app web pagine Razor</span><span class="sxs-lookup"><span data-stu-id="5491c-129">Create a Razor Pages web app</span></span>

* <span data-ttu-id="5491c-130">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="5491c-130">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="5491c-131">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5491c-131">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="5491c-132">Denominare il progetto **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="5491c-132">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="5491c-133">È importante denominare il progetto *ContosoUniversity* pertanto gli spazi dei nomi corrisponde a quando il codice è copiati o incollati.</span><span class="sxs-lookup"><span data-stu-id="5491c-133">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="5491c-134">![nuova applicazione Web ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="5491c-134">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="5491c-135">Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="5491c-135">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="5491c-136">![Applicazione Web (pagine Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="5491c-136">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="5491c-137">Premere **F5** per eseguire l'app in modalità di debug o **Ctrl-F5** per l'esecuzione senza collegamento del debugger</span><span class="sxs-lookup"><span data-stu-id="5491c-137">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="5491c-138">Impostare lo stile del sito</span><span class="sxs-lookup"><span data-stu-id="5491c-138">Set up the site style</span></span>

<span data-ttu-id="5491c-139">Alcune modifiche consente di impostare il menu del sito, layout e home page di.</span><span class="sxs-lookup"><span data-stu-id="5491c-139">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="5491c-140">Aprire *Pages/_Layout.cshtml* e apportare le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="5491c-140">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="5491c-141">Modificare tutte le occorrenze di "ContosoUniversity" in "Contoso University."</span><span class="sxs-lookup"><span data-stu-id="5491c-141">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="5491c-142">Esistono tre occorrenze.</span><span class="sxs-lookup"><span data-stu-id="5491c-142">There are three occurrences.</span></span>

* <span data-ttu-id="5491c-143">Aggiungere le voci di menu per **studenti**, **corsi**, **i docenti**, e **reparti**ed eliminare il **contatto** voce di menu.</span><span class="sxs-lookup"><span data-stu-id="5491c-143">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="5491c-144">Le modifiche sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="5491c-144">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-39,47&range=1-50)]

<span data-ttu-id="5491c-145">In *Views/Home/Index.cshtml*, sostituire il contenuto del file con il codice seguente per sostituire il testo su ASP.NET e MVC con testo sull'app:</span><span class="sxs-lookup"><span data-stu-id="5491c-145">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="5491c-146">Premere CTRL+F5 per eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="5491c-146">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="5491c-147">Viene visualizzata la home page con le schede create nelle esercitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5491c-147">The home page is displayed with tabs created in the following tutorials:</span></span>

![Home page di Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="5491c-149">Creare il modello di dati</span><span class="sxs-lookup"><span data-stu-id="5491c-149">Create the data model</span></span>

<span data-ttu-id="5491c-150">Creare classi di entità per l'applicazione Contoso University.</span><span class="sxs-lookup"><span data-stu-id="5491c-150">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="5491c-151">Iniziare con le tre entità seguenti:</span><span class="sxs-lookup"><span data-stu-id="5491c-151">Start with the following three entities:</span></span>

![Diagramma del modello dati corso-registrazione degli studenti](intro/_static/data-model-diagram.png)

<span data-ttu-id="5491c-153">È una relazione uno-a-molti tra `Student` e `Enrollment` entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-153">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="5491c-154">È una relazione uno-a-molti tra `Course` e `Enrollment` entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-154">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="5491c-155">Uno studente può registrarsi in un numero qualsiasi di corsi.</span><span class="sxs-lookup"><span data-stu-id="5491c-155">A student can enroll in any number of courses.</span></span> <span data-ttu-id="5491c-156">Un corso può avere qualsiasi numero di studenti iscritti in essa contenuti.</span><span class="sxs-lookup"><span data-stu-id="5491c-156">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="5491c-157">Nelle sezioni seguenti, viene creata una classe per ognuna di queste entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-157">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="5491c-158">L'entità di Student</span><span class="sxs-lookup"><span data-stu-id="5491c-158">The Student entity</span></span>

![Diagramma di entità per studenti](intro/_static/student-entity.png)

<span data-ttu-id="5491c-160">Creare un *modelli* cartella.</span><span class="sxs-lookup"><span data-stu-id="5491c-160">Create a *Models* folder.</span></span> <span data-ttu-id="5491c-161">Nel *modelli* cartella, creare un file di classe denominato *Student.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5491c-161">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="5491c-162">Il `ID` proprietà diventa la colonna chiave primaria della tabella di database (DB) che corrisponde a questa classe.</span><span class="sxs-lookup"><span data-stu-id="5491c-162">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="5491c-163">Per impostazione predefinita, i Core EF interpreta una proprietà denominata `ID` o `classnameID` come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="5491c-163">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="5491c-164">Il `Enrollments` è una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="5491c-164">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="5491c-165">Le proprietà di navigazione di collegare altre entità correlate a questa entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-165">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="5491c-166">In questo caso, il `Enrollments` proprietà di un `Student entity` contiene tutti il `Enrollment` entità correlate che `Student`.</span><span class="sxs-lookup"><span data-stu-id="5491c-166">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="5491c-167">Ad esempio, se una riga studente nella DB presenta due correlate righe di registrazione, il `Enrollments` proprietà di navigazione contiene questi due `Enrollment` entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-167">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="5491c-168">Un processo di `Enrollment` riga è una riga che contiene valore della chiave primaria tale student di `StudentID` colonna.</span><span class="sxs-lookup"><span data-stu-id="5491c-168">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="5491c-169">Si supponga ad esempio gli studenti con ID = 1 dispone di due righe `Enrollment` tabella.</span><span class="sxs-lookup"><span data-stu-id="5491c-169">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="5491c-170">Il `Enrollment` tabella contiene due righe con `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="5491c-170">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="5491c-171">`StudentID`è una chiave esterna nel `Enrollment` tabella che specifica lo studente il `Student` tabella.</span><span class="sxs-lookup"><span data-stu-id="5491c-171">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="5491c-172">Se una proprietà di navigazione può contenere più entità, è possibile che la proprietà di navigazione deve essere un tipo di elenco, ad esempio `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="5491c-172">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="5491c-173">`ICollection<T>`è possibile specificare, o un tipo, ad esempio `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="5491c-173">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="5491c-174">Quando `ICollection<T>` è, Core EF crea un `HashSet<T>` insieme per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5491c-174">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="5491c-175">Proprietà di navigazione che contengono più entità provengono dalle relazioni molti-a-molti e uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="5491c-175">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="5491c-176">L'entità di registrazione</span><span class="sxs-lookup"><span data-stu-id="5491c-176">The Enrollment entity</span></span>

![Diagramma di entità di registrazione](intro/_static/enrollment-entity.png)

<span data-ttu-id="5491c-178">Nel *modelli* cartella, creare *Enrollment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5491c-178">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="5491c-179">Il `EnrollmentID` proprietà è la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="5491c-179">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="5491c-180">Questa entità viene utilizzata la `classnameID` schema anziché `ID` come il `Student` entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-180">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="5491c-181">In genere gli sviluppatori di scegliere un modello e di utilizzano in tutto il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="5491c-181">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="5491c-182">In un'esercitazione successiva, usando un ID senza classname è visualizzato per renderne più semplice implementare l'ereditarietà nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="5491c-182">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="5491c-183">Il `Grade` proprietà è un `enum`.</span><span class="sxs-lookup"><span data-stu-id="5491c-183">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="5491c-184">Il punto interrogativo dopo il `Grade` dichiarazione del tipo di indica che il `Grade` proprietà è di tipo nullable.</span><span class="sxs-lookup"><span data-stu-id="5491c-184">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="5491c-185">Un livello null è diverso da un livello zero - null indica un livello non è noto o non è stato ancora assegnato.</span><span class="sxs-lookup"><span data-stu-id="5491c-185">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="5491c-186">Il `StudentID` proprietà è una chiave esterna e la proprietà di navigazione corrispondente è `Student`.</span><span class="sxs-lookup"><span data-stu-id="5491c-186">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="5491c-187">Un `Enrollment` è associata a una entità `Student` entità, pertanto la proprietà contiene un singolo `Student` entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-187">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="5491c-188">Il `Student` rispetto all'entità di `Student.Enrollments` proprietà di navigazione, che contiene più `Enrollment` entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-188">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="5491c-189">Il `CourseID` proprietà è una chiave esterna e la proprietà di navigazione corrispondente è `Course`.</span><span class="sxs-lookup"><span data-stu-id="5491c-189">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="5491c-190">Un `Enrollment` è associata a una entità `Course` entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-190">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="5491c-191">Core EF interpreta una proprietà come chiave esterna se il file è denominato `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="5491c-191">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="5491c-192">Ad esempio,`StudentID` per il `Student` proprietà di navigazione, poiché il `Student` chiave primaria dell'entità è `ID`.</span><span class="sxs-lookup"><span data-stu-id="5491c-192">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="5491c-193">Proprietà di chiave esterna può anche essere denominata `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="5491c-193">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="5491c-194">Ad esempio, `CourseID` poiché il `Course` chiave primaria dell'entità è `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="5491c-194">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="5491c-195">L'entità Course</span><span class="sxs-lookup"><span data-stu-id="5491c-195">The Course entity</span></span>

![Diagramma di entità Course](intro/_static/course-entity.png)

<span data-ttu-id="5491c-197">Nel *modelli* cartella, creare *Course.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5491c-197">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="5491c-198">Il `Enrollments` è una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="5491c-198">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="5491c-199">Oggetto `Course` entità può essere correlato a un numero qualsiasi di `Enrollment` entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-199">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="5491c-200">Il `DatabaseGenerated` attributo consente all'app di specificare la chiave primaria, anziché con il database di generarlo.</span><span class="sxs-lookup"><span data-stu-id="5491c-200">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="5491c-201">Creare il contesto DB SchoolContext</span><span class="sxs-lookup"><span data-stu-id="5491c-201">Create the SchoolContext DB context</span></span>

<span data-ttu-id="5491c-202">La classe principale che coordina le funzionalità principali di Entity Framework per la classe del contesto DB non è un modello di dati specificato.</span><span class="sxs-lookup"><span data-stu-id="5491c-202">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="5491c-203">Il contesto dei dati è derivato da `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="5491c-203">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="5491c-204">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="5491c-204">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="5491c-205">In questo progetto, la classe è denominata `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="5491c-205">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="5491c-206">Nella cartella del progetto, creare una cartella denominata *dati*.</span><span class="sxs-lookup"><span data-stu-id="5491c-206">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="5491c-207">Nel *dati* cartella creare *SchoolContext.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5491c-207">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="5491c-208">Questo codice crea un `DbSet` proprietà per ogni set di entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-208">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="5491c-209">Nella terminologia di base di Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="5491c-209">In EF Core terminology:</span></span>

* <span data-ttu-id="5491c-210">Un set in genere di entità corrisponde a una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="5491c-210">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="5491c-211">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="5491c-211">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="5491c-212">`DbSet<Enrollment>`e `DbSet<Course>` può essere omesso.</span><span class="sxs-lookup"><span data-stu-id="5491c-212">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="5491c-213">Core EF li include in modo implicito perché il `Student` i riferimenti alle entità di `Enrollment` entità e `Enrollment` i riferimenti alle entità la `Course` entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-213">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="5491c-214">Per questa esercitazione, mantenere `DbSet<Enrollment>` e `DbSet<Course>` nel `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="5491c-214">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="5491c-215">Quando viene creato il database, Core EF crea le tabelle che presentano lo stesso come nomi di `DbSet` i nomi delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="5491c-215">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="5491c-216">I nomi delle proprietà per le raccolte vengono in genere plurale (studenti anziché studente).</span><span class="sxs-lookup"><span data-stu-id="5491c-216">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="5491c-217">Gli sviluppatori non d'accordo sul se i nomi di tabella devono essere plurali.</span><span class="sxs-lookup"><span data-stu-id="5491c-217">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="5491c-218">Per queste esercitazioni, viene eseguito l'override del comportamento predefinito specificando i nomi di tabella singolare in DbContext.</span><span class="sxs-lookup"><span data-stu-id="5491c-218">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="5491c-219">Per specificare i nomi di tabella singolare, aggiungere il codice evidenziato di seguito:</span><span class="sxs-lookup"><span data-stu-id="5491c-219">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="5491c-220">Registrare il contesto con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="5491c-220">Register the context with dependency injection</span></span>

<span data-ttu-id="5491c-221">ASP.NET Core include [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5491c-221">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5491c-222">Servizi (ad esempio, il contesto di database di base di EF) vengono registrati con l'inserimento di dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5491c-222">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="5491c-223">I componenti che richiedono tali servizi (ad esempio pagine Razor) vengono forniti questi servizi tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="5491c-223">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="5491c-224">Il codice del costruttore che ottiene un'istanza di contesto db è illustrato più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5491c-224">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="5491c-225">Per registrare `SchoolContext` come servizio, aprire *Startup.cs*e aggiungere le righe evidenziate per il `ConfigureServices` metodo.</span><span class="sxs-lookup"><span data-stu-id="5491c-225">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="5491c-226">Il nome della stringa di connessione viene passato al contesto chiamando un metodo su un `DbContextOptionsBuilder` oggetto.</span><span class="sxs-lookup"><span data-stu-id="5491c-226">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="5491c-227">Per lo sviluppo locale, il [il sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione di *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="5491c-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="5491c-228">Aggiungere `using` istruzioni per `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore` gli spazi dei nomi.</span><span class="sxs-lookup"><span data-stu-id="5491c-228">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="5491c-229">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="5491c-229">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="5491c-230">Aprire il *appSettings. JSON* file e aggiungere una stringa di connessione come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5491c-230">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="5491c-231">La stringa di connessione precedente utilizza `ConnectRetryCount=0` per impedire [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) di bloccarsi.</span><span class="sxs-lookup"><span data-stu-id="5491c-231">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="5491c-232">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="5491c-232">SQL Server Express LocalDB</span></span>

<span data-ttu-id="5491c-233">La stringa di connessione specifica un database LocalDB di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5491c-233">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="5491c-234">LocalDB è una versione leggera di SQL Server Express Database Engine ed è destinato per lo sviluppo di app non uso in produzione.</span><span class="sxs-lookup"><span data-stu-id="5491c-234">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="5491c-235">Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa.</span><span class="sxs-lookup"><span data-stu-id="5491c-235">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="5491c-236">Per impostazione predefinita, crea LocalDB *con estensione mdf* file DB il `C:/Users/<user>` directory.</span><span class="sxs-lookup"><span data-stu-id="5491c-236">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="5491c-237">Aggiungere codice per inizializzare il database con dati di test</span><span class="sxs-lookup"><span data-stu-id="5491c-237">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="5491c-238">Core EF crea un database vuoto.</span><span class="sxs-lookup"><span data-stu-id="5491c-238">EF Core creates an empty DB.</span></span> <span data-ttu-id="5491c-239">In questa sezione, un *valore di inizializzazione* viene scritto un metodo viene popolato con dati di test.</span><span class="sxs-lookup"><span data-stu-id="5491c-239">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="5491c-240">Nel *dati* cartella, creare un nuovo file di classe denominato *DbInitializer.cs* e aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5491c-240">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="5491c-241">Il codice controlla se sono presenti studenti nel database.</span><span class="sxs-lookup"><span data-stu-id="5491c-241">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="5491c-242">Se sono presenti Nessuno studente nel database, il database viene inizializzato con dati di test.</span><span class="sxs-lookup"><span data-stu-id="5491c-242">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="5491c-243">Carica i dati di test in matrici anziché `List<T>` raccolte per ottimizzare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="5491c-243">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="5491c-244">Il `EnsureCreated` metodo crea automaticamente il database per il contesto di database.</span><span class="sxs-lookup"><span data-stu-id="5491c-244">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="5491c-245">Se il database esiste, `EnsureCreated` restituisce senza modificare il database.</span><span class="sxs-lookup"><span data-stu-id="5491c-245">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="5491c-246">In *Program.cs*, modificare il `Main` metodo per eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5491c-246">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="5491c-247">Ottenere un'istanza di contesto DB dal contenitore dell'inserimento di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="5491c-247">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="5491c-248">Chiamare il metodo di inizializzazione, passare il contesto.</span><span class="sxs-lookup"><span data-stu-id="5491c-248">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="5491c-249">Eliminare il contesto al completamento del metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="5491c-249">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="5491c-250">Nel codice seguente viene aggiornato *Program.cs* file.</span><span class="sxs-lookup"><span data-stu-id="5491c-250">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="5491c-251">La prima volta che si esegue l'app, il database viene creato e sottoposto a seeding con dati di test.</span><span class="sxs-lookup"><span data-stu-id="5491c-251">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="5491c-252">Quando viene aggiornato il modello di dati:</span><span class="sxs-lookup"><span data-stu-id="5491c-252">When the data model is updated:</span></span>
* <span data-ttu-id="5491c-253">Eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="5491c-253">Delete the DB.</span></span>
* <span data-ttu-id="5491c-254">Aggiornare il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="5491c-254">Update the seed method.</span></span>
* <span data-ttu-id="5491c-255">Eseguire l'app e viene creato un nuovo database di seeding.</span><span class="sxs-lookup"><span data-stu-id="5491c-255">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="5491c-256">Nelle esercitazioni successive, il database viene aggiornato quando i modello di dati, senza eliminare e ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="5491c-256">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="5491c-257">Aggiungere lo scaffolding strumenti</span><span class="sxs-lookup"><span data-stu-id="5491c-257">Add scaffold tooling</span></span>

<span data-ttu-id="5491c-258">In questa sezione, il Package Manager Console (PMC) viene utilizzato per aggiungere il pacchetto di generazione codice web Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5491c-258">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="5491c-259">Questo pacchetto è necessario per eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="5491c-259">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="5491c-260">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="5491c-260">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="5491c-261">In Package Manager Console (PMC), immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="5491c-261">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="5491c-262">Il comando precedente consente di aggiungere i pacchetti NuGet per il file *. csproj:</span><span class="sxs-lookup"><span data-stu-id="5491c-262">The previous command adds the NuGet packages to the *.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="5491c-263">Lo scaffolding di modello</span><span class="sxs-lookup"><span data-stu-id="5491c-263">Scaffold the model</span></span>

* <span data-ttu-id="5491c-264">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="5491c-264">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5491c-265">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="5491c-265">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="5491c-266">Se viene generato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="5491c-266">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="5491c-267">Eseguire nuovamente il comando e lascia un commento nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="5491c-267">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="5491c-268">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="5491c-268">Build the project.</span></span> <span data-ttu-id="5491c-269">La compilazione genera errori simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="5491c-269">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="5491c-270">Una modifica globale `_context.Student` a `_context.Students` (ovvero, aggiungere una "s" `Student`).</span><span class="sxs-lookup"><span data-stu-id="5491c-270">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="5491c-271">7 occorrenze sono disponibili e aggiornate.</span><span class="sxs-lookup"><span data-stu-id="5491c-271">7 occurrences are found and updated.</span></span> <span data-ttu-id="5491c-272">Ci auguriamo che correggere [questo bug](https://github.com/aspnet/Scaffolding/issues/633) nella prossima versione.</span><span class="sxs-lookup"><span data-stu-id="5491c-272">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="5491c-273">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="5491c-273">Test the app</span></span>

<span data-ttu-id="5491c-274">Eseguire l'app e selezionare il **studenti** collegamento.</span><span class="sxs-lookup"><span data-stu-id="5491c-274">Run the app and select the **Students** link.</span></span> <span data-ttu-id="5491c-275">A seconda della larghezza del browser, il **studenti** collegamento viene visualizzato nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="5491c-275">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="5491c-276">Se il **studenti** collegamento non è visibile, fare clic sull'icona di spostamento in alto a destra.</span><span class="sxs-lookup"><span data-stu-id="5491c-276">If the **Students** link is not visible, click the navigation icon in the upper right corner.</span></span>

![Pagina iniziale di Contoso University "narrow"](intro/_static/home-page-narrow.png)

<span data-ttu-id="5491c-278">Test di **crea**, **modifica**, e **dettagli** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="5491c-278">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="5491c-279">Visualizzare il database</span><span class="sxs-lookup"><span data-stu-id="5491c-279">View the DB</span></span>

<span data-ttu-id="5491c-280">Quando viene avviata l'app, `DbInitializer.Initialize` chiamate `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="5491c-280">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="5491c-281">`EnsureCreated`rileva se il database esiste e viene creata una se necessario.</span><span class="sxs-lookup"><span data-stu-id="5491c-281">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="5491c-282">Se sono presenti Nessuno studente nel database, il `Initialize` metodo aggiunge gli studenti.</span><span class="sxs-lookup"><span data-stu-id="5491c-282">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="5491c-283">Aprire **Esplora oggetti di SQL Server** (sillaba SSOX) dal **vista** menu in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5491c-283">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="5491c-284">Sillaba SSOX, fare clic su **(localdb) \MSSQLLocalDB > database > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="5491c-284">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="5491c-285">Espandere il **tabelle** nodo.</span><span class="sxs-lookup"><span data-stu-id="5491c-285">Expand the **Tables** node.</span></span>

<span data-ttu-id="5491c-286">Fare doppio clic su di **Student** tabella e fare clic su **Visualizza dati** per visualizzare le colonne create e le righe inserite nella tabella.</span><span class="sxs-lookup"><span data-stu-id="5491c-286">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="5491c-287">Il *con estensione mdf* e *ldf* presenti file di database di *C:\Users\<nomeutente >* cartella.</span><span class="sxs-lookup"><span data-stu-id="5491c-287">The *.mdf* and *.ldf* DB files are in the *C:\Users\<yourusername>* folder.</span></span>

<span data-ttu-id="5491c-288">`EnsureCreated`viene chiamato all'avvio dell'app, che consente il flusso di lavoro seguente:</span><span class="sxs-lookup"><span data-stu-id="5491c-288">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="5491c-289">Eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="5491c-289">Delete the DB.</span></span>
* <span data-ttu-id="5491c-290">Modificare lo schema di database (ad esempio, aggiungere un `EmailAddress` campo).</span><span class="sxs-lookup"><span data-stu-id="5491c-290">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="5491c-291">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="5491c-291">Run the app.</span></span>

<span data-ttu-id="5491c-292">`EnsureCreated`Crea un database con il`EmailAddress` colonna.</span><span class="sxs-lookup"><span data-stu-id="5491c-292">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="5491c-293">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="5491c-293">Conventions</span></span>

<span data-ttu-id="5491c-294">La quantità di codice scritto in ordine per Core di Entity Framework creare un database completo è minima a causa dell'utilizzo di convenzioni o presupposti che rende Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5491c-294">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="5491c-295">I nomi delle `DbSet` proprietà vengono utilizzate come nomi di tabella.</span><span class="sxs-lookup"><span data-stu-id="5491c-295">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="5491c-296">Per le entità non fa riferimento un `DbSet` proprietà, come nomi di tabella vengono utilizzati i nomi di classe di entità.</span><span class="sxs-lookup"><span data-stu-id="5491c-296">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="5491c-297">I nomi delle proprietà di entità vengono utilizzati per i nomi di colonna.</span><span class="sxs-lookup"><span data-stu-id="5491c-297">Entity property names are used for column names.</span></span>

* <span data-ttu-id="5491c-298">Le proprietà delle entità denominate ID o classnameID sono riconosciute come proprietà della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="5491c-298">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="5491c-299">Una proprietà viene interpretata come una proprietà di chiave esterna se il file è denominato  *<navigation property name> <primary key property name>*  (ad esempio, `StudentID` per il `Student` proprietà di navigazione perché il `Student` chiave primaria dell'entità è `ID`).</span><span class="sxs-lookup"><span data-stu-id="5491c-299">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="5491c-300">Proprietà di chiave esterna può essere denominata  *<primary key property name>*  (ad esempio, `EnrollmentID` poiché il `Enrollment` chiave primaria dell'entità è `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="5491c-300">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="5491c-301">È possibile sostituire un comportamento convenzionale.</span><span class="sxs-lookup"><span data-stu-id="5491c-301">Conventional behavior can be overridden.</span></span> <span data-ttu-id="5491c-302">Ad esempio, i nomi di tabella possono essere specificati in modo esplicito, come illustrato in precedenza in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5491c-302">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="5491c-303">I nomi di colonna possono essere impostati in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="5491c-303">The column names can be explicitly set.</span></span> <span data-ttu-id="5491c-304">Le chiavi primarie e chiavi esterne possono essere impostate in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="5491c-304">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="5491c-305">Codice asincrono</span><span class="sxs-lookup"><span data-stu-id="5491c-305">Asynchronous code</span></span>

<span data-ttu-id="5491c-306">Programmazione asincrona è la modalità predefinita per ASP.NET di base e componenti di base di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5491c-306">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="5491c-307">Un server web ha un numero limitato di thread disponibili, e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso.</span><span class="sxs-lookup"><span data-stu-id="5491c-307">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="5491c-308">In questo caso, il server non è possibile elaborare nuove richieste fino a quando non verranno liberati i thread.</span><span class="sxs-lookup"><span data-stu-id="5491c-308">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="5491c-309">Con il codice sincrono, molti thread può essere vincolato mentre sono in realtà non sono svolgere alcuna operazione perché in attesa di completamento dei / o.</span><span class="sxs-lookup"><span data-stu-id="5491c-309">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="5491c-310">Con il codice asincrono, quando un processo è in attesa dei / o di completamento, il thread viene liberato per il server da utilizzare per l'elaborazione di altre richieste.</span><span class="sxs-lookup"><span data-stu-id="5491c-310">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="5491c-311">Di conseguenza, codice asincrono consente alle risorse di server da utilizzare in modo più efficiente e il server è abilitato per gestire più traffico senza ritardi.</span><span class="sxs-lookup"><span data-stu-id="5491c-311">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="5491c-312">Codice asincrono che presenta una piccola quantità di overhead in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5491c-312">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="5491c-313">Per le situazioni di traffico ridotto, il calo di prestazioni è irrilevante, mentre per le situazioni di traffico elevato, il potenziale miglioramento delle prestazioni è notevole.</span><span class="sxs-lookup"><span data-stu-id="5491c-313">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="5491c-314">Nel codice seguente, il `async` (parola chiave), `Task<T>` valore restituito, `await` (parola chiave), e `ToListAsync` metodo affinché il codice venga eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="5491c-314">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="5491c-315">Il `async` parola chiave indica al compilatore di:</span><span class="sxs-lookup"><span data-stu-id="5491c-315">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="5491c-316">Generare i callback per parti del corpo del metodo.</span><span class="sxs-lookup"><span data-stu-id="5491c-316">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="5491c-317">Creare automaticamente il [attività](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) oggetto restituito.</span><span class="sxs-lookup"><span data-stu-id="5491c-317">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that is returned.</span></span> <span data-ttu-id="5491c-318">Per ulteriori informazioni, vedere [attività Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="5491c-318">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="5491c-319">L'operatore implicito il tipo restituito `Task` rappresenta il lavoro in corso.</span><span class="sxs-lookup"><span data-stu-id="5491c-319">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="5491c-320">Il `await` (parola chiave), il compilatore suddividere il metodo in due parti.</span><span class="sxs-lookup"><span data-stu-id="5491c-320">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="5491c-321">La prima parte termina con l'operazione che viene avviato in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="5491c-321">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="5491c-322">La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="5491c-322">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="5491c-323">`ToListAsync`è la versione asincrona del `ToList` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="5491c-323">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="5491c-324">Alcuni aspetti da tenere presenti quando si scrive codice asincrono che utilizza Entity Framework Core:</span><span class="sxs-lookup"><span data-stu-id="5491c-324">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="5491c-325">Solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="5491c-325">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="5491c-326">Che include, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, e `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="5491c-326">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="5491c-327">Non include istruzioni che modificano solo un `IQueryable`, ad esempio `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="5491c-327">It does not include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="5491c-328">Un contesto di EF Core non è thread-safe: non tenta di eseguire più operazioni in parallelo.</span><span class="sxs-lookup"><span data-stu-id="5491c-328">An EF Core context is not threaded safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="5491c-329">Per sfruttare i vantaggi delle prestazioni del codice asincrono, verificare di pacchetti di raccolta, ad esempio per il paging, utilizzano async se esse chiamano metodi di Entity Framework Core che inviano query al database.</span><span class="sxs-lookup"><span data-stu-id="5491c-329">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="5491c-330">Per ulteriori informazioni sulla programmazione asincrona in .NET, vedere [Async Panoramica](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="5491c-330">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="5491c-331">Nella prossima esercitazione base CRUD (create, leggere, aggiornare ed eliminare) vengono esaminate le operazioni.</span><span class="sxs-lookup"><span data-stu-id="5491c-331">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="5491c-332">Successivo</span><span class="sxs-lookup"><span data-stu-id="5491c-332">Next</span></span>](xref:data/ef-rp/crud)
