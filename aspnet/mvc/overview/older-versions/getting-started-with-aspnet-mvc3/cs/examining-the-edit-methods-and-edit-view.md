---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
title: Esaminare i metodi di modifica e visualizzazione di modifica (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 1d266bf0-a61e-423b-a3d2-13773d7dafe2
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: f206d04e0c9db6149b1d01f7dcbb6be733754881
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835368"
---
<a name="examining-the-edit-methods-and-edit-view-c"></a>Esaminare i metodi di modifica e visualizzazione di modifica (c#)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.
> 
> 
> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare al [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.


In questa sezione si esamina i metodi di azione generata e visualizzazioni per il controller di film. Quindi si aggiungerà una pagina di ricerca personalizzato.

Eseguire l'applicazione e passare al `Movies` controller aggiungendo */Movies* all'URL nella barra degli indirizzi del browser. Sposta il puntatore del mouse su un **modifica** collegamento per visualizzare l'URL a essa collegate.

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image2.png)](examining-the-edit-methods-and-edit-view/_static/image1.png)

Il **Edit** collegamento è stato generato dal `Html.ActionLink` metodo nel *Views\Movies\Index.cshtml* visualizzazione:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image4.png)](examining-the-edit-methods-and-edit-view/_static/image3.png)

Il `Html` oggetto è un helper esposto mediante una proprietà nel `WebViewPage` classe di base. Il `ActionLink` metodo dell'helper consente di generare in modo dinamico i collegamenti ipertestuali HTML che si collegano ai metodi di azione nel controller. Il primo argomento per il `ActionLink` metodo è il testo del collegamento per eseguire il rendering (ad esempio, `<a>Edit Me</a>`). Il secondo argomento è il nome del metodo di azione da richiamare. L'argomento finale è un [oggetto anonimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) che genera i dati della route (in questo caso, l'ID di 4).

Il collegamento generato illustrato nell'immagine precedente è `http://localhost:xxxxx/Movies/Edit/4`. La route predefinita ha il modello di URL `{controller}/{action}/{id}`. Pertanto, ASP.NET si traduce `http://localhost:xxxxx/Movies/Edit/4` in una richiesta per il `Edit` metodo di azione del `Movies` controller con il parametro `ID` pari a 4.

È anche possibile passare parametri di metodo di azione usando una stringa di query. Ad esempio, l'URL `http://localhost:xxxxx/Movies/Edit?ID=4` passa inoltre il parametro `ID` pari a 4 per il `Edit` metodo di azione del `Movies` controller.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image6.png)](examining-the-edit-methods-and-edit-view/_static/image5.png)

Aprire il `Movies` controller. I due `Edit` i metodi di azione sono illustrati di seguito.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Si noti che il secondo metodo di azione `Edit` è preceduto dall'attributo `HttpPost`. Questo attributo specifica che eseguono l'overload del `Edit` metodo può essere richiamato solo per le richieste POST. È possibile applicare il `HttpGet` attributo al primo metodo di modifica, ma che non è necessario perché è il valore predefinito. (Si farà riferimento ai metodi di azione che vengono assegnati in modo implicito il `HttpGet` dell'attributo come `HttpGet` metodi.)

Il `HttpGet` `Edit` metodo accetta il parametro ID film, Cerca il film tramite Entity Framework `Find` (metodo) e restituisce il film selezionato nella visualizzazione di modifica. Quando il sistema di scaffolding ha creato la vista Edit, ha esaminato la classe `Movie` e il codice creato per eseguire il rendering degli elementi `<label>` e `<input>` per ogni proprietà della classe. Nell'esempio seguente mostra la visualizzazione di modifica che è stata generata:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

Si noti come il modello di vista ha un `@model MvcMovie.Models.Movie` istruzione all'inizio del file: Specifica che il modello per il modello di vista sia del tipo previsto dalla vista `Movie`.

Il codice con scaffolding Usa diversi *metodi helper* per semplificare il markup HTML. Il [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) helper Visualizza il nome del campo ("Title", "ReleaseDate", "Genre" o "Price"). Il [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper Visualizza HTML `<input>` elemento. Il [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper Visualizza gli eventuali messaggi di convalida associati alla proprietà.

Eseguire l'applicazione e passare al */Movies* URL. Fare clic su un collegamento **Edit** (Modifica). Nel browser visualizzare l'origine per la pagina. Il codice HTML nella pagina sarà simile all'esempio seguente. (Per maggiore chiarezza è stato escluso il markup di menu).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample4.html)]

Il `<input>` elementi si trovano in un elemento HTML `<form>` elemento la cui `action` attributo è impostato su post per il */Movies/Edit* URL. I dati del modulo verranno inviati al server quando il **modifica** si fa clic sul pulsante.

## <a name="processing-the-post-request"></a>Elaborazione della richiesta POST

Nell'elenco seguente viene indicata la versione `HttpPost` del metodo di azione `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs)]

Il raccoglitore di modelli ASP.NET framework accetta i valori di modulo e crea un `Movie` che viene passato come il `movie` parametro. Il `ModelState.IsValid` archivia il codice verifica che i dati inviati nel formato possono essere utilizzati per modificare un `Movie` oggetto. Se i dati sono validi, il codice consente di salvare i dati dei film per i `Movies` raccolta del `MovieDBContext` istanza. Il codice quindi Salva i nuovi dati del film nel database chiamando il `SaveChanges` metodo `MovieDBContext`, che mantiene le modifiche al database. Dopo aver salvato i dati, il codice reindirizza l'utente per il `Index` metodo di azione del `MoviesController` classe, che fa sì che il film aggiornato da visualizzare nell'elenco di film.

Se i valori inviati non sono validi, essi verranno nuovamente visualizzati nel formato. Il `Html.ValidationMessageFor` helper nel *Edit. cshtml* vista modello ci occupiamo della visualizzazione dei messaggi di errore appropriato.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image8.png)](examining-the-edit-methods-and-edit-view/_static/image7.png)

> **Nota sulle impostazioni internazionali** se si usano in genere le impostazioni locali diverse dall'inglese, vedere [che supportano ASP.NET MVC 3 convalida con le impostazioni locali diverse dall'inglese.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Rendere più affidabile il metodo di modifica

Il `HttpGet` `Edit` metodo generato dal sistema di scaffolding non verifica che l'ID che viene passato al metodo è valido. Se un utente rimuove il segmento ID dall'URL (`http://localhost:xxxxx/Movies/Edit`), viene visualizzato l'errore seguente:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image10.png)](examining-the-edit-methods-and-edit-view/_static/image9.png)

Un utente può anche passare un ID che non esiste nel database, ad esempio `http://localhost:xxxxx/Movies/Edit/1234`. È possibile apportare due modifiche per il `HttpGet` `Edit` metodo di azione per risolvere questa limitazione. In primo luogo, modificare il `ID` parametro in modo che un valore predefinito pari a zero quando non viene passato in modo esplicito un ID. È anche possibile verificare che il `Find` metodo effettivamente trovato un film prima di restituire l'oggetto film al modello di visualizzazione. Aggiornato `Edit` metodo è illustrato di seguito.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Se non viene trovato alcun film, il `HttpNotFound` viene chiamato il metodo.

Tutti i `HttpGet` metodi seguono un modello simile. Ricevono un oggetto film (o un elenco di oggetti, nel caso di `Index`) e passare il modello alla visualizzazione. Il `Create` metodo passa un oggetto vuoto per la visualizzazione di creazione. Tutti i metodi che creano, modificano, eliminano o cambiano in altro modo i dati, eseguono questa operazione nell'overload `HttpPost` del metodo. Modifica dei dati in un metodo HTTP GET è un rischio per la sicurezza, come descritto nella voce del post di blog [ASP.NET MVC suggerimento & 46: non usare i collegamenti di eliminazione poiché creano le falle](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modifica dei dati in un metodo GET viola anche le procedure consigliate HTTP e il modello architetturale REST, che specifica che le richieste GET non devono modificherà lo stato dell'applicazione. In altre parole, eseguendo un'operazione GET deve essere un'operazione sicura che non ha effetti collaterali.

## <a name="adding-a-search-method-and-search-view"></a>Aggiunta di un metodo di ricerca e visualizzazione di ricerca

In questa sezione si aggiungerà un `SearchIndex` metodo di azione che consente di ricercare i film per genere o un nome. Questo sarà disponibile tramite il */Movies/SearchIndex* URL. La richiesta verrà visualizzato un form HTML che contiene gli elementi di input che un utente può inserire per cercare un film. Quando un utente invia il form, il metodo di azione verrà ottenere i valori di ricerca registrati dall'utente e usare i valori per eseguire ricerche nel database.

## <a name="displaying-the-searchindex-form"></a>Visualizzazione di Form SearchIndex

Iniziare aggiungendo un `SearchIndex` metodo di azione esistente `MoviesController` classe. Il metodo restituisce una visualizzazione contenente un form HTML. Ecco il codice:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cs)]

