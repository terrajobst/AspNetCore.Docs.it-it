---
title: DevOps con ASP.NET Core e Azure
author: CamSoper
description: Questa guida include informazioni complete sulla creazione di una pipeline DevOps per un'app ASP.NET Core ospitata in Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722538"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="3a6e8-103">DevOps con ASP.NET Core e Azure</span><span class="sxs-lookup"><span data-stu-id="3a6e8-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="3a6e8-104">Benvenuti alla guida del ciclo di sviluppo di Azure per .NET.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-104">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="3a6e8-105">Questa guida presenta i concetti di base della creazione di un ciclo di sviluppo basato su Azure usando processi e strumenti di .NET.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-105">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="3a6e8-106">Dopo aver completato questa guida, è possibile sfruttare i vantaggi di una toolchain DevOps completa.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-106">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="3a6e8-107">A chi è destinata questa guida</span><span class="sxs-lookup"><span data-stu-id="3a6e8-107">Who this guide is for</span></span>

<span data-ttu-id="3a6e8-108">Questa guida è destinata a sviluppatori ASP.NET esperti (livello 200-300).</span><span class="sxs-lookup"><span data-stu-id="3a6e8-108">You should be an experienced ASP.NET developer (200-300 level).</span></span> <span data-ttu-id="3a6e8-109">Non è necessario conoscere Azure, che viene illustrato in questa introduzione.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-109">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="3a6e8-110">La guida può essere utile anche per i tecnici di DevOps che si occupano di operazioni più che di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-110">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="3a6e8-111">Questa guida è destinata agli sviluppatori per Windows.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-111">This guide targets Windows developers.</span></span> <span data-ttu-id="3a6e8-112">Tuttavia .NET Core dispone di supporto completo per Linux e macOS.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-112">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="3a6e8-113">Per adattare la guida a Linux/macOS, vedere i callout che indicano le differenze per Linux/macOS.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-113">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="3a6e8-114">Argomenti non trattati dalla guida</span><span class="sxs-lookup"><span data-stu-id="3a6e8-114">What this guide doesn't cover</span></span>

<span data-ttu-id="3a6e8-115">Questa guida è incentrata su un'esperienza di distribuzione continua end-to-end per gli sviluppatori .NET.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-115">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="3a6e8-116">Non è una guida completa per tutti gli aspetti di Azure e non illustra nel dettaglio le API .NET per i servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-116">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="3a6e8-117">Il contenuto riguarda l'integrazione continua, la distribuzione, il monitoraggio e il debug.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-117">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="3a6e8-118">Verso la fine della guida vengono inclusi suggerimenti per le fasi successive.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-118">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="3a6e8-119">I suggerimenti includono servizi della piattaforma Azure utili per gli sviluppatori ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-119">Included in the suggestions are Azure platform services that are useful to ASP.NET developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="3a6e8-120">Contenuto della Guida</span><span class="sxs-lookup"><span data-stu-id="3a6e8-120">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="3a6e8-121">Strumenti e download</span><span class="sxs-lookup"><span data-stu-id="3a6e8-121">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="3a6e8-122">Informazioni su come ottenere gli strumenti usati nella guida.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-122">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="3a6e8-123">Distribuire nel servizio app</span><span class="sxs-lookup"><span data-stu-id="3a6e8-123">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="3a6e8-124">Informazioni sui diversi metodi per distribuire un'app ASP.NET Core nel servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-124">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="3a6e8-125">Integrazione e distribuzione continua</span><span class="sxs-lookup"><span data-stu-id="3a6e8-125">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="3a6e8-126">Compilazione di una soluzione end-to-end di integrazione e distribuzione continua per l'app ASP.NET Core con GitHub, VSTS e Azure.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-126">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, VSTS, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="3a6e8-127">Monitoraggio e debug</span><span class="sxs-lookup"><span data-stu-id="3a6e8-127">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="3a6e8-128">Informazioni sull'uso degli strumenti di Azure per eseguire il monitoraggio, risolvere i problemi e ottimizzare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-128">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="3a6e8-129">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="3a6e8-129">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="3a6e8-130">Altri percorsi di apprendimento per gli sviluppatori ASP.NET Core che si avvicinano ad Azure.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-130">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="3a6e8-131">Ringraziamenti</span><span class="sxs-lookup"><span data-stu-id="3a6e8-131">Acknowledgments</span></span>

<span data-ttu-id="3a6e8-132">Grazie a tutti gli utenti della community .NET che hanno contribuito a questa guida con suggerimenti utili.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-132">Thank you to everyone in the .NET community who contributed to this guide with helpful suggestions!</span></span> <span data-ttu-id="3a6e8-133">Un ringraziamento speciale ai seguenti membri della community, che hanno contribuito alla revisione finale di questo materiale:</span><span class="sxs-lookup"><span data-stu-id="3a6e8-133">We'd like to specifically thank the following community members who contributed to the final review of this material:</span></span>

* [<span data-ttu-id="3a6e8-134">Sam Wronski</span><span class="sxs-lookup"><span data-stu-id="3a6e8-134">Sam Wronski</span></span>](https://www.youtube.com/c/worldofzerodevelopment)
* [<span data-ttu-id="3a6e8-135">Jeffrey Palermo</span><span class="sxs-lookup"><span data-stu-id="3a6e8-135">Jeffrey Palermo</span></span>](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a><span data-ttu-id="3a6e8-136">Conclusione</span><span class="sxs-lookup"><span data-stu-id="3a6e8-136">Conclusion</span></span>

<span data-ttu-id="3a6e8-137">Questa guida illustra la creazione di un ciclo di sviluppo a integrazione continua basato su ASP.NET Core e sul servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="3a6e8-137">This guide prepares you to build a continuous integration development lifecycle built around ASP.NET Core and Azure App Service.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="3a6e8-138">Altre informazioni</span><span class="sxs-lookup"><span data-stu-id="3a6e8-138">Additional reading</span></span>

* [<span data-ttu-id="3a6e8-139">Che cos'è il cloud computing?</span><span class="sxs-lookup"><span data-stu-id="3a6e8-139">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="3a6e8-140">Esempi di Cloud Computing</span><span class="sxs-lookup"><span data-stu-id="3a6e8-140">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="3a6e8-141">Che cos'è IaaS?</span><span class="sxs-lookup"><span data-stu-id="3a6e8-141">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="3a6e8-142">Che cos'è PaaS?</span><span class="sxs-lookup"><span data-stu-id="3a6e8-142">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
