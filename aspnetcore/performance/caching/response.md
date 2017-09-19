---
title: La memorizzazione nella cache di risposta
author: rick-anderson
description: Viene illustrato come utilizzare la memorizzazione nella cache per ridurre la larghezza di banda e migliorare le prestazioni di risposta.
keywords: ASP.NET Core, risposta, la memorizzazione nella cache, le intestazioni HTTP
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 97bc56a3cfe7383f207a4f621ab3fe8b8dc258ef
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2017
---
# <a name="response-caching"></a>La memorizzazione nella cache di risposta

Da [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Steve Smith](https://ardalis.com/)

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a>Che cos'è la memorizzazione nella cache di risposta

*La memorizzazione nella cache di risposta* aumenta intestazioni correlate alla cache delle risposte. Queste intestazioni specificano la modalità client, proxy e middleware per memorizzare risposte. La memorizzazione nella cache di risposta, è possibile ridurre il numero di richieste di che un client o proxy effettua al server web. La memorizzazione nella cache di risposta consente inoltre di ridurre la quantità di lavoro del server web esegue per generare la risposta. 

L'intestazione HTTP primario utilizzato per la memorizzazione nella cache è `Cache-Control`. Vedere il [la memorizzazione nella cache di HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2) per ulteriori informazioni. Direttive cache comuni:

* [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)
* [Variare](https://tools.ietf.org/html/rfc7231#section-7.1.4)

Il server web è possibile memorizzare nella cache le risposte aggiungendo la risposta di middleware di memorizzazione nella cache. Vedere [risposta la memorizzazione nella cache middleware](middleware.md) per ulteriori informazioni.

## <a name="distributed-cache-tag-helper"></a>Helper di Tag Cache distribuita

Il [Helper di Tag della Cache distribuita](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) Abilita memorizzazione nella cache distribuita.


## <a name="responsecache-attribute"></a>Attributo ResponseCache

Il `ResponseCacheAttribute` specifica i parametri necessari per l'impostazione delle intestazioni appropriate nella cache delle risposte. Vedere [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) per una descrizione dei parametri.

>[!WARNING]
> Disabilitare la memorizzazione nella cache per il contenuto che contiene informazioni per i client autenticati. La memorizzazione nella cache deve essere abilitata solo per il contenuto che non cambia in base all'identità dell'utente, o se un utente è connesso.

`VaryByQueryKeys string[]`(richiede ASP.NET Core 1.1.0 e versioni successive): se è impostata, la risposta di memorizzazione nella cache middleware varia la risposta memorizzata dai valori di elenco specificato di chiavi di query. La risposta di middleware di memorizzazione nella cache deve essere abilitata per impostare il `VaryByQueryKeys` proprietà, altrimenti un'eccezione di runtime verrà generata. Non è presente alcuna intestazione HTTP corrispondente per il `VaryByQueryKeys` proprietà. Questa proprietà è una funzionalità HTTP gestita dalla risposta middleware di memorizzazione nella cache. Per il middleware gestire una risposta memorizzata nella cache, la stringa di query e il valore di stringa di query deve corrispondere una richiesta precedente. Ad esempio, si consideri la seguente sequenza:

| Richiesta          | Risultato |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | restituito dal server |
| `http://example.com?key1=value1` | restituito dal middleware |
| `http://example.com?key1=value2` | restituito dal server |

La prima richiesta viene restituita dal server e memorizzati nella cache nel middleware. La seconda richiesta viene restituita dal middleware perché la richiesta precedente corrisponde alla stringa di query. La terza richiesta non è nella cache middleware perché il valore di stringa di query non corrisponde a una richiesta precedente. 

Il `ResponseCacheAttribute` viene utilizzato per configurare e creare (tramite `IFilterFactory`) un `ResponseCacheFilter`. Il `ResponseCacheFilter` esegue l'operazione di aggiornamento delle intestazioni HTTP appropriate e le funzionalità di risposta. Il filtro:

* Rimuove tutte le intestazioni esistenti per `Vary`, `Cache-Control`, e `Pragma`. 
* Scrive le intestazioni appropriate in base alle proprietà impostate `ResponseCacheAttribute`. 
* Aggiorna la risposta HTTP funzionalità di cache se `VaryByQueryKeys` è impostata.

### <a name="vary"></a>Variare

Questa intestazione viene scritto solo quando il `VaryByHeader` proprietà è impostata. È impostato sul `Vary` valore della proprietà. L'esempio seguente usa il `VaryByHeader` proprietà.

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

È possibile visualizzare le intestazioni di risposta con gli strumenti di rete del browser. La figura seguente mostra il F12 Edge in uscita nel **rete** scheda quando il `About2` viene aggiornato il metodo di azione. 

![Bordo output F12 nel * * scheda di rete * * quando viene chiamato il metodo di azione 'About2'](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore e Location.None

`NoStore`sostituisce la maggior parte delle altre proprietà. Quando questa proprietà è impostata su `true`, `Cache-Control` intestazione verrà impostata su "no-store". Se `Location` è impostato su `None`:

* `Cache-Control` è impostato su `"no-store, no-cache"`. 
* `Pragma` è impostato su `no-cache`. 

Se `NoStore` è `false` e `Location` è `None`, `Cache-Control` e `Pragma` verrà impostato su `no-cache`.

In genere si imposta `NoStore` a `true` nelle pagine di errore. Ad esempio:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

Restituirà le intestazioni seguenti:

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Posizione e la durata

Per abilitare la memorizzazione nella cache, `Duration` deve essere impostata su un valore positivo e `Location` deve essere `Any` (impostazione predefinita) o `Client`. In questo caso, il `Cache-Control` intestazione verrà impostata sul valore del percorso seguito da "max-age" della risposta.

> [!NOTE]
> `Location`di opzioni di `Any` e `Client` tradurre `Cache-Control` valori di intestazione `public` e `private`rispettivamente. Come indicato in precedenza, l'impostazione `Location` a `None` imposterà entrambi `Cache-Control` e `Pragma` intestazioni per `no-cache`.

Di seguito è riportato un esempio che mostra le intestazioni ottenuto impostando `Duration` e lasciando il valore predefinito `Location` valore.

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

Genera le intestazioni seguenti:

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a>Profili della cache

Anziché ripetere `ResponseCache` su molti attributi di azione controller, i profili di cache possono essere configurate come opzioni durante la configurazione di MVC nel `ConfigureServices` metodo `Startup`. Valori trovati in un profilo della cache di cui viene fatto riferimento da utilizzare come impostazioni predefinite per il `ResponseCache` attributo e verranno sovrascritte dalle eventuali proprietà specificate per l'attributo.

Impostazione di un profilo della cache:

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

Riferimento a un profilo della cache:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

Il `ResponseCache` attributo può essere applicato sia per le azioni (metodi), nonché i controller (classi). Gli attributi a livello di metodo sostituiranno le impostazioni specificate negli attributi a livello di classe.

Nell'esempio precedente, un attributo a livello di classe specifica una durata di 30 secondi, mentre gli attributi di un livello di metodo fa riferimento a un profilo della cache con una durata impostata su 60 secondi.

L'intestazione risulta:

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a>Risorse aggiuntive

* [Memorizzazione nella cache in HTTP dalla specifica](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
