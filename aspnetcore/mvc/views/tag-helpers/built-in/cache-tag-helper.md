---
title: Helper di Tag in componenti di base di ASP.NET MVC di memorizzare nella cache
author: pkellner
description: Di seguito viene illustrato l'utilizzo di Helper di Tag della Cache
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 10aa1b493dbd0672cac789f6e48ddf2f14ba35dc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="ff1af-103">Helper di Tag in componenti di base di ASP.NET MVC di memorizzare nella cache</span><span class="sxs-lookup"><span data-stu-id="ff1af-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="ff1af-104">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="ff1af-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="ff1af-105">L'Helper di Tag della Cache consente di migliorare notevolmente le prestazioni dell'applicazione ASP.NET di base per la memorizzazione nella cache il contenuto per il provider di cache ASP.NET Core interno.</span><span class="sxs-lookup"><span data-stu-id="ff1af-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="ff1af-106">Il motore di visualizzazione Razor imposta il valore predefinito `expires-after` a venti minuti.</span><span class="sxs-lookup"><span data-stu-id="ff1af-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="ff1af-107">Il seguente codice Razor memorizza nella cache la data/ora:</span><span class="sxs-lookup"><span data-stu-id="ff1af-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="ff1af-108">La prima richiesta per la pagina che contiene `CacheTagHelper` visualizzerà la data/ora corrente.</span><span class="sxs-lookup"><span data-stu-id="ff1af-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="ff1af-109">Richieste aggiuntive Mostra il valore memorizzato nella cache fino a quando la cache scade (impostazione predefinita 20 minuti) o viene rimosso dalla memoria.</span><span class="sxs-lookup"><span data-stu-id="ff1af-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="ff1af-110">È possibile impostare la durata della cache con i seguenti attributi:</span><span class="sxs-lookup"><span data-stu-id="ff1af-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="ff1af-111">Memorizzare nella cache gli attributi di Helper Tag</span><span class="sxs-lookup"><span data-stu-id="ff1af-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="ff1af-112">enabled</span><span class="sxs-lookup"><span data-stu-id="ff1af-112">enabled</span></span>    


| <span data-ttu-id="ff1af-113">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="ff1af-113">Attribute Type</span></span>    | <span data-ttu-id="ff1af-114">Valori validi</span><span class="sxs-lookup"><span data-stu-id="ff1af-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="ff1af-115">boolean</span><span class="sxs-lookup"><span data-stu-id="ff1af-115">boolean</span></span>           | <span data-ttu-id="ff1af-116">"true" (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="ff1af-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="ff1af-117">"false"</span><span class="sxs-lookup"><span data-stu-id="ff1af-117">"false"</span></span>   |


<span data-ttu-id="ff1af-118">Determina se il contenuto racchiuso tra l'Helper di Tag della Cache è memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="ff1af-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="ff1af-119">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="ff1af-119">The default is `true`.</span></span>  <span data-ttu-id="ff1af-120">Se impostato su `false` questo Helper di Tag della Cache non avranno effetto la memorizzazione nella cache nell'output sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="ff1af-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="ff1af-121">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff1af-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="ff1af-122">in scadenza</span><span class="sxs-lookup"><span data-stu-id="ff1af-122">expires-on</span></span> 

| <span data-ttu-id="ff1af-123">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="ff1af-123">Attribute Type</span></span>    | <span data-ttu-id="ff1af-124">Valore di esempio</span><span class="sxs-lookup"><span data-stu-id="ff1af-124">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="ff1af-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ff1af-125">DateTimeOffset</span></span>    | <span data-ttu-id="ff1af-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="ff1af-126">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="ff1af-127">Imposta la data di scadenza assoluto.</span><span class="sxs-lookup"><span data-stu-id="ff1af-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="ff1af-128">Nell'esempio seguente verrà memorizzati nella cache il contenuto dell'Helper di Tag della Cache fino a 17:02:00 su 29 gennaio 2025.</span><span class="sxs-lookup"><span data-stu-id="ff1af-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="ff1af-129">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff1af-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="ff1af-130">scade dopo</span><span class="sxs-lookup"><span data-stu-id="ff1af-130">expires-after</span></span>

