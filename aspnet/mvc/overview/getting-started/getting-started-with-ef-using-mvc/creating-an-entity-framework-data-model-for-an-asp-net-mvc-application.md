---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Introduzione a Entity Framework 6 Code First con MVC 5 | Microsoft Docs
author: tdykstra
ms.author: riande
ms.date: 12/04/2018
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c7ab9458f83e05af84f72d9a2519a8c1c39b84b5
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861433"
---
# <a name="get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Introduzione a Entity Framework 6 Code First con MVC 5

da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> [!NOTE]
> Per i nuovi sviluppi, è consigliabile [pagine Razor di ASP.NET Core](/aspnet/core/razor-pages) su ASP.NET MVC controller e visualizzazioni. Una serie di esercitazioni simile alla seguente è disponibile per le pagine Razor, il [esercitazione sulle pagine Razor](/aspnet/core/tutorials/razor-pages/razor-pages-start):
>
> * È più semplice da seguire.
> * Offre un maggior numero di procedure consigliate per EF Core.
> * Usa query più efficienti.
> * È più aggiornata con le API più recenti.
> * Riguarda più funzionalità.
> * È l'approccio consigliato per lo sviluppo di nuove applicazioni.

> Questo articolo illustra come creare applicazioni ASP.NET MVC 5 con Entity Framework 6 e Visual Studio. Questa esercitazione Usa il flusso di lavoro di Code First. Per informazioni su come scegliere tra Code First, Database First e Model First, vedere [creare un modello](/ef/ef6/modeling/).
>
> L'applicazione di esempio è un sito web per un'università fittizia denominata Contoso University. Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati. Questa serie di esercitazioni illustra come compilare l'applicazione di esempio Contoso University. È possibile [scaricare l'applicazione completata](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
>
> È disponibile una versione di Visual Basic tradotto da Mike Brind: [MVC 5 con Entity Framework 6 in Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) sul sito Mikesdotnetting.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - [Entity Framework 6](https://www.nuget.org/packages/EntityFramework)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (facoltativo)
>
> ## <a name="tutorial-versions"></a>Versioni dell'esercitazione
>
> Per le versioni precedenti di questa esercitazione, vedere [4.1 di EF / MVC 3 eBook](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) e [Introduzione a EF 5 con MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviaci un feedback sul modo in cui si hanno aggiunto mi piace in questa esercitazione e cosa possiamo migliorare usando i commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx) oppure [StackOverflow.com](http://stackoverflow.com/).
>
> Se si verifica un problema che non è possibile risolvere, è generalmente possibile trovare la soluzione al problema confrontando il codice del progetto completato che è possibile scaricare. Per alcuni errori comuni e come risolverli, vedere [errori comuni e soluzioni o soluzioni alternative](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors).

## <a name="the-contoso-university-web-app"></a>App Web di Contoso University

L'applicazione che verrà compilata in queste esercitazioni è un semplice sito web universitario. Gli utenti possono visualizzare e aggiornare le informazioni che riguardano studenti, corsi e insegnanti. Di seguito sono alcune delle schermate di cui che verrà creato:

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Modifica studente](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

In modo che l'esercitazione si concentra principalmente su come usare Entity Framework, non sarà possibile modificare l'interfaccia utente del sito web molto da quanto è stato generato tramite i modelli predefiniti.

## <a name="prerequisites"></a>Prerequisiti

Visualizzare **le versioni del Software** nella parte superiore della pagina. Entity Framework 6 non è un prerequisito, poiché si installa il pacchetto NuGet di Entity Framework come parte dell'esercitazione.

## <a name="create-an-mvc-web-app"></a>Creare un'app web MVC

1. Aprire Visual Studio e creare un nuovo linguaggio c# web progetto usando il **applicazione Web ASP.NET (.NET Framework)** modello. Denominare il progetto "ContosoUniversity".

   ![Finestra di dialogo Nuovo progetto in Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

2. Nella finestra di dialogo Nuovo progetto ASP.NET, selezionare la **MVC** modello.

   ![Nuova finestra di dialogo app web in Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

3. Se **Authentication** non è impostata su **Nessuna autenticazione**, modificarla, fare clic su **Modifica autenticazione**.

   Nel **Modifica autenticazione** finestra di dialogo **Nessuna autenticazione**, quindi scegliere **OK**. Per questa esercitazione, l'app web non richiede agli utenti di accedere, né limitare l'accesso basato su chi ha eseguito l'accesso.

   ![Finestra di dialogo Modifica autenticazione in Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/change-authentication.png)

4. Nella finestra di dialogo Nuovo progetto ASP.NET, fare clic su **OK** per creare il progetto.

## <a name="set-up-the-site-style"></a>Impostare lo stile del sito

Con alcune modifiche è possibile impostare il menu del sito, il layout e la home page.

1. Aprire *Views\Shared\\layout. cshtml*e apportare le modifiche seguenti:

   - Modificare tutte le occorrenze di "My ASP.NET Application" e "Application name" in "Contoso University".
   - Aggiungere le voci di menu per gli studenti, corsi, instructors (insegnanti) e i reparti ed eliminare la voce del contatto.

   Le modifiche sono evidenziate nel frammento di codice seguente:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. Nelle *Views\Home\Index.cshtml*, sostituire il contenuto del file con il codice seguente sostituire il testo su ASP.NET e MVC testo relativo a questa applicazione:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Premere **Ctrl**+**F5** per eseguire il sito web. Viene visualizzata la pagina Home page con il menu principale.

   ![Home page di Contoso University](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Installare Entity Framework 6

1. Dal **degli strumenti** menu, scegliere **Gestione pacchetti NuGet**e quindi scegliere **Package Manager Console**.

2. Nel **Console di gestione pacchetti** finestra, immettere il comando seguente:

   ```text
   Install-Package EntityFramework
   ```

   ![Entity Framework installati](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

   L'immagine Mostra 6.0.0 in corso l'installazione, ma NuGet installerà la versione più recente di Entity Framework (escluse le versioni non definitive), che al momento dell'aggiornamento più recente con l'esercitazione è 6.2.0.

Questo passaggio è uno dei pochi passaggi in questa esercitazione con è eseguire l'operazione manualmente, ma che potrebbe essere stata effettuata automaticamente dalla funzionalità di scaffolding di ASP.NET MVC. Li si sta eseguendo manualmente in modo che è possibile visualizzare i passaggi necessari per usare Entity Framework (EF). Si userà lo scaffolding in un secondo momento per creare le visualizzazioni e controller MVC. In alternativa è possibile lasciare che lo scaffolding automaticamente installare il pacchetto NuGet di Entity Framework, creare la classe del contesto del database e creare la stringa di connessione. Quando è pronti per eseguire questa operazione in questo modo, sufficiente è ignorare tali passaggi ed eseguire lo scaffolding di controller MVC dopo aver creato le classi di entità.

## <a name="create-the-data-model"></a>Creare il modello di dati

A questo punto è possibile creare le classi delle entità per l'applicazione di Contoso University. Si inizia con le tre entità seguenti:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Esiste una relazione uno-a-molti tra le entità `Student` e `Enrollment` ed esiste una relazione uno-a-molti tra le entità `Course` e `Enrollment`. In altre parole, uno studente può iscriversi a un numero qualsiasi di corsi e un corso può avere un numero qualsiasi di studenti iscritti.

Nelle sezioni seguenti, si creerà una classe per ognuna di queste entità.

> [!NOTE]
> Se si prova a compilare il progetto prima di aver creato tutte queste classi di entità, si otterrà gli errori del compilatore.

### <a name="the-student-entity"></a>Entità Student (Studente)

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

- Nel *modelli* cartella, creare un file di classe denominato *Student.cs* facendo clic sulla cartella nella **Esplora soluzioni** e scegliendo **Add**  >  **Classe**. Sostituire il codice del modello con il codice seguente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

La proprietà `ID` diventa la colonna di chiave primaria della tabella di database che corrisponde a questa classe. Per impostazione predefinita, Entity Framework interpreta una proprietà denominata `ID` oppure *NomeClasse* `ID` come chiave primaria.

La proprietà `Enrollments` rappresenta una *proprietà di navigazione*. Le proprietà di navigazione contengono altre entità correlate a questa entità. In questo caso, il `Enrollments` proprietà di un `Student` entità conterrà tutte le `Enrollment` entità correlate a quella `Student` entità. In altre parole, se un determinato `Student` riga nel database ha due correlate `Enrollment` righe (valore righe che contengono una chiave primaria dello studente in loro `StudentID` colonna chiave esterna), che `Student` dell'entità `Enrollments` proprietà di navigazione contiene quelle due `Enrollment` entità.

Le proprietà di navigazione sono in genere definite come `virtual` in modo che è possibile sfruttare alcune funzionalità di Entity Framework, ad esempio *caricamento lazy*. (Caricamento lazy sarà spiegato più avanti, nelle [lettura di dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie.)

Se una proprietà di navigazione può contenere più entità (come nel caso di relazioni molti-a-molti e uno-a-molti), il tipo della proprietà deve essere un elenco in cui le voci possono essere aggiunte, eliminate e aggiornate, come ad esempio `ICollection`.

### <a name="the-enrollment-entity"></a>Entità Enrollment (Iscrizione)

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

- Nella cartella *Models* creare *Enrollment.cs* e sostituire il codice esistente con il codice seguente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Il `EnrollmentID` proprietà potranno essere la chiave primaria, questa entità Usa il *classname* `ID` modello anziché `ID` come visto nel `Student` entità. In genere si sceglie un criterio e lo si usa poi in tutto il modello di dati. In questo caso la variazione illustra che è possibile sia uno sia l'altro criterio. In un'esercitazione successiva, si noterà come l'uso `ID` senza `classname` rende più semplice implementare l'ereditarietà nel modello di dati.

Il `Grade` proprietà è un [enum](/ef/ef6/modeling/code-first/data-types/enums). Il punto interrogativo dopo la `Grade` dichiarazione del tipo indica che il `Grade` è di proprietà [nullable](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Un voto il cui valore è null è diverso da un voto con valore zero, ovvero null indica che un voto è sconosciuto o non è stato ancora assegnato.

La proprietà `StudentID` è una chiave esterna e la proprietà di navigazione corrispondente è `Student`. Un'entità `Enrollment` è associata a un'entità `Student`, pertanto la proprietà può contenere un'unica entità `Student`, a differenza della proprietà di navigazione `Student.Enrollments` vista in precedenza, che può contenere più entità `Enrollment`.

La proprietà `CourseID` è una chiave esterna e la proprietà di navigazione corrispondente è `Course`. Un'entità `Enrollment` è associata a un'entità `Course`.

Entity Framework interpreta una proprietà come proprietà di chiave esterna se è denominata *&lt;il nome di proprietà di navigazione&gt;&lt;il nome di proprietà di chiave primaria&gt;* (ad esempio `StudentID`per il `Student` proprietà di navigazione dopo la `Student` chiave primaria dell'entità è `ID`). Proprietà di chiave esterna possono anche essere denominate lo stesso semplicemente *&lt;nome proprietà della chiave primaria&gt;* (ad esempio, `CourseID` poiché il `Course` chiave primaria dell'entità è `CourseID`).

### <a name="the-course-entity"></a>Entità Course (Corso)

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

- Nel *modelli* cartella, creare *Course.cs*, sostituendo il codice del modello con il codice seguente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La proprietà `Enrollments` rappresenta una proprietà di navigazione. È possibile correlare un'entità `Course` a un numero qualsiasi di entità `Enrollment`.

Ciò che diremo informazioni sul <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute> attributo in un'esercitazione successiva di questa serie. In pratica, questo attributo consente di immettere la chiave primaria per il corso invece di essere generata dal database.

## <a name="create-the-database-context"></a>Creare il contesto di database

La classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati è il *contesto di database* classe. Questa classe viene creata mediante derivazione dal [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) classe. Nel codice, specificare le entità incluse nel modello di dati. È anche possibile personalizzare un determinato comportamento di Entity Framework. In questo progetto la classe è denominata `SchoolContext`.

- Per creare una cartella nel progetto ContosoUniversity, fare clic sul progetto in **Esplora soluzioni** e fare clic su **Add**, quindi fare clic su **nuova cartella**. Denominare la nuova cartella *DAL* (per il livello di accesso ai dati). In questa cartella creare un nuovo file di classe denominato *SchoolContext.cs*e sostituire il codice del modello con il codice seguente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Specificare i set di entità

Questo codice crea un [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) proprietà per ogni set di entità. Nella terminologia di Entity Framework, un' *set di entità* in genere corrisponde a una tabella di database e un *entità* corrisponde a una riga nella tabella.

> [!NOTE]
>
> È possibile omettere il `DbSet<Enrollment>` e `DbSet<Course>` istruzioni, potrebbe comunque funzionare lo stesso. Entity Framework le include in modo implicito perché la `Student` i riferimenti alle entità il `Enrollment` entità e il `Enrollment` i riferimenti alle entità di `Course` entità.

### <a name="specify-the-connection-string"></a>Specificare la stringa di connessione

Il nome della stringa di connessione (che si aggiungerà il file Web. config in un secondo momento) viene passato al costruttore.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

È inoltre possibile passare la stringa di connessione stessa anziché il nome di uno che viene archiviato nel file Web. config. Per altre informazioni sulle opzioni che consentono di specificare il database da usare, vedere [stringhe di connessione e modelli](/ef/ef6/fundamentals/configuring/connection-strings).

Se non si specifica una stringa di connessione o il nome di uno in modo esplicito, Entity Framework presuppone che il nome di stringa di connessione sia lo stesso nome della classe. Il nome di stringa di connessione predefinito in questo esempio sarebbe quindi `SchoolContext`, quella cosa si sta specificando in modo esplicito.

### <a name="specify-singular-table-names"></a>Specificare i nomi di tabella al singolare

Il `modelBuilder.Conventions.Remove` istruzione il [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metodo impedisce che i nomi delle tabelle in corso pluralizzati. Se non è stato in questo caso, le tabelle generate nel database verrà denominate `Students`, `Courses`, e `Enrollments`. Al contrario, i nomi di tabella saranno `Student`, `Course`, e `Enrollment`. Gli sviluppatori non hanno un'opinione unanime sul fatto che i nomi di tabella debbano essere pluralizzati oppure no. Questa esercitazione Usa la forma singolare, ma il punto importante è che è possibile selezionare qualsiasi formato si preferisce includendo o l'omissione di questa riga di codice.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Configurare Entity Framework per inizializzare il database con dati di test

Entity Framework può automaticamente create (o eliminare e ricreare) un database per l'utente quando viene eseguita l'applicazione. È possibile specificare che questa operazione deve essere eseguita ogni volta che viene eseguita l'applicazione o solo quando il modello non è sincronizzato con il database esistente. È anche possibile scrivere un `Seed` che Entity Framework chiama automaticamente dopo aver creato il database per popolarlo con i dati di test.

Il comportamento predefinito consiste nel creare un database solo se non esiste (e generare un'eccezione se il modello è stato modificato e il database esiste già). In questa sezione si specificherà che il database deve essere eliminato e ricreato ogni volta che viene modificato il modello. Eliminazione del database comporta la perdita di tutti i dati. Si tratta in genere bene durante lo sviluppo, perché il `Seed` metodo viene eseguito quando il database viene ricreato e crea nuovamente i dati di test. Ma nell'ambiente di produzione in genere non si vuole perdere tutti i dati ogni volta che è necessario modificare lo schema del database. In un secondo momento verrà illustrato come gestire le modifiche al modello usando migrazioni Code First per modificare lo schema del database anziché dover eliminare e ricreare il database.

1. Nella cartella di DAL, creare un nuovo file di classe denominato *SchoolInitializer.cs* e sostituire il codice del modello con il codice seguente, che fa sì che un database da creare quando necessario e carica i dati di test nel nuovo database.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   Il `Seed` accetta l'oggetto di contesto di database come parametro di input e il codice nel metodo Usa l'oggetto per aggiungere nuove entità nel database. Per ogni tipo di entità, il codice crea una raccolta di nuove entità, li aggiunge a appropriato `DbSet` proprietà e quindi Salva le modifiche al database. Non è necessario chiamare il `SaveChanges` metodo dopo ogni gruppo di entità, come avviene in questo caso, ma questa operazione consente di individuare l'origine di un problema se si verifica un'eccezione mentre il codice è la scrittura nel database.

2. Per indicare a Entity Framework da usare la classe dell'inizializzatore, aggiungere un elemento per il `entityFramework` elemento dell'applicazione *Web. config* (il file di un file nella cartella radice del progetto), come illustrato nell'esempio seguente:

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   Il `context type` specifica il nome della classe contesto completo e l'assembly si trovi, e il `databaseinitializer type` specifica il nome completo della classe inizializzatore e l'assembly si trova in. (Quando non si vuole usare l'inizializzatore a Entity Framework, è possibile impostare un attributo nel `context` elemento: `disableDatabaseInitialization="true"`.) Per altre informazioni, vedere [impostazioni del File di configurazione](/ef/ef6/fundamentals/configuring/config-file).

   Un'alternativa all'impostazione l'inizializzatore *Web. config* file è eseguire questa operazione nel codice tramite l'aggiunta di un `Database.SetInitializer` istruzione per il `Application_Start` metodo nella *Global.asax.cs* file. Per altre informazioni, vedere [Understanding inizializzatori di Database in Code First di Entity Framework](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

L'applicazione è ora configurata in modo che quando si accede al database per la prima volta in una determinata esecuzione dell'applicazione, Entity Framework consente di confrontare il database al modello (il `SchoolContext` e classi di entità). Se è presente una differenza, l'applicazione elimina e ricrea il database.

> [!NOTE]
> Quando si distribuisce un'applicazione a un server web di produzione, è necessario rimuovere o disabilitare il codice che viene eliminato e ricreato il database. È possibile farlo in un'esercitazione successiva di questa serie.

## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Configurare Entity Framework per usare un database di SQL Server Express LocalDB

[LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) è una versione leggera del motore di database SQL Server Express. È facile da installare e configurare, viene avviato su richiesta e viene eseguito in modalità utente. LocalDB viene eseguito in una modalità di esecuzione speciale di SQL Server Express che consente di lavorare con i database come *mdf* file. È possibile inserire i file di database LocalDB nel *App\_dati* cartella del progetto web se si desidera essere in grado di copiare il database con il progetto. La funzionalità dell'istanza utente in SQL Server Express consente inoltre di usare *mdf* i file, ma la funzionalità dell'istanza utente è deprecato; pertanto, Local DB è consigliato per l'utilizzo di *mdf* file. Local DB viene installato per impostazione predefinita con Visual Studio.

In genere, SQL Server Express non viene utilizzato per le applicazioni web di produzione. LocalDB in particolare non è consigliato per la produzione con un'applicazione web perché non è progettato per funzionare con IIS.

- In questa esercitazione si utilizzerà LocalDB. Aprire l'applicazione *Web. config* file e aggiungere un `connectionStrings` elemento che precede il `appSettings` elemento, come illustrato nell'esempio seguente. (Assicurarsi di aggiornare il *Web. config* file nella cartella radice del progetto. È inoltre disponibile un' *Web. config* del file nei *viste* sottocartella che non è necessario aggiornare.)

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Specifica la stringa di connessione è stato aggiunto che Entity Framework userà un database LocalDB denominato *ContosoUniversity1.mdf*. (Il database non esiste già ma verrà creata da Entity Framework). Se si desidera creare il database di *App\_Data* cartella, è possibile aggiungere `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` alla stringa di connessione. Per altre informazioni sulle stringhe di connessione, vedere [stringhe di connessione di SQL Server per le applicazioni Web ASP.NET](/previous-versions/aspnet/jj653752(v=vs.110)).

Non è necessaria una stringa di connessione nel *Web. config* file. Se non viene fornita una stringa di connessione, Entity Framework Usa una stringa di connessione predefinito basata sulla classe del contesto. Per altre informazioni, vedere [Code First per un nuovo Database](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-a-student-controller-and-views"></a>Creare un controller per studenti e visualizzazioni

A questo punto si creerà una pagina web per visualizzare i dati. Il processo di richiesta automaticamente i dati attiva la creazione del database. Inizierai creando un nuovo controller. Ma prima di procedere, compilare il progetto per rendere le classi modello e al contesto disponibili per lo scaffolding di controller MVC.

1. Fare doppio clic sui **controller** cartella **Esplora soluzioni**, selezionare **Add**e quindi fare clic su **nuovo elemento di scaffolding**.
2. Nel **Add Scaffold** finestra di dialogo **Controller MVC 5 con visualizzazioni, mediante Entity Framework**, quindi scegliere **Add**.

     ![Aggiungi finestra di dialogo di scaffolding in Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. Nel **Aggiungi Controller** finestra di dialogo, effettuare le selezioni seguenti e quindi scegliere **Add**:

   - Classe modello: **Student (ContosoUniversity.Models)**. (Se non trovi questo opzione nell'elenco a discesa, compilare il progetto e riprovare.)
   - Classe del contesto dati: **SchoolContext (ContosoUniversity.DAL)**.
   - Nome del controller: **StudentController** (non StudentsController).
   - Lasciare i valori predefiniti per gli altri campi.

     ![Aggiungi finestra di dialogo Controller in Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-controller.png)

     Quando fa clic su **Add**, l'utilità di scaffolding crea una *StudentController.cs* file e un set di viste (*. cshtml* file) che funzionano con il controller. In futuro quando si creano progetti che utilizzano Entity Framework, è possibile usufruire di alcune funzionalità aggiuntive di scaffolder: creazione di una classe di modello, non creare una stringa di connessione e quindi il **aggiungere Controller** casella specificare **nuovo contesto dati** selezionando il **+** accanto a **classe del contesto dati**. L'utilità di scaffolding creerà la `DbContext` classe e le stringhe di connessione, nonché le visualizzazioni e controller.
4. Visual Studio apre il *Controllers\StudentController.cs* file. Vedere che sia stata creata una variabile di classe che crea un'istanza di un oggetto di contesto di database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     Il `Index` metodo di azione Ottiene un elenco di studenti dal *studenti* set tramite la lettura di entità di `Students` proprietà dell'istanza di contesto del database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     Il *Student\Index.cshtml* Vista sono riportati in questo elenco in una tabella:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Premere **Ctrl**+**F5** per eseguire il progetto. (Se si verifica un errore "Non è possibile creare una copia Shadow", chiudere il browser e riprovare.)

     Fare clic sui **studenti** scheda per visualizzare i dati di test che di `Seed` metodo inserito. A seconda delle dimensione la finestra del browser è, si noterà il collegamento della scheda degli studenti nella barra degli indirizzi superiore oppure è possibile fare clic su nell'angolo superiore destro per visualizzare il collegamento.

     ![Pulsante di menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Pagina di indice degli studenti](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Visualizzare il database

Quando si esegue la pagina di studenti e l'applicazione ha tentato di accedere al database, EF scoperto che non vi era alcun database e creato uno. EF ha quindi il metodo di inizializzazione per popolare il database con i dati.

È possibile usare **Esplora Server** oppure **Esplora oggetti di SQL Server** (SSOX) per visualizzare il database in Visual Studio. Per questa esercitazione, si utilizzerà **Esplora Server**.

1. Chiudere il browser.
2. In **Esplora Server**, espandere **connessioni dati** (potrebbe essere necessario selezionare prima il pulsante Aggiorna), espandere **contesto scuola (ContosoUniversity)**, quindi espandere  **Le tabelle** per vedere le tabelle nel nuovo database.

    ![Tabelle di database in Esplora Server](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)

3. Fare doppio clic il **studente** tabella e fare clic su **Mostra dati tabella** per visualizzare le colonne che sono state create e le righe che sono state inserite nella tabella.

    ![Tabella Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/table-data.png)
4. Chiudi il **Esplora Server** connessione.

Il *ContosoUniversity1.mdf* e *ldf* file di database si trovano nel *% USERPROFILE %* cartella.

Perché si sta usando il `DropCreateDatabaseIfModelChanges` inizializzatore, ora è possibile apportare una modifica al `Student` classe, eseguire nuovamente l'applicazione e il database sarà automaticamente ricreato e rispecchierà la modifica. Ad esempio, se si aggiunge un `EmailAddress` proprietà per il `Student` classe, eseguire nuovamente la pagina di studenti e quindi di cercare la tabella nuovamente, verrà visualizzato un nuovo `EmailAddress` colonna.

## <a name="conventions"></a>Convenzioni

La quantità di codice era necessario scrivere affinché Entity Framework in grado di creare un database completo per l'utente è minima di *convenzioni*, o presupposti che rende Entity Framework. Alcuni di essi sono già stati annotati o sono stati utilizzati senza essere stati necessario tenere in considerazione:

- Le forme pluralizzate di nomi delle classi di entità vengono utilizzate come nomi di tabella.
- I nomi della proprietà di entità vengono usati come nomi di colonna.
- Proprietà dell'entità che sono denominate `ID` oppure *NomeClasse* `ID` vengono riconosciute come proprietà di chiave primaria.
- Una proprietà viene interpretata come una proprietà di chiave esterna se è denominata *&lt;il nome di proprietà di navigazione&gt;&lt;il nome di proprietà di chiave primaria&gt;* (ad esempio, `StudentID` per il `Student` proprietà di navigazione dopo il `Student` chiave primaria dell'entità è `ID`). Proprietà di chiave esterna possono anche essere denominate lo stesso semplicemente &lt;nome proprietà della chiave primaria&gt; (ad esempio, `EnrollmentID` poiché la `Enrollment` chiave primaria dell'entità è `EnrollmentID`).

Si è visto che è possano eseguire l'override di convenzioni. Ad esempio, è specificato che i nomi di tabella non devono essere pluralizzati e si vedrà in seguito come contrassegnare in modo esplicito una proprietà come proprietà di chiave esterna. Verranno fornite altre informazioni sulle convenzioni e come eseguire l'override nel [creazione di un modello di dati più complessi](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie. Per altre informazioni sulle convenzioni, vedere [convenzioni del primo codice](/ef/ef6/modeling/code-first/conventions/built-in).

## <a name="summary"></a>Riepilogo

Aver creato una semplice applicazione che utilizza Entity Framework e SQL Server Express LocalDB per archiviare e visualizzare i dati. La prossima esercitazione si apprenderà come eseguire base creare, leggere, aggiornare ed eliminazione (CRUD).

Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare.

Collegamenti ad altre risorse di Entity Framework sono disponibili nel [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [avanti](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
