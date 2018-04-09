---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Configurazione degli ambienti di Server per la distribuzione Web | Documenti Microsoft
author: jrjlee
description: In questa esercitazione viene illustrato come configurare gli ambienti server a supporto di un solo clic o automatizzato, distribuzione del sito Web e la pubblicazione in vari dello scenario diverse...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ff6118be618a170ac76d66a9de24a7b5cc2d840a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="configuring-server-environments-for-web-deployment"></a>Configurazione degli ambienti di Server per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questa esercitazione Mostra come configurare gli ambienti server a un solo clic, o automatizzati, distribuzione del sito Web e la pubblicazione in vari scenari diversi. L'esercitazione include gli argomenti che consentono di completare varie attività, ad esempio la configurazione di un server web per approcci specifici per la distribuzione e configurazione di una server farm Web Farm Framework (WFF), insieme a panoramiche basata su scenario che forniscono supporto istruzioni di livello superiore end-to-end.
> 
> L'esercitazione Usa lo scenario di distribuzione di Fabrikam, Inc. descritto in [distribuzione Web aziendale: panoramica dello Scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) come punto di riferimento per gli esempi e l'infrastruttura di rete.
> 
> Per una traduzione italiana di queste esercitazioni, visitare [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


In questa esercitazione include questi argomenti:

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

Il primo argomento, [scelta dell'approccio di destra per distribuzione Web](choosing-the-right-approach-to-web-deployment.md), vengono descritti gli approcci principali, è possibile utilizzare per pubblicare applicazioni web tramite lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) 2.0. Vengono inoltre identificati gli scenari in cui eseguire il mapping a ciascun approccio. Da qui, ogni argomento di scenario viene fornita una panoramica delle attività da completare e identifica gli argomenti in cui che è necessario per completare queste attività di lavoro tramite.

Se si utilizza l'approccio di file di progetto split descritto in [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md) per compilare e distribuire la soluzione, l'argomento finale, [configurazione delle proprietà di distribuzione per un ambiente di destinazione](configuring-deployment-properties-for-a-target-environment.md), viene descritto come configurare i file di progetto specifici dell'ambiente per la distribuzione in ambienti di destinazione diversi.

## <a name="key-technologies"></a>Tecnologie principali

In questa esercitazione viene illustrato come utilizzare questi prodotti e tecnologie per supportare la distribuzione web:

- IIS 7.5
- 2. x di distribuzione Web
- WFF 2.x
- Servizio di gestione IIS Web (WMSvc)

L'esercitazione interessa anche l'utilizzo di ASP.NET MVC 3, SQL Server 2008 R2, ASP.NET 4.0 e Windows Server 2008 R2.

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Questo fa parte di una serie di cinque esercitazioni su distribuzione web su larga scala. Ecco le altre esercitazioni nella serie:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo di scelta rapida per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e procedure dettagliate descritte in tutta la serie rientrano in un processo più ampio di Application Lifecycle Management (ALM).
- [Distribuzione nell'organizzazione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). In questa esercitazione vengono introdotti i concetti relativi ai file di progetto di Microsoft Build Engine (MSBuild), la Pipeline di pubblicazione sul Web, distribuzione Web e altre tecnologie correlate. Spiega come è possibile utilizzare questi strumenti insieme per gestire i processi di distribuzione complessi.
- [Configurazione di Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In questa esercitazione viene descritto come configurare Team Foundation Server (TFS) per supportare diversi scenari di distribuzione, tra cui la distribuzione automatica come parte di un processo di integrazione continua (CI) e attivate manualmente le distribuzioni di compilazioni specifiche.
- [Distribuzione Web aziendale avanzate](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In questa esercitazione viene illustrato come eseguire diverse attività di distribuzione più avanzate, ad esempio personalizzare le distribuzioni di database per più ambienti, esclusione dalla distribuzione di file e cartelle e l'esecuzione di applicazioni web offline durante il processo di distribuzione .

> [!div class="step-by-step"]
> [avanti](choosing-the-right-approach-to-web-deployment.md)