| <span data-ttu-id="ff1af-131">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="ff1af-131">Attribute Type</span></span>    | <span data-ttu-id="ff1af-132">Valore di esempio</span><span class="sxs-lookup"><span data-stu-id="ff1af-132">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="ff1af-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ff1af-133">TimeSpan</span></span>    | <span data-ttu-id="ff1af-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="ff1af-134">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="ff1af-135">Imposta il periodo di tempo alla prima richiesta per memorizzare nella cache il contenuto.</span><span class="sxs-lookup"><span data-stu-id="ff1af-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="ff1af-136">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff1af-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="ff1af-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="ff1af-137">expires-sliding</span></span>

| <span data-ttu-id="ff1af-138">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="ff1af-138">Attribute Type</span></span>    | <span data-ttu-id="ff1af-139">Valore di esempio</span><span class="sxs-lookup"><span data-stu-id="ff1af-139">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="ff1af-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ff1af-140">TimeSpan</span></span>    | <span data-ttu-id="ff1af-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="ff1af-141">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="ff1af-142">Imposta il tempo che una voce della cache deve essere eliminata se non ha ricevuto accessi.</span><span class="sxs-lookup"><span data-stu-id="ff1af-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="ff1af-143">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff1af-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="ff1af-144">variano in base all'intestazione</span><span class="sxs-lookup"><span data-stu-id="ff1af-144">vary-by-header</span></span>

