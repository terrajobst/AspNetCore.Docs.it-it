---
title: Razor Pages con EF Core in ASP.NET Core - Migrazioni - 4 di 8
author: rick-anderson
description: In questa esercitazione si inizia a usare la funzionalità delle migrazioni EF Core per la gestione delle modifiche al modello di dati in un'app ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 690beaabeab098cf9b764730b1bf1bd04bf6b003
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="80b78-103">Razor Pages con EF Core in ASP.NET Core - Migrazioni - 4 di 8</span><span class="sxs-lookup"><span data-stu-id="80b78-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="80b78-104">Di [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="80b78-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="80b78-105">In questa esercitazione viene usata la funzionalità delle migrazioni EF Core per la gestione delle modifiche al modello di dati.</span><span class="sxs-lookup"><span data-stu-id="80b78-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="80b78-106">Se si verificano problemi che non è possibile risolvere, scaricare l'[app completa per questa fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="80b78-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="80b78-107">Quando viene sviluppata una nuova app, il modello di dati cambia spesso.</span><span class="sxs-lookup"><span data-stu-id="80b78-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="80b78-108">Ogni volta che vengono apportate modifiche al modello, questo non è più sincronizzato con il database.</span><span class="sxs-lookup"><span data-stu-id="80b78-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="80b78-109">L'esercitazione è iniziata con la configurazione di Entity Framework per creare il database se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="80b78-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="80b78-110">Ogni volta che il modello di dati cambia:</span><span class="sxs-lookup"><span data-stu-id="80b78-110">Each time the data model changes:</span></span>

* <span data-ttu-id="80b78-111">Il database viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="80b78-111">The DB is dropped.</span></span>
* <span data-ttu-id="80b78-112">EF ne crea uno nuovo che corrisponde al modello.</span><span class="sxs-lookup"><span data-stu-id="80b78-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="80b78-113">L'app esegue il seeding del database con dati di test.</span><span class="sxs-lookup"><span data-stu-id="80b78-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="80b78-114">Questo approccio che consiste nel mantenere il database sincronizzato con il modello di dati funziona bene fino a quando non si distribuisce l'app nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="80b78-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="80b78-115">Quando l'app è in esecuzione nell'ambiente di produzione, in genere esegue l'archiviazione di dati che devono essere mantenuti.</span><span class="sxs-lookup"><span data-stu-id="80b78-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="80b78-116">L'app non può essere avviata con un database di test ogni volta che viene apportata una modifica, come ad esempio l'aggiunta di una colonna.</span><span class="sxs-lookup"><span data-stu-id="80b78-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="80b78-117">La funzionalità delle migrazioni di EF Core risolve questo problema consentendo a EF Core di aggiornare lo schema del database anziché creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="80b78-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="80b78-118">Anziché eliminare e ricreare il database quando il modello di dati viene modificato, le migrazioni aggiornano lo schema e mantengono i dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="80b78-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="80b78-119">Pacchetti NuGet di Entity Framework Core per le migrazioni</span><span class="sxs-lookup"><span data-stu-id="80b78-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="80b78-120">Per usare le migrazioni, è possibile usare la **Console di Gestione pacchetti** o l'interfaccia della riga di comando (CLI).</span><span class="sxs-lookup"><span data-stu-id="80b78-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="80b78-121">Queste esercitazioni illustrano come usare i comandi dell'interfaccia della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="80b78-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="80b78-122">Per informazioni sulla Console di Gestione pacchetti, vedere [la fine di questa esercitazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="80b78-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="80b78-123">Gli strumenti di EF Core per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="80b78-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="80b78-124">Per installare il pacchetto, aggiungerlo alla raccolta `DotNetCliToolReference` nel file con estensione *.csproj*, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="80b78-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="80b78-125">**Nota:** il pacchetto deve essere installato mediante la modifica del file con estensione *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="80b78-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="80b78-126">Non è possibile usare il comando `install-package` o la GUI di gestione di pacchetti per installare il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="80b78-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="80b78-127">Modificare il file con estensione *.csproj* facendo clic con il pulsante destro del mouse sul nome del progetto in **Esplora soluzioni** e selezionando **Modifica ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="80b78-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="80b78-128">Il markup riportato di seguito illustra il file con estensione *.csproj* aggiornato con gli strumenti della CLI di EF Core evidenziati:</span><span class="sxs-lookup"><span data-stu-id="80b78-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="80b78-129">I numeri di versione dell'esempio precedente erano quelli correnti nel momento in cui l'esercitazione è stata scritta.</span><span class="sxs-lookup"><span data-stu-id="80b78-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="80b78-130">Usare la stessa versione per gli strumenti della CLI di EF Core trovata negli altri pacchetti.</span><span class="sxs-lookup"><span data-stu-id="80b78-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="80b78-131">Modificare la stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="80b78-131">Change the connection string</span></span>

