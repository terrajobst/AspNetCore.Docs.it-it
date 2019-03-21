---
title: Razor Pages con EF Core in ASP.NET Core - Migrazioni - 4 di 8
author: rick-anderson
description: In questa esercitazione si inizia a usare la funzionalità delle migrazioni EF Core per la gestione delle modifiche al modello di dati in un'app ASP.NET Core MVC.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 5848e5e1e45708c3ab5c2a79614111662701aa77
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320160"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="14580-103">Razor Pages con EF Core in ASP.NET Core - Migrazioni - 4 di 8</span><span class="sxs-lookup"><span data-stu-id="14580-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="14580-104">Di [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="14580-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="14580-105">In questa esercitazione viene usata la funzionalità delle migrazioni EF Core per la gestione delle modifiche al modello di dati.</span><span class="sxs-lookup"><span data-stu-id="14580-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="14580-106">Se si verificano problemi che non si è in grado di risolvere, scaricare l'[app completa](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="14580-106">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

<span data-ttu-id="14580-107">Quando viene sviluppata una nuova app, il modello di dati cambia spesso.</span><span class="sxs-lookup"><span data-stu-id="14580-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="14580-108">Ogni volta che vengono apportate modifiche al modello, questo non è più sincronizzato con il database.</span><span class="sxs-lookup"><span data-stu-id="14580-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="14580-109">L'esercitazione è iniziata con la configurazione di Entity Framework per creare il database se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="14580-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="14580-110">Ogni volta che il modello di dati cambia:</span><span class="sxs-lookup"><span data-stu-id="14580-110">Each time the data model changes:</span></span>

* <span data-ttu-id="14580-111">Il database viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="14580-111">The DB is dropped.</span></span>
* <span data-ttu-id="14580-112">EF ne crea uno nuovo che corrisponde al modello.</span><span class="sxs-lookup"><span data-stu-id="14580-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="14580-113">L'app esegue il seeding del database con dati di test.</span><span class="sxs-lookup"><span data-stu-id="14580-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="14580-114">Questo approccio che consiste nel mantenere il database sincronizzato con il modello di dati funziona bene fino a quando non si distribuisce l'app nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="14580-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="14580-115">Quando l'app è in esecuzione nell'ambiente di produzione, in genere esegue l'archiviazione di dati che devono essere mantenuti.</span><span class="sxs-lookup"><span data-stu-id="14580-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="14580-116">L'app non può essere avviata con un database di test ogni volta che viene apportata una modifica, come ad esempio l'aggiunta di una colonna.</span><span class="sxs-lookup"><span data-stu-id="14580-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="14580-117">La funzionalità delle migrazioni di EF Core risolve questo problema consentendo a EF Core di aggiornare lo schema del database anziché creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="14580-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="14580-118">Anziché eliminare e ricreare il database quando il modello di dati viene modificato, le migrazioni aggiornano lo schema e mantengono i dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="14580-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="drop-the-database"></a><span data-ttu-id="14580-119">Eliminare il database</span><span class="sxs-lookup"><span data-stu-id="14580-119">Drop the database</span></span>

<span data-ttu-id="14580-120">Usare **Esplora oggetti di SQL Server** o il comando `database drop`:</span><span class="sxs-lookup"><span data-stu-id="14580-120">Use **SQL Server Object Explorer** (SSOX) or the `database drop` command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="14580-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14580-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="14580-122">Nella **console di Gestione pacchetti** eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="14580-122">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
```

<span data-ttu-id="14580-123">Eseguire `Get-Help about_EntityFrameworkCore` dalla console di Gestione pacchetti per ottenere informazioni.</span><span class="sxs-lookup"><span data-stu-id="14580-123">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="14580-124">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="14580-124">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="14580-125">Aprire una finestra di comando e passare alla cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="14580-125">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="14580-126">La cartella del progetto contiene il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="14580-126">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="14580-127">Digitare quanto segue nella finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="14580-127">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
 ```

---

## <a name="create-an-initial-migration-and-update-the-db"></a><span data-ttu-id="14580-128">Creare una migrazione iniziale e aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="14580-128">Create an initial migration and update the DB</span></span>

<span data-ttu-id="14580-129">Compilare il progetto e creare la prima migrazione.</span><span class="sxs-lookup"><span data-stu-id="14580-129">Build the project and create the first migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="14580-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14580-130">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="14580-131">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="14580-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="14580-132">Esaminare i metodi Up e Down</span><span class="sxs-lookup"><span data-stu-id="14580-132">Examine the Up and Down methods</span></span>

<span data-ttu-id="14580-133">Il comando `migrations add` di EF Core ha generato codice per la creazione del database.</span><span class="sxs-lookup"><span data-stu-id="14580-133">The EF Core `migrations add` command  generated code to create the DB.</span></span> <span data-ttu-id="14580-134">Questo codice di migrazioni si trova nel file *Migrations\<timestamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="14580-134">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="14580-135">Il metodo `Up` della classe `InitialCreate` crea le tabelle di database che corrispondono al set di entità del modello di dati.</span><span class="sxs-lookup"><span data-stu-id="14580-135">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="14580-136">Il metodo `Down` le elimina, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="14580-136">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

<span data-ttu-id="14580-137">Le migrazioni chiamano il metodo `Up` per implementare le modifiche al modello di dati per una migrazione.</span><span class="sxs-lookup"><span data-stu-id="14580-137">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="14580-138">Quando si immette un comando per annullare l'aggiornamento, le migrazioni chiamano il metodo `Down`.</span><span class="sxs-lookup"><span data-stu-id="14580-138">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="14580-139">Il codice precedente è per la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="14580-139">The preceding code is for the initial migration.</span></span> <span data-ttu-id="14580-140">Tale codice è stato creato quando è stato eseguito il comando `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="14580-140">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="14580-141">Il parametro del nome della migrazione, nell'esempio "InitialCreate", viene usato per il nome del file.</span><span class="sxs-lookup"><span data-stu-id="14580-141">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="14580-142">Il nome della migrazione può essere qualsiasi nome di file valido.</span><span class="sxs-lookup"><span data-stu-id="14580-142">The migration name can be any valid file name.</span></span> <span data-ttu-id="14580-143">È consigliabile scegliere una parola o una frase che riepiloghi cosa viene fatto nella migrazione.</span><span class="sxs-lookup"><span data-stu-id="14580-143">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="14580-144">Ad esempio, una migrazione che ha aggiunto una tabella di reparto potrebbe essere denominata "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="14580-144">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="14580-145">Se la migrazione iniziale viene creata e il database esiste:</span><span class="sxs-lookup"><span data-stu-id="14580-145">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="14580-146">Viene generato il codice di creazione del database.</span><span class="sxs-lookup"><span data-stu-id="14580-146">The DB creation code is generated.</span></span>
* <span data-ttu-id="14580-147">Non è necessario che venga eseguito il codice di creazione del database perché il database corrisponde già al modello di dati.</span><span class="sxs-lookup"><span data-stu-id="14580-147">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="14580-148">Se viene eseguito il codice di creazione del database, non apporta alcuna modifica perché il database corrisponde già al modello di dati.</span><span class="sxs-lookup"><span data-stu-id="14580-148">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="14580-149">Quando l'app viene distribuita in un nuovo ambiente, è necessario eseguire il codice di creazione del database per creare il database.</span><span class="sxs-lookup"><span data-stu-id="14580-149">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="14580-150">Il database infatti non esiste in quanto è stato precedentemente eliminato, pertanto la migrazione crea il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="14580-150">Previously the DB was dropped and doesn't exist, so migrations creates the new DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="14580-151">Snapshot del modello di dati</span><span class="sxs-lookup"><span data-stu-id="14580-151">The data model snapshot</span></span>

<span data-ttu-id="14580-152">Le migrazioni creano uno *snapshot* dello schema del database corrente in *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="14580-152">Migrations create a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="14580-153">Quando si aggiunge una migrazione, EF determina le modifiche apportate confrontando il modello di dati con il file dello snapshot.</span><span class="sxs-lookup"><span data-stu-id="14580-153">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="14580-154">Per eliminare una migrazione, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="14580-154">To delete a migration, use the following command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="14580-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14580-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="14580-156">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="14580-156">Remove-Migration</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="14580-157">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="14580-157">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

<span data-ttu-id="14580-158">Per altre informazioni, vedere [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="14580-158">For more information, see  [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span></span>

---

<span data-ttu-id="14580-159">Il comando di rimozione migrazioni elimina la migrazione e garantisce che lo snapshot venga reimpostato correttamente.</span><span class="sxs-lookup"><span data-stu-id="14580-159">The remove migrations command deletes the migration and ensures the snapshot is correctly reset.</span></span>

### <a name="remove-ensurecreated-and-test-the-app"></a><span data-ttu-id="14580-160">Rimuovere EnsureCreated e testare l'app</span><span class="sxs-lookup"><span data-stu-id="14580-160">Remove EnsureCreated and test the app</span></span>

<span data-ttu-id="14580-161">Per lo sviluppo iniziale è stato usato il comando `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="14580-161">For early development, `EnsureCreated` was used.</span></span> <span data-ttu-id="14580-162">In questa esercitazione vengono usate le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="14580-162">In this tutorial, migrations are used.</span></span> <span data-ttu-id="14580-163">`EnsureCreated` presenta le limitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="14580-163">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="14580-164">Ignora le migrazioni e crea il database e lo schema.</span><span class="sxs-lookup"><span data-stu-id="14580-164">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="14580-165">Non crea una tabella delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="14580-165">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="14580-166">*Non* può essere usato con le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="14580-166">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="14580-167">È progettato per il test o per la creazione rapida di prototipi in cui il database viene eliminato e ricreato frequentemente.</span><span class="sxs-lookup"><span data-stu-id="14580-167">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="14580-168">Rimuovere la riga seguente da `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="14580-168">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

<span data-ttu-id="14580-169">Eseguire l'app e verificare che il database venga inizializzato.</span><span class="sxs-lookup"><span data-stu-id="14580-169">Run the app and verify the DB is seeded.</span></span>

### <a name="inspect-the-database"></a><span data-ttu-id="14580-170">Esaminare il database</span><span class="sxs-lookup"><span data-stu-id="14580-170">Inspect the database</span></span>

<span data-ttu-id="14580-171">Per controllare il database, usare **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="14580-171">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="14580-172">Si noti l'aggiunta di una tabella `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="14580-172">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="14580-173">La tabella `__EFMigrationsHistory` tiene traccia di quali migrazioni sono state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="14580-173">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="14580-174">Visualizzare i dati nella tabella `__EFMigrationsHistory`: viene visualizzata una riga per la prima migrazione.</span><span class="sxs-lookup"><span data-stu-id="14580-174">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="14580-175">Nell'ultimo log nell'esempio di output della CLI precedente viene visualizzata l'istruzione INSERT che crea tale riga.</span><span class="sxs-lookup"><span data-stu-id="14580-175">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="14580-176">Eseguire l'app e verificare che tutto funzioni.</span><span class="sxs-lookup"><span data-stu-id="14580-176">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="14580-177">Applicazione delle migrazioni nell'ambiente di produzione</span><span class="sxs-lookup"><span data-stu-id="14580-177">Applying migrations in production</span></span>

<span data-ttu-id="14580-178">È consigliabile fare in modo che le app nell'ambiente di produzione **non** chiamino [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="14580-178">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="14580-179">L'elemento `Migrate` non deve essere chiamato da un'app nella server farm.</span><span class="sxs-lookup"><span data-stu-id="14580-179">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="14580-180">Ad esempio, se l'app è stata distribuita in un cloud con scale-out, ovvero se sono in esecuzione più istanze dell'app.</span><span class="sxs-lookup"><span data-stu-id="14580-180">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="14580-181">La migrazione del database deve essere eseguita come parte della distribuzione e in modo controllato.</span><span class="sxs-lookup"><span data-stu-id="14580-181">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="14580-182">Gli approcci alla migrazione di database in ambiente di produzione includono:</span><span class="sxs-lookup"><span data-stu-id="14580-182">Production database migration approaches include:</span></span>

* <span data-ttu-id="14580-183">Uso delle migrazioni per creare script SQL e uso degli script SQL nella distribuzione.</span><span class="sxs-lookup"><span data-stu-id="14580-183">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="14580-184">Esecuzione di `dotnet ef database update` da un ambiente controllato.</span><span class="sxs-lookup"><span data-stu-id="14580-184">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="14580-185">EF Core usa la tabella `__MigrationsHistory` per stabilire se è necessario eseguire migrazioni.</span><span class="sxs-lookup"><span data-stu-id="14580-185">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="14580-186">Se il database è aggiornato, non viene eseguita alcuna migrazione.</span><span class="sxs-lookup"><span data-stu-id="14580-186">If the DB is up-to-date, no migration is run.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="14580-187">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="14580-187">Troubleshooting</span></span>

<span data-ttu-id="14580-188">Scaricare l'[app completa](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="14580-188">Download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="14580-189">L'app genera l'eccezione seguente:</span><span class="sxs-lookup"><span data-stu-id="14580-189">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="14580-190">Soluzione: Eseguire `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="14580-190">Solution: Run `dotnet ef database update`</span></span>

### <a name="additional-resources"></a><span data-ttu-id="14580-191">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="14580-191">Additional resources</span></span>

* [<span data-ttu-id="14580-192">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="14580-192">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* <span data-ttu-id="14580-193">[Interfaccia della riga di comando di .NET Core](/ef/core/miscellaneous/cli/dotnet)</span><span class="sxs-lookup"><span data-stu-id="14580-193">[.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>
* [<span data-ttu-id="14580-194">Console di Gestione pacchetti (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="14580-194">Package Manager Console (Visual Studio)</span></span>](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="14580-195">[Precedente](xref:data/ef-rp/sort-filter-page)
> [Successivo](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="14580-195">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
