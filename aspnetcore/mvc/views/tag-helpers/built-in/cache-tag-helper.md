---
title: Helper tag di cache in ASP.NET Core MVC
author: pkellner
description: Descrive l'uso dell'helper tag di cache
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 51811ee1669a24a0fc4ce9bc67e782b61bff655c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Helper tag di cache in ASP.NET Core MVC

Di [Peter Kellner](http://peterkellner.net) 

L'helper tag di cache consente di migliorare notevolmente le prestazioni dell'app ASP.NET Core memorizzandone il contenuto nel provider di cache ASP.NET Core interno.

Il motore di visualizzazione Razor imposta il valore predefinito di `expires-after` su venti minuti.

Il seguente codice Razor memorizza nella cache la data/ora:

```cshtml
<cache>@DateTime.Now</cache>
```

La prima richiesta della pagina che contiene `CacheTagHelper` visualizza la data/ora corrente. Le richieste successive visualizzano il valore memorizzato nella cache fino a quando la cache scade (impostazione predefinita: 20 minuti) o viene rimossa da richieste d'uso della memoria.

È possibile impostare la durata della cache con i seguenti attributi:

## <a name="cache-tag-helper-attributes"></a>Attributi dell'helper tag di cache

- - -

### <a name="enabled"></a>enabled    


| Tipo di attributo    | Valori validi      |
|----------------   |----------------   |
| boolean           | "true" (impostazione predefinita)  |
|                   | "false"   |


Determina se il contenuto incluso nell'helper tag di cache è memorizzato nella cache. Il valore predefinito è `true`.  Se impostato su `false` l'helper tag di cache non applica la memorizzazione nella cache all'output sottoposto a rendering.

Esempio:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| Tipo di attributo    | Valore di esempio     |
|----------------   |----------------   |
| DateTimeOffset    | "@new DateTime(2025,1,29,17,02,0)"    |


Imposta un'ora di scadenza assoluta. Il codice di esempio seguente memorizza nella cache il contenuto dell'helper tag di cache fino alle 17:02 del 29 gennaio 2025.

Esempio:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| Tipo di attributo    | Valore di esempio     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(120)"    |


Imposta il tempo di memorizzazione del contenuto nella cache a partire dalla prima richiesta. 

Esempio:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>expires-sliding

| Tipo di attributo    | Valore di esempio     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(60)"     |


Imposta il tempo trascorso il quale un elemento memorizzato nella cache viene rimosso se non è stato usato.

Esempio:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>vary-by-header

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Stringa            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

Accetta un valore di intestazione singolo o un elenco delimitato da virgole di valori di intestazione che attivano un aggiornamento della cache quando vengono modificati. L'esempio seguente esegue il monitoraggio del valore dell'intestazione `User-Agent`. Il codice dell'esempio memorizza nella cache il contenuto di ogni singolo elemento `User-Agent` presentato al server Web.

Esempio:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>vary-by-query

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Stringa            | "Make"                |
|                   | "Make,Model" |

Accetta un valore di intestazione singolo o un elenco delimitato da virgole di valori di intestazione che attivano un aggiornamento della cache quando il valore dell'intestazione cambia. Il codice dell'esempio seguente esamina i valori di `Make` e `Model`.

Esempio:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>vary-by-route

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Stringa            | "Make"                |
|                   | "Make,Model" |

Accetta un valore di intestazione singolo o un elenco delimitato da virgole di valori di intestazione che attivano un aggiornamento della cache quando il o i valori del parametro dati della route cambiano. Esempio:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>vary-by-cookie

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Stringa            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

Accetta un valore di intestazione singolo o un elenco delimitato da virgole di valori di intestazione che attivano un aggiornamento della cache quando il o i valore dell'intestazione cambiano. Il codice dell'esempio seguente esamina il cookie associato ad ASP.NET Identity. Quando un utente è autenticato, questo è il cookie di richiesta da impostare per attivare un aggiornamento della cache.

Esempio:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>vary-by-user

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Booleano             | "true"                  |
|                     | "false" (impostazione predefinita) |

Specifica se è necessario reimpostare la cache quando l'utente registrato (o l'entità di contesto registrata) cambia. L'utente corrente è detto anche entità di sicurezza del contesto della richiesta e può essere visualizzato in una visualizzazione Razor mediante riferimento a `@User.Identity.Name`.

Il codice dell'esempio seguente esamina l'utente corrente registrato.  

Esempio:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Questo attributo consente di mantenere il contenuto nella cache durante un ciclo di accesso e chiusura sessione.  Quando si usa `vary-by-user="true"` un'azione di accesso e chiusura sessione invalida la cache per l'utente autenticato.  La cache viene invalidata perché al momento dell'accesso viene generato un nuovo valore di cookie univoco. La cache viene mantenuta per lo stato anonimo se non è presente nessun cookie o il cookie è scaduto. Ciò significa che se nessun utente ha eseguito l'accesso la cache viene mantenuta.

- - -

### <a name="vary-by"></a>vary-by

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| Stringa             | "@Model"                 |


Consente di personalizzare quali dati vengono memorizzati nella cache. Quando l'oggetto al quale fa riferimento il valore stringa dell'attributo cambia, il contenuto dell'helper tag di cache viene aggiornato. Spesso a questo attributo viene assegnata una concatenazione stringa di valori del modello.  Di fatto ciò significa che l'aggiornamento di uno qualsiasi dei valori concatenati invalida la cache.

L'esempio seguente presuppone che il metodo del controller che esegue il rendering della vista effettui la somma dei valori interi dei due parametri di route `myParam1` e `myParam2` e restituisca la somma come proprietà singola del modello. Quando questa somma cambia, il contenuto dell'helper tag di cache viene sottoposto a rendering e memorizzato di nuovo nella cache.  

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
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>priority

| Tipo di attributo    | Valori di esempio                |
|----------------   |----------------               |
| CacheItemPriority  | "High"                   |
|                    | "Low" |
|                    | "NeverRemove" |
|                    | "Normal" |

Offre indicazioni per la rimozione dalla cache al provider di cache incorporato. Se sottoposto a richieste di memoria intensive, il server Web rimuove inizialmente `Low` voci della cache.

Esempio:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

L'attributo `priority` non garantisce un livello specifico di mantenimento nella cache. `CacheItemPriority` è un semplice suggerimento. L'impostazione dell'attributo su `NeverRemove` non garantisce che la cache verrà sempre mantenuta. Per altre informazioni, vedere [Risorse aggiuntive](#additional-resources).

L'helper tag di cache dipende dal [servizio cache in memoria](xref:performance/caching/memory). L'helper tag di cache aggiunge il servizio se non è stato aggiunto.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Memorizzazione nella cache in memoria](xref:performance/caching/memory)
* [Introduzione a Identity](xref:security/authentication/identity)
