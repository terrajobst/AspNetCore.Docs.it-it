---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Esaminare i metodi di modifica e visualizzazione di modifica | Documenti Microsoft
author: Rick-Anderson
description: "Nota: Una versione aggiornata di questa esercitazione è disponibile qui che utilizza ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice seguire e demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 315914056c0a666fdf23cf82a314a999e03114b6
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Esaminare i metodi di modifica e visualizzazione di modifica
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013. È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.


In questa sezione, si esamina i metodi di azione generato e viste per il controller di film. Quindi aggiungere una pagina di ricerca personalizzato.

Eseguire l'applicazione e individuare il `Movies` controller aggiungendo */Movies* per l'URL nella barra degli indirizzi del browser. Posiziona il puntatore del mouse su un **modifica** collegamento per visualizzare l'URL a essa collegate.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Il **modifica** collegamento è stato generato dal `Html.ActionLink` metodo il *Views\Movies\Index.cshtml* Vista:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

Il `Html` oggetto è un helper che viene esposta utilizzando una proprietà sul [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) classe di base. Il `ActionLink` metodo dell'helper semplifica la generazione dinamica di collegamenti ipertestuali HTML che si collegano a metodi di azione nel controller. Il primo argomento per il `ActionLink` metodo è il testo del collegamento per eseguire il rendering (ad esempio, `<a>Edit Me</a>`). Il secondo argomento è il nome del metodo di azione da richiamare. L'argomento finale è un [oggetto anonimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) che genera i dati della route (in questo caso, l'ID di 4).

Il collegamento generato illustrato nella figura precedente è `http://localhost:xxxxx/Movies/Edit/4`. La route predefinita (stabilita *App\_Start\RouteConfig.cs*) utilizza il modello di URL `{controller}/{action}/{id}`. Pertanto, ASP.NET traduce `http://localhost:xxxxx/Movies/Edit/4` in una richiesta per il `Edit` metodo di azione del `Movies` controller con il parametro `ID` uguale a 4. Esaminare il codice seguente dal *App\_Start\RouteConfig.cs* file.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

È inoltre possibile passare parametri di metodo di azione utilizzando una stringa di query. Ad esempio, l'URL `http://localhost:xxxxx/Movies/Edit?ID=4` passa anche il parametro `ID` 4 al `Edit` il metodo di azione del `Movies` controller.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Aprire il `Movies` controller. I due `Edit` metodi di azione vengono mostrati di seguito.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Si noti che il secondo metodo di azione `Edit` è preceduto dall'attributo `HttpPost`. Questo attributo specifica che eseguono l'overload di `Edit` metodo può essere richiamato solo per le richieste POST. È possibile applicare il `HttpGet` attributo per il primo metodo di modifica, ma che non è necessario perché è il valore predefinito. (Si farà riferimento ai metodi di azione che vengono assegnati in modo implicito il `HttpGet` attributo `HttpGet` metodi.)

Il `HttpGet` `Edit` metodo accetta il parametro ID film, Cerca il film tramite Entity Framework `Find` (metodo) e restituisce il filmato selezionato per la visualizzazione di modifica. Specifica il parametro ID un [il valore predefinito](https://msdn.microsoft.com/library/dd264739.aspx) di zero se la `Edit` metodo viene chiamato senza un parametro. Se non viene trovato un filmato, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) viene restituito. Quando il sistema di scaffolding ha creato la vista Edit, ha esaminato la classe `Movie` e il codice creato per eseguire il rendering degli elementi `<label>` e `<input>` per ogni proprietà della classe. Nell'esempio seguente viene illustrata la visualizzazione di modifica che è stata generata:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Si noti come il modello di visualizzazione è un `@model MvcMovie.Models.Movie` istruzione all'inizio del file: Specifica che la vista prevede che il modello per il modello di visualizzazione di tipo `Movie`.

Il codice di scaffolding vengono utilizzati diversi *metodi helper* per semplificare il markup HTML. Il [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) helper Visualizza il nome del campo (&quot;titolo&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, o &quot;prezzo &quot;). Il [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper esegue il rendering HTML `<input>` elemento. Il [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper Visualizza gli eventuali messaggi di convalida associati alla proprietà.

Eseguire l'applicazione e passare il */Movies* URL. Fare clic su un collegamento **Edit** (Modifica). Nel browser visualizzare l'origine per la pagina. Il codice HTML dell'elemento di formato è illustrato di seguito.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

Il `<input>` nell'HTML sono elementi `<form>` elemento il cui `action` attributo è impostato su post per il *filmati/modifica* URL. I dati verranno inviati al server quando il **modifica** si fa clic sul pulsante.

## <a name="processing-the-post-request"></a>Elaborazione della richiesta POST

Nell'elenco seguente viene indicata la versione `HttpPost` del metodo di azione `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Il [Raccoglitore di modelli di MVC ASP.NET](https://msdn.microsoft.com/magazine/hh781022.aspx) accetta i valori del form inseriti e crea un `Movie` oggetto passato come il `movie` parametro. Il metodo `ModelState.IsValid` verifica che i dati inviati nel formato possano essere usati per cambiare (modificare o aggiornare) un oggetto `Movie`. Se i dati non validi, i dati del film viene salvati il `Movies` insieme il `db(MovieDBContext` istanza). I nuovi dati film viene salvati nel database chiamando il `SaveChanges` metodo `MovieDBContext`. Dopo aver salvato i dati, il codice reindirizza l'utente per il `Index` metodo di azione della `MoviesController` classe, che consente di visualizzare della raccolta di film, incluse le modifiche appena apportate.

Se i valori registrati non sono validi, essi verranno nuovamente visualizzati nel form. Il `Html.ValidationMessageFor` helper nel *Edit.cshtml* vista modello si occuperà di visualizzazione dei messaggi di errore appropriato.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> per supportare la convalida jQuery per inglesi che utilizzano una virgola (&quot;,&quot;) per un punto decimale, è necessario includere *globalize.js* specifici e *cultures/globalize.cultures.js* file (da [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript per utilizzare `Globalize.parseFloat`. Il codice seguente illustra le modifiche al file Views\Movies\Edit.cshtml per lavorare con i &quot;fr-FR&quot; delle impostazioni cultura:


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Il campo decimale può richiedere una virgola, non un separatore decimale. Risolvere il problema temporaneo, è possibile aggiungere l'elemento di globalizzazione per il file Web. config radice di progetti. Il codice seguente viene illustrato l'elemento di globalizzazione con le impostazioni cultura in inglese Stati Uniti.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Tutti i `HttpGet` metodi seguono un modello simile. Ricevono un oggetto filmato (o un elenco di oggetti, nel caso di `Index`) e passare alla visualizzazione del modello. Il `Create` metodo passa un oggetto film vuoto per la visualizzazione di creazione. Tutti i metodi che creano, modificano, eliminano o cambiano in altro modo i dati, eseguono questa operazione nell'overload `HttpPost` del metodo. Modifica dei dati in un metodo GET HTTP è un rischio per la sicurezza, come descritto nella voce del post di blog [ASP.NET MVC suggerimento #46 – non utilizzare eliminare collegamenti poiché creano problemi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modifica dei dati in un metodo GET viene violato anche le procedure consigliate HTTP e l'architettura [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) di schema, che consente di specificare che le richieste GET non dovrebbero modificare lo stato dell'applicazione. In altre parole, l'esecuzione di un'operazione GET deve essere sicura, senza effetti collaterali e non modificare dati persistenti.

## <a name="adding-a-search-method-and-search-view"></a>Aggiunta di un metodo di ricerca e visualizzazione di ricerca

In questa sezione si aggiungerà un `SearchIndex` metodo di azione che consente di eseguire la ricerca filmati genre o nome. Questo sarà disponibile tramite il *filmati/SearchIndex* URL. La richiesta verrà visualizzato un form HTML che contiene gli elementi di input che un utente può immettere per cercare un filmato. Quando un utente invia il form, il metodo di azione otterrà i valori di ricerca registrati dall'utente e utilizzare i valori per eseguire ricerche nel database.

## <a name="displaying-the-searchindex-form"></a>Visualizzazione di Form SearchIndex

Per iniziare, aggiungere un `SearchIndex` il metodo di azione esistente `MoviesController` classe. Il metodo restituisce una visualizzazione contenente un form HTML. Ecco il codice:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

La prima riga del `SearchIndex` metodo vengono creati i seguenti [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query per selezionare i film:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

La query viene definita a questo punto, ma non è ancora stata eseguita sull'archivio dati.

Se il `searchString` parametro contiene una stringa, viene modificata la query di filmati per filtrare il valore della stringa di ricerca, tramite il codice seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

Il codice `s => s.Title` precedente è un'[espressione lambda](https://msdn.microsoft.com/library/bb397687.aspx). Le espressioni lambda vengono utilizzate in base al metodo [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) esegue una query come argomenti dei metodi degli operatori query standard, ad esempio il [dove](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) utilizzato nel codice precedente. Le query LINQ non vengono eseguite quando vengono definite o quando vengono modificati chiamando un metodo, ad esempio `Where` o `OrderBy`. In alternativa, esecuzione della query è rinviata, il che significa che la valutazione di un'espressione viene ritardata fino a quando il relativo valore realizzato viene effettivamente eseguita un'iterazione o [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metodo viene chiamato. Nel `SearchIndex` esempio, viene eseguita la query nella vista SearchIndex. Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).

Ora è possibile implementare il `SearchIndex` visualizzazione che consente di visualizzare il form per l'utente. Fare doppio clic all'interno di `SearchIndex` (metodo) e quindi fare clic su **Aggiungi visualizzazione**. Nel **Aggiungi visualizzazione** finestra di dialogo, specificare che si desidera passare un `Movie` oggetto per il modello di visualizzazione della relativa classe di modello. Nel **modello di scaffolding** scegliere **elenco**, quindi fare clic su **Aggiungi**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Quando si fa clic il **Aggiungi** pulsante, il *Views\Movies\SearchIndex.cshtml* viene creato il modello di visualizzazione. Perché è stato selezionato **elenco** nel **modello di scaffolding** elencare, Visual Studio generati automaticamente (scaffolding) del markup predefinito nella visualizzazione. Lo scaffolding creato un form HTML. Viene esaminato il `Movie` classe e codice creato per il rendering `<label>` gli elementi per ogni proprietà della classe. L'elenco di seguito viene illustrata la visualizzazione di creazione che è stata generata:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Eseguire l'applicazione e passare a *filmati/SearchIndex*. Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL. Vengono visualizzati i film filtrati.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Se si modifica la firma del `SearchIndex` metodo contiene un parametro denominato `id`, `id` parametro corrisponderà il `{id}` indirizza i segnaposto per il valore predefinito impostato nel *Global. asax* file.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

Originale `SearchIndex` metodo è simile al seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Modificato `SearchIndex` metodo si presenterebbe come segue:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film. Quindi, ora è aggiungerai dell'interfaccia utente per consentire loro filtrare film. Se è stata modificata la firma del `SearchIndex` metodo per verificare come passare il parametro ID di associazione di route, modificarlo in modo che il `SearchIndex` metodo accetta un parametro di stringa denominato `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Aprire il *Views\Movies\SearchIndex.cshtml* file e subito dopo `@Html.ActionLink("Create New", "Create")`, aggiungere quanto segue:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

Nell'esempio seguente viene mostrata una parte di *Views\Movies\SearchIndex.cshtml* file con il markup filtro aggiunto.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

Il `Html.BeginForm` helper crea un'apertura `<form>` tag. Il `Html.BeginForm` helper, il form inviare a se stessa quando l'utente invia il form scegliendo il **filtro** pulsante.

Eseguire l'applicazione e provare a cercare un filmato.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

È presente alcun `HttpPost` overload di `SearchIndex` metodo. Non è necessaria, poiché il metodo non modifica lo stato dell'applicazione, solo il filtraggio dei dati.

È possibile aggiungere il metodo `HttpPost SearchIndex` seguente. In tal caso, corrisponderà a invoker dell'azione di `HttpPost SearchIndex` (metodo) e `HttpPost SearchIndex` metodo verrebbe eseguito come illustrato nell'immagine seguente.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Tuttavia, anche se si aggiunge questa versione `HttpPost` del metodo `SearchIndex`, esiste una limitazione sul modo sulla relativa implementazione. Si supponga che si desideri usare come segnalibro una ricerca specifica o inviare un collegamento agli amici su cui possono fare clic per visualizzare lo stesso elenco filtrato di film. Si noti che l'URL per la richiesta HTTP POST è lo stesso come URL per la richiesta di recupero (localhost:xxxxx filmati/SearchIndex) - Nessuna informazione di ricerca nell'URL stesso. Diritto a questo punto, le informazioni sulla stringa di ricerca viene inviati al server come un valore del campo modulo. Ciò significa che non è possibile acquisire le informazioni di ricerca per aggiungere un segnalibro o inviare agli amici in un URL.

La soluzione consiste nell'usare un overload di `BeginForm` che specifica che la richiesta POST è necessario aggiungere le informazioni di ricerca per l'URL e che deve essere indirizzato alla versione di HttpGet il `SearchIndex` metodo. Sostituire le funzioni senza parametri `BeginForm` metodo con le operazioni seguenti:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

A questo punto quando si invia una ricerca, l'URL contiene una stringa di query di ricerca. La ricerca passerà anche al metodo di azione `HttpGet SearchIndex`, anche se si dispone di un metodo `HttpPost SearchIndex`.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Aggiunta di ricerca per genere

Se è stato aggiunto il `HttpPost` versione il `SearchIndex` metodo elimina ora.

Successivamente, aggiungere una funzionalità per consentire agli utenti di cercare film per genere. Sostituire il metodo `SearchIndex` con il codice seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Questa versione di `SearchIndex` metodo accetta un parametro aggiuntivo, vale a dire `movieGenre`. Creano le prime righe di codice un `List` oggetto che contenga generi film dal database.

Il codice seguente è una query LINQ che recupera tutti i generi dal database.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Il codice Usa il `AddRange` metodo del tipo generico `List` raccolta da aggiungere all'elenco di tutti i generi distinti. (Senza il `Distinct` modificatore, generi duplicati verrebbero aggiunto, ad esempio, verrebbero aggiunto comici due volte nel nostro esempio). Il codice quindi archivia l'elenco di generi nel `ViewBag` oggetto.

Il codice seguente viene illustrato come controllare il `movieGenre` parametro. Se non è vuota, ulteriormente il codice vincola la query di filmati per limitare i film selezionati per il genere specificato.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Aggiungere tag alla visualizzazione SearchIndex per supportare la ricerca per genere

Aggiungere un `Html.DropDownList` helper per il *Views\Movies\SearchIndex.cshtml* file, appena prima di `TextBox` helper. Di seguito è riportato il markup completato:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Eseguire l'applicazione e passare a *filmati/SearchIndex*. Eseguire una ricerca per genere, in base al nome di film e da entrambi i criteri.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

In questa sezione sono stati esaminati i metodi di azione CRUD e visualizzazioni generate dal framework. È stato creato un metodo di azione di ricerca e visualizzazione che consentono agli utenti di ricerca in base al genere e titolo del film. Nella sezione successiva verrà esaminato come aggiungere una proprietà per il `Movie` modello e come aggiungere un inizializzatore che verrà creato automaticamente un database di test.

>[!div class="step-by-step"]
[Precedente](accessing-your-models-data-from-a-controller.md)
[Successivo](adding-a-new-field-to-the-movie-model-and-table.md)
