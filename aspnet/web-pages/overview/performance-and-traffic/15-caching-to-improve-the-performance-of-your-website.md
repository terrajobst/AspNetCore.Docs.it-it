---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: La memorizzazione nella cache di dati in un Web ASP.NET di pagine del sito (Razor) per ottenere prestazioni migliori | Documenti Microsoft
author: tfitzmac
description: "È possibile velocizzare il sito Web richiedendo store - ovvero cache: i risultati dei dati che in genere richiederebbe molto tempo per recuperare o elaborare un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 742409219bd3b05f8ddf2c0d5034919fc9bf1d26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Memorizzazione nella cache i dati nel sito Web ASP.NET (Razor) pagine per migliorare le prestazioni
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come utilizzare un metodo di supporto per informazioni sulla cache per migliorare le prestazioni in un sito Web ASP.NET Web Pages (Razor). È possibile velocizzare il sito Web facendo sì che store &#8212; vale a dire cache &#8212; i risultati dei dati che in genere richiederebbe molto tempo per recuperare o l'elaborazione e che non vengono modificati spesso.
> 
> **Illustra quanto segue:** 
> 
> - Come utilizzare la memorizzazione nella cache per migliorare la velocità di risposta del sito Web.
> 
> Queste sono le funzionalità ASP.NET introdotte nell'articolo:
> 
> - Il `WebCache` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


Ogni volta che un utente richiede una pagina del sito, il server web deve eseguire alcune operazioni per soddisfare la richiesta. Per alcune delle pagine, il server potrebbe essere necessario eseguire le attività che accettano (relativamente) molto tempo, ad esempio il recupero dei dati da un database. Anche se queste attività traggono tempo in termini assoluti, se il sito di cui si verifichi un traffico elevato, può aggiungere un'intera serie di singole richieste che causano il server web eseguire l'attività complessa o lento a una grande quantità di lavoro. Infine, si possono influenzare le prestazioni del sito.

Un modo per migliorare le prestazioni del sito Web in casi come questo è per memorizzare i dati. Se il sito Ottiene le richieste ripetute per le stesse informazioni e le informazioni non devono essere modificate per ogni persona e non è possibile ora sensibili, anziché recuperare nuovamente o il ricalcolo, è possibile recuperare i dati una volta e quindi archiviare i risultati. La volta successiva che arriva una richiesta per tale informazioni, è sufficiente scaricare il pacchetto dalla cache.

In generale, nella cache di informazioni che non cambiano frequentemente. Le informazioni inserite nella cache, archiviato in memoria nel server web. È possibile specificare il tempo che deve essere memorizzata nella cache, da secondi a giorni. Quando il periodo di memorizzazione nella cache scade, le informazioni vengono rimosse automaticamente dalla cache.

> [!NOTE]
> Le voci nella cache potrebbero essere rimossi per motivi diversi da sono scadute. Ad esempio, il server web potrebbe essere temporaneamente insufficiente per la memoria e può recuperare memoria, è possibile generare voci dalla cache. Come si vedrà, anche se è stata inserire informazioni nella cache, è necessario verificare che è ancora presente quando necessario.


Si supponga che il sito Web disponga di una pagina che consente di visualizzare la temperatura corrente e previsioni del tempo. Per ottenere questo tipo di informazioni, è possibile inviare una richiesta a un servizio esterno. Poiché queste informazioni non viene modificato molto (all'interno di un periodo di tempo di due ore, ad esempio) e dal momento che le chiamate esterne richiedono tempo e larghezza di banda, è un buon candidato per la memorizzazione nella cache.

## <a name="adding-caching-to-a-page"></a>Aggiunta a una pagina di memorizzazione nella cache

ASP.NET include un `WebCache` helper che rende più semplice aggiungere la memorizzazione nella cache per il sito e aggiungere dati alla cache. In questa procedura, si creerà una pagina che memorizza nella cache l'ora corrente. Questo non è un esempio reale, poiché l'ora corrente è un elemento che cambiano spesso e che inoltre non è difficile da calcolare. Tuttavia, è un buon metodo per illustrare la memorizzazione nella cache in azione.

1. Aggiungere una nuova pagina denominata *WebCache.cshtml* al sito Web.
2. Aggiungere il seguente codice e markup alla pagina:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Quando si memorizza nella cache di dati, si inserisce nella cache usando un nome di questo è univoco tra il sito Web. In questo caso, si userà una voce della cache denominata `CachedTime`. Si tratta di `cacheItemKey` illustrato nell'esempio di codice.

    Il codice legge innanzitutto la `CachedTime` della voce della cache. Se viene restituito un valore (ovvero, se la voce della cache non è null), il codice imposta solo il valore della variabile di tempo per i dati della cache.

    Tuttavia, se la voce della cache non esiste (vale a dire è null), il codice imposta il valore di ora, viene aggiunto alla cache e imposta un valore di scadenza a un minuto. Dopo un minuto, verrà eliminata la voce della cache. (Il valore di scadenza predefinita per un elemento nella cache è 20 minuti). Il comando `WebCache.Set(cacheItemKey, time, 1, false)` viene illustrato come aggiungere il valore di tempo corrente nella cache e impostare la scadenza su 1 minuto. L'impostazione di *slidingExpiration* parametro `false` indica l'ora di scadenza non viene rinnovato ogni volta che viene richiesto. Scadenza esattamente 1 minuto dopo che è stati precedentemente aggiunti alla cache. Se si imposta questo valore su `true` l'ora di scadenza di 1 minuto viene reimpostato ogni volta che viene richiesto il valore dalla cache.

    Questo codice illustra il modello che è consigliabile usare sempre quando si memorizza nella cache di dati. Prima di ottenere un elemento dalla cache, sempre controllare innanzitutto se il `WebCache.Get` metodo ha restituito null. Tenere presente che la voce della cache potrebbe essere scaduti o che potrebbe essere stato rimosso per altri motivi, pertanto qualsiasi voce specificata non è nella cache.
3. Eseguire *WebCache.cshtml* in un browser. (Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.) La prima volta che la richiesta di pagina, i dati di ora non sono nella cache e il codice deve aggiungere il valore di ora alla cache.

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Aggiorna *WebCache.cshtml* nel browser. Questa volta, i dati di ora sono nella cache. Si noti che l'ora non è stato modificato dall'ultima volta che si visualizza la pagina.

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Attendere un minuto per svuotare la cache e quindi aggiornare la pagina. La pagina nuovo indica che i dati di ora non è stati trovati nella cache e l'ora dell'aggiornamento viene aggiunto alla cache.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


- [Visualizzazione dei dati in un grafico](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Riferimento all'API WebCache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
