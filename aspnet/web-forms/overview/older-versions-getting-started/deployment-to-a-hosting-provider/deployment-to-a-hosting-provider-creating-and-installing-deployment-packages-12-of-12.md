---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: risoluzione dei problemi (12 pari a 12) | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: d6175d46c6df3c9a4d9bc98492a4458bec45221c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811597"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: risoluzione dei problemi (12 pari a 12)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e illustra come distribuire siti Web di Windows Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


Questa pagina descrive alcuni problemi comuni che possono verificarsi quando si distribuisce un'applicazione web ASP.NET tramite Visual Studio. Per ognuno di essi, vengono fornite uno o più possibili cause e soluzioni corrispondenti.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Errore server nell'applicazione: '/' attuali impostazioni personalizzate errori impediscono di dettagli dell'errore di visualizzare in modalità remota

### <a name="scenario"></a>Scenario

Dopo aver distribuito un sito in un host remoto, viene visualizzato un messaggio di errore che indica l'impostazione customErrors nel file Web. config, ma non indica qual è la causa effettiva dell'errore:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Per impostazione predefinita, ASP.NET Mostra informazioni dettagliate sull'errore solo quando l'applicazione web è in esecuzione nel computer locale. In genere non si desidera visualizzare informazioni dettagliate sull'errore quando l'applicazione web è disponibile pubblicamente su Internet, in quanto hacker potrebbe essere in grado di usare queste informazioni per individuare le vulnerabilità dell'applicazione. Tuttavia, quando si distribuisce un sito o a un sito, a volte un elemento è inevitabili ed è necessario ottenere il messaggio di errore effettivo.

Per abilitare l'applicazione visualizzare i messaggi di errore dettagliati quando è in esecuzione nell'host remoto, modificare il file Web. config per impostare `customErrors` modalità disattivato, ridistribuire l'applicazione ed eseguire nuovamente l'applicazione:

1. Se il file Web. config dell'applicazione ha un `customErrors` elemento il `system.web` elemento, modificare il `mode` attributo su "off". In caso contrario aggiungere un `customErrors` elemento il `system.web` elemento con la `mode` attributo impostato su "off", come illustrato nell'esempio seguente:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Distribuire l'applicazione.
3. Eseguire l'applicazione e ripetere indipendentemente dal fatto in precedenza che ha causato l'errore si verificherà. Ora si può vedere che cos'è il messaggio di errore effettivo.
4. Dopo aver risolto l'errore, ripristino del file originale `customErrors` impostando e ridistribuire l'applicazione.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Accesso negato in una pagina Web che Usa SQL Server Compact

### <a name="scenario"></a>Scenario

Quando si distribuisce un sito che Usa SQL Server Compact e si esegue una pagina nel sito distribuito che accede al database, è visualizzato il messaggio di errore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

L'account del servizio di rete nel server deve essere in grado di leggere i servizi di SQL Compact file binari nativi nel *bin\amd64* oppure *bin\x86* cartella, ma non dispone delle autorizzazioni di lettura per tali cartelle. Set di autorizzazione di lettura per il servizio di rete di *bin* cartella, assicurandosi di estendere le autorizzazioni per le sottocartelle.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Impossibile leggere il File di configurazione a causa di autorizzazioni insufficienti

### <a name="scenario"></a>Scenario

Quando si fa clic di Visual Studio pulsante pubblica per distribuire un'applicazione in IIS nel computer locale, la pubblicazione ha esito negativo e il **Output** finestra Mostra un messaggio di errore simile al seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Usare un solo clic di pubblicazione in IIS nel computer locale, è necessario eseguire Visual Studio con autorizzazioni di amministratore. Chiudere Visual Studio e riavviarlo con autorizzazioni di amministratore.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Impossibile connettersi al Computer di destinazione... Tramite il processo specificato

### <a name="scenario"></a>Scenario

Quando si fa clic di Visual Studio pulsante pubblica per distribuire un'applicazione, la pubblicazione ha esito negativo e il **Output** finestra Mostra un messaggio di errore simile al seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Un server proxy è interrompere la comunicazione con il server di destinazione. Dal Pannello di controllo di Windows o in Internet Explorer, selezionare **Opzioni Internet** e selezionare il **connessioni** scheda. Nel **proprietà Internet** finestra di dialogo, fare clic su **impostazioni LAN**. Nel **le impostazioni di rete locale (LAN)** della finestra di dialogo deseleziona le **rileva automaticamente impostazioni** casella di controllo. Fare clic sul pulsante Pubblica nuovamente.

