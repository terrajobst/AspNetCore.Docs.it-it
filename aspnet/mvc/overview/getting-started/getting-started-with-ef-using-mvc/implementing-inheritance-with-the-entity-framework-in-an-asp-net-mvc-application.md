---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Modello: Implementazione dell'ereditarietà con Entity Framework in un'App ASP.NET MVC 5"
description: In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 79513edce7ac3044f6f547149400cba7d307edfa
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889895"
---
# <a name="template-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a><span data-ttu-id="89f52-103">Modello: Implementazione dell'ereditarietà con Entity Framework in un'app ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="89f52-103">Template: Implement Inheritance with EF in an ASP.NET MVC 5 app</span></span>

<span data-ttu-id="89f52-104">Nell'esercitazione precedente si gestite le eccezioni di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="89f52-104">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="89f52-105">In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="89f52-105">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="89f52-106">Nella programmazione orientata agli oggetti, è possibile usare [ereditarietà](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) per facilitare [riutilizzo del codice](http://en.wikipedia.org/wiki/Code_reuse).</span><span class="sxs-lookup"><span data-stu-id="89f52-106">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="89f52-107">In questa esercitazione verranno modificate le classi `Instructor` e `Student` in modo che derivino da una classe di base `Person` contenente proprietà quali `LastName` comuni a docenti e studenti.</span><span class="sxs-lookup"><span data-stu-id="89f52-107">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="89f52-108">Non verranno aggiunte o modificate pagine Web, ma si modificherà parte del codice e le modifiche verranno automaticamente riflesse nel database.</span><span class="sxs-lookup"><span data-stu-id="89f52-108">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

<span data-ttu-id="89f52-109">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="89f52-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="89f52-110">Informazioni su come eseguire il mapping di ereditarietà al database</span><span class="sxs-lookup"><span data-stu-id="89f52-110">Learn to map inheritance to database</span></span>
> * <span data-ttu-id="89f52-111">Creare la classe Person</span><span class="sxs-lookup"><span data-stu-id="89f52-111">Create the Person class</span></span>
> * <span data-ttu-id="89f52-112">Student e Instructor update</span><span class="sxs-lookup"><span data-stu-id="89f52-112">Update Instructor and Student</span></span>
> * <span data-ttu-id="89f52-113">Aggiungere persone al modello</span><span class="sxs-lookup"><span data-stu-id="89f52-113">Add Person to the Model</span></span>
> * <span data-ttu-id="89f52-114">Creare e aggiornare le migrazioni</span><span class="sxs-lookup"><span data-stu-id="89f52-114">Create and Update Migrations</span></span>
> * <span data-ttu-id="89f52-115">L'implementazione di test</span><span class="sxs-lookup"><span data-stu-id="89f52-115">Test the implementation</span></span>
> * <span data-ttu-id="89f52-116">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="89f52-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89f52-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="89f52-117">Prerequisites</span></span>

