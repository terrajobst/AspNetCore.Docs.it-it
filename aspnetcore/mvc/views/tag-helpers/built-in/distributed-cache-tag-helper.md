---
title: Helper tag di cache distribuita in ASP.NET Core
author: pkellner
description: Descrive l'uso dell'helper tag di cache
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 929156633048b8ee68a66290f44b12026a08c8c9
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="d2625-103">Helper tag di cache distribuita in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2625-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="d2625-104">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="d2625-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="d2625-105">L'helper tag di cache distribuita consente di migliorare notevolmente le prestazioni dell'app ASP.NET Core memorizzandone il contenuto in un'origine cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="d2625-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="d2625-106">L'helper tag di cache distribuita eredita dalla stessa classe di base da cui eredita l'helper tag di cache.</span><span class="sxs-lookup"><span data-stu-id="d2625-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="d2625-107">Tutti gli attributi associati all'helper tag di cache funzionano anche nell'helper tag di cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="d2625-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="d2625-108">L'helper tag di cache distribuita segue il **principio delle dipendenze esplicite** noto come **inserimento del costruttore**.</span><span class="sxs-lookup"><span data-stu-id="d2625-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="d2625-109">In particolare, il contenitore di interfaccia `IDistributedCache` viene passato con il costruttore dell'helper tag di cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="d2625-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="d2625-110">Se non è stata creata nessuna implementazione concreta specifica di `IDistributedCache` in `ConfigureServices`, disponibile in genere in startup.cs, l'helper tag di cache distribuita usa per l'archiviazione dei dati lo stesso provider in memoria usato dall'helper tag di cache di base.</span><span class="sxs-lookup"><span data-stu-id="d2625-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="d2625-111">Attributi dell'helper per tag di cache distribuita</span><span class="sxs-lookup"><span data-stu-id="d2625-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="d2625-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span><span class="sxs-lookup"><span data-stu-id="d2625-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="d2625-113">Per le definizioni, vedere Helper tag di cache.</span><span class="sxs-lookup"><span data-stu-id="d2625-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="d2625-114">L'helper tag di cache distribuita eredita dalla stessa classe dell'helper tag di cache, pertanto tutti questi attributi sono condivisi con l'helper tag di cache.</span><span class="sxs-lookup"><span data-stu-id="d2625-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="d2625-115">name (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="d2625-115">name (required)</span></span>

| <span data-ttu-id="d2625-116">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="d2625-116">Attribute Type</span></span>    | <span data-ttu-id="d2625-117">Valore di esempio</span><span class="sxs-lookup"><span data-stu-id="d2625-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="d2625-118">stringa</span><span class="sxs-lookup"><span data-stu-id="d2625-118">string</span></span>    | <span data-ttu-id="d2625-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="d2625-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="d2625-120">L'attributo `name` obbligatorio viene usato come chiave per la cache archiviata per ogni istanza di un helper tag di cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="d2625-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="d2625-121">A differenza dell'helper tag di cache di base, che assegna una chiave a ogni istanza di helper tag di cache in base al nome della pagina Razor e alla posizione dell'helper tag nella pagina Razor, nell'helper tag di cache distribuita la chiave è basata solo sull'attributo `name`.</span><span class="sxs-lookup"><span data-stu-id="d2625-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="d2625-122">Esempio di uso:</span><span class="sxs-lookup"><span data-stu-id="d2625-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="d2625-123">Implementazioni IDistributedCache dell'helper tag di cache distribuita</span><span class="sxs-lookup"><span data-stu-id="d2625-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="d2625-124">Esistono due implementazioni di `IDistributedCache` integrate in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2625-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="d2625-125">Una è basata su **SQL Server** e l'altra è basata su **Redis**.</span><span class="sxs-lookup"><span data-stu-id="d2625-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="d2625-126">I dettagli di queste implementazioni sono disponibili nella risorsa "Uso di una cache distribuita" citata di seguito.</span><span class="sxs-lookup"><span data-stu-id="d2625-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="d2625-127">Entrambe le implementazioni implicano l'impostazione di un'istanza di `IDistributedCache` in **startup.cs** di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2625-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="d2625-128">Nessun attributo di tag è associato specificamente all'uso di un'implementazione specifica di `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="d2625-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="d2625-129">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d2625-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
