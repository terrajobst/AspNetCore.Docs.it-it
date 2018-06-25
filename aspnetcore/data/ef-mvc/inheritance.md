---
title: ASP.NET Core MVC con EF Core - Ereditarietà - 9 di 10
author: rick-anderson
description: Questa esercitazione illustra come implementare l'ereditarietà nel modello di dati usando Entity Framework Core in un'applicazione ASP.NET Core.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 818af711c23d37810b29eda8915b3c195a3e48f8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272854"
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a><span data-ttu-id="e840f-103">ASP.NET Core MVC con EF Core - Ereditarietà - 9 di 10</span><span class="sxs-lookup"><span data-stu-id="e840f-103">ASP.NET Core MVC with EF Core - Inheritance - 9 of 10</span></span>

<span data-ttu-id="e840f-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e840f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e840f-105">L'applicazione Web di esempio Contoso University illustra come creare applicazioni Web ASP.NET Core MVC con Entity Framework Core e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e840f-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="e840f-106">Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="e840f-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="e840f-107">Nell'esercitazione precedente sono state presentate le eccezioni di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="e840f-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="e840f-108">In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="e840f-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="e840f-109">Nella programmazione orientata a oggetti è possibile usare l'ereditarietà per facilitare il riutilizzo del codice.</span><span class="sxs-lookup"><span data-stu-id="e840f-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="e840f-110">In questa esercitazione verranno modificate le classi `Instructor` e `Student` in modo che derivino da una classe di base `Person` contenente proprietà quali `LastName` comuni a docenti e studenti.</span><span class="sxs-lookup"><span data-stu-id="e840f-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="e840f-111">Non verranno aggiunte o modificate pagine Web, ma si modificherà parte del codice e le modifiche verranno automaticamente riflesse nel database.</span><span class="sxs-lookup"><span data-stu-id="e840f-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="e840f-112">Opzioni per il mapping dell'ereditarietà alle tabelle di database</span><span class="sxs-lookup"><span data-stu-id="e840f-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="e840f-113">Le classi `Instructor` e `Student` nel modello di dati School presentano molte proprietà identiche:</span><span class="sxs-lookup"><span data-stu-id="e840f-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Classi Student e Instructor](inheritance/_static/no-inheritance.png)

<span data-ttu-id="e840f-115">Si supponga di voler eliminare il codice ridondante per le proprietà condivise dalle entità `Instructor` e `Student`.</span><span class="sxs-lookup"><span data-stu-id="e840f-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="e840f-116">Oppure che si desideri scrivere un servizio in grado di formattare i nomi senza sapere se il nome appartiene a un docente o a uno studente.</span><span class="sxs-lookup"><span data-stu-id="e840f-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="e840f-117">È possibile creare una classe di base `Person` che contiene solo le proprietà condivise e quindi fare in modo che le classi `Instructor` e `Student` ereditino da questa classe di base, come illustrato nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="e840f-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Classi Student e Instructor derivanti dalla classe Person](inheritance/_static/inheritance.png)

<span data-ttu-id="e840f-119">Questa struttura di ereditarietà può essere rappresentata nel database in diversi modi.</span><span class="sxs-lookup"><span data-stu-id="e840f-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="e840f-120">È possibile usare una tabella Person che includa le informazioni relative a studenti e docenti in un'unica tabella.</span><span class="sxs-lookup"><span data-stu-id="e840f-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="e840f-121">Alcune colonne possono riguardare solo i docenti (HireDate), altre solo gli studenti (EnrollmentDate) e altre ancora entrambi (LastName, FirstName).</span><span class="sxs-lookup"><span data-stu-id="e840f-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="e840f-122">Per indicare il tipo rappresentato da ogni riga viene in genere usata una colonna discriminante.</span><span class="sxs-lookup"><span data-stu-id="e840f-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="e840f-123">La colonna discriminante può ad esempio indicare "Instructor" per i docenti e "Student" per gli studenti.</span><span class="sxs-lookup"><span data-stu-id="e840f-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Esempio di tabella per gerarchia](inheritance/_static/tph.png)

<span data-ttu-id="e840f-125">Questo criterio di generazione di una struttura di ereditarietà delle entità da una singola tabella di database è denominato ereditarietà tabella per gerarchia.</span><span class="sxs-lookup"><span data-stu-id="e840f-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="e840f-126">Un'alternativa consiste nel rendere il database più simile alla struttura di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="e840f-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="e840f-127">È possibile, ad esempio, includere nella tabella Person solo i campi del nome e usare tabelle Instructor e Student separate con i campi della data.</span><span class="sxs-lookup"><span data-stu-id="e840f-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Ereditarietà tabella per tipo](inheritance/_static/tpt.png)

