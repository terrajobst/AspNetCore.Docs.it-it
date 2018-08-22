---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Distribuzione di pacchetti Web | Microsoft Docs
author: jrjlee
description: Questo argomento viene descritto come pubblicare i pacchetti di distribuzione web a un server remoto utilizzando lo strumento di distribuzione di Internet Information Services (IIS) Web (Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: d7a17a2830ab044f6f7446edc18c394d97b09fa0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827081"
---
<a name="deploying-web-packages"></a>Distribuzione di pacchetti Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento viene descritto come pubblicare i pacchetti di distribuzione web a un server remoto utilizzando lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web) 2.0.
> 
> Esistono due modi principali in cui è possibile distribuire un pacchetto web a un server remoto:
> 
> - È possibile usare l'utilità della riga di comando MSDeploy.exe direttamente.
> - È possibile eseguire la *[nome progetto].deploy.cmd* file che il processo di compilazione che genera l'errore.
> 
> Il risultato finale è lo stesso indipendentemente dall'approccio usato. In pratica, tutte le *. deploy. cmd* file non consiste nell'eseguire MSDeploy.exe con alcuni valori predeterminati, in modo che non è necessario fornire quante più informazioni per distribuire il pacchetto. Ciò semplifica il processo di distribuzione. D'altra parte, usando direttamente MSDeploy.exe offre maggiore flessibilità esattamente come viene distribuito il pacchetto.
> 
> Approccio utilizzato dipende da numerosi fattori, tra cui il livello di controllo sono necessarie tramite il processo di distribuzione e indica se la destinazione scelta è il servizio Web Deploy Remote Agent oppure il gestore di distribuzione Web. Questo argomento viene illustrato come utilizzare ciascun approccio e identifica quando ogni approccio è appropriato.
> 
> Le attività e procedure dettagliate in questo argomento presuppongono che:
> 
> - È stata creata ed è stato incluso nel pacchetto di applicazione web, come descritto in [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).
> - È stato modificato il *SetParameters* file per fornire i valori del parametro corretto per l'ambiente di destinazione, come descritto in [configurazione dei parametri per una distribuzione pacchetto Web](configuring-parameters-for-web-package-deployment.md).


Esegue il [*nome progetto*]*. deploy. cmd* file è il modo più semplice per distribuire un pacchetto di web. In particolare, tramite il *. deploy. cmd* file offre questi vantaggi rispetto all'uso MSDeploy.exe direttamente:

- Non è necessario specificare il percorso del pacchetto di distribuzione web&#x2014;il *. deploy. cmd* file è già noto in cui è.
- Non è necessario specificare il percorso dei *SetParameters* file&#x2014;il *. deploy. cmd* file è già noto in cui è.
- Non è necessario specificare i provider di MSDeploy di origine e destinazione&#x2014;il *. deploy. cmd* file conosca già i valori da usare.
- Non è necessario specificare le impostazioni di MSDeploy operazione&#x2014;il *. deploy. cmd* file aggiunge i valori comunemente necessari al comando MSDeploy.exe automaticamente.

Prima di usare la *. deploy. cmd* file per distribuire un pacchetto web, è necessario assicurarsi che:

- Il *. deploy. cmd* file, il [*nome del progetto*]. *SetParameters* file e il pacchetto web ([*nome progetto*]. *zip*) sono nella stessa cartella.
- La funzionalità distribuzione Web (MSDeploy.exe) viene installata nel computer in cui viene eseguito il *. deploy. cmd* file.

Il *. deploy. cmd* file supporta varie opzioni della riga di comando. Quando si esegue il file da un prompt dei comandi, si tratta la sintassi di base:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


È necessario specificare una **/T** flag o un' **/Y** flag, per indicare se si desidera eseguire rispettivamente un'esecuzione della versione di valutazione o una distribuzione in tempo reale (non usare entrambi i flag nello stesso comando). Questa tabella illustra le finalità di ciascuno di questi flag.

