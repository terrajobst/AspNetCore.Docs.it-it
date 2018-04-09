---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di database SQL Server Compact - 2 di 12 | Documenti Microsoft"
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 7e2d430bd8e07ed7d97d11a00c61d90beeac005f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di database SQL Server Compact - 2 di 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione di serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire l'App Web di servizio App di Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come impostare i due database di SQL Server Compact e il motore di database per la distribuzione.

Per l'accesso al database, l'applicazione Contoso University richiede il seguente software che deve essere distribuito con l'applicazione perché non è incluso in .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (motore di database).
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (che consentono il sistema di appartenenze ASP.NET utilizzare SQL Server Compact)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First con le migrazioni).

La struttura del database e alcune (non tutte) dei dati in due dell'applicazione devono essere distribuiti anche i database. In genere, quando si sviluppa un'applicazione, immettere i dati di test in un database che non si desidera distribuire in un sito in tempo reale. Tuttavia, è anche possibile immettere alcuni dati di produzione che si desidera distribuire. In questa esercitazione si configurerà il progetto University Contoso in modo che il software necessario e i dati corretti vengano inclusi quando si distribuisce.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact e SQL Server Express

L'applicazione di esempio utilizza SQL Server Compact 4.0. Questo motore di database è un'opzione relativamente nuova per siti Web; le versioni precedenti di SQL Server Compact non funzionano in un ambiente di hosting web. SQL Server Compact offre alcuni vantaggi rispetto allo scenario più comune di sviluppo con SQL Server Express e distribuzione completa di SQL Server. A seconda del provider di hosting che si sceglie, SQL Server Compact potrebbero essere più economica da distribuire, perché alcuni provider addebitano un costo aggiuntivo per supportare un database di SQL Server completo. Costi aggiuntivi per SQL Server Compact non infatti è possibile distribuire il motore di database come parte dell'applicazione web.