La prima riga del `SearchIndex` metodo creati i seguenti elementi [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query per selezionare i film:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cs)]

La query viene definita in questo punto, ma non è ancora stata eseguita a fronte dell'archivio dati.

Se il `searchString` parametro contiene una stringa, la query dei film viene modificata per filtrare il valore della stringa di ricerca, usando il codice seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Le query LINQ non vengono eseguite al momento della definizione o quando vengono modificati chiamando un metodo, ad esempio `Where` o `OrderBy`. Al contrario, viene rinviato l'esecuzione di query, il che significa che la valutazione di un'espressione viene ritardata finché non viene effettivamente un'iterazione il relativo valore realizzato o la [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) viene chiamato il metodo. Nel `SearchIndex` esempio, la query viene eseguita nella visualizzazione SearchIndex. Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).

A questo punto è possibile implementare il `SearchIndex` vista in cui verrà visualizzato il form per l'utente. Pulsante destro del mouse all'interno di `SearchIndex` metodo e quindi fare clic su **Aggiungi visualizzazione**. Nel **Aggiungi visualizzazione** finestra di dialogo, specificare che si intende passare un `Movie` oggetto per il modello di vista della relativa classe di modello. Nel **modelli di scaffolding** casella di riepilogo **elenco**, quindi fare clic su **Aggiungi**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Quando si fa clic il **Add** pulsante, il *Views\Movies\SearchIndex.cshtml* viene creato il modello di visualizzazione. Perché è stato selezionato **elenco** nel **modello Scaffold** elencare, Visual Web Developer generata automaticamente (sottoposto a scaffolding) del contenuto predefinito nella visualizzazione. Lo scaffolding creato un form HTML. Viene esaminata la `Movie` classe e il codice creato per eseguire il rendering `<label>` elementi per ogni proprietà della classe. Il codice seguente mostra la visualizzazione di creazione che è stata generata:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Eseguire l'applicazione e passare a */Movies/SearchIndex*. Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL. Vengono visualizzati i film filtrati.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Se si modifica la firma del `SearchIndex` metodo di avere un parametro denominato `id`, il `id` corrisponderà a parametro il `{id}` segnaposto per il valore predefinito consente di indirizzare set nel *Global. asax* file.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Modificato `SearchIndex` metodo sarà simile alla seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cs)]

