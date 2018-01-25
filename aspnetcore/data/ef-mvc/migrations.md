---
title: Core di ASP.NET MVC con Entity Framework Core - Migrations - 4 di 10
author: tdykstra
description: "In questa esercitazione, iniziare a usare la funzionalità di migrazioni EF Core per la gestione delle modifiche al modello di dati in un'applicazione ASP.NET MVC di base."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: a2f8b01e16d1be818b4338455a40605fcbdb3400
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>Migrazioni - EF Core con l'esercitazione di base di ASP.NET MVC (4 di 10)

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).

In questa esercitazione, iniziare a usare la funzionalità di migrazioni EF Core per la gestione delle modifiche al modello di dati. Nelle esercitazioni successive, si aggiungeranno ulteriori migrazioni quando si modifica il modello di dati.

## <a name="introduction-to-migrations"></a>Introduzione alle migrazioni

Quando si sviluppa una nuova applicazione, il modello di dati cambia di frequente e ogni volta che le modifiche del modello, viene sincronizzato con il database. Queste esercitazioni è stato avviato tramite la configurazione di Entity Framework per creare il database se non esiste. Quindi, ogni volta che si modifica il modello di dati - aggiungere, rimuovere, o modificare le classi di entità o modifica la classe DbContext - è possibile eliminare il database ed EF crea una nuova istanza che corrisponde al modello e si esegue il seeding con dati di test.

Questo metodo di sincronizzazione del database con il modello di dati funziona bene fino a quando non si distribuisce l'applicazione nell'ambiente di produzione. Quando l'applicazione è in esecuzione nell'ambiente di produzione che è in genere l'archiviazione di dati che si desidera mantenere e non si desidera perdere tutti gli elementi ogni volta che si apporta una modifica ad esempio l'aggiunta di una nuova colonna. La funzionalità di Entity Framework Core migrazioni risolve questo problema abilitando EF aggiornare lo schema del database anziché creare un nuovo database.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Pacchetti NuGet di Entity Framework Core per le migrazioni

Per lavorare con le migrazioni, è possibile utilizzare il **Package Manager Console** (PMC) o l'interfaccia della riga di comando (CLI).  Queste esercitazioni viene illustrato come utilizzare i comandi CLI. Informazioni sul PMC, vedere [la fine di questa esercitazione](#pmc).

Gli strumenti di Entity Framework per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Per installare questo pacchetto, aggiungerlo al `DotNetCliToolReference` insieme il *csproj* file, come illustrato. **Nota:** è necessario installare questo pacchetto modificando il file *.csproj* ; non è possibile utilizzare il comando `install-package` o la GUI di gestione di pacchetti. È possibile modificare il *csproj* file facendo clic con il nome del progetto in **Esplora** e selezionando **ContosoUniversity.csproj modifica**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(I numeri di versione in questo esempio sono stati correnti quando l'esercitazione è stato scritto.) 

## <a name="change-the-connection-string"></a>Modificare la stringa di connessione

Nel *appSettings. JSON* file, modificare il nome del database nella stringa di connessione in ContosoUniversity2 o un altro nome che non è stato utilizzato nel computer in uso.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Questa modifica si configura il progetto in modo che la migrazione prima creerà un nuovo database. Questo non è necessario per iniziare a usare le migrazioni, ma verrà visualizzato in un secondo momento perché è una buona idea.

> [!NOTE]
> Come alternativa alla modifica del nome di database, è possibile eliminare il database. Utilizzare **Esplora oggetti di SQL Server** (sillaba SSOX) o `database drop` comando CLI:
> ```console
> dotnet ef database drop
> ```
> Nella sezione seguente viene illustrato come eseguire i comandi CLI.

## <a name="create-an-initial-migration"></a>Creazione di una migrazione iniziale

Salvare le modifiche e compilare il progetto. Quindi, aprire una finestra di comando e passare alla cartella del progetto. Ecco un modo rapido per eseguire questa operazione:

* In **Esplora**, fare clic sul progetto e scegliere **Apri in Esplora File** dal menu di scelta rapida.

  ![Apri nella voce di menu di Esplora File](migrations/_static/open-in-file-explorer.png)

* Immettere "cmd" nella barra degli indirizzi e premere INVIO.

  ![Apri finestra di comando](migrations/_static/open-command-window.png)

Immettere il comando seguente nella finestra Comando:

```console
dotnet ef migrations add InitialCreate
```

È visualizzato un output simile al seguente nella finestra di comando:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Se viene visualizzato un messaggio di errore *alcun eseguibile non trovato corrispondente comando "dotnet-ef"*, vedere [questo post di blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) per la risoluzione dei problemi della Guida.

Se viene visualizzato un messaggio di errore "*Impossibile accedere al file... ContosoUniversity.dll perché è utilizzato da un altro processo.* ", individuare l'icona di IIS Express nella barra delle applicazioni di Windows, pulsante destro del mouse, quindi fare clic su **ContosoUniversity > sito**.

## <a name="examine-the-up-and-down-methods"></a>Esaminare l'alto e verso il basso di metodi

Quando è stata eseguita la `migrations add` comando EF ha generato il codice che verrà creato il database da zero. Questo codice è il *migrazioni* cartella, nel file denominato  *\<timestamp > _InitialCreate.cs*. Il `Up` metodo il `InitialCreate` classe crea le tabelle di database che corrispondono ai set di entità del modello di dati, e `Down` metodo eliminati, come illustrato nell'esempio seguente.

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Chiamate di migrazioni di `Up` metodo per implementare le modifiche del modello di dati per la migrazione. Quando si immette un comando per annullare l'aggiornamento, le chiamate di migrazioni di `Down` metodo.

Questo codice è per la migrazione iniziale creato al momento dell'immissione di `migrations add InitialCreate` comando. Il parametro name di migrazione ("InitialCreate" nell'esempio) viene utilizzato per il nome del file e può essere elementi desiderati. È consigliabile scegliere una parola o frase che riepiloga le quali viene eseguita la migrazione. Ad esempio, è possibile denominare una migrazione successiva "AddDepartmentTable".

