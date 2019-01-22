---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: Usare migrazioni di Entity Framework in un'app ASP.NET MVC e distribuire in Azure"
author: tdykstra
description: In questa esercitazione abilitare migrazioni Code First e distribuire l'applicazione al cloud in Azure.
ms.author: riande
ms.date: 01/16/2019
ms.topic: tutorial
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 131540113f5ac8ce9e15c8ab92b8dc7ee11de115
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444220"
---
# <a name="tutorial-use-ef-migrations-in-an-aspnet-mvc-app-and-deploy-to-azure"></a>Esercitazione: Usare migrazioni di Entity Framework in un'app ASP.NET MVC e distribuire in Azure

Finora l'applicazione web di esempio Contoso University è stato in esecuzione in locale in IIS Express nel computer di sviluppo. Per rendere disponibile ad altri utenti per usare la rete Internet un'applicazione reale, è necessario distribuirla in un provider di hosting web. In questa esercitazione abilitare migrazioni Code First e distribuire l'applicazione al cloud in Azure:

- Abilitare migrazioni Code First. La funzionalità delle migrazioni consente di modificare il modello di dati e distribuire le modifiche nell'ambiente di produzione aggiornando lo schema del database senza dover eliminare e ricreare il database.
- Distribuire in Azure. Questo passaggio è facoltativo. è possibile continuare con le esercitazioni rimanenti senza avere distribuito il progetto.

