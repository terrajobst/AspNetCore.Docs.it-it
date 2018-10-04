---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Esaminare i metodi di modifica e visualizzazione di modifica | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro e molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 1223e110ce98c5b511312de42bc2992045a4a40b
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577990"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Esaminare i metodi di modifica e visualizzazione di modifica
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.


In questa sezione si esamina i metodi di azione generata e visualizzazioni per il controller di film. Quindi si aggiungerà una pagina di ricerca personalizzato.

Eseguire l'applicazione e passare al `Movies` controller aggiungendo */Movies* all'URL nella barra degli indirizzi del browser. Sposta il puntatore del mouse su un **modifica** collegamento per visualizzare l'URL a essa collegate.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Il **Edit** collegamento è stato generato dal `Html.ActionLink` metodo nel *Views\Movies\Index.cshtml* visualizzazione:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![HTML. ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

Il `Html` oggetto è un helper esposto mediante una proprietà nel [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) classe di base. Il `ActionLink` metodo dell'helper consente di generare in modo dinamico i collegamenti ipertestuali HTML che si collegano ai metodi di azione nel controller. Il primo argomento per il `ActionLink` metodo è il testo del collegamento per eseguire il rendering (ad esempio, `<a>Edit Me</a>`). Il secondo argomento è il nome del metodo di azione da richiamare. L'argomento finale è un [oggetto anonimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) che genera i dati della route (in questo caso, l'ID di 4).

Il collegamento generato illustrato nell'immagine precedente è `http://localhost:xxxxx/Movies/Edit/4`. La route predefinita (stabilito nelle *App\_Start\RouteConfig.cs*) accetta il modello di URL `{controller}/{action}/{id}`. Pertanto, ASP.NET si traduce `http://localhost:xxxxx/Movies/Edit/4` in una richiesta per il `Edit` metodo di azione del `Movies` controller con il parametro `ID` pari a 4. Esaminare il codice seguente il *App\_Start\RouteConfig.cs* file.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

È anche possibile passare parametri di metodo di azione usando una stringa di query. Ad esempio, l'URL `http://localhost:xxxxx/Movies/Edit?ID=4` passa inoltre il parametro `ID` pari a 4 per il `Edit` metodo di azione del `Movies` controller.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Aprire il `Movies` controller. I due `Edit` i metodi di azione sono illustrati di seguito.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Si noti che il secondo metodo di azione `Edit` è preceduto dall'attributo `HttpPost`. Questo attributo specifica che eseguono l'overload del `Edit` metodo può essere richiamato solo per le richieste POST. È possibile applicare il `HttpGet` attributo al primo metodo di modifica, ma che non è necessario perché è il valore predefinito. (Si farà riferimento ai metodi di azione che vengono assegnati in modo implicito il `HttpGet` dell'attributo come `HttpGet` metodi.)

