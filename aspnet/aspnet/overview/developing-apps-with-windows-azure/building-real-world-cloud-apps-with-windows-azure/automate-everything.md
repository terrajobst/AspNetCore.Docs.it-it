---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatizzare tutto (compilazione di App per Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 87b697847845ab88943e7a659ccd9487b9408d5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839635"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatizzare tutto (compilazione di App per Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per un'introduzione all'e-book, vedere [capitolo prima](introduction.md).


I primi tre modelli che vengono esaminati effettivamente si applicano a qualsiasi progetto di sviluppo software, ma soprattutto per i progetti di cloud. Questo modello è su come automatizzare le attività di sviluppo. È un tema importante perché i processi manuali sono lenta e soggetta a errori; automatizzare la maggior parte come possibili consente di impostare un flusso di lavoro veloce, affidabili e flessibili. È importante in modo univoco per lo sviluppo cloud perché è possibile automatizzare molte attività che sono difficili o impossibili da automatizzare in un ambiente locale. Ad esempio, è possibile impostare intero test degli ambienti inclusi nuovo server web e macchine virtuali back-end, database, blob di archiviazione (archiviazione di file), le code e così via.

## <a name="devops-workflow"></a>Flusso di lavoro DevOps

Sempre più spesso si sente il termine "DevOps". Il termine sviluppato da un riconoscimento che è necessario integrare le attività di sviluppo e operazioni per sviluppare software in modo efficiente. Il tipo di flusso di lavoro che si desidera abilitare è uno in cui è possibile sviluppare un'app, distribuirla, impara dagli scopi di produzione di esso, modificarlo in risposta a quello che hai appreso e ripetere il ciclo rapido e affidabile.

Alcuni team di sviluppo cloud di successo distribuire più volte al giorno per un ambiente reale. Il team di Azure usato per distribuire un grande aggiornarlo ogni mesi 2 e 3, ma ora aggiornamenti secondari rilasciati ogni 2-3 giorni e principali versioni ogni 2-3 settimane. Introduzione in tale frequenza è estremamente utile per rispondere ai commenti dei clienti.

Per farlo, è necessario abilitare un ciclo di sviluppo e distribuzione ripetibile, affidabili e prevedibili, e con durata del ciclo bassa.

![Flusso di lavoro DevOps](automate-everything/_static/image1.png)

Il periodo di tempo tra quando si ha un'idea per una funzione e quando i clienti sono utilizzarlo e inviare commenti e suggerimenti in altre parole, deve essere più corte possibili. Primi tre modelli, ovvero automatizzare tutto, controllo del codice sorgente e integrazione continua e recapito, sono indispensabili per le procedure consigliate che è consigliabile per consentire questo tipo di processo.

## <a name="azure-management-scripts"></a>Script di gestione di Azure

Nel [Introduzione a questo e-book](introduction.md), si è visto la console basata sul web, il portale di gestione di Azure. Il portale di gestione consente di monitorare e gestire tutte le risorse che è stato distribuito in Azure. È un modo semplice per creare ed eliminare servizi, ad esempio App web e macchine virtuali, configurare i servizi, monitorare l'operazione del servizio e così via. È uno strumento molto utile, ma usarlo è un processo manuale. Se si intende sviluppare un'applicazione di produzione di qualsiasi dimensione e in particolare in un ambiente di team, è consigliabile che passano attraverso il portale dell'interfaccia utente per informazioni su ed esplorare Azure e quindi automatizzare i processi che procedura ripetutamente.

Quasi tutto ciò che è possibile eseguire manualmente nel portale di gestione o da Visual Studio è anche possibile chiamando l'API REST di gestione. È possibile scrivere script usando [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), oppure è possibile usare un framework open source, ad esempio [Chef](http://www.opscode.com/chef/) oppure [Puppet](http://puppetlabs.com/puppet/what-is-puppet). È anche possibile usare lo strumento da riga di comando di Bash in un ambiente Linux o Mac. Azure offre API di script per tutti questi ambienti diversi e ha un [API di gestione .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) nel caso in cui si vuole scrivere codice invece dello script.

Per l'app Fix It abbiamo creato alcuni script di Windows PowerShell che consentono di automatizzare i processi di creazione di un ambiente di test e la distribuzione del progetto a quell'ambiente e verranno esaminati alcuni dei contenuti di tali script.

## <a name="environment-creation-script"></a>Script di creazione ambiente

Lo script prima verrà esaminato è denominato *New-AzureWebsiteEnv.ps1*. Crea un ambiente di Azure che è possibile distribuire il Fix It app a per il test. Di seguito sono elencate le attività principali che esegue questo script:

- Creare un'app web.
- Creare un account di archiviazione. (Obbligatorio per i BLOB e code, come si vedrà nei capitoli successivi).
- Creare un Database di SQL server e due database: un database dell'applicazione e un database di appartenenza.
- Store le impostazioni di Azure che l'app userà per accedere ai database e account di archiviazione.
- Creare i file di impostazioni che verranno usati per automatizzare la distribuzione.

### <a name="run-the-script"></a>Eseguire lo script


> [!NOTE]
> Questa parte del capitolo vengono illustrati esempi di script e i comandi immessi per eseguirle. Questa demo e non offre tutto quello che devi sapere per poter eseguire gli script. Per istruzioni dettagliate how-to--it, vedere [appendice: la correzione, applicazione di esempio](the-fix-it-sample-application.md#deploybase).


Per eseguire uno script di PowerShell che gestisce i servizi di Azure che è necessario installare la console di PowerShell di Azure e configurarlo in modo da funzionare con la sottoscrizione di Azure. Una volta che eseguita la configurazione, è possibile eseguire Fix It ambiente script per la creazione con un comando simile alla seguente:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

Il `Name` parametro specifica il nome da usare quando si crea l'account di archiviazione e database e il `SqlDatabasePassword` parametro specifica la password per l'account amministratore che verrà creato per il Database SQL. Esistono altri parametri che è possibile usare che esamineremo in seguito.

![Finestra di PowerShell](automate-everything/_static/image2.png)

Al termine dello script nel portale di gestione è possibile visualizzare ciò che è stato creato. Sono disponibili due database:

![Database](automate-everything/_static/image3.png)

Un account di archiviazione:

![Account di archiviazione](automate-everything/_static/image4.png)

E un'app web:

![Sito Web](automate-everything/_static/image5.png)

Nel **configura** scheda per l'app web, noterete che include le impostazioni dell'account di archiviazione e le stringhe di connessione di database SQL configurato per la correzione è app.

![connectionStrings e appSettings](automate-everything/_static/image6.png)

Il *Automation* cartella ora contiene anche una  *&lt;websitename&gt;pubxml* file. Questo file vengono archiviate le impostazioni che MSBuild userà per distribuire l'applicazione all'ambiente di Azure che è stato appena creato. Ad esempio:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Come si può notare, lo script ha creato un ambiente di test completa e l'intero processo viene eseguita in circa 90 secondi.

Se qualcuno del team deve creare un ambiente di test, possono solo eseguire lo script. Non solo è veloce, ma anche possono essere certi che si sta usando un ambiente identico a quello in uso. Non è stato possibile piuttosto certi di che, se tutti gli utenti è stata altre impostazioni manualmente tramite il portale di gestione dell'interfaccia utente.

### <a name="a-look-at-the-scripts"></a>Esaminare gli script

Esistono effettivamente tre script che operano. Si chiama uno dalla riga di comando e usa automaticamente gli altri due per eseguire tutte o alcune delle attività:

- *Nuovo AzureWebSiteEnv.ps1* è riportato lo script principale.

    - *Nuovo AzureStorage.ps1* crea l'account di archiviazione.
    - *Nuovo AzureSql.ps1* i database creati.

### <a name="parameters-in-the-main-script"></a>Parametri dello script principale

Lo script principale *New-AzureWebSiteEnv.ps1*, definisce diversi parametri:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Sono necessari due parametri:

- Il nome dell'app web che consente di creare lo script. (Ciò viene usato anche per l'URL: `<name>.azurewebsites.net`.)
- La password per il nuovo utente amministratore del server di database create dallo script.

I parametri facoltativi consentono di specificare la posizione del data center (impostazione predefinita è "West US"), nome amministratore del server del database (impostazione predefinita è "dbuser") e una regola del firewall per il server di database.

### <a name="create-the-web-app"></a>Creare l'app web

La prima operazione esegue lo script è creare l'app web chiamando il `New-AzureWebsite` cmdlet, passando ad esso nell'app web i valori di parametro di nome e il percorso:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Creare l'account di archiviazione

Quindi lo script principale viene eseguito il <em>New-AzureStorage.ps1</em> uno script, specificando "<em>&lt;websitename&gt;</em>archiviazione" per il nome di account di archiviazione, e la stessa posizione data center come l'app web.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*Nuovo AzureStorage.ps1* chiama il `New-AzureStorageAccount` cmdlet per creare l'account di archiviazione che restituisce l'account di valori di chiave di accesso e il nome. L'applicazione sono necessari questi valori per poter accedere a BLOB e code nell'account di archiviazione.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Non sempre voler creare un nuovo account di archiviazione; è possibile migliorare lo script mediante l'aggiunta di un parametro che indica se lo si desidera, è possibile utilizzare un account di archiviazione.

### <a name="create-the-databases"></a>Creare i database

Lo script principale viene quindi eseguito lo script di creazione di database, *New-AzureSql.ps1*, dopo l'impostazione di database predefinito e i nomi delle regole del firewall:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Script di creazione del database recupera indirizzo IP del computer di sviluppo e imposta una regola del firewall in modo che il computer di sviluppo possono connettersi e gestire il server. Lo script di creazione database passa quindi attraverso diversi passaggi per configurare i database:

- Crea il server utilizzando il `New-AzureSqlDatabaseServer` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Crea regole del firewall per abilitare il computer di sviluppo per gestire il server e per consentire all'app web per la connessione. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Crea un contesto di database che include il nome del server e le credenziali, usando il `New-AzureSqlDatabaseServerContext` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` è una funzione nello script che chiama il `ConvertTo-SecureString` cmdlet per crittografare la password e restituisce un `PSCredential` dell'oggetto, lo stesso tipo che il `Get-Credential` cmdlet restituisce.
- Crea il database dell'applicazione e il database delle appartenenze utilizzando il `New-AzureSqlDatabase` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Chiama una funzione definita localmente tocreates una stringa di connessione per ogni database. L'applicazione userà queste stringhe di connessione per accedere ai database. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString è una funzione definita nello script che crea la stringa di connessione dai valori dei parametri forniti a esso.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Restituisce una tabella hash con il nome del server di database e le stringhe di connessione.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

L'app Fix It Usa appartenenza separato e i database dell'applicazione. È anche possibile inserire dati all'appartenenza e dell'applicazione in un singolo database.

### <a name="store-app-settings-and-connection-strings"></a>Le impostazioni dell'app Store e le stringhe di connessione

Azure offre una funzionalità che consente di archiviare le impostazioni e le stringhe di connessione automaticamente l'override di ciò che viene restituito all'applicazione quando si tenta di leggere il `appSettings` o `connectionStrings` raccolte nel file Web. config. Si tratta di un'alternativa all'applicazione [trasformazioni di Web. config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) durante la distribuzione. Per altre informazioni, vedere [Store dati sensibili in Azure](source-control.md#appsettings) più avanti in questo e-book.

Lo script di creazione ambiente vengono archiviati in Azure tutte le `appSettings` e `connectionStrings` valori che l'applicazione deve accedere l'account di archiviazione e i database quando è in esecuzione in Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) è un framework di dati di telemetria illustrato nel [monitoraggio e telemetria](monitoring-and-telemetry.md) capitolo. Inoltre, lo script di creazione ambiente riavvia l'app web per assicurarsi che lo preleva le impostazioni di New Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Preparazione per la distribuzione

Al termine del processo, lo script di creazione ambiente chiama due funzioni per creare i file che verranno usati dallo script di distribuzione.

Una di queste funzioni crea un profilo di pubblicazione *(&lt;websitename&gt;pubxml* file). Il codice chiama l'API REST di Azure per ottenere le impostazioni di pubblicazione e Salva le informazioni contenute in un *publishsettings* file. Quindi, Usa le informazioni da tale file insieme a un file di modello (*pubxml.template*) per creare il *pubxml* file che contiene il profilo di pubblicazione. Questo processo in due passaggi simula operazioni in Visual Studio: scaricare un *publishsettings* file e importarla per creare un profilo di pubblicazione.

L'altra funzione Usa un altro file di modello (sito Web-environment.template) per creare un *sito Web-environment.xml* file che contiene impostazioni, lo script di distribuzione è necessario utilizzare insieme al *pubxml*file.

### <a name="troubleshooting-and-error-handling"></a>Risoluzione dei problemi e la gestione degli errori

Gli script sono simili a programmi: è possibile non riesca e quando non lo si desidera sapere come può sull'errore e che lo ha generato. Per questo motivo, lo script di creazione ambiente modifica il valore della `VerbosePreference` variabile dal `SilentlyContinue` a `Continue` in modo che tutti i messaggi dettagliati vengono visualizzati. Modifica anche il valore della `ErrorActionPreference` variabile da `Continue` a `Stop`, in modo che lo script si interrompe anche quando si verificano errori non fatali:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Prima che esegue qualsiasi attività, lo script memorizza l'ora di inizio in modo da poter calcolare il tempo trascorso dopo che è stata eseguita:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Al termine il proprio lavoro, lo script visualizza il tempo trascorso:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

E per ogni operazione della chiave lo script scrive i messaggi dettagliati, ad esempio:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Script di distribuzione

Ciò che il *New-AzureWebsiteEnv.ps1* lo script esegue per la creazione dell'ambiente, il *Publish-AzureWebsite.ps1* lo script esegue per la distribuzione dell'applicazione.

Lo script di distribuzione Ottiene il nome dell'app web dal *sito Web-environment.xml* file creati dallo script di creazione ambiente.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Ottiene la password utente della distribuzione dal *publishsettings* file:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Esegue la [MSBuild](http://msbuildbook.com/) comando Compila e distribuisce il progetto:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

E se è stato specificato il `Launch` parametro della riga di comando, chiama il `Show-AzureWebsite` cmdlet per aprire il browser predefinito usando l'URL del sito Web.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

È possibile eseguire lo script di distribuzione con un comando simile alla seguente:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

E al termine, si apre il browser con il sito in esecuzione nel cloud di `<websitename>.azurewebsites.net` URL.

![Risolverlo app distribuita in Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Riepilogo

Questi script è possibile essere certi che gli stessi passaggi verranno sempre eseguiti nello stesso ordine con le stesse opzioni. Ciò garantisce che ogni sviluppatore del team non viene perso un avvenimento o crei qualcosa o distribuire un nome personalizzato nella propria macchina che non verrà effettivamente funzionano in modo analogo nell'ambiente di un altro membro del team o nell'ambiente di produzione.

In modo analogo, è possibile automatizzare Azure più funzioni di gestione che è possibile eseguire nel portale di gestione, usando l'API REST, gli script Windows PowerShell, un linguaggio .NET, API o un'utilità di Bash che è possibile eseguire su Linux o Mac.

Nel [capitolo successivo](source-control.md) verrà esaminare il codice sorgente e spiegano perché è importante includere gli script nel repository del codice sorgente.

## <a name="resources"></a>Risorse

- [Installare e configurare Windows PowerShell per Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Viene illustrato come installare i cmdlet di PowerShell di Azure e su come installare il certificato che è necessario nel computer per gestire Azure account. Si tratta di un posto ideale per iniziare perché include anche collegamenti a risorse per l'apprendimento di PowerShell.
- [Script Center di Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portale WindowsAzure.com alle risorse per lo sviluppo di script per la gestione di servizi di Azure, con collegamenti per ottenere esercitazioni introduttive, codice di origine e documentazione di riferimento di cmdlet e script di esempio
- [Oggetto Scripter festivo: Introduzione a Azure e PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). In un blog dedicato a Windows PowerShell, questo post offre un'ottima introduzione all'uso di PowerShell per le funzioni di gestione di Azure.
- [Installare e configurare l'interfaccia della riga di comando multipiattaforma](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Esercitazione introduttiva per un framework di scripting Azure che funziona su Mac e Linux, nonché Windows sistemi.
- [Sezione strumenti da riga di comando dell'argomento scaricare Azure SDK e strumenti](https://azure.microsoft.com/downloads/). Pagina del portale per la documentazione e download relativi agli strumenti da riga di comando di Azure.
- [Automazione completa con le librerie di gestione di Azure e .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman introduce l'API di gestione .NET per Azure.
- [Uso degli script di PowerShell di Windows per la pubblicazione in ambienti di Test e sviluppo](https://msdn.microsoft.com/library/azure/dn642480.aspx). Documentazione su MSDN che illustra come usare script che Visual Studio genera automaticamente per i progetti web di pubblicazione.
- [PowerShell Tools per Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Estensione di Visual Studio che aggiunge il supporto di language per Windows PowerShell in Visual Studio.

> [!div class="step-by-step"]
> [Precedente](introduction.md)
> [Successivo](source-control.md)
