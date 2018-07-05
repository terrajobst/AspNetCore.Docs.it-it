---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: La memorizzazione nella cache i dati in un Web ASP.NET le pagine del sito (Razor) per ottenere prestazioni migliori | Microsoft Docs
author: tfitzmac
description: È possibile velocizzare il sito Web facendo sì che archiviano, vale a dire, cache - i risultati dei dati che in genere richiederebbe molto tempo per recuperare o elaborare un...
ms.author: aspnetcontent
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 28be9194bbd95e896311700ddcf89379a82ee636
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805200"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>La memorizzazione nella cache i dati in un sito di ASP.NET Web Pages (Razor) per ottenere prestazioni migliori
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come usare helper per le informazioni della cache per migliorare le prestazioni in un sito Web ASP.NET Web Pages (Razor). È possibile velocizzare il sito Web facendo sì che archivio &#8212; , memorizzare nella cache &#8212; i risultati di dati che in genere richiederebbe molto tempo per recuperare o l'elaborazione e che non cambia spesso.
> 
> **Che cosa si apprenderà come:** 
> 
> - Come usare la memorizzazione nella cache per migliorare la velocità di risposta del sito Web.
> 
> Queste sono le funzionalità ASP.NET introdotte nell'articolo:
> 
> - Il `WebCache` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


Ogni volta che un utente richiede una pagina del sito, il server web deve eseguire alcune operazioni per soddisfare la richiesta. Per alcune delle pagine, il server potrebbe essere necessario eseguire le attività che richiedono tempo (relativamente) lungo, ad esempio il recupero dei dati da un database. Anche se queste attività traggono tempo in termini assoluti, se il sito si verifica una grande quantità di traffico, un'intera serie di richieste singole in modo che il server web eseguire l'attività complessa o lenta possibile aggiungere fino a una grande quantità di lavoro. Ciò può in ultima analisi sulle prestazioni del sito.

Un modo per migliorare le prestazioni del tuo sito Web in casi come questo è per memorizzare i dati. Se il sito Ottiene le richieste ripetute per le stesse informazioni, le informazioni non devono essere modificate per ogni persona e non è ora sensibile, anziché recuperare nuovamente o ricalcolo, è possibile recuperare i dati una sola volta e quindi archiviare i risultati. La volta successiva che viene ricevuta una richiesta per tale informazioni, è sufficiente ottenerlo dalla cache.

In generale, si memorizzano nella cache le informazioni che non cambiano di frequente. Quando si inserisce le informazioni nella cache, questo viene archiviato in memoria nel server web. È possibile specificare quanto tempo deve essere memorizzato nella cache, da secondi a giorni. Al termine del periodo di memorizzazione nella cache, le informazioni vengono rimosse automaticamente dalla cache.

> [!NOTE]
> Le voci nella cache potrebbero essere rimossa per motivi diversi da che siano scaduti. Ad esempio, il server web potrebbe essere temporaneamente insufficiente memoria e può recuperare la memoria, è possibile generare le voci dalla cache. Come si vedrà, anche se è stata inserire informazioni nella cache, è necessario verificare che è ancora disponibile quando ti serve.


Si supponga che il sito Web ha una pagina che consente di visualizzare la temperatura attuale e le previsioni meteo. Per ottenere questo tipo di informazioni, è possibile inviare una richiesta a un servizio esterno. Poiché queste informazioni non cambia molto (entro un periodo di tempo di due ore, ad esempio) e poiché le chiamate esterne richiedano tempo e larghezza di banda, è un buon candidato per la memorizzazione nella cache.

## <a name="adding-caching-to-a-page"></a>Aggiunta a una pagina di memorizzazione nella cache

ASP.NET include un `WebCache` helper che rende più semplice aggiungere la memorizzazione nella cache per il sito e aggiungere dati alla cache. In questa procedura si creerà una pagina che memorizza nella cache l'ora corrente. Ciò non è un esempio reale, poiché l'ora corrente è un elemento che cambia spesso e che inoltre non è difficile da calcolare. Tuttavia, è un buon metodo per illustrare la memorizzazione nella cache in azione.

1. Aggiungere una nuova pagina denominata *WebCache.cshtml* al sito Web.
2. Aggiungere il codice e il markup seguente alla pagina:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Quando si memorizza nella cache dei dati, è inserirlo nella cache usando un nome di questo è univoco tra il sito Web. In questo caso, si userà una voce della cache denominata `CachedTime`. Si tratta di `cacheItemKey` illustrato nell'esempio di codice.

    Il codice legge per primi i `CachedTime` voce della cache. Se viene restituito un valore (ovvero, se la voce della cache non è null), il codice semplicemente imposta il valore della variabile di tempo per i dati della cache.

    Tuttavia, se la voce della cache non esiste (vale a dire, è null), il codice imposta il valore di tempo, lo aggiunge alla cache e imposta un valore di scadenza a un minuto. Dopo un minuto, viene eliminata la voce della cache. (Il valore di scadenza predefinito per un elemento nella cache è 20 minuti). Il comando `WebCache.Set(cacheItemKey, time, 1, false)` viene illustrato come aggiungere il valore di tempo corrente nella cache e impostare la scadenza di 1 minuto. Impostando il *slidingExpiration* parametro per `false` significa che l'ora di scadenza non viene rinnovata ogni volta che viene richiesto. Scadenza esattamente 1 minuto dopo che è stato originariamente aggiunto alla cache. Se si imposta questo valore su `true` l'ora di scadenza di 1 minuto viene reimpostato ogni volta che viene richiesto il valore dalla cache.

    Questo codice viene illustrato il modello che è consigliabile usare sempre quando si memorizza nella cache dei dati. Prima di ottenere un elemento dalla cache, verificare sempre prima di tutto se le `WebCache.Get` metodo ha restituito null. Tenere presente che la voce della cache potrebbe essere scaduto o sia stato rimosso per altri motivi, in modo che qualsiasi voce specificata non è mai considerata nella cache.
3. Eseguire *WebCache.cshtml* in un browser. (Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.) La prima volta che la richiesta di pagina, i dati ora non sono nella cache e il codice deve aggiungere il valore di ora nella cache.

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Aggiornare *WebCache.cshtml* nel browser. Questa volta, i dati temporali sono nella cache. Si noti che l'ora non è stato modificato dall'ultima volta che visualizzata la pagina.

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Attendere qualche minuto per la cache essere svuotato in seguito e quindi aggiornare la pagina. La pagina indica nuovamente che i dati di ora non è stati trovati nella cache e l'ora dell'aggiornamento viene aggiunto alla cache.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


- [Visualizzazione dei dati in un grafico](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Riferimento all'API WebCache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
