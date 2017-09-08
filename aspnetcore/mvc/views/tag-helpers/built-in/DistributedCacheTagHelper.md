---
title: Helper di Tag della Cache distribuita | Documenti Microsoft
author: pkellner
description: Di seguito viene illustrato l'utilizzo di Helper di Tag della Cache
keywords: ASP.NET Core, helper tag
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper
ms.openlocfilehash: b6e0beca0833b1dbe0843e8f8848b976726cc7b0
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="179b9-104">Helper di Tag Cache distribuita</span><span class="sxs-lookup"><span data-stu-id="179b9-104">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="179b9-105">Da [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="179b9-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="179b9-106">L'Helper di Tag della Cache distribuita consente di migliorare notevolmente le prestazioni dell'applicazione ASP.NET di base per la memorizzazione nella cache il contenuto a un'origine cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="179b9-106">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="179b9-107">L'Helper di Tag della Cache distribuita eredita dalla stessa classe di base come l'Helper di Tag della Cache.</span><span class="sxs-lookup"><span data-stu-id="179b9-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="179b9-108">Tutti gli attributi associati all'Helper di Tag della Cache funziona anche nell'Helper di Tag distribuita.</span><span class="sxs-lookup"><span data-stu-id="179b9-108">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="179b9-109">L'Helper di Tag della Cache distribuita segue il **principio dipendenze esplicite** noto come **inserimento costruttore**.</span><span class="sxs-lookup"><span data-stu-id="179b9-109">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="179b9-110">In particolare, il `IDistributedCache` interfaccia contenitore viene passato al costruttore distribuito Cache Tag del supporto.</span><span class="sxs-lookup"><span data-stu-id="179b9-110">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="179b9-111">Se non specifica implementazione concreta di `IDistributedCache` è stata creata in `ConfigureServices`, disponibile in genere in startup.cs, quindi l'Helper di Tag della Cache distribuita verrà utilizzato lo stesso provider in memoria per l'archiviazione dei dati memorizzati nella cache come l'Helper di Tag di Cache basic.</span><span class="sxs-lookup"><span data-stu-id="179b9-111">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="179b9-112">Attributi di Helper di Tag della Cache distribuita</span><span class="sxs-lookup"><span data-stu-id="179b9-112">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="179b9-113">abilitato su scade scade dopo scade-scorrevole variano in base all'intestazione variano da query variano da route variano da cookie variano da utente variano per priorità</span><span class="sxs-lookup"><span data-stu-id="179b9-113">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="179b9-114">Per le definizioni, vedere l'Helper di Tag della Cache.</span><span class="sxs-lookup"><span data-stu-id="179b9-114">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="179b9-115">Helper di Tag della Cache distribuita eredita dalla stessa classe Helper di Tag della Cache in modo che tutti gli attributi siano comuni da Helper di Tag della Cache.</span><span class="sxs-lookup"><span data-stu-id="179b9-115">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="179b9-116">nome (obbligatorio)</span><span class="sxs-lookup"><span data-stu-id="179b9-116">name (required)</span></span>

| <span data-ttu-id="179b9-117">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="179b9-117">Attribute Type</span></span>    | <span data-ttu-id="179b9-118">Valore di esempio</span><span class="sxs-lookup"><span data-stu-id="179b9-118">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="179b9-119">string</span><span class="sxs-lookup"><span data-stu-id="179b9-119">string</span></span>    | <span data-ttu-id="179b9-120">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="179b9-120">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="179b9-121">Obbligatorio `name` attributo viene utilizzato come chiave di cache archiviata per ogni istanza di un Helper di Tag della Cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="179b9-121">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="179b9-122">A differenza di base Helper Tag di Cache che assegna una chiave a ogni istanza dell'Helper di Tag della Cache in base Razor pagina nome e il percorso dell'helper di tag nella pagina razor, l'Helper di Tag della Cache distribuita basi solo la chiave dell'attributo`name`</span><span class="sxs-lookup"><span data-stu-id="179b9-122">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="179b9-123">Esempio di utilizzo:</span><span class="sxs-lookup"><span data-stu-id="179b9-123">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="179b9-124">Implementazioni IDistributedCache Helper di Tag della Cache distribuita</span><span class="sxs-lookup"><span data-stu-id="179b9-124">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="179b9-125">Esistono due implementazioni di `IDistributedCache` integrata in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="179b9-125">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="179b9-126">Una si basa sul **Sql Server** e l'altro è basato su **Redis**.</span><span class="sxs-lookup"><span data-stu-id="179b9-126">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="179b9-127">Dettagli di queste implementazioni sono disponibili alla risorsa a cui fa riferimento denominata "utilizzo di una cache distribuita" di seguito.</span><span class="sxs-lookup"><span data-stu-id="179b9-127">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="179b9-128">Entrambe le implementazioni implicano l'impostazione di un'istanza di `IDistributedCache` in ASP.NET Core **startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="179b9-128">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="179b9-129">Non esiste alcun attributo tag specificamente associati utilizzando qualsiasi implementazione specifica di `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="179b9-129">There no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="179b9-130">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="179b9-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>