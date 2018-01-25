---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: distribuzione di Test | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 01f72e0240e84944f8ffece9a2dbc5802be4646b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: distribuzione di Test
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come distribuire un'applicazione web ASP.NET in IIS nel computer locale.

Quando si sviluppa un'applicazione, verifica in genere eseguendolo in Visual Studio. Per impostazione predefinita, i progetti di applicazione web in Visual Studio 2012 utilizzano IIS Express come server web di sviluppo. IIS Express è più simile a IIS completo rispetto a di Visual Studio Development Server (anche noto come Cassini), utilizzato da Visual Studio 2010 per impostazione predefinita. Ma nessuno dei due server web di sviluppo funziona esattamente come IIS. Di conseguenza, è possibile che le applicazioni verranno eseguite correttamente quando si eseguirne il test in Visual Studio, ma può avere esito negativo quando viene distribuito a IIS.

È possibile testare l'applicazione in modo più affidabile nei modi seguenti:

1. Distribuire l'applicazione di IIS sul computer di sviluppo utilizzando lo stesso processo che verrà usato in seguito possibile distribuirlo in ambiente di produzione. È possibile configurare Visual Studio per utilizzare IIS quando si esegue un progetto web, ma questa operazione non testa il processo di distribuzione. Questo metodo convalida il processo di distribuzione, oltre a convalidare che l'applicazione verrà eseguita correttamente in IIS.
2. Distribuire l'applicazione in un ambiente di test che è quasi identico a quella nell'ambiente di produzione. Poiché l'ambiente di produzione per queste esercitazioni sono disponibili le app Web in Azure App Service, l'ambiente di test ideale è un'applicazione web aggiuntivi creata in Azure App Service. Utilizzare questa seconda app web solo per i test, ma potrebbe essere configurato esattamente come app web di produzione.

Opzione 2 è il modo più affidabile per eseguire il test, e se a tale scopo, non è necessariamente necessario all'opzione 1. Tuttavia, se si distribuisce in un'opzione di provider hosting terze parti 2 potrebbe non essere fattibile o potrebbe essere costosa, pertanto questa serie di esercitazioni Mostra entrambi i metodi. Cui vengono fornite istruzioni per l'opzione 2 di [distribuzione nell'ambiente di produzione](deploying-to-production.md) esercitazione.

Per ulteriori informazioni sull'utilizzo di server web in Visual Studio, vedere [server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](troubleshooting.md).

## <a name="install-iis"></a>Installare IIS

