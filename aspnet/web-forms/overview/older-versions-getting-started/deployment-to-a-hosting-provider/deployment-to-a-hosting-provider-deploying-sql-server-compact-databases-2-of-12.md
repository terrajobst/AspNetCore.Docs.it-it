---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione di database SQL Server Compact - 2 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 378bcc038335ee852cd1a6c6e545eb72c6e0c78b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829524"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione di database SQL Server Compact - 2 pari a 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e Mostra come distribuire in App Web di servizio App di Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come configurare il motore di database per la distribuzione e due database di SQL Server Compact.

Per l'accesso al database, l'applicazione Contoso University richiede il seguente software che deve essere distribuito con l'applicazione perché non è incluso in .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (il motore di database).
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (che consentono al sistema di appartenenza ASP.NET di usare SQL Server Compact)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First con migrazioni).

La struttura del database e alcuni (non tutti) dei dati in due dell'applicazione devono essere distribuiti anche i database. In genere, quando si sviluppa un'applicazione, immettere i dati di test in un database che non si desidera distribuire in un sito live. Tuttavia, è anche possibile immettere alcuni dati di produzione che si desidera distribuire. In questa esercitazione si configurerà il progetto di Contoso University specificano in modo che durante la distribuzione sono inclusi i requisiti software e i dati corretti.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact e SQL Server Express

L'applicazione di esempio Usa SQL Server Compact 4.0. Questo motore di database è un'opzione relativamente nuova per i siti Web; le versioni precedenti di SQL Server Compact non funzionano in un ambiente di hosting web. SQL Server Compact offre alcuni vantaggi rispetto allo scenario più comune di sviluppo con SQL Server Express e la distribuzione completa di SQL Server. A seconda del provider di hosting che scelto, SQL Server Compact potrebbero essere più conveniente da distribuire, perché alcuni provider applicano un addebito extra per supportare un database di SQL Server completo. Non sono previsti addebiti aggiuntivi per SQL Server Compact poiché è possibile distribuire il motore di database come parte dell'applicazione web.

