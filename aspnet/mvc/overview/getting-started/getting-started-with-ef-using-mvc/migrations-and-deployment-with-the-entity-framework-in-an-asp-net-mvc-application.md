---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: Il codice prima di tutto le migrazioni e distribuzione con Entity Framework in un'applicazione MVC ASP.NET | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Code First di Entity Framework 6 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 04d393edca0469df140f06a7d083a48aa8f84b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Il codice prima di tutto le migrazioni e distribuzione con Entity Framework in un'applicazione MVC ASP.NET
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Visual Studio 2013 e Code First di Entity Framework 6. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Fino a questo punto l'applicazione è stato in esecuzione in locale in IIS Express nel computer di sviluppo. Per rendere disponibile ad altri utenti di utilizzare in Internet un'applicazione reale, è necessario distribuirla in un provider di hosting web. In questa esercitazione verrà distribuita l'applicazione Contoso University al cloud in Azure.

L'esercitazione include le sezioni seguenti:

- Abilitare le migrazioni Code First. La funzionalità di migrazione consente di modificare il modello di dati e distribuire le modifiche nell'ambiente di produzione, aggiornare lo schema del database senza dover eliminare e ricreare il database.
- Distribuire in Azure. Questo passaggio è facoltativo. è possibile continuare con le esercitazioni rimanenti senza avere distribuito il progetto.

È consigliabile utilizzare un processo di integrazione continua con controllo del codice sorgente per la distribuzione, ma in questa esercitazione non copre gli argomenti. Per ulteriori informazioni, vedere il [controllo del codice sorgente](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) e [integrazione continua](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) capitoli del [creazione di App per Cloud del mondo reale con Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction) e-book.

## <a name="enable-code-first-migrations"></a>Abilitare le migrazioni Code First

Quando si sviluppa una nuova applicazione, il modello di dati cambia di frequente e, a ogni cambiamento, non è più sincronizzato con il database. È stato configurato per automaticamente, eliminare e ricreare il database ogni volta che si modifica il modello di dati Entity Framework. Quando aggiungere, rimuovere, o modificare le classi di entità o modificare la `DbContext` classe, alla successiva esecuzione dell'applicazione viene automaticamente Elimina il database esistente, si crea una nuova istanza che corrisponde al modello e si esegue il seeding con dati di test.

Questo metodo che consiste nel mantenere il database sincronizzato con il modello di dati funziona bene fino a quando non si distribuisce l'applicazione nell'ambiente di produzione. Quando l'applicazione è in esecuzione nell'ambiente di produzione, in genere l'archiviazione dei dati che si desidera mantenere, e non si desidera perdere tutti gli elementi ogni volta che si apporta una modifica ad esempio l'aggiunta di una nuova colonna. Il [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) funzionalità risolve questo problema, l'abilitazione di Code First aggiornare lo schema del database anziché eliminare e ricreare il database. In questa esercitazione, l'applicazione verrà distribuita e preparazione per che verranno abilitate le migrazioni.

