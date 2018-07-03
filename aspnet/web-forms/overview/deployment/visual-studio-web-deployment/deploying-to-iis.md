---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Distribuzione Web ASP.NET tramite Visual Studio: distribuzione per il Test | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 8c5034dd4948d96c5722e2dcc960cc0349241a1a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365685"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di test
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come distribuire un'applicazione web ASP.NET in IIS nel computer locale.

Quando si sviluppa un'applicazione, in genere testare eseguendolo in Visual Studio. Per impostazione predefinita, i progetti applicazione web in Visual Studio 2012 usano IIS Express come server web di sviluppo. IIS Express è più simile a IIS completo rispetto di Visual Studio Development Server (noto anche come Cassini), che Usa Visual Studio 2010 per impostazione predefinita. Tuttavia, nessuno dei due server web di sviluppo funziona esattamente come IIS. Di conseguenza, è possibile che un'applicazione verrà eseguita correttamente quando si eseguirne il test in Visual Studio, ma non riesce quando viene distribuito a IIS.

È possibile testare l'applicazione in modo più affidabile nei modi seguenti:

1. Distribuire l'applicazione in IIS nel computer di sviluppo utilizzando la stessa procedura che si userà in un secondo momento per la distribuzione in ambiente di produzione. È possibile configurare Visual Studio per utilizzare IIS quando si esegue un progetto web, ma questa operazione non testa il processo di distribuzione. Questo metodo convalida il processo di distribuzione, oltre a verificare che l'applicazione verrà eseguita correttamente in IIS.
2. Distribuire l'applicazione in un ambiente di test che è quasi identico all'ambiente di produzione. Poiché l'ambiente di produzione per queste esercitazioni le app Web nel servizio App di Azure, l'ambiente di test ideale è un'app web aggiuntive creata nel servizio App di Azure. Utilizzare questa seconda app web solo per i test, ma sia necessario configurare esattamente come l'app web di produzione.

Opzione 2 è il modo più affidabile per eseguire il test e, tal caso, non necessariamente hai all'opzione 1. Tuttavia, se si distribuisce in un'opzione di provider hosting terze parti 2 potrebbe non essere fattibile o potrebbe essere costosa, pertanto questa serie di esercitazioni illustra entrambi i metodi. Vengono fornite istruzioni per l'opzione 2 nel [distribuzione nell'ambiente di produzione](deploying-to-production.md) esercitazione.

Per altre informazioni sull'uso di server web in Visual Studio, vedere [server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](troubleshooting.md).

## <a name="install-iis"></a>Installare IIS

