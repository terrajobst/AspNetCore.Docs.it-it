---
title: ASP.NET Core MVC con EF Core - Migrazioni - 4 di 10
author: rick-anderson
description: In questa esercitazione si inizia a usare la funzionalità delle migrazioni EF Core per la gestione delle modifiche al modello di dati in un'applicazione ASP.NET Core MVC.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: 0a3ff28c9edefd2c7f96222060a0df76d538012b
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a>ASP.NET Core MVC con EF Core - Migrazioni - 4 di 10

Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione Web di esempio Contoso University illustra come creare applicazioni Web ASP.NET Core MVC con Entity Framework Core e Visual Studio. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](intro.md).

In questa esercitazione si inizia a usare la funzionalità delle migrazioni EF Core per la gestione delle modifiche al modello di dati. Nelle esercitazioni successive si aggiungeranno altre migrazioni quando si modifica il modello di dati.

## <a name="introduction-to-migrations"></a>Introduzione alle migrazioni

Quando si sviluppa una nuova applicazione, il modello di dati cambia di frequente e, a ogni cambiamento, non è più sincronizzato con il database. La serie di esercitazioni è iniziata con la configurazione di Entity Framework per creare il database se non esiste già. Quindi, ogni volta che si modifica il modello di dati, ovvero si aggiungono, rimuovono, o modificano le classi di entità o si modifica la classe DbContext, è possibile eliminare il database. In seguito EF ne crea uno nuovo corrispondente al modello ed esegue il seeding con dati di test.

Questo metodo che consiste nel mantenere il database sincronizzato con il modello di dati funziona bene fino a quando non si distribuisce l'applicazione nell'ambiente di produzione. Quando l'applicazione è in esecuzione nell'ambiente di produzione in genere archivia dati che devono essere mantenuti ed è necessario evitare di perdere tutto ad ogni modifica, come ad esempio all'aggiunta di una colonna. La funzionalità delle migrazioni di EF Core risolve questo problema consentendo a EF Core di aggiornare lo schema del database anziché crearne uno nuovo.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Pacchetti NuGet di Entity Framework Core per le migrazioni

