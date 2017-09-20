---
title: Helper di Tag in componenti di base di ASP.NET MVC di memorizzare nella cache
author: pkellner
description: Di seguito viene illustrato l'utilizzo di Helper di Tag della Cache
keywords: Helper per tag di ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/CacheTagHelper
ms.openlocfilehash: 31c362a70e44c7178efe1c35b28a02fd712ac2b4
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Helper di Tag in componenti di base di ASP.NET MVC di memorizzare nella cache

Di [Peter Kellner](http://peterkellner.net) 


L'Helper di Tag della Cache consente di migliorare notevolmente le prestazioni dell'applicazione ASP.NET di base per la memorizzazione nella cache il contenuto per il provider di cache ASP.NET Core interno.

Il motore di visualizzazione Razor imposta il valore predefinito `expires-after` a venti minuti.

Il seguente codice Razor memorizza nella cache la data/ora:

```cshtml
<Cache>@DateTime.Now<Cache>
```

La prima richiesta per la pagina che contiene `CacheTagHelper` visualizzerà la data/ora corrente. Richieste aggiuntive Mostra il valore memorizzato nella cache fino a quando la cache scade (impostazione predefinita 20 minuti) o viene rimosso dalla memoria.

È possibile impostare la durata della cache con i seguenti attributi:

## <a name="cache-tag-helper-attributes"></a>Memorizzare nella cache gli attributi di Helper Tag

- - -

### <a name="enabled"></a>enabled    


| Tipo di attributo    | Valori validi      |
|----------------   |----------------   |
| boolean           | "true" (impostazione predefinita)  |
|                   | "false"   |


Determina se il contenuto racchiuso tra l'Helper di Tag della Cache è memorizzato nella cache. Il valore predefinito è `true`.  Se impostato su `false` questo Helper di Tag della Cache non avranno effetto la memorizzazione nella cache nell'output sottoposto a rendering.

Esempio:

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a>in scadenza 

| Tipo di attributo    | Valore di esempio     |
|----------------   |----------------   |
| DateTimeOffset    | "@new DateTime(2025,1,29,17,02,0)"    |


Imposta la data di scadenza assoluto. Nell'esempio seguente verrà memorizzati nella cache il contenuto dell'Helper di Tag della Cache fino a 17:02:00 su 29 gennaio 2025.

Esempio:

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a>scade dopo

| Tipo di attributo    | Valore di esempio     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(120)"    |


Imposta il periodo di tempo alla prima richiesta per memorizzare nella cache il contenuto. 

Esempio:

```cshtml
<Cache expires-on="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a>la variabile scade

| Tipo di attributo    | Valore di esempio     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(60)"     |


Imposta il tempo che una voce della cache deve essere eliminata se non ha ricevuto accessi.

Esempio:

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a>variano in base all'intestazione

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Stringa            | "User-Agent"                  |
|                   | "content-encoding, User-Agent" |

Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando vengono modificate. Nell'esempio seguente consente di monitorare il valore dell'intestazione `User-Agent`. Nell'esempio verrà memorizzati nella cache il contenuto per ogni diversi `User-Agent` presentati al server web.

Esempio:

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a>variano da query

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Stringa            | "Rendere"                |
|                   | "Creazione, modello" |

Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando cambia il valore dell'intestazione. Nell'esempio seguente vengono esaminati i valori di `Make` e `Model`.

Esempio:

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a>variano da route

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Stringa            | "Rendere"                |
|                   | "Creazione, modello" |

Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando il parametro di route dati valore o i valori di modifica. Esempio:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
*Index.cshtml*

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a>variano da cookie

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Stringa            | ". AspNetCore.Identity.Application"                |
|                   | ". AspNetCore.Identity.Application,HairColor" |

Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando i valori di intestazione modifica (s). Nell'esempio seguente vengono esaminate il cookie associato all'identità di ASP.NET. Quando un utente è autenticato da impostare il cookie di richiesta che attiva un aggiornamento della cache.

Esempio:

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a>variare dall'utente

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Booleano             | "true"                  |
|                     | "false" (predefinito) |

Specifica se deve reimpostare la cache quando l'utente connesso (o l'entità di contesto). L'utente corrente è noto anche come entità di contesto di richiesta e possono essere visualizzato in una visualizzazione Razor facendo riferimento a `@User.Identity.Name`.

Nell'esempio seguente vengono esaminate l'utente corrente registrato.  

Esempio:

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

Utilizzo di questo attributo mantiene il contenuto nella cache tramite un ciclo di accesso e di log.  Quando si utilizza `vary-by-user="true"`, un'azione di log e di log di estrazione invalida la cache per l'utente autenticato.  La cache viene invalidata in quanto viene generato un nuovo valore di cookie univoci per account di accesso. Cache viene mantenuta per lo stato anonimo se nessun cookie è presente o è scaduto. Ciò significa che se nessun utente è connesso, verrà mantenuta la cache.

- - -

### <a name="vary-by"></a>variano da

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Stringa             | "@Model"                 |


Consente la personalizzazione dell'Ottiene memorizzati nella cache i dati. Quando viene aggiornato l'oggetto di riferimento dalle modifiche del valore stringa dell'attributo, il contenuto dell'Helper di Tag della Cache. Spesso una concatenazione di valori del modello vengono assegnati a questo attributo.  In effetti, ciò significa che l'aggiornamento di uno dei valori concatenati invalida la cache.

Nell'esempio seguente si presuppone che il metodo del controller per il rendering le somme Visualizza il valore intero tra i due parametri di route, `myParam1` e `myParam2`e che restituisce come proprietà singolo modello. Quando la somma viene modificato, il contenuto dell'Helper di Tag della Cache viene eseguito il rendering e nuovamente memorizzata nella cache.  

Esempio:

Azione:

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*

```cshtml
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a>priority

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Enumerazione CacheItemPriority  | "Alto"                   |
|                    | "Basso" |
|                    | "NeverRemove" |
|                    | "Normal" |

Vengono fornite indicazioni di eliminazione della cache per il provider di cache predefiniti. Rimuove il server web `Low` quando è eccessivo della memoria nella cache prima le voci.

Esempio:

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

Il `priority` attributo non garantisce un livello specifico di memorizzazione della cache. `CacheItemPriority`è solo un suggerimento. Impostare questo attributo su `NeverRemove` non garantisce che la cache verrà mantenuta sempre. Vedere [risorse aggiuntive](#additional-resources) per ulteriori informazioni.

L'Helper di Tag della Cache dipende il [servizio cache di memoria](xref:performance/caching/memory). L'Helper di Tag di Cache aggiunge il servizio se non è stato aggiunto.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
