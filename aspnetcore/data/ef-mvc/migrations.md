---
title: Core di ASP.NET MVC con Entity Framework Core - Migrations - 4 di 10
author: tdykstra
description: "In questa esercitazione, iniziare a usare la funzionalità di migrazioni EF Core per la gestione delle modifiche al modello di dati in un'applicazione ASP.NET MVC di base."
keywords: Migrazioni di ASP.NET Core, Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 81f6c9c2-a819-4f3a-97a4-4b0503b56c26
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: 4d81099d1ab97a8a49d96657153a54aa96dd6bf8
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2017
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a><span data-ttu-id="8f31e-104">Migrazioni - EF Core con l'esercitazione di base di ASP.NET MVC (4 di 10)</span><span class="sxs-lookup"><span data-stu-id="8f31e-104">Migrations - EF Core with ASP.NET Core MVC tutorial (4 of 10)</span></span>

<span data-ttu-id="8f31e-105">Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8f31e-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8f31e-106">L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f31e-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="8f31e-107">Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="8f31e-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="8f31e-108">In questa esercitazione, iniziare a usare la funzionalità di migrazioni EF Core per la gestione delle modifiche al modello di dati.</span><span class="sxs-lookup"><span data-stu-id="8f31e-108">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="8f31e-109">Nelle esercitazioni successive, si aggiungeranno ulteriori migrazioni quando si modifica il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="8f31e-109">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="8f31e-110">Introduzione alle migrazioni</span><span class="sxs-lookup"><span data-stu-id="8f31e-110">Introduction to migrations</span></span>

<span data-ttu-id="8f31e-111">Quando si sviluppa una nuova applicazione, il modello di dati cambia di frequente e ogni volta che le modifiche del modello, viene sincronizzato con il database.</span><span class="sxs-lookup"><span data-stu-id="8f31e-111">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="8f31e-112">Queste esercitazioni è stato avviato tramite la configurazione di Entity Framework per creare il database se non esiste.</span><span class="sxs-lookup"><span data-stu-id="8f31e-112">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="8f31e-113">Quindi, ogni volta che si modifica il modello di dati - aggiungere, rimuovere, o modificare le classi di entità o modifica la classe DbContext - è possibile eliminare il database ed EF crea una nuova istanza che corrisponde al modello e si esegue il seeding con dati di test.</span><span class="sxs-lookup"><span data-stu-id="8f31e-113">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="8f31e-114">Questo metodo di sincronizzazione del database con il modello di dati funziona bene fino a quando non si distribuisce l'applicazione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="8f31e-114">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="8f31e-115">Quando l'applicazione è in esecuzione nell'ambiente di produzione che è in genere l'archiviazione di dati che si desidera mantenere e non si desidera perdere tutti gli elementi ogni volta che si apporta una modifica ad esempio l'aggiunta di una nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="8f31e-115">When the application is running in production it is usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="8f31e-116">La funzionalità di Entity Framework Core migrazioni risolve questo problema abilitando EF aggiornare lo schema del database anziché creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="8f31e-116">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="8f31e-117">Pacchetti di NuGet Core Framework entità per le migrazioni</span><span class="sxs-lookup"><span data-stu-id="8f31e-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="8f31e-118">Per lavorare con le migrazioni, è possibile utilizzare il **Package Manager Console** (PMC) o l'interfaccia della riga di comando (CLI).</span><span class="sxs-lookup"><span data-stu-id="8f31e-118">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="8f31e-119">Queste esercitazioni viene illustrato come utilizzare i comandi CLI.</span><span class="sxs-lookup"><span data-stu-id="8f31e-119">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="8f31e-120">Informazioni sul PMC, vedere [la fine di questa esercitazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="8f31e-120">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="8f31e-121">Vengono forniti gli strumenti di Entity Framework per l'interfaccia della riga di comando (CLI) in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="8f31e-121">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="8f31e-122">Per installare questo pacchetto, aggiungerlo al `DotNetCliToolReference` insieme il *csproj* file, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="8f31e-122">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="8f31e-123">**Nota:** è necessario installare questo pacchetto modificando il *csproj* file; non è possibile utilizzare il `install-package` comando o la GUI di gestione di pacchetti.</span><span class="sxs-lookup"><span data-stu-id="8f31e-123">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="8f31e-124">È possibile modificare il *csproj* file facendo clic con il nome del progetto in **Esplora** e selezionando **ContosoUniversity.csproj modifica**.</span><span class="sxs-lookup"><span data-stu-id="8f31e-124">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="8f31e-125">(I numeri di versione in questo esempio sono stati correnti quando l'esercitazione è stato scritto.)</span><span class="sxs-lookup"><span data-stu-id="8f31e-125">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="8f31e-126">Modificare la stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="8f31e-126">Change the connection string</span></span>

