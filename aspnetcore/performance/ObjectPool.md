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
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>Riutilizzo di oggetti con avvio in ASP.NET Core

Dal [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), e [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool> fa parte dell'infrastruttura di ASP.NET Core che supporta il mantenimento di un gruppo di oggetti in memoria per il riutilizzo anziché consentire gli oggetti da sottoporre a garbage collection.

Si potrebbe voler usare il pool di oggetti se gli oggetti gestiti sono:

- Costoso da allocare o inizializzare.
- Rappresentano alcune risorse limitate.
- Usato in modo prevedibile e frequente.

Ad esempio, il framework di ASP.NET Core Usa il pool di oggetti in alcune posizioni riutilizzare <xref:System.Text.StringBuilder> istanze. `StringBuilder` Alloca e gestisce il proprio buffer che deve contenere dati di tipo carattere. ASP.NET Core Usa regolarmente `StringBuilder` per implementare le funzionalità e riutilizzandole offre un miglioramento delle prestazioni.

Il pool di oggetti non sempre servirà a migliorare le prestazioni:

- A meno che il costo di inizializzazione di un oggetto è elevato, è in genere più lento ottenere l'oggetto dal pool.
- Non sono oggetti gestiti dal pool deallocati fino a quando il pool verrà deallocato.

Usare il pooling degli oggetti solo dopo aver raccolto i dati sulle prestazioni tramite scenari realistici per l'app o una libreria.

**AVVISO: Il `ObjectPool` non implementa `IDisposable`. Non è consigliabile utilizzarlo con tipi che necessitano di eliminazione.**

**NOTA: L'avvio non imporre alcun limite al numero di oggetti che allocherà, inserisce un limite al numero di oggetti che verrà mantenute.**

## <a name="concepts"></a>Concetti

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -l'astrazione di pool di oggetti di base. Utilizzato per ottenere e restituire oggetti.

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> -implementare questo metodo per personalizzare la modalità di creazione di un oggetto e sulla sua *reimpostare* quando restituite al pool. Questo può essere passato in un pool di oggetti che si costruisce direttamente... Oppure

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> si comporta come una factory per la creazione di pool di oggetti.
<!-- REview, there is no ObjectPoolProvider<T> -->

L'avvio può essere utilizzato in un'app in diversi modi:

* Creare un'istanza di un pool.
* La registrazione di un pool nel [inserimento delle dipendenze](xref:fundamentals/dependency-injection) (inserimento delle dipendenze) come un'istanza.
* La registrazione di `ObjectPoolProvider<>` nell'inserimento delle dipendenze e usandolo come una factory.

## <a name="how-to-use-objectpool"></a>Come usare avvio

Chiamare <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> per ottenere un oggetto e <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> per restituire l'oggetto.  Non è necessario che venga restituito tutti gli oggetti. Se si non restituisce un oggetto, sarà sottoposto a garbage collection.

## <a name="objectpool-sample"></a>Esempio di avvio

Il codice seguente:

* Aggiunge `ObjectPoolProvider` per il [inserimento delle dipendenze](xref:fundamentals/dependency-injection) contenitore (inserimento delle dipendenze).
* Aggiunge e configura `ObjectPool<StringBuilder>` al contenitore di inserimento delle dipendenze.
* Aggiunge il `BirthdayMiddleware`.

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

Il codice seguente implementa `BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