Se il problema persiste, contattare l'amministratore di sistema per determinare cosa può essere eseguita con le impostazioni di proxy o firewall. Il problema si verifica perché la distribuzione Web utilizza una porta non standard per la distribuzione di servizio di gestione Web (8172); per altre connessioni, distribuzione Web Usa la porta 80. Quando si distribuisce in un provider di hosting di terze parti, in genere si usa il servizio di gestione Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>Pool di applicazioni 4.0 di .NET predefinito non esiste

### <a name="scenario"></a>Scenario

Quando si distribuisce un'applicazione che richiede .NET Framework 4, è visualizzato il messaggio di errore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

ASP.NET 4 non è installato in IIS. Se il server che viene eseguita la distribuzione è il computer di sviluppo e Visual Studio 2010 è installato su di esso, ASP.NET 4 è installato nel computer ma potrebbero non essere installato in IIS. Nel server in cui viene eseguita la distribuzione, aprire un prompt dei comandi con privilegi elevati e installare ASP.NET 4 in IIS eseguendo i comandi seguenti:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

È anche necessario impostare manualmente la versione di .NET Framework del pool di applicazioni predefinito. Per altre informazioni, vedere la [distribuzione in IIS come ambiente di Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) esercitazione.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Formato della stringa di inizializzazione non è conforme alla specifica che inizia in corrispondenza dell'indice 0.

### <a name="scenario"></a>Scenario

Dopo aver distribuito un'applicazione che usa un solo clic pubblicare, quando si esegue una pagina di accesso al database viene visualizzato il messaggio di errore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Aprire il *Web. config* file nel sito distribuito e controllo per verificare se i valori di stringa di connessione iniziano con `$(ReplacableToken_`, come illustrato nell'esempio seguente:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Se le stringhe di connessione simile a questo esempio, modificare il file di progetto e aggiungere la proprietà seguente al `PropertyGroup` elemento che si trova per tutte le configurazioni di compilazione:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Quindi ridistribuire l'applicazione.

## <a name="http-500-internal-server-error"></a>HTTP 500 Errore interno del Server

### <a name="scenario"></a>Scenario

Quando si esegue il sito distribuito, viene visualizzato il messaggio di errore seguenti senza informazioni specifiche che indica la causa dell'errore:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Esistono molte cause di errori di tipo 500, ma una delle possibili cause se si siano seguendo queste esercitazioni è sufficiente inserire un elemento XML in una posizione errata in uno dei file di trasformazione XML. Questo errore, ad esempio, si otterrebbe se si inserisce la trasformazione che inserisce un `<location>` nell'elemento `<system.web>` invece che direttamente sotto `<configuration>`. La soluzione è in questo caso per correggere il file di trasformazione XML e ridistribuire.

## <a name="http-50021-internal-server-error"></a>Errore interno del Server 500.21 HTTP

### <a name="scenario"></a>Scenario

Quando si esegue il sito distribuito, è visualizzato il messaggio di errore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Il sito è stato distribuito destinazioni ASP.NET 4, ma ASP.NET 4 non è registrato in IIS sul server. Nel server aprire un prompt dei comandi con privilegi elevati e registrare ASP.NET 4 eseguendo i comandi seguenti:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

È anche necessario impostare manualmente la versione di .NET Framework del pool di applicazioni predefinito. Per altre informazioni, vedere la [distribuzione in IIS come ambiente di Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) esercitazione.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Accesso non riuscito di apertura Database SQL Server Express nell'App\_dati

### <a name="scenario"></a>Scenario

Aggiornato il *Web. config* stringa di connessione in modo che punti a un database di SQL Server Express come file un' *mdf* del file nei *App\_dati* cartella e il primo ora che si esegue l'applicazione che viene visualizzato il messaggio di errore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Il nome del *mdf* file non può corrispondere al nome di qualsiasi database di SQL Server Express che è stata presente nel computer, anche se è stato eliminato il *mdf* file del database già esistente. Modificare il nome del *mdf* file a un nome che non sia mai stato utilizzato come un nome di database e modificare le *Web. config* file usare il nuovo nome. In alternativa, è possibile usare [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) per eliminare in precedenza esistenti SQL Server Express i database.

