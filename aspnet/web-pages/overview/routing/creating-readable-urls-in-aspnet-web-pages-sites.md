---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Creazione di URL leggibili in ASP.NET Web Pages siti (Razor) | Documenti Microsoft
author: tfitzmac
description: "Questo articolo descrive routing in un sito Web ASP.NET Web Pages (Razor) e come è possibile utilizzare gli URL che risultano più leggibile e migliori per SEO. Sarà necessario..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Creazione di URL leggibili in siti Web di ASP.NET di pagine (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive routing in un sito Web ASP.NET Web Pages (Razor) e come è possibile utilizzare gli URL che risultano più leggibile e migliori per SEO.
> 
> Illustra quanto segue:
> 
> - Utilizzo di ASP.NET routing per l'utilizzo di più leggibili e possono essere cercati gli URL.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="about-routing"></a>Sul Routing

Gli URL per le pagine del sito possono avere un impatto sulla modalità anche il sito. Un URL che &quot;descrittivo&quot; può rendere più semplice per gli utenti di utilizzare il sito. Consente inoltre di aiutare con l'ottimizzazione motore di ricerca (SEO) per il sito. I siti Web ASP.NET includono la possibilità di utilizzare automaticamente gli URL brevi.

ASP.NET consente di creare URL che descrivono le azioni dell'utente anziché fare riferimento solo a un file nel server. Considerare queste URL per un blog fittizio:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Confrontare tali URL riportate di seguito:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

Nella prima coppia, un utente necessario conoscere il blog viene visualizzato utilizzando il *blog.cshtml* pagina e sarà necessario costruire una stringa di query che ottiene l'intervallo di categoria o data destra. Il secondo set di esempi è molto più semplice da comprendere e creare.

Gli URL per il primo esempio viene inoltre fare riferimento direttamente a un file specifico (*blog.cshtml*). Se per qualche motivo il blog sono stato spostato in un'altra cartella nel server o se il blog sono state riscritte per utilizzare una pagina diversa, i collegamenti sarebbe errati. Il secondo set di URL non fa riferimento a una pagina specifica, pertanto anche se viene modificata l'implementazione di blog o il percorso, gli URL ancora sarebbe validi.

In ASP.NET Web Pages, è possibile creare più URL come quelli negli esempi precedenti poiché ASP.NET usa *routing*. Routing crea mapping logico da un URL a una pagina (o pagine) che può soddisfare la richiesta. Poiché il mapping è logico (non fisica, a un file specifico), il routing fornisce una notevole flessibilità, in cui viene illustrato come definire gli URL per il sito.

## <a name="how-routing-works"></a>Il funzionamento del Routing

Quando ASP.NET elabora una richiesta, viene anche letto l'URL per determinare come eseguirne il routing. ASP.NET tenta di abbinare i singoli segmenti di URL di file su disco, da sinistra verso destra. Se viene trovata una corrispondenza, qualsiasi elemento rimanenti nell'URL viene passato alla pagina come *informazioni sul percorso*.

Si supponga che un utente effettua una richiesta con questo URL:

`http://www.contoso.com/a/b/c`

La ricerca procede come segue:

1. È presente un file con il nome e percorso */a/b/c.cshtml*? In questo caso, eseguire la pagina e non passarle alcuna informazione. In caso contrario...
2. È presente un file con il nome e percorso */a/b.cshtml*? Se in tal caso, eseguire la pagina e passare il valore `c` a esso. In caso contrario...
3. È presente un file con il nome e percorso */a.cshtml*? Se in tal caso, eseguire la pagina e passare il valore `b/c` a esso.

Se la ricerca trovata esatta non corrispondente per *. cshtml* file nelle rispettive cartelle specificate, ASP.NET continua la ricerca per questi file, a sua volta:

1. */a/b/c/default.cshtml* (senza informazioni sul percorso).
2. */a/b/c/index.cshtml* (senza informazioni sul percorso).

> [!NOTE]
> Per essere chiaro, le richieste di pagine specifiche (vale a dire le richieste che includono il *. cshtml* estensione) funziona esattamente come ci si aspetta. Ad esempio una richiesta `http://www.contoso.com/a/b.cshtml` eseguirà la pagina *b.cshtml* correttamente.


All'interno di una pagina, è possibile ottenere le informazioni sul percorso tramite la pagina `UrlData` proprietà, che è un dizionario. Si supponga di disporre di un file denominato *ViewCustomers.cshtml* e il sito riceve la richiesta:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Come descritto nelle regole di precedenza, la richiesta verrà inviata a una pagina. All'interno della pagina, è possibile utilizzare codice simile al seguente per ottenere e visualizzare le informazioni sul percorso (in questo caso, il valore &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Poiché il routing non implica i nomi di file completo, può esserci delle ambiguità nel caso di pagine che presentano lo stesso nome ma diverse estensioni di file (ad esempio, *MyPage.cshtml* e *MyPage.html*) . Per evitare problemi con il routing, è consigliabile verificare che non si hanno le pagine del sito i cui nomi differiscono solo per l'estensione.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[WebMatrix - URL, UrlData e Routing per SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Questo post di blog da Mike Brind fornisce alcuni dettagli aggiuntivi sul funzionamento del routing in ASP.NET Web Pages.
