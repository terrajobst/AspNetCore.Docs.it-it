---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Scenario: Configurazione di un ambiente di produzione per la distribuzione Web | Microsoft Docs'
author: jrjlee
description: In questo argomento viene descritto uno scenario di distribuzione web tipico per un ambiente di produzione e illustra le attività da completare per impostare una simile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ff9a1e7657852f37b3dc4fc1dbc4f6e78e6427cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370348"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Scenario: Configurazione di un ambiente di produzione per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento descrive uno scenario di distribuzione web tipico per un ambiente di produzione e illustra le attività da completare per poter configurare un ambiente simile.


L'ambiente di produzione è la destinazione finale per un'applicazione web o un sito Web. A questo punto l'applicazione è stato sottoposto a test, è stato distribuito in un ambiente di staging e sia pronto per "go live". Le caratteristiche di un ambiente di produzione possono variare notevolmente in base alla natura e finalità del contenuto web, le dimensioni dell'organizzazione, i destinatari e molti altri fattori. In uno scenario di livello aziendale, l'ambiente di produzione può presentare queste caratteristiche:

- L'ambiente è costituito da più server web con bilanciamento del carico e uno o più server di database, spesso con clustering di failover e il mirroring del database.
- Se l'ambiente è connesso a Internet, è probabile che isolate dalla rete interna. Potrebbe essere in una subnet diversa in una rete perimetrale, potrebbe essere in un dominio diverso e potrebbe essere in un'infrastruttura di rete completamente diverso.
- Gli sviluppatori e gli account di processo server di compilazione sono altamente improbabile che dispone dei privilegi di amministratore nel server di produzione.
- Modifiche alle applicazioni vengono distribuite in modo meno frequente di test o le distribuzioni di gestione temporanea.

> [!NOTE]
> Scalabilità orizzontale di una distribuzione di database tra più server esula dall'ambito di questa esercitazione. Per altre informazioni su quest'area, consultare [documentazione Online di SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Ad esempio, nel nostro [scenario dell'esercitazione](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), un server Team Build include definizioni di compilazione che consentono agli utenti di compilare la soluzione Contact Manager e distribuirla in un ambiente di staging in un unico passaggio. Quando l'applicazione è pronta per essere distribuita nell'ambiente di produzione, a causa di vincoli imposti dai requisiti di sicurezza e l'infrastruttura di rete, l'amministratore di ambiente di produzione è necessario copiare il pacchetto web a un server web di produzione e importare manualmente esso tramite Gestione Internet Information Services (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Panoramica della soluzione

In questo scenario, si può dedurre tali fact dall'analisi dei requisiti di distribuzione:

- A causa di restrizioni di sicurezza e la configurazione di rete, è possibile configurare l'ambiente di produzione per supportare la distribuzione automatizzata o di un solo clic. Distribuzione offline è l'unico approccio possibile in questo scenario.
- L'ambiente di produzione include più server web, quindi è possibile usare la Web Farm Framework (WFF) per creare una server farm. In questo modo, solo l'amministratore dovrà importare l'applicazione in un unico server web (il server primario) e WFF replicherà la distribuzione in tutti gli altri server web nell'ambiente di produzione.

Questi argomenti includono tutte le informazioni necessarie per completare queste attività:

- [Creare una Server Farm con Web Farm Framework](configuring-a-database-server-for-web-deploy-publishing.md). In questo argomento viene descritto come creare e configurare una server farm utilizzando WFF, in modo che i prodotti della piattaforma web e componenti, le impostazioni di configurazione e i siti Web e applicazioni vengono replicate tra più server web con bilanciamento del carico.
- [Configurare un Server Web per la pubblicazione (distribuzione Offline) con distribuzione Web](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Questo argomento descrive come creare un server web che consente agli amministratori importare e distribuire i pacchetti web manualmente, a partire da una compilazione pulita di Windows Server 2008 R2.
- [Configurare un Server di Database per la pubblicazione con distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md). Questo argomento descrive come configurare un server di database per supportare l'accesso remoto e la distribuzione, a partire da un'installazione predefinita di SQL Server 2008 R2.

## <a name="further-reading"></a>Ulteriori informazioni

Per indicazioni su come configurare un ambiente di test tipico per gli sviluppatori, vedere [Scenario: configurazione di un ambiente di Test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md). Per istruzioni sulla configurazione di un tipico ambiente di staging, vedere [Scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Precedente](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Successivo](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