Per distribuire in IIS nel computer di sviluppo, è necessario che IIS e distribuzione Web installato. Distribuzione Web è installata per impostazione predefinita con Visual Studio, ma IIS non è incluso nella configurazione predefinita di Windows 8 o Windows 7. Se IIS è già stato installato e il pool di applicazioni predefinito è già impostato su .NET 4, andare al [nella sezione successiva](#sqlexpress).

1. Utilizzo di [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx) è il modo migliore per installare IIS e distribuzione Web, perché l'installazione guidata piattaforma Web installa una configurazione consigliata per IIS e installa automaticamente i prerequisiti per IIS e Web Se è necessario distribuire.

    Per eseguire l'installazione guidata piattaforma Web per installare IIS e distribuzione Web, utilizzare il collegamento seguente. Se è già stato installato IIS, distribuzione Web o dei relativi componenti necessari, l'installazione guidata piattaforma Web installa solo le funzionalità mancanti.

    - [Installare IIS e distribuzione Web tramite WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

    Verranno visualizzati messaggi che indica che verrà installato IIS 7. Il funzionamento del collegamento per IIS 8 in Windows 8, ma per Windows 8 verificare che ASP.NET 4.5 sia installato attenendosi alla procedura seguente:

    1. Aprire **Pannello di controllo**, **programmi e funzionalità**, **o disattivazione delle funzionalità Windows attivare**.
    2. Espandere **Internet Information Services**, **servizi World Wide Web**, e **funzionalità per lo sviluppo di applicazioni**.
    3. Assicurarsi che **ASP.NET 4.5** è selezionata.

        ![Selezionare ASP.NET 4.5](deploying-to-iis/_static/image1.png)

Dopo aver installato IIS, eseguire **Gestione IIS** per assicurarsi che .NET Framework versione 4 sia assegnato al pool di applicazioni predefinito.

1. Premere WINDOWS + R per aprire la **eseguire** la finestra di dialogo.

    (O in Windows 8 immettere "Esegui" il **avviare** pagina o in Windows 7 selezionare **eseguire** dal **avviare** menu. Se **eseguire** non è il **avviare** menu, fare clic sulla barra delle applicazioni, fare clic su **proprietà**, selezionare il **Menu Start** scheda, fare clic su **Personalizza**e selezionare **comando**.)
2. Immettere "inetmgr" e quindi fare clic su **OK**.
3. Nel **connessioni** riquadro espandere il nodo del server e selezionare **pool di applicazioni**. Nel **pool di applicazioni** riquadro se **DefaultAppPool** è assegnato a .NET framework versione 4, come illustrato nella figura seguente, passare alla sezione successiva.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Se viene visualizzata solo due pool di applicazioni e di entrambi gli elementi impostati per .NET Framework 2.0, è necessario installare ASP.NET 4 in IIS.

    Per Windows 8, vedere le istruzioni nella precedente sezione per assicurarsi che ASP.NET 4.5 è installato, o [questo articolo della Knowledge Base](https://support.microsoft.com/kb/2736284). Per Windows 7, aprire una finestra del prompt dei comandi facendo clic **prompt dei comandi** nelle finestre **avviare** menu e selezionando **Esegui come amministratore**. Eseguire quindi [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) per installare ASP.NET 4 in IIS, utilizzando i comandi seguenti. (In sistemi a 32 bit, sostituire "Framework64" con "Framework").

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    Questo comando Crea nuovo pool di applicazioni per .NET Framework 4, ma il pool di applicazioni predefinito verrà comunque impostato a 2.0. Verrà distribuita un'applicazione che ha come destinazione .NET 4 al pool di applicazioni, è necessario modificare il pool di applicazioni in .NET 4.
5. Se è stata chiusa **Gestione IIS**, eseguirlo nuovamente, espandere il nodo del server e fare clic su **pool di applicazioni** per visualizzare il **pool di applicazioni** nuovo riquadro.
6. Nel **pool di applicazioni** riquadro, fare clic su **DefaultAppPool**e quindi la **azioni** fare clic su riquadro **le impostazioni di base**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. Nel **modifica Pool di applicazioni** nella finestra di dialogo Modifica **versione di .NET Framework** a **.NET Framework v 4.0.30319** e fare clic su **OK**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS è ora pronto per poter pubblicare un'applicazione web, ma prima di poter eseguire che è necessario creare i database che verrà utilizzato nell'ambiente di test.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Installare SQL Server Express

LocalDB non è progettato per funzionare in IIS, pertanto per l'ambiente di test è necessario avere installato SQL Server Express. Se si utilizza Visual Studio 2010 SQL Server Express è già installato per impostazione predefinita. Se si utilizza Visual Studio 2012, è necessario installarlo.

Per installare SQL Server Express, installarlo da [area Download: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) facendo [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) o [ ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Se si scegliere quello corretto per il sistema non riuscirà installare e sarà possibile provare l'altro.

Nella prima pagina di Centro installazione SQL Server, fare clic su **nuova installazione SQL Server autonomo o aggiungere funzionalità a un'installazione esistente**e seguire le istruzioni, accettando i valori predefiniti. Nell'installazione guidata di accettare le impostazioni predefinite. Per ulteriori informazioni sulle opzioni di installazione, vedere [installare SQL Server 2012 dall'installazione guidata (programma di installazione)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Creare i database di SQL Server Express per l'ambiente di test

L'applicazione Contoso University è disponibili due database: il database delle appartenenze e il database dell'applicazione. Per due database separati o a un singolo database, è possibile distribuire questi database. È possibile combinarle per facilitare il join del database tra il database dell'applicazione e il database di appartenenza. Se si distribuisce in un provider di hosting di terze parti, il piano di hosting potrebbe inoltre in grado di fornire un motivo per combinarle. Ad esempio, il provider di hosting potrebbe addebitare i costi ulteriori per più database oppure potrebbe non consentire anche di più di un database.

In questa esercitazione, vedrà come distribuire per due database nell'ambiente di test e per un database in ambienti di gestione temporanea e produzione.

Dal **vista** menu selezionare Visual Studio **Esplora Server** (**Esplora Database** in Visual Web Developer) e quindi fare doppio clic su **connessioni dati**  e selezionare **Crea nuovo Database di SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

Nel **Crea nuovo Database di SQL Server** finestra di dialogo immettere ". \SQLExpress" nel **nome Server** casella e "aspnet-ContosoUniversity" nel **nome nuovo database** casella, quindi Fare clic su **OK**.

![Creare aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

Seguire la stessa procedura per creare un nuovo database di SQL Server Express School denominato "ContosoUniversity".

**Esplora server** verranno visualizzati due nuovi database.

![Nuovi database in Esplora Server](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Creare uno script di concessione per i nuovi database

Quando l'applicazione viene eseguita in IIS nel computer di sviluppo, l'applicazione accede al database utilizzando le credenziali del pool di applicazioni predefinito. Tuttavia, per impostazione predefinita, l'identità del pool di applicazioni non dispone dell'autorizzazione per aprire i database. Pertanto, è necessario eseguire uno script per concedere tale autorizzazione. In questa sezione creare lo script che verrà eseguito successivamente per assicurarsi che l'applicazione è possibile aprire i database quando viene eseguito in IIS.

Visualizzare la soluzione (non uno dei progetti), quindi scegliere **Aggiungi nuovo elemento**e quindi creare un nuovo **File SQL** denominato *Grant.sql*. Copiare i seguenti comandi SQL nel file, quindi salvare e chiudere il file:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Questo script è progettato per funzionare con SQL Server Express 2012 e con le impostazioni di IIS in Windows 8 o Windows 7, secondo quanto specificato in questa esercitazione. Se si utilizza una versione diversa di SQL Server o di Windows o configuri IIS nel computer in modo diverso, le modifiche apportate a questo script potrebbero essere necessarie. Per ulteriori informazioni sugli script di SQL Server, vedere [la documentazione Online di SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Nota sulla sicurezza** questo script fornisce db\_le autorizzazioni del proprietario per l'utente che accede al database in fase di esecuzione, ovvero è necessario nell'ambiente di produzione. In alcuni scenari è possibile specificare un utente che contiene lo schema completo del database, aggiornare le autorizzazioni solo per la distribuzione e specificare in fase di esecuzione di un altro utente che disponga delle autorizzazioni solo per leggere e scrivere dati. Per ulteriori informazioni, vedere [esaminare le modifiche automatiche di Web. config per le migrazioni Code First](#reviewingmigrations) più avanti in questa esercitazione.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Eseguire lo script di concessione nel database dell'applicazione

È possibile configurare il profilo di pubblicazione per eseguire lo script di concessione nel database delle appartenenze durante la distribuzione perché la distribuzione del database utilizza il provider dbDacFx. È possibile eseguire gli script durante la distribuzione di migrazioni Code First, ovvero come si distribuisce il database dell'applicazione. Pertanto, è necessario eseguire manualmente lo script prima della distribuzione nel database dell'applicazione.

1. In Visual Studio, aprire il *Grant.sql* file creato in precedenza.
2. Fare clic su **Connetti**. 

    ![Pulsante Connetti](deploying-to-iis/_static/image11.png)
3. Nel **Connetti al Server** finestra di dialogo immettere *. \SQLExpress* come il **nome Server**e quindi fare clic su **Connetti**.
4. Nell'elenco a discesa database selezionare **ContosoUniversity**, quindi fare clic su **Execute**. 

    ![](deploying-to-iis/_static/image12.png)

A questo punto l'identità del pool di applicazioni predefinito disponga di autorizzazioni sufficienti nel database dell'applicazione per le migrazioni Code First creare le tabelle di database quando viene eseguita l'applicazione.

## <a name="publish-to-iis"></a>Esegue la pubblicazione in IIS

Esistono diversi modi per distribuire a IIS tramite Visual Studio e distribuzione Web:

- Utilizzare Visual Studio pubblicazione con un clic.
- Pubblicare dalla riga di comando.
- Creare un *pacchetto di distribuzione* e installarlo utilizzando la UI Gestione IIS. Il pacchetto di distribuzione è costituito un *zip* file che contiene tutti i file e i metadati necessari per installare un sito in IIS.
- Creare un pacchetto di distribuzione e installarlo mediante la riga di comando.

Il processo che è parlato nelle esercitazioni precedenti per configurare Visual Studio per automatizzare le attività di distribuzione si applica a tutti questi metodi. In queste esercitazioni si utilizzeranno i primi due di questi metodi. Per informazioni sull'utilizzo di pacchetti di distribuzione, vedere [distribuendo un'applicazione web creando e installando un pacchetto di distribuzione web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) nella mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET.

Prima della pubblicazione, assicurarsi che si esegue Visual Studio in modalità amministratore. Se non viene visualizzato **(amministratore)** nella barra del titolo, chiudere Visual Studio. In Windows 8 **avviare** pagina o Windows 7 **avviare** menu, fare clic sull'icona per la versione di Visual Studio in uso e selezionare **Esegui come amministratore**. Modalità di amministratore è necessaria per la pubblicazione solo quando esegue la pubblicazione in IIS nel computer locale.

### <a name="create-the-publish-profile"></a>Creare il profilo di pubblicazione

1. In **Esplora**, fare clic sul progetto ContosoUniversity (non il progetto ContosoUniversity.DAL) e selezionare **pubblica**.

    Il **pubblica sul Web** procedura guidata viene visualizzata.

    ![Scheda del Web Creazione guidata profilo di pubblicazione](deploying-to-iis/_static/image13.png)
2. Nell'elenco a discesa, selezionare  **&lt;New... &gt;**. (Con l'aggiornamento di Visual Studio più recente è installato, è presente alcun elenco di riepilogo a discesa e il pulsante di fare clic su per creare un nuovo profilo da zero è **personalizzato**.)
3. Nel **nuovo profilo** nella finestra di dialogo immettere "Test" e quindi fare clic su **OK**.

    La procedura guidata passa automaticamente al **connessione** scheda.
4. Nel **URL del servizio** immettere *localhost*.
5. Nel **sito/applicazione** immettere *ContosoUniversity/sito Web predefinito*
6. Nel **URL di destinazione** immettere`http://localhost/ContosoUniversity`

    Il **URL di destinazione** impostazione non è necessario. Al termine Visual Studio distribuisce l'applicazione, viene automaticamente aperto il browser predefinito per questo URL. Se non si desidera il browser per aprire automaticamente dopo la distribuzione, lasciare vuota questa casella.
7. Fare clic su **convalida connessione** per verificare che le impostazioni siano corrette e che è possibile connettersi a IIS nel computer locale.

    Un segno di spunta verde verifica che la connessione ha esito positivo.

    ![Scheda di connessione Web guidata di pubblicazione](deploying-to-iis/_static/image14.png)
8. Fare clic su **Avanti** per spostare il **impostazioni** scheda.
9. Il **configurazione** casella di riepilogo a discesa specifica configurazione della build da distribuire. Lasciare impostata sul valore predefinito della versione. In questa esercitazione si non distribuendo le compilazioni di Debug.
10. Espandere **Opzioni pubblicazione File**, quindi selezionare **escludere i file dall'App\_cartella dati**.

    Nell'ambiente di test l'applicazione accederà i database creati nell'istanza locale di SQL Server Express, non il file con estensione mdf file il *App\_dati* cartella.
11. Lasciare il **Precompile durante la pubblicazione** e **Rimuovi file aggiuntivi nella destinazione** caselle di controllo deselezionate.

    ![Opzioni di pubblicazione di file nella scheda Impostazioni](deploying-to-iis/_static/image15.png)

    La precompilazione è un'opzione che risulta particolarmente utile per siti di dimensioni molto grandi. può ridurre il tempo di avvio pagina per la prima volta dopo che il sito viene pubblicato, è richiesta una pagina.

    Non è necessario rimuovere i file aggiuntivi, poiché si tratta della prima distribuzione e non vi sarà tutti i file nella cartella di destinazione ancora.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Se si seleziona **Rimuovi file aggiuntivi** per una distribuzione successivi allo stesso sito, assicurarsi utilizzare la funzionalità di anteprima che consente di visualizzare in anticipo quali file verranno eliminati prima di distribuire. Il comportamento previsto è che distribuzione Web verranno eliminati i file nel server di destinazione che è stato eliminato nel progetto. Tuttavia, viene confrontata l'intera struttura delle cartelle in cartelle di origine e destinazione e in alcuni scenari di distribuzione Web potrebbe eliminare i file che non si desidera eliminare.
    > 
    > Ad esempio, se si dispone di un'applicazione web in una sottocartella nel server quando si distribuisce un progetto nella cartella radice, verrà eliminata nella sottocartella. Potrebbe essere un unico progetto per il sito principale all'indirizzo contoso.com e un altro progetto per un blog all'indirizzo contoso.com /blog. L'applicazione di blog è in una sottocartella. Se si seleziona Rimuovi file aggiuntivi nella destinazione quando si distribuisce il sito principale, l'applicazione di blog verrà eliminato.
    > 
    > Per un altro esempio, l'App\_cartella dati potrà essere eliminata in modo imprevisto. Alcuni database, ad esempio SQL Server Compact archiviano file di database nell'App\_cartella dati. Dopo la distribuzione iniziale non si desidera mantenere la copia i file di database nelle distribuzioni successive in modo da selezionare App escludere\_dati nella scheda Pubblicazione/creazione pacchetto Web. Dopo aver eseguito che, se si dispone di Rimuovi file aggiuntivi nella destinazione selezionata, i file di database e l'App\_cartella dati verrà eliminato la volta successiva che si esegue la pubblicazione.

### <a name="configure-deployment-for-the-membership-database"></a>Configurare la distribuzione per il database delle appartenenze

I passaggi seguenti si applicano le **DefaultConnection** database il **database** sezione della finestra di dialogo.

1. Nel **stringa di connessione remota** , immettere la seguente stringa di connessione che punti al nuovo database di appartenenza di SQL Server Express.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    Il processo di distribuzione verrà inserito la stringa di connessione nel file Web. config distribuito perché **utilizza stringa di connessione in fase di esecuzione** è selezionata.

    È inoltre possibile ottenere la stringa di connessione **Esplora Server**. In **Esplora Server**, espandere **connessioni dati** e selezionare il  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** database, quindi dal **proprietà** copia finestra il **stringa di connessione** valore. Che la stringa di connessione avrà un'impostazione aggiuntiva che è possibile eliminare: `Pooling=False`.
2. Selezionare **Aggiorna database**.

    In questo modo lo schema del database da creare nel database di destinazione durante la distribuzione. Nei passaggi seguenti specificare gli script aggiuntivi che è necessario eseguire: uno per concedere l'accesso al database al pool di applicazioni predefinito e uno per la distribuzione di dati.
3. Fare clic su **configurare gli aggiornamenti di database**.
4. Nel **Configura Aggiornamenti Database** la finestra di dialogo, fare clic su **aggiungere Script SQL** e quindi passare al *Grant.sql* script salvato in precedenza nella cartella della soluzione.
5. Ripetere il processo per aggiungere il *aspnet-data-dev.sql* script.

    ![Configurare gli aggiornamenti di Database per database delle appartenenze](deploying-to-iis/_static/image16.png)
6. Fare clic su **Chiudi**.

### <a name="configure-deployment-for-the-application-database"></a>Configurare la distribuzione per il database dell'applicazione

Quando Visual Studio rileva un Entity Framework `DbContext` (classe), viene creata una voce nel **database** sezione che presenta un **eseguire le migrazioni Code First** casella di controllo anziché un  **Aggiornare Database** casella di controllo. Per questa esercitazione si userà la casella di controllo per specificare la distribuzione di migrazioni Code First.

In alcuni scenari, è possibile utilizzare un `DbContext` database ma si desidera utilizzare il provider dbDacFx anziché le migrazioni per distribuire il database. In tal caso, vedere [come distribuire un database senza migrazioni Code First?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) in domande frequenti su distribuzione Web ASP.NET su MSDN.

I passaggi seguenti si applicano le **SchoolContext** database il **database** sezione della finestra di dialogo.

1. Nel **stringa di connessione remota** , immettere la seguente stringa di connessione che punti al nuovo database dell'applicazione SQL Server Express.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    Il processo di distribuzione verrà inserito la stringa di connessione nel file Web. config distribuito perché **utilizza stringa di connessione in fase di esecuzione** è selezionata.

    È inoltre possibile ottenere la stringa di connessione del database dell'applicazione da **Esplora Server** stringa di connessione del database allo stesso modo è stato ottenuto l'appartenenza.
2. Selezionare **eseguire migrazioni Code First (esecuzione all'avvio dell'applicazione)**.

    Questa opzione fa sì che il processo di distribuzione configurare il file Web. config distribuito per specificare il `MigrateDatabaseToLatestVersion` inizializzatore. L'inizializzatore Aggiorna automaticamente il database alla versione più recente quando l'applicazione accede al database per la prima volta dopo la distribuzione.

### <a name="configure-publish-profile-transforms"></a>Configurare le trasformazioni di profilo di pubblicazione

1. Fare clic su **Chiudi**, quindi fare clic su **Sì** quando viene chiesto se si desidera salvare le modifiche.
2. In **Esplora**, espandere **proprietà**, espandere **PublishProfiles**.
3. Fare clic su di Rright *Test.pubxml,* e quindi fare clic su **aggiungere Configurazione trasformazione**.

    ![Aggiungere menu Configurazione trasformazione](deploying-to-iis/_static/image17.png)

    Visual Studio crea il *Web.Test.config* file di trasformazione e lo apre.
4. Nel *Web.Test.config* trasformare il file, inserire il codice seguente immediatamente dopo l'apertura tag di configurazione.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Quando si utilizza il Test di profilo di pubblicazione, la trasformazione imposta l'indicatore di ambiente "Test". Nel sito distribuito verrà visualizzato "(Test)" dopo l'intestazione H1 "Contoso University".
5. Salvare e chiudere il file.
6. Fare doppio clic su di *Web.Test.config* file e fare clic su **anteprima trasformare** per assicurarsi che la trasformazione è codificato produce modifiche previste.

    Il **anteprima Web. config** finestra viene visualizzato il risultato dell'applicazione sia di *Web.Release.config* Trasforma e *Web.Test.config* Trasforma.

### <a name="preview-the-deployment-updates"></a>Visualizzare in anteprima gli aggiornamenti di distribuzione

1. Aprire il **pubblica sul Web** nuovamente (fare clic sul progetto ContosoUniversity e fare clic su **pubblica**).
2. Nel **anteprima** scheda, assicurarsi che il **Test** profilo è ancora selezionato e quindi fare clic su **avviare Anteprima** per visualizzare un elenco dei file che verranno copiati.

    ![Pulsante Anteprima](deploying-to-iis/_static/image18.png)

    ![Anteprima di pubblicazione](deploying-to-iis/_static/image19.png)

    È anche possibile scegliere di **database di anteprima** collegamento per visualizzare gli script che verranno eseguita nel database delle appartenenze. (Non gli script vengono eseguiti per la distribuzione di migrazioni Code First, pertanto non è necessario visualizzare in anteprima per il database dell'applicazione.)
3. Fare clic su **Pubblica**.

    Se Visual Studio non è in modalità amministratore, è possibile che venga visualizzato un messaggio di errore che indica un errore di autorizzazione. In tal caso, chiudere Visual Studio, aprirlo in modalità amministratore e provare di nuovo la pubblicazione.

    Se Visual Studio è in modalità amministratore, il **Output** finestra report ha esito positivo, compilare e pubblicare.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    Se è stato immesso l'URL di **URL di destinazione** casella il profilo di pubblicazione **connessione** scheda nel browser verrà aperta automaticamente per Contoso University Home page in esecuzione in IIS nel computer locale.

## <a name="test-in-the-test-environment"></a>Test nell'ambiente di test

Si noti che l'indicatore di ambiente illustra "(Test)" anziché "(Dev)", che indica che il *Web. config* trasformazione per l'indicatore di ambiente ha avuto esito positivo.

Eseguire il **i docenti** pagina per verificare che il primo codice seeding del database con dati istruttore. Quando si seleziona questa pagina, potrebbe richiedere alcuni minuti per il caricamento perché il primo codice viene creato il database e quindi esegue il `Seed` metodo. (Non eseguire questa operazione quando erano nella home page, poiché l'applicazione non tenta di accedere ancora al database.)

Fare clic su di **studenti** scheda per verificare che il database distribuito non ha studenti.

Selezionare **aggiungere studenti** dal **studenti** menu, aggiungere uno studente e quindi visualizzare il nuovo studente nel **studenti** pagina per verificare che è possibile scrivere nel database .

Dal **corsi** dal menu **aggiornamento crediti**. Il **aggiornamento crediti** pagina richiede autorizzazioni di amministratore, pertanto la **Accedi** viene visualizzata la pagina. Immettere le credenziali dell'account amministratore creato precedenti ("admin" e "devpwd"). Il **aggiornamento crediti** viene visualizzata la pagina, che verifica che l'account amministratore creato nell'esercitazione precedente è stata distribuita correttamente nell'ambiente di testing.

Verificare che un *Elmah* esiste la cartella di *c:\inetpub\wwwroot\ContosoUniversity* cartella con solo il file segnaposto in essa contenuti.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Esaminare le modifiche automatiche di Web. config per le migrazioni Code First

Aprire il *Web. config* file nell'applicazione distribuita in *C:\inetpub\wwwroot\ContosoUniversity* ed è possibile visualizzare in cui il processo di distribuzione configurato migrazioni Code First per automaticamente aggiornare il database alla versione più recente.

![](deploying-to-iis/_static/image21.png)

Il processo di distribuzione creata anche una nuova stringa di connessione per le migrazioni Code First da utilizzare esclusivamente per l'aggiornamento dello schema di database:

![Stringa di connessione Database_Publish](deploying-to-iis/_static/image22.png)

La stringa di connessione aggiuntive consente di specificare un account utente per gli aggiornamenti dello schema di database e un account utente diverso per accedere ai dati dell'applicazione. Ad esempio, è possibile assegnare il **db\_proprietario** ruolo migrazioni Code First e **db\_datareader** e **db\_datawriter**ruoli all'applicazione. Si tratta di un modello comune di difesa in profondità che impedisce di codice potenzialmente dannoso nell'applicazione di modificare lo schema del database. (Ad esempio, questo potrebbe verificarsi in un attacco injection SQL.) Questo modello non è usato da queste esercitazioni. Se si desidera implementare questo pattern nello scenario, è possibile farlo eseguendo i passaggi seguenti:

1. Nel **impostazioni** scheda della finestra di **pubblica sul Web** procedura guidata, specificare la stringa di connessione che specifica un utente con autorizzazioni per l'aggiornamento dello schema completo del database e deselezionare il **utilizza stringa di connessione in fase di esecuzione** casella di controllo. Nel file Web. config distribuito, questa diventa il `DatabasePublish` stringa di connessione.
2. Creare una trasformazione del file Web. config per la stringa di connessione che si desidera che l'applicazione da utilizzare in fase di esecuzione.

## <a name="summary"></a>Riepilogo

È ora distribuito l'applicazione in IIS nel computer di sviluppo e testato non esiste.

![Home page di Test](deploying-to-iis/_static/image23.png)

Verifica che il processo di distribuzione copiato il contenuto dell'applicazione nel percorso corretto (esclusi i file non desiderati per la distribuzione) e anche che distribuzione Web configurato IIS correttamente durante la distribuzione. Nell'esercitazione successiva, verrà eseguito un test più che consente di trovare un'attività di distribuzione che non è ancora stata eseguita: impostazione delle autorizzazioni di cartella per il *Elmah* cartella.

## <a name="more-information"></a>Altre informazioni

Per informazioni sull'esecuzione di IIS o IIS Express in Visual Studio, vedere le risorse seguenti:

- [Panoramica di IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) nel sito IIS.net.
- [Introduzione a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) sul blog di Scott Guthrie.
- [Web server in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Principali differenze tra IIS e il Server di sviluppo ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) sul sito ASP.NET.

Per informazioni su quali problemi possono verificarsi durante l'applicazione viene eseguita in attendibilità media, vedere [Hosting di applicazioni ASP.NET in attendibilità media](http://www.4guysfromrolla.com/articles/100307-1.aspx) sugli Guy dal sito Rolla 4.

>[!div class="step-by-step"]
[Precedente](project-properties.md)
[Successivo](setting-folder-permissions.md)
