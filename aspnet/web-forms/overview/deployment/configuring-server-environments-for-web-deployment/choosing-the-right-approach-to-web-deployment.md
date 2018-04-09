---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Scelta dell'approccio corretto per la distribuzione Web | Documenti Microsoft
author: jrjlee
description: Quando utilizza Internet Information Services (IIS) strumento di distribuzione Web (distribuzione Web) 2.0 o versione successiva, sono disponibili tre approcci principali, che è possibile utilizzare per ottenere...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2d690744687af93a69743dc6ce6c853629f61f5d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Scelta dell'approccio corretto per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Quando si utilizza Internet Information Services (IIS) strumento di distribuzione Web (distribuzione Web) 2.0 o versione successiva, sono disponibili tre approcci principali, che è possibile utilizzare per ottenere le applicazioni web nel pacchetto in un server web. È possibile:
> 
> - Distribuire l'applicazione da una posizione remota includendo il *servizio agente distribuzione Web* (noto anche come il "agente remoto") nel server di destinazione.
> - Distribuire l'applicazione da una posizione remota usando distribuzione Web su richiesta (noto anche come "temp agente").
> - Distribuire l'applicazione da una posizione remota includendo il *gestore distribuzione Web di IIS* nel server di destinazione.
> - Distribuire l'applicazione manualmente copiando il pacchetto web al server di destinazione e importarlo tramite Gestione IIS.
> 
> Come configurare i server web di destinazione dipenderà quale approccio alla distribuzione che si desidera utilizzare. In questo argomento consentono di decidere quale approccio alla distribuzione è adatta alle proprie esigenze.


Questa tabella mostra i principali vantaggi e svantaggi di ogni approccio di distribuzione, con gli scenari più adatti a ciascun approccio.

| Approccio | Vantaggi | Svantaggi | Scenari tipici |
| --- | --- | --- | --- |
| Agente remoto | È semplice da configurare. È adatto per gli aggiornamenti regolari alle applicazioni web e il contenuto. | L'utente deve essere un amministratore nel server di destinazione. Non è possibile specificare credenziali alternative. | Ambienti di sviluppo. Ambienti di test. |
| Agente TEMP | Non è necessario per installare distribuzione Web nel computer di destinazione. La versione più recente di distribuzione Web viene utilizzata automaticamente. | L'utente deve essere un amministratore nel server di destinazione. Non è possibile specificare credenziali alternative. | Ambienti di sviluppo. Ambienti di test. |
| Gestore distribuzione Web | Gli utenti non amministratori possono distribuire il contenuto. È adatto per gli aggiornamenti regolari alle applicazioni web e il contenuto. | È molto più complessa da impostare. | Ambienti di staging. Ambienti di produzione Intranet. Ambienti ospitati. |
| Distribuzione non in linea | È molto semplice da configurare. È adatto per ambienti isolati. | L'amministratore del server deve copiare e importare il pacchetto web ogni volta manualmente. | Ambienti di produzione con connessione Internet. Ambienti con isolamento rete. |
  

## <a name="using-the-remote-agent"></a>Usa l'agente remoto

Quando si installa utilizzando le impostazioni predefinite in un server di destinazione di distribuzione Web, viene automaticamente installato e avviato il servizio agente di distribuzione Web (il "agente remoto"). Per impostazione predefinita, l'agente remoto espone un endpoint HTTP a questo indirizzo:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> È possibile sostituire [*server*] con il nome del computer del server web, un indirizzo IP per il server web o un nome host che si risolve nel server web.


Gli amministratori del server è possono distribuire i pacchetti web da una posizione remota, ad esempio un computer dello sviluppatore o un server di compilazione, specificando l'indirizzo dell'endpoint. Si supponga, ad esempio, che Matt Hink Fabrikam, Inc. è stato compilato il progetto di applicazione web ContactManager.Mvc nel suo computer dello sviluppatore. Il processo di compilazione genera un pacchetto web, insieme a un *. deploy* file che contiene i comandi di distribuzione Web è necessario per installare il pacchetto. Se un amministratore del server nel server TESTWEB1, Matt poter distribuire l'applicazione web sul server di prova web eseguendo questo comando nel suo computer dello sviluppatore:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


In effetti, il file eseguibile di distribuzione Web può dedurre l'indirizzo dell'endpoint dell'agente remoto, se si fornisce il nome del computer, in modo Matt è sufficiente digitare quanto segue:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Per ulteriori informazioni sulla sintassi della riga di comando di distribuzione Web e *. deploy* file, vedere [procedura: installare un pacchetto di distribuzione tramite il File deploy](https://msdn.microsoft.com/library/ff356104.aspx).


L'agente remoto offre un modo semplice per distribuire il contenuto da una postazione remota e questo approccio può funzionare bene con la distribuzione automatica o di un solo clic. Tuttavia, l'utente che esegue il comando di distribuzione deve essere un amministratore di dominio o un membro del gruppo administrators locale nel server di destinazione. Inoltre, l'agente remoto non supporta l'autenticazione di base, non è possibile passare credenziali alternative nella riga di comando.

L'agente remoto fornisce un approccio utile per la distribuzione nello sviluppo e scenari di test, in cui non è insolito per gli sviluppatori di controllo amministratore completo in un ambiente di server di test e le applicazioni sono in genere ricompilate e ridistribuite molto di frequente. Tuttavia, questo approccio è in genere meno accettabile per ambienti di produzione o gestione temporanea.

Per un esempio end-to-end di uno scenario che utilizza l'approccio di agente remoto, vedere [Scenario: configurazione di un ambiente di Test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Tramite l'agente di Temp

L'approccio temporanea dell'agente di distribuzione è simile a quella dell'agente remoto. Tuttavia, a differenza dell'approccio di agente remoto, è necessario installare distribuzione Web nel server web di destinazione. Al contrario, quando si esegue la distribuzione, distribuzione Web verrà installato una versione temporanea del servizio agente distribuzione web nel server di destinazione e verrà utilizzato per distribuire i contenuti a IIS. Una volta completata la distribuzione, vengono rimossi tutti i file temporanei.

Se si desidera utilizzare l'impostazione del provider agente temporanea, aggiungere il **/g** flag al comando di distribuzione:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> È possibile usare l'agente temporaneo se il servizio agente di distribuzione web è installato nel computer di destinazione, anche se il servizio non è in esecuzione.


Il vantaggio di questo approccio è che non è necessario gestire le installazioni di distribuzione Web sui server di destinazione. Inoltre, non è necessario assicurarsi che il computer di origine e destinazione siano in esecuzione la stessa versione di distribuzione Web. Tuttavia, questo approccio presenta le stesse limitazioni dell'entità come l'approccio di agente remoto, vale a dire che è necessario essere un amministratore locale nel server di destinazione per distribuire il contenuto è supportato solo l'autenticazione NTLM. L'approccio temp agente richiede anche la configurazione iniziale molto più dell'ambiente di destinazione.