| <span data-ttu-id="ff1af-145">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="ff1af-145">Attribute Type</span></span>    | <span data-ttu-id="ff1af-146">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="ff1af-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ff1af-147">Stringa</span><span class="sxs-lookup"><span data-stu-id="ff1af-147">String</span></span>            | <span data-ttu-id="ff1af-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="ff1af-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="ff1af-149">"content-encoding, User-Agent"</span><span class="sxs-lookup"><span data-stu-id="ff1af-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="ff1af-150">Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando vengono modificate.</span><span class="sxs-lookup"><span data-stu-id="ff1af-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="ff1af-151">Nell'esempio seguente consente di monitorare il valore dell'intestazione `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="ff1af-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="ff1af-152">Nell'esempio verrà memorizzati nella cache il contenuto per ogni diversi `User-Agent` presentati al server web.</span><span class="sxs-lookup"><span data-stu-id="ff1af-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="ff1af-153">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff1af-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="ff1af-154">variano da query</span><span class="sxs-lookup"><span data-stu-id="ff1af-154">vary-by-query</span></span>

| <span data-ttu-id="ff1af-155">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="ff1af-155">Attribute Type</span></span>    | <span data-ttu-id="ff1af-156">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="ff1af-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ff1af-157">Stringa</span><span class="sxs-lookup"><span data-stu-id="ff1af-157">String</span></span>            | <span data-ttu-id="ff1af-158">"Rendere"</span><span class="sxs-lookup"><span data-stu-id="ff1af-158">"Make"</span></span>                |
|                   | <span data-ttu-id="ff1af-159">"Creazione, modello"</span><span class="sxs-lookup"><span data-stu-id="ff1af-159">"Make,Model"</span></span> |

<span data-ttu-id="ff1af-160">Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando cambia il valore dell'intestazione.</span><span class="sxs-lookup"><span data-stu-id="ff1af-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="ff1af-161">Nell'esempio seguente vengono esaminati i valori di `Make` e `Model`.</span><span class="sxs-lookup"><span data-stu-id="ff1af-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="ff1af-162">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff1af-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="ff1af-163">variano da route</span><span class="sxs-lookup"><span data-stu-id="ff1af-163">vary-by-route</span></span>

| <span data-ttu-id="ff1af-164">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="ff1af-164">Attribute Type</span></span>    | <span data-ttu-id="ff1af-165">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="ff1af-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ff1af-166">Stringa</span><span class="sxs-lookup"><span data-stu-id="ff1af-166">String</span></span>            | <span data-ttu-id="ff1af-167">"Rendere"</span><span class="sxs-lookup"><span data-stu-id="ff1af-167">"Make"</span></span>                |
|                   | <span data-ttu-id="ff1af-168">"Creazione, modello"</span><span class="sxs-lookup"><span data-stu-id="ff1af-168">"Make,Model"</span></span> |

<span data-ttu-id="ff1af-169">Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando il parametro di route dati valore o i valori di modifica.</span><span class="sxs-lookup"><span data-stu-id="ff1af-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="ff1af-170">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff1af-170">Example:</span></span>

<span data-ttu-id="ff1af-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="ff1af-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="ff1af-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ff1af-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="ff1af-173">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="ff1af-173">vary-by-cookie</span></span>

| <span data-ttu-id="ff1af-174">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="ff1af-174">Attribute Type</span></span>    | <span data-ttu-id="ff1af-175">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="ff1af-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ff1af-176">Stringa</span><span class="sxs-lookup"><span data-stu-id="ff1af-176">String</span></span>            | <span data-ttu-id="ff1af-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="ff1af-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="ff1af-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="ff1af-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="ff1af-179">Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando i valori di intestazione modifica (s).</span><span class="sxs-lookup"><span data-stu-id="ff1af-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="ff1af-180">Nell'esempio seguente vengono esaminate il cookie associato all'identità di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ff1af-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="ff1af-181">Quando un utente è autenticato da impostare il cookie di richiesta che attiva un aggiornamento della cache.</span><span class="sxs-lookup"><span data-stu-id="ff1af-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="ff1af-182">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff1af-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="ff1af-183">variare dall'utente</span><span class="sxs-lookup"><span data-stu-id="ff1af-183">vary-by-user</span></span>

| <span data-ttu-id="ff1af-184">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="ff1af-184">Attribute Type</span></span>    | <span data-ttu-id="ff1af-185">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="ff1af-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ff1af-186">Booleano</span><span class="sxs-lookup"><span data-stu-id="ff1af-186">Boolean</span></span>             | <span data-ttu-id="ff1af-187">"true"</span><span class="sxs-lookup"><span data-stu-id="ff1af-187">"true"</span></span>                  |
|                     | <span data-ttu-id="ff1af-188">"false" (predefinito)</span><span class="sxs-lookup"><span data-stu-id="ff1af-188">"false" (default)</span></span> |

<span data-ttu-id="ff1af-189">Specifica se deve reimpostare la cache quando l'utente connesso (o l'entità di contesto).</span><span class="sxs-lookup"><span data-stu-id="ff1af-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="ff1af-190">L'utente corrente è noto anche come entità di contesto di richiesta e possono essere visualizzato in una visualizzazione Razor facendo riferimento a `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="ff1af-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="ff1af-191">Nell'esempio seguente vengono esaminate l'utente corrente registrato.</span><span class="sxs-lookup"><span data-stu-id="ff1af-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="ff1af-192">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff1af-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="ff1af-193">Utilizzo di questo attributo mantiene il contenuto nella cache tramite un ciclo di accesso e di log.</span><span class="sxs-lookup"><span data-stu-id="ff1af-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="ff1af-194">Quando si utilizza `vary-by-user="true"`, un'azione di log e di log di estrazione invalida la cache per l'utente autenticato.</span><span class="sxs-lookup"><span data-stu-id="ff1af-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="ff1af-195">La cache viene invalidata in quanto viene generato un nuovo valore di cookie univoci per account di accesso.</span><span class="sxs-lookup"><span data-stu-id="ff1af-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="ff1af-196">Cache viene mantenuta per lo stato anonimo se nessun cookie è presente o è scaduto.</span><span class="sxs-lookup"><span data-stu-id="ff1af-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="ff1af-197">Ciò significa che se nessun utente è connesso, verrà mantenuta la cache.</span><span class="sxs-lookup"><span data-stu-id="ff1af-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="ff1af-198">variano da</span><span class="sxs-lookup"><span data-stu-id="ff1af-198">vary-by</span></span>

