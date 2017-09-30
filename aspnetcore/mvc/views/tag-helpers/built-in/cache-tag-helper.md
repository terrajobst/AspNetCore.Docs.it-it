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
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: da5b7b3bf1aa01ee22edf9bd003d8f79a00a5d0b
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="61e2f-104">Helper di Tag in componenti di base di ASP.NET MVC di memorizzare nella cache</span><span class="sxs-lookup"><span data-stu-id="61e2f-104">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="61e2f-105">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="61e2f-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="61e2f-106">L'Helper di Tag della Cache consente di migliorare notevolmente le prestazioni dell'applicazione ASP.NET di base per la memorizzazione nella cache il contenuto per il provider di cache ASP.NET Core interno.</span><span class="sxs-lookup"><span data-stu-id="61e2f-106">The  Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="61e2f-107">Il motore di visualizzazione Razor imposta il valore predefinito `expires-after` a venti minuti.</span><span class="sxs-lookup"><span data-stu-id="61e2f-107">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="61e2f-108">Il seguente codice Razor memorizza nella cache la data/ora:</span><span class="sxs-lookup"><span data-stu-id="61e2f-108">The following Razor markup caches the date/time:</span></span>

```cshtml
<Cache>@DateTime.Now<Cache>
```

<span data-ttu-id="61e2f-109">La prima richiesta per la pagina che contiene `CacheTagHelper` visualizzerà la data/ora corrente.</span><span class="sxs-lookup"><span data-stu-id="61e2f-109">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="61e2f-110">Richieste aggiuntive Mostra il valore memorizzato nella cache fino a quando la cache scade (impostazione predefinita 20 minuti) o viene rimosso dalla memoria.</span><span class="sxs-lookup"><span data-stu-id="61e2f-110">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="61e2f-111">È possibile impostare la durata della cache con i seguenti attributi:</span><span class="sxs-lookup"><span data-stu-id="61e2f-111">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="61e2f-112">Memorizzare nella cache gli attributi di Helper Tag</span><span class="sxs-lookup"><span data-stu-id="61e2f-112">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="61e2f-113">enabled</span><span class="sxs-lookup"><span data-stu-id="61e2f-113">enabled</span></span>    


