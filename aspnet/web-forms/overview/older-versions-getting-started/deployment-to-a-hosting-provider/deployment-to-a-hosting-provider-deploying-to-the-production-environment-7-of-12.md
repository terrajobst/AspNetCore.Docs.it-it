---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Distribuzione di un''applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione nell''ambiente di produzione - 7 di 12 | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4aa6766c2c7765f499f5c5380962a5fe443e8c9d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione nell'ambiente di produzione - 7 di 12
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione di serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire l'App Web di servizio App di Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione, impostare un account con un provider di hosting e la distribuzione di ASP.NET funzionalità di pubblicazione dell'applicazione web nell'ambiente di produzione con Visual Studio con un clic.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Selezione di un Provider di Hosting

Per l'applicazione Contoso University e questa serie di esercitazioni, è necessario un provider che supporti ASP.NET 4 e distribuzione Web. Una società di hosting specifica è stato scelto in modo che le esercitazioni possano illustrare un'esperienza completa di distribuzione in un sito Web in tempo reale. Ogni società di hosting fornisce diverse funzionalità e l'esperienza di distribuzione per i server varia leggermente. Tuttavia, il processo descritto in questa esercitazione è tipico per l'intero processo. Il provider di hosting utilizzato per questa esercitazione, Cytanium.com, è uno dei numerosi bug che sono disponibili e il relativo utilizzo in questa esercitazione non costituisce una raccomandazione o verifica dell'autenticità.