## <a name="model-compatibility-cannot-be-checked"></a>Modello compatibilità non può essere estratto

### <a name="scenario"></a>Scenario

Aggiornato il *Web. config* file la stringa di connessione in modo che punti a un nuovo database di SQL Server Express e la prima volta che si esegue l'applicazione è visualizzato il messaggio di errore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Se il nome del database viene inserito nel file Web. config è stato mai usato prima che il computer, un database potrebbe già esistere con alcune tabelle in esso. Selezionare un nuovo nome che non è stato usato nel computer prima e modifica il *Web. config* file in modo che punti per usare questo nuovo nome del database. In alternativa, è possibile usare [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) oppure [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) per eliminare il database esistente.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Errore SQL quando si tenta di uno Script creare utenti o ruoli

### <a name="scenario"></a>Scenario

Si usa una distribuzione configurata nel database di **pubblicazione/creazione pacchetto SQL** scheda script SQL che vengono eseguiti durante la distribuzione includono comandi Create Role o Create User e ha esito negativo l'esecuzione di script quando vengono eseguiti tali comandi. È possibile visualizzare che ulteriori messaggi, ad esempio il seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Se questo errore si verifica dopo la configurazione di distribuzione del database nel **pubblica sul Web** guidata anziché **pubblicazione/creazione pacchetto SQL** scheda, creare un thread nel [configurazione e Distribuzione](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum e la soluzione verrà aggiunto a questa pagina sulla risoluzione dei problemi.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

L'account utente utilizzato per eseguire una distribuzione non dispone dell'autorizzazione per creare utenti o ruoli. Ad esempio, la società di hosting potrebbe assegnare il `db_datareader`, `db_datawriter`, e `db_ddladmin` ruoli per l'account utente che viene impostata automaticamente. Questi sono sufficienti per creare la maggior parte degli oggetti di database, ma non per la creazione di utenti o ruoli. Un modo per evitare l'errore è l'esclusione di ruoli e utenti dalla distribuzione del database. È possibile farlo modificando il `PreSource` (elemento) per il database del generato automaticamente uno script in modo che includa gli attributi seguenti:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Per informazioni su come modificare la `PreSource` elemento nel file di progetto, vedere [procedura: modificare le impostazioni di distribuzione nel File di progetto](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Se gli utenti o ruoli del database di sviluppo necessario che nel database di destinazione, contattare il provider di hosting per assistenza.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Errore di Timeout SQL Server durante l'esecuzione di script personalizzate durante la distribuzione

### <a name="scenario"></a>Scenario

Si sono specificati gli script SQL personalizzati per l'esecuzione durante la distribuzione e quando la distribuzione Web in esecuzione tali, sono un timeout.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Esecuzione di più script con le modalità di transazione diversi può causare errori di timeout. Per impostazione predefinita, gli script generati automaticamente eseguiti in una transazione, ma non gli script personalizzati. Se si seleziona il **eseguire il Pull dei dati e/o schema da un database esistente** opzione il **pubblicazione/creazione pacchetto SQL** scheda, e se si aggiunge uno script SQL personalizzato, è necessario modificare le impostazioni delle transazioni su alcuni script in modo che tutti gli script usano le stesse impostazioni delle transazioni. Per altre informazioni, vedere [procedura: distribuire un Database con un progetto di applicazione Web](https://msdn.microsoft.com/library/dd465343.aspx).

Se si hanno configurato le impostazioni delle transazioni in modo che tutti sono uguali ma ancora visualizzato questo errore, una possibile soluzione alternativa consiste nell'eseguire gli script separatamente. Nel **script di Database** griglia nel **pubblicazione/creazione pacchetto** scheda SQL, deseleziona il **Includi** casella di controllo per lo script che provoca l'errore di timeout, quindi pubblicare il progetto. Passare quindi al **script del Database** griglia, selezionare tale script **inclusione** casella di controllo e deselezionare il **Include** caselle di controllo per gli altri script. Quindi, pubblicare nuovamente il progetto. Questo tempo quando si pubblica, solo le esecuzioni di script personalizzato selezionato.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Stream dati del manifesto di sito non sono ancora disponibili

### <a name="scenario"></a>Scenario

Quando si installa un pacchetto tramite il *deploy. cmd* file con il `t` opzione (test), viene visualizzato il messaggio di errore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Il messaggio di errore indica che il comando non è possibile generare un report di test. Tuttavia, potrebbe essere eseguito il comando se si usa il `y` opzione (installazione effettivo). Il messaggio indica solo che si è verificato un problema con l'esecuzione del comando in modalità di test.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Questa applicazione richiede ManagedRuntimeVersion v4.0

