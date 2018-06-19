---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementazione dell'ereditarietà con Entity Framework in un'applicazione ASP.NET MVC (8 di 10) | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ee088f841bdb68f4806b0b62be7d379b9eab9f8c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874005"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a><span data-ttu-id="0c975-103">Implementazione dell'ereditarietà con Entity Framework in un'applicazione ASP.NET MVC (8 di 10)</span><span class="sxs-lookup"><span data-stu-id="0c975-103">Implementing Inheritance with the Entity Framework in an ASP.NET MVC Application (8 of 10)</span></span>
====================
<span data-ttu-id="0c975-104">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0c975-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="0c975-105">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="0c975-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="0c975-106">L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 utilizzando Code First di Entity Framework 5 e Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="0c975-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="0c975-107">Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="0c975-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="0c975-108">È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto di avvio per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.</span><span class="sxs-lookup"><span data-stu-id="0c975-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="0c975-109">Se si esegue un problema, è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema.</span><span class="sxs-lookup"><span data-stu-id="0c975-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="0c975-110">In genere, è possibile trovare la soluzione al problema di mediante un confronto tra il codice per il codice completato.</span><span class="sxs-lookup"><span data-stu-id="0c975-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="0c975-111">Per alcuni errori comuni e su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="0c975-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="0c975-112">Nell'esercitazione precedente si gestite le eccezioni di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="0c975-112">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="0c975-113">In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="0c975-113">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="0c975-114">Nella programmazione orientata agli oggetti, è possibile utilizzare l'ereditarietà per eliminare il codice ridondante.</span><span class="sxs-lookup"><span data-stu-id="0c975-114">In object-oriented programming, you can use inheritance to eliminate redundant code.</span></span> <span data-ttu-id="0c975-115">In questa esercitazione verranno modificate le classi `Instructor` e `Student` in modo che derivino da una classe di base `Person` contenente proprietà quali `LastName` comuni a docenti e studenti.</span><span class="sxs-lookup"><span data-stu-id="0c975-115">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="0c975-116">Non verranno aggiunte o modificate pagine Web, ma si modificherà parte del codice e le modifiche verranno automaticamente riflesse nel database.</span><span class="sxs-lookup"><span data-stu-id="0c975-116">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a><span data-ttu-id="0c975-117">Tabella per gerarchia rispetto a ereditarietà tabella per tipo</span><span class="sxs-lookup"><span data-stu-id="0c975-117">Table-per-Hierarchy versus Table-per-Type Inheritance</span></span>

<span data-ttu-id="0c975-118">Nella programmazione orientata agli oggetti, è possibile utilizzare l'ereditarietà per renderlo più facile lavorare con le classi correlate.</span><span class="sxs-lookup"><span data-stu-id="0c975-118">In object-oriented programming, you can use inheritance to make it easier to work with related classes.</span></span> <span data-ttu-id="0c975-119">Ad esempio, il `Instructor` e `Student` classi di `School` modello di dati condividono diverse proprietà, che comporta il codice ridondante:</span><span class="sxs-lookup"><span data-stu-id="0c975-119">For example, the `Instructor` and `Student` classes in the `School` data model share several properties, which results in redundant code:</span></span>

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

<span data-ttu-id="0c975-121">Si supponga di voler eliminare il codice ridondante per le proprietà condivise dalle entità `Instructor` e `Student`.</span><span class="sxs-lookup"><span data-stu-id="0c975-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="0c975-122">È possibile creare un `Person` classe che contiene solo quelli condivisi di proprietà di base, quindi rendere la `Instructor` e `Student` che ereditano da tale classe di base, come illustrato nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="0c975-122">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

<span data-ttu-id="0c975-124">Questa struttura di ereditarietà può essere rappresentata nel database in diversi modi.</span><span class="sxs-lookup"><span data-stu-id="0c975-124">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="0c975-125">È possibile disporre di un `Person` tabella che include informazioni su studenti e insegnanti in un'unica tabella.</span><span class="sxs-lookup"><span data-stu-id="0c975-125">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="0c975-126">Alcune delle colonne è stato possibile applicare solo per i docenti (`HireDate`), alcuni solo per gli studenti (`EnrollmentDate`), in alcune entrambe (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="0c975-126">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="0c975-127">In genere, è necessario un *discriminatore* colonna per indicare quale tipo di ogni riga rappresenta.</span><span class="sxs-lookup"><span data-stu-id="0c975-127">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="0c975-128">La colonna discriminante può ad esempio indicare "Instructor" per i docenti e "Student" per gli studenti.</span><span class="sxs-lookup"><span data-stu-id="0c975-128">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Table-per-hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