<span data-ttu-id="e840f-129">Questo criterio di creazione di una tabella di database per ogni classe di entità è denominato ereditarietà tabella per tipo.</span><span class="sxs-lookup"><span data-stu-id="e840f-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="e840f-130">Un'altra opzione consiste nell'eseguire il mapping di tutti i tipi non astratti a singole tabelle.</span><span class="sxs-lookup"><span data-stu-id="e840f-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="e840f-131">Tutte le proprietà di una classe, incluse le proprietà ereditate, eseguono il mapping alle colonne della tabella corrispondente.</span><span class="sxs-lookup"><span data-stu-id="e840f-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="e840f-132">Questo criterio è denominato ereditarietà della classe tabella per tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="e840f-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="e840f-133">Se per le classi Person, Student e Instructor è stata implementata l'ereditarietà tabella per tipo concreto come illustrato in precedenza, l'aspetto delle tabelle Student e Instructor dopo l'implementazione dell'ereditarietà non sarà diverso da quello precedente.</span><span class="sxs-lookup"><span data-stu-id="e840f-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="e840f-134">I criteri tabella per tipo concreto e tabella per gerarchia offrono in genere prestazioni migliori rispetto ai criteri tabella per tipo perché questi ultimi possono generare query join complesse.</span><span class="sxs-lookup"><span data-stu-id="e840f-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="e840f-135">Questa esercitazione illustra come implementare l'ereditarietà tabella per gerarchia.</span><span class="sxs-lookup"><span data-stu-id="e840f-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="e840f-136">Il criterio di ereditarietà tabella per gerarchia è l'unico supportato da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e840f-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="e840f-137">Verranno eseguite le operazioni di creazione di una classe `Person`, modifica delle classi `Instructor` e `Student` in modo che derivino da `Person`, aggiunta della nuova classe a `DbContext` e creazione di una migrazione.</span><span class="sxs-lookup"><span data-stu-id="e840f-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="e840f-138">È consigliabile salvare una copia del progetto prima di apportare le modifiche seguenti.</span><span class="sxs-lookup"><span data-stu-id="e840f-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="e840f-139">In caso di problemi e se fosse necessario ricominciare da capo, sarà più facile iniziare dal progetto salvato invece di annullare i passaggi eseguiti per questa esercitazione o tornare all'inizio dell'intera serie.</span><span class="sxs-lookup"><span data-stu-id="e840f-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="e840f-140">Creare la classe Person</span><span class="sxs-lookup"><span data-stu-id="e840f-140">Create the Person class</span></span>

<span data-ttu-id="e840f-141">Nella cartella Models creare Person.cs e sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e840f-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="e840f-142">Modificare le classi Student e Instructor in modo da stabilire l'ereditarietà da Person</span><span class="sxs-lookup"><span data-stu-id="e840f-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="e840f-143">In *Instructor.cs* derivare la classe Instructor dalla classe Person e rimuovere i campi chiave e nome.</span><span class="sxs-lookup"><span data-stu-id="e840f-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="e840f-144">Il codice sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e840f-144">The code will look like the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="e840f-145">Apportare le stesse modifiche in *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="e840f-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="e840f-146">Aggiungere il tipo di entità Person al modello di dati</span><span class="sxs-lookup"><span data-stu-id="e840f-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="e840f-147">Aggiungere il tipo di entità Person a *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="e840f-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="e840f-148">Le nuove righe sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="e840f-148">The new lines are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="e840f-149">L'ereditarietà tabella per gerarchia in Entity Framework è stata configurata.</span><span class="sxs-lookup"><span data-stu-id="e840f-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="e840f-150">Come si vedrà, quando il database verrà aggiornato conterrà una tabella Person invece delle tabelle Student e Instructor.</span><span class="sxs-lookup"><span data-stu-id="e840f-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="e840f-151">Creare e personalizzare il codice di migrazione</span><span class="sxs-lookup"><span data-stu-id="e840f-151">Create and customize migration code</span></span>

