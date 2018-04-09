---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Creazione di un modello di dati di Entity Framework per un'applicazione ASP.NET MVC (1 di 10) | Documenti Microsoft
author: tdykstra
description: 'Una versione più recente di questa serie di esercitazioni è disponibile, per MVC 5, Entity Framework 6 e Visual Studio 2013. La Germania esempio Contoso University: applicazione web...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a963f26b408f2a54bd9cd3e852bc1e368f86c41f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Creazione di un modello di dati di Entity Framework per un'applicazione ASP.NET MVC (1 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > Oggetto [versione più recente di questa serie di esercitazioni](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) è disponibile, per MVC 5, Entity Framework 6 e Visual Studio 2013.
> 
> 
> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 utilizzo di Entity Framework 5 e Visual Studio 2012. L'applicazione di esempio è un sito Web per una fittizia Contoso University. Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati. Questa serie di esercitazioni viene illustrato come compilare l'applicazione di esempio Contoso University. È possibile [il download dell'applicazione completata](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> È possibile utilizzare i dati in Entity Framework in tre modi: *Database First*, *Model First*, e *Code First*. In questa esercitazione è per Code First. Per informazioni sulle differenze tra questi flussi di lavoro e indicazioni su come scegliere quello più adatto per lo scenario, vedere [flussi di lavoro di Entity Framework sviluppo](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> L'applicazione di esempio è basato su [ASP.NET MVC](../../../index.md). Se si preferisce usare il modello Web Form ASP.NET, vedere il [associazione di modelli e Web Form](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) serie di esercitazioni e [mappa del contenuto ASP.NET dati accesso](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> | **Nell'esercitazione** | **Funziona anche con** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio Express 2012 per Web. Questo viene installato automaticamente da Windows Azure SDK, se si dispone già di Visual Studio 2012 o Visual Studio 2012 Express per il Web. Visual Studio 2013 dovrebbero funzionare, ma l'esercitazione non è stata testata con esso e alcune selezioni di menu e finestre di dialogo sono diverse. Il [Visual Studio 2013 versione di Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) necessarie per la distribuzione di Windows Azure. |
> | .NET 4.5 | La maggior parte delle funzionalità illustrate funziona in .NET 4, ma alcune non. Ad esempio, il supporto di enum in EF richiede .NET 4.5. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Per ignorare i passaggi di distribuzione di Windows Azure, è necessario il SDK. Quando viene rilasciata una nuova versione del SDK, il collegamento verrà installata la versione più recente. In tal caso, potrebbe essere necessario adattare alcuni le istruzioni per una nuova interfaccia utente e funzionalità. |
> 
> ## <a name="questions"></a>Domande
> 
> Nel caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework e LINQ to forum entità](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Ringraziamenti
> 
> Vedere l'esercitazione della serie per ultimo [una nota relativa VB e riconoscimenti](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Versione originale dell'esercitazione
> 
> La versione originale dell'esercitazione è disponibile nel [Entity Framework 4.1 / MVC 3 e-book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>L'applicazione Web di Contoso University

L'applicazione che sarà compilata in queste esercitazioni è un semplice sito Web universitario.

Gli utenti possono visualizzare e aggiornare le informazioni che riguardano studenti, corsi e insegnanti. Di seguito sono disponibili alcune schermate che saranno create.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Lo stile dell'interfaccia utente del sito è simile a quanto è stato generato tramite i modelli predefiniti. L'esercitazione si concentra pertanto soprattutto sull'uso di Entity Framework.

## <a name="prerequisites"></a>Prerequisiti

Le direzioni e catture di schermata in questa esercitazione si presuppongono che si usi [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) o [Visual Studio Express 2012 per Web](https://go.microsoft.com/fwlink/?LinkID=275131), con l'aggiornamento e Azure SDK per .NET installato a partire da luglio, più recenti 2013. È possibile ottenere mediante il collegamento seguente:

[Azure SDK per .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Se si è installato Visual Studio, il collegamento riportato sopra installerà i componenti mancanti. Se non si dispone di Visual Studio, il collegamento verrà installato Visual Studio Express 2012 per Web. È possibile utilizzare Visual Studio 2013, ma alcune delle procedure necessarie e schermate sarà diverso.

## <a name="create-an-mvc-web-application"></a>Creare un'applicazione Web MVC

Aprire Visual Studio e creare un nuovo progetto c# denominato "ContosoUniversity" utilizzando il **applicazione Web ASP.NET MVC 4** modello. Verificare che la destinazione è **.NET Framework 4.5** (userai [ `enum` proprietà](https://msdn.microsoft.com/data/hh859576.aspx), che richiede .NET 4.5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo selezionare il **applicazione Internet** modello.

Lasciare il **Razor** visualizzare motore selezionato e lascia il **creare un progetto di unit test** casella di controllo deselezionata.

Fare clic su **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Impostare lo stile del sito

Con alcune modifiche è possibile impostare il menu del sito, il layout e la home page.

Aprire *Views\Shared\\layout. cshtml*e sostituire il contenuto del file con il codice seguente. Le modifiche sono evidenziate.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Questo codice determina le modifiche seguenti:

- Sostituisce le istanze di modello di "My ASP.NET MVC Application" e "inserire qui il logo" con "Contoso University".
- Aggiunge diversi collegamenti di azione che verranno utilizzati più avanti nell'esercitazione.

In *Views\Home\Index.cshtml*, sostituire il contenuto del file con il codice seguente per eliminare i paragrafi del modello su ASP.NET e MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

In *controllers\homecontroller.cs.*, modificare il valore di `ViewBag.Message` nel `Index` metodo di azione per "Benvenuti Contoso University.", come illustrato nell'esempio seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Premere CTRL + F5 per eseguire il sito. Viene visualizzata la pagina home al menu principale.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Creare il modello di dati

A questo punto è possibile creare le classi delle entità per l'applicazione di Contoso University. Si inizierà con tre entità seguenti:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Esiste una relazione uno-a-molti tra le entità `Student` e `Enrollment` ed esiste una relazione uno-a-molti tra le entità `Course` e `Enrollment`. In altre parole, uno studente può iscriversi a un numero qualsiasi di corsi e un corso può avere un numero qualsiasi di studenti iscritti.

Nelle sezioni seguenti viene creata una classe per ognuna di queste entità.

> [!NOTE]
> Se si tenta di compilare il progetto per completare la creazione di tutte queste classi di entità, si ottengono gli errori del compilatore.


### <a name="the-student-entity"></a>L'entità di Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Nel *modelli* cartella, creare *Student.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

La proprietà `StudentID` diventa la colonna di chiave primaria della tabella di database che corrisponde a questa classe. Per impostazione predefinita, Entity Framework interpreta una proprietà denominata `ID` o *classname* `ID` come chiave primaria.

Il `Enrollments` proprietà è un *proprietà di navigazione*. Le proprietà di navigazione contengono altre entità correlate a questa entità. In questo caso, il `Enrollments` proprietà di un `Student` entità conterrà tutte le `Enrollment` entità correlate che `Student` entità. In altre parole, se un determinato `Student` riga del database dispone di due correlati `Enrollment` righe (le righe che contengono la chiave primaria di student con tale valore nel loro `StudentID` colonna chiave esterna), che `Student` dell'entità `Enrollments` proprietà di navigazione conterrà questi due `Enrollment` entità.

Le proprietà di navigazione sono in genere definite come `virtual` in modo che è possibile sfruttare alcune funzionalità di Entity Framework, ad esempio *caricamento lazy*. (Caricamento lazy verrà spiegato più avanti nel [lettura di dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie.

Se una proprietà di navigazione può contenere più entità (come nel caso di relazioni molti-a-molti e uno-a-molti), il tipo della proprietà deve essere un elenco in cui le voci possono essere aggiunte, eliminate e aggiornate, come ad esempio `ICollection`.

### <a name="the-enrollment-entity"></a>L'entità di registrazione

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Nella cartella *Models* creare *Enrollment.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La proprietà di livello è un [enum](https://msdn.microsoft.com/data/hh859576.aspx). Il punto interrogativo dopo il `Grade` dichiarazione del tipo di indica che il `Grade` proprietà [nullable](https://msdn.microsoft.com/library/2cf62fcy.aspx). Un livello null è diverso da un livello pari a zero, ovvero null indica che un livello non è noto o non è stato ancora assegnato.

La proprietà `StudentID` è una chiave esterna e la proprietà di navigazione corrispondente è `Student`. Un'entità `Enrollment` è associata a un'entità `Student`, pertanto la proprietà può contenere un'unica entità `Student`, a differenza della proprietà di navigazione `Student.Enrollments` vista in precedenza, che può contenere più entità `Enrollment`.

La proprietà `CourseID` è una chiave esterna e la proprietà di navigazione corrispondente è `Course`. Un'entità `Enrollment` è associata a un'entità `Course`.

### <a name="the-course-entity"></a>L'entità Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Nel *modelli* cartella, creare *Course.cs*, sostituendo il codice esistente con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

La proprietà `Enrollments` rappresenta una proprietà di navigazione. È possibile correlare un'entità `Course` a un numero qualsiasi di entità `Enrollment`.

Ciò che diremo più sul [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). None)] attributo nella prossima esercitazione. In pratica, questo attributo consente di immettere la chiave primaria per il corso invece di essere generata dal database.

## <a name="create-the-database-context"></a>Creare il contesto di database

La classe principale che coordina le funzionalità di Entity Framework per un modello di dati specificato è il *contesto di database* classe. Creazione di questa classe mediante la derivazione da di [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) classe. Nel codice vengono specificate le entità incluse nel modello di dati. È anche possibile personalizzare un determinato comportamento di Entity Framework. In questo progetto la classe è denominata `SchoolContext`.

Creare una cartella denominata *DAL* (per il livello di accesso ai dati). In questa cartella creare un nuovo file di classe denominato *SchoolContext.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Questo codice crea un [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) proprietà per ogni set di entità. Nella terminologia di Entity Framework, un *set di entità* in genere corrisponde a una tabella di database e un *entità* corrisponde a una riga nella tabella.

Il `modelBuilder.Conventions.Remove` istruzione il [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metodo impedisce che i nomi delle tabelle da pluralizzati. Se non si esegue questa operazione, le tabelle generate verrebbero denominate `Students`, `Courses`, e `Enrollments`. Al contrario, i nomi di tabella andranno `Student`, `Course`, e `Enrollment`. Gli sviluppatori non hanno un'opinione unanime sul fatto che i nomi di tabella debbano essere pluralizzati oppure no. In questa esercitazione Usa la forma singolare, ma l'aspetto importante è che è possibile selezionare qualsiasi modulo che si preferisce includendo o l'omissione di questa riga di codice.

## <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) è una versione leggera di SQL Server Express motore di Database che viene avviato su richiesta e viene eseguito in modalità utente. LocalDB viene eseguito in modalità speciali di esecuzione di SQL Server Express che consente di lavorare con i database come *con estensione mdf* file. In genere, vengono archiviati i database LocalDB di *App\_dati* cartella del progetto web. La funzionalità istanze utente in SQL Server Express consente inoltre di utilizzare *con estensione mdf* file, ma la funzionalità istanze utente è deprecato; pertanto, LocalDB è consigliato per l'utilizzo di *con estensione mdf* file.

In genere SQL Server Express non viene usato per le applicazioni web di produzione. LocalDB in particolare non è consigliato per la produzione con un'applicazione web perché non è progettato per funzionare con IIS.

In Visual Studio 2012 e versioni successive, LocalDB è installato per impostazione predefinita con Visual Studio. In Visual Studio 2010 e versioni precedenti, SQL Server Express (senza LocalDB) viene installato per impostazione predefinita con Visual Studio. è necessario installarla manualmente se si utilizza Visual Studio 2010.

In questa esercitazione si utilizzerà del database locale in modo che il database può essere archiviato nel *App\_dati* cartella come un *con estensione mdf* file. Aprire la directory radice *Web. config* file e aggiungere una nuova stringa di connessione per il `connectionStrings` insieme, come illustrato nell'esempio seguente. (Assicurarsi di aggiornare il *Web. config* file nella cartella radice del progetto. È inoltre disponibile un *Web. config* file è il *viste* sottocartella che non è necessario aggiornare.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Per impostazione predefinita, Entity Framework Cerca una stringa di connessione denominata come il `DbContext` classe (`SchoolContext` per questo progetto). È stata aggiunta la stringa di connessione specifica un database LocalDB denominato *ContosoUniversity.mdf* nella *App\_dati* cartella. Per ulteriori informazioni, vedere [stringhe di connessione di SQL Server per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

È in realtà non necessario specificare la stringa di connessione. Se non si specifica una stringa di connessione, Entity Framework verrà creato uno automaticamente. Tuttavia, il database potrebbe non essere nel *App\_dati* cartella dell'app. Per informazioni su dove verrà creato il database, vedere [Code First in un nuovo Database](https://msdn.microsoft.com/data/jj193542).

Il `connectionStrings` raccolta dispone di una stringa di connessione denominata `DefaultConnection` che viene usato per il database delle appartenenze. In questa esercitazione si non utilizzando il database delle appartenenze. L'unica differenza tra due stringhe di connessione è il nome del database e il valore dell'attributo name.

## <a name="set-up-and-execute-a-code-first-migration"></a>Impostare ed eseguire una migrazione prima del codice

Al primo avvio di sviluppare un'applicazione, il modello di dati delle modifiche di frequente e ogni volta che le modifiche del modello che viene sincronizzato con il database. È possibile configurare per automaticamente, eliminare e ricreare il database ogni volta che si modifica il modello di dati Entity Framework. Questo non è un problema nelle prime fasi di sviluppo perché i dati di test viene facilmente ricreati, ma dopo aver distribuito nell'ambiente di produzione in genere si desidera aggiornare lo schema del database senza eliminare il database. La funzionalità di migrazioni consente Code First aggiornare il database senza eliminare e crearne uno nuovo. Nelle prime fasi del ciclo di sviluppo di un nuovo progetto si potrebbe voler usare [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) per eliminare, ricreare e reseeding del database ogni volta che le modifiche del modello. Uno prepararsi alla distribuzione dell'applicazione, è possibile convertire l'approccio di migrazioni. Per questa esercitazione si utilizzerà solo le migrazioni. Per ulteriori informazioni, vedere [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) e [migrazioni Screencast serie](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Abilitare le migrazioni Code First

1. Dal **strumenti** menu, fare clic su **Gestione pacchetti libreria** e quindi **Package Manager Console**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. Nel `PM>` prompt dei comandi immettere il comando seguente:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![comando Enable-migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Questo comando crea un *migrazioni* cartella nel progetto ContosoUniversity e inserisce in questa cartella un *Configuration.cs* file che è possibile modificare per configurare le migrazioni.

    ![Cartella Migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    Il `Configuration` classe include un `Seed` metodo che viene chiamato quando viene creato il database e ogni volta che viene aggiornata dopo la modifica del modello di un tipo di dati.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Lo scopo di questo `Seed` metodo consiste nella possibilità di inserire i dati di test nel database dopo il primo codice crea o Aggiorna.

### <a name="set-up-the-seed-method"></a>Impostare il metodo di inizializzazione

Il [valore di inizializzazione](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodo viene eseguito quando si crea il database migrazioni Code First e ogni volta che aggiorna il database per la migrazione di più recente. Lo scopo del metodo di inizializzazione è che consente di inserire dati in tabelle prima che l'applicazione accede al database per la prima volta.

Nelle versioni precedenti di Code First, prima del rilascio, migrazioni è comune per `Seed` metodi per inserire i dati di test, perché con ogni modifica del modello durante lo sviluppo il database deve essere completamente eliminato e ricreato da zero. Con le migrazioni Code First, test, i dati vengono mantenuti dopo le modifiche del database, pertanto l'inclusione di dati di test nel [valore di inizializzazione](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodo non è in genere necessario. Infatti, non si desidera il `Seed` per inserire i dati di test se sarà possibile usare le migrazioni per distribuire il database nell'ambiente di produzione, perché il `Seed` metodo viene eseguito nell'ambiente di produzione. In tal caso è necessario il `Seed` per inserire nel database solo i dati che si desidera inserire nell'ambiente di produzione. Ad esempio, è il database per includere i nomi di reparto effettivo nel `Department` tabella quando l'applicazione diventa disponibile nell'ambiente di produzione.

Per questa esercitazione, sarà possibile usare le migrazioni per la distribuzione, ma il `Seed` metodo inserirà comunque i dati di test per renderlo più semplice vedere come utilizzare la funzionalità dell'applicazione senza dover inserire manualmente una grande quantità di dati.

1. Sostituire il contenuto del *Configuration.cs* file con il codice seguente, che verrà caricati i dati di test nel nuovo database. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    Il [valore di inizializzazione](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodo accetta l'oggetto di contesto di database come parametro di input e il codice nel metodo utilizza tale oggetto per aggiungere nuove entità nel database. Per ogni tipo di entità, il codice crea una raccolta di nuove entità, li aggiunge alla appropriata [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) proprietà e quindi Salva le modifiche al database. Non è necessario chiamare il [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metodo dopo ogni gruppo di entità, come avviene, ma questa operazione consente di individuare l'origine di un problema se si verifica un'eccezione mentre il codice è la scrittura nel database.

    Alcune istruzioni che inseriscono dati utilizzare il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo per eseguire un'operazione "upsert". Poiché il `Seed` metodo viene eseguito con ogni tipo di migrazione, è possibile inserire solo dati, in quanto le righe che si sta tentando di aggiungere già sarà disponibile dopo la migrazione prima che crea il database. L'operazione "upsert" impedisce gli errori che accadrebbe se si tenta di inserire una riga già esistente, ma ***esegue l'override*** qualsiasi modifica ai dati apportate durante il test dell'applicazione. I dati di test in alcune tabelle è possibile evitare di raggiungere tale obiettivo: in alcuni casi quando si modificano i dati durante il test le proprie modifiche rimangano dopo gli aggiornamenti del database. In questo caso si desidera eseguire un'operazione di inserimento condizionale: inserire una riga solo se non esiste già. Il metodo di inizializzazione utilizza entrambi gli approcci.

    Il primo parametro passato per il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo consente di specificare la proprietà da utilizzare per verificare se esiste già una riga. Per i dati di test dello studente che si desidera fornire, il `LastName` proprietà può essere utilizzata per questo scopo poiché ogni cognome nell'elenco è univoco:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Questo codice presuppone che i cognomi siano univoci. Se si aggiunge manualmente uno studente con un nome duplicato ultimo, si otterrà la seguente eccezione alla successiva che si eseguire la migrazione.

    La sequenza contiene più di un elemento

    Per ulteriori informazioni sul `AddOrUpdate` metodo, vedere [prestare attenzione con metodo AddOrUpdate di EF 4.3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sul blog di Julie Lerman.

    Il codice che aggiunge `Enrollment` entità non usa il `AddOrUpdate` metodo. Controlla se esiste già un'entità e inserisce l'entità se non esiste. Questo approccio consente di mantenere le modifiche apportate a un livello di registrazione quando eseguire le migrazioni. Il codice scorre ogni membro del `Enrollment` [elenco](https://msdn.microsoft.com/library/6sh2ey19.aspx) e se la registrazione non viene trovata nel database, viene aggiunta la registrazione al database. La prima volta che si aggiorna il database, il database sarà vuoto, in modo che aggiungerà ogni registrazione.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Per informazioni su come eseguire il debug di `Seed` metodo e la modalità di gestione di dati ridondanti, ad esempio due studenti denominati "Alexander Carson", vedere [Seeding e database di debug di Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) sul blog di Rick Anderson.
2. Compilare il progetto.

### <a name="create-and-execute-the-first-migration"></a>Creare ed eseguire la migrazione prima

1. Nella finestra della Console di gestione pacchetti, immettere i comandi seguenti: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    Il `add-migration` comando aggiunge la cartella Migrations un *[DateStamp]\_InitialCreate.cs* file che contiene codice che crea il database. Il primo parametro (`InitialCreate)` viene utilizzato per il file di nome e può essere qualsiasi, si sceglie in genere una parola o frase che riepiloga le quali viene eseguita la migrazione. Ad esempio, è possibile denominare una migrazione successiva &quot;AddDepartmentTable&quot;.

    ![Cartella Migrations iniziale migrazione](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    Il `Up` metodo il `InitialCreate` classe crea le tabelle di database che corrispondono ai set di entità del modello di dati, e `Down` metodo li elimina. Le migrazioni chiamano il metodo `Up` per implementare le modifiche al modello di dati per una migrazione. Quando si immette un comando per annullare l'aggiornamento, le migrazioni chiamano il metodo `Down`. Nel codice seguente viene illustrato il contenuto del `InitialCreate` file:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    Il `update-database` comando viene eseguito il `Up` metodo per creare il database e quindi si esegue il `Seed` metodo per popolare il database.

È stato creato un database di SQL Server per il modello di dati. Il nome del database è *ContosoUniversity*e *con estensione mdf* è file del progetto *App\_dati* cartella perché è specificato nella stringa di connessione.

È possibile utilizzare **Esplora Server** o **Esplora oggetti di SQL Server** (sillaba SSOX) per visualizzare il database in Visual Studio. Per questa esercitazione si utilizzerà **Esplora Server**. In Visual Studio Express 2012 per Web, **Esplora Server** viene chiamato **Esplora Database**.

1. Dal **vista** menu, fare clic su **Esplora Server**.
2. Fare clic su di **Aggiungi connessione** icona.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Se viene richiesto con la **Scegli origine dati** finestra di dialogo, fare clic su **Microsoft SQL Server**, quindi fare clic su **continua**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. Nel **Aggiungi connessione** finestra di dialogo immettere **(localdb) \v11.0** per il **nome Server**. In **selezionare o immettere un nome di database**selezionare **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Fare clic su **OK**.
6. Espandere **SchoolContext** e quindi espandere **tabelle**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Fare doppio clic su di **Student** tabella e fare clic su **Mostra dati tabella** per visualizzare le colonne che sono state create e le righe che sono state inserite nella tabella.

    ![Tabella degli studenti](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Creazione di un Controller di studenti e visualizzazioni

Il passaggio successivo consiste nel creare un ASP.NET MVC controller e visualizzazioni nell'applicazione che può funzionare con una di queste tabelle.

1. Per creare un `Student` controller, fare doppio clic sul **controller** cartella **Esplora**, selezionare **Aggiungi**e quindi fare clic su **Controller** . Nel **Aggiungi Controller** finestra di dialogo casella, effettuare le selezioni seguenti e quindi fare clic su **Aggiungi**: 

   - Nome del controller: **StudentController**.
   - Modello: **controller MVC con azioni di lettura/scrittura e viste, mediante Entity Framework**.
   - Classe del modello: **studente (ContosoUniversity.Models)**. (Se questa opzione nell'elenco a discesa non viene visualizzato, compilare il progetto e riprovare.)
   - Classe del contesto dati: **SchoolContext (ContosoUniversity.Models)**.
   - Visualizzazioni: **Razor (CSHTML)**. (Predefinito).

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio apre il *Controllers\StudentController.cs* file. Viene visualizzato di che una variabile di classe è stata creata che crea un'istanza di un oggetto di contesto di database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     Il `Index` metodo di azione Ottiene un elenco di studenti il *studenti* set tramite la lettura di entità di `Students` proprietà dell'istanza del contesto di database:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     Il *Student\Index.cshtml* Visualizza l'elenco in una tabella:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Premere CTRL+F5 per eseguire il progetto.

     Fare clic su di **studenti** scheda per visualizzare i dati di test che il `Seed` metodo inserito.

     ![Pagina di indice per studenti](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Convenzioni

La quantità di codice era necessario scrivere affinché Entity Framework sia in grado di creare un database completo per l'utente è minima a causa dell'utilizzo di *convenzioni*, o presupposti che rende Entity Framework. Alcuni di essi sono stati già:

- Le forme pluralized di nomi di classe di entità vengono utilizzate come nomi di tabella.
- I nomi della proprietà di entità vengono usati come nomi di colonna.
- Le proprietà delle entità denominate `ID` o *classname* `ID` sono riconosciuti come proprietà della chiave primaria.

Si è visto che è possono eseguire l'override di convenzioni (ad esempio, è specificato che i nomi di tabella non devono essere pluralizzati), e si apprenderà ulteriori informazioni sulle convenzioni e come eseguire l'override nel [la creazione di un modello di dati più complesso](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie. Per ulteriori informazioni, vedere [convenzioni nel codice prima](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Riepilogo

Ora è stata creata una semplice applicazione che usa Entity Framework sia SQL Server Express per archiviare e visualizzare i dati. Nell'esercitazione seguente si apprenderà come eseguire operazioni di base (creare, leggere, aggiornare ed eliminare) operazioni. È possibile lasciare commenti e suggerimenti nella parte inferiore della pagina. Saremmo lieti di sapere come si ritengono questa parte dell'esercitazione e come è stato possibile apportare miglioramenti.

Collegamenti ad altre risorse di Entity Framework, vedere il [mappa del contenuto ASP.NET dati accesso](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [avanti](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
