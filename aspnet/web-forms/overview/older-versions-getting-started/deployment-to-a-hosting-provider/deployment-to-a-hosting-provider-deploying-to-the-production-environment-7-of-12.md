---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione nell'ambiente di produzione - 7 dei 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 26a832fd336f886ba1ddfb1682930afa4df56c58
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383636"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione nell'ambiente di produzione - 7 dei 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e Mostra come distribuire in App Web di servizio App di Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione si crea un account con un provider di hosting e distribuire l'ASP.NET funzionalità di pubblicazione dell'applicazione web nell'ambiente di produzione usando Visual Studio con un clic.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Selezione di un Provider di Hosting

Per l'applicazione Contoso University e questa serie di esercitazioni, è necessario un provider che supporta ASP.NET 4 e distribuzione Web. Una società di hosting specifica è stato scelto in modo che le esercitazioni è stato possibile illustrare un'esperienza completa di distribuzione in un sito Web in tempo reale. Ogni società di hosting fornisce diverse funzionalità e l'esperienza di distribuzione dei server varia alquanto. Tuttavia, la procedura descritta in questa esercitazione è tipica per il processo complessivo. Il provider di hosting utilizzato per questa esercitazione, Cytanium.com, è uno dei numerosi disponibili e l'utilizzo in questa esercitazione non costituisce un vuole raccomandare né consigliare.