<span data-ttu-id="80b78-132">Nel file *appsettings.json* modificare il nome del database nella stringa di connessione in ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="80b78-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="80b78-133">Se si modifica il nome del database nella stringa di connessione, la prima migrazione crea un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="80b78-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="80b78-134">Viene creato un nuovo database poiché un database con quel nome non esiste.</span><span class="sxs-lookup"><span data-stu-id="80b78-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="80b78-135">La modifica della stringa di connessione non è obbligatoria per iniziare a usare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="80b78-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="80b78-136">Un'alternativa alla modifica del nome del database è l'eliminazione del database.</span><span class="sxs-lookup"><span data-stu-id="80b78-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="80b78-137">Usare **Esplora oggetti di SQL Server** o il comando della CLI `database drop`:</span><span class="sxs-lookup"><span data-stu-id="80b78-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="80b78-138">Nella sezione seguente viene illustrato come eseguire i comandi della CLI.</span><span class="sxs-lookup"><span data-stu-id="80b78-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="80b78-139">Creare una migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="80b78-139">Create an initial migration</span></span>

<span data-ttu-id="80b78-140">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="80b78-140">Build the project.</span></span>

<span data-ttu-id="80b78-141">Aprire una finestra di comando e passare alla cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="80b78-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="80b78-142">La cartella del progetto contiene il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="80b78-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="80b78-143">Digitare quanto segue nella finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="80b78-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="80b78-144">Nella finestra di comando vengono visualizzate informazioni simili alle seguenti:</span><span class="sxs-lookup"><span data-stu-id="80b78-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="80b78-145">Se la migrazione non riesce e viene visualizzato il messaggio "*Impossibile accedere al file ContosoUniversity.dll perché utilizzato da un altro processo.*"</span><span class="sxs-lookup"><span data-stu-id="80b78-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="80b78-146">:</span><span class="sxs-lookup"><span data-stu-id="80b78-146">is displayed:</span></span>

