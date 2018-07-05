---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementare il Paging dei dati efficiente | Microsoft Docs
author: microsoft
description: Passaggio 8 viene illustrato come aggiungere supporto per lo spostamento al nostro URL /Dinners in modo che anziché visualizzare 1000 nastri in caso dinners in una sola volta, verrà visualizzato solo 10 dinners imminenti in...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: e6347c817c7518ef96ffbbf83cf98dd4dc6c011e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372449"
---
<a name="implement-efficient-data-paging"></a>Implementare il Paging dei dati efficiente
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 8 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 8 viene illustrato come aggiungere supporto per lo spostamento al nostro URL /Dinners in modo che anziché visualizzare 1000 nastri in caso dinners in una sola volta, verranno solo 10 dinners imminente di visualizzare contemporaneamente - e consentire agli utenti finali di pagina indietro e inoltrare tramite l'intero elenco in un modo descrittivo SEO.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner passaggio 8: Supporto del Paging

Se il sito ha esito positivo, avrà migliaia di dinners imminente. È necessario assicurarsi che l'interfaccia utente scalabile per gestire tutti questi dinners e consente agli utenti di esplorarle. A tale scopo, verrà aggiunto il supporto di paging a nostro */Dinners* URL in modo che anziché visualizzazione 1000 nastri in caso dinners a una sola volta, si verrà solo dinners prossimi 10 di visualizzare contemporaneamente - e consentire agli utenti finali di pagina back e inoltrare tramite l'intero elenco nel un modo descrittivo SEO.

### <a name="index-action-method-recap"></a>Riepilogo di metodo di azione index)

Il metodo di azione Index () all'interno della classe DinnersController attualmente si presenta come di seguito:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Quando viene effettuata una richiesta per il */Dinners* URL, recupera un elenco di tutte le future dinners e quindi esegue il rendering di un elenco di tutti gli elementi out:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Understanding IQuerable&lt;T&gt;

*Oggetto IQueryable&lt;T&gt;*  è un'interfaccia che è stata introdotta con LINQ come parte di .NET 3.5. Consente gli scenari avanzati "esecuzione posticipata" che possiamo usufruire dei vantaggi di implementare il supporto di paging.

Nel nostro DinnerRepository è sta restituendo un oggetto IQueryable&lt;Dinner&gt; sequenza dal nostro metodo FindUpcomingDinners():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

All'oggetto IQueryable&lt;Dinner&gt; incapsulato dall'oggetto restituito dal nostro metodo FindUpcomingDinners() una query per recuperare oggetti Dinner dal database utilizzando LINQ to SQL. Importante, non verrà eseguito la query sul database finché non si tenta di accedere/scorrere i dati nella query oppure fino a quando non viene chiamato il metodo ToList () su di esso. Il codice che chiama il metodo FindUpcomingDinners(), facoltativamente, è possibile scegliere di aggiungere ulteriori filtri/operazioni "concatenati" all'oggetto IQueryable&lt;Dinner&gt; oggetto prima di eseguire la query. LINQ to SQL è quindi in grado di eseguire la query combinata sul database quando vengono richiesti i dati.

Per implementare la logica di paging è possibile aggiornare in modo da essere applicata operatori aggiuntivi "Skip" e "Take" all'oggetto IQueryable restituito metodo di azione Index () del nostro DinnersController&lt;Dinner&gt; sequenza prima di chiamare ToList () su di essa:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Il codice sopra riportato ignora i primi 10 dinners imminenti nel database e restituisce quindi 20 dinners. LINQ to SQL è abbastanza intelligente da costruire una con ottimizzazione per la query SQL che esegue questa verrà ignorata per la logica nel database SQL: e non nel server web. Ciò significa che anche se si dispongono di milioni di Dinners imminenti nel database, solo il 10 che vogliamo verranno recuperate come parte di questa richiesta (rendendola efficiente e scalabile).

### <a name="adding-a-page-value-to-the-url"></a>Aggiunta di un valore "pagina" all'URL

Anziché impostare come hardcoded un intervallo di pagine specifici, è opportuno gli URL per includere un parametro "pagina" che indica quale intervallo la cena richiede un utente.

