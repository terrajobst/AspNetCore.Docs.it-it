---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Esaminare i metodi di modifica e visualizzazione di modifica | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: a3baa8e9af572d4c21813218ba394715a6db65cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Esaminare i metodi di modifica e visualizzazione di modifica
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In questa sezione si esamineranno generato `Edit` metodi di azione e le visualizzazioni per il controller di film. Ma prima diventerà la deviazione breve per rendere la data di rilascio di un aspetto migliore. Aprire il *Models\Movie.cs* file e aggiungere le righe evidenziate illustrate di seguito:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

È anche possibile impostare le impostazioni cultura data specifico simile al seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Gli attributi [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) verranno esaminati nell'esercitazione successiva. L'attributo [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) specifica il testo da visualizzare per il nome di un campo, in questo caso "Release Date" anziché "ReleaseDate". Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo specifica il tipo di dati, in questo caso è una data, in modo che le informazioni sull'ora archiviati nel campo non è visualizzati. Il [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo è necessario per un bug nel browser Chrome che esegue il rendering dei formati di data in modo non corretto.

Eseguire l'applicazione e individuare il `Movies` controller. Posiziona il puntatore del mouse su un **modifica** collegamento per visualizzare l'URL a essa collegate.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Il **modifica** collegamento è stato generato dal `Html.ActionLink` metodo il *Views\Movies\Index.cshtml* Vista:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

