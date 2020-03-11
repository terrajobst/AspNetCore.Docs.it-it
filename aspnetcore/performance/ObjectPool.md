---
title: Riutilizzo di oggetti con ObjectPool in ASP.NET Core
author: rick-anderson
description: Suggerimenti per migliorare le prestazioni in ASP.NET Core app usando ObjectPool.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 771f19e54a908b8b2cd85ff72f368f16e94a2310
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666110"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a><span data-ttu-id="a2206-103">Riutilizzo di oggetti con ObjectPool in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2206-103">Object reuse with ObjectPool in ASP.NET Core</span></span>

<span data-ttu-id="a2206-104">Di [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak)e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a2206-104">By [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a2206-105"><xref:Microsoft.Extensions.ObjectPool> fa parte dell'infrastruttura ASP.NET Core che supporta la conservazione di un gruppo di oggetti in memoria per il riutilizzo, anziché consentire l'operazione di Garbage Collection degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="a2206-105"><xref:Microsoft.Extensions.ObjectPool> is part of the ASP.NET Core infrastructure that supports keeping a group of objects in memory for reuse rather than allowing the objects to be garbage collected.</span></span>

<span data-ttu-id="a2206-106">Potrebbe essere necessario utilizzare il pool di oggetti se gli oggetti gestiti sono:</span><span class="sxs-lookup"><span data-stu-id="a2206-106">You might want to use the object pool if the objects that are being managed are:</span></span>

- <span data-ttu-id="a2206-107">Costoso da allocare o inizializzare.</span><span class="sxs-lookup"><span data-stu-id="a2206-107">Expensive to allocate/initialize.</span></span>
- <span data-ttu-id="a2206-108">Rappresentare una risorsa limitata.</span><span class="sxs-lookup"><span data-stu-id="a2206-108">Represent some limited resource.</span></span>
- <span data-ttu-id="a2206-109">Utilizzato in modo prevedibile e frequente.</span><span class="sxs-lookup"><span data-stu-id="a2206-109">Used predictably and frequently.</span></span>

<span data-ttu-id="a2206-110">Ad esempio, il framework ASP.NET Core usa il pool di oggetti in alcune posizioni per riutilizzare <xref:System.Text.StringBuilder> istanze.</span><span class="sxs-lookup"><span data-stu-id="a2206-110">For example, the ASP.NET Core framework uses the object pool in some places to reuse <xref:System.Text.StringBuilder> instances.</span></span> <span data-ttu-id="a2206-111">`StringBuilder` alloca e gestisce i propri buffer per conservare i dati di tipo carattere.</span><span class="sxs-lookup"><span data-stu-id="a2206-111">`StringBuilder` allocates and manages its own buffers to hold character data.</span></span> <span data-ttu-id="a2206-112">ASP.NET Core usa regolarmente `StringBuilder` per implementare le funzionalità e riutilizzarle offre un vantaggio in merito alle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="a2206-112">ASP.NET Core regularly uses `StringBuilder` to implement features, and reusing them provides a performance benefit.</span></span>

<span data-ttu-id="a2206-113">Il pool di oggetti non migliora sempre le prestazioni:</span><span class="sxs-lookup"><span data-stu-id="a2206-113">Object pooling doesn't always improve performance:</span></span>

- <span data-ttu-id="a2206-114">A meno che il costo di inizializzazione di un oggetto non sia elevato, è in genere più lento ottenere l'oggetto dal pool.</span><span class="sxs-lookup"><span data-stu-id="a2206-114">Unless the initialization cost of an object is high, it's usually slower to get the object from the pool.</span></span>
- <span data-ttu-id="a2206-115">Gli oggetti gestiti dal pool non vengono deallocati fino a quando il pool non viene deallocato.</span><span class="sxs-lookup"><span data-stu-id="a2206-115">Objects managed by the pool aren't de-allocated until the pool is de-allocated.</span></span>

<span data-ttu-id="a2206-116">Usare il pool di oggetti solo dopo aver raccolto i dati sulle prestazioni usando scenari realistici per l'app o la libreria.</span><span class="sxs-lookup"><span data-stu-id="a2206-116">Use object pooling only after collecting performance data using realistic scenarios for your app or library.</span></span>

<span data-ttu-id="a2206-117">**AVVISO: il `ObjectPool` non implementa `IDisposable`. Non è consigliabile usarlo con i tipi che richiedono l'eliminazione.**</span><span class="sxs-lookup"><span data-stu-id="a2206-117">**WARNING: The `ObjectPool` doesn't implement `IDisposable`. We don't recommend using it with types that need disposal.**</span></span>

