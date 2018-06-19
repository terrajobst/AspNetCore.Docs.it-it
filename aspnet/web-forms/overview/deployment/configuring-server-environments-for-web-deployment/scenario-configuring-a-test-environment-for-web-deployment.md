---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scenario: Configurazione di un ambiente di Test per la distribuzione Web | Documenti Microsoft'
author: jrjlee
description: In questo argomento viene descritto uno scenario di distribuzione web tipica per sviluppatori o ambienti di test e illustra le attività da completare per impostare un si...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2976be642815e715ac19bd9db34485cf5474cb32
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879855"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Scenario: Configurazione di un ambiente di Test per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento viene descritto uno scenario di distribuzione web tipica per sviluppatori o ambienti di test e illustra le attività da completare per configurare un ambiente simile.


Quando gli sviluppatori lavorano su applicazioni web, sono spesso è concesso l'accesso a un ambiente server che possono utilizzare per testare le modifiche alle applicazioni in un'impostazione di test realistici. In genere, questo tipo di ambiente di sviluppo o test abbia queste caratteristiche:

- L'ambiente è costituito da un singolo server web e un singolo server di database.
- Gli sviluppatori in genere privilegi dispongono amministratore per i server, che consentono di configurare l'ambiente per i requisiti delle applicazioni.
- Modifiche alle applicazioni vengono distribuite regolarmente, quindi l'ambiente deve supportare singolo passaggio o distribuzione automatizzata.

Ad esempio, nel nostro [scenario dell'esercitazione](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink è uno sviluppatore di Fabrikam, Inc. Matt stia utilizzando la soluzione di gestione di contatto e regolarmente deve distribuire le modifiche all'ambiente di test. Matt è un amministratore del server web di test e il server di database di test. Inizialmente, Matt deve essere in grado di distribuire la soluzione nell'ambiente di testing direttamente.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Come lavoro l'avanzamento e gli sviluppatori più join del team, il responsabile del contatto soluzione è configurata per l'integrazione continua (CI) in Team Foundation Server (TFS). Ogni volta che uno sviluppatore archivia nel contenuto, Team Build deve compilare la soluzione, eseguire unit test e distribuire automaticamente la soluzione nell'ambiente di testing.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Panoramica della soluzione

L'ambiente di test deve supportare passo a passo o automatici distribuzione da un computer remoto, pertanto è possibile scegliere di due approcci principali. È possibile:

- Configurare il server web di test per supportare la distribuzione utilizzando il servizio agente di distribuzione Web (il "agente remoto").
- Configurare il server web di test per supportare la distribuzione utilizzando il gestore distribuzione Web.

> [!NOTE]
> È anche possibile usare [distribuzione Web su richiesta](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (il "agente temporanea"). È simile a quella dell'agente remoto in termini di requisiti e vincoli.


In questo caso, gli sviluppatori di devono di privilegi di amministratore nei server di destinazione e l'ambiente di test non è soggetti a vincoli di sicurezza rigidi, pertanto la scelta logica consiste nel configurare il server web di test per supportare la distribuzione utilizzando l'agente remoto. Questo è meno complesso e richiede la configurazione iniziale inferiore rispetto a quella del gestore distribuzione Web. È inoltre necessario configurare il server di database per supportare la distribuzione e accesso remoto.

Questi argomenti includono tutte le informazioni necessarie per completare queste attività:

- [Configurare un Server Web per la pubblicazione (agente remoto) di distribuzione Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). In questo argomento viene descritto come creare un server web che supporta la distribuzione Web di pubblicazione, utilizzando l'approccio di agente remoto, a partire da una compilazione pulita di Windows Server 2008 R2.
- [Configurare un Server di Database per la pubblicazione di distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md). In questo argomento viene descritto come configurare un server di database per supportare la distribuzione, a partire da un'installazione predefinita di SQL Server 2008 R2 e accesso remoto.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla configurazione di un tipico ambiente di gestione temporanea, vedere [Scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Per ulteriori informazioni sulla configurazione di un ambiente di produzione tipico, vedere [Scenario: configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Precedente](choosing-the-right-approach-to-web-deployment.md)
> [Successivo](scenario-configuring-a-staging-environment-for-web-deployment.md)