### <a name="scenario"></a>Scenario

Quando si tenta di distribuire, è visualizzato il messaggio di errore seguente:

 Errore: I dati del flusso di ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' non è ancora disponibile. Il pool di applicazioni che si sta tentando di usare è la proprietà 'managedRuntimeVersion' impostato su '2.0'. Questa applicazione richiede "v4.0". 

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

ASP.NET 4 non è installato in IIS. Se il server che viene eseguita la distribuzione è il computer di sviluppo e Visual Studio 2010 è installato su di esso, ASP.NET 4 è installato nel computer ma potrebbero non essere installato in IIS. Nel server in cui viene eseguita la distribuzione, aprire un prompt dei comandi con privilegi elevati e installare ASP.NET 4 in IIS eseguendo i comandi seguenti:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Impossibile eseguire il cast Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Scenario

Quando si distribuisce un pacchetto, è visualizzato il messaggio di errore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Si tenta di distribuire da Gestione IIS usando l'interfaccia 1.1 distribuzione Web a un server con distribuzione Web 2.0 installato. Se si usa lo strumento di amministrazione remota di IIS per la distribuzione tramite l'importazione di un pacchetto, verificare i **nuove funzionalità disponibili in** finestra di dialogo quando si stabilisce la connessione. (Questa finestra di dialogo potrebbe essere visualizzata solo una volta quando innanzitutto viene stabilita la connessione. Per cancellare la connessione e ricominciare da capo, chiudere Gestione IIS e avviarlo nuovo immettendo `inetmgr /reset` al prompt dei comandi.) Se una delle funzionalità elencate **interfaccia utente Web di distribuire**e ha un numero di versione inferiore a 8, il server viene eseguita la distribuzione potrebbe avere versioni sia 1.1 e 2.0 di distribuzione Web installati. Per distribuire da un client che ha installato 2.0, il server deve avere solo distribuzione Web 2.0 installato. È necessario contattare il provider di hosting per risolvere il problema.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Non è possibile caricare i componenti nativi di SQL Server Compact

### <a name="scenario"></a>Scenario

Quando si esegue il sito distribuito, è visualizzato il messaggio di errore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Il sito distribuito non dispone *amd64* e *x86* sottocartelle con gli assembly nativi in essi contenuti sotto l'applicazione *bin* cartella. In un computer dotato di SQL Server Compact è installato, si trovano gli assembly nativi nella *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Il modo migliore per ottenere i file corretti nelle cartelle corrette in un progetto di Visual Studio consiste nell'installare il pacchetto NuGet SqlServerCompact. Installazione del pacchetto aggiunge uno script di post-compilazione per copiare gli assembly nativi nella *amd64* e *x86*. Affinché queste per la distribuzione, tuttavia, è necessario includerlo manualmente nel progetto. Per altre informazioni, vedere la [distribuzione di SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) esercitazione.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Errore di "Percorso non valido" dopo la distribuzione di un'applicazione Entity Framework Code First

### <a name="scenario"></a>Scenario

Si distribuisce un'applicazione che usa le migrazioni di Entity Framework Code First e un DBMS, ad esempio SQL Server Compact che archivia il relativo database in un file nell'App\_cartella dati. Si dispone di migrazioni Code First configurato per la creazione del database dopo la prima distribuzione. Quando si esegue l'applicazione viene visualizzato un messaggio di errore simile al seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Prima di tutto è tentato di creare il database, ma l'App\_cartella dati non esiste. È non è presente qualsiasi file *App\_dati* cartella quando è stato distribuito o è stata selezionata **escludere App\_dati** nel **pubblicazione/creazione pacchetto Web** scheda della finestra di **le proprietà del progetto** finestra. Il processo di distribuzione non creerà una cartella sul server se non sono presenti file nella cartella da copiare nel server. Se è già presente al database configurato nel sito, il processo di distribuzione verrà eliminati i file e il *App\_Data* cartella se è stata selezionata **Rimuovi file aggiuntivi nella destinazione** in il profilo di pubblicazione. Per risolvere il problema, inserire un file segnaposto, ad esempio un file con estensione txt nel *App\_Data* cartella, assicurarsi che non è **escludere App\_dati** selezionata e ridistribuire. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Oggetto COM di cui è stato separato dalla relativa RCW sottostante non è possibile usare".

