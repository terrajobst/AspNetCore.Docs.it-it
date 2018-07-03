---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Introduzione a Entity Framework 6 Code First con MVC 5 | Microsoft Docs
author: tdykstra
description: 'È disponibile una versione più recente di questa serie di esercitazioni: Introduzione a ASP.NET Core ed Entity Framework Core con Visual Studio 2015. Universi di Contoso...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1b8d78954746cd6908f9ca9c2a51591f45fa01f7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403076"
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Introduzione a Entity Framework 6 Code First con MVC 5
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > È disponibile una versione più recente di questa serie di esercitazioni: [Introduzione a ASP.NET Core ed Entity Framework Core con Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 5 con l'Entity Framework 6 e Visual Studio 2013. Questa esercitazione Usa il flusso di lavoro di Code First. Per informazioni su come scegliere tra Code First, Database First e Model First, vedere [flussi di lavoro sviluppo di Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> L'applicazione di esempio è un sito Web per una fittizia Contoso University. Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati. Questa serie di esercitazioni illustra come compilare l'applicazione di esempio Contoso University. È possibile [scaricare l'applicazione completata](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> È disponibile una versione di Visual Basic tradotto da Mike Brind: [MVC 5 con Entity Framework 6 in Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) sul sito Mikesdotnetting.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (pacchetto NuGet EntityFramework 6.1.0)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (facoltativo)
>   
> 
> L'esercitazione dovrebbe funzionare anche con [Visual Studio 2013 Express per Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) o Visual Studio 2012. Il [versione di Visual Studio 2012 di Microsoft Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511) è obbligatorio per la distribuzione di Windows Azure con Visual Studio 2012.
> 
> 
> ## <a name="tutorial-versions"></a>Versioni dell'esercitazione
> 
> Per le versioni precedenti di questa esercitazione, vedere [4.1 di EF / MVC 3 eBook](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) e [Introduzione a EF 5 con MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), il [Entity Framework e LINQ al forum su entità](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).
> 
> Se si verifica un problema che non è possibile risolvere, è generalmente possibile trovare la soluzione al problema confrontando il codice del progetto completato che è possibile scaricare. Per alcuni errori comuni e come risolverli, vedere [gli errori comuni e soluzioni o soluzioni alternative per loro.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>L'applicazione Web di Contoso University

L'applicazione che sarà compilata in queste esercitazioni è un semplice sito Web universitario.

Gli utenti possono visualizzare e aggiornare le informazioni che riguardano studenti, corsi e insegnanti. Di seguito sono disponibili alcune schermate che saranno create.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Modifica studente](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Lo stile dell'interfaccia utente del sito è simile a quanto è stato generato tramite i modelli predefiniti. L'esercitazione si concentra pertanto soprattutto sull'uso di Entity Framework.

## <a name="prerequisites"></a>Prerequisiti

Visualizzare **le versioni del Software** nella parte superiore della pagina. Entity Framework 6 non è un prerequisito, poiché si installa il pacchetto NuGet di Entity Framework come parte dell'esercitazione.

## <a name="create-an-mvc-web-application"></a>Creare un'applicazione Web MVC

Aprire Visual Studio e creare un nuovo progetto c# Web denominato "ContosoUniversity".

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo per selezionare la **MVC** modello.

Se il **ospita nel cloud** casella di controllo la **Microsoft Azure** sezione è selezionata, deselezionarla.

Fare clic su **Modifica autenticazione**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

Nel **Modifica autenticazione** finestra di dialogo **Nessuna autenticazione**, quindi fare clic su **OK**. Per questa esercitazione si non richiedendo agli utenti di accedere o limitare l'accesso basato sull'utente collegato.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Nella finestra di dialogo Nuovo progetto ASP.NET, fare clic su **OK** per creare il progetto.

## <a name="set-up-the-site-style"></a>Configurare lo stile del sito

Con alcune modifiche è possibile impostare il menu del sito, il layout e la home page.

Aprire *Views\Shared\\layout. cshtml*e apportare le modifiche seguenti:

- Modificare tutte le occorrenze di "My ASP.NET Application" e "Application name" in "Contoso University".
- Aggiungere le voci di menu per gli studenti, corsi, instructors (insegnanti) e i reparti ed eliminare la voce del contatto.

Le modifiche sono evidenziate.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

Nelle *Views\Home\Index.cshtml*, sostituire il contenuto del file con il codice seguente sostituire il testo su ASP.NET e MVC testo relativo a questa applicazione:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Premere CTRL+F5 per eseguire il sito. Viene visualizzata la pagina Home page con il menu principale.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Installare Entity Framework 6

Dal **degli strumenti** menu fare clic su **Gestione pacchetti NuGet** e quindi fare clic su **Package Manager Console**.

Nel **Console di gestione pacchetti** finestra immettere il comando seguente:

`Install-Package EntityFramework`

![Entity Framework installati](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

L'immagine Mostra 6.0.0 in corso l'installazione, ma NuGet installerà la versione più recente di Entity Framework (escluse le versioni non definitive), che al momento dell'aggiornamento più recente con l'esercitazione è 6.1.1.

Questo passaggio è uno dei pochi passaggi che in questa esercitazione si dispone di eseguire l'operazione manualmente, ma che potrebbe essere stata effettuata automaticamente dalla funzionalità di scaffolding di ASP.NET MVC. Li si sta eseguendo manualmente in modo che è possibile visualizzare i passaggi necessari per l'utilizzo di Entity Framework. Si userà lo scaffolding in un secondo momento per creare le visualizzazioni e controller MVC. In alternativa è possibile lasciare che lo scaffolding automaticamente installare il pacchetto NuGet di Entity Framework, creare la classe del contesto del database e creare la stringa di connessione. Quando è pronti per eseguire questa operazione in questo modo, sufficiente è ignorare tali passaggi ed eseguire lo scaffolding di controller MVC dopo aver creato le classi di entità.

## <a name="create-the-data-model"></a>Creare il modello di dati

A questo punto è possibile creare le classi delle entità per l'applicazione di Contoso University. Si inizia con le tre entità seguenti:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Esiste una relazione uno-a-molti tra le entità `Student` e `Enrollment` ed esiste una relazione uno-a-molti tra le entità `Course` e `Enrollment`. In altre parole, uno studente può iscriversi a un numero qualsiasi di corsi e un corso può avere un numero qualsiasi di studenti iscritti.

Nelle sezioni seguenti viene creata una classe per ognuna di queste entità.

> [!NOTE]
> Se si prova a compilare il progetto prima di aver creato tutte queste classi di entità, si otterrà gli errori del compilatore.


### <a name="the-student-entity"></a>L'entità Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Nel *modelli* cartella, creare un file di classe denominato *Student.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

La proprietà `ID` diventa la colonna di chiave primaria della tabella di database che corrisponde a questa classe. Per impostazione predefinita, Entity Framework interpreta una proprietà denominata `ID` oppure *NomeClasse* `ID` come chiave primaria.

Il `Enrollments` proprietà è una *proprietà di navigazione*. Le proprietà di navigazione contengono altre entità correlate a questa entità. In questo caso, il `Enrollments` proprietà di un `Student` entità conterrà tutte le `Enrollment` entità correlate a quella `Student` entità. In altre parole, se un determinato `Student` riga nel database ha due correlate `Enrollment` righe (valore righe che contengono una chiave primaria dello studente in loro `StudentID` colonna chiave esterna), che `Student` dell'entità `Enrollments` proprietà di navigazione contiene quelle due `Enrollment` entità.

Le proprietà di navigazione sono in genere definite come `virtual` in modo che è possibile sfruttare alcune funzionalità di Entity Framework, ad esempio *caricamento lazy*. (Caricamento lazy sarà spiegato più avanti, nelle [lettura di dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie.)

Se una proprietà di navigazione può contenere più entità (come nel caso di relazioni molti-a-molti e uno-a-molti), il tipo della proprietà deve essere un elenco in cui le voci possono essere aggiunte, eliminate e aggiornate, come ad esempio `ICollection`.

### <a name="the-enrollment-entity"></a>L'entità Enrollment

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Nella cartella *Models* creare *Enrollment.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Il `EnrollmentID` proprietà potranno essere la chiave primaria, questa entità Usa il *classname* `ID` modello anziché `ID` come visto nel `Student` entità. In genere si sceglie un criterio e lo si usa poi in tutto il modello di dati. In questo caso la variazione illustra che è possibile sia uno sia l'altro criterio. In un'esercitazione successiva, si noterà come l'uso `ID` senza `classname` rende più semplice implementare l'ereditarietà nel modello di dati.

Il `Grade` proprietà è un [enum](https://msdn.microsoft.com/data/hh859576.aspx). Il punto interrogativo dopo la `Grade` dichiarazione del tipo indica che il `Grade` è di proprietà [nullable](https://msdn.microsoft.com/library/2cf62fcy.aspx). Un voto il cui valore è null è diverso da un voto con valore zero, ovvero null indica che un voto è sconosciuto o non è stato ancora assegnato.

La proprietà `StudentID` è una chiave esterna e la proprietà di navigazione corrispondente è `Student`. Un'entità `Enrollment` è associata a un'entità `Student`, pertanto la proprietà può contenere un'unica entità `Student`, a differenza della proprietà di navigazione `Student.Enrollments` vista in precedenza, che può contenere più entità `Enrollment`.

La proprietà `CourseID` è una chiave esterna e la proprietà di navigazione corrispondente è `Course`. Un'entità `Enrollment` è associata a un'entità `Course`.

Entity Framework interpreta una proprietà come proprietà di chiave esterna se è denominata *&lt;il nome di proprietà di navigazione&gt;&lt;il nome di proprietà di chiave primaria&gt;* (ad esempio `StudentID`per il `Student` proprietà di navigazione dopo la `Student` chiave primaria dell'entità è `ID`). Proprietà di chiave esterna possono anche essere denominate lo stesso semplicemente *&lt;nome proprietà della chiave primaria&gt;* (ad esempio, `CourseID` poiché il `Course` chiave primaria dell'entità è `CourseID`).

### <a name="the-course-entity"></a>L'entità Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Nel *modelli* cartella, creare *Course.cs*, sostituendo il codice del modello con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La proprietà `Enrollments` rappresenta una proprietà di navigazione. È possibile correlare un'entità `Course` a un numero qualsiasi di entità `Enrollment`.

Ciò che diremo informazioni sul [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) attributo in un'esercitazione successiva di questa serie. In pratica, questo attributo consente di immettere la chiave primaria per il corso invece di essere generata dal database.

## <a name="create-the-database-context"></a>Creare il contesto di database

La classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati è il *contesto di database* classe. Questa classe viene creata mediante derivazione dal [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) classe. Nel codice vengono specificate le entità incluse nel modello di dati. È anche possibile personalizzare un determinato comportamento di Entity Framework. In questo progetto la classe è denominata `SchoolContext`.

Per creare una cartella nel progetto ContosoUniversity, fare clic sul progetto in **Esplora soluzioni** e fare clic su **Add**, quindi fare clic su **nuova cartella**. Denominare la nuova cartella *DAL* (per il livello di accesso ai dati). In tale cartella creare un nuovo file di classe denominato *SchoolContext.cs*e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Se si specifica set di entità

Questo codice crea un [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) proprietà per ogni set di entità. Nella terminologia di Entity Framework, un' *set di entità* in genere corrisponde a una tabella di database e un *entità* corrisponde a una riga nella tabella.

> [!NOTE] 
> 
> Venisse omessa il `DbSet<Enrollment>` e `DbSet<Course>` istruzioni, potrebbe comunque funzionare lo stesso. Entity Framework le include in modo implicito perché l'entità `Student` fa riferimento all'entità `Enrollment` e l'entità `Enrollment` fa riferimento all'entità `Course`.


### <a name="specifying-the-connection-string"></a>Specifica la stringa di connessione

Il nome della stringa di connessione (che si aggiungerà il file Web. config in un secondo momento) viene passato al costruttore.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

È inoltre possibile passare la stringa di connessione stessa anziché il nome di uno che viene archiviato nel file Web. config. Per altre informazioni sulle opzioni che consentono di specificare il database da usare, vedere [Entity Framework - modelli e le connessioni](https://msdn.microsoft.com/data/jj592674).

Se non si specifica una stringa di connessione o il nome di uno in modo esplicito, Entity Framework presuppone che il nome di stringa di connessione sia lo stesso nome della classe. Il nome di stringa di connessione predefinito in questo esempio sarebbe quindi `SchoolContext`, quella cosa si sta specificando in modo esplicito.

### <a name="specifying-singular-table-names"></a>Specifica dei nomi di tabella al singolare

Il `modelBuilder.Conventions.Remove` istruzione il [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metodo impedisce che i nomi delle tabelle in corso pluralizzati. Se non è stato in questo caso, le tabelle generate nel database verrà denominate `Students`, `Courses`, e `Enrollments`. Al contrario, i nomi di tabella saranno `Student`, `Course`, e `Enrollment`. Gli sviluppatori non hanno un'opinione unanime sul fatto che i nomi di tabella debbano essere pluralizzati oppure no. Questa esercitazione Usa la forma singolare, ma il punto importante è che è possibile selezionare qualsiasi formato si preferisce includendo o l'omissione di questa riga di codice.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Configurare Entity Framework per inizializzare il database con dati di test

Entity Framework può automaticamente create (o eliminare e ricreare) un database per l'utente quando viene eseguita l'applicazione. È possibile specificare che questa operazione deve essere eseguita ogni volta che viene eseguita l'applicazione o solo quando il modello non è sincronizzato con il database esistente. È anche possibile scrivere un `Seed` metodo che Entity Framework chiama automaticamente dopo aver creato il database per popolarlo con i dati di test.

Il comportamento predefinito consiste nel creare un database solo se non esiste (e generare un'eccezione se il modello è stato modificato e il database esiste già). In questa sezione si specificherà che il database deve essere eliminato e ricreato ogni volta che viene modificato il modello. Eliminazione del database comporta la perdita di tutti i dati. Si tratta in genere OK durante lo sviluppo, perché il `Seed` metodo viene eseguito quando il database viene ricreato e crea nuovamente i dati di test. Ma nell'ambiente di produzione in genere non si vuole perdere tutti i dati ogni volta che è necessario modificare lo schema del database. In un secondo momento verrà illustrato come gestire le modifiche al modello usando migrazioni Code First per modificare lo schema del database anziché dover eliminare e ricreare il database.

Nella cartella di DAL, creare un nuovo file di classe denominato *SchoolInitializer.cs* e sostituire il codice del modello con il  
codice che fa sì che un database da creare quando necessario e carica i dati di test nel nuovo database.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

Il `Seed` accetta l'oggetto di contesto di database come parametro di input e il codice nel metodo Usa l'oggetto per aggiungere nuove entità nel database. Per ogni tipo di entità, il codice crea una raccolta di nuove entità, li aggiunge a appropriato `DbSet` proprietà e quindi Salva le modifiche al database. Non è necessario chiamare il `SaveChanges` metodo dopo ogni gruppo di entità, come avviene in questo caso, ma questa operazione consente di individuare l'origine di un problema se si verifica un'eccezione mentre il codice è la scrittura nel database.

Per indicare a Entity Framework da usare la classe dell'inizializzatore, aggiungere un elemento per il `entityFramework` elemento dell'applicazione *Web. config* (il file di un file nella cartella radice del progetto), come illustrato nell'esempio seguente:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

Il `context type` specifica il nome della classe contesto completo e l'assembly si trovi, e il `databaseinitializer type` specifica il nome completo della classe inizializzatore e l'assembly si trova in. (Quando non si vuole usare l'inizializzatore a Entity Framework, è possibile impostare un attributo nel `context` elemento: `disableDatabaseInitialization="true"`.) Per altre informazioni, vedere [Entity Framework - impostazioni del File Config](https://msdn.microsoft.com/data/jj556606).

Come alternativa all'impostazione dell'inizializzatore nel *Web. config* file è eseguire questa operazione nel codice tramite l'aggiunta di un `Database.SetInitializer` istruzione per il `Application_Start` metodo nella *Global.asax.cs* file. Per altre informazioni, vedere [Understanding inizializzatori di Database in Code First di Entity Framework](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

L'applicazione è ora impostata backup in modo che quando si accede al database per la prima volta in una determinata esecuzione del  
applicazione, Entity Framework consente di confrontare il database al modello (il `SchoolContext` e classi di entità). Se è presente una differenza, l'applicazione elimina e ricrea il database.

> [!NOTE]
> Quando si distribuisce un'applicazione a un server web di produzione, è necessario rimuovere o disabilitare il codice che viene eliminato e ricreato il database. È possibile farlo in un'esercitazione successiva di questa serie.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Configurare Entity Framework per usare un database di SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) è una versione leggera del motore di SQL Server Express Database. È facile da installare e configurare, viene avviato su richiesta e viene eseguito in modalità utente. LocalDB viene eseguito in una modalità di esecuzione speciale di SQL Server Express che consente di lavorare con i database come *mdf* file. È possibile inserire i file di database LocalDB nel *App\_dati* cartella del progetto web se si desidera essere in grado di copiare il database con il progetto. La funzionalità dell'istanza utente in SQL Server Express consente inoltre di usare *mdf* i file, ma la funzionalità dell'istanza utente è deprecato; pertanto, Local DB è consigliato per l'utilizzo di *mdf* file. In Visual Studio 2012 e versioni successive, Local DB viene installato per impostazione predefinita con Visual Studio.

In genere SQL Server Express non viene utilizzato per le applicazioni web di produzione. LocalDB in particolare non è consigliato per la produzione con un'applicazione web perché non è progettato per funzionare con IIS.

In questa esercitazione si utilizzerà LocalDB. Aprire l'applicazione *Web. config* file e aggiungere un `connectionStrings` elemento che precede il `appSettings` elemento, come illustrato nell'esempio seguente. (Assicurarsi di aggiornare il *Web. config* file nella cartella radice del progetto. È inoltre disponibile una *Web. config* file si trova nel *viste* sottocartella che non è necessario Aggiorna.)

Se si usa Visual Studio 2015, sostituire "v11.0" nella stringa di connessione con "MSSQLLocalDB", perché è stato modificato il nome di istanza di SQL Server predefinito.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Specifica la stringa di connessione è stato aggiunto che Entity Framework userà un database LocalDB denominato *ContosoUniversity1.mdf*. (Il database non esiste ancora; EF creerà.) Se si desidera che il database da creare nelle *App\_Data* cartella, è possibile aggiungere `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` alla stringa di connessione. Per altre informazioni sulle stringhe di connessione, vedere [stringhe di connessione di SQL Server per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Non è effettivamente necessario avere una stringa di connessione nel *Web. config* file. Se non viene fornita una stringa di connessione, Entity Framework userà uno basato sulla classe del contesto predefinito. Per altre informazioni, vedere [Code First per un nuovo Database](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Creazione di un Controller per studenti e visualizzazioni

A questo punto si creerà una pagina web per visualizzare i dati, e attiverà automaticamente il processo di richiesta dati  
la creazione del database. Inizierai creando un nuovo controller. Ma prima di procedere, compilare il progetto per rendere le classi modello e al contesto disponibili per lo scaffolding di controller MVC.

1. Fare doppio clic sui **controller** cartella **Esplora soluzioni**, selezionare **Add**e quindi fare clic su **nuovo elemento di scaffolding**.
2. Nel **Add Scaffold** finestra di dialogo **Controller MVC 5 con visualizzazioni, mediante Entity Framework**.

     ![Aggiungi scaffolding](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. Nella finestra di dialogo Aggiungi Controller, effettuare le selezioni seguenti e quindi fare clic su **Add**:

   - Classe modello: **Student (ContosoUniversity.Models)**. (Se non trovi questo opzione nell'elenco a discesa, compilare il progetto e riprovare.)
   - Classe del contesto dati: **SchoolContext (ContosoUniversity.DAL)**.
   - Nome del controller: **StudentController** (non StudentsController).
   - Lasciare i valori predefiniti per gli altri campi.

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     Quando fa clic su **Add**, l'utilità di scaffolding crea un file StudentController.cs e un set di visualizzazioni (file con estensione cshtml) che funzionano con il controller. In futuro, quando si creano progetti che usano Entity Framework è possibile inoltre sfruttare alcune funzionalità aggiuntive dell'utilità di scaffolding: è sufficiente creare la prima classe di modello, non creare una stringa di connessione e quindi il **Aggiungi Controller** casella specificare nuova classe di contesto. L'utilità di scaffolding creerà la `DbContext` classe e le stringhe di connessione, nonché le visualizzazioni e controller.
4. Visual Studio apre il *Controllers\StudentController.cs* file. Vedrai che una variabile di classe è stata creata che crea un'istanza di un oggetto di contesto di database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     Il `Index` metodo di azione Ottiene un elenco di studenti dal *studenti* set tramite la lettura di entità di `Students` proprietà dell'istanza di contesto del database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     Il *Student\Index.cshtml* Vista sono riportati in questo elenco in una tabella:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Premere CTRL+F5 per eseguire il progetto. (Se si verifica un errore "Non è possibile creare una copia Shadow", chiudere il browser e riprovare.)

     Fare clic sui **studenti** scheda per visualizzare i dati di test che di `Seed` metodo inserito. A seconda delle dimensione la finestra del browser è, si noterà il collegamento della scheda degli studenti nella barra degli indirizzi superiore oppure è possibile fare clic su nell'angolo superiore destro per visualizzare il collegamento.

     ![Pulsante di menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Pagina di indice degli studenti](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Visualizzare il database

Quando è stata eseguita la pagina di studenti e l'applicazione ha provato ad accedere al database, Entity Framework è emerso che si è verificato alcun database e pertanto creato uno, quindi è stato eseguito il metodo di inizializzazione per popolare il database con i dati.

È possibile usare **Esplora Server** oppure **Esplora oggetti di SQL Server** (SSOX) per visualizzare il database in Visual Studio. Per questa esercitazione si userà **Esplora Server**. (In anteriori al 2013, edizioni Visual Studio Express **Esplora Server** viene chiamato **Esplora Database**.)

1. Chiudere il browser.
2. Nelle **Esplora Server**, espandere **connessioni dati**, espandere **contesto dell'istituto di istruzione (ContosoUniversity)**, quindi espandere **tabelle** per visualizzare le tabelle nel nuovo database.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Fare doppio clic il **studente** tabella e fare clic su **Mostra dati tabella** per visualizzare le colonne che sono state create e le righe che sono state inserite nella tabella.

    ![Tabella Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Chiudi il **Esplora Server** connessione.

Il *ContosoUniversity1.mdf* e *ldf* file di database si trovano nel `C:\Users\<yourusername>` cartella.

Perché si sta usando il `DropCreateDatabaseIfModelChanges` inizializzatore, ora è possibile apportare una modifica al `Student` classe, eseguire nuovamente l'applicazione e il database sarà automaticamente ricreato e rispecchierà la modifica. Ad esempio, se si aggiunge un `EmailAddress` proprietà per il `Student` classe, eseguire di nuovo la pagina di studenti e quindi esaminare la tabella anche in questo caso, verrà visualizzato un nuovo `EmailAddress` colonna.

## <a name="conventions"></a>Convenzioni

La quantità di codice era necessario scrivere affinché Entity Framework sia in grado di creare un database completo per l'utente è minima grazie all'uso di *convenzioni*, o di ipotesi di Entity Framework. Alcuni di essi sono già stati annotati o sono stati utilizzati senza essere stati necessario tenere in considerazione:

- Le forme pluralizzate di nomi delle classi di entità vengono utilizzate come nomi di tabella.
- I nomi della proprietà di entità vengono usati come nomi di colonna.
- Proprietà dell'entità che sono denominate `ID` oppure *NomeClasse* `ID` vengono riconosciute come proprietà di chiave primaria.
- Una proprietà viene interpretata come una proprietà di chiave esterna se è denominata *&lt;il nome di proprietà di navigazione&gt;&lt;il nome di proprietà di chiave primaria&gt;* (ad esempio, `StudentID` per il `Student` proprietà di navigazione dopo il `Student` chiave primaria dell'entità è `ID`). Proprietà di chiave esterna possono anche essere denominate lo stesso semplicemente &lt;nome proprietà della chiave primaria&gt; (ad esempio, `EnrollmentID` poiché la `Enrollment` chiave primaria dell'entità è `EnrollmentID`).

Si è visto che è possano eseguire l'override di convenzioni. Ad esempio, è specificato che i nomi di tabella non devono essere pluralizzati e si vedrà in seguito come contrassegnare in modo esplicito una proprietà come proprietà di chiave esterna. Verranno fornite altre informazioni sulle convenzioni e come eseguire l'override nel [creazione di un modello di dati più complessi](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie. Per altre informazioni sulle convenzioni, vedere [convenzioni del primo codice](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Riepilogo

A questo punto è stata creata una semplice applicazione che usa Entity Framework e SQL Server Express LocalDB per archiviare e visualizzare i dati. Nell'esercitazione si apprenderà come eseguire CRUD di base (creare, leggere, aggiornare ed eliminare) le operazioni.

Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare. È anche possibile richiedere nuovi argomenti in [Mostra Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Collegamenti ad altre risorse di Entity Framework sono disponibili nel [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [avanti](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