Il `Html` oggetto è un helper che viene esposta utilizzando una proprietà sul [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) classe di base. Il `ActionLink` metodo dell'helper semplifica la generazione dinamica di collegamenti ipertestuali HTML che si collegano a metodi di azione nel controller. Il primo argomento per il `ActionLink` metodo è il testo del collegamento per eseguire il rendering (ad esempio, `<a>Edit Me</a>`). Il secondo argomento è il nome del metodo di azione da richiamare (In questo caso, il `Edit` azione). L'argomento finale è un [oggetto anonimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) che genera i dati della route (in questo caso, l'ID di 4).

Il collegamento generato illustrato nella figura precedente è `http://localhost:1234/Movies/Edit/4`. La route predefinita (stabilita *App\_Start\RouteConfig.cs*) utilizza il modello di URL `{controller}/{action}/{id}`. Pertanto, ASP.NET traduce `http://localhost:1234/Movies/Edit/4` in una richiesta per il `Edit` metodo di azione del `Movies` controller con il parametro `ID` uguale a 4. Esaminare il codice seguente dal *App\_Start\RouteConfig.cs* file. Il [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metodo viene utilizzato per indirizzare le richieste HTTP per il metodo di azione e del controller corretto e fornire il parametro ID facoltativo. Il [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metodo viene utilizzato anche il [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) , ad esempio `ActionLink` per generare URL dato il controller, il metodo di azione e i dati di route.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

È inoltre possibile passare parametri di metodo di azione utilizzando una stringa di query. Ad esempio, l'URL `http://localhost:1234/Movies/Edit?ID=3` passa anche il parametro `ID` pari a 3 per il `Edit` il metodo di azione del `Movies` controller.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Aprire il `Movies` controller. I due `Edit` metodi di azione vengono mostrati di seguito.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Si noti che il secondo metodo di azione `Edit` è preceduto dall'attributo `HttpPost`. Questo attributo specifica che il metodo di overload di `Edit` metodo può essere richiamato solo per le richieste POST. È possibile applicare il `HttpGet` attributo per il primo metodo di modifica, ma che non è necessario perché è il valore predefinito. (Si farà riferimento ai metodi di azione che vengono assegnati in modo implicito il `HttpGet` attributo `HttpGet` metodi.) Il [associare](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attributo è un altro meccanismo di sicurezza importanti che consente di mantenere lontani pirati informatici dalla registrazione eccessiva di dati al modello. È necessario includere solo le proprietà dell'attributo di associazione che si desidera modificare. Per ulteriori informazioni overposting e l'attributo di associazione in my [overposting Nota sulla sicurezza](https://go.microsoft.com/fwlink/?LinkId=317598). Nel modello semplice utilizzato in questa esercitazione, si verranno associazione tutti i dati nel modello. Il [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) attributo viene utilizzato per impedire richieste false ed è accoppiato con `@Html.AntiForgeryToken()` nel file di visualizzazione di modifica (*Views\Movies\Edit.cshtml*), una parte è illustrata di seguito:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` Genera un token antifalsificazione nascosto del modulo che deve corrispondere il `Edit` metodo il `Movies` controller. Altre informazioni su Cross-site request forgery (noto anche come XSRF o CSRF) in my esercitazione [XSRF/CSRF prevenzione in MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

Il `HttpGet` `Edit` metodo accetta il parametro ID film, Cerca il film tramite Entity Framework `Find` (metodo) e restituisce il filmato selezionato per la visualizzazione di modifica. Se non viene trovato un filmato, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) viene restituito. Quando il sistema di scaffolding ha creato la vista Edit, ha esaminato la classe `Movie` e il codice creato per eseguire il rendering degli elementi `<label>` e `<input>` per ogni proprietà della classe. Nell'esempio seguente viene illustrata la visualizzazione di modifica è stata generata con il sistema lo scaffolding di visual studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Si noti come il modello di visualizzazione è un `@model MvcMovie.Models.Movie` istruzione all'inizio del file: Specifica che la vista prevede che il modello per il modello di visualizzazione di tipo `Movie`.

Il codice di scaffolding vengono utilizzati diversi *metodi helper* per semplificare il markup HTML. Il [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) helper Visualizza il nome del campo (&quot;titolo&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, o &quot;prezzo &quot;). Il [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper esegue il rendering HTML `<input>` elemento. Il [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper Visualizza gli eventuali messaggi di convalida associati alla proprietà.

Eseguire l'applicazione e passare il */Movies* URL. Fare clic su un collegamento **Edit** (Modifica). Nel browser visualizzare l'origine per la pagina. Il codice HTML dell'elemento di formato è illustrato di seguito.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

Il `<input>` nell'HTML sono elementi `<form>` elemento il cui `action` attributo è impostato su post per il *filmati/modifica* URL. I dati verranno inviati al server quando il **salvare** si fa clic sul pulsante. La seconda riga viene nascosto [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) generati dal token di `@Html.AntiForgeryToken()` chiamare.

## <a name="processing-the-post-request"></a>Elaborazione della richiesta POST

Nell'elenco seguente viene indicata la versione `HttpPost` del metodo di azione `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Il [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) attributo convalida il [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generato dal `@Html.AntiForgeryToken()` chiamare nella vista.

Il [Raccoglitore di modelli di MVC ASP.NET](https://msdn.microsoft.com/library/dd410405.aspx) accetta i valori del form inseriti e crea un `Movie` oggetto passato come il `movie` parametro. Il metodo `ModelState.IsValid` verifica che i dati inviati nel formato possano essere usati per cambiare (modificare o aggiornare) un oggetto `Movie`. Se i dati non validi, i dati del film viene salvati il `Movies` insieme il `db(MovieDBContext` istanza). I nuovi dati film viene salvati nel database chiamando il `SaveChanges` metodo `MovieDBContext`. Dopo avere salvato i dati, il codice reindirizza l'utente al metodo di azione `Index` della classe `MoviesController`, che visualizza la raccolta di film, incluse le modifiche appena apportate.

Non appena la convalida lato client determina che i valori di un campo non vengono, viene visualizzato un messaggio di errore. Se si disabilita JavaScript, si avrà la convalida lato client, ma il server rileva i valori registrati non sono validi e verranno è possibile visualizzare nuovamente i valori del form con messaggi di errore. Più avanti in questa esercitazione verranno esaminati convalida in modo più dettagliato.

Il `Html.ValidationMessageFor` helper nel *Edit.cshtml* vista modello si occuperà di visualizzazione dei messaggi di errore appropriato.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Tutti i `HttpGet` metodi seguono un modello simile. Ricevono un oggetto filmato (o un elenco di oggetti, nel caso di `Index`) e passare alla visualizzazione del modello. Il `Create` metodo passa un oggetto film vuoto per la visualizzazione di creazione. Tutti i metodi che creano, modificano, eliminano o cambiano in altro modo i dati, eseguono questa operazione nell'overload `HttpPost` del metodo. Modifica dei dati in un metodo GET HTTP è un rischio per la sicurezza, come descritto nella voce del post di blog [ASP.NET MVC suggerimento #46 – non utilizzare eliminare collegamenti poiché creano problemi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modifica dei dati in un metodo GET viene violato anche le procedure consigliate HTTP e l'architettura [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) di schema, che consente di specificare che le richieste GET non dovrebbero modificare lo stato dell'applicazione. In altre parole, l'esecuzione di un'operazione GET deve essere sicura, senza effetti collaterali e non modificare dati persistenti.

Se si utilizza un computer in lingua inglese Stati Uniti, è possibile ignorare questa sezione e passare all'esercitazione successiva. È possibile scaricare la versione Globalize di questa esercitazione [qui](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Per un'ottima esercitazione di internazionalizzazione due parti, vedere [ASP.NET MVC 5 internazionalizzazione di Nadeem](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> per supportare la convalida jQuery per inglesi che utilizzano una virgola (&quot;,&quot;) per un separatore decimale e formati di data non in lingua inglese Stati Uniti, è necessario includere *globalize.js* specifici e  *Cultures/globalize.Cultures.js* file (da [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) e JavaScript per utilizzare `Globalize.parseFloat`. È possibile ottenere la convalida non inglesi jQuery da NuGet. (Non installare Globalize se si utilizza una lingua inglese.)


1. Dal **strumenti** dal menu **NuGetLibrary Package Manager**, quindi fare clic su **Gestisci pacchetti NuGet per la soluzione**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. Nel riquadro sinistro, selezionare <strong>esplorare*.</strong>* (Vedere la figura riportata di seguito).
3. Nella casella di input, immettere * Globalize * *.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Scegliere `jQuery.Validation.Globalize`, scegliere `MvcMovie` e fare clic su **installare**. Il *Scripts\jquery.globalize\globalize.js* file verrà aggiunto al progetto. Il *Scripts\jquery.globalize\cultures\* cartella conterrà i file JavaScript di molte delle impostazioni cultura. Si noti che potrebbe richiedere cinque minuti per installare questo pacchetto.

   Il codice seguente illustra le modifiche al file Views\Movies\Edit.cshtml: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Per evitare la ripetizione di questo codice in ogni visualizzazione di modifica, è possibile spostarla nel file di layout. Per ottimizzare il download dello script, vedere l'esercitazione [Bundling and Minification](../../performance/bundling-and-minification.md).

Per ulteriori informazioni vedere [ASP.NET MVC 3 internazionalizzazione](http://afana.me/post/aspnet-mvc-internationalization.aspx) e [internazionalizzazione di ASP.NET MVC 3 - parte 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Risolvere il problema temporaneo, se non è possibile ottenere convalida utilizza le impostazioni locali, è possibile forzare il computer di utilizzare l'inglese o è possibile disabilitare JavaScript nel browser. Per forzare il computer di utilizzare l'inglese, è possibile aggiungere l'elemento di globalizzazione per la radice di progetti *Web. config* file. Il codice seguente viene illustrato l'elemento di globalizzazione con le impostazioni cultura in inglese Stati Uniti.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> Nella prossima esercitazione, verrà implementato la funzionalità di ricerca.

> [!div class="step-by-step"]
> [Precedente](accessing-your-models-data-from-a-controller.md)
> [Successivo](adding-search.md)
