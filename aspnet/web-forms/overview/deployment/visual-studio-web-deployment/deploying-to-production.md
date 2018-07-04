---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: "Distribuzione Web ASP.NET tramite Visual Studio: la distribuzione nell'ambiente di produzione | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: 5bc39162bc53dc27230f8ccc55266212a51b6380
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369645"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Distribuzione Web ASP.NET tramite Visual Studio: distribuzione in produzione
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione si configura un account di Microsoft Azure, creare ambienti di staging e produzione e distribuisce l'applicazione web ASP.NET per la gestione temporanea e funzionalità di pubblicazione di ambienti di produzione usando Visual Studio con un clic.

Se si preferisce, è possibile distribuire in un provider di hosting di terze parti. La maggior parte delle procedure descritte in questa esercitazione sono gli stessi per un provider di hosting o per Azure, ad eccezione del fatto che ogni provider è la propria interfaccia utente per la gestione di account e il sito web. È possibile trovare un provider di hosting nel [raccolta di provider](https://www.microsoft.com/web/hosting) sul sito web Microsoft.com.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la pagina di risoluzione dei problemi in questa serie di esercitazioni.

## <a name="get-a-microsoft-azure-account"></a>Ottenere un account di Microsoft Azure

Se non si ha già un account Azure, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Creare un ambiente di staging

> [!NOTE]
> Poiché questa esercitazione è stata scritta, servizio App di Azure aggiunta una nuova funzionalità per automatizzare molti processi per la creazione di ambienti di staging e produzione. Visualizzare [configurare ambienti di staging per le app web nel servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Come spiegato nel [Distribuisci con l'esercitazione di ambiente di Test](deploying-to-iis.md), più ambiente di testing affidabile è un sito web al provider di hosting che dispone esattamente come il sito web di produzione. In molti provider di hosting è necessario valutare i vantaggi di questa in significativi costi aggiuntivi, ma in Azure è possibile creare un'app web gratuita aggiuntive come le app di gestione temporanea. È necessario anche un database e i costi aggiuntivi per tale failover il costo del database di produzione sarà entrambi nessuno o minimo. In Azure si paga la quantità di archiviazione del database che si utilizza invece che per ogni database e la quantità di spazio di archiviazione aggiuntivo che si useranno in gestione temporanea sarà minima.

Come spiegato nel [Distribuisci con l'esercitazione di ambiente di Test](deploying-to-iis.md), in gestione temporanea e produzione che si intende distribuire i due database in un unico database. Se si vuole mantenerli distinti, il processo è lo stesso ad eccezione del fatto che si crea un database aggiuntivo per ogni ambiente e scegliere la stringa di destinazione corretto per ogni database quando si crea il profilo di pubblicazione.

In questa sezione dell'esercitazione si creerà un'app web e database da utilizzare per l'ambiente di gestione temporanea e sarà distribuire in gestione temporanea e di test non esiste prima della creazione e la distribuzione nell'ambiente di produzione.

> [!NOTE]
> La procedura seguente illustra come creare un'app web in servizio App di Azure usando il portale di gestione di Azure. Nella versione più recente di Azure SDK, è possibile anche eseguire questa operazione senza uscire da Visual Studio, usando Esplora Server. In Visual Studio 2013, è anche possibile creare un'app web direttamente dalla finestra di dialogo pubblica. Per altre informazioni, vedere [creare un'app web ASP.NET in servizio App di Azure.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. Nel [portale di gestione di Azure](https://manage.windowsazure.com/), fare clic su **siti Web**, quindi fare clic su **New**.
2. Fare clic su **sito Web**, quindi fare clic su **creazione personalizzata**.

    Il **nuovo sito Web - creazione personalizzata** verrà visualizzata la procedura guidata. Il **creazione personalizzata** procedura guidata consente di creare un sito web e un database nello stesso momento.
3. Nel **Crea sito Web** passaggio della procedura guidata, immettere una stringa nel **URL** finestra da usare come URL univoco per l'applicazione dell'ambiente di staging. Ad esempio, immettere ContosoUniversity-staging123 (inclusi i numeri casuali alla fine per renderlo univoco nel caso in cui viene eseguita il ContosoUniversity-gestione temporanea).

    L'URL completo sarà costituito da quanto immesso e dal suffisso visualizzato accanto alla casella di testo.
4. Nel **regione** elenco a discesa scegliere l'area più vicina all'utente.

    Questa impostazione specifica l'app web viene eseguita in quale data center.
5. Nel **Database** elenco a discesa scegliere **creare un nuovo database SQL**.
6. Nel **DB Connection String Name** casella, lasciare il valore predefinito *DefaultConnection*.
7. Fare clic sulla freccia che punta a destra nella parte inferiore della finestra.

    La figura seguente mostra le **Crea sito Web** finestra di dialogo con valori di esempio in essa contenuti. L'URL e l'area in cui è stato immesso sarà diverso.

    ![Creare il passaggio sito Web](deploying-to-production/_static/image1.png)

    Verrà visualizzata la **specifica impostazioni database** passaggio.
8. Nel **Name** casella, immettere *ContosoUniversity* oltre a un numero casuale per renderlo univoco, ad esempio *ContosoUniversity123*.
9. Nel **Server** , quindi selezionare **nuovo Server SQL Database**.
10. Immettere un nome e password amministratore.

    Non si immetteranno un nome esistente e una password. Immettere un nuovo nome e una password che si sta definendo ora per l'utilizzo in un secondo momento quando si accede al database.
11. Nel **regione** scegliere la stessa area in cui si è scelto per l'app web.

    Mantenendo il server web e il server di database nella stessa area ti offre le migliori prestazioni e riduce al minimo le spese.
12. Scegliere il segno di spunta nella parte inferiore della casella per indicare che si è finito.

    La figura seguente mostra le **specifica impostazioni database** finestra di dialogo con valori di esempio in essa contenuti. I valori immessi siano diversi.

    ![Passaggio di impostazioni di database del sito Web di New - Create con Creazione guidata Database](deploying-to-production/_static/image2.png)

    Il portale di gestione restituisce alla pagina di siti Web e il **stato** colonna mostra che viene creata l'app web. Dopo un periodo di tempo (in genere meno di un minuto), il **stato** colonna mostra che l'app web è stato creato correttamente. Nella barra di spostamento a sinistra, viene visualizzato accanto al numero di App web avere nel proprio account di **siti Web** icona e il numero di database viene visualizzato accanto al **database SQL** icona.

    ![Pagina di siti Web del portale di gestione sito web creato](deploying-to-production/_static/image3.png)

    Il nome dell'app web sarà diverso da dell'app di esempio nella figura.

## <a name="deploy-the-application-to-staging"></a>Distribuire l'applicazione in gestione temporanea

Ora che è stato creato un'app web e database per l'ambiente di gestione temporanea, è possibile distribuire il progetto a esso.

> [!NOTE]
> Queste istruzioni illustrano come creare un profilo di pubblicazione scaricando una *publishsettings* file, che funziona non solo per Azure ma anche per i provider di hosting di terze parti. La versione più recente SDK di Azure consente inoltre di connettersi direttamente ad Azure da Visual Studio e scegliere da un elenco di App web presenti nel proprio account Azure. In Visual Studio 2013, è possibile accedere ad Azure dal **pubblicazione sul Web** finestra di dialogo o dalle **Esplora Server** finestra. Per altre informazioni, vedere [creare un'app web ASP.NET in servizio App di Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Scaricare il file con estensione publishsettings

1. Fare clic sul nome dell'app web appena creata.

    ![Fare clic sul sito per passare al Dashboard](deploying-to-production/_static/image4.png)
2. Sotto **Quick glance** nel **Dashboard** scheda, fare clic su **Scarica profilo di pubblicazione**.

    ![Profilo di pubblicazione collegamento di download](deploying-to-production/_static/image5.png)

    Questo passaggio consente di scaricare un file che contiene tutte le impostazioni necessarie per distribuire un'applicazione in app web. Come importare questo file in Visual Studio in modo da non dover immettere manualmente queste informazioni.
3. Salvare il *publishsettings* file in una cartella che è possibile accedere da Visual Studio.

    ![salvataggio del file con estensione publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Security - i *publishsettings* file contiene le credenziali (non codificate) usate per amministrare i servizi e le sottoscrizioni di Azure. La procedura consigliata di sicurezza per questo file consiste nell'archiviarlo temporaneamente all'esterno delle directory di origine (ad esempio nella cartella raccolte\documenti) e quindi eliminarlo al termine dell'importazione è stata completata. Un utente malintenzionato che riesce ad accedere per il *publishsettings* file può modificare, creare ed eliminare i servizi di Azure.

### <a name="create-a-publish-profile"></a>Creare un profilo di pubblicazione

1. In Visual Studio, fare doppio clic su progetto in ContosoUniversity **Esplora soluzioni** e selezionare **Publish** dal menu di scelta rapida.

    Il **pubblica sul Web** verrà visualizzata la procedura guidata.
2. Scegliere il **profilo** scheda.
3. Fare clic su **Importa**.
4. Passare al *publishsettings* del file scaricato in precedenza e quindi fare clic su **Open**.

    ![Finestra di dialogo Importa impostazioni di pubblicazione](deploying-to-production/_static/image7.png)
5. Nel **Connection** scheda, fare clic su **convalida connessione** per assicurarsi che le impostazioni siano corrette.

    Quando la connessione è stata convalidata, un segno di spunta verde verrà visualizzato accanto al **convalida connessione** pulsante.

    Per alcuni provider di hosting, quando si fa clic **convalida connessione**, è possibile visualizzare un **Errore certificato** nella finestra di dialogo. Se esegue l'operazione, verificare che il nome del server è quello previsto. Se il nome del server sia corretto, selezionare **salvare questo certificato per le sessioni future di Visual Studio** e fare clic su **Accept**. (Questo errore indica che il provider di hosting ha scelto di evitare le spese per l'acquisto di un certificato SSL per l'URL a cui viene eseguita la distribuzione. Se si preferisce per stabilire una connessione protetta con un certificato valido, contattare il provider di hosting.)
6. Scegliere **Avanti**.

    ![icona della connessione ha esito positivo e sul pulsante Avanti nella scheda connessione](deploying-to-production/_static/image8.png)
7. Nel **le impostazioni** scheda, quindi espandere **Opzioni pubblicazione File**e quindi selezionare **escludere i file dall'App\_cartella dati**.

    Per informazioni sulle altre opzioni sotto **Opzioni pubblicazione File**, vedere la [distribuzione in IIS](deploying-to-iis.md) esercitazione. Lo screenshot che mostra il risultato di questo passaggio e i seguenti passaggi di configurazione di database si trova alla fine dei passaggi di configurazione del database.
8. Sotto **DefaultConnection** nel **database** sezione, configurare la distribuzione di database per il database di appartenenza.
9. 1. Selezionare **aggiornare il database**.

        Il **stringa di connessione remota** casella riportata di seguito **DefaultConnection** compilata con la stringa di connessione dal file con estensione publishsettings. La stringa di connessione include le credenziali di SQL Server, che vengono archiviate in testo normale nel *pubxml* file. Se si preferisce non archiviarle in modo permanente non esiste, è possibile rimuoverli dal profilo di pubblicazione dopo aver distribuito il database e archiviarle in Azure. Per altre informazioni, vedere [come proteggere il database ASP.NET le stringhe di connessione durante la distribuzione in Azure dall'origine](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) sul blog di Scott Hanselman.
      2. Fare clic su **Configura Aggiornamenti database**.
      3. Nel **Configura Aggiornamenti Database** finestra di dialogo, fare clic su **Aggiungi Script SQL**.
      4. Nel **Aggiungi Script SQL** passare alle *aspnet-data-prod.sql* script salvato in precedenza nella cartella della soluzione e quindi fare clic su **Open**.
      5. Chiudi il **Configura Aggiornamenti Database** nella finestra di dialogo.
10. Sotto **SchoolContext** nel **database** sezione, selezionare **Esegui migrazioni Code First (inizia all'avvio dell'applicazione)**.

    Visual Studio visualizza **Esegui migrazioni Code First** invece di **aggiornamento Database** per `DbContext` classi. Se si desidera utilizzare il provider dbDacFx anziché le migrazioni per distribuire un database di cui si accede tramite un `DbContext` classe, vedere [come distribuire un database senza le migrazioni Code First?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) nelle domande frequenti distribuzione Web per Visual Studio e ASP.NET su MSDN.

    Il **impostazioni** scheda avrà ora un aspetto simile al seguente:

    ![Scheda Impostazioni per la gestione temporanea](deploying-to-production/_static/image9.png)
11. Eseguire la procedura seguente per salvare il profilo e rinominarlo *Staging*:

    1. Scegliere il **profilo** scheda e quindi fare clic su **Gestione profili**.
    2. L'importazione creati due nuovi profili, uno per FTP e uno per distribuzione Web. È configurato il profilo di distribuzione Web: rinominare questo profilo per poter *Staging*.

        ![Rinomina profilo di gestione temporanea](deploying-to-production/_static/image10.png)
    3. Chiudi il **modifica dei profili di pubblicazione Web** nella finestra di dialogo.
    4. Chiudi il **pubblica sul Web** procedura guidata.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Configurare una trasformazione di profilo di pubblicazione per l'indicatore di ambiente

> [!NOTE]
> Questa sezione illustra come configurare una trasformazione Web. config per l'indicatore di ambiente. Poiché l'indicatore è nel `<appSettings>` elemento, si dispone di un'altra alternativa per specificare la trasformazione quando si esegue la distribuzione servizio App di Azure. Per altre informazioni, vedere [Web. config che specifica le impostazioni in Azure](web-config-transformations.md#watransforms).


1. Nelle **Esplora soluzioni**, espandere **delle proprietà**, quindi espandere **PublishProfiles**.
2. Fare doppio clic su *Staging.pubxml*, quindi fare clic su **aggiungere Config trasformare**.

    ![Aggiungi trasformazione di configurazione per la gestione temporanea](deploying-to-production/_static/image11.png)

    Visual Studio crea il *staging* file di trasformazione e lo apre.
3. Nel *staging* trasforma il file, inserire il codice seguente immediatamente dopo l'apertura `configuration` tag.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Quando si usa il profilo di pubblicazione di gestione temporanea, questa trasformazione imposta l'indicatore di ambiente da "Prod". Nell'app web distribuita, sarà possibile vedere qualsiasi suffisso, ad esempio "(Dev)" o "(Test)" dopo l'intestazione H1 "Contoso University".
4. Fare doppio clic il *staging* del file e fare clic su **anteprima trasformare** per assicurarsi che la trasformazione si codificano produce le modifiche previste.

    Il **anteprima Web. config** finestra Mostra il risultato dell'applicazione sia la *Release* Trasforma e il *staging* Trasforma.

### <a name="prevent-public-use-of-the-test-app"></a>Impedire l'uso pubblico dell'app di test

Una considerazione importante per le app di gestione temporanea è che sarà in tempo reale su Internet, ma si preferisce non al pubblico per usarlo. Per ridurre al minimo la probabilità che gli utenti individuare e verranno utilizzato, è possibile usare uno o più dei metodi seguenti:

- Impostare le regole del firewall che consentono l'accesso all'app di gestione temporanea solo da indirizzi IP utilizzabili per testare gestione temporanea.
- Usare un URL offuscato impossibili da indovinare.
- Creare un *robots* file per garantire che i motori di ricerca verranno non una ricerca per indicizzazione collegamenti di app e report di test ad esso nei risultati della ricerca.

Il primo di questi metodi è il più efficace, ma non è illustrato in questa esercitazione perché sarebbe necessario distribuire a un servizio Cloud di Azure invece di servizio App di Azure. Per altre informazioni sui servizi Cloud e le restrizioni di IP in Azure, vedere [calcolo che ospita le opzioni fornite da Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) e [blocco di indirizzi IP specifici di accedere a un ruolo Web](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Se si distribuisce in un provider di hosting di terze parti, contattare il provider per scoprire come implementare restrizioni IP.

Per questa esercitazione si creerà una *robots* file.

1. Nelle **Esplora soluzioni**, fare clic sul progetto ContosoUniversity e fare clic su **Aggiungi nuovo elemento**.
2. Creare una nuova **File di testo** denominate *robots*e inserirvi il testo seguente:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    Il `User-agent` riga indica che le regole nel file di si applicano a tutti i ricerca web dai crawler del motore (robot), i motori di ricerca e `Disallow` righe specifica che non devono essere una ricerca per indicizzazione le pagine del sito.

    Si desidera motori di ricerca catalogo app di produzione, pertanto è necessario escludere questo file dalla distribuzione di produzione. A scopo, si configurerà un'impostazione in produzione il profilo di pubblicazione durante la creazione.

### <a name="deploy-to-staging"></a>Distribuire in gestione temporanea

1. Aprire il **pubblica sul Web** procedura guidata facendo clic sul progetto Contoso University e facendo clic su **Publish**.
2. Assicurarsi che il **Staging** profilo è selezionato.
3. Fare clic su **Pubblica**.

    Il **Output** finestra Mostra le azioni di distribuzione sono state eseguite e segnalato il corretto completamento della distribuzione. Il browser predefinito verrà aperto automaticamente l'URL dell'app web distribuita.

## <a name="test-in-the-staging-environment"></a>Test nell'ambiente di staging

Si noti che l'indicatore di ambiente è assente (nessun "(test)" o "(Dev)" dopo l'intestazione H1, che indica che il *Web. config* trasformazione per l'indicatore di ambiente ha avuto esito positivo.

![Home page gestione temporanea](deploying-to-production/_static/image12.png)

Eseguire la **studenti** pagina per verificare che il database distribuito non disponga di alcun studenti.

Eseguire la **instructors (insegnanti)** pagina per verificare che Code First effettuato il seeding del database con dati instructor:

Selezionare **aggiungere gli studenti** dal **studenti** dal menu Aggiungi uno studente e quindi visualizzare il nuovo studente nella **studenti** pagina per verificare che è possibile scrivere nel database .

Dal **corsi** pagina, fare clic su **crediti Update**. Il **aggiornamento crediti** pagina richiede autorizzazioni di amministratore, in modo che il **Accedi** viene visualizzata la pagina. Immettere le credenziali dell'account amministratore creato precedenti ("admin" e "prodpwd"). Il **crediti Update** pagina viene visualizzata, verifica che l'account amministratore creato nell'esercitazione precedente è stato distribuito correttamente nell'ambiente di test.

Richiedere un URL non valido per provocare un errore che ELMAH registrerà e quindi richiedono il report di errore ELMAH. Se si distribuisce in un provider di hosting di terze parti, è probabile che il report è vuoto per lo stesso motivo che era vuoto nell'esercitazione precedente. È necessario usare gli strumenti di gestione di account del provider di hosting per configurare le autorizzazioni per abilitare ELMAH scrivere nella cartella log.

L'applicazione creata è in esecuzione nel cloud in un'app web che è esattamente come quello che si userà per la produzione. Poiché tutto funziona correttamente, il passaggio successivo consiste nel distribuire nell'ambiente di produzione.

## <a name="deploy-to-production"></a>Distribuire nell'ambiente di produzione

Il processo per la creazione di un'app web di produzione e la distribuzione nell'ambiente di produzione è identico a quello di gestione temporanea, ad eccezione del fatto che è necessario escludere i *robots* dalla distribuzione. A tale scopo si modificherà il file di profilo di pubblicazione.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Creare l'ambiente di produzione e produzione profilo di pubblicazione

1. Creare il database e l'app web di produzione in Azure, seguire la stessa procedura utilizzata per la gestione temporanea.

    Quando si crea il database, è possibile scegliere di inserirlo nello stesso server creato in precedenza, o creare un nuovo server.
2. Scaricare il *publishsettings* file.
3. Creare il profilo di pubblicazione mediante l'importazione di produzione *publishsettings* file, seguire la stessa procedura utilizzata per la gestione temporanea.

    Non dimenticare di configurare lo script di distribuzione dei dati sotto **DefaultConnection** nel **database** sezione del **impostazioni** scheda.
4. Il profilo di pubblicazione per rinominare *produzione*.
5. Configurare una trasformazione di profilo di pubblicazione per l'indicatore di ambiente, seguire la stessa procedura utilizzata per la gestione temporanea...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Modificare il file con estensione pubxml per escludere robots. txt

I file sono denominati profilo di pubblicazione &lt;profilename&gt;*pubxml* e si trovano nel *PublishProfiles* cartella. Il *PublishProfiles* cartella si trova sotto il *proprietà* cartella in un'applicazione web c# del progetto, sotto il *progetto personale* cartella in un progetto di applicazione web di Visual Basic o nel *App\_dati* cartella in un progetto di app web. Ciascuna *pubxml* file contiene le impostazioni che si applicano a un profilo di pubblicazione. In questi file vengono archiviati i valori che immessi nella procedura guidata Pubblica sito Web, ed è possibile modificare in modo da creare o modificare le impostazioni che non sono stati resi disponibili nell'interfaccia utente Visual Studio.

Per impostazione predefinita *pubxml* file sono inclusi nel progetto quando si crea un profilo di pubblicazione, ma è possibile escluderli dal progetto e verrà comunque usare Visual Studio. Visual Studio cerca nella *PublishProfiles* cartella *pubxml* file, indipendentemente dal fatto che vengono inclusi nel progetto.

Per ognuno *pubxml* file è presente un *. pubxml* file. Il *. pubxml* file contiene la password crittografata se è stata selezionata la **Salva password** opzione e per impostazione predefinita esso viene escluso dal progetto.

Oggetto *pubxml* che contiene le impostazioni che si riferiscono a un profilo di pubblicazione specifico. Se si desidera configurare le impostazioni che si applicano a tutti i profili, è possibile creare un *. WPP* file. Il processo di compilazione come importare questi file nei *csproj* oppure *vbproj* file di progetto, in modo che la maggior parte delle impostazioni che è possibile configurare nel file di progetto possono essere configurate in questi file. Per altre informazioni sulle *pubxml* i file e *. WPP* i file, vedere [come: modificare le impostazioni di distribuzione nei file di profilo di pubblicazione (con estensione pubxml) e il. WPP File in Visual Studio Progetti Web](https://msdn.microsoft.com/library/ff398069.aspx).

1. Nelle **Esplora soluzioni**, espandere **delle proprietà** ed espandere **PublishProfiles**.
2. Fare doppio clic su *Production.pubxml* e fare clic su **Open**.

    ![Aprire il file con estensione pubxml](deploying-to-production/_static/image13.png)
3. Fare doppio clic su *Production.pubxml* e fare clic su **Open**.
4. Aggiungere le righe seguenti immediatamente prima della chiusura `PropertyGroup` elemento:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Il file con estensione pubxml è ora simile al seguente:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Per altre informazioni su come escludere file e cartelle, vedere [è possibile escludere cartelle o file specifici dalla distribuzione?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) nel **domande frequenti sulla distribuzione Web per Visual Studio e ASP.NET** su MSDN.

### <a name="deploy-to-production"></a>Distribuire nell'ambiente di produzione

1. Aprire il **pubblica sul Web** procedura guidata verificare che il **produzione** profilo di pubblicazione sia selezionata e quindi fare clic su **Avvia anteprima** nel **anteprima**pressione di tab per verificare che il *robots* file non verrà copiati all'app di produzione.

    ![Anteprima del file da pubblicare nell'ambiente di produzione](deploying-to-production/_static/image14.png)

    Esaminare l'elenco dei file che verranno copiati. Si noterà che tutti i *cs* i file, inclusi *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, e  *Master.Designer.cs* i file sono stati omessi. Tutto il codice compilato nel *contosouniversity. dll* e *ContosUniversity.pdb* i file che è possibile trovare nel *bin* cartella. Poiché solo il *. dll* è necessaria per eseguire l'applicazione ed è stato specificato in precedenza che devono essere distribuiti solo i file necessari per eseguire l'applicazione, no *cs* file sono stati copiati nella destinazione ambiente. Il *obj* cartella e il *Contosouniversity* e *. csproj* file sono stati omessi per lo stesso motivo.

    Fare clic su **pubblica** distribuire nell'ambiente di produzione.
2. Test nell'ambiente di produzione, seguire la stessa procedura usata per la gestione temporanea.

    Tutto ciò che è identica alla gestione temporanea tranne l'URL e l'assenza del *robots* file.

## <a name="summary"></a>Riepilogo

Abbiamo correttamente distribuito e testato l'app web ed è disponibile pubblicamente su Internet.

![Home page produzione](deploying-to-production/_static/image15.png)

Nella prossima esercitazione, è possibile aggiornare il codice dell'applicazione e distribuire la modifica agli ambienti di test, staging e produzione.

> [!NOTE]
> Mentre l'applicazione è in uso nell'ambiente di produzione si deve implementare un piano di ripristino. Vale a dire, dovrebbe essere periodicamente il backup dei database dell'app di produzione in un percorso di archiviazione protetta e si devono mantenere varie generazioni di tali backup. Quando si aggiorna il database, è necessario eseguire una copia di backup da immediatamente prima della modifica. Quindi, se si commette un errore e non individua solo dopo aver distribuito nell'ambiente di produzione, è comunque sarà in grado di ripristinare il database allo stato che precedente danneggiamento. Per altre informazioni, vedere [Azure SQL Database Backup and Restore](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> In questa esercitazione di SQL Server edition che viene eseguita la distribuzione è il Database SQL di Azure. Durante il processo di distribuzione è simile ad altre edizioni di SQL Server, un'applicazione di produzione reale potrebbe richiedere un codice speciale per il Database SQL di Azure in alcuni scenari. Per altre informazioni, vedere [utilizzo di Database SQL di Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) e [scelta tra SQL Server e Database SQL di Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Precedente](setting-folder-permissions.md)
> [Successivo](deploying-a-code-update.md)
