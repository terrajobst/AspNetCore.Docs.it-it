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
ms.openlocfilehash: 9c1d91fc185a0afecf59af8927ddf6f25eff29ab
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962324"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Helper tag di cache distribuita in ASP.NET Core

Di [Peter Kellner](http://peterkellner.net) 

L'helper tag di cache distribuita consente di migliorare notevolmente le prestazioni dell'app ASP.NET Core memorizzandone il contenuto in un'origine cache distribuita.

L'helper tag di cache distribuita eredita dalla stessa classe di base da cui eredita l'helper tag di cache. Tutti gli attributi associati all'helper tag di cache funzionano anche nell'helper tag di cache distribuita.

L'helper tag di cache distribuita segue il **principio delle dipendenze esplicite** noto come **inserimento del costruttore**. In particolare, il contenitore di interfaccia `IDistributedCache` viene passato con il costruttore dell'helper tag di cache distribuita. Se non è stata creata nessuna implementazione concreta specifica di `IDistributedCache` in `ConfigureServices`, disponibile in genere in startup.cs, l'helper tag di cache distribuita usa per l'archiviazione dei dati lo stesso provider in memoria usato dall'helper tag di cache di base.

## <a name="distributed-cache-tag-helper-attributes"></a>Attributi dell'helper per tag di cache distribuita

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority

Per le definizioni, vedere Helper tag di cache. L'helper tag di cache distribuita eredita dalla stessa classe dell'helper tag di cache, pertanto tutti questi attributi sono condivisi con l'helper tag di cache.

- - -

### <a name="name-required"></a>name (obbligatorio)

| Tipo di attributo    | Valore di esempio     |
|----------------   |----------------   |
| stringa    | "my-distributed-cache-unique-key-101"     |

L'attributo `name` obbligatorio viene usato come chiave per la cache archiviata per ogni istanza di un helper tag di cache distribuita. A differenza dell'helper tag di cache di base, che assegna una chiave a ogni istanza di helper tag di cache in base al nome della pagina Razor e alla posizione dell'helper tag nella pagina Razor, nell'helper tag di cache distribuita la chiave è basata solo sull'attributo `name`

Esempio di uso:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implementazioni IDistributedCache dell'helper tag di cache distribuita

Esistono due implementazioni di `IDistributedCache` integrate in ASP.NET Core. Una è basata su SQL Server e l'altra su Redis. I dettagli di queste implementazioni sono disponibili in <xref:performance/caching/distributed>. Entrambe le implementazioni implicano l'impostazione di un'istanza di `IDistributedCache` in *Startup.cs* di ASP.NET Core.

Nessun attributo di tag è associato specificamente all'uso di un'implementazione specifica di `IDistributedCache`.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
