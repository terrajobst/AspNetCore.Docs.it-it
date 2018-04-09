---
uid: mvc/overview/getting-started/introduction/adding-search
title: Ricerca | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 8afa72d4dbc4695e7d26c6ef4052be08a7c69080
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="search"></a>Cerca
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Aggiunta di un metodo di ricerca e visualizzazione di ricerca

In questa sezione viene aggiunta la funzionalità di ricerca per il `Index` metodo di azione che consente di eseguire la ricerca filmati genre o nome.

## <a name="updating-the-index-form"></a>Aggiornamenti del modulo di indice

Avviare aggiornando il `Index` il metodo di azione esistente `MoviesController` classe. Ecco il codice:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

La prima riga del `Index` metodo vengono creati i seguenti [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query per selezionare i film:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

La query viene definita a questo punto, ma non è ancora stata eseguita sul database.

Se il `searchString` parametro contiene una stringa, viene modificata la query di filmati per filtrare il valore della stringa di ricerca, tramite il codice seguente:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

Il codice `s => s.Title` precedente è un'[espressione lambda](https://msdn.microsoft.com/library/bb397687.aspx). Le espressioni lambda vengono utilizzate in base al metodo [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) esegue una query come argomenti dei metodi degli operatori query standard, ad esempio il [dove](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) utilizzato nel codice precedente. Le query LINQ non vengono eseguite quando vengono definite o quando vengono modificati chiamando un metodo, ad esempio `Where` o `OrderBy`. In alternativa, esecuzione della query è rinviata, il che significa che la valutazione di un'espressione viene ritardata fino a quando il relativo valore realizzato viene effettivamente eseguita un'iterazione o [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metodo viene chiamato. Nel `Search` esempio, in cui viene eseguita la query di *cshtml* visualizzazione. Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> Il [Contains](https://msdn.microsoft.com/library/bb155125.aspx) metodo viene eseguito sul database, non il codice c# sopra riportato. Il database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) esegue il mapping a [SQL come](https://msdn.microsoft.com/library/ms179859.aspx), che viene fatta distinzione tra maiuscole e minuscole.

Ora è possibile aggiornare il `Index` visualizzazione che consente di visualizzare il form per l'utente.

Eseguire l'applicazione e passare a *filmati/indice*. Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL. Vengono visualizzati i film filtrati.

![SearchQryStr](adding-search/_static/image1.png)

Se si modifica la firma del `Index` metodo contiene un parametro denominato `id`, `id` parametro corrisponderà il `{id}` indirizza i segnaposto per il valore predefinito impostato nel *App\_Start RouteConfig.cs* file.

[!code-json[Main](adding-search/samples/sample4.json)]

Originale `Index` metodo è simile al seguente:

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Modificato `Index` metodo si presenterebbe come segue:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.

![](adding-search/_static/image2.png)

Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film. Quindi, ora è aggiungerai dell'interfaccia utente per consentire loro filtrare film. Se è stata modificata la firma del `Index` metodo per verificare come passare il parametro ID di associazione di route, modificarlo in modo che il `Index` metodo accetta un parametro di stringa denominato `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Aprire il *Views\Movies\Index.cshtml* file e subito dopo `@Html.ActionLink("Create New", "Create")`, aggiungere il markup modulo evidenziato di seguito:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

Il `Html.BeginForm` helper crea un'apertura `<form>` tag. Il `Html.BeginForm` helper, il form inviare a se stessa quando l'utente invia il form scegliendo il **filtro** pulsante.

Visual Studio 2013 è un miglioramento utile per la visualizzazione e modifica dei file di visualizzazione. Quando si esegue l'applicazione con un file della vista aperta, Visual Studio 2013 richiama il metodo di azione del controller corretto per visualizzare la visualizzazione.

![](adding-search/_static/image3.png)

Con la visualizzazione dell'indice aperto in Visual Studio (come illustrato nella figura precedente), toccare Ctr F5 o F5 per eseguire l'applicazione, quindi ripetere la ricerca di un film.

![](adding-search/_static/image4.png)

È presente alcun `HttpPost` overload di `Index` metodo. Non è necessaria, poiché il metodo non modifica lo stato dell'applicazione, solo il filtraggio dei dati.

È possibile aggiungere il metodo `HttpPost Index` seguente. In tal caso, corrisponderà a invoker dell'azione di `HttpPost Index` (metodo) e `HttpPost Index` metodo verrebbe eseguito come illustrato nell'immagine seguente.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Tuttavia, anche se si aggiunge questa versione `HttpPost` del metodo `Index`, esiste una limitazione sul modo sulla relativa implementazione. Si supponga che si desideri usare come segnalibro una ricerca specifica o inviare un collegamento agli amici su cui possono fare clic per visualizzare lo stesso elenco filtrato di film. Si noti che l'URL per la richiesta HTTP POST è lo stesso come URL per la richiesta di recupero (localhost:xxxxx filmati/indice)--non sono disponibili informazioni di ricerca nell'URL stesso. Diritto a questo punto, le informazioni sulla stringa di ricerca viene inviati al server come un valore del campo modulo. Ciò significa che non è possibile acquisire le informazioni di ricerca per aggiungere un segnalibro o inviare agli amici in un URL.

La soluzione consiste nell'usare un overload di `BeginForm` che specifica che la richiesta POST è necessario aggiungere le informazioni di ricerca per l'URL e che devono essere instradato al `HttpGet` versione il `Index` (metodo). Sostituire le funzioni senza parametri `BeginForm` (metodo) con il markup seguente:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

A questo punto quando si invia una ricerca, l'URL contiene una stringa di query di ricerca. La ricerca passerà anche al metodo di azione `HttpGet Index`, anche se si dispone di un metodo `HttpPost Index`.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Aggiunta di ricerca per genere

Se è stato aggiunto il `HttpPost` versione il `Index` metodo elimina ora.

Successivamente, aggiungere una funzionalità per consentire agli utenti di cercare film per genere. Sostituire il metodo `Index` con il codice seguente:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Questa versione di `Index` metodo accetta un parametro aggiuntivo, vale a dire `movieGenre`. Creano le prime righe di codice un `List` oggetto che contenga generi film dal database.

Il codice seguente è una query LINQ che recupera tutti i generi dal database.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Il codice Usa il `AddRange` metodo del tipo generico `List` raccolta da aggiungere all'elenco di tutti i generi distinti. (Senza il `Distinct` modificatore, generi duplicati verrebbero aggiunto, ad esempio, verrebbero aggiunto comici due volte nel nostro esempio). Il codice quindi archivia l'elenco di generi nel `ViewBag.MovieGenre` oggetto. L'archiviazione dei dati della categoria (del tali un genere) come un [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) dell'oggetto un `ViewBag`, quindi l'accesso ai dati di categoria in una casella di riepilogo a discesa è un approccio tipico per le applicazioni MVC.

Il codice seguente viene illustrato come controllare il `movieGenre` parametro. Se non è vuota, ulteriormente il codice vincola la query di filmati per limitare i film selezionati per il genere specificato.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Come indicato in precedenza, la query non viene eseguita sulla base dei dati fino a quando non viene scorso l'elenco di film (situazione che si verifica nella visualizzazione, dopo il `Index` metodo di azione restituisce).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Aggiunta di Markup per la visualizzazione dell'indice per supportare la ricerca per genere

Aggiungere un `Html.DropDownList` helper per il *Views\Movies\Index.cshtml* file, appena prima di `TextBox` helper. Di seguito è riportato il markup completato:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

Nel codice seguente:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

Il parametro "MovieGenre" fornisce la chiave per la `DropDownList` helper per trovare un `IEnumerable<SelectListItem>` nel `ViewBag`. Il `ViewBag` è stato popolato nel metodo di azione:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Il parametro "All" fornisce un'etichetta di opzione. Se si analizza l'opzione nel browser, si noterà che l'attributo "value" è vuota. Poiché il controller filtra solo `if` la stringa non è `null` o empty, invio di un valore vuoto per `movieGenre` Mostra tutti i generi.

È inoltre possibile impostare un'opzione per essere selezionata per impostazione predefinita. Se si desidera "Commedia" come opzione di impostazione predefinita, si modificherebbe il codice nel Controller come illustrato di seguito:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Eseguire l'applicazione e passare a *filmati/indice*. Eseguire una ricerca per genere, in base al nome di film e da entrambi i criteri.

![](adding-search/_static/image8.png)

In questa sezione è creato un metodo di azione di ricerca e visualizzazione che consentono agli utenti di ricerca in base al genere e titolo del film. Nella sezione successiva verrà esaminato come aggiungere una proprietà per il `Movie` modello e come aggiungere un inizializzatore che verrà creato automaticamente un database di test.

> [!div class="step-by-step"]
> [Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-a-new-field.md)