Il `HttpGet` `Edit` metodo accetta il parametro ID film, Cerca il film tramite Entity Framework `Find` (metodo) e restituisce il film selezionato nella visualizzazione di modifica. Il parametro ID specifica un [il valore predefinito](https://msdn.microsoft.com/library/dd264739.aspx) di zero se la `Edit` viene chiamato senza un parametro. Se non viene trovato un film, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) viene restituito. Quando il sistema di scaffolding ha creato la vista Edit, ha esaminato la classe `Movie` e il codice creato per eseguire il rendering degli elementi `<label>` e `<input>` per ogni proprietà della classe. Nell'esempio seguente mostra la visualizzazione di modifica che è stata generata:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Si noti come il modello di vista ha un `@model MvcMovie.Models.Movie` istruzione all'inizio del file: Specifica che il modello per il modello di vista sia del tipo previsto dalla vista `Movie`.

Il codice con scaffolding Usa diversi *metodi helper* per semplificare il markup HTML. Il [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) helper Visualizza il nome del campo (&quot;Title&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, o &quot;prezzo &quot;). Il [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper esegue il rendering HTML `<input>` elemento. Il [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper Visualizza gli eventuali messaggi di convalida associati alla proprietà.

Eseguire l'applicazione e passare al */Movies* URL. Fare clic su un collegamento **Edit** (Modifica). Nel browser visualizzare l'origine per la pagina. Seguito è riportato il codice HTML per l'elemento del form.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

Il `<input>` elementi si trovano in un elemento HTML `<form>` elemento la cui `action` attributo è impostato su post per il */Movies/Edit* URL. I dati del modulo verranno inviati al server quando il **modifica** si fa clic sul pulsante.

## <a name="processing-the-post-request"></a>Elaborazione della richiesta POST

Nell'elenco seguente viene indicata la versione `HttpPost` del metodo di azione `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Il [Raccoglitore di modelli ASP.NET MVC](https://msdn.microsoft.com/magazine/hh781022.aspx) accetta i valori di modulo e consente di creare un `Movie` che viene passato come il `movie` parametro. Il metodo `ModelState.IsValid` verifica che i dati inviati nel formato possano essere usati per cambiare (modificare o aggiornare) un oggetto `Movie`. Se i dati sono validi, i dati dei film viene salvati per il `Movies` raccolta del `db(MovieDBContext` istanza). I nuovi dati del film viene salvati nel database chiamando il `SaveChanges` metodo `MovieDBContext`. Dopo aver salvato i dati, il codice reindirizza l'utente per il `Index` metodo di azione del `MoviesController` classe, che consente di visualizzare della raccolta di film, incluse le modifiche appena apportate.

Se i valori inviati non sono validi, essi verranno nuovamente visualizzati nel formato. Il `Html.ValidationMessageFor` helper nel *Edit. cshtml* vista modello ci occupiamo della visualizzazione dei messaggi di errore appropriato.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> per supportare la convalida di jQuery per impostazioni locali di lingua diversa dall'inglese che usano la virgola (&quot;,&quot;) per un separatore decimale, è necessario includere *globalize.js* specifiche e *cultures/globalize.cultures.js* file (da [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) e JavaScript usare `Globalize.parseFloat`. Il codice seguente illustra le modifiche al file Views\Movies\Edit.cshtml per lavorare con i &quot;fr-FR&quot; delle impostazioni cultura:


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Il campo decimale richiedano una virgola, non un separatore decimale. Come correzione temporanea, è possibile aggiungere l'elemento di globalizzazione nel file Web. config radice di progetti. Il codice seguente illustra l'elemento globalization con le impostazioni cultura impostate su inglese degli Stati Uniti.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Tutti i `HttpGet` metodi seguono un modello simile. Ricevono un oggetto film (o un elenco di oggetti, nel caso di `Index`) e passare il modello alla visualizzazione. Il `Create` metodo passa un oggetto vuoto per la visualizzazione di creazione. Tutti i metodi che creano, modificano, eliminano o cambiano in altro modo i dati, eseguono questa operazione nell'overload `HttpPost` del metodo. Modifica dei dati in un metodo HTTP GET è un rischio per la sicurezza, come descritto nella voce del post di blog [ASP.NET MVC suggerimento & 46: non usare i collegamenti di eliminazione poiché creano le falle](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modifica dei dati in un metodo GET viola anche le procedure consigliate HTTP e l'architettura [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) modello, che consente di specificare che le richieste GET non devono modificherà lo stato dell'applicazione. In altre parole, l'esecuzione di un'operazione GET deve essere sicura, senza effetti collaterali e non modificare dati persistenti.

## <a name="adding-a-search-method-and-search-view"></a>Aggiunta di un metodo di ricerca e visualizzazione di ricerca

In questa sezione si aggiungerà un `SearchIndex` metodo di azione che consente di ricercare i film per genere o un nome. Questo sarà disponibile tramite il */Movies/SearchIndex* URL. La richiesta verrà visualizzato un form HTML che contiene gli elementi di input che un utente può immettere per cercare un film. Quando un utente invia il form, il metodo di azione verrà ottenere i valori di ricerca registrati dall'utente e usare i valori per eseguire ricerche nel database.

## <a name="displaying-the-searchindex-form"></a>Visualizzazione di Form SearchIndex

Iniziare aggiungendo un `SearchIndex` metodo di azione esistente `MoviesController` classe. Il metodo restituisce una visualizzazione contenente un form HTML. Ecco il codice:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

La prima riga del `SearchIndex` metodo creati i seguenti elementi [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query per selezionare i film:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

La query viene definita in questo punto, ma non è ancora stata eseguita a fronte dell'archivio dati.

Se il `searchString` parametro contiene una stringa, la query dei film viene modificata per filtrare il valore della stringa di ricerca, usando il codice seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

Il codice `s => s.Title` precedente è un'[espressione lambda](https://msdn.microsoft.com/library/bb397687.aspx). Le espressioni lambda vengono utilizzate in basate sul metodo [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query come argomenti dei metodi degli operatori query standard, ad esempio le [in cui](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metodo usato nel codice precedente. Le query LINQ non vengono eseguite al momento della definizione o quando vengono modificati chiamando un metodo, ad esempio `Where` o `OrderBy`. Al contrario, viene rinviato l'esecuzione di query, il che significa che la valutazione di un'espressione viene ritardata finché non viene effettivamente un'iterazione il relativo valore realizzato o la [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) viene chiamato il metodo. Nel `SearchIndex` esempio, la query viene eseguita nella visualizzazione SearchIndex. Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).

A questo punto è possibile implementare il `SearchIndex` vista in cui verrà visualizzato il form per l'utente. Pulsante destro del mouse all'interno di `SearchIndex` metodo e quindi fare clic su **Aggiungi visualizzazione**. Nel **Aggiungi visualizzazione** finestra di dialogo, specificare che si intende passare un `Movie` oggetto per il modello di vista della relativa classe di modello. Nel **modelli di scaffolding** casella di riepilogo **elenco**, quindi fare clic su **Aggiungi**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Quando si fa clic il **Add** pulsante, il *Views\Movies\SearchIndex.cshtml* viene creato il modello di visualizzazione. Perché è stato selezionato **elenco** nel **modelli di scaffolding** elencare, Visual Studio generato automaticamente (sottoposto a scaffolding) alcuni tag predefinito nella visualizzazione. Lo scaffolding creato un form HTML. Viene esaminata la `Movie` classe e il codice creato per eseguire il rendering `<label>` elementi per ogni proprietà della classe. Il codice seguente mostra la visualizzazione di creazione che è stata generata:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Eseguire l'applicazione e passare a */Movies/SearchIndex*. Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL. Vengono visualizzati i film filtrati.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Se si modifica la firma del `SearchIndex` metodo di avere un parametro denominato `id`, il `id` corrisponderà a parametro il `{id}` segnaposto per il valore predefinito consente di indirizzare set nel *Global. asax* file.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

Originale `SearchIndex` metodo si presenta come segue:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Modificato `SearchIndex` metodo sarà simile alla seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film. Quindi, ora si aggiungerai dell'interfaccia utente per consentire loro filtrare i film. Se è stata modificata la firma del `SearchIndex` metodo da testare come passare il parametro ID associato alla route, modificarlo in modo che i `SearchIndex` metodo accetta un parametro di stringa denominato `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Aprire il *Views\Movies\SearchIndex.cshtml* del file e subito dopo `@Html.ActionLink("Create New", "Create")`, aggiungere quanto segue:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

Nell'esempio seguente illustra una parte del *Views\Movies\SearchIndex.cshtml* file con il markup filtro aggiunto.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

Il `Html.BeginForm` helper crea un'apertura `<form>` tag. Il `Html.BeginForm` helper, il form inviare a se stessa quando l'utente invia il form scegliendo il **filtro** pulsante.

Eseguire l'applicazione e provare a cercare un film.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

È presente alcun `HttpPost` eseguire l'overload del `SearchIndex` (metodo). Non è necessario, poiché il metodo non modifica lo stato dell'applicazione, ma filtra solo i dati.

È possibile aggiungere il metodo `HttpPost SearchIndex` seguente. In tal caso, l'invoker dell'azione corrisponderebbe la `HttpPost SearchIndex` metodo e il `HttpPost SearchIndex` metodo viene eseguito come illustrato nell'immagine seguente.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Tuttavia, anche se si aggiunge questa versione `HttpPost` del metodo `SearchIndex`, esiste una limitazione sul modo sulla relativa implementazione. Si supponga che si desideri usare come segnalibro una ricerca specifica o inviare un collegamento agli amici su cui possono fare clic per visualizzare lo stesso elenco filtrato di film. Si noti che l'URL per la richiesta HTTP POST è lo stesso URL per la richiesta GET (localhost: xxxxx/Movies/SearchIndex): nessuna informazione di ricerca nell'URL stesso. Diritto a questo punto, le informazioni sulla stringa di ricerca viene inviati al server come un valore del campo modulo. Ciò significa che non è possibile acquisire le informazioni di ricerca per aggiungere un segnalibro o inviare agli amici in un URL.

La soluzione consiste nell'usare un overload del `BeginForm` che specifica che la richiesta POST deve aggiungere le informazioni sulla ricerca per l'URL e che devono essere indirizzato alla versione HttpGet del `SearchIndex` (metodo). Sostituire le funzioni senza parametri `BeginForm` metodo con il codice seguente:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

A questo punto quando si invia una ricerca, l'URL contiene una stringa di query di ricerca. La ricerca passerà anche al metodo di azione `HttpGet SearchIndex`, anche se si dispone di un metodo `HttpPost SearchIndex`.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Aggiunta di ricerca in base al genere

Se è stato aggiunto il `HttpPost` versione del `SearchIndex` metodo, eliminarlo subito.

Successivamente, si aggiungerà una funzionalità per consentire agli utenti di eseguire la ricerca di film in base al genere. Sostituire il metodo `SearchIndex` con il codice seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Questa versione del `SearchIndex` metodo accetta un parametro aggiuntivo, vale a dire `movieGenre`. Creano le prime righe di codice un `List` oggetto per contenere cinematografici dal database.

Il codice seguente è una query LINQ che recupera tutti i generi dal database.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Il codice Usa il `AddRange` metodo del tipo generico `List` raccolta a cui aggiungere tutti i generi distinti all'elenco. (Senza il `Distinct` modificatore, verrebbero aggiunti generi duplicati, ad esempio, commedie verrebbero aggiunto due volte nel nostro esempio). Il codice quindi archivia l'elenco dei generi nel `ViewBag` oggetto.

Il codice seguente viene illustrato come controllare il `movieGenre` parametro. Se non è vuota, il codice ulteriormente vincola la query dei film per limitare il film selezionato per il genere specificato.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Aggiunta di Markup alla visualizzazione SearchIndex per supportare la ricerca in base al genere

Aggiungere un `Html.DropDownList` helper per il *Views\Movies\SearchIndex.cshtml* nel file, subito prima di `TextBox` helper. Di seguito è riportato il markup completato:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Eseguire l'applicazione e passare a */Movies/SearchIndex*. Eseguire una ricerca in base al genere dal nome del film e da entrambi i criteri.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

In questa sezione sono stati esaminati i metodi di azione CRUD e visualizzazioni generate dal framework. È stato creato un metodo di azione di ricerca e visualizzazione che consentono agli utenti di ricerca in base al titolo del film e genere. Nella sezione successiva, vedrà come aggiungere una proprietà per il `Movie` modello e come aggiungere un inizializzatore che creerà automaticamente un database di test.

> [!div class="step-by-step"]
> [Precedente](accessing-your-models-data-from-a-controller.md)
> [Successivo](adding-a-new-field-to-the-movie-model-and-table.md)