È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film. Quindi, ora si aggiungerai dell'interfaccia utente per consentire loro filtrare i film. Se è stata modificata la firma del `SearchIndex` metodo da testare come passare il parametro ID associato alla route, modificarlo in modo che i `SearchIndex` metodo accetta un parametro di stringa denominato `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample13.cs)]

Aprire il *Views\Movies\SearchIndex.cshtml* del file e subito dopo `@Html.ActionLink("Create New", "Create")`, aggiungere quanto segue:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cshtml)]

Nell'esempio seguente illustra una parte del *Views\Movies\SearchIndex.cshtml* file con il markup filtro aggiunto.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cshtml)]

Il `Html.BeginForm` helper crea un'apertura `<form>` tag. Il `Html.BeginForm` helper, il form inviare a se stessa quando l'utente invia il form scegliendo il **filtro** pulsante.

Eseguire l'applicazione e provare a cercare un film.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

È presente alcun `HttpPost` eseguire l'overload del `SearchIndex` (metodo). Non è necessario, poiché il metodo non modifica lo stato dell'applicazione, ma filtra solo i dati.

È possibile aggiungere il metodo `HttpPost SearchIndex` seguente. In tal caso, l'invoker dell'azione corrisponderebbe la `HttpPost SearchIndex` metodo e il `HttpPost SearchIndex` metodo viene eseguito come illustrato nell'immagine seguente.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