Se la migrazione iniziale è stato creato quando il database esiste già, viene generato il codice di creazione del database, ma non è necessario eseguire perché il database è già corrisponde il modello di dati. Quando si distribuisce l'app in un altro ambiente in cui il database non esiste ancora, questo codice verrà eseguito per creare il database, è consigliabile prima di tutto testare. Ecco perché è stato modificato il nome del database nella stringa di connessione in precedenza, in modo che le migrazioni possano crearne uno nuovo da zero.

## <a name="examine-the-data-model-snapshot"></a>Esaminare lo snapshot del modello di dati

Le migrazioni crea anche un *snapshot* dello schema del database corrente in *Migrations/SchoolContextModelSnapshot.cs*. Ecco come appare il codice:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Poiché lo schema del database corrente è rappresentato nel codice, Core EF non deve interagire con il database per creare le migrazioni. Quando si aggiunge una migrazione, EF determina le modifiche confrontando il modello di dati del file di snapshot. EF interagisce con il database solo quando è necessario aggiornare il database. 

Il file di snapshot deve essere mantenuta sincronizzata con le migrazioni che creano, pertanto non è possibile rimuovere una migrazione solo eliminando il file denominato  *\<timestamp > _\<migrationname >. cs*. Se si elimina il file, le migrazioni rimanenti sarà sincronizzate con il file di snapshot di database. Per eliminare l'ultima migrazione che è stato aggiunto, usare il [migrazioni ef dotnet rimuovere](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) comando.

## <a name="apply-the-migration-to-the-database"></a>Applicare la migrazione al database

Nella finestra di comando, immettere il comando seguente per creare il database e le tabelle in esso.

```console
dotnet ef database update
```

L'output del comando è simile al `migrations add` comando, a eccezione del fatto che si registri per i comandi SQL che imposta backup del database. La maggior parte dei log vengono omessi negli output di esempio seguente. Se si preferisce non visualizzare questo livello di dettaglio nei messaggi di log, è possibile modificare il livello di registrazione nel *appsettings. Development.JSON* file. Per ulteriori informazioni, vedere [Introduzione a registrazione](xref:fundamentals/logging/index).

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

Utilizzare **Esplora oggetti di SQL Server** controllare il database, come indicato nella prima esercitazione.  Si noterà che l'aggiunta di una tabella __EFMigrationsHistory che tiene traccia di quali migrazioni sono state applicate al database. Verrà visualizzata una riga per la migrazione prima e visualizzare i dati della tabella. (L'ultimo log nell'esempio di output CLI precedente visualizza l'istruzione INSERT che crea la riga).

Eseguire l'applicazione per verificare che tutto funzioni ancora identico al precedente.

![Pagina di indice di studenti](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Visual Studio interfaccia della riga di comando (CLI). Console di gestione pacchetti (PMC)

Strumenti di Entity Framework per le migrazioni di gestione è disponibile dai comandi di .NET Core CLI o dai cmdlet di PowerShell in Visual Studio **Package Manager Console** finestra (PMC). In questa esercitazione viene illustrato come utilizzare l'interfaccia CLI, ma se si preferisce, è possibile utilizzare PMC.

I comandi di Entity Framework per i comandi PMC presenti il [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pacchetto. Questo pacchetto è già incluso nel [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, pertanto non è necessario installarlo.

**Importante:** questo non è il pacchetto stesso di quello in cui si installa per l'interfaccia CLI modificando il *csproj* file. Il nome di questo termina `Tools`, a differenza del nome di pacchetto CLI che termina in `Tools.DotNet`.

Per ulteriori informazioni sui comandi CLI, vedere [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet). 

Per ulteriori informazioni sui comandi PMC, vedere [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato spiegato come creare e applicare la migrazione prima. Nella prossima esercitazione, verrà iniziare esaminando argomenti più avanzati espandendo il modello di dati. Lungo il percorso viene creata e applicare ulteriori migrazioni.

>[!div class="step-by-step"]
[Precedente](sort-filter-page.md)
[Successivo](complex-data-model.md)  
