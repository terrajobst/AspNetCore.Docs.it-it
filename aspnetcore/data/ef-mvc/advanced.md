---
title: Core di ASP.NET MVC con Entity Framework di base - avanzate - 10 di 10
author: tdykstra
description: Questa esercitazione vengono descritti i diversi argomenti utili da tenere presenti quando superano le nozioni di base dello sviluppo di applicazioni web ASP.NET che utilizzano componenti di base di Entity Framework.
keywords: "ASP.NET Core, Entity Framework Core, sql non elaborati, esaminare sql, modello di repository, l'unità di lavoro modello, il rilevamento delle modifiche automatico, database esistente"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 92a2986a-d005-4ff6-9559-6657fd466bb7
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: fa5e0a66f22cc14f34d05481ce2e4381085d122d
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2017
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a>Argomenti avanzati - EF Core con l'esercitazione di base di ASP.NET MVC (10 di 10)

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).

Nell'esercitazione precedente, l'implementazione è ereditarietà tabella per gerarchia. Questa esercitazione vengono descritti i diversi argomenti utili da tenere presenti quando superano le nozioni di base dello sviluppo di applicazioni web ASP.NET di base che usa Entity Framework Core.

## <a name="raw-sql-queries"></a>Query SQL non elaborato

Uno dei vantaggi dell'utilizzo di Entity Framework è che evita abbinata codice posizione troppo ravvicinata a un metodo particolare per l'archiviazione dei dati. Ciò avviene mediante la generazione di comandi e query SQL, che consente inoltre di dover scrivere manualmente. Ma esistono scenari eccezionali, quando è necessario eseguire query SQL specifiche che è stato creato manualmente. Per questi scenari, l'API di Entity Framework codice prima include metodi che consentono di passare i comandi SQL direttamente al database. Sono disponibili le opzioni seguenti in EF Core 1.0:

* Utilizzare il `DbSet.FromSql` metodo per le query che restituiscono tipi di entità. Gli oggetti restituiti devono essere del tipo previsto per il `DbSet` oggetto e vengono rilevate automaticamente dal contesto di database a meno che non si [Disattiva rilevamento](crud.md#no-tracking-queries).

* Utilizzare il `Database.ExecuteSqlCommand` per i comandi non query.

Se è necessario eseguire una query che restituisce i tipi che non sono entità, è possibile utilizzare ADO.NET con la connessione al database fornita da Entity Framework. I dati restituiti non sono rilevati dal contesto del database, anche se si utilizza questo metodo per recuperare i tipi di entità.

Come è sempre true per l'esecuzione di comandi SQL in un'applicazione web, è necessario adottare le precauzioni necessarie per proteggere il sito da attacchi SQL injection. Un modo per eseguire questa operazione consiste nell'utilizzare le query con parametri per assicurarsi che le stringhe inviate da una pagina web non possono essere interpretate come comandi SQL. In questa esercitazione si userà le query con parametri per l'integrazione di input dell'utente in una query.

## <a name="call-a-query-that-returns-entities"></a>Chiamare una query che restituisce le entità

Il `DbSet<TEntity>` classe fornisce un metodo che è possibile utilizzare per eseguire una query che restituisce un'entità di tipo `TEntity`. Per visualizzarne il funzionamento si modificherà il codice nel `Details` metodo del controller di reparto.

In *DepartmentsController.cs*nella `Details` (metodo), sostituire il codice che recupera un reparto con un `FromSql` chiamata al metodo, come illustrato nel codice evidenziato seguente:

[!code-csharp[Principale](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

Per verificare che il nuovo codice funziona correttamente, selezionare il **reparti** scheda e quindi **dettagli** per uno dei servizi.

![Dettagli di reparto](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a>Chiamare una query che restituisce gli altri tipi

È stato creato in precedenza una griglia delle statistiche di studenti per la pagina di informazioni che è stato illustrato il numero di studenti per ogni data di registrazione. I dati ottenuti dal set di entità studenti (`_context.Students`) e utilizzato LINQ per proiettare i risultati in un elenco di `EnrollmentDateGroup` consente di visualizzare gli oggetti del modello. Si supponga di che voler scrivere un'istruzione SQL se stesso, anziché tramite LINQ. Per eseguire l'operazione che si desidera eseguire una query SQL che restituisce un valore diverso da oggetti entità. In Entity Framework Core 1.0, è possibile eseguire questa operazione è scrivere codice ADO.NET e ottenere la connessione al database da Entity Framework.

In *HomeController.cs*, sostituire il `About` (metodo) con il codice seguente:

[!code-csharp[Principale](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

Aggiungere un tramite istruzione:

[!code-csharp[Principale](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

Eseguire la pagina di informazioni. Visualizza gli stessi dati che non è stato modificato.

![Informazioni sulla pagina](advanced/_static/about.png)

## <a name="call-an-update-query"></a>Chiamare una query di aggiornamento

Si supponga che gli amministratori University Contoso desiderano eseguire modifiche globali nel database, ad esempio il numero di crediti per ogni corso di modifica. Università dispone di un numero elevato di corsi, sarebbe opportuno recuperarli tutti come entità e modificarli singolarmente. In questa sezione viene implementato una pagina web che consente all'utente di specificare un fattore in base a cui si desidera modificare il numero di crediti per tutti i corsi e ti la modifica tramite l'esecuzione di un'istruzione SQL UPDATE. La pagina web sarà simile al seguente:

![Pagina crediti corso di aggiornamento](advanced/_static/update-credits.png)

In *CoursesContoller.cs*, aggiungere metodi UpdateCourseCredits per HttpGet e HttpPost:

[!code-csharp[Principale](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Principale](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

Quando il controller elabora una richiesta di HttpGet, non viene restituito `ViewData["RowsAffected"]`, e la visualizzazione consente di visualizzare una casella di testo e un pulsante di invio, come illustrato nella figura precedente.

Quando il **aggiornamento** si fa clic sul pulsante, viene chiamato il metodo HttpPost e moltiplicatore con il valore immesso nella casella di testo. Il codice quindi viene eseguita la query SQL che aggiorna corsi e restituisce il numero di righe interessate alla vista `ViewData`. Quando la vista Ottiene un `RowsAffected` valore, viene visualizzato il numero di righe aggiornate.

In **Esplora**, fare doppio clic su di *viste/corsi* cartella e quindi fare clic su **Aggiungi > Nuovo elemento**.

Nel **Aggiungi nuovo elemento** finestra di dialogo, fare clic su **ASP.NET** in **installato** nel riquadro a sinistra, fare clic su **pagina visualizzazione MVC**e denominare la nuova vista  *UpdateCourseCredits.cshtml*.

In *Views/Courses/UpdateCourseCredits.cshtml*, sostituire il codice del modello con il codice seguente:

[!code-html[Principale](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

Eseguire il `UpdateCourseCredits` metodo selezionando il **corsi** scheda, quindi aggiungendo "/ UpdateCourseCredits" alla fine dell'URL nella barra degli indirizzi del browser (ad esempio: `http://localhost:5813/Courses/UpdateCourseCredits`). Immettere un numero nella casella di testo:

![Pagina crediti corso di aggiornamento](advanced/_static/update-credits.png)

Fare clic su **Aggiorna**. Visualizzare il numero di righe interessate:

![Crediti corso di aggiornamento pagina righe interessate](advanced/_static/update-credits-rows-affected.png)

Fare clic su **elenco** per visualizzare l'elenco dei corsi con il numero di crediti modificato.

Si noti che il codice di produzione di assicurarsi che aggiorna sempre risultato nei dati validi. Il codice semplificato qui Impossibile moltiplicare il numero di crediti sufficientemente dare luogo a numeri maggiori di 5. (Il `Credits` proprietà ha un `[Range(0, 5)]` attributo.) La query di aggiornamento funzionerà, ma i dati non validi può provocare risultati imprevisti in altre parti del sistema che si presuppone che il numero di crediti sia minore o uguale a 5.

Per ulteriori informazioni sulle query SQL non elaborate, vedere [query SQL non elaborati](https://docs.microsoft.com/ef/core/querying/raw-sql).

## <a name="examine-sql-sent-to-the-database"></a>Esaminare inviate al database SQL

A volte risulta utile essere in grado di visualizzare le query SQL effettive vengono inviate al database. La funzionalità di registrazione predefinito per ASP.NET Core usata automaticamente da Entity Framework Core per scrivere i registri che contengono il codice SQL della query e aggiornamenti. In questa sezione si noterà alcuni esempi di registrazione SQL.

Aprire *StudentsController.cs* e il `Details` metodo imposta un punto di interruzione il `if (student == null)` istruzione.

Eseguire l'applicazione in modalità di debug e passare alla pagina dei dettagli per uno studente.

Passare al **Output** finestra che mostra il debug di output e viene visualizzata la query:

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

Si noterà nulla che potrebbe sorprendere: SQL seleziona fino a 2 righe (`TOP(2)`) dalla tabella Person. Il `SingleOrDefaultAsync` metodo non si risolve in 1 riga nel server. Ecco perché:

* Se la query restituisce più righe, il metodo restituisce null.
* Per determinare se la query restituisce più righe, EF deve controllare se viene restituito almeno 2.

Si noti che non è necessario utilizzare la modalità debug e termina a un punto di interruzione per ottenere l'output di registrazione di **Output** finestra. È semplicemente un modo pratico per arrestare la registrazione nel punto di cui che si desidera esaminare l'output. Se è non farlo, continua la registrazione ed è necessario scorrere indietro per trovare le parti che si è interessati.

## <a name="repository-and-unit-of-work-patterns"></a>Repository e un'unità di lavoro modelli

Molti sviluppatori di scrivono codice per implementare il repository e l'unità di lavoro modelli come wrapper attorno al codice che interagisce con Entity Framework. Questi modelli di cui si intendono creare un livello di astrazione tra il livello di accesso ai dati e il livello di logica di business di un'applicazione. L'implementazione di questi modelli consentono di isolare l'applicazione dalle modifiche nell'archivio dati e possono facilitare lo sviluppo basato su test (TDD) o unit test automatici. Tuttavia, la scrittura di codice aggiuntivo per l'implementazione di questi modelli non è sempre la scelta ideale per applicazioni che usano Entity Framework, per diversi motivi:

* La classe di contesto EF stessa isola del codice dal codice specifiche dell'archivio dati.

* La classe del contesto EF può fungere da una classe di unità di lavoro per il database Aggiorna effettuare mediante EF.

* EF include funzionalità per l'implementazione TDD senza scrivere codice di repository.

Per informazioni su come implementare il repository e l'unità di lavoro modelli, vedere [la versione di Entity Framework 5 di questa serie di esercitazioni](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).

Componenti di base di Entity Framework implementa un provider di database in memoria che può essere usato per il test. Per ulteriori informazioni, vedere [test con InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).

## <a name="automatic-change-detection"></a>Rilevamento delle modifiche automatico

Entity Framework determina come è stata modificata un'entità (e pertanto gli aggiornamenti devono essere inviati al database), confrontando i valori correnti di un'entità con i valori originali. Quando l'entità viene eseguita una query o collegato, vengono archiviati i valori originali. Alcuni dei metodi che causano il rilevamento delle modifiche automatico sono i seguenti:

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker

Se si desidera tenere traccia di un numero elevato di entità e viene chiamato uno di questi metodi più volte in un ciclo, è possibile ottenere miglioramenti significativi delle prestazioni disattivando temporaneamente il rilevamento delle modifiche automatico utilizzando il `ChangeTracker.AutoDetectChangesEnabled` proprietà. Ad esempio:

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a>Entity Framework Core codice e lo sviluppo piani di origine

Il codice sorgente per Entity Framework Core è disponibile all'indirizzo [https://github.com/aspnet/EntityFramework](https://github.com/aspnet/EntityFramework). Oltre a codice sorgente, è possibile ottenere le compilazioni notturne, Gestione problemi, funzionalità specifiche, note, progettare [Guida di orientamento per lo sviluppo futuro](https://github.com/aspnet/EntityFramework/wiki/Roadmap)e altro ancora. È possibile segnalare i bug ed è possibile fornire il propria miglioramenti per il codice sorgente di Entity Framework.

Anche se il codice sorgente è aperto, Entity Framework Core è completamente supportata come un prodotto Microsoft. Il team Microsoft Entity Framework mantiene controllo su cui vengono accettati i contributi e verifica di tutte le modifiche al codice per assicurare la qualità di ogni rilascio.

## <a name="reverse-engineer-from-existing-database"></a>Decodificare un database esistente

Per decodificare un modello di dati, incluse le classi di entità da un database esistente, utilizzare il [scaffold dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) comando. Vedere il [esercitazione introduttiva](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).

<a id="dynamic-linq">
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a>Utilizzo di LINQ dinamica per semplificare il codice di selezione di ordinamento

Il [terza esercitazione di questa serie](sort-filter-page.md) viene illustrato come scrivere codice LINQ da nomi di colonne a livello di codice in un `switch` istruzione. Con due colonne da selezionare, questa procedura funziona correttamente, ma se si dispone di un numero di colonne è stato possibile ottenere dettagliato il codice. Per risolvere il problema, è possibile utilizzare il `EF.Property` metodo per specificare il nome della proprietà come stringa. Per provare a utilizzare questo approccio, sostituire il `Index` metodo il `StudentsController` con il codice seguente.

[!code-csharp[Principale](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a>Passaggi successivi

Questa serie di esercitazioni sull'uso di Entity Framework Core in un'applicazione MVC ASP.NET è stata completata.

Per ulteriori informazioni sulla base di Entity Framework, vedere il [documentazione di Entity Framework Core](https://docs.microsoft.com/ef/core). È disponibile anche un libro: [Entity Framework Core nell'azione](https://www.manning.com/books/entity-framework-core-in-action).

Per informazioni su come distribuire l'applicazione web dopo aver creato, vedere [pubblicazione e distribuzione](../../publishing/index.md).

Per informazioni su altri argomenti relativi a ASP.NET MVC di base, quali l'autenticazione e autorizzazione, vedere il [documentazione di ASP.NET Core](https://docs.microsoft.com/aspnet/core/).

## <a name="acknowledgments"></a>Riconoscimenti

Tom Dykstra e Rick Anderson (twitter @RickAndMSFT) ha scritto in questa esercitazione. Rowan Miller, Diego Vega e altri membri del team di Entity Framework assistiti da con le revisioni del codice che ha contribuito il debug di problemi che si è verificato mentre stessimo scrivendo codice per le esercitazioni.

## <a name="common-errors"></a>Errori più comuni  

### <a name="contosouniversitydll-used-by-another-process"></a>ContosoUniversity.dll utilizzato da un altro processo

Messaggio di errore:

> Impossibile aprire '... bin\Debug\netcoreapp1.0\ContosoUniversity.dll' per la scrittura: "il processo non è possibile accedere al file '... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll' perché è utilizzato da un altro processo.

Soluzione:

Arrestare il sito in IIS Express. Accesso a barra delle applicazioni di Windows, per trovare IIS Express e fare doppio clic sull'icona, selezionare il sito Contoso University e quindi fare clic su **arresta sito**.

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>Migrazione di scaffolding senza codice in su e giù metodi

Causa possibile:

I comandi CLI EF non chiudere automaticamente e salvare i file di codice. Se le modifiche non salvate le modifiche quando si esegue il `migrations add` comando EF non troverà le modifiche.

Soluzione:

Eseguire il `migrations remove` comando, salvare le modifiche al codice e rieseguire il `migrations add` comando.

### <a name="errors-while-running-database-update"></a>Errori durante l'aggiornamento del database in esecuzione

È possibile ottenere altri errori quando si apportano modifiche allo schema in un database che contiene dati esistenti. Se si verificano errori di migrazione che non è possibile risolvere, è possibile modificare il nome del database nella stringa di connessione o eliminare il database. Con un nuovo database, non sono presenti dati per eseguire la migrazione e il comando update-database è molto più probabile completata senza errori.

L'approccio più semplice consiste nel rinominare il database in *appSettings. JSON*. Alla successiva esecuzione `database update`, verrà creato un nuovo database.

Per eliminare un database in sillaba SSOX, il pulsante destro del database, fare clic su **eliminare**e quindi la **Elimina Database** finestra di dialogo selezionare **Chiudi connessioni esistenti** e fare clic su  **OK**.

Per eliminare un database tramite l'interfaccia CLI, eseguire il `database drop` comando CLI:

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>Istanza di SQL Server individuazione errore

Messaggio di errore:

> Si è verificato un errore di rete o specifico dell'istanza durante la connessione a SQL Server. Il server non è stato trovato o non è accessibile. Verificare che il nome dell'istanza sia corretto e che SQL Server sia configurato in modo da consentire connessioni remote. (provider: interfacce di rete SQL, errore: 26 - errore nell'individuazione Server / dell'istanza specificati)

Soluzione:

Controllare la stringa di connessione. Se è stato eliminato manualmente il file di database, modificare il nome del database nella stringa di costruzione per ricominciare con un nuovo database.

>[!div class="step-by-step"]
[Precedente](inheritance.md)