Tuttavia, anche se si aggiunge questa versione `HttpPost` del metodo `SearchIndex`, esiste una limitazione sul modo sulla relativa implementazione. Si supponga che si desideri usare come segnalibro una ricerca specifica o inviare un collegamento agli amici su cui possono fare clic per visualizzare lo stesso elenco filtrato di film. Si noti che l'URL per la richiesta HTTP POST è lo stesso URL per la richiesta GET (localhost: xxxxx/Movies/SearchIndex): nessuna informazione di ricerca nell'URL stesso. Diritto a questo punto, le informazioni sulla stringa di ricerca viene inviati al server come un valore del campo modulo. Ciò significa che non è possibile acquisire le informazioni di ricerca per aggiungere un segnalibro o inviare agli amici in un URL.

La soluzione consiste nell'usare un overload di `BeginForm` che specifica che la richiesta POST deve aggiungere le informazioni sulla ricerca per l'URL e che dovrebbe essere indirizzati alla versione HttpGet del `SearchIndex` (metodo). Sostituire le funzioni senza parametri `BeginForm` metodo con il codice seguente:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml)]

[![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image22.png)](examining-the-edit-methods-and-edit-view/_static/image21.png)

A questo punto quando si invia una ricerca, l'URL contiene una stringa di query di ricerca. La ricerca passerà anche al metodo di azione `HttpGet SearchIndex`, anche se si dispone di un metodo `HttpPost SearchIndex`.

[![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image24.png)](examining-the-edit-methods-and-edit-view/_static/image23.png)

## <a name="adding-search-by-genre"></a>Aggiunta di ricerca in base al genere

Se è stato aggiunto il `HttpPost` versione del `SearchIndex` metodo, eliminarlo subito.

Successivamente, si aggiungerà una funzionalità per consentire agli utenti di eseguire la ricerca di film in base al genere. Sostituire il metodo `SearchIndex` con il codice seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cs)]

Questa versione del `SearchIndex` metodo accetta un parametro aggiuntivo, vale a dire `movieGenre`. Creano le prime righe di codice un `List` oggetto per contenere cinematografici dal database.

Il codice seguente è una query LINQ che recupera tutti i generi dal database.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

Il codice Usa il `AddRange` metodo del tipo generico `List` raccolta a cui aggiungere tutti i generi distinti all'elenco. (Senza il `Distinct` modificatore, verrebbero aggiunti generi duplicati, ad esempio, commedie verrebbero aggiunto due volte nel nostro esempio). Il codice quindi archivia l'elenco dei generi nel `ViewBag` oggetto.

Il codice seguente viene illustrato come controllare il `movieGenre` parametro. Se non è vuota ulteriormente il codice vincola la query dei film per limitare il film selezionato per il genere specificato.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Aggiunta di Markup alla visualizzazione SearchIndex per supportare la ricerca in base al genere

Aggiungere un `Html.DropDownList` helper per il *Views\Movies\SearchIndex.cshtml* nel file, subito prima di `TextBox` helper. Di seguito è riportato il markup completato:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cshtml)]

Eseguire l'applicazione e passare a */Movies/SearchIndex*. Eseguire una ricerca in base al genere dal nome del film e da entrambi i criteri.

In questa sezione sono stati esaminati i metodi di azione CRUD e visualizzazioni generate dal framework. È stato creato un metodo di azione di ricerca e visualizzazione che consentono agli utenti di ricerca in base al titolo del film e genere. Nella sezione successiva, vedrà come aggiungere una proprietà per il `Movie` modello e come aggiungere un inizializzatore che creerà automaticamente un database di test.

> [!div class="step-by-step"]
> [Precedente](accessing-your-models-data-from-a-controller.md)
> [Successivo](adding-a-new-field.md)
