---
title: Riutilizzo di oggetti con avvio in ASP.NET Core
author: rick-anderson
description: Suggerimenti per migliorare le prestazioni delle App ASP.NET Core con l'avvio.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 771f19e54a908b8b2cd85ff72f368f16e94a2310
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815516"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a><span data-ttu-id="fc2ab-103">Riutilizzo di oggetti con avvio in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc2ab-103">Object reuse with ObjectPool in ASP.NET Core</span></span>

<span data-ttu-id="fc2ab-104">Dal [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fc2ab-104">By [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fc2ab-105"><xref:Microsoft.Extensions.ObjectPool> fa parte dell'infrastruttura di ASP.NET Core che supporta il mantenimento di un gruppo di oggetti in memoria per il riutilizzo anziché consentire gli oggetti da sottoporre a garbage collection.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-105"><xref:Microsoft.Extensions.ObjectPool> is part of the ASP.NET Core infrastructure that supports keeping a group of objects in memory for reuse rather than allowing the objects to be garbage collected.</span></span>

<span data-ttu-id="fc2ab-106">Si potrebbe voler usare il pool di oggetti se gli oggetti gestiti sono:</span><span class="sxs-lookup"><span data-stu-id="fc2ab-106">You might want to use the object pool if the objects that are being managed are:</span></span>

- <span data-ttu-id="fc2ab-107">Costoso da allocare o inizializzare.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-107">Expensive to allocate/initialize.</span></span>
- <span data-ttu-id="fc2ab-108">Rappresentano alcune risorse limitate.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-108">Represent some limited resource.</span></span>
- <span data-ttu-id="fc2ab-109">Usato in modo prevedibile e frequente.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-109">Used predictably and frequently.</span></span>

<span data-ttu-id="fc2ab-110">Ad esempio, il framework di ASP.NET Core Usa il pool di oggetti in alcune posizioni riutilizzare <xref:System.Text.StringBuilder> istanze.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-110">For example, the ASP.NET Core framework uses the object pool in some places to reuse <xref:System.Text.StringBuilder> instances.</span></span> <span data-ttu-id="fc2ab-111">`StringBuilder` Alloca e gestisce il proprio buffer che deve contenere dati di tipo carattere.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-111">`StringBuilder` allocates and manages its own buffers to hold character data.</span></span> <span data-ttu-id="fc2ab-112">ASP.NET Core Usa regolarmente `StringBuilder` per implementare le funzionalità e riutilizzandole offre un miglioramento delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-112">ASP.NET Core regularly uses `StringBuilder` to implement features, and reusing them provides a performance benefit.</span></span>

<span data-ttu-id="fc2ab-113">Il pool di oggetti non sempre servirà a migliorare le prestazioni:</span><span class="sxs-lookup"><span data-stu-id="fc2ab-113">Object pooling doesn't always improve performance:</span></span>

- <span data-ttu-id="fc2ab-114">A meno che il costo di inizializzazione di un oggetto è elevato, è in genere più lento ottenere l'oggetto dal pool.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-114">Unless the initialization cost of an object is high, it's usually slower to get the object from the pool.</span></span>
- <span data-ttu-id="fc2ab-115">Non sono oggetti gestiti dal pool deallocati fino a quando il pool verrà deallocato.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-115">Objects managed by the pool aren't de-allocated until the pool is de-allocated.</span></span>

<span data-ttu-id="fc2ab-116">Usare il pooling degli oggetti solo dopo aver raccolto i dati sulle prestazioni tramite scenari realistici per l'app o una libreria.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-116">Use object pooling only after collecting performance data using realistic scenarios for your app or library.</span></span>

<span data-ttu-id="fc2ab-117">**AVVISO: Il `ObjectPool` non implementa `IDisposable`. Non è consigliabile utilizzarlo con tipi che necessitano di eliminazione.**</span><span class="sxs-lookup"><span data-stu-id="fc2ab-117">**WARNING: The `ObjectPool` doesn't implement `IDisposable`. We don't recommend using it with types that need disposal.**</span></span>

<span data-ttu-id="fc2ab-118">**NOTA: L'avvio non imporre alcun limite al numero di oggetti che allocherà, inserisce un limite al numero di oggetti che verrà mantenute.**</span><span class="sxs-lookup"><span data-stu-id="fc2ab-118">**NOTE: The ObjectPool doesn't place a limit on the number of objects that it will allocate, it places a limit on the number of objects it will retain.**</span></span>

## <a name="concepts"></a><span data-ttu-id="fc2ab-119">Concetti</span><span class="sxs-lookup"><span data-stu-id="fc2ab-119">Concepts</span></span>

<span data-ttu-id="fc2ab-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -l'astrazione di pool di oggetti di base.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> - the basic object pool abstraction.</span></span> <span data-ttu-id="fc2ab-121">Utilizzato per ottenere e restituire oggetti.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-121">Used to get and return objects.</span></span>

<span data-ttu-id="fc2ab-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> -implementare questo metodo per personalizzare la modalità di creazione di un oggetto e sulla sua *reimpostare* quando restituite al pool.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> - implement this to customize how an object is created and how it is *reset* when returned to the pool.</span></span> <span data-ttu-id="fc2ab-123">Questo può essere passato in un pool di oggetti che si costruisce direttamente... Oppure</span><span class="sxs-lookup"><span data-stu-id="fc2ab-123">This can be passed into an object pool that you construct directly.... OR</span></span>

<span data-ttu-id="fc2ab-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> si comporta come una factory per la creazione di pool di oggetti.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> acts as a factory for creating object pools.</span></span>
<!-- REview, there is no ObjectPoolProvider<T> -->

<span data-ttu-id="fc2ab-125">L'avvio può essere utilizzato in un'app in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="fc2ab-125">The ObjectPool can be used in an app in multiple ways:</span></span>

* <span data-ttu-id="fc2ab-126">Creare un'istanza di un pool.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-126">Instantiating a pool.</span></span>
* <span data-ttu-id="fc2ab-127">La registrazione di un pool nel [inserimento delle dipendenze](xref:fundamentals/dependency-injection) (inserimento delle dipendenze) come un'istanza.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-127">Registering a pool in [Dependency injection](xref:fundamentals/dependency-injection) (DI) as an instance.</span></span>
* <span data-ttu-id="fc2ab-128">La registrazione di `ObjectPoolProvider<>` nell'inserimento delle dipendenze e usandolo come una factory.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-128">Registering the `ObjectPoolProvider<>` in DI and using it as a factory.</span></span>

## <a name="how-to-use-objectpool"></a><span data-ttu-id="fc2ab-129">Come usare avvio</span><span class="sxs-lookup"><span data-stu-id="fc2ab-129">How to use ObjectPool</span></span>

<span data-ttu-id="fc2ab-130">Chiamare <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> per ottenere un oggetto e <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> per restituire l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-130">Call <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> to get an object and <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> to return the object.</span></span>  <span data-ttu-id="fc2ab-131">Non è necessario che venga restituito tutti gli oggetti.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-131">There's no requirement that you return every object.</span></span> <span data-ttu-id="fc2ab-132">Se si non restituisce un oggetto, sarà sottoposto a garbage collection.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-132">If you don't return an object, it will be garbage collected.</span></span>

## <a name="objectpool-sample"></a><span data-ttu-id="fc2ab-133">Esempio di avvio</span><span class="sxs-lookup"><span data-stu-id="fc2ab-133">ObjectPool sample</span></span>

<span data-ttu-id="fc2ab-134">Il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fc2ab-134">The following code:</span></span>

* <span data-ttu-id="fc2ab-135">Aggiunge `ObjectPoolProvider` per il [inserimento delle dipendenze](xref:fundamentals/dependency-injection) contenitore (inserimento delle dipendenze).</span><span class="sxs-lookup"><span data-stu-id="fc2ab-135">Adds `ObjectPoolProvider` to the [Dependency injection](xref:fundamentals/dependency-injection) (DI) container.</span></span>
* <span data-ttu-id="fc2ab-136">Aggiunge e configura `ObjectPool<StringBuilder>` al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-136">Adds and configures `ObjectPool<StringBuilder>` to the DI container.</span></span>
* <span data-ttu-id="fc2ab-137">Aggiunge il `BirthdayMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="fc2ab-137">Adds the `BirthdayMiddleware`.</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

<span data-ttu-id="fc2ab-138">Il codice seguente implementa `BirthdayMiddleware`</span><span class="sxs-lookup"><span data-stu-id="fc2ab-138">The following code implements `BirthdayMiddleware`</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
