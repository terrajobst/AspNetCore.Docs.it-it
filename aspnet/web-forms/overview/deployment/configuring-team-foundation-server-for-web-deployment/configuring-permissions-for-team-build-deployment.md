---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Configurazione delle autorizzazioni per Team di distribuzione della compilazione | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come configurare le autorizzazioni per consentire al server di compilazione distribuire il contenuto al server web e server di database come parte di un automatizzati b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: cb3d013d69e36f97335ea31dd6e4997772ba2d8e
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
<a name="configuring-permissions-for-team-build-deployment"></a>Configurazione delle autorizzazioni per Team di distribuzione della compilazione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come configurare le autorizzazioni per consentire al server di compilazione distribuire il contenuto al server web e server di database come parte di un processo di compilazione automatizzato.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [soluzione responsabile contatto](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due progetti #x 2014; & file contenente una compilare le istruzioni che si applicano a ogni ambiente di destinazione e quella contenente impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

Quando si installa il servizio di compilazione 2010 Team Foundation Server (TFS), specificare l'identità con cui si desidera eseguire il servizio. Per impostazione predefinita, questo è l'account del servizio di rete. In alternativa, è possibile configurare il servizio di compilazione per l'esecuzione con un account di dominio.

Qualsiasi attività di distribuzione che richiedono l'autenticazione di Windows e che si intende automatizzare con Team Build, viene eseguito con l'identità del servizio di compilazione. Di conseguenza, è necessario concedere l'identità del servizio di compilazione le autorizzazioni necessarie per i server web e i server di database.

> [!NOTE]
> L'account del servizio di rete Usa l'account del computer per l'autenticazione in altri computer. Gli account computer assumono la forma * [nome dominio]\[nome macchina] ***$**& #x 2014, ad esempio **FABRIKAM\TFSBUILD$**. Di conseguenza, se il servizio di compilazione viene eseguita utilizzando l'identità del servizio di rete, è necessario concedere alcuna delle autorizzazioni necessarie per l'identità dell'account computer per il server di compilazione.


## <a name="configuring-web-server-permissions"></a>Configurazione delle autorizzazioni di Server Web

Come descritto in [scelta dell'approccio di destra per distribuzione Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), esistono due approcci principali, è possibile utilizzare se si desidera distribuire i pacchetti web in un server web remoto:

- Distribuire l'applicazione da una posizione remota includendo il *servizio agente distribuzione Web* (noto anche come l'agente remoto) nel server di destinazione.
- Distribuire l'applicazione da una posizione remota includendo il *Internet Information Services* (*IIS) gestore distribuzione Web* nel server di destinazione.

L'agente remoto presenta due limitazioni principali in questo caso:

- L'agente remoto supporta solo l'autenticazione NTLM. In altre parole, la distribuzione deve utilizzare l'identità del servizio di compilazione & #x 2014, è possibile rappresentare un altro account.
- Per utilizzare l'agente remoto, l'account che esegue la distribuzione deve essere un amministratore nel server di destinazione.

Insieme, queste due limitazioni rendono l'approccio di agente remoto indesiderabile per una distribuzione automatizzata Team Build. Per utilizzare questo approccio, è necessario rendere il servizio di compilazione di account di amministratore su tutti i server web di destinazione.

Al contrario, l'approccio gestore distribuzione Web offre diversi vantaggi:

- Il gestore di distribuzione Web supporta l'autenticazione di base su HTTPS, che consente di passare le credenziali di un account alternativo per lo strumento di distribuzione Web di IIS (distribuzione Web).
- È possibile configurare i server web di destinazione per consentire agli utenti non amministratori di distribuire il contenuto a specifici siti Web IIS utilizzando il gestore di distribuzione Web.

Di conseguenza, è preferibile chiaramente il gestore di distribuzione Web di destinazione quando si automatizza la distribuzione di pacchetti web da Team Build. Questa è la procedura consigliata:

1. Creare un account di dominio con privilegi limitati da usare per la distribuzione.
2. Configurare il gestore di distribuzione Web e concedere all'account le autorizzazioni necessarie per distribuire il contenuto a un sito Web IIS specifico, come descritto in [configurazione di un Server Web per distribuire pubblicazione sul Web (gestore di distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Richiamare distribuzione Web e il gestore di distribuzione Web di destinazione, utilizzando l'autenticazione di base e fornendo le credenziali dell'account di dominio è stato creato, per eseguire la distribuzione.

Nel [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , soluzione di esempio si specifica il tipo di autenticazione (base o NTLM), le credenziali di distribuzione Web e l'indirizzo dell'endpoint (agente remoto o il gestore di distribuzione Web) nel file di progetto specifici dell'ambiente. Questi valori vengono utilizzati per formulare ed eseguire un comando di distribuzione Web quando viene eseguito il file di progetto. Per ulteriori informazioni, vedere [distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Per ulteriori informazioni sulla configurazione Web distribuire gestore, inclusa la modalità di configurazione delle autorizzazioni, vedere [configurazione di un Server Web per distribuire pubblicazione sul Web (gestore di distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Per ulteriori informazioni su come configurare l'agente remoto, vedere [configurazione di un Server Web per distribuire pubblicazione sul Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Configurazione delle autorizzazioni di Server di Database

Per distribuire un database in SQL Server, è necessario:

- Creare un account di accesso per l'account di distribuzione nell'istanza di SQL Server.
- Concedere l'accesso **DBCreator** le autorizzazioni per l'istanza di SQL Server.
- Dopo la distribuzione iniziale, aggiungere l'account di accesso di **db\_proprietario** ruolo nel database di destinazione. Ciò è necessario quanto nelle distribuzioni successive, è si modifica un database esistente anziché creare un nuovo database.

È possibile eseguire l'autenticazione a un'istanza di SQL Server utilizzando l'autenticazione NTLM o autenticazione di SQL Server:

- Se si utilizza l'autenticazione NTLM, è necessario concedere le autorizzazioni descritte in precedenza per l'account del servizio di compilazione.
- Se si utilizza l'autenticazione di SQL Server, è necessario concedere le autorizzazioni descritte in precedenza per l'account di SQL Server. È inoltre necessario includere il nome utente di SQL Server e la password nella stringa di connessione che si utilizza per distribuire il database.

Per informazioni dettagliate su come configurare le autorizzazioni per la distribuzione del database, vedere [configurazione di un Server di Database per la pubblicazione di distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Conclusione

A questo punto, è necessario comprendere le autorizzazioni necessarie, insieme alle opzioni di autenticazione open, quando si automatizza le distribuzioni di applicazioni e database web da Team Build. È anche possibile implementare le autorizzazioni necessarie sul server web IIS e il server di database di SQL Server.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla configurazione degli ambienti server Windows per supportare la distribuzione remota, vedere [configurazione di ambienti Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

>[!div class="step-by-step"]
[Precedente](deploying-a-specific-build.md)
