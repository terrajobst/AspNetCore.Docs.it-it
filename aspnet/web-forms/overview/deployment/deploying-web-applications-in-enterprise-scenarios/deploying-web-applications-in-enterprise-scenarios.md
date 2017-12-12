---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Distribuzione di applicazioni Web in scenari aziendali utilizzando Visual Studio 2010 | Documenti Microsoft
author: jrjlee
description: "Questo set di esercitazioni vengono descritti strumenti e tecniche che è possibile utilizzare per distribuire applicazioni web in diversi scenari aziendali. Viene spiegato come utilizzare meglio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 99bab371dd34b30f3554843e49bbec7f57c3f96c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Distribuzione di applicazioni Web in scenari aziendali utilizzando Visual Studio 2010
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo set di esercitazioni vengono descritti strumenti e tecniche che è possibile utilizzare per distribuire applicazioni web in diversi scenari aziendali. Viene spiegato come utilizzare più di tecnologie quali Visual Studio 2010, Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7.5, lo strumento di distribuzione Web di IIS (distribuzione Web), la Web Farm Framework (WFF) e utilità VSDBCMD.exe per semplificare e gestire il processo di distribuzione. Include una panoramica concettuale e informazioni aggiuntive orientate alle attività che consentirà di:
> 
> - Rivedere e stabilire i requisiti di distribuzione per un'applicazione web su larga scala.
> - Configurare gli ambienti server web test, gestione temporanea e produzione per supportare la distribuzione di web.
> - Configurare i processi di integrazione continua (CI) di Team Foundation Server (TFS) per supportare la distribuzione web automatizzati.
> - Distribuire le applicazioni web su larga scala per diversi ambienti server con diversi requisiti e restrizioni.
> - Distribuire le modifiche alle applicazioni web che sono in esecuzione in diversi ambienti server.
> 
> > [!NOTE]
> > Durante queste esercitazioni vengono descritti l'utilizzo di TFS, come un elemento di configurazione di server, il materiale sussidiario è facilmente adattato per qualsiasi elemento di configurazione di server. Non è necessaria una conoscenza approfondita di TFS per comprendere e utilizzare le esercitazioni.
> 
> 
> Per una traduzione italiana di queste esercitazioni, visitare [http://www.lucamorelli.it](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>Informazioni sugli autori

Jason Lee è un esperto principale con [contenuto Master](http://www.contentmaster.com/) in cui ha lavorato con prodotti e tecnologie, in particolare SharePoint e ASP.NET, Microsoft per diversi anni. Jason contiene il titolo di PhD nel calcolo ed è attualmente MCPD e MCTS Certificate. È possibile leggere i blog di documentazione tecnico di Jason all'indirizzo [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry è un esperto principale con [contenuto Master](http://www.contentmaster.com/) che ha scritto white paper, documentazione SDK, presentazioni di PowerPoint e corsi di formazione istruttore e online durante la sua carriera. Un membro originale del team della documentazione di ASP.NET, ha lavorato con tecnologie web di Microsoft per oltre un decennio.

## <a name="target-audience"></a>Destinatari

Il set di esercitazioni è per gli sviluppatori di applicazioni web ASP.NET e architetti di soluzioni che utilizzano Visual Studio 2010 per creare applicazioni web su larga scala. Per ottenere il maggior valore dal contenuto, è consigliabile una certa familiarità con Visual Studio 2010 e nozioni di base con TFS, insieme a conoscenza del tecnologie della piattaforma web Microsoft ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL Server e progetti di database di Visual Studio. Tuttavia, non è necessario avere familiarità con le tecnologie e strumenti di distribuzione o necessario sapere come configurare i sistemi di elemento di configurazione.

## <a name="requirements"></a>Requisiti

Per seguire le procedure dettagliate ed eseguire le attività che descrivono queste esercitazioni, è necessario installare il software nel computer di sviluppo:

- Visual Studio 2010 Premium o Ultimate Edition con Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 con Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Per eseguire i passaggi di distribuzione descritti in queste procedure dettagliate, è necessario disporre di accesso per gli ambienti di distribuzione dell'applicazione Web di esempio. Per ottenere risultati ottimali, questi ambienti devono riflettere modello di distribuzione aziendale dell'organizzazione. È quindi possibile modificare le procedure dettagliate fornite nella presente documentazione in modo da riflettere i requisiti dell'organizzazione e ambienti di distribuzione.

## <a name="series-contents"></a>Contenuto di serie

In questa sezione introduttiva è costituito da due ulteriori argomenti. Queste sono progettate per fornire il contesto più ampio per le esercitazioni che seguono:

- [Distribuzione Web aziendale: Panoramica dello Scenario](enterprise-web-deployment-scenario-overview.md). In questo argomento viene descritto lo scenario che stabili ognuna delle esercitazioni di questa serie. Lo scenario è incentrato sui requisiti di Application Lifecycle Management (ALM) di una società fittizia denominata Fabrikam, Inc. come si sviluppa un'applicazione web su larga scala.
- [Gestione del ciclo di vita delle applicazioni: Dallo sviluppo alla produzione](application-lifecycle-management-from-development-to-production.md). In questo argomento fornisce una panoramica di alto livello, end-to-end di un processo di distribuzione. Viene illustrato come Fabrikam, Inc. si sposta un'applicazione web ASP.NET su larga scala tramite ambienti di test, gestione temporanea e produzione come parte di un processo di sviluppo continuo.

La serie include quattro set di esercitazione. Ogni è incentrata sui diversi aspetti della distribuzione web:

- [Distribuzione nell'organizzazione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione viene fornita un'introduzione concettuale a file di progetto MSBuild, la Pipeline di pubblicazione sul Web, distribuzione Web e altre tecnologie correlate. Spiega come è possibile utilizzare questi strumenti insieme per gestire i processi di distribuzione complessi.
- [Configurazione degli ambienti di Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In questa esercitazione viene descritto come configurare i server di Windows per supportare diversi scenari di distribuzione, inclusa la distribuzione di pacchetto web remoto tramite il servizio agente di distribuzione Web (il "agente remoto") o il gestore distribuzione Web e la distribuzione del database remoto. Vengono fornite informazioni aggiuntive su come scegliere il metodo di distribuzione appropriata per il proprio ambiente e viene descritto come utilizzare il WFF per replicare le applicazioni web distribuite tra tutti i server web in una server farm.
- [Configurazione di Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In questa esercitazione viene descritto come configurare TFS per supportare diversi scenari di distribuzione, tra cui la distribuzione automatica come parte di un processo di integrazione continua e attivate manualmente le distribuzioni di compilazioni specifiche.
- [Distribuzione Web aziendale avanzate](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In questa esercitazione viene illustrato come eseguire diverse attività di distribuzione più avanzate, ad esempio personalizzare le distribuzioni di database per più ambienti, esclusione dalla distribuzione di file e cartelle e l'esecuzione di applicazioni web offline durante il processo di distribuzione .

## <a name="where-to-start"></a>Dove iniziare

Questo set di esercitazioni Usa una soluzione di esempio con un livello di complessità, insieme a uno scenario di distribuzione dell'organizzazione fittizia, realistico per fornire un'implementazione di riferimento e per assegnare le attività e procedure dettagliate di un contesto comune. L'argomento successivo, [distribuzione Web aziendale: panoramica dello Scenario](enterprise-web-deployment-scenario-overview.md), illustra lo scenario e soluzione di esempio. Da qui è possibile utilizzare le esercitazioni e gli argomenti che meglio soddisfano le proprie esigenze.

>[!div class="step-by-step"]
[Successivo](enterprise-web-deployment-scenario-overview.md)
