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
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>Riutilizzo di oggetti con ObjectPool in ASP.NET Core

Di [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak)e [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool> fa parte dell'infrastruttura ASP.NET Core che supporta la conservazione di un gruppo di oggetti in memoria per il riutilizzo, anziché consentire l'operazione di Garbage Collection degli oggetti.

Potrebbe essere necessario utilizzare il pool di oggetti se gli oggetti gestiti sono:

- Costoso da allocare o inizializzare.
- Rappresentare una risorsa limitata.
- Utilizzato in modo prevedibile e frequente.

Ad esempio, il framework ASP.NET Core usa il pool di oggetti in alcune posizioni per riutilizzare <xref:System.Text.StringBuilder> istanze. `StringBuilder` alloca e gestisce i propri buffer per conservare i dati di tipo carattere. ASP.NET Core usa regolarmente `StringBuilder` per implementare le funzionalità e riutilizzarle offre un vantaggio in merito alle prestazioni.

Il pool di oggetti non migliora sempre le prestazioni:

- A meno che il costo di inizializzazione di un oggetto non sia elevato, è in genere più lento ottenere l'oggetto dal pool.
- Gli oggetti gestiti dal pool non vengono deallocati fino a quando il pool non viene deallocato.

Usare il pool di oggetti solo dopo aver raccolto i dati sulle prestazioni usando scenari realistici per l'app o la libreria.

**AVVISO: il `ObjectPool` non implementa `IDisposable`. Non è consigliabile usarlo con i tipi che richiedono l'eliminazione.**

**Nota: il ObjectPool non impone un limite al numero di oggetti che verrà allocato, quindi viene inserito un limite al numero di oggetti che verrà mantenuto.**

## <a name="concepts"></a>Concetti

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1>-l'astrazione del pool di oggetti di base. Utilizzato per ottenere e restituire oggetti.

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> implementare questo oggetto per personalizzare la modalità di creazione di un oggetto e il modo in cui viene *reimpostato* quando viene restituito al pool. Questa operazione può essere passata in un pool di oggetti costruito direttamente.... O

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> funge da Factory per la creazione di pool di oggetti.
<!-- REview, there is no ObjectPoolProvider<T> -->

ObjectPool può essere usato in un'app in diversi modi:

* Creazione di un'istanza di un pool.
* Registrazione di un pool nell' [inserimento delle dipendenze](xref:fundamentals/dependency-injection) (di) come istanza di.
* Registrazione del `ObjectPoolProvider<>` in DI e utilizzo come Factory.

## <a name="how-to-use-objectpool"></a>Come usare ObjectPool

Chiamare <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> per ottenere un oggetto e <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> per restituire l'oggetto.  Non è necessario restituire tutti gli oggetti. Se non viene restituito alcun oggetto, sarà sottoposta a Garbage Collection.

## <a name="objectpool-sample"></a>Esempio ObjectPool

Il codice seguente:

* Aggiunge `ObjectPoolProvider` al contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) .
* Aggiunge e configura `ObjectPool<StringBuilder>` al contenitore DI.
* Aggiunge la `BirthdayMiddleware`.

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

Il codice seguente implementa `BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
