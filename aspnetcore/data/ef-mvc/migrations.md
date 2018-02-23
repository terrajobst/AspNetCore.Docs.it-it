---
title: ASP.NET Core MVC con EF Core - Migrazioni - 4 di 10
author: tdykstra
description: "In questa esercitazione si inizia a usare la funzionalità delle migrazioni EF Core per la gestione delle modifiche al modello di dati in un'applicazione ASP.NET Core MVC."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: fd466af8a73bf4c568fafe7e7fdcaa82021624da
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a><span data-ttu-id="90e3d-103">Migrazioni - Esercitazione su EF Core con ASP.NET Core MVC (4 di 10)</span><span class="sxs-lookup"><span data-stu-id="90e3d-103">Migrations - EF Core with ASP.NET Core MVC tutorial (4 of 10)</span></span>

<span data-ttu-id="90e3d-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="90e3d-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="90e3d-105">L'applicazione Web di esempio Contoso University illustra come creare applicazioni Web ASP.NET Core MVC con Entity Framework Core e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90e3d-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="90e3d-106">Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="90e3d-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="90e3d-107">In questa esercitazione si inizia a usare la funzionalità delle migrazioni EF Core per la gestione delle modifiche al modello di dati.</span><span class="sxs-lookup"><span data-stu-id="90e3d-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="90e3d-108">Nelle esercitazioni successive si aggiungeranno altre migrazioni quando si modifica il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="90e3d-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="90e3d-109">Introduzione alle migrazioni</span><span class="sxs-lookup"><span data-stu-id="90e3d-109">Introduction to migrations</span></span>

