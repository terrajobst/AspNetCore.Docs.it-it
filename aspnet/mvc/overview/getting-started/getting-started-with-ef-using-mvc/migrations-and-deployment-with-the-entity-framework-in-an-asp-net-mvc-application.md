---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: Code First per le migrazioni e la distribuzione con Entity Framework in un'applicazione ASP.NET MVC | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 5 con Entity Framework 6 Code First e Visual Studio...
ms.author: riande
ms.date: 11/07/2014
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4c1f852bab5e8f77b35239c356d11b5058cbdaef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832190"
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Code First per le migrazioni e la distribuzione con Entity Framework in un'applicazione ASP.NET MVC
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 5 con Entity Framework 6 Code First e Visual Studio 2013. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Finora l'applicazione è stata in esecuzione in locale in IIS Express nel computer di sviluppo. Per rendere disponibile ad altri utenti per usare la rete Internet un'applicazione reale, è necessario distribuirla in un provider di hosting web. In questa esercitazione verrà distribuita l'applicazione Contoso University al cloud in Azure.

L'esercitazione include le sezioni seguenti:

- Abilitare migrazioni Code First. La funzionalità delle migrazioni consente di modificare il modello di dati e distribuire le modifiche nell'ambiente di produzione aggiornando lo schema del database senza dover eliminare e ricreare il database.
- Distribuire in Azure. Questo passaggio è facoltativo. è possibile continuare con le esercitazioni rimanenti senza avere distribuito il progetto.

È consigliabile che si utilizza un processo di continuous integration con controllo del codice sorgente per la distribuzione, ma questa esercitazione non illustra tali argomenti. Per altre informazioni, vedere la [controllo del codice sorgente](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) e [ricchissimo](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) capitoli del [creazione di App Cloud reali con Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction) eBook.

## <a name="enable-code-first-migrations"></a>Abilitare migrazioni Code First

Quando si sviluppa una nuova applicazione, il modello di dati cambia di frequente e, a ogni cambiamento, non è più sincronizzato con il database. Durante la configurazione di Entity Framework per automaticamente eliminare e ricreare il database ogni volta che si modifica il modello di dati. Quando aggiungere, rimuovere, o modificare le classi di entità o modificare i `DbContext` classe, alla successiva esecuzione dell'applicazione automaticamente consente di eliminare il database esistente, viene creata una nuova istanza che corrisponde al modello ed esegue il seeding con dati di test.

Questo metodo che consiste nel mantenere il database sincronizzato con il modello di dati funziona bene fino a quando non si distribuisce l'applicazione nell'ambiente di produzione. Quando l'applicazione è in esecuzione nell'ambiente di produzione, in genere archivia i dati che si desidera mantenere e non si vuole perdere tutto ad ogni volta che si apporta una modifica ad esempio l'aggiunta di una nuova colonna. Il [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) funzionalità risolve questo problema abilitando Code First aggiornare lo schema del database anziché dover eliminare e ricreare il database. In questa esercitazione si distribuirà l'applicazione e per preparare che è possibile abilitare migrazioni.

