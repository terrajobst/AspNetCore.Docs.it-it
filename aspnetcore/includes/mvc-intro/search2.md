<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Il metodo `Index` precedente:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

Il metodo `Index` aggiornato con il parametro `id`:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.

![Vista Index con la parola ghost aggiunta all'URL e un elenco restituito di due film: Ghostbusters e Ghostbusters 2](~/tutorials/first-mvc-app/search/_static/g2.png)

Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film. A questo punto si aggiungeranno elementi dell'interfaccia utente per filtrare i film. Se è stata modificata la firma del metodo `Index` per testare come passare il parametro `ID` associato alla route, impostarlo di nuovo in modo che accetti un parametro denominato `searchString`:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

Aprire il file *Views/Movies/Index.cshtml* e aggiungere il markup `<form>` evidenziato di seguito:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

Il tag `<form>` HTML usa l'[helper tag del modulo](xref:mvc/views/working-with-forms) e quindi quando si invia il modulo, la stringa di filtro viene registrata nell'azione `Index` del controller di film. Salvare le modifiche e quindi testare il filtro.

![Vista Index con la parola ghost digitata nella casella di testo del filtro del titolo](~/tutorials/first-mvc-app/search/_static/filter.png)

Non è presente alcun overload del metodo `[HttpPost]` `Index` come previsto. Non è necessario perché il metodo non modifica lo stato dell'app, ma filtra solo i dati.

È possibile aggiungere il metodo `[HttpPost] Index` seguente.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

Il parametro `notUsed` viene usato per creare un overload per il metodo `Index`. Questo aspetto verrà trattato in una fase successiva dell'esercitazione.

Se si aggiunge questo metodo, l'invoker di azione trova la corrispondenza con il metodo `[HttpPost] Index` e il metodo `[HttpPost] Index` viene eseguito come illustrato nell'immagine seguente.

![Finestra del browser con la risposta dell'applicazione From HttpPost Index: filter on ghost](~/tutorials/first-mvc-app/search/_static/fo.png)

Tuttavia, anche se si aggiunge questa versione `[HttpPost]` del metodo `Index`, esiste una limitazione sul modo sulla relativa implementazione. Si supponga che si desideri usare come segnalibro una ricerca specifica o inviare un collegamento agli amici su cui possono fare clic per visualizzare lo stesso elenco filtrato di film. Si noti che l'URL per la richiesta HTTP POST è lo stesso URL per la richiesta GET (localhost:xxxxx/Movies/Index); le informazioni sulla ricerca non sono disponibili nell'URL. Le informazioni sulla stringa di ricerca vengono inviate al server come un [valore del campo modulo](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data). È possibile eseguire una verifica con gli strumenti di sviluppo del browser o l'eccellente [strumento Fiddler](http://www.telerik.com/fiddler). L'immagine seguente mostra gli strumenti di sviluppo del browser Chrome:

![Scheda di rete degli strumenti di sviluppo in Microsoft Edge che mostra il corpo di una richiesta con un valore searchString di ghost](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

È possibile esaminare il parametro di ricerca e il token [XSRF](xref:security/anti-request-forgery) nel corpo della richiesta. Si noti, come indicato nell'esercitazione precedente, che l'[helper tag del modulo](xref:mvc/views/working-with-forms) genera un token antifalsificazione [XSRF](xref:security/anti-request-forgery). Poiché non si stanno modificando i dati, non è necessario convalidare il token nel metodo del controller.

Poiché il parametro di ricerca si trova nel corpo della richiesta e non nell'URL, non è possibile acquisire queste informazioni sulla ricerca da usare come segnalibro o condividerle con altri utenti. È possibile risolvere il problema specificando che la richiesta deve essere `HTTP GET`.