| <span data-ttu-id="61e2f-114">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="61e2f-114">Attribute Type</span></span>    | <span data-ttu-id="61e2f-115">Valori validi</span><span class="sxs-lookup"><span data-stu-id="61e2f-115">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="61e2f-116">boolean</span><span class="sxs-lookup"><span data-stu-id="61e2f-116">boolean</span></span>           | <span data-ttu-id="61e2f-117">"true" (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="61e2f-117">"true" (default)</span></span>  |
|                   | <span data-ttu-id="61e2f-118">"false"</span><span class="sxs-lookup"><span data-stu-id="61e2f-118">"false"</span></span>   |


<span data-ttu-id="61e2f-119">Determina se il contenuto racchiuso tra l'Helper di Tag della Cache è memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="61e2f-119">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="61e2f-120">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="61e2f-120">The default is `true`.</span></span>  <span data-ttu-id="61e2f-121">Se impostato su `false` questo Helper di Tag della Cache non avranno effetto la memorizzazione nella cache nell'output sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="61e2f-121">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="61e2f-122">Esempio:</span><span class="sxs-lookup"><span data-stu-id="61e2f-122">Example:</span></span>

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="61e2f-123">in scadenza</span><span class="sxs-lookup"><span data-stu-id="61e2f-123">expires-on</span></span> 

| <span data-ttu-id="61e2f-124">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="61e2f-124">Attribute Type</span></span>    | <span data-ttu-id="61e2f-125">Valore di esempio</span><span class="sxs-lookup"><span data-stu-id="61e2f-125">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="61e2f-126">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="61e2f-126">DateTimeOffset</span></span>    | <span data-ttu-id="61e2f-127">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="61e2f-127">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="61e2f-128">Imposta la data di scadenza assoluto.</span><span class="sxs-lookup"><span data-stu-id="61e2f-128">Sets an absolute expiration date.</span></span> <span data-ttu-id="61e2f-129">Nell'esempio seguente verrà memorizzati nella cache il contenuto dell'Helper di Tag della Cache fino a 17:02:00 su 29 gennaio 2025.</span><span class="sxs-lookup"><span data-stu-id="61e2f-129">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="61e2f-130">Esempio:</span><span class="sxs-lookup"><span data-stu-id="61e2f-130">Example:</span></span>

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="61e2f-131">scade dopo</span><span class="sxs-lookup"><span data-stu-id="61e2f-131">expires-after</span></span>

| <span data-ttu-id="61e2f-132">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="61e2f-132">Attribute Type</span></span>    | <span data-ttu-id="61e2f-133">Valore di esempio</span><span class="sxs-lookup"><span data-stu-id="61e2f-133">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="61e2f-134">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="61e2f-134">TimeSpan</span></span>    | <span data-ttu-id="61e2f-135">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="61e2f-135">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="61e2f-136">Imposta il periodo di tempo alla prima richiesta per memorizzare nella cache il contenuto.</span><span class="sxs-lookup"><span data-stu-id="61e2f-136">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="61e2f-137">Esempio:</span><span class="sxs-lookup"><span data-stu-id="61e2f-137">Example:</span></span>

```cshtml
<Cache expires-on="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="61e2f-138">la variabile scade</span><span class="sxs-lookup"><span data-stu-id="61e2f-138">expires-sliding</span></span>

| <span data-ttu-id="61e2f-139">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="61e2f-139">Attribute Type</span></span>    | <span data-ttu-id="61e2f-140">Valore di esempio</span><span class="sxs-lookup"><span data-stu-id="61e2f-140">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="61e2f-141">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="61e2f-141">TimeSpan</span></span>    | <span data-ttu-id="61e2f-142">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="61e2f-142">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="61e2f-143">Imposta il tempo che una voce della cache deve essere eliminata se non ha ricevuto accessi.</span><span class="sxs-lookup"><span data-stu-id="61e2f-143">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="61e2f-144">Esempio:</span><span class="sxs-lookup"><span data-stu-id="61e2f-144">Example:</span></span>

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="61e2f-145">variano in base all'intestazione</span><span class="sxs-lookup"><span data-stu-id="61e2f-145">vary-by-header</span></span>

| <span data-ttu-id="61e2f-146">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="61e2f-146">Attribute Type</span></span>    | <span data-ttu-id="61e2f-147">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="61e2f-147">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="61e2f-148">Stringa</span><span class="sxs-lookup"><span data-stu-id="61e2f-148">String</span></span>            | <span data-ttu-id="61e2f-149">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="61e2f-149">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="61e2f-150">"content-encoding, User-Agent"</span><span class="sxs-lookup"><span data-stu-id="61e2f-150">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="61e2f-151">Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando vengono modificate.</span><span class="sxs-lookup"><span data-stu-id="61e2f-151">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="61e2f-152">Nell'esempio seguente consente di monitorare il valore dell'intestazione `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="61e2f-152">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="61e2f-153">Nell'esempio verrà memorizzati nella cache il contenuto per ogni diversi `User-Agent` presentati al server web.</span><span class="sxs-lookup"><span data-stu-id="61e2f-153">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="61e2f-154">Esempio:</span><span class="sxs-lookup"><span data-stu-id="61e2f-154">Example:</span></span>

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="61e2f-155">variano da query</span><span class="sxs-lookup"><span data-stu-id="61e2f-155">vary-by-query</span></span>

| <span data-ttu-id="61e2f-156">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="61e2f-156">Attribute Type</span></span>    | <span data-ttu-id="61e2f-157">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="61e2f-157">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="61e2f-158">Stringa</span><span class="sxs-lookup"><span data-stu-id="61e2f-158">String</span></span>            | <span data-ttu-id="61e2f-159">"Rendere"</span><span class="sxs-lookup"><span data-stu-id="61e2f-159">"Make"</span></span>                |
|                   | <span data-ttu-id="61e2f-160">"Creazione, modello"</span><span class="sxs-lookup"><span data-stu-id="61e2f-160">"Make,Model"</span></span> |

<span data-ttu-id="61e2f-161">Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando cambia il valore dell'intestazione.</span><span class="sxs-lookup"><span data-stu-id="61e2f-161">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="61e2f-162">Nell'esempio seguente vengono esaminati i valori di `Make` e `Model`.</span><span class="sxs-lookup"><span data-stu-id="61e2f-162">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="61e2f-163">Esempio:</span><span class="sxs-lookup"><span data-stu-id="61e2f-163">Example:</span></span>

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="61e2f-164">variano da route</span><span class="sxs-lookup"><span data-stu-id="61e2f-164">vary-by-route</span></span>

| <span data-ttu-id="61e2f-165">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="61e2f-165">Attribute Type</span></span>    | <span data-ttu-id="61e2f-166">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="61e2f-166">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="61e2f-167">Stringa</span><span class="sxs-lookup"><span data-stu-id="61e2f-167">String</span></span>            | <span data-ttu-id="61e2f-168">"Rendere"</span><span class="sxs-lookup"><span data-stu-id="61e2f-168">"Make"</span></span>                |
|                   | <span data-ttu-id="61e2f-169">"Creazione, modello"</span><span class="sxs-lookup"><span data-stu-id="61e2f-169">"Make,Model"</span></span> |

<span data-ttu-id="61e2f-170">Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando il parametro di route dati valore o i valori di modifica.</span><span class="sxs-lookup"><span data-stu-id="61e2f-170">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="61e2f-171">Esempio:</span><span class="sxs-lookup"><span data-stu-id="61e2f-171">Example:</span></span>

<span data-ttu-id="61e2f-172">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="61e2f-172">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="61e2f-173">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="61e2f-173">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="61e2f-174">variano da cookie</span><span class="sxs-lookup"><span data-stu-id="61e2f-174">vary-by-cookie</span></span>

| <span data-ttu-id="61e2f-175">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="61e2f-175">Attribute Type</span></span>    | <span data-ttu-id="61e2f-176">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="61e2f-176">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="61e2f-177">Stringa</span><span class="sxs-lookup"><span data-stu-id="61e2f-177">String</span></span>            | <span data-ttu-id="61e2f-178">". AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="61e2f-178">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="61e2f-179">". AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="61e2f-179">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="61e2f-180">Accetta un valore di intestazione singolo o un elenco delimitato da virgole dei valori di intestazione che attivano un aggiornamento della cache quando i valori di intestazione modifica (s).</span><span class="sxs-lookup"><span data-stu-id="61e2f-180">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="61e2f-181">Nell'esempio seguente vengono esaminate il cookie associato all'identità di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="61e2f-181">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="61e2f-182">Quando un utente è autenticato da impostare il cookie di richiesta che attiva un aggiornamento della cache.</span><span class="sxs-lookup"><span data-stu-id="61e2f-182">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="61e2f-183">Esempio:</span><span class="sxs-lookup"><span data-stu-id="61e2f-183">Example:</span></span>

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="61e2f-184">variare dall'utente</span><span class="sxs-lookup"><span data-stu-id="61e2f-184">vary-by-user</span></span>

| <span data-ttu-id="61e2f-185">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="61e2f-185">Attribute Type</span></span>    | <span data-ttu-id="61e2f-186">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="61e2f-186">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="61e2f-187">Booleano</span><span class="sxs-lookup"><span data-stu-id="61e2f-187">Boolean</span></span>             | <span data-ttu-id="61e2f-188">"true"</span><span class="sxs-lookup"><span data-stu-id="61e2f-188">"true"</span></span>                  |
|                     | <span data-ttu-id="61e2f-189">"false" (predefinito)</span><span class="sxs-lookup"><span data-stu-id="61e2f-189">"false" (default)</span></span> |

<span data-ttu-id="61e2f-190">Specifica se deve reimpostare la cache quando l'utente connesso (o l'entità di contesto).</span><span class="sxs-lookup"><span data-stu-id="61e2f-190">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="61e2f-191">L'utente corrente è noto anche come entità di contesto di richiesta e possono essere visualizzato in una visualizzazione Razor facendo riferimento a `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="61e2f-191">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="61e2f-192">Nell'esempio seguente vengono esaminate l'utente corrente registrato.</span><span class="sxs-lookup"><span data-stu-id="61e2f-192">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="61e2f-193">Esempio:</span><span class="sxs-lookup"><span data-stu-id="61e2f-193">Example:</span></span>

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="61e2f-194">Utilizzo di questo attributo mantiene il contenuto nella cache tramite un ciclo di accesso e di log.</span><span class="sxs-lookup"><span data-stu-id="61e2f-194">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="61e2f-195">Quando si utilizza `vary-by-user="true"`, un'azione di log e di log di estrazione invalida la cache per l'utente autenticato.</span><span class="sxs-lookup"><span data-stu-id="61e2f-195">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="61e2f-196">La cache viene invalidata in quanto viene generato un nuovo valore di cookie univoci per account di accesso.</span><span class="sxs-lookup"><span data-stu-id="61e2f-196">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="61e2f-197">Cache viene mantenuta per lo stato anonimo se nessun cookie è presente o è scaduto.</span><span class="sxs-lookup"><span data-stu-id="61e2f-197">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="61e2f-198">Ciò significa che se nessun utente è connesso, verrà mantenuta la cache.</span><span class="sxs-lookup"><span data-stu-id="61e2f-198">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="61e2f-199">variano da</span><span class="sxs-lookup"><span data-stu-id="61e2f-199">vary-by</span></span>

