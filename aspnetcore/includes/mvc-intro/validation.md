# <a name="adding-validation"></a>Aggiunta della convalida

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa sezione si aggiungerà la logica di convalida al modello `Movie` e si farà in modo che le regole di convalida vengano applicate ogni volta che un utente crea o modifica un film.

## <a name="keeping-things-dry"></a>Rispetto del principio DRY

Uno dei principi di progettazione MVC è [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("Don't Repeat Yourself"). In ASP.NET MVC si incoraggia il principio in base al quale l'utente specifica le funzionalità o il comportamento una sola volta e la specifica viene quindi applicata ovunque in un'app. In questo modo si riduce la quantità di codice che è necessario scrivere e si rende il codice meno soggetto a errori, più semplice da testare e gestire.

Il supporto della convalida fornito da MVC ed Entity Framework Core Code First è un valido esempio pratico del principio DRY. È possibile specificare in modo dichiarativo le regole di convalida in un'unica posizione (nella classe del modello) e le regole vengono applicate ovunque nell'app.

## <a name="adding-validation-rules-to-the-movie-model"></a>Aggiunta di regole di convalida al modello movie

Aprire il file *Movie.cs*. DataAnnotations fornisce un set predefinito di attributi di convalida che si applicano in modo dichiarativo a qualsiasi classe o proprietà. Contiene anche gli attributi di formattazione come `DataType` che guidano nella formattazione e non forniscono la convalida.

Aggiornare la classe `Movie` per poter sfruttare gli attributi di convalida `Required`, `StringLength`, `RegularExpression` e `Range` predefiniti.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]
::: moniker-end

Gli attributi di convalida specificano il comportamento da applicare per le proprietà del modello a cui vengono applicati. Gli attributi `Required` e `MinimumLength` indicano che una proprietà deve avere un valore, ma nulla impedisce all'utente di inserire spazi vuoti per soddisfare questa convalida. L'attributo `RegularExpression` viene usato per limitare i caratteri che possono essere inseriti. Nel codice precedente, per `Genre` e `Rating` è necessario usare solo lettere. Prima lettera in maiuscolo, spazi vuoti, numeri e caratteri speciali non sono consentiti. L'attributo `Range` vincola un valore all'interno di un intervallo specificato. L'attributo `StringLength` consente di impostare la lunghezza massima di una proprietà stringa e, facoltativamente, la lunghezza minima. I tipi di valore, ad esempio `decimal`, `int`, `float` e `DateTime`, sono intrinsecamente necessari e non richiedono l'attributo `[Required]`.

L'uso di regole di convalida applicate automaticamente da ASP.NET consente di rendere un'app più solida. In questo modo inoltre non è possibile omettere la convalida di un elemento e quindi inserire involontariamente dati errati nel database.

## <a name="validation-error-ui-in-mvc"></a>Interfaccia utente dell'errore di convalida in MVC

Eseguire l'app e passare al controller di film.

Toccare il collegamento **Crea nuovo** per aggiungere un nuovo film. Completare il modulo con alcuni valori non validi. Non appena la convalida del lato client jQuery rileva l'errore, viene visualizzato un messaggio di errore.

![Modulo di vista del film con diversi errori di convalida sul lato client jQuery](~/tutorials/first-mvc-app/validation/_static/val.png)

> [!NOTE]
> Potrebbe non essere possibile immettere virgole decimali nel campo `Price`. Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese degli Stati Uniti, è necessario eseguire alcuni passaggi per globalizzare l'app. [Problema 4076 su GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) per istruzioni sull'aggiunta della virgola decimale. 

Si noti come il modulo ha eseguito automaticamente il rendering di un messaggio di errore di convalida appropriato in ogni campo contenente un valore non valido. Gli errori vengono applicati sia sul lato client (utilizzo di JavaScript e jQuery) sia sul lato server (nel caso di un utente con JavaScript disabilitato).

Un vantaggio significativo è che non è necessario modificare una singola riga di codice nella classe `MoviesController` oppure nella vista *Create.cshtml* per abilitare l'interfaccia utente di convalida. Il controller e le viste creati in una fase precedente di questa esercitazione hanno selezionato automaticamente le regole di convalida specificate usando gli attributi di convalida delle proprietà della classe `Movie` del modello. Eseguire il test della convalida usando il metodo di azione `Edit` e viene applicata la stessa convalida.