Quando si è pronti per selezionare il provider di hosting, è possibile confrontare le funzionalità e prezzi nel [raccolta di provider](https://www.microsoft.com/web/hosting) sul sito Web Microsoft.com/.

## <a name="creating-an-account"></a>Creazione di un Account

Creare un account presso il provider selezionato. Se il supporto per un database di SQL Server completo è un'aggiunta molto, non è necessaria per selezionarlo per questa esercitazione, ma sarà necessaria per la [la migrazione a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) esercitazione più avanti in questa serie.

Per queste esercitazioni, non devi registrare un nuovo nome di dominio. È possibile testare per verificare la corretta distribuzione usando l'URL temporaneo assegnato al sito dal provider.

Dopo aver creato l'account, in genere si riceverà un messaggio di benvenuto che contiene tutte le informazioni che necessarie per distribuire e gestire il sito. Le informazioni che invia il provider di hosting sarà simile a quanto illustrato qui. Il messaggio di benvenuto Cytanium che viene inviato ai nuovi proprietari dell'account include le informazioni seguenti:

- L'URL al sito di Pannello di controllo del provider, in cui è possibile gestire le impostazioni per il sito. L'ID e la password specificati sono inclusi in questa parte del messaggio di benvenuto per riferimento. (Entrambe sono stati modificati in un valore di demo per illustrare questo concetto.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- La versione di .NET Framework predefinito e informazioni su come modificarla. Molti hosting siti predefiniti per 2.0, che funziona con applicazioni ASP.NET destinate a .NET Framework 2.0, 3.0 o 3.5. Tuttavia Contoso University è un'applicazione .NET Framework 4, pertanto è necessario modificare questa impostazione. (Per un'applicazione ASP.NET 4.5 si utilizzerebbe l'impostazione di .NET 4.0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- URL temporaneo che è possibile usare per accedere al sito web. Quando questo account è stato creato, "contosouniversity.com" è stato immesso il nome di dominio esistente. È pertanto l'URL temporaneo `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informazioni su come configurare il database e le stringhe di connessione necessarie per accedervi:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informazioni su strumenti e le impostazioni per la distribuzione del sito. (Il messaggio di posta elettronica da Cytanium tralascia neanche la WebMatrix, che è omesso qui.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Per impostare la versione di .NET Framework

Il messaggio di benvenuto Cytanium include un collegamento alle istruzioni su come modificare la versione di .NET Framework. Queste istruzioni spiegano che questa operazione può essere eseguita tramite il pannello di controllo Cytanium. Altri provider di avere siti di Pannello di controllo che hanno un aspetto diversi o potrebbe essere richiesto di eseguire questa operazione in modo diverso.

Passare all'URL del Pannello di controllo. Dopo l'accesso con il nome utente e password, il pannello di controllo è visibile.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

Nel **Hosting spazi** finestra, posizionare il puntatore del mouse sull'icona Web e selezionare **siti Web** dal menu di scelta.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

Nel **siti Web** fare clic su **contosouniversity.com** (il nome del sito usato durante la creazione dell'account).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

Nel **proprietà-sito Web** , quindi selezionare la **estensioni** scheda.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

ASP.NET da modificare **2.0 Pipeline integrata** a **4.0 (Pipeline integrata)**, quindi fare clic su **Update**.

## <a name="publishing-to-the-hosting-provider"></a>Pubblicazione sul Provider di Hosting

Il messaggio di benvenuto da provider di hosting include tutte le impostazioni che necessarie per pubblicare il progetto è possibile immettere manualmente le informazioni in un profilo di pubblicazione. Ma si userà una più semplice e meno soggetta a errori metodo per configurare la distribuzione del provider: è possibile scaricare una *publishsettings* file e importarlo in un profilo di pubblicazione.

Nel browser, passare al pannello di controllo Cytanium e selezionare **Web** e quindi selezionare **siti Web.**

![Pannello di controllo selezione di siti Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Selezionare il **contosouniversity.com** sito web.

![Pannello di controllo selezionando contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Selezionare il **pubblicazione sul Web** scheda.

![Scheda Pubblicazione sul Web di Pannello di controllo](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Creare le credenziali da usare per il web, immettere un nome utente e una password di pubblicazione. È possibile immettere le stesse credenziali che usano per accedere al pannello di controllo. Quindi fare clic su **abilitare**.

![Pannello di controllo creare credenziali di pubblicazione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Fare clic su **Download Publishing Profile per il sito web**.

![Controllo pannello Scarica profilo di pubblicazione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Quando viene chiesto di aprire o salvare il file, salvare il file.

![Salvare il file di profilo di pubblicazione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

Nelle **Esplora soluzioni** in Visual Studio, fare clic sul progetto ContosoUniversity e selezionare **Publish**. Il **pubblica sul Web** verrà visualizzata la finestra di dialogo nel **anteprima** scheda con il **Test** profilo selezionato perché questo è il profilo ultimo è stato usato.

Selezionare il **profilo** scheda e quindi fare clic su **importazione**.

![Pulsante di importazione guidata Web pubblica](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

Nel **importare le impostazioni di pubblicazione** finestra di dialogo, seleziona la *con estensione publishsettings* file exe scaricato e fare clic su **Open**. Verrà visualizzata la scheda connessione con tutti i campi compilati.

![Scheda Connessione guidata Web di pubblicazione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Il file con estensione publishsettings, l'URL di permanente pianificato per il sito viene inserito nella casella URL di destinazione, ma se ancora non hanno acquistato tale dominio, sostituire il valore con l'URL temporaneo. In questo esempio è l'URL  *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com).* L'unico scopo di questa casella è di specificare l'URL verrà aperto il browser per automaticamente dopo aver correttamente dopo la distribuzione. Se si lascia vuoto, l'unica conseguenza è che il browser non verrà avviato automaticamente dopo la distribuzione.

Fare clic su **convalida connessione** per verificare che le impostazioni siano corrette e che è possibile connettersi al server. Come illustrato in precedenza, un segno di spunta verde verifica che la connessione ha esito positivo.

Quando si fa clic su convalida connessione, si potrebbe notare un **errore di certificato** nella finestra di dialogo. Se esegue l'operazione, verificare che il nome del server è quello previsto. Se è disponibile, selezionare **salvare questo certificato per le sessioni future di Visual Studio** e fare clic su **Accept**. (Questo errore indica che il provider di hosting ha scelto di evitare le spese per l'acquisto di un certificato SSL per l'URL a cui viene eseguita la distribuzione. Se si preferisce per stabilire una connessione protetta con un certificato valido, contattare il provider di hosting.)

![Errore di certificato](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Scegliere **Avanti**.

Nel **database** sezione del **impostazioni** , immettere lo stesso profilo di pubblicazione ai valori immessi per il Test. Le stringhe di connessione che necessarie sono disponibili negli elenchi a discesa.

- Nella casella stringa di connessione per **SchoolContext,** selezionare `Data Source=|DataDirectory|School-Prod.sdf`
- Sotto **SchoolContext**, selezionare **applicare le migrazioni Code First**.
- Nella casella stringa di connessione per **DefaultConnection**selezionare `Data Source=|DataDirectory|aspnet-Prod.sdf`
- Sotto **DefaultConnection**, lasciare **Aggiorna database** deselezionata.

![Scheda delle impostazioni di creazione guidata Web pubblica](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Scegliere **Avanti**.

Nel **Preview** scheda, fare clic su **Avvia anteprima** per visualizzare un elenco dei file che verranno copiati. Viene visualizzato l'elenco stesso che hai visto in precedenza quando è distribuita in IIS nel computer locale.

Prima di pubblicare, modificare il nome del profilo in modo che il file di trasformazione Web.Production.config verrà applicato. Selezionare il **profilo** scheda e fare clic su **Gestione profili**.

![Pubblicazione Web guidata gestione profili](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

Nel **modifica dei profili di pubblicazione Web** finestra di dialogo selezionare il profilo di produzione, fare clic su **rinominare**e modificare il nome del profilo nell'ambiente di produzione. Quindi fare clic su **Chiudi**.

![Finestra di dialogo profili di pubblicazione Web modifica](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Fare clic su **Pubblica**.

L'applicazione viene pubblicata per il provider di hosting. Il risultato Mostra nel **Output** finestra.

![Finestra di output dopo la distribuzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Si apre automaticamente il browser all'URL immesso nella **URL di destinazione** nella casella il **connessione** scheda della finestra di **pubblica sul Web** guidata. Viene visualizzata la pagina di casa stessa come quando si esegue il sito in Visual Studio, ma ora non è disponibile alcun "(Test)" o un indicatore di ambiente "(Dev)" nella barra del titolo. Ciò indica che l'indicatore di ambiente *Web. config* trasformazione funzionava correttamente.

> [!NOTE]
> Se viene ancora visualizzato "(Test)" nell'intestazione, eliminare il *obj* cartella dal progetto ContosoUniversity e ridistribuire. Nelle versioni provvisorie del software, il file di trasformazione applicate in precedenza (Web.Test.config) potrebbe essere applicato anche in questo caso anche se si usa il profilo di produzione.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Prima di eseguire una pagina che fa sì che l'accesso al database, assicurarsi che Elmah saranno in grado di registrare eventuali errori che si verificano.

## <a name="setting-folder-permissions-for-elmah"></a>Impostazione delle autorizzazioni di cartella per Elmah

Come si ricorderà dall'esercitazione precedente di questa serie, è necessario assicurarsi che l'applicazione disponga delle autorizzazioni di scrittura per la cartella dell'applicazione in cui Elmah archivia i file di log degli errori. Quando è distribuita in IIS in locale nel computer, impostare le autorizzazioni manualmente. In questa sezione verrà illustrato come impostare le autorizzazioni in Cytanium. (Alcuni provider di hosting potrebbe non consentono di eseguire questa operazione, può offrire una o più cartelle predefinite con le autorizzazioni di scrittura. In questo caso è necessario modificare l'applicazione per usare le cartelle specificate.)

Nel Pannello di controllo Cytanium, è possibile impostare le autorizzazioni della cartella. Passare al controllo pannello URL e selezionare **gestione File**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

Nel **gestione File** , quindi selezionare **contosouniversity.com** e quindi **wwwrooot** per visualizzare una cartella radice dell'applicazione. Fare clic sull'icona del lucchetto accanto a **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

Nel **File**/**autorizzazioni delle cartelle** finestra, seleziona il **lettura** e **scrivere** caselle di controllo per  **contosouniversity.com** e fare clic su **impostare le autorizzazioni**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Assicurarsi che Elmah abbia accesso in scrittura per il *Elmah* cartella provoca un errore e quindi visualizzando il report errori Elmah. Richiedere un URL non valido, ad esempio *Studentsxxx.aspx*. Come in precedenza, viene visualizzato il *GenericErrorPage* pagina. Fare clic sui **Disconnetti** collegamento e quindi eseguire *Elmah.axd*. Viene visualizzato il **Log In** pagina in primo luogo, che verifica che il *Web. config* trasformazione è stata aggiunta Elmah autorizzazione. Dopo l'accesso, noterete che il report che mostra l'errore che è causato proprio.

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Test nell'ambiente di produzione

Eseguire la **studenti** pagina. L'applicazione tenta di accedere al database dell'istituto di istruzione per la prima volta, che attiva le migrazioni Code First per creare il database. Quando la pagina viene visualizzata dopo il ritardo di qualche secondo, indica che non ci sono Nessun studenti.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Eseguire la **instructors (insegnanti)** pagina per verificare che i dati di seeding è stato inserito dati instructor nel database.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Come è stato fatto nell'ambiente di test, si desidera verificare che gli aggiornamenti del database di lavoro nell'ambiente di produzione, ma in genere non desidera digitare i dati di test nel database di produzione. Per questa esercitazione si userà il metodo di stesso che è stato eseguito nel test. Ma in un'applicazione reale che si potrebbe voler trovare un metodo che verifica che database vengano completati gli aggiornamenti senza introdurre dati di test nel database di produzione. In alcune applicazioni, potrebbe essere pratico aggiungere un elemento e quindi eliminarlo.

Aggiungere uno studente e quindi visualizzare i dati immessi nel **studenti** pagina per verificare che è possibile aggiornare i dati nel database.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Verificare che le regole di autorizzazione sono funzioni correttamente selezionando **aggiornamento crediti** dalle **corsi** menu. Il **Log In** viene visualizzata la pagina. Immettere le credenziali dell'account amministratore, fare clic su **Log In**e il **crediti Update** viene visualizzata la pagina.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Se l'account di accesso ha esito positivo, il **crediti Update** viene visualizzata la pagina. Ciò indica che il database delle appartenenze ASP.NET (con l'account amministratore single) è stato distribuito correttamente.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Abbiamo correttamente distribuito e testato il sito ed è disponibile pubblicamente su Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Creazione di un ambiente di Test più affidabile

Come spiegato nel [distribuzione nell'ambiente di Test di](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) esercitazione, l'ambiente di test più affidabile sarebbe un secondo account presso il provider di hosting che dispone esattamente come l'account di produzione. Questo potrebbe rivelarsi più costoso rispetto all'uso di IIS locale come ambiente di test, poiché è necessario effettuare l'iscrizione per un secondo account di hosting. Ma se evita che gli errori del sito di produzione o interruzioni del servizio, è possibile decidere che vale la pena il costo.

La maggior parte del processo di creazione e la distribuzione in un account di prova è simile a ciò che è già stato per la distribuzione nell'ambiente di produzione:

- Creare un *Web. config* file di trasformazione.
- Creare un account presso il provider di hosting.
- Creare un nuovo profilo di pubblicazione e distribuzione per l'account di test.

### <a name="preventing-public-access-to-the-test-site"></a>Impedisce l'accesso pubblico al sito di Test

Una considerazione importante per l'account di test è che sarà in tempo reale su Internet, ma si preferisce non al pubblico per usarlo. Per mantenere privato il sito è possibile usare uno o più dei metodi seguenti:

- Contattare il provider di hosting per impostare le regole del firewall che consentono l'accesso al sito di test solo da indirizzi IP usati per il test.
- Camuffare URL in modo che non è simile all'URL del sito pubblico.
- Usare un *robots* file per garantire che i motori di ricerca verranno non una ricerca per indicizzazione collegamenti di sito e i report di test ad esso nei risultati della ricerca.

Il primo di questi metodi è ovviamente la più sicura, ma la procedura per cui è specifica per ciascun provider di hosting e non trattata in questa esercitazione. Se le Disponi con provider di hosting per consentire solo l'indirizzo IP per passare all'URL dell'account di test, in teoria non è necessario preoccuparsi di ricerca per indicizzazione, i motori di ricerca. Ma anche in questo caso, la distribuzione di un *robots* file è una buona idea come backup nel caso in cui tale regola del firewall è disattivato inserisca accidentalmente.

Il *robots* file va inserito nella cartella del progetto e deve contenere il testo seguente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

Il `User-agent` riga indica che le regole nel file di si applicano a tutti i ricerca web dai crawler del motore (robot), i motori di ricerca e `Disallow` righe specifica che non devono essere una ricerca per indicizzazione le pagine del sito.

Probabilmente si desidera che i motori di ricerca per catalogare sito di produzione, pertanto è necessario escludere questo file dalla distribuzione di produzione. A tale scopo, vedere **è possibile escludere cartelle o file specifici dalla distribuzione?** nelle [domande frequenti sulla distribuzione di ASP.NET Web Application progetto](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Assicurarsi di specificare l'esclusione solo per la produzione di profilo di pubblicazione.

Creazione di un secondo account di hosting è un approccio all'uso di un ambiente di test che non è obbligatorio, ma potrebbe valere la pena il costo aggiuntivo. Le esercitazioni seguenti, si continuerà a usare IIS come ambiente di test.

Nella prossima esercitazione, si sarà aggiornare il codice dell'applicazione e distribuire le modifiche apportate agli ambienti di test e produzione.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
