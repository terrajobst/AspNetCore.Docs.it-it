---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Distribuzione di applicazioni Web in scenari aziendali tramite Visual Studio 2010 | Microsoft Docs
author: jrjlee
description: Questa serie di esercitazioni descrive strumenti e tecniche che è possibile utilizzare per distribuire le applicazioni web in diversi scenari aziendali. Viene spiegato come usare meglio...
ms.author: aspnetcontent
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: b55aeb863694ca3ac71f2a1a46d11981e178dcb4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816587"
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Distribuzione di applicazioni Web in scenari aziendali tramite Visual Studio 2010
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questa serie di esercitazioni descrive strumenti e tecniche che è possibile utilizzare per distribuire le applicazioni web in diversi scenari aziendali. Viene spiegato come usare meglio le tecnologie come Visual Studio 2010, Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7.5, lo strumento di distribuzione Web IIS (distribuzione Web), la Web Farm Framework (WFF) e utilità come VSDBCMD.exe a semplifica e gestire il processo di distribuzione. Include una panoramica concettuale e materiale sussidiario e orientati alle attività che consente di:
> 
> - Rivedere e stabilire i requisiti di distribuzione per un'applicazione web su larga scala.
> - Configurare gli ambienti server web test, staging e produzione per supportare la distribuzione di web.
> - Configurare Team Foundation Server (TFS) i processi di integrazione continua (CI) per supportare la distribuzione web automatizzati.
> - Distribuire applicazioni web su larga scala per ambienti server diversi con diversi requisiti e restrizioni.
> - Distribuire le modifiche alle applicazioni web che sono in esecuzione in diversi ambienti server.
> 
> > [!NOTE]
> > Anche se queste esercitazioni viene illustrato l'utilizzo di TFS come server di integrazione continua, il materiale sussidiario è facilmente adattato a qualsiasi server CI. Non è necessario una conoscenza approfondita di TFS di capire e utilizzare le esercitazioni.
> 
> 
> Per una traduzione italiana di queste esercitazioni, visitare [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>Informazioni sugli autori

Jason Lee è un principal technologist presso [Content Master](http://www.contentmaster.com/) dove ha lavorato con prodotti e tecnologie, soprattutto SharePoint e ASP.NET, Microsoft per diversi anni. Jason contiene il titolo di PhD in elaborazione ed è attualmente MCPD e MCTS certificata. È possibile leggere blog tecnico di Jason all'indirizzo [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry è un principal technologist presso [Content Master](http://www.contentmaster.com/) chi ha scritto white paper, documentazione di SDK, le presentazioni di PowerPoint e corsi di formazione tenuti da istruttori e online durante la sua carriera. Un membro originale del team della documentazione di ASP.NET, ha lavorato con le tecnologie web di Microsoft per oltre un decennio.

## <a name="target-audience"></a>Destinatari

Questa serie di esercitazioni è per gli sviluppatori di applicazioni web ASP.NET e gli architetti di soluzioni che usano Visual Studio 2010 per creare applicazioni web su larga scala. Per ottenere il massimo dal contenuto, è necessario essere a proprio agio usando Visual Studio 2010 e prevedono una certa familiarità con TFS, insieme a una conoscenza delle tecnologie della piattaforma web Microsoft, ad esempio ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL Server e i progetti di database di Visual Studio. Tuttavia, non occorre avere familiarità con le tecnologie e strumenti di distribuzione o necessario sapere come configurare i sistemi di integrazione continua.

## <a name="requirements"></a>Requisiti

Per seguire le procedure dettagliate ed eseguire le attività che descrivono queste esercitazioni, è necessario installare il software nel computer di sviluppo:

- Visual Studio 2010 Premium o Ultimate Edition con Service Pack 1
- .NET Framework 4.0
- .NET framework 3.5 con Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Per eseguire i passaggi di distribuzione descritti in queste procedure dettagliate, è necessario avere accesso per gli ambienti di distribuzione dell'applicazione Web di esempio. Per ottenere risultati ottimali, questi ambienti devono riflettere modello di distribuzione aziendale dell'organizzazione. È quindi possibile modificare le procedure dettagliate fornite nella presente documentazione in modo da riflettere i requisiti dell'organizzazione e gli ambienti di distribuzione.

## <a name="series-contents"></a>Contenuto di serie

In questa sezione introduttiva è costituito da due ulteriori argomenti. Queste sono progettate per fornire il contesto più ampio per le esercitazioni che seguono:

- [Distribuzione Web aziendale: Panoramica dello Scenario](enterprise-web-deployment-scenario-overview.md). In questo argomento viene descritto lo scenario che supporta ciascuna delle esercitazioni di questa serie. Lo scenario è incentrato sui requisiti di Application Lifecycle Management (ALM) di una società fittizia, denominata Fabrikam, Inc. come sviluppa un'applicazione web su larga scala.
- [Application Lifecycle Management: Dallo sviluppo alla produzione](application-lifecycle-management-from-development-to-production.md). In questo argomento fornisce una panoramica di alto livello, end-to-end di un processo di distribuzione. Viene illustrato come Fabrikam, Inc. consente di spostare un'applicazione web ASP.NET su larga scala tramite gli ambienti di test, staging e produzione come parte di un processo di sviluppo continuo.

La serie include quattro set di esercitazioni. Ognuno è incentrato su diversi aspetti della distribuzione web:

- [Distribuzione nell'organizzazione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione offre un'introduzione concettuale a file di progetto MSBuild, la Pipeline di pubblicazione sul Web, distribuzione Web e altre tecnologie correlate. Viene spiegato come è possibile utilizzare questi strumenti insieme per gestire i processi di distribuzione complesse.
- [Configurazione degli ambienti Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Questa esercitazione descrive come configurare i server di Windows per supportare vari scenari di distribuzione, tra cui la distribuzione di pacchetto web remoto utilizzando il servizio agente di distribuzione Web (il "agente remoto") o il gestore di distribuzione Web e la distribuzione di database remoto. Vengono fornite istruzioni su come scegliere il metodo di distribuzione appropriate per il proprio ambiente e viene descritto come utilizzare il WFF per replicare applicazioni web distribuite tra tutti i server web in una server farm.
- [Configurazione di Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Questa esercitazione descrive come configurare TFS per supportare diversi scenari di distribuzione, tra cui la distribuzione automatizzata come parte di un processo di integrazione continua e attivata manualmente le distribuzioni di compilazioni specifiche.
- [Distribuzione Web aziendale avanzata](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Questa esercitazione descrive come eseguire varie attività di distribuzione più avanzate, come la personalizzazione delle distribuzioni di database per più ambienti, esclusione di file e cartelle dalla distribuzione e portare offline l'applicazioni web durante il processo di distribuzione .

## <a name="where-to-start"></a>Dove iniziare

Questa serie di esercitazioni pratiche Usa una soluzione di esempio con un livello di complessità, insieme a uno scenario di distribuzione azienda fittizia, realistico per fornire un'implementazione di riferimento e per assegnare le attività e procedure dettagliate di contesto comune. Argomento successivo [distribuzione Web aziendale: panoramica dello Scenario](enterprise-web-deployment-scenario-overview.md), illustra lo scenario e la soluzione di esempio. Da qui è possibile consultare le esercitazioni e gli argomenti che meglio soddisfano le proprie esigenze.

> [!div class="step-by-step"]
> [avanti](enterprise-web-deployment-scenario-overview.md)