| Flag | Descrizione |
| --- | --- |
| **/T** | Chiama MSDeploy.exe con il **– whatif** flag che indica un'esecuzione di test. Anziché distribuire il pacchetto, viene creato un report di che cosa accadrebbe se si distribuisce il pacchetto. |
| **/Y** | Chiama MSDeploy.exe senza il **– whatif** flag. Questa opzione distribuisce il pacchetto per il computer locale o server di destinazione specificato. |
| **/M** | Specifica il server di destinazione assegnare un nome o URL del servizio. Per altre informazioni sui valori di cui è possibile fornire qui, vedere la **considerazioni sull'Endpoint** in questo argomento. Se si omette il **/M** flag, il pacchetto verrà distribuito nel computer locale. |
| **/A** | Specifica il tipo di autenticazione che MSDeploy.exe deve usare per eseguire la distribuzione. I valori possibili sono **NTLM** e **base**. Se si omette il **/A** flag, per impostazione predefinita il tipo di autenticazione **NTLM** per la distribuzione al servizio Web distribuire l'agente remoto e a **base** per la distribuzione per la distribuzione Web Gestore. |
| **/U** | Specifica il nome utente. Questo vale solo se si usa l'autenticazione di base. |
| **/P** | Modifica la password. Questo vale solo se si usa l'autenticazione di base. |
| **/L** | Indica che il pacchetto deve essere distribuito all'istanza locale di IIS Express. |
| **/G** | Specifica che il pacchetto viene distribuito usando il [del provider tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Se si omette il **/G** flag, il valore predefinito è **false**. |

> [!NOTE]
> Ogni volta che il processo di compilazione crea un pacchetto web, crea anche un file denominato *[nome progetto] deploy-file Readme. txt* che spiega queste opzioni di distribuzione.


Oltre a questi flag, è possibile specificare impostazioni per le operazioni di distribuzione Web come aggiuntiva *. deploy. cmd* parametri. Eventuali impostazioni aggiuntive specificate vengono semplicemente passati al comando MSDeploy.exe sottostante. Per altre informazioni su queste impostazioni, vedere [Web Deploy operazione Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Si supponga che si vuole distribuire il progetto di applicazione web ContactManager.Mvc all'ambiente di test eseguendo il *. deploy. cmd* file. L'ambiente di test è configurato per usare il servizio Web distribuire l'agente remoto, come descritto in [configurare un Server Web per la pubblicazione distribuzione Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Per distribuire l'applicazione web, è necessario completare i passaggi successivi.

**Per distribuire un'applicazione web usando il. file Deploy. cmd**

1. Compilare e assemblare il progetto di applicazione web, come descritto in [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).
2. Modificare il *ContactManager.Mvc.SetParameters.xml* file per contenere i valori di parametro corretti per l'ambiente di test, come descritto in [configurazione dei parametri per una distribuzione pacchetto Web](configuring-parameters-for-web-package-deployment.md).
3. Aprire una finestra del prompt dei comandi e passare al percorso dei *ContactManager.Mvc.deploy.cmd* file.
4. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

In questo esempio:

- Il **/Y** flag indica che si vuole distribuire il pacchetto, anziché eseguire esegue una versione di valutazione.
- Il **/M** flag indica che si vuole distribuire il pacchetto al server denominato TESTWEB1. Da questo valore, MSDeploy.exe tenterà di distribuire il pacchetto per il servizio Web Deploy Remote Agent http://TESTWEB1/MSDeployAgentService.
- Il **/A** flag indica che si vuole usare l'autenticazione NTLM. Di conseguenza, non devi specificare un nome utente e password.

Per illustrare come l'uso di *. deploy. cmd* file semplifica il processo di distribuzione, esaminare il comando MSDeploy.exe che ottiene generato ed eseguito quando si esegue *ContactManager.Mvc.deploy.cmd* utilizzando le opzioni illustrate in precedenza.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Per altre informazioni sull'uso il *. deploy. cmd* file per distribuire un pacchetto web, vedere [procedura: installare un pacchetto di distribuzione tramite il File deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Usando MSDeploy.exe

Anche se tramite il *. deploy. cmd* file a livello generale semplifica il processo di distribuzione, esistono alcune situazioni, quando è preferibile usare MSDeploy.exe direttamente. Ad esempio:

- Se si desidera distribuire al gestore di distribuzione Web come utente senza privilegi di amministratore, è possibile usare la *. deploy. cmd* file. Ciò è dovuto a un bug nella distribuzione Web 2.0, come descritto in **considerazioni sull'Endpoint**.
- Se si desidera passare manualmente tra diverse *SetParameters* file in posizioni diverse, è preferibile utilizzare MSDeploy.exe direttamente.
- Se si desidera eseguire l'override di diversi argomenti della riga di comando MSDeploy.exe, è preferibile utilizzare MSDeploy.exe direttamente.

Quando si usa MSDeploy.exe, è necessario fornire tre informazioni essenziali:

- Oggetto **– origine** parametro che indica la provenienza dei dati.
- Oggetto **dest-** parametro che indica dove i dati verrà.
- Oggetto **– verbo** parametro che indica il [operazione](https://technet.microsoft.com/library/dd568989(WS.10).aspx) da eseguire.

Si affida a MSDeploy.exe [provider di distribuzione Web](https://technet.microsoft.com/library/dd569040(WS.10).aspx) per elaborare i dati di origine e destinazione. La funzionalità distribuzione Web sono inclusi numerosi provider che rappresentano la gamma di origini dati e applicazioni consente l'utilizzo di&#x2014;, ad esempio, sono disponibili provider per database di SQL Server, server web IIS, i certificati, gli assembly cache (GAC) di assembly globale, vari file di configurazione diversi e molti altri tipi di dati. Entrambi i **– origine** parametro e il **– dest** parametro deve specificare un provider, nel formato **– origine**: [*providerName*] = [*posizione*]. Quando si distribuisce un pacchetto web in un sito Web IIS, è consigliabile usare questi valori:

- Il **– origine** provider è sempre [pacchetto](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Ad esempio:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- Il **– dest** provider è sempre [auto](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Ad esempio:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- Il **– verbo** è sempre **sincronizzazione**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Inoltre, è necessario specificare vari altri [impostazioni specifiche del provider](https://technet.microsoft.com/library/dd569001(WS.10).aspx) e generali [impostazioni per le operazioni](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Ad esempio, si supponga di che voler distribuire l'applicazione web di ContactManager.Mvc in un ambiente di staging. La distribuzione sarà destinati al gestore di distribuzione Web e deve usare l'autenticazione di base. Per distribuire l'applicazione web, è necessario completare i passaggi successivi.

**Per distribuire un'applicazione web con MSDeploy.exe**

1. Compilare e assemblare il progetto di applicazione web, come descritto in [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).
2. Modificare il *ContactManager.Mvc.SetParameters.xml* file per contenere i valori di parametro corretti per l'ambiente di staging, come descritto in [configurazione dei parametri per una distribuzione pacchetto Web](configuring-parameters-for-web-package-deployment.md).
3. Aprire una finestra del prompt dei comandi e passare al percorso di MSDeploy.exe. Si tratta in genere al %PROGRAMFILES%\IIS\Microsoft V2\msdeploy.exe distribuzione Web.
4. Digitare il comando seguente e quindi premere INVIO (ignorare le interruzioni di riga):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

In questo esempio:

- Il **– origine** parametro specifica il **pacchetto** provider e indica la posizione del pacchetto di web.
- Il **– dest** parametro specifica il **automatico** provider. Il **computerName** impostazione fornisce l'URL del servizio del gestore distribuzione Web nel server di destinazione. Il **authtype** impostazione indica che si desidera utilizzare l'autenticazione di base e di conseguenza è necessario fornire un **username** e un **password**. Infine, il **includeAcls = "False"** impostazione indica che non si desidera copiare gli elenchi di controllo di accesso (ACL) dei file dell'applicazione web di origine al server di destinazione.
- Il **– verbo: sincronizzazione** argomento indica che si vuole replicare il contenuto di origine nel server di destinazione.
- Il **-disableLink** argomenti indicano che non si desidera replicare i pool di applicazioni, la configurazione di directory virtuale o i certificati Secure Sockets Layer (SSL) nel server di destinazione. Per altre informazioni, vedere [Web Deploy collegamento Extensions](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- Il **– setParamFile** parametro fornisce la posizione del *SetParameters* file.
- Il **-allowUntrusted** commutatore indica che distribuzione Web devono accettare i certificati SSL che non sono stati rilasciati da un'autorità di certificazione attendibile. Se si esegue la distribuzione del gestore di distribuzione Web e si è utilizzato un certificato autofirmato per proteggere l'URL del servizio, è necessario includere questa opzione è attivata.

## <a name="automating-web-package-deployment"></a>Automatizzare la distribuzione di pacchetti Web

In molti scenari aziendali, si desidera distribuire i pacchetti web come parte di un passo a passo più grande o una distribuzione automatizzata. Indipendentemente dal fatto che si scelga di distribuire i pacchetti web eseguendo il *. deploy. cmd* file o utilizzando MSDeploy.exe direttamente, è possibile aggiungere parametri ai comandi e possono essere chiamati da una destinazione in un Microsoft Build Engine (MSBuild) file di progetto.

Nella soluzione di esempio Contact Manager, esaminare i **PublishWebPackages** di destinazione nel *Publish.proj* file. Questa destinazione viene eseguita una volta per ogni *. deploy. cmd* file identificato da un elenco di elementi denominato **PublishPackages**. La destinazione Usa le proprietà e metadati degli elementi per creare un set completo di valori di argomento per ogni *. deploy. cmd* file e quindi Usa le **Exec** attività per eseguire il comando.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Per una panoramica più ampia del modello di file di progetto nella soluzione di esempio e un'introduzione ai file di progetto personalizzato in generale, vedere [informazioni sul File di progetto](understanding-the-project-file.md) e [informazioni sul processo di compilazione](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Considerazioni sull'endpoint

Indipendentemente dal fatto se si distribuisce il pacchetto web eseguendo il *. deploy. cmd* file o utilizzando MSDeploy.exe direttamente, è necessario specificare un nome di computer o un endpoint del servizio per la distribuzione.

Se il server web di destinazione è configurato per la distribuzione usando il servizio Web distribuire l'agente remoto, specificare l'URL del servizio di destinazione come destinazione.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


In alternativa, è possibile specificare il nome del server solo come destinazione e distribuzione Web verrà dedotto l'URL del servizio agente remoto.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Se il server web di destinazione è configurato per la distribuzione usando il gestore di distribuzione Web, è necessario specificare l'indirizzo dell'endpoint del servizio di gestione Web (WMSvc) di IIS come destinazione. Per impostazione predefinita, questo assume il formato:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


È possibile assegnare uno qualsiasi di questi endpoint usando il *. deploy. cmd* MSDeploy.exe direttamente o file. Tuttavia, se si desidera distribuire il gestore di distribuzione Web come un utente senza privilegi di amministratore, come descritto in [configurare un Server Web per la pubblicazione distribuzione Web (gestore di distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), è necessario aggiungere una stringa di query per l'indirizzo dell'endpoint servizio.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Infatti, l'utente non amministratore non ha accesso a livello di server in IIS; Egli può accedere solo a un sito Web IIS specifico. Al momento della scrittura, a causa di un bug nella Pipeline di pubblicazione sul Web (WPP), non è possibile eseguire la *. deploy. cmd* file usando un indirizzo endpoint che include una stringa di query. In questo scenario, è necessario distribuire il pacchetto web usando MSDeploy.exe direttamente.

> [!NOTE]
> Per altre informazioni sul servizio di Web Deploy Remote Agent e il gestore di distribuzione Web, vedere [scelta dell'approccio a destra per la distribuzione Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Per indicazioni su come configurare i file di progetto specifici dell'ambiente per la distribuzione a questi endpoint, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Considerazioni sull'autenticazione

Indipendentemente dal fatto se si distribuisce il pacchetto web eseguendo il *. deploy. cmd* file o utilizzando MSDeploy.exe direttamente, è necessario specificare un tipo di autenticazione. La funzionalità distribuzione Web accetta due valori possibili: **NTLM** oppure **base**. Se si specifica l'autenticazione di base, è anche necessario specificare un nome utente e password. Esistono vari fattori, che è necessario tenere presenti quando si seleziona un tipo di autenticazione:

- Se si esegue la distribuzione del servizio Web distribuire l'agente remoto, è necessario usare l'autenticazione NTLM. Il servizio agente remoto non accetta le credenziali di autenticazione di base.
- Se si esegue la distribuzione del gestore di distribuzione Web, è possibile usare NTLM o l'autenticazione di base. L'impostazione predefinita è autenticazione di base. Anche se l'autenticazione di base si basa su nomi utente e password vengono trasmesse come testo normale, le credenziali sono protette perché il gestore di distribuzione Web utilizza sempre la crittografia SSL.
- Se il pacchetto web include un database e il server web e server di database sono computer diversi, non sarà in grado di distribuire il database utilizzando l'autenticazione NTLM a causa dell'errore il [limitazione di NTLM "doppio hop"](https://go.microsoft.com/?linkid=9805120). È necessario usare le credenziali di SQL Server nella stringa di connessione di distribuzione o specificare le credenziali di autenticazione di base per la distribuzione Web. Questo problema è descritto più dettagliatamente [la distribuzione dei database di appartenenza negli ambienti aziendali](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Conclusione

Questo argomento descrive come distribuire un pacchetto web eseguendo il *. deploy. cmd* file o utilizzando MSDeploy.exe direttamente. Spiegato quando ogni approccio potrebbe essere appropriato e descritto come è possibile impostare i parametri ed eseguire un comando di distribuzione come parte di un più ampio processo di compilazione passo a passo o automatizzati.

## <a name="further-reading"></a>Ulteriori informazioni

Per indicazioni su come creare e impostare i parametri per un pacchetto di distribuzione web, vedere [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md) e [configurazione dei parametri per una distribuzione pacchetto Web](configuring-parameters-for-web-package-deployment.md). Per indicazioni su come compilare e distribuire i pacchetti web da un'istanza di Team Foundation Server (TFS), vedere [configurazione di Team Foundation Server per la distribuzione di Web automatizzata](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Per informazioni su come personalizzare e risolvere i problemi del processo di distribuzione, vedere [esclusione file e cartelle dalla distribuzione](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Precedente](configuring-parameters-for-web-package-deployment.md)
> [Successivo](deploying-database-projects.md)
