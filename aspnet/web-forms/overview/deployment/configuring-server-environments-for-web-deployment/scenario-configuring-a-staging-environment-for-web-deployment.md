---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Scenario: Configurazione di un ambiente di gestione temporanea per la distribuzione Web | Documenti Microsoft'
author: jrjlee
description: "Questo argomento viene descritto uno scenario di distribuzione web tipiche per un ambiente di gestione temporanea e le attività da completare per impostare un simile env..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 683a0cf88225fee762e82925afe3785a2defd5bf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Scenario: Configurazione di un ambiente di gestione temporanea per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento viene descritto uno scenario di distribuzione web tipiche per un ambiente di gestione temporanea e le attività da completare per configurare un ambiente simile.


Molte organizzazioni usano gli ambienti di gestione temporanea per visualizzare in anteprima gli aggiornamenti di applicazioni web o siti Web. In questo modo gli utenti dell'organizzazione a esplorare ed esaminare le nuove funzionalità o il contenuto prima che il sito "attivazione" o in altre parole viene distribuito in un ambiente di produzione. Ambiente di gestione temporanea è progettato per replicare l'ambiente di produzione il più vicino possibile, per fornire un'anteprima realistica. Questo tipo di ambiente di gestione temporanea ha in genere queste caratteristiche:

- L'ambiente è costituito da più server web con bilanciamento del carico e uno o più server di database, spesso con clustering di failover e il mirroring del database.
- Le applicazioni possono essere distribuite manualmente da un team di sviluppo o automaticamente da un server Team Build.
- Gli utenti o account del processo che la distribuzione di applicazioni sono in genere non hanno privilegi di amministratore nei server di gestione temporanea.
- Modifiche alle applicazioni vengono distribuite regolarmente, quindi l'ambiente deve supportare singolo passaggio o distribuzione automatizzata.

> [!NOTE]
> Scalabilità orizzontale di una distribuzione di database tra più server non rientra nell'ambito di questa esercitazione. Per ulteriori informazioni su un'area, consultare [la documentazione Online di SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Ad esempio, nel nostro [scenario dell'esercitazione](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) consente di gestire la soluzione di gestione di contatto. L'amministratore TFS, Rob Walters, ha creato una definizione di compilazione che consente agli sviluppatori di attivare una distribuzione all'ambiente di gestione temporanea in base alle esigenze.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Si noti che nella maggior parte dei casi, non necessariamente vuoi distribuire la build più recente per l'ambiente di gestione temporanea. Al contrario, si è molto più probabile che per distribuire una compilazione specifica che è già sottoposte a livello di convalida e verifica nell'ambiente di test.

## <a name="solution-overview"></a>Panoramica della soluzione

In questo scenario, si possono dedurre questi fatti da un'analisi dei requisiti di distribuzione:

- L'account utente o processo che esegue la distribuzione non disporrà di privilegi di amministratore nel server di gestione temporanea, quindi i server web di gestione temporanea devono supportare la distribuzione senza privilegi di amministratore. Di conseguenza, è necessario configurare i server web di gestione temporanea per utilizzare il gestore di distribuzione Web, anziché l'agente remoto.
- L'ambiente di gestione temporanea include più server web, ma è necessaria supportare la distribuzione automatica o di un solo clic, pertanto sarà necessario utilizzare la Web Farm Framework (WFF) per creare una server farm. Questo approccio, è possibile distribuire un'applicazione in un unico server web (il server primario) e WFF replicherà la distribuzione in tutti gli altri server web nell'ambiente di gestione temporanea.
- L'account utente o processo che esegue la distribuzione deve disporre delle autorizzazioni per creare database. Di conseguenza, è necessario aggiungere l'account di **dbcreator** ruolo del server nel server di database, oltre a configurare il server di database per supportare la distribuzione e accesso remoto.

Questi argomenti includono tutte le informazioni necessarie per completare queste attività:

- [Creare una Server Farm con Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md). In questo argomento viene descritto come creare e configurare una server farm con WFF, in modo che i prodotti della piattaforma web e i componenti, le impostazioni di configurazione e i siti Web e applicazioni vengono replicate in più server web con bilanciamento del carico.
- [Configurare un Server Web per la pubblicazione di distribuzione Web (gestore distribuzione Web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). In questo argomento viene descritto come creare un server web che supporta la distribuzione Web di pubblicazione, utilizzando l'approccio di agente remoto, a partire da una compilazione pulita di Windows Server 2008 R2.
- [Configurare un Server di Database per la pubblicazione di distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md). In questo argomento viene descritto come configurare un server di database per supportare la distribuzione, a partire da un'installazione predefinita di SQL Server 2008 R2 e accesso remoto.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla configurazione di un ambiente di test di sviluppo comuni, vedere [Scenario: configurazione di un ambiente di Test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md). Per ulteriori informazioni sulla configurazione di un ambiente di produzione tipico, vedere [Scenario: configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).

>[!div class="step-by-step"]
[Precedente](scenario-configuring-a-test-environment-for-web-deployment.md)
[Successivo](scenario-configuring-a-production-environment-for-web-deployment.md)