<span data-ttu-id="8f31e-127">Nel *appSettings. JSON* file, modificare il nome del database nella stringa di connessione in ContosoUniversity2 o un altro nome che non è stato utilizzato nel computer in uso.</span><span class="sxs-lookup"><span data-stu-id="8f31e-127">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

<span data-ttu-id="8f31e-128">[!code-json[Principale](intro/samples/cu/appsettings2.json?range=1-4)]</span><span class="sxs-lookup"><span data-stu-id="8f31e-128">[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]</span></span>

<span data-ttu-id="8f31e-129">Questa modifica si configura il progetto in modo che la migrazione prima creerà un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="8f31e-129">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="8f31e-130">Questo non è necessario per iniziare a usare le migrazioni, ma verrà visualizzato in un secondo momento perché è una buona idea.</span><span class="sxs-lookup"><span data-stu-id="8f31e-130">This isn't required for getting started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="8f31e-131">Come alternativa alla modifica del nome di database, è possibile eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="8f31e-131">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="8f31e-132">Utilizzare **Esplora oggetti di SQL Server** (sillaba SSOX) o `database drop` comando CLI:</span><span class="sxs-lookup"><span data-stu-id="8f31e-132">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="8f31e-133">Nella sezione seguente viene illustrato come eseguire i comandi CLI.</span><span class="sxs-lookup"><span data-stu-id="8f31e-133">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="8f31e-134">Creazione di una migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="8f31e-134">Create an initial migration</span></span>

<span data-ttu-id="8f31e-135">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="8f31e-135">Save your changes and build the project.</span></span> <span data-ttu-id="8f31e-136">Quindi, aprire una finestra di comando e passare alla cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="8f31e-136">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="8f31e-137">Ecco un modo rapido per eseguire questa operazione:</span><span class="sxs-lookup"><span data-stu-id="8f31e-137">Here's a quick way to do that:</span></span>

* <span data-ttu-id="8f31e-138">In **Esplora**, fare clic sul progetto e scegliere **Apri in Esplora File** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="8f31e-138">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Apri nella voce di menu di Esplora File](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="8f31e-140">Immettere "cmd" nella barra degli indirizzi e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="8f31e-140">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Apri finestra di comando](migrations/_static/open-command-window.png)

<span data-ttu-id="8f31e-142">Immettere il comando seguente nella finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="8f31e-142">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="8f31e-143">È visualizzato un output simile al seguente nella finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="8f31e-143">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="8f31e-144">Se viene visualizzato un messaggio di errore *alcun eseguibile non trovato corrispondente comando "dotnet-ef"*, vedere [questo post di blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) per la risoluzione dei problemi della Guida.</span><span class="sxs-lookup"><span data-stu-id="8f31e-144">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="8f31e-145">Se viene visualizzato un messaggio di errore "*Impossibile accedere al file... ContosoUniversity.dll perché è utilizzato da un altro processo.* ", individuare l'icona di IIS Express nella barra delle applicazioni di Windows, pulsante destro del mouse, quindi fare clic su **ContosoUniversity > sito**.</span><span class="sxs-lookup"><span data-stu-id="8f31e-145">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="8f31e-146">Esaminare l'alto e verso il basso di metodi</span><span class="sxs-lookup"><span data-stu-id="8f31e-146">Examine the Up and Down methods</span></span>

<span data-ttu-id="8f31e-147">Quando è stata eseguita la `migrations add` comando EF ha generato il codice che verrà creato il database da zero.</span><span class="sxs-lookup"><span data-stu-id="8f31e-147">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="8f31e-148">Questo codice è il *migrazioni* cartella, nel file denominato  *\<timestamp > _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="8f31e-148">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="8f31e-149">Il `Up` metodo il `InitialCreate` classe crea le tabelle di database che corrispondono ai set di entità del modello di dati, e `Down` metodo eliminati, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="8f31e-149">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