<span data-ttu-id="90e3d-110">Quando si sviluppa una nuova applicazione, il modello di dati cambia di frequente e, a ogni cambiamento, non è più sincronizzato con il database.</span><span class="sxs-lookup"><span data-stu-id="90e3d-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="90e3d-111">La serie di esercitazioni è iniziata con la configurazione di Entity Framework per creare il database se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="90e3d-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="90e3d-112">Quindi, ogni volta che si modifica il modello di dati, ovvero si aggiungono, rimuovono, o modificano le classi di entità o si modifica la classe DbContext, è possibile eliminare il database. In seguito EF ne crea uno nuovo corrispondente al modello ed esegue il seeding con dati di test.</span><span class="sxs-lookup"><span data-stu-id="90e3d-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="90e3d-113">Questo metodo che consiste nel mantenere il database sincronizzato con il modello di dati funziona bene fino a quando non si distribuisce l'applicazione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="90e3d-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="90e3d-114">Quando l'applicazione è in esecuzione nell'ambiente di produzione in genere archivia dati che devono essere mantenuti ed è necessario evitare di perdere tutto ad ogni modifica, come ad esempio all'aggiunta di una colonna.</span><span class="sxs-lookup"><span data-stu-id="90e3d-114">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="90e3d-115">La funzionalità delle migrazioni di EF Core risolve questo problema consentendo a EF Core di aggiornare lo schema del database anziché crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="90e3d-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="90e3d-116">Pacchetti NuGet di Entity Framework Core per le migrazioni</span><span class="sxs-lookup"><span data-stu-id="90e3d-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="90e3d-117">Per usare le migrazioni, è possibile usare la **Console di Gestione pacchetti** o l'interfaccia della riga di comando (CLI).</span><span class="sxs-lookup"><span data-stu-id="90e3d-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="90e3d-118">Queste esercitazioni illustrano come usare i comandi dell'interfaccia della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="90e3d-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="90e3d-119">Per informazioni sulla Console di Gestione pacchetti, vedere [la fine di questa esercitazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="90e3d-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="90e3d-120">Gli strumenti di Entity Framework per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="90e3d-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="90e3d-121">Per installare il pacchetto, aggiungerlo alla raccolta `DotNetCliToolReference` nel file con estensione *.csproj*, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="90e3d-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="90e3d-122">**Nota:** è necessario installare questo pacchetto modificando il file *.csproj* ; non è possibile utilizzare il comando `install-package` o la GUI di gestione di pacchetti.</span><span class="sxs-lookup"><span data-stu-id="90e3d-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="90e3d-123">È possibile modificare il file con estensione *.csproj* facendo clic con il pulsante destro del mouse sul nome del progetto in **Esplora soluzioni** e selezionando **Modifica ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="90e3d-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="90e3d-124">I numeri di versione dell'esempio erano quelli correnti nel momento in cui l'esercitazione è stata scritta.</span><span class="sxs-lookup"><span data-stu-id="90e3d-124">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="90e3d-125">Modificare la stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="90e3d-125">Change the connection string</span></span>

<span data-ttu-id="90e3d-126">Nel file *appsettings.json* cambiare il nome del database nella stringa di connessione digitando ContosoUniversity2 o un altro nome non ancora adottato nel computer in uso.</span><span class="sxs-lookup"><span data-stu-id="90e3d-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="90e3d-127">Questa modifica configura il progetto in modo che la prima migrazione crei un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="90e3d-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="90e3d-128">Questa operazione non è necessaria per iniziare a usare le migrazioni, ma si capirà più avanti perché è utile eseguirla.</span><span class="sxs-lookup"><span data-stu-id="90e3d-128">This isn't required for getting started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="90e3d-129">In alternativa alla modifica del nome del database, è possibile eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="90e3d-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="90e3d-130">Usare **Esplora oggetti di SQL Server** o il comando della CLI `database drop`:</span><span class="sxs-lookup"><span data-stu-id="90e3d-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="90e3d-131">Nella sezione seguente viene illustrato come eseguire i comandi della CLI.</span><span class="sxs-lookup"><span data-stu-id="90e3d-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="90e3d-132">Creare una migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="90e3d-132">Create an initial migration</span></span>

<span data-ttu-id="90e3d-133">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="90e3d-133">Save your changes and build the project.</span></span> <span data-ttu-id="90e3d-134">Aprire una finestra di comando e passare alla cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="90e3d-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="90e3d-135">Ecco un modo rapido per eseguire questa operazione:</span><span class="sxs-lookup"><span data-stu-id="90e3d-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="90e3d-136">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Apri in Esplora File** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="90e3d-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Voce di menu Apri in Esplora file](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="90e3d-138">Immettere "cmd" nella barra degli indirizzi e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="90e3d-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Finestra di comando](migrations/_static/open-command-window.png)

