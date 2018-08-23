---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Configurazione degli ambienti Server per la distribuzione Web | Microsoft Docs
author: jrjlee
description: Questa esercitazione illustrerà come configurare gli ambienti server supporta un solo clic, o automatizzato, distribuzione nel sito Web e pubblicazione dei vari dello scenario diverse...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4f6433f0b8a9ad3b3634c9bcd8d95015eebaa865
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835852"
---
<a name="configuring-server-environments-for-web-deployment"></a>Configurazione degli ambienti Server per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questa esercitazione illustrerà come configurare gli ambienti server a supporta un solo clic, o automatizzato, distribuzione nel sito Web e pubblicazione nei vari scenari diversi. L'esercitazione include gli argomenti che consentono di completare varie attività, come la configurazione di un server web per approcci specifici per la distribuzione e configurazione di una farm di server Web Farm Framework (WFF), insieme a panoramiche basati su scenari che forniscono supporto istruzioni di end-to-end di livello superiore.
> 
> L'esercitazione Usa lo scenario di distribuzione di Fabrikam, Inc. descritto nella [distribuzione Web aziendale: panoramica dello Scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) come punto di riferimento per gli esempi e l'infrastruttura di rete.
> 
> Per una traduzione italiana di queste esercitazioni, visitare [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Questa esercitazione include questi argomenti:

- [Scelta dell'approccio corretto per la distribuzione Web](choosing-the-right-approach-to-web-deployment.md)
- [Scenario: Configurazione di un ambiente di test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Scenario: Configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Scenario: Configurazione di un ambiente di produzione temporanea per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Configurazione di un server Web per la pubblicazione con Distribuzione Web (agente remoto)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Configurazione di un server Web per la pubblicazione con Distribuzione Web (gestore di Distribuzione Web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Configurazione di un server Web per la pubblicazione con Distribuzione Web (distribuzione offline)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Configurazione di un server di database per la pubblicazione con Distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md)
- [Creazione di una server farm con Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Configurazione delle proprietà di distribuzione per un ambiente di destinazione](configuring-deployment-properties-for-a-target-environment.md)

Il primo argomento, [scelta dell'approccio a destra per la distribuzione Web](choosing-the-right-approach-to-web-deployment.md), vengono descritti gli approcci principali, è possibile usare per pubblicare le applicazioni web tramite lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web) 2.0. Vengono inoltre identificati gli scenari che eseguono il mapping a ogni approccio. A questo punto, ogni argomento scenario fornisce una panoramica generale delle attività da completare e identifica gli argomenti, occorre usare per completare queste attività.

Se si usa l'approccio di file di progetto split descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md) per compilare e distribuire la soluzione, l'argomento finale, [configurazione le proprietà di distribuzione per un ambiente di destinazione](configuring-deployment-properties-for-a-target-environment.md), viene descritto come configurare i file di progetto specifici dell'ambiente per la distribuzione in ambienti di destinazione diversi.

## <a name="key-technologies"></a>Tecnologie chiave

Questa esercitazione è incentrata su come usare questi prodotti e tecnologie per supportare la distribuzione web:

- IIS 7.5
- Distribuzione Web 2.x
- WFF 2.x
- Servizio di gestione IIS Web (WMSvc)

L'esercitazione interessa anche all'uso di Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4.0 e ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Ciò fa parte di una serie di cinque esercitazioni su distribuzione web su larga scala. Ecco le altre esercitazioni della serie:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo contesto per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e procedure dettagliate descritte in tutta la serie rientrano in un processo più ampio di Application Lifecycle Management (ALM).
- [Distribuzione nell'organizzazione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione offre un'introduzione concettuale a file di progetto di Microsoft Build Engine (MSBuild), la Pipeline di pubblicazione sul Web, distribuzione Web e altre tecnologie correlate. Viene spiegato come è possibile utilizzare questi strumenti insieme per gestire i processi di distribuzione complesse.
- [Configurazione di Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Questa esercitazione descrive come configurare Team Foundation Server (TFS) per supportare diversi scenari di distribuzione, tra cui la distribuzione automatizzata come parte di un processo di integrazione continua (CI) e attivata manualmente le distribuzioni di compilazioni specifiche.
- [Distribuzione Web aziendale avanzata](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Questa esercitazione descrive come eseguire varie attività di distribuzione più avanzate, come la personalizzazione delle distribuzioni di database per più ambienti, esclusione di file e cartelle dalla distribuzione e portare offline l'applicazioni web durante il processo di distribuzione .

> [!div class="step-by-step"]
> [avanti](choosing-the-right-approach-to-web-deployment.md)
