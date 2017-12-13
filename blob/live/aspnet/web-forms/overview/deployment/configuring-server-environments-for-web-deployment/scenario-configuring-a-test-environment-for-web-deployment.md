---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scenario: Configurazione di un ambiente di Test per la distribuzione Web | Documenti Microsoft'
author: jrjlee
description: "In questo argomento viene descritto uno scenario di distribuzione web tipica per sviluppatori o ambienti di test e illustra le attività da completare per impostare un si..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 008b9cd081152e6a378d0fa2e08497a6771fd9b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="370f2-103">Scenario: Configurazione di un ambiente di Test per la distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="370f2-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="370f2-104">da [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="370f2-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="370f2-105">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="370f2-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="370f2-106">Questo argomento viene descritto uno scenario di distribuzione web tipica per sviluppatori o ambienti di test e illustra le attività da completare per configurare un ambiente simile.</span><span class="sxs-lookup"><span data-stu-id="370f2-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="370f2-107">Quando gli sviluppatori lavorano su applicazioni web, sono spesso è concesso l'accesso a un ambiente server che possono utilizzare per testare le modifiche alle applicazioni in un'impostazione di test realistici.</span><span class="sxs-lookup"><span data-stu-id="370f2-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="370f2-108">In genere, questo tipo di ambiente di sviluppo o test abbia queste caratteristiche:</span><span class="sxs-lookup"><span data-stu-id="370f2-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="370f2-109">L'ambiente è costituito da un singolo server web e un singolo server di database.</span><span class="sxs-lookup"><span data-stu-id="370f2-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="370f2-110">Gli sviluppatori in genere privilegi dispongono amministratore per i server, che consentono di configurare l'ambiente per i requisiti delle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="370f2-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="370f2-111">Modifiche alle applicazioni vengono distribuite regolarmente, quindi l'ambiente deve supportare singolo passaggio o distribuzione automatizzata.</span><span class="sxs-lookup"><span data-stu-id="370f2-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="370f2-112">Ad esempio, nel nostro [scenario dell'esercitazione](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink è uno sviluppatore di Fabrikam, Inc. Matt stia utilizzando la soluzione di gestione di contatto e regolarmente deve distribuire le modifiche all'ambiente di test.</span><span class="sxs-lookup"><span data-stu-id="370f2-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="370f2-113">Matt è un amministratore del server web di test e il server di database di test.</span><span class="sxs-lookup"><span data-stu-id="370f2-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="370f2-114">Inizialmente, Matt deve essere in grado di distribuire la soluzione nell'ambiente di testing direttamente.</span><span class="sxs-lookup"><span data-stu-id="370f2-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="370f2-115">Come lavoro l'avanzamento e gli sviluppatori più join del team, il responsabile del contatto soluzione è configurata per l'integrazione continua (CI) in Team Foundation Server (TFS).</span><span class="sxs-lookup"><span data-stu-id="370f2-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="370f2-116">Ogni volta che uno sviluppatore archivia nel contenuto, Team Build deve compilare la soluzione, eseguire unit test e distribuire automaticamente la soluzione nell'ambiente di testing.</span><span class="sxs-lookup"><span data-stu-id="370f2-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="370f2-117">Panoramica della soluzione</span><span class="sxs-lookup"><span data-stu-id="370f2-117">Solution Overview</span></span>

<span data-ttu-id="370f2-118">L'ambiente di test deve supportare passo a passo o automatici distribuzione da un computer remoto, pertanto è possibile scegliere di due approcci principali.</span><span class="sxs-lookup"><span data-stu-id="370f2-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="370f2-119">È possibile:</span><span class="sxs-lookup"><span data-stu-id="370f2-119">You can:</span></span>

- <span data-ttu-id="370f2-120">Configurare il server web di test per supportare la distribuzione utilizzando il servizio agente di distribuzione Web (il "agente remoto").</span><span class="sxs-lookup"><span data-stu-id="370f2-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="370f2-121">Configurare il server web di test per supportare la distribuzione utilizzando il gestore distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="370f2-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="370f2-122">È anche possibile usare [distribuzione Web su richiesta](https://technet.microsoft.com/en-us/library/ee517345(WS.10).aspx) (il "agente temporanea").</span><span class="sxs-lookup"><span data-stu-id="370f2-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/en-us/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="370f2-123">È simile a quella dell'agente remoto in termini di requisiti e vincoli.</span><span class="sxs-lookup"><span data-stu-id="370f2-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="370f2-124">In questo caso, gli sviluppatori di devono di privilegi di amministratore nei server di destinazione e l'ambiente di test non è soggetti a vincoli di sicurezza rigidi, pertanto la scelta logica consiste nel configurare il server web di test per supportare la distribuzione utilizzando l'agente remoto.</span><span class="sxs-lookup"><span data-stu-id="370f2-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="370f2-125">Questo è meno complesso e richiede la configurazione iniziale inferiore rispetto a quella del gestore distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="370f2-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="370f2-126">È inoltre necessario configurare il server di database per supportare la distribuzione e accesso remoto.</span><span class="sxs-lookup"><span data-stu-id="370f2-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="370f2-127">Questi argomenti includono tutte le informazioni necessarie per completare queste attività:</span><span class="sxs-lookup"><span data-stu-id="370f2-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="370f2-128">[Configurare un Server Web per la pubblicazione (agente remoto) di distribuzione Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="370f2-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="370f2-129">In questo argomento viene descritto come creare un server web che supporta la distribuzione Web di pubblicazione, utilizzando l'approccio di agente remoto, a partire da una compilazione pulita di Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="370f2-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="370f2-130">[Configurare un Server di Database per la pubblicazione di distribuzione Web](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="370f2-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="370f2-131">In questo argomento viene descritto come configurare un server di database per supportare la distribuzione, a partire da un'installazione predefinita di SQL Server 2008 R2 e accesso remoto.</span><span class="sxs-lookup"><span data-stu-id="370f2-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="370f2-132">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="370f2-132">Further Reading</span></span>

<span data-ttu-id="370f2-133">Per ulteriori informazioni sulla configurazione di un tipico ambiente di gestione temporanea, vedere [Scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="370f2-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="370f2-134">Per ulteriori informazioni sulla configurazione di un ambiente di produzione tipico, vedere [Scenario: configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="370f2-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="370f2-135">[Precedente](choosing-the-right-approach-to-web-deployment.md)
[Successivo](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="370f2-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