<span data-ttu-id="0c975-130">Questo modello di generazione di una struttura di ereditarietà di entità da una singola tabella di database è denominato *della tabella per gerarchia* ereditarietà della.</span><span class="sxs-lookup"><span data-stu-id="0c975-130">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="0c975-131">Un'alternativa consiste nel rendere il database più simile alla struttura di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="0c975-131">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="0c975-132">Ad esempio, si potrebbe avere solo i campi nome `Person` tabella e sono separate `Instructor` e `Student` le tabelle con i campi delle date.</span><span class="sxs-lookup"><span data-stu-id="0c975-132">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

<span data-ttu-id="0c975-134">Questo modello di esecuzione di una tabella di database per ogni classe di entità viene denominata *tabella per tipo* ereditarietà (tabella per tipo).</span><span class="sxs-lookup"><span data-stu-id="0c975-134">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="0c975-135">Criteri di ereditarietà della tabella per gerarchia offrono in genere prestazioni migliori in Entity Framework di criteri di ereditarietà tabella per tipo, in quanto possono generare modelli di tabella per tipo query join complessi.</span><span class="sxs-lookup"><span data-stu-id="0c975-135">TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span> <span data-ttu-id="0c975-136">Questa esercitazione illustra come implementare l'ereditarietà tabella per gerarchia.</span><span class="sxs-lookup"><span data-stu-id="0c975-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="0c975-137">Che verranno eseguite eseguendo i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="0c975-137">You'll do that by performing the following steps:</span></span>