#### <a name="using-a-querystring-value"></a>Utilizzo di un valore di stringa di query

Il codice seguente illustra come è possibile aggiornare il metodo di azione Index () per supportare un parametro di stringa di query e consentire gli URL, ad esempio */Dinners? pagina = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Il metodo di azione Index () precedente ha un parametro denominato "page". Il parametro viene dichiarato come un numero intero che ammette valori null (ovvero quali int? indica). Ciò significa che il */Dinners? pagina = 2* URL causerà un valore "2" deve essere passato come valore del parametro. Il */Dinners* URL (senza un valore querystring) causerà un valore null da passare.

Si sta moltiplicando il valore di pagina per le dimensioni della pagina (in questo caso 10 righe) per determinare quanti dinners ignorare. Usiamo il [c# "operatore null-coalescing" (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) che è utile quando si lavora con tipi nullable. Il codice precedente assegna pagina il valore 0 se il parametro di pagina è null.

#### <a name="using-embedded-url-values"></a>Usando i valori di URL incorporati

Un'alternativa all'utilizzo di un valore di stringa di query, è possibile incorporare il parametro di pagina all'interno dell'URL effettivo stesso. Ad esempio: */Dinners/Page/2* oppure *Dinners/2*. ASP.NET MVC include un potente motore di routing di URL che rende più semplice supportare scenari simili.

Possiamo registriamo le regole di routing personalizzate che eseguono il mapping di formato qualsiasi URL o l'URL in ingresso per qualsiasi metodo di azione o classe controller desiderato. È sufficiente attività da eseguire consiste nell'aprire il file Global. asax all'interno del progetto:

![](implement-efficient-data-paging/_static/image2.png)

E quindi registrare una nuova regola di mapping utilizzando il metodo helper MapRoute(), ad esempio la prima chiamata alla route. MapRoute() riportato di seguito:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Sopra si registra una nuova regola di routing denominata "UpcomingDinners". Si indica ha il formato dell'URL "Dinners/pagina / {pagina}", dove {pagina} è il valore di un parametro incorporato all'interno dell'URL. Il terzo parametro al metodo MapRoute() indica che si dovrebbe eseguire il mapping di URL che corrisponde a questo formato per il metodo di azione Index () sulla classe DinnersController.

È possibile usare lo stesso esatto codice di Index (), creato in precedenza con questo scenario di stringa di query, ad eccezione del fatto ora il parametro "pagina" proverranno dall'URL e non il parametro querystring:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

E ora quando si esegue l'applicazione e digitare */Dinners* vedremo i primi 10 dinners future:

![](implement-efficient-data-paging/_static/image3.png)

E quando si digita nella */Dinners/Page/1* si vedrà la pagina successiva di dinners:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Aggiunta di navigazione tra le pagine dell'interfaccia utente

L'ultimo passaggio per completare questo scenario il paging sarà implementare l'esplorazione "Avanti" e "precedente" dell'interfaccia utente entro il modello di visualizzazione per consentire agli utenti di ignorare facilmente i dati cena.

Per implementare correttamente questo, è necessario conoscere il numero totale di Dinners nel database, anche il numero di pagine di dati questo si traduce in. È quindi necessario calcolare se il valore attualmente richiesta "page" è all'inizio o alla fine dei dati e visualizzare o nascondere l'interfaccia utente "Avanti" e "precedente" di conseguenza. È stato possibile implementare questa logica all'interno di metodo di azione Index (). In alternativa è possibile aggiungere una classe helper per il progetto che incapsula la logica in modo più riutilizzabile.

Di seguito è una classe helper "PaginatedList" semplice che deriva dall'elenco&lt;T&gt; classe collection integrata in .NET Framework. Implementa una classe collection riutilizzabile che può essere utilizzata per paginare qualsiasi sequenza di dati di IQueryable. Nella nostra applicazione NerdDinner avremo funziona solo su IQueryable&lt;Dinner&gt; risultati, ma può altrettanto facilmente essere usato contro IQueryable&lt;prodotto&gt; IQueryable o&lt;cliente&gt;comporterà altri scenari di applicazione:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Si noti che in precedenza come calcola e quindi espone proprietà, ad esempio "PageIndex", "PageSize", "TotalCount" e "TotalPages". Inoltre espone due proprietà di supporto "HasPreviousPage" e "HasNextPage" che indicano se la pagina di dati nella raccolta è all'inizio o alla fine della sequenza originale. Il codice precedente causa due query SQL da eseguire - i primi a recuperare il conteggio del numero totale di oggetti Dinner (questo non restituisce oggetti: invece di eseguire un'istruzione "SELECT COUNT" che restituisce un numero intero), mentre la seconda per recuperare solo le righe di dati che necessari dal database per la pagina corrente dei dati.

È quindi possibile aggiornare il metodo helper DinnersController.Index() per creare un PaginatedList&lt;Dinner&gt; dal nostro DinnerRepository.FindUpcomingDinners() risultato e passarlo al nostro modello di vista:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

È quindi possibile aggiornare il modello di vista \Views\Dinners\Index.aspx da cui ereditare ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; anziché ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;e quindi aggiungere il codice seguente alla fine del nostro modello di visualizzazione per mostrare o nascondere l'interfaccia utente di navigazione successivo e precedente:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Si noti che in precedenza come utilizziamo il metodo helper Html.RouteLink() per generare i collegamenti ipertestuali. Questo metodo è simile al metodo helper Html.ActionLink() che abbiamo usato in precedenza. La differenza è che la generazione di URL usando la regola che viene impostata nel nostro file Global. asax di routing "UpcomingDinners". In questo modo si garantisce che verrà generata gli URL per il metodo di azione Index () che hanno il formato: */Dinners/pagina / {pagina}* : il valore {pagina} è una variabile forniamo sopra base PageIndex corrente.

E ora quando eseguiamo l'applicazione anche in questo caso si vedrà 10 dinners alla volta nel browser:

![](implement-efficient-data-paging/_static/image5.png)

È anche disponibile &lt; &lt; &lt; e &gt; &gt; &gt; navigazione dell'interfaccia utente nella parte inferiore della pagina che consente di andare avanti e con le versioni precedenti tramite i dati tramite ricerca gli URL accessibili motore:

![](implement-efficient-data-paging/_static/image6.png)

| **Sul lato dell'argomento: Comprendere le implicazioni di IQueryable&lt;T&gt;** |
| --- |
| Oggetto IQueryable&lt;T&gt; è una funzionalità molto potente che consente un'ampia gamma di scenari interessanti esecuzione posticipata (come il paging e composizione di query basate su). Come con tutte le funzionalità avanzate, opportuno prestare attenzione con modalità di utilizzo e assicurarsi che non è eccessivo. È importante riconoscere che restituisce un'interfaccia IQueryable&lt;T&gt; risultato dal repository consente al codice chiama accodare sui metodi di operatore concatenate a esso e quindi partecipare all'esecuzione delle query ultimate. Se non si desidera fornire codice chiamante questa funzionalità, quindi è necessario restituire il backup di IList&lt;T&gt; o IEnumerable&lt;T&gt; - risultati che contengono i risultati di una query che è già stata eseguita. Per gli scenari di paginazione si dovrebbero eseguire il push la logica di impaginazione effettiva dei dati nel metodo repository chiamato. In questo scenario è possibile aggiornare il metodo finder FindUpcomingDinners() per avere una firma che sia restituito un PaginatedList: PaginatedList&lt; Dinner&gt; FindUpcomingDinners (pageIndex int, int pageSize) {} o restituire un oggetto IList &lt;Dinner&gt;e utilizzare un "totalCount" out param per restituire il numero totale di Dinners: IList&lt;Dinner&gt; FindUpcomingDinners (pageIndex int, int pageSize, out totalCount int) {} |

### <a name="next-step"></a>Passo successivo

Ora esaminiamo come possiamo aggiungere supportano l'autenticazione e autorizzazione all'applicazione.

> [!div class="step-by-step"]
> [Precedente](re-use-ui-using-master-pages-and-partials.md)
> [Successivo](secure-applications-using-authentication-and-authorization.md)
