---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Creazione di URL leggibili in ASP.NET Web Pages (Razor) siti | Microsoft Docs
author: tfitzmac
description: Questo articolo descrive il routing in un sito Web di ASP.NET Web Pages (Razor) e come è possibile utilizzare gli URL che risultano più leggibili e migliori per SEO. Che cosa si imposterà un...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: b8405283dc5bf44a4cd8d1122d327346774d95e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830363"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Creazione di URL leggibili nei siti di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive il routing in un sito Web di ASP.NET Web Pages (Razor) e come è possibile utilizzare gli URL che risultano più leggibili e migliori per SEO.
> 
> Che cosa si apprenderà come:
> 
> - Come ASP.NET utilizza il routing per l'utilizzo di più URL leggibili e disponibili per la ricerca.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="about-routing"></a>Informazioni sul Routing

Gli URL per le pagine del sito possono avere un impatto su come funziona il sito. Un URL Ecco &quot;descrittivo&quot; rendono più semplice per gli utenti a utilizzare il sito. Inoltre possibile facilitare ottimizzazione motore di ricerca (SEO) per il sito. Siti Web ASP.NET includono la possibilità di utilizzare automaticamente gli URL brevi.

ASP.NET consente di creare URL significativo che descrivono le azioni dell'utente anziché semplicemente riferimento a un file nel server. Prendere in considerazione questi URL per un blog fittizio:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Confrontare questi URL per i seguenti:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

Nella coppia primo, un utente deve sapere che il blog viene visualizzato utilizzando il *blog.cshtml* pagina e sarà necessario costruire una stringa di query che ottiene gli intervalli di categoria e data a destra. È molto più semplice da comprendere e creare il secondo set di esempi.

Gli URL per il primo esempio anche puntino direttamente a un file specifico (*blog.cshtml*). Se per qualche motivo i blog sono state spostate in un'altra cartella nel server o se i blog sono state riscritte per utilizzare un'altra pagina, i collegamenti avresti torto. Il secondo set di URL non punta a una pagina specifica, pertanto anche se viene modificata l'implementazione di blog o località, l'URL può essere ancora adottato.

In ASP.NET Web Pages, è possibile creare un URL più descrittivo, ad esempio quelli negli esempi precedenti poiché in ASP.NET viene utilizzato *routing*. Routing crea mapping logico da un URL a una pagina (o pagine) che può soddisfare la richiesta. Poiché il mapping è logico (non fisico, a un file specifico), routing offre una notevole flessibilità nel modo in cui si definiscono l'URL per il sito.

## <a name="how-routing-works"></a>Funzionamento del Routing

Quando ASP.NET elabora una richiesta, legge l'URL per determinare come eseguirne il routing. ASP.NET tenta di abbinare singoli segmenti dell'URL per i file su disco, procedendo da sinistra verso destra. Se non esiste una corrispondenza, qualsiasi elemento rimanenti nell'URL viene passato alla pagina *le informazioni sul percorso*.

Si supponga che un utente effettua una richiesta con questo URL:

`http://www.contoso.com/a/b/c`

La ricerca procede come segue:

1. È presente un file con il percorso e il nome della */a/b/c.cshtml*? In questo caso, eseguire tale pagina e non passarle alcuna informazione. In caso contrario...
2. È presente un file con il percorso e il nome della */a/b.cshtml*? Se quindi, eseguire tale pagina e passare il valore `c` ad esso. In caso contrario...
3. È presente un file con il percorso e il nome della */a.cshtml*? Se quindi, eseguire tale pagina e passare il valore `b/c` ad esso.

Se la ricerca ha trovato non esatta corrisponda *cshtml* file nelle rispettive cartelle specificate, ASP.NET continuano a cercare questi file, a sua volta:

1. */a/b/c/default.cshtml* (senza informazioni sul percorso).
2. */a/b/c/index.cshtml* (senza informazioni sul percorso).

> [!NOTE]
> Per maggiore chiarezza, le richieste di pagine specifiche (vale a dire, le richieste che includono il *. cshtml* estensione del nome file) funzionare esattamente come si aspetterebbe. Una richiesta simile `http://www.contoso.com/a/b.cshtml` eseguiranno la pagina *b.cshtml* correttamente.


All'interno di una pagina, è possibile ottenere le informazioni sui percorsi tramite la pagina `UrlData` proprietà, ovvero un dizionario. Si supponga di avere un file denominato *ViewCustomers.cshtml* e il sito riceve questa richiesta:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Come descritto nelle regole precedenti, la richiesta verrà inviata a una pagina. All'interno della pagina, è possibile usare codice simile al seguente per ottenere e visualizzare le informazioni sul percorso (in questo caso, il valore &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Poiché il routing non prevede l'utilizzo di nomi di file completi, è possibile che ambiguità nel caso di pagine che presentano lo stesso nome ma con una diversa estensioni di file (ad esempio, *MyPage.cshtml* e *MyPage.html*) . Per evitare problemi con il routing, è consigliabile verificare che non hai le pagine nel sito i cui nomi si differenziano solo per l'estensione.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[WebMatrix - URL UrlData e Routing per SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). In questo intervento sul blog di Mike Brind fornisce alcuni dettagli aggiuntivi sul funzionamento del routing in ASP.NET Web Pages.