Per ulteriori informazioni sull'utilizzo dell'agente temporaneo, vedere [procedura: installare un pacchetto di distribuzione tramite il File deploy](https://msdn.microsoft.com/library/ff356104.aspx) e [distribuzione Web su richiesta](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Tramite il Web distribuire gestore

Per IIS 7 in poi, distribuzione Web offre un approccio alternativo alla distribuzione tramite il gestore di distribuzione Web di IIS. Il gestore di distribuzione Web sono strettamente integrato con il servizio di gestione Web (WMSvc), di IIS che è progettata per consentire agli utenti di gestire i siti Web IIS da posizioni remote.

Per impostazione predefinita, l'agente remoto espone un endpoint HTTP a questo indirizzo:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> È possibile sostituire [*server*] con il nome del computer del server web, un indirizzo IP per il server web o un nome host che si risolve nel server web.


Il vantaggio rilevante del gestore distribuzione Web tramite l'agente remoto e l'agente temporaneo, è che è possibile configurare IIS per consentire agli utenti non amministratori di distribuire applicazioni e il contenuto a specifici siti Web IIS. Il gestore di distribuzione Web supporta inoltre l'autenticazione di base, pertanto è possibile fornire credenziali alternative come parametri nei comandi di distribuzione Web. Lo svantaggio principale è che inizialmente è molto più complicato da installare e configurare il gestore di distribuzione Web.

Nel caso di utenti non amministratori, il servizio di gestione Web (WMSvc) consentirà solo l'utente per la connessione a IIS utilizzando una connessione a livello di sito, anziché una connessione a livello di server. Per accedere a un sito specifico, è possibile includere una stringa di query specifico nell'indirizzo endpoint:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Si supponga, ad esempio, che un processo di compilazione è configurato per distribuire automaticamente un'applicazione web in un ambiente di gestione temporanea dopo ogni compilazione ha esito positivo. Se si utilizza l'approccio di agente remoto, è necessario verificare l'identità del processo di compilazione di un amministratore sui server di destinazione. Al contrario, Usa l'approccio gestore distribuzione Web è possibile assegnare un utente senza privilegi di amministratore&#x2014;**FABRIKAM\stagingdeployer** in questo caso&#x2014;possono fornire queste autorizzazioni necessarie per un sito Web di IIS specifico solo, il processo di compilazione credenziali per distribuire il pacchetto di web.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Per ulteriori informazioni sulla sintassi e le operazioni da riga di comando di distribuzione Web, vedere [riferimento della riga di comando di distribuzione Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Per ulteriori informazioni sull'utilizzo di *. deploy* file, vedere [come: installare un pacchetto di distribuzione tramite il File deploy](https://msdn.microsoft.com/library/ff356104.aspx).


Il gestore di distribuzione Web fornisce un approccio utile per la distribuzione in ambienti di produzione basato sulla intranet, in cui l'accesso remoto al server è disponibile ma non sono credenziali di amministratore, ambienti host e gli ambienti di gestione temporanea.

Per un esempio end-to-end di uno scenario che utilizza l'approccio gestore distribuzione Web, vedere [Scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Utilizzando la distribuzione non in linea

In alcuni casi, non è possibile o fattibile distribuire applicazioni e il contenuto in un sito Web IIS da una posizione remota. Ad esempio, il computer di origine e di destinazione sia in reti isolate o segmenti di rete, o criterio firewall potrebbe non consentire l'accesso remoto.

In questi casi è possibile utilizzare comunque la creazione di pacchetti e la pubblicazione delle funzionalità di distribuzione Web; non appena possono essere utilizzati da una posizione remota. Al contrario, un amministratore nel server di destinazione deve copiare il pacchetto web sul server e importarlo tramite Gestione IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

L'approccio di distribuzione non in linea è in genere è utile negli ambienti di produzione con connessione Internet, in cui i server in una rete perimetrale possono limitata la connettività con i computer nella rete interna.

Per un esempio end-to-end di uno scenario che utilizza l'approccio di distribuzione non in linea, vedere [Scenario: configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla sintassi e le operazioni da riga di comando di distribuzione Web, vedere [riferimento della riga di comando di distribuzione Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Per ulteriori informazioni sull'utilizzo di *. deploy* file, vedere [come: installare un pacchetto di distribuzione tramite il File deploy](https://msdn.microsoft.com/library/ff356104.aspx).

Per istruzioni generali sulle diverse modalità in cui è possibile distribuire i pacchetti web da un computer remoto, vedere [tramite distribuzione Web in remoto](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Per ulteriori informazioni sull'utilizzo di distribuzione Web su richiesta, vedere [distribuzione Web su richiesta](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Precedente](configuring-server-environments-for-web-deployment.md)
> [Successivo](scenario-configuring-a-test-environment-for-web-deployment.md)
