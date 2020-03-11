---
title: Helper tag di cache distribuita in ASP.NET Core
author: pkellner
description: Informazioni su come usare l'helper tag di cache distribuita.
ms.author: riande
ms.custom: mvc
ms.date: 01/24/2020
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: f5957adf3cef8966812a1bf0cbc6b2627d19d026
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664017"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Helper tag di cache distribuita in ASP.NET Core

Di [Peter Kellner](https://peterkellner.net)

L'helper tag di cache distribuita consente di migliorare notevolmente le prestazioni dell'app ASP.NET Core memorizzandone il contenuto in un'origine cache distribuita.

Per una panoramica degli helper tag, vedere <xref:mvc/views/tag-helpers/intro>.

L'helper tag di cache distribuita eredita dalla stessa classe di base da cui eredita l'helper tag di cache. Tutti gli attributi dell'[helper tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) sono disponibili per l'helper tag di cache distribuita.

L'helper tag di cache distribuita usa l'[inserimento del costruttore](xref:fundamentals/dependency-injection#constructor-injection-behavior). L'interfaccia <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> viene passata nel costruttore dell'helper tag di cache distribuita. Se non viene creata alcuna implementazione concreta di `IDistributedCache` in `Startup.ConfigureServices` (*Startup.cs*), l'helper tag di cache distribuita usa lo stesso provider in memoria per l'archiviazione dei dati memorizzati nella cache dell'[helper tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

## <a name="distributed-cache-tag-helper-attributes"></a>Attributi dell'helper per tag di cache distribuita

### <a name="attributes-shared-with-the-cache-tag-helper"></a>Attributi condivisi con l'helper tag di cache

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

L'helper tag di cache distribuita eredita dalla stessa classe dell'helper tag di cache. Per una descrizione di questi attributi, vedere [Helper tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="name"></a>name

| Tipo di attributo | Esempio                               |
| -------------- | ------------------------------------- |
| string         | `my-distributed-cache-unique-key-101` |

`name` è obbligatorio. L'attributo `name` viene usato come chiave per ogni istanza di cache archiviata. A differenza dell'helper tag di cache, che assegna una chiave di cache a ogni istanza in base al nome della pagina Razor e alla posizione nella pagina Razor, nell'helper tag di cache distribuita la chiave è basata solo sull'attributo `name`.

Esempio:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implementazioni IDistributedCache dell'helper tag di cache distribuita

Esistono due implementazioni di <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> integrate in ASP.NET Core. Una è basata su SQL Server e l'altra su Redis. Sono inoltre disponibili implementazioni di terze parti, ad esempio [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html). I dettagli di queste implementazioni sono disponibili in <xref:performance/caching/distributed>. Entrambe le implementazioni implicano l'impostazione di un'istanza di `IDistributedCache` in `Startup`.

Nessun attributo di tag è associato specificamente all'uso di un'implementazione specifica di `IDistributedCache`.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
