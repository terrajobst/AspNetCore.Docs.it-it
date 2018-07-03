---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Distribuzione Web ASP.NET tramite Visual Studio: preparazione per la distribuzione di Database | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 21b4a924115daff6ee79ce045330c748b58246ee
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388539"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Distribuzione Web ASP.NET tramite Visual Studio: preparazione per la distribuzione di Database
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come ottenere il progetto è pronto per la distribuzione di database. La struttura del database e alcuni (non tutti) dei dati in due dell'applicazione i database devono essere distribuiti in ambienti di produzione, staging e test.

In genere, quando si sviluppa un'applicazione, immettere i dati di test in un database che non si desidera distribuire in un sito live. Tuttavia, potrebbe essere anche alcuni dati di produzione che si desidera distribuire. In questa esercitazione si configura il progetto di Contoso University e preparare gli script SQL in modo che i dati corretti sono inclusi quando si distribuisce.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

L'applicazione di esempio Usa SQL Server Express LocalDB. SQL Server Express è l'edizione gratuita di SQL Server. Viene comunemente usato durante lo sviluppo perché si basa sul motore di database stesso, come le versioni complete di SQL Server. È possibile testare con SQL Server Express e avere la certezza che l'applicazione si comporta allo stesso nell'ambiente di produzione, con alcune eccezioni per le funzionalità che variano tra le edizioni di SQL Server.

LocalDB è una modalità di esecuzione speciale di SQL Server Express che consente di lavorare con i database come *mdf* file. In genere, vengono archiviati i database LocalDB di *App\_dati* cartella del progetto web. La funzionalità dell'istanza utente in SQL Server Express consente inoltre di usare *mdf* i file, ma la funzionalità dell'istanza utente è deprecato; pertanto, Local DB è consigliato per l'utilizzo di *mdf* file.

In genere SQL Server Express non viene utilizzato per le applicazioni web di produzione. LocalDB in particolare non è consigliato per la produzione con un'applicazione web perché non è progettato per funzionare con IIS.

In Visual Studio 2012, Local DB viene installato per impostazione predefinita con Visual Studio. In Visual Studio 2010 e versioni precedenti, SQL Server Express (senza LocalDB) viene installato per impostazione predefinita con Visual Studio. vale a dire il motivo per cui è installato come uno dei prerequisiti nella [la prima esercitazione di questa serie](introduction.md).

