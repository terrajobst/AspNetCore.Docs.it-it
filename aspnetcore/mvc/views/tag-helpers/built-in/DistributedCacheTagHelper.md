---
title: Helper di Tag della Cache distribuita | Documenti Microsoft
author: pkellner
description: Di seguito viene illustrato l'utilizzo di Helper di Tag della Cache
keywords: Helper per tag di ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper
ms.openlocfilehash: 2b260624fb2d85ab1a2625511397bcb4a85b6e77
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2017
---
# <a name="distributed-cache-tag-helper"></a>Helper di Tag Cache distribuita

Di [Peter Kellner](http://peterkellner.net) 


L'Helper di Tag della Cache distribuita consente di migliorare notevolmente le prestazioni dell'applicazione ASP.NET di base per la memorizzazione nella cache il contenuto a un'origine cache distribuita.

L'Helper di Tag della Cache distribuita eredita dalla stessa classe di base come l'Helper di Tag della Cache.  Tutti gli attributi associati all'Helper di Tag della Cache funziona anche nell'Helper di Tag distribuita.


L'Helper di Tag della Cache distribuita segue il **principio dipendenze esplicite** noto come **inserimento costruttore**.  In particolare, il `IDistributedCache` interfaccia contenitore viene passato al costruttore distribuito Cache Tag del supporto.  Se non specifica implementazione concreta di `IDistributedCache` è stata creata in `ConfigureServices`, disponibile in genere in startup.cs, quindi l'Helper di Tag della Cache distribuita verrà utilizzato lo stesso provider in memoria per l'archiviazione dei dati memorizzati nella cache come l'Helper di Tag di Cache basic.

## <a name="distributed-cache-tag-helper-attributes"></a>Attributi di Helper di Tag della Cache distribuita

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>abilitato su scade scade dopo scade-scorrevole variano in base all'intestazione variano da query variano da route variano da cookie variano da utente variano per priorità

Per le definizioni, vedere l'Helper di Tag della Cache. Helper di Tag della Cache distribuita eredita dalla stessa classe Helper di Tag della Cache in modo che tutti gli attributi siano comuni da Helper di Tag della Cache.

- - -

### <a name="name-required"></a>nome (obbligatorio)

| Tipo di attributo    | Valore di esempio     |
|----------------   |----------------   |
| string    | "my-distributed-cache-unique-key-101"     |

Obbligatorio `name` attributo viene utilizzato come chiave di cache archiviata per ogni istanza di un Helper di Tag della Cache distribuita.  A differenza di base Helper Tag di Cache che assegna una chiave a ogni istanza dell'Helper di Tag della Cache in base Razor pagina nome e il percorso dell'helper di tag nella pagina razor, l'Helper di Tag della Cache distribuita basi solo la chiave dell'attributo`name`

Esempio di utilizzo:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implementazioni IDistributedCache Helper di Tag della Cache distribuita

Esistono due implementazioni di `IDistributedCache` integrata in ASP.NET Core.  Una si basa sul **Sql Server** e l'altro è basato su **Redis**. Dettagli di queste implementazioni sono disponibili alla risorsa a cui fa riferimento denominata "utilizzo di una cache distribuita" di seguito. Entrambe le implementazioni implicano l'impostazione di un'istanza di `IDistributedCache` in ASP.NET Core **startup.cs**.

Non sono presenti attributi di tag specificamente associati utilizzando qualsiasi implementazione specifica di `IDistributedCache`.



- - -



## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
