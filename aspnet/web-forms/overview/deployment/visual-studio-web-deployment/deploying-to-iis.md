---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Distribuzione Web ASP.NET tramite Visual Studio: Distribuzione per il Test | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: d49dfad368ca4b81bb865103a99ec223a1cc66df
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396337"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Distribuzione Web ASP.NET tramite Visual Studio: Distribuzione per il Test
====================
da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET web dell'applicazione per App Web di servizio App di Azure o per un provider di hosting di terze parti con Visual Studio 2017. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

## <a name="overview"></a>Panoramica

In questa esercitazione si distribuirà un'applicazione web ASP.NET per Internet Information Server (IIS) nel computer locale.

In genere quando si sviluppa un'applicazione, eseguirla e testarla in Visual Studio. Per impostazione predefinita, i progetti applicazione web in Visual Studio 2017 usano IIS Express come server web di sviluppo. IIS Express è più simile a IIS completo rispetto a Visual Studio Development Server (noto anche come Cassini), che Visual Studio 2017 Usa per impostazione predefinita. Tuttavia, nessuno dei due server web di sviluppo funziona esattamente come IIS. Di conseguenza, un'app è stato possibile eseguire e testare in modo corretto in Visual Studio ma non riesce quando viene distribuito a IIS.

È possibile testare l'applicazione in modo affidabile in due modi:

1. Distribuire l'applicazione in IIS nel computer di sviluppo usando lo stesso processo che si userà in un secondo momento per la distribuzione in ambiente di produzione.

   È possibile configurare Visual Studio per utilizzare IIS quando si esegue un progetto web, ma che non sarebbe testare il processo di distribuzione. Questo metodo convalida il processo di distribuzione e che l'applicazione viene eseguita correttamente in IIS.

2. Distribuire l'applicazione in un ambiente di test simile all'ambiente di produzione. 
 
   L'ambiente di produzione per queste esercitazioni è App Web nel servizio App di Azure. L'ambiente di test ideale è un'app web aggiuntive creata nel servizio di Azure. Anche se è necessario configurare esattamente come un'app web di produzione, si sarebbe usarlo solo per i test.

Opzione 2 è il modo più affidabile per eseguire il test. Se si usa l'opzione 2, non necessariamente devi usare l'opzione 1. Tuttavia se si distribuisce in una terza parte hosting provider, opzione 2 potrebbe non essere fattibile o potrebbe essere costosa, pertanto questa serie di esercitazioni illustra entrambi i metodi. Vengono fornite istruzioni per l'opzione 2 nel [distribuzione nell'ambiente di produzione](deploying-to-production.md) esercitazione.

Per altre informazioni sull'uso di server web in Visual Studio, vedere [server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Promemoria: Se si riceve un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](troubleshooting.md).

## <a name="download-the-contoso-university-starter-project"></a>Scaricare il progetto di avvio di Contoso University

Scaricare e installare la soluzione di Visual Studio di Contoso University starter e il progetto. Questa soluzione contiene esercitazione completata. 

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a>Installare IIS

