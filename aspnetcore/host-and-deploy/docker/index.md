---
title: Eseguire l'hosting di ASP.NET Core nei contenitori Docker
author: rick-anderson
description: Collegamenti alle risorse che illustrano come eseguire l'hosting di app ASP.NET Core nei contenitori Docker.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/index
ms.openlocfilehash: 12a179287ec302994380e0faf4b843596f8c2f4e
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2018
ms.locfileid: "30280095"
---
# <a name="host-aspnet-core-in-docker-containers"></a><span data-ttu-id="82cf2-103">Eseguire l'hosting di ASP.NET Core nei contenitori Docker</span><span class="sxs-lookup"><span data-stu-id="82cf2-103">Host ASP.NET Core in Docker containers</span></span>

<span data-ttu-id="82cf2-104">Per apprendere come eseguire l'hosting di app ASP.NET Core in Docker, leggere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="82cf2-104">The following articles are available for learning about hosting ASP.NET Core apps in Docker:</span></span>

[<span data-ttu-id="82cf2-105">Introduzione a contenitori e Docker</span><span class="sxs-lookup"><span data-stu-id="82cf2-105">Introduction to Containers and Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
<span data-ttu-id="82cf2-106">Descrive la containerizzazione, un approccio allo sviluppo del software in cui un'applicazione o un servizio, le relative dipendenze e la configurazione corrispondente sono inclusi in uno stesso pacchetto come immagine del contenitore.</span><span class="sxs-lookup"><span data-stu-id="82cf2-106">See how containerization is an approach to software development in which an application or service, its dependencies, and its configuration are packaged together as a container image.</span></span> <span data-ttu-id="82cf2-107">L'immagine può essere testata e quindi distribuita in un host.</span><span class="sxs-lookup"><span data-stu-id="82cf2-107">The image can be tested and then deployed to a host.</span></span>

[<span data-ttu-id="82cf2-108">Che cos'è Docker?</span><span class="sxs-lookup"><span data-stu-id="82cf2-108">What is Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
<span data-ttu-id="82cf2-109">Informazioni su Docker, un progetto open source per automatizzare la distribuzione di app come contenitori portabili e autosufficienti eseguibili nel cloud o in locale.</span><span class="sxs-lookup"><span data-stu-id="82cf2-109">Discover how Docker is an open-source project for automating the deployment of apps as portable, self-sufficient containers that can run on the cloud or on-premises.</span></span>

[<span data-ttu-id="82cf2-110">Terminologia di Docker</span><span class="sxs-lookup"><span data-stu-id="82cf2-110">Docker Terminology</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
<span data-ttu-id="82cf2-111">Informazioni su termini e definizioni relativi alla tecnologia Docker.</span><span class="sxs-lookup"><span data-stu-id="82cf2-111">Learn terms and definitions for Docker technology.</span></span>

[<span data-ttu-id="82cf2-112">Contenitori, immagini e registri di Docker</span><span class="sxs-lookup"><span data-stu-id="82cf2-112">Docker containers, images, and registries</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
<span data-ttu-id="82cf2-113">Informazioni sulle modalità con cui le immagini del contenitore Docker vengono archiviate in un registro immagini per una distribuzione uniforme nei diversi ambienti.</span><span class="sxs-lookup"><span data-stu-id="82cf2-113">Find out how Docker container images are stored in an image registry for consistent deployment across environments.</span></span>

[<span data-ttu-id="82cf2-114">Compilare immagini Docker per applicazioni .NET Core</span><span class="sxs-lookup"><span data-stu-id="82cf2-114">Build Docker Images for .NET Core Applications</span></span>](/dotnet/articles/core/docker/building-net-docker-images)  
<span data-ttu-id="82cf2-115">Informazioni sulle procedure per compilare un'app di ASP.NET Core e inserirla in un contenitore Docker.</span><span class="sxs-lookup"><span data-stu-id="82cf2-115">Learn how to build and dockerize an ASP.NET Core app.</span></span> <span data-ttu-id="82cf2-116">Si analizzano le immagini Docker gestite da Microsoft e si esaminano casi d'uso.</span><span class="sxs-lookup"><span data-stu-id="82cf2-116">Explore Docker images maintained by Microsoft and examine use cases.</span></span>

[<span data-ttu-id="82cf2-117">Visual Studio Tools per Docker</span><span class="sxs-lookup"><span data-stu-id="82cf2-117">Visual Studio Tools for Docker</span></span>](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
<span data-ttu-id="82cf2-118">Informazioni su come Visual Studio 2017 supporta la compilazione, il debug e l'esecuzione di app ASP.NET Core destinate a .NET Framework o .NET Core in Docker per Windows.</span><span class="sxs-lookup"><span data-stu-id="82cf2-118">Discover how Visual Studio 2017 supports building, debugging, and running ASP.NET Core apps targeting either .NET Framework or .NET Core on Docker for Windows.</span></span> <span data-ttu-id="82cf2-119">Sono supportati sia contenitori Windows che contenitori Linux.</span><span class="sxs-lookup"><span data-stu-id="82cf2-119">Both Windows and Linux containers are supported.</span></span>

[<span data-ttu-id="82cf2-120">Pubblicare in un'immagine Docker</span><span class="sxs-lookup"><span data-stu-id="82cf2-120">Publish to a Docker Image</span></span>](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
<span data-ttu-id="82cf2-121">Informazioni su come usare Visual Studio Tools per l'estensione Docker per distribuire un'app ASP.NET Core in un host Docker in Azure tramite PowerShell.</span><span class="sxs-lookup"><span data-stu-id="82cf2-121">Find out how to use the Visual Studio Tools for Docker extension to deploy an ASP.NET Core app to a Docker host on Azure using PowerShell.</span></span>

[<span data-ttu-id="82cf2-122">Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="82cf2-122">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)  
<span data-ttu-id="82cf2-123">Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro a server proxy e a servizi di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="82cf2-123">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="82cf2-124">Il passaggio delle richieste attraverso un proxy spesso oscura le informazioni sulla richiesta originale, ad esempio lo schema e l'indirizzo IP del client.</span><span class="sxs-lookup"><span data-stu-id="82cf2-124">Passing requests through a proxy often obscures information about the original request, such as the scheme and client IP.</span></span> <span data-ttu-id="82cf2-125">Potrebbe essere necessario inoltrare alcune informazioni sulla richiesta manualmente all'app.</span><span class="sxs-lookup"><span data-stu-id="82cf2-125">It might be necessary to forwarded some information about the request manually to the app.</span></span>