Tuttavia, inoltre è necessario tenere presenti alcune limitazioni. SQL Server Compact non supporta stored procedure, trigger, viste o replica. (Per un elenco completo delle funzionalità di SQL Server che non sono supportati da SQL Server Compact, vedere [differenze tra SQL Server Compact e SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Inoltre, alcuni degli strumenti che è possibile usare per gestire gli schemi e i dati in SQL Server Express e database di SQL Server non funzionano con SQL Server Compact. È ad esempio, non è possibile usare SQL Server Management Studio o SQL Server Data Tools in Visual Studio con i database di SQL Server Compact. Sono disponibili altre opzioni per l'uso con i database di SQL Server Compact:

- È possibile usare Esplora Server in Visual Studio, che offre funzionalità di modifica dei database limitato per SQL Server Compact.
- È possibile usare la funzionalità di modifica dei database di [WebMatrix](https://www.microsoft.com/web/webmatrix/), che ha maggiori funzionalità rispetto a Esplora Server.
- È possibile usare relativamente complete di terze parti o strumenti open source, ad esempio la [strumenti di SQL Server Compact](https://github.com/ErikEJ/SqlCeToolbox) e [i dati di SQL Compact e schema script utilità](https://github.com/ErikEJ/SqlCeToolbox).
- È possibile scrivere ed eseguire i propri script (DDL) DDL per la modifica dello schema del database.

È possibile iniziare con SQL Server Compact e quindi eseguire l'aggiornamento in un secondo momento man mano che cambiano le proprie esigenze. Le esercitazioni successive di questa serie mostrano come eseguire la migrazione da SQL Server Compact a SQL Server Express e SQL Server. Tuttavia, se si sta creando una nuova applicazione e si prevede di aver bisogno di SQL Server in un prossimo futuro, è probabilmente preferibile iniziare con SQL Server o SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Configurazione del motore di Database compatto di SQL Server per la distribuzione

Il software necessario per l'accesso ai dati nell'applicazione di Contoso University è stato aggiunto installando i pacchetti NuGet seguenti:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (provider universal ASP.NET)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

I collegamenti puntino alle versioni correnti di questi pacchetti, che potrebbero essere più recenti rispetto a quella installata nel progetto di avvio che è stato scaricato per questa esercitazione. Per la distribuzione in un provider di hosting, assicurarsi di usare Entity Framework 5.0 o versioni successive. Le versioni precedenti di migrazioni Code First richiedono attendibilità totale e in molti provider di hosting dell'applicazione verrà eseguito in Medium Trust. Per altre informazioni sull'attendibilità media, vedere la [distribuzione in IIS come ambiente di Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) esercitazione.

Installazione dei pacchetti NuGet in genere si occupa di tutto ciò che occorre per distribuire il software con l'applicazione. In alcuni casi, ciò comporta attività, ad esempio modificando il file Web. config e aggiungendo gli script di PowerShell che eseguono ogni volta che si compila la soluzione. **Se si desidera aggiungere il supporto per queste funzionalità (ad esempio SQL Server Compact ed Entity Framework) senza l'utilizzo di NuGet, assicurarsi che si conosce risultato installazione dei pacchetti NuGet in modo da poter eseguire la stessa operazione manualmente.**

Vi è un'eccezione in cui NuGet non prende cura di tutto ciò che è necessario eseguire per garantire una corretta distribuzione. Il pacchetto SqlServerCompact NuGet aggiunge uno script di post-compilazione per il progetto in cui vengono copiati gli assembly nativi *x86* e *amd64* le sottocartelle in un progetto *bin* cartella, ma lo script non include le cartelle nel progetto. Di conseguenza, distribuzione Web non verrà copiata li al sito web di destinazione a meno che non si manualmente includerli nel progetto. (Questo comportamento è dovuta la configurazione della distribuzione predefinita, un'altra opzione, non verranno usati in queste esercitazioni, consiste nel modificare l'impostazione che controlla questo comportamento. È l'impostazione che è possibile modificare **solo i file necessari per eseguire l'applicazione** sotto **elementi da distribuire** sul **pubblicazione/creazione pacchetto Web** scheda della finestra di **progetto Proprietà** finestra. Questa impostazione non è in genere consigliabile modificare perché ciò può comportare la distribuzione di molti altri file nell'ambiente di produzione quelle necessarie sono. Per altre informazioni sulle alternative, vedere la [configurare le proprietà del progetto](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) esercitazione.)

Compilare il progetto, quindi nel **Esplora soluzioni** fare clic su **Mostra tutti i file** se non si è già stato fatto. Potrebbe anche essere necessario fare clic su **Aggiorna**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Espandere la **bin** cartella per visualizzare i **amd64** e **x86** cartelle e quindi selezionare le cartelle, pulsante destro del mouse e selezionare **Includi nel progetto**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Cartella icone aggiornate per mostrare che la cartella è stato incluso nel progetto.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Configurazione di migrazioni Code First per la distribuzione di applicazioni Database

Quando si distribuisce un database dell'applicazione, in genere non è sufficiente vengono distribuiti nel database di sviluppo con tutti i dati in esso nell'ambiente di produzione, perché la maggior parte dei dati in esso è probabilmente presente solo per scopi di test. Ad esempio, i nomi degli studenti in un database di test sono fittizi. D'altra parte, è spesso non è possibile distribuire solo la struttura del database senza dati in esso affatto. Alcuni dei dati nel database di test potrebbe essere dati reali e deve essere presente quando gli utenti iniziano a usare l'applicazione. Ad esempio, il database potrebbe creare una tabella che contiene i valori validi di livello o i nomi dei reparti reali.

Per simulare questo scenario comune, si configurerà un metodo di inizializzazione migrazioni prima di codice che inserisce nel database solo i dati che si desidera essere presenti nell'ambiente di produzione. Questo metodo di inizializzazione non inserisce i dati di test perché verrà eseguito nell'ambiente di produzione dopo Code First consente di creare il database nell'ambiente di produzione.

Nelle versioni precedenti di Code First prima che le migrazioni è stato rilasciato, era comune per i metodi di inizializzazione per inserire i dati di test, inoltre, poiché con ogni modifica del modello durante lo sviluppo di database doveva essere completamente eliminato e ricreato da zero. Con migrazioni Code First, i dati di test venga mantenuti dopo modifiche del database, quindi inclusi i dati di test nel metodo di inizializzazione non è necessario. Il progetto che è stato scaricato Usa il metodo di pre-migrazioni di inclusione di tutti i dati nel metodo di inizializzazione di una classe di inizializzatore. In questa esercitazione si disabilita la classe dell'inizializzatore e abilitare migrazioni. Quindi si aggiornerà il metodo di inizializzazione della classe di configurazione di migrazioni in modo da inserire solo i dati desiderati da inserire nell'ambiente di produzione.

Il diagramma seguente illustra lo schema del database dell'applicazione:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Per queste esercitazioni, si partirà dal presupposto che il `Student` e `Enrollment` tabelle devono essere vuote quando il sito prima di tutto viene distribuito. Le altre tabelle contengono dati che deve essere precaricata quando l'applicazione viene resa disponibile.

Poiché si userà migrazioni Code First, non è più necessario usare il **DropCreateDatabaseIfModelChanges** inizializzatore Code First. Il codice per questo inizializzatore è nel file SchoolInitializer.cs nel progetto ContosoUniversity.DAL. Un'impostazione di **appSettings** elemento del file Web. config fa sì che questo inizializzatore per l'esecuzione ogni volta che l'applicazione tenta di accedere al database per la prima volta:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Aprire il file Web. config dell'applicazione e rimuovere l'elemento che specifica la classe inizializzatore Code First dall'elemento appSettings. Elemento appSettings ora simile alla seguente:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Un altro modo per specificare un inizializzatore di classe è eseguire questa operazione chiamando `Database.SetInitializer` nella `Application_Start` metodo nella *Global. asax* file. Se si intende abilitare le migrazioni in un progetto che utilizza questo metodo per specificare l'inizializzatore, rimuovere questa riga di codice.


Successivamente, abilitare migrazioni Code First.

Il primo passaggio è assicurarsi che il progetto ContosoUniversity è impostato come progetto di avvio. Nelle **Esplora soluzioni**, fare clic sul progetto ContosoUniversity e selezionare **imposta come progetto di avvio**. Migrazioni Code First avrà un aspetto nel progetto di avvio per trovare la stringa di connessione di database.

Dal **degli strumenti** menu, fare clic su **Library Package Manager** e quindi **Package Manager Console**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

In cima il **Console di gestione pacchetti** finestra Seleziona ContosoUniversity.DAL come progetto predefinito e quindi at il `PM>` prompt dei comandi immettere "enable-migrations".

![Enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Questo comando crea una *Configuration.cs* in un nuovo file *migrazioni* cartella nel progetto ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

È selezionato il progetto DAL perché il comando "enable-migrations" deve essere eseguito nel progetto che contiene la classe del contesto Code First. Quando tale classe è in un progetto di libreria di classi, le migrazioni Code First Cerca la stringa di connessione di database nel progetto di avvio per la soluzione. Nella soluzione ContosoUniversity, il progetto web è stato impostato come progetto di avvio. (Se non si volesse designare il progetto con la stringa di connessione come progetto di avvio in Visual Studio, è possibile specificare il progetto di avvio nel comando di PowerShell. Per visualizzare la sintassi del comando per il comando enable-migrations, è possibile immettere il comando "get-help enable-migrations".)

Aprire il file Configuration.cs e sostituire i commenti nel `Seed` metodo con il codice seguente:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

I riferimenti a `List` hanno le righe ondulate rosse sotto di essi, perché non è necessario un `using` ancora l'istruzione per lo spazio dei nomi. Fare doppio clic su una delle istanze del `List` e fare clic su **risolvere**, quindi fare clic su **usando System.Collections.Generic**.

![Risolvere con l'istruzione using](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Questa opzione di menu consente di aggiungere il codice seguente per il `using` le istruzioni nella parte superiore del file.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Aggiunta di codice per il `Seed` metodo è uno dei tanti metodi che è possibile inserire dati fissa nel database. Un'alternativa consiste nell'aggiungere codice per il `Up` e `Down` metodi della classe ogni migrazione. Il `Up` e `Down` metodi contengono codice che implementa le modifiche del database. Sono riportati esempi di esse nel [distribuzione di un aggiornamento del Database](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) esercitazione.
> 
> È anche possibile scrivere codice che esegue istruzioni SQL usando il `Sql` (metodo). Ad esempio, se è stato aggiunta di una colonna di Budget per la tabella Department e si desidera inizializzare tutti i budget di reparto a $1.000,00 come parte di una migrazione, si potrebbe aggiungere la riga riportata di codice per il `Up` metodo di migrazione:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> In questo esempio viene visualizzato per questa esercitazione Usa la `AddOrUpdate` metodo nella `Seed` metodo per le migrazioni Code First `Configuration` classe. Codice prima le migrazioni chiamano il `Seed` metodo dopo tutte le migrazioni e questo metodo aggiorna le righe che sono già state inserite o li inserisce se non esistono ancora. Il `AddOrUpdate` metodo potrebbe non essere la scelta migliore per il proprio scenario. Per altre informazioni, vedere [prestare attenzione con Entity Framework 4.3 AddOrUpdate metodo](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sul blog di Julie.


Premere CTRL + MAIUSC + B per compilare il progetto.

Il passaggio successivo consiste nel creare un `DbMigration` classe per la migrazione iniziale. Si desidera che questa migrazione per creare un nuovo database, pertanto è necessario eliminare il database già esistente. Contenuti nel database SQL Server Compact *sdf* file con il *App\_dati* cartella. Nelle **Esplora soluzioni**, espandere *App\_Data* nel progetto ContosoUniversity per vedere i due database di SQL Server Compact, che sono rappresentati da *sdf*i file.

Fare doppio clic il *School.sdf* del file e fare clic su **eliminare**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

Nel **Console di gestione pacchetti** finestra, immettere il comando "add-migration Initial" per creare la migrazione iniziale e denominarla "Iniziale".

![aggiungere-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migrazioni Code First consente di creare un altro file di classe nel *migrazioni* cartella e questa classe contiene codice che crea lo schema del database.

Nel **Console di gestione pacchetti**, immettere il comando "update-database" per creare il database ed eseguire il **Seed** (metodo).

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Se si verifica un errore che indica una tabella esiste già e non può essere creata, è probabilmente perché è stata eseguita l'applicazione dopo che è stato eliminato il database e prima di eseguire `update-database`. Caso, eliminare il *School.sdf* nuovamente file e ripetere il `update-database` comando.)

Eseguire l'applicazione. A questo punto della pagina Students è vuota ma la pagina instructors (insegnanti) contiene instructors (insegnanti). Questo è ciò che verrà visualizzato nell'ambiente di produzione dopo aver distribuito l'applicazione.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Il progetto è ora pronto per distribuire il *School* database.

## <a name="creating-a-membership-database-for-deployment"></a>Creazione di un Database di appartenenza per la distribuzione

L'applicazione Contoso University Usa l'autenticazione di sistema e form di appartenenza ASP.NET per autenticare e autorizzare gli utenti. Una delle relative pagine è accessibile solo agli amministratori. Per visualizzare questa pagina, eseguire l'applicazione e selezionare **aggiornamento crediti** dal menu nel menu a comparsa **corsi**. L'applicazione visualizza la **Log In** pagina, poiché solo gli amministratori sono autorizzati a utilizzare il **crediti Update** pagina.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Accedere come "admin" usando la password "Pas$ w0rd" (si noti che il numero zero al posto la lettera "o" in "w0rd"). Dopo l'accesso, il **crediti Update** viene visualizzata la pagina.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Quando si distribuisce un sito per la prima volta, è comune per escludere la maggior parte o tutti gli account utente creati per il test. In questo caso, si distribuirà un account amministratore e non account utente. Anziché eliminare manualmente gli account di prova, si creerà un nuovo database di appartenenza che ha solo un account dell'utente amministratore che è necessario nell'ambiente di produzione.

> [!NOTE]
> Il database di appartenenza archivia un hash della password dell'account. Per distribuire gli account da un computer a un altro, è necessario assicurarsi che le routine di hash non generano hash diversi nel server di destinazione che non nel computer di origine. I generatori produrranno lo stesso hash quando si usa ASP.NET Universal Providers, fino a quando non modificare l'algoritmo predefinito. L'algoritmo predefinito è HMACSHA256 e viene specificato nel **convalida** attributo il **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** elemento nel file Web. config.


Il database di appartenenza non viene mantenuto per le migrazioni Code First e non vi è alcun inizializzatore automatico che effettua il seeding del database con gli account di prova (quanto accade per il database School). Pertanto, per mantenere disponibili i dati di test è verrà effettuata una copia del database di test prima di creare una nuova.

In **Esplora soluzioni**, Rinomina la *aspnet.sdf* del file nei *App\_dati* cartella in cui *aspnet-Dev.sdf*. (Non creare una copia, rinominarla, si creerà un nuovo database in un istante.)

Nelle **Esplora soluzioni**, assicurarsi che il progetto web (ContosoUniversity, non ContosoUniversity.DAL) sia selezionato. Quindi nel **Project** dal menu **configurazione di ASP.NET** per eseguire il **strumento Amministrazione sito Web**(WAT).

Selezionare il **sicurezza** scheda.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Fare clic su **crea o Gestisci ruoli** e aggiungere un' **amministratore** ruolo.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Tornare al **sicurezza** scheda, fare clic su **Create User**, e aggiungere l'utente "admin" come amministratore. Prima di scegliere il **Create User** pulsante il **Create User** pagina, assicurarsi di selezionare il **amministratore** casella di controllo. La password usata in questa esercitazione è "$w0rd agli indirizzi PA", ed è possibile immettere qualsiasi indirizzo di posta elettronica.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Chiudere il browser. Nelle **Esplora soluzioni**, fare clic sul pulsante Aggiorna per visualizzare la nuova *aspnet.sdf* file.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Fare doppio clic su **aspnet.sdf** e selezionare **Includi nel progetto**.

## <a name="distinguishing-development-from-production-databases"></a>Sviluppo distintivo dal database di produzione

In questa sezione, si verranno rinominare i database in modo che le versioni di sviluppo dell'istituto di istruzione Dev.sdf aspnet-Dev.sdf e le versioni di produzione sono aspnet-Prod.sdf e dell'istituto di istruzione Prod.sdf. Ciò non è necessaria, ma tale operazione così consentirà di rimanere confusi dai test e produzione versioni dei database.

Nelle **Esplora soluzioni**, fare clic su **aggiornare** ed espandere l'App\_cartella dati per visualizzare il database School creato in precedenza, pulsante destro del mouse e selezionare **Includi nel progetto** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Rinominare *aspnet.sdf* al *aspnet-Prod.sdf*.

Rinominare *School.sdf* al *School-Dev.sdf*.

Quando si esegue l'applicazione in Visual Studio non si vuole usare il *-Prod* le versioni dei file di database, si desidera utilizzare *- Dev* versioni. Di conseguenza è necessario modificare le stringhe di connessione nel file Web. config in modo che facciano riferimento il *- Dev* versioni dei database. (È ancora stato creato un file dell'istituto di istruzione Prod.sdf, ma va bene perché Code First creerà il database in fase di produzione prima che eseguire l'app non esiste).

Aprire il file Web. config dell'applicazione e individuare le stringhe di connessione:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Modificare "aspnet.sdf" a "aspnet-Dev.sdf" e "School.sdf" a "Dev.sdf dell'istituto di istruzione":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Entrambi i database e il motore di database SQL Server Compact sono ora pronti per essere distribuito. Nell'esercitazione seguente configuri automatic *Web. config* trasformazioni di file per le impostazioni che devono essere diverse in ambienti di sviluppo, test e produzione. (Tra le impostazioni da modificare sono le stringhe di connessione, ma si configurerà le modifiche in un secondo momento quando si crea un profilo di pubblicazione).

## <a name="more-information"></a>Altre informazioni

Per altre informazioni su NuGet, vedere [gestire le librerie di progetto con NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) e [documentazione di NuGet](http://docs.nuget.org/docs/start-here/overview). Se non si desidera usare NuGet, è necessario per informazioni su come analizzare un pacchetto NuGet per determinare che cosa accade quando viene installato. (Ad esempio, è possibile configurare *Web. config* trasformazioni, configurare gli script di PowerShell da eseguire in fase di compilazione e così via.) Per altre informazioni sull'uso di NuGet, vedere in particolare [creazione e pubblicazione di un pacchetto](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e [File di configurazione e trasformazioni di codice sorgente](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
