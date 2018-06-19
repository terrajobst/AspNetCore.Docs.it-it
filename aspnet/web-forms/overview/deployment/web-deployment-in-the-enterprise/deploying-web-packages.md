---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Distribuzione di pacchetti Web | Documenti Microsoft
author: jrjlee
description: Questo argomento viene descritto come è possibile pubblicare pacchetti di distribuzione web a un server remoto utilizzando lo strumento di distribuzione Web (Web Internet Information Services (IIS)
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 5d3af0fdcc6e7ae20194ba658e0cf72ad22c1234
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887503"
---
<a name="deploying-web-packages"></a>Distribuzione di pacchetti Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento viene descritto come è possibile pubblicare pacchetti di distribuzione web a un server remoto utilizzando lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) 2.0.
> 
> Esistono due modi in cui è possibile distribuire un pacchetto web a un server remoto:
> 
> - È possibile utilizzare direttamente l'utilità della riga di comando di MSDeploy.exe.
> - È possibile eseguire il *.deploy.cmd [nome progetto]* file che genera il processo di compilazione.
> 
> Il risultato finale è lo stesso indipendentemente dall'approccio è utilizzare. Essenzialmente, tutto il *. deploy* file non è eseguire MSDeploy.exe con alcuni valori predefiniti, in modo che non è necessario fornire le informazioni per distribuire il pacchetto. Questa operazione semplifica il processo di distribuzione. D'altra parte, utilizzando direttamente MSDeploy.exe offre maggiore flessibilità su esattamente come viene distribuito il pacchetto.
> 
> Approccio adottato dipenderà da numerosi fattori, tra cui il livello di controllo è necessario sul processo di distribuzione e destinazione se il servizio agente remoto di distribuzione Web oppure il gestore di distribuzione Web. In questo argomento viene illustrato come utilizzare ogni approccio e identifica quando ogni approccio è appropriato.
> 
> Le attività e procedure dettagliate in questo argomento si presuppongono che:
> 
> - Generata e incluso nel pacchetto di applicazione web, come descritto in [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).
> - È stato modificato il *SetParameters* file per fornire i valori di parametri corretti per l'ambiente di destinazione, come descritto in [configurazione dei parametri per la distribuzione di pacchetto Web](configuring-parameters-for-web-package-deployment.md).


Esecuzione di [*nome progetto*]*. deploy* file è il modo più semplice per distribuire un pacchetto di web. In particolare, l'uso di *. deploy* file offre i seguenti vantaggi rispetto all'uso di MSDeploy.exe direttamente:

- Non è necessario specificare il percorso del pacchetto di distribuzione web&#x2014;il *. deploy. cmd* file già in cui lo sa.
- Non è necessario specificare il percorso del *SetParameters* file&#x2014;il *. deploy. cmd* file già in cui lo sa.
- Non è necessario specificare i provider di MSDeploy origine e di destinazione&#x2014;il *. deploy. cmd* file già conosca i valori da utilizzare.
- Non è necessario specificare le impostazioni di MSDeploy operazione&#x2014;il *. deploy. cmd* file aggiunge i valori comunemente necessari al comando MSDeploy.exe automaticamente.

Prima di utilizzare il *. deploy* file per distribuire un pacchetto web, è consigliabile verificare che:

- Il *. deploy* file, il [*nome progetto*]. *SetParameters* file e il pacchetto web ([*nome progetto*]. *zip*) sono nella stessa cartella.
- Distribuzione Web (MSDeploy.exe) è installata nel computer che esegue il *. deploy* file.

Il *. deploy* file supporta diverse opzioni della riga di comando. Quando si esegue il file da un prompt dei comandi, questa è la sintassi di base:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


È necessario specificare un **/T** flag o **/Y** flag per indicare se si desidera eseguire un'esecuzione di prova o di una distribuzione in tempo reale (non usare entrambi i flag nello stesso comando). Questa tabella illustra lo scopo di ciascuno di questi flag.

| Flag | Descrizione |
| --- | --- |
| **/T** | Chiama MSDeploy.exe con il **– whatif** flag che indica un'esecuzione di test. Anziché distribuire il pacchetto, viene creato un report di che cosa accadrebbe se è distribuire il pacchetto. |
| **/Y** | Chiama MSDeploy.exe senza il **– whatif** flag. Consente di distribuire il pacchetto nel computer locale oppure il server di destinazione specificato. |
| **/M** | Specifica il server di destinazione nome o URL del servizio. Per ulteriori informazioni sui valori è possibile fornire qui, vedere il **considerazioni Endpoint** in questo argomento. Se si omette il **/M** flag, il pacchetto verrà distribuito nel computer locale. |
| **/A** | Specifica il tipo di autenticazione che MSDeploy.exe deve utilizzare per eseguire la distribuzione. I valori possibili sono **NTLM** e **base**. Se si omette il **/A** flag, per impostazione predefinita il tipo di autenticazione **NTLM** per la distribuzione per il servizio agente remoto di distribuzione Web e a **base** per la distribuzione per la distribuzione Web Gestore. |
| **/U** | Specifica il nome utente. Si applica solo se si utilizza l'autenticazione di base. |
| **/P** | Modifica la password. Si applica solo se si utilizza l'autenticazione di base. |
| **/L** | Indica che è necessario distribuire il pacchetto per l'istanza locale di IIS Express. |
| **/G** | Specifica che il pacchetto viene distribuito con il [del provider tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Se si omette il **/G** flag, il valore predefinito è **false**. |

> [!NOTE]
> Ogni volta che il processo di compilazione crea un pacchetto web, crea anche un file denominato *[nome progetto] deploy-file Readme. txt* che illustra queste opzioni di distribuzione.


Oltre a questi flag, è possibile specificare le impostazioni di operazione di distribuzione Web come aggiuntive *. deploy* parametri. Eventuali impostazioni aggiuntive specificate vengono semplicemente passati al comando di MSDeploy.exe sottostante. Per ulteriori informazioni su queste impostazioni, vedere [Web Deploy operazione Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Si supponga che si desidera distribuire il progetto di applicazione web ContactManager.Mvc all'ambiente di test eseguendo il *. deploy* file. L'ambiente di test è configurato per utilizzare il servizio agente remoto di distribuzione Web, come descritto in [configurare un Server Web per distribuire pubblicazione sul Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Per distribuire l'applicazione web, è necessario completare i passaggi successivi.

**Per distribuire un'applicazione web utilizzando il. deploy file**

1. Compilare e distribuire il progetto di applicazione web, come descritto in [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).
2. Modificare il *ContactManager.Mvc.SetParameters.xml* file per contenere i valori di parametro corretti per l'ambiente di test, come descritto in [configurazione dei parametri per la distribuzione di pacchetto Web](configuring-parameters-for-web-package-deployment.md).
3. Aprire una finestra del prompt dei comandi e passare al percorso del *ContactManager.Mvc.deploy.cmd* file.
4. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

In questo esempio:

- Il **/Y** flag indica che si desidera distribuire il pacchetto, anziché eseguire una versione di valutazione.
- Il **/M** flag indica che si desidera distribuire il pacchetto al server denominato TESTWEB1. Da questo valore, MSDeploy.exe tenterà di distribuire il pacchetto per il servizio agente remoto di distribuzione Web http://TESTWEB1/MSDeployAgentService.
- Il **/A** flag indica che si desidera utilizzare l'autenticazione NTLM. Di conseguenza, non è necessario specificare un nome utente e una password.

Per illustrare come l'utilizzo di *. deploy* file semplifica il processo di distribuzione, esaminiamo il comando MSDeploy.exe generato viene eseguito quando si esegue *ContactManager.Mvc.deploy.cmd* utilizzando le opzioni illustrate in precedenza.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Per ulteriori informazioni sull'utilizzo di *. deploy* file per distribuire un pacchetto web, vedere [come: installare un pacchetto di distribuzione tramite il File deploy](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Utilizzo di MSDeploy.exe

Sebbene l'utilizzo di *. deploy* file è in genere semplifica il processo di distribuzione, esistono alcune situazioni, quando è preferibile utilizzare MSDeploy.exe direttamente. Ad esempio:

- Se si desidera distribuire al gestore distribuzione Web come un utente non amministratore, è possibile utilizzare il *. deploy* file. Ciò è dovuto a un bug nella distribuzione Web 2.0, come descritto in **considerazioni Endpoint**.
- Se si desidera passare manualmente tra diversi *SetParameters* file in posizioni diverse, è preferibile utilizzare MSDeploy.exe direttamente.
- Se si desidera eseguire l'override di diversi argomenti della riga di comando di MSDeploy.exe, è preferibile utilizzare MSDeploy.exe direttamente.

Quando si utilizza MSDeploy.exe, è necessario fornire tre tipi principali di informazioni:

- Oggetto **: origine** parametro che indica la provenienza dei dati.
- Oggetto **-dest** parametro che indica dove verrà i dati.
- Oggetto **– verbo** parametro che indica il [operazione](https://technet.microsoft.com/library/dd568989(WS.10).aspx) si desidera eseguire.

MSDeploy.exe si basa su [i provider di distribuzione Web](https://technet.microsoft.com/library/dd569040(WS.10).aspx) per elaborare i dati di origine e di destinazione. La funzionalità distribuzione Web include molti provider di rappresentare la gamma di applicazioni e le origini dati consente l'utilizzo di&#x2014;, ad esempio, esistono provider per i database di SQL Server, server web IIS, i certificati, gli assembly cache (GAC) di assembly globale, vari file di configurazione diverso e un numero elevato di altri tipi di dati. Entrambi i **: origine** parametro e **-dest** parametro deve specificare un provider, nel formato **: origine**: [*providerName*] = [*percorso*]. Quando si distribuisce un pacchetto web in un sito Web IIS, è consigliabile utilizzare questi valori:

- Il **: origine** è sempre [pacchetto](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Ad esempio:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- Il **-dest** è sempre [auto](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Ad esempio:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- Il **– verbo** è sempre **sincronizzazione**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Inoltre, è necessario specificare varie altre [impostazioni specifiche del provider](https://technet.microsoft.com/library/dd569001(WS.10).aspx) e generali [impostazioni operazione](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Si supponga, ad esempio, che si desidera distribuire l'applicazione web ContactManager.Mvc in un ambiente di gestione temporanea. La distribuzione verrà associato il gestore di distribuzione Web e utilizza l'autenticazione di base. Per distribuire l'applicazione web, è necessario completare i passaggi successivi.

**Per distribuire un'applicazione web tramite MSDeploy.exe**

1. Compilare e distribuire il progetto di applicazione web, come descritto in [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).
2. Modificare il *ContactManager.Mvc.SetParameters.xml* file per contenere i valori di parametro corretti per l'ambiente di gestione temporanea, come descritto in [configurazione dei parametri per la distribuzione di pacchetto Web](configuring-parameters-for-web-package-deployment.md).
3. Aprire una finestra del prompt dei comandi e passare al percorso di MSDeploy.exe. Si tratta in genere %PROGRAMFILES%\IIS\Microsoft V2\msdeploy.exe distribuzione Web.
4. Digitare il comando seguente e quindi premere INVIO (ignora le interruzioni di riga):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

In questo esempio:

- Il **: origine** parametro specifica il **pacchetto** provider e indica la posizione del pacchetto web.
- Il **-dest** parametro specifica il **auto** provider. Il **computerName** impostazione fornisce l'URL del servizio del gestore distribuzione Web nel server di destinazione. Il **authtype** impostazione indica che si desidera utilizzare l'autenticazione di base e di conseguenza è necessario fornire un **username** e **password**. Infine, il **includeAcls = "False"** impostazione indica che non si desidera copiare gli elenchi di controllo di accesso (ACL) dei file nell'applicazione web di origine al server di destinazione.
- Il **– verbo: sincronizzazione** argomento indica che si desidera replicare il contenuto di origine nel server di destinazione.
- Il **– disableLink** gli argomenti indicano che non si desidera replicare i pool di applicazioni, configurazione della directory virtuale o certificati Secure Sockets Layer (SSL) nel server di destinazione. Per ulteriori informazioni, vedere [Web distribuire le estensioni del collegamento](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- Il **– setParamFile** parametro fornisce la posizione del *SetParameters* file.
- Il **-allowUntrusted** parametro indica che la distribuzione Web accetta i certificati SSL che non sono stati rilasciati da un'autorità di certificazione attendibile. Se si distribuisce al gestore distribuzione Web e si utilizza un certificato autofirmato per proteggere l'URL del servizio, è necessario includere questo parametro.

## <a name="automating-web-package-deployment"></a>L'automatizzazione della distribuzione di pacchetto Web

In molti scenari aziendali, si desidera distribuire i pacchetti web come parte di un passo-passo più grande o la distribuzione automatica. È possibile scegliere di distribuire i pacchetti web eseguendo il *. deploy* file o tramite MSDeploy.exe direttamente, è possibile aggiungere parametri ai comandi e chiamarle da una destinazione in un Microsoft Build Engine (MSBuild) file di progetto.

Nella soluzione di esempio Contact Manager, esaminare il **PublishWebPackages** indirizzare il *Publish.proj* file. Questa destinazione viene eseguita una volta per ogni *. deploy* file identificato da un elenco di elementi denominato **PublishPackages**. La destinazione utilizza le proprietà e i metadati dell'elemento per creare un set completo di valori di argomento per ogni *. deploy* file e quindi viene utilizzato il **Exec** attività per eseguire il comando.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Per una panoramica più ampia del modello di file di progetto nella soluzione di esempio e un'introduzione ai file di progetto personalizzati in generale, vedere [informazioni sui File di progetto](understanding-the-project-file.md) e [comprendere il processo di compilazione](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Considerazioni di endpoint

Indipendentemente dal fatto se si distribuisce il pacchetto web eseguendo il *. deploy* file o tramite MSDeploy.exe direttamente, è necessario specificare un nome di computer o un endpoint del servizio per la distribuzione.

Se il server web di destinazione è configurato per la distribuzione tramite il servizio agente remoto di distribuzione Web, specificare l'URL del servizio di destinazione come destinazione.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


In alternativa, è possibile specificare il nome del server solo come destinazione e distribuzione Web dedurrà l'URL del servizio agente remoto.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Se il server web di destinazione è configurato per la distribuzione tramite il gestore di distribuzione Web, è necessario specificare l'indirizzo dell'endpoint del servizio di gestione Web (WMSvc) di IIS come destinazione di. Per impostazione predefinita, questo assume il formato:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


È possibile utilizzare uno di questi endpoint utilizzando il *. deploy* MSDeploy.exe direttamente o file. Tuttavia, se si desidera distribuire il gestore di distribuzione Web come un utente non amministratore, come descritto in [configurare un Server Web per distribuire pubblicazione sul Web (gestore di distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), è necessario aggiungere una stringa di query per l'indirizzo dell'endpoint del servizio.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


In questo modo l'utente non amministratore non dispone dell'accesso a livello di server a IIS. Egli può accedere solo a un sito Web IIS specifico. Al momento della scrittura, a causa di un bug nella Pipeline di pubblicazione sul Web (WPP), non è possibile eseguire il *. deploy* file utilizzando un indirizzo endpoint che include una stringa di query. In questo scenario, è necessario distribuire il pacchetto web utilizzando direttamente MSDeploy.exe.

> [!NOTE]
> Per ulteriori informazioni sul servizio di agente remoto di distribuzione Web e il gestore di distribuzione Web, vedere [scelta dell'approccio di destra per distribuzione Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Per istruzioni su come configurare i file di progetto specifici dell'ambiente per distribuire questi endpoint, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Considerazioni sull'autenticazione

Indipendentemente dal fatto se si distribuisce il pacchetto web eseguendo il *. deploy* file o tramite MSDeploy.exe direttamente, è necessario specificare un tipo di autenticazione. Distribuzione Web accetta due valori possibili: **NTLM** o **base**. Se si specifica l'autenticazione di base, è inoltre necessario specificare un nome utente e password. Esistono vari fattori, che è necessario tenere in considerazione quando si seleziona un tipo di autenticazione:

- Se si distribuisce il servizio agente remoto di distribuzione Web, è necessario utilizzare l'autenticazione NTLM. Il servizio agente remoto non accetta le credenziali di autenticazione di base.
- Se si distribuisce al gestore distribuzione Web, è possibile utilizzare NTLM o autenticazione di base. L'impostazione predefinita è autenticazione di base. Anche se l'autenticazione di base si basa su nomi utente e password trasmesse in testo normale, le credenziali sono protetti come il gestore di distribuzione Web utilizza sempre la crittografia SSL.
- Se il pacchetto web include un database e il server web e il server di database sono computer diversi, non sarà in grado di distribuire il database utilizzando l'autenticazione NTLM a causa di [limitazione "doppio hop" NTLM](https://go.microsoft.com/?linkid=9805120). È necessario utilizzare le credenziali di SQL Server nella stringa di connessione di distribuzione oppure fornire le credenziali di autenticazione di base per la distribuzione di Web. Questo problema è descritto in dettaglio in [la distribuzione di database di appartenenza per ambienti aziendali](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come è possibile distribuire un pacchetto web eseguendo il *. deploy* file o usando direttamente MSDeploy.exe. Spiegato quando ogni approccio potrebbe essere appropriato e descritto come è possibile parametrizzare ed eseguire un comando di distribuzione come parte di un processo di compilazione singolo passaggio o automatizzati più ampio.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni su come creare e impostare parametri di un pacchetto di distribuzione web, vedere [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md) e [configurazione dei parametri per la distribuzione di pacchetto Web](configuring-parameters-for-web-package-deployment.md). Per istruzioni su come compilare e distribuire i pacchetti web da un'istanza di Team Foundation Server (TFS), vedere [la configurazione di Team Foundation Server per la distribuzione di Web automatizzata](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Per informazioni su come personalizzare e risolvere i problemi del processo di distribuzione, vedere [esclusione dei file e cartelle da distribuzione](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Precedente](configuring-parameters-for-web-package-deployment.md)
> [Successivo](deploying-database-projects.md)
