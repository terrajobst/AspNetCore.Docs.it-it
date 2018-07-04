---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione in IIS come ambiente di Test - 5 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 70bc1b8e3c5d01470c2cebfbabd0c119e8f6d360
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394716"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione in IIS come ambiente di Test - 5 di 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e Mostra come distribuire in App Web di servizio App di Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come distribuire un'applicazione web ASP.NET in IIS nel computer locale.

Quando si sviluppa un'applicazione, in genere testare eseguendolo in Visual Studio. Per impostazione predefinita, questo significa che si usa Visual Studio Development Server (noto anche come Cassini). Visual Studio Development Server rende più semplice testare durante lo sviluppo in Visual Studio, ma non funziona esattamente come IIS. Di conseguenza, è possibile che un'applicazione verrà eseguita correttamente quando si eseguirne il test in Visual Studio, ma non riesce quando viene distribuito a IIS in un ambiente di hosting.

È possibile testare l'applicazione in modo più affidabile nei modi seguenti:

1. Usa IIS Express o IIS completo invece di Visual Studio Development Server quando esegue il test in Visual Studio durante lo sviluppo. Questo metodo emula a livello generale con maggiore precisione la modalità di esecuzione del sito in IIS. Tuttavia, questo metodo non testa il processo di distribuzione o convalidare che il risultato del processo di distribuzione verrà eseguito correttamente.
2. Distribuire l'applicazione in IIS nel computer di sviluppo utilizzando la stessa procedura che si userà in un secondo momento per la distribuzione in ambiente di produzione. Questo metodo convalida il processo di distribuzione, oltre a verificare che l'applicazione verrà eseguita correttamente in IIS.
3. Distribuire l'applicazione in un ambiente di test che è più vicino possibile all'ambiente di produzione. Poiché l'ambiente di produzione per queste esercitazioni è un provider di hosting di terze parti, l'ambiente di test ideale sarebbe un secondo account con il provider di hosting. Utilizzare questo secondo account solo per i test, ma sia necessario configurare esattamente come l'account di produzione.