| <span data-ttu-id="ff1af-199">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="ff1af-199">Attribute Type</span></span>    | <span data-ttu-id="ff1af-200">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="ff1af-200">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ff1af-201">Stringa</span><span class="sxs-lookup"><span data-stu-id="ff1af-201">String</span></span>             | <span data-ttu-id="ff1af-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="ff1af-202">"@Model"</span></span>                 |


<span data-ttu-id="ff1af-203">Consente la personalizzazione dell'Ottiene memorizzati nella cache i dati.</span><span class="sxs-lookup"><span data-stu-id="ff1af-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="ff1af-204">Quando viene aggiornato l'oggetto di riferimento dalle modifiche del valore stringa dell'attributo, il contenuto dell'Helper di Tag della Cache.</span><span class="sxs-lookup"><span data-stu-id="ff1af-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="ff1af-205">Spesso una concatenazione di valori del modello vengono assegnati a questo attributo.</span><span class="sxs-lookup"><span data-stu-id="ff1af-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="ff1af-206">In effetti, ciò significa che l'aggiornamento di uno dei valori concatenati invalida la cache.</span><span class="sxs-lookup"><span data-stu-id="ff1af-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="ff1af-207">Nell'esempio seguente si presuppone che il metodo del controller per il rendering le somme Visualizza il valore intero tra i due parametri di route, `myParam1` e `myParam2`e che restituisce come proprietà singolo modello.</span><span class="sxs-lookup"><span data-stu-id="ff1af-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="ff1af-208">Quando la somma viene modificato, il contenuto dell'Helper di Tag della Cache viene eseguito il rendering e nuovamente memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="ff1af-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="ff1af-209">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff1af-209">Example:</span></span>

<span data-ttu-id="ff1af-210">Azione:</span><span class="sxs-lookup"><span data-stu-id="ff1af-210">Action:</span></span>

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

<span data-ttu-id="ff1af-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ff1af-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="ff1af-212">priority</span><span class="sxs-lookup"><span data-stu-id="ff1af-212">priority</span></span>

| <span data-ttu-id="ff1af-213">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="ff1af-213">Attribute Type</span></span>    | <span data-ttu-id="ff1af-214">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="ff1af-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="ff1af-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="ff1af-215">CacheItemPriority</span></span>  | <span data-ttu-id="ff1af-216">"Alto"</span><span class="sxs-lookup"><span data-stu-id="ff1af-216">"High"</span></span>                   |
|                    | <span data-ttu-id="ff1af-217">"Basso"</span><span class="sxs-lookup"><span data-stu-id="ff1af-217">"Low"</span></span> |
|                    | <span data-ttu-id="ff1af-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="ff1af-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="ff1af-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="ff1af-219">"Normal"</span></span> |

<span data-ttu-id="ff1af-220">Vengono fornite indicazioni di eliminazione della cache per il provider di cache predefiniti.</span><span class="sxs-lookup"><span data-stu-id="ff1af-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="ff1af-221">Rimuove il server web `Low` quando è eccessivo della memoria nella cache prima le voci.</span><span class="sxs-lookup"><span data-stu-id="ff1af-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="ff1af-222">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff1af-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="ff1af-223">Il `priority` attributo non garantisce un livello specifico di memorizzazione della cache.</span><span class="sxs-lookup"><span data-stu-id="ff1af-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="ff1af-224">`CacheItemPriority`è solo un suggerimento.</span><span class="sxs-lookup"><span data-stu-id="ff1af-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="ff1af-225">Impostare questo attributo su `NeverRemove` non garantisce che la cache verrà mantenuta sempre.</span><span class="sxs-lookup"><span data-stu-id="ff1af-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="ff1af-226">Vedere [risorse aggiuntive](#additional-resources) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="ff1af-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="ff1af-227">L'Helper di Tag della Cache dipende il [servizio cache di memoria](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="ff1af-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="ff1af-228">L'Helper di Tag di Cache aggiunge il servizio se non è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="ff1af-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff1af-229">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ff1af-229">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
