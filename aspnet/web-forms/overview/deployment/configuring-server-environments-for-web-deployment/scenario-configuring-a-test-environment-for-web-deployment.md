---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scenario: Configurazione di un ambiente di Test per la distribuzione Web | Microsoft Docs'
author: jrjlee
description: In questo argomento viene descritto uno scenario di distribuzione web tipica per sviluppatori o ambienti di test e illustra le attività da completare per impostare un intervento di servizio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f8636fab82b63edab50fc13ae32f4dd536f133ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382494"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Scenario: Configurazione di un ambiente di Test per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto uno scenario di distribuzione web tipica per sviluppatori o ambienti di test e illustra le attività da completare per poter configurare un ambiente simile.


Quando gli sviluppatori di lavorano alle applicazioni web, spesso assegnata loro l'accesso a un ambiente server che possono usare per testare le modifiche alle applicazioni in un'impostazione test realistica. Questo tipo di ambiente di sviluppo o test in genere presenta queste caratteristiche:

- L'ambiente è costituito da un singolo server web e un singolo server di database.
- Gli sviluppatori in genere hanno privilegi administrator per i server, per consentire loro di configurare l'ambiente per i requisiti delle applicazioni.
- Modifiche alle applicazioni distribuite in modo frequente, in modo che l'ambiente deve supportare passo a passo o la distribuzione automatica.

Ad esempio, nel nostro [scenario dell'esercitazione](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink è uno sviluppatore presso Fabrikam, Inc. Matt sta lavorando a della soluzione Contact Manager e regolarmente deve distribuire le modifiche in un ambiente di test. Matt è un amministratore nel server web di test e il server di database di test. Inizialmente, Matt deve essere in grado di distribuire la soluzione nell'ambiente di test direttamente.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Lavoro avanza e dagli sviluppatori Unisciti al team, gestione contatti soluzione è configurata per l'integrazione continua (CI) in Team Foundation Server (TFS). Ogni volta che uno sviluppatore archivia il contenuto, Team Build deve compilare la soluzione, eseguire tutti gli unit test e distribuire automaticamente la soluzione nell'ambiente di test.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Panoramica della soluzione

L'ambiente di test deve supportare solo passaggio oppure automatizzata la distribuzione da un computer remoto, in modo che è possibile scegliere di due approcci principali. È possibile:

- Configurare il server web di test per supportare la distribuzione usando il servizio agente di distribuzione Web (il "agente remoto").
- Configurare il server web di test per supportare la distribuzione usando il gestore di distribuzione Web.

> [!NOTE]
> È anche possibile usare [distribuzione Web su richiesta](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (il "agente temp"). È simile all'approccio dell'agente remoto in termini di requisiti e vincoli.


In questo caso, gli sviluppatori con privilegi di amministratore per i server di destinazione e l'ambiente di test non è soggetta a vincoli di sicurezza restrittivi, pertanto la scelta più ovvia consiste nel configurare il server web di test per supportare la distribuzione usando l'agente remoto. Questo è meno complesso e richiede la configurazione iniziale meno rispetto all'approccio gestore distribuzione Web. È anche necessario configurare il server di database per supportare la distribuzione e accesso remoto.

Questi argomenti includono tutte le informazioni necessarie per completare queste attività:

- [Configurare un Server Web per la pubblicazione (agente remoto) di distribuzione Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Questo argomento descrive come creare un server web che supporta la distribuzione Web di pubblicazione, usando l'approccio dell'agente remoto, a partire da una compilazione pulita di Windows Server 2008 R2.
- [Configurare un Server di Database per la pubblicazione con distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md). Questo argomento descrive come configurare un server di database per supportare l'accesso remoto e la distribuzione, a partire da un'installazione predefinita di SQL Server 2008 R2.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni sulla configurazione di un tipico ambiente di staging, vedere [Scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Per indicazioni su come configurare un ambiente di produzione tipici, vedere [Scenario: configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Precedente](choosing-the-right-approach-to-web-deployment.md)
> [Successivo](scenario-configuring-a-staging-environment-for-web-deployment.md)