I dati del modulo non vengono inviati al server fino a quando non sono più presenti errori di convalida sul lato client. È possibile verificare questa condizione inserendo un punto di interruzione nel metodo `HTTP Post` usando lo [strumento Fiddler](http://www.telerik.com/fiddler) o gli [strumenti di sviluppo F12](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).

## <a name="how-validation-works"></a>Funzionamento della convalida

Ci si potrebbe chiedere come la convalida dell'interfaccia utente sia stata generata senza aggiornamenti al codice nel controller o nelle viste. Il codice seguente mostra i due metodi `Create`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

Il primo metodo di azione (HTTP GET) `Create` visualizza il modulo di creazione iniziale. La seconda versione (`[HttpPost]`) gestisce l'invio del modulo. Il secondo metodo `Create` (la versione `[HttpPost]`) chiama `ModelState.IsValid` per verificare se esistono errori di convalida per il film. La chiamata a questo metodo valuta tutti gli attributi di convalida applicati all'oggetto. Se l'oggetto presenta errori di convalida, il metodo `Create` visualizza di nuovo il modulo. Se non sono presenti errori, il metodo salva il nuovo film nel database. Nell'esempio del film, il modulo non viene inviato al server quando vengono rilevati errori di convalida sul lato client; il secondo metodo `Create` non viene mai chiamato quando sono presenti errori di convalida sul lato client. Se si disabilita JavaScript nel browser, la convalida sul lato client viene disabilitata ed è possibile testare `ModelState.IsValid` del metodo `Create` HTTP POST rilevando eventuali errori di convalida.

È possibile impostare un punto di interruzione nel metodo `[HttpPost] Create` e verificare che il metodo non venga mai chiamato, la convalida sul lato client non invierà i dati del modulo in caso di rilevamento di errori di convalida. Se si disabilita JavaScript nel browser, quindi si invia il modulo con errori, verrà raggiunto il punto di interruzione. Si ottiene comunque la convalida completa senza JavaScript. 

La figura seguente illustra come disabilitare JavaScript nel browser FireFox.

![Firefox: nella scheda Contenuti in Opzioni deselezionare la casella di controllo Abilita Javascript.](~/tutorials/first-mvc-app/validation/_static/ff.png)

La figura seguente illustra come disabilitare JavaScript nel browser Chrome.

![Google Chrome: nella sezione Javascript delle impostazioni dei contenuti selezionare l'opzione per non consentire ai siti di eseguire JavaScript.](~/tutorials/first-mvc-app/validation/_static/chrome.png)

Dopo avere disabilitato JavaScript, inviare i dati non validi e procedere con il debugger.

![Durante il debug su un invio di dati non validi, Intellisense su ModelState.IsValid mostra che il valore è false.](~/tutorials/first-mvc-app/validation/_static/ms.png)

Di seguito è riportata la parte del modello di vista *Create.cshtml* di cui è stato eseguito lo scaffolding in precedenza nell'esercitazione. Viene usata dai metodi di azione illustrati in precedenza per visualizzare il modulo iniziale e per visualizzarlo nuovamente in caso di errore.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]

L'[helper tag di input](xref:mvc/views/working-with-forms) usa gli attributi [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) e produce gli attributi HTML necessari per la convalida jQuery sul lato client. L'[helper tag di convalida](xref:mvc/views/working-with-forms#the-validation-tag-helpers) visualizza gli errori di convalida. Per altre informazioni, vedere [Convalida](xref:mvc/models/validation).

L'aspetto molto interessante di questo approccio è che né il controller né il modello di vista `Create` sono consapevoli delle regole di convalida effettive applicate o dei messaggi di errore specifici visualizzati. Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`. Le stesse regole di convalida vengono applicate automaticamente alla vista `Edit` e ad altri modelli di vista eventualmente creati che modificano il modello.

Se necessario, è possibile modificare la logica di convalida in una posizione tramite l'aggiunta di attributi di convalida al modello, in questo esempio la classe `Movie`. Non è necessario preoccuparsi dell'incoerenza delle diverse parti dell'applicazione con la modalità di applicazione delle regole perché tutta la logica di convalida verrà definita in un'unica posizione e usata ovunque. In questo modo il codice rimane molto pulito e facile da gestire e sviluppare. Il principio DRY sarà ampiamente rispettato.

## <a name="using-datatype-attributes"></a>Utilizzo degli attributi DataType

Aprire il file *Movie.cs* ed esaminare la classe `Movie`. Lo spazio dei nomi `System.ComponentModel.DataAnnotations` fornisce gli attributi di formattazione oltre al set predefinito di attributi di convalida. È già stato applicato un valore di enumerazione `DataType` ai campi della data di rilascio e del prezzo. Il codice seguente illustra le proprietà `ReleaseDate` e `Price` con l'attributo appropriato `DataType`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

Gli attributi `DataType` specificano solo gli hint per far sì che il motore di vista formatti i dati (e fornisca gli elementi/attributi, ad esempio `<a>` per l'URL e `<a href="mailto:EmailAddress.com">` per la posta elettronica). È possibile usare l'attributo `RegularExpression` per convalidare il formato dei dati. L'attributo `DataType` viene usato per specificare un tipo di dati più specifico del tipo intrinseco del database; non si tratta di attributi di convalida. In questo caso si vuole solo tenere traccia della data, non dell'ora. L'enumerazione `DataType` fornisce molti tipi di dati, ad esempio Data, Ora, Numero di telefono, Valuta, Indirizzo di posta elettronica e altro ancora. L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo. Ad esempio, è possibile creare un collegamento `mailto:` per `DataType.EmailAddress` e fornire un selettore data per `DataType.Date` nei browser che supportano HTML5. Gli attributi `DataType` generano attributi `data-` HTML5 (dash di dati pronunciati) supportati dai browser HTML5. Gli attributi `DataType` **non** forniscono alcuna convalida.

`DataType.Date` non specifica il formato della data visualizzata. Per impostazione predefinita, il campo dei dati viene visualizzato in base ai formati predefiniti per il valore `CultureInfo` del server.

L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

L'impostazione `ApplyFormatInEditMode` specifica che la formattazione deve essere applicata anche quando il valore viene visualizzato in una casella di testo per la modifica. Questa scelta non è consigliabile per alcuni campi, ad esempio per i valori della valuta poiché non è opportuno che il simbolo di valuta sia inserito nella casella di testo per la modifica.

È possibile usare l'attributo `DisplayFormat` da solo, ma in genere è consigliabile usare l'attributo `DataType`. L'attributo `DataType` fornisce la semantica dei dati anziché la modalità di esecuzione del rendering in una schermata e fornisce i seguenti vantaggi che non si ottengono con DisplayFormat:

* Il browser può abilitare le funzionalità HTML5, ad esempio visualizzare un controllo di calendario, il simbolo della valuta appropriato per le impostazioni locali, i collegamenti alla posta elettronica e così via.

* Per impostazione predefinita, il browser eseguirà il rendering dei dati usando il formato corretto in base alle impostazioni locali del sistema.

* L'attributo `DataType` consente di abilitare MVC in modo da scegliere il modello di campo corretto per il rendering dei dati (`DisplayFormat`, se usato da solo, usa il modello di stringa).

> [!NOTE]
> La convalida jQuery non funziona con l'attributo `Range` e con `DateTime`. Ad esempio, il codice seguente visualizzerà sempre un errore di convalida sul lato client, anche quando la data è compreso nell'intervallo specificato:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

È necessario disabilitare la convalida della data jQuery per usare l'attributo `Range` con `DateTime`. In genere non è consigliabile compilare date reali nei modelli, quindi l'uso dell'attributo `Range` e di `DateTime` è sconsigliato.

Il codice seguente illustra la combinazione di attributi in una sola riga:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

Nella parte successiva della serie verrà esaminata l'applicazione e verranno apportati alcuni miglioramenti ai metodi `Details` e `Delete` generati automaticamente.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Utilizzo dei moduli](xref:mvc/views/working-with-forms)
* [Globalizzazione e localizzazione](xref:fundamentals/localization)
* [Introduzione agli helper tag](xref:mvc/views/tag-helpers/intro)
* [Creare helper tag](xref:mvc/views/tag-helpers/authoring)
