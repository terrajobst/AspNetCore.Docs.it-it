---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: "Distribuzione Web ASP.NET utilizzando Visual Studio: la distribuzione nell'ambiente di produzione | Documenti Microsoft"
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: f3b3898bd003ace100ba05619f2c45ca808462df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: la distribuzione nell'ambiente di produzione
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione, impostare un account di Microsoft Azure, creare ambienti di gestione temporanea e produzione e distribuire l'applicazione web ASP.NET per la gestione temporanea e ambienti di produzione utilizzando Visual Studio fare clic su una funzionalità di pubblicazione.

Se si preferisce, è possibile distribuire a un provider di hosting di terze parti. La maggior parte delle procedure descritte in questa esercitazione sono gli stessi per un provider di hosting o per Azure, ad eccezione del fatto che ogni provider è un'interfaccia utente per la gestione di account e il sito web. È possibile trovare un provider di hosting nel [raccolta di provider di](https://www.microsoft.com/web/hosting) sul sito web Microsoft.com.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di selezionare la pagina di risoluzione dei problemi in questa serie di esercitazioni.

## <a name="get-a-microsoft-azure-account"></a>Ottenere un account di Microsoft Azure

Se si dispone già di un account Azure, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Creare un ambiente di gestione temporanea

> [!NOTE]
> Dopo la scrittura di questa esercitazione, il servizio App di Azure aggiunta una nuova funzionalità per automatizzare molti processi per la creazione di ambienti di gestione temporanea e produzione. Vedere [configurare ambienti per le app web in Azure App Service di gestione temporanea](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Come spiegato nel [distribuzione per l'esercitazione di ambiente di Test](deploying-to-iis.md), più ambiente di test affidabile è un sito web al provider di hosting che ha come sito web di produzione. Molti provider di hosting è necessario valutare i vantaggi di questo oggetto in costi aggiuntivi significativi, ma in Azure è possibile creare un'app web gratuito aggiuntive come l'applicazione di gestione temporanea. È inoltre necessario un database e i costi aggiuntivi per che tramite il costo del database di produzione sarà Nessuno o minima. In Azure, si paga per la quantità di archiviazione di database da utilizzare invece che per ogni database e la quantità di memoria aggiuntiva da usare in gestione temporanea sarà minima.

Come spiegato nel [distribuzione per l'esercitazione di ambiente di Test](deploying-to-iis.md), in gestione temporanea e produzione che si intende distribuire i due database in un database. Se si desidera mantenere separate, il processo saranno gli stessi ad eccezione del fatto che si creerà un altro database per ogni ambiente e selezionare la stringa di destinazione corretto per ogni database quando si crea il profilo di pubblicazione.

In questa sezione dell'esercitazione si creerà un'app web e il database da utilizzare per l'ambiente di gestione temporanea e verrà distribuzione di gestione temporanea e test presente prima della creazione e la distribuzione nell'ambiente di produzione.

> [!NOTE]
> La procedura seguente viene illustrato come creare un'app web in Azure App Service tramite il portale di gestione di Azure. Nella versione più recente di Azure SDK, è possibile farlo anche senza uscire da Visual Studio, tramite Esplora Server. In Visual Studio 2013, è anche possibile creare un'app web direttamente dalla finestra di dialogo di pubblicazione. Per altre informazioni, vedere [creare un'app web ASP.NET in Azure App Service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. Nel [il portale di gestione di Azure](https://manage.windowsazure.com/), fare clic su **siti Web**, quindi fare clic su **New**.
2. Fare clic su **sito Web**, quindi fare clic su **creazione personalizzata**.

    Il **nuovo sito Web - creazione personalizzata** apre la procedura guidata. Il **creazione personalizzata** procedura guidata consente di creare un sito web e un database nello stesso momento.
3. Nel **Crea sito Web** passaggio della procedura guidata, immettere una stringa di **URL** casella da usare come URL univoco per l'applicazione di gestione temporanea di ambiente. Ad esempio, immettere ContosoUniversity-staging123 (inclusi i numeri casuali alla fine per renderla univoca nel caso in cui viene eseguita ContosoUniversity-gestione temporanea).

    L'URL completo sarà costituito da quelle immesse, più il suffisso che viene visualizzato accanto alla casella di testo.
4. Nel **area** elenco a discesa scegliere l'area più vicina all'utente.

    Questa impostazione specifica quale data center in cui verrà eseguito l'app web.
5. Nel **Database** elenco a discesa scegliere **creare un nuovo database SQL**.
6. Nel **DB Connection String Name** , lasciare il valore predefinito, *DefaultConnection*.
7. Fare clic sulla freccia rivolta verso destra nella parte inferiore della finestra.

    La figura seguente mostra il **Crea sito Web** finestra di dialogo con valori di esempio in essa contenuti. L'URL e l'area che è stato immesso sarà diverso.

    ![Creare il passaggio sito Web](deploying-to-production/_static/image1.png)

    Verrà visualizzata la **specifica impostazioni database** passaggio.
8. Nel **nome** immettere *ContosoUniversity* insieme a un numero casuale per renderlo univoco, ad esempio *ContosoUniversity123*.
9. Nel **Server** , quindi selezionare **nuovo Server di Database SQL**.
10. Immettere un nome dell'amministratore e una password.

    Non immettere un nome esistente e la password qui. Si sta immettendo un nuovo nome e una password che si sta definendo ora per l'utilizzo in un secondo momento quando si accede al database.
11. Nel **area** scegliere la stessa area scelto per l'app web.

    Mantenere il server web e il server di database nella stessa area offre le migliori prestazioni e riduce al minimo le spese.
12. Fare clic sul segno di spunta nella parte inferiore della finestra per indicare che si è finito.

    La figura seguente mostra il **specifica impostazioni database** finestra di dialogo con valori di esempio in essa contenuti. I valori immessi siano diversi.

    ![Passaggio di impostazioni di database del sito Web di New - creare con la procedura guidata Database](deploying-to-production/_static/image2.png)

    Il portale di gestione restituisce alla pagina di siti Web e **stato** colonna Mostra la creazione di app web. Dopo un periodo di tempo (in genere inferiore a un minuto), il **stato** colonna mostra che l'app web è stato creato. Nella barra di spostamento a sinistra, il numero di App web sono presenti account viene visualizzato accanto al **siti Web** icona e il numero di database viene visualizzato accanto al **database SQL** icona.

    ![Pagina di siti Web del portale di gestione, il sito web creato](deploying-to-production/_static/image3.png)

    Nome dell'app web sarà diverso dell'applicazione di esempio nella figura.

## <a name="deploy-the-application-to-staging"></a>Distribuire l'applicazione di gestione temporanea

Dopo avere creato un'app web e i database per l'ambiente di gestione temporanea, è possibile distribuire il progetto a esso.

> [!NOTE]
> Queste istruzioni viene illustrato come creare un profilo di pubblicazione scaricando un *publishsettings* file, che funziona non solo per Azure ma anche per i provider di hosting di terze parti. La versione più recente Azure SDK consente inoltre di connettersi direttamente ad Azure da Visual Studio e scegliere da un elenco di App web presenti nel proprio account Azure. In Visual Studio 2013, è possibile accedere ad Azure dal **pubblicazione sul Web** finestra di dialogo o dal **Esplora Server** finestra. Per ulteriori informazioni, vedere [creare un'app web ASP.NET in Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Scaricare il file con estensione publishsettings

1. Fare clic sul nome dell'app web appena creato.

    ![Fare clic sul sito per passare al Dashboard](deploying-to-production/_static/image4.png)
2. In **Quick glance** nel **Dashboard** scheda, fare clic su **Scarica profilo di pubblicazione**.

    ![Collegamento profilo di pubblicazione per il download](deploying-to-production/_static/image5.png)

    Questo passaggio consente di scaricare un file che contiene tutte le impostazioni necessarie per distribuire un'applicazione in app web. Si importerà il file in Visual Studio non è necessario immettere manualmente queste informazioni.
3. Salvare il *publishsettings* file in una cartella che è possibile accedere da Visual Studio.

    ![salvataggio del file con estensione publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Sicurezza: il *publishsettings* file contiene le credenziali (non codificate) utilizzate per amministrare i servizi e le sottoscrizioni di Azure. Il livello di protezione ottimale per il file consiste nell'archiviarlo temporaneamente all'esterno delle directory di origine (ad esempio nella cartella raccolte\documenti) e quindi eliminarlo una volta completata l'importazione. Un utente malintenzionato che accede al *publishsettings* file può modificare, creare ed eliminare i servizi di Azure.

### <a name="create-a-publish-profile"></a>Creare un profilo di pubblicazione

1. In Visual Studio, fare clic sul progetto ContosoUniversity in **Esplora** e selezionare **pubblica** dal menu di scelta rapida.

    Il **pubblica sul Web** apre la procedura guidata.
2. Fare clic su di **profilo** scheda.
3. Fare clic su **Importa**.
4. Passare il *publishsettings* file scaricato in precedenza e quindi fare clic su **aprire**.

    ![La finestra di dialogo di importazione delle impostazioni di pubblicazione](deploying-to-production/_static/image7.png)
5. Nel **connessione** scheda, fare clic su **convalida connessione** per assicurarsi che le impostazioni siano corrette.

    Quando la connessione è stata convalidata, un segno di spunta verde viene visualizzato accanto al **convalida connessione** pulsante.

    Per alcuni provider di hosting, quando si fa clic **convalida connessione**, potrebbe essere visualizzato un **Errore certificato** la finestra di dialogo. In caso contrario, verificare che il nome del server è quello previsto. Se il nome del server sia corretto, selezionare **Salva il certificato per le sessioni future di Visual Studio** e fare clic su **Accept**. (Questo errore indica che il provider di hosting ha scelto di evitare il costo di acquisto di un certificato SSL per l'URL a cui si distribuisce in. Se si preferisce per stabilire una connessione protetta con un certificato valido, contattare il provider di hosting.)
6. Scegliere **Avanti**.

    ![icona della connessione ha esito positivo e il pulsante Avanti nella scheda connessione](deploying-to-production/_static/image8.png)
7. Nel **impostazioni** espandere **Opzioni pubblicazione File**, quindi selezionare **escludere i file dall'App\_cartella dati**.

    Per informazioni sulle altre opzioni in **Opzioni pubblicazione File**, vedere il [distribuzione in IIS](deploying-to-iis.md) esercitazione. L'immagine che mostra il risultato di questo passaggio e i seguenti passaggi di configurazione di database si trova alla fine dei passaggi di configurazione del database.
8. In **DefaultConnection** nel **database** sezione, configurare la distribuzione di database per il database delle appartenenze.
9. 1. Selezionare **Aggiorna database**.

        Il **stringa di connessione remota** casella direttamente sotto **DefaultConnection** viene compilato con la stringa di connessione dal file con estensione publishsettings. La stringa di connessione include le credenziali di SQL Server, che vengono archiviate in testo normale nel *pubxml* file. Se si preferisce non archiviarli in modo permanente non esiste, è possibile rimuoverli dal profilo di pubblicazione dopo aver distribuito il database e archiviarle in Azure. Per ulteriori informazioni, vedere [come proteggere il database ASP.NET le stringhe di connessione durante la distribuzione in Azure dall'origine](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) sul blog di Scott Hanselman.
      2. Fare clic su **configurare gli aggiornamenti di database**.
      3. Nel **Configura Aggiornamenti Database** la finestra di dialogo, fare clic su **aggiungere Script SQL**.
      4. Nel **aggiungere Script SQL** passare al *aspnet-data-prod.sql* script che è salvato in precedenza nella cartella della soluzione e quindi fare clic su **aprire**.
      5. Chiudi il **Configura Aggiornamenti Database** la finestra di dialogo.
10. In **SchoolContext** nel **database** selezionare **eseguire migrazioni Code First (esecuzione all'avvio dell'applicazione)**.

    Visual Studio visualizza **eseguire le migrazioni Code First** anziché **aggiornamento Database** per `DbContext` classi. Se si desidera utilizzare il provider dbDacFx anziché le migrazioni per distribuire un database a cui accede tramite un `DbContext` classe, vedere [come distribuire un database senza migrazioni Code First?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) nelle domande frequenti di distribuzione Web per Visual Studio e ASP.NET su MSDN.

    Il **impostazioni** scheda avrà ora un aspetto analogo al seguente:

    ![Scheda Impostazioni per la gestione temporanea](deploying-to-production/_static/image9.png)
11. Eseguire la procedura seguente per salvare il profilo e rinominarlo in *gestione temporanea*:

    1. Fare clic su di **profilo** scheda e quindi fare clic su **Gestione profili**.
    2. L'importazione creati due nuovi profili, uno per FTP e uno per distribuzione Web. È stato configurato il profilo di distribuzione Web: rinominare questo profilo per *gestione temporanea*.

        ![Rinominare un profilo di gestione temporanea](deploying-to-production/_static/image10.png)
    3. Chiudi il **modifica dei profili di pubblicazione Web** la finestra di dialogo.
    4. Chiudi il **pubblica sul Web** procedura guidata.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Configurare una trasformazione del profilo di pubblicazione per l'indicatore di ambiente

> [!NOTE]
> In questa sezione viene illustrato come configurare una trasformazione Web. config per l'indicatore di ambiente. Poiché è l'indicatore di `<appSettings>` elemento, si dispone di un'altra alternativa per specificare la trasformazione quando si distribuisce in Azure App Service. Per ulteriori informazioni, vedere [config che specifica le impostazioni in Azure](web-config-transformations.md#watransforms).


1. In **Esplora**, espandere **proprietà**, quindi espandere **PublishProfiles**.
2. Fare doppio clic su *Staging.pubxml*, quindi fare clic su **aggiungere Configurazione trasformazione**.

    ![Aggiungere una trasformazione di configurazione per la gestione temporanea](deploying-to-production/_static/image11.png)

    Visual Studio crea il *Web.Staging.config* file di trasformazione e lo apre.
3. Nel *Web.Staging.config* trasformare il file, inserire il codice seguente immediatamente dopo l'apertura `configuration` tag.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Quando si utilizza il profilo di pubblicazione di gestione temporanea, la trasformazione imposta l'indicatore di ambiente per "Produzione". Nell'app web distribuite, sarà possibile vedere un suffisso, ad esempio "(Dev)" o "(Test)" dopo l'intestazione H1 "Contoso University".
4. Fare doppio clic su di *Web.Staging.config* file e fare clic su **anteprima trasformare** per assicurarsi che la trasformazione è codificato produce modifiche previste.

    Il **anteprima Web. config** finestra viene visualizzato il risultato dell'applicazione sia di *Web.Release.config* Trasforma e *Web.Staging.config* Trasforma.

### <a name="prevent-public-use-of-the-test-app"></a>Impedire l'uso pubblico delle app di test

Una considerazione importante per l'applicazione di gestione temporanea è che verrà visualizzato in tempo reale su Internet, ma non si desidera pubblico per utilizzarlo. Per ridurre al minimo la probabilità che gli utenti verranno trovare e utilizzarlo, è possibile utilizzare uno o più dei metodi seguenti:

- Impostare le regole firewall che consentono l'accesso all'app di gestione temporanea solo da indirizzi IP utilizzati per test di gestione temporanea.
- Utilizzare un URL offuscato che sarà impossibile da indovinare.
- Creare un *robots* file per garantire che i motori di ricerca verranno non ricerca per indicizzazione collegamenti app e i report di test a esso nei risultati della ricerca.

Il primo di questi metodi è più efficace, ma non viene descritta in questa esercitazione, poiché richiede che viene distribuita a un servizio Cloud di Azure invece di servizio App di Azure. Per ulteriori informazioni sui servizi Cloud e le restrizioni IP in Azure, vedere [calcolo Hosting opzioni fornite da Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) e [blocco di indirizzi IP specifici di accedere a un ruolo Web](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Se si distribuisce in un provider di hosting di terze parti, contattare il provider per scoprire come implementare le restrizioni IP.

Per questa esercitazione si creerà un *robots* file.

1. In **Esplora**, fare clic sul progetto ContosoUniversity e fare clic su **Aggiungi nuovo elemento**.
2. Creare un nuovo **File di testo** denominato *robots*e inserire il testo seguente:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    Il `User-agent` riga indica i motori di ricerca che le regole nel file si applicano a tutti di crawler motore ricerca (robot), e `Disallow` riga specifica che non le pagine del sito devono essere ricerca per indicizzazione.

    Si desidera motori di ricerca di catalogo app di produzione, è necessario escludere questo file dalla distribuzione di produzione. Per eseguire, che è possibile configurare un'impostazione per la produzione profilo di pubblicazione durante la creazione.

### <a name="deploy-to-staging"></a>Distribuzione di gestione temporanea

1. Aprire il **pubblica sul Web** guidata facendo clic su progetto University Contoso e facendo clic su **pubblica**.
2. Assicurarsi che il **gestione temporanea** è selezionato un profilo.
3. Fare clic su **Pubblica**.

    Il **Output** finestra Mostra intraprese le azioni di distribuzione e segnala il completamento corretto della distribuzione. Il browser predefinito verrà aperta automaticamente per l'URL dell'app web distribuite.

## <a name="test-in-the-staging-environment"></a>Test nell'ambiente di gestione temporanea

Si noti che l'indicatore di ambiente è assente (nessun "(test)" o "(Dev)" dopo l'intestazione H1, indicante che il *Web. config* trasformazione per l'indicatore di ambiente ha avuto esito positivo.

![Gestione temporanea home page](deploying-to-production/_static/image12.png)

Eseguire il **studenti** pagina per verificare che il database distribuito non ha studenti.

Eseguire il **i docenti** pagina per verificare che il primo codice seeding del database con dati istruttore:

Selezionare **aggiungere studenti** dal **studenti** menu, aggiungere uno studente e quindi visualizzare il nuovo studente nel **studenti** pagina per verificare che è possibile scrivere nel database .

Dal **corsi** pagina, fare clic su **aggiornamento crediti**. Il **aggiornamento crediti** pagina richiede autorizzazioni di amministratore, pertanto la **Accedi** viene visualizzata la pagina. Immettere le credenziali dell'account amministratore creato precedenti ("admin" e "prodpwd"). Il **aggiornamento crediti** viene visualizzata la pagina, che verifica che l'account amministratore creato nell'esercitazione precedente è stata distribuita correttamente nell'ambiente di testing.

Richiedere un URL non valido per causare un errore che ELMAH registrerà e quindi richiedono il report di errore ELMAH. Se si distribuisce in un provider di hosting di terze parti, è probabile che il report è vuoto per lo stesso motivo che era vuoto nell'esercitazione precedente. È necessario utilizzare gli strumenti di gestione di account del provider di hosting per configurare le autorizzazioni per abilitare ELMAH scrivere nella cartella log.

L'applicazione creata è in esecuzione nel cloud in un'app web che è esattamente come si utilizzerà per la produzione. Poiché tutto funziona correttamente, il passaggio successivo consiste nel distribuire nell'ambiente di produzione.

## <a name="deploy-to-production"></a>Distribuire nell'ambiente di produzione

Il processo per creare un'app web di produzione e la distribuzione nell'ambiente di produzione è la stessa di gestione temporanea, ad eccezione del fatto che sia necessario escludere il *robots* dalla distribuzione. A tale scopo si modificherà il file del profilo di pubblicazione.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Creare l'ambiente di produzione e la produzione di profilo di pubblicazione

1. Creare l'app web di produzione e i database in Azure, seguire la stessa procedura utilizzata per la gestione temporanea.

    Quando si crea il database, è possibile scegliere di inserirlo nello stesso server creato in precedenza o creare un nuovo server.
2. Scaricare il *publishsettings* file.
3. Creare il profilo di pubblicazione mediante l'importazione di produzione *publishsettings* file, seguire la stessa procedura utilizzata per la gestione temporanea.

    Non dimenticare di configurare lo script di distribuzione di dati in **DefaultConnection** nel **database** sezione la **impostazioni** scheda.
4. Rinominare il profilo di pubblicazione per *produzione*.
5. Configurare una trasformazione del profilo di pubblicazione per l'indicatore di ambiente, seguire la stessa procedura utilizzata per la gestione temporanea...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Modificare il file con estensione pubxml per escludere i file robots.txt

Denominare i file di profilo di pubblicazione &lt;profilename&gt;*pubxml* e si trovano nel *PublishProfiles* cartella. Il *PublishProfiles* cartella si trova nel *proprietà* cartella in un'applicazione web c# del progetto, nel *progetto* cartella in un progetto di applicazione web di Visual Basic o sotto il *App\_dati* cartella in un progetto di app web. Ogni *pubxml* file contiene le impostazioni che si applicano a un profilo di pubblicazione. I valori che immessi nella procedura guidata Pubblica sito Web vengono archiviati in questi file, ed è possibile modificare in modo da creare o modificare le impostazioni che non sono resi disponibili nell'interfaccia utente Visual Studio.

Per impostazione predefinita, *pubxml* file sono inclusi nel progetto quando si crea un profilo di pubblicazione, ma è possibile escluderli dal progetto e verrà comunque usare Visual Studio. Cui Visual Studio cerca il *PublishProfiles* cartella per *pubxml* file, indipendentemente dai inclusi nel progetto.

Per ogni *pubxml* file è presente un *. pubxml.user* file. Il *. pubxml.user* file contiene la password crittografata se è stata selezionata la **Salva password** opzione e per impostazione predefinita esso viene escluso dal progetto.

Oggetto *pubxml* file contiene le impostazioni che riguardano un profilo di pubblicazione specifico. Se si desidera configurare le impostazioni che si applicano a tutti i profili, è possibile creare un *. wpp.targets* file. Il processo di compilazione Importa questi file nel *csproj* o *vbproj* file di progetto, pertanto la maggior parte delle impostazioni che è possibile configurare nel file di progetto possono essere configurate in questi file. Per ulteriori informazioni su *pubxml* file e *. wpp.targets* file, vedere [come: modificare le impostazioni di distribuzione nei file di profilo di pubblicazione (con estensione pubxml) e. wpp.targets File in Visual Studio Progetti Web](https://msdn.microsoft.com/library/ff398069.aspx).

1. In **Esplora**, espandere **proprietà** espandere **PublishProfiles**.
2. Fare doppio clic su *Production.pubxml* e fare clic su **aprire**.

    ![Aprire il file pubxml](deploying-to-production/_static/image13.png)
3. Fare doppio clic su *Production.pubxml* e fare clic su **aprire**.
4. Aggiungere le seguenti righe immediatamente prima della chiusura `PropertyGroup` elemento:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Il file con estensione pubxml avrà ora un aspetto analogo al seguente:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Per ulteriori informazioni su come escludere file e cartelle, vedere [è escludere specifici file o cartelle dalla distribuzione?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) nel **domande frequenti su distribuzione Web per Visual Studio e ASP.NET** su MSDN.

### <a name="deploy-to-production"></a>Distribuire nell'ambiente di produzione

1. Aprire il **pubblica sul Web** guidata assicurarsi che il **produzione** profilo di pubblicazione sia selezionata e quindi fare clic su **avviare Anteprima** sul **anteprima**scheda per verificare che il *robots* file non verrà copiati all'app di produzione.

    ![Anteprima del file da pubblicare in produzione](deploying-to-production/_static/image14.png)

    Esaminare l'elenco dei file che verranno copiati. Si noterà che tutti i *cs* file, inclusi *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, e  *Master.Designer.cs* file sono stati omessi. Tutto il codice compilato nel *ContosoUniversity.dll* e *ContosUniversity.pdb* file che si trova nel *bin* cartella. Poiché solo il *DLL* è necessaria per eseguire l'applicazione ed è specificato in precedenza che devono essere distribuiti solo i file necessari per eseguire l'applicazione, no *cs* file sono stati copiati nella destinazione ambiente. Il *obj* cartella e *ContosoUniversity.csproj* e *. csproj* file sono stati omessi per lo stesso motivo.

    Fare clic su **pubblica** per la distribuzione nell'ambiente di produzione.
2. Test nell'ambiente di produzione, seguire la stessa procedura che è utilizzato per la gestione temporanea.

    Tutto ciò che è identica alla gestione temporanea tranne l'URL e l'assenza del *robots* file.

## <a name="summary"></a>Riepilogo

È ora correttamente distribuito e testato l'app web ed è disponibile pubblicamente su Internet.

![Home page produzione](deploying-to-production/_static/image15.png)

Nella prossima esercitazione, verrà di aggiornare il codice dell'applicazione e distribuzione della modifica per gli ambienti di test, gestione temporanea e produzione.

> [!NOTE]
> Mentre l'applicazione è in uso nell'ambiente di produzione deve implementare un piano di ripristino. Deve essere periodicamente backup dei database da app di produzione in un percorso di archiviazione sicura e si devono mantenere diverse generazioni di tali backup. Quando si aggiorna il database, è necessario creare una copia di backup da immediatamente prima della modifica. Quindi, se si commette un errore e non individuarla solo dopo aver distribuito nell'ambiente di produzione, sarà comunque in grado di ripristinare il database allo stato che precedente danneggiamento. Per ulteriori informazioni, vedere [Database SQL di Azure Backup e ripristino](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> In questa esercitazione di SQL Server edizione che si desidera distribuire a è il Database SQL di Azure. Durante il processo di distribuzione è simile ad altre edizioni di SQL Server, un'applicazione di produzione reale potrebbe richiedere un codice speciale per il Database SQL di Azure in alcuni scenari. Per ulteriori informazioni, vedere [utilizzo con Database SQL di Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) e [scelta tra SQL Server e Database SQL di Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Precedente](setting-folder-permissions.md)
> [Successivo](deploying-a-code-update.md)