Per distribuire in IIS nel computer di sviluppo, è necessario che IIS e distribuzione Web installati. La funzionalità distribuzione Web è installata per impostazione predefinita con Visual Studio, ma IIS non è incluso nella configurazione predefinita di Windows 8 o Windows 7. Se IIS è già stato installato e il pool di applicazioni predefinito è già impostato su .NET 4, andare al [nella sezione successiva](#sqlexpress).

1. Usando il [instalace Webové Platformy](https://www.microsoft.com/web/downloads/platform.aspx) è il modo migliore per installare IIS e distribuzione Web, perché l'installazione guidata piattaforma Web installa una configurazione consigliata per IIS e installa automaticamente i prerequisiti per IIS e Web La distribuzione se necessario.

    Per eseguire l'installazione guidata piattaforma Web per installare IIS e distribuzione Web, usare il collegamento seguente. Se è già stato installato IIS, distribuzione Web o dei relativi componenti necessari, l'installazione guidata piattaforma Web installa solo cosa manca.

   - [Installare IIS e distribuzione Web tramite WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     Si vedrà messaggi che indicano che verrà installato IIS 7. Il funzionamento di collegamento per IIS 8 in Windows 8, ma per Windows 8 assicurarsi che sia installato ASP.NET 4.5 attenendosi alla procedura seguente:

   - Aprire **Pannello di controllo**, **programmi e funzionalità**, **o disattivazione delle funzionalità Windows attivare**.
   - Espandere **Internet Information Services**, **servizi World Wide Web**, e **funzionalità per lo sviluppo dell'applicazione**.
   - Verificare che l'opzione **ASP.NET 4.5** sia selezionata.

      ![Selezionare ASP.NET 4.5](deploying-to-iis/_static/image1.png)

Dopo aver installato IIS, eseguire **Gestione IIS** per assicurarsi che .NET Framework versione 4 sia assegnato al pool di applicazioni predefinito.

1. Premere WINDOWS + R per aprire la **eseguire** nella finestra di dialogo.

    (O in Windows 8 consente di immettere "run" **avviare** pagina, o Windows 7, selezionare **eseguire** dal **avviare** menu. Se **eseguiti** non è presente nel **avviare** menu, pulsante destro del mouse la barra delle applicazioni, fare clic su **proprietà**, selezionare il **Menu Start** scheda, fare clic su **Personalizza**e selezionare **eseguire comando**.)
2. Immettere "inetmgr" e quindi fare clic su **OK**.
3. Nel **connessioni** riquadro, espandere il nodo del server e selezionare **pool di applicazioni**. Nel **pool di applicazioni** riquadro, se **DefaultAppPool** viene assegnato a .NET framework versione 4 come illustrato nella figura seguente, passare alla sezione successiva.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Se viene visualizzato solo due pool di applicazioni e di entrambi gli elementi configurati per .NET Framework 2.0, è necessario installare ASP.NET 4 in IIS.

    Per Windows 8, vedere le istruzioni nella precedente sezione per verificare che ASP.NET 4.5 è installato oppure vedere [questo articolo della Knowledge Base](https://support.microsoft.com/kb/2736284). Per Windows 7, aprire una finestra del prompt dei comandi facendo clic **prompt dei comandi** nella finestra di Windows **avviare** dal menu e selezionando **Esegui come amministratore**. Quindi eseguire [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) installare ASP.NET 4 in IIS, usando i comandi seguenti. (In sistemi a 32 bit, sostituire "Framework64" con "Framework").

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    Questo comando Crea nuovo pool di applicazioni per .NET Framework 4, ma il pool di applicazioni predefinito verrà ancora impostato su 2.0. Verrà distribuita un'applicazione destinata a .NET 4 per tale pool di applicazioni, pertanto è necessario modificare il pool di applicazioni in .NET 4.
5. Se è stato chiuso **Gestione IIS**eseguirlo di nuovo, espandere il nodo del server e fare clic su **pool di applicazioni** per visualizzare il **pool di applicazioni** nuovo riquadro.
6. Nel **pool di applicazioni** riquadro, fare clic su **DefaultAppPool**, quindi nel **azioni** riquadro fare clic su **le impostazioni di base**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. Nel **modifica Pool di applicazioni** della finestra di dialogo Modifica **versione di .NET Framework** al **.NET Framework v4.0.30319** e fare clic su **OK**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS è ora pronti per pubblicare un'applicazione web ad esso, ma prima è possibile farlo è necessario creare i database che verrà utilizzato nell'ambiente di test.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Installare SQL Server Express

Local DB non è progettato per funzionare in IIS, pertanto per l'ambiente di test è necessario avere installato SQL Server Express. Se si usa Visual Studio 2010 SQL Server Express è già installato per impostazione predefinita. Se si usa Visual Studio 2012, è necessario installarlo.

Per installare SQL Server Express, installarlo dalla [area Download Microsoft: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) facendo clic [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) o [ ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Se si sceglie quello errato per il sistema avrà esito negativo installare e provare a effettuare un altro.

Nella prima pagina del Centro installazione SQL Server, fare clic su **nuova installazione SQL Server autonomo o aggiungere caratteristiche a un'installazione esistente**e seguire le istruzioni, accettando le scelte predefinite. Nell'installazione guidata accettare le impostazioni predefinite. Per altre informazioni sulle opzioni di installazione, vedere [installare SQL Server 2012 dall'installazione guidata (programma di installazione)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Creare i database di SQL Server Express per l'ambiente di test

L'applicazione Contoso University è disponibili due database: il database di appartenenza e il database dell'applicazione. È possibile distribuire questi database a due distinti database o a un singolo database. È possibile combinarli per facilitare il join di database tra il database dell'applicazione e il database di appartenenza. Se si distribuisce in un provider di hosting di terze parti, il piano di hosting può anche fornire un motivo per combinarle. Ad esempio, il provider di hosting può addebitare informazioni per più database oppure potrebbe non consentire anche di più di un database.

In questa esercitazione, si distribuiranno due database nell'ambiente di test di un database in ambienti di staging e produzione.

Dal **View** dal menu in Visual Studio seleziona **Esplora Server** (**Esplora Database** in Visual Web Developer) e quindi fare doppio clic su **connessioni dati**  e selezionare **Crea nuovo Database di SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

Nel **Crea nuovo Database di SQL Server** finestra di dialogo immettere ". \SQLExpress" nel **nome Server** casella e "aspnet-ContosoUniversity" nel **nuovo nome del database** casella, quindi Fare clic su **OK**.

![Creare aspnet-ContosoUniversity](deploying-to-iis/_static/image9.png)

Seguire la stessa procedura per creare un nuovo database di SQL Server Express School denominato "ContosoUniversity".

**Esplora server** ora mostra i due nuovi database.

![Nuovo database in Esplora Server](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Creare uno script di concessione per i nuovi database

Quando l'applicazione viene eseguita in IIS nel computer di sviluppo, l'applicazione accede al database usando le credenziali del pool di applicazioni predefinito. Tuttavia, per impostazione predefinita, l'identità del pool di applicazioni non dispone dell'autorizzazione per aprire i database. Pertanto, è necessario eseguire uno script per concedere tale autorizzazione. In questa sezione si crea lo script che verrà eseguito successivamente per assicurarsi che l'applicazione può aprire i database quando è in esecuzione in IIS.

Visualizzare la soluzione (non uno dei progetti), quindi scegliere **Aggiungi nuovo elemento**e quindi creare un nuovo **File SQL** denominato *Grant.sql*. Copiare i comandi SQL seguenti nel file, quindi salvare e chiudere il file:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Questo script è progettato per funzionare con SQL Server Express 2012 e con le impostazioni di IIS in Windows 8 o Windows 7, secondo quanto specificato in questa esercitazione. Se si usa una versione diversa di SQL Server o di Windows, o se si configura IIS nel computer in modo diverso, le modifiche apportate allo script potrebbero essere necessari. Per altre informazioni sugli script di SQL Server, vedere [documentazione Online di SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Nota sulla sicurezza** nello script viene db\_autorizzazioni di proprietario per l'utente che accede al database in fase di esecuzione, che è ciò che è possibile nell'ambiente di produzione. In alcuni scenari è possibile specificare un utente che contiene lo schema completo del database, aggiornare le autorizzazioni solo per la distribuzione e specificare in fase di esecuzione di un utente diverso che disponga delle autorizzazioni solo per leggere e scrivere dati. Per altre informazioni, vedere [rivedono le modifiche automatiche di Web. config per le migrazioni Code First](#reviewingmigrations) più avanti in questa esercitazione.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Eseguire lo script di concessione del database dell'applicazione

È possibile configurare il profilo di pubblicazione per eseguire lo script di concessione nel database di appartenenza durante la distribuzione perché la distribuzione del database Usa il provider dbDacFx. È possibile eseguire script durante la distribuzione di migrazioni Code First, ovvero come si distribuisce il database dell'applicazione. Pertanto, è necessario eseguire manualmente lo script prima della distribuzione nel database dell'applicazione.

1. In Visual Studio, aprire il *Grant.sql* file creato in precedenza.
2. Fare clic su **Connetti**. 

    ![Pulsante di connessione](deploying-to-iis/_static/image11.png)
3. Nel **Connetti al Server** finestra di dialogo immettere *. \SQLExpress* come il **nome del Server**e quindi fare clic su **Connect**.
4. Nell'elenco a discesa dei database selezionare **ContosoUniversity**, quindi fare clic su **Execute**. 

    ![](deploying-to-iis/_static/image12.png)

A questo punto, l'identità del pool predefinito abbia autorizzazioni sufficienti nel database dell'applicazione per le migrazioni Code First creare le tabelle di database quando viene eseguita l'applicazione.

## <a name="publish-to-iis"></a>Pubblicazione in IIS

Esistono diversi modi, che è possibile distribuire IIS usando Visual Studio e distribuzione Web:

- Usare Visual Studio, pubblicare un solo clic.
- Pubblicare dalla riga di comando.
- Creare un *pacchetto di distribuzione* e installarlo utilizzando la UI Gestione IIS. Il pacchetto di distribuzione è costituito un *zip* file che contiene tutti i file e i metadati necessari per installare un sito in IIS.
- Creare un pacchetto di distribuzione e installarlo dalla riga di comando.

Il processo che è stato eseguito nelle esercitazioni precedenti per configurare Visual Studio per automatizzare le attività di distribuzione si applica a tutti questi metodi. In queste esercitazioni si userà i primi due di questi metodi. Per informazioni sull'uso di pacchetti di distribuzione, vedere [distribuzione di un'applicazione web creando e installando un pacchetto di distribuzione web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) nella mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET.

Prima della pubblicazione, assicurarsi che si esegue Visual Studio in modalità amministratore. Se non viene visualizzata **(amministratore)** nella barra del titolo, chiudere Visual Studio. In Windows 8 **avviare** pagina o Windows 7 **avviare** menu, fare doppio clic sull'icona per la versione di Visual Studio in uso e selezionare **Esegui come amministratore**. Modalità amministratore è necessaria per la pubblicazione solo quando esegue la pubblicazione in IIS nel computer locale.

### <a name="create-the-publish-profile"></a>Creare il profilo di pubblicazione

1. Nelle **Esplora soluzioni**, fare clic sul progetto ContosoUniversity (non sul progetto ContosoUniversity.DAL) e selezionare **Publish**.

    Il **pubblica sul Web** procedura guidata viene visualizzata.

    ![Scheda del Web Creazione guidata profilo di pubblicazione](deploying-to-iis/_static/image13.png)
2. Nell'elenco a discesa, selezionare  **&lt;New... &gt;**. (Con gli ultimi aggiornamenti di Visual Studio installato, non sono presenti elenchi a discesa e il pulsante di fare clic su per creare un nuovo profilo da zero **Custom**.)
3. Nel **nuovo profilo** della finestra di dialogo immettere "Test" e quindi fare clic su **OK**.

    La procedura guidata passa automaticamente per il **connessione** scheda.
4. Nel **URL del servizio** casella, immettere *localhost*.
5. Nel **sito/applicazione** immettere *ContosoUniversity/sito Web predefinito*
6. Nel **URL di destinazione** immettere `http://localhost/ContosoUniversity`

    Il **URL di destinazione** impostazione non è obbligatorio. Quando Visual Studio ha completato la distribuzione dell'applicazione, venga automaticamente aperto il browser predefinito usando l'URL. Se non si desidera il browser per aprire automaticamente dopo la distribuzione, lasciare vuota questa casella.
7. Fare clic su **convalida connessione** per verificare che le impostazioni siano corrette e che è possibile connettersi a IIS nel computer locale.

    Un segno di spunta verde verifica che la connessione ha esito positivo.

    ![Scheda Connessione guidata Web di pubblicazione](deploying-to-iis/_static/image14.png)
8. Fare clic su **successivo** per passare alle **impostazioni** scheda.
9. Il **configurazione** casella di riepilogo a discesa consente di specificare la configurazione della build per la distribuzione. Lasciare impostato sul valore predefinito della versione. In questa esercitazione si non distribuire le build di Debug.
10. Espandere **Opzioni pubblicazione File**, quindi selezionare **escludere i file dall'App\_cartella dati**.

    Nell'ambiente di test l'applicazione accederà i database creato nell'istanza locale di SQL Server Express, non il file con estensione mdf i file nei *App\_dati* cartella.
11. Lasciare il **precompila durante la pubblicazione** e **Rimuovi file aggiuntivi nella destinazione** deselezionate le caselle di controllo.

    ![Opzioni pubblicazione file nella scheda Impostazioni](deploying-to-iis/_static/image15.png)

    La precompilazione è un'opzione che risulta utile principalmente per siti molto grandi. è possibile ridurre i tempi di avvio pagina per la prima volta che viene richiesta una pagina dopo che il sito è pubblicato.

    Non è necessario rimuovere i file aggiuntivi, poiché si tratta della prima distribuzione e non saranno tutti i file nella cartella di destinazione ancora.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Se si seleziona **Rimuovi file aggiuntivi** per una distribuzione successivi allo stesso sito, verificare che si usino la funzionalità di anteprima che consente di visualizzare in anticipo quali file verranno eliminati prima di distribuire. Il comportamento previsto è che distribuzione Web eliminerà i file nel server di destinazione che sono stati eliminati nel progetto. Tuttavia, viene confrontata con l'intera struttura delle cartelle in cartelle di origine e destinazione e in alcuni scenari di distribuzione Web potrebbe eliminare i file che non si desidera eliminare.
    > 
    > Ad esempio, se si dispone di un'applicazione web in una sottocartella nel server quando si distribuisce un progetto nella cartella radice, la sottocartella verrà eliminata. Si può avere un unico progetto per il sito primario in contoso.com e un altro progetto per un blog all'indirizzo contoso.com /blog. L'applicazione di blog è in una sottocartella. Se si seleziona Rimuovi file aggiuntivi nella destinazione quando si distribuisce il sito principale, l'applicazione di blog verrà eliminato.
    > 
    > Per un altro esempio, l'App\_cartella dati vengano eliminata in modo imprevisto. Alcuni database, ad esempio SQL Server Compact archiviano file di database nell'App\_cartella dati. Dopo la distribuzione iniziale non si vuole mantenere la copia i file di database nelle distribuzioni successive in modo da selezionare escludere App\_dati nella scheda Pubblicazione/creazione pacchetto Web. Al termine dell'operazione che, se hai Rimuovi file aggiuntivi nella destinazione selezionata, i file di database e l'App\_cartella dati verrà eliminato alla successiva pubblicazione.

### <a name="configure-deployment-for-the-membership-database"></a>Configurare la distribuzione per il database di appartenenza

I passaggi seguenti si applicano i **DefaultConnection** del database nel **database** sezione della finestra di dialogo.

1. Nel **stringa di connessione remota** immettere la seguente stringa di connessione che punti al nuovo database di appartenenze di SQL Server Express.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    Il processo di distribuzione inserirà questa stringa di connessione nel file Web. config distribuito in quanto **utilizzare questa stringa di connessione in fase di esecuzione** sia selezionata.

    È anche possibile ottenere la stringa di connessione **Esplora Server**. Nelle **Esplora Server**, espandere **connessioni dati** e selezionare il  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** database, quindi dal **delle proprietà** copia finestra la **stringa di connessione** valore. Che la stringa di connessione avrà un'impostazione altra che è possibile eliminare: `Pooling=False`.
2. Selezionare **aggiornare il database**.

    In questo modo lo schema del database da creare nel database di destinazione durante la distribuzione. Nei passaggi seguenti specificare gli script aggiuntivi che è necessario eseguire: uno per concedere l'accesso al database al pool di applicazioni predefinito e uno per la distribuzione dei dati.
3. Fare clic su **Configura Aggiornamenti database**.
4. Nel **Configura Aggiornamenti Database** finestra di dialogo, fare clic su **Aggiungi Script SQL** e quindi passare al *Grant.sql* script salvato in precedenza nella cartella della soluzione.
5. Ripetere il processo per aggiungere il *aspnet-data-dev.sql* script.

    ![Configurare gli aggiornamenti di Database per database di appartenenza](deploying-to-iis/_static/image16.png)
6. Fare clic su **Chiudi**.

### <a name="configure-deployment-for-the-application-database"></a>Configurare la distribuzione per il database dell'applicazione

Quando Visual Studio rileva un Entity Framework `DbContext` (classe), viene creata una voce nel **database** sezione che presenta un' **Esegui migrazioni Code First** casella di controllo anziché un  **Aggiornare Database** casella di controllo. Per questa esercitazione si userà tale casella di controllo per specificare la distribuzione di migrazioni Code First.

In alcuni scenari, è possibile utilizzare un `DbContext` database ma si vuole usare il provider dbDacFx anziché le migrazioni per distribuire il database. In tal caso, vedere [come si distribuisce un database senza le migrazioni Code First?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) nelle domande frequenti distribuzione Web ASP.NET su MSDN.

I passaggi seguenti si applicano i **SchoolContext** del database nel **database** sezione della finestra di dialogo.

1. Nel **stringa di connessione remota** immettere la seguente stringa di connessione che punti al nuovo database dell'applicazione SQL Server Express.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    Il processo di distribuzione inserirà questa stringa di connessione nel file Web. config distribuito in quanto **utilizzare questa stringa di connessione in fase di esecuzione** sia selezionata.

    È anche possibile ottenere la stringa di connessione di database dell'applicazione **Esplora Server** stringa di connessione del database allo stesso modo è stato ottenuto l'appartenenza.
2. Selezionare **Esegui migrazioni Code First (inizia all'avvio dell'applicazione)**.

    Questa opzione fa sì che il processo di distribuzione configurare il file Web. config distribuito di specificare il `MigrateDatabaseToLatestVersion` inizializzatore. Quando l'applicazione accede al database per la prima volta dopo la distribuzione, questo inizializzatore Aggiorna automaticamente il database alla versione più recente.

### <a name="configure-publish-profile-transforms"></a>Configurare le trasformazioni di profilo di pubblicazione

1. Fare clic su **Close**, quindi fare clic su **Yes** quando viene chiesto se si desidera salvare le modifiche.
2. Nelle **Esplora soluzioni**, espandere **delle proprietà**, espandere **PublishProfiles**.
3. Rright clic *Test.pubxml,* e quindi fare clic su **aggiungere Config trasformare**.

    ![Aggiungere il menu di trasformazione di configurazione](deploying-to-iis/_static/image17.png)

    Visual Studio crea il *Web.Test.config* file di trasformazione e lo apre.
4. Nel *Web.Test.config* trasforma il file, inserire il codice seguente immediatamente dopo l'apertura tag di configurazione.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Quando si usa il Test del profilo di pubblicazione, questa trasformazione imposta l'indicatore di ambiente "Test". Nel sito distribuito verrà visualizzato "(Test)" dopo l'intestazione H1 "Contoso University".
5. Salvare e chiudere il file.
6. Fare doppio clic il *Web.Test.config* del file e fare clic su **anteprima trasformare** per assicurarsi che la trasformazione si codificano produce le modifiche previste.

    Il **anteprima Web. config** finestra Mostra il risultato dell'applicazione sia la *Release* Trasforma e il *Web.Test.config* Trasforma.

### <a name="preview-the-deployment-updates"></a>Visualizzare in anteprima gli aggiornamenti di distribuzione

1. Aprire il **pubblica sul Web** Creazione guidata nuovo (fare clic sul progetto ContosoUniversity e fare clic su **Publish**).
2. Nel **Preview** scheda, verificare che il **Test** profilo è ancora selezionato e quindi fare clic su **Avvia anteprima** per visualizzare un elenco dei file che verranno copiati.

    ![Pulsante Anteprima](deploying-to-iis/_static/image18.png)

    ![Anteprima di pubblicazione](deploying-to-iis/_static/image19.png)

    È anche possibile scegliere il **anteprima database** collegamento per visualizzare gli script che verranno eseguito nel database delle appartenenze. (Non gli script vengono eseguiti per la distribuzione di migrazioni Code First, in modo che nessun elemento da visualizzare in anteprima per il database dell'applicazione.)
3. Fare clic su **Pubblica**.

    Se Visual Studio non è in modalità amministratore, si potrebbe ottenere un messaggio di errore che indica un errore di autorizzazione. In tal caso, chiudere Visual Studio, aprirlo in modalità amministratore e provare a eseguire nuovamente la pubblicazione.

    Se Visual Studio è in modalità amministratore, il **Output** finestra report ha esito positivo, compilare e pubblicare.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    Se è stato immesso l'URL nel **URL di destinazione** finestra nel profilo di pubblicazione **connessione** scheda nel browser verrà aperta automaticamente alla pagina principale di Contoso University in esecuzione in IIS nel computer locale.

## <a name="test-in-the-test-environment"></a>Test nell'ambiente di test

Si noti che l'indicatore di ambiente Visualizza "(Test)" anziché "(Dev)", che mostra che il *Web. config* trasformazione per l'indicatore di ambiente ha avuto esito positivo.

Eseguire la **instructors (insegnanti)** pagina per verificare che Code First effettuato il seeding del database con dati insegnante. Quando si seleziona questa pagina, potrebbe volerci alcuni minuti per caricare perché Code First consente di creare il database e quindi esegue il `Seed` (metodo). (Non che quando erano nella home page, poiché l'applicazione non tenta di accedere al database ancora.)

Fare clic sui **studenti** pressione di tab per verificare che il database distribuito non disponga di alcun studenti.

Selezionare **aggiungere gli studenti** dal **studenti** dal menu Aggiungi uno studente e quindi visualizzare il nuovo studente nella **studenti** pagina per verificare che è possibile scrivere nel database .

Dal **corsi** dal menu **crediti Update**. Il **aggiornamento crediti** pagina richiede autorizzazioni di amministratore, in modo che il **Accedi** viene visualizzata la pagina. Immettere le credenziali dell'account amministratore creato precedenti ("admin" e "devpwd"). Il **crediti Update** pagina viene visualizzata, verifica che l'account amministratore creato nell'esercitazione precedente è stato distribuito correttamente nell'ambiente di test.

Verificare che un *Elmah* esiste la cartella le *c:\inetpub\wwwroot\ContosoUniversity* con solo il file segnaposto nella relativa cartella.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Esaminare le modifiche automatiche di Web. config per le migrazioni Code First

Aprire il *Web. config* file nell'applicazione distribuita *C:\inetpub\wwwroot\ContosoUniversity* ed è possibile visualizzare dove il processo di distribuzione configurato le migrazioni Code First per automaticamente aggiornare il database alla versione più recente.

![](deploying-to-iis/_static/image21.png)

Il processo di distribuzione creato anche una nuova stringa di connessione per le migrazioni Code First usare esclusivamente per l'aggiornamento dello schema del database:

![Stringa di connessione Database_Publish](deploying-to-iis/_static/image22.png)

Questa stringa di connessione aggiuntive consente di specificare un account utente per gli aggiornamenti dello schema di database e un account utente diverso per accedere ai dati dell'applicazione. Ad esempio, è possibile assegnare il **db\_proprietario** ruolo per le migrazioni Code First, e **db\_datareader** e **db\_datawriter**ruoli all'applicazione. Si tratta di un modello comune di difesa in profondità che impedisce a codice potenzialmente dannoso dell'applicazione di modificare lo schema del database. (Ad esempio, questa situazione può verificarsi in un attacco injection SQL.) Questo modello non viene utilizzato da queste esercitazioni. Se si desidera implementare questo modello nel proprio scenario, è possibile farlo attenendosi alla procedura seguente:

1. Nel **impostazioni** scheda della finestra di **Pubblica sito Web** procedura guidata, immettere la stringa di connessione che specifica l'utente con autorizzazioni per l'aggiornamento dello schema completo del database e deselezionare il **Usa questa stringa di connessione in fase di esecuzione** casella di controllo. Nel file Web. config distribuito, questo diventa il `DatabasePublish` stringa di connessione.
2. Creare una trasformazione del file Web. config per la stringa di connessione che si desidera che l'applicazione da usare in fase di esecuzione.

## <a name="summary"></a>Riepilogo

Abbiamo distribuito l'applicazione in IIS nel computer di sviluppo e testata non esiste.

![Home page di Test](deploying-to-iis/_static/image23.png)

Verifica che il processo di distribuzione copiato il contenuto dell'applicazione nel percorso corretto (esclusi i file non desiderati per la distribuzione) e anche tale distribuzione Web configurato IIS correttamente durante la distribuzione. Nella prossima esercitazione, si eseguirà un test più che consente di trovare un'attività di distribuzione che non è ancora stato fatto: impostazione delle autorizzazioni di cartella per il *Elmah* cartella.

## <a name="more-information"></a>Altre informazioni

Per informazioni sull'esecuzione di IIS o IIS Express in Visual Studio, vedere le risorse seguenti:

- [Panoramica di IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) sul sito IIS.net.
- [Introduzione a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) sul blog Guthrie.
- [Web server in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Principali differenze tra IIS e ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) sul sito ASP.NET.

Per informazioni su quali problemi potrebbero verificarsi quando l'applicazione viene eseguita in attendibilità media, vedere [che ospita le applicazioni ASP.NET in attendibilità media](http://www.4guysfromrolla.com/articles/100307-1.aspx) sui 4 utenti malintenzionati Rolla sito.

> [!div class="step-by-step"]
> [Precedente](project-properties.md)
> [Successivo](setting-folder-permissions.md)
