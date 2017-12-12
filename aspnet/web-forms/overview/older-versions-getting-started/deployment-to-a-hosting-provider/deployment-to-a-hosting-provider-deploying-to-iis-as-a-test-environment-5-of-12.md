---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Distribuzione di un''applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione in IIS come ambiente di Test - 5 di 12 | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: a5538744dfaff76f28c5f17d8f5d782ef3f6c118
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione in IIS come ambiente di Test - 5 di 12
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione di serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire l'App Web di servizio App di Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come distribuire un'applicazione web ASP.NET in IIS nel computer locale.

Quando si sviluppa un'applicazione, verifica in genere eseguendolo in Visual Studio. Per impostazione predefinita, pertanto che si utilizza Visual Studio Development Server (anche noto come Cassini). Visual Studio Development Server semplifica test durante lo sviluppo in Visual Studio, ma non funziona esattamente come IIS. Di conseguenza, è possibile che le applicazioni verranno eseguite correttamente quando si eseguirne il test in Visual Studio, ma può avere esito negativo quando viene distribuito a IIS in un ambiente host.

È possibile testare l'applicazione in modo più affidabile nei modi seguenti:

1. Usa IIS Express o completa di IIS anziché Visual Studio Development Server quando esegue il test durante lo sviluppo in Visual Studio. Questo metodo emula in genere in modo più accurato modalità di esecuzione del sito in IIS. Tuttavia, questo metodo non test del processo di distribuzione o la convalida che il risultato del processo di distribuzione verrà eseguito correttamente.
2. Distribuire l'applicazione di IIS sul computer di sviluppo utilizzando lo stesso processo che verrà usato in seguito possibile distribuirlo in ambiente di produzione. Questo metodo convalida il processo di distribuzione, oltre a convalidare che l'applicazione verrà eseguita correttamente in IIS.
3. Distribuire l'applicazione in un ambiente di test che è il più vicino possibile all'ambiente di produzione. Poiché l'ambiente di produzione per queste esercitazioni è un provider di hosting di terze parti, l'ambiente di test ideale sarebbe un secondo account con il provider di hosting. Utilizzare questo secondo account solo per i test, ma potrebbe essere configurato esattamente come l'account di produzione.