| <span data-ttu-id="61e2f-200">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="61e2f-200">Attribute Type</span></span>    | <span data-ttu-id="61e2f-201">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="61e2f-201">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="61e2f-202">Stringa</span><span class="sxs-lookup"><span data-stu-id="61e2f-202">String</span></span>             | <span data-ttu-id="61e2f-203">"@Model"</span><span class="sxs-lookup"><span data-stu-id="61e2f-203">"@Model"</span></span>                 |


<span data-ttu-id="61e2f-204">Consente la personalizzazione dell'Ottiene memorizzati nella cache i dati.</span><span class="sxs-lookup"><span data-stu-id="61e2f-204">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="61e2f-205">Quando viene aggiornato l'oggetto di riferimento dalle modifiche del valore stringa dell'attributo, il contenuto dell'Helper di Tag della Cache.</span><span class="sxs-lookup"><span data-stu-id="61e2f-205">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="61e2f-206">Spesso una concatenazione di valori del modello vengono assegnati a questo attributo.</span><span class="sxs-lookup"><span data-stu-id="61e2f-206">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="61e2f-207">In effetti, ciò significa che l'aggiornamento di uno dei valori concatenati invalida la cache.</span><span class="sxs-lookup"><span data-stu-id="61e2f-207">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="61e2f-208">Nell'esempio seguente si presuppone che il metodo del controller per il rendering le somme Visualizza il valore intero tra i due parametri di route, `myParam1` e `myParam2`e che restituisce come proprietà singolo modello.</span><span class="sxs-lookup"><span data-stu-id="61e2f-208">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="61e2f-209">Quando la somma viene modificato, il contenuto dell'Helper di Tag della Cache viene eseguito il rendering e nuovamente memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="61e2f-209">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="61e2f-210">Esempio:</span><span class="sxs-lookup"><span data-stu-id="61e2f-210">Example:</span></span>

