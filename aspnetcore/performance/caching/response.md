---
title: La memorizzazione nella cache di risposta in ASP.NET Core
author: rick-anderson
description: Imparare a usare la risposta alla memorizzazione nella cache minori requisiti di larghezza di banda e migliorare le prestazioni delle applicazioni ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: c53ae3f6ab8d26588533772dd4fdacb36ec12059
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077764"
---
# <a name="response-caching-in-aspnet-core"></a>La memorizzazione nella cache di risposta in ASP.NET Core

Dal [Giorgio Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), e [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> La memorizzazione nella cache nelle pagine Razor di risposta è disponibile in ASP.NET Core 2.1 o versione successiva.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

Risposta di memorizzazione nella cache riduce il numero di richieste di che un client o proxy effettua a un server web. Cache delle risposte consente anche di ridurre la quantità di lavoro esegue il server web per generare una risposta. La memorizzazione nella cache di risposta è controllata dalle intestazioni che specificano la modalità client, proxy e middleware per memorizzare risposte.

Il server web può memorizzare nella cache le risposte quando si aggiungono [Middleware la memorizzazione nella cache risposta](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>La memorizzazione nella cache basata su HTTP risposta

Il [specifica la memorizzazione nella cache di HTTP 1.1](https://tools.ietf.org/html/rfc7234) viene descritto il comportamento delle cache di Internet. L'intestazione HTTP principale utilizzato per la memorizzazione nella cache [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), che consente di specificare la cache *direttive*. Le direttive controllano la memorizzazione nella cache le richieste arrivino dai client ai server man mano e risposte arrivino dal server ai client. Richieste e risposte spostano tra i server proxy e server proxy devono inoltre essere conforme alla specifica la memorizzazione nella cache di HTTP 1.1.

Comuni `Cache-Control` direttive vengono visualizzate nella tabella seguente.

| Direttiva                                                       | Operazione |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Una cache può archiviare la risposta. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | La risposta non deve essere archiviata per una cache condivisa. Una cache privata può archiviare e riutilizzare la risposta. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Il client non accetta una risposta cui età è maggiore del numero specificato di secondi. Esempi: `max-age=60` (60 secondi), `max-age=2592000` (mese) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **Nelle richieste**: una cache non deve utilizzare una risposta memorizzata per soddisfare la richiesta. Nota: Il server di origine genera nuovamente la risposta per il client e il middleware Aggiorna la risposta memorizzata nella cache.<br><br>**Sulle risposte**: la risposta non deve essere usata per una richiesta successiva senza la convalida sul server di origine. |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **Nelle richieste**: una cache non è consentito archiviare la richiesta.<br><br>**Sulle risposte**: una cache non è consentito archiviare qualsiasi parte della risposta. |

Altre intestazioni cache che svolgono un ruolo nella memorizzazione nella cache vengono visualizzati nella tabella seguente.

| Header                                                     | Funzione |
| ---------------------------------------------------------- | -------- |
| [Età](https://tools.ietf.org/html/rfc7234#section-5.1)     | Stima della quantità di tempo in secondi trascorsi da quando viene generata la risposta o convalidata nel server di origine. |
| [Alla scadenza](https://tools.ietf.org/html/rfc7234#section-5.3) | La data/ora dopo il quale la risposta viene considerata non aggiornata. |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Per garantire la compatibilità con HTTP/1.0 vengono memorizzati nella cache per impostazione esiste `no-cache` comportamento. Se il `Cache-Control` intestazione è presente, il `Pragma` intestazione viene ignorata. |
| [Variare](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Specifica che una risposta memorizzata nella cache non deve essere inviata a meno che non tutti del `Vary` intestazione campi delle corrispondenze nella richiesta originale della risposta memorizzata nella cache sia la nuova richiesta. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>Gli aspetti di memorizzazione nella cache basata su HTTP richiesta direttive Cache-Control

Il [specifica la memorizzazione nella cache di HTTP 1.1 per l'intestazione Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) richiede una cache per rispettare un valido `Cache-Control` intestazione inviata dal client. Un client può inviare le richieste con un `no-cache` valore dell'intestazione e forza il server a generare una nuova risposta per ogni richiesta.

Sempre rispettando la distinzione tra client `Cache-Control` intestazioni della richiesta ha senso se si considera l'obiettivo della memorizzazione nella cache HTTP. In specifica ufficiale, la memorizzazione nella cache è progettata per ridurre il sovraccarico di latenza e la rete di soddisfare le richieste attraverso una rete di client, proxy e server. Non è necessariamente un modo per controllare il carico sul server di origine.

Sia presente alcun controllo sviluppatore corrente su questo comportamento di memorizzazione nella cache quando si utilizza il [risposta la memorizzazione nella cache Middleware](xref:performance/caching/middleware) perché il middleware rispetti ufficiali la memorizzazione nella cache specifica. [Miglioramenti futuri correlati al middleware](https://github.com/aspnet/ResponseCaching/issues/96) consentiranno di configurare il middleware per ignorare una richiesta `Cache-Control` intestazione quando si decide di servire una risposta memorizzata nella cache. Questo fornirà è la possibilità di maggiore controllo il carico sul server quando si utilizza il middleware.

## <a name="other-caching-technology-in-aspnet-core"></a>Altre tecnologie di memorizzazione nella cache in ASP.NET Core

### <a name="in-memory-caching"></a>La memorizzazione nella cache in memoria

La memorizzazione nella cache in memoria utilizza la memoria del server per archiviare i dati memorizzati nella cache. Questo tipo di memorizzazione nella cache è adatto per uno o più server tramite *sessioni permanenti*. Le sessioni permanenti significa che le richieste da un client vengono sempre indirizzate allo stesso server per l'elaborazione.

Per altre informazioni, vedere [memorizza nella Cache in memoria](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Cache distribuita

Utilizzare una cache distribuita per memorizzare dati in memoria quando l'applicazione è ospitata in una farm di server o cloud. La cache viene condivisa tra il server di elaborazione delle richieste. Un client può inviare una richiesta che viene gestita da qualsiasi server nel gruppo di dati memorizzati nella cache per il client sono disponibili. ASP.NET Core offre le cache Redis distribuiti e SQL Server.

Per altre informazioni, vedere [funziona con una cache distribuita](xref:performance/caching/distributed).

### <a name="cache-tag-helper"></a>Helper di Tag della cache

È possibile memorizzare nella cache il contenuto da una visualizzazione MVC o pagina Razor con l'Helper di Tag della Cache. L'Helper di Tag della Cache utilizza la memorizzazione nella cache in memoria per archiviare i dati.

Per altre informazioni, vedere [Helper di Tag della Cache in MVC ASP.NET Core](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Helper tag di cache distribuita

È possibile memorizzare nella cache il contenuto da una visualizzazione MVC o pagina Razor in cloud distribuito o in scenari web farm con l'Helper di Tag della Cache distribuita. L'Helper di Tag della Cache distribuita utilizza SQL Server o Redis per archiviare i dati.

Per altre informazioni, vedere [Helper di Tag della Cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>Attributo ResponseCache

Il [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifica i parametri necessari per l'impostazione delle intestazioni appropriate nella cache delle risposte.

> [!WARNING]
> Disabilitare la memorizzazione nella cache per il contenuto che contiene informazioni per i clienti autenticati. La memorizzazione nella cache deve essere abilitata solo per il contenuto che non cambia in base alle identità di un utente o che un utente viene eseguito l'accesso.

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) la risposta memorizzata varia in base ai valori di elenco specificato di chiavi di query. Quando un singolo valore di `*` viene fornito, il middleware varia a seconda delle risposte da tutte le richieste i parametri di stringa di query. `VaryByQueryKeys` è necessario ASP.NET Core 1.1 o successiva.

Il Middleware di memorizzazione nella cache della risposta deve essere abilitato per impostare il `VaryByQueryKeys` proprietà; in caso contrario, viene generata un'eccezione di runtime. Non è presente un'intestazione HTTP corrispondente per il `VaryByQueryKeys` proprietà. La proprietà è una funzionalità HTTP gestita dal Middleware di memorizzazione nella cache risposta. Per il middleware soddisfare una risposta memorizzata nella cache, la stringa di query e il valore di stringa di query deve corrispondere una richiesta precedente. Si consideri ad esempio, la sequenza di richieste e i risultati mostrati nella tabella seguente.

| Richiesta                          | Risultato                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | Restituito dal server     |
| `http://example.com?key1=value1` | Restituito dal middleware |
| `http://example.com?key1=value2` | Restituito dal server     |

La prima richiesta viene restituita dal server e memorizzati nella cache nel middleware. La seconda richiesta viene restituita dal middleware perché la stringa di query corrispondente alla richiesta precedente. La terza richiesta non è nella cache middleware perché il valore di stringa di query non corrisponde una richiesta precedente. 

Il `ResponseCacheAttribute` consente di configurare e creare (tramite `IFilterFactory`) un [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter). Il `ResponseCacheFilter` esegue l'operazione di aggiornamento delle intestazioni HTTP appropriate e le funzionalità di risposta. Il filtro:

* Rimuove tutte le intestazioni esistenti per `Vary`, `Cache-Control`, e `Pragma`. 
* Scrive le intestazioni appropriate in base alle proprietà impostate `ResponseCacheAttribute`. 
* Aggiorna la risposta HTTP funzionalità di cache se `VaryByQueryKeys` è impostata.

### <a name="vary"></a>Variare

Questa intestazione verrà scritti solo quando il `VaryByHeader` è impostata. È impostato sul `Vary` valore della proprietà. L'esempio seguente usa il `VaryByHeader` proprietà:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

È possibile visualizzare le intestazioni di risposta con gli strumenti di rete del browser. L'immagine seguente mostra il F12 Edge di output on il **rete** scheda quando il `About2` metodo di azione viene aggiornato:

![Bordo F12 output nella scheda di rete quando viene chiamato il metodo di azione About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore e Location.None

`NoStore` sostituisce la maggior parte delle altre proprietà. Quando questa proprietà è impostata su `true`, il `Cache-Control` intestazione è impostata su `no-store`. Se `Location` è impostata su `None`:

* `Cache-Control` è impostato su `no-store,no-cache`.
* `Pragma` è impostato su `no-cache`.

Se `NoStore` viene `false` e `Location` viene `None`, `Cache-Control` e `Pragma` sono impostate su `no-cache`.

In genere impostato `NoStore` a `true` nelle pagine di errore. Ad esempio:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

Di conseguenza le intestazioni seguenti:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Percorso e la durata

Per abilitare la memorizzazione nella cache, `Duration` deve essere impostata su un valore positivo e `Location` deve essere `Any` (impostazione predefinita) o `Client`. In questo caso, il `Cache-Control` intestazione è impostata sul valore di percorso aggiungendo il `max-age` della risposta.

> [!NOTE]
> `Location`del opzioni di `Any` e `Client` tradurre `Cache-Control` valori di intestazione `public` e `private`, rispettivamente. Come accennato in precedenza, l'impostazione `Location` al `None` imposta entrambi `Cache-Control` e `Pragma` intestazioni per `no-cache`.

Di seguito è riportato un esempio che mostra le intestazioni ottenuto impostando `Duration` e lasciando il valore predefinito `Location` valore:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

Viene prodotto l'intestazione seguente:

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Profili della cache

Anziché ripetere `ResponseCache` numero controller azione di attributi, profili della cache possono essere configurate come opzioni durante l'impostazione di MVC nel `ConfigureServices` metodo `Startup`. Valori trovati in un profilo della cache di cui viene fatto riferimento vengono utilizzati come valori predefiniti per il `ResponseCache` vengono sostituite dalle eventuali proprietà specificate per l'attributo e di attributo.

Configurazione di un profilo di cache:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

Riferimento a un profilo della cache:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

Il `ResponseCache` attributo può essere applicato sia alle azioni (metodi) e controller (classi). Gli attributi a livello di metodo sostituiscono le impostazioni specificate negli attributi a livello di classe.

Nell'esempio precedente, un attributo a livello di classe specifica una durata di 30 secondi, mentre un attributo a livello di metodo fa riferimento a un profilo della cache con una durata impostata su 60 secondi.

L'intestazione risulta:

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [La memorizzazione delle risposte nella cache](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Cache in memoria](xref:performance/caching/memory)
* [Usare una cache distribuita](xref:performance/caching/distributed)
* [Rilevare le modifiche apportate con i token di modifica](xref:fundamentals/primitives/change-tokens)
* [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
* [Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
