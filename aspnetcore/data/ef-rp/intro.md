---
title: 'Razor Pages con Entity Framework Core in ASP.NET Core: esercitazione 1 di 8'
author: rick-anderson
description: Viene illustrato come creare un'app Razor Pages con Entity Framework Core
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/intro
ms.openlocfilehash: cadf36f4e1ff3776ad4139e1d7c4e9b73687bc5c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279230"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Razor Pages con Entity Framework Core in ASP.NET Core: esercitazione 1 di 8

Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'app Web di esempio di Contoso University illustra come creare applicazioni Web ASP.NET Core 2.0 MVC con Entity Framework Core 2.0 e Visual Studio 2017.

L'app di esempio è un sito Web per una fittizia Contoso University. Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati. Questa pagina è il prima di una serie di esercitazioni che illustrano come compilare l'app di esempio di Contoso University.

[Scaricare o visualizzare l'app completata.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Istruzioni per il download](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [](~/includes/net-core-prereqs.md)]

Conoscenza di [Razor Pages](xref:razor-pages/index). Prima di iniziare questa serie, i programmatori non esperti dovranno completare l'[introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se si verifica un problema che non si sa come risolvere, è generalmente possibile trovare la soluzione confrontando il codice con la [fase completata](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots). Per un elenco degli errori comuni e delle relative soluzioni, vedere [la sezione relativa alla risoluzione dei problemi nell'ultima esercitazione della serie](xref:data/ef-mvc/advanced#common-errors). Se non si trova la soluzione, è possibile inviare una domanda a [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) per [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [Entity Framework Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Questa serie di esercitazioni si basa sulle attività eseguite nelle esercitazioni precedenti. È consigliabile salvare una copia del progetto dopo aver completato ogni esercitazione. Se si verificano problemi, è possibile ricominciare dall'esercitazione precedente anziché tornare all'inizio. In alternativa, è possibile scaricare una [ fase completa](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) e ricominciare usando questa fase.

## <a name="the-contoso-university-web-app"></a>App Web di Contoso University

L'applicazione compilata in queste esercitazioni è il sito Web di base di un'università.

Gli utenti possono visualizzare e aggiornare le informazioni che riguardano studenti, corsi e insegnanti. Di seguito sono disponibili alcune schermate dell'esercitazione.

![Pagina Student Index (Indice degli studenti)](intro/_static/students-index.png)

![Pagina Students Edit (Modifica studenti)](intro/_static/student-edit.png)

Lo stile dell'interfaccia utente del sito è simile a quanto è stato generato tramite i modelli predefiniti. L'esercitazione è incentrata su Entity Framework Core con Razor Pages e non sull'interfaccia utente.

## <a name="create-a-razor-pages-web-app"></a>Creare un'app Web di Razor Pages

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Creare una nuova applicazione Web ASP.NET Core. Denominare il progetto **ContosoUniversity**. È importante denominare il progetto *ContosoUniversity* in modo che gli spazi dei nomi corrispondano quando il codice viene copiato/incollato.
 ![nuova applicazione Web ASP.NET Core](intro/_static/np.png)
* Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.
 ![Applicazione Web (pagine Razor)](../../razor-pages/index/_static/np2.png)

Premere **F5** per eseguire l'app in modalità di debug o **Ctrl-F5** per l'esecuzione senza collegamento del debugger

## <a name="set-up-the-site-style"></a>Impostare lo stile del sito

Con alcune modifiche è possibile impostare il menu del sito, il layout e la home page.

Aprire *Pages/_Layout.cshtml* e apportare le modifiche seguenti:

* Modificare tutte le occorrenze di "ContosoUniversity" in "Contoso University." Le occorrenze sono tre.

* Aggiungere le voci di menu per **Students** (Studenti), **Courses** (Corsi), **Instructors** (Insegnanti) e **Departments** (Dipartimenti) ed eliminare la voce di menu **Contact** (Contatto).

Le modifiche vengono evidenziate. *Non* viene visualizzato l'intero markup.

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

In *Pages/Index.cshtml* sostituire il contenuto del file con il codice seguente. In questo modo il testo su ASP.NET e MVC sarà sostituito dal testo relativo a questa app:

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Premere CTRL+F5 per eseguire il progetto. La home page sarà visualizzata con le schede create nelle esercitazioni seguenti:

![Home page di Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>Creare il modello di dati

Creare le classi delle entità per l'app di Contoso University. Iniziare con le tre entità seguenti:

![Diagramma modello di dati Course/Enrollment/Student (Corso/Iscrizione/Studente)](intro/_static/data-model-diagram.png)

Esiste una relazione uno-a-molti tra le entità `Student` e `Enrollment`. Esiste una relazione uno-a-molti tra le entità `Course` e `Enrollment`. Uno studente può iscriversi a un numero qualsiasi di corsi. Un corso può avere un numero qualsiasi di studenti iscritti.

Nelle sezioni seguenti viene creata una classe per ognuna di queste entità.

### <a name="the-student-entity"></a>Entità Student (Studente)

![Diagramma entità Student (Studente)](intro/_static/student-entity.png)

Creare una cartella *Models* (Modelli). Nella cartella *Models* (Modelli) creare un file di classe denominato *Student.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

La proprietà `ID` diventa la colonna di chiave primaria della tabella di database (DB) che corrisponde a questa classe. Per impostazione predefinita, Entity Framework Core interpreta una proprietà denominata `ID` o `classnameID` come chiave primaria. In `classnameID` `classname` è il nome della classe, ad esempio `Student` nell'esempio precedente.

La proprietà `Enrollments` rappresenta una proprietà di navigazione. Le proprietà di navigazione si collegano ad altre entità correlate a questa entità. In questo caso la proprietà `Enrollments` di `Student entity` contiene tutte le entità `Enrollment` correlate a `Student`. Ad esempio, se una riga Student (Studente) nella database presenta due righe Enrollment (Iscrizione) correlate, la proprietà di navigazione `Enrollments` contiene questi due entità `Enrollment`. Una riga `Enrollment` correlata è una riga che contiene il valore della chiave primaria dello studente nella colonna `StudentID`. Si supponga ad esempio che per lo studente con ID = 1 siano presenti due righe nella tabella `Enrollment`. La tabella `Enrollment` contiene due righe con `StudentID` = 1. `StudentID` è una chiave esterna nella tabella `Enrollment` che specifica lo studente nella tabella `Student`.

Se una proprietà di navigazione può contenere più entità, deve essere un tipo elenco, ad esempio `ICollection<T>`. È possibile specificare `ICollection<T>` o un tipo, ad esempio `List<T>` o `HashSet<T>`. Se si specifica `ICollection<T>`, per impostazione predefinita Entity Framework Core crea una raccolta `HashSet<T>`. Le proprietà di navigazione che contengono più entità provengono da relazioni molti-a-molti e uno-a-molti.

### <a name="the-enrollment-entity"></a>Entità Enrollment (Iscrizione)

![Diagramma entità Enrollment (Iscrizione)](intro/_static/enrollment-entity.png)

Nella cartella *Models* (Modelli) creare un file *Enrollment.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

La proprietà `EnrollmentID` è la chiave primaria. Questa entità usa il criterio `classnameID` anziché `ID` come l'entità `Student`. In genere gli sviluppatori scelgono un criterio che usano poi in tutto il modello di dati. In un'esercitazione successiva viene illustrato l'uso di ID senza classname. In questo modo si semplifica l'implementazione dell'ereditarietà nel modello di dati.

La proprietà `Grade` è un oggetto`enum`. Il punto interrogativo dopo la dichiarazione del tipo `Grade` indica che la proprietà `Grade` ammette i valori Null. Un voto il cui valore è Null è diverso da un voto con valore zero. Null significa che un voto è sconosciuto o non è stato ancora assegnato.

La proprietà `StudentID` è una chiave esterna e la proprietà di navigazione corrispondente è `Student`. Un'entità `Enrollment` è associata a un'entità `Student`, pertanto la proprietà contiene un'unica entità `Student`. L'entità `Student` si differenzia dalla proprietà di navigazione `Student.Enrollments` che contiene più entità `Enrollment`.

La proprietà `CourseID` è una chiave esterna e la proprietà di navigazione corrispondente è `Course`. Un'entità `Enrollment` è associata a un'entità `Course`.

Entity Framework Core interpreta una proprietà come chiave esterna se è denominata `<navigation property name><primary key property name>`. Ad esempio, `StudentID` per la proprietà di navigazione `Student`, poiché la chiave primaria `Student` dell'entità è `ID`. Le proprietà di chiave esterna possono anche essere denominate `<primary key property name>`. Ad esempio, `CourseID` poiché la chiave primaria `Course` dell'entità è `CourseID`.

### <a name="the-course-entity"></a>Entità Course (Corso)

![Diagramma entità Course (Corso)](intro/_static/course-entity.png)

Nella cartella *Models* (Modelli) creare un file *Course.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

La proprietà `Enrollments` rappresenta una proprietà di navigazione. È possibile correlare un'entità `Course` a un numero qualsiasi di entità `Enrollment`.

L'attributo `DatabaseGenerated` consente all'app di specificare la chiave primaria anziché chiedere al database di generarla.

## <a name="create-the-schoolcontext-db-context"></a>Creare il contesto di database SchoolContext

La classe principale che coordina le funzionalità di Entity Framework Core per un determinato modello di dati è la classe del contesto di database. Il contesto dei dati viene derivato da `Microsoft.EntityFrameworkCore.DbContext`. Il contesto dei dati specifica le entità incluse nel modello di dati. In questo progetto la classe è denominata `SchoolContext`.

Creare una cartella denominata *Data* (Dati) nella cartella del progetto.

Nella cartella *Data* (Dati) creare *SchoolContext.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Questo codice crea una proprietà `DbSet` per ogni set di entità. Nella terminologia di Entity Framework Core:

* Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.
* Un'entità corrisponde a una riga nella tabella.

Le istruzioni `DbSet<Enrollment>` e `DbSet<Course>` possono essere omesse. Core Entity Framework le include in modo implicito perché l'entità `Student` fa riferimento all'entità `Enrollment` e l'entità `Enrollment` fa riferimento all'entità `Course`. Per questa esercitazione, mantenere `DbSet<Enrollment>` e `DbSet<Course>` in `SchoolContext`.

Quando viene creato il database, Entity Framework Core crea tabelle i cui nomi sono gli stessi della proprietà `DbSet`. I nomi di proprietà per le raccolte sono generalmente usati al plurale (Students anziché Student). Gli sviluppatori non hanno un'opinione unanime sul fatto che i nomi di tabella debbano essere usati al plurale. Per queste esercitazioni, viene eseguito l'override del comportamento predefinito specificando nomi di tabella al singolare in DbContext. Per specificare nomi di tabella al singolare, aggiungere il codice evidenziato seguente:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Registrare il contesto con l'inserimento delle dipendenze

ASP.NET Core include l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection). I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione. Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore. Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.

Per registrare `SchoolContext` come servizio, aprire *Startup.cs* e aggiungere le righe evidenziate al metodo `ConfigureServices`.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto `DbContextOptionsBuilder`. Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.

Aggiungere le istruzioni `using` per gli spazi dei nomi `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore`. Compilare il progetto.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Aprire il file *appsettings.json* e aggiungere una stringa di connessione come illustrato nel codice seguente:

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

La stringa di connessione precedente usa `ConnectRetryCount=0` per impedire l'interruzione di [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework).

### <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

La stringa di connessione specifica un database Local DB di SQL Server. Local DB è una versione leggera del motore di database di SQL Server Express appositamente pensato per lo sviluppo di app e non per la produzione. Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa. Per impostazione predefinita, Local DB crea file di database con estensione *mdf* nella directory `C:/Users/<user>`.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Aggiungere il codice per inizializzare il database con dati di test

Entity Framework Core crea un database vuoto. In questa sezione viene scritto un metodo di *inizializzazione* per popolare il database con i dati di test.

Nella cartella *Data* (Dati) creare un nuovo file di classe denominato *DbInitializer.cs* e aggiungere il codice seguente:

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Il codice controlla se esistono studenti nel database. Se il database non contiene studenti, il database viene inizializzato con i dati di test. I dati di test vengono caricati in matrici anziché in raccolte `List<T>` per ottimizzare le prestazioni.

Il metodo `EnsureCreated` crea automaticamente il database per il contesto di database. Se il database esiste, `EnsureCreated` restituisce senza modificare il database.

In *Program.cs* modificare il metodo `Main` per eseguire le operazioni seguenti:

* Ottenere un'istanza del contesto di database dal contenitore di inserimento delle dipendenze.
* Chiamare il metodo di inizializzazione, passandolo al contesto.
* Eliminare il contesto dopo che il metodo di inizializzazione è stato completato.

Il codice seguente illustra il file *Program.cs* aggiornato.

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

La prima volta che si esegue l'app, il database viene creato e inizializzato con i dati di test. Quando il modello di dati è aggiornato, eseguire le operazioni seguenti:
* Eliminare il database.
* Aggiornare il metodo di inizializzazione.
* Eseguire l'app. Viene creato un nuovo database inizializzato. 

Nelle esercitazioni successive il database viene aggiornato alla modifica del modello di dati, senza eliminare e ricreare il database.

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>Aggiungere gli strumenti di scaffolding

In questa sezione viene usata la console di Gestione pacchetti per aggiungere il pacchetto di generazione del codice Web di Visual Studio. Questo pacchetto è necessario per eseguire il motore di scaffolding.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.

Nella console di Gestione pacchetti immettere i comandi seguenti:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

Con il comando precedente i pacchetti NuGet vengono aggiunti al file * con estensione csproj:

[!code-xml[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>Eseguire lo scaffolding del modello

* Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).
* Eseguire i comandi seguenti:


 ```console
dotnet restore
dotnet tool install --global dotnet-aspnet-codegenerator --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

Se viene visualizzato l'errore:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).


Compilare il progetto. La compilazione genera errori simili al seguente:

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 Convertire a livello globale `_context.Student` in `_context.Students` (ovvero aggiungere una "s" a `Student`). Vengono trovate e aggiornate sette occorrenze. Si spera di risolvere [questo bug](https://github.com/aspnet/Scaffolding/issues/633) nella prossima versione.

[!INCLUDE [model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>Eseguire il test dell'app

Eseguire l'app e selezionare il collegamento **Students** (Studenti). A seconda delle dimensione del browser, il collegamento **Students** (Studenti) viene visualizzato in alto alla pagina. Se il collegamento **Students** (Studenti) non è visibile, fare clic sull'icona di spostamento in alto a destra.

![Home page di Contoso University ristretta](intro/_static/home-page-narrow.png)

Eseguire il test dei collegamenti **Create** (Crea), **Edit** (Modifica) e **Details** (Dettagli).

## <a name="view-the-db"></a>Visualizzare il database

Dopo aver avviato l'app, `DbInitializer.Initialize` chiama `EnsureCreated`. `EnsureCreated` controlla l'esistenza del database e se necessario ne crea uno. Se il database non contiene studenti, il metodo `Initialize` aggiunge gli studenti.

Aprire **Esplora oggetti di SQL Server** dal menu **Visualizza** in Visual Studio.
In Esplora oggetti di SQL Server fare clic su **(localdb)\MSSQLLocalDB > Database > ContosoUniversity1**.

Espandere il nodo **Tabelle**.

Fare clic con il pulsante destro del mouse sulla tabella **Student** (Studente) e fare clic su **Visualizza dati** per visualizzare le colonne create e le righe inserite nella tabella.

I file di database con estensione <em>mdf</em> e <em>ldf</em> sono contenuti nella cartella <em>C:\Utenti\\<yourusername></em>.

`EnsureCreated` viene chiamato all'avvio dell'app e consente il flusso di lavoro seguente:

* Eliminare il database.
* Modificare lo schema di database, ad esempio, aggiungere un campo `EmailAddress`.
* Eseguire l'app.

`EnsureCreated` crea un database con la colonna `EmailAddress`.

## <a name="conventions"></a>Convenzioni

Grazie all'uso di convenzioni o di ipotesi di Entity Framework Core, la quantità di codice scritto che è necessario a Entity Framework Core per creare un database completo è minino.

* I nomi delle proprietà `DbSet` vengono usate come nomi di tabella. Per le entità a cui non fa riferimento una proprietà `DbSet`, i nomi della classe di entità vengono usati come nome di tabella.

* I nomi della proprietà di entità vengono usati come nomi di colonna.

* Le proprietà dell'entità vengono denominate ID o classnameID e vengono riconosciute come proprietà della chiave primaria.

* Una proprietà viene interpretata come proprietà di una chiave esterna se è denominata *<navigation property name><primary key property name>*, ad esempio `StudentID` per la proprietà di navigazione `Student` poiché la chiave primaria dell'entità `Student` è `ID`. Le proprietà di una chiave esterna possono essere denominate *<primary key property name>*, ad esempio `EnrollmentID` poiché la chiave primaria dell'entità `Enrollment` è `EnrollmentID`.

È possibile eseguire l'override del comportamento convenzionale. Ad esempio, i nomi di tabella possono essere specificati in modo esplicito, come illustrato in precedenza in questa esercitazione. I nomi delle colonne possono essere impostati in modo esplicito. Le chiavi primarie e le chiavi esterne possono essere impostate in modo esplicito.

## <a name="asynchronous-code"></a>Codice asincrono

La programmazione asincrona è la modalità predefinita per ASP.NET Core ed Entity Framework Core.

Per un server Web è disponibile un numero limitato di thread e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso. In queste circostanze il server non può elaborare nuove richieste finché i thread non saranno liberi. Con il codice sincrono, può succedere che molti thread siano vincolati nonostante in quel momento non stiano eseguendo alcuna operazione. Rimangono tuttavia in attesa che l'operazione I/O sia completata. Con il codice asincrono, se un processo è in attesa del completamento dell'operazione I/O, il thread viene liberato e il server lo può usare per l'elaborazione di altre richieste. Di conseguenza, il codice asincrono consente un uso più efficiente delle risorse e il server è abilitato per gestire una maggiore quantità di traffico senza ritardi.

Il codice asincrono comporta un minimo sovraccarico in fase di esecuzione. In caso di traffico ridotto, il calo delle prestazioni è irrilevante, mentre nelle situazioni di traffico elevato, è essenziale ottimizzare le prestazioni potenziali.

Nel codice seguente la parola chiave `async`, il valore restituito `Task<T>`, la parola chiave `await` e il metodo `ToListAsync` consentono di eseguire il codice in modo asincrono.

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* La parola chiave `async` indica al compilatore di eseguire le operazioni seguenti:

  * Generare callback per parti del corpo del metodo.
  * Creare automaticamente l' oggetto [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) restituito. Per altre informazioni, vedere [Tipo restituito Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Il tipo restituito implicito `Task` rappresenta il lavoro in corso.

* La parola chiave `await` indica al compilatore di suddividere il metodo in due parti. La prima parte termina con l'operazione avviata in modo asincrono. La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.

* `ToListAsync` è la versione asincrona del metodo di estensione `ToList`.

Alcuni aspetti da considerare quando si scrive codice asincrono usato da Entity Framework Core:

* Solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono. Sono incluse `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` e `SaveChangesAsync`. Non sono incluse le istruzioni che modificano solo un oggetto `IQueryable`, ad esempio `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Un contesto di Entity Framework Core non è thread-safe. Non tentare di eseguire più operazioni parallelamente. 

* Per sfruttare i vantaggi del codice asincrono in termini di prestazioni, verificare che i pacchetti della libreria impiegati, ad esempio per il paging, usino la modalità asincrona per chiamare i metodi di Entity Framework Core che generano query da inviare al database.

Per altre informazioni sulla programmazione asincrona in .NET, vedere [Panoramica della programmazione asincrona](/dotnet/articles/standard/async).

Nella prossima esercitazione, vengono esaminate le operazioni CRUD per creare, leggere, aggiornare, eliminare ed elencare.

> [!div class="step-by-step"]
> [avanti](xref:data/ef-rp/crud)
