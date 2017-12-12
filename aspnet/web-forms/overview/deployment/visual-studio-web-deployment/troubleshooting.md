---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: risoluzione dei problemi | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 2d416432aad9d5654aefd8c63b84b6ae18967515
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: risoluzione dei problemi
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).


Questa pagina vengono descritti alcuni problemi comuni che possono verificarsi quando si distribuisce un'applicazione web ASP.NET utilizzando Visual Studio. Per ognuno di essi, vengono fornite uno o più possibili cause e soluzioni corrispondenti.

Gli scenari illustrati valide sia per Azure che i provider di hosting di terze parti. Per ulteriori informazioni sulla risoluzione dei problemi di App web nel servizio App di Azure, vedere le risorse seguenti:

- [Risolvere i problemi relativi a un'app web nel servizio App di Azure con Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Monitorare le app Web nel servizio App di Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-monitor//)
- [Annuncio del rilascio di Windows Azure SDK 2.0 per .NET](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (blog di scottgu, Mostra come ottenere i log di diagnostica in Visual Studio)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Errore server nell'applicazione - '/' attuali impostazioni personalizzate errori evitare che i dettagli dell'errore venga visualizzato in modalità remota

### <a name="scenario"></a>Scenario

Dopo aver distribuito un sito in un host remoto, viene visualizzato un messaggio di errore che indica l'impostazione customErrors nel file Web. config, ma non indica ciò che è stata la causa effettiva dell'errore:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Per impostazione predefinita, ASP.NET Mostra informazioni dettagliate sull'errore solo quando l'applicazione web è in esecuzione nel computer locale. In genere non si desidera visualizzare informazioni dettagliate sull'errore quando l'applicazione web è disponibile pubblicamente su Internet, perché i pirati informatici potrebbero essere in grado di utilizzare queste informazioni per individuare le vulnerabilità nell'applicazione. Tuttavia, quando si distribuisce un sito o gli aggiornamenti a un sito, a volte un elemento entra errato, è necessario ottenere il messaggio di errore effettivo.

Per abilitare l'applicazione visualizzare i messaggi di errore dettagliati durante l'esecuzione nell'host remoto, modificare il file Web. config per disattivare la modalità customErrors, ridistribuire l'applicazione ed eseguire nuovamente l'applicazione:

1. Se il file Web. config dell'applicazione contiene l'elemento di acustomErrors nell'elemento thesystem.web, modificare l'attributo themode su "off". In caso contrario aggiungere elemento acustomErrors nell'elemento thesystem.web con attributo themode impostato su "off", come illustrato nell'esempio seguente: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Distribuire l'applicazione.
3. Eseguire l'applicazione e ripetere indipendentemente dal fatto in precedenza che ha causato l'errore si verificherà. Ora è possibile visualizzare il messaggio di errore effettivo.
4. Dopo aver risolto l'errore, ripristinare l'impostazione customErrors originale e ridistribuire l'applicazione.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Impossibile creare/shadow copia 'ContosoUniversity' se il file esiste già.

### <a name="scenario"></a>Scenario

Quando si tenta di eseguire un progetto in Visual Studio ottenere una pagina di errore con un messaggio analogo al seguente:

Errore del server nell'applicazione '/'. Impossibile creare/shadow copia 'ContosoUniversity' se il file esiste già.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Attendere un minuto e aggiornare il browser, o ricompilare il sito e provare a ripetere l'esecuzione.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Accesso negato in una pagina Web che utilizza SQL Server Compact

### <a name="scenario"></a>Scenario

Quando si distribuisce un sito che Usa SQL Server Compact e si esegue una pagina nel sito distribuito che accede al database, è visualizzato il messaggio di errore seguente:

Accesso negato. (Eccezione da HRESULT: 0x80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

L'account del servizio di rete nel server deve essere in grado di leggere i file binari native servizio SQL Compact presenti il *bin\amd64* o *bin\x86* cartella, ma non dispone delle autorizzazioni di lettura per tali cartelle. Set di autorizzazione di lettura per il servizio di rete di *bin* cartella, assicurandosi di estendere le autorizzazioni per le sottocartelle.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Impossibile leggere il File di configurazione a causa di autorizzazioni insufficienti

### <a name="scenario"></a>Scenario

Quando si sceglie di Visual Studio pulsante pubblica per distribuire un'applicazione in IIS sul computer locale, la pubblicazione ha esito negativo e **Output** finestra viene visualizzato un messaggio di errore simile al seguente:

Si è verificato un errore durante la lettura del File di configurazione di IIS ' MACHINE/reindirizzamento'. L'identità di questa operazione è stata... Errore: Impossibile leggere il file di configurazione a causa di autorizzazioni sufficienti.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Per utilizzare un solo clic pubblicati in IIS sul computer locale, è necessario eseguire Visual Studio con autorizzazioni di amministratore. Chiudere Visual Studio e riavviarlo con autorizzazioni di amministratore.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Impossibile connettersi al Computer di destinazione... Il processo specificato

### <a name="scenario"></a>Scenario

Quando si sceglie di Visual Studio pulsante pubblica per distribuire un'applicazione, la pubblicazione ha esito negativo e **Output** finestra viene visualizzato un messaggio di errore simile al seguente:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Un server proxy è interrompere la comunicazione con il server di destinazione. Dal Pannello di controllo di Windows o in Internet Explorer, selezionare **Opzioni Internet** e selezionare il **connessioni** scheda. Nel **proprietà Internet** la finestra di dialogo, fare clic su **impostazioni LAN**. Nel **Impostazioni rete locale (LAN)** la finestra di dialogo, deseleziona il **rileva automaticamente impostazioni** casella di controllo. Quindi fare clic sul pulsante Pubblica nuovamente.

Se il problema persiste, contattare l'amministratore di sistema per determinare ciò che può essere eseguita con le impostazioni di proxy o firewall. Il problema si verifica perché la distribuzione Web utilizza una porta non standard per la distribuzione di servizio di gestione Web (8172); per altre connessioni, distribuzione Web Usa la porta 80. Quando si distribuisce in un provider di hosting di terze parti, si utilizza in genere il servizio gestione Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>Pool di applicazioni 4.0 .NET predefinito non esiste.

### <a name="scenario"></a>Scenario

Quando si distribuisce un'applicazione che richiede .NET Framework 4, è visualizzato il messaggio di errore seguente:

Il pool di applicazioni .NET 4.0 predefinito non esiste o non è possibile aggiungere l'applicazione. Verificare che ASP.NET 4.0 sia installato nel computer.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

In ASP.NET 4 non è installato in IIS. Se il server di che distribuzione è il computer di sviluppo e che dispone di Visual Studio 2010 installato, ASP.NET 4 è installato nel computer, ma potrebbe non essere installato in IIS. Nel server di distribuzione, aprire un prompt dei comandi con privilegi elevati e installare ASP.NET 4 in IIS eseguendo i comandi seguenti:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Inoltre, si potrebbe essere necessario impostare manualmente la versione di .NET Framework del pool di applicazioni predefinito. Per ulteriori informazioni, vedere la distribuzione in IIS come un'esercitazione di ambiente di Test di questa serie.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Formato della stringa di inizializzazione non è conforme alla specifica che inizia in corrispondenza dell'indice 0.

### <a name="scenario"></a>Scenario

Dopo la distribuzione di un'applicazione che utilizza un solo clic pubblica, quando si esegue una pagina che accede al database viene visualizzato il messaggio di errore seguente:

Formato della stringa di inizializzazione non è conforme alla specifica che inizia in corrispondenza dell'indice 0.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Aprire il *Web. config* file nel sito distribuito e verificare se i valori di stringa di connessione deve iniziare con $(ReplacableToken\_, come nell'esempio seguente:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Se le stringhe di connessione simile a quella in questo esempio, modificare il file di progetto e aggiungere la seguente proprietà per l'elemento PropertyGroup per tutte le configurazioni di compilazione:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Quindi ridistribuire l'applicazione.

## <a name="http-500-internal-server-error"></a>HTTP 500 Errore interno del Server

### <a name="scenario"></a>Scenario

Quando si esegue il sito distribuito, viene visualizzato il seguente messaggio di errore senza informazioni specifiche che indica la causa dell'errore:

Errore HTTP 500 - Errore interno del Server.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Esistono molte cause di 500 errori, ma una delle possibili cause se si seguono queste esercitazioni è sufficiente inserire un elemento XML nella posizione sbagliata in uno dei file di trasformazione Web. config. Questo errore, ad esempio, si otterrebbe se si inserisce la trasformazione che inserisce un &lt;percorso&gt; elemento sotto &lt;system.web&gt; anziché direttamente in &lt;configurazione&gt;. È possibile utilizzare la funzionalità di anteprima di trasformazione Web. config per verificare che le trasformazioni funzionino come previsto. La soluzione se si trova una trasformazione che è stata codificata in modo non corretto è per correggere il file di trasformazione e ridistribuire. Se un errore non è ovvio, impostare come commento le trasformazioni e ridistribuire per vedere quale causa l'errore 500.

## <a name="http-50021-internal-server-error"></a>Errore interno del Server 500.21 HTTP

### <a name="scenario"></a>Scenario

Quando si esegue il sito distribuito, è visualizzato il messaggio di errore seguente:

Errore HTTP 500.21 - errore interno del Server. Gestore ""PageHandlerFactory-integrata dispone di un modulo non valido "ManagedPipelineHandler" nell'elenco moduli.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Il sito è stato distribuito destinazioni ASP.NET 4, ma ASP.NET 4 non è registrato in IIS sul server. Nel server aprire un prompt dei comandi con privilegi elevati e registrare ASP.NET 4 eseguendo i comandi seguenti:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Inoltre, si potrebbe essere necessario impostare manualmente la versione di .NET Framework del pool di applicazioni predefinito. Per ulteriori informazioni, vedere la distribuzione in IIS come un'esercitazione di ambiente di Test di questa serie.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Accesso non riuscito di apertura Database SQL Server Express in App\_dati

### <a name="scenario"></a>Scenario

Aggiornate il *Web. config* file la stringa di connessione in modo che punti a un database di SQL Server Express come un *con estensione mdf* file nel *App\_dati* cartella e il primo Quando che si esegue l'applicazione che viene visualizzato il seguente messaggio di errore:

System.Data.SqlClient.SqlException: Impossibile aprire il database "DatabaseName" richiesto dall'account di accesso. Accesso non riuscito.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Il nome del *con estensione mdf* file non può corrispondere al nome di qualsiasi database di SQL Server Express che è stata presente nel computer, anche se è stato eliminato il *con estensione mdf* file del database esistente in precedenza. Modificare il nome del *con estensione mdf* file a un nome che non sia mai stato utilizzato come un nome di database e modificare il *Web. config* file da utilizzare il nuovo nome. In alternativa, è possibile utilizzare [SQL Server Management Studio Express](https://www.microsoft.com/en-us/download/details.aspx?displaylang=en&amp;id=7593) eliminare già SQL Server Express database.

## <a name="model-compatibility-cannot-be-checked"></a>Compatibilità di modello non selezionata

### <a name="scenario"></a>Scenario

Aggiornamento di *Web. config* file la stringa di connessione in modo che punti a un nuovo database di SQL Server Express e la prima volta che si esegue l'applicazione vedere il seguente messaggio di errore:

Impossibile verificare la compatibilità del modello perché il database non contiene i metadati del modello. Verificare che sia stato aggiunto IncludeMetadataConvention alle convenzioni di DbModelBuilder.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Se il nome del database viene inserito nel file Web. config è stato utilizzato prima che il computer, potrebbe esistere già un database con alcune tabelle in essa contenuti. Selezionare un nuovo nome che non è stato utilizzato nel computer prima di e modifica di *Web. config* file in modo da puntare per utilizzare il nome del nuovo database. In alternativa, è possibile utilizzare [SQL Server Express utilità](https://www.microsoft.com/en-us/download/details.aspx?DisplayLang=en&amp;id=3990) o [SQL Server Management Studio Express](https://www.microsoft.com/en-us/download/details.aspx?displaylang=en&amp;id=7593) per eliminare il database esistente.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Errore SQL quando uno Script tenta di creare utenti o ruoli

### <a name="scenario"></a>Scenario

Si utilizza la distribuzione di database configurata nel **pubblicazione/creazione pacchetto SQL** scheda script SQL da eseguire durante la distribuzione includono i comandi Create User o Create Role e si verifica un errore di esecuzione script quando vengono eseguiti tali comandi. Verranno visualizzati che ulteriori messaggi, ad esempio le operazioni seguenti:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Se questo errore si verifica dopo aver configurato la distribuzione di database nel **pubblica sul Web** guidata anziché **pubblicazione/creazione pacchetto SQL** scheda, creare un thread nel [configurazione e Distribuzione](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum e la soluzione verrà aggiunto a questa pagina sulla risoluzione dei problemi.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

L'account utente utilizzato per eseguire la distribuzione non dispone dell'autorizzazione per creare utenti o ruoli. Ad esempio, la società di hosting potrebbe assegnare il db\_datareader, db\_datawriter e db\_ddladmin ruoli per l'account utente che viene impostata automaticamente. Questi sono sufficienti per la maggior parte degli oggetti di database di creazione, ma non per la creazione di utenti o ruoli. Un modo per evitare l'errore è escludendo gli utenti e ruoli dalla distribuzione del database. È possibile farlo modificando l'elemento PreSource per lo script generato automaticamente del database in modo che includa i seguenti attributi:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Per informazioni su come modificare l'elemento PreSource nel file di progetto, vedere [procedura: modificare le impostazioni di distribuzione nel File di progetto](https://msdn.microsoft.com/en-us/library/ff398069(v=vs.100).aspx). Se gli utenti o ruoli nel database di sviluppo devono essere nel database di destinazione, contattare il provider di hosting per assistenza.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Errore di Timeout SQL Server durante l'esecuzione di script personalizzati durante la distribuzione

### <a name="scenario"></a>Scenario

È stato specificato gli script SQL personalizzati per l'esecuzione durante la distribuzione e quando la distribuzione Web li esegue, è previsto un timeout.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Esecuzione di più script che dispongono di modalità di transazione diversi può causare errori di timeout. Per impostazione predefinita, eseguire gli script generati automaticamente in una transazione, ma non gli script personalizzati. Se si seleziona il **Pull dei dati e/o schema da un database esistente** opzione il **pubblicazione/creazione pacchetto SQL** scheda, e se si aggiunge uno script SQL personalizzato, è necessario modificare le impostazioni delle transazioni su alcuni script in modo che tutti gli script utilizzano le stesse impostazioni di transazione. Per ulteriori informazioni, vedere [procedura: distribuire un Database con un progetto di applicazione Web](https://msdn.microsoft.com/en-us/library/dd465343.aspx).

Se si riceve ancora questo errore aver configurato le impostazioni delle transazioni in modo che tutti sono uguali, una possibile soluzione consiste nell'eseguire gli script separatamente. Nel **degli script di Database** griglia il **pubblicazione/creazione pacchetto** scheda SQL, deseleziona il **Include** casella di controllo per lo script che provoca l'errore di timeout, quindi pubblicare il progetto. Quindi passare nuovamente il **degli script di Database** griglia, selezionare lo script **Include** casella di controllo e deselezionare il **Include** caselle di controllo per gli altri script. Pubblicare nuovamente il progetto. Questa volta quando si pubblica, che viene eseguito solo lo script personalizzato selezionato.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Flusso di dati del manifesto del sito non sono ancora disponibili

### <a name="scenario"></a>Scenario

Quando si installa un pacchetto tramite il *deploy* file con l'opzione t (test), è visualizzato il messaggio di errore seguente:

Errore: I dati del flusso di ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' non è ancora disponibile.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Il messaggio di errore indica che il comando non può produrre un report di test. Tuttavia, il comando può essere eseguito se si utilizza l'opzione di y (installazione effettivo). Il messaggio indica solo che si è verificato un problema con l'esecuzione del comando in modalità test.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Questa applicazione richiede v 4.0 ManagedRuntimeVersion

### <a name="scenario"></a>Scenario

Quando si tenta di distribuire, è visualizzato il seguente messaggio di errore:

Il pool di applicazioni che si sta tentando di utilizzare è la proprietà 'managedRuntimeVersion' impostato su 'v 2.0'. Questa applicazione richiede 'v 4.0'.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

In ASP.NET 4 non è installato in IIS. Se il server di che distribuzione è il computer di sviluppo e che dispone di Visual Studio 2010 installato, ASP.NET 4 è installato nel computer, ma potrebbe non essere installato in IIS. Nel server di distribuzione, aprire un prompt dei comandi con privilegi elevati e installare ASP.NET 4 in IIS eseguendo i comandi seguenti:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Impossibile eseguire il cast Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Scenario

Quando si distribuisce un pacchetto, è visualizzato il messaggio di errore seguente:

Impossibile eseguire il cast dell'oggetto di tipo 'Microsoft.Web.Deployment.DeploymentProviderOptions' a 'Microsoft.Web.Deployment.DeploymentProviderOptions'.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Si sta tentando di distribuire da Gestione IIS utilizzando l'interfaccia 1.1 distribuzione Web a un server che dispone di Web Deploy 2.0. Se si utilizza lo strumento di amministrazione remota di IIS per la distribuzione tramite l'importazione di un pacchetto, controllare il **nuove funzionalità disponibili in** la finestra di dialogo quando si stabilisce la connessione. (Questa finestra di dialogo potrebbe essere visualizzata solo una volta quando la connessione viene innanzitutto stabilita. Per eliminare la connessione e ricominciare, chiudere Gestione IIS e lo si riapre immettendo inetmgr/Reimposta il prompt dei comandi.) Se una delle funzionalità elencate è **dell'interfaccia utente Web distribuire**e ha un numero di versione inferiore a 8, il server di distribuzione potrebbe essere versioni 1.1 e 2.0 di distribuzione Web installato. Per distribuire da un client che ha installato 2.0, il server deve disporre solo distribuzione Web 2.0 installato. È necessario contattare il provider di hosting per risolvere il problema.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Impossibile caricare i componenti nativi di SQL Server Compact

### <a name="scenario"></a>Scenario

Quando si esegue il sito distribuito, è visualizzato il messaggio di errore seguente:

Impossibile caricare i componenti di SQL Server Compact corrispondente per il provider ADO.NET della versione 8482 nativi. Installare la versione corretta di SQL Server Compact. Per ulteriori informazioni, vedere l'articolo della KB 974247, vedere.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Il sito distribuito non dispone *amd64* e *x86* le sottocartelle con gli assembly nativi in essi contenuti nell'applicazione *bin* cartella. In un computer dotato di SQL Server Compact è installato, gli assembly nativi si trovano *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Il modo migliore per ottenere i file corretti nelle cartelle corrette in un progetto di Visual Studio è installare il pacchetto NuGet SqlServerCompact. Installazione del pacchetto aggiunge uno script di post-compilazione per copiare gli assembly nativi in *amd64* e *x86*. Affinché queste da distribuire, tuttavia, è necessario includere manualmente nel progetto. Per ulteriori informazioni, vedere il [la distribuzione di SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) esercitazione.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Percorso non valido "errore" dopo la distribuzione di un'applicazione Entity Framework Code First

### <a name="scenario"></a>Scenario

Si distribuisce un'applicazione che utilizza Entity Framework migrazioni Code First e un DBMS, ad esempio SQL Server Compact che archivia il database in un file nell'App\_cartella dati. È necessario migrazioni Code First configurato per la creazione del database dopo la prima distribuzione. Quando si esegue l'applicazione viene visualizzato un messaggio di errore analogo al seguente:

Il percorso non è valido. Controllare la directory per il database. [Percorso = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Prima di tutto è tentato di creare il database, ma l'App\_dati cartella non esiste. Non essere di qualsiasi file *App\_dati* cartella quando è stato distribuito o è stato selezionato **escludere App\_dati** sul **pubblicazione/creazione pacchetto Web** scheda della finestra delle proprietà del progetto. Il processo di distribuzione non crea una cartella sul server se non sono presenti file nella cartella da copiare nel server. Se si dispone già di configurazione del sito del database, il processo di distribuzione verrà eliminati i file e *App\_dati* cartella stessa se si seleziona **Rimuovi file aggiuntivi nella destinazione** in il profilo di pubblicazione. Per risolvere il problema, inserire un file segnaposto, ad esempio un file con estensione txt nella *App\_dati* cartella, assicurarsi che non si dispone **escludere App\_dati** selezionata e ridistribuire.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Impossibile utilizzare l'oggetto COM che è stato separato dal RCW sottostante."

### <a name="scenario"></a>Scenario

È stato completato con un clic pubblicare per distribuire l'applicazione e si avvia venga visualizzato questo errore:

Attività di distribuzione Web non riuscita. (Impossibile completare la richiesta per l'URL agente remoto 'https://serverurl.com/msdeploy.axd?site=sitename').  
 Non è stato possibile completare la richiesta per l'URL agente remoto 'https://url/msdeploy.axd?site=sitename'.  
La richiesta è stata interrotta: la richiesta è stata annullata.  
Impossibile utilizzare l'oggetto COM che è stato separato dal RCW sottostante.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Chiudere e riavviare Visual Studio è in genere sono necessari per risolvere questo errore.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Distribuzione non riesce poiché utente credenziali utilizzate per la pubblicazione non è setACL autorità

### <a name="scenario"></a>Scenario

Pubblicazione ha esito negativo con un errore indicante che è non dispone di autorizzazioni sufficienti per impostare le autorizzazioni della cartella (l'account utente in uso non ha autorità setACL).

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Per impostazione predefinita, Visual Studio imposta le autorizzazioni per la cartella radice del sito autorizzazioni lettura e scrittura nell'App\_cartella dati. Se è noto che le autorizzazioni predefinite per le cartelle sito siano corrette e non è necessario impostare, è possibile disattivare questo comportamento aggiungendo  **&lt;IncludeSetACLProviderOn destinazione&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;**  al file del profilo di pubblicazione (per influisce su un unico profilo) o al file wpp.targets (per influisce su tutti i profili). Per informazioni su come modificare questi file, vedere [procedura: modificare le impostazioni di distribuzione nel file del profilo (con estensione pubxml)](https://msdn.microsoft.com/en-us/library/ff398069.aspx).

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Errori di accesso negato quando l'applicazione tenta di scrivere in una cartella dell'applicazione

### <a name="scenario"></a>Scenario

Gli errori dell'applicazione quando si tenta di creare o modificare un file in una delle cartelle dell'applicazione, perché non dispone di autorità di scrittura per tale cartella.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Per impostazione predefinita, Visual Studio imposta le autorizzazioni per la cartella radice del sito autorizzazioni lettura e scrittura nell'App\_cartella dati. Se l'applicazione necessita di accesso in scrittura a una sottocartella, è possibile impostare le autorizzazioni per tale cartella come illustrato nell'impostazione delle autorizzazioni di cartella di distribuzione per le esercitazioni di ambiente di produzione di questa serie. Se l'applicazione necessita di accesso in scrittura alla cartella radice del sito, è necessario impedirne l'impostazione di accesso in sola lettura nella cartella radice aggiungendo  **&lt;IncludeSetACLProviderOn destinazione&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;**  al file del profilo di pubblicazione (per influisce su un unico profilo) o al file wpp.targets (per influisce su tutti i profili). Per informazioni su come modificare questi file, vedere [procedura: modificare le impostazioni di distribuzione nel file del profilo (con estensione pubxml)](https://msdn.microsoft.com/en-us/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Errore di configurazione - attributo targetFramework fa riferimento a una versione successiva rispetto alla versione installata di .NET Framework

### <a name="scenario"></a>Scenario

Un progetto web destinato a ASP.NET 4.5 è stato pubblicato correttamente, ma quando si esegue l'applicazione (con la modalità customErrors impostata su "off" in Web. config file) viene visualizzato l'errore seguente:

Attributo 'targetFramework' il &lt;compilazione&gt; elemento del file Web. config viene utilizzato solo per la versione di destinazione 4.0 e versioni successive di .NET Framework (ad esempio, '&lt;compilazione targetFramework = "4.0"&gt;'). L'attributo 'targetFramework' fa attualmente riferimento a una versione successiva rispetto alla versione installata di .NET Framework. Specificare una versione di .NET Framework di destinazione valida oppure installare la versione richiesta di .NET Framework.

Finestra di errore dell'origine della pagina di errore evidenzia la riga seguente da Web. config come la causa dell'errore:

&lt;compilazione targetFramework = "4.5" /&gt;

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Il server non supporta ASP.NET 4.5. Contattare il provider di hosting per determinare quando e se può essere aggiunto il supporto per ASP.NET 4.5. Se l'aggiornamento del server non è un'opzione, è necessario distribuire un progetto web destinato a 4 o versioni precedenti di ASP.NET invece.

Se si distribuisce un progetto web precedenti ASP.NET 4 per la stessa destinazione, selezionare il **Rimuovi file aggiuntivi nella destinazione** casella di controllo di **impostazioni** scheda della finestra il **pubblica sul Web**procedura guidata. Se non si seleziona **Rimuovi file aggiuntivi nella destinazione**, si continuerà a ottenere la pagina di errore di configurazione.

Il progetto **proprietà** windows include un elenco di riepilogo a discesa dei framework di destinazione, ma è possibile risolvere questo problema modificando solo da **.NET Framework 4.5** a **di.NETFramework4**. Se si modifica il framework di destinazione a una versione precedente di framework, il progetto sarà ancora disponibili riferimenti agli assembly della versione di framework successive e non verrà eseguito. È necessario modificare tali riferimenti manualmente o creare un nuovo progetto destinato a .NET Framework 4 o versioni precedenti. Per ulteriori informazioni, vedere [.NET Framework come destinazione per i siti Web](https://msdn.microsoft.com/en-us/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Errori di attendibilità media

### <a name="scenario"></a>Scenario

Quando si esegue l'applicazione nell'ambiente di produzione, ottiene un errore relativo all'attendibilità Media.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Molti provider di hosting di terze parti esegue il sito web in attendibilità media, il che significa che non esistono alcuni aspetti che tale non è consentita. Ad esempio, il codice dell'applicazione non è possibile accedere al Registro di Windows e non può leggere o scrivere file che non rientrano nella gerarchia di cartelle dell'applicazione. Per impostazione predefinita, l'applicazione viene eseguita *attendibilità* nel computer locale, il che significa che l'applicazione potrebbe essere in grado di eseguire operazioni che avrà esito negativo quando viene distribuito nell'ambiente di produzione.

È possibile configurare l'applicazione per l'esecuzione in attendibilità media nell'ambiente locale di IIS per risolvere i problemi. A tale scopo, aprire l'applicazione *Web. config* file e aggiungere un **trust** elemento il **System. Web** elemento, come illustrato in questo esempio.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

L'applicazione verrà ora eseguita in attendibilità media in IIS anche sul computer locale.

Non farlo se esegue la distribuzione in Azure App Service, poiché Azure non richiede attendibilità Media. Al momento in questa esercitazione viene scritto in febbraio 2012, questo metodo per eseguire l'applicazione in attendibilità media genererà un errore in Azure.

Se si utilizza Entity Framework migrazioni Code First e si distribuisce in un provider di hosting che esegue l'applicazione in attendibilità media, assicurarsi di avere la versione 5.0 o versione successiva. In Entity Framework, versione 4.3, migrazioni richiede l'attendibilità totale per aggiornare lo schema del database.

## <a name="http-40417-not-found-error"></a>Errore HTTP 404.17 non trovato

### <a name="scenario"></a>Scenario

Quando si esegue il sito distribuito nel computer di sviluppo in IIS, viene visualizzato il seguente messaggio di errore reporting che il server sia in grado di elaborare Default.aspx:

Errore HTTP 404.17 - non trovato

Il contenuto richiesto sembra essere uno script e non verrà fornito dal gestore di file statici.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

ASP.NET 4.5 potrebbe non essere installato nel computer in uso. Vedere i passaggi di distribuzione a IIS come un'esercitazione di ambiente di Test di questa serie che spiegano come installare ASP.NET 4.5.

>[!div class="step-by-step"]
[Precedente](deploying-extra-files.md)
