---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: preparazione per la distribuzione di Database | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 1f19d54a5f2679f790575d520b28472d4ff3233f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: preparazione per la distribuzione di Database
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come ottenere il progetto è pronta per la distribuzione di database. La struttura del database e alcune (non tutte) dei dati in due dell'applicazione i database devono essere distribuiti a test, gestione temporanea e ambienti di produzione.

In genere, quando si sviluppa un'applicazione, immettere i dati di test in un database che non si desidera distribuire in un sito in tempo reale. Tuttavia, potrebbe essere anche alcuni dati di produzione che si desidera distribuire. In questa esercitazione viene configurare il progetto Contoso University e preparare gli script SQL in modo che i dati corretti vengono inclusi quando si distribuisce.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

L'applicazione di esempio utilizza SQL Server Express LocalDB. SQL Server Express è l'edizione gratuita di SQL Server. Viene usata in genere durante lo sviluppo perché si basa sul motore di database stesso, come le versioni complete di SQL Server. È possibile testare con SQL Server Express e verificare che l'applicazione si comporterà in produzione, con alcune eccezioni per le funzionalità che variano tra le edizioni di SQL Server.

LocalDB è una modalità speciali di esecuzione di SQL Server Express che consente di lavorare con i database come *con estensione mdf* file. In genere, vengono archiviati i database LocalDB di *App\_dati* cartella del progetto web. La funzionalità istanze utente in SQL Server Express consente inoltre di utilizzare *con estensione mdf* file, ma la funzionalità istanze utente è deprecato; pertanto, LocalDB è consigliato per l'utilizzo di *con estensione mdf* file.

In genere SQL Server Express non viene usato per le applicazioni web di produzione. LocalDB in particolare non è consigliato per la produzione con un'applicazione web perché non è progettato per funzionare con IIS.

In Visual Studio 2012, LocalDB è installato per impostazione predefinita con Visual Studio. In Visual Studio 2010 e versioni precedenti, SQL Server Express (senza LocalDB) viene installato per impostazione predefinita con Visual Studio. vale a dire motivo per cui è installato come uno dei prerequisiti in [la prima esercitazione di questa serie](introduction.md).