<span data-ttu-id="90e3d-140">Immettere il comando seguente nella finestra Comando:</span><span class="sxs-lookup"><span data-stu-id="90e3d-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="90e3d-141">Nella finestra di comando viene visualizzato un output come il seguente:</span><span class="sxs-lookup"><span data-stu-id="90e3d-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="90e3d-142">Se viene visualizzato un messaggio di errore che indica che non sono stati trovati eseguibili corrispondenti al comando "dotnet-ef", vedere [il post di questo blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="90e3d-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="90e3d-143">Se viene visualizzato il messaggio di errore "*Impossibile accedere al file  ContosoUniversity.dll perché utilizzato da un altro processo.*", individuare l'icona di IIS Express nella barra delle applicazioni di Windows, selezionarla con il pulsante destro del mouse e fare clic su **ContosoUniversity > Arresta sito**.</span><span class="sxs-lookup"><span data-stu-id="90e3d-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="90e3d-144">Esaminare i metodi Up e Down</span><span class="sxs-lookup"><span data-stu-id="90e3d-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="90e3d-145">Quando è stato eseguito il comando `migrations add`, EF ha generato il codice che crea il database da zero.</span><span class="sxs-lookup"><span data-stu-id="90e3d-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="90e3d-146">Questo codice si trova nella cartella *Migrations*, nel file denominato *\<timestamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="90e3d-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="90e3d-147">Il metodo `Up` della classe `InitialCreate` crea le tabelle di database che corrispondono ai set di entità del modello di dati, e il metodo `Down` le elimina, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="90e3d-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="90e3d-148">Le migrazioni chiamano il metodo `Up` per implementare le modifiche al modello di dati per una migrazione.</span><span class="sxs-lookup"><span data-stu-id="90e3d-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="90e3d-149">Quando si immette un comando per annullare l'aggiornamento, le migrazioni chiamano il metodo `Down`.</span><span class="sxs-lookup"><span data-stu-id="90e3d-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="90e3d-150">Questo codice è per la migrazione iniziale creata al momento dell'immissione del comando `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="90e3d-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="90e3d-151">Il parametro del nome della migrazione, nell'esempio "InitialCreate", viene usato per il nome del file e può essere ciò che si vuole.</span><span class="sxs-lookup"><span data-stu-id="90e3d-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="90e3d-152">È consigliabile scegliere una parola o una frase che riepiloghi cosa viene fatto nella migrazione.</span><span class="sxs-lookup"><span data-stu-id="90e3d-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="90e3d-153">Ad esempio, è possibile denominare una migrazione successiva "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="90e3d-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="90e3d-154">Se la migrazione iniziale è stata creata quando il database esisteva già, il codice di creazione del database viene generato ma non è necessario eseguirlo perché il database corrisponde già al modello di dati.</span><span class="sxs-lookup"><span data-stu-id="90e3d-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="90e3d-155">Quando si distribuisce l'app in un altro ambiente in cui il database non esiste ancora, questo codice verrà eseguito per creare il database, è consigliabile quindi testarlo prima.</span><span class="sxs-lookup"><span data-stu-id="90e3d-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="90e3d-156">Ecco perché in precedenza è stato modificato il nome del database nella stringa di connessione: per far sì che le migrazioni possano crearne uno nuovo da zero.</span><span class="sxs-lookup"><span data-stu-id="90e3d-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="90e3d-157">Esaminare lo snapshot del modello di dati</span><span class="sxs-lookup"><span data-stu-id="90e3d-157">Examine the data model snapshot</span></span>

<span data-ttu-id="90e3d-158">Le migrazioni creano anche uno *snapshot* dello schema del database corrente in *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="90e3d-158">Migrations also creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="90e3d-159">Ecco come viene visualizzato il codice:</span><span class="sxs-lookup"><span data-stu-id="90e3d-159">Here's what that code looks like:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="90e3d-160">Poiché lo schema di database corrente è rappresentato nel codice, non è necessario che EF Core interagisca con il database per creare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="90e3d-160">Because the current database schema is represented in code, EF Core doesn't have to interact with the database to create migrations.</span></span> <span data-ttu-id="90e3d-161">Quando si aggiunge una migrazione, EF determina le modifiche apportate confrontando il modello di dati con il file dello snapshot.</span><span class="sxs-lookup"><span data-stu-id="90e3d-161">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="90e3d-162">EF interagisce con il database solo quando è necessario aggiornarlo.</span><span class="sxs-lookup"><span data-stu-id="90e3d-162">EF interacts with the database only when it has to update the database.</span></span> 

<span data-ttu-id="90e3d-163">Il file di snapshot deve rimanere sincronizzato con le migrazioni che lo creano, pertanto non è possibile rimuovere una migrazione solo eliminando il file denominato *\<timestamp>_\<nomemigrazione>.cs*.</span><span class="sxs-lookup"><span data-stu-id="90e3d-163">The snapshot file has to be kept in sync with the migrations that create it, so you can't remove a migration just by deleting the file named  *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="90e3d-164">Se si elimina tale file, le migrazioni rimanenti non saranno sincronizzate con il file dello snapshot del database.</span><span class="sxs-lookup"><span data-stu-id="90e3d-164">If you delete that file, the remaining migrations will be out of sync with the database snapshot file.</span></span> <span data-ttu-id="90e3d-165">Per eliminare l'ultima migrazione aggiunta, usare il comando [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="90e3d-165">To delete the last migration that you added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="90e3d-166">Applicare la migrazione al database</span><span class="sxs-lookup"><span data-stu-id="90e3d-166">Apply the migration to the database</span></span>

<span data-ttu-id="90e3d-167">Nella finestra di comando immettere il comando seguente per creare il database e le tabelle al suo interno.</span><span class="sxs-lookup"><span data-stu-id="90e3d-167">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="90e3d-168">L'output del comando è simile al comando `migrations add`, a eccezione del fatto che vengono visualizzati i log per i comandi SQL che configurano il database.</span><span class="sxs-lookup"><span data-stu-id="90e3d-168">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="90e3d-169">La maggior parte dei log viene omessa nell'output di esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="90e3d-169">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="90e3d-170">Se si vuole ridurre il livello di dettaglio nei messaggi di log, è possibile modificare i livelli di log nel file *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="90e3d-170">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="90e3d-171">Per altre informazioni, vedere [Come creare log](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="90e3d-171">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

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

<span data-ttu-id="90e3d-172">Per controllare il database come è stato fatto nella prima esercitazione, usare **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="90e3d-172">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="90e3d-173">Si noterà l'aggiunta di una tabella __EFMigrationsHistory che tiene traccia di quali migrazioni sono state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="90e3d-173">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="90e3d-174">Visualizzare i dati nella tabella: si noterà la presenza di una riga per la prima migrazione.</span><span class="sxs-lookup"><span data-stu-id="90e3d-174">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="90e3d-175">Nell'ultimo log nell'esempio di output della CLI precedente viene visualizzata l'istruzione INSERT che crea tale riga.</span><span class="sxs-lookup"><span data-stu-id="90e3d-175">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="90e3d-176">Eseguire l'applicazione per verificare che tutto funzioni come prima.</span><span class="sxs-lookup"><span data-stu-id="90e3d-176">Run the application to verify that everything still works the same as before.</span></span>

![Pagina Student Index (Indice degli studenti)](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="90e3d-178">Interfaccia della riga di comando e console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="90e3d-178">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="90e3d-179">Gli strumenti di EF per la gestione delle migrazioni sono disponibili dai comandi della CLI di .NET Core o dai cmdlet di PowerShell in Visual Studio nella finestra **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="90e3d-179">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="90e3d-180">In questa esercitazione viene illustrato come usare la CLI, ma se si preferisce è possibile usare la console di Gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="90e3d-180">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="90e3d-181">I comandi di EF per la console di Gestione pacchetti sono inclusi nel pacchetto [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="90e3d-181">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="90e3d-182">Il pacchetto è incluso nel metapacchetto [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), quindi non è necessario installarlo.</span><span class="sxs-lookup"><span data-stu-id="90e3d-182">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="90e3d-183">**Importante:** non si tratta dello stesso pacchetto che si installa per l'interfaccia della riga di comando modificando il file con estensione *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="90e3d-183">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="90e3d-184">Il nome di questo pacchetto termina con `Tools`, a differenza del nome del pacchetto della CLI che termina con `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="90e3d-184">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="90e3d-185">Per altre informazioni sui comandi della CLI, vedere [Strumenti da riga di comando di EF Core .NET](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="90e3d-185">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="90e3d-186">Per altre informazioni sui comandi della console di Gestione pacchetti, vedere [Console di Gestione pacchetti (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="90e3d-186">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="90e3d-187">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="90e3d-187">Summary</span></span>

<span data-ttu-id="90e3d-188">In questa esercitazione è stato spiegato come creare e applicare una prima migrazione.</span><span class="sxs-lookup"><span data-stu-id="90e3d-188">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="90e3d-189">Nella prossima esercitazione verranno illustrati argomenti più avanzati espandendo il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="90e3d-189">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="90e3d-190">Lungo il percorso verranno create e applicate altre migrazioni.</span><span class="sxs-lookup"><span data-stu-id="90e3d-190">Along the way you'll create and apply additional migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="90e3d-191">[Precedente](sort-filter-page.md)
[Successivo](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="90e3d-191">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  