### <a name="scenario"></a>Scenario

È stata completata con un solo clic pubblica per distribuire l'applicazione e quindi si iniziano a questo errore:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Chiudere e riavviare Visual Studio è in genere tutto ciò che serve per risolvere l'errore.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>La distribuzione ha esito negativo in quanto utente credenziali utilizzate per setACL pubblicazione non dispone dell'autorità

### <a name="scenario"></a>Scenario

Pubblicazione ha esito negativo generando un errore che indica che non dispone dell'autorità per impostare le autorizzazioni di cartella (l'account utente in uso non ha autorità setACL).

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Per impostazione predefinita, Visual Studio imposta autorizzazioni sulla cartella radice del sito di autorizzazioni lettura e scrittura per l'App\_cartella dati. Se si sa che le autorizzazioni predefinite per le cartelle sito siano corrette e non sono necessario impostare, è possibile disattivare questo comportamento aggiungendo **&lt;destinazione IncludeSetACLProviderOn&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** per il file del profilo di pubblicazione (da assegnare un singolo profilo) o al file WPP (per influiscono su tutti i profili). Per informazioni su come modificare questi file, vedere [procedura: modificare le impostazioni di distribuzione nel file di profilo (con estensione pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Errori di accesso negato quando l'applicazione tenta di scrivere in una cartella dell'applicazione

### <a name="scenario"></a>Scenario

Gli errori dell'applicazione quando prova a creare o modificare un file in una delle cartelle dell'applicazione, perché non dispone dell'autorità di scrittura per la cartella.

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Per impostazione predefinita, Visual Studio imposta autorizzazioni sulla cartella radice del sito di autorizzazioni lettura e scrittura per l'App\_cartella dati. Se l'applicazione necessita di accesso in scrittura a una sottocartella, è possibile impostare le autorizzazioni per la cartella come illustrato nella [impostazione delle autorizzazioni di cartelle](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) e [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazioni. Se l'applicazione necessita di accesso in scrittura alla cartella radice del sito, è necessario impedirne l'impostazione di accesso in sola lettura nella cartella radice aggiungendo **&lt;destinazione IncludeSetACLProviderOn&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** per il file del profilo di pubblicazione (da assegnare un singolo profilo) o al file WPP (per influiscono su tutti i profili). Per informazioni su come modificare questi file, vedere [procedura: modificare le impostazioni di distribuzione nel file di profilo (con estensione pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Errore di configurazione - attributo targetFramework fa riferimento a una versione successiva a quella installata di .NET Framework

### <a name="scenario"></a>Scenario

È stato pubblicato correttamente un progetto web destinato a ASP.NET 4.5, ma quando si esegue l'applicazione (con il `customErrors` modalità è impostata su "off" nel file Web. config) viene visualizzato l'errore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

La finestra di errore dell'origine della pagina di errore evidenzia la riga seguente da Web. config come la causa dell'errore:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Possibile causa e soluzione

Il server non supporta ASP.NET 4.5. Contattare il provider di hosting per determinare quando e se può essere aggiunto il supporto per ASP.NET 4.5. Se l'aggiornamento del server non è un'opzione, è necessario distribuire un progetto web destinata a 4 o versioni precedenti di ASP.NET invece. Se si distribuisce un ASP.NET 4 o versioni precedenti progetto web alla stessa destinazione, selezionare la **Rimuovi file aggiuntivi nella destinazione** casella di controllo la **impostazioni** scheda della finestra di **pubblica sul Web**procedura guidata. Se non si seleziona **Rimuovi file aggiuntivi nella destinazione**, continuerà a essere visualizzata la pagina di errore di configurazione.

Il progetto **delle proprietà** windows include un elenco di riepilogo a discesa dei framework di destinazione, ma è possibile risolvere questo problema semplicemente modificando queste informazioni dal **.NET Framework 4.5** a **di.NETFramework4**. Se si modifica il framework di destinazione a una versione precedente di framework, il progetto sarà ancora disponibili riferimenti agli assembly della versione framework successiva e non verrà eseguito. È necessario modificare tali riferimenti manualmente o creare un nuovo progetto destinato a .NET Framework 4 o versioni precedenti. Per altre informazioni, vedere [.NET Framework come destinazione per i siti Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