In questa esercitazione vengono illustrati i passaggi per l'opzione 2. Vengono fornite istruzioni per l'opzione 3 alla fine del [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione e alla fine di questa esercitazione sono disponibili collegamenti a risorse per l'opzione 1.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Configurazione dell'applicazione per l'esecuzione in attendibilità media

Prima di installare IIS e distribuzione di ad esso, sarà necessario sostituire un'impostazione del file Web. config per eseguire il sito più come verranno eseguite in un ambiente di hosting condiviso tipico.

I provider di hosting in genere esegue il sito web in *attendibilità media*, ovvero non è possibile effettuare quanto segue. Ad esempio, il codice dell'applicazione non è possibile accedere al Registro di Windows e non può leggere o scrivere file che non rientrano nella gerarchia di cartelle dell'applicazione. Per impostazione predefinita, l'applicazione viene eseguita *di attendibilità elevato* nel computer locale, il che significa che l'applicazione potrebbe essere in grado di eseguire operazioni che avrà esito negativo quando viene distribuito nell'ambiente di produzione. Pertanto, per rendere l'ambiente di test che riflettere più accuratamente l'ambiente di produzione, si configurerà l'esecuzione in attendibilità media dell'applicazione.

Nel file Web. config dell'applicazione, aggiungere un **trust** elemento il **system.web** elemento, come illustrato in questo esempio.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

L'applicazione verrà ora eseguita in attendibilità media in IIS anche sul computer locale. Questa impostazione consente di intercettare il prima possibile tutti i tentativi dal codice dell'applicazione per eseguire un'operazione che avrà esito negativo in fase di produzione.

> [!NOTE]
> Se si utilizza Entity Framework migrazioni Code First, assicurarsi di avere la versione 5.0 o versione successiva. In Entity Framework, versione 4.3, migrazioni richiede l'attendibilità totale per aggiornare lo schema del database.


## <a name="installing-iis-and-web-deploy"></a>Installare IIS e distribuzione Web

Per distribuire in IIS nel computer di sviluppo, è necessario che IIS e distribuzione Web installato. Che non sono inclusi nella configurazione predefinita di Windows 7. Se è già stato installato IIS e distribuzione Web, passare alla sezione successiva.

Utilizzo di [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx) è il modo migliore per installare IIS e distribuzione Web, perché l'installazione guidata piattaforma Web installa una configurazione consigliata per IIS e installa automaticamente i prerequisiti per IIS e Web Se è necessario distribuire.

Per eseguire l'installazione guidata piattaforma Web per installare IIS e distribuzione Web, utilizzare il collegamento seguente. Se è già stato installato IIS, distribuzione Web o dei relativi componenti necessari, l'installazione guidata piattaforma Web installa solo le funzionalità mancanti.

- [Installare IIS e distribuzione Web tramite WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>L'impostazione di Pool di applicazioni predefinito in .NET 4

Dopo aver installato IIS, eseguire **Gestione IIS** per assicurarsi che .NET Framework versione 4 sia assegnato al pool di applicazioni predefinito.

Da Windows **avviare** dal menu **eseguire**, immettere "inetmgr" e quindi fare clic su **OK**. (Se il **eseguire** comando non è nel **avviare** menu, è possibile premere il tasto Windows e R per aprirlo. O fare clic sulla barra delle applicazioni, fare clic su **proprietà**, selezionare il **Menu Start** scheda, fare clic su **Personalizza**e selezionare **comando**.)

Nel **connessioni** riquadro espandere il nodo del server e selezionare **pool di applicazioni**. Nel **pool di applicazioni** riquadro se **DefaultAppPool** è assegnato a .NET framework versione 4, come illustrato nella figura seguente, passare alla sezione successiva.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Se viene visualizzata solo due pool di applicazioni e di entrambi gli elementi impostati per .NET Framework 2.0, è necessario installare ASP.NET 4 in IIS:

- Aprire una finestra del prompt dei comandi facendo clic **prompt dei comandi** nelle finestre **avviare** menu e selezionando **Esegui come amministratore**. Eseguire quindi [aspnet\_regiis.exe](https://msdn.microsoft.com/en-us/library/k6h9cz8h.aspx) per installare ASP.NET 4 in IIS, utilizzando i comandi seguenti. (In sistemi a 64 bit, sostituire "Framework" con "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Questo comando Crea nuovo pool di applicazioni per .NET Framework 4, ma il pool di applicazioni predefinito verrà comunque impostato a 2.0. Verrà distribuita un'applicazione che ha come destinazione .NET 4 al pool di applicazioni, è necessario modificare il pool di applicazioni in .NET 4.

Se è stata chiusa **Gestione IIS**, eseguirlo nuovamente, espandere il nodo del server e fare clic su **pool di applicazioni** per visualizzare il **pool di applicazioni** nuovo riquadro.

Nel **pool di applicazioni** riquadro, fare clic su **DefaultAppPool**e quindi la **azioni** fare clic su riquadro **le impostazioni di base**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

Nel **modifica Pool di applicazioni** nella finestra di dialogo Modifica **versione di .NET Framework** a **.NET Framework v 4.0.30319** e fare clic su **OK**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

A questo punto si è pronti per la pubblicazione in IIS.

## <a name="publishing-to-iis"></a>Pubblicazione in IIS

Esistono diversi modi, è possibile distribuire usando Visual Studio 2010 e distribuzione Web:

- Utilizzare Visual Studio pubblicazione con un clic.
- Creare un *pacchetto di distribuzione* e installarlo utilizzando la UI Gestione IIS. Il pacchetto di distribuzione è costituito un *zip* file che contiene tutti i file e i metadati necessari per installare un sito in IIS.
- Creare un pacchetto di distribuzione e installarlo mediante la riga di comando.

Il processo che è parlato nelle esercitazioni precedenti per configurare Visual Studio per automatizzare le attività di distribuzione si applica a tutti i tre metodi seguenti. In queste esercitazioni si utilizzerà il primo di questi metodi. Per informazioni sull'utilizzo di pacchetti di distribuzione, vedere [mappa del contenuto di distribuzione di ASP.NET](https://msdn.microsoft.com/en-us/library/bb386521.aspx).

Prima della pubblicazione, assicurarsi che si esegue Visual Studio in modalità amministratore. (In Windows 7 **avviare** menu, fare clic sull'icona per la versione di Visual Studio in uso e selezionare **Esegui come amministratore**.) Modalità di amministratore è necessaria per la pubblicazione solo quando esegue la pubblicazione in IIS nel computer locale.

In **Esplora**, fare clic sul progetto ContosoUniversity (non il progetto ContosoUniversity.DAL) e selezionare **pubblica**.

Il **pubblica sul Web** procedura guidata viene visualizzata.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Nell'elenco a discesa, selezionare  **&lt;New... &gt;**.

Nel **nuovo profilo** nella finestra di dialogo immettere "Test" e quindi fare clic su **OK**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Questo nome è che lo stesso come nodo intermedio del Web.Test.config trasformare il file creato in precedenza. Questa corrispondenza è quello che determina le trasformazioni Web.Test.config da applicare durante la pubblicazione utilizzando questo profilo.

La procedura guidata passa automaticamente al **connessione** scheda.

Nel **URL del servizio** immettere *localhost*.

Nel **sito/applicazione** immettere *Default Web Site/ContosoUniversity*.

Nel **URL di destinazione** immettere `http://localhost/ContosoUniversity`.

Il **URL di destinazione** impostazione non è necessario. Al termine Visual Studio distribuisce l'applicazione, viene automaticamente aperto il browser predefinito per questo URL. Se non si desidera il browser per aprire automaticamente dopo la distribuzione, lasciare vuota questa casella.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Fare clic su **convalida connessione** per verificare che le impostazioni siano corrette e che è possibile connettersi a IIS nel computer locale.

Un segno di spunta verde verifica che la connessione ha esito positivo.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Fare clic su **Avanti** per spostare il **impostazioni** scheda.

Il **configurazione** casella di riepilogo a discesa specifica configurazione della build da distribuire. Il valore predefinito è una versione, che si desidera eseguire.

Lasciare il **Rimuovi file aggiuntivi nella destinazione** casella di controllo deselezionata. Poiché si tratta della prima distribuzione, vi sarà tutti i file nella cartella di destinazione ancora.

Nel **database** sezione, immettere il valore seguente nella casella stringa di connessione per **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Il processo di distribuzione verrà inserito la stringa di connessione nel file Web. config distribuito perché **utilizza stringa di connessione in fase di esecuzione** è selezionata.

Anche in **SchoolContext**selezionare **applicare le migrazioni Code First**. Questa opzione fa sì che il processo di distribuzione configurare il file Web. config distribuito per specificare il `MigrateDatabaseToLatestVersion` inizializzatore. L'inizializzatore Aggiorna automaticamente il database alla versione più recente quando l'applicazione accede al database per la prima volta dopo la distribuzione.

Nella casella stringa di connessione per **DefaultConnection**, immettere il valore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Lasciare **Aggiorna database** deselezionata. Verrà distribuito il database delle appartenenze copiando il file. sdf nell'App\_dati e non si desidera il processo di distribuzione per eseguire tutte le operazioni con il database.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Fare clic su **Avanti** per spostare il **anteprima** scheda.

Nel **anteprima** scheda, fare clic su **avviare Anteprima** per visualizzare un elenco dei file che verranno copiati.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Fare clic su **Pubblica**.

Se Visual Studio non è in modalità amministratore, è possibile che venga visualizzato un messaggio di errore che indica un errore di autorizzazione. In tal caso, chiudere Visual Studio, aprirlo in modalità amministratore e provare di nuovo la pubblicazione.

Se Visual Studio è in modalità amministratore, il **Output** finestra report ha esito positivo, compilare e pubblicare.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Il browser verrà aperta automaticamente per Contoso University Home page in esecuzione in IIS nel computer locale.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Test nell'ambiente di Test

Si noti che l'indicatore di ambiente illustra "(Test)" anziché "(Dev)", che indica che il *Web. config* trasformazione per l'indicatore di ambiente ha avuto esito positivo.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Eseguire il **studenti** pagina per verificare che il database distribuito non ha studenti. Quando si seleziona questa pagina potrebbe richiedere alcuni minuti per caricare perché il primo codice viene creato il database e quindi esegue il `Seed` metodo. (Non eseguire questa operazione quando erano nella home page, poiché l'applicazione non tenta di accedere ancora al database.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Eseguire il **i docenti** pagina per verificare che il primo codice seeding del database con dati istruttore:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Selezionare **aggiungere studenti** dal **studenti** menu, aggiungere uno studente e quindi visualizzare il nuovo studente nel **studenti** pagina per verificare che è possibile scrivere nel database :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Dal **corsi** dal menu **aggiornamento crediti**. Il **aggiornamento crediti** pagina richiede autorizzazioni di amministratore, pertanto la **Accedi** viene visualizzata la pagina. Immettere le credenziali dell'account amministratore creato precedenti ("admin" e "Pa$ w0rd"). Il **aggiornamento crediti** viene visualizzata la pagina, che verifica che l'account amministratore creato nell'esercitazione precedente è stata distribuita correttamente nell'ambiente di testing.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Verificare che un *Elmah* cartella esiste con solo il file segnaposto in essa contenuti.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revisione delle modifiche automatico Web. config per le migrazioni Code First

Aprire il *Web. config* file nell'applicazione distribuita in *C:\inetpub\wwwroot\ContosoUniversity* ed è possibile visualizzare in cui il processo di distribuzione configurato migrazioni Code First per automaticamente aggiornare il database alla versione più recente.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Il processo di distribuzione creata anche una nuova stringa di connessione per le migrazioni Code First da utilizzare esclusivamente per l'aggiornamento dello schema di database:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

La stringa di connessione aggiuntive consente di specificare un account utente per gli aggiornamenti dello schema di database e un account utente diverso per accedere ai dati dell'applicazione. Ad esempio, è possibile assegnare il db\_il ruolo di proprietario per migrazioni Code First e db\_datareader e db\_datawriter ruoli all'applicazione. Si tratta di un modello comune di difesa in profondità che impedisce di codice potenzialmente dannoso nell'applicazione di modificare lo schema del database. (Ad esempio, questo potrebbe verificarsi in un attacco injection SQL.) Questo modello non è usato da queste esercitazioni. Non si applica a SQL Server Compact, e non si applica quando esegue la migrazione a SQL Server in un'esercitazione successiva di questa serie. Il sito Cytanium offre un unico account utente per l'accesso al database di SQL Server creato su Cytanium. Se si è in grado di implementare questo pattern nello scenario, è possibile farlo eseguendo i passaggi seguenti:

1. Nel **impostazioni** scheda della finestra di **pubblica sul Web** procedura guidata, specificare la stringa di connessione che specifica un utente con autorizzazioni per l'aggiornamento dello schema completo del database e deselezionare il **utilizza stringa di connessione in fase di esecuzione** casella di controllo. Nel file Web. config distribuito, questa diventa il `DatabasePublish` stringa di connessione.
2. Creare una trasformazione del file Web. config per la stringa di connessione che si desidera che l'applicazione da utilizzare in fase di esecuzione.

È ora distribuito l'applicazione in IIS nel computer di sviluppo e testato non esiste. Verifica che il processo di distribuzione copiato il contenuto dell'applicazione nel percorso corretto (esclusi i file non desiderati per la distribuzione) e anche che distribuzione Web configurato IIS correttamente durante la distribuzione. Nell'esercitazione successiva, verrà eseguito un test più che consente di trovare un'attività di distribuzione che non è ancora stata eseguita: impostazione delle autorizzazioni di cartella per il *Elmah* cartella.

## <a name="more-information"></a>Altre informazioni

Per informazioni sull'esecuzione di IIS o IIS Express in Visual Studio, vedere le risorse seguenti:

- [Panoramica di IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) nel sito IIS.net.
- [Introduzione a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) sul blog di Scott Guthrie.
- [Procedura: specificare il Server Web per i progetti Web in Visual Studio](https://msdn.microsoft.com/en-us/library/ms178108.aspx).
- [Principali differenze tra IIS e il Server di sviluppo ASP.NET](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) sul sito ASP.NET.
- [Testare l'applicazione Web Form in IIS 7 o ASP.NET MVC in 30 secondi](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) sul blog di Rick Anderson. Questa voce vengono forniti esempi di perché il test con Visual Studio Development Server (Cassini) non è così affidabile come test in IIS Express e perché il test in IIS Express non è così affidabile come test in IIS.

Per informazioni su quali problemi possono verificarsi durante l'applicazione viene eseguita in attendibilità media, vedere [Hosting di applicazioni ASP.NET in attendibilità media](http://www.4guysfromrolla.com/articles/100307-1.aspx) sugli Guy dal sito Rolla 4.

>[!div class="step-by-step"]
[Precedente](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
[Successivo](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
