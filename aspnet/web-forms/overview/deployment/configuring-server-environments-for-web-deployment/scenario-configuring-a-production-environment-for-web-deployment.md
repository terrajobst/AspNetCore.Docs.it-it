---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Scenario: Configurazione di un ambiente di produzione per la distribuzione Web | Documenti Microsoft'
author: jrjlee
description: In questo argomento viene descritto uno scenario di distribuzione web tipiche per un ambiente di produzione e vengono illustrate le attività da completare per impostare un simile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4de5b1f20f3adcb53765c7cb9765c0d90a80e677
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Scenario: Configurazione di un ambiente di produzione per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto uno scenario di distribuzione web tipiche per un ambiente di produzione e vengono illustrate le attività da completare per configurare un ambiente simile.


L'ambiente di produzione è la destinazione finale per un'applicazione web o un sito Web. A questo punto l'applicazione è stato sottoposto a test, è stato distribuito in un ambiente di gestione temporanea ed è pronto "in tempo reale." Le caratteristiche di un ambiente di produzione possono variare notevolmente a seconda della natura e lo scopo del contenuto web, le dimensioni dell'organizzazione, il gruppo di destinatari e molti altri fattori. In uno scenario su larga scala, l'ambiente di produzione può avere le seguenti caratteristiche:

- L'ambiente è costituito da più server web con bilanciamento del carico e uno o più server di database, spesso con clustering di failover e il mirroring del database.
- Se l'ambiente è rivolto a Internet, è probabile che verranno isolati dalla rete interna. Potrebbe essere in una subnet diversa in una rete perimetrale, potrebbe essere in un dominio diverso e potrebbe essere in un'infrastruttura di rete di completamente diverso.
- Gli sviluppatori e gli account di processo server di compilazione sono altamente improbabile che dispone dei privilegi di amministratore nel server di produzione.
- Modifiche alle applicazioni vengono distribuite in una frequenza inferiore rispetto a test o le distribuzioni di gestione temporanea.

> [!NOTE]
> Scalabilità orizzontale di una distribuzione di database tra più server non rientra nell'ambito di questa esercitazione. Per ulteriori informazioni su un'area, consultare [la documentazione Online di SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Ad esempio, nel nostro [scenario dell'esercitazione](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), un server Team Build include definizioni di compilazione che consentono di compilare la soluzione di gestione di contatto e averlo distribuito in un ambiente di gestione temporanea in un unico passaggio. Quando l'applicazione è pronta per essere distribuita nell'ambiente di produzione, a causa di vincoli imposti i requisiti di sicurezza e l'infrastruttura di rete, l'amministratore di ambiente di produzione è necessario copiare il pacchetto web in un server web di produzione e importare manualmente esso tramite Gestione Internet Information Services (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Panoramica della soluzione

In questo scenario, si possono dedurre questi fatti da un'analisi dei requisiti di distribuzione:

- A causa di restrizioni di sicurezza e la configurazione di rete, è possibile configurare l'ambiente di produzione per supportare la distribuzione automatica o di un solo clic. Distribuzione non in linea è l'unico approccio valido in questo scenario.
- L'ambiente di produzione include più server web, è possibile utilizzare la Web Farm Framework (WFF) per creare una server farm. Questo approccio, l'amministratore è sufficiente importare l'applicazione in un unico server web (il server primario) e WFF replicherà la distribuzione in tutti gli altri server web nell'ambiente di produzione.

Questi argomenti includono tutte le informazioni necessarie per completare queste attività:

- [Creare una Server Farm con Web Farm Framework](configuring-a-database-server-for-web-deploy-publishing.md). In questo argomento viene descritto come creare e configurare una server farm con WFF, in modo che i prodotti della piattaforma web e i componenti, le impostazioni di configurazione e i siti Web e applicazioni vengono replicate in più server web con bilanciamento del carico.
- [Configurare un Server Web per la pubblicazione (distribuzione Offline) di distribuzione Web](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). In questo argomento viene descritto come creare un server web che consente agli amministratori importare e distribuire i pacchetti web manualmente, a partire da una compilazione pulita di Windows Server 2008 R2.
- [Configurare un Server di Database per la pubblicazione di distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md). In questo argomento viene descritto come configurare un server di database per supportare la distribuzione, a partire da un'installazione predefinita di SQL Server 2008 R2 e accesso remoto.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla configurazione di un ambiente di test di sviluppo comuni, vedere [Scenario: configurazione di un ambiente di Test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md). Per ulteriori informazioni sulla configurazione di un tipico ambiente di gestione temporanea, vedere [Scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Precedente](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Successivo](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