<span data-ttu-id="a2206-118">**Nota: il ObjectPool non impone un limite al numero di oggetti che verrà allocato, quindi viene inserito un limite al numero di oggetti che verrà mantenuto.**</span><span class="sxs-lookup"><span data-stu-id="a2206-118">**NOTE: The ObjectPool doesn't place a limit on the number of objects that it will allocate, it places a limit on the number of objects it will retain.**</span></span>

## <a name="concepts"></a><span data-ttu-id="a2206-119">Concetti</span><span class="sxs-lookup"><span data-stu-id="a2206-119">Concepts</span></span>

<span data-ttu-id="a2206-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1>-l'astrazione del pool di oggetti di base.</span><span class="sxs-lookup"><span data-stu-id="a2206-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> - the basic object pool abstraction.</span></span> <span data-ttu-id="a2206-121">Utilizzato per ottenere e restituire oggetti.</span><span class="sxs-lookup"><span data-stu-id="a2206-121">Used to get and return objects.</span></span>

<span data-ttu-id="a2206-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> implementare questo oggetto per personalizzare la modalità di creazione di un oggetto e il modo in cui viene *reimpostato* quando viene restituito al pool.</span><span class="sxs-lookup"><span data-stu-id="a2206-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> - implement this to customize how an object is created and how it is *reset* when returned to the pool.</span></span> <span data-ttu-id="a2206-123">Questa operazione può essere passata in un pool di oggetti costruito direttamente.... O</span><span class="sxs-lookup"><span data-stu-id="a2206-123">This can be passed into an object pool that you construct directly.... OR</span></span>

<span data-ttu-id="a2206-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> funge da Factory per la creazione di pool di oggetti.</span><span class="sxs-lookup"><span data-stu-id="a2206-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> acts as a factory for creating object pools.</span></span>
<!-- REview, there is no ObjectPoolProvider<T> -->

<span data-ttu-id="a2206-125">ObjectPool può essere usato in un'app in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="a2206-125">The ObjectPool can be used in an app in multiple ways:</span></span>

* <span data-ttu-id="a2206-126">Creazione di un'istanza di un pool.</span><span class="sxs-lookup"><span data-stu-id="a2206-126">Instantiating a pool.</span></span>
* <span data-ttu-id="a2206-127">Registrazione di un pool nell' [inserimento delle dipendenze](xref:fundamentals/dependency-injection) (di) come istanza di.</span><span class="sxs-lookup"><span data-stu-id="a2206-127">Registering a pool in [Dependency injection](xref:fundamentals/dependency-injection) (DI) as an instance.</span></span>
* <span data-ttu-id="a2206-128">Registrazione del `ObjectPoolProvider<>` in DI e utilizzo come Factory.</span><span class="sxs-lookup"><span data-stu-id="a2206-128">Registering the `ObjectPoolProvider<>` in DI and using it as a factory.</span></span>

## <a name="how-to-use-objectpool"></a><span data-ttu-id="a2206-129">Come usare ObjectPool</span><span class="sxs-lookup"><span data-stu-id="a2206-129">How to use ObjectPool</span></span>

<span data-ttu-id="a2206-130">Chiamare <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> per ottenere un oggetto e <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> per restituire l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="a2206-130">Call <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> to get an object and <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> to return the object.</span></span>  <span data-ttu-id="a2206-131">Non è necessario restituire tutti gli oggetti.</span><span class="sxs-lookup"><span data-stu-id="a2206-131">There's no requirement that you return every object.</span></span> <span data-ttu-id="a2206-132">Se non viene restituito alcun oggetto, sarà sottoposta a Garbage Collection.</span><span class="sxs-lookup"><span data-stu-id="a2206-132">If you don't return an object, it will be garbage collected.</span></span>

## <a name="objectpool-sample"></a><span data-ttu-id="a2206-133">Esempio ObjectPool</span><span class="sxs-lookup"><span data-stu-id="a2206-133">ObjectPool sample</span></span>

<span data-ttu-id="a2206-134">Il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a2206-134">The following code:</span></span>

* <span data-ttu-id="a2206-135">Aggiunge `ObjectPoolProvider` al contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="a2206-135">Adds `ObjectPoolProvider` to the [Dependency injection](xref:fundamentals/dependency-injection) (DI) container.</span></span>
* <span data-ttu-id="a2206-136">Aggiunge e configura `ObjectPool<StringBuilder>` al contenitore DI.</span><span class="sxs-lookup"><span data-stu-id="a2206-136">Adds and configures `ObjectPool<StringBuilder>` to the DI container.</span></span>
* <span data-ttu-id="a2206-137">Aggiunge la `BirthdayMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="a2206-137">Adds the `BirthdayMiddleware`.</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

<span data-ttu-id="a2206-138">Il codice seguente implementa `BirthdayMiddleware`</span><span class="sxs-lookup"><span data-stu-id="a2206-138">The following code implements `BirthdayMiddleware`</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
