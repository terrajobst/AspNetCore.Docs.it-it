---
title: Uso dei dati in ASP.NET Core
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe0ba2ef
ms.technology: aspnet
ms.prod: asp.net-core
ms.openlocfilehash: 3566127476289ae085a9161132b103638bc9b068
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-data-in-aspnet-core"></a><span data-ttu-id="6d800-103">Uso dei dati in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d800-103">Working with Data in ASP.NET Core</span></span> 

*   [<span data-ttu-id="6d800-104">Introduzione ad ASP.NET Core ed Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d800-104">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](ef-mvc/index.md)
    *   [<span data-ttu-id="6d800-105">Introduzione</span><span class="sxs-lookup"><span data-stu-id="6d800-105">Getting started</span></span>](ef-mvc/intro.md)
    *   [<span data-ttu-id="6d800-106">Operazioni di creazione, lettura, aggiornamento ed eliminazione</span><span class="sxs-lookup"><span data-stu-id="6d800-106">Create, Read, Update, and Delete operations</span></span>](ef-mvc/crud.md)
    *   [<span data-ttu-id="6d800-107">Ordinamento, filtro, paging e raggruppamento</span><span class="sxs-lookup"><span data-stu-id="6d800-107">Sorting, filtering, paging, and grouping</span></span>](ef-mvc/sort-filter-page.md)
    *   [<span data-ttu-id="6d800-108">Migrazioni</span><span class="sxs-lookup"><span data-stu-id="6d800-108">Migrations</span></span>](ef-mvc/migrations.md)
    *   [<span data-ttu-id="6d800-109">Creazione di un modello di dati complesso</span><span class="sxs-lookup"><span data-stu-id="6d800-109">Creating a complex data model</span></span>](ef-mvc/complex-data-model.md)
    *   [<span data-ttu-id="6d800-110">Lettura di dati correlati</span><span class="sxs-lookup"><span data-stu-id="6d800-110">Reading related data</span></span>](ef-mvc/read-related-data.md)
    *   [<span data-ttu-id="6d800-111">Aggiornamento di dati correlati</span><span class="sxs-lookup"><span data-stu-id="6d800-111">Updating related data</span></span>](ef-mvc/update-related-data.md)
    *   [<span data-ttu-id="6d800-112">Gestione di conflitti di concorrenza</span><span class="sxs-lookup"><span data-stu-id="6d800-112">Handling concurrency conflicts</span></span>](ef-mvc/concurrency.md)
    *   [<span data-ttu-id="6d800-113">Ereditarietà</span><span class="sxs-lookup"><span data-stu-id="6d800-113">Inheritance</span></span>](ef-mvc/inheritance.md)
    *   [<span data-ttu-id="6d800-114">Argomenti avanzati</span><span class="sxs-lookup"><span data-stu-id="6d800-114">Advanced topics</span></span>](ef-mvc/advanced.md)
* <span data-ttu-id="6d800-115">[ASP.NET Core con Entity Framework Core con nuovo database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (sito di documentazione di Entity Framework Core)</span><span class="sxs-lookup"><span data-stu-id="6d800-115">[ASP.NET Core with EF Core - new database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (Entity Framework Core documentation site)</span></span>
* <span data-ttu-id="6d800-116">[ASP.NET Core con Entity Framework Core con database esistente](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (sito di documentazione di Entity Framework Core)</span><span class="sxs-lookup"><span data-stu-id="6d800-116">[ASP.NET Core with EF Core - existing database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (Entity Framework Core documentation site)</span></span>
*   [<span data-ttu-id="6d800-117">Introduzione ad ASP.NET Core ed Entity Framework Core 6</span><span class="sxs-lookup"><span data-stu-id="6d800-117">Getting Started with ASP.NET Core and Entity Framework 6</span></span>](entity-framework-6.md)
*   [<span data-ttu-id="6d800-118">Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="6d800-118">Azure Storage</span></span>](azure-storage/index.md)
    *   [<span data-ttu-id="6d800-119">Aggiunta dell'archiviazione di Azure tramite Servizi connessi di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d800-119">Adding Azure Storage by Using Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-azure-tools-connected-services-storage/)
    *   [<span data-ttu-id="6d800-120">Introduzione all'archiviazione BLOB di Azure a Servizi connessi di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d800-120">Get Started with Azure Blob storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)
    *   [<span data-ttu-id="6d800-121">Introduzione all'archiviazione code e a Servizi connessi di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d800-121">Get Started with Queue Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-queues/)
    *   [<span data-ttu-id="6d800-122">Modalità riguardanti l'introduzione all'archiviazione tabelle di Azure a Servizi connessi di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d800-122">How to Get Started with Azure Table Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-tables/)