Quando si è pronti per selezionare il proprio provider di hosting, è possibile confrontare le funzionalità e prezzi di [raccolta di provider di](https://www.microsoft.com/web/hosting) sul sito Web Microsoft.com.

## <a name="creating-an-account"></a>Creazione di un Account

Creare un account presso il provider selezionato. Se il supporto per un database di SQL Server completo è un'aggiunta extra, non è necessario selezionarlo per questa esercitazione, ma è necessario per il [la migrazione a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) esercitazione più avanti in questa serie.

Per queste esercitazioni, non è necessario registrare un nuovo nome di dominio. È possibile testare per verificare la corretta distribuzione tramite l'URL temporaneo assegnato al sito dal provider.

Dopo aver creato l'account, in genere si riceve un messaggio di benvenuto che contiene tutte le informazioni che necessarie per distribuire e gestire il sito. Le informazioni che invia al provider di hosting sarà simile a quello mostrato di seguito. Il messaggio di benvenuto Cytanium che viene inviato ai proprietari di nuovi account include le informazioni seguenti:

- L'URL al sito di Pannello di controllo del provider, in cui è possibile gestire le impostazioni per il sito. L'ID e la password specificati sono inclusi in questa parte del messaggio di posta elettronica di benvenuto, come riferimento. (Entrambi sono stati modificati in un valore demo per questa illustrazione.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- La versione di .NET Framework predefinita e informazioni su come modificarlo. Molti hosting siti predefiniti 2.0, che funziona con applicazioni ASP.NET destinate a .NET Framework 2.0, 3.0 o 3.5. Tuttavia University Contoso è un'applicazione .NET Framework 4, è necessario modificare questa impostazione. (Per un'applicazione ASP.NET 4.5 si utilizzerà l'impostazione di .NET 4.0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- L'URL temporanea che è possibile utilizzare per accedere al sito web. Quando questo account è stato creato, "contosouniversity.com" è stato immesso come nome di dominio esistente. È pertanto l'URL temporaneo `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informazioni su come impostare il database e le stringhe di connessione necessarie per accedervi:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informazioni sugli strumenti e le impostazioni per la distribuzione del sito. (Messaggio di posta elettronica da Cytanium tralascia neanche WebMatrix, che viene omesso qui.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Impostazione della versione di .NET Framework

Il messaggio di benvenuto Cytanium include un collegamento per istruzioni su come modificare la versione di .NET Framework. Queste istruzioni spiegano che questa operazione può essere eseguita tramite il pannello di controllo Cytanium. Altri provider di avere siti di Pannello di controllo che hanno un aspetto diversi o potrebbe essere richiesto di eseguire questa operazione in modo diverso.

Passare all'URL del Pannello di controllo. Dopo l'accesso con il nome utente e password, viene visualizzato il pannello di controllo.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

Nel **Hosting spazi** , posizionare il puntatore del mouse sull'icona Web e selezionare **siti Web** dal menu.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

Nel **siti Web** fare clic su **contosouniversity.com** (il nome del sito che è stato utilizzato durante la creazione di account).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

Nel **proprietà sito Web** , quindi selezionare il **estensioni** scheda.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Modificare ASP.NET da **2.0 Pipeline integrata** a **4.0 (Pipeline integrata)**, quindi fare clic su **aggiornamento**.

## <a name="publishing-to-the-hosting-provider"></a>Pubblicazione per il Provider di Hosting

Il messaggio di benvenuto dal provider di hosting include tutte le impostazioni che necessarie per pubblicare il progetto è possibile immettere manualmente le informazioni in un profilo di pubblicazione. Ma si userà un più semplice e meno soggetto a errori metodo per configurare la distribuzione per il provider: verrà scaricato un *publishsettings* file e importarla in un profilo di pubblicazione.

Nel browser, passare al pannello di controllo Cytanium e selezionare **Web** e quindi selezionare **siti Web.**

![Pannello di selezione di siti Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Selezionare il **contosouniversity.com** sito web.

![Pannello di controllo selezionando contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Selezionare il **pubblicazione sul Web** scheda.

![Scheda Pubblicazione sul Web Pannello di controllo](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Creare le credenziali da utilizzare per immettere un nome utente e una password di pubblicazione sul web. È possibile immettere le stesse credenziali che consentono di accedere al pannello di controllo. Quindi fare clic su **abilitare**.

![Pannello di controllo creare le credenziali di pubblicazione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Fare clic su **scaricare il profilo di pubblicazione per il sito web**.

![Controllo pannello Scarica profilo di pubblicazione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Quando viene chiesto di aprire o salvare il file, salvarlo.

![Salvare il file di profilo di pubblicazione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

In **Esplora** in Visual Studio, fare clic sul progetto ContosoUniversity e selezionare **pubblica**. Il **pubblica sul Web** visualizzata nella finestra di dialogo di **anteprima** scheda con il **Test** profilo selezionato perché è l'ultimo profilo è stato utilizzato.

Selezionare il **profilo** scheda e quindi fare clic su **importazione**.

![Pulsante di importazione guidata Web pubblica](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

Nel **importazione delle impostazioni di pubblicazione** la finestra di dialogo, seleziona il *publishsettings* file scaricato e fare clic su **aprire**. La procedura guidata passa alla scheda connessione con tutti i campi compilati.

![Scheda di connessione Web guidata di pubblicazione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Il file con estensione publishsettings, l'URL di permanente pianificato per il sito viene inserito nella casella URL di destinazione, ma se non acquistata ancora tale dominio, è possibile sostituire il valore con l'URL temporaneo. Per questo esempio, l'URL è  *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com).* L'unico scopo di questa casella è di specificare l'URL verrà aperto il browser per automaticamente dopo aver correttamente dopo la distribuzione. Se si lascia vuota, l'unica conseguenza è che il browser non verrà avviata automaticamente dopo la distribuzione.

Fare clic su **convalida connessione** per verificare che le impostazioni siano corrette e che è possibile connettersi al server. Come illustrato in precedenza, un segno di spunta verde verifica che la connessione ha esito positivo.

Quando si fa clic su convalida connessione, potresti vedere un **Errore certificato** la finestra di dialogo. In caso contrario, verificare che il nome del server è quello previsto. In tal caso, selezionare **Salva il certificato per le sessioni future di Visual Studio** e fare clic su **Accept**. (Questo errore indica che il provider di hosting ha scelto di evitare il costo di acquisto di un certificato SSL per l'URL a cui si distribuisce in. Se si preferisce per stabilire una connessione protetta con un certificato valido, contattare il provider di hosting.)

![Errore del certificato](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Scegliere **Avanti**.

Nel **database** sezione la **impostazioni** , immettere lo stesso profilo di pubblicazione ai valori immessi per il Test. Sono disponibili le stringhe di connessione che è necessario negli elenchi a discesa.

- Nella casella stringa di connessione per **SchoolContext,** selezionare`Data Source=|DataDirectory|School-Prod.sdf`
- In **SchoolContext**selezionare **applicare le migrazioni Code First**.
- Nella casella stringa di connessione per **DefaultConnection**selezionare`Data Source=|DataDirectory|aspnet-Prod.sdf`
- In **DefaultConnection**, lasciare **Aggiorna database** deselezionata.

![Scheda Impostazioni di Web guidata di pubblicazione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Scegliere **Avanti**.

Nel **anteprima** scheda, fare clic su **avviare Anteprima** per visualizzare un elenco dei file che verranno copiati. Visualizzare l'elenco stesso che illustrato in precedenza quando è stato distribuito a IIS nel computer locale.

Prima di pubblicare, modificare il nome del profilo in modo che il file di trasformazione Web.Production.config verrà applicato. Selezionare il **profilo** scheda e fare clic su **Gestione profili**.

![Pubblicazione Web guidata gestione profili](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

Nel **modifica dei profili di pubblicazione Web** finestra di dialogo selezionare il profilo di produzione, fare clic su **rinominare**e modificare il nome del profilo nell'ambiente di produzione. Quindi fare clic su **Chiudi**.

![Modificare la finestra di dialogo profili di pubblicazione Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Fare clic su **Pubblica**.

L'applicazione viene pubblicata per il provider di hosting. Mostra il risultato di **Output** finestra.

![Finestra di output dopo la distribuzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Il browser apre automaticamente l'URL immesso nel **URL di destinazione** casella il **connessione** scheda della finestra il **pubblica sul Web** procedura guidata. Home page stesso viene visualizzato come quando si esegue il sito in Visual Studio, ma ora non è presente alcuna "(Test)" o di un indicatore di ambiente "(Dev)" nella barra del titolo. Ciò indica che l'indicatore di ambiente *Web. config* trasformazione funzionava correttamente.

> [!NOTE]
> Se ancora "(Test)" nel titolo, eliminare il *obj* cartella dal progetto ContosoUniversity e ridistribuire. Nelle versioni non definitive del software, il file di trasformazione applicata in precedenza (Web.Test.config) potrebbe ottenere nuovamente applicato anche se si utilizza il profilo di produzione.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Prima di eseguire una pagina che causa l'accesso al database, verificare che Elmah saranno in grado di registrare gli eventuali errori che si verificano.

## <a name="setting-folder-permissions-for-elmah"></a>Impostazione delle autorizzazioni di cartella per Elmah

Come si ricorderà dall'esercitazione precedente di questa serie, è necessario assicurarsi che l'applicazione disponga delle autorizzazioni di scrittura per la cartella dell'applicazione in cui Elmah archivia i file di log degli errori. Quando è stato distribuito a IIS in locale nel computer in uso, impostare le autorizzazioni manualmente. In questa sezione, si noterà come impostare le autorizzazioni a Cytanium. (Alcuni provider di hosting non consente di eseguire questa operazione, può offrire uno o più cartelle predefinite con autorizzazioni di scrittura. In questo caso è necessario modificare l'applicazione per utilizzare le cartelle specificate.)

È possibile impostare autorizzazioni per le cartelle nel Pannello di controllo Cytanium. Passare al controllo del pannello URL e selezionare **gestione File**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

Nel **gestione File** , quindi selezionare **contosouniversity.com** e quindi **wwwrooot** per visualizzare una cartella radice dell'applicazione. Fare clic sull'icona del lucchetto accanto a **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

Nel **File**/**autorizzazioni cartella** finestra, seleziona il **lettura** e **scrivere** caselle di controllo per  **contosouniversity.com** e fare clic su **impostare le autorizzazioni**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Assicurarsi che Elmah abbia accesso in scrittura per il *Elmah* cartella provoca un errore e quindi visualizzando il report di errore Elmah. Richiedere un URL non valido come *Studentsxxx.aspx*. Come in precedenza, viene visualizzato il *GenericErrorPage* pagina. Fare clic su di **Log Out** collegamento e quindi eseguire *Elmah.axd*. Viene visualizzato il **Accedi** pagina in primo luogo, che verifica che il *Web. config* trasformazione aggiunto Elmah autorizzazione. Dopo l'accesso, vedrai il report che contiene l'errore è causato sufficiente.

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Test nell'ambiente di produzione

Eseguire il **studenti** pagina. L'applicazione tenta di accedere al database dell'istituto di istruzione per la prima volta che attiva le migrazioni Code First per creare il database. Quando la pagina viene visualizzata dopo il ritardo di qualche istante, mostra che non esistono alcun studenti.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Eseguire il **i docenti** pagina per verificare che i dati del valore di inizializzazione dati istruttore correttamente inseriti nel database.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Nell'ambiente di test, come si desidera verificare che gli aggiornamenti del database di lavoro nell'ambiente di produzione, ma in genere non si desidera immettere i dati di test nel database di produzione. Per questa esercitazione si utilizzerà lo stesso metodo che nel test. Ma in un'applicazione reale che si potrebbe voler trovare un metodo che verifica che il database vengano completati gli aggiornamenti senza introdurre dati di test nel database di produzione. In alcune applicazioni, potrebbe essere utile aggiungere un elemento e quindi eliminarlo.

Aggiungere uno studente e quindi visualizzare i dati immessi nel **studenti** pagina per verificare che è possibile aggiornare i dati nel database.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Convalidare che le regole di autorizzazione funzionino correttamente selezionando **aggiornamento crediti** dal **corsi** menu. Il **Accedi** viene visualizzata la pagina. Immettere le credenziali dell'account amministratore, fare clic su **Accedi**e **aggiornamento crediti** viene visualizzata la pagina.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Se l'account di accesso ha esito positivo, il **aggiornamento crediti** viene visualizzata la pagina. Ciò indica che il database delle appartenenze ASP.NET (con l'account amministratore single) è stato distribuito correttamente.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

È ora correttamente distribuito e testato il sito ed è disponibile pubblicamente su Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Creazione di un ambiente di Test più affidabile

Come spiegato nel [distribuzione nell'ambiente di Test di](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) esercitazione, l'ambiente di test più affidabile sarebbe un secondo account presso il provider di hosting che dispone esattamente come l'account di produzione. Questo potrebbe essere più costoso rispetto all'utilizzo di IIS locale come ambiente di test, poiché è necessario iscriversi a un secondo account di hosting. Ma se impedisce gli errori del sito di produzione o interruzioni del servizio, è possibile decidere che vale la pena il costo.

La maggior parte del processo di creazione e la distribuzione di un account di prova è simile a ciò che già fatto per la distribuzione nell'ambiente di produzione:

- Creare un *Web. config* file di trasformazione.
- Creare un account presso il provider di hosting.
- Creare un nuovo profilo di pubblicazione e distribuzione per l'account di prova.

### <a name="preventing-public-access-to-the-test-site"></a>Impedire l'accesso pubblico per il sito di Test

Una considerazione importante per l'account di test è che verrà visualizzato in tempo reale su Internet, ma non si desidera pubblico per utilizzarlo. Per mantenere privato il sito è possibile utilizzare uno o più dei metodi seguenti:

- Contattare il provider di hosting per impostare le regole firewall che consentono l'accesso al sito di test solo da indirizzi IP utilizzati per il test.
- Passare l'URL in modo che non è simile all'URL del sito pubblica.
- Utilizzare un *robots* file per garantire che i motori di ricerca verranno non ricerca per indicizzazione collegamenti di sito e i report di test a esso nei risultati della ricerca.

Il primo di questi metodi è ovviamente la più sicura, ma la procedura per che è specifica per ogni provider di hosting e non trattata in questa esercitazione. Se le Disponi con il provider di hosting per consentire solo l'indirizzo IP passare all'URL di account di test, in teoria non è necessario preoccuparsi di ricerca per indicizzazione, i motori di ricerca. Ma anche in questo caso, la distribuzione di un *robots* file è una buona idea come backup nel caso in cui tale regola del firewall è disattivato mai accidentalmente.

Il *robots* file va inserito nella cartella del progetto e deve contenere il testo seguente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

Il `User-agent` riga indica i motori di ricerca che le regole nel file si applicano a tutti di crawler motore ricerca (robot), e `Disallow` riga specifica che non le pagine del sito devono essere ricerca per indicizzazione.

Si vorrà motori di ricerca per catalogo del sito di produzione, è necessario escludere questo file dalla distribuzione di produzione. A tale scopo, vedere **è escludere specifici file o cartelle dalla distribuzione?** in [domande frequenti sulla distribuzione di ASP.NET Web applicazione progetto](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Assicurarsi di specificare l'esclusione solo per la produzione di profilo di pubblicazione.

Creazione di un account di hosting secondo è un approccio per l'utilizzo di un ambiente di test che non è obbligatorio, ma potrebbe essere utile il costo aggiuntivo. Nelle esercitazioni seguenti, si procederà a utilizzare IIS come ambiente di test.

Nella prossima esercitazione, si sarà aggiornare il codice dell'applicazione e distribuire le modifiche per gli ambienti di test e produzione.

>[!div class="step-by-step"]
[Precedente](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
[Successivo](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