1. Disabilitare l'inizializzatore che è impostato in precedenza come commento o eliminando il `contexts` elemento che è stato aggiunto al file Web. config dell'applicazione.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Anche nell'applicazione *Web. config* file, modificare il nome del database nella stringa di connessione in ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Questa modifica configura il progetto in modo che la prima migrazione crei un nuovo database. Questo non è necessario ma verrà visualizzato in un secondo momento perché è una buona idea.
3. Dal **strumenti** menu, fare clic su **Gestione pacchetti libreria** e quindi **Package Manager Console**.

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. Nel `PM>` prompt dei comandi digitare i comandi seguenti:

    `enable-migrations`  
    `add-migration InitialCreate`

    ![comando Enable-migrations](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    Il `enable-migrations` comando crea un *migrazioni* cartella nel progetto ContosoUniversity e inserisce in questa cartella un *Configuration.cs* file che è possibile modificare per configurare le migrazioni.

    (Se si perde il passaggio precedente che indirizza l'utente di modificare il nome del database, le migrazioni verranno trovare il database esistente e di eseguire automaticamente il `add-migration` comando. OK, significa semplicemente che si non esegue un test del codice migrazioni prima di distribuire il database. In seguito quando si esegue il `update-database` comando non accade nulla perché il database esiste già.)

    ![Cartella Migrations](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Analogamente alla classe inizializzatore che illustrato in precedenza, il `Configuration` classe include un `Seed` metodo.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Lo scopo del [valore di inizializzazione](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodo consiste nella possibilità di inserire o aggiornare i dati di test dopo il primo codice crea o aggiorna il database. Il metodo viene chiamato quando viene creato il database e ogni volta che viene aggiornato lo schema del database dopo la modifica di un modello di dati.

### <a name="set-up-the-seed-method"></a>Impostare il metodo di inizializzazione

Se si elimina e ricreare il database per ogni modifica del modello di dati, si utilizza la classe di inizializzatore `Seed` per inserire i dati di test, poiché dopo ogni modifica del modello viene eliminato il database e tutti i dati di test viene perso. Con le migrazioni Code First, test, i dati vengono mantenuti dopo le modifiche del database, pertanto l'inclusione di dati di test nel [valore di inizializzazione](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodo non è in genere necessario. Infatti, non si desidera il `Seed` per inserire i dati di test se sarà possibile usare le migrazioni per distribuire il database nell'ambiente di produzione, perché il `Seed` metodo viene eseguito nell'ambiente di produzione. In tal caso è necessario il `Seed` per inserire nel database solo i dati che è necessario nell'ambiente di produzione. Ad esempio, è il database per includere i nomi di reparto effettivo nel `Department` tabella quando l'applicazione diventa disponibile nell'ambiente di produzione.

Per questa esercitazione, sarà possibile usare le migrazioni per la distribuzione, ma il `Seed` metodo inserirà comunque i dati di test per renderlo più semplice vedere come utilizzare la funzionalità dell'applicazione senza dover inserire manualmente una grande quantità di dati.

1. Sostituire il contenuto del *Configuration.cs* file con il codice seguente, che verrà caricati i dati di test nel nuovo database. 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Il [valore di inizializzazione](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodo accetta l'oggetto di contesto di database come parametro di input e il codice nel metodo utilizza tale oggetto per aggiungere nuove entità nel database. Per ogni tipo di entità, il codice crea una raccolta di nuove entità, li aggiunge alla appropriata [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) proprietà e quindi Salva le modifiche al database. Non è necessario chiamare il [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metodo dopo ogni gruppo di entità, come avviene, ma questa operazione consente di individuare l'origine di un problema se si verifica un'eccezione mentre il codice è la scrittura nel database.

    Alcune istruzioni che inseriscono dati utilizzare il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo per eseguire un'operazione "upsert". Poiché il `Seed` metodo viene eseguito ogni volta che si esegue il `update-database` comando, in genere dopo la migrazione di ogni tipo, è possibile inserire solo dati, in quanto le righe che si sta tentando di aggiungere già sarà disponibile dopo la migrazione prima che crea il database. L'operazione "upsert" impedisce gli errori che accadrebbe se si tenta di inserire una riga già esistente, ma ***esegue l'override*** qualsiasi modifica ai dati apportate durante il test dell'applicazione. I dati di test in alcune tabelle è possibile evitare di raggiungere tale obiettivo: in alcuni casi quando si modificano i dati durante il test le proprie modifiche rimangano dopo gli aggiornamenti del database. In questo caso si desidera eseguire un'operazione di inserimento condizionale: inserire una riga solo se non esiste già. Il metodo di inizializzazione utilizza entrambi gli approcci.

    Il primo parametro passato per il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo consente di specificare la proprietà da utilizzare per verificare se esiste già una riga. Per i dati di test dello studente che si desidera fornire, il `LastName` proprietà può essere utilizzata per questo scopo poiché ogni cognome nell'elenco è univoco:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Questo codice presuppone che i cognomi siano univoci. Se si aggiunge manualmente uno studente con un nome duplicato ultimo, si otterrà la seguente eccezione alla successiva che si eseguire la migrazione.

    La sequenza contiene più di un elemento

    Per informazioni sulle modalità di gestione di dati ridondanti, ad esempio due studenti denominati "Alexander Carson", vedere [Seeding e database di debug di Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) sul blog di Rick Anderson. Per ulteriori informazioni sul `AddOrUpdate` metodo, vedere [prestare attenzione con metodo AddOrUpdate di EF 4.3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sul blog di Julie Lerman.

    Il codice che crea `Enrollment` entità si suppone che il `ID` valore nelle entità nel `students` raccolta, anche se non è stata impostata nel codice che crea la raccolta.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    È possibile utilizzare il `ID` proprietà perché il `ID` valore viene impostato quando si chiama `SaveChanges` per il `students` insieme. EF ottiene automaticamente il valore di chiave primario quando inserisce un'entità nel database e aggiorna il `ID` proprietà dell'entità in memoria.

    Il codice che aggiunge ogni `Enrollment` entità per il `Enrollments` set di entità non usa il `AddOrUpdate` (metodo). Controlla se esiste già un'entità e inserisce l'entità se non esiste. Questo approccio consente di mantenere le modifiche apportate a un livello di registrazione tramite l'interfaccia utente dell'applicazione. Il codice scorre ogni membro del `Enrollment` [elenco](https://msdn.microsoft.com/library/6sh2ey19.aspx) e se la registrazione non viene trovata nel database, viene aggiunta la registrazione al database. La prima volta che si aggiorna il database, il database sarà vuoto, in modo che aggiungerà ogni registrazione.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. Compilare il progetto.

### <a name="execute-the-first-migration"></a>Eseguire la migrazione prima

Quando è stata eseguita la `add-migration` comando migrazioni ha generato il codice che comporterebbero la creazione del database da zero. Questo codice è disponibile anche nella *migrazioni* cartella, nel file denominato  *&lt;timestamp&gt;\_InitialCreate.cs*. Il `Up` metodo il `InitialCreate` classe crea le tabelle di database che corrispondono ai set di entità del modello di dati, e `Down` metodo li elimina.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Le migrazioni chiamano il metodo `Up` per implementare le modifiche al modello di dati per una migrazione. Quando si immette un comando per annullare l'aggiornamento, le migrazioni chiamano il metodo `Down`.

Questa è la migrazione iniziale creato al momento dell'immissione di `add-migration InitialCreate` comando. Il parametro (`InitialCreate` nell'esempio) viene utilizzato per il file di nome e può essere qualsiasi, si sceglie in genere una parola o frase che riepiloga le quali viene eseguita la migrazione. Ad esempio, è possibile denominare una migrazione successiva &quot;AddDepartmentTable&quot;.

Se la migrazione iniziale è stata creata quando il database esisteva già, il codice di creazione del database viene generato ma non è necessario eseguirlo perché il database corrisponde già al modello di dati. Quando si distribuisce l'app in un altro ambiente in cui il database non esiste ancora, questo codice verrà eseguito per creare il database, è consigliabile quindi testarlo prima. Ecco perché in precedenza è stato modificato il nome del database nella stringa di connessione: per far sì che le migrazioni possano crearne uno nuovo da zero.

1. Nel **Package Manager Console** finestra, immettere il comando seguente:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Il `update-database` comando viene eseguito il `Up` metodo per creare il database e quindi si esegue il `Seed` metodo per popolare il database. Lo stesso processo verrà eseguito automaticamente nell'ambiente di produzione dopo aver distribuito l'applicazione, come illustrato nella sezione seguente.
2. Utilizzare **Esplora Server** per controllare il database, come indicato nella prima esercitazione ed eseguire l'applicazione per verificare che tutto funzioni ancora lo stesso come in precedenza.

## <a name="deploy-to-azure"></a>Distribuire in Azure

Fino a questo punto l'applicazione è stato in esecuzione in locale in IIS Express nel computer di sviluppo. Per renderlo disponibile ad altri utenti per l'utilizzo su Internet, è necessario distribuirla in un provider di hosting web. In questa sezione dell'esercitazione verrà distribuirla in Azure. In questa sezione è facoltativa. è possibile ignorare questo passaggio e continuare con l'esercitazione seguente oppure è possibile adattare le istruzioni in questa sezione per un provider di hosting diverso di propria scelta.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Utilizzare migrazioni Code First per distribuire il Database

Per distribuire il database utilizzare migrazioni Code First. Quando si crea il profilo di pubblicazione che consente di configurare le impostazioni per la distribuzione da Visual Studio, si selezionano una casella di controllo **aggiornamento Database**. Questa impostazione, il processo di distribuzione configurare automaticamente l'applicazione *Web. config* file nel server di destinazione in modo che usa Code First di `MigrateDatabaseToLatestVersion` classe inizializzatore.

Visual Studio non esegue alcuna operazione con il database durante il processo di distribuzione mentre viene copiato il progetto nel server di destinazione. Quando si esegue l'applicazione distribuita e accede al database per la prima volta dopo la distribuzione, il primo codice controlla se il database corrisponde al modello di dati. Se è presente una mancata corrispondenza, Code First crea automaticamente il database (se non esiste ancora) o aggiorna lo schema del database alla versione più recente (se esiste un database, ma non corrisponde al modello). Se l'applicazione implementa un migrazioni `Seed` metodo, l'esecuzione del metodo dopo la creazione del database o lo schema viene aggiornato.

Le migrazioni `Seed` metodo inserisce i dati di test. Se sono stati distribuito in un ambiente di produzione, è necessario modificare il `Seed` metodo in modo che venga inserito solo dati che si desidera essere inseriti nel database di produzione. Ad esempio, nel modello di dati corrente è potrebbe desidera disporre corsi reali ma studenti fittizi nel database di sviluppo. È possibile scrivere un `Seed` metodo per caricare entrambe in fase di sviluppo e quindi impostare come commento studenti fittizi prima di distribuire nell'ambiente di produzione. In alternativa è possibile scrivere un `Seed` metodo per caricare solo i corsi e immettere gli studenti fittizi manualmente nel database di prova tramite interfaccia utente dell'applicazione.

### <a name="get-an-azure-account"></a>Ottenere un account di Azure

È necessario un account di Azure. Se si dispone già di uno, ma si dispone di una sottoscrizione di Visual Studio, è possibile [attivare i vantaggi dell'abbonamento](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). In caso contrario, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Creare un sito web e un database SQL in Azure

L'app web in Azure viene eseguita in un ambiente di hosting condiviso, ovvero che viene eseguito su macchine virtuali (VM) che vengono condivisi con altri client di Azure. Un ambiente di hosting condiviso è una soluzione a basso costo per iniziare nel cloud. In un secondo momento, se il traffico web aumenta, l'applicazione scalabile per soddisfare le esigenze di mediante l'esecuzione su macchine virtuali dedicate. Per ulteriori informazioni sulle opzioni di prezzi per il servizio App di Azure, leggere la documentazione su [documenti di Azure](https://azure.microsoft.com/pricing/details/app-service/)

Distribuire il database per Database SQL di Azure. Database SQL è un servizio di database relazionale basato su cloud sviluppato su tecnologie di SQL Server. Strumenti e applicazioni che funzionano con SQL Server funzionano anche con il Database SQL.

1. Nel [portale di gestione di Azure](https://portal.azure.com), fare clic su **nuovo** nella scheda di sinistra, fare clic su **tutti** nuovo pannello e quindi fare clic su **App Web di & SQL** nel **Web** sezione e infine **crea**.

    ![Pulsante nuovo nel portale di gestione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   Il **nuova App Web e SQL - creare** apre la procedura guidata.

2. Nel pannello, immettere una stringa di **nome App** casella da usare come URL univoco per l'applicazione. L'URL completo sarà costituito da quelle immesse qui oltre il dominio predefinito di servizi di App di Azure (. azurewebsites.net). Se il **nome App** è già in uso, la procedura guidata notificherà all'utente di questo con rossa *il nome dell'applicazione non è disponibile* messaggio. Se il **nome App** è disponibile, verrà visualizzato un segno di spunta verde.

    ![Creare con il collegamento di Database nel portale di gestione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. Nel **sottoscrizione** elenco a discesa, scegliere la sottoscrizione di Azure in cui si desidera il **servizio App** risiedano.

4. Nel **gruppo di risorse** casella di testo, scegliere un gruppo di risorse o crearne uno nuovo. Questa impostazione specifica quale data center in cui verrà eseguito il sito web. Per ulteriori informazioni sui gruppi di risorse, leggere la documentazione su [documenti Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
5. Creare un nuovo **piano di servizio App** facendo il *sezione servizio App*, **Crea nuovo**e compilare **piano di servizio App** (può essere stesso nome Servizio App), **percorso**, e **tariffario** (è disponibile un'opzione disponibile).

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. Fare clic su di **Database SQL**e scegliere *Crea nuovo* o seleziona un database esistente

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. Nel **nome** , immettere un nome per il database.
8. Fare clic su di **Server di destinazione** , quindi selezionare **creare un nuovo server**. In alternativa, se un server è stato creato in precedenza, è possibile selezionare il server dall'elenco dei server disponibili.
9. Scegliere **tariffario** , scegliere *libero*. Se sono necessarie risorse aggiuntive, il database supporta la scalabilità verticale in qualsiasi momento. Per ulteriori informazioni sui prezzi di SQL Azure, leggere la documentazione su [documenti Azure](https://azure.microsoft.com/pricing/details/sql-database/).
10. Modificare [delle regole di confronto](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support) in base alle esigenze.
11. Immettere un amministratore **nome utente amministratore SQL** e **Password amministratore SQL**. Se si seleziona **server nuovo Database SQL**, non immettere un nome esistente e la password in questo caso, si sta immettendo un nuovo nome e una password che si sta definendo ora da utilizzare in un secondo momento quando si accede al database. Se si seleziona un server in cui è stato creato in precedenza, si immetteranno le credenziali per il server.
12. Raccolta di dati di telemetria può essere abilitata per il servizio App con Application Insights. Application Insights con una configurazione minima raccoglie eventi importanti, eccezioni, dipendenze, richiesta e informazioni di traccia. Per ulteriori informazioni su Application Insights, iniziare a utilizzare [documenti Azure](https://azure.microsoft.com/services/application-insights/).
13. Fare clic su **crea** nella parte inferiore del pannello per indicare che si è finito.
  
    Il portale di gestione torna alla pagina dashboard e **notifiche** pannello nella parte superiore della pagina mostra che il sito viene creato. Dopo un periodo di tempo (in genere inferiore a un minuto), sarà presente una notifica che la distribuzione ha avuto esito positivo. Nella barra di spostamento a sinistra, il nuovo **servizio App** viene visualizzato nel *servizi App* sezione e il nuovo **Database SQL** viene visualizzato nel *database SQL*  sezione.

### <a name="deploy-the-application-to-azure"></a>Distribuire l'applicazione in Azure

1. In Visual Studio, fare clic sul progetto in **Esplora** e selezionare **pubblica** dal menu di scelta rapida.
  
    ![Pubblicare nel menu di scelta rapida progetto](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. Nel **profilo** scheda della finestra di **pubblica sul Web** procedura guidata, fare clic su **servizio App di Microsoft Azure**.
  
    ![Importazione delle impostazioni di pubblicazione.](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. Se non è stato aggiunto in precedenza la sottoscrizione di Azure in Visual Studio, eseguire i passaggi sullo schermo. Questi passaggi consentono di Visual Studio per connettersi alla sottoscrizione di Azure in modo che l'elenco di **servizi App** includerà il sito web.
 
4. Selezionare il **sottoscrizione** aggiunto il servizio App, quindi il **piano di servizio App** cartella del servizio App è una parte, e infine il **servizio App** stesso seguita da **Ok**.

    ![Selezionare servizio App](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. Dopo aver configurato il profilo, il **connessione** scheda verrà visualizzata. Fare clic su **convalida connessione** per assicurarsi che le impostazioni siano corrette

    ![Convalidare la connessione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. Quando la connessione è stata convalidata, un segno di spunta verde viene visualizzato accanto al **convalida connessione** pulsante. Scegliere **Avanti**.
  
    ![Connessione convalidata correttamente](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. Aprire il **stringa di connessione remota** elenco a discesa in **SchoolContext** e selezionare la stringa di connessione per il database è stato creato.
8. Selezionare **Aggiorna database**.

    ![Scheda Impostazioni](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    Questa impostazione, il processo di distribuzione configurare automaticamente l'applicazione *Web. config* file nel server di destinazione in modo che usa Code First di `MigrateDatabaseToLatestVersion` classe inizializzatore.
9. Scegliere **Avanti**.
10. Nel **anteprima** scheda, fare clic su **avviare Anteprima**.
  
    ![Pulsante StartPreview nella scheda Anteprima](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    La scheda Visualizza un elenco di file che verranno copiati nel server. La visualizzazione dell'anteprima non è necessario pubblicare l'applicazione, ma è una funzione utile conoscere. In questo caso, non occorre eseguire alcuna operazione con l'elenco di file che viene visualizzato. Alla successiva che si distribuisce questa applicazione, sarà solo i file che sono stati modificati in questo elenco.
    ![Output del file StartPreview](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. Fare clic su **Pubblica**.
    Visual Studio avvia il processo di copia dei file per il server Azure.
12. Il **Output** finestra Mostra intraprese le azioni di distribuzione e segnala il completamento corretto della distribuzione.
  
    ![Finestra di output corretta distribuzione di reporting](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. Durante la distribuzione ha esito positivo, il browser predefinito verrà aperta automaticamente per l'URL del sito web distribuito.
    L'applicazione creata è in esecuzione nel cloud. 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

A questo punto il *SchoolContext* database è stato creato nel Database di SQL Azure perché è stato selezionato **eseguire migrazioni Code First (esecuzione all'avvio dell'app)**. Il *Web. config* file nel sito web distribuito è stato modificato in modo che il [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inizializzatore viene eseguita la prima volta il codice legge o scrive dati nel database (che si sono verificati Se è selezionata la **studenti** scheda):

![](https://asp.net/media/4367421/mig.png)

Il processo di distribuzione creata anche una nuova stringa di connessione *(SchoolContext\_DatabasePublish*) per le migrazioni Code First da utilizzare per l'aggiornamento dello schema del database e il seeding del database.

![Stringa di connessione Database_Publish](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

È possibile trovare la versione distribuita del file Web. config nel computer in *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. È possibile accedere distribuito *Web. config* file tramite FTP. Per istruzioni, vedere [distribuzione Web ASP.NET utilizzando Visual Studio: distribuzione di un aggiornamento del codice](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Seguire le istruzioni che iniziano con "per utilizzare uno strumento FTP, sono necessari tre operazioni: l'URL di FTP, il nome utente e la password."

> [!NOTE]
> L'app web non implementa la sicurezza, in modo che tutti gli utenti che consente di trovare l'URL può modificare i dati. Per istruzioni su come proteggere il sito web, vedere [distribuire un'app protetta ASP.NET MVC con appartenenza, OAuth e il Database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). È possibile impedire che altri utenti tramite il sito tramite il portale di gestione di Azure o **Esplora Server** in Visual Studio per arrestare il sito.


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>Scenari di migrazione avanzate

Se si distribuisce un database eseguendo migrazioni automaticamente, come illustrato in questa esercitazione e si distribuisce in un sito web in esecuzione su più server, è possibile ottenere più server tenta di eseguire le migrazioni nello stesso momento. Le migrazioni sono atomiche, in modo che se due server tenta di eseguire la migrazione stesso, uno avrà esito positivo e l'altra avrà esito negativo (presupponendo che le operazioni non possono essere eseguite due volte). In questo scenario se si desidera evitare tali problemi, è possibile chiamare manualmente le migrazioni e impostare il proprio codice in modo che il problema interessa solo una volta. Per ulteriori informazioni, vedere [in esecuzione e Scripting le migrazioni da codice](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) sul blog di Miller Rowan e [Migrate.exe](https://msdn.microsoft.com/data/jj618307) (per l'esecuzione di migrazioni dalla riga di comando) in MSDN.

Per informazioni sugli altri scenari di migrazione, vedere [migrazioni Screencast serie](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Inizializzatori prima di codice

Nella sezione distribuzione è stato illustrato il [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inizializzatore in uso. Codice innanzitutto fornisce anche altri inizializzatori, tra cui [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (impostazione predefinita), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (che è utilizzata in precedenza) e [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Il `DropCreateAlways` inizializzatore può essere utile per impostare le condizioni per gli unit test. È anche possibile scrivere il propria inizializzatori ed è possibile chiamare un inizializzatore in modo esplicito se non si desidera attendere che l'applicazione legge o scrive nel database. Al momento in questa esercitazione viene scritto nel novembre 2013, è possibile utilizzare solo gli inizializzatori di creare e DropCreate prima di abilitare le migrazioni. Il team di Entity Framework sta lavorando su come rendere utilizzabile con le migrazioni anche questi inizializzatori.

Per ulteriori informazioni sugli inizializzatori, vedere [informazioni sui Database gli inizializzatori di Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) capitolo 6 del libro e [Entity Framework di programmazione: Code First](http://shop.oreilly.com/product/0636920022220.do) da Julie Lerman e Miller Rowan.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato spiegato come abilitare le migrazioni e distribuire l'applicazione. Nella prossima esercitazione verrà iniziare esaminando argomenti più avanzati espandendo il modello di dati.

Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione ed è stato possibile apportare miglioramenti. È inoltre possibile richiedere nuovi argomenti in [Mostra Me come con codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Sono disponibili collegamenti ad altre risorse di Entity Framework in [accesso ai dati ASP.NET - risorse](xref:whitepapers/aspnet-data-access-content-map).

> [!div class="step-by-step"]
> [Precedente](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [Successivo](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
