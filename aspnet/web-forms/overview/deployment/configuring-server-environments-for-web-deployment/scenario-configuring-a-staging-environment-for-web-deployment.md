---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Scenario: Configurazione di un ambiente di gestione temporanea per la distribuzione Web | Microsoft Docs'
author: jrjlee
description: In questo argomento viene descritto uno scenario di distribuzione web tipico per un ambiente di staging e illustra le attività da completare per configurare un ambiente simile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: fda4f056eebb77df3e93c63bdd4fabb071c0a0a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365227"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Scenario: Configurazione di un ambiente di gestione temporanea per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento descrive uno scenario di distribuzione web tipico per un ambiente di staging e illustra le attività da completare per poter configurare un ambiente simile.


Molte organizzazioni usano gli ambienti di staging per visualizzare in anteprima gli aggiornamenti di applicazioni web o siti Web. In questo modo gli utenti all'interno dell'organizzazione la possibilità di esplorare ed esaminare le nuove funzionalità o il contenuto prima che il sito "viene resa disponibile", o in altre parole viene distribuito in un ambiente di produzione. Ambiente di gestione temporanea è progettata per replicare l'ambiente di produzione più vicino possibile, per fornire un'anteprima realistica. Questo tipo di ambiente di gestione temporanea ha in genere queste caratteristiche:

- L'ambiente è costituito da più server web con bilanciamento del carico e uno o più server di database, spesso con clustering di failover e il mirroring del database.
- Le applicazioni possono essere distribuite manualmente dal team di sviluppo o automaticamente da un server Team Build.
- Gli utenti o account del processo che distribuiscono le applicazioni sono in genere non hanno privilegi di amministratore nei server di gestione temporanea.
- Modifiche alle applicazioni distribuite in modo frequente, in modo che l'ambiente deve supportare passo a passo o la distribuzione automatica.

> [!NOTE]
> Scalabilità orizzontale di una distribuzione di database tra più server esula dall'ambito di questa esercitazione. Per altre informazioni su quest'area, consultare [documentazione Online di SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Ad esempio, nel nostro [scenario dell'esercitazione](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) consente di gestire la soluzione Contact Manager. L'amministratore TFS, Rob Walters, ha creato una definizione di compilazione che consente agli sviluppatori di attivare una distribuzione in ambiente di gestione temporanea in base alle esigenze.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Si noti che nella maggior parte dei casi, non sarà necessariamente desiderato distribuire la build più recente per l'ambiente di gestione temporanea. Al contrario, si è molto più probabile che vuole distribuire una compilazione specifica che è già stato sottoposto a convalida e verifica nell'ambiente di test.

## <a name="solution-overview"></a>Panoramica della soluzione

In questo scenario, si può dedurre tali fact dall'analisi dei requisiti di distribuzione:

- L'account utente o processo che esegue la distribuzione non disporrà di privilegi di amministratore nel server di gestione temporanea, in modo che i server web di staging devono supportare la distribuzione senza privilegi di amministratore. Di conseguenza, è necessario configurare i server web di staging per l'uso del gestore di distribuzione Web piuttosto che l'agente remoto.
- L'ambiente di staging include più server web, ma deve supportare la distribuzione automatizzata o di un solo clic, quindi è necessario usare la Web Farm Framework (WFF) per creare una server farm. Con questo approccio, è possibile distribuire un'applicazione in un unico server web (il server primario) e WFF replicherà la distribuzione in tutti gli altri server web nell'ambiente di staging.
- L'account utente o processo che esegue la distribuzione deve disporre delle autorizzazioni per creare i database. Di conseguenza, è necessario aggiungere l'account per il **dbcreator** ruolo del server nel server di database, oltre a configurare il server di database per supportare l'accesso remoto e la distribuzione.

Questi argomenti includono tutte le informazioni necessarie per completare queste attività:

- [Creare una Server Farm con Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md). In questo argomento viene descritto come creare e configurare una server farm utilizzando WFF, in modo che i prodotti della piattaforma web e componenti, le impostazioni di configurazione e i siti Web e applicazioni vengono replicate tra più server web con bilanciamento del carico.
- [Configurare un Server Web per la pubblicazione con distribuzione Web (gestore di distribuzione Web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Questo argomento descrive come creare un server web che supporta la distribuzione Web di pubblicazione, usando l'approccio dell'agente remoto, a partire da una compilazione pulita di Windows Server 2008 R2.
- [Configurare un Server di Database per la pubblicazione con distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md). Questo argomento descrive come configurare un server di database per supportare l'accesso remoto e la distribuzione, a partire da un'installazione predefinita di SQL Server 2008 R2.

## <a name="further-reading"></a>Ulteriori informazioni

Per indicazioni su come configurare un ambiente di test tipico per gli sviluppatori, vedere [Scenario: configurazione di un ambiente di Test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md). Per indicazioni su come configurare un ambiente di produzione tipici, vedere [Scenario: configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Precedente](scenario-configuring-a-test-environment-for-web-deployment.md)
> [Successivo](scenario-configuring-a-production-environment-for-web-deployment.md)
