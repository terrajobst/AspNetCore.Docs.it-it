---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Introduzione a Entity Framework 6 Code First con MVC 5 | Documenti Microsoft
author: tdykstra
description: 'È disponibile una versione più recente di questa serie di esercitazioni: Introduzione a ASP.NET di base e di Entity Framework Core usando Visual Studio 2015. Universi di Contoso...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 2417a872bb57b18f4a61ef70f5dd35cb3d94ff73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Introduzione a Entity Framework 6 Code First con MVC 5
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > È disponibile una versione più recente di questa serie di esercitazioni: [acquisire familiarità con Visual Studio 2015 Entity Framework Core e ASP.NET Core](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con di Entity Framework 6 e Visual Studio 2013. Questa esercitazione viene utilizzato il flusso di lavoro Code First. Per informazioni su come scegliere tra Code First, Database First e Model First, vedere [flussi di lavoro di Entity Framework sviluppo](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> L'applicazione di esempio è un sito Web per una fittizia Contoso University. Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati. Questa serie di esercitazioni viene illustrato come compilare l'applicazione di esempio Contoso University. È possibile [il download dell'applicazione completata](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> È disponibile una versione di Visual Basic convertita da Mike Brind: [MVC 5 con 6 di Entity Framework in Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) sul sito Mikesdotnetting.
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
> L'esercitazione dovrebbe funzionare anche con [Visual Studio 2013 Express per Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) o Visual Studio 2012. Il [Visual Studio 2012 versione di Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511) necessarie per la distribuzione di Windows Azure con Visual Studio 2012.
> 
> 
> ## <a name="tutorial-versions"></a>Versioni dell'esercitazione
> 
> Per le versioni precedenti di questa esercitazione, vedere [4.1 di EF / MVC 3 e-book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) e [Introduzione a Entity Framework 5 utilizzando MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. Nel caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework e LINQ to forum entità](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).
> 
> Se si esegue un problema che non è possibile risolvere, è possibile trovare in genere la soluzione al problema confrontando il codice per il progetto completato che è possibile scaricare. Per alcuni errori comuni e su come risolverli, vedere [gli errori comuni e le soluzioni per essi.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>L'applicazione Web di Contoso University

L'applicazione che sarà compilata in queste esercitazioni è un semplice sito Web universitario.

Gli utenti possono visualizzare e aggiornare le informazioni che riguardano studenti, corsi e insegnanti. Di seguito sono disponibili alcune schermate che saranno create.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Modifica di Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Lo stile dell'interfaccia utente del sito è simile a quanto è stato generato tramite i modelli predefiniti. L'esercitazione si concentra pertanto soprattutto sull'uso di Entity Framework.

## <a name="prerequisites"></a>Prerequisiti

Vedere **versioni del Software** nella parte superiore della pagina. Entity Framework 6 non è un prerequisito, perché il pacchetto NuGet di Entity Framework è stato installato come parte dell'esercitazione.

## <a name="create-an-mvc-web-application"></a>Creare un'applicazione Web MVC

Aprire Visual Studio e creare un nuovo progetto di c# Web denominato "ContosoUniversity".

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo selezionare il **MVC** modello.

Se il **Host nel cloud** casella di controllo di **Microsoft Azure** sezione è selezionata, deselezionarla.

Fare clic su **modificare autenticazione**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

Nel **Modifica autenticazione** nella finestra di dialogo **Nessuna autenticazione**, quindi fare clic su **OK**. Per questa esercitazione si non richiedendo agli utenti di accedere o limitare l'accesso in base che è connesso.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Nella finestra di dialogo Nuovo progetto ASP.NET, fare clic su **OK** per creare il progetto.

## <a name="set-up-the-site-style"></a>Impostare lo stile del sito

Con alcune modifiche è possibile impostare il menu del sito, il layout e la home page.

Aprire *Views\Shared\\layout. cshtml*e apportare le modifiche seguenti:

- Modificare tutte le occorrenze di "My Application ASP.NET" e "Application name" in "Contoso University".
- Aggiungere le voci di menu per studenti, corsi, istruttori e reparti ed eliminare la voce di contatto.

Le modifiche sono evidenziate.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

In *Views\Home\Index.cshtml*, sostituire il contenuto del file con il codice seguente per sostituire il testo su ASP.NET e MVC con testo sull'applicazione:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Premere CTRL + F5 per eseguire il sito. Viene visualizzata la pagina home al menu principale.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Installare Entity Framework 6

Dal **strumenti** dal menu **Gestione pacchetti NuGet** e quindi fare clic su **Package Manager Console**.

Nel **Package Manager Console** finestra immettere il comando seguente:

`Install-Package EntityFramework`

![EF installato](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

L'immagine Mostra 6.0.0 in corso l'installazione, ma NuGet verrà installata la versione più recente di Entity Framework (escluse le versioni non definitive), che al momento dell'ultimo aggiornamento per l'esercitazione è 6.1.1.

Questo passaggio è uno dei pochi passaggi che per questa esercitazione sono eseguire manualmente, ma che potrebbe essere stata effettuata automaticamente dalla funzionalità di scaffolding di ASP.NET MVC. Si sta eseguendo li manualmente in modo da poter visualizzare i passaggi necessari per l'utilizzo di Entity Framework. Si utilizzerà lo scaffolding in un secondo momento per creare le visualizzazioni e controller MVC. In alternativa è possibile lasciare che lo scaffolding automaticamente installare il pacchetto NuGet di Entity Framework, creare la classe di contesto di database e creare la stringa di connessione. Quando si è pronti per eseguire l'operazione in questo modo, sarà sufficiente è ignorare tali passaggi e lo scaffolding controller MVC dopo aver creato le classi di entità.

## <a name="create-the-data-model"></a>Creare il modello di dati

A questo punto è possibile creare le classi delle entità per l'applicazione di Contoso University. Si inizierà con tre entità seguenti:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Esiste una relazione uno-a-molti tra le entità `Student` e `Enrollment` ed esiste una relazione uno-a-molti tra le entità `Course` e `Enrollment`. In altre parole, uno studente può iscriversi a un numero qualsiasi di corsi e un corso può avere un numero qualsiasi di studenti iscritti.

Nelle sezioni seguenti viene creata una classe per ognuna di queste entità.

> [!NOTE]
> Se si tenta di compilare il progetto per completare la creazione di tutte queste classi di entità, si ottengono gli errori del compilatore.


### <a name="the-student-entity"></a>L'entità di Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Nel *modelli* cartella, creare un file di classe denominato *Student.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

La proprietà `ID` diventa la colonna di chiave primaria della tabella di database che corrisponde a questa classe. Per impostazione predefinita, Entity Framework interpreta una proprietà denominata `ID` o *classname* `ID` come chiave primaria.

Il `Enrollments` proprietà è un *proprietà di navigazione*. Le proprietà di navigazione contengono altre entità correlate a questa entità. In questo caso, il `Enrollments` proprietà di un `Student` entità conterrà tutte le `Enrollment` entità correlate che `Student` entità. In altre parole, se un determinato `Student` riga del database dispone di due correlati `Enrollment` righe (le righe che contengono la chiave primaria di student con tale valore nel loro `StudentID` colonna chiave esterna), che `Student` dell'entità `Enrollments` proprietà di navigazione conterrà questi due `Enrollment` entità.

Le proprietà di navigazione sono in genere definite come `virtual` in modo che è possibile sfruttare alcune funzionalità di Entity Framework, ad esempio *caricamento lazy*. (Caricamento lazy verrà spiegato più avanti nel [lettura di dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie.)

Se una proprietà di navigazione può contenere più entità (come nel caso di relazioni molti-a-molti e uno-a-molti), il tipo della proprietà deve essere un elenco in cui le voci possono essere aggiunte, eliminate e aggiornate, come ad esempio `ICollection`.

### <a name="the-enrollment-entity"></a>L'entità di registrazione

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Nella cartella *Models* creare *Enrollment.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Il `EnrollmentID` proprietà sarà la chiave primaria, questa entità vengono utilizzati il *classname* `ID` schema anziché `ID` da se stesso come è illustrato nel `Student` entità. In genere si sceglie un criterio e lo si usa poi in tutto il modello di dati. In questo caso la variazione illustra che è possibile sia uno sia l'altro criterio. In un'esercitazione successiva, verrà visualizzato come l'utilizzo `ID` senza `classname` rende più semplice implementare l'ereditarietà nel modello di dati.

Il `Grade` proprietà è un [enum](https://msdn.microsoft.com/data/hh859576.aspx). Il punto interrogativo dopo il `Grade` dichiarazione del tipo di indica che il `Grade` proprietà [nullable](https://msdn.microsoft.com/library/2cf62fcy.aspx). Un livello null è diverso da un livello pari a zero, ovvero null indica che un livello non è noto o non è stato ancora assegnato.

La proprietà `StudentID` è una chiave esterna e la proprietà di navigazione corrispondente è `Student`. Un'entità `Enrollment` è associata a un'entità `Student`, pertanto la proprietà può contenere un'unica entità `Student`, a differenza della proprietà di navigazione `Student.Enrollments` vista in precedenza, che può contenere più entità `Enrollment`.

La proprietà `CourseID` è una chiave esterna e la proprietà di navigazione corrispondente è `Course`. Un'entità `Enrollment` è associata a un'entità `Course`.

Entity Framework interpreta una proprietà come proprietà di chiave esterna se il file è denominato *&lt;nome di proprietà di navigazione&gt;&lt;nome di proprietà di chiave primaria&gt;* (ad esempio, `StudentID`per il `Student` proprietà di navigazione perché il `Student` chiave primaria dell'entità è `ID`). Proprietà di chiave esterna può anche essere lo stesso nome semplicemente *&lt;nome di proprietà di chiave primaria&gt;* (ad esempio, `CourseID` poiché il `Course` chiave primaria dell'entità è `CourseID`).

### <a name="the-course-entity"></a>L'entità Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Nel *modelli* cartella, creare *Course.cs*, sostituendo il codice del modello con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La proprietà `Enrollments` rappresenta una proprietà di navigazione. È possibile correlare un'entità `Course` a un numero qualsiasi di entità `Enrollment`.

Ciò che diremo più sul [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) attributo in un'esercitazione successiva di questa serie. In pratica, questo attributo consente di immettere la chiave primaria per il corso invece di essere generata dal database.

## <a name="create-the-database-context"></a>Creare il contesto di database

La classe principale che coordina le funzionalità di Entity Framework per un modello di dati specificato è il *contesto di database* classe. Creazione di questa classe mediante la derivazione da di [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) classe. Nel codice vengono specificate le entità incluse nel modello di dati. È anche possibile personalizzare un determinato comportamento di Entity Framework. In questo progetto la classe è denominata `SchoolContext`.

Per creare una cartella nel progetto ContosoUniversity, fare clic sul progetto in **Esplora** e fare clic su **Aggiungi**, quindi fare clic su **nuova cartella**. Denominare la nuova cartella *DAL* (per il livello di accesso ai dati). In questa cartella creare un nuovo file di classe denominato *SchoolContext.cs*e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Specifica i set di entità

Questo codice crea un [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) proprietà per ogni set di entità. Nella terminologia di Entity Framework, un *set di entità* in genere corrisponde a una tabella di database e un *entità* corrisponde a una riga nella tabella.

> [!NOTE] 
> 
> È stato possibile stato omesso il `DbSet<Enrollment>` e `DbSet<Course>` istruzioni e funzionerà la stessa. Entity Framework le include in modo implicito perché l'entità `Student` fa riferimento all'entità `Enrollment` e l'entità `Enrollment` fa riferimento all'entità `Course`.


### <a name="specifying-the-connection-string"></a>Specifica la stringa di connessione

Il nome della stringa di connessione (che si aggiungerà il file Web. config in un secondo momento) viene passato al costruttore.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

È inoltre possibile passare la stringa di connessione stessa anziché il nome di quella memorizzata nel file Web. config. Per ulteriori informazioni sulle opzioni per specificare il database da utilizzare, vedere [Entity Framework - connessioni e i modelli](https://msdn.microsoft.com/data/jj592674).

Se non si specifica una stringa di connessione o il nome di uno in modo esplicito, Entity Framework presuppone che il nome di stringa di connessione è lo stesso nome della classe. Il nome di stringa di connessione predefinito in questo esempio sarebbe quindi `SchoolContext`, lo stesso come ciò che si sta specificando in modo esplicito.

### <a name="specifying-singular-table-names"></a>Specifica i nomi di tabella singolare

Il `modelBuilder.Conventions.Remove` istruzione il [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metodo impedisce che i nomi delle tabelle da pluralizzati. Se non si esegue questa operazione, le tabelle generate nel database verrebbero denominate `Students`, `Courses`, e `Enrollments`. Al contrario, i nomi di tabella andranno `Student`, `Course`, e `Enrollment`. Gli sviluppatori non hanno un'opinione unanime sul fatto che i nomi di tabella debbano essere pluralizzati oppure no. In questa esercitazione Usa la forma singolare, ma l'aspetto importante è che è possibile selezionare qualsiasi modulo che si preferisce includendo o l'omissione di questa riga di codice.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Impostare EF per inizializzare il database con dati di test

Entity Framework può automaticamente creare (o eliminare e ricreare) un database per l'utente quando viene eseguita l'applicazione. È possibile specificare che questo deve essere eseguito ogni volta che viene eseguita l'applicazione o solo quando il modello non è sincronizzato con il database esistente. È anche possibile scrivere un `Seed` metodo che Entity Framework chiama automaticamente dopo la creazione del database per popolarlo con i dati di test.

Il comportamento predefinito consiste nel creare un database solo se non esiste (e generare un'eccezione se il modello è stato modificato e il database esiste già). In questa sezione specificare che il database deve essere eliminato e ricreato ogni volta che viene modificato il modello. Eliminazione del database comporta la perdita di tutti i dati. Si tratta in genere OK durante lo sviluppo, poiché il `Seed` metodo viene eseguito quando il database viene ricreato e verrà ricreata in modo univoco i dati di test. Ma nell'ambiente di produzione in genere non si desidera perdere tutti i dati ogni volta che si desidera modificare lo schema del database. In un secondo momento, si noterà come gestire le modifiche al modello utilizzando migrazioni Code First per modificare lo schema del database anziché eliminare e ricreare il database.

Nella cartella di DAL, creare un nuovo file di classe denominato *SchoolInitializer.cs* e sostituire il codice del modello con il  
codice che fa sì che un database da creare quando necessario e carica i dati di test nel nuovo database.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

Il `Seed` metodo accetta l'oggetto di contesto di database come parametro di input e utilizza il codice nel metodo  
oggetto per aggiungere nuove entità nel database. Per ogni tipo di entità, il codice crea una raccolta di nuovi  
 le entità, li aggiunge alla appropriata `DbSet` proprietà e quindi Salva le modifiche al database. Non lo è  
necessario per chiamare il `SaveChanges` dopo ogni gruppo di entità, come avviene in questo caso, ma ciò che consente di (metodo)  
individuare l'origine di un problema se si verifica un'eccezione mentre il codice è la scrittura nel database.

Per indicare a Entity Framework di utilizzare l'inizializzatore di classe, aggiungere un elemento di `entityFramework` elemento nell'applicazione *Web. config* (il file di un file nella cartella radice del progetto), come illustrato nell'esempio seguente:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

Il `context type` specifica il nome della classe contesto completo e l'assembly è in, e `databaseinitializer type` specifica il nome completo della classe inizializzatore e l'assembly è in. (Quando EF per usare l'inizializzatore non si desidera, è possibile impostare un attributo nel `context` elemento: `disableDatabaseInitialization="true"`.) Per ulteriori informazioni, vedere [Entity Framework - impostazioni del File di configurazione](https://msdn.microsoft.com/data/jj556606).

Come alternativa all'impostazione l'inizializzatore *Web. config* file è farlo nel codice aggiungendo un `Database.SetInitializer` istruzione per il `Application_Start` metodo nel *Global.asax.cs* file. Per ulteriori informazioni, vedere [informazioni sui Database gli inizializzatori di Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

L'applicazione è ora impostato backup in modo che quando si accede al database per la prima volta in una determinata esecuzione del  
Confronta di Entity Framework applicazione, il database al modello (il `SchoolContext` e classi di entità). Se è presente una differenza, l'applicazione elimina e ricrea il database.

> [!NOTE]
> Quando si distribuisce un'applicazione a un server web di produzione, è necessario rimuovere o disabilitare il codice che elimina e ricrea il database. L'utente eseguirà che in un'esercitazione successiva di questa serie.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Impostare EF per utilizzare un database di SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) è una versione leggera di SQL Server Express Database Engine. È facile da installare e configurare, viene avviato su richiesta e viene eseguito in modalità utente. LocalDB viene eseguito in modalità speciali di esecuzione di SQL Server Express che consente di lavorare con i database come *con estensione mdf* file. È possibile inserire i file di database LocalDB nel *App\_dati* cartella del progetto web se si desidera essere in grado di copiare il database con il progetto. La funzionalità istanze utente in SQL Server Express consente inoltre di utilizzare *con estensione mdf* file, ma la funzionalità istanze utente è deprecato; pertanto, LocalDB è consigliato per l'utilizzo di *con estensione mdf* file. In Visual Studio 2012 e versioni successive, LocalDB è installato per impostazione predefinita con Visual Studio.

In genere SQL Server Express non viene usato per le applicazioni web di produzione. LocalDB in particolare non è consigliato per la produzione con un'applicazione web perché non è progettato per funzionare con IIS.

In questa esercitazione si utilizzerà LocalDB. Aprire l'applicazione *Web. config* file e aggiungere un `connectionStrings` elemento che precede il `appSettings` elemento, come illustrato nell'esempio seguente. (Assicurarsi di aggiornare il *Web. config* file nella cartella radice del progetto. È inoltre disponibile un *Web. config* file è il *viste* sottocartella che non è necessario aggiornare.)

Se si utilizza Visual Studio 2015, sostituire "v 11.0" nella stringa di connessione con "MSSQLLocalDB", perché è stato modificato il nome di istanza di SQL Server predefinito.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

La stringa di connessione aggiunti specifica che Entity Framework utilizzerà un database LocalDB denominato *ContosoUniversity1.mdf*. (Il database non esiste ancora; EF creerà.) Se si desidera che il database verrà creata la *App\_dati* cartella, è possibile aggiungere `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` alla stringa di connessione. Per ulteriori informazioni sulle stringhe di connessione, vedere [stringhe di connessione di SQL Server per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Non è necessario disporre di una stringa di connessione *Web. config* file. Se non si specifica una stringa di connessione, Entity Framework utilizzerà una basata sulla classe del contesto predefinito. Per ulteriori informazioni, vedere [Code First in un nuovo Database](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Creazione di un Controller di studenti e visualizzazioni

A questo punto si creerà una pagina web per visualizzare i dati e il processo di richiesta dei dati verrà automaticamente avviato  
la creazione del database. Innanzitutto creare un nuovo controller. Ma prima che a tale scopo, compilare il progetto per rendere disponibile lo scaffolding di controller MVC le classi di modello e il contesto.

1. Fare doppio clic sul **controller** cartella **Esplora**, selezionare **Aggiungi**e quindi fare clic su **nuovo elemento di scaffolding**.
2. Nel **aggiungere lo scaffolding** nella finestra di dialogo **Controller MVC 5 con visualizzazioni, mediante Entity Framework**.

     ![Aggiungere lo scaffolding](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. Nella finestra di dialogo Aggiungi Controller, effettuare le selezioni seguenti e quindi fare clic su **Aggiungi**:

   - Classe del modello: **studente (ContosoUniversity.Models)**. (Se questa opzione nell'elenco a discesa non viene visualizzato, compilare il progetto e riprovare.)
   - Classe del contesto dati: **SchoolContext (ContosoUniversity.DAL)**.
   - Nome del controller: **StudentController** (non StudentsController).
   - Lasciare i valori predefiniti per gli altri campi.

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     Quando fa clic su **Aggiungi**, il scaffolder crea un file StudentController.cs e un set di visualizzazioni (file con estensione cshtml) compatibili con il controller. In futuro, quando si creano progetti che usano Entity Framework è possibile usufruire di funzionalità aggiuntive del scaffolder: sufficiente creare la prima classe di modello, non creare una stringa di connessione e quindi la **Aggiungi Controller** casella specificare nuova classe di contesto. Verrà creato il scaffolder la `DbContext` classe e la stringa di connessione e il controller e visualizzazioni.
4. Visual Studio apre il *Controllers\StudentController.cs* file. Viene visualizzato di che una variabile di classe è stata creata che crea un'istanza di un oggetto di contesto di database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     Il `Index` metodo di azione Ottiene un elenco di studenti il *studenti* set tramite la lettura di entità di `Students` proprietà dell'istanza del contesto di database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     Il *Student\Index.cshtml* Visualizza l'elenco in una tabella:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Premere CTRL+F5 per eseguire il progetto. (Se si verifica un errore 'Impossibile creare la copia Shadow', chiudere il browser e riprovare.)

     Fare clic su di **studenti** scheda per visualizzare i dati di test che il `Seed` metodo inserito. A seconda di come narrow la finestra del browser è, verrà visualizzato il collegamento della scheda studente nella barra degli indirizzi superiore o è necessario fare clic su nell'angolo superiore destro per visualizzare il collegamento.

     ![Pulsante di menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Pagina di indice per studenti](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Visualizzare il database

Quando è stata eseguita la pagina di studenti e l'applicazione ha tentato di accedere al database, Entity Framework è emerso che si è verificato alcun database e quindi creato uno, quindi eseguire il metodo di inizializzazione per popolare il database con i dati.

È possibile utilizzare **Esplora Server** o **Esplora oggetti di SQL Server** (sillaba SSOX) per visualizzare il database in Visual Studio. Per questa esercitazione si utilizzerà **Esplora Server**. (Nelle edizioni Express di Visual Studio precedenti alla 2013, **Esplora Server** viene chiamato **Esplora Database**.)

1. Chiudere il browser.
2. In **Esplora Server**, espandere **connessioni dati**, espandere **il contesto dell'istituto di istruzione (ContosoUniversity)**, quindi espandere **tabelle** per visualizzare le tabelle nel nuovo database.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Fare doppio clic su di **Student** tabella e fare clic su **Mostra dati tabella** per visualizzare le colonne che sono state create e le righe che sono state inserite nella tabella.

    ![Tabella degli studenti](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Chiudi il **Esplora Server** connessione.

Il *ContosoUniversity1.mdf* e *ldf* file di database si trovano nel `C:\Users\<yourusername>` cartella.

Poiché si utilizza il `DropCreateDatabaseIfModelChanges` inizializzatore, ora è possibile apportare una modifica per il `Student` classe ed eseguire nuovamente l'applicazione, il database sarà automaticamente ricreato per trovare la corrispondenza della modifica. Ad esempio, se si aggiunge un `EmailAddress` proprietà per il `Student` classe, eseguire nuovamente la pagina di studenti e quindi controllare la tabella nuovamente, verrà visualizzato un nuovo `EmailAddress` colonna.

## <a name="conventions"></a>Convenzioni

La quantità di codice era necessario scrivere affinché Entity Framework sia in grado di creare un database completo per l'utente è minima a causa dell'utilizzo di *convenzioni*, o presupposti che rende Entity Framework. Alcuni di essi sono stati già o sono stati utilizzati senza la conoscenza della loro:

- Le forme pluralized di nomi di classe di entità vengono utilizzate come nomi di tabella.
- I nomi della proprietà di entità vengono usati come nomi di colonna.
- Le proprietà delle entità denominate `ID` o *classname* `ID` sono riconosciuti come proprietà della chiave primaria.
- Una proprietà viene interpretata come una proprietà di chiave esterna se il file è denominato *&lt;nome di proprietà di navigazione&gt;&lt;nome di proprietà di chiave primaria&gt;* (ad esempio, `StudentID` per il `Student` proprietà di navigazione perché il `Student` chiave primaria dell'entità è `ID`). Proprietà di chiave esterna può anche essere lo stesso nome semplicemente &lt;nome di proprietà di chiave primaria&gt; (ad esempio, `EnrollmentID` poiché il `Enrollment` chiave primaria dell'entità è `EnrollmentID`).

Si è visto che è possono eseguire l'override di convenzioni. Ad esempio, è specificato che i nomi di tabella non devono essere pluralizzati e verrà visualizzato in un secondo momento come contrassegnare in modo esplicito una proprietà come proprietà di chiave esterna. Si apprenderà ulteriori informazioni sulle convenzioni e come eseguire l'override nel [la creazione di un modello di dati più complesso](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie. Per ulteriori informazioni sulle convenzioni, vedere [convenzioni nel codice prima](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Riepilogo

Ora è stata creata una semplice applicazione che usa Entity Framework sia SQL Server Express LocalDB per archiviare e visualizzare i dati. Nell'esercitazione seguente si apprenderà come eseguire operazioni di base (creare, leggere, aggiornare ed eliminare) operazioni.

Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione ed è stato possibile apportare miglioramenti. È inoltre possibile richiedere nuovi argomenti in [Mostra Me come con codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Sono disponibili collegamenti ad altre risorse di Entity Framework in [accesso ai dati ASP.NET - risorse](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [avanti](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