<span data-ttu-id="8f31e-150">[!code-csharp[Principale](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]</span><span class="sxs-lookup"><span data-stu-id="8f31e-150">[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]</span></span>

<span data-ttu-id="8f31e-151">Chiamate di migrazioni di `Up` metodo per implementare le modifiche del modello di dati per la migrazione.</span><span class="sxs-lookup"><span data-stu-id="8f31e-151">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="8f31e-152">Quando si immette un comando per annullare l'aggiornamento, le chiamate di migrazioni di `Down` metodo.</span><span class="sxs-lookup"><span data-stu-id="8f31e-152">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="8f31e-153">Questo codice è per la migrazione iniziale creato al momento dell'immissione di `migrations add InitialCreate` comando.</span><span class="sxs-lookup"><span data-stu-id="8f31e-153">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="8f31e-154">Il parametro name di migrazione ("InitialCreate" nell'esempio) viene utilizzato per il nome del file e può essere elementi desiderati.</span><span class="sxs-lookup"><span data-stu-id="8f31e-154">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="8f31e-155">È consigliabile scegliere una parola o frase che riepiloga le quali viene eseguita la migrazione.</span><span class="sxs-lookup"><span data-stu-id="8f31e-155">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="8f31e-156">Ad esempio, è possibile denominare una migrazione successiva "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="8f31e-156">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="8f31e-157">Se la migrazione iniziale è stato creato quando il database esiste già, viene generato il codice di creazione del database, ma non è necessario eseguire perché il database è già corrisponde il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="8f31e-157">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="8f31e-158">Quando si distribuisce l'app in un altro ambiente in cui il database non esiste ancora, questo codice verrà eseguito per creare il database, è consigliabile prima di tutto testare.</span><span class="sxs-lookup"><span data-stu-id="8f31e-158">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="8f31e-159">Ecco perché è stato modificato il nome del database nella stringa di connessione in precedenza, in modo che le migrazioni possano crearne uno nuovo da zero.</span><span class="sxs-lookup"><span data-stu-id="8f31e-159">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="8f31e-160">Esaminare lo snapshot del modello di dati</span><span class="sxs-lookup"><span data-stu-id="8f31e-160">Examine the data model snapshot</span></span>

<span data-ttu-id="8f31e-161">Le migrazioni crea anche un *snapshot* dello schema del database corrente in *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="8f31e-161">Migrations also creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="8f31e-162">Ecco come appare il codice:</span><span class="sxs-lookup"><span data-stu-id="8f31e-162">Here's what that code looks like:</span></span>

<span data-ttu-id="8f31e-163">[!code-csharp[Principale](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]</span><span class="sxs-lookup"><span data-stu-id="8f31e-163">[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]</span></span>

<span data-ttu-id="8f31e-164">Poiché lo schema del database corrente è rappresentato nel codice, Core EF non deve interagire con il database per creare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="8f31e-164">Because the current database schema is represented in code, EF Core doesn't have to interact with the database to create migrations.</span></span> <span data-ttu-id="8f31e-165">Quando si aggiunge una migrazione, EF determina le modifiche confrontando il modello di dati del file di snapshot.</span><span class="sxs-lookup"><span data-stu-id="8f31e-165">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="8f31e-166">EF interagisce con il database solo quando è necessario aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="8f31e-166">EF interacts with the database only when it has to update the database.</span></span> 

<span data-ttu-id="8f31e-167">Il file di snapshot deve essere mantenuta sincronizzata con le migrazioni che creano, pertanto non è possibile rimuovere una migrazione solo eliminando il file denominato  *\<timestamp > _\<migrationname >. cs*.</span><span class="sxs-lookup"><span data-stu-id="8f31e-167">The snapshot file has to be kept in sync with the migrations that create it, so you can't remove a migration just by deleting the file named  *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="8f31e-168">Se si elimina il file, le migrazioni rimanenti sarà sincronizzate con il file di snapshot di database.</span><span class="sxs-lookup"><span data-stu-id="8f31e-168">If you delete that file, the remaining migrations will be out of sync with the database snapshot file.</span></span> <span data-ttu-id="8f31e-169">Per eliminare l'ultima migrazione che è stato aggiunto, usare il [migrazioni ef dotnet rimuovere](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) comando.</span><span class="sxs-lookup"><span data-stu-id="8f31e-169">To delete the last migration that you added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="8f31e-170">Applicare la migrazione al database</span><span class="sxs-lookup"><span data-stu-id="8f31e-170">Apply the migration to the database</span></span>

<span data-ttu-id="8f31e-171">Nella finestra di comando, immettere il comando seguente per creare il database e le tabelle in esso.</span><span class="sxs-lookup"><span data-stu-id="8f31e-171">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="8f31e-172">L'output del comando è simile al `migrations add` comando, a eccezione del fatto che si registri per i comandi SQL che imposta backup del database.</span><span class="sxs-lookup"><span data-stu-id="8f31e-172">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="8f31e-173">La maggior parte dei log vengono omessi negli output di esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="8f31e-173">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="8f31e-174">Se si preferisce non visualizzare questo livello di dettaglio nei messaggi di log, è possibile modificare i livelli di log di *appsettings. Development.JSON* file.</span><span class="sxs-lookup"><span data-stu-id="8f31e-174">If you prefer not to see this level of detail in log messages, you can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="8f31e-175">Per ulteriori informazioni, vedere [Introduzione a registrazione](xref:fundamentals/logging).</span><span class="sxs-lookup"><span data-stu-id="8f31e-175">For more information, see [Introduction to logging](xref:fundamentals/logging).</span></span>

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

<span data-ttu-id="8f31e-176">Utilizzare **Esplora oggetti di SQL Server** controllare il database, come indicato nella prima esercitazione.</span><span class="sxs-lookup"><span data-stu-id="8f31e-176">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="8f31e-177">Si noterà che l'aggiunta di una tabella __EFMigrationsHistory che tiene traccia di quali migrazioni sono state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="8f31e-177">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="8f31e-178">Verrà visualizzata una riga per la migrazione prima e visualizzare i dati della tabella.</span><span class="sxs-lookup"><span data-stu-id="8f31e-178">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="8f31e-179">(L'ultimo log nell'esempio di output CLI precedente visualizza l'istruzione INSERT che crea la riga).</span><span class="sxs-lookup"><span data-stu-id="8f31e-179">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="8f31e-180">Eseguire l'applicazione per verificare che tutto funzioni ancora identico al precedente.</span><span class="sxs-lookup"><span data-stu-id="8f31e-180">Run the application to verify that everything still works the same as before.</span></span>

![Pagina di indice di studenti](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="8f31e-182">Visual Studio interfaccia della riga di comando (CLI). Console di gestione pacchetti (PMC)</span><span class="sxs-lookup"><span data-stu-id="8f31e-182">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="8f31e-183">Strumenti di Entity Framework per le migrazioni di gestione è disponibile dai comandi di .NET Core CLI o dai cmdlet di PowerShell in Visual Studio **Package Manager Console** finestra (PMC).</span><span class="sxs-lookup"><span data-stu-id="8f31e-183">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="8f31e-184">In questa esercitazione viene illustrato come utilizzare l'interfaccia CLI, ma se si preferisce, è possibile utilizzare PMC.</span><span class="sxs-lookup"><span data-stu-id="8f31e-184">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="8f31e-185">I comandi di Entity Framework per i comandi PMC presenti il [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="8f31e-185">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="8f31e-186">Questo pacchetto è già incluso nel [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, pertanto non è necessario installarlo.</span><span class="sxs-lookup"><span data-stu-id="8f31e-186">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="8f31e-187">**Importante:** non è il pacchetto stesso di quello in cui si installa per l'interfaccia CLI modificando il *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="8f31e-187">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="8f31e-188">Il nome di questo termina `Tools`, a differenza del nome di pacchetto CLI che termina in `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="8f31e-188">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="8f31e-189">Per ulteriori informazioni sui comandi CLI, vedere [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="8f31e-189">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="8f31e-190">Per ulteriori informazioni sui comandi PMC, vedere [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="8f31e-190">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="8f31e-191">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="8f31e-191">Summary</span></span>

<span data-ttu-id="8f31e-192">In questa esercitazione è stato spiegato come creare e applicare la migrazione prima.</span><span class="sxs-lookup"><span data-stu-id="8f31e-192">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="8f31e-193">Nella prossima esercitazione, verrà iniziare esaminando argomenti più avanzati espandendo il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="8f31e-193">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="8f31e-194">Lungo il percorso viene creata e applicare ulteriori migrazioni.</span><span class="sxs-lookup"><span data-stu-id="8f31e-194">Along the way you'll create and apply additional migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8f31e-195">[Precedente](sort-filter-page.md)
[Successivo](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="8f31e-195">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  
