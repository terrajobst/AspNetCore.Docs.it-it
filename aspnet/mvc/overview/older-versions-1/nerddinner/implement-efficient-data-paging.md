---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementare il Paging dei dati efficiente | Documenti Microsoft
author: microsoft
description: Passaggio 8 di seguito viene illustrato come aggiungere il supporto del paging l'URL /Dinners in modo che anziché visualizzare 1000 nastri in caso dinners in una sola volta, viene visualizzato solo 10 dinners future alla...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 0188e21438820adf2adbe05b047fdb772540e1a0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="implement-efficient-data-paging"></a>Implementare il Paging dei dati efficiente
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 8 di una liberazione [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 8 viene illustrato come aggiungere il supporto del paging a questo URL /Dinners anziché visualizzare 1000 nastri in caso dinners in una sola volta, verrà solo consente di visualizzare contemporaneamente - 10 dinners future e permettono agli utenti finali di pagina nuovamente e inoltrare tramite l'intero elenco in un modo descrittivo SEO.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner passaggio 8: Il supporto del Paging

Se il sito ha esito positivo, avrà migliaia di dinners future. È necessario assicurarsi che l'interfaccia utente riesce a gestire tutte queste dinners e che consente agli utenti di sfogliare le. A tale scopo, verrà aggiunto il supporto del paging per il nostro */Dinners* URL in modo che anziché 1000 nastri dinners in caso di visualizzazione in una sola volta, si sarà solo dinners future 10 di visualizzare contemporaneamente - e permettono agli utenti finali di pagina back e inoltrare tramite l'intero elenco in un modo descrittivo SEO.

### <a name="index-action-method-recap"></a>Riepilogo di metodo di azione index)

Il metodo di azione Index () all'interno della classe DinnersController attualmente si presenta come di seguito:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Quando viene effettuata una richiesta per il */Dinners* URL, recupera un elenco di tutti i futuri dinners e quindi esegue il rendering di un elenco di tutti i:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Informazioni sui IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;*  è un'interfaccia che è stata introdotta con LINQ come parte di .NET 3.5. Consente gli scenari potente "esecuzione posticipata" che è possibile sfruttare per implementare il supporto di paging.

Nel nostro DinnerRepository ci stiamo restituzione IQueryable&lt;Dinner&gt; sequenza dal metodo di FindUpcomingDinners():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

Oggetto IQueryable&lt;Dinner&gt; oggetto restituito dal metodo di FindUpcomingDinners() incapsula una query per recuperare oggetti Dinner dal database utilizzando LINQ to SQL. Importante, non eseguirà la query nel database fino a quando si tenta di accesso/scorrere i dati nella query o fino a quando non viene chiamato il metodo ToList () su di esso. Il codice che chiama il metodo FindUpcomingDinners(), facoltativamente, è possibile scegliere di aggiungere ulteriori "concatenati" operazioni filtri all'oggetto IQueryable&lt;Dinner&gt; oggetto prima di eseguire la query. LINQ to SQL è quindi abbastanza per eseguire la query combinata sul database quando vengono richiesti i dati.

Per implementare la logica di paging è possibile aggiornare in modo che altri operatori "Skip" e "Take" si applica all'oggetto IQueryable restituito metodo di azione Index () del nostro DinnersController&lt;Dinner&gt; sequenza prima della chiamata ToList () su di essa:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Il codice sopra riportato ignora i primi 10 dinners future nel database e quindi restituisce 20 dinners. LINQ to SQL è abbastanza per costruire una query SQL ottimizzata che esegue questa verrà ignorata la logica nel database SQL: e non nel server web. Ciò significa che, anche se si dispongono di milioni di Dinners future nel database, solo il 10 che vogliamo verranno recuperate come parte della richiesta (rendendola efficiente e scalabile).

### <a name="adding-a-page-value-to-the-url"></a>Aggiunta di un valore di "pagina" all'URL

Anziché a livello di codice un intervallo di pagine specifiche, è opportuno gli URL per includere un parametro "pagina" che indica l'intervallo che Dinner richiede un utente.

#### <a name="using-a-querystring-value"></a>Utilizzo di un valore di stringa di query

Il codice riportato di seguito viene illustrato come è possibile aggiornare il metodo di azione Index () per supportare un parametro di stringa di query e abilitare l'URL come */Dinners? pagina = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Il metodo di azione Index () precedente include un parametro denominato "pagina". Il parametro è dichiarato come un numero intero che ammette valori null (che è il valore int? indica). Ciò significa che il */Dinners? pagina = 2* URL causerà il valore "2" deve essere passato come valore del parametro. Il */Dinners* URL (senza un valore querystring) causerà un valore null da passare.

Si sta moltiplicando il valore di pagina per le dimensioni della pagina (in questo caso 10 righe) per determinare quanti dinners da ignorare. Si sta usando il [c# "operatore null-coalescing" (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) che è utile quando si gestiscono tipi nullable. Il codice precedente assegna pagina sul valore 0 se il parametro della tabella è null.

#### <a name="using-embedded-url-values"></a>Utilizzando i valori di URL incorporato

Un'alternativa all'utilizzo di un valore di stringa di query, è possibile incorporare il parametro della tabella all'interno dell'URL effettivo stesso. Ad esempio: */Dinners/Page/2* o *Dinners/2*. ASP.NET MVC include un potente motore di routing di URL che rende più semplice supportare scenari simili.

È possibile registrare le regole di routine personalizzate che eseguono il mapping di formato qualsiasi URL o l'URL in ingresso a qualsiasi metodo di classe o un'azione del controller che si desidera. Ecco cosa da fare è aprire il file Global. asax all'interno del progetto:

![](implement-efficient-data-paging/_static/image2.png)

E quindi registrare una nuova regola di mapping utilizzando il metodo di supporto MapRoute() come la prima chiamata a una route. MapRoute() riportato di seguito:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Sopra in questa fase Registriamo una nuova regola di routing denominata "UpcomingDinners". Si ha il formato di URL indica "Dinners/pagina / {pagina}", dove {page} è incorporato nell'URL di un valore di parametro. Il terzo parametro al metodo MapRoute() indica che è necessario eseguire il mapping di URL che corrispondono a questo formato per il metodo di azione Index () sulla classe DinnersController.

È possibile usare esattamente lo stesso indice codice che abbiamo prima con questo scenario: stringa di query ad eccezione del fatto ora il parametro "pagina" proverrà dall'URL e non la stringa di query:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

E ora quando si esegue l'applicazione e digitare */Dinners* vedremo i primi 10 dinners future:

![](implement-efficient-data-paging/_static/image3.png)

E quando si digita in */Dinners/Page/1* vedremo la pagina successiva di dinners:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Aggiunta di navigazione tra le pagine dell'interfaccia utente

L'ultimo passaggio per completare lo scenario di paging sarà "Avanti" e "precedente" interfaccia utente di spostamento all'interno di questo modello di visualizzazione per consentire agli utenti di ignorare facilmente i dati di Dinner implementare.

Per implementare correttamente questa, è necessario conoscere il numero totale di Dinners nel database, nonché come numero di pagine di dati questo si traduce in. Sarà quindi necessario calcolare se il valore di "pagina" attualmente richiesta è all'inizio o alla fine dei dati e di mostrare o nascondere l'interfaccia utente "precedente" e "Avanti", di conseguenza. È stato possibile implementare questa logica all'interno di questo metodo di azione Index (). In alternativa è possibile aggiungere una classe helper per il progetto che incapsula la logica in modo più riutilizzabile.

Di seguito è una classe di supporto "PaginatedList" semplice che deriva dall'elenco&lt;T&gt; classe collection integrata in .NET Framework. Implementa una classe collection riutilizzabile che può essere utilizzata per l'impaginazione qualsiasi sequenza di dati IQueryable. Nell'applicazione NerdDinner avremo funziona in IQueryable&lt;Dinner&gt; risultati, ma può altrettanto facilmente essere usato contro IQueryable&lt;prodotto&gt; o IQueryable&lt;cliente&gt;comporta altri scenari di applicazione:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Si noti che in precedenza come viene calcolato e quindi espone proprietà come "PageIndex", "PageSize", "TotalCount" e "TotalPages". Espone inoltre due proprietà di supporto "HasPreviousPage" e "HasNextPage" che indicano se la pagina di dati nella raccolta è all'inizio o alla fine della sequenza originale. Il codice precedente causerà due query SQL da eseguire: il primo a recuperare il conteggio del numero totale di oggetti Dinner (ciò non restituisce gli oggetti: invece di eseguire un'istruzione "Conteggio selezionare" che restituisce un valore integer), mentre la seconda per recuperare solo le righe di dati che necessari dal database per la pagina corrente dei dati.

È quindi possibile aggiornare il metodo di supporto DinnersController.Index() per creare un PaginatedList&lt;Dinner&gt; dal nostro DinnerRepository.FindUpcomingDinners() risultato e passarla a questo modello di visualizzazione:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

È quindi possibile aggiornare il modello di visualizzazione da cui ereditare ViewPage \Views\Dinners\Index.aspx&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; anziché ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;, quindi aggiungere il codice seguente alla fine di questo modello di visualizzazione per mostrare o nascondere l'interfaccia utente di spostamento precedente e successiva:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Si noti in precedenza come viene utilizzato il metodo di supporto Html.RouteLink() per generare i collegamenti ipertestuali. Questo metodo è simile al metodo di supporto Html.ActionLink() che è stata utilizzata in precedenza. La differenza è che la generazione l'URL con "UpcomingDinners" routing regola che viene impostata all'interno di questo file Global. asax. In questo modo si garantisce che abbiamo verranno generati gli URL per il metodo di azione Index () con il formato: */Dinners/pagina / {page}* : il valore {page} è una variabile viene fornito in precedenza in base a PageIndex corrente.

E ora quando si esegue l'applicazione nuovamente vedremo 10 dinners in un momento nel browser:

![](implement-efficient-data-paging/_static/image5.png)

È necessario anche &lt; &lt; &lt; e &gt; &gt; &gt; spostamento dell'interfaccia utente nella parte inferiore della pagina che consente di ignorare inoltra e con le versioni precedenti tramite i dati utilizzando gli URL accessibili motore di ricerca:

![](implement-efficient-data-paging/_static/image6.png)

| **Sul lato dell'argomento: Comprendere le implicazioni di interfaccia IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; è una funzionalità molto potente che consente un'ampia gamma di scenari di esecuzione posticipata interessanti (ad esempio il paging e composizione di query basate su). Come con tutte le potenti funzionalità, opportuno prestare attenzione con modalità di utilizzo e assicurarsi che non è eccessivo. È importante riconoscere che restituisce un oggetto IQueryable&lt;T&gt; risultati dall'archivio consente al codice chiamante accodare sui metodi di operatore concatenate ad esso e pertanto partecipano all'esecuzione di query finale. Se non si desidera fornire codice chiamante questa possibilità, quindi è consigliabile ripristinare il backup di IList&lt;T&gt; o IEnumerable&lt;T&gt; - risultati che contengono i risultati di una query che è già stata eseguita. Per scenari di paginazione in questo caso è necessario inserire la logica di paginazione dati effettivi la chiamata al metodo di repository. In questo scenario è possibile aggiornare il metodo finder FindUpcomingDinners() per una firma che sia restituito un PaginatedList: PaginatedList&lt; Dinner&gt; {} FindUpcomingDinners (pageIndex int, int pageSize) o restituito IList &lt;Dinner&gt;e utilizzare "totalCount" out param per restituire il numero totale di Dinners: IList&lt;Dinner&gt; FindUpcomingDinners (pageIndex int, int pageSize, out int totalCount) {} |

### <a name="next-step"></a>Passo successivo

Ora esaminiamo come è possibile aggiungere il supporto autenticazione e autorizzazione per l'applicazione.

> [!div class="step-by-step"]
> [Precedente](re-use-ui-using-master-pages-and-partials.md)
> [Successivo](secure-applications-using-authentication-and-authorization.md)