Per ulteriori informazioni sulle edizioni di SQL Server, tra cui LocalDB, vedere le risorse seguenti [si lavora con database di SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework e Universal Providers

Per l'accesso al database, l'applicazione Contoso University richiede il seguente software che deve essere distribuito con l'applicazione perché non è incluso in .NET Framework:

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (consente il sistema di appartenenze ASP.NET di utilizzare il Database di SQL Azure)
- [Entity Framework](https://msdn.microsoft.com/en-us/library/gg696172.aspx)

Poiché questo software è incluso in pacchetti NuGet, il progetto è già configurato in modo che gli assembly necessari vengono distribuiti con il progetto. (I collegamenti puntano alle versioni correnti di questi pacchetti, che potrebbero essere più recenti rispetto a ciò che viene installato nel progetto di avvio che per questa esercitazione è stato scaricato).

Se si distribuisce in un provider di hosting di terze parti invece di Azure, assicurarsi che si utilizza Entity Framework 5.0 o versione successiva. Versioni precedenti di migrazioni Code First richiedono l'attendibilità totale e la maggior parte dei provider di hosting che esegue l'applicazione in Medium Trust. Per ulteriori informazioni sull'attendibilità media, vedere il [Distribuisci a IIS come ambiente di Test](deploying-to-iis.md) esercitazione.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Configurare le migrazioni Code First per la distribuzione di applicazioni database

Il database dell'applicazione Contoso University è gestito da Code First e verrà distribuita utilizzando migrazioni Code First. Per una panoramica della distribuzione di database utilizzando migrazioni Code First, vedere [la prima esercitazione di questa serie](introduction.md).

Quando si distribuisce un database dell'applicazione, in genere non sufficiente vengono distribuiti nel database di sviluppo con tutti i dati in essa contenuti nell'ambiente di produzione, perché la maggior parte dei dati in esso è probabilmente presente solo a scopo di test. Ad esempio, i nomi di studenti in un database di test sono fittizi. D'altra parte, è spesso possibile distribuire solo la struttura di database senza dati in essa contenuti affatto. Alcuni dei dati nel database di test potrebbe essere dati reali e deve essere presente quando gli utenti iniziano a usare l'applicazione. Ad esempio, il database potrebbe disporre di una tabella che contiene i valori del livello valido o nomi di reparto reale.

Per simulare questo scenario comune, sarà necessario configurare un migrazioni Code First `Seed` metodo che inserisce nel database solo i dati che si desidera essere presenti nell'ambiente di produzione. Questo `Seed` (metodo) non devono inserire i dati di test perché verrà eseguito nell'ambiente di produzione dopo Code First, viene creato il database nell'ambiente di produzione.

Nelle versioni precedenti di Code First prima del rilascio, migrazioni è comune per `Seed` metodi per inserire dati di test, inoltre, poiché con ogni modifica del modello durante lo sviluppo il database deve essere completamente eliminato e ricreato da zero. Con le migrazioni Code First, test, i dati vengono mantenuti dopo le modifiche del database, pertanto l'inclusione di dati di test nel `Seed` metodo non è necessario. Il progetto scaricato Usa il metodo di inclusione di tutti i dati di `Seed` metodo della classe inizializzatore. In questa esercitazione si sarà disabilitare tale classe inizializzatore e `enable Migrations. Then you'll update the `valore di inizializzazione ' metodo nella configurazione migrazioni classe in modo che venga inserita solo i dati che si desidera inserire nell'ambiente di produzione.

Il diagramma seguente illustra lo schema del database dell'applicazione:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Per queste esercitazioni, si presuppone che il `Student` e `Enrollment` tabelle devono essere vuote quando il sito viene innanzitutto distribuito. Le altre tabelle contengono dati che deve essere precaricata quando l'applicazione passa in tempo reale.

### <a name="disable-the-initializer"></a>Disabilitare l'inizializzatore

Poiché si utilizzando migrazioni Code First, non è necessario utilizzare il `DropCreateDatabaseIfModelChanges` inizializzatore Code First. Il codice per questo inizializzatore è il *SchoolInitializer.cs* file nel progetto ContosoUniversity.DAL. Un'impostazione di `appSettings` elemento del *Web. config* file causa questo inizializzatore eseguita ogni volta che l'applicazione tenta di accedere al database per la prima volta:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Aprire l'applicazione *Web. config* file e rimuovere o impostare come commento il `add` elemento che specifica la classe di inizializzatore Code First. Il `appSettings` elemento ora simile al seguente:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> È possibile specificare un inizializzatore di classe farlo chiamando `Database.SetInitializer` nel `Application_Start` metodo il *Global. asax* file. Se si siano abilitando le migrazioni in un progetto che utilizza tale metodo per specificare l'inizializzatore, rimuovere la riga di codice.


> [!NOTE]
> Se si utilizza Visual Studio 2013, aggiungere quanto segue tra i passaggi 2 e 3: immettere PMC (a) In "pacchetto di aggiornamento entityframework-versione 6.1.1" per ottenere la versione corrente di Entity Framework. Quindi la compilazione (b) il progetto per ottenere un elenco di errori di compilazione e correggerli. Eliminare l'utilizzo di istruzioni per gli spazi dei nomi che non esiste, destro e fare clic su Risolvi aggiungere istruzioni using in cui sono necessari e modificare le occorrenze di System.Data.EntityState a System.Data.Entity.EntityState.


### <a name="enable-code-first-migrations"></a>Abilitare le migrazioni Code First

1. Assicurarsi che il progetto di ContosoUniversity (non ContosoUniversity.DAL) è impostato come progetto di avvio. In **Esplora**, fare clic sul progetto ContosoUniversity e selezionare **imposta come progetto di avvio**. Migrazioni Code First avrà un aspetto del progetto di avvio per trovare la stringa di connessione di database.
2. Dal **strumenti** menu, fare clic su **Gestione pacchetti libreria** (o **Gestione pacchetti NuGet**) e quindi **Package Manager Console**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. Nella parte superiore del **Package Manager Console** finestra selezionare ContosoUniversity.DAL come progetto predefinito e quindi at il `PM>` prompt dei comandi immettere "enable-migrations".

    ![comando Enable-migrations](preparing-databases/_static/image4.png)

    (Se si verifica un errore che segnala che il *enable-migrations* comando non è riconosciuto, immettere il comando *pacchetto di aggiornamento EntityFramework-reinstallare* e riprovare..)

    Questo comando crea un *migrazioni* cartella nel progetto ContosoUniversity.DAL e lo inserisce nella cartella due file: un *Configuration.cs* file che è possibile utilizzare per configurare le migrazioni e un *InitialCreate.cs* file per la migrazione prima che crea il database.

    ![Cartella Migrations](preparing-databases/_static/image5.png)

    È selezionato il progetto nel **progetto predefinito** elenco a discesa del **Package Manager Console** perché il `enable-migrations` comando deve essere eseguito nel progetto che contiene il primo codice classe del contesto. Quando tale classe è in un progetto libreria di classi, migrazioni Code First Cerca la stringa di connessione di database nel progetto di avvio per la soluzione. Nella soluzione ContosoUniversity, il progetto web è stato impostato come progetto di avvio. Se non si desidera impostare il progetto che include la stringa di connessione come progetto di avvio in Visual Studio, è possibile specificare il progetto di avvio del comando di PowerShell. Per visualizzare la sintassi del comando, immettere il comando `get-help enable-migrations`.

    Il `enable-migrations` comando creato automaticamente la migrazione prima perché il database esiste già. Alternativa, è possibile disporre le migrazioni di creare il database. A tale scopo, utilizzare **Esplora Server** o **Esplora oggetti di SQL Server** per eliminare il database ContosoUniversity prima di abilitare le migrazioni. Dopo aver abilitato le migrazioni, creare manualmente la migrazione prima immettendo il comando "migrazione aggiungere InitialCreate". È quindi possibile creare il database immettendo il comando "update-database".

### <a name="set-up-the-seed-method"></a>Impostare il metodo di inizializzazione

Per questa esercitazione si aggiungerà i dati aggiungendo codice di `Seed` metodo le migrazioni Code First `Configuration` classe. Codice viene chiamato prima di migrazioni di `Seed` metodo dopo tutte le migrazioni.

Poiché il `Seed` metodo viene eseguito dopo ogni la migrazione, è dati già presente nelle tabelle dopo la migrazione prima. Per gestire questa situazione si userà il `AddOrUpdate` metodo per aggiornare le righe che sono già state inserite o inserirli se non esistono ancora. Il `AddOrUpdate` metodo potrebbe non essere la scelta ottimale per lo scenario. Per ulteriori informazioni, vedere [prestare attenzione con metodo AddOrUpdate di EF 4.3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sul blog di Julie Lerman.

1. Aprire il *Configuration.cs* file e sostituire i commenti nel `Seed` (metodo) con il codice seguente:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. I riferimenti a `List` sono le righe ondulate rosse sottostanti perché non è un `using` ancora l'istruzione per lo spazio dei nomi. Fare doppio clic su una delle istanze di `List` e fare clic su **risolvere**, quindi fare clic su **using System.Collections.Generic**.

    ![Risolvere con l'istruzione using](preparing-databases/_static/image6.png)

    Questa opzione di menu consente di aggiungere il codice seguente per il `using` istruzioni nella parte superiore del file.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Premere CTRL-MAIUSC-B per compilare il progetto.

Il progetto è ora possibile distribuire il *ContosoUniversity* database. Dopo aver distribuito l'applicazione, la prima volta, eseguirlo e passare a una pagina che accede al database, Code First verrà creare il database ed eseguire questa query `Seed` metodo.

> [!NOTE]
> Aggiunta di codice per il `Seed` metodo è uno dei tanti metodi che è possibile inserire i dati nel database. In alternativa è possibile aggiungere codice per il `Up` e `Down` metodi di ogni classe di migrazione. Il `Up` e `Down` metodi contengono codice che implementa le modifiche del database. Esempi di in verrà visualizzato il [un aggiornamento del Database di distribuzione](deploying-a-database-update.md) esercitazione.
> 
> È anche possibile scrivere codice che esegue istruzioni SQL utilizzando il `Sql` metodo. Ad esempio, se si desidera inizializzare tutti i budget di reparto a $1.000,00 come parte di una migrazione sono stati aggiunta di una colonna di Budget per la tabella del reparto, è Impossibile aggiungere la riga seguenti di codice per il `Up` metodo per la migrazione:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Creare script per la distribuzione del database di appartenenza

L'applicazione Contoso University utilizza l'autenticazione di sistema e form di appartenenza ASP.NET per autenticare e autorizzare gli utenti. Il **aggiornamento crediti** pagina è accessibile solo agli utenti che appartengono al ruolo di amministratore.

Eseguire l'applicazione e fare clic su **corsi**, quindi fare clic su **aggiornamento crediti**.

![Fare clic su aggiornamento crediti](preparing-databases/_static/image7.png)

Il **Accedi** verrà visualizzata la pagina perché il **aggiornamento crediti** pagina richiede privilegi amministrativi.

Immettere *admin* come nome utente e *devpwd* come la password e fare clic su **Accedi**.

![Pagina di accesso](preparing-databases/_static/image8.png)

Il **aggiornamento crediti** verrà visualizzata la pagina.

![Aggiornare la pagina crediti](preparing-databases/_static/image9.png)

Informazioni utente e il ruolo sono nel *aspnet ContosoUniversity* database specificato dal **DefaultConnection** nella stringa di connessione di *Web. config* file.

Questo database non è gestito da Entity Framework Code First, non è possibile utilizzare le migrazioni di distribuirlo. Si utilizzerà il provider dbDacFx per distribuire lo schema del database e si configurerà il profilo di pubblicazione per eseguire uno script che consentono di immettere i dati iniziali in tabelle di database.

> [!NOTE]
> Con Visual Studio 2013, è stato introdotto un nuovo sistema di appartenenze ASP.NET (ora denominato ASP.NET Identity). Il nuovo sistema consente di mantenere l'applicazione sia tabelle delle appartenenze nello stesso database e utilizzare migrazioni Code First per distribuire entrambi. L'applicazione di esempio utilizza il sistema di appartenenze ASP.NET precedenti, che non può essere distribuito utilizzando migrazioni Code First. Le procedure per la distribuzione di questo database delle appartenenze si applicano anche a qualsiasi altro scenario in cui l'applicazione deve distribuire un database di SQL Server che non viene creato da Entity Framework Code First.


Di seguito, in genere non si desidera gli stessi dati nell'ambiente di produzione che è necessario in fase di sviluppo. Quando si distribuisce un sito per la prima volta, è comune per escludere la maggior parte o tutti gli account utente creati per il test. Pertanto, il progetto scaricato è disponibili due database di appartenenza: *aspnet ContosoUniversity.mdf* con gli utenti di sviluppo e *aspnet-ContosoUniversity-Prod.mdf* con gli utenti di produzione. Per questa esercitazione, i nomi utente sono gli stessi in entrambi i database: *admin* e *nonadmin*. Gli utenti dispongono di password *devpwd* nel database di sviluppo e *prodpwd* nel database di produzione.

Gli utenti di sviluppo verrà distribuita per l'ambiente di test e gli utenti di produzione di gestione temporanea e produzione. A tale scopo si creeranno due script SQL in questa esercitazione, uno per lo sviluppo e uno per la produzione e nelle esercitazioni successive si configurerà il processo di pubblicazione per eseguirli.

> [!NOTE]
> Il database delle appartenenze archivia un hash della password dell'account. Per distribuire gli account da un computer a un altro, è necessario assicurarsi che la routine hash non generano hash diverso nel server di destinazione rispetto a nel computer di origine. Gli hash stesso verrà generato quando si utilizza ASP.NET Universal Providers, purché non modificare l'algoritmo predefinito. L'algoritmo predefinito è HMACSHA256 e viene specificato nella **convalida** attributo del  **[machineKey](https://msdn.microsoft.com/en-us/library/system.web.configuration.machinekeysection.aspx)**  elemento nel file Web. config.


È possibile creare script di distribuzione di dati manualmente, utilizzando SQL Server Management Studio (SSMS) o tramite uno strumento di terze parti. Il resto di questa esercitazione viene illustrato come eseguire questa operazione in SQL Server Management Studio, ma se non si desidera installare e utilizzare SQL Server Management Studio è possibile richiamare gli script dalla versione completata del progetto e passare alla sezione in cui vengono archiviati nella cartella della soluzione.

Per installare SQL Server Management Studio, installarlo da [area Download: Microsoft SQL Server 2012 Express](https://www.microsoft.com/en-us/download/details.aspx?id=29062) facendo [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) o [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Se si scegliere quello corretto per il sistema non riuscirà installare e sarà possibile provare l'altro.

(Si noti che questo è un download di 600 MB. Potrebbe richiedere molto tempo per installare e sarà necessario un riavvio del computer in uso.)

Nella prima pagina di Centro installazione SQL Server, fare clic su **nuova installazione SQL Server autonomo o aggiungere funzionalità a un'installazione esistente**e seguire le istruzioni, accettando i valori predefiniti.

### <a name="create-the-development-database-script"></a>Creare lo script di database di sviluppo

1. Eseguire SQL Server Management Studio.
2. Nel **Connetti al Server** finestra di dialogo immettere *(localdb) \v11.0* come il **nome Server**, lasciare **autenticazione** impostato su **L'autenticazione di Windows**, quindi fare clic su **Connetti**.

    ![SQL Server Management Studio connettersi al Server](preparing-databases/_static/image10.png)
3. Nel **Esplora oggetti** finestra, espandere **database**, fare doppio clic su **aspnet ContosoUniversity**, fare clic su **attività**, quindi fare clic su **Generare script**.

    ![SQL Server Management Studio genera script](preparing-databases/_static/image11.png)
4. Nel **genera e pubblica script** la finestra di dialogo, fare clic su **Imposta opzioni di Scripting**.

    È possibile ignorare il **Seleziona oggetti** passaggio perché il valore predefinito è **genera Script per intero database e tutti gli oggetti di database** e che sia quella desiderata.
5. Scegliere **Avanzate**.

    ![Opzioni di generazione script di SQL Server Management Studio](preparing-databases/_static/image12.png)
6. Nel **opzioni di Scripting avanzate** la finestra di dialogo, scorrere fino a **tipi di dati da script**, fare clic sul **solo dati** opzione nell'elenco a discesa.
7. Modifica **Script per USE DATABASE** a **False**. Usa istruzioni non sono valide per il Database SQL di Azure e che non sono necessari per la distribuzione in SQL Server Express nell'ambiente di test.

    ![SQL Server Management Studio Script solo dati, alcuna istruzione USE](preparing-databases/_static/image13.png)
8. Fare clic su **OK**.
9. Nel **genera e pubblica script** nella finestra di dialogo di **nome File** casella specifica in cui verrà creato lo script. Modificare il percorso alla cartella della soluzione (la cartella contenente il file ContosoUniversity.sln) e il nome di file per *aspnet-data-dev.sql*.
10. Fare clic su **Avanti** per passare al **riepilogo** scheda e quindi fare clic su **Avanti** nuovamente per creare lo script.

    ![Creata Script di SQL Server Management Studio](preparing-databases/_static/image14.png)
11. Scegliere **Fine**.

### <a name="create-the-production-database-script"></a>Creare lo script di database di produzione

Poiché si non esegue il progetto con il database di produzione, non è associata ancora per l'istanza di LocalDB. Pertanto, è necessario collegare prima il database.

1. In SSMS **Esplora oggetti**, fare doppio clic su **database** e fare clic su **collegamento**.

    ![Connettersi a SQL Server Management Studio](preparing-databases/_static/image15.png)
- Nel **Collega database** la finestra di dialogo, fare clic su **Aggiungi** e quindi passare al *aspnet-ContosoUniversity-Prod.mdf* file nel *App\_ Dati* cartella.

    ![SQL Server Management Studio aggiungere file con estensione mdf per collegare](preparing-databases/_static/image16.png)
- Fare clic su **OK**.
- Seguire la stessa procedura utilizzata in precedenza per creare uno script per il file di produzione. Nome del file di script *aspnet-data-prod.sql*.

## <a name="summary"></a>Riepilogo

Entrambi i database sono ora pronti per essere distribuito e si dispone di due script di distribuzione di dati nella cartella della soluzione.

![Script di distribuzione di dati](preparing-databases/_static/image17.png)

Nell'esercitazione seguente configurare le impostazioni di progetto che influiscono sulla distribuzione e impostare automatico *Web. config* file trasformazioni per le impostazioni che devono essere diverse nell'applicazione distribuita.

## <a name="more-information"></a>Altre informazioni

Per ulteriori informazioni su NuGet, vedere [gestire raccolte di progetto con NuGet](https://msdn.microsoft.com/en-us/magazine/hh547106.aspx) e [documentazione di NuGet](http://docs.nuget.org/docs/start-here/overview). Se non si desidera usare NuGet, è necessario apprendere come analizzare un pacchetto NuGet per determinare cosa accade quando viene installato. (Ad esempio, è possibile configurare *Web. config* trasformazioni, configurare gli script di PowerShell da eseguire in fase di compilazione e così via.) Per ulteriori informazioni sull'uso di NuGet, vedere [la creazione e pubblicazione di un pacchetto](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e [File di configurazione e le trasformazioni di codice sorgente](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

>[!div class="step-by-step"]
[Precedente](introduction.md)
[Successivo](web-config-transformations.md)