Per distribuire in IIS nel computer di sviluppo, verificare che siano installati IIS e distribuzione Web. Per impostazione predefinita, Visual Studio consente di installare distribuzione Web, ma IIS non è incluso nella configurazione predefinita di Windows 10, Windows 8 o Windows 7. Se è già stato installato IIS e il pool di applicazioni predefinito è già impostato su .NET 4, andare al [nella sezione successiva](#sqlexpress).

1. Si consiglia di usare la [installazione guidata piattaforma Web (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) per installare IIS e distribuzione Web. WPI installa una configurazione consigliata per IIS che include i prerequisiti IIS e distribuzione Web, se necessario.

   Se già stato installato IIS, distribuzione Web o dei relativi componenti necessari, viene installato il WPI solo cosa manca.

   * Usare l'installazione guidata piattaforma Web per installare IIS e distribuzione Web:
    
     ![Installare IIS seguendo WPI](deploying-to-iis/_static/image30.png)

     ![Installare distribuzione Web usando WPI](deploying-to-iis/_static/image31.png)

     Si vedrà messaggi che indicano che verrà installato IIS 7. Il collegamento funzionerà per IIS 8 in Windows 8; ma per Windows 8 e versioni successive, seguire i passaggi seguenti per assicurarsi che sia installato ASP.NET 4.7:

   * Aprire **Pannello di controllo** > **programmi** > **programmi e funzionalità** > **o disattivazione delle funzionalità Windows attivare** .

   * Espandere **Internet Information Services**, **servizi World Wide Web**, e **funzionalità per lo sviluppo dell'applicazione**.
   
   * Verificare che **ASP.NET 4.7** sia selezionata.

     ![Selezionare ASP.NET 4.7](deploying-to-iis/_static/image1a.png)

   * Verificare che **servizi World Wide Web** e **Console di gestione IIS** sia selezionata. Ciò consente di installare IIS e gestione IIS.
    
     ![Selezionare Servizi World Wide Web](deploying-to-iis/_static/image24.png)    
  
   * Scegliere **OK**. Vengono visualizzati i messaggi di finestra di dialogo che indica l'installazione è in corso.

Dopo aver installato IIS, eseguire **Gestione IIS** per assicurarsi che .NET Framework versione 4 sia assegnato al pool di applicazioni predefinito.

1. Premere WINDOWS + R per aprire la **eseguire** nella finestra di dialogo.

   (In Windows 8 o versione successiva, immettere "run" **avviare** pagina. In Windows 7, selezionare **eseguiti** dalle **avviare** menu. Se **eseguiti** non è presente nel **avviare** menu, pulsante destro del mouse la barra delle applicazioni, selezionare **proprietà**, selezionare il **Menu Start** selezionare **Personalizza**e selezionare **eseguire comando**.)

2. Immettere "inetmgr" e selezionare **OK**.

3. Nel **connessioni** riquadro, espandere il nodo del server e selezionare **pool di applicazioni**. Nel **pool di applicazioni** riquadro se **DefaultAppPool** viene assegnato a .NET framework versione 4 come illustrato nella figura seguente, passare alla sezione successiva.

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. Se viene visualizzato solo due pool di applicazioni ed entrambe vengono impostate su .NET Framework 2.0, installare ASP.NET 4 in IIS.

   Per Windows 8 o versioni successive, vedere le istruzioni della sezione precedente per verificare che ASP.NET 4.7 è installata oppure l'articolo [come installare ASP.NET 4.5 in Windows 8 e Windows Server 2012](https://support.microsoft.com/kb/2736284). Per Windows 7, aprire una finestra del prompt dei comandi facendo clic **prompt dei comandi** nella finestra di Windows **avviare** dal menu e selezionando **Esegui come amministratore**. Eseguire [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) installare ASP.NET 4 in IIS usando i comandi seguenti. (In sistemi a 32 bit, sostituire "Framework64" con "Framework").

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   Questo comando crea una nuova applicazione che pool per .NET Framework 4, ma il pool di applicazioni predefinito rimane impostata su 2.0. Si distribuisce un'applicazione che è destinata a .NET 4 per tale pool di applicazioni, modificare quindi il pool di applicazioni in .NET 4.

5. Se è stato chiuso **Gestione IIS**eseguirlo di nuovo, espandere il nodo del server e selezionare **pool di applicazioni**.

6. Nel **pool di applicazioni** riquadro, selezionare **DefaultAppPool**. Nel **azioni** riquadro, selezionare **impostazioni di base**.

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. Nel **modifica Pool di applicazioni** della finestra di dialogo Modifica il **versione Clr.net** al **CLR .NET v4.0.30319**. Scegliere **OK**.

   ![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

A questo punto si è pronti pubblicare un'applicazione web in IIS. In primo luogo, tuttavia, creare i database per il test.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Installare SQL Server Express

Local DB non è progettato per funzionare in IIS, l'ambiente di test deve pertanto avere installato SQL Server Express. Se si usa Visual Studio 2010 SQL Server Express, è già installato per impostazione predefinita. Se si usa Visual Studio 2012 o versioni successiva, installare SQL Server Express.

Per installare SQL Server Express, scaricare e installare l'app da [area Download Microsoft: Microsoft SQL Server 2017 Express edition](https://www.microsoft.com/sql-server/sql-server-editions-express). 

Nella prima pagina del Centro installazione SQL Server, selezionare **nuova installazione SQL Server autonomo o aggiungere caratteristiche a un'installazione esistente** e seguire le istruzioni, accettando le scelte predefinite. Nell'installazione guidata, accettare le impostazioni predefinite. Per altre informazioni sulle opzioni di installazione, vedere [installare SQL Server dall'installazione guidata (programma di installazione)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Creare i database di SQL Server Express per l'ambiente di test

L'applicazione Contoso University è disponibili due database: 

1. Database di appartenenza 
2. database dell'applicazione 

È possibile distribuire questi database a due distinti database o a un singolo database. Combinandoli rende database join tra di essi è più semplice. 

Se si distribuisce in un provider di hosting di terze parti, il piano di hosting può anche indicare il motivo per combinarle. Ad esempio, il provider può addebitare informazioni per più database o potrebbe non consentire anche di più di un database.

In questa esercitazione, si distribuiranno due database nell'ambiente di test e a un database in ambienti di staging e produzione.

Dal **View** dal menu di Visual Studio, selezionare **Esplora Server** (**Esplora Database** in Visual Web Developer). Fare doppio clic su **connessioni dati** e selezionare **Crea nuovo Database di SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

Nel **Crea nuovo Database di SQL Server** finestra di dialogo immettere ". \SQLExpress" nel **nome Server** casella e "aspnet-ContosoUniversity" nel **nuovo nome del database** casella. Scegliere **OK**.

![Creare aspnet-ContosoUniversity](deploying-to-iis/_static/image9.png)

Seguire la stessa procedura per creare un nuovo database dell'istituto di istruzione di SQL Server Express denominato `ContosoUniversity`.

**Esplora server** Mostra i due nuovi database.

![Nuovo database in Esplora Server](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Creare uno script di concessione per i nuovi database

Quando l'applicazione viene eseguita in IIS nel computer di sviluppo, l'applicazione usa le credenziali del pool di applicazioni predefinito per accedere al database. Tuttavia, per impostazione predefinita, il pool di applicazioni non dispone dell'autorizzazione per aprire i database. Ciò significa che è necessario eseguire uno script per concedere tale autorizzazione. In questa sezione, si sarà creare lo script ed eseguirlo in un secondo momento per assicurarsi che l'applicazione può aprire i database quando è in esecuzione in IIS.

In un editor di testo, copiare i comandi SQL seguenti in un nuovo file e salvarlo come *Grant.sql*. 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

In Visual Studio, aprire la soluzione di Contoso University. La soluzione (non uno dei progetti) e scegliere **Add**. Selezionare **elemento esistente**, passare alla *Grant.sql*e aprirlo.

> [!NOTE]
> Questo script è progettato per lavorare con SQL Server Express 2012 o versioni successive e con le impostazioni di IIS in Windows 10, Windows 8 o Windows 7, secondo quanto specificato in questa esercitazione. Se si usa una versione diversa di SQL Server o Windows, o se si configura IIS nel computer in modo diverso, le modifiche apportate allo script potrebbero essere necessari. Per altre informazioni sugli script di SQL Server, vedere [documentazione Online di SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> **Nota sulla sicurezza** nello script viene `db_owner` autorizzazioni all'utente che accede al database in fase di esecuzione, che è ciò che è possibile nell'ambiente di produzione. In alcuni scenari, è possibile specificare un utente che contiene lo schema completo del database aggiornare le autorizzazioni solo per la distribuzione e specificare in fase di esecuzione di un utente diverso che disponga delle autorizzazioni solo per leggere e scrivere dati. Per altre informazioni, vedere [rivedono le modifiche automatiche di Web. config per le migrazioni Code First](#reviewingmigrations) più avanti in questa esercitazione.

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Eseguire lo script di concessione del database dell'applicazione

È possibile configurare il profilo di pubblicazione per eseguire lo script di concessione nel database delle appartenenze durante la distribuzione perché la distribuzione del database utilizza il [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) provider. È possibile eseguire script durante la distribuzione di migrazioni Code First, ovvero come si distribuisce il database dell'applicazione. Ciò significa che è necessario eseguire manualmente lo script prima della distribuzione nel database dell'applicazione.

1. In Visual Studio, aprire il *Grant.sql* file creato in precedenza.

2. Selezionare **connettere**. 

    ![Pulsante di connessione](deploying-to-iis/_static/image11.png)

3. Nel **Connetti al Server** finestra di dialogo immettere *. \SQLExpress* come il **nome Server**. Selezionare **connettere**.

4. Nell'elenco a discesa dei database selezionare **ContosoUniversity**. Selezionare **Execute**. 

   ![](deploying-to-iis/_static/image12.png)

A questo punto, l'identità del pool predefinito abbia autorizzazioni sufficienti nel database dell'applicazione per le migrazioni Code First creare le tabelle di database quando viene eseguita l'applicazione.

## <a name="publish-to-iis"></a>Eseguire la pubblicazione in IIS

Esistono diversi modi, che è possibile distribuire IIS usando Visual Studio e distribuzione Web:

* Usare Visual Studio, pubblicare un solo clic.
* Pubblicare dalla riga di comando.
* Creare un pacchetto di distribuzione e installarlo mediante Gestione IIS. Il pacchetto dispone di un file con estensione zip con tutti i file e i metadati necessari per installare un sito in IIS.
* Creare un pacchetto di distribuzione e installarlo dalla riga di comando.

Il processo che è stato eseguito nelle esercitazioni precedenti per configurare Visual Studio per automatizzare le attività di distribuzione si applica a tutti questi metodi. In queste esercitazioni, userai i primi due metodi. Per informazioni sull'uso di pacchetti di distribuzione, vedere [distribuzione di un'applicazione web creando e installando un pacchetto di distribuzione web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) nella mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET.

Prima della pubblicazione, assicurarsi che si sta eseguendo Visual Studio in modalità amministratore. Se non viene visualizzata **(amministratore)** nella barra del titolo, chiudere Visual Studio. In Windows 8 (o versione successiva) **avviare** pagina o Windows 7 **avviare** dal menu, fare doppio clic sull'icona di Visual Studio e selezionare **Esegui come amministratore**. È solo in modalità amministratore necessarie per la pubblicazione quando esegue la pubblicazione in IIS nel computer locale.

### <a name="create-the-publish-profile"></a>Creare il profilo di pubblicazione

1. Nella **Esplora soluzioni**, fare doppio clic il **ContosoUniversity** progetto (non il **ContosoUniversity.DAL** progetto). Selezionare **Pubblica**. Il **pubblica** verrà visualizzata la pagina.

2. Selezionare **nuovo profilo**. Il **selezionare una destinazione di pubblicazione** verrà visualizzata la finestra di dialogo.

3. Selezionare **IIS, FTP, e così via**. Selezionare **Crea profilo**. Il **pubblica** procedura guidata viene visualizzata.

   ![Scheda del Web Creazione guidata profilo di pubblicazione](deploying-to-iis/_static/image26.png)

4. Dal **metodo di pubblicazione** elenco a discesa dal menu **distribuzione Web**.

5. Per la **Server**, immettere *localhost*.

6. Per la **nomesito**, immettere *Default Web Site/ContosoUniversity*.

7. Per la **URL di destinazione**, immettere *http://localhost/ContosoUniversity*.

   Il **URL di destinazione** impostazione non è obbligatorio. Quando Visual Studio ha completato la distribuzione dell'applicazione, venga automaticamente aperto il browser predefinito usando l'URL. Se non si desidera il browser per aprire automaticamente dopo la distribuzione, lasciare vuota questa casella.

8. Selezionare **convalida connessione** per verificare che le impostazioni siano corrette e che è possibile connettersi a IIS nel computer locale.

   Un segno di spunta verde verifica che la connessione ha esito positivo.

   ![Scheda Connessione guidata Web di pubblicazione](deploying-to-iis/_static/image27.png)

9. Selezionare **successivo** per passare alle **impostazioni** scheda.

10. Il **configurazione** casella di riepilogo a discesa consente di specificare la configurazione della build per la distribuzione. Non modificare l'impostazione il valore predefinito **rilascio**. In questa esercitazione si non distribuire le build di Debug.

11. Espandere **Opzioni pubblicazione File**. Selezionare **escludere i file dall'App\_cartella dati**.

    Nell'ambiente di test, l'applicazione accede ai database che è stato creato in SQL Server Express istanza locale, non i file con estensione mdf nel *App\_dati* cartella.

12. Lasciare il **precompila durante la pubblicazione** e **Rimuovi file aggiuntivi nella destinazione** deselezionate le caselle di controllo.

    ![Opzioni pubblicazione file nella scheda Impostazioni](deploying-to-iis/_static/image15a.png)

    La precompilazione è un'opzione che risulta utile principalmente per i siti di grandi dimensioni. È possibile ridurre il tempo di avvio alla prima che richiesta di una pagina dopo che il sito è pubblicato.

    Non è necessario rimuovere i file aggiuntivi, poiché si tratta della prima distribuzione e non saranno tutti i file nella cartella di destinazione ancora.

    > [!NOTE] 
    > Se si seleziona **Rimuovi file aggiuntivi nella destinazione** per una distribuzione successivi allo stesso sito, verificare che si usino la funzionalità di anteprima che consente di visualizzare in anticipo quali file verranno eliminati prima di distribuire. Il comportamento previsto è che distribuzione Web eliminerà i file nel server di destinazione che sono stati eliminati nel progetto. Tuttavia, viene confrontata con l'intera struttura delle cartelle in cartelle di origine e destinazione; e in alcuni scenari di distribuzione Web potrebbe eliminare i file che non si desidera eliminare.
    > 
    > Ad esempio se si dispone di un'applicazione web in una sottocartella nel server quando si distribuisce un progetto nella cartella radice, la sottocartella verrà eliminata. Si può avere un unico progetto per il sito primario in contoso.com e un altro progetto per un blog all'indirizzo contoso.com /blog. L'applicazione di blog è in una sottocartella. Se si seleziona **Rimuovi file aggiuntivi nella destinazione** quando si distribuisce il sito principale, l'applicazione di blog verrà eliminato.
    > 
    > Per un altro esempio, l'App\_cartella dati vengano eliminata in modo imprevisto. Alcuni database, ad esempio SQL Server Compact archiviano file di database nell'App\_cartella dati. Dopo la distribuzione iniziale, non si vuole mantenere la copia i file di database nelle distribuzioni successive, in modo da selezionare **escludere App\_dati** nella scheda Pubblicazione/creazione pacchetto Web. Al termine dell'operazione che, se hai **Rimuovi file aggiuntivi nella destinazione** selezionata, i file di database e l'App\_cartella dati verrà eliminato alla successiva pubblicazione.

### <a name="configure-deployment-for-the-membership-database"></a>Configurare la distribuzione per il database di appartenenza

I passaggi seguenti si applicano i **DefaultConnection** database nella finestra di dialogo **database** sezione.

1. Nel **stringa di connessione remota** immettere la seguente stringa di connessione che punti al nuovo database di appartenenze di SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   Il processo di distribuzione inserisce la stringa di connessione nel file Web. config distribuito in quanto **utilizzare questa stringa di connessione in fase di esecuzione** sia selezionata.

    È anche possibile ottenere la stringa di connessione **Esplora Server**. Nelle **Esplora Server**, espandere **connessioni dati** e selezionare il  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** database, quindi dal **delle proprietà** copia finestra la **stringa di connessione** valore. Che la stringa di connessione avrà un'impostazione altra che è possibile eliminare: `Pooling=False`.

2. Selezionare **aggiornare il database**.

   In questo modo lo schema del database da creare nel database di destinazione durante la distribuzione. Nei passaggi successivi si specificare gli script aggiuntivi che è necessario eseguire: uno per concedere l'accesso al database al pool di applicazioni predefinito e uno per la distribuzione dei dati.

3. Selezionare **Configura Aggiornamenti database**.
 
4. Nel **configurare gli aggiornamenti del Database** finestra di dialogo **Aggiungi Script SQL**. Passare il *Grant.sql* script salvato in precedenza nella cartella della soluzione.

5. Ripetere il processo per aggiungere il *aspnet-data-dev.sql* script.

   ![Configurare gli aggiornamenti di Database per database di appartenenza](deploying-to-iis/_static/image16.png)

6. Selezionare **Chiudi**.

### <a name="configure-deployment-for-the-application-database"></a>Configurare la distribuzione per il database dell'applicazione

Quando Visual Studio rileva un Entity Framework `DbContext` (classe), viene creata una voce nel **database** sezione che presenta un' **Esegui migrazioni Code First** casella di controllo anziché un  **Aggiornare Database** casella di controllo. Per questa esercitazione si userà tale casella di controllo per specificare la distribuzione di migrazioni Code First.

In alcuni scenari, è possibile utilizzare un `DbContext` database ma si vuole usare il provider dbDacFx anziché le migrazioni per distribuire il database. In tal caso, vedere [come si distribuisce un database senza le migrazioni Code First?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) nelle domande frequenti distribuzione Web ASP.NET su MSDN.

I passaggi seguenti si applicano i **SchoolContext** database nella finestra di dialogo **database** sezione.

1. Nel **stringa di connessione remota** immettere la seguente stringa di connessione che punti al nuovo database dell'applicazione SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   Il processo di distribuzione inserisce la stringa di connessione nel file Web. config distribuito in quanto **utilizzare questa stringa di connessione in fase di esecuzione** sia selezionata.

   È anche possibile ottenere la stringa di connessione di database dell'applicazione **Esplora Server** nello stesso modo è stato ottenuto la stringa di connessione di database di appartenenza.

2. Selezionare **Esegui migrazioni Code First (inizia all'avvio dell'applicazione)**.

   Questa opzione fa sì che il processo di distribuzione configurare il file Web. config distribuito di specificare il `MigrateDatabaseToLatestVersion` inizializzatore. Quando l'applicazione accede al database per la prima volta dopo la distribuzione, questo inizializzatore Aggiorna automaticamente il database alla versione più recente.

### <a name="configure-publish-profile-transforms"></a>Configurare le trasformazioni di profilo di pubblicazione

1. Selezionare **Chiudi**. Selezionare **Sì** quando viene chiesto se si desidera salvare le modifiche.

2. Nelle **Esplora soluzioni**, espandere **delle proprietà**, espandere **PublishProfiles**.

3. Fare doppio clic su *CustomProfile.pubxml* e rinominarlo *Test.pubxml*.

4. Fare doppio clic su *Test.pubxml*. Selezionare **Aggiungi trasformazione di configurazione**.

   ![Aggiungere il menu di trasformazione di configurazione](deploying-to-iis/_static/image17.png)

   Visual Studio crea il *Web.Test.config* file di trasformazione e lo apre.

5. Nel *Web.Test.config* trasforma il file, inserire il codice seguente immediatamente dopo l'apertura tag di configurazione.

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Quando si usa il Test del profilo di pubblicazione, questa trasformazione imposta l'indicatore di ambiente "Test". Nel sito distribuito, verrà visualizzato "(Test)" dopo l'intestazione H1 "Contoso University".

6. Salvare e chiudere il file.

7. Fare doppio clic il *Web.Test.config* del file e selezionare **anteprima trasformare** per assicurarsi che la trasformazione si codificano produce le modifiche previste.

    Il **anteprima Web. config** finestra Mostra il risultato dell'applicazione sia la *Release* Trasforma e il *Web.Test.config* Trasforma.

### <a name="preview-the-deployment-updates"></a>Visualizzare in anteprima gli aggiornamenti di distribuzione

1. Aprire il **pubblica sul Web** Creazione guidata nuovo (fare clic sul progetto ContosoUniversity, selezionare **Publish**, quindi **anteprima**).

2. Nel **Preview** finestra di dialogo **Avvia anteprima** per visualizzare un elenco dei file che verranno copiati. 

   ![Anteprima di pubblicazione](deploying-to-iis/_static/image29.png)

   È anche possibile selezionare i **anteprima database** collegamento per visualizzare gli script che verranno eseguito nel database delle appartenenze. (Non gli script vengono eseguiti per la distribuzione di migrazioni Code First, in modo che nessun elemento da visualizzare in anteprima per il database dell'applicazione.)

3. Selezionare **Pubblica**.

   Se Visual Studio non è in modalità amministratore, si potrebbe ottenere un messaggio di errore di autorizzazioni. In tal caso, chiudere Visual Studio, aprirlo in modalità amministratore e provare a eseguire nuovamente la pubblicazione.

   Se Visual Studio è in modalità amministratore, il **Output** finestra report ha esito positivo, compilare e pubblicare.

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   Se è stato immesso l'URL nel **URL di destinazione** finestra nel profilo di pubblicazione **connessione** scheda nel browser verrà aperta automaticamente alla pagina principale di Contoso University in esecuzione in IIS nel computer.

## <a name="test-in-the-test-environment"></a>Test nell'ambiente di test

Si noti che l'indicatore di ambiente Visualizza "(Test)" anziché "(Dev)," che mostra che il *Web. config* trasformazione per l'indicatore di ambiente ha avuto esito positivo.

Eseguire la **instructors (insegnanti)** pagina per verificare che Code First effettuato il seeding del database con dati insegnante. Quando si seleziona questa pagina, potrebbe volerci alcuni minuti per caricare perché Code First consente di creare il database e quindi esegue il `Seed` (metodo). (Non che quando erano nella home page, poiché l'applicazione non tenta di accedere al database ancora.)

Selezionare il **studenti** pressione di tab per verificare che il database distribuito non disponga di alcun studenti.

Selezionare **aggiungere gli studenti** dalle **studenti** menu. Aggiungere uno studente e quindi visualizzare il nuovo studente nella **studenti** pagina. In questo modo si verifica che è possibile scrivere nel database.

Dal **corsi** dal menu **crediti Update**. Il **aggiornamento crediti** pagina richiede autorizzazioni di amministratore, in modo che il **Accedi** viene visualizzata la pagina. Immettere le credenziali dell'account amministratore creato precedenti ("admin" e "devpwd"). Il **crediti Update** viene visualizzata la pagina. In questo modo si verifica che l'account amministratore creato nell'esercitazione precedente è stato distribuito correttamente nell'ambiente di test.

Verificare che un *ELMAH* esiste la cartella le *c:\inetpub\wwwroot\ContosoUniversity* con solo il file segnaposto nella relativa cartella.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Esaminare le modifiche automatiche di Web. config per le migrazioni Code First

Aprire il *Web. config* file nell'applicazione distribuita *C:\inetpub\wwwroot\ContosoUniversity* ed è possibile visualizzare dove il processo di distribuzione configurato le migrazioni Code First per automaticamente aggiornare il database alla versione più recente.

![](deploying-to-iis/_static/image21.png)

Il processo di distribuzione creato anche una nuova stringa di connessione per le migrazioni Code First usare esclusivamente per l'aggiornamento dello schema del database:

![Stringa di connessione Database_Publish](deploying-to-iis/_static/image22.png)

Questa stringa di connessione aggiuntive consente di specificare un account utente per gli aggiornamenti dello schema di database e un account utente diverso per accedere ai dati dell'applicazione. Ad esempio, è possibile assegnare il **db\_proprietario** ruolo per le migrazioni Code First e **db\_datareader** con **db\_datawriter**ruoli all'applicazione. Si tratta di un modello comune di difesa in profondità che impedisce a codice potenzialmente dannoso dell'applicazione di modificare lo schema del database. (Ad esempio, questa situazione può verificarsi in un attacco injection SQL.) Queste esercitazioni non usano questo modello. Per implementare questo modello nello scenario corrente, eseguire questi passaggi:

1. Nel **pubblica sul Web** procedura guidata sotto il **impostazioni** , immettere la stringa di connessione che specifica l'utente con autorizzazioni per l'aggiornamento dello schema completo del database. Cancella il **utilizzare questa stringa di connessione in fase di esecuzione** casella di controllo. Nel file Web. config distribuito, questo diventa il `DatabasePublish` stringa di connessione.

2. Creare una trasformazione del file Web. config per la stringa di connessione che si desidera che l'applicazione da usare in fase di esecuzione.

## <a name="summary"></a>Riepilogo

È stata distribuita l'applicazione in IIS nel computer di sviluppo ed testato non esiste.

![Home page di Test](deploying-to-iis/_static/image23.png)

Si verifica che il processo di distribuzione copiato il contenuto dell'applicazione nel percorso corretto (esclusi i file non desiderati per la distribuzione) e anche tale distribuzione Web configurato IIS correttamente durante la distribuzione. Nella prossima esercitazione, si eseguirà un test più che consente di trovare un'attività di distribuzione che non è ancora stato fatto: impostazione delle autorizzazioni di cartella per il *Elm ah* cartella.

## <a name="more-information"></a>Altre informazioni

Per informazioni sull'esecuzione di IIS o IIS Express in Visual Studio, vedere le risorse seguenti:

- [Panoramica di IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) sul sito IIS.net.
- [Introduzione a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) sul blog Guthrie.
- [Web server in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Principali differenze tra IIS e ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) sul sito ASP.NET.

Per informazioni su quali problemi potrebbero verificarsi quando l'applicazione viene eseguita in attendibilità media, vedere [che ospita le applicazioni ASP.NET in attendibilità media](http://www.4guysfromrolla.com/articles/100307-1.aspx) sui quattro utenti malintenzionati Rolla sito.

> [!div class="step-by-step"]
> [Precedente](project-properties.md)
> [Successivo](setting-folder-permissions.md)