È consigliabile che si utilizza un processo di continuous integration con controllo del codice sorgente per la distribuzione, ma questa esercitazione non illustra tali argomenti. Per altre informazioni, vedere la [controllo del codice sorgente](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) e [ricchissimo](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) capitoli di [creazione di App Cloud reali con Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Abilitare migrazioni Code First
> * Distribuire l'app in Azure (facoltativo)

## <a name="prerequisites"></a>Prerequisiti

- [Resilienza della connessione e intercettazione dei comandi](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-code-first-migrations"></a>Abilitare migrazioni Code First

Quando si sviluppa una nuova applicazione, il modello di dati cambia di frequente e, a ogni cambiamento, non è più sincronizzato con il database. Durante la configurazione di Entity Framework per automaticamente eliminare e ricreare il database ogni volta che si modifica il modello di dati. Quando aggiungere, rimuovere, o modificare le classi di entità o modificare i `DbContext` classe, alla successiva esecuzione dell'applicazione automaticamente consente di eliminare il database esistente, viene creata una nuova istanza che corrisponde al modello ed esegue il seeding con dati di test.

Questo metodo che consiste nel mantenere il database sincronizzato con il modello di dati funziona bene fino a quando non si distribuisce l'applicazione nell'ambiente di produzione. Quando l'applicazione è in esecuzione nell'ambiente di produzione, in genere archivia i dati che si desidera mantenere e non si vuole perdere tutto ad ogni volta che si apporta una modifica ad esempio l'aggiunta di una nuova colonna. Il [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) funzionalità risolve questo problema abilitando Code First aggiornare lo schema del database anziché dover eliminare e ricreare il database. In questa esercitazione si distribuirà l'applicazione e per preparare che è possibile abilitare migrazioni.

1. Disabilitare l'inizializzatore che è impostato in precedenza da impostare come commento o eliminare il `contexts` elemento che è stato aggiunto al file Web. config dell'applicazione.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Anche nell'applicazione *Web. config* file, modificare il nome del database nella stringa di connessione in ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Questa modifica configura il progetto in modo che la prima migrazione crei un nuovo database. Non è obbligatorio, ma capirà più avanti perché è una buona idea.
3. Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.

1. Nel `PM>` prompt dei comandi immettere i comandi seguenti:

    ```text
    enable-migrations
    add-migration InitialCreate
    ```

    Il `enable-migrations` comando crea un *migrazioni* cartella nel progetto ContosoUniversity e lo inserisce in tale cartella un *Configuration.cs* file che è possibile modificare per configurare le migrazioni.

    (Se hai perso il passaggio precedente che indirizza l'utente per modificare il nome del database, migrazioni troveranno il database esistente ed eseguire automaticamente il `add-migration` comando. Questa impostazione è corretta, che significa semplicemente che è non verrà eseguito un test del codice di migrazioni prima di distribuire il database. In seguito quando si esegue il `update-database` comando non accade nulla perché il database esiste già.)

    Aprire il *ContosoUniversity\Migrations\Configuration.cs* file. Analogamente alla classe inizializzatore che hai visto in precedenza, il `Configuration` classe include una `Seed` (metodo).

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Lo scopo del [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodo consiste nel consentire di inserire o aggiornare i dati di test dopo Code First crea o aggiorna il database. Il metodo viene chiamato quando viene creato il database e ogni volta che viene aggiornato lo schema del database dopo la modifica di un modello di dati.

### <a name="set-up-the-seed-method"></a>Configurare il metodo di inizializzazione

Quando si elimina e ricrea il database per ogni modifica del modello di dati, si usa la classe di inizializzatore `Seed` metodo per inserire i dati di test, perché dopo ogni modifica del modello di database viene eliminato e tutti i dati di test viene perso. Con migrazioni Code First, test, i dati vengono conservati dopo le modifiche di database, pertanto, inclusi i dati di test nel [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) (metodo) non è in genere necessario. Infatti, non si desidera che il `Seed` metodo per inserire i dati di test se si useranno le migrazioni per distribuire il database nell'ambiente di produzione, perché il `Seed` metodo verrà eseguito nell'ambiente di produzione. In tal caso, si desidera che il `Seed` per inserire nel database solo i dati necessari nella produzione. Ad esempio, è possibile il database da includere i nomi dei reparti effettivo nel `Department` tabella quando l'applicazione diventa disponibile nell'ambiente di produzione.

Per questa esercitazione, si useranno le migrazioni per la distribuzione, ma il `Seed` metodo inserirà comunque i dati di test per renderlo più semplice verificare il funzionamento delle funzionalità dell'applicazione senza dover inserire manualmente una grande quantità di dati.

1. Sostituire il contenuto di *Configuration.cs* file con il codice seguente, che carica i dati di test nel nuovo database.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Il [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) accetta l'oggetto di contesto di database come parametro di input e il codice nel metodo Usa l'oggetto per aggiungere nuove entità nel database. Per ogni tipo di entità, il codice crea una raccolta di nuove entità, li aggiunge a appropriato [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) proprietà e quindi Salva le modifiche al database. Non è necessario chiamare il [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metodo dopo ogni gruppo di entità, come avviene in questo caso, ma questa operazione consente di individuare l'origine di un problema se si verifica un'eccezione mentre il codice è la scrittura nel database.

    Alcune istruzioni che inseriscono dati usare la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo per eseguire un'operazione "upsert". Poiché il `Seed` metodo viene eseguito ogni volta che si esegue il `update-database` comando, in genere dopo ogni migrazione, non è sufficiente inserire i dati, perché si sta provando ad aggiungere righe saranno già presenti dopo la migrazione prima che crea il database. L'operazione "upsert" impedisce gli errori che accadrebbe se si prova a inserire una riga già esistente, ma ***esegue l'override*** eventuali modifiche ai dati apportate durante il test dell'applicazione. I dati di test in alcune tabelle è possibile evitare di raggiungere tale obiettivo: in alcuni casi quando si modificano i dati durante il test per le proprie modifiche permangono dopo gli aggiornamenti del database. In questo caso si vuole eseguire un'operazione di inserimento condizionale: inserire una riga solo se non esiste già. Il metodo di inizializzazione Usa entrambi gli approcci.

    Il primo parametro passato per il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo consente di specificare la proprietà da utilizzare per verificare se esiste già una riga. Per i dati degli studenti di test che si desidera fornire, il `LastName` proprietà può essere utilizzata per questo scopo poiché ogni cognome nell'elenco è univoco:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Questo codice si presuppone che i cognomi univoci. Se si aggiungono manualmente gli studenti il cui cognome duplicato, si otterrà la seguente eccezione la volta successiva che si esegue una migrazione:

    **La sequenza contiene più di un elemento**

    Per informazioni su come gestire i dati ridondanti, ad esempio due studenti denominati "Alexander Carson", vedere [Seeding e al database di debug di Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) sul blog di Rick Anderson. Per altre informazioni sul `AddOrUpdate` metodo, vedere [prestare attenzione con metodo AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sul blog di Julie.

    Il codice che crea `Enrollment` entità si presuppone il `ID` valore nelle entità nel `students` raccolta, anche se si non imposta tale proprietà nel codice che crea la raccolta.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    È possibile usare il `ID` proprietà qui perché il `ID` è impostato quando si chiama `SaveChanges` per il `students` raccolta. Entity Framework ottiene automaticamente il valore della chiave primaria quando inserisce un'entità nel database e aggiorna il `ID` proprietà dell'entità in memoria.

    Il codice che aggiunge ognuno `Enrollment` entità per il `Enrollments` set di entità non usa il `AddOrUpdate` (metodo). Controlla se esiste già un'entità e inserisce l'entità se non esiste. Questo approccio consente di mantenere le modifiche apportate a un livello di registrazione usando l'interfaccia utente dell'applicazione. Il codice scorre in ciclo ogni membro del `Enrollment` [elenco](https://msdn.microsoft.com/library/6sh2ey19.aspx) e se la registrazione non viene trovata nel database, aggiunge la registrazione al database. La prima volta che si aggiorna il database, il database sarà vuoto, in modo che verranno aggiunti ogni registrazione.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

2. Compilare il progetto.

### <a name="execute-the-first-migration"></a>Eseguire la migrazione prima

Quando è stato eseguito il `add-migration` comando, le migrazioni ha generato il codice che comportano la creazione del database da zero. Questo codice è disponibile anche nella *migrazioni* cartella, nel file denominato  *&lt;timestamp&gt;\_InitialCreate.cs*. Il `Up` metodo per il `InitialCreate` classe crea le tabelle di database che corrispondono ai set di entità del modello di dati, e il `Down` li elimina metodo.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Le migrazioni chiamano il metodo `Up` per implementare le modifiche al modello di dati per una migrazione. Quando si immette un comando per annullare l'aggiornamento, le migrazioni chiamano il metodo `Down`.

Questa è la migrazione iniziale creata al momento dell'immissione di `add-migration InitialCreate` comando. Il parametro (`InitialCreate` nell'esempio) viene usato per il file di un nome e può essere qualsiasi elemento; in genere si sceglie una parola o frase che riepiloghi cosa viene fatto nella migrazione. Ad esempio, è possibile denominare una migrazione successiva &quot;AddDepartmentTable&quot;.

Se la migrazione iniziale è stata creata quando il database esisteva già, il codice di creazione del database viene generato ma non è necessario eseguirlo perché il database corrisponde già al modello di dati. Quando si distribuisce l'app in un altro ambiente in cui il database non esiste ancora, questo codice verrà eseguito per creare il database, è consigliabile quindi testarlo prima. Per tale motivo è stato modificato il nome del database nella stringa di connessione precedentemente&mdash;in modo che le migrazioni possono crearne uno nuovo da zero.

1. Nel **Console di gestione pacchetti** finestra, immettere il comando seguente:

    `update-database`

    Il `update-database` comando esegue il `Up` metodo per creare il database e quindi viene eseguito il `Seed` metodo per popolare il database. Lo stesso processo verrà eseguito automaticamente nell'ambiente di produzione dopo aver distribuito l'applicazione, come si vedrà nella sezione seguente.
2. Uso **Esplora Server** per controllare il database come è stato fatto nella prima esercitazione ed eseguire l'applicazione per verificare che tutto funzioni uguale come in precedenza.

## <a name="deploy-to-azure"></a>Distribuire in Azure

Finora l'applicazione è stata in esecuzione in locale in IIS Express nel computer di sviluppo. Per renderlo disponibile ad altri utenti per l'utilizzo su Internet, è necessario distribuirla in un provider di hosting web. In questa sezione dell'esercitazione, è necessario distribuirlo in Azure. In questa sezione è facoltativa. è possibile ignorare questo passaggio e continuare con l'esercitazione seguente, oppure è possibile adattare le istruzioni in questa sezione per un provider di hosting diverso di propria scelta.

### <a name="use-code-first-migrations-to-deploy-the-database"></a>Utilizzare le migrazioni codice prima di distribuire il database

Per distribuire il database, si utilizzeranno le migrazioni primo codice. Quando si crea il profilo di pubblicazione che consente di configurare le impostazioni per la distribuzione da Visual Studio, si selezionano una casella di controllo etichettata **aggiornare il Database**. Questa impostazione fa in modo che il processo di distribuzione configurare automaticamente l'applicazione *Web. config* del file nel server di destinazione in modo che usa Code First di `MigrateDatabaseToLatestVersion` classe inizializzatore.

Visual Studio non esegue alcuna operazione con il database durante il processo di distribuzione mentre si sta copiando il progetto nel server di destinazione. Quando si esegue l'applicazione distribuita e accede al database per la prima volta dopo la distribuzione, Code First controlla se il database corrisponde al modello di dati. Se è presente una mancata corrispondenza, Code First crea automaticamente il database (se non esiste ancora) o ne aggiorna lo schema del database alla versione più recente (se un database esiste ma non corrisponda al modello). Se l'applicazione implementa un migrazioni `Seed` metodo, l'esecuzione del metodo dopo la creazione del database o lo schema viene aggiornato.

Le migrazioni `Seed` metodo inserisce i dati di test. Se si distribuisse un ambiente di produzione, è necessario modificare il `Seed` metodo in modo che non solo inserisce i dati che si desidera essere inseriti nel database di produzione. Ad esempio, il modello di dati corrente è consigliabile avere corsi reali ma fittizi studenti nel database di sviluppo. È possibile scrivere un `Seed` metodo caricare entrambi in fase di sviluppo e quindi impostare come commento fittizi studenti prima di distribuire nell'ambiente di produzione. In alternativa è possibile scrivere un `Seed` metodo per caricare solo i corsi e immettere gli studenti fittizi nel database di test manualmente tramite interfaccia utente dell'applicazione.

### <a name="get-an-azure-account"></a>Creare un account Azure

È necessario un account Azure. Se si ha già uno, ma si dispone di una sottoscrizione di Visual Studio, puoi [attivare i benefici della sottoscrizione](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/
). In caso contrario, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Creare un sito web e un database SQL in Azure

L'app web in Azure verrà eseguito in un ambiente di hosting condiviso, ovvero che viene eseguito in macchine virtuali (VM) che vengono condivisi con altri client di Azure. Un ambiente di hosting condiviso è un modo economico per iniziare a usare nel cloud. In un secondo momento, se l'incremento del traffico web, la scalabilità per soddisfare le esigenze tramite l'esecuzione in macchine virtuali dedicate. Per ulteriori informazioni sulle opzioni di prezzi per il servizio di applicazione Azure, Leggi [servizio App prezzi](https://azure.microsoft.com/pricing/details/app-service/).

Distribuire il database al database SQL Azure. Database SQL è un servizio di database relazionali basati su cloud che si basa su tecnologie di SQL Server. Strumenti e applicazioni che utilizzano SQL Server funzionano anche con database SQL.

1. Nella [portale di gestione Azure](https://portal.azure.com), scegliere **creare una risorsa** sinistra scheda e quindi scegliere **tutti** sulla **nuovo** riquadro (o *blade*) per visualizzare tutte le risorse disponibili. Scegliere **Web App + SQL** nel **Web** sezione di **tutto** blade. Infine, scegliere **crea**.

    ![Creare una risorsa nel portale di Azure](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/create-azure-resource.png)

   Il modulo per creare un nuovo **nuova Web App + SQL** verrà visualizzata la risorsa.

2. Immettere una stringa di **nome App** casella per utilizzare come URL univoco per l'applicazione. L'URL completo sarà costituito da quanto immesso in questo caso oltre il dominio predefinito dei servizi App di Azure (. azurewebsites.net). Se il **nome App** è già utilizzato, la procedura guidata segnala con un colore rosso *il nome dell'applicazione non è disponibile* messaggio. Se il **nome applicazione** è disponibile, viene visualizzato un segno di spunta verde.

3. Nella **sottoscrizione** la sottoscrizione di Azure in cui si desidera scegliere di **servizio App** collocare.

4. Nel **gruppo di risorse** casella di testo, scegliere un gruppo di risorse o crearne uno nuovo. Questa impostazione specifica il sito web verrà eseguito in quale data center. Per ulteriori informazioni sui gruppi di risorse, vedere [gruppi di risorse](/azure/azure-resource-manager/resource-group-overview#resource-groups).

5. Creare un nuovo **piano di servizio App** facendo clic il *sezione del servizio App*, **Crea nuovo**, quindi compilare **piano di servizio App** (può essere stesso nome Servizio App), **ubicazione**, e **tariffario** (è disponibile un'opzione gratuita).

6. Fare clic su **Database SQL**, quindi scegliere **creare un nuovo database** o selezionare un database esistente.

7. Nel **nome** immettere un nome per il database.
8. Scegliere la **Server di destinazione** e quindi selezionare **creare un nuovo server**. In alternativa, se è stato creato un server, è possibile selezionare quel server dall'elenco dei server disponibili.
9. Scegli **piano tariffario** keychains *gratuito*. Se sono necessarie risorse aggiuntive, il database può essere aumentato in qualsiasi momento. Per ulteriori informazioni sui prezzi di SQL Azure, vedere [Database SQL Azure prezzi](https://azure.microsoft.com/pricing/details/sql-database/managed/).
10. Modificare [regole di confronto](/sql/relational-databases/collations/collation-and-unicode-support) in base alle esigenze.
11. Immettere un amministratore **nome utente amministratore SQL** e **Password amministratore SQL**.

   - Se si seleziona **Nuovo Database di SQL server**, definire un nuovo nome e una password che verranno utilizzati in un secondo momento quando si accede al database.
   - Se si seleziona un server che è stato creato in precedenza, immettere le credenziali per il server.

12. Raccolta di dati di telemetria può essere abilitata per il servizio App con Application Insights. Con alcune operazioni di configurazione applicazione approfondimenti raccoglie eventi importanti, eccezioni, dipendenza, richiesta e le informazioni di traccia. Per ulteriori informazioni dagli esperti dell'applicazione, vedere [Monitor Azure](https://azure.microsoft.com/services/monitor/).
13. Fare clic su **crea** nella parte inferiore per indicare che è terminato.

    Portale di gestione restituisce alla pagina del Dashboard e la **notifiche** area nella parte superiore della pagina indica che il sito viene creato. Dopo un certo tempo (in genere inferiore a un minuto), vi è una notifica che la distribuzione completata. Nella barra di navigazione a sinistra, viene visualizzato il nuovo servizio App nel **servizi di applicazione** sezione e il nuovo database SQL viene visualizzata nella **database SQL** sezione.

### <a name="deploy-the-app-to-azure"></a>Distribuire l'app in Azure

1. In Visual Studio, fare clic sul progetto in **Esplora soluzioni** e selezionare **Publish** dal menu di scelta rapida.

2. Nella **scegliete una destinazione di pubblicazione** , selezionare **servizio App** e quindi **selezionare esistente**e quindi scegliere **pubblica**.

    ![Scegliete una pagina di destinazione di pubblicazione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/publish-select-existing-azure-app-service.png)

3. Se in precedenza, non sono stati aggiunti l'abbonamento Azure in Visual Studio, eseguire la procedura sullo schermo. Questi passaggi abilitano Visual Studio per connettersi alla sottoscrizione di Azure in modo che l'elenco delle **servizi App** includerà il sito web.

4. Nella **servizio App** pagina, selezionare il **sottoscrizione** aggiunto il servizio di applicazione per. In **vista**, selezionare **gruppo di risorse**. Espandere il gruppo di risorse per che è stato aggiunto il servizio di applicazione e quindi selezionare il servizio di applicazione. Scegliere **OK** per pubblicare l'applicazione.

5. Il **Output** finestra Mostra le azioni di distribuzione sono state eseguite e segnalato il corretto completamento della distribuzione.

6. Al termine della distribuzione, il browser predefinito viene aperto automaticamente per l'URL del sito web distribuito.

    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/cloud-app-browser.png)

    L'applicazione è in esecuzione nel cloud.

A questo punto, il *SchoolContext* database è stato creato nel database SQL Azure è stato selezionato **eseguire codice prima le migrazioni (viene eseguito all'avvio di app)**. Il *Web. config* file nel sito web distribuito è stato modificato in modo che le [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inizializzatore viene eseguita la prima volta il codice legge o scrive dati nel database (che si sono verificati Se è selezionata la **studenti** scheda):

![Frammento di file Web. config](https://asp.net/media/4367421/mig.png)

Il processo di distribuzione creata anche una nuova stringa di connessione *(SchoolContext\_DatabasePublish*) per le migrazioni Code First da utilizzare per l'aggiornamento dello schema del database e il seeding del database.

![Stringa di connessione nel file Web. config](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

È possibile trovare la versione distribuita del file Web. config nel computer in *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. È possibile accedere a distribuito *Web. config* file tramite FTP. Per istruzioni, vedere [distribuzione Web ASP.NET tramite Visual Studio: Distribuzione di un aggiornamento del codice](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Seguire le istruzioni che iniziano con "per usare uno strumento FTP, sono necessarie tre cose: l'URL di FTP, il nome utente e la password."

> [!NOTE]
> L'app web non implementa la sicurezza, in modo che chiunque individui l'URL può modificare i dati. Per istruzioni su come proteggere il sito web, vedere [distribuire un'applicazione MVC ASP.NET protetta con i database di appartenenza, OAuth e SQL Azure](/aspnet/core/security/authorization/secure-data). È possibile impedire che altri utenti tramite il sito interrompendo il servizio tramite il portale di gestione di Azure o **Esplora Server** in Visual Studio.

![Interrompere una voce di menu dell'applicazione del servizio](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/server-explorer-stop-app-service.png)

## <a name="advanced-migrations-scenarios"></a>Scenari avanzati di migrazioni

Se si distribuisce un database, eseguire le migrazioni automaticamente come illustrato in questa esercitazione e si distribuisce in un sito web che viene eseguito su più server, è possibile ottenere più server si prova a eseguire le migrazioni nello stesso momento. Le migrazioni sono atomiche, in modo che se due server prova a eseguire la migrazione, uno avrà esito positivo e l'altra avrà esito negativo (presupponendo che le operazioni non è possibile eseguire due volte). In questo scenario se si desidera evitare questi problemi, è possibile chiamare manualmente le migrazioni e configurare il proprio codice in modo che il problema interessa solo una volta. Per ulteriori informazioni, vedere [in esecuzione e le migrazioni di Scripting da codice](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) sul blog del Miller Rowan e [Migrate.exe](/ef/ef6/modeling/code-first/migrations/migrate-exe) (per l'esecuzione di migrazioni dalla riga di comando).

Per informazioni su altri scenari di migrazione, vedere [migrazioni Screencast serie](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Inizializzatori di primo codice

Nella sezione distribuzione, si è visto la [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inizializzatore in uso. Codice prima di tutto fornisce anche altri inizializzatori, tra cui [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (impostazione predefinita), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (che è utilizzata in precedenza) e [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Il `DropCreateAlways` inizializzatore può essere utile per impostare le condizioni per gli unit test. È anche possibile scrivere i propri inizializzatori ed è possibile chiamare un inizializzatore in modo esplicito, se non si vuole attendere fino a quando l'applicazione legge o scrive nel database.

Per altre informazioni sugli inizializzatori, vedere [Understanding inizializzatori di Database in Code First di Entity Framework](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) e il capitolo 6 del libro [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) da Julie Lerman e Rowan Miller.

## <a name="get-the-code"></a>Ottenere il codice

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>Risorse aggiuntive

Collegamenti ad altre risorse di Entity Framework sono disponibili nel [l'accesso ai dati ASP.NET - risorse consigliate](xref:whitepapers/aspnet-data-access-content-map).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Abilitato migrazioni Code First
> * Distribuzione dell'app in Azure (facoltativo)

Passare all'articolo successivo per informazioni su come creare un modello di dati più complesso per un'applicazione ASP.NET MVC.
> [!div class="nextstepaction"]
> [Creare un modello di dati più complesso](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
