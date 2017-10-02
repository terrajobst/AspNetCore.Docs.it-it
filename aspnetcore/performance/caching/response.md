---
title: La memorizzazione nella cache di risposta in ASP.NET Core
author: rick-anderson
description: Informazioni su come utilizzare la memorizzazione nella cache per ridurre la larghezza di banda e migliorare le prestazioni di risposta.
keywords: ASP.NET Core, risposta, la memorizzazione nella cache, le intestazioni HTTP
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 79d9246632aae0fe9c3629fd7202842836828151
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2017
---
# <a name="response-caching-in-aspnet-core"></a>La memorizzazione nella cache di risposta in ASP.NET Core

Da [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), e [Luke Latham](https://github.com/guardrex)

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))

Risposta di memorizzazione nella cache riduce il numero di richieste di che un client o proxy consente a un server web. La memorizzazione nella cache di risposta consente inoltre di ridurre la quantità di lavoro del server web esegue per generare una risposta. La memorizzazione nella cache di risposta è controllata dalle intestazioni che specificano la modalità client, proxy e middleware per memorizzare risposte.

Il server web può memorizzare nella cache le risposte quando si aggiunge [Middleware la memorizzazione nella cache risposta](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>Memorizzazione nella cache la risposta basata su HTTP

Il [specifica la memorizzazione nella cache di HTTP 1.1](https://tools.ietf.org/html/rfc7234) descrive il comportamento delle cache di Internet. L'intestazione HTTP principale utilizzato per la memorizzazione nella cache [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), che consente di specificare la cache *direttive*. Le direttive controllano il comportamento di memorizzazione nella cache come richieste arrivino dai client ai server e come risposte arrivino dal server ai client. Richieste e risposte di spostarsi tra i server proxy e server proxy devono inoltre essere conforme alla specifica la memorizzazione nella cache di HTTP 1.1.

Comuni `Cache-Control` direttive vengono visualizzate nella tabella seguente.

| Direttiva                                                       | Azione |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Una cache può archiviare la risposta. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | La risposta non deve essere archiviata per una cache condivisa. Una cache privata può archiviare e riutilizzare la risposta. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Il client non accetta una risposta cui età è maggiore del numero di secondi specificato. Esempi: `max-age=60` (60 secondi), `max-age=2592000` (mese) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **Per le richieste**: una cache non deve utilizzare una risposta memorizzata per soddisfare la richiesta. Nota: Il server di origine genera nuovamente la risposta per il client e il middleware Aggiorna la risposta memorizzata nella cache.<br><br>**Sulle risposte**: la risposta non deve essere usata per una richiesta successiva senza la convalida sul server di origine. |
| [No-archivio](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **Per le richieste**: una cache non è necessario archiviare la richiesta.<br><br>**Sulle risposte**: una cache non deve memorizzare qualsiasi parte della risposta. |

Altre intestazioni cache che svolgono un ruolo per la memorizzazione nella cache vengono visualizzati nella tabella seguente.

| Header                                                     | Funzione |
| ---------------------------------------------------------- | -------- |
| [Età](https://tools.ietf.org/html/rfc7234#section-5.1)     | Una stima della quantità di tempo in secondi, poiché la risposta è stata generata o convalidata nel server di origine. |
| [Scadenza](https://tools.ietf.org/html/rfc7234#section-5.3) | La data/ora dopo il quale la risposta viene considerata non aggiornata. |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Per garantire la compatibilità con HTTP/1.0 vengono memorizzati nella cache per impostazione esiste `no-cache` comportamento. Se il `Cache-Control` intestazione è presente, il `Pragma` intestazione viene ignorata. |
| [Variare](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Specifica che una risposta memorizzata nella cache non deve essere inviata a meno che non tutti del `Vary` corrispondano i campi di intestazione nella richiesta originale della risposta memorizzata nella cache sia la nuova richiesta. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>Differenze di memorizzazione nella cache basata su HTTP richiesta direttive Cache-Control

Il [specifica la memorizzazione nella cache di HTTP 1.1 per l'intestazione Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) richiede una cache per rispettare un valido `Cache-Control` intestazione inviata dal client. Un client può eseguire richieste con un `no-cache` valore dell'intestazione e forza il server per generare una nuova risposta per ogni richiesta.

Sempre rispettando la distinzione tra client `Cache-Control` le intestazioni di richiesta ha senso se si considera l'obiettivo della memorizzazione nella cache HTTP. In specifica ufficiale, la memorizzazione nella cache è progettata per ridurre il sovraccarico di rete e la latenza di soddisfare le richieste in una rete di client, proxy e server. Non è necessariamente un modo per controllare il carico sul server di origine.

È presente alcun controllo di sviluppatore corrente su questo comportamento di memorizzazione nella cache quando si utilizza il [risposta la memorizzazione nella cache Middleware](xref:performance/caching/middleware) perché il middleware rispetti ufficiali la memorizzazione nella cache specifica. [Miglioramenti futuri correlati al middleware](https://github.com/aspnet/ResponseCaching/issues/96) consentirà di configurare il middleware per ignorare una richiesta `Cache-Control` intestazione quando si decide di gestire una risposta memorizzata nella cache. Questo verrà offre un'opportunità per controllare meglio il carico sul server quando si utilizza il middleware.

## <a name="other-caching-technology-in-aspnet-core"></a>Altre tecnologie di memorizzazione nella cache in ASP.NET Core

### <a name="in-memory-caching"></a>Memorizzazione nella cache in memoria

Memorizzazione nella cache in memoria utilizza la memoria del server per archiviare i dati memorizzati nella cache. Questo tipo di memorizzazione nella cache è adatto per uno o più server tramite *sessioni permanenti*. Le sessioni permanenti significa che le richieste da un client vengono sempre indirizzate allo stesso server per l'elaborazione.

Per ulteriori informazioni, vedere [Introduzione alla memorizzazione nella cache in memoria in ASP.NET Core](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Cache distribuita

Utilizzare una cache distribuita per memorizzare dati in memoria quando l'applicazione è ospitata in una farm di server o cloud. La cache viene condivisa tra i server di elaborazione delle richieste. Un client può inviare una richiesta che viene gestita da qualsiasi server nel gruppo e i dati memorizzati nella cache per il client sono disponibili. ASP.NET Core offre le cache Redis distribuita e SQL Server.

Per ulteriori informazioni, vedere [utilizzano una cache distribuita](xref:performance/caching/distributed).

### <a name="cache-tag-helper"></a>Helper di Tag della cache

È possibile memorizzare nella cache il contenuto da una visualizzazione MVC o pagina Razor con l'Helper di Tag della Cache. L'Helper di Tag della Cache Usa memorizzazione nella cache in memoria per archiviare i dati.

Per ulteriori informazioni, vedere [Helper di Tag della Cache in ASP.NET MVC Core](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Helper di Tag Cache distribuita

È possibile memorizzare nella cache il contenuto da una visualizzazione MVC o pagina Razor in cloud distribuito o in scenari web farm con l'Helper di Tag della Cache distribuita. L'Helper di Tag della Cache distribuita utilizza SQL Server o Redis per archiviare i dati.

Per ulteriori informazioni, vedere [Helper di Tag della Cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>Attributo ResponseCache

Il `ResponseCacheAttribute` specifica i parametri necessari per l'impostazione delle intestazioni appropriate nella cache delle risposte. Vedere [ResponseCacheAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) per una descrizione dei parametri.

> [!WARNING]
> Disabilitare la memorizzazione nella cache per il contenuto che contiene informazioni per i client autenticati. La memorizzazione nella cache deve essere abilitata solo per il contenuto che non cambia in base alle identità di un utente o se un utente è connesso.

`VaryByQueryKeys string[]`(richiede ASP.NET Core 1.1 e versioni successive): se è impostata, il Middleware di memorizzazione nella cache della risposta varia la risposta memorizzata dai valori di elenco specificato di chiavi di query. Il Middleware di memorizzazione nella cache della risposta deve essere abilitato per impostare il `VaryByQueryKeys` proprietà; in caso contrario, viene generata un'eccezione di runtime. Non è presente alcuna intestazione HTTP corrispondente per il `VaryByQueryKeys` proprietà. Questa proprietà è una funzionalità HTTP gestita dal Middleware di memorizzazione nella cache risposta. Per il middleware gestire una risposta memorizzata nella cache, la stringa di query e il valore di stringa di query deve corrispondere una richiesta precedente. Si consideri, ad esempio, la sequenza di richieste e i risultati mostrati nella tabella riportata di seguito.

| Richiesta                          | Risultato                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | restituito dal server     |
| `http://example.com?key1=value1` | restituito dal middleware |
| `http://example.com?key1=value2` | restituito dal server     |

La prima richiesta viene restituita dal server e memorizzati nella cache nel middleware. La seconda richiesta viene restituita dal middleware perché la richiesta precedente corrisponde alla stringa di query. La terza richiesta non è nella cache middleware perché il valore di stringa di query non corrisponde a una richiesta precedente. 

Il `ResponseCacheAttribute` viene utilizzato per configurare e creare (tramite `IFilterFactory`) un `ResponseCacheFilter`. Il `ResponseCacheFilter` esegue l'operazione di aggiornamento delle intestazioni HTTP appropriate e le funzionalità di risposta. Il filtro:

* Rimuove tutte le intestazioni esistenti per `Vary`, `Cache-Control`, e `Pragma`. 
* Scrive le intestazioni appropriate in base alle proprietà impostate `ResponseCacheAttribute`. 
* Aggiorna la risposta HTTP funzionalità di cache se `VaryByQueryKeys` è impostata.

### <a name="vary"></a>Variare

Questa intestazione viene scritto solo quando il `VaryByHeader` proprietà è impostata. È impostato sul `Vary` valore della proprietà. L'esempio seguente usa il `VaryByHeader` proprietà:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

È possibile visualizzare le intestazioni di risposta con gli strumenti di rete del browser. La figura seguente mostra il F12 Edge in uscita nel **rete** scheda quando il `About2` viene aggiornato il metodo di azione:

![Bordo F12 output nella scheda di rete quando viene chiamato il metodo di azione About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore e Location.None

`NoStore`sostituisce la maggior parte delle altre proprietà. Quando questa proprietà è impostata su `true`, `Cache-Control` intestazione è impostata su `no-store`. Se `Location` è impostato su `None`:

* `Cache-Control` è impostato su `no-store,no-cache`.
* `Pragma` è impostato su `no-cache`.

Se `NoStore` è `false` e `Location` è `None`, `Cache-Control` e `Pragma` sono impostate su `no-cache`.

In genere si imposta `NoStore` a `true` nelle pagine di errore. Ad esempio:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

Ciò comporta le intestazioni seguenti:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Posizione e la durata

Per abilitare la memorizzazione nella cache, `Duration` deve essere impostata su un valore positivo e `Location` deve essere `Any` (impostazione predefinita) o `Client`. In questo caso, il `Cache-Control` intestazione è impostata sul valore di percorso aggiungendo il `max-age` della risposta.

> [!NOTE]
> `Location`di opzioni di `Any` e `Client` tradurre `Cache-Control` valori di intestazione `public` e `private`rispettivamente. Come indicato in precedenza, l'impostazione `Location` a `None` imposta sia `Cache-Control` e `Pragma` intestazioni per `no-cache`.

Di seguito è riportato un esempio che mostra le intestazioni ottenuto impostando `Duration` e lasciando il valore predefinito `Location` valore:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

Viene prodotto l'intestazione seguente:

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Profili della cache

Anziché ripetere `ResponseCache` su molti attributi di azione controller, i profili di cache possono essere configurate come opzioni durante la configurazione di MVC nel `ConfigureServices` metodo `Startup`. Valori trovati in un profilo di cache di riferimento vengono utilizzati come le impostazioni predefinite per il `ResponseCache` vengono sostituite dalle eventuali proprietà specificate per l'attributo e di attributo.

Impostazione di un profilo della cache:

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

Riferimento a un profilo della cache:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

Il `ResponseCache` attributo può essere applicato sia alle azioni (metodi) e controller (classi). Gli attributi a livello di metodo sostituiscono le impostazioni specificate negli attributi a livello di classe.

Nell'esempio precedente, un attributo a livello di classe specifica una durata di 30 secondi, mentre un attributo a livello di metodo fa riferimento a un profilo della cache con una durata impostata su 60 secondi.

L'intestazione risulta:

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Memorizzazione nella cache in HTTP dalla specifica](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
