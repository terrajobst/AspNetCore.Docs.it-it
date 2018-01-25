---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatizzare tutti gli elementi (creazione di applicazioni Cloud del mondo reale con Azure) | Documenti Microsoft
author: MikeWasson
description: "Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: aa8bb895ed6eaa0ef4c5752f475ea7c911544ef2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatizzare tutti gli elementi (creazione di applicazioni Cloud del mondo reale con Azure)
====================
da [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [E-book di Download](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni introduttive per l'e-book, vedere [primo capitolo](introduction.md).


In realtà i primi tre modelli che verrà esaminato si applicano a qualsiasi progetto di sviluppo software, ma soprattutto per i progetti di cloud. Questo modello è sull'automazione di attività di sviluppo. È un argomento importante perché i processi manuali sono lente e soggetta a errori; automazione come molti di essi come possibili consente di impostare un flusso di lavoro veloce, affidabile e agile. È importante in modo univoco per lo sviluppo cloud perché è possibile automatizzare facilmente molte attività che sono difficili o impossibili da automatizzare in un ambiente locale. Ad esempio, è possibile impostare intero test ambienti, tra cui macchine virtuali di back-end e nuovo server web, database, blob (archiviazione di file) di archiviazione, code e così via.

## <a name="devops-workflow"></a>Flusso di lavoro DevOps

Sempre più spesso si sentono il termine "DevOps". Il termine sviluppato da un riconoscimento che è necessario integrare le attività di sviluppo e le operazioni per lo sviluppo di software in modo efficiente. Il tipo di flusso di lavoro che si desidera abilitare è uno in cui è possibile sviluppare un'app, distribuirla, informazioni da parte dell'utilizzo di produzione di esso, modificarlo in risposta a cui si è appreso e ripetere il ciclo in modo rapido e affidabile.

Alcuni team di sviluppo cloud riuscita distribuire più volte al giorno per un ambiente attivo. Il team di Azure consentono di distribuire un importante aggiornamento ogni 2-3 mesi, ma ora versioni aggiornamenti minori ogni 2-3 giorni e i principali rilascia ogni 2-3 settimane. Recupero di tale frequenza è estremamente utile per rispondere ai commenti dei clienti.

A tale scopo, è necessario abilitare un ciclo di sviluppo e distribuzione ripetibile, affidabile e prevedibile, e con durata del ciclo bassa.

![Flusso di lavoro DevOps](automate-everything/_static/image1.png)

Il periodo di tempo tra quando si ha un'idea per una funzionalità e quando i clienti sono utilizzarlo come commenti e suggerimenti in altre parole, deve essere il più breve possibile. Automatizzare le prime tre modelli, ovvero tutto, il controllo del codice sorgente, e di integrazione continua e consegna sono tutti sulle procedure consigliate che è consigliabile per consentire questo tipo di processo.

## <a name="azure-management-scripts"></a>Script di gestione di Azure

Nel [Introduzione a questo e-book](introduction.md), si è visto la console basata sul web, il portale di gestione di Azure. Il portale di gestione consente di monitorare e gestire tutte le risorse che è stato distribuito in Azure. È un modo semplice di creare ed eliminare servizi, ad esempio le applicazioni web e macchine virtuali, configurare i servizi, monitorare l'operazione del servizio e così via. È un ottimo strumento, ma rappresenta una un processo manuale. Se si intende sviluppare un'applicazione di produzione di qualsiasi dimensione, e in particolare in un ambiente di team, è consigliabile accedere al portale dell'interfaccia utente per ulteriori informazioni ed esplorare Azure e quindi automatizzare i processi che si saranno eseguendo ripetutamente.

Quasi tutto ciò che è possibile eseguire manualmente nel portale di gestione o da Visual Studio può essere eseguita anche chiamando l'API di gestione di REST. È possibile scrivere gli script utilizzando [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), oppure è possibile utilizzare un framework open source, ad esempio [Chef](http://www.opscode.com/chef/) o [Puppet](http://puppetlabs.com/puppet/what-is-puppet). È inoltre possibile utilizzare lo strumento da riga di comando Bash in un ambiente Mac o Linux. Azure offre API per i diversi ambienti di scripting e ha un [API di gestione .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) nel caso in cui si desidera scrivere codice invece di script.

Per l'app Correggi abbiamo creato alcuni script di Windows PowerShell che consentono di automatizzare i processi di creazione di un ambiente di test e la distribuzione del progetto a un ambiente e verrà esaminato il contenuto di tali script.

## <a name="environment-creation-script"></a>Script di creazione ambiente

Il primo script verrà esaminato denominato *New AzureWebsiteEnv.ps1*. Viene creato un ambiente di Azure che è possibile distribuire il Correggi dell'app per il test. Di seguito sono elencate le attività principali che lo script esegue:

- Creare un'app web.
- Creare un account di archiviazione. (Obbligatorio per BLOB e code, come si vedrà nei capitoli successivi).
- Creare un server di Database SQL e due database: un database dell'applicazione e un database di appartenenza.
- Archiviare le impostazioni in Azure che l'app verrà utilizzato per accedere ai database e account di archiviazione.
- Creare file di impostazioni che verranno utilizzati per automatizzare la distribuzione.

### <a name="run-the-script"></a>Eseguire lo script


> [!NOTE]
> Questa parte del capitolo vengono illustrati esempi di script e i comandi immessi per eseguirle. Questo una demo e non offre tutto ciò che occorre conoscere per eseguire gli script. Per istruzioni dettagliate procedure-to--it, vedere [appendice: la correzione, applicazione di esempio](the-fix-it-sample-application.md#deploybase).


Per eseguire uno script di PowerShell che gestisce i servizi di Azure è necessario installare la console di Azure PowerShell e configurarlo in modo da funzionare con la sottoscrizione di Azure. Una volta impostate, è possibile eseguire Correggi ambiente script per la creazione con un comando simile al seguente:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

Il `Name` parametro specifica il nome da utilizzare quando si creano gli account di archiviazione e del database e `SqlDatabasePassword` parametro specifica la password per l'account amministratore che verrà creato per il Database SQL. Esistono altri parametri che è possibile utilizzare che verrà esaminato più avanti.

![Finestra di PowerShell](automate-everything/_static/image2.png)

Termine dello script nel portale di gestione è possibile vedere cosa è stata creata. Sono disponibili due database:

![Database](automate-everything/_static/image3.png)

Un account di archiviazione:

![Account di archiviazione](automate-everything/_static/image4.png)

E un'app web:

![Sito Web](automate-everything/_static/image5.png)

Nel **configura** scheda per l'app web, è possibile vedere che contiene le impostazioni dell'account di archiviazione e le stringhe di connessione di database SQL impostato per la correzione è app.

![appSettings e connectionStrings](automate-everything/_static/image6.png)

Il *automazione* cartella ora contiene inoltre un  *&lt;websitename&gt;pubxml* file. Questo file contiene impostazioni che MSBuild verranno usate per distribuire l'applicazione all'ambiente Azure, che è stato appena creato. Ad esempio:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Come si può notare, lo script ha creato un ambiente di test completo e l'intero processo viene eseguita in circa 90 secondi.

Se qualcun altro membro del team decide di creare un ambiente di test, eseguire solo script. Non solo è veloce, ma è anche possibile essere certi che si sta usando un ambiente identico a quello in uso. Non è stato possibile piuttosto certi di che, se tutti gli utenti è stato configurazione manualmente tramite il portale di gestione dell'interfaccia utente.

### <a name="a-look-at-the-scripts"></a>Esaminare gli script

Esistono effettivamente tre script che questa operazione. Chiamare uno dalla riga di comando e usa automaticamente le altre due per svolgere le attività:

- *Nuovo AzureWebSiteEnv.ps1* è riportato lo script principale.

    - *Nuovo AzureStorage.ps1* crea l'account di archiviazione.
    - *Nuovo AzureSql.ps1* i database creati.

### <a name="parameters-in-the-main-script"></a>Parametri nello script principale

Lo script principale, *New AzureWebSiteEnv.ps1*, definisce vari parametri:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Sono necessari due parametri:

- Il nome dell'applicazione web che consente di creare lo script. (Viene utilizzato anche per l'URL: `<name>.azurewebsites.net`.)
- La password per il nuovo utente amministratore del server di database che consente di creare lo script.

Parametri facoltativi consentono di specificare la posizione del data center (valore predefinito è "Stati Uniti occidentali"), nome amministratore del server database (impostazione predefinita "dbuser") e una regola del firewall per il server di database.

### <a name="create-the-web-app"></a>Creare l'app web

La prima cosa lo script è creare l'app web tramite la chiamata di `New-AzureWebsite` cmdlet, passando a esso l'app web valori di parametro di nome e percorso:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Creare l'account di archiviazione

Quindi lo script principale viene eseguito il *New AzureStorage.ps1* uno script, specificando "*&lt;websitename&gt;*archiviazione" per il nome di account di archiviazione, e la stessa posizione data center come l'app web.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*Nuovo AzureStorage.ps1* chiamate di `New-AzureStorageAccount` cmdlet per creare l'account di archiviazione e restituisce l'account del nome e l'accesso ai valori di chiave. L'applicazione sono necessari questi valori per accedere ai BLOB e code nell'account di archiviazione.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Si potrebbe non sempre desidera creare un nuovo account di archiviazione. è possibile migliorare lo script mediante l'aggiunta di un parametro che indica se lo si desidera l'uso di un account di archiviazione esistente.

### <a name="create-the-databases"></a>Creare i database

Lo script principale viene quindi eseguito lo script di creazione del database, *New AzureSql.ps1*, dopo l'impostazione di database predefinito e i nomi delle regole del firewall:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Script di creazione del database recupera l'indirizzo IP del computer di sviluppo e imposta una regola del firewall in modo che il computer di sviluppo possa connettersi e gestire il server. Script di creazione del database viene quindi inviato attraverso diversi passaggi per impostare i database:

- Crea il server utilizzando il `New-AzureSqlDatabaseServer` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Crea regole del firewall per attivare il computer di sviluppo per gestire il server e abilitare l'app web per connettersi ad esso. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Crea un contesto di database che include il nome del server e le credenziali, usando il `New-AzureSqlDatabaseServerContext` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText`è una funzione nello script che chiama il `ConvertTo-SecureString` cmdlet per crittografare la password e restituisce un `PSCredential` oggetto dello stesso tipo che il `Get-Credential` cmdlet restituisce.
- Crea il database dell'applicazione e il database delle appartenenze utilizzando il `New-AzureSqlDatabase` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Chiama una funzione definita localmente di tocreates una stringa di connessione per ogni database. L'applicazione utilizzerà queste stringhe di connessione per accedere ai database. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString è una funzione definita nello script che crea la stringa di connessione dai valori dei parametri forniti.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Restituisce una tabella hash con il nome del server di database e le stringhe di connessione.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

L'app Correggi utilizza appartenenze separata e database dell'applicazione. È inoltre possibile inserire i dati di appartenenza sia le applicazioni in un singolo database.

### <a name="store-app-settings-and-connection-strings"></a>Archiviare le stringhe di connessione e le impostazioni di app

Azure offre una funzionalità che consente di archiviare le impostazioni e le stringhe di connessione automaticamente l'override di ciò che viene restituito all'applicazione durante il tentativo di leggere il `appSettings` o `connectionStrings` raccolte nel file Web. config. Si tratta di un'alternativa all'applicazione [trasformazioni di Web. config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) quando si distribuisce. Per ulteriori informazioni, vedere [memorizzare i dati riservati in Azure](source-control.md#appsettings) più avanti in questo e-book.

Lo script di creazione ambiente vengono archiviati in Azure di tutti i `appSettings` e `connectionStrings` valori che l'applicazione deve accedere l'account di archiviazione e il database durante l'esecuzione in Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) è un framework di telemetria indicanti nel [monitoraggio e telemetria](monitoring-and-telemetry.md) capitolo. Inoltre, lo script di creazione ambiente riavvia l'app web per verificare che preleva le impostazioni di New Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Preparazione per la distribuzione

Al termine del processo, lo script di creazione ambiente chiama due funzioni per creare i file che verranno utilizzati dallo script di distribuzione.

Una di queste funzioni crea un profilo di pubblicazione *(&lt;websitename&gt;pubxml* file). Il codice chiama l'API REST di Azure per ottenere le impostazioni di pubblicazione e Salva le informazioni contenute in un *publishsettings* file. Quindi, Usa le informazioni dal file insieme a un file di modello (*pubxml.template*) per creare il *pubxml* file che contiene il profilo di pubblicazione. Questo processo in due passaggi simula operazioni in Visual Studio: scaricare un *publishsettings* file e di importazione che per creare un profilo di pubblicazione.

L'altra funzione utilizza un altro file di modello (sito Web-environment.template) per creare un *sito Web environment.xml* file che contiene lo script di distribuzione verrà utilizzato insieme a impostazioni di *pubxml*file.

### <a name="troubleshooting-and-error-handling"></a>Risoluzione dei problemi e la gestione degli errori

Gli script sono come i programmi: è possibile non riesca e quando non lo si desidera sapere come è possibile sull'errore e ha generato. Per questo motivo, lo script di creazione ambiente cambia il valore di `VerbosePreference` variabile dal `SilentlyContinue` per `Continue` in modo che tutti i messaggi dettagliati vengono visualizzati. Modifica inoltre il valore della `ErrorActionPreference` variabile dal `Continue` a `Stop`, in modo che lo script viene interrotto anche quando viene rilevato errori non fatali:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Prima che qualsiasi funziona, lo script memorizza l'ora di inizio in modo da poter calcolare il tempo trascorso dopo averlo:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Dopo aver completato il proprio lavoro, lo script viene visualizzato il tempo trascorso:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

E per ogni operazione della chiave lo script scrive i messaggi dettagliati, ad esempio:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Script di distribuzione

Cosa il *New AzureWebsiteEnv.ps1* non per la creazione dell'ambiente, gli script di *pubblica AzureWebsite.ps1* script esegue per la distribuzione di applicazioni.

Lo script di distribuzione Ottiene il nome dell'app web dal *sito Web environment.xml* file creati dallo script di creazione ambiente.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Ottiene la password dell'utente di distribuzione dal *publishsettings* file:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Esegue il [MSBuild](http://msbuildbook.com/) comando che crea e distribuisce il progetto:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

E se è stato specificato il `Launch` parametro della riga di comando, viene chiamato il `Show-AzureWebsite` cmdlet per aprire il browser predefinito per l'URL del sito Web.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

È possibile eseguire lo script di distribuzione con un comando simile al seguente:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

E al termine, si apre il browser con il sito in esecuzione nel cloud di `<websitename>.azurewebsites.net` URL.

![Correggerlo app distribuita in Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Riepilogo

Con questi script è possibile essere certi che gli stessi passaggi verranno eseguiti sempre nello stesso ordine con le stesse opzioni. Ciò garantisce che ogni sviluppatore del team non non per un elemento o un elemento sottovalutato o distribuire un elemento personalizzato nel proprio computer che non effettivamente funzionano allo stesso modo nell'ambiente di un altro membro del team o nell'ambiente di produzione.

In modo analogo, è possibile automatizzare Azure più funzioni di gestione che è possibile eseguire nel portale di gestione, tramite l'API REST, gli script di Windows PowerShell, un linguaggio .NET, API o un'utilità Bash che è possibile eseguire in Linux o Mac.

Nel [capitolo successivo](source-control.md) verrà esaminare il codice sorgente e spiegare perché è importante includere gli script nel repository del codice sorgente.

## <a name="resources"></a>Risorse

- [Installare e configurare Windows PowerShell per Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Viene illustrato come installare i cmdlet PowerShell di Azure e come installare il certificato che è necessario nel computer in uso per la gestione di Azure dell'account. Questo è un ottimo punto di partenza poiché dispone anche di collegamenti a risorse per l'apprendimento di PowerShell.
- [Script Center di Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portale WindowsAzure.com alle risorse per lo sviluppo di script per la gestione di servizi di Azure, con collegamenti alle informazioni esercitazioni introduttive, codice di origine e la documentazione di riferimento cmdlet e script di esempio
- [Weekend Scripter: Introduzione a Azure e PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). In un blog dedicato a Windows PowerShell, questo post fornisce un'ottima introduzione all'utilizzo di PowerShell per le funzioni di gestione di Azure.
- [Installare e configurare l'interfaccia della riga di comando multipiattaforma Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Guida introduttiva di esercitazione per un framework di scripting Azure che funziona sul Mac e Linux, nonché Windows sistemi.
- [Sezione strumenti da riga di comando dell'argomento scaricare Azure SDK e strumenti](https://azure.microsoft.com/downloads/). Pagina del portale per la documentazione e download correlati agli strumenti da riga di comando per Azure.
- [Automazione di tutti gli elementi e le raccolte di gestione di Azure .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman introduce l'API di gestione .NET per Azure.
- [Utilizzo di script di Windows PowerShell per la pubblicazione in ambienti di Test e Dev](https://msdn.microsoft.com/library/azure/dn642480.aspx). Documentazione MSDN che spiega come usare script di Visual Studio genera automaticamente per i progetti web di pubblicazione.
- [PowerShell Tools per Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Estensione di Visual Studio che aggiunge il supporto lingua per Windows PowerShell in Visual Studio.

>[!div class="step-by-step"]
[Precedente](introduction.md)
[Successivo](source-control.md)