* [<span data-ttu-id="89f52-118">Implementazione dell'ereditarietà</span><span class="sxs-lookup"><span data-stu-id="89f52-118">Implementing Inheritance</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a><span data-ttu-id="89f52-119">Eseguire il mapping di ereditarietà al database</span><span class="sxs-lookup"><span data-stu-id="89f52-119">Map inheritance to database</span></span>

<span data-ttu-id="89f52-120">Il `Instructor` e `Student` le classi di `School` modello di dati sono associate diverse proprietà che sono identiche:</span><span class="sxs-lookup"><span data-stu-id="89f52-120">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="89f52-122">Si supponga di voler eliminare il codice ridondante per le proprietà condivise dalle entità `Instructor` e `Student`.</span><span class="sxs-lookup"><span data-stu-id="89f52-122">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="89f52-123">Oppure che si desideri scrivere un servizio in grado di formattare i nomi senza sapere se il nome appartiene a un docente o a uno studente.</span><span class="sxs-lookup"><span data-stu-id="89f52-123">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="89f52-124">È possibile creare un `Person` classe base che contiene solo le proprietà condivise e quindi apportare le `Instructor` e `Student` entità ereditano da tale classe, come illustrato nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="89f52-124">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="89f52-126">Questa struttura di ereditarietà può essere rappresentata nel database in diversi modi.</span><span class="sxs-lookup"><span data-stu-id="89f52-126">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="89f52-127">È possibile avere un `Person` tabella che include informazioni relative a studenti e docenti in un'unica tabella.</span><span class="sxs-lookup"><span data-stu-id="89f52-127">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="89f52-128">Alcune colonne possono riguardare solo instructors (insegnanti) (`HireDate`), altre solo gli studenti (`EnrollmentDate`), alcuni a entrambi (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="89f52-128">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="89f52-129">In genere, è necessario un *discriminatore* colonna per indicare quale tipo di ogni riga rappresenta.</span><span class="sxs-lookup"><span data-stu-id="89f52-129">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="89f52-130">La colonna discriminante può ad esempio indicare "Instructor" per i docenti e "Student" per gli studenti.</span><span class="sxs-lookup"><span data-stu-id="89f52-130">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="89f52-132">Il modello di generazione di una struttura di ereditarietà di entità da una singola tabella di database è definito *tabella per gerarchia* ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="89f52-132">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="89f52-133">Un'alternativa consiste nel rendere il database più simile alla struttura di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="89f52-133">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="89f52-134">Ad esempio, si potrebbero avere solo i campi del nome nel `Person` tabella e sono separati `Instructor` e `Student` tabelle con i campi di Data.</span><span class="sxs-lookup"><span data-stu-id="89f52-134">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="89f52-136">Questo criterio di creazione di una tabella di database per ogni classe di entità è definita *tabella per tipo* ereditarietà (TPT).</span><span class="sxs-lookup"><span data-stu-id="89f52-136">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="89f52-137">Un'altra opzione consiste nell'eseguire il mapping di tutti i tipi non astratti a singole tabelle.</span><span class="sxs-lookup"><span data-stu-id="89f52-137">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="89f52-138">Tutte le proprietà di una classe, incluse le proprietà ereditate, eseguono il mapping alle colonne della tabella corrispondente.</span><span class="sxs-lookup"><span data-stu-id="89f52-138">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="89f52-139">Questo criterio è denominato ereditarietà della classe tabella per tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="89f52-139">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="89f52-140">Se è stata implementata l'ereditarietà TPC per il `Person`, `Student`, e `Instructor` classi come illustrato in precedenza, il `Student` e `Instructor` aspetto alcuna differenza delle tabelle dopo l'implementazione dell'ereditarietà rispetto a quello precedente.</span><span class="sxs-lookup"><span data-stu-id="89f52-140">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="89f52-141">Concreto e criteri di ereditarietà tabella per gerarchia offrono in genere prestazioni migliori in Entity Framework rispetto ai modelli di ereditarietà tabella per tipo, in quanto possono comportare modelli TPT query join complesse.</span><span class="sxs-lookup"><span data-stu-id="89f52-141">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="89f52-142">Questa esercitazione illustra come implementare l'ereditarietà tabella per gerarchia.</span><span class="sxs-lookup"><span data-stu-id="89f52-142">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="89f52-143">Tabella per gerarchia è il modello di ereditarietà predefinita in Entity Framework, in modo che tutto è necessario eseguire è creare un `Person` classe, modificare il `Instructor` e `Student` classi di derivare da `Person`, aggiungere la nuova classe il `DbContext`e creare un migrazione.</span><span class="sxs-lookup"><span data-stu-id="89f52-143">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="89f52-144">(Per informazioni su come implementare altri modelli di ereditarietà, vedere [Mapping dell'ereditarietà tabella Per tipo TPT ()](https://msdn.microsoft.com/data/jj591617#2.5) e [Mapping dell'ereditarietà tabella Per tipo concreto classe (TP)](https://msdn.microsoft.com/data/jj591617#2.6) in MSDN Documentazione di Entity Framework.)</span><span class="sxs-lookup"><span data-stu-id="89f52-144">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="89f52-145">Creare la classe Person</span><span class="sxs-lookup"><span data-stu-id="89f52-145">Create the Person class</span></span>

<span data-ttu-id="89f52-146">Nel *modelli* cartella, creare *Person.cs* e sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="89f52-146">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a><span data-ttu-id="89f52-147">Student e Instructor update</span><span class="sxs-lookup"><span data-stu-id="89f52-147">Update Instructor and Student</span></span>

<span data-ttu-id="89f52-148">A questo punto aggiornare i *Instructor.cs* e *Sudent.cs* ereditare valori dal *Person.sc*.</span><span class="sxs-lookup"><span data-stu-id="89f52-148">Now update the *Instructor.cs* and *Sudent.cs* to inherit values from the *Person.sc*.</span></span>

<span data-ttu-id="89f52-149">Nella *Instructor.cs*, derivare le `Instructor` classe il `Person` classe e rimuovere i campi chiavi e nome.</span><span class="sxs-lookup"><span data-stu-id="89f52-149">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="89f52-150">Il codice sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="89f52-150">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="89f52-151">Apportare modifiche analoghe a *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="89f52-151">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="89f52-152">Il `Student` classe avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="89f52-152">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a><span data-ttu-id="89f52-153">Aggiungere persone al modello</span><span class="sxs-lookup"><span data-stu-id="89f52-153">Add Person to the Model</span></span>

<span data-ttu-id="89f52-154">Nelle *SchoolContext.cs*, aggiungere un `DbSet` proprietà per il `Person` tipo di entità:</span><span class="sxs-lookup"><span data-stu-id="89f52-154">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="89f52-155">L'ereditarietà tabella per gerarchia in Entity Framework è stata configurata.</span><span class="sxs-lookup"><span data-stu-id="89f52-155">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="89f52-156">Come si vedrà, quando viene aggiornato il database, avrà un `Person` tabella anziché il `Student` e `Instructor` tabelle.</span><span class="sxs-lookup"><span data-stu-id="89f52-156">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-migrations"></a><span data-ttu-id="89f52-157">Creare e aggiornare le migrazioni</span><span class="sxs-lookup"><span data-stu-id="89f52-157">Create and Update Migrations</span></span>

<span data-ttu-id="89f52-158">In Package Manager Console (PMC), immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="89f52-158">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="89f52-159">Eseguire il `Update-Database` comando nella console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="89f52-159">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="89f52-160">Il comando avrà esito negativo a questo punto perché abbiamo i dati esistenti che le migrazioni non sa come gestire.</span><span class="sxs-lookup"><span data-stu-id="89f52-160">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="89f52-161">Viene visualizzato un messaggio di errore simile a quello seguente:</span><span class="sxs-lookup"><span data-stu-id="89f52-161">You get an error message like the following one:</span></span>

> <span data-ttu-id="89f52-162">*Impossibile eliminare l'oggetto ' dbo. Instructor' perché vi fanno riferimento tramite un vincolo FOREIGN KEY.*</span><span class="sxs-lookup"><span data-stu-id="89f52-162">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>


<span data-ttu-id="89f52-163">Aprire *migrazioni\&lt; timestamp&gt;\_Inheritance.cs* e sostituire il `Up` metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="89f52-163">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="89f52-164">Questo codice esegue le attività di aggiornamento del database seguenti:</span><span class="sxs-lookup"><span data-stu-id="89f52-164">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="89f52-165">Rimuove i vincoli di chiave esterna e gli indici che puntano alla tabella Student.</span><span class="sxs-lookup"><span data-stu-id="89f52-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="89f52-166">Rinomina la tabella Instructor in Person e apporta le modifiche necessarie perché sia in grado di archiviare i dati Student:</span><span class="sxs-lookup"><span data-stu-id="89f52-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="89f52-167">Aggiunge un elemento EnrollmentDate che ammette i valori Null per gli studenti.</span><span class="sxs-lookup"><span data-stu-id="89f52-167">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="89f52-168">Aggiunge una colonna discriminante che indica se una riga è per uno studente o un docente.</span><span class="sxs-lookup"><span data-stu-id="89f52-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="89f52-169">Modifica HireDate in modo che ammetta i valori Null poiché le righe degli studenti non includono date di assunzione.</span><span class="sxs-lookup"><span data-stu-id="89f52-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="89f52-170">Aggiunge un campo temporaneo che sarà usato per aggiornare le chiavi esterne che puntano agli studenti.</span><span class="sxs-lookup"><span data-stu-id="89f52-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="89f52-171">Quando si copiano studenti nella tabella Person otterranno nuovi valori di chiave primari.</span><span class="sxs-lookup"><span data-stu-id="89f52-171">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="89f52-172">Copia i dati dalla tabella Student alla tabella Person.</span><span class="sxs-lookup"><span data-stu-id="89f52-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="89f52-173">Ciò comporta l'assegnazione di nuovi valori di chiave primaria agli studenti.</span><span class="sxs-lookup"><span data-stu-id="89f52-173">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="89f52-174">Corregge i valori di chiave esterna che puntano agli studenti.</span><span class="sxs-lookup"><span data-stu-id="89f52-174">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="89f52-175">Ricrea i vincoli di chiave esterna e gli indici che ora puntano alla tabella Person.</span><span class="sxs-lookup"><span data-stu-id="89f52-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="89f52-176">Se come tipo di chiave primaria fosse stato usato GUID anziché Integer, non sarebbe necessario modificare i valori di chiave primaria degli studenti e molti di questi passaggi avrebbero potuto essere omessi.</span><span class="sxs-lookup"><span data-stu-id="89f52-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="89f52-177">Eseguire il `update-database` nuovo il comando.</span><span class="sxs-lookup"><span data-stu-id="89f52-177">Run the `update-database` command again.</span></span>

<span data-ttu-id="89f52-178">(In un sistema di produzione è verrebbero apportate modifiche corrispondenti al metodo verso il basso nel caso in cui fosse necessario usarlo per tornare alla versione precedente del database.</span><span class="sxs-lookup"><span data-stu-id="89f52-178">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="89f52-179">Per questa esercitazione non utilizzeremo il metodo verso il basso.)</span><span class="sxs-lookup"><span data-stu-id="89f52-179">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="89f52-180">È possibile ottenere altri errori durante la migrazione dei dati e apportare le modifiche dello schema.</span><span class="sxs-lookup"><span data-stu-id="89f52-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="89f52-181">Se si verificano errori di migrazione non è possibile risolvere, è possibile continuare con l'esercitazione modificando la stringa di connessione nel *Web. config* file o eliminando il database.</span><span class="sxs-lookup"><span data-stu-id="89f52-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="89f52-182">L'approccio più semplice consiste nel rinominare il database di *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="89f52-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="89f52-183">Ad esempio, modificare il nome del database in ContosoUniversity2 come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="89f52-183">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> <span data-ttu-id="89f52-184">Con un nuovo database, non sono presenti dati per eseguire la migrazione e il `update-database` comando è molto probabile che venga completato senza errori.</span><span class="sxs-lookup"><span data-stu-id="89f52-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="89f52-185">Per istruzioni su come eliminare il database, vedere [come eliminare un Database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="89f52-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="89f52-186">Se si adotta questo approccio per poter continuare con l'esercitazione, ignorare il passaggio di distribuzione alla fine di questa esercitazione o la distribuzione in un nuovo sito e del database.</span><span class="sxs-lookup"><span data-stu-id="89f52-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="89f52-187">Se si distribuisce un aggiornamento allo stesso sito di che cui è stato distribuito già, EF otterrà lo stesso errore quando viene eseguita automaticamente le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="89f52-187">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="89f52-188">Se si desidera risolvere un errore di migrazioni, la risorsa migliore è uno dei forum di Entity Framework o StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="89f52-188">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>

## <a name="test-the-implementation"></a><span data-ttu-id="89f52-189">L'implementazione di test</span><span class="sxs-lookup"><span data-stu-id="89f52-189">Test the implementation</span></span>

<span data-ttu-id="89f52-190">Esecuzione del sito e provare diverse pagine.</span><span class="sxs-lookup"><span data-stu-id="89f52-190">Run the site and try various pages.</span></span> <span data-ttu-id="89f52-191">Tutto funziona come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="89f52-191">Everything works the same as it did before.</span></span>

<span data-ttu-id="89f52-192">In **Esplora Server** espandere **dati Connections\SchoolContext** e quindi **tabelle**, e si può osservare che il **studente** e **Insegnante** le tabelle sono state sostituite da un **persona** tabella.</span><span class="sxs-lookup"><span data-stu-id="89f52-192">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="89f52-193">Espandere la **Person** tabella e verificare che dispone di tutte le colonne che erano nel **studente** e **insegnante** tabelle.</span><span class="sxs-lookup"><span data-stu-id="89f52-193">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

<span data-ttu-id="89f52-194">Fare clic con il pulsante destro del mouse sulla tabella Person e quindi su **Mostra dati tabella** per vedere la colonna discriminante.</span><span class="sxs-lookup"><span data-stu-id="89f52-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

<span data-ttu-id="89f52-195">Il diagramma seguente illustra la struttura del nuovo database School:</span><span class="sxs-lookup"><span data-stu-id="89f52-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="89f52-197">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="89f52-197">Deploy to Azure</span></span>

<span data-ttu-id="89f52-198">In questa sezione è necessario aver completato l'opzione facoltativa **la distribuzione dell'app in Azure** sezione [parte 3, l'ordinamento, filtro e Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) di questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="89f52-198">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="89f52-199">Se si sono verificati errori di migrazione che è stato risolto, eliminare il database nel progetto locale, ignorare questo passaggio. o creare un nuovo sito e i database e distribuire nel nuovo ambiente.</span><span class="sxs-lookup"><span data-stu-id="89f52-199">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="89f52-200">In Visual Studio, fare clic sul progetto in **Esplora soluzioni** e selezionare **Publish** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="89f52-200">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>

2. <span data-ttu-id="89f52-201">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="89f52-201">Click **Publish**.</span></span>

    <span data-ttu-id="89f52-202">Consente di aprire l'app Web nel browser predefinito.</span><span class="sxs-lookup"><span data-stu-id="89f52-202">The Web app opens in your default browser.</span></span>

3. <span data-ttu-id="89f52-203">Testare l'applicazione per verificare funzioni.</span><span class="sxs-lookup"><span data-stu-id="89f52-203">Test the application to verify it's working.</span></span>

    <span data-ttu-id="89f52-204">Alla prima esecuzione di una pagina che accede al database, Entity Framework esegue tutte le migrazioni `Up` metodi necessari per portare il database aggiornato con il modello di dati corrente.</span><span class="sxs-lookup"><span data-stu-id="89f52-204">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="89f52-205">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="89f52-205">Get the code</span></span>

[<span data-ttu-id="89f52-206">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="89f52-206">Download Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="89f52-207">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="89f52-207">Additional resources</span></span>

<span data-ttu-id="89f52-208">Sono disponibili collegamenti ad altre risorse di Entity Framework nel [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="89f52-208">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="89f52-209">Per altre informazioni su questa e altre strutture di ereditarietà, vedere [modello di ereditarietà TPT](https://msdn.microsoft.com/data/jj618293) e [modello di ereditarietà tabella per gerarchia](https://msdn.microsoft.com/data/jj618292) su MSDN.</span><span class="sxs-lookup"><span data-stu-id="89f52-209">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="89f52-210">Nella prossima esercitazione si apprenderà come gestire diversi scenari Entity Framework relativamente avanzati.</span><span class="sxs-lookup"><span data-stu-id="89f52-210">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="89f52-211">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="89f52-211">Next steps</span></span>

<span data-ttu-id="89f52-212">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="89f52-212">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="89f52-213">Stato descritto come eseguire il mapping di ereditarietà al database</span><span class="sxs-lookup"><span data-stu-id="89f52-213">Learned to map inheritance to database</span></span>
> * <span data-ttu-id="89f52-214">Creata la classe Person</span><span class="sxs-lookup"><span data-stu-id="89f52-214">Created the Person class</span></span>
> * <span data-ttu-id="89f52-215">Student e Instructor aggiornato</span><span class="sxs-lookup"><span data-stu-id="89f52-215">Updated Instructor and Student</span></span>
> * <span data-ttu-id="89f52-216">Persona aggiunta al modello</span><span class="sxs-lookup"><span data-stu-id="89f52-216">Added Person to the Model</span></span>
> * <span data-ttu-id="89f52-217">Creazione e aggiornare le migrazioni</span><span class="sxs-lookup"><span data-stu-id="89f52-217">Created and Update Migrations</span></span>
> * <span data-ttu-id="89f52-218">Testare l'implementazione</span><span class="sxs-lookup"><span data-stu-id="89f52-218">Tested the implementation</span></span>
> * <span data-ttu-id="89f52-219">Distribuito in Azure</span><span class="sxs-lookup"><span data-stu-id="89f52-219">Deployed to Azure</span></span>

<span data-ttu-id="89f52-220">Passare all'articolo successivo per informazioni su argomenti che è utile da tenere presente dopo aver appreso le nozioni di base dello sviluppo di applicazioni web ASP.NET che usano Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="89f52-220">Advance to the next article to learn about topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="89f52-221">Scenari avanzati di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="89f52-221">Advanced Entity Framework Scenarios</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)