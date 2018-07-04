---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Scelta dell'approccio corretto per la distribuzione Web | Microsoft Docs
author: jrjlee
description: Quando si lavora con Internet Information Services (IIS) strumento di distribuzione Web (distribuzione Web) 2.0 o versione successiva, sono presenti tre approcci principali, che è possibile usare per ottenere...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f1c3a09d9e960a53a25e0dd0b953224204a9ba7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367745"
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Scelta dell'approccio corretto per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Quando si lavora con Internet Information Services (IIS) strumento di distribuzione Web (distribuzione Web) 2.0 o versione successiva, esistono tre approcci principali, che è possibile usare per ottenere le applicazioni web nel pacchetto in un server web. È possibile:
> 
> - Distribuire l'applicazione da una posizione remota usando come destinazione il *servizio agente distribuzione Web* (noto anche come "remoto agente") nel server di destinazione.
> - Distribuire l'applicazione da una posizione remota usando distribuzione Web su richiesta (noto anche come "temp agente").
> - Distribuire l'applicazione da una posizione remota usando come destinazione il *gestore distribuzione Web di IIS* nel server di destinazione.
> - Distribuire l'applicazione quando si copia il pacchetto web nel server di destinazione e importarlo tramite Gestione IIS manualmente.
> 
> Come configurare i server web di destinazione varia in base quale approccio alla distribuzione da usare. Questo argomento consente di decidere quale approccio alla distribuzione è adatta a te.


Questa tabella illustra i principali vantaggi e svantaggi di ogni approccio di distribuzione, con gli scenari più comuni in base ciascun approccio.

| Approccio | Vantaggi | Svantaggi | Scenari tipici |
| --- | --- | --- | --- |
| Agente remoto | È semplice da configurare. È adatto per gli aggiornamenti periodici per le applicazioni web e il contenuto. | L'utente deve essere un amministratore nel server di destinazione. l'utente non è possibile fornire credenziali alternative. | Ambienti di sviluppo. Ambienti di test. |
| Agente TEMP | Non è necessario per installare distribuzione Web nel computer di destinazione. La versione più recente di distribuzione Web viene utilizzata automaticamente. | L'utente deve essere un amministratore nel server di destinazione. L'utente non è possibile fornire credenziali alternative. | Ambienti di sviluppo. Ambienti di test. |
| Gestore di distribuzione Web | Gli utenti non amministratori possono distribuire il contenuto. È adatto per gli aggiornamenti periodici per le applicazioni web e il contenuto. | È molto più complessa da configurare. | Ambienti di staging. Ambienti di produzione di Intranet. Ambienti host. |
| Distribuzione offline | È molto semplice da configurare. È adatto per ambienti isolati. | L'amministratore del server deve copiare e importare il pacchetto web ogni volta manualmente. | Ambienti di produzione con connessione Internet. Ambienti di rete isolato. |
  

## <a name="using-the-remote-agent"></a>Usa l'agente remoto

Quando si installa utilizzando le impostazioni predefinite in un server di destinazione di distribuzione Web, il servizio agente di distribuzione Web (il "agente remoto") viene automaticamente installato e avviato. Per impostazione predefinita, l'agente remoto espone un endpoint HTTP a questo indirizzo:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> È possibile sostituire [*server*] con il nome del computer del server web, un indirizzo IP per il server web o un nome host che si risolve nel server web.


Gli amministratori del server è possono distribuire pacchetti web da una posizione remota, ad esempio un computer di sviluppo o un server di compilazione specificando l'indirizzo dell'endpoint. Si supponga, ad esempio, che Matt Hink Fabrikam, Inc. ha compilato il progetto di applicazione web ContactManager.Mvc nel suo computer dello sviluppatore. Il processo di compilazione genera un pacchetto web, insieme a un *. deploy. cmd* file che contiene i comandi di distribuzione Web necessari per installare il pacchetto. Se Matt è un amministratore del server nel server TESTWEB1, è possibile distribuire l'applicazione web sul server di prova web eseguendo questo comando nel suo computer dello sviluppatore:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


In effetti, l'eseguibile di Web Deploy può dedurre l'indirizzo dell'endpoint dell'agente remoto, se si specifica il nome del computer, in modo Matt è sufficiente digitare quanto segue:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Per altre informazioni sulla sintassi della riga di comando di distribuzione Web e *. deploy. cmd* i file, vedere [procedura: installare un pacchetto di distribuzione tramite il File deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).


L'agente remoto offre un modo semplice per distribuire il contenuto da una posizione remota, e questo approccio può funzionare bene con la distribuzione di un solo clic o automatizzata. Tuttavia, l'utente che esegue il comando di distribuzione deve essere anche amministratore di dominio o un membro del gruppo administrators locale nel server di destinazione. Inoltre, l'agente remoto non supporta l'autenticazione di base, pertanto non è possibile passare credenziali alternative nella riga di comando.

L'agente remoto offre un approccio utile per la distribuzione negli scenari di test, in cui non è insolito per gli sviluppatori di controllo amministratore completo su un ambiente di server di test e le applicazioni sono in genere ricompilate e ridistribuite molto o lo sviluppo di frequente. Tuttavia, questo approccio è in genere meno accettabile per ambienti di gestione temporanea o produzione.