Tuttavia, inoltre è necessario tenere presenti alcune limitazioni. SQL Server Compact non supporta stored procedure, trigger, viste o replica. (Per un elenco completo delle funzionalità di SQL Server che non sono supportate da SQL Server Compact, vedere [le differenze tra SQL Server Compact e SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Inoltre, alcuni degli strumenti che è possibile utilizzare per gestire gli schemi e i dati in SQL Server Express e database di SQL Server non funzionano con SQL Server Compact. Ad esempio, è possibile utilizzare SQL Server Management Studio o SQL Server Data Tools in Visual Studio con i database di SQL Server Compact. Sono disponibili altre opzioni per l'utilizzo di database di SQL Server Compact:

- È possibile utilizzare Esplora Server in Visual Studio, che offre funzionalità di modifica database limitato per SQL Server Compact.
- È possibile utilizzare la funzionalità di modifica del database di [WebMatrix](https://www.microsoft.com/web/webmatrix/), che dispone di maggiori funzionalità rispetto a Esplora Server.
- È possibile utilizzare relativamente completa di terze parti o aprire Strumenti di origine, ad esempio il [strumenti di SQL Server Compact](https://github.com/ErikEJ/SqlCeToolbox) e [SQL Compact dati e schema script utilità](https://github.com/ErikEJ/SqlCeToolbox).
- È possibile scrivere ed eseguire script personalizzati per DDL (DDL) per modificare lo schema del database.

È possibile iniziare con SQL Server Compact e quindi eseguire l'aggiornamento in un secondo momento come evolversi delle esigenze. Le esercitazioni successive di questa serie viene illustrato come eseguire la migrazione da SQL Server Compact a SQL Server Express e SQL Server. Tuttavia, se si sta creando una nuova applicazione e si prevede di aver bisogno di SQL Server in un prossimo futuro, è preferibile iniziare con SQL Server o SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Il motore SQL Server Compact Database di configurazione per la distribuzione

È stato aggiunto il software necessario per l'accesso ai dati nell'applicazione Contoso University installando i pacchetti NuGet seguenti:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (provider universal ASP.NET)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

I collegamenti puntano alle versioni correnti di questi pacchetti, che potrebbero essere più recenti rispetto a ciò che viene installato nel progetto di avvio che per questa esercitazione è stato scaricato. Per la distribuzione in un provider di hosting, assicurarsi che si utilizza Entity Framework 5.0 o versione successiva. Versioni precedenti di migrazioni Code First richiedono l'attendibilità totale e molti provider di hosting, l'applicazione verrà eseguita in attendibilità Media. Per ulteriori informazioni sull'attendibilità media, vedere il [distribuzione in IIS come ambiente di Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) esercitazione.

Installazione dei pacchetti NuGet in genere si occupa di tutto ciò che occorre per distribuire questo software con l'applicazione. In alcuni casi, ciò comporta attività quali la modifica del file Web. config e l'aggiunta di script di PowerShell che vengono eseguiti ogni volta che si compila la soluzione. **Se si desidera aggiungere il supporto per queste funzionalità (ad esempio SQL Server Compact ed Entity Framework) senza usare NuGet, assicurarsi che il risultato è noto installazione dei pacchetti NuGet in modo che è possibile eseguire manualmente le stesse operazioni.**

Vi è un'eccezione in NuGet non prestare attenzione di tutto ciò che è necessario effettuare per garantire una corretta distribuzione. Il pacchetto SqlServerCompact NuGet aggiunge uno script di post-compilazione per il progetto in cui vengono copiati gli assembly nativi per *x86* e *amd64* le sottocartelle in un progetto *bin* cartella, ma lo script non include le cartelle nel progetto. Di conseguenza, distribuzione Web non verrà copiata li al sito web di destinazione a meno che non si manualmente includerli nel progetto. (Questo comportamento risultante dalla configurazione di distribuzione predefinita; non verrà usata in queste esercitazioni, è anche possibile modificare l'impostazione che controlla questo comportamento. È l'impostazione che è possibile modificare **solo i file necessari per eseguire l'applicazione** in **gli elementi da distribuire** sul **pubblicazione/creazione pacchetto Web** scheda della finestra di **progetto Proprietà** finestra. Modifica questa impostazione non in genere è consigliabile, perché ciò può impedire la distribuzione di molti file altre nell'ambiente di produzione di quelli necessari non esiste. Per ulteriori informazioni sulle alternative, vedere il [configurazione delle proprietà di progetto](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) esercitazione.)

Compilare il progetto, quindi nel **Esplora** fare clic su **Mostra tutti i file** se non è già stato fatto. Potrebbe inoltre essere necessario fare clic su **aggiornamento**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Espandere il **bin** cartella per visualizzare il **amd64** e **x86** cartelle, quindi selezionare le cartelle, pulsante destro del mouse e selezionare **Includi nel progetto**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Le icone di cartella cambiano per mostrare che la cartella è stato incluso nel progetto.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>La configurazione migrazioni Code First per la distribuzione di applicazioni Database

Quando si distribuisce un database dell'applicazione, in genere non sufficiente vengono distribuiti nel database di sviluppo con tutti i dati in essa contenuti nell'ambiente di produzione, perché la maggior parte dei dati in esso è probabilmente presente solo a scopo di test. Ad esempio, i nomi di studenti in un database di test sono fittizi. D'altra parte, è spesso possibile distribuire solo la struttura di database senza dati in essa contenuti affatto. Alcuni dei dati nel database di test potrebbe essere dati reali e deve essere presente quando gli utenti iniziano a usare l'applicazione. Ad esempio, il database potrebbe disporre di una tabella che contiene i valori del livello valido o nomi di reparto reale.

Per simulare questo scenario comune, è possibile configurare un metodo di inizializzazione migrazioni primo codice che inserisce nel database solo i dati che si desidera essere presenti nell'ambiente di produzione. Questo metodo di inizializzazione non inserisce i dati di test perché verrà eseguito nell'ambiente di produzione dopo Code First, viene creato il database nell'ambiente di produzione.

Nelle versioni precedenti di Code First prima del rilascio, migrazioni era comune per i metodi di inizializzazione per inserire i dati di test, inoltre, poiché con ogni modifica del modello durante lo sviluppo il database deve essere completamente eliminato e ricreato da zero. Con le migrazioni Code First, dati di test viene mantenuti dopo le modifiche del database, inclusi i dati di test nel metodo di inizializzazione non è necessario. Il progetto che è stato scaricato Usa il metodo di pre-migrazioni di inclusione di tutti i dati nel metodo di inizializzazione di una classe di inizializzatore. In questa esercitazione viene disabilitare la classe di inizializzatore e abilitare le migrazioni. Quindi si aggiornerà il metodo di inizializzazione nella classe di configurazione migrazioni in modo che venga inserita solo i dati che si desidera inserire nell'ambiente di produzione.

Il diagramma seguente illustra lo schema del database dell'applicazione:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Per queste esercitazioni, si presuppone che il `Student` e `Enrollment` tabelle devono essere vuote quando il sito viene innanzitutto distribuito. Le altre tabelle contengono dati che deve essere precaricata quando l'applicazione passa in tempo reale.

Poiché si utilizzando migrazioni Code First, non è più necessario utilizzare il **DropCreateDatabaseIfModelChanges** inizializzatore Code First. Il codice per questo inizializzatore è nel file SchoolInitializer.cs nel progetto ContosoUniversity.DAL. Un'impostazione di **appSettings** elemento del file Web. config determina l'inizializzatore per l'esecuzione ogni volta che l'applicazione tenta di accedere al database per la prima volta:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Aprire il file Web. config dell'applicazione e rimuovere l'elemento che specifica la classe di inizializzatore Code First dall'elemento appSettings. L'elemento appSettings è ora simile al seguente:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> È possibile specificare un inizializzatore di classe farlo chiamando `Database.SetInitializer` nel `Application_Start` metodo il *Global. asax* file. Se si siano abilitando le migrazioni in un progetto che utilizza tale metodo per specificare l'inizializzatore, rimuovere la riga di codice.


Successivamente, abilitare le migrazioni Code First.

Il primo passaggio è assicurarsi che il progetto ContosoUniversity è impostato come progetto di avvio. In **Esplora**, fare clic sul progetto ContosoUniversity e selezionare **imposta come progetto di avvio**. Migrazioni Code First avrà un aspetto del progetto di avvio per trovare la stringa di connessione di database.

Dal **strumenti** menu, fare clic su **Gestione pacchetti libreria** e quindi **Package Manager Console**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

Nella parte superiore del **Package Manager Console** finestra selezionare ContosoUniversity.DAL come progetto predefinito e quindi at il `PM>` prompt dei comandi immettere "enable-migrations".

![Enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Questo comando crea un *Configuration.cs* file in un nuovo *migrazioni* cartella nel progetto ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

È selezionato il progetto DAL perché il comando "enable-migrations" deve essere eseguito nel progetto che contiene la classe di contesto Code First. Quando tale classe è in un progetto libreria di classi, migrazioni Code First Cerca la stringa di connessione di database nel progetto di avvio per la soluzione. Nella soluzione ContosoUniversity, il progetto web è stato impostato come progetto di avvio. (Se non desidera designare il progetto che include la stringa di connessione come progetto di avvio in Visual Studio, è possibile specificare il progetto di avvio del comando di PowerShell. Per visualizzare la sintassi del comando per il comando enable-migrations, è possibile immettere il comando "get-help enable-migrations".)

Aprire il file Configuration.cs e sostituire i commenti di `Seed` (metodo) con il codice seguente:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

I riferimenti a `List` sono le righe ondulate rosse sottostanti perché non è un `using` ancora l'istruzione per lo spazio dei nomi. Fare doppio clic su una delle istanze di `List` e fare clic su **risolvere**, quindi fare clic su **using System.Collections.Generic**.

![Risolvere con l'istruzione using](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Questa opzione di menu consente di aggiungere il codice seguente per il `using` istruzioni nella parte superiore del file.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Aggiunta di codice per il `Seed` metodo è uno dei tanti metodi che è possibile inserire i dati nel database. In alternativa è possibile aggiungere codice per il `Up` e `Down` metodi di ogni classe di migrazione. Il `Up` e `Down` metodi contengono codice che implementa le modifiche del database. Esempi di in verrà visualizzato il [un aggiornamento del Database di distribuzione](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) esercitazione.
> 
> È anche possibile scrivere codice che esegue istruzioni SQL utilizzando il `Sql` metodo. Ad esempio, se si desidera inizializzare tutti i budget di reparto a $1.000,00 come parte di una migrazione sono stati aggiunta di una colonna di Budget per la tabella del reparto, è Impossibile aggiungere la riga seguenti di codice per il `Up` metodo per la migrazione:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> In questo esempio viene visualizzato per questa esercitazione viene utilizzato il `AddOrUpdate` metodo il `Seed` metodo le migrazioni Code First `Configuration` classe. Codice viene chiamato prima di migrazioni di `Seed` metodo dopo tutte le migrazioni e questo metodo aggiorna le righe che sono già state inserite o li inserisce se non esistono ancora. Il `AddOrUpdate` metodo potrebbe non essere la scelta ottimale per lo scenario. Per ulteriori informazioni, vedere [prestare attenzione con metodo AddOrUpdate di EF 4.3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sul blog di Julie Lerman.


Premere CTRL-MAIUSC-B per compilare il progetto.

Il passaggio successivo consiste nel creare un `DbMigration` classe per la migrazione iniziale. Desideri che la migrazione per creare un nuovo database, è necessario eliminare il database già esistente. Database di SQL Server Compact sono contenuti in *sdf* file il *App\_dati* cartella. In **Esplora**, espandere *App\_dati* nel progetto ContosoUniversity per visualizzare i due database di SQL Server Compact, che sono rappresentati da *sdf*file.

Fare doppio clic su di *School.sdf* file e fare clic su **eliminare**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

Nel **Package Manager Console** finestra, immettere il comando "migrazione aggiungere iniziale" per creare la migrazione iniziale e denominarla "Iniziale".

![add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migrazioni Code First consente di creare un altro file di classe di *migrazioni* cartella e questa classe contiene codice che crea lo schema del database.

Nel **Package Manager Console**, immettere il comando "update-database" per creare il database ed eseguire il **valore di inizializzazione** metodo.

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Se si verifica un errore che indica una tabella esiste già e non può essere creata, è probabilmente perché è stata eseguita l'applicazione dopo l'eliminazione di database e prima di eseguire `update-database`. Assume, eliminare il *School.sdf* file nuovo, quindi ripetere il `update-database` comando.)

Eseguire l'applicazione. Ora la pagina di studenti è vuota, ma la pagina istruttori contiene i docenti. Questo è ciò che verrà visualizzato nell'ambiente di produzione dopo aver distribuito l'applicazione.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Il progetto è ora possibile distribuire il *dell'istituto di istruzione* database.

## <a name="creating-a-membership-database-for-deployment"></a>Creazione di un Database di appartenenza per la distribuzione

L'applicazione Contoso University utilizza l'autenticazione di sistema e form di appartenenza ASP.NET per autenticare e autorizzare gli utenti. Una delle pagine è accessibile solo agli amministratori. Per visualizzare questa pagina, eseguire l'applicazione e selezionare **aggiornamento crediti** nel menu a comparsa in **corsi**. L'applicazione visualizza la **Accedi** pagina, perché solo gli amministratori sono autorizzati a utilizzare il **aggiornamento crediti** pagina.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Accedere come "admin" utilizzando la password "Pa$ w0rd" (si noti il numero zero al posto la lettera "a" in "w0rd"). Dopo l'accesso, il **aggiornamento crediti** viene visualizzata la pagina.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Quando si distribuisce un sito per la prima volta, è comune per escludere la maggior parte o tutti gli account utente creati per il test. In questo caso, verrà distribuita un account amministratore e nessun account utente. Anziché eliminare manualmente gli account di prova, si creerà un nuovo database di appartenenza che è solo un account utente amministratore che è necessario nell'ambiente di produzione.

> [!NOTE]
> Il database delle appartenenze archivia un hash della password dell'account. Per distribuire gli account da un computer a un altro, è necessario assicurarsi che la routine hash non generano hash diverso nel server di destinazione rispetto a nel computer di origine. Gli hash stesso verrà generato quando si utilizza ASP.NET Universal Providers, purché non modificare l'algoritmo predefinito. L'algoritmo predefinito è HMACSHA256 e viene specificato nella **convalida** attributo del **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** elemento nel file Web. config.


Database delle appartenenze non è gestito da migrazioni Code First e non vi è alcun inizializzatore automatica che esegue il seeding del database con gli account di prova (quanto accade per il database School). Pertanto, per mantenere i dati di test disponibile ti una copia del database di test prima di creare una nuova.

In **Esplora**, rinominare il *aspnet.sdf* file nel *App\_dati* cartella *aspnet Dev.sdf*. (Non effettuare una copia, rinominarla appena, si creerà un nuovo database in proposito.)

In **Esplora**, assicurarsi che sia selezionato il progetto web (ContosoUniversity, non ContosoUniversity.DAL). Quindi nel **progetto** dal menu **configurazione ASP.NET** per eseguire il **strumento Amministrazione sito Web**(WAT).

Selezionare il **sicurezza** scheda.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Fare clic su **crea o Gestisci ruoli** e aggiungere un **amministratore** ruolo.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Tornare al **sicurezza** scheda, fare clic su **Create User**, e aggiungere l'utente "admin" come amministratore. Prima di scegliere il **Create User** pulsante il **Create User** pagina, assicurarsi di selezionare il **amministratore** casella di controllo. La password utilizzata in questa esercitazione è "Pa$ w0rd", ed è possibile immettere qualsiasi indirizzo di posta elettronica.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Chiudere il browser. In **Esplora**, fare clic sul pulsante Aggiorna per visualizzare il nuovo *aspnet.sdf* file.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Fare doppio clic su **aspnet.sdf** e selezionare **Includi nel progetto**.

## <a name="distinguishing-development-from-production-databases"></a>Sviluppo distintivo dal database di produzione

In questa sezione, si verranno rinominare i database in modo che le versioni di sviluppo dell'istituto di istruzione Dev.sdf aspnet Dev.sdf e le versioni di produzione sono dell'istituto di istruzione Prod.sdf e aspnet Prod.sdf. Non necessario, ma tale così consentirà di evitare di confondere l'acquisizione di versioni di test e produzione dei database.

In **Esplora**, fare clic su **aggiornamento** ed espandere l'applicazione\_cartella di dati per vedere il database School creato in precedenza; pulsante destro del mouse e selezionare **Includi nel progetto** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Rinominare *aspnet.sdf* a *aspnet Prod.sdf*.

Rinominare *School.sdf* a *dell'istituto di istruzione-Dev.sdf*.

Quando si esegue l'applicazione in Visual Studio non si desidera utilizzare il *-Prod* versioni dei file di database, si desidera utilizzare *- Dev* versioni. Pertanto è necessario modificare le stringhe di connessione nel file Web. config in modo che facciano riferimento il *- Dev* versioni dei database. (È ancora stato creato un file dell'istituto di istruzione Prod.sdf, ma va bene perché Code First consente di creare il database in produzione la prima esecuzione di app non esiste).

Aprire il file Web. config dell'applicazione e individuare le stringhe di connessione:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Modificare "aspnet.sdf" a "aspnet Dev.sdf" e "School.sdf" a "Dell'istituto di istruzione Dev.sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Entrambi i database e motore di database SQL Server Compact sono ora pronti per essere distribuito. Nell'esercitazione seguente impostare automatico *Web. config* file trasformazioni per le impostazioni che devono essere diverse in ambienti di sviluppo, test e produzione. (Tra le impostazioni che devono essere modificate sono le stringhe di connessione, ma si imposterà le modifiche in un secondo momento quando si crea un profilo di pubblicazione).

## <a name="more-information"></a>Altre informazioni

Per ulteriori informazioni su NuGet, vedere [gestire raccolte di progetto con NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) e [documentazione di NuGet](http://docs.nuget.org/docs/start-here/overview). Se non si desidera usare NuGet, è necessario apprendere come analizzare un pacchetto NuGet per determinare cosa accade quando viene installato. (Ad esempio, è possibile configurare *Web. config* trasformazioni, configurare gli script di PowerShell da eseguire in fase di compilazione e così via.) Per ulteriori informazioni sull'uso di NuGet, vedere in particolare [la creazione e pubblicazione di un pacchetto](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e [File di configurazione e le trasformazioni di codice sorgente](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
