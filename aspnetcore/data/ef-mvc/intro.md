---
title: Core di ASP.NET MVC con Entity Framework Core - esercitazione 1 di 10
author: tdykstra
description: 
keywords: Esercitazione di ASP.NET Core, Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: b67c3d4a-f2bf-4132-a48b-4b0d599d7981
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/intro
ms.openlocfilehash: a4e9ab26fa49720aa2334101ee12916fc797d944
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a>Introduzione a ASP.NET MVC di base e di Entity Framework Core con Visual Studio (1 di 10)

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC 2.0 di base utilizzando componenti di base di Entity Framework (EF) 2.0 e Visual Studio 2017.

L'applicazione di esempio è un sito web per un'università Contoso fittizio. Include funzionalità, ad esempio l'ammissione di studenti, la creazione di corsi e assegnazioni istruttore. Questo è il primo in una serie di esercitazioni che illustrano come compilare l'applicazione di esempio Contoso University da zero.

[Scaricare o visualizzare l'applicazione completata.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

Componenti di base di Entity Framework 2.0 è la versione più recente di Entity Framework, ma non dispone ancora di tutte le funzionalità di Entity Framework 6. x. Per informazioni su come scegliere tra Entity Framework 6. x ed EF Core, vedere [vs EF Core. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/). Se si sceglie di Entity Framework 6. x, vedere [la versione precedente di questa serie di esercitazioni](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

> [!NOTE]
> * Per la versione di ASP.NET 1.1 componenti di base di questa esercitazione, vedere il [versione di Visual Studio 2017 aggiornamento 2 di questa esercitazione in formato PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).
> * Per la versione Visual Studio 2015 di questa esercitazione, vedere la [versione Visual Studio 2015 della documentazione di ASP.NET Core in formato PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a>Risoluzione dei problemi

Se si esegue un problema non è possibile risolvere, è possibile trovare la soluzione in genere confrontando il codice per il [progetto completato](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final). Per un elenco di errori comuni e come risolverli, vedere [la sezione Risoluzione dell'ultima esercitazione nella serie](advanced.md#common-errors). Se non si trova ciò che è necessario non esiste, è possibile pubblicare una domanda StackOverflow.com per [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP] 
> Si tratta di una serie di 10 esercitazioni, ognuno dei quali si basa su ciò che viene eseguita nelle esercitazioni precedenti.  Si consiglia di salvare una copia del progetto dopo ogni completamento dell'esercitazione.  Se si verificano problemi, si può iniziare dall'esercitazione precedente anziché tornando all'inizio dell'intera serie.

## <a name="the-contoso-university-web-application"></a>L'applicazione web di Contoso University

L'applicazione da generare in queste esercitazioni è un sito web semplice university.

Gli utenti possono visualizzare e aggiornare studente, corsi e istruttore informazioni. Ecco alcune delle schermate che verranno create.

![Pagina di indice di studenti](intro/_static/students-index.png)

![Pagina Modifica studenti](intro/_static/student-edit.png)

Lo stile dell'interfaccia utente del sito è disponibile vicino a ciò che viene generato tramite i modelli predefiniti, in modo che l'esercitazione è possibile concentrarsi principalmente sull'utilizzo di Entity Framework.

## <a name="create-an-aspnet-core-mvc-web-application"></a>Creare un'applicazione web ASP.NET MVC di base

Aprire Visual Studio e creare un nuovo progetto web ASP.NET Core c# denominato "ContosoUniversity".

* Dal **File** dal menu **nuovo > progetto**.

* Nel riquadro sinistro, selezionare **installato > Visual c# > Web**.

* Selezionare il **applicazione Web di ASP.NET Core** modello di progetto.

* Immettere **ContosoUniversity** come il nome e fare clic su **OK**.

  ![Finestra di dialogo Nuovo progetto](intro/_static/new-project.png)

* Attendere il **nuova applicazione di Web ASP.NET Core (Core .NET)** visualizzata finestra di dialogo

* Selezionare **ASP.NET Core 2.0** e **applicazione Web (Model-View-Controller)** modello.

  **Nota:** questa esercitazione richiede ASP.NET Core 2.0 e Core di Entity Framework 2.0 o versione successiva--verificare che **ASP.NET Core 1.1** non è selezionata.

* Assicurarsi che **autenticazione** è impostato su **Nessuna autenticazione**.

* Fare clic su **OK**

  ![Nuova finestra di dialogo Progetto ASP.NET](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a>Impostare lo stile del sito

Alcune semplici modifiche imposterà il menu del sito, layout e home page di.

Aprire *Views/Shared/_Layout.cshtml* e apportare le modifiche seguenti:

* Modificare tutte le occorrenze di "ContosoUniversity" in "Contoso University". Esistono tre occorrenze.

* Aggiungere le voci di menu per **studenti**, **corsi**, **i docenti**, e **reparti**ed eliminare il **contatto** voce di menu.

Le modifiche sono evidenziate.

[!code-html[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

In *Views/Home/Index.cshtml*, sostituire il contenuto del file con il codice seguente per sostituire il testo su ASP.NET e MVC con testo sull'applicazione:

[!code-html[](intro/samples/cu/Views/Home/Index.cshtml)]

Premere CTRL + F5 per eseguire il progetto oppure scegliere **Debug > Avvia senza eseguire debug** dal menu. Viene visualizzata la pagina home con schede per le pagine create in queste esercitazioni.

![Home page di Contoso University](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a>Pacchetti di Entity Framework Core NuGet

Per aggiungere il supporto di base di EF a un progetto, installare il provider di database che si desidera di destinazione. Questa esercitazione viene utilizzato SQL Server e il pacchetto del provider è [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/). Questo pacchetto è incluso nel [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, pertanto non è necessario installarlo.

Questo pacchetto e le relative dipendenze (`Microsoft.EntityFrameworkCore` e `Microsoft.EntityFrameworkCore.Relational`) forniscono il supporto runtime per Entity Framework. Si aggiungerà un pacchetto di strumenti più avanti nel [migrazioni](migrations.md) esercitazione. 

Per informazioni su altri provider di database che sono disponibili per Entity Framework Core, vedere [Database provider](https://docs.microsoft.com/ef/core/providers/).

## <a name="create-the-data-model"></a>Creare il modello di dati

Successivamente, si creeranno le classi di entità per l'applicazione Contoso University. Inizierà con le seguenti tre entità.

![Diagramma del modello dati corso-registrazione degli studenti](intro/_static/data-model-diagram.png)

È una relazione uno-a-molti tra `Student` e `Enrollment` entità, ed è presente una relazione uno-a-molti tra `Course` e `Enrollment` entità. In altre parole, gli studenti possono essere registrati in un numero qualsiasi di corsi e un corso può avere qualsiasi numero di studenti iscritti in essa contenuti.

Nelle sezioni seguenti si creerà una classe per ognuna di queste entità.

### <a name="the-student-entity"></a>L'entità di Student

![Diagramma di entità per studenti](intro/_static/student-entity.png)

Nel *modelli* cartella, creare un file di classe denominato *Student.cs* e sostituire il codice del modello con il codice seguente.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

Il `ID` proprietà diventerà una colonna chiave primaria della tabella di database che corrisponde a questa classe. Per impostazione predefinita, Entity Framework interpreta una proprietà denominata `ID` o `classnameID` come chiave primaria.

Il `Enrollments` è una proprietà di navigazione. Le proprietà di navigazione contengono altre entità correlate a questa entità. In questo caso, il `Enrollments` proprietà di un `Student entity` conterrà tutte le `Enrollment` entità correlate che `Student` entità. In altre parole, se una determinata riga studente nel database presenta due correlate (righe che contengono chiave primaria tale student nella rispettiva colonna di chiave esterna StudentID), le righe di registrazione che `Student` dell'entità `Enrollments` conterrà le proprietà di navigazione due `Enrollment` entità.

Se una proprietà di navigazione può contenere più entità (ad esempio relazioni molti-a-molti o uno-a-molti), il tipo deve essere un elenco in cui le voci possono essere aggiunte, eliminate e aggiornate, ad esempio `ICollection<T>`.  È possibile specificare `ICollection<T>` o un tipo, ad esempio `List<T>` o `HashSet<T>`. Se si specifica `ICollection<T>`, EF crea un `HashSet<T>` insieme per impostazione predefinita.

### <a name="the-enrollment-entity"></a>L'entità di registrazione

![Diagramma di entità di registrazione](intro/_static/enrollment-entity.png)

Nel *modelli* cartella, creare *Enrollment.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

Il `EnrollmentID` proprietà sarà la chiave primaria, questa entità vengono utilizzati il `classnameID` schema anziché `ID` da se stesso come è illustrato nel `Student` entità. In genere si potrebbe scegliere un modello e utilizzarlo in tutto il modello di dati. In questo caso, la variazione di seguito viene illustrato che è possibile utilizzare un modello. In un [esercitazione successiva](inheritance.md), si noterà come usando un ID senza classname rende più semplice implementare l'ereditarietà nel modello di dati.

Il `Grade` proprietà è un `enum`. Il punto interrogativo dopo il `Grade` dichiarazione del tipo di indica che il `Grade` proprietà è di tipo nullable. Un livello null è diverso da un livello zero - null indica un livello non è noto o non è stato ancora assegnato.

Il `StudentID` proprietà è una chiave esterna e la proprietà di navigazione corrispondente è `Student`. Un `Enrollment` entità è associata a una `Student` entità, pertanto la proprietà può contenere solo un unico `Student` entità (a differenza di `Student.Enrollments` proprietà di navigazione illustrato in precedenza, che può contenere più `Enrollment` entità).

Il `CourseID` proprietà è una chiave esterna e la proprietà di navigazione corrispondente è `Course`. Un `Enrollment` è associata a una entità `Course` entità.

Entity Framework interpreta una proprietà come proprietà di chiave esterna se il file è denominato `<navigation property name><primary key property name>` (ad esempio, `StudentID` per il `Student` proprietà di navigazione perché il `Student` chiave primaria dell'entità è `ID`). Proprietà di chiave esterna può anche essere denominata semplicemente `<primary key property name>` (ad esempio, `CourseID` poiché il `Course` chiave primaria dell'entità è `CourseID`).

### <a name="the-course-entity"></a>L'entità Course

![Diagramma di entità Course](intro/_static/course-entity.png)

Nel *modelli* cartella, creare *Course.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

Il `Enrollments` è una proprietà di navigazione. Oggetto `Course` entità può essere correlato a un numero qualsiasi di `Enrollment` entità.

Ciò che diremo più sul `DatabaseGenerated` attributo un [esercitazione successiva](complex-data-model.md) di questa serie. In pratica, questo attributo consente di immettere la chiave primaria per il corso anziché il database di generarlo.

## <a name="create-the-database-context"></a>Creare il contesto del Database

La classe principale che coordina le funzionalità di Entity Framework per un modello di dati specificato è la classe di contesto di database. Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`. Nel codice specificare le entità incluse nel modello di dati. È anche possibile personalizzare determinati comportamenti di Entity Framework. In questo progetto, la classe è denominata `SchoolContext`.

Nella cartella del progetto, creare una cartella denominata *dati*.

Nel *dati* cartella creare un nuovo file di classe denominato *SchoolContext.cs*e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Questo codice crea un `DbSet` proprietà per ogni set di entità. Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database e un'entità corrisponde a una riga della tabella.

È stato possibile stato omesso il `DbSet<Enrollment>` e `DbSet<Course>` istruzioni e funzionerà la stessa. Entity Framework sarebbe includerli in modo implicito perché il `Student` i riferimenti alle entità di `Enrollment` entità e il `Enrollment` i riferimenti alle entità di `Course` entità.

Quando viene creato il database, EF crea le tabelle che presentano lo stesso come nomi di `DbSet` i nomi delle proprietà. I nomi delle proprietà per le raccolte sono in genere al plurale (studenti anziché studente), ma gli sviluppatori non d'accordo sul se i nomi di tabella devono essere pluralizzati o non. Per queste esercitazioni è sarà eseguire l'override del comportamento predefinito specificando i nomi di tabella singolare in DbContext. A tale scopo, aggiungere il codice evidenziato di seguito dopo l'ultima proprietà DbSet.

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Registrare il contesto con l'inserimento di dipendenze

Implementa ASP.NET Core [inserimento di dipendenze](../../fundamentals/dependency-injection.md) per impostazione predefinita. Servizi (ad esempio, il contesto di database di Entity Framework) vengono registrati con l'inserimento di dipendenze durante l'avvio dell'applicazione. I componenti che richiedono tali servizi (ad esempio controller MVC) vengono forniti questi servizi tramite i parametri del costruttore. Verrà visualizzato il codice del costruttore controller che ottiene un'istanza di contesto più avanti in questa esercitazione.

Per registrare `SchoolContext` come servizio, aprire *Startup.cs*e aggiungere le righe evidenziate per il `ConfigureServices` metodo.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Il nome della stringa di connessione viene passato al contesto chiamando un metodo su un `DbContextOptionsBuilder` oggetto. Per lo sviluppo locale, il [il sistema di configurazione di ASP.NET Core](../../fundamentals/configuration.md) legge la stringa di connessione di *appSettings. JSON* file.

Aggiungere `using` istruzioni per `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore` gli spazi dei nomi e quindi compilare il progetto.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Aprire il *appSettings. JSON* file e aggiungere una stringa di connessione come illustrato nell'esempio seguente.

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

La stringa di connessione specifica un database LocalDB di SQL Server. LocalDB è una versione leggera di SQL Server Express Database Engine e deve essere lo sviluppo di applicazioni, non uso in produzione. Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa. Per impostazione predefinita, crea LocalDB *con estensione mdf* file di database nel `C:/Users/<user>` directory.

## <a name="add-code-to-initialize-the-database-with-test-data"></a>Aggiungere il codice per inizializzare il database con dati di test

Entity Framework creerà un database vuoto per l'utente.  In questa sezione, scrivere un metodo che viene chiamato dopo che il database viene creato per popolarlo con i dati di test.

In questo caso verrà utilizzato il `EnsureCreated` metodo per creare automaticamente il database. In un [esercitazione successiva](migrations.md) si noterà come gestire le modifiche al modello utilizzando migrazioni Code First per modificare lo schema del database anziché eliminare e ricreare il database.

Nel *dati* cartella, creare un nuovo file di classe denominato *DbInitializer.cs* e sostituire il codice del modello con il codice seguente, che fa sì che un database da creare quando necessario e carica i dati nel nuovo di test database.

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Il codice verifica se sono presenti eventuali studenti nel database, e se non si presuppone che il database è nuovo e deve essere effettuato il seeding con dati di test.  Carica i dati di test in matrici anziché `List<T>` raccolte per ottimizzare le prestazioni.

In *Program.cs*, modificare il `Main` metodo per eseguire le operazioni seguenti all'avvio dell'applicazione:

* Ottenere un'istanza del contesto di database dal contenitore dell'inserimento di dipendenza.
* Chiamare il metodo di inizializzazione, passare il contesto.
* Eliminare il contesto quando il metodo di inizializzazione viene eseguito.

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

Aggiungere `using` istruzioni:

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]

Nelle esercitazioni precedenti, si può vedere codice simile nel `Configure` metodo *Startup.cs*. È consigliabile utilizzare il `Configure` metodo solo per impostare la pipeline della richiesta. Codice di avvio dell'applicazione cui appartiene il `Main` metodo.

Ora la prima volta che si esegue l'applicazione, il database verrà creato e sottoposto a seeding con dati di test. Ogni volta che si modifica il modello di dati, è possibile eliminare il database, aggiornare il metodo di inizializzazione e ricominciare con un nuovo database allo stesso modo. Nelle esercitazioni successive, si noterà come modificare il database quando i modello di dati, senza eliminare e crearne uno nuovo.

## <a name="create-a-controller-and-views"></a>Creare un controller e visualizzazioni

Successivamente, si userà il motore di scaffolding in Visual Studio per aggiungere un controller MVC e viste che verranno utilizzato Entity Framework per eseguire una query e salvare i dati.

La creazione automatica delle viste e i metodi di azione CRUD è noto come lo scaffolding. Lo scaffolding differisce dalla generazione di codice in quanto il codice di supporto temporaneo è un punto di partenza che è possibile modificare per adattarli ai propri requisiti, mentre è in genere non modificare il codice generato. Quando è necessario personalizzare il codice generato, utilizzare le classi parziali o rigenerare il codice quando ci sono modifiche.

* Fare doppio clic sul **controller** cartella **Esplora** e selezionare **Aggiungi > Nuovo elemento di scaffolding**.

* Nella finestra di dialogo **Aggiungi dipendenze MVC** selezionare **Dipendenze minime**, quindi **Aggiungi**.

  ![Aggiunta di dipendenze](intro/_static/add-depend.png)

  Visual Studio aggiunge le dipendenze necessarie per eseguire lo scaffolding di un controller. La modifica sola nel file di progetto è l'aggiunta del `Microsoft.VisualStudio.Web.CodeGeneration.Design` pacchetto.

  Oggetto *ScaffoldingReadMe.txt* viene creato un file che è possibile eliminare.

* Ancora una volta, fare doppio clic sul **controller** cartella **Esplora** e selezionare **Aggiungi > Nuovo elemento di scaffolding**.

* Nel **aggiungere lo scaffolding** la finestra di dialogo:

  * Selezionare **controller MVC con viste, mediante Entity Framework**.

  * Fare clic su **Aggiungi**.

* Nel **Aggiungi Controller** la finestra di dialogo:

  * In **classe modello** selezionare **Student**.

  * In **classe contesto dati** selezionare **SchoolContext**.

  * Accettare il valore predefinito **StudentsController** come nome.

  * Fare clic su **Aggiungi**.

  ![Lo scaffolding Student](intro/_static/scaffold-student.png)

  Quando fa clic su **Aggiungi**, il motore di scaffolding di Visual Studio crea un *StudentsController.cs* file e un set di viste (*. cshtml* file) che funziona con il controller.

(Il motore di scaffolding può anche creare il contesto del database per l'utente se non crei manualmente prima, come in precedenza per questa esercitazione. È possibile specificare una nuova classe di contesto nel **Aggiungi Controller** facendo clic sul segno più a destra della casella di **classe contesto dati**.  Visual Studio creerà quindi la `DbContext` classe nonché il controller e visualizzazioni.)

Si noterà che il controller che utilizza un `SchoolContext` come parametro di costruttore.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

Inserimento di dipendenze ASP.NET si occuperà di passare un'istanza di `SchoolContext` nel controller. È stato configurato nel *Startup.cs* file in precedenza.

Il controller contiene un `Index` metodo di azione, che consente di visualizzare tutti gli studenti nel database. Il metodo ottiene un elenco di studenti dall'entità studenti impostato leggendo il `Students` proprietà dell'istanza del contesto di database:

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

Più avanti nell'esercitazione si apprenderà sugli elementi di programmazione asincroni in questo codice.

Il *Views/Students/Index.cshtml* Visualizza l'elenco in una tabella:

[!code-html[](intro/samples/cu/Views/Students/Index1.cshtml)]

Premere CTRL + F5 per eseguire il progetto oppure scegliere **Debug > Avvia senza eseguire debug** dal menu.

Fare clic sulla scheda di studenti per visualizzare i dati di test che il `DbInitializer.Initialize` metodo inserito. A seconda della modalità narrow la finestra del browser, verrà visualizzato il `Student` collegamento scheda nella parte superiore della pagina o è necessario fare clic sull'icona di spostamento nell'angolo superiore destro per visualizzare il collegamento.

![Pagina iniziale di Contoso University "narrow"](intro/_static/home-page-narrow.png)

![Pagina di indice di studenti](intro/_static/students-index.png)

## <a name="view-the-database"></a>Visualizzare il Database

Durante l'avvio dell'applicazione, il `DbInitializer.Initialize` chiamate al metodo `EnsureCreated`. EF visto che si è verificato alcun database e pertanto creato uno, quindi nella parte restante del `Initialize` codice del metodo compilato il database con i dati. È possibile utilizzare **Esplora oggetti di SQL Server** (sillaba SSOX) per visualizzare il database in Visual Studio.

Chiudere il browser.

Se la finestra sillaba SSOX non è già aperta, selezionarlo il **vista** menu in Visual Studio.

Sillaba SSOX, fare clic su **(localdb) \MSSQLLocalDB > database**e quindi fare clic sulla voce per il nome del database nella stringa di connessione nel *appSettings. JSON* file.

Espandere il **tabelle** nodo per visualizzare le tabelle nel database.

![Tabelle in sillaba SSOX](intro/_static/ssox-tables.png)

Fare doppio clic su di **Student** tabella e fare clic su **Visualizza dati** per visualizzare le colonne che sono state create e le righe che sono state inserite nella tabella.

![Tabella Student sillaba SSOX](intro/_static/ssox-student-table.png)

Il *con estensione mdf* e *ldf* presenti file di database di *C:\Users\<nomeutente >* cartella.

Poiché si sta chiamando `EnsureCreated` nel metodo di inizializzatore che viene eseguito all'avvio dell'app, è ora possibile apportare una modifica per la `Student` classe, eliminare il database, eseguire nuovamente l'applicazione e il database sarà automaticamente ricreato per trovare la corrispondenza della modifica. Ad esempio, se si aggiunge un `EmailAddress` proprietà per il `Student` (classe), si noterà una nuova `EmailAddress` colonna nella tabella ricreata.

## <a name="conventions"></a>Convenzioni

La quantità di codice che era necessario scrivere affinché Entity Framework sia in grado di creare un database completo per l'utente è minima a causa dell'utilizzo di convenzioni o presupposti che rende Entity Framework.

* I nomi delle `DbSet` proprietà vengono utilizzate come nomi di tabella. Per le entità non fa riferimento un `DbSet` proprietà, come nomi di tabella vengono utilizzati i nomi di classe di entità.

* I nomi delle proprietà di entità vengono utilizzati per i nomi di colonna.

* Le proprietà delle entità denominate ID o classnameID sono riconosciute come proprietà della chiave primaria.

* Una proprietà viene interpretata come una proprietà di chiave esterna se il file è denominato * <navigation property name> <primary key property name> * (ad esempio, `StudentID` per il `Student` proprietà di navigazione perché il `Student` chiave primaria dell'entità è `ID`). Proprietà di chiave esterna può anche essere denominata semplicemente * <primary key property name> * (ad esempio, `EnrollmentID` poiché il `Enrollment` chiave primaria dell'entità è `EnrollmentID`).

È possibile sostituire un comportamento convenzionale. Ad esempio, è possibile specificare in modo esplicito i nomi di tabella, come illustrato in precedenza in questa esercitazione. Ed è possibile impostare i nomi delle colonne e impostare qualsiasi proprietà di primary key o foreign key come verrà visualizzato un [esercitazione successiva](complex-data-model.md) di questa serie.

## <a name="asynchronous-code"></a>Codice asincrono

Programmazione asincrona è la modalità predefinita per ASP.NET di base e componenti di base di Entity Framework.

Un server web ha un numero limitato di thread disponibili, e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso. In questo caso, il server non è possibile elaborare nuove richieste fino a quando non verranno liberati i thread. Con il codice sincrono, molti thread può essere vincolato mentre sono in realtà non sono svolgere alcuna operazione perché in attesa di completamento dei / o. Con il codice asincrono, quando un processo è in attesa dei / o di completamento, il thread viene liberato per il server da utilizzare per l'elaborazione di altre richieste. Di conseguenza, codice asincrono consente alle risorse di server da utilizzare in modo più efficiente e il server è abilitato per gestire più traffico senza ritardi.

Codice asincrono che presenta una piccola quantità di overhead in fase di esecuzione, ma il potenziale miglioramento delle prestazioni in situazioni con traffico ridotto il calo di prestazioni è irrilevante, mentre per le situazioni di traffico elevato, è sostanziale.

Nel codice seguente, il `async` (parola chiave), `Task<T>` valore restituito, `await` (parola chiave), e `ToListAsync` metodo affinché il codice venga eseguito in modo asincrono.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* Il `async` parola chiave indica al compilatore di generare i callback per parti del corpo del metodo e per la creazione automatica di `Task<IActionResult>` oggetto restituito.

* Il tipo restituito `Task<IActionResult>` rappresenta il lavoro in corso con un risultato di tipo `IActionResult`.

* Il `await` (parola chiave), il compilatore suddividere il metodo in due parti. La prima parte termina con l'operazione che viene avviato in modo asincrono. La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.

* `ToListAsync`è la versione asincrona del `ToList` metodo di estensione.

Alcuni aspetti da tenere in considerazione quando si scrive codice asincrono che usa Entity Framework:

* Solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono. Ad esempio, che include `ToListAsync`, `SingleOrDefaultAsync`, e `SaveChangesAsync`.  Non include, ad esempio, le istruzioni che modificano solo un `IQueryable`, ad esempio `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Un contesto di Entity Framework non è thread-safe: non tenta di eseguire più operazioni in parallelo. Quando si chiama qualsiasi metodo asincrono in Entity Framework, usare sempre il `await` (parola chiave).

* Se si desidera sfruttare i vantaggi delle prestazioni del codice asincrono, assicurarsi che qualsiasi libreria di pacchetti in uso (ad esempio per il paging), usano anche il metodo async se esse chiamano metodi di Entity Framework che vengono generate query da inviare al database.

Per ulteriori informazioni sulla programmazione asincrona in .NET, vedere [Async Panoramica](https://docs.microsoft.com/dotnet/articles/standard/async).

## <a name="summary"></a>Riepilogo

Ora è stata creata una semplice applicazione che usa il Core di Entity Framework e SQL Server Express LocalDB per archiviare e visualizzare i dati. Nell'esercitazione seguente, si apprenderà come eseguire operazioni di base (creare, leggere, aggiornare ed eliminare) operazioni.

>[!div class="step-by-step"]
[Successivo](crud.md)  