Per un esempio end-to-end di uno scenario che usa l'approccio dell'agente remoto, vedere [Scenario: configurazione di un ambiente di Test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Tramite l'agente Temp

L'approccio agente temporanea per la distribuzione è simile all'approccio dell'agente remoto. Tuttavia, a differenza dell'approccio di agente remoto, non devi installare distribuzione Web nel server web di destinazione. Al contrario, quando si esegue la distribuzione, distribuzione Web installerà una versione temporanea del servizio agente distribuzione web nel server di destinazione e verrà usato per distribuire il contenuto in IIS. Una volta completata la distribuzione, vengono rimossi tutti i file temporanei.

Se si desidera utilizzare l'impostazione del provider agente temp, aggiungere il **/g** flag al comando di distribuzione:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> È possibile usare l'agente temporanea se il servizio agente di distribuzione web è installato nel computer di destinazione, anche se il servizio non è in esecuzione.


Il vantaggio di questo approccio è che non è necessario gestire installazioni di distribuzione Web sui server di destinazione. Inoltre, non è necessario assicurarsi che il computer di origine e destinazione siano in esecuzione la stessa versione di distribuzione Web. Tuttavia, questo approccio presenta le stesse limitazioni dell'entità come l'approccio dell'agente remoto, vale a dire che è necessario essere un amministratore locale nel server di destinazione per poter distribuire il contenuto è supportato solo l'autenticazione NTLM. L'approccio temp agente richiede anche la configurazione iniziale di molte altre operazioni dell'ambiente di destinazione.

Per altre informazioni sull'uso dell'agente temporanea, vedere [procedura: installare un pacchetto di distribuzione tramite il File deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx) e [distribuzione Web su richiesta](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Tramite il Web distribuire gestore

Per IIS 7 e versioni successive, distribuzione Web offre un approccio alternativo alla distribuzione tramite il gestore di distribuzione Web di IIS. Il gestore di distribuzione Web sono strettamente integrato con il servizio di gestione Web (WMSvc), di IIS che è progettato per consentire agli utenti di gestire siti Web IIS da posizioni remote.

Per impostazione predefinita, l'agente remoto espone un endpoint HTTP a questo indirizzo:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> È possibile sostituire [*server*] con il nome del computer del server web, un indirizzo IP per il server web o un nome host che si risolve nel server web.


Il grande vantaggio del gestore distribuzione Web tramite l'agente remoto e l'agente temporanea, è possibile configurare IIS per consentire agli utenti senza privilegi di amministratore distribuire le applicazioni e il contenuto a specifici siti Web IIS. Il gestore di distribuzione Web supporta inoltre l'autenticazione di base, pertanto è possibile fornire credenziali alternative come parametri nei comandi di distribuzione Web. Il principale svantaggio è che il gestore di distribuzione Web è inizialmente molto più complicato da configurare.

Nel caso degli utenti senza privilegi di amministratore, il servizio di gestione Web (WMSvc) consentirà solo l'utente per la connessione a IIS tramite una connessione a livello di sito, anziché una connessione a livello di server. Per accedere a un particolare sito, è possibile includere una stringa di query specifico del sito nell'indirizzo dell'endpoint:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Si supponga, ad esempio, che un processo di compilazione è configurato per distribuire automaticamente un'applicazione web in un ambiente di staging dopo ogni compilazione riuscita. Se si usa l'approccio dell'agente remoto, è necessario configurare l'identità del processo di compilazione come amministratore nei server di destinazione. Al contrario, Usa l'approccio gestore distribuzione Web è possibile assegnare un utente senza privilegi di amministratore&#x2014;**FABRIKAM\stagingdeployer** in questo caso&#x2014;possono fornire queste autorizzazioni per un solo sito specifico di IIS e il processo di compilazione credenziali per distribuire il pacchetto di web.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Per ulteriori informazioni sulla sintassi e le operazioni da riga di comando di distribuzione Web, vedere [riferimento della riga di comando di distribuzione Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Per altre informazioni sull'uso il *. deploy. cmd* , vedere [procedura: installare un pacchetto di distribuzione tramite il File deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).


Il gestore di distribuzione Web fornisce un approccio utile per la distribuzione in ambienti, ambienti host e gli ambienti di produzione basati su intranet, in cui è disponibile l'accesso remoto al server, ma non sono credenziali di amministratore di gestione temporanea.

Per un esempio end-to-end di uno scenario che usa l'approccio gestore distribuzione Web, vedere [Scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Con la distribuzione Offline

In alcuni casi, non è possibile o pratica distribuire le applicazioni e il contenuto in un sito Web IIS da una posizione remota. Ad esempio, il computer di origine e destinazione potrebbe essere in reti isolate o segmenti di rete, o criteri del firewall potrebbero non consentire l'accesso remoto.

In questi scenari, è comunque possibile usare la creazione di pacchetti e la pubblicazione delle funzionalità di distribuzione Web; è sufficiente non è possibile usarli da una posizione remota. Al contrario, un amministratore nel server di destinazione deve copiare il pacchetto web nel server e importarlo tramite Gestione IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

L'approccio di distribuzione non in linea è in genere utile in ambienti di produzione con connessione Internet, in cui i server in una rete perimetrale possono dispone di connettività limitata con i computer nella rete interna.

Per un esempio end-to-end di uno scenario che usa l'approccio di distribuzione non in linea, vedere [Scenario: configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla sintassi e le operazioni da riga di comando di distribuzione Web, vedere [riferimento della riga di comando di distribuzione Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Per altre informazioni sull'uso il *. deploy. cmd* , vedere [procedura: installare un pacchetto di distribuzione tramite il File deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Per istruzioni generali sui diversi modi in cui è possibile distribuire i pacchetti web da un computer remoto, vedere [tramite distribuzione Web in remoto](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Per altre informazioni sull'uso di distribuzione Web su richiesta, vedere [distribuzione Web su richiesta](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Precedente](configuring-server-environments-for-web-deployment.md)
> [Successivo](scenario-configuring-a-test-environment-for-web-deployment.md)
