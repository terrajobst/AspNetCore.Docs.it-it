---
title: La memorizzazione nella cache di risposta in ASP.NET Core
author: rick-anderson
description: Informazioni su come usare la memorizzazione nella cache delle risposte per ridurre i requisiti di larghezza di banda e migliorare le prestazioni delle app ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: 99093cd281ffa8dddc574dc27254c0175e2651b3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207368"
---
# <a name="response-caching-in-aspnet-core"></a>La memorizzazione nella cache di risposta in ASP.NET Core

Dal [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), e [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> La memorizzazione nella cache di risposta in Razor Pages è disponibile in ASP.NET Core 2.1 o versione successiva.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

Risposta di memorizzazione nella cache riduce il numero di richieste di che un client o un proxy effettua a un server web. Risposta di memorizzazione nella cache riduce anche la quantità di lavoro esegue il server web per generare una risposta. La memorizzazione nella cache di risposta viene controllato dalle intestazioni che specificano la modalità client, proxy e middleware per memorizzare le risposte.

Il server web può memorizzare nella cache le risposte quando si aggiungono [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>La memorizzazione nella cache di risposta basato su HTTP

Il [specifica la memorizzazione nella cache di HTTP 1.1](https://tools.ietf.org/html/rfc7234) descrive il comportamento delle cache di Internet. L'intestazione HTTP principale usato per la memorizzazione nella cache [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), che consente di specificare la cache *direttive*. Le direttive controllano il comportamento di memorizzazione nella cache come richieste arrivino dai client ai server e come risposte arrivino dal server ai client. Richieste e risposte spostano tra i server proxy e server proxy devono inoltre essere conforme alla specifica la memorizzazione nella cache di HTTP 1.1.

Common `Cache-Control` direttive vengono visualizzate nella tabella seguente.

| Direttiva                                                       | Operazione |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Una cache può archiviare la risposta. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | La risposta non deve essere archiviata da una cache condivisa. Una cache privata può archiviare e riutilizzare la risposta. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Il client non accetta una risposta cui età è maggiore del numero specificato di secondi. Esempi: `max-age=60` (60 secondi), `max-age=2592000` (mese) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **Nelle richieste**: una cache non deve utilizzare una risposta memorizzata per soddisfare la richiesta. Nota: Il server di origine genera nuovamente la risposta per il client e il middleware Aggiorna la risposta memorizzata nella cache.<br><br>**Nelle risposte**: la risposta non deve essere usata per una richiesta successiva senza convalida nel server di origine. |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **Nelle richieste**: una cache non deve memorizzare la richiesta.<br><br>**Nelle risposte**: una cache non deve memorizzare qualsiasi parte della risposta. |

Nella tabella seguente vengono visualizzate altre intestazioni di cache che svolgono un ruolo di memorizzazione nella cache.

| Intestazione                                                     | Funzione |
| ---------------------------------------------------------- | -------- |
| [Età](https://tools.ietf.org/html/rfc7234#section-5.1)     | Stima della quantità di tempo in secondi, poiché la risposta è stata generata o convalidata nel server di origine. |
| [Alla scadenza](https://tools.ietf.org/html/rfc7234#section-5.3) | La data/ora dopo il quale la risposta viene considerata non aggiornata. |
| [(Pragma)](https://tools.ietf.org/html/rfc7234#section-5.4)  | Per le versioni precedenti la compatibilità con HTTP/1.0 vengono memorizzati nella cache per l'impostazione esiste `no-cache` comportamento. Se il `Cache-Control` intestazione è presente, il `Pragma` intestazione viene ignorata. |
| [Variare](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Specifica che una risposta memorizzata nella cache non deve essere inviata a meno che non tutti del `Vary` corrispondano i campi di intestazione nella richiesta originale della risposta memorizzata nella cache sia la nuova richiesta. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>Gli aspetti di memorizzazione nella cache basata su HTTP richiesta delle direttive Cache-Control

Il [specifica la memorizzazione nella cache di HTTP 1.1 per l'intestazione Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) richiede una cache a rispettare un valido `Cache-Control` intestazione inviato dal client. Un client può eseguire richieste con un `no-cache` valore dell'intestazione e forza il server a generare una nuova risposta per ogni richiesta.

Rispetta sempre client `Cache-Control` intestazioni della richiesta ha senso se si prende in considerazione l'obiettivo della memorizzazione nella cache HTTP. Con la specifica ufficiale, la memorizzazione nella cache è progettata per ridurre il sovraccarico di latenza e la rete di soddisfare le richieste attraverso una rete di client, proxy e server. Non è necessariamente un modo per controllare il carico su un server di origine.

Non vi è alcun controllo per gli sviluppatori corrente su questo comportamento di memorizzazione nella cache quando si usa la [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware) poiché il middleware è conforme a ufficiale la memorizzazione nella cache specifica. [I miglioramenti futuri al middleware](https://github.com/aspnet/ResponseCaching/issues/96) permetterà di configurare il middleware per ignorare una richiesta `Cache-Control` intestazione quando si decide di gestire una risposta memorizzata nella cache. Ciò offrirà un'opportunità per controllare meglio il carico sul server quando si usa il middleware.

## <a name="other-caching-technology-in-aspnet-core"></a>Altre tecnologie di memorizzazione nella cache in ASP.NET Core

### <a name="in-memory-caching"></a>La memorizzazione nella cache in memoria

La memorizzazione nella cache in memoria utilizza la memoria del server per archiviare i dati memorizzati nella cache. Questo tipo di memorizzazione nella cache è adatto per uno o più server utilizzando *sessioni permanenti*. Le sessioni permanenti significa che le richieste da un client vengano sempre indirizzate allo stesso server per l'elaborazione.

Per altre informazioni, vedere [memorizza nella Cache in memoria](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Cache distribuita

Usare una cache distribuita per archiviare i dati in memoria quando l'app è ospitata in una farm di server o cloud. La cache viene condivisa tra i server di elaborazione delle richieste. Un client può inviare una richiesta che viene gestita da qualsiasi server nel gruppo, se i dati memorizzati nella cache per il client sono disponibili. ASP.NET Core offre SQL Server e le cache distribuite Redis.

Per altre informazioni, vedere <xref:performance/caching/distributed>.

### <a name="cache-tag-helper"></a>Helper Tag di cache

È possibile memorizzare nella cache il contenuto da una visualizzazione MVC o una pagina Razor con l'Helper Tag di Cache. L'Helper Tag di Cache Usa la memorizzazione nella cache in memoria per archiviare i dati.

Per altre informazioni, vedere [Helper Tag di Cache in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Helper tag di cache distribuita

È possibile memorizzare nella cache il contenuto da una visualizzazione MVC o una pagina Razor in scenari web farm o cloud distribuite con l'Helper Tag di Cache distribuita. L'Helper Tag di Cache distribuita Usa Redis o SQL Server per archiviare i dati.

Per altre informazioni, vedere [Helper Tag di Cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>Attributo ResponseCache

Il [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifica i parametri necessari per l'impostazione delle intestazioni appropriate nella cache delle risposte.

> [!WARNING]
> Disabilitare la memorizzazione nella cache per il contenuto che contiene informazioni relative ai client autenticati. La memorizzazione nella cache deve essere abilitata solo per contenuto che non cambia in base a identità di un utente o se un utente è connesso.

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varia in base alla risposta stored dai valori di elenco specificato di chiavi di query. Quando un singolo valore di `*` è specificato, il middleware varia le risposte da tutti i parametri della stringa di query di richiesta. `VaryByQueryKeys` richiede ASP.NET Core 1.1 o versione successiva.

È necessario abilitare il Middleware di memorizzazione nella cache della risposta per impostare il `VaryByQueryKeys` proprietà; in caso contrario, viene generata un'eccezione di runtime. Non c'è un'intestazione HTTP corrispondente per il `VaryByQueryKeys` proprietà. La proprietà è una funzionalità HTTP gestita dal Middleware di memorizzazione nella cache delle risposte. Per il middleware servire una risposta memorizzata nella cache, la stringa di query e il valore di stringa di query deve corrispondere una richiesta precedente. Si consideri, ad esempio, la sequenza di richieste e i risultati mostrati nella tabella seguente.

| Richiesta                          | Risultato                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | Restituito dal server     |
| `http://example.com?key1=value1` | Restituito dal middleware |
| `http://example.com?key1=value2` | Restituito dal server     |

La prima richiesta viene restituita dal server e memorizzati nella cache nel middleware. La seconda richiesta viene restituita dal middleware in quanto la stringa di query corrisponde alla richiesta precedente. La terza richiesta non è nella cache del middleware in quanto il valore di stringa di query non corrisponde una richiesta precedente. 

Il `ResponseCacheAttribute` viene usato per configurare e creare (tramite `IFilterFactory`) un [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter). Il `ResponseCacheFilter` esegua l'operazione di aggiornamento delle intestazioni HTTP appropriate e le funzionalità di risposta. Il filtro:

* Rimuove tutte le intestazioni esistenti per `Vary`, `Cache-Control`, e `Pragma`. 
* Scrive le intestazioni appropriate in base alle proprietà impostate `ResponseCacheAttribute`. 
* Aggiorna la risposta HTTP funzionalità di cache se `VaryByQueryKeys` è impostata.

### <a name="vary"></a>Variare

Questa intestazione viene scritto solo quando il `VaryByHeader` è impostata. È impostato sul `Vary` valore della proprietà. L'esempio seguente usa il `VaryByHeader` proprietà:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

È possibile visualizzare le intestazioni della risposta con gli strumenti di rete del browser. L'immagine seguente mostra il F12 Edge in uscita nel **Network** scheda quando il `About2` viene aggiornato il metodo di azione:

![Bordo F12 output nella scheda rete quando viene chiamato il metodo di azione About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore e Location.None

`NoStore` esegue l'override la maggior parte delle altre proprietà. Quando questa proprietà è impostata su `true`, il `Cache-Control` intestazione è impostata su `no-store`. Se `Location` è impostata su `None`:

* `Cache-Control` è impostato su `no-store,no-cache`.
* `Pragma` è impostato su `no-cache`.

Se `NoStore` viene `false` e `Location` viene `None`, `Cache-Control` e `Pragma` sono impostati su `no-cache`.

In genere impostata `NoStore` a `true` nelle pagine di errore. Ad esempio:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

Ciò comporta le intestazioni seguenti:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Percorso e la durata

Per abilitare la memorizzazione nella cache, `Duration` deve essere impostata su un valore positivo e `Location` deve essere `Any` (predefinito) o `Client`. In questo caso, il `Cache-Control` intestazione è impostata sul valore di percorso aggiungendo la `max-age` della risposta.

> [!NOTE]
> `Location`di opzioni di `Any` e `Client` tradurre in `Cache-Control` valori di intestazione `public` e `private`rispettivamente. Come indicato in precedenza, l'impostazione `Location` al `None` imposta entrambe `Cache-Control` e `Pragma` intestazioni `no-cache`.

Di seguito è riportato un esempio che mostra le intestazioni di prodotto impostando `Duration` e lasciare il valore predefinito `Location` valore:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

Questo codice produce l'intestazione seguente:

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Profili della cache

Anziché ripetere `ResponseCache` impostazioni negli attributi di molte azioni di controller, sui profili cache possono essere configurate come opzioni quando si configura MVC nel `ConfigureServices` metodo `Startup`. Vengono usati valori trovati in un profilo della cache di cui viene fatto riferimento come le impostazioni predefinite per il `ResponseCache` attributo e vengono sovrascritte dalle eventuali proprietà specificate per l'attributo.

Configurazione di un profilo della cache:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

Che fanno riferimento a un profilo della cache:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

Il `ResponseCache` attributo può essere applicato sia alle azioni (metodi) e controller (classi). Gli attributi a livello di metodo sostituiscono le impostazioni specificate negli attributi a livello di classe.

Nell'esempio precedente, un attributo a livello di classe specifica una durata di 30 secondi, mentre un attributo a livello di metodo fa riferimento a un profilo della cache con una durata di 60 secondi.

L'intestazione risulta:

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [La memorizzazione delle risposte nella cache](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
