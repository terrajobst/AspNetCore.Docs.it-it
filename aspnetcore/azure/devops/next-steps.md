---
title: Passaggi successivi - DevOps con ASP.NET Core e Azure
author: CamSoper
description: Altri percorsi di apprendimento per DevOps con ASP.NET Core e Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: 7c3b1c701b13b2a2052c72f5f84bba33d4995ad7
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121324"
---
# <a name="next-steps"></a><span data-ttu-id="5738f-103">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="5738f-103">Next steps</span></span>

<span data-ttu-id="5738f-104">In questa guida è stata creata una pipeline di DevOps per un'app di esempio ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5738f-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="5738f-105">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="5738f-105">Congratulations!</span></span> <span data-ttu-id="5738f-106">Ci auguriamo che sia stata soddisfacente di apprendimento pubblicare le app web ASP.NET Core in servizio App di Azure e automatizzare l'integrazione continua delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="5738f-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="5738f-107">Oltre a DevOps e hosting web, Azure offre un'ampia gamma di servizi di Platform-as-a-Service (PaaS) utili agli sviluppatori di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5738f-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="5738f-108">Questa sezione viene fornita una breve panoramica di alcuni dei servizi più usati.</span><span class="sxs-lookup"><span data-stu-id="5738f-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="5738f-109">Archiviazione e database</span><span class="sxs-lookup"><span data-stu-id="5738f-109">Storage and databases</span></span>

<span data-ttu-id="5738f-110">[Cache redis](/azure/redis-cache/) è memorizzazione disponibile come servizio di dati e velocità effettiva elevata e bassa latenza.</span><span class="sxs-lookup"><span data-stu-id="5738f-110">[Redis Cache](/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="5738f-111">Può essere utilizzato per la memorizzazione nella cache dell'output delle pagine, riducendo le richieste del database e che fornisce lo stato della sessione ASP.NET Core in più istanze di un'app.</span><span class="sxs-lookup"><span data-stu-id="5738f-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="5738f-112">[Archiviazione di Azure](/azure/storage/) è un'archiviazione cloud altamente scalabile di Azure.</span><span class="sxs-lookup"><span data-stu-id="5738f-112">[Azure Storage](/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="5738f-113">Gli sviluppatori possono sfruttare [archiviazione di Accodamento](/azure/storage/queues/storage-queues-introduction) per l'accodamento dei messaggi affidabile, e [archiviazione tabelle](/azure/storage/tables/table-storage-overview) è un archivio chiave-valore NoSQL progettato per lo sviluppo rapido con set di dati di grandi dimensioni, semi-strutturati.</span><span class="sxs-lookup"><span data-stu-id="5738f-113">Developers can take advantage of [Queue Storage](/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="5738f-114">[Database SQL di Azure](/azure/sql-database/) fornisce funzionalità di database relazionale intuitiva di un servizio usando il motore Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5738f-114">[Azure SQL Database](/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="5738f-115">[COSMOS DB](/azure/cosmos-db/) servizio di database NoSQL multimodello, distribuito a livello globale.</span><span class="sxs-lookup"><span data-stu-id="5738f-115">[Cosmos DB](/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="5738f-116">Più API sono disponibili, tra cui MongoDB, Cassandra e API SQL (in precedenza denominato DocumentDB).</span><span class="sxs-lookup"><span data-stu-id="5738f-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="5738f-117">identità</span><span class="sxs-lookup"><span data-stu-id="5738f-117">Identity</span></span>

<span data-ttu-id="5738f-118">[Azure Active Directory](/azure/active-directory/) e [Azure Active Directory B2C](/azure/active-directory-b2c/) sono entrambi servizi di identità.</span><span class="sxs-lookup"><span data-stu-id="5738f-118">[Azure Active Directory](/azure/active-directory/) and [Azure Active Directory B2C](/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="5738f-119">Azure Active Directory è progettato per gli scenari aziendali e consente di collaborazione di Azure AD B2B (business-to-business), mentre Azure Active Directory B2C è previsti scenari business to consumer, tra cui accedi social network.</span><span class="sxs-lookup"><span data-stu-id="5738f-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="5738f-120">Cellulare</span><span class="sxs-lookup"><span data-stu-id="5738f-120">Mobile</span></span>

<span data-ttu-id="5738f-121">[Hub di notifica](/azure/notification-hubs/) è un motore di notifiche push multipiattaforma e scalabile per inviare rapidamente milioni di messaggi alle applicazioni eseguite in diversi tipi di dispositivi.</span><span class="sxs-lookup"><span data-stu-id="5738f-121">[Notification Hubs](/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="5738f-122">Infrastruttura Web</span><span class="sxs-lookup"><span data-stu-id="5738f-122">Web infrastructure</span></span>

<span data-ttu-id="5738f-123">[Servizio contenitore di Azure](/azure/aks/) gestisce l'ambiente Kubernetes ospitato, rendendo veloce e facile da distribuire e gestire le App in contenitori senza competenze nell'orchestrazione di contenitori.</span><span class="sxs-lookup"><span data-stu-id="5738f-123">[Azure Container Service](/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="5738f-124">[Ricerca di Azure](/azure/search/) viene usato per creare una soluzione di ricerca aziendale su contenuto privato eterogeneo.</span><span class="sxs-lookup"><span data-stu-id="5738f-124">[Azure Search](/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="5738f-125">[Service Fabric](/azure/service-fabric/) è una piattaforma di sistemi distribuiti che consente di creare un pacchetto, distribuire e gestire facilmente scalabile e affidabile microservizi e contenitori.</span><span class="sxs-lookup"><span data-stu-id="5738f-125">[Service Fabric](/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
