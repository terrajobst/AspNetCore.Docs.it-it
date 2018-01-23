Sostituire il contenuto di *Controllers/HelloWorldController.cs* con quanto segue:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

È possibile chiamare ogni metodo `public` in un controller come endpoint HTTP. Nell'esempio precedente entrambi i metodi restituiscono una stringa.  Notare i commenti che precedono ogni metodo.

Un endpoint HTTP è un URL definibile come destinazione nell'applicazione Web, ad esempio `http://localhost:1234/HelloWorld`, e combina il protocollo usato (`HTTP`), il percorso di rete del server Web, tra cui la porta TCP (`localhost:1234`) e l'URI di destinazione (`HelloWorld`).

Il primo commento indica che si tratta di un metodo [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) che viene richiamato aggiungendo "/HelloWorld/" all'URL di base. Il secondo commento specifica un metodo [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) che viene richiamato aggiungendo "/HelloWorld/Welcome/" all'URL. In una fase successiva dell'esercitazione si userà il motore di scaffolding per generare i metodi `HTTP POST`.

Eseguire l'app in modalità non di debug e aggiungere "HelloWorld" al percorso nella barra degli indirizzi. Il metodo `Index` restituisce una stringa.

![Finestra del browser con una risposta dell'applicazione, This is my default action](../../tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC richiama le classi controller, e i metodi di azione in esse contenute, in base all'URL in ingresso. Il valore predefinito della [logica di routing degli URL](../../mvc/controllers/routing.md) usata da MVC usa un formato simile al seguente per determinare il codice di richiamare:

`/[Controller]/[ActionName]/[Parameters]`

Il formato per il routing viene impostato nel metodo `Configure` nel file *Startup.cs*.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Quando si esegue l'app e non si forniscono i segmenti di URL, i valori predefiniti sono il controller "Home" e il metodo "Index" specificati nella riga del modello evidenziata sopra.

Il primo segmento di URL determina la classe di controller da eseguire. `localhost:xxxx/HelloWorld` esegue quindi il mapping alla classe `HelloWorldController`. La seconda parte del segmento URL determina il metodo di azione per la classe. Pertanto `localhost:xxxx/HelloWorld/Index` causa l'esecuzione del metodo `Index` della classe `HelloWorldController`. Si noti che è necessario solo passare a `localhost:xxxx/HelloWorld` e il metodo `Index` viene chiamato per impostazione predefinita. In questo modo `Index` è il metodo predefinito che verrà chiamato per un controller, se non viene specificato un nome di metodo in modo esplicito. La terza parte del segmento di URL ( `id`) è relativa ai dati di route. I dati di route verranno esaminati in una fase successiva di questa esercitazione.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il metodo `Welcome` viene eseguito e restituisce la stringa "This is the Welcome action method...". Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`. Non è stata ancora usata la parte `[Parameters]` dell'URL.

![Finestra del browser con una risposta dell'applicazione, This is the Welcome action method](../../tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Modificare il codice in modo da passare le informazioni dei parametri dall'URL al controller. Ad esempio `/HelloWorld/Welcome?name=Rick&numtimes=4`. Modificare il metodo `Welcome` in modo da includere due parametri, come illustrato nel codice seguente. 

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Il codice precedente:

* Usa la funzionalità di parametro facoltativo in C# per indicare che il parametro `numTimes` viene impostato come predefinito su 1, se non viene passato alcun valore.
* Usa `HtmlEncoder.Default.Encode` per proteggere le app da input dannoso, in particolare in JavaScript. 
* Usa [stringhe interpolate](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).

Eseguire l'app e passare a:

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Sostituire xxxx con il numero di porta. È possibile provare diversi valori per `name` e `numtimes` nell'URL. Il sistema di [associazione di modelli](../../mvc/models/model-binding.md) MVC esegue il mapping dei parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo. Per altre informazioni, vedere [Associazione di modelli](../../mvc/models/model-binding.md).

![Finestra del browser con una risposta dell'applicazione, Hello Rick, NumTimes is: 4](../../tutorials/first-mvc-app/adding-controller/_static/rick4.png)

Nell'immagine precedente non viene usato il segmento di URL (`Parameters`), i parametri `name` e `numTimes` vengono passati come [stringhe di query](https://wikipedia.org/wiki/Query_string). Il punto interrogativo `?` nell'URL precedente è un separatore e seguono le stringhe di query. Il carattere `&` separa le stringhe di query.

Sostituire il metodo `Welcome` con il codice seguente:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Eseguire l'app e immettere l'URL seguente: `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![Finestra del browser con una risposta dell'applicazione, Hello Rick, ID: 3](../../tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

Questa volta il terzo segmento di URL corrisponde al parametro di route `id`. Il metodo `Welcome` contiene un parametro `id` che corrisponde al modello di URL nel metodo `MapRoute`. Il carattere finale `?` (in `id?`) indica che il parametro `id` è facoltativo.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

In questi esempi il controller rappresenta la parte "VC" di MVC, ovvero il ruolo di vista e controller. Il controller restituisce direttamente l'HTML. In genere è preferibile che i controller non restituiscano direttamente l'HTML, dal momento che la codifica e la gestione sono molto complesse. In genere usare invece un file del modello di vista Razor separato per generare la risposta HTML. Questa operazione viene eseguita nell'esercitazione successiva.
