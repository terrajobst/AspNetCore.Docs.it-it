---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Esaminare i metodi di modifica e visualizzazione di modifica | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/22/2015
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: a166f6c4450c72adc23f7d36113ceba7e04f1929
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820889"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Esaminare i metodi di modifica e visualizzazione di modifica
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In questa sezione si esamineranno generato `Edit` metodi di azione e visualizzazioni per il controller di film. Ma prima di tutto richiederà la breve deviazione per rendere la data di rilascio di un aspetto migliore. Aprire il *Models\Movie.cs* file e aggiungere le righe evidenziate illustrate di seguito:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

È inoltre possibile rendere le impostazioni cultura date specifiche simile al seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Gli attributi [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) verranno esaminati nell'esercitazione successiva. L'attributo [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) specifica il testo da visualizzare per il nome di un campo, in questo caso "Release Date" anziché "ReleaseDate". Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo specifica il tipo di dati, in questo caso è una data, in modo che le informazioni sull'ora archiviate nel campo non viene visualizzate. Il [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo è necessario per un bug nel browser Chrome che esegue il rendering di formati di data in modo non corretto.

Eseguire l'applicazione e individuare il `Movies` controller. Sposta il puntatore del mouse su un **modifica** collegamento per visualizzare l'URL a essa collegate.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Il **Edit** collegamento è stato generato dal `Html.ActionLink` metodo nel *Views\Movies\Index.cshtml* visualizzazione:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![HTML. ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

Il `Html` oggetto è un helper esposto mediante una proprietà nel [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) classe di base. Il `ActionLink` metodo dell'helper consente di generare in modo dinamico i collegamenti ipertestuali HTML che si collegano ai metodi di azione nel controller. Il primo argomento per il `ActionLink` metodo è il testo del collegamento per eseguire il rendering (ad esempio, `<a>Edit Me</a>`). Il secondo argomento è il nome del metodo di azione da richiamare (In questo caso, il `Edit` azione). L'argomento finale è un [oggetto anonimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) che genera i dati della route (in questo caso, l'ID di 4).

Il collegamento generato illustrato nell'immagine precedente è `http://localhost:1234/Movies/Edit/4`. La route predefinita (stabilito nelle *App\_Start\RouteConfig.cs*) accetta il modello di URL `{controller}/{action}/{id}`. Pertanto, ASP.NET si traduce `http://localhost:1234/Movies/Edit/4` in una richiesta per il `Edit` metodo di azione del `Movies` controller con il parametro `ID` pari a 4. Esaminare il codice seguente il *App\_Start\RouteConfig.cs* file. Il [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metodo viene utilizzato per indirizzare le richieste HTTP per il metodo di azione e del controller corretto e specificare il parametro ID facoltativo. Il [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metodo viene usato anche per il [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) , ad esempio `ActionLink` per generare gli URL di base del controller, metodo di azione e i dati di route.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

È anche possibile passare parametri di metodo di azione usando una stringa di query. Ad esempio, l'URL `http://localhost:1234/Movies/Edit?ID=3` passa inoltre il parametro `ID` pari a 3 per il `Edit` metodo di azione del `Movies` controller.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Aprire il `Movies` controller. I due `Edit` i metodi di azione sono illustrati di seguito.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Si noti che il secondo metodo di azione `Edit` è preceduto dall'attributo `HttpPost`. Questo attributo specifica che l'overload dei metodi di `Edit` metodo può essere richiamato solo per le richieste POST. È possibile applicare il `HttpGet` attributo al primo metodo di modifica, ma che non è necessario perché è il valore predefinito. (Si farà riferimento ai metodi di azione che vengono assegnati in modo implicito il `HttpGet` dell'attributo come `HttpGet` metodi.) Il [associare](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attributo è un altro meccanismo di sicurezza importanti che impedisce l'overposting dati al modello di pirati informatici. È necessario includere solo le proprietà nell'attributo binding che si desidera modificare. Per ulteriori informazioni sull'overposting e l'attributo di associazione nella mio [overposting Nota sulla sicurezza](https://go.microsoft.com/fwlink/?LinkId=317598). Nel modello con registrazione minima usato in questa esercitazione, eseguiremo l'associazione tutti i dati nel modello. Il [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) attributo viene utilizzato per impedire la falsificazione di una richiesta ed è accoppiato con `@Html.AntiForgeryToken()` nel file di visualizzazione di modifica (*Views\Movies\Edit.cshtml*), di seguito è riportata una parte:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` Genera un token antifalsificazione form nascosto che deve corrispondere per la `Edit` metodo del `Movies` controller. Altre informazioni sulla Cross-site request forgery (noto anche come XSRF o CSRF) nella mio esercitazione [prevenzione di XSRF/CSRF in MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

Il `HttpGet` `Edit` metodo accetta il parametro ID film, Cerca il film tramite Entity Framework `Find` (metodo) e restituisce il film selezionato nella visualizzazione di modifica. Se non viene trovato un film, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) viene restituito. Quando il sistema di scaffolding ha creato la vista Edit, ha esaminato la classe `Movie` e il codice creato per eseguire il rendering degli elementi `<label>` e `<input>` per ogni proprietà della classe. Nell'esempio seguente mostra la visualizzazione di modifica che è stata generata dal sistema di scaffolding di visual studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Si noti come il modello di vista ha un `@model MvcMovie.Models.Movie` istruzione all'inizio del file: Specifica che il modello per il modello di vista sia del tipo previsto dalla vista `Movie`.

Il codice con scaffolding Usa diversi *metodi helper* per semplificare il markup HTML. Il [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) helper Visualizza il nome del campo (&quot;Title&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, o &quot;prezzo &quot;). Il [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper esegue il rendering HTML `<input>` elemento. Il [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper Visualizza gli eventuali messaggi di convalida associati alla proprietà.

Eseguire l'applicazione e passare al */Movies* URL. Fare clic su un collegamento **Edit** (Modifica). Nel browser visualizzare l'origine per la pagina. Seguito è riportato il codice HTML per l'elemento del form.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

Il `<input>` elementi si trovano in un elemento HTML `<form>` elemento la cui `action` attributo è impostato su post per il */Movies/Edit* URL. I dati del modulo verranno inviati al server quando il **salvare** si fa clic sul pulsante. La seconda riga Mostra nascosti [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generati dal `@Html.AntiForgeryToken()` chiamare.

## <a name="processing-the-post-request"></a>Elaborazione della richiesta POST

Nell'elenco seguente viene indicata la versione `HttpPost` del metodo di azione `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Il [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) attributo convalida il [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generati dal `@Html.AntiForgeryToken()` chiamare nella visualizzazione.

Il [Raccoglitore di modelli ASP.NET MVC](https://msdn.microsoft.com/library/dd410405.aspx) accetta i valori di modulo e consente di creare un `Movie` che viene passato come il `movie` parametro. Il metodo `ModelState.IsValid` verifica che i dati inviati nel formato possano essere usati per cambiare (modificare o aggiornare) un oggetto `Movie`. Se i dati sono validi, i dati dei film viene salvati per il `Movies` raccolta del `db(MovieDBContext` istanza). I nuovi dati del film viene salvati nel database chiamando il `SaveChanges` metodo `MovieDBContext`. Dopo avere salvato i dati, il codice reindirizza l'utente al metodo di azione `Index` della classe `MoviesController`, che visualizza la raccolta di film, incluse le modifiche appena apportate.

Non appena la convalida lato client determina che i valori di un campo non vengono, viene visualizzato un messaggio di errore. Se si disabilita JavaScript, si avrà la convalida lato client, ma il server rileverà i valori inviati non sono validi e i valori del modulo verranno visualizzati nuovamente con messaggi di errore. Più avanti nell'esercitazione verranno esaminati convalida in modo più dettagliato.

Il `Html.ValidationMessageFor` helper nel *Edit. cshtml* vista modello ci occupiamo della visualizzazione dei messaggi di errore appropriato.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Tutti i `HttpGet` metodi seguono un modello simile. Ricevono un oggetto film (o un elenco di oggetti, nel caso di `Index`) e passare il modello alla visualizzazione. Il `Create` metodo passa un oggetto vuoto per la visualizzazione di creazione. Tutti i metodi che creano, modificano, eliminano o cambiano in altro modo i dati, eseguono questa operazione nell'overload `HttpPost` del metodo. Modifica dei dati in un metodo HTTP GET è un rischio per la sicurezza, come descritto nella voce del post di blog [ASP.NET MVC suggerimento & 46: non usare i collegamenti di eliminazione poiché creano le falle](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modifica dei dati in un metodo GET viola anche le procedure consigliate HTTP e l'architettura [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) modello, che consente di specificare che le richieste GET non devono modificherà lo stato dell'applicazione. In altre parole, l'esecuzione di un'operazione GET deve essere sicura, senza effetti collaterali e non modificare dati persistenti.

Se si utilizza un computer in lingua inglese Stati Uniti, è possibile ignorare questa sezione e passare all'esercitazione successiva. È possibile scaricare la versione Globalize di questa esercitazione [qui](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Per un'esercitazione di due parti eccellente di internazionalizzazione, vedere [ASP.NET MVC 5 internazionalizzazione di Nadeem](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> per supportare la convalida di jQuery per impostazioni locali di lingua diversa dall'inglese che usano la virgola (&quot;,&quot;) per un separatore decimale e formati di data non in lingua inglese Stati Uniti, è necessario includere *globalize.js* specifiche e  *Cultures/globalize.Cultures.js* file (da [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) e JavaScript usare `Globalize.parseFloat`. È possibile ottenere la convalida di jQuery non in lingua inglese da NuGet. (Non installare Globalize se si usa delle impostazioni locali in inglese.)


1. Dal **degli strumenti** menu fare clic su **NuGetLibrary Package Manager**e quindi fare clic su **Gestisci pacchetti NuGet per la soluzione**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. Nel riquadro sinistro, selezionare <strong>esplorare *.</strong>* (Vedere la figura seguente).
3. Nella casella di input, immettere * Globalize * *.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Scegli `jQuery.Validation.Globalize`, scegliere `MvcMovie` e fare clic su **installare**. Il *Scripts\jquery.globalize\globalize.js* file verrà aggiunto al progetto. Il *Scripts\jquery.globalize\cultures\* cartella contiene molti file JavaScript dalle impostazioni cultura. Si noti che potrebbe richiedere cinque minuti per installare questo pacchetto.

   Il codice seguente illustra le modifiche al file Views\Movies\Edit.cshtml: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Per evitare di ripetere questo codice in ogni visualizzazione di modifica, è possibile spostarlo nel file di layout. Per ottimizzare il download dello script, vedere l'esercitazione [Bundling and Minification](../../performance/bundling-and-minification.md).

Per altre informazioni, vedere [ASP.NET MVC 3 internazionalizzazione](http://afana.me/post/aspnet-mvc-internationalization.aspx) e [internazionalizzazione di ASP.NET MVC 3 - Part 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Come correzione temporanea, se non è possibile ottenere la convalida Usa le impostazioni locali, è possibile forzare il computer da usare inglese Stati Uniti o è possibile disabilitare JavaScript nel browser. Per forzare il computer da usare inglese Stati Uniti, è possibile aggiungere l'elemento di globalizzazione nella radice di progetti *Web. config* file. Il codice seguente illustra l'elemento globalization con le impostazioni cultura impostate su inglese degli Stati Uniti.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> Nella prossima esercitazione, si sarà implementare funzionalità di ricerca.

> [!div class="step-by-step"]
> [Precedente](accessing-your-models-data-from-a-controller.md)
> [Successivo](adding-search.md)