<span data-ttu-id="e840f-152">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="e840f-152">Save your changes and build the project.</span></span> <span data-ttu-id="e840f-153">Quindi aprire la finestra di comando nella cartella del progetto e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e840f-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="e840f-154">Non eseguire ancora il comando `database update`.</span><span class="sxs-lookup"><span data-stu-id="e840f-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="e840f-155">Questo comando determinerà la perdita di dati poiché eliminerà la tabella Instructor e rinominerà la tabella Student in Person.</span><span class="sxs-lookup"><span data-stu-id="e840f-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="e840f-156">Per mantenere i dati esistenti è necessario specificare codice personalizzato.</span><span class="sxs-lookup"><span data-stu-id="e840f-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="e840f-157">Aprire *Migrations/\<timestamp>_Inheritance.cs* e sostituire il metodo `Up` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e840f-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="e840f-158">Questo codice esegue le attività di aggiornamento del database seguenti:</span><span class="sxs-lookup"><span data-stu-id="e840f-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="e840f-159">Rimuove i vincoli di chiave esterna e gli indici che puntano alla tabella Student.</span><span class="sxs-lookup"><span data-stu-id="e840f-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="e840f-160">Rinomina la tabella Instructor in Person e apporta le modifiche necessarie perché sia in grado di archiviare i dati Student:</span><span class="sxs-lookup"><span data-stu-id="e840f-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="e840f-161">Aggiunge un elemento EnrollmentDate che ammette i valori Null per gli studenti.</span><span class="sxs-lookup"><span data-stu-id="e840f-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="e840f-162">Aggiunge una colonna discriminante che indica se una riga è per uno studente o un docente.</span><span class="sxs-lookup"><span data-stu-id="e840f-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="e840f-163">Modifica HireDate in modo che ammetta i valori Null poiché le righe degli studenti non includono date di assunzione.</span><span class="sxs-lookup"><span data-stu-id="e840f-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="e840f-164">Aggiunge un campo temporaneo che sarà usato per aggiornare le chiavi esterne che puntano agli studenti.</span><span class="sxs-lookup"><span data-stu-id="e840f-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="e840f-165">Quando si copiano studenti nella tabella Person, questi otterranno nuovi valori di chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="e840f-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="e840f-166">Copia i dati dalla tabella Student alla tabella Person.</span><span class="sxs-lookup"><span data-stu-id="e840f-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="e840f-167">Ciò comporta l'assegnazione di nuovi valori di chiave primaria agli studenti.</span><span class="sxs-lookup"><span data-stu-id="e840f-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="e840f-168">Corregge i valori di chiave esterna che puntano agli studenti.</span><span class="sxs-lookup"><span data-stu-id="e840f-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="e840f-169">Ricrea i vincoli di chiave esterna e gli indici che ora puntano alla tabella Person.</span><span class="sxs-lookup"><span data-stu-id="e840f-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="e840f-170">Se come tipo di chiave primaria fosse stato usato GUID anziché Integer, non sarebbe necessario modificare i valori di chiave primaria degli studenti e molti di questi passaggi avrebbero potuto essere omessi.</span><span class="sxs-lookup"><span data-stu-id="e840f-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="e840f-171">Eseguire il comando `database update`:</span><span class="sxs-lookup"><span data-stu-id="e840f-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="e840f-172">In un sistema di produzione verrebbero apportate modifiche corrispondenti al metodo `Down` nel caso fosse necessario usarlo per tornare alla versione precedente del database.</span><span class="sxs-lookup"><span data-stu-id="e840f-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="e840f-173">Per questa esercitazione, il metodo `Down` non verrà usato.</span><span class="sxs-lookup"><span data-stu-id="e840f-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="e840f-174">Quando si apportano modifiche allo schema in un database con dati esistenti è possibile che si riscontrino altri errori.</span><span class="sxs-lookup"><span data-stu-id="e840f-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="e840f-175">Se si verificano errori di migrazione che non si riesce a risolvere, è possibile modificare il nome del database nella stringa di connessione o eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="e840f-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="e840f-176">Un nuovo database non contiene dati di cui eseguire la migrazione e ci sono maggiori probabilità che il comando update-database venga completato senza errori.</span><span class="sxs-lookup"><span data-stu-id="e840f-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="e840f-177">Per eliminare il database, usare SSOX o eseguire il comando dell’interfaccia della riga di comando `database drop`.</span><span class="sxs-lookup"><span data-stu-id="e840f-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="e840f-178">Eseguire il test con l'ereditarietà implementata</span><span class="sxs-lookup"><span data-stu-id="e840f-178">Test with inheritance implemented</span></span>

<span data-ttu-id="e840f-179">Eseguire l'app e provare diverse pagine.</span><span class="sxs-lookup"><span data-stu-id="e840f-179">Run the app and try various pages.</span></span> <span data-ttu-id="e840f-180">Tutto funziona come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e840f-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="e840f-181">In **Esplora oggetti di SQL Server** espandere **Connessioni dati/SchoolContext** e quindi **Tabelle**. Si osserverà che le tabelle Student e Instructor sono state sostituite da una tabella Person.</span><span class="sxs-lookup"><span data-stu-id="e840f-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="e840f-182">Dopo aver aperto la tabella Person in Progettazione tabelle si noterà che contiene tutte le colonne che in precedenza erano presenti nelle tabelle Student e Instructor.</span><span class="sxs-lookup"><span data-stu-id="e840f-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Tabella Person in SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="e840f-184">Fare clic con il pulsante destro del mouse sulla tabella Person e quindi su **Mostra dati tabella** per vedere la colonna discriminante.</span><span class="sxs-lookup"><span data-stu-id="e840f-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Tabella Person in SSOX, dati della tabella](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="e840f-186">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e840f-186">Summary</span></span>

<span data-ttu-id="e840f-187">È stata implementata l'ereditarietà tabella per gerarchia per le classi `Person`, `Student` e `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="e840f-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="e840f-188">Per altre informazioni sull'ereditarietà in Entity Framework Core, vedere [Ereditarietà](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="e840f-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="e840f-189">Nella prossima esercitazione si apprenderà come gestire diversi scenari Entity Framework relativamente avanzati.</span><span class="sxs-lookup"><span data-stu-id="e840f-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e840f-190">[Precedente](concurrency.md)
> [Successivo](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="e840f-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