<span data-ttu-id="61e2f-211">Azione:</span><span class="sxs-lookup"><span data-stu-id="61e2f-211">Action:</span></span>

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

<span data-ttu-id="61e2f-212">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="61e2f-212">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="61e2f-213">priority</span><span class="sxs-lookup"><span data-stu-id="61e2f-213">priority</span></span>

| <span data-ttu-id="61e2f-214">Tipo di attributo</span><span class="sxs-lookup"><span data-stu-id="61e2f-214">Attribute Type</span></span>    | <span data-ttu-id="61e2f-215">Valori di esempio</span><span class="sxs-lookup"><span data-stu-id="61e2f-215">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="61e2f-216">Enumerazione CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="61e2f-216">CacheItemPriority</span></span>  | <span data-ttu-id="61e2f-217">"Alto"</span><span class="sxs-lookup"><span data-stu-id="61e2f-217">"High"</span></span>                   |
|                    | <span data-ttu-id="61e2f-218">"Basso"</span><span class="sxs-lookup"><span data-stu-id="61e2f-218">"Low"</span></span> |
|                    | <span data-ttu-id="61e2f-219">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="61e2f-219">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="61e2f-220">"Normal"</span><span class="sxs-lookup"><span data-stu-id="61e2f-220">"Normal"</span></span> |

<span data-ttu-id="61e2f-221">Vengono fornite indicazioni di eliminazione della cache per il provider di cache predefiniti.</span><span class="sxs-lookup"><span data-stu-id="61e2f-221">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="61e2f-222">Rimuove il server web `Low` quando è eccessivo della memoria nella cache prima le voci.</span><span class="sxs-lookup"><span data-stu-id="61e2f-222">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="61e2f-223">Esempio:</span><span class="sxs-lookup"><span data-stu-id="61e2f-223">Example:</span></span>

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="61e2f-224">Il `priority` attributo non garantisce un livello specifico di memorizzazione della cache.</span><span class="sxs-lookup"><span data-stu-id="61e2f-224">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="61e2f-225">`CacheItemPriority`è solo un suggerimento.</span><span class="sxs-lookup"><span data-stu-id="61e2f-225">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="61e2f-226">Impostare questo attributo su `NeverRemove` non garantisce che la cache verrà mantenuta sempre.</span><span class="sxs-lookup"><span data-stu-id="61e2f-226">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="61e2f-227">Vedere [risorse aggiuntive](#additional-resources) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="61e2f-227">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="61e2f-228">L'Helper di Tag della Cache dipende il [servizio cache di memoria](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="61e2f-228">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="61e2f-229">L'Helper di Tag di Cache aggiunge il servizio se non è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="61e2f-229">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61e2f-230">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="61e2f-230">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
