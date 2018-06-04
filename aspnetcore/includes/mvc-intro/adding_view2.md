Sostituire il contenuto del file di vista Razor *Views/HelloWorld/Index.cshtml* con quanto segue:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

Passare a `http://localhost:xxxx/HelloWorld`. Il metodo `Index` in `HelloWorldController` non ha eseguito molte operazioni; ha eseguito l'istruzione `return View();` che ha specificato che il metodo deve usare un file di modello della vista per eseguire il rendering di una risposta al browser. Poiché non è stato specificato in modo esplicito il nome del file di modello della vista, MVC imposta come predefinito il file della vista *Index.cshtml* nella cartella */Views/HelloWorld*. L'immagine seguente mostra la stringa "Hello from our View Template!" hardcoded nella vista.

![Finestra del browser](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

Se la finestra del browser è di piccole dimensioni, ad esempio in un dispositivo mobile, potrebbe essere necessario alternare (toccare) il [pulsante di spostamento Bootstrap](http://getbootstrap.com/components/#navbar) in alto a destra per rendere visibili i collegamenti **Home**, **About** (Informazioni su) e **Contact** (Contatto).

![Finestra del browser con il pulsante di navigazione Bootstrap evidenziato](~/tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>Modifica delle viste e delle pagine di layout

Toccare i collegamenti di menu: **MvcMovie**, **Home** e **About** (Informazioni su). Ogni pagina mostra lo stesso layout di menu. Il layout di menu viene implementato nel file *Views/Shared/_Layout.cshtml*. Aprire il file *Views/Shared/_Layout.cshtml*.

I modelli di [layout](xref:mvc/views/layout) consentono di specificare il layout del contenitore HTML del sito in un'unica posizione e quindi di applicarlo in più pagine del sito. Trovare la riga `@RenderBody()`. `RenderBody` è un segnaposto dove tutte le pagine specifiche della vista vengono presentate, *incapsulate* nella pagina di layout. Ad esempio, se si seleziona il collegamento **About** (Informazioni su), il rendering della vista **Views/Home/About.cshtml** viene eseguito all'interno del metodo `RenderBody`.

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>Modificare il titolo e il collegamento di menu nel file di layout

Nell'elemento del titolo cambiare `MvcMovie` in `Movie App`. Modificare il testo di ancoraggio nel modello di layout da `MvcMovie` a `Movie App` e il controller da `Home` a `Movies`, come evidenziato di seguito:

::: moniker range="<= aspnetcore-2.0"
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout21.cshtml?highlight=6,29)]
::: moniker-end

>[!WARNING]
> Il controller `Movies` non è stato ancora implementato e quindi, se si fa clic su questo collegamento, verrà visualizzato un errore 404 (Non trovato).

Salvare le modifiche e toccare il collegamento **About** (Informazioni su). Si noti come il titolo sulla scheda del browser visualizzi ora **About - Movie App** (Informazioni su - Movie App) anziché **About - Mvc Movie** (Informazioni su - Mvc Movie): 

![Informazioni sulla scheda](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

Toccare il collegamento **Contact** e notare che anche il titolo e il testo di ancoraggio visualizzano **Movie App**. La modifica è stata apportata una volta nel modello di layout e tutte le pagine nel sito riflettono il nuovo testo del collegamento e il nuovo titolo.

Esaminare il file *Views/_ViewStart.cshtml*:


```HTML
@{
    Layout = "_Layout";
}
```

Il file *Views/_ViewStart.cshtml* attiva il file *Views/Shared/_Layout.cshtml* per ogni vista. È possibile usare la proprietà `Layout` per impostare una vista di layout differente oppure impostarlo su `null` e quindi non verrà usato alcun file di layout.

Cambiare il titolo della vista `Index`.

Aprire *Views/HelloWorld/Index.cshtml*. Apportare una modifica in due punti:

   * Il testo visualizzato nel titolo del browser.
   * L'intestazione secondaria (elemento `<h2>`).

Renderli leggermente diversi in modo da poter esaminare la parte specifica di app modificata dal frammento specifico di codice.


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

`ViewData["Title"] = "Movie List";` nel codice precedente imposta la proprietà `Title` del dizionario `ViewData` su "Movie List". La proprietà `Title` viene usata nell'elemento HTML `<title>` nella pagina di layout:


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Salvare le modifiche e passare a `http://localhost:xxxx/HelloWorld`. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server. Il titolo del browser viene creato con il valore `ViewData["Title"]` impostato nel modello di vista *Index.cshtml* e la stringa "- Movie App" aggiuntiva viene aggiunta nel file di layout.

Si noti anche come il contenuto del modello di vista *Index.cshtml* sia stato unito con il modello di vista *Views/Shared/_Layout.cshtml* e sia stata inviata una singola risposta HTML al browser. I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione. Per altre informazioni, vedere [Layout](xref:mvc/views/layout).

![Vista dell'elenco di film](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

Il breve testo di "dati", in questo caso il messaggio "Hello from our View Template!", è tuttavia hardcoded. L'applicazione MVC ha un elemento "V" (vista) ed è stato ottenuto un elemento "C" (controller), ma non ancora un elemento "M" (modello).

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Le azioni del controller vengono richiamate in risposta a una richiesta URL in ingresso. Una classe controller è il punto in cui viene scritto il codice che gestisce le richieste in ingresso del browser. Il controller recupera i dati da un'origine dati e determina il tipo di risposta da inviare al browser. I modelli di vista possono essere usati da un controller per generare e formattare una risposta HTML al browser.

Il compito dei controller è fornire i dati necessari affinché un modello di vista possa eseguire il rendering di una risposta. Procedura consigliata: i modelli di vista **non** devono eseguire la logica di business o interagire direttamente con un database. Un modello di vista deve invece usare solo i dati forniti dal controller. Questa "separazione delle problematiche" consente di mantenere il codice pulito e semplifica la gestione e i test.

Attualmente il metodo `Welcome` nella classe `HelloWorldController` accetta un parametro `name` e `ID` e quindi genera i valori direttamente al browser. Invece di ottenere il rendering di questa risposta come stringa, cambiare il controller in modo che usi un modello di vista. Il modello di vista genera una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta. A tale scopo, fare in modo che il controller inserisca i dati dinamici (parametri) di cui il modello di vista necessita in un dizionario `ViewData` a cui il modello di vista può accedere.

Tornare al file *HelloWorldController.cs* e modificare il metodo `Welcome` in modo da aggiungere un valore `Message` e `NumTimes` al dizionario `ViewData`. Il dizionario `ViewData` è un oggetto dinamico, ovvero è possibile inserirvi gli elementi desiderati; l'oggetto `ViewData` non dispone di proprietà definite fino a quando non si inserisce un elemento al suo interno. Il sistema di [associazione di modelli MVC](xref:mvc/models/model-binding) esegue il mapping dei parametri denominati (`name` e `numTimes`) dalla stringa di query nella barra degli indirizzi ai parametri nel metodo. Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

L'oggetto di dizionario `ViewData` contiene i dati che verranno passati alla vista. 

Creare un modello di vista Welcome denominato *Views/HelloWorld/Welcome.cshtml*.

Si creerà un ciclo nel modello di vista *Welcome.cshtml* che visualizza la stringa "Hello" `NumTimes`. Sostituire il contenuto di *Views/HelloWorld/Welcome.cshtml* con quanto segue:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Salvare le modifiche e passare all'URL seguente:

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

I dati vengono prelevati dall'URL e passati al controller usando lo [strumento di associazione di modelli MVC](xref:mvc/models/model-binding). Il controller crea un pacchetto di dati in un dizionario `ViewData` e passa tale oggetto alla vista. La vista esegue quindi il rendering dei dati in formato HTML al browser.

![Vista About (Informazioni su) con un'etichetta di benvenuto e la frase Hello Rick riportata quattro volte](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

Nell'esempio precedente è stato usato il dizionario `ViewData` per passare i dati dal controller a una vista. Più avanti nell'esercitazione si userà un modello di vista per passare i dati da un controller a una vista. In genere l'approccio basato sul modello di vista per passare i dati è preferito rispetto all'approccio basato sul dizionario `ViewData`. Per altre informazioni, vedere [ViewModel vs ViewData vs ViewBag vs TempData vs Session in MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) (Confronto tra ViewModel, ViewData, ViewBag, TempData e Session in MVC).

Queste operazioni hanno riguardato un tipo di "M" per modello, ma non il tipo di database. Creare un database di film con i concetti appresi.