- <span data-ttu-id="0c975-138">Creare un `Person` classe e modificare il `Instructor` e `Student` classi da cui derivare `Person`.</span><span class="sxs-lookup"><span data-stu-id="0c975-138">Create a `Person` class and change the `Instructor` and `Student` classes to derive from `Person`.</span></span>
- <span data-ttu-id="0c975-139">Aggiungere codice al database modello mapping alla classe contesto di database.</span><span class="sxs-lookup"><span data-stu-id="0c975-139">Add model-to-database mapping code to the database context class.</span></span>
- <span data-ttu-id="0c975-140">Modifica `InstructorID` e `StudentID` riferimenti nel corso del progetto per `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="0c975-140">Change `InstructorID` and `StudentID` references throughout the project to `PersonID`.</span></span>

## <a name="creating-the-person-class"></a><span data-ttu-id="0c975-141">Creazione della classe di persona</span><span class="sxs-lookup"><span data-stu-id="0c975-141">Creating the Person Class</span></span>

 <span data-ttu-id="0c975-142">Nota: Non sarà in grado di compilare il progetto dopo aver creato le classi seguenti finché non si aggiorna il controller che utilizza queste classi.</span><span class="sxs-lookup"><span data-stu-id="0c975-142">Note: You won't be able to compile the project after creating the classes below until you update the controllers that uses these classes.</span></span> 

<span data-ttu-id="0c975-143">Nel *modelli* cartella, creare *Person* e sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0c975-143">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="0c975-144">In *Instructor.cs*, derivare la `Instructor` classe il `Person` classe e rimuovere i campi chiavi e il nome.</span><span class="sxs-lookup"><span data-stu-id="0c975-144">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="0c975-145">Il codice sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0c975-145">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="0c975-146">Apportare modifiche analoghe a *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="0c975-146">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="0c975-147">La `Student` classe sarà analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="0c975-147">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a><span data-ttu-id="0c975-148">Aggiungere il tipo di entità utente al modello</span><span class="sxs-lookup"><span data-stu-id="0c975-148">Adding the Person Entity Type to the Model</span></span>

<span data-ttu-id="0c975-149">In *SchoolContext.cs*, aggiungere un `DbSet` proprietà per il `Person` tipo di entità:</span><span class="sxs-lookup"><span data-stu-id="0c975-149">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="0c975-150">L'ereditarietà tabella per gerarchia in Entity Framework è stata configurata.</span><span class="sxs-lookup"><span data-stu-id="0c975-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="0c975-151">Come si vedrà, quando il database viene ricreato, avrà un `Person` tabella anziché il `Student` e `Instructor` tabelle.</span><span class="sxs-lookup"><span data-stu-id="0c975-151">As you'll see, when the database is re-created, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="changing-instructorid-and-studentid-to-personid"></a><span data-ttu-id="0c975-152">Modifica InstructorID e StudentID a PersonID</span><span class="sxs-lookup"><span data-stu-id="0c975-152">Changing InstructorID and StudentID to PersonID</span></span>

<span data-ttu-id="0c975-153">In *SchoolContext.cs*, nell'istruzione mapping istruttore corso modificare `MapRightKey("InstructorID")` a `MapRightKey("PersonID")`:</span><span class="sxs-lookup"><span data-stu-id="0c975-153">In *SchoolContext.cs*, in the Instructor-Course mapping statement, change `MapRightKey("InstructorID")` to `MapRightKey("PersonID")`:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

<span data-ttu-id="0c975-154">Questa modifica non è obbligatoria. Cambia semplicemente il nome della colonna InstructorID nella tabella di join molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="0c975-154">This change isn't required; it just changes the name of the InstructorID column in the many-to-many join table.</span></span> <span data-ttu-id="0c975-155">Se si lascia il nome come InstructorID, l'applicazione potrebbe funzionare correttamente.</span><span class="sxs-lookup"><span data-stu-id="0c975-155">If you left the name as InstructorID, the application would still work correctly.</span></span> <span data-ttu-id="0c975-156">Di seguito è completato *SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="0c975-156">Here is the completed *SchoolContext.cs*:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

<span data-ttu-id="0c975-157">Successivamente è necessario modificare `InstructorID` a `PersonID` e `StudentID` a `PersonID` nel corso del progetto ***tranne*** nei file timestamp migrazioni nel *migrazioni* cartella.</span><span class="sxs-lookup"><span data-stu-id="0c975-157">Next you need to change `InstructorID` to `PersonID` and `StudentID` to `PersonID` throughout the project ***except*** in the time-stamped migrations files in the *Migrations* folder.</span></span> <span data-ttu-id="0c975-158">A tale scopo si sarà trovare e aprire solo i file che devono essere modificati, quindi eseguire una modifica globale per i file aperti.</span><span class="sxs-lookup"><span data-stu-id="0c975-158">To do that you'll find and open only the files that need to be changed, then perform a global change on the opened files.</span></span> <span data-ttu-id="0c975-159">L'unico file la *migrazioni* cartella è consigliabile modificare *Migrations\Configuration.cs.*</span><span class="sxs-lookup"><span data-stu-id="0c975-159">The only file in the *Migrations* folder you should change is *Migrations\Configuration.cs.*</span></span>

1. > [!IMPORTANT]
   > <span data-ttu-id="0c975-160">Iniziare con la chiusura di tutti i file aperti in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0c975-160">Begin by closing all the open files in Visual Studio.</span></span>
2. <span data-ttu-id="0c975-161">Fare clic su **Trova e Sostituisci - trovare tutti i file** nel **modifica** menu e quindi eseguire una ricerca per tutti i file di progetto contenenti `InstructorID`.</span><span class="sxs-lookup"><span data-stu-id="0c975-161">Click **Find and Replace -- Find all Files** in the **Edit** menu, and then search for all files in the project that contain `InstructorID`.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. <span data-ttu-id="0c975-162">Aprire ogni file nel **risultati ricerca** finestra ***tranne*** il &lt;timestamp&gt;*\_cs* i file di migrazione nel *Migrazioni* cartella, facendo doppio clic su una riga per ogni file.</span><span class="sxs-lookup"><span data-stu-id="0c975-162">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. <span data-ttu-id="0c975-163">Aprire il **Sostituisci nei file** finestra di dialogo e modificare **Cerca in** a **tutti i documenti aperti**.</span><span class="sxs-lookup"><span data-stu-id="0c975-163">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
5. <span data-ttu-id="0c975-164">Usare la **Sostituisci nei file** finestra di dialogo per modificare tutte `InstructorID` a `PersonID.`</span><span class="sxs-lookup"><span data-stu-id="0c975-164">Use the **Replace in Files** dialog to change all `InstructorID` to `PersonID.`</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. <span data-ttu-id="0c975-165">Trovare tutti i file nel progetto che contengono `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="0c975-165">Find all the files in the project that contain `StudentID`.</span></span>
7. <span data-ttu-id="0c975-166">Aprire ogni file nel **risultati ricerca** finestra ***tranne*** il &lt;timestamp&gt;*\_\*cs* i file di migrazione nel *migrazioni* cartella, facendo doppio clic su una riga per ogni file.</span><span class="sxs-lookup"><span data-stu-id="0c975-166">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_\*.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. <span data-ttu-id="0c975-167">Aprire il **Sostituisci nei file** finestra di dialogo e modificare **Cerca in** a **tutti i documenti aperti**.</span><span class="sxs-lookup"><span data-stu-id="0c975-167">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
9. <span data-ttu-id="0c975-168">Utilizzare il **Sostituisci nei file** finestra di dialogo per modificare tutte `StudentID` a `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="0c975-168">Use the **Replace in Files** dialog to change all `StudentID` to `PersonID`.</span></span>   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. <span data-ttu-id="0c975-169">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="0c975-169">Build the project.</span></span>

<span data-ttu-id="0c975-170">(Si noti che nell'esempio viene illustrato un *svantaggio* del `classnameID` modello per la denominazione delle chiavi primarie.</span><span class="sxs-lookup"><span data-stu-id="0c975-170">(Note that this demonstrates a *disadvantage* of the `classnameID` pattern for naming primary keys.</span></span> <span data-ttu-id="0c975-171">Se è stato denominato ID di chiavi primarie senza un prefisso, il nome della classe *non* ridenominazione sarebbe necessaria a questo punto.)</span><span class="sxs-lookup"><span data-stu-id="0c975-171">If you had named primary keys ID without prefixing the class name, *no* renaming would be necessary now.)</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="0c975-172">Creare e aggiornare un File di migrazione</span><span class="sxs-lookup"><span data-stu-id="0c975-172">Create and Update a Migrations File</span></span>

<span data-ttu-id="0c975-173">In Package Manager Console (PMC), immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0c975-173">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="0c975-174">Eseguire il `Update-Database` comando PMC.</span><span class="sxs-lookup"><span data-stu-id="0c975-174">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="0c975-175">Il comando avrà esito negativo a questo punto perché abbiamo migrazioni non sa come gestire i dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="0c975-175">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="0c975-176">Viene visualizzato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="0c975-176">You get the following error:</span></span>

<span data-ttu-id="0c975-177">*L'istruzione ALTER TABLE è in conflitto con il vincolo FOREIGN KEY "si intende\_dbo. Reparto\_dbo. Persona\_PersonID ". Il conflitto si è verificato nella tabella del database "ContosoUniversity", "dbo. Persona", colonna 'PersonID'.*</span><span class="sxs-lookup"><span data-stu-id="0c975-177">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Department\_dbo.Person\_PersonID". The conflict occurred in database "ContosoUniversity", table "dbo.Person", column 'PersonID'.*</span></span>

<span data-ttu-id="0c975-178">Aprire *migrazioni\&lt; timestamp&gt;\_Inheritance.cs* e sostituire il `Up` (metodo) con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0c975-178">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

<span data-ttu-id="0c975-179">Eseguire il `update-database` nuovo il comando.</span><span class="sxs-lookup"><span data-stu-id="0c975-179">Run the `update-database` command again.</span></span>

> [!NOTE]
> <span data-ttu-id="0c975-180">È possibile ottenere altri errori durante la migrazione di dati e apportare le modifiche dello schema.</span><span class="sxs-lookup"><span data-stu-id="0c975-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="0c975-181">Se si verificano errori di migrazione non è possibile risolvere, è possibile continuare con l'esercitazione modificando la stringa di connessione nel *Web. config* file o l'eliminazione del database.</span><span class="sxs-lookup"><span data-stu-id="0c975-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or deleting the database.</span></span> <span data-ttu-id="0c975-182">L'approccio più semplice consiste nel rinominare il database di *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="0c975-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="0c975-183">Ad esempio, modificare il nome del database in CU\_testare, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0c975-183">For example, change the database name to CU\_test as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> <span data-ttu-id="0c975-184">Con un nuovo database, non sono presenti dati per eseguire la migrazione e `update-database` comando è molto più probabile completata senza errori.</span><span class="sxs-lookup"><span data-stu-id="0c975-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="0c975-185">Per istruzioni su come eliminare il database, vedere [come eliminare un Database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="0c975-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="0c975-186">Se si adotta questo approccio per continuare l'esercitazione, ignorare il passaggio di distribuzione alla fine di questa esercitazione, poiché il sito distribuito potrebbe ottenere lo stesso errore durante l'esecuzione di migrazioni automaticamente.</span><span class="sxs-lookup"><span data-stu-id="0c975-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial, since the deployed site would get the same error when it runs migrations automatically.</span></span> <span data-ttu-id="0c975-187">Se si desidera risolvere un errore di migrazioni, la risorsa migliore è uno dei forum di Entity Framework o di StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="0c975-187">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="0c975-188">Test</span><span class="sxs-lookup"><span data-stu-id="0c975-188">Testing</span></span>

<span data-ttu-id="0c975-189">Aprire il sito e provare diverse pagine.</span><span class="sxs-lookup"><span data-stu-id="0c975-189">Run the site and try various pages.</span></span> <span data-ttu-id="0c975-190">Tutto funziona come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="0c975-190">Everything works the same as it did before.</span></span>

<span data-ttu-id="0c975-191">In **Esplora Server,** espandere **SchoolContext** e quindi **tabelle**, noterai che il **Student** e **Instructor**  tabelle sono state sostituite da un **persona** tabella.</span><span class="sxs-lookup"><span data-stu-id="0c975-191">In **Server Explorer,** expand **SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="0c975-192">Espandere il **persona** tabella e verificare che dispone di tutte le colonne utilizzate da tutti i **studente** e **istruttore** tabelle.</span><span class="sxs-lookup"><span data-stu-id="0c975-192">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="0c975-194">Fare clic con il pulsante destro del mouse sulla tabella Person e quindi su **Mostra dati tabella** per vedere la colonna discriminante.</span><span class="sxs-lookup"><span data-stu-id="0c975-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="0c975-195">Nel diagramma seguente viene illustrata la struttura del nuovo database School:</span><span class="sxs-lookup"><span data-stu-id="0c975-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a><span data-ttu-id="0c975-197">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="0c975-197">Summary</span></span>

<span data-ttu-id="0c975-198">Ereditarietà tabella per gerarchia è stato implementato per il `Person`, `Student`, e `Instructor` classi.</span><span class="sxs-lookup"><span data-stu-id="0c975-198">Table-per-hierarchy inheritance has now been implemented for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="0c975-199">Per ulteriori informazioni su questa e altre strutture di ereditarietà, vedere [strategie di Mapping di ereditarietà](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) sul blog di Morteza Manavi.</span><span class="sxs-lookup"><span data-stu-id="0c975-199">For more information about this and other inheritance structures, see [Inheritance Mapping Strategies](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) on Morteza Manavi's blog.</span></span> <span data-ttu-id="0c975-200">Nella prossima esercitazione verrà visualizzato alcuni modi per implementare il repository e l'unità di lavoro modelli.</span><span class="sxs-lookup"><span data-stu-id="0c975-200">In the next tutorial you'll see some ways to implement the repository and unit of work patterns.</span></span>

<span data-ttu-id="0c975-201">Collegamenti ad altre risorse di Entity Framework, vedere il [mappa del contenuto ASP.NET dati accesso](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="0c975-201">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0c975-202">[Precedente](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="0c975-202">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span></span>