Per altre informazioni sulle edizioni di SQL Server, tra cui LocalDB, vedere le risorse seguenti [utilizzo di database di SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework e i provider universali

Per l'accesso al database, l'applicazione Contoso University richiede il seguente software che deve essere distribuito con l'applicazione perché non è incluso in .NET Framework:

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (consente al sistema di appartenenza ASP.NET di usare Database SQL di Azure)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Poiché questo software è incluso nei pacchetti NuGet, il progetto è già stato configurato in modo che gli assembly necessari vengono distribuiti con il progetto. (I collegamenti puntino alle versioni correnti di questi pacchetti, che potrebbero essere più recenti rispetto a quella installata nel progetto di avvio che è stato scaricato per questa esercitazione).

Se si distribuisce in un provider di hosting di terze parti invece che in Azure, assicurarsi di usare Entity Framework 5.0 o versioni successive. Le versioni precedenti di migrazioni Code First richiedono attendibilità totale e la maggior parte dei provider di hosting che esegue l'applicazione in Medium Trust. Per altre informazioni sull'attendibilità media, vedere la [Distribuisci in IIS come ambiente di Test](deploying-to-iis.md) esercitazione.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Configurare le migrazioni Code First per la distribuzione di applicazioni database

Il database dell'applicazione di Contoso University è gestito da Code First e si distribuirà usando migrazioni Code First. Per una panoramica della distribuzione di database usando migrazioni Code First, vedere [la prima esercitazione di questa serie](introduction.md).

Quando si distribuisce un database dell'applicazione, in genere non è sufficiente vengono distribuiti nel database di sviluppo con tutti i dati in esso nell'ambiente di produzione, perché la maggior parte dei dati in esso è probabilmente presente solo per scopi di test. Ad esempio, i nomi degli studenti in un database di test sono fittizi. D'altra parte, è spesso non è possibile distribuire solo la struttura del database senza dati in esso affatto. Alcuni dei dati nel database di test potrebbe essere dati reali e deve essere presente quando gli utenti iniziano a usare l'applicazione. Ad esempio, il database potrebbe creare una tabella che contiene i valori validi di livello o i nomi dei reparti reali.

Per simulare questo scenario comune, si configurerà un migrazioni Code First `Seed` metodo che inserisce nel database solo i dati che si desidera essere presenti nell'ambiente di produzione. Ciò `Seed` (metodo) non dovrà inserire dati di test perché verrà eseguito nell'ambiente di produzione dopo Code First consente di creare il database nell'ambiente di produzione.

Nelle versioni precedenti di Code First prima che le migrazioni è stato rilasciato, era comune per `Seed` metodi per inserire dati di test, inoltre, poiché con ogni modifica del modello durante lo sviluppo di database doveva essere completamente eliminato e ricreato da zero. Con migrazioni Code First, test, i dati vengono conservati dopo le modifiche di database, pertanto, inclusi i dati di test nel `Seed` (metodo) non è necessario. Il progetto scaricato Usa il metodo di inclusione di tutti i dati di `Seed` metodo della classe inizializzatore. In questa esercitazione si sarà disabilitare tale classe inizializzatore e `enable Migrations. Then you'll update the `inizializzazione ' metodo nella configurazione di migrazioni la classe in modo da inserire solo i dati che si desidera essere inserite nell'ambiente di produzione.

Il diagramma seguente illustra lo schema del database dell'applicazione:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Per queste esercitazioni, si partirà dal presupposto che il `Student` e `Enrollment` tabelle devono essere vuote quando il sito prima di tutto viene distribuito. Le altre tabelle contengono dati che deve essere precaricata quando l'applicazione viene resa disponibile.

### <a name="disable-the-initializer"></a>Disabilitare l'inizializzatore

Poiché si userà migrazioni Code First, non è necessario utilizzare il `DropCreateDatabaseIfModelChanges` inizializzatore Code First. Il codice per questo inizializzatore è nel *SchoolInitializer.cs* file nel progetto ContosoUniversity.DAL. Un'impostazione nel `appSettings` elemento del *Web. config* file causa questo inizializzatore per l'esecuzione ogni volta che l'applicazione tenta di accedere al database per la prima volta:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Aprire l'applicazione *Web. config* del file e rimuovere o impostare come commento il `add` elemento che specifica la classe inizializzatore Code First. Il `appSettings` elemento avrà ora un aspetto simile al seguente:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Un altro modo per specificare un inizializzatore di classe è eseguire questa operazione chiamando `Database.SetInitializer` nella `Application_Start` metodo nella *Global. asax* file. Se si intende abilitare le migrazioni in un progetto che utilizza questo metodo per specificare l'inizializzatore, rimuovere questa riga di codice.


> [!NOTE]
> Se si usa Visual Studio 2013, aggiungere i passaggi seguenti tra i passaggi 2 e 3: (a) nella console di gestione pacchetti immettere "update-package entityframework-versione 6.1.1" per ottenere la versione corrente di Entity Framework. Quindi compilare (b) il progetto per ottenere un elenco di errori di compilazione e risolverli. Eliminare con istruzioni per gli spazi dei nomi non sono più presenti, fare doppio clic su e fare clic su Risolvi aggiungere istruzioni using dove sono più necessarie e modificare le occorrenze di System.Data.EntityState System.Data.Entity.EntityState.


### <a name="enable-code-first-migrations"></a>Abilitare migrazioni Code First

1. Assicurarsi che il progetto ContosoUniversity (non ContosoUniversity.DAL) è impostato come progetto di avvio. Nelle **Esplora soluzioni**, fare clic sul progetto ContosoUniversity e selezionare **imposta come progetto di avvio**. Migrazioni Code First avrà un aspetto nel progetto di avvio per trovare la stringa di connessione di database.
2. Dal **degli strumenti** menu, fare clic su **Library Package Manager** (o **Gestione pacchetti NuGet**) e quindi **Package Manager Console**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. In cima il **Console di gestione pacchetti** finestra Seleziona ContosoUniversity.DAL come progetto predefinito e quindi at il `PM>` prompt dei comandi immettere "enable-migrations".

    ![comando Enable-migrations](preparing-databases/_static/image4.png)

    (Se si verifica un errore che segnala che il *enable-migrations* comando non è riconosciuto, immettere il comando *update-package EntityFramework-reinstallare* e riprovare.)

    Questo comando crea una *migrazioni* cartella nel progetto ContosoUniversity.DAL e lo inserisce nella cartella due file: un *Configuration.cs* file che è possibile usare per configurare le migrazioni e un *InitialCreate.cs* file per la prima migrazione che crea il database.

    ![Cartella migrazioni](preparing-databases/_static/image5.png)

    È selezionato il progetto nel **progetto predefinito** elenco a discesa delle **Console di gestione pacchetti** perché i `enable-migrations` comando deve essere eseguito nel progetto che contiene il Code First classe del contesto. Quando tale classe è in un progetto di libreria di classi, le migrazioni Code First Cerca la stringa di connessione di database nel progetto di avvio per la soluzione. Nella soluzione ContosoUniversity, il progetto web è stato impostato come progetto di avvio. Se non si desidera designare il progetto con la stringa di connessione come progetto di avvio in Visual Studio, è possibile specificare il progetto di avvio nel comando di PowerShell. Per visualizzare la sintassi del comando, immettere il comando `get-help enable-migrations`.

    Il `enable-migrations` comando creata automaticamente la prima migrazione perché il database esiste già. Un'alternativa consiste nell'avere Migrations crea il database. A tale scopo, usare **Esplora Server** oppure **Esplora oggetti di SQL Server** per eliminare il database ContosoUniversity prima di abilitare le migrazioni. Dopo aver abilitato migrazioni, creare manualmente la prima migrazione immettendo il comando "add-migration InitialCreate". È quindi possibile creare il database immettendo il comando "update-database".

### <a name="set-up-the-seed-method"></a>Configurare il metodo di inizializzazione

Per questa esercitazione si aggiungerà dati fissa aggiungendo il codice per il `Seed` metodo per le migrazioni Code First `Configuration` classe. Codice prima le migrazioni chiamano il `Seed` metodo dopo ogni migrazione.

Poiché il `Seed` metodo viene eseguito dopo tutte le migrazioni, quindi i dati già nelle tabelle dopo la prima migrazione. Per gestire questa situazione si userà il `AddOrUpdate` metodo per aggiornare le righe che sono già state inserite o inserirli se non esistono ancora. Il `AddOrUpdate` metodo potrebbe non essere la scelta migliore per il proprio scenario. Per altre informazioni, vedere [prestare attenzione con Entity Framework 4.3 AddOrUpdate metodo](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sul blog di Julie.

1. Aprire il *Configuration.cs* del file e sostituire i commenti nel `Seed` metodo con il codice seguente:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. I riferimenti a `List` hanno le righe ondulate rosse sotto di essi, perché non è necessario un `using` ancora l'istruzione per lo spazio dei nomi. Fare doppio clic su una delle istanze del `List` e fare clic su **risolvere**, quindi fare clic su **usando System.Collections.Generic**.

    ![Risolvere con l'istruzione using](preparing-databases/_static/image6.png)

    Questa opzione di menu consente di aggiungere il codice seguente per il `using` le istruzioni nella parte superiore del file.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Premere CTRL + MAIUSC + B per compilare il progetto.

Il progetto è ora pronto per distribuire il *ContosoUniversity* database. Dopo aver distribuito l'applicazione, la prima volta eseguirlo e passare alla pagina di accesso al database, Code First verranno creare il database ed eseguire questo `Seed` (metodo).

> [!NOTE]
> Aggiunta di codice per il `Seed` metodo è uno dei tanti metodi che è possibile inserire dati fissa nel database. Un'alternativa consiste nell'aggiungere codice per il `Up` e `Down` metodi della classe ogni migrazione. Il `Up` e `Down` metodi contengono codice che implementa le modifiche del database. Sono riportati esempi di esse nel [distribuzione di un aggiornamento del Database](deploying-a-database-update.md) esercitazione.
> 
> È anche possibile scrivere codice che esegue istruzioni SQL usando il `Sql` (metodo). Ad esempio, se è stato aggiunta di una colonna di Budget per la tabella Department e si desidera inizializzare tutti i budget di reparto a $1.000,00 come parte di una migrazione, si potrebbe aggiungere la riga riportata di codice per il `Up` metodo di migrazione:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Creare script per la distribuzione del database di appartenenza

L'applicazione Contoso University Usa l'autenticazione di sistema e form di appartenenza ASP.NET per autenticare e autorizzare gli utenti. Il **crediti Update** pagina è accessibile solo agli utenti che appartengono al ruolo di amministratore.

Eseguire l'applicazione e fare clic su **corsi**, quindi fare clic su **crediti Update**.

![Fare clic su Update crediti](preparing-databases/_static/image7.png)

Il **accedere** verrà visualizzata la pagina perché il **crediti Update** pagina richiede privilegi amministrativi.

Immettere *admin* come nome utente e *devpwd* come le password e fare clic su **Accedi**.

![Pagina di accesso](preparing-databases/_static/image8.png)

Il **crediti Update** verrà visualizzata la pagina.

![Pagina di crediti di aggiornamento](preparing-databases/_static/image9.png)

Informazioni utente e il ruolo sono nel *aspnet-ContosoUniversity* database specificato dalle **DefaultConnection** stringa di connessione nel *Web. config* file.

Questo database non è gestito da Code First di Entity Framework, pertanto è possibile usare le migrazioni per distribuirla. Si userà il provider dbDacFx per distribuire lo schema del database e si configurerà il profilo di pubblicazione per eseguire uno script che inserirà iniziale dei dati in tabelle di database.

> [!NOTE]
> Con Visual Studio 2013 è stato introdotto un nuovo sistema di appartenenze ASP.NET (ora denominato ASP.NET Identity). Il nuovo sistema consente di mantenere l'applicazione sia nelle tabelle di appartenenza nello stesso database ed è possibile usare migrazioni Code First per distribuire entrambi. L'applicazione di esempio Usa il sistema di appartenenze ASP.NET precedenti, che non può essere distribuito usando migrazioni Code First. Le procedure per la distribuzione di questo database di appartenenza si applicano anche a qualsiasi altro scenario in cui l'applicazione deve distribuire un database di SQL Server che non viene creato da Code First di Entity Framework.


Di seguito, in genere non si desidera gli stessi dati nell'ambiente di produzione che è necessario in fase di sviluppo. Quando si distribuisce un sito per la prima volta, è comune per escludere la maggior parte o tutti gli account utente creati per il test. Pertanto, il progetto scaricato è disponibili due database di appartenenza: *aspnet-ContosoUniversity.mdf* con gli utenti di sviluppo e *aspnet-ContosoUniversity-Prod.mdf* con gli utenti di produzione. Per questa esercitazione i nomi utente sono gli stessi in entrambi i database: *admin* e *nonadmin*. Gli utenti dispongono della password *devpwd* nel database di sviluppo e *prodpwd* nel database di produzione.

Gli utenti di sviluppo verrà distribuito l'ambiente di test e gli utenti di produzione in gestione temporanea e produzione. A tale scopo si creerà due script SQL in questa esercitazione, uno per lo sviluppo e uno per la produzione e nelle esercitazioni successive si configurerà il processo di pubblicazione per eseguirli.

> [!NOTE]
> Il database di appartenenza archivia un hash della password dell'account. Per distribuire gli account da un computer a un altro, è necessario assicurarsi che le routine di hash non generano hash diversi nel server di destinazione che non nel computer di origine. I generatori produrranno lo stesso hash quando si usa ASP.NET Universal Providers, fino a quando non modificare l'algoritmo predefinito. L'algoritmo predefinito è HMACSHA256 e viene specificato nel **convalida** attributo il **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** elemento nel file Web. config.


È possibile creare script di distribuzione dei dati manualmente, usando SQL Server Management Studio (SSMS) o tramite uno strumento di terze parti. La parte restante di questa esercitazione illustrerà come eseguire questa operazione in SQL Server Management Studio, ma se non si vuole installare e usare SQL Server Management Studio è possibile ottenere gli script dalla versione completata del progetto e passare alla sezione in cui vengono archiviati nella cartella della soluzione.

Per installare SQL Server Management Studio, installarlo dalla [area Download Microsoft: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) facendo clic [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) o [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Se si sceglie quello errato per il sistema avrà esito negativo installare e provare a effettuare un altro.

(Si noti che questo è un download di 600 MB. Potrebbe richiedere molto tempo a installare e richiede un riavvio del computer in uso.)

Nella prima pagina del Centro installazione SQL Server, fare clic su **nuova installazione SQL Server autonomo o aggiungere caratteristiche a un'installazione esistente**e seguire le istruzioni, accettando le scelte predefinite.

### <a name="create-the-development-database-script"></a>Creare uno script di database di sviluppo

1. Eseguire SSMS.
2. Nel **Connetti al Server** finestra di dialogo immettere *\v11.0 (localdb)* come il **nome del Server**, lasciare **autenticazione** impostato su **L'autenticazione di Windows**, quindi fare clic su **Connect**.

    ![SQL Server Management Studio connettersi al Server](preparing-databases/_static/image10.png)
3. Nel **Esplora oggetti** finestra, espandere **database**, fare doppio clic su **aspnet-ContosoUniversity**, fare clic su **attività**, quindi fare clic su **Genera script**.

    ![SSMS genera script](preparing-databases/_static/image11.png)
4. Nel **genera e pubblica script** finestra di dialogo, fare clic su **Imposta opzioni di Scripting**.

    È possibile ignorare il **Seleziona oggetti** passaggio perché il valore predefinito è **genera Script per intero database e tutti gli oggetti di database** e che è quello desiderato.
5. Scegliere **Avanzate**.

    ![Opzioni di generazione script di SQL Server Management Studio](preparing-databases/_static/image12.png)
6. Nel **opzioni di generazione script avanzate** scorrere fino alla finestra di dialogo **tipi di dati da script**e fare clic sui **solo dati** opzione nell'elenco a discesa.
7. Change **Script per USE DATABASE** al **False**. USO delle istruzioni non è validi per il Database SQL di Azure e non è necessari per la distribuzione in SQL Server Express nell'ambiente di test.

    ![SQL Server Management Studio Script solo dati, alcuna istruzione USE](preparing-databases/_static/image13.png)
8. Fare clic su **OK**.
9. Nel **genera e pubblica script** finestra di dialogo, il **nome File** finestra specifica in cui verrà creato lo script. Modificare il percorso di cartella della soluzione (la cartella che contiene il file ContosoUniversity.sln) e il nome del file per *aspnet-data-dev.sql*.
10. Fare clic su **successivo** per passare alle **riepilogo** scheda e quindi fare clic su **Avanti** nuovamente per creare lo script.

    ![Creati Script di SQL Server Management Studio](preparing-databases/_static/image14.png)
11. Scegliere **Fine**.

### <a name="create-the-production-database-script"></a>Creare uno script di database di produzione

Poiché il progetto è stata ancora eseguita con il database di produzione, non è associata ancora all'istanza di Local DB. Pertanto è necessario collegare il database prima di tutto.

1. In SSMS **Esplora oggetti**, fare doppio clic su **database** e fare clic su **Attach**.

    ![Connettere SQL Server Management Studio](preparing-databases/_static/image15.png)
2. Nel **Collega database** nella finestra di dialogo fare clic su **Add** e quindi passare al *aspnet-ContosoUniversity-Prod.mdf* del file nei *App\_ Dati* cartella.

     ![SQL Server Management Studio aggiungere file con estensione mdf da collegare](preparing-databases/_static/image16.png)
3. Fare clic su **OK**.
4. Seguire la stessa procedura usata in precedenza per creare uno script per il file di produzione. Denominare il file di script *aspnet-data-prod.sql*.

## <a name="summary"></a>Riepilogo

Entrambi i database sono ora pronti per essere distribuito e si dispone di due script di distribuzione dei dati nella cartella della soluzione.

![Script di distribuzione dei dati](preparing-databases/_static/image17.png)

Nell'esercitazione seguente configurare le impostazioni di progetto che influiscono sulla distribuzione e si configura automaticamente *Web. config* trasformazioni di file per le impostazioni che devono essere diverse nell'applicazione distribuita.

## <a name="more-information"></a>Altre informazioni

Per altre informazioni su NuGet, vedere [gestire le librerie di progetto con NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) e [documentazione di NuGet](http://docs.nuget.org/docs/start-here/overview). Se non si desidera usare NuGet, è necessario per informazioni su come analizzare un pacchetto NuGet per determinare che cosa accade quando viene installato. (Ad esempio, è possibile configurare *Web. config* trasformazioni, configurare gli script di PowerShell da eseguire in fase di compilazione e così via.) Per altre informazioni sull'uso di NuGet, vedere [creazione e pubblicazione di un pacchetto](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e [File di configurazione e trasformazioni di codice sorgente](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Precedente](introduction.md)
> [Successivo](web-config-transformations.md)
