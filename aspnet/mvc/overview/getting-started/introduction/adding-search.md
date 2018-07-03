---
uid: mvc/overview/getting-started/introduction/adding-search
title: Ricerca | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 797384fb04d3ec6fa618ac1848a7efd548b30dbf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362578"
---
<a name="search"></a>Cerca
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Aggiunta di un metodo di ricerca e visualizzazione di ricerca

In questa sezione si aggiungerà la funzionalità di ricerca per il `Index` metodo di azione che consente di ricercare i film per genere o un nome.

## <a name="updating-the-index-form"></a>Aggiornamenti del modulo di indice

Per iniziare, aggiornare il `Index` metodo di azione esistente `MoviesController` classe. Ecco il codice:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

La prima riga del `Index` metodo creati i seguenti elementi [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query per selezionare i film:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

La query viene definita in questo punto, ma non è ancora stata eseguita sul database.

Se il `searchString` parametro contiene una stringa, la query dei film viene modificata per filtrare il valore della stringa di ricerca, usando il codice seguente:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

Il codice `s => s.Title` precedente è un'[espressione lambda](https://msdn.microsoft.com/library/bb397687.aspx). Le espressioni lambda vengono utilizzate in basate sul metodo [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query come argomenti dei metodi degli operatori query standard, ad esempio le [in cui](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metodo usato nel codice precedente. Le query LINQ non vengono eseguite al momento della definizione o quando vengono modificati chiamando un metodo, ad esempio `Where` o `OrderBy`. Al contrario, viene rinviato l'esecuzione di query, il che significa che la valutazione di un'espressione viene ritardata finché non viene effettivamente un'iterazione il relativo valore realizzato o la [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) viene chiamato il metodo. Nel `Search` campione, in cui viene eseguita la query di *index. cshtml* visualizzazione. Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> Il [Contains](https://msdn.microsoft.com/library/bb155125.aspx) metodo viene eseguito sul database, non il codice c# riportato sopra. Nel database di [Contains](https://msdn.microsoft.com/library/bb155125.aspx) esegue il mapping a [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), ovvero maiuscole e minuscole.

A questo punto è possibile aggiornare il `Index` vista in cui verrà visualizzato il form per l'utente.

Eseguire l'applicazione e passare a */Movies/Index*. Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL. Vengono visualizzati i film filtrati.

![SearchQryStr](adding-search/_static/image1.png)

Se si modifica la firma del `Index` metodo di avere un parametro denominato `id`, il `id` corrisponderà a parametro il `{id}` segnaposto per il valore predefinito consente di indirizzare set nel *App\_Start RouteConfig.cs* file.

[!code-json[Main](adding-search/samples/sample4.json)]

Originale `Index` metodo si presenta come segue:

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Modificato `Index` metodo sarà simile alla seguente:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.

![](adding-search/_static/image2.png)

Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film. Quindi, ora si aggiungerai dell'interfaccia utente per consentire loro filtrare i film. Se è stata modificata la firma del `Index` metodo da testare come passare il parametro ID associato alla route, modificarlo in modo che i `Index` metodo accetta un parametro di stringa denominato `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Aprire il *Views\Movies\Index.cshtml* del file e subito dopo `@Html.ActionLink("Create New", "Create")`, aggiungere il modulo markup evidenziato di seguito:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

Il `Html.BeginForm` helper crea un'apertura `<form>` tag. Il `Html.BeginForm` helper, il form inviare a se stessa quando l'utente invia il form scegliendo il **filtro** pulsante.

Visual Studio 2013 è un miglioramento interessante per la visualizzazione e modifica dei file di visualizzazione. Quando si esegue l'applicazione con un file di visualizzazione open, Visual Studio 2013 richiama il metodo di azione del controller corretto per visualizzare la vista.

![](adding-search/_static/image3.png)

Con la visualizzazione dell'indice aperto in Visual Studio (come illustrato nell'immagine precedente), toccare Ctr F5 o F5 per eseguire l'applicazione e quindi provare a cercare un film.

![](adding-search/_static/image4.png)

È presente alcun `HttpPost` eseguire l'overload del `Index` (metodo). Non è necessario, poiché il metodo non modifica lo stato dell'applicazione, ma filtra solo i dati.

È possibile aggiungere il metodo `HttpPost Index` seguente. In tal caso, l'invoker dell'azione corrisponderebbe la `HttpPost Index` metodo e il `HttpPost Index` metodo viene eseguito come illustrato nell'immagine seguente.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Tuttavia, anche se si aggiunge questa versione `HttpPost` del metodo `Index`, esiste una limitazione sul modo sulla relativa implementazione. Si supponga che si desideri usare come segnalibro una ricerca specifica o inviare un collegamento agli amici su cui possono fare clic per visualizzare lo stesso elenco filtrato di film. Si noti che l'URL per la richiesta HTTP POST è lo stesso URL per la richiesta GET (localhost: xxxxx/Movies/Index): nessuna informazione di ricerca nell'URL stesso. Diritto a questo punto, le informazioni sulla stringa di ricerca viene inviati al server come un valore del campo modulo. Ciò significa che non è possibile acquisire le informazioni di ricerca per aggiungere un segnalibro o inviare agli amici in un URL.

La soluzione consiste nell'usare un overload di `BeginForm` che specifica che la richiesta POST deve aggiungere le informazioni sulla ricerca per l'URL e che devono essere instradato al `HttpGet` versione del `Index` (metodo). Sostituire le funzioni senza parametri `BeginForm` metodo con il markup seguente:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

A questo punto quando si invia una ricerca, l'URL contiene una stringa di query di ricerca. La ricerca passerà anche al metodo di azione `HttpGet Index`, anche se si dispone di un metodo `HttpPost Index`.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Aggiunta di ricerca in base al genere

Se è stato aggiunto il `HttpPost` versione del `Index` metodo, eliminarlo subito.

Successivamente, si aggiungerà una funzionalità per consentire agli utenti di eseguire la ricerca di film in base al genere. Sostituire il metodo `Index` con il codice seguente:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Questa versione del `Index` metodo accetta un parametro aggiuntivo, vale a dire `movieGenre`. Creano le prime righe di codice un `List` oggetto per contenere cinematografici dal database.

Il codice seguente è una query LINQ che recupera tutti i generi dal database.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Il codice Usa il `AddRange` metodo del tipo generico `List` raccolta a cui aggiungere tutti i generi distinti all'elenco. (Senza il `Distinct` modificatore, verrebbero aggiunti generi duplicati, ad esempio, commedie verrebbero aggiunto due volte nel nostro esempio). Il codice quindi archivia l'elenco dei generi nel `ViewBag.MovieGenre` oggetto. L'archiviazione dei dati della categoria (del tali un film genre) come un [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) dell'oggetto un `ViewBag`, quindi l'accesso ai dati in una casella di riepilogo a discesa categoria è un approccio tipico per le applicazioni MVC.

Il codice seguente viene illustrato come controllare il `movieGenre` parametro. Se non è vuota, il codice ulteriormente vincola la query dei film per limitare il film selezionato per il genere specificato.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Come affermato in precedenza, la query non viene eseguita sulla base dei dati fino a quando non viene scorso l'elenco di film (situazione che si verifica nella visualizzazione dopo il `Index` metodo di azione restituisce).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Aggiunta di Markup per la visualizzazione dell'indice per supportare la ricerca in base al genere

Aggiungere un `Html.DropDownList` helper per il *Views\Movies\Index.cshtml* nel file, subito prima di `TextBox` helper. Di seguito è riportato il markup completato:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

Nel codice seguente:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

Il parametro "MovieGenre" fornisce la chiave per la `DropDownList` helper per trovare un' `IEnumerable<SelectListItem>` nel `ViewBag`. Il `ViewBag` è stato popolato nel metodo di azione:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Il parametro "All" fornisce un'etichetta di opzione. Se si osserva tale scelta nel browser, si noterà che il relativo attributo "value" sia vuoto. Poiché consente di filtrare solo i controller `if` la stringa non è `null` o vuoto, invio di un valore vuoto per `movieGenre` Mostra tutti i generi.

È anche possibile impostare un'opzione da selezionare per impostazione predefinita. Se si vuole ottenere "Commedie" come opzione di impostazione predefinita, modificare il codice nel Controller come segue:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Eseguire l'applicazione e passare a */Movies/Index*. Eseguire una ricerca in base al genere dal nome del film e da entrambi i criteri.

![](adding-search/_static/image8.png)

In questa sezione si crea un metodo di azione di ricerca e visualizzazione che consentono agli utenti di ricerca in base al titolo del film e genere. Nella sezione successiva, vedrà come aggiungere una proprietà per il `Movie` modello e come aggiungere un inizializzatore che creerà automaticamente un database di test.

> [!div class="step-by-step"]
> [Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-a-new-field.md)
