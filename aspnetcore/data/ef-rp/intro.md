---
title: Pagine Razor con Entity Framework Core - esercitazione 1 di 8
author: rick-anderson
description: Viene illustrato come creare un'app di pagine Razor usando Entity Framework Core
keywords: Esercitazione di ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: 86f9eceb5b8646e371811fa4611a4509ff652231
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a>Introduzione a Entity Framework Core con Visual Studio (1 di 8) e di pagine Razor

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'app web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC 2.0 di base utilizzando componenti di base di Entity Framework (EF) 2.0 e Visual Studio 2017.

L'app di esempio è un sito web per un'università Contoso fittizio. Include funzionalità, ad esempio l'ammissione di studenti, la creazione di corsi e assegnazioni istruttore. Questa pagina è il primo in una serie di esercitazioni che illustrano come compilare l'applicazione di esempio Contoso University.

[Scaricare o visualizzare app completata.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Istruzioni di download](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

Conoscenza [pagine Razor](xref:mvc/razor-pages/index). Dovrà essere completato per i programmatori nuovo [introduzione pagine Razor](xref:tutorials/razor-pages/razor-pages-start) prima di avviare questa serie.

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se si esegue un problema non è possibile risolvere, è possibile trovare la soluzione in genere confrontando il codice per il [completato fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) o [progetto completato](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final). Per un elenco di errori comuni e come risolverli, vedere [la sezione Risoluzione dell'ultima esercitazione nella serie](xref:data/ef-mvc/advanced#common-errors). Se non si trova ciò che è necessario non esiste, è possibile pubblicare una domanda StackOverflow.com per [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Questa serie di esercitazioni si basa su ciò che viene eseguita nelle esercitazioni precedenti. Si consiglia di salvare una copia del progetto dopo ogni completamento dell'esercitazione. Se si verificano problemi, è possibile ricominciare dall'esercitazione precedente anziché tornando all'inizio. In alternativa, è possibile scaricare un [completato fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) e ricominciare usando la fase completata.

## <a name="the-contoso-university-web-app"></a>L'app web di Contoso University

L'applicazione compilata in queste esercitazioni è un sito web di base university.

Gli utenti possono visualizzare e aggiornare studente, corsi e istruttore informazioni. Ecco alcune delle schermate create nell'esercitazione.

![Pagina di indice di studenti](intro/_static/students-index.png)

![Pagina Modifica studenti](intro/_static/student-edit.png)

Lo stile dell'interfaccia utente del sito è vicino a ciò che viene generato tramite i modelli predefiniti. L'esercitazione è attiva EF Core con pagine Razor, non l'interfaccia utente.

## <a name="create-a-razor-pages-web-app"></a>Creare un'app web pagine Razor

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Creare una nuova applicazione Web ASP.NET Core. Denominare il progetto **ContosoUniversity**. È importante denominare il progetto *ContosoUniversity* pertanto gli spazi dei nomi corrisponde a quando il codice è copiati o incollati.
 ![nuova applicazione Web ASP.NET Core](intro/_static/np.png)
* Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.
 ![Applicazione Web (pagine Razor)](../../mvc/razor-pages/index/_static/np2.png)

Premere **F5** per eseguire l'app in modalità di debug o **Ctrl-F5** per l'esecuzione senza collegamento del debugger

## <a name="set-up-the-site-style"></a>Impostare lo stile del sito

Alcune modifiche consente di impostare il menu del sito, layout e home page di.

Aprire *Pages/_Layout.cshtml* e apportare le modifiche seguenti:

* Modificare tutte le occorrenze di "ContosoUniversity" in "Contoso University." Esistono tre occorrenze.

* Aggiungere le voci di menu per **studenti**, **corsi**, **i docenti**, e **reparti**ed eliminare il **contatto** voce di menu.

Le modifiche sono evidenziate. (Tutti il markup *non* visualizzato.)

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

In *Pages/Index.cshtml*, sostituire il contenuto del file con il codice seguente per sostituire il testo su ASP.NET e MVC con testo sull'app:

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Premere CTRL+F5 per eseguire il progetto. Viene visualizzata la home page con le schede create nelle esercitazioni seguenti:

![Home page di Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>Creare il modello di dati

Creare classi di entità per l'applicazione Contoso University. Iniziare con le tre entità seguenti:

![Diagramma del modello dati corso-registrazione degli studenti](intro/_static/data-model-diagram.png)

È una relazione uno-a-molti tra `Student` e `Enrollment` entità. È una relazione uno-a-molti tra `Course` e `Enrollment` entità. Uno studente può registrarsi in un numero qualsiasi di corsi. Un corso può avere qualsiasi numero di studenti iscritti in essa contenuti.

Nelle sezioni seguenti, viene creata una classe per ognuna di queste entità.

### <a name="the-student-entity"></a>L'entità di Student

![Diagramma di entità per studenti](intro/_static/student-entity.png)

Creare un *modelli* cartella. Nel *modelli* cartella, creare un file di classe denominato *Student.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

Il `ID` proprietà diventa la colonna chiave primaria della tabella di database (DB) che corrisponde a questa classe. Per impostazione predefinita, i Core EF interpreta una proprietà denominata `ID` o `classnameID` come chiave primaria.

Il `Enrollments` è una proprietà di navigazione. Le proprietà di navigazione di collegare altre entità correlate a questa entità. In questo caso, il `Enrollments` proprietà di un `Student entity` contiene tutti il `Enrollment` entità correlate che `Student`. Ad esempio, se una riga studente nella DB presenta due correlate righe di registrazione, il `Enrollments` proprietà di navigazione contiene questi due `Enrollment` entità. Un processo di `Enrollment` riga è una riga che contiene valore della chiave primaria tale student di `StudentID` colonna. Si supponga ad esempio gli studenti con ID = 1 dispone di due righe `Enrollment` tabella. Il `Enrollment` tabella contiene due righe con `StudentID` = 1. `StudentID`è una chiave esterna nel `Enrollment` tabella che specifica lo studente il `Student` tabella.

Se una proprietà di navigazione può contenere più entità, è possibile che la proprietà di navigazione deve essere un tipo di elenco, ad esempio `ICollection<T>`. `ICollection<T>`è possibile specificare, o un tipo, ad esempio `List<T>` o `HashSet<T>`. Quando `ICollection<T>` è, Core EF crea un `HashSet<T>` insieme per impostazione predefinita. Proprietà di navigazione che contengono più entità provengono dalle relazioni molti-a-molti e uno-a-molti.

### <a name="the-enrollment-entity"></a>L'entità di registrazione

![Diagramma di entità di registrazione](intro/_static/enrollment-entity.png)

Nel *modelli* cartella, creare *Enrollment.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

Il `EnrollmentID` proprietà è la chiave primaria. Questa entità viene utilizzata la `classnameID` schema anziché `ID` come il `Student` entità. In genere gli sviluppatori di scegliere un modello e di utilizzano in tutto il modello di dati. In un'esercitazione successiva, usando un ID senza classname è visualizzato per renderne più semplice implementare l'ereditarietà nel modello di dati.

Il `Grade` proprietà è un `enum`. Il punto interrogativo dopo il `Grade` dichiarazione del tipo di indica che il `Grade` proprietà è di tipo nullable. Un livello null è diverso da un livello zero - null indica un livello non è noto o non è stato ancora assegnato.

Il `StudentID` proprietà è una chiave esterna e la proprietà di navigazione corrispondente è `Student`. Un `Enrollment` è associata a una entità `Student` entità, pertanto la proprietà contiene un singolo `Student` entità. Il `Student` rispetto all'entità di `Student.Enrollments` proprietà di navigazione, che contiene più `Enrollment` entità.

Il `CourseID` proprietà è una chiave esterna e la proprietà di navigazione corrispondente è `Course`. Un `Enrollment` è associata a una entità `Course` entità.

Core EF interpreta una proprietà come chiave esterna se il file è denominato `<navigation property name><primary key property name>`. Ad esempio,`StudentID` per il `Student` proprietà di navigazione, poiché il `Student` chiave primaria dell'entità è `ID`. Proprietà di chiave esterna può anche essere denominata `<primary key property name>`. Ad esempio, `CourseID` poiché il `Course` chiave primaria dell'entità è `CourseID`.

### <a name="the-course-entity"></a>L'entità Course

![Diagramma di entità Course](intro/_static/course-entity.png)

Nel *modelli* cartella, creare *Course.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

Il `Enrollments` è una proprietà di navigazione. Oggetto `Course` entità può essere correlato a un numero qualsiasi di `Enrollment` entità.

Il `DatabaseGenerated` attributo consente all'app di specificare la chiave primaria, anziché con il database di generarlo.

## <a name="create-the-schoolcontext-db-context"></a>Creare il contesto DB SchoolContext

La classe principale che coordina le funzionalità principali di Entity Framework per la classe del contesto DB non è un modello di dati specificato. Il contesto dei dati è derivato da `Microsoft.EntityFrameworkCore.DbContext`. Il contesto dei dati specifica le entità incluse nel modello di dati. In questo progetto, la classe è denominata `SchoolContext`.

Nella cartella del progetto, creare una cartella denominata *dati*.

Nel *dati* cartella creare *SchoolContext.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Questo codice crea un `DbSet` proprietà per ogni set di entità. Nella terminologia di base di Entity Framework:

* Un set in genere di entità corrisponde a una tabella di database.
* Un'entità corrisponde a una riga nella tabella.

`DbSet<Enrollment>`e `DbSet<Course>` può essere omesso. Core EF li include in modo implicito perché il `Student` i riferimenti alle entità di `Enrollment` entità e `Enrollment` i riferimenti alle entità la `Course` entità. Per questa esercitazione, mantenere `DbSet<Enrollment>` e `DbSet<Course>` nel `SchoolContext`.

Quando viene creato il database, Core EF crea le tabelle che presentano lo stesso come nomi di `DbSet` i nomi delle proprietà. I nomi delle proprietà per le raccolte vengono in genere plurale (studenti anziché studente). Gli sviluppatori non d'accordo sul se i nomi di tabella devono essere plurali. Per queste esercitazioni, viene eseguito l'override del comportamento predefinito specificando i nomi di tabella singolare in DbContext. Per specificare i nomi di tabella singolare, aggiungere il codice evidenziato di seguito:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Registrare il contesto con l'inserimento di dipendenze

ASP.NET Core include [inserimento di dipendenze](xref:fundamentals/dependency-injection). Servizi (ad esempio, il contesto di database di base di EF) vengono registrati con l'inserimento di dipendenze durante l'avvio dell'applicazione. I componenti che richiedono tali servizi (ad esempio pagine Razor) vengono forniti questi servizi tramite i parametri del costruttore. Il codice del costruttore che ottiene un'istanza di contesto db è illustrato più avanti nell'esercitazione.

Per registrare `SchoolContext` come servizio, aprire *Startup.cs*e aggiungere le righe evidenziate per il `ConfigureServices` metodo.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Il nome della stringa di connessione viene passato al contesto chiamando un metodo su un `DbContextOptionsBuilder` oggetto. Per lo sviluppo locale, il [il sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione di *appSettings. JSON* file.

Aggiungere `using` istruzioni per `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore` gli spazi dei nomi. Compilare il progetto.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Aprire il *appSettings. JSON* file e aggiungere una stringa di connessione come illustrato nel codice seguente:

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

La stringa di connessione precedente utilizza `ConnectRetryCount=0` per impedire [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) di bloccarsi.

### <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

La stringa di connessione specifica un database LocalDB di SQL Server. LocalDB è una versione leggera di SQL Server Express Database Engine ed è destinato per lo sviluppo di app non uso in produzione. Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa. Per impostazione predefinita, crea LocalDB *con estensione mdf* file DB il `C:/Users/<user>` directory.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Aggiungere codice per inizializzare il database con dati di test

Core EF crea un database vuoto. In questa sezione, un *valore di inizializzazione* viene scritto un metodo viene popolato con dati di test.

Nel *dati* cartella, creare un nuovo file di classe denominato *DbInitializer.cs* e aggiungere il codice seguente:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Il codice controlla se sono presenti studenti nel database. Se sono presenti Nessuno studente nel database, il database viene inizializzato con dati di test. Carica i dati di test in matrici anziché `List<T>` raccolte per ottimizzare le prestazioni.

Il `EnsureCreated` metodo crea automaticamente il database per il contesto di database. Se il database esiste, `EnsureCreated` restituisce senza modificare il database.

In *Program.cs*, modificare il `Main` metodo per eseguire le operazioni seguenti:

* Ottenere un'istanza di contesto DB dal contenitore dell'inserimento di dipendenza.
* Chiamare il metodo di inizializzazione, passare il contesto.
* Eliminare il contesto al completamento del metodo di inizializzazione.

Nel codice seguente viene aggiornato *Program.cs* file.

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

La prima volta che si esegue l'app, il database viene creato e sottoposto a seeding con dati di test. Quando viene aggiornato il modello di dati:
* Eliminare il database.
* Aggiornare il metodo di inizializzazione.
* Eseguire l'app e viene creato un nuovo database di seeding. 

Nelle esercitazioni successive, il database viene aggiornato quando i modello di dati, senza eliminare e ricreare il database.

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>Aggiungere lo scaffolding strumenti

In questa sezione, il Package Manager Console (PMC) viene utilizzato per aggiungere il pacchetto di generazione codice web Visual Studio. Questo pacchetto è necessario per eseguire il motore di scaffolding.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.

In Package Manager Console (PMC), immettere i comandi seguenti:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

Il comando precedente consente di aggiungere i pacchetti NuGet per il file *. csproj:

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>Lo scaffolding di modello

* Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).
* Eseguire i comandi seguenti:


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
Se viene generato l'errore seguente:

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

Eseguire nuovamente il comando e lascia un commento nella parte inferiore della pagina.

Se viene visualizzato l'errore:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).


Compilare il progetto. La compilazione genera errori simile al seguente:

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 Una modifica globale `_context.Student` a `_context.Students` (ovvero, aggiungere una "s" `Student`). 7 occorrenze sono disponibili e aggiornate. Ci auguriamo che correggere [questo bug](https://github.com/aspnet/Scaffolding/issues/633) nella prossima versione.

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>Eseguire il test dell'app

Eseguire l'app e selezionare il **studenti** collegamento. A seconda della larghezza del browser, il **studenti** collegamento viene visualizzato nella parte superiore della pagina. Se il **studenti** collegamento non è visibile, fare clic sull'icona di spostamento in alto a destra.

![Pagina iniziale di Contoso University "narrow"](intro/_static/home-page-narrow.png)

Test di **crea**, **modifica**, e **dettagli** collegamenti.

## <a name="view-the-db"></a>Visualizzare il database

Quando viene avviata l'app, `DbInitializer.Initialize` chiamate `EnsureCreated`. `EnsureCreated`rileva se il database esiste e viene creata una se necessario. Se sono presenti Nessuno studente nel database, il `Initialize` metodo aggiunge gli studenti.

Aprire **Esplora oggetti di SQL Server** (sillaba SSOX) dal **vista** menu in Visual Studio.
Sillaba SSOX, fare clic su **(localdb) \MSSQLLocalDB > database > ContosoUniversity1**.

Espandere il **tabelle** nodo.

Fare doppio clic su di **Student** tabella e fare clic su **Visualizza dati** per visualizzare le colonne create e le righe inserite nella tabella.

Il *con estensione mdf* e *ldf* presenti file di database di *C:\Users\\ <yourusername>*  cartella.

`EnsureCreated`viene chiamato all'avvio dell'app, che consente il flusso di lavoro seguente:

* Eliminare il database.
* Modificare lo schema di database (ad esempio, aggiungere un `EmailAddress` campo).
* Eseguire l'app.

`EnsureCreated`Crea un database con il`EmailAddress` colonna.

## <a name="conventions"></a>Convenzioni

La quantità di codice scritto in ordine per Core di Entity Framework creare un database completo è minima a causa dell'utilizzo di convenzioni o presupposti che rende Entity Framework Core.

* I nomi delle `DbSet` proprietà vengono utilizzate come nomi di tabella. Per le entità non fa riferimento un `DbSet` proprietà, come nomi di tabella vengono utilizzati i nomi di classe di entità.

* I nomi delle proprietà di entità vengono utilizzati per i nomi di colonna.

* Le proprietà delle entità denominate ID o classnameID sono riconosciute come proprietà della chiave primaria.

* Una proprietà viene interpretata come una proprietà di chiave esterna se il file è denominato  *<navigation property name> <primary key property name>*  (ad esempio, `StudentID` per il `Student` proprietà di navigazione perché il `Student` chiave primaria dell'entità è `ID`). Proprietà di chiave esterna può essere denominata  *<primary key property name>*  (ad esempio, `EnrollmentID` poiché il `Enrollment` chiave primaria dell'entità è `EnrollmentID`).

È possibile sostituire un comportamento convenzionale. Ad esempio, i nomi di tabella possono essere specificati in modo esplicito, come illustrato in precedenza in questa esercitazione. I nomi di colonna possono essere impostati in modo esplicito. Le chiavi primarie e chiavi esterne possono essere impostate in modo esplicito.

## <a name="asynchronous-code"></a>Codice asincrono

Programmazione asincrona è la modalità predefinita per ASP.NET di base e componenti di base di Entity Framework.

Un server web ha un numero limitato di thread disponibili, e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso. In questo caso, il server non è possibile elaborare nuove richieste fino a quando non verranno liberati i thread. Con il codice sincrono, molti thread può essere vincolato mentre sono in realtà non sono svolgere alcuna operazione perché in attesa di completamento dei / o. Con il codice asincrono, quando un processo è in attesa dei / o di completamento, il thread viene liberato per il server da utilizzare per l'elaborazione di altre richieste. Di conseguenza, codice asincrono consente alle risorse di server da utilizzare in modo più efficiente e il server è abilitato per gestire più traffico senza ritardi.

Codice asincrono che presenta una piccola quantità di overhead in fase di esecuzione. Per le situazioni di traffico ridotto, il calo di prestazioni è irrilevante, mentre per le situazioni di traffico elevato, il potenziale miglioramento delle prestazioni è notevole.

Nel codice seguente, il `async` (parola chiave), `Task<T>` valore restituito, `await` (parola chiave), e `ToListAsync` metodo affinché il codice venga eseguito in modo asincrono.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* Il `async` parola chiave indica al compilatore di:

  * Generare i callback per parti del corpo del metodo.
  * Creare automaticamente il [attività](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) oggetto restituito. Per ulteriori informazioni, vedere [attività Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* L'operatore implicito il tipo restituito `Task` rappresenta il lavoro in corso.

* Il `await` (parola chiave), il compilatore suddividere il metodo in due parti. La prima parte termina con l'operazione che viene avviato in modo asincrono. La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.

* `ToListAsync`è la versione asincrona del `ToList` metodo di estensione.

Alcuni aspetti da tenere presenti quando si scrive codice asincrono che utilizza Entity Framework Core:

* Solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono. Che include, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, e `SaveChangesAsync`. Non include istruzioni che modificano solo un `IQueryable`, ad esempio `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Un contesto di EF Core non è thread-safe: non tenta di eseguire più operazioni in parallelo. 

* Per sfruttare i vantaggi delle prestazioni del codice asincrono, verificare di pacchetti di raccolta, ad esempio per il paging, utilizzano async se esse chiamano metodi di Entity Framework Core che inviano query al database.

Per ulteriori informazioni sulla programmazione asincrona in .NET, vedere [Async Panoramica](https://docs.microsoft.com/dotnet/articles/standard/async).

Nella prossima esercitazione base CRUD (create, leggere, aggiornare ed eliminare) vengono esaminate le operazioni.

>[!div class="step-by-step"]
[avanti](xref:data/ef-rp/crud)