* <span data-ttu-id="80b78-147">Arrestare IIS Express.</span><span class="sxs-lookup"><span data-stu-id="80b78-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="80b78-148">Chiudere e riavviare Visual Studio oppure</span><span class="sxs-lookup"><span data-stu-id="80b78-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="80b78-149">Individuare l'icona di IIS Express nella barra delle applicazioni di Windows.</span><span class="sxs-lookup"><span data-stu-id="80b78-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="80b78-150">Fare clic con il pulsante destro del mouse sull'icona di IIS Express e quindi fare clic su **ContosoUniversity > Arresta sito**.</span><span class="sxs-lookup"><span data-stu-id="80b78-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="80b78-151">Se viene visualizzato il messaggio di errore "Compilazione non riuscita.",</span><span class="sxs-lookup"><span data-stu-id="80b78-151">If the error message "Build failed."</span></span> <span data-ttu-id="80b78-152">eseguire nuovamente il comando.</span><span class="sxs-lookup"><span data-stu-id="80b78-152">is displayed, run the command again.</span></span> <span data-ttu-id="80b78-153">Se si verifica questo errore, lasciare una nota alla fine di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="80b78-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="80b78-154">Esaminare i metodi Up e Down</span><span class="sxs-lookup"><span data-stu-id="80b78-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="80b78-155">Il comando `migrations add` di EF Core ha generato codice da cui creare il database.</span><span class="sxs-lookup"><span data-stu-id="80b78-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="80b78-156">Questo codice di migrazioni si trova nel file *Migrations\<timestamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="80b78-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="80b78-157">Il metodo `Up` della classe `InitialCreate` crea le tabelle di database che corrispondono al set di entità del modello di dati.</span><span class="sxs-lookup"><span data-stu-id="80b78-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="80b78-158">Il metodo `Down` le elimina, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="80b78-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="80b78-159">Le migrazioni chiamano il metodo `Up` per implementare le modifiche al modello di dati per una migrazione.</span><span class="sxs-lookup"><span data-stu-id="80b78-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="80b78-160">Quando si immette un comando per annullare l'aggiornamento, le migrazioni chiamano il metodo `Down`.</span><span class="sxs-lookup"><span data-stu-id="80b78-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="80b78-161">Il codice precedente è per la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="80b78-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="80b78-162">Tale codice è stato creato quando è stato eseguito il comando `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="80b78-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="80b78-163">Il parametro del nome della migrazione, nell'esempio "InitialCreate", viene usato per il nome del file.</span><span class="sxs-lookup"><span data-stu-id="80b78-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="80b78-164">Il nome della migrazione può essere qualsiasi nome di file valido.</span><span class="sxs-lookup"><span data-stu-id="80b78-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="80b78-165">È consigliabile scegliere una parola o una frase che riepiloghi cosa viene fatto nella migrazione.</span><span class="sxs-lookup"><span data-stu-id="80b78-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="80b78-166">Ad esempio, una migrazione che ha aggiunto una tabella di reparto potrebbe essere denominata "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="80b78-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="80b78-167">Se la migrazione iniziale viene creata e il database esiste:</span><span class="sxs-lookup"><span data-stu-id="80b78-167">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="80b78-168">Viene generato il codice di creazione del database.</span><span class="sxs-lookup"><span data-stu-id="80b78-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="80b78-169">Non è necessario che venga eseguito il codice di creazione del database perché il database corrisponde già al modello di dati.</span><span class="sxs-lookup"><span data-stu-id="80b78-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="80b78-170">Se viene eseguito il codice di creazione del database, non apporta alcuna modifica perché il database corrisponde già al modello di dati.</span><span class="sxs-lookup"><span data-stu-id="80b78-170">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="80b78-171">Quando l'app viene distribuita in un nuovo ambiente, è necessario eseguire il codice di creazione del database per creare il database.</span><span class="sxs-lookup"><span data-stu-id="80b78-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="80b78-172">In precedenza è stata modificata la stringa di connessione per usare un nuovo nome per il database.</span><span class="sxs-lookup"><span data-stu-id="80b78-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="80b78-173">Il database specificato non esiste, pertanto viene creato dalle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="80b78-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="80b78-174">Snapshot del modello di dati</span><span class="sxs-lookup"><span data-stu-id="80b78-174">The data model snapshot</span></span>

