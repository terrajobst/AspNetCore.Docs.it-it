---
title: Pagine Razor con Entity Framework Core - Migrations - 4 di 8
author: rick-anderson
description: "In questa esercitazione, iniziare a usare la funzionalità di migrazioni EF Core per la gestione delle modifiche al modello di dati in un'applicazione ASP.NET MVC di base."
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 26fbda99b0c1dfa2d09cf387e43f3123c58215f8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a>Migrazioni - Core EF esercitazione pagine Razor (4 di 8)

Da [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

In questa esercitazione viene utilizzata la funzionalità di migrazioni EF Core per la gestione delle modifiche al modello di dati.

Se si verificano problemi, è possibile risolvere, scaricare il [app completata per questa fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Quando è stata sviluppata un'app, i modello di dati spesso. Ogni volta che le modifiche del modello, il modello Ottiene sincronizzato con il database. In questa esercitazione avviata tramite la configurazione di Entity Framework per creare il database se non esiste. Ogni volta che i modello di dati:

* Il database viene eliminato.
* EF crea una nuova istanza che corrisponde al modello.
* L'app esegue il seeding del database con dati di test.

Questo approccio per mantenere sincronizzati con il modello di dati del database funziona bene fino a quando non si distribuisce l'app nell'ambiente di produzione. Quando l'app è in esecuzione nell'ambiente di produzione, in genere l'archiviazione dei dati che devono essere conservate. L'app non può iniziare con un test di database ogni volta che viene apportata una modifica (ad esempio aggiungendo una nuova colonna). La funzionalità di Entity Framework Core migrazioni risolve questo problema abilitando EF Core aggiornare lo schema di database anziché creare un nuovo database.

Anziché eliminare e ricreare il database quando le modifiche del modello di dati, le migrazioni Aggiorna lo schema e mantiene i dati esistenti.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Pacchetti NuGet di Entity Framework Core per le migrazioni

Per lavorare con le migrazioni, utilizzare il **Package Manager Console** (PMC) o l'interfaccia della riga di comando (CLI). Queste esercitazioni viene illustrato come utilizzare i comandi CLI. Informazioni sul PMC, vedere [la fine di questa esercitazione](#pmc).

Vengono forniti gli strumenti di Entity Framework Core per l'interfaccia della riga di comando (CLI) in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Per installare questo pacchetto, aggiungerlo al `DotNetCliToolReference` insieme il *csproj* file, come illustrato. **Nota:** il pacchetto deve essere installato mediante la modifica di *csproj* file. Il`install-package` comando o la GUI di gestione di pacchetti non può essere utilizzata per installare questo pacchetto. Modificare il *csproj* file facendo clic con il nome del progetto in **Esplora** e selezionando **ContosoUniversity.csproj modifica**.

Di seguito viene illustrato l'aggiornamento *csproj* file con gli strumenti di Entity Framework Core CLI evidenziato:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
I numeri di versione nell'esempio precedente sono stati correnti quando l'esercitazione è stato scritto. Utilizzare la stessa versione per gli strumenti di Entity Framework Core CLI, vedere gli altri pacchetti.

## <a name="change-the-connection-string"></a>Modificare la stringa di connessione

Nel *appSettings. JSON* file, modificare il nome del database nella stringa di connessione in ContosoUniversity2.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Se si modifica il nome di database nella stringa di connessione, la migrazione prima di creare un nuovo database. Poiché non ne esiste uno con lo stesso nome, viene creato un nuovo database. Modifica la stringa di connessione non è necessaria per iniziare a usare le migrazioni.

Un'alternativa alla modifica del nome di database sta eliminando il database. Utilizzare **Esplora oggetti di SQL Server** (sillaba SSOX) o `database drop` comando CLI:

 ```console
 dotnet ef database drop
 ```

Nella sezione seguente viene illustrato come eseguire i comandi CLI.

## <a name="create-an-initial-migration"></a>Creazione di una migrazione iniziale

Compilare il progetto.

Aprire una finestra di comando e passare alla cartella del progetto. La cartella di progetto contiene il *Startup.cs* file.

Nella finestra di comando, immettere quanto segue:

```console
dotnet ef migrations add InitialCreate
```

La finestra di comando consente di visualizzare informazioni simile al seguente:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Se la migrazione non riesce con il messaggio "*Impossibile accedere al file... ContosoUniversity.dll perché è utilizzato da un altro processo.* " viene visualizzato:

* Arrestare IIS Express.

   * Chiudere e riavviare Visual Studio, o
   * Nella barra delle applicazioni di Windows, individuare l'icona di IIS Express.
   * Fare doppio clic sull'icona di IIS Express e quindi fare clic su **ContosoUniversity > Arresta sito**.

Se il messaggio di errore "generazione non riuscita." viene visualizzato, eseguire nuovamente il comando. Se questo errore si verifica, lasciare una nota nella parte inferiore di questa esercitazione.

### <a name="examine-the-up-and-down-methods"></a>Esaminare l'alto e verso il basso di metodi

Il comando Core EF `migrations add` generato codice per il database da creare. Questo codice migrazioni è il *migrazioni\<timestamp > _InitialCreate.cs* file. Il `Up` metodo la `InitialCreate` classe crea le tabelle di database che corrispondono al set di entità del modello di dati. Il `Down` metodo eliminati, come illustrato nell'esempio seguente:

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Chiamate di migrazioni di `Up` metodo per implementare le modifiche del modello di dati per la migrazione. Quando si immette un comando per annullare l'aggiornamento, le chiamate di migrazioni di `Down` metodo.

Il codice precedente è per la migrazione iniziale. Tale codice è stato creato quando il `migrations add InitialCreate` comando è stato eseguito. Il parametro di nome migrazione ("InitialCreate" nell'esempio) viene utilizzato per il nome del file. Il nome della migrazione può essere qualsiasi nome di file valido. È consigliabile scegliere una parola o frase che riepiloga le quali viene eseguita la migrazione. Ad esempio, una migrazione aggiunto una tabella di reparto, denominata "AddDepartmentTable".

Se la migrazione iniziale viene creata e il database viene chiuso:

* Viene generato il codice di creazione del database.
* Il codice di creazione database non è necessario eseguire perché il database è già corrisponde al modello di dati. Se viene eseguito il codice di creazione del database, non apporta alcuna modifica perché il database è già corrisponde al modello di dati.

Quando l'applicazione viene distribuita in un nuovo ambiente, è necessario eseguire il codice di creazione del database per creare il database.

La stringa di connessione in precedenza è stata modificata per l'utilizzo di un nuovo nome per il database. Il database specificato non esiste, pertanto le migrazioni creerà il database.

### <a name="examine-the-data-model-snapshot"></a>Esaminare lo snapshot del modello di dati

Le migrazioni crea un *snapshot* dello schema del database corrente in *Migrations/SchoolContextModelSnapshot.cs*:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Poiché lo schema di database corrente è rappresentato nel codice, Core EF non deve interagire con il database per creare le migrazioni. Quando si aggiunge una migrazione, Core EF determina le modifiche confrontando il modello di dati del file di snapshot. Core EF interagisce con il database solo quando è necessario aggiornare il database.

Il file di snapshot deve essere sincronizzato con le migrazioni che li ha creati. Impossibile rimuovere una migrazione eliminando il file denominato  *\<timestamp > _\<migrationname >. cs*. Se tale file viene eliminato, le migrazioni rimanenti sono sincronizzate con il file di snapshot di database. Per eliminare l'ultima migrazione aggiunto, usare il [migrazioni ef dotnet rimuovere](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) comando.

## <a name="remove-ensurecreated"></a>Rimuovere EnsureCreated

Per lo sviluppo iniziale, il `EnsureCreated` comando è stato utilizzato. In questa esercitazione viene utilizzato le migrazioni. `EnsureCreated`è il seguente limatitions:

* Ignora le migrazioni e crea il database e lo schema.
* Non crea una tabella delle migrazioni.
* Possibile *non* utilizzabile con le migrazioni.
* È progettato per prototipi di test o rapida in cui il database viene eliminato e ricreato frequentemente.

Rimuovere la riga seguente da `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Applicare la migrazione al database in fase di sviluppo

Nella finestra di comando, immettere il comando seguente per creare il database e tabelle.

```console
dotnet ef database update
```

Nota: Se il `update` comando restituisce l'errore "La compilazione non riuscita.":

* Eseguire nuovamente il comando.
* Se non viene completato, uscire da Visual Studio e quindi eseguire il `update` comando.
* Lasciare un messaggio nella parte inferiore della pagina.

L'output del comando è simile al `migrations add` comando output. Nel comando precedente, vengono visualizzati i registri per i comandi SQL che imposta backup del database. La maggior parte dei log vengono omessi negli output di esempio seguente:

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

Per ridurre il livello di dettaglio nei messaggi di log, può modificare i livelli di log di *appsettings. Development.JSON* file. Per ulteriori informazioni, vedere [Introduzione a registrazione](xref:fundamentals/logging/index).

Utilizzare **Esplora oggetti di SQL Server** per controllare il database. Si noti l'aggiunta di un `__EFMigrationsHistory` tabella. Il `__EFMigrationsHistory` tabella tiene traccia di quali migrazioni sono state applicate al database. Visualizzare i dati di `__EFMigrationsHistory` tabella mostra una riga per la migrazione prima. L'ultimo log nell'esempio di output CLI precedente mostra l'istruzione INSERT che consente di creare questa riga.

Eseguire l'applicazione e verificare che tutto funzioni.

## <a name="appling-migrations-in-production"></a>Migrazioni di applicazione nell'ambiente di produzione

Si consiglia di App di produzione deve **non** chiamare [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) all'avvio dell'applicazione. `Migrate`non deve essere chiamato da un'app nella server farm. Ad esempio, se l'app è stato cloud distribuito con scalabilità orizzontale (più istanze dell'app sono in esecuzione).

Migrazione del database deve essere eseguita come parte della distribuzione in modo controllato. Approcci di migrazione di database di produzione includono:

* Usando le migrazioni per creare script SQL e gli script SQL nella distribuzione.
* Esecuzione `dotnet ef database update` da un ambiente controllato.

Core EF utilizza il `__MigrationsHistory` tabella per vedere se è necessario eseguire le migrazioni. Se il database viene aggiornato, non viene eseguita alcuna migrazione.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Visual Studio interfaccia della riga di comando (CLI). Console di gestione pacchetti (PMC)

Il nucleo di Entity Framework gli strumenti per la gestione delle migrazioni sono disponibile da:

* Comandi di .NET core CLI.
* I cmdlet di PowerShell in Visual Studio **Package Manager Console** finestra (PMC).

In questa esercitazione viene illustrato come utilizzare l'interfaccia CLI, alcuni sviluppatori preferiscono utilizzare PMC.

I comandi di base di Entity Framework per PMC presenti il [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pacchetto. Questo pacchetto è incluso nel [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, pertanto non è necessario installarlo.

**Importante:** non è il pacchetto stesso di quello in cui si installa per l'interfaccia CLI modificando il *csproj* file. Il nome di questo termina `Tools`, a differenza del nome di pacchetto CLI che termina in `Tools.DotNet`.

Per ulteriori informazioni sui comandi CLI, vedere [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Per ulteriori informazioni sui comandi PMC, vedere [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Risoluzione dei problemi

Scaricare il [app completata per questa fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

L'applicazione genera l'eccezione seguente:

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Soluzione: eseguire`dotnet ef database update`

Se il `update` comando restituisce l'errore "La compilazione non riuscita.":

* Eseguire nuovamente il comando.
* Lasciare un messaggio nella parte inferiore della pagina.

>[!div class="step-by-step"]
[Precedente](xref:data/ef-rp/sort-filter-page)
[Successivo](xref:data/ef-rp/complex-data-model)
