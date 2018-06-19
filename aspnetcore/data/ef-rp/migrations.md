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
ms.locfileid: "32740075"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Razor Pages con EF Core in ASP.NET Core - Migrazioni - 4 di 8

Di [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

In questa esercitazione viene usata la funzionalità delle migrazioni EF Core per la gestione delle modifiche al modello di dati.

Se si verificano problemi che non è possibile risolvere, scaricare l'[app completa per questa fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Quando viene sviluppata una nuova app, il modello di dati cambia spesso. Ogni volta che vengono apportate modifiche al modello, questo non è più sincronizzato con il database. L'esercitazione è iniziata con la configurazione di Entity Framework per creare il database se non esiste già. Ogni volta che il modello di dati cambia:

* Il database viene eliminato.
* EF ne crea uno nuovo che corrisponde al modello.
* L'app esegue il seeding del database con dati di test.

Questo approccio che consiste nel mantenere il database sincronizzato con il modello di dati funziona bene fino a quando non si distribuisce l'app nell'ambiente di produzione. Quando l'app è in esecuzione nell'ambiente di produzione, in genere esegue l'archiviazione di dati che devono essere mantenuti. L'app non può essere avviata con un database di test ogni volta che viene apportata una modifica, come ad esempio l'aggiunta di una colonna. La funzionalità delle migrazioni di EF Core risolve questo problema consentendo a EF Core di aggiornare lo schema del database anziché creare un nuovo database.

Anziché eliminare e ricreare il database quando il modello di dati viene modificato, le migrazioni aggiornano lo schema e mantengono i dati esistenti.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Pacchetti NuGet di Entity Framework Core per le migrazioni

Per usare le migrazioni, è possibile usare la **Console di Gestione pacchetti** o l'interfaccia della riga di comando (CLI). Queste esercitazioni illustrano come usare i comandi dell'interfaccia della riga di comando. Per informazioni sulla Console di Gestione pacchetti, vedere [la fine di questa esercitazione](#pmc).

Gli strumenti di EF Core per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Per installare il pacchetto, aggiungerlo alla raccolta `DotNetCliToolReference` nel file con estensione *.csproj*, come illustrato. **Nota:** il pacchetto deve essere installato mediante la modifica del file con estensione *.csproj*. Non è possibile usare il comando `install-package` o la GUI di gestione di pacchetti per installare il pacchetto. Modificare il file con estensione *.csproj* facendo clic con il pulsante destro del mouse sul nome del progetto in **Esplora soluzioni** e selezionando **Modifica ContosoUniversity.csproj**.

Il markup riportato di seguito illustra il file con estensione *.csproj* aggiornato con gli strumenti della CLI di EF Core evidenziati:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
I numeri di versione dell'esempio precedente erano quelli correnti nel momento in cui l'esercitazione è stata scritta. Usare la stessa versione per gli strumenti della CLI di EF Core trovata negli altri pacchetti.

## <a name="change-the-connection-string"></a>Modificare la stringa di connessione

Nel file *appsettings.json* modificare il nome del database nella stringa di connessione in ContosoUniversity2.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Se si modifica il nome del database nella stringa di connessione, la prima migrazione crea un nuovo database. Viene creato un nuovo database poiché un database con quel nome non esiste. La modifica della stringa di connessione non è obbligatoria per iniziare a usare le migrazioni.

Un'alternativa alla modifica del nome del database è l'eliminazione del database. Usare **Esplora oggetti di SQL Server** o il comando della CLI `database drop`:

 ```console
 dotnet ef database drop
 ```

Nella sezione seguente viene illustrato come eseguire i comandi della CLI.

## <a name="create-an-initial-migration"></a>Creare una migrazione iniziale

Compilare il progetto.

Aprire una finestra di comando e passare alla cartella del progetto. La cartella del progetto contiene il file *Startup.cs*.

Digitare quanto segue nella finestra di comando:

```console
dotnet ef migrations add InitialCreate
```

Nella finestra di comando vengono visualizzate informazioni simili alle seguenti:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Se la migrazione non riesce e viene visualizzato il messaggio "*Impossibile accedere al file ContosoUniversity.dll perché utilizzato da un altro processo.*" :

* Arrestare IIS Express.

   * Chiudere e riavviare Visual Studio oppure
   * Individuare l'icona di IIS Express nella barra delle applicazioni di Windows.
   * Fare clic con il pulsante destro del mouse sull'icona di IIS Express e quindi fare clic su **ContosoUniversity > Arresta sito**.

Se viene visualizzato il messaggio di errore "Compilazione non riuscita.", eseguire nuovamente il comando. Se si verifica questo errore, lasciare una nota alla fine di questa esercitazione.

### <a name="examine-the-up-and-down-methods"></a>Esaminare i metodi Up e Down

Il comando `migrations add` di EF Core ha generato codice da cui creare il database. Questo codice di migrazioni si trova nel file *Migrations\<timestamp>_InitialCreate.cs*. Il metodo `Up` della classe `InitialCreate` crea le tabelle di database che corrispondono al set di entità del modello di dati. Il metodo `Down` le elimina, come illustrato nell'esempio seguente:

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Le migrazioni chiamano il metodo `Up` per implementare le modifiche al modello di dati per una migrazione. Quando si immette un comando per annullare l'aggiornamento, le migrazioni chiamano il metodo `Down`.

Il codice precedente è per la migrazione iniziale. Tale codice è stato creato quando è stato eseguito il comando `migrations add InitialCreate`. Il parametro del nome della migrazione, nell'esempio "InitialCreate", viene usato per il nome del file. Il nome della migrazione può essere qualsiasi nome di file valido. È consigliabile scegliere una parola o una frase che riepiloghi cosa viene fatto nella migrazione. Ad esempio, una migrazione che ha aggiunto una tabella di reparto potrebbe essere denominata "AddDepartmentTable".

Se la migrazione iniziale viene creata e il database esiste:

* Viene generato il codice di creazione del database.
* Non è necessario che venga eseguito il codice di creazione del database perché il database corrisponde già al modello di dati. Se viene eseguito il codice di creazione del database, non apporta alcuna modifica perché il database corrisponde già al modello di dati.

Quando l'app viene distribuita in un nuovo ambiente, è necessario eseguire il codice di creazione del database per creare il database.

In precedenza è stata modificata la stringa di connessione per usare un nuovo nome per il database. Il database specificato non esiste, pertanto viene creato dalle migrazioni.

### <a name="the-data-model-snapshot"></a>Snapshot del modello di dati

Le migrazioni creano uno *snapshot* dello schema del database corrente in *Migrations/SchoolContextModelSnapshot.cs*. Quando si aggiunge una migrazione, EF determina le modifiche apportate confrontando il modello di dati con il file dello snapshot.

Quando si elimina una migrazione, usare il comando [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove). `dotnet ef migrations remove` elimina la migrazione e garantisce che lo snapshot venga reimpostato correttamente.

Per altre informazioni sull'uso del file di snapshot, vedere [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) (Migrazioni EF Core in ambienti team).

## <a name="remove-ensurecreated"></a>Rimuovere EnsureCreated

Per lo sviluppo iniziale è stato usato il comando `EnsureCreated`. In questa esercitazione vengono usate le migrazioni. `EnsureCreated` presenta le limitazioni seguenti:

* Ignora le migrazioni e crea il database e lo schema.
* Non crea una tabella delle migrazioni.
* *Non* può essere usato con le migrazioni.
* È progettato per il test o per la creazione rapida di prototipi in cui il database viene eliminato e ricreato frequentemente.

Rimuovere la riga seguente da `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Applicare la migrazione al database in fase di sviluppo

Nella finestra di comando immettere quanto segue per creare il database e le tabelle.

```console
dotnet ef database update
```

Nota: se il comando `update` restituisce l'errore "Compilazione non riuscita.":

* Eseguire nuovamente il comando.
* Se l'errore persiste, uscire da Visual Studio ed eseguire il comando `update`.
* Lasciare un messaggio in fondo alla pagina.

L'output del comando è simile a quello del comando `migrations add`. Nel comando precedente vengono visualizzati i log per i comandi SQL che hanno configurato il database. La maggior parte dei log viene omessa nell'output di esempio seguente:

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

Per ridurre il livello di dettaglio nei messaggi di log, modificare i livelli di log del file *appsettings.Development.json*. Per altre informazioni, vedere [Come creare log](xref:fundamentals/logging/index).

Per controllare il database, usare **Esplora oggetti di SQL Server**. Si noti l'aggiunta di una tabella `__EFMigrationsHistory`. La tabella `__EFMigrationsHistory` tiene traccia di quali migrazioni sono state applicate al database. Visualizzare i dati nella tabella `__EFMigrationsHistory`: viene visualizzata una riga per la prima migrazione. Nell'ultimo log nell'esempio di output della CLI precedente viene visualizzata l'istruzione INSERT che crea tale riga.

Eseguire l'app e verificare che tutto funzioni.

## <a name="applying-migrations-in-production"></a>Applicazione delle migrazioni nell'ambiente di produzione

È consigliabile fare in modo che le app nell'ambiente di produzione **non** chiamino [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) all'avvio dell'applicazione. L'elemento `Migrate` non deve essere chiamato da un'app nella server farm. Ad esempio, se l'app è stata distribuita in un cloud con scale-out, ovvero se sono in esecuzione più istanze dell'app.

La migrazione del database deve essere eseguita come parte della distribuzione e in modo controllato. Gli approcci alla migrazione di database in ambiente di produzione includono:

* Uso delle migrazioni per creare script SQL e uso degli script SQL nella distribuzione.
* Esecuzione di `dotnet ef database update` da un ambiente controllato.

EF Core usa la tabella `__MigrationsHistory` per stabilire se è necessario eseguire migrazioni. Se il database è aggiornato, non viene eseguita alcuna migrazione.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Interfaccia della riga di comando e console di Gestione pacchetti

Gli strumenti di EF Core per la gestione delle migrazioni sono disponibili da:

* Comandi dell'interfaccia della riga di comando di .NET Core.
* Cmdlet di PowerShell nella finestra di Visual Studio **Console di Gestione pacchetti**.

In questa esercitazione viene illustrato come usare l'interfaccia della riga di comando. Alcuni sviluppatori preferiscono usare invece la console di Gestione pacchetti.

I comandi di EF Core per la console di Gestione pacchetti sono inclusi nel pacchetto [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools). Il pacchetto è incluso nel metapacchetto [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), quindi non è necessario installarlo.

**Importante:** non si tratta dello stesso pacchetto che si installa per l'interfaccia della riga di comando modificando il file con estensione *.csproj*. Il nome di questo pacchetto termina con `Tools`, a differenza del nome del pacchetto della CLI che termina con `Tools.DotNet`.

Per altre informazioni sui comandi della CLI, vedere [Strumenti da riga di comando di EF Core .NET](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Per altre informazioni sui comandi della console di Gestione pacchetti, vedere [Console di Gestione pacchetti (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Risoluzione dei problemi

Scaricare l'[app completata per questa fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

L'app genera l'eccezione seguente:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Soluzione: eseguire `dotnet ef database update`

Se il comando `update` restituisce l'errore "Compilazione non riuscita.":

* Eseguire nuovamente il comando.
* Lasciare un messaggio in fondo alla pagina.

> [!div class="step-by-step"]
> [Precedente](xref:data/ef-rp/sort-filter-page)
> [Successivo](xref:data/ef-rp/complex-data-model)