<span data-ttu-id="80b78-175">Le migrazioni creano uno *snapshot* dello schema del database corrente in *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="80b78-175">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="80b78-176">Quando si aggiunge una migrazione, EF determina le modifiche apportate confrontando il modello di dati con il file dello snapshot.</span><span class="sxs-lookup"><span data-stu-id="80b78-176">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="80b78-177">Quando si elimina una migrazione, usare il comando [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="80b78-177">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="80b78-178">`dotnet ef migrations remove` elimina la migrazione e garantisce che lo snapshot venga reimpostato correttamente.</span><span class="sxs-lookup"><span data-stu-id="80b78-178">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="80b78-179">Per altre informazioni sull'uso del file di snapshot, vedere [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) (Migrazioni EF Core in ambienti team).</span><span class="sxs-lookup"><span data-stu-id="80b78-179">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="80b78-180">Rimuovere EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="80b78-180">Remove EnsureCreated</span></span>

<span data-ttu-id="80b78-181">Per lo sviluppo iniziale è stato usato il comando `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="80b78-181">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="80b78-182">In questa esercitazione vengono usate le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="80b78-182">In this tutorial, migrations is used.</span></span> <span data-ttu-id="80b78-183">`EnsureCreated` presenta le limitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="80b78-183">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="80b78-184">Ignora le migrazioni e crea il database e lo schema.</span><span class="sxs-lookup"><span data-stu-id="80b78-184">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="80b78-185">Non crea una tabella delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="80b78-185">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="80b78-186">*Non* può essere usato con le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="80b78-186">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="80b78-187">È progettato per il test o per la creazione rapida di prototipi in cui il database viene eliminato e ricreato frequentemente.</span><span class="sxs-lookup"><span data-stu-id="80b78-187">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="80b78-188">Rimuovere la riga seguente da `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="80b78-188">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="80b78-189">Applicare la migrazione al database in fase di sviluppo</span><span class="sxs-lookup"><span data-stu-id="80b78-189">Apply the migration to the DB in development</span></span>

<span data-ttu-id="80b78-190">Nella finestra di comando immettere quanto segue per creare il database e le tabelle.</span><span class="sxs-lookup"><span data-stu-id="80b78-190">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="80b78-191">Nota: se il comando `update` restituisce l'errore "Compilazione non riuscita.":</span><span class="sxs-lookup"><span data-stu-id="80b78-191">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="80b78-192">Eseguire nuovamente il comando.</span><span class="sxs-lookup"><span data-stu-id="80b78-192">Run the command again.</span></span>
* <span data-ttu-id="80b78-193">Se l'errore persiste, uscire da Visual Studio ed eseguire il comando `update`.</span><span class="sxs-lookup"><span data-stu-id="80b78-193">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="80b78-194">Lasciare un messaggio in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="80b78-194">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="80b78-195">L'output del comando è simile a quello del comando `migrations add`.</span><span class="sxs-lookup"><span data-stu-id="80b78-195">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="80b78-196">Nel comando precedente vengono visualizzati i log per i comandi SQL che hanno configurato il database.</span><span class="sxs-lookup"><span data-stu-id="80b78-196">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="80b78-197">La maggior parte dei log viene omessa nell'output di esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="80b78-197">Most of the logs are omitted in the following sample output:</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="80b78-198">Per ridurre il livello di dettaglio nei messaggi di log, modificare i livelli di log del file *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="80b78-198">To reduce the level of detail in log messages, change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="80b78-199">Per altre informazioni, vedere [Come creare log](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="80b78-199">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="80b78-200">Per controllare il database, usare **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="80b78-200">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="80b78-201">Si noti l'aggiunta di una tabella `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="80b78-201">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="80b78-202">La tabella `__EFMigrationsHistory` tiene traccia di quali migrazioni sono state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="80b78-202">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="80b78-203">Visualizzare i dati nella tabella `__EFMigrationsHistory`: viene visualizzata una riga per la prima migrazione.</span><span class="sxs-lookup"><span data-stu-id="80b78-203">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="80b78-204">Nell'ultimo log nell'esempio di output della CLI precedente viene visualizzata l'istruzione INSERT che crea tale riga.</span><span class="sxs-lookup"><span data-stu-id="80b78-204">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="80b78-205">Eseguire l'app e verificare che tutto funzioni.</span><span class="sxs-lookup"><span data-stu-id="80b78-205">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="80b78-206">Applicazione delle migrazioni nell'ambiente di produzione</span><span class="sxs-lookup"><span data-stu-id="80b78-206">Applying migrations in production</span></span>

<span data-ttu-id="80b78-207">È consigliabile fare in modo che le app nell'ambiente di produzione **non** chiamino [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="80b78-207">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="80b78-208">L'elemento `Migrate` non deve essere chiamato da un'app nella server farm.</span><span class="sxs-lookup"><span data-stu-id="80b78-208">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="80b78-209">Ad esempio, se l'app è stata distribuita in un cloud con scale-out, ovvero se sono in esecuzione più istanze dell'app.</span><span class="sxs-lookup"><span data-stu-id="80b78-209">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="80b78-210">La migrazione del database deve essere eseguita come parte della distribuzione e in modo controllato.</span><span class="sxs-lookup"><span data-stu-id="80b78-210">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="80b78-211">Gli approcci alla migrazione di database in ambiente di produzione includono:</span><span class="sxs-lookup"><span data-stu-id="80b78-211">Production database migration approaches include:</span></span>

* <span data-ttu-id="80b78-212">Uso delle migrazioni per creare script SQL e uso degli script SQL nella distribuzione.</span><span class="sxs-lookup"><span data-stu-id="80b78-212">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="80b78-213">Esecuzione di `dotnet ef database update` da un ambiente controllato.</span><span class="sxs-lookup"><span data-stu-id="80b78-213">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="80b78-214">EF Core usa la tabella `__MigrationsHistory` per stabilire se è necessario eseguire migrazioni.</span><span class="sxs-lookup"><span data-stu-id="80b78-214">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="80b78-215">Se il database è aggiornato, non viene eseguita alcuna migrazione.</span><span class="sxs-lookup"><span data-stu-id="80b78-215">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="80b78-216">Interfaccia della riga di comando e console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="80b78-216">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="80b78-217">Gli strumenti di EF Core per la gestione delle migrazioni sono disponibili da:</span><span class="sxs-lookup"><span data-stu-id="80b78-217">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="80b78-218">Comandi dell'interfaccia della riga di comando di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="80b78-218">.NET Core CLI commands.</span></span>
* <span data-ttu-id="80b78-219">Cmdlet di PowerShell nella finestra di Visual Studio **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="80b78-219">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="80b78-220">In questa esercitazione viene illustrato come usare l'interfaccia della riga di comando. Alcuni sviluppatori preferiscono usare invece la console di Gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="80b78-220">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="80b78-221">I comandi di EF Core per la console di Gestione pacchetti sono inclusi nel pacchetto [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="80b78-221">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="80b78-222">Il pacchetto è incluso nel metapacchetto [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), quindi non è necessario installarlo.</span><span class="sxs-lookup"><span data-stu-id="80b78-222">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="80b78-223">**Importante:** non si tratta dello stesso pacchetto che si installa per l'interfaccia della riga di comando modificando il file con estensione *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="80b78-223">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="80b78-224">Il nome di questo pacchetto termina con `Tools`, a differenza del nome del pacchetto della CLI che termina con `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="80b78-224">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="80b78-225">Per altre informazioni sui comandi della CLI, vedere [Strumenti da riga di comando di EF Core .NET](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="80b78-225">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="80b78-226">Per altre informazioni sui comandi della console di Gestione pacchetti, vedere [Console di Gestione pacchetti (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="80b78-226">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="80b78-227">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="80b78-227">Troubleshooting</span></span>

<span data-ttu-id="80b78-228">Scaricare l'[app completata per questa fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="80b78-228">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="80b78-229">L'app genera l'eccezione seguente:</span><span class="sxs-lookup"><span data-stu-id="80b78-229">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="80b78-230">Soluzione: eseguire `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="80b78-230">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="80b78-231">Se il comando `update` restituisce l'errore "Compilazione non riuscita.":</span><span class="sxs-lookup"><span data-stu-id="80b78-231">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="80b78-232">Eseguire nuovamente il comando.</span><span class="sxs-lookup"><span data-stu-id="80b78-232">Run the command again.</span></span>
* <span data-ttu-id="80b78-233">Lasciare un messaggio in fondo alla pagina.</span><span class="sxs-lookup"><span data-stu-id="80b78-233">Leave a message at the bottom of the page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="80b78-234">[Precedente](xref:data/ef-rp/sort-filter-page)
> [Successivo](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="80b78-234">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