1. Disabilitare l'inizializzatore che è impostato in precedenza da impostare come commento o eliminare il `contexts` elemento che è stato aggiunto al file Web. config dell'applicazione.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Anche nell'applicazione *Web. config* file, modificare il nome del database nella stringa di connessione in ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Questa modifica configura il progetto in modo che la prima migrazione crei un nuovo database. Non è obbligatorio, ma capirà più avanti perché è una buona idea.
3. Dal **degli strumenti** menu, fare clic su **Library Package Manager** e quindi **Package Manager Console**.

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. Nel `PM>` prompt dei comandi immettere i comandi seguenti:

    `enable-migrations`  
    `add-migration InitialCreate`

    ![comando Enable-migrations](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    Il `enable-migrations` comando crea un *migrazioni* cartella nel progetto ContosoUniversity e lo inserisce in tale cartella un *Configuration.cs* file che è possibile modificare per configurare le migrazioni.

    (Se hai perso il passaggio precedente che indirizza l'utente per modificare il nome del database, migrazioni troveranno il database esistente ed eseguire automaticamente il `add-migration` comando. OK, significa semplicemente che è non verrà eseguito un test del codice di migrazioni prima di distribuire il database. In seguito quando si esegue il `update-database` comando non accade nulla perché il database esiste già.)

    ![Cartella migrazioni](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Analogamente alla classe inizializzatore che hai visto in precedenza, il `Configuration` classe include una `Seed` (metodo).

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Lo scopo del [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodo consiste nel consentire di inserire o aggiornare i dati di test dopo Code First crea o aggiorna il database. Il metodo viene chiamato quando viene creato il database e ogni volta che viene aggiornato lo schema del database dopo la modifica di un modello di dati.

### <a name="set-up-the-seed-method"></a>Configurare il metodo di inizializzazione

Se si elimina e ricreare il database per ogni modifica del modello di dati, si utilizza la classe di inizializzatore `Seed` metodo per inserire i dati di test, perché dopo ogni modifica del modello di database viene eliminato e tutti i dati di test viene perso. Con migrazioni Code First, test, i dati vengono conservati dopo le modifiche di database, pertanto, inclusi i dati di test nel [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) (metodo) non è in genere necessario. Infatti, non si desidera che il `Seed` metodo per inserire i dati di test se si useranno le migrazioni per distribuire il database nell'ambiente di produzione, perché il `Seed` metodo verrà eseguito nell'ambiente di produzione. In tal caso è necessario il `Seed` metodo da inserire nel database solo i dati che è necessario nell'ambiente di produzione. Ad esempio, è possibile il database da includere i nomi dei reparti effettivo nel `Department` tabella quando l'applicazione diventa disponibile nell'ambiente di produzione.

Per questa esercitazione, si useranno le migrazioni per la distribuzione, ma il `Seed` metodo inserirà comunque i dati di test per renderlo più semplice verificare il funzionamento delle funzionalità dell'applicazione senza dover inserire manualmente una grande quantità di dati.

1. Sostituire il contenuto del *Configuration.cs* file con il codice seguente, che verrà caricati i dati di test nel nuovo database. 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Il [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) accetta l'oggetto di contesto di database come parametro di input e il codice nel metodo Usa l'oggetto per aggiungere nuove entità nel database. Per ogni tipo di entità, il codice crea una raccolta di nuove entità, li aggiunge a appropriato [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) proprietà e quindi Salva le modifiche al database. Non è necessario chiamare il [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metodo dopo ogni gruppo di entità, come avviene in questo caso, ma questa operazione consente di individuare l'origine di un problema se si verifica un'eccezione mentre il codice è la scrittura nel database.

    Alcune istruzioni che inseriscono dati usare la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo per eseguire un'operazione "upsert". Poiché il `Seed` metodo viene eseguito ogni volta che si esegue il `update-database` comando, in genere dopo ogni migrazione, non è sufficiente inserire i dati, perché si sta provando ad aggiungere righe saranno già presenti dopo la migrazione prima che crea il database. L'operazione "upsert" impedisce gli errori che accadrebbe se si prova a inserire una riga già esistente, ma ***esegue l'override*** eventuali modifiche ai dati apportate durante il test dell'applicazione. I dati di test in alcune tabelle è possibile evitare di raggiungere tale obiettivo: in alcuni casi quando si modificano i dati durante il test per le proprie modifiche permangono dopo gli aggiornamenti del database. In questo caso si vuole eseguire un'operazione di inserimento condizionale: inserire una riga solo se non esiste già. Il metodo di inizializzazione Usa entrambi gli approcci.

    Il primo parametro passato per il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo consente di specificare la proprietà da utilizzare per verificare se esiste già una riga. Per i dati degli studenti di test che si desidera fornire, il `LastName` proprietà può essere utilizzata per questo scopo poiché ogni cognome nell'elenco è univoco:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Questo codice si presuppone che i cognomi univoci. Se si aggiunge manualmente uno studente con un nome duplicato ultimo, si otterrà l'eccezione seguente la volta successiva che si esegue una migrazione.

    La sequenza contiene più di un elemento

    Per informazioni su come gestire i dati ridondanti, ad esempio due studenti denominati "Alexander Carson", vedere [Seeding e al database di debug di Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) sul blog di Rick Anderson. Per altre informazioni sul `AddOrUpdate` metodo, vedere [prestare attenzione con metodo AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sul blog di Julie.

    Il codice che crea `Enrollment` entità si presuppone il `ID` valore nelle entità nel `students` raccolta, anche se si non imposta tale proprietà nel codice che crea la raccolta.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    È possibile usare il `ID` proprietà qui perché il `ID` è impostato quando si chiama `SaveChanges` per il `students` raccolta. Entity Framework ottiene automaticamente il valore della chiave primaria quando inserisce un'entità nel database e aggiorna il `ID` proprietà dell'entità in memoria.

    Il codice che aggiunge ognuno `Enrollment` entità per il `Enrollments` set di entità non usa il `AddOrUpdate` (metodo). Controlla se esiste già un'entità e inserisce l'entità se non esiste. Questo approccio consente di mantenere le modifiche apportate a un livello di registrazione usando l'interfaccia utente dell'applicazione. Il codice scorre in ciclo ogni membro del `Enrollment` [elenco](https://msdn.microsoft.com/library/6sh2ey19.aspx) e se la registrazione non viene trovata nel database, aggiunge la registrazione al database. La prima volta che si aggiorna il database, il database sarà vuoto, in modo che verranno aggiunti ogni registrazione.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. Compilare il progetto.

### <a name="execute-the-first-migration"></a>Eseguire la prima migrazione

Quando è stato eseguito il `add-migration` comando, le migrazioni ha generato il codice che comportano la creazione del database da zero. Questo codice è disponibile anche nella *migrazioni* cartella, nel file denominato  *&lt;timestamp&gt;\_InitialCreate.cs*. Il `Up` metodo per il `InitialCreate` classe crea le tabelle di database che corrispondono ai set di entità del modello di dati, e il `Down` li elimina metodo.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Le migrazioni chiamano il metodo `Up` per implementare le modifiche al modello di dati per una migrazione. Quando si immette un comando per annullare l'aggiornamento, le migrazioni chiamano il metodo `Down`.

Questa è la migrazione iniziale creata al momento dell'immissione di `add-migration InitialCreate` comando. Il parametro (`InitialCreate` nell'esempio) viene usato per il file di un nome e può essere qualsiasi elemento; in genere si sceglie una parola o frase che riepiloghi cosa viene fatto nella migrazione. Ad esempio, è possibile denominare una migrazione successiva &quot;AddDepartmentTable&quot;.

Se la migrazione iniziale è stata creata quando il database esisteva già, il codice di creazione del database viene generato ma non è necessario eseguirlo perché il database corrisponde già al modello di dati. Quando si distribuisce l'app in un altro ambiente in cui il database non esiste ancora, questo codice verrà eseguito per creare il database, è consigliabile quindi testarlo prima. Ecco perché in precedenza è stato modificato il nome del database nella stringa di connessione: per far sì che le migrazioni possano crearne uno nuovo da zero.

1. Nel **Console di gestione pacchetti** finestra, immettere il comando seguente:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Il `update-database` comando esegue il `Up` metodo per creare il database e quindi viene eseguito il `Seed` metodo per popolare il database. Lo stesso processo verrà eseguito automaticamente nell'ambiente di produzione dopo aver distribuito l'applicazione, come si vedrà nella sezione seguente.
2. Uso **Esplora Server** per controllare il database come è stato fatto nella prima esercitazione ed eseguire l'applicazione per verificare che tutto funzioni uguale come in precedenza.

## <a name="deploy-to-azure"></a>Distribuire in Azure

Finora l'applicazione è stata in esecuzione in locale in IIS Express nel computer di sviluppo. Per renderlo disponibile ad altri utenti per l'utilizzo su Internet, è necessario distribuirla in un provider di hosting web. In questa sezione dell'esercitazione è possibile distribuirla in Azure. In questa sezione è facoltativa. è possibile ignorare questo passaggio e continuare con l'esercitazione seguente, oppure è possibile adattare le istruzioni in questa sezione per un provider di hosting diverso di propria scelta.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Con migrazioni Code First per distribuire il Database

Per distribuire il database si userà migrazioni Code First. Quando si crea il profilo di pubblicazione che consente di configurare le impostazioni per la distribuzione da Visual Studio, si selezionano una casella di controllo etichettata **aggiornare il Database**. Questa impostazione fa in modo che il processo di distribuzione configurare automaticamente l'applicazione *Web. config* del file nel server di destinazione in modo che usa Code First di `MigrateDatabaseToLatestVersion` classe inizializzatore.

Visual Studio non esegue alcuna operazione con il database durante il processo di distribuzione mentre si sta copiando il progetto nel server di destinazione. Quando si esegue l'applicazione distribuita e accede al database per la prima volta dopo la distribuzione, Code First controlla se il database corrisponde al modello di dati. Se è presente una mancata corrispondenza, Code First crea automaticamente il database (se non esiste ancora) o ne aggiorna lo schema del database alla versione più recente (se un database esiste ma non corrisponda al modello). Se l'applicazione implementa un migrazioni `Seed` metodo, l'esecuzione del metodo dopo la creazione del database o lo schema viene aggiornato.

Le migrazioni `Seed` metodo inserisce i dati di test. Se si distribuisse un ambiente di produzione, è necessario modificare il `Seed` metodo in modo che non solo inserisce i dati che si desidera essere inseriti nel database di produzione. Ad esempio, il modello di dati corrente è consigliabile avere corsi reali ma fittizi studenti nel database di sviluppo. È possibile scrivere un `Seed` metodo caricare entrambi in fase di sviluppo e quindi impostare come commento fittizi studenti prima di distribuire nell'ambiente di produzione. In alternativa è possibile scrivere un `Seed` metodo per caricare solo i corsi e immettere gli studenti fittizi nel database di test manualmente tramite interfaccia utente dell'applicazione.

### <a name="get-an-azure-account"></a>Creare un account Azure

È necessario un account Azure. Se si ha già uno, ma si dispone di una sottoscrizione di Visual Studio, puoi [attivare i benefici della sottoscrizione](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). In caso contrario, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Creare un sito web e un database SQL in Azure

L'app web in Azure verrà eseguito in un ambiente di hosting condiviso, ovvero che viene eseguito in macchine virtuali (VM) che vengono condivisi con altri client di Azure. Un ambiente di hosting condiviso è un modo economico per iniziare a usare nel cloud. In un secondo momento, se l'incremento del traffico web, la scalabilità per soddisfare le esigenze tramite l'esecuzione in macchine virtuali dedicate. Per altre informazioni sulle opzioni di prezzo per il servizio App di Azure, leggere la documentazione su [Azure Docs](https://azure.microsoft.com/pricing/details/app-service/)

Si verrà distribuito il database SQL di Azure. Database SQL è un servizio di database relazionale basato su cloud che si basa su tecnologie SQL Server. Strumenti e le applicazioni che funzionano con SQL Server sono utilizzabili anche con il Database SQL.

1. Nel [portale di gestione di Azure](https://portal.azure.com), fare clic su **New** nella scheda di sinistra, fare clic su **Vedi tutto** nel pannello nuovo e quindi fare clic su **App Web e SQL** nel **Web** sezione e infine **crea**.

    ![Pulsante nuovo nel portale di gestione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   Il **nuova App Web e SQL - creare** verrà visualizzata la procedura guidata.

2. Nel pannello, immettere una stringa nel **nome App** finestra da usare come URL univoco per l'applicazione. L'URL completo sarà costituito da quanto immesso in questo caso oltre il dominio predefinito dei servizi App di Azure (. azurewebsites.net). Se il **nome dell'App** è già in uso, la procedura guidata notifica di ciò con una linea rossa *il nome dell'app non è disponibile* messaggio. Se il **nome App** è disponibile, verrà visualizzato un segno di spunta verde.

    ![Crea con Database collegamento nel portale di gestione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. Nel **sottoscrizione** dal menu a discesa scegliere la sottoscrizione di Azure in cui si desidera il **servizio App** risiedano.

4. Nel **gruppo di risorse** casella di testo, scegliere un gruppo di risorse o crearne uno nuovo. Questa impostazione specifica il sito web verrà eseguito in quale data center. Per altre informazioni sui gruppi di risorse, leggere la documentazione sul [documentazione di Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
5. Creare un nuovo **piano di servizio App** facendo clic il *sezione del servizio App*, **Crea nuovo**, quindi compilare **piano di servizio App** (può essere stesso nome Servizio App), **ubicazione**, e **tariffario** (è disponibile un'opzione gratuita).

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. Fare clic sui **Database SQL**, scegliere *Crea nuovo* o selezionare un database esistente

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. Nel **nome** immettere un nome per il database.
8. Scegliere il **Server di destinazione** , quindi selezionare **creare un nuovo server**. In alternativa, se è stato creato un server, è possibile selezionare quel server dall'elenco dei server disponibili.
9. Scegli **piano tariffario** keychains *gratuito*. Se sono necessarie risorse aggiuntive, il database può essere aumentato in qualsiasi momento. Per altre informazioni sui prezzi di SQL Azure, continuare a leggere la documentazione [documentazione di Azure](https://azure.microsoft.com/pricing/details/sql-database/).
10. Modificare [regole di confronto](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support) in base alle esigenze.
11. Immettere un amministratore **nome utente amministratore SQL** e **Password amministratore SQL**. Se è stato selezionato **server di Database SQL nuova**, non si immetteranno un nome esistente e una password, immettere un nuovo nome e una password che si sta definendo ora da usare in seguito quando si accede al database. Se è stato selezionato un server creato in precedenza, immettere le credenziali per il server.
12. Raccolta di dati di telemetria può essere abilitata per il servizio App con Application Insights. Application Insights con una configurazione minima raccoglie importanti eventi, eccezioni, dipendenze, richiesta e le informazioni di traccia. Per altre informazioni su Application Insights, iniziare a utilizzare [documentazione di Azure](https://azure.microsoft.com/services/application-insights/).
13. Fare clic su **Create** nella parte inferiore del pannello per indicare che si è finito.
  
    Il portale di gestione restituisce alla pagina dashboard e il **notifiche** pannello nella parte superiore della pagina mostra che il sito viene creato. Dopo un periodo di tempo (in genere meno di un minuto), sarà presente una notifica che la distribuzione ha avuto esito positivo. Nella barra di spostamento a sinistra, il nuovo **servizio App** viene visualizzato nella *servizi App* sezione e il nuovo **Database SQL** viene visualizzato nel *i database SQL*  sezione.

### <a name="deploy-the-application-to-azure"></a>Distribuire l'applicazione in Azure

1. In Visual Studio, fare clic sul progetto in **Esplora soluzioni** e selezionare **Publish** dal menu di scelta rapida.
  
    ![Pubblicare nel menu di scelta rapida progetto](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. Nel **profilo** scheda della finestra di **Pubblica sito Web** procedura guidata, fare clic su **Microsoft Azure App Service**.
  
    ![Importa impostazioni di pubblicazione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. Se non è stato aggiunto in precedenza la sottoscrizione di Azure in Visual Studio, eseguire i passaggi sullo schermo. Questi passaggi abilitano Visual Studio per connettersi alla sottoscrizione di Azure in modo che l'elenco delle **servizi App** includerà il sito web.
 
4. Selezionare il **sottoscrizione** nel servizio App aggiunto a, quindi il **piano di servizio App** cartella del servizio App è una parte di, e infine il **servizio App** stessa seguita da **Accettabile**.

    ![Selezionare un servizio App](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. Dopo aver configurato il profilo, il **connessione** scheda verrà visualizzata. Fare clic su **convalida connessione** per assicurarsi che le impostazioni siano corrette

    ![Convalidare la connessione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. Quando la connessione è stata convalidata, un segno di spunta verde verrà visualizzato accanto al **convalida connessione** pulsante. Scegliere **Avanti**.
  
    ![Connessione è stata convalidata](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. Aprire il **stringa di connessione remota** elenco a discesa sotto **SchoolContext** e selezionare la stringa di connessione per il database è stato creato.
8. Selezionare **aggiornare il database**.

    ![Scheda Impostazioni](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    Questa impostazione fa in modo che il processo di distribuzione configurare automaticamente l'applicazione *Web. config* del file nel server di destinazione in modo che usa Code First di `MigrateDatabaseToLatestVersion` classe inizializzatore.
9. Scegliere **Avanti**.
10. Nel **Preview** scheda, fare clic su **Avvia anteprima**.
  
    ![Pulsante StartPreview nella scheda Anteprima](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    La scheda Visualizza un elenco dei file che verranno copiati nel server. Visualizzazione dell'anteprima per pubblicare l'applicazione non è necessario ma è una funzione utile da tenere presenti. In questo caso, non devi eseguire alcuna operazione con l'elenco di file che viene visualizzato. La volta successiva che si distribuirà questa applicazione, sarà solo i file che sono stati modificati in questo elenco.
    ![Output del file StartPreview](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. Fare clic su **Pubblica**.
    Visual Studio avvia il processo di copia dei file al server di Azure.
12. Il **Output** finestra Mostra le azioni di distribuzione sono state eseguite e segnalato il corretto completamento della distribuzione.
  
    ![Finestra di output di segnalazione della corretta distribuzione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. Al termine della distribuzione, il browser predefinito viene aperto automaticamente per l'URL del sito web distribuito.
    L'applicazione creata è in esecuzione nel cloud. 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

A questo punto il *SchoolContext* database è stato creato il Database SQL di Azure perché è stato selezionato **Esegui migrazioni Code First (inizia all'avvio dell'app)**. Il *Web. config* file nel sito web distribuito è stato modificato in modo che le [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inizializzatore viene eseguita la prima volta il codice legge o scrive dati nel database (che si sono verificati Se è selezionata la **studenti** scheda):

![](https://asp.net/media/4367421/mig.png)

Il processo di distribuzione creata anche una nuova stringa di connessione *(SchoolContext\_DatabasePublish*) per le migrazioni Code First da utilizzare per l'aggiornamento dello schema del database e il seeding del database.

![Stringa di connessione Database_Publish](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

È possibile trovare la versione distribuita del file Web. config nel computer in *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. È possibile accedere a distribuito *Web. config* file tramite FTP. Per istruzioni, vedere [distribuzione Web ASP.NET tramite Visual Studio: distribuzione di un aggiornamento del codice](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Seguire le istruzioni che iniziano con "per usare uno strumento FTP, sono necessarie tre cose: l'URL di FTP, il nome utente e la password."

> [!NOTE]
> L'app web non implementa la sicurezza, in modo che chiunque individui l'URL può modificare i dati. Per istruzioni su come proteggere il sito web, vedere [distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e Database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). È possibile impedire che altri utenti tramite il sito usando il portale di gestione di Azure oppure **Esplora Server** in Visual Studio per arrestare il sito.


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>Scenari di migrazione avanzati

Se si distribuisce un database, eseguire le migrazioni automaticamente come illustrato in questa esercitazione e si distribuisce in un sito web che viene eseguito su più server, è possibile ottenere più server si prova a eseguire le migrazioni nello stesso momento. Le migrazioni sono atomiche, in modo che se due server prova a eseguire la migrazione, uno avrà esito positivo e l'altra avrà esito negativo (presupponendo che le operazioni non è possibile eseguire due volte). In questo scenario se si desidera evitare questi problemi, è possibile chiamare manualmente le migrazioni e configurare il proprio codice in modo che il problema interessa solo una volta. Per altre informazioni, vedere [in esecuzione e le migrazioni di Scripting da codice](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) nel blog Rowan Miller e [Migrate.exe](https://msdn.microsoft.com/data/jj618307) (per l'esecuzione di migrazioni dalla riga di comando) in MSDN.

Per informazioni su altri scenari di migrazione, vedere [migrazioni Screencast serie](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Gli inizializzatori prima del codice

Nella sezione distribuzione è stato illustrato il [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inizializzatore in uso. Codice prima di tutto fornisce anche altri inizializzatori, tra cui [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (impostazione predefinita), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (che è utilizzata in precedenza) e [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Il `DropCreateAlways` inizializzatore può essere utile per impostare le condizioni per gli unit test. È anche possibile scrivere i propri inizializzatori ed è possibile chiamare un inizializzatore in modo esplicito, se non si vuole attendere fino a quando l'applicazione legge o scrive nel database. Al momento in questa esercitazione viene scritto nel novembre 2013, è possibile usare solo gli inizializzatori DropCreate e crea prima di abilitare le migrazioni. Il team di Entity Framework sta rendendo questi inizializzatori utilizzabile con le migrazioni anche.

Per altre informazioni sugli inizializzatori, vedere [Understanding inizializzatori di Database in Code First di Entity Framework](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) e il capitolo 6 del libro [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) di Julie Lerman e Rowan Miller.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato spiegato come abilitare le migrazioni e distribuire l'applicazione. Nella prossima esercitazione verrà iniziare esaminando gli argomenti più avanzati espandendo il modello di dati.

Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare. È anche possibile richiedere nuovi argomenti in [Mostra Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Collegamenti ad altre risorse di Entity Framework sono disponibili nel [l'accesso ai dati ASP.NET - risorse consigliate](xref:whitepapers/aspnet-data-access-content-map).

> [!div class="step-by-step"]
> [Precedente](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [Successivo](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