Per usare le migrazioni, è possibile usare la **Console di Gestione pacchetti** o l'interfaccia della riga di comando (CLI).  Queste esercitazioni illustrano come usare i comandi dell'interfaccia della riga di comando. Per informazioni sulla Console di Gestione pacchetti, vedere [la fine di questa esercitazione](#pmc).

Gli strumenti di Entity Framework per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Per installare il pacchetto, aggiungerlo alla raccolta `DotNetCliToolReference` nel file con estensione *.csproj*, come illustrato. **Nota:** è necessario installare questo pacchetto modificando il file *.csproj* ; non è possibile utilizzare il comando `install-package` o la GUI di gestione di pacchetti. È possibile modificare il file con estensione *.csproj* facendo clic con il pulsante destro del mouse sul nome del progetto in **Esplora soluzioni** e selezionando **Modifica ContosoUniversity.csproj**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
I numeri di versione dell'esempio erano quelli correnti nel momento in cui l'esercitazione è stata scritta. 

## <a name="change-the-connection-string"></a>Modificare la stringa di connessione

Nel file *appsettings.json* cambiare il nome del database nella stringa di connessione digitando ContosoUniversity2 o un altro nome non ancora adottato nel computer in uso.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Questa modifica configura il progetto in modo che la prima migrazione crei un nuovo database. Questa operazione non è necessaria per iniziare a usare le migrazioni, ma si capirà più avanti perché è utile eseguirla.

> [!NOTE]
> In alternativa alla modifica del nome del database, è possibile eliminare il database. Usare **Esplora oggetti di SQL Server** o il comando della CLI `database drop`:
> ```console
> dotnet ef database drop
> ```
> Nella sezione seguente viene illustrato come eseguire i comandi della CLI.

## <a name="create-an-initial-migration"></a>Creare una migrazione iniziale

Salvare le modifiche e compilare il progetto. Aprire una finestra di comando e passare alla cartella del progetto. Ecco un modo rapido per eseguire questa operazione:

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Apri in Esplora File** dal menu di scelta rapida.

  ![Voce di menu Apri in Esplora file](migrations/_static/open-in-file-explorer.png)

* Immettere "cmd" nella barra degli indirizzi e premere INVIO.

  ![Finestra di comando](migrations/_static/open-command-window.png)

Immettere il comando seguente nella finestra Comando:

```console
dotnet ef migrations add InitialCreate
```

Nella finestra di comando viene visualizzato un output come il seguente:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Se viene visualizzato un messaggio di errore che indica che non sono stati trovati eseguibili corrispondenti al comando "dotnet-ef", vedere [il post di questo blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) per risolvere il problema.

Se viene visualizzato il messaggio di errore "*Impossibile accedere al file  ContosoUniversity.dll perché utilizzato da un altro processo.*", individuare l'icona di IIS Express nella barra delle applicazioni di Windows, selezionarla con il pulsante destro del mouse e fare clic su **ContosoUniversity > Arresta sito**.

## <a name="examine-the-up-and-down-methods"></a>Esaminare i metodi Up e Down

Quando è stato eseguito il comando `migrations add`, EF ha generato il codice che crea il database da zero. Questo codice si trova nella cartella *Migrations*, nel file denominato *\<timestamp>_InitialCreate.cs*. Il metodo `Up` della classe `InitialCreate` crea le tabelle di database che corrispondono ai set di entità del modello di dati, e il metodo `Down` le elimina, come illustrato nell'esempio seguente.

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Le migrazioni chiamano il metodo `Up` per implementare le modifiche al modello di dati per una migrazione. Quando si immette un comando per annullare l'aggiornamento, le migrazioni chiamano il metodo `Down`.

Questo codice è per la migrazione iniziale creata al momento dell'immissione del comando `migrations add InitialCreate`. Il parametro del nome della migrazione, nell'esempio "InitialCreate", viene usato per il nome del file e può essere ciò che si vuole. È consigliabile scegliere una parola o una frase che riepiloghi cosa viene fatto nella migrazione. Ad esempio, è possibile denominare una migrazione successiva "AddDepartmentTable".

Se la migrazione iniziale è stata creata quando il database esisteva già, il codice di creazione del database viene generato ma non è necessario eseguirlo perché il database corrisponde già al modello di dati. Quando si distribuisce l'app in un altro ambiente in cui il database non esiste ancora, questo codice verrà eseguito per creare il database, è consigliabile quindi testarlo prima. Ecco perché in precedenza è stato modificato il nome del database nella stringa di connessione: per far sì che le migrazioni possano crearne uno nuovo da zero.

## <a name="the-data-model-snapshot"></a>Snapshot del modello di dati

Le migrazioni creano uno *snapshot* dello schema del database corrente in *Migrations/SchoolContextModelSnapshot.cs*. Quando si aggiunge una migrazione, EF determina le modifiche apportate confrontando il modello di dati con il file dello snapshot.

Quando si elimina una migrazione, usare il comando [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove). `dotnet ef migrations remove` elimina la migrazione e garantisce che lo snapshot venga reimpostato correttamente.

Per altre informazioni sull'uso del file di snapshot, vedere [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) (Migrazioni EF Core in ambienti team).

## <a name="apply-the-migration-to-the-database"></a>Applicare la migrazione al database

Nella finestra di comando immettere il comando seguente per creare il database e le tabelle al suo interno.

```console
dotnet ef database update
```

L'output del comando è simile al comando `migrations add`, a eccezione del fatto che vengono visualizzati i log per i comandi SQL che configurano il database. La maggior parte dei log viene omessa nell'output di esempio seguente. Se si vuole ridurre il livello di dettaglio nei messaggi di log, è possibile modificare i livelli di log nel file *appsettings.Development.json*. Per altre informazioni, vedere [Come creare log](xref:fundamentals/logging/index).

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

Per controllare il database come è stato fatto nella prima esercitazione, usare **Esplora oggetti di SQL Server**.  Si noterà l'aggiunta di una tabella __EFMigrationsHistory che tiene traccia di quali migrazioni sono state applicate al database. Visualizzare i dati nella tabella: si noterà la presenza di una riga per la prima migrazione. Nell'ultimo log nell'esempio di output della CLI precedente viene visualizzata l'istruzione INSERT che crea tale riga.

Eseguire l'applicazione per verificare che tutto funzioni come prima.

![Pagina Student Index (Indice degli studenti)](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Interfaccia della riga di comando e console di Gestione pacchetti

Gli strumenti di EF per la gestione delle migrazioni sono disponibili dai comandi della CLI di .NET Core o dai cmdlet di PowerShell in Visual Studio nella finestra **Console di Gestione pacchetti**. In questa esercitazione viene illustrato come usare la CLI, ma se si preferisce è possibile usare la console di Gestione pacchetti.

I comandi di EF per la console di Gestione pacchetti sono inclusi nel pacchetto [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools). Il pacchetto è incluso nel metapacchetto [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), quindi non è necessario installarlo.

**Importante:** non si tratta dello stesso pacchetto che si installa per l'interfaccia della riga di comando modificando il file con estensione *.csproj*. Il nome di questo pacchetto termina con `Tools`, a differenza del nome del pacchetto della CLI che termina con `Tools.DotNet`.

Per altre informazioni sui comandi della CLI, vedere [Strumenti da riga di comando di EF Core .NET](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet). 

Per altre informazioni sui comandi della console di Gestione pacchetti, vedere [Console di Gestione pacchetti (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato spiegato come creare e applicare una prima migrazione. Nella prossima esercitazione verranno illustrati argomenti più avanzati espandendo il modello di dati. Lungo il percorso verranno create e applicate altre migrazioni.

> [!div class="step-by-step"]
> [Precedente](sort-filter-page.md)
> [Successivo](complex-data-model.md)  