Questa esercitazione illustra i passaggi per l'opzione 2. Vengono fornite istruzioni per l'opzione 3 alla fine del [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione e alla fine di questa esercitazione sono disponibili collegamenti alle risorse per l'opzione 1.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Configurazione dell'applicazione per l'esecuzione in attendibilità media

Prima di installare IIS e distribuzione di ad esso, sarà necessario modificare un'impostazione del file Web. config per eseguire il sito più simile si verifica in un ambiente di hosting condiviso tipico.

I provider di hosting esegue in genere il sito web in *attendibilità media*, che significa che esistono alcuni aspetti non è consentito eseguire. Ad esempio, il codice dell'applicazione non è possibile accedere al Registro di Windows e Impossibile leggere o scrivere i file che non rientrano nella gerarchia di cartelle dell'applicazione. Per impostazione predefinita l'applicazione viene eseguita *attendibilità alta* nel computer locale, a indicare che l'applicazione potrebbe essere in grado di eseguire operazioni che non riesce quando viene distribuito nell'ambiente di produzione. Pertanto, per rendere l'ambiente di test che più riflettere l'ambiente di produzione, si configurerà l'esecuzione in attendibilità media dell'applicazione.

Nel file Web. config dell'applicazione, aggiungere un **trust** elemento le **System. Web** elemento, come illustrato in questo esempio.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

L'applicazione verrà ora eseguita in attendibilità media in IIS anche nel computer locale. Questa impostazione consente di intercettare il prima possibile eventuali tentativi dal codice dell'applicazione per eseguire un'operazione che avrà esito negativo nell'ambiente di produzione.

> [!NOTE]
> Se si usa migrazioni di Entity Framework Code First, assicurarsi di avere la versione 5.0 o versione successiva. In Entity Framework versione 4.3, migrazioni richiede attendibilità totale per aggiornare lo schema del database.


## <a name="installing-iis-and-web-deploy"></a>Distribuzione di installazione Web e IIS

Per distribuire in IIS nel computer di sviluppo, è necessario che IIS e distribuzione Web installati. Questi non sono inclusi nella configurazione predefinita di Windows 7. Se già stato installato IIS e distribuzione Web, passare alla sezione successiva.

Usando il [instalace Webové Platformy](https://www.microsoft.com/web/downloads/platform.aspx) è il modo migliore per installare IIS e distribuzione Web, perché l'installazione guidata piattaforma Web installa una configurazione consigliata per IIS e installa automaticamente i prerequisiti per IIS e Web La distribuzione se necessario.

Per eseguire l'installazione guidata piattaforma Web per installare IIS e distribuzione Web, usare il collegamento seguente. Se è già stato installato IIS, distribuzione Web o dei relativi componenti necessari, l'installazione guidata piattaforma Web installa solo cosa manca.

- [Installare IIS e distribuzione Web tramite WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Impostare il Pool di applicazioni predefinito per .NET 4

Dopo aver installato IIS, eseguire **Gestione IIS** per assicurarsi che .NET Framework versione 4 sia assegnato al pool di applicazioni predefinito.

Dalla finestra di Windows **avviare** dal menu **eseguito**, immettere "inetmgr" e quindi fare clic su **OK**. (Se il **eseguiti** comando non è nel **avviare** menu, è possibile premere il tasto Windows e R per aprirlo. O sulla barra delle applicazioni, fare clic su **delle proprietà**, selezionare il **dal Menu di avvio** scheda, fare clic su **Personalizza**e selezionare **eseguire comando**.)

Nel **connessioni** riquadro, espandere il nodo del server e selezionare **pool di applicazioni**. Nel **pool di applicazioni** riquadro, se **DefaultAppPool** viene assegnato a .NET framework versione 4 come illustrato nella figura seguente, passare alla sezione successiva.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Se viene visualizzato solo due pool di applicazioni e di entrambi gli elementi configurati per .NET Framework 2.0, è necessario installare ASP.NET 4 in IIS:

- Aprire una finestra del prompt dei comandi facendo clic **prompt dei comandi** nella finestra di Windows **avviare** dal menu e selezionando **Esegui come amministratore**. Quindi eseguire [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) installare ASP.NET 4 in IIS, usando i comandi seguenti. (Nei sistemi a 64 bit, sostituire "Framework" con "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Questo comando Crea nuovo pool di applicazioni per .NET Framework 4, ma il pool di applicazioni predefinito verrà ancora impostato su 2.0. Verrà distribuita un'applicazione destinata a .NET 4 per tale pool di applicazioni, pertanto è necessario modificare il pool di applicazioni in .NET 4.

Se è stato chiuso **Gestione IIS**eseguirlo di nuovo, espandere il nodo del server e fare clic su **pool di applicazioni** per visualizzare il **pool di applicazioni** nuovo riquadro.

Nel **pool di applicazioni** riquadro, fare clic su **DefaultAppPool**, quindi nel **azioni** riquadro fare clic su **le impostazioni di base**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

Nel **modifica Pool di applicazioni** della finestra di dialogo Modifica **versione di .NET Framework** al **.NET Framework v4.0.30319** e fare clic su **OK**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

A questo punto si è pronti per la pubblicazione in IIS.

## <a name="publishing-to-iis"></a>Pubblicazione in IIS

Esistono diversi modi, è possibile distribuire con Visual Studio 2010 e distribuzione Web:

- Usare Visual Studio, pubblicare un solo clic.
- Creare un *pacchetto di distribuzione* e installarlo utilizzando la UI Gestione IIS. Il pacchetto di distribuzione è costituito un *zip* file che contiene tutti i file e i metadati necessari per installare un sito in IIS.
- Creare un pacchetto di distribuzione e installarlo dalla riga di comando.

Il processo che è stato eseguito nelle esercitazioni precedenti per configurare Visual Studio per automatizzare le attività di distribuzione si applica a tutti di questi tre metodi. In queste esercitazioni si userà il primo di questi metodi. Per informazioni sull'uso di pacchetti di distribuzione, vedere [mappa del contenuto ASP.NET distribuzione](https://msdn.microsoft.com/library/bb386521.aspx).

Prima della pubblicazione, assicurarsi che si esegue Visual Studio in modalità amministratore. (In Windows 7 **avviare** menu, fare doppio clic sull'icona per la versione di Visual Studio in uso e selezionare **Esegui come amministratore**.) Modalità amministratore è necessaria per la pubblicazione solo quando esegue la pubblicazione in IIS nel computer locale.

Nelle **Esplora soluzioni**, fare clic sul progetto ContosoUniversity (non sul progetto ContosoUniversity.DAL) e selezionare **Publish**.

Il **pubblica sul Web** procedura guidata viene visualizzata.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Nell'elenco a discesa, selezionare  **&lt;New... &gt;**.

Nel **nuovo profilo** della finestra di dialogo immettere "Test" e quindi fare clic su **OK**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Questo nome è che identico al nodo centrale del Web.Test.config trasformare il file creato in precedenza. Questa corrispondenza è quello che determina le trasformazioni Web.Test.config da applicare durante la pubblicazione utilizzando questo profilo.

La procedura guidata passa automaticamente per il **connessione** scheda.

Nel **URL del servizio** casella, immettere *localhost*.

Nel **sito/applicazione** casella, immettere *Default Web Site/ContosoUniversity*.

Nel **URL di destinazione** immettere `http://localhost/ContosoUniversity`.

Il **URL di destinazione** impostazione non è obbligatorio. Quando Visual Studio ha completato la distribuzione dell'applicazione, venga automaticamente aperto il browser predefinito usando l'URL. Se non si desidera il browser per aprire automaticamente dopo la distribuzione, lasciare vuota questa casella.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Fare clic su **convalida connessione** per verificare che le impostazioni siano corrette e che è possibile connettersi a IIS nel computer locale.

Un segno di spunta verde verifica che la connessione ha esito positivo.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Fare clic su **successivo** per passare alle **impostazioni** scheda.

Il **configurazione** casella di riepilogo a discesa consente di specificare la configurazione della build per la distribuzione. Il valore predefinito è una versione, che si desidera eseguire.

Lasciare il **Rimuovi file aggiuntivi nella destinazione** casella di controllo deselezionata. Poiché si tratta della prima distribuzione, non saranno tutti i file nella cartella di destinazione ancora.

Nel **database** , quindi immettere il seguente valore nella casella stringa di connessione per **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Il processo di distribuzione inserirà questa stringa di connessione nel file Web. config distribuito in quanto **utilizzare questa stringa di connessione in fase di esecuzione** sia selezionata.

Anche in **SchoolContext**, selezionare **applicare le migrazioni Code First**. Questa opzione fa sì che il processo di distribuzione configurare il file Web. config distribuito di specificare il `MigrateDatabaseToLatestVersion` inizializzatore. Quando l'applicazione accede al database per la prima volta dopo la distribuzione, questo inizializzatore Aggiorna automaticamente il database alla versione più recente.

Nella casella stringa di connessione per **DefaultConnection**, immettere il valore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Lasciare **aggiornare il database** deselezionata. Il database di appartenenza verrà distribuito copiando il file con estensione sdf nell'App\_dati e non si desidera il processo di distribuzione eseguire altre operazioni con il database.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Fare clic su **successivo** per passare alle **anteprima** scheda.

Nel **Preview** scheda, fare clic su **Avvia anteprima** per visualizzare un elenco dei file che verranno copiati.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Fare clic su **Pubblica**.

Se Visual Studio non è in modalità amministratore, si potrebbe ottenere un messaggio di errore che indica un errore di autorizzazione. In tal caso, chiudere Visual Studio, aprirlo in modalità amministratore e provare a eseguire nuovamente la pubblicazione.

Se Visual Studio è in modalità amministratore, il **Output** finestra report ha esito positivo, compilare e pubblicare.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Il browser si aprirà automaticamente alla pagina principale di Contoso University in esecuzione in IIS nel computer locale.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Test nell'ambiente di Test

Si noti che l'indicatore di ambiente Visualizza "(Test)" anziché "(Dev)", che mostra che il *Web. config* trasformazione per l'indicatore di ambiente ha avuto esito positivo.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Eseguire la **studenti** pagina per verificare che il database distribuito non disponga di alcun studenti. Quando si seleziona questa pagina potrebbero occorrere alcuni minuti per caricare perché Code First consente di creare il database e quindi esegue il `Seed` (metodo). (Non che quando erano nella home page, poiché l'applicazione non tenta di accedere al database ancora.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Eseguire la **instructors (insegnanti)** pagina per verificare che Code First effettuato il seeding del database con dati instructor:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Selezionare **aggiungere gli studenti** dal **studenti** dal menu Aggiungi uno studente e quindi visualizzare il nuovo studente nella **studenti** pagina per verificare che è possibile scrivere nel database :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Dal **corsi** dal menu **crediti Update**. Il **aggiornamento crediti** pagina richiede autorizzazioni di amministratore, in modo che il **Accedi** viene visualizzata la pagina. Immettere le credenziali dell'account amministratore creato precedenti ("admin" e "Pas w0rd$"). Il **crediti Update** pagina viene visualizzata, verifica che l'account amministratore creato nell'esercitazione precedente è stato distribuito correttamente nell'ambiente di test.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Verificare che un *Elmah* esiste con solo il file segnaposto nella relativa cartella.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revisione delle modifiche automatico Web. config per le migrazioni Code First

Aprire il *Web. config* file nell'applicazione distribuita *C:\inetpub\wwwroot\ContosoUniversity* ed è possibile visualizzare dove il processo di distribuzione configurato le migrazioni Code First per automaticamente aggiornare il database alla versione più recente.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Il processo di distribuzione creato anche una nuova stringa di connessione per le migrazioni Code First usare esclusivamente per l'aggiornamento dello schema del database:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Questa stringa di connessione aggiuntive consente di specificare un account utente per gli aggiornamenti dello schema di database e un account utente diverso per accedere ai dati dell'applicazione. Ad esempio, è possibile assegnare il database\_ruolo di proprietario per le migrazioni Code First e db\_datareader e db\_datawriter ruoli all'applicazione. Si tratta di un modello comune di difesa in profondità che impedisce a codice potenzialmente dannoso dell'applicazione di modificare lo schema del database. (Ad esempio, questa situazione può verificarsi in un attacco injection SQL.) Questo modello non viene utilizzato da queste esercitazioni. Non è applicabile a SQL Server Compact, e non è applicabile quando esegue la migrazione a SQL Server in un'esercitazione successiva di questa serie. Il sito Cytanium offre solo un account utente per l'accesso al database di SQL Server creato su Cytanium. Se si è in grado di implementare questo modello nel proprio scenario, è possibile farlo attenendosi alla procedura seguente:

1. Nel **impostazioni** scheda della finestra di **Pubblica sito Web** procedura guidata, immettere la stringa di connessione che specifica l'utente con autorizzazioni per l'aggiornamento dello schema completo del database e deselezionare il **Usa questa stringa di connessione in fase di esecuzione** casella di controllo. Nel file Web. config distribuito, questo diventa il `DatabasePublish` stringa di connessione.
2. Creare una trasformazione del file Web. config per la stringa di connessione che si desidera che l'applicazione da usare in fase di esecuzione.

Abbiamo distribuito l'applicazione in IIS nel computer di sviluppo e testata non esiste. Verifica che il processo di distribuzione copiato il contenuto dell'applicazione nel percorso corretto (esclusi i file non desiderati per la distribuzione) e anche tale distribuzione Web configurato IIS correttamente durante la distribuzione. Nella prossima esercitazione, si eseguirà un test più che consente di trovare un'attività di distribuzione che non è ancora stato fatto: impostazione delle autorizzazioni di cartella per il *Elmah* cartella.

## <a name="more-information"></a>Altre informazioni

Per informazioni sull'esecuzione di IIS o IIS Express in Visual Studio, vedere le risorse seguenti:

- [Panoramica di IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) sul sito IIS.net.
- [Introduzione a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) sul blog Guthrie.
- [Procedura: specificare il Server Web per i progetti Web in Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Principali differenze tra IIS e ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) sul sito ASP.NET.
- [Verifica di ASP.NET MVC o applicazione Web Form in IIS 7 in 30 secondi](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) sul blog di Rick Anderson. Questa voce vengono forniti esempi di perché il test con Visual Studio Development Server (Cassini) non è così affidabile come test in IIS Express e il motivo per cui il test in IIS Express non è così affidabile come test in IIS.

Per informazioni su quali problemi potrebbero verificarsi quando l'applicazione viene eseguita in attendibilità media, vedere [che ospita le applicazioni ASP.NET in attendibilità media](http://www.4guysfromrolla.com/articles/100307-1.aspx) sui 4 utenti malintenzionati Rolla sito.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
