---
title: Routing in ASP.NET Core
author: ardalis
description: Descrizione del modo in cui la funzionalità di routing di ASP.NET Core è responsabile del mapping di una richiesta in ingresso a un gestore di route.
ms.author: riande
ms.date: 07/25/2018
uid: fundamentals/routing
ms.openlocfilehash: 19265ac4d19915462c50628061600b1fde04694b
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254883"
---
# <a name="routing-in-aspnet-core"></a>Routing in ASP.NET Core

Di [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/) e [Rick Anderson](https://twitter.com/RickAndMSFT)

La funzionalità di routing è responsabile dell'esecuzione del mapping di una richiesta in ingresso a un gestore di route. Le route sono definite nell'app ASP.NET e vengono configurate all'avvio dell'app. Una route può facoltativamente estrarre valori dall'URL contenuto nella richiesta e questi valori possono quindi essere usati per l'elaborazione della richiesta. Usando le informazioni sulla route dell'app ASP.NET, la funzionalità di routing è anche in grado di generare URL che eseguono il mapping ai gestori di route. Di conseguenza, il routing è in grado di trovare un gestore di route in base a un URL oppure l'URL corrispondente a un gestore di route specifico in base alle informazioni del gestore di route.

>[!IMPORTANT]
> Questo documento descrive il routing di basso livello di ASP.NET Core. Per il routing di ASP.NET Core MVC, vedere [Route ad azioni del controller](../mvc/controllers/routing.md)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Nozioni fondamentali sul routing

Il routing usa le *route* (implementazioni di [IRouter](/dotnet/api/microsoft.aspnetcore.routing.irouter)) per:

* eseguire il mapping di richieste in ingresso ai *gestori di route*

* generare gli URL usati nelle risposte

In genere, un'app ha un'unica raccolta di route. Quando arriva una richiesta, la raccolta di route viene elaborata in ordine. La richiesta in ingresso cerca una route corrispondente all'URL di richiesta chiamando il metodo `RouteAsync` per ogni route disponibile nella raccolta di route. Al contrario, una risposta può usare il routing per generare URL, ad esempio per il reindirizzamento o i collegamenti, in base alle informazioni sulla route e in questo modo si evita di impostare gli URL come hardcoded ottimizzando la manutenibilità.

Il routing è connesso alla pipeline [middleware](xref:fundamentals/middleware/index) per mezzo della classe `RouterMiddleware`. [ASP.NET Core MVC](xref:mvc/overview) aggiunge il routing alla pipeline middleware come parte della configurazione. Per informazioni sull'uso del routing come componente autonomo, vedere [Uso del middleware di routing](#using-routing-middleware).

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>Corrispondenza URL

La corrispondenza dell'URL è il processo con cui il routing invia una richiesta in ingresso a un *gestore*. Questo processo in genere si basa sui dati presenti nel percorso URL, ma può essere esteso in modo da prendere in considerazione tutti i dati della richiesta. La possibilità di inviare le richieste a gestori separati è fondamentale per ridurre le dimensioni e la complessità di un'applicazione.

Le richieste in ingresso immettono `RouterMiddleware`, che chiama il metodo `RouteAsync` per ogni route della sequenza. L'istanza di `IRouter` sceglie se *gestire* la richiesta impostando `RouteContext.Handler` su un valore `RequestDelegate` diverso da Null. Se una route imposta un gestore per la richiesta, l'elaborazione della route si interrompe e viene richiamato il gestore per elaborare la richiesta. Se tutte le route vengono tentate e non viene trovato un gestore per la richiesta, il middleware chiama *next* e viene richiamato il middleware successivo nella pipeline di richieste.

L'input principale per `RouteAsync` è l'elemento `RouteContext.HttpContext` associato alla richiesta corrente. `RouteContext.Handler` e `RouteContext.RouteData` sono gli output che verranno impostati quando una route corrisponde.

Una corrispondenza durante `RouteAsync` imposta anche le proprietà di `RouteContext.RouteData` sui valori appropriati in base all'elaborazione della richiesta eseguita fino a questo punto. Se una route corrisponde a una richiesta, `RouteContext.RouteData` conterrà informazioni importanti sullo stato relative al *risultato*.

`RouteData.Values` è un dizionario di *valori di route* prodotti dalla route. Questi valori in genere sono determinati dalla suddivisione in token dell'URL e possono essere usati per accettare l'input dell'utente o per prendere ulteriori decisioni riguardo all'invio all'interno dell'applicazione.

`RouteData.DataTokens` è un elenco proprietà dei dati aggiuntivi correlati alla route corrispondente. Gli elementi `DataTokens` sono disponibili per supportare l'associazione dei dati sullo stato con ogni route in modo che l'applicazione in un secondo tempo sia in grado di prendere decisioni in base alla route corrispondente. Questi valori sono definiti dallo sviluppatore e **non** influiscono sul comportamento del routing in alcun modo. Inoltre, i valori accantonati nei token di dati possono essere di qualsiasi tipo, a differenza dei valori di route, che devono essere facilmente convertibili in e da stringhe.

`RouteData.Routers` è un elenco delle route che hanno preso parte alla corrispondenza corretta della richiesta. Le route possono essere annidate l'una nell'altra e la proprietà `Routers` riflette il percorso attraverso l'albero logico di route che hanno prodotto una corrispondenza. In genere il primo elemento in `Routers` è la raccolta di route e deve essere usato per la generazione di URL. L'ultimo elemento in `Routers` è il gestore di route corrispondente.

### <a name="url-generation"></a>Generazione di URL

La generazione di URL è il processo con cui il routing crea un percorso URL basato su un set di valori di route. Questo consente di avere una separazione logica tra i gestori e gli URL che vi accedono.

La generazione di URL segue un processo iterativo simile, ma inizia con la chiamata del codice dell'utente o del framework nel metodo `GetVirtualPath` della raccolta di route. Per ogni *route* il metodo `GetVirtualPath` verrà quindi chiamato in sequenza finché non viene restituito un valore `VirtualPathData` diverso da Null.

Gli input primari per `GetVirtualPath` sono:

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

Le route usano principalmente i valori di route specificati da `Values` e `AmbientValues` per decidere dove è possibile generare un URL e quali valori includere. Gli elementi `AmbientValues` sono il set di valori di route prodotti dalla corrispondenza della richiesta corrente con il sistema di routing. Al contrario, gli elementi `Values` sono i valori di route che specificano in che modo viene generato l'URL per l'operazione corrente. `HttpContext` viene specificato nel caso in cui una route richieda la disponibilità di servizi o dati aggiuntivi associati al contesto corrente.

Suggerimento: considerare `Values` come un set di override per `AmbientValues`. La generazione di URL tenta di usare nuovamente i valori di route della richiesta corrente per semplificare la generazione di URL per i collegamenti che usano la stessa route o gli stessi valori di route.

L'output di `GetVirtualPath` è un oggetto `VirtualPathData`. `VirtualPathData` è parallelo di `RouteData`, contiene `VirtualPath` per l'URL di output e alcune proprietà aggiuntive che devono essere impostate dalla route.

La proprietà `VirtualPathData.VirtualPath` contiene il *percorso virtuale* prodotto dalla route. In base alle esigenze specifiche, può essere necessario elaborare ulteriormente il percorso. Ad esempio, per eseguire il rendering in HTML dell'URL generato, è necessario anteporre il percorso di base dell'applicazione.

`VirtualPathData.Router` è un riferimento alla route che ha generato correttamente l'URL.

`VirtualPathData.DataTokens` è un dizionario di dati aggiuntivi correlati alla route che ha generato l'URL. È l'elemento parallelo di `RouteData.DataTokens`.

### <a name="creating-routes"></a>Creazione di route

Il routing specifica la classe `Route` come implementazione standard di `IRouter`. `Route` usa la sintassi del *modello di route* per definire i criteri in base ai quali confrontare il percorso dell'URL quando si chiama `RouteAsync`. `Route` userà lo stesso modello di route per generare un URL quando si chiama `GetVirtualPath`.

La maggior parte delle applicazioni crea le route chiamando `MapRoute` o uno dei metodi di estensione simili definiti per `IRouteBuilder`. Questi metodi creano tutti un'istanza di `Route` e la aggiungono alla raccolta di route.

Nota: `MapRoute` non accetta un parametro del gestore di route, aggiunge solo le route che verranno gestite da `DefaultHandler`. Poiché il gestore predefinito è un oggetto `IRouter`, può decidere di non gestire la richiesta. Ad esempio, ASP.NET MVC viene in genere configurato come un gestore predefinito che gestisce solo le richieste che corrispondono a un'azione e un controller disponibili. Per altre informazioni sul routing per MVC, vedere [Route ad azioni del controller](../mvc/controllers/routing.md).

Questo è un esempio di una chiamata `MapRoute` usata da una tipica definizione di route di ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Questo modello confronta un percorso URL come `/Products/Details/17` ed estrae i valori della route `{ controller = Products, action = Details, id = 17 }`. I valori della route vengono determinati suddividendo il percorso URL in segmenti e confrontando ogni segmento con il nome del *parametro di route* nel modello di route. I parametri di route sono denominati. Vengono definiti racchiudendo il nome del parametro tra parentesi graffe `{ }`.

Il modello precedente può anche confrontare il percorso URL `/` generando i valori `{ controller = Home, action = Index }`. Ciò accade perché i parametri di route `{controller}` e `{action}` hanno valori predefiniti e il parametro di route `id` è facoltativo. Un segno di uguale `=` seguito da un valore dopo il nome del parametro di route definisce un valore predefinito per il parametro. Un punto interrogativo `?` dopo il nome del parametro di route definisce il parametro come facoltativo. I parametri di route con un valore predefinito producono *sempre* un valore di route quando la route corrisponde. I parametri facoltativi non producono un valore di route, se non esiste un segmento di percorso URL corrispondente.

Vedere [Riferimento per i modelli di route](#route-template-reference) per una descrizione completa delle funzionalità e della sintassi del modello di route.

Questo esempio include un *vincolo di route*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Questo modello confronta un percorso URL come `/Products/Details/17`, ma non `/Products/Details/Apples`. La definizione del parametro di route `{id:int}` definisce un *vincolo di route* per il parametro di route `id`. I vincoli di route implementano `IRouteConstraint` ed esaminano i valori di route per verificarli. In questo esempio il valore di route `id` deve poter essere convertito in un numero intero. Vedere [Riferimento per i vincoli di route](#route-constraint-reference) per una spiegazione più dettagliata dei vincoli di route specificati dal framework.

Altri overload di `MapRoute` accettano i valori per `constraints`, `dataTokens` e `defaults`. Questi parametri aggiuntivi di `MapRoute` sono definiti come tipo `object`. L'uso tipico di questi parametri è passare un oggetto tipizzato in modo anonimo, in cui i nomi di proprietà di tipo anonimo corrispondono ai nomi dei parametri di route.

I due esempi seguenti creano route equivalenti:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Suggerimento: la sintassi inline per definire i vincoli e i valori predefiniti può essere più utile per le route semplici. Tuttavia, esistono funzionalità, ad esempio i token di dati, che non sono supportate dalla sintassi inline.

Questo esempio illustra alcune altre funzionalità:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Questo modello confronta un percorso URL come `/Blog/All-About-Routing/Introduction` ed estrae i valori `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. I valori di route predefiniti per `controller` e `action` vengono generati dalla route anche se sono non presenti parametri di route corrispondenti nel modello. I valori predefiniti possono essere specificati nel modello di route. Il parametro di route `article` è definito come *catch-all* dall'aspetto di un asterisco `*` prima del nome del parametro di route. I parametri di route catch-all acquisiscono il resto del percorso URL e possono anche corrispondere alla stringa vuota.

In questo esempio vengono aggiunti i vincoli di route e i token di dati:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Questo modello confronta un percorso URL come `/en-US/Products/5` ed estrae i valori `{ controller = Products, action = Details, id = 5 }` e i token di dati `{ locale = en-US }`.

![Token della finestra Variabili locali](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a>Generazione di URL

La classe `Route` è inoltre in grado di eseguire la generazione di URL combinando un set di valori di route con il relativo modello di route. Questo è logicamente il processo inverso di corrispondenza del percorso URL.

Suggerimento: per comprendere meglio la generazione di URL, si pensi a quale URL si vuole generare e al modo in cui un modello di route deve corrispondere a tale URL. Quali valori devono essere prodotti? Questo equivale approssimativamente al funzionamento della generazione di URL nella classe `Route`.

In questo esempio viene usata una route di base in stile ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Con i valori di route `{ controller = Products, action = List }` questa route genera l'URL `/Products/List`. I valori di route vengono sostituiti dai parametri di route corrispondenti per formare il percorso URL. Poiché `id` è un parametro di route facoltativo, non è un problema se non ha un valore.

Con i valori di route `{ controller = Home, action = Index }` questa route genera l'URL `/`. I valori di specificati corrispondono ai valori predefiniti, quindi i segmenti corrispondenti a tali valori possono essere omessi. Si noti che per entrambi gli URL generati viene eseguito il round trip con questa definizione di route e vengono prodotti gli stessi valori di route usati per generare l'URL.

Suggerimento: un'app che usa ASP.NET MVC deve usare `UrlHelper` per generare gli URL anziché eseguire la chiamata direttamente nel routing.

Per altre informazioni sul processo di generazione degli URL, vedere [Riferimento per la generazione di URL](#url-generation-reference).

## <a name="using-routing-middleware"></a>Uso del middleware di routing

Aggiungere il pacchetto NuGet "Microsoft.AspNetCore.Routing".

Aggiungere il routing al contenitore del servizio in *Startup.cs*:

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

Le route devono essere configurate nel metodo `Configure` della classe `Startup`. Nell'esempio seguente vengono usate queste API:

* `RouteBuilder`
* `Build`
* `MapGet` Confronta solo le richieste HTTP GET
* `UseRouter`

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
{
    var trackPackageRouteHandler = new RouteHandler(context =>
    {
        var routeValues = context.GetRouteData().Values;
        return context.Response.WriteAsync(
            $"Hello! Route values: {string.Join(", ", routeValues)}");
    });

    var routeBuilder = new RouteBuilder(app, trackPackageRouteHandler);

    routeBuilder.MapRoute(
        "Track Package Route",
        "package/{operation:regex(^(track|create|detonate)$)}/{id:int}");

    routeBuilder.MapGet("hello/{name}", context =>
    {
        var name = context.GetRouteValue("name");
        // This is the route handler when HTTP GET "hello/<anything>"  matches
        // To match HTTP GET "hello/<anything>/<anything>,
        // use routeBuilder.MapGet("hello/{*name}"
        return context.Response.WriteAsync($"Hi, {name}!");
    });

    var routes = routeBuilder.Build();
    app.UseRouter(routes);
}
```

Nella tabella seguente sono riportate le risposte con gli URI specificati.

| URI | Risposta  |
| ------- | -------- |
| /package/create/3  | Hello! Route values: [operation, create], [id, 3] |
| /package/track/-3  | Hello! Route values: [operation, track], [id, -3] |
| /package/track/-3/ | Hello! Route values: [operation, track], [id, -3]  |
| /package/track/ | \<Passaggio, nessuna corrispondenza> |
| GET /hello/Joe | Hi, Joe! |
| POST /hello/Joe | \<Passaggio, corrispondenza solo con HTTP GET> |
| GET /hello/Joe/Smith | \<Passaggio, nessuna corrispondenza> |

Se si sta configurando una singola route, chiamare `app.UseRouter` passando un'istanza di `IRouter`. Non è necessario chiamare `RouteBuilder`.

Il framework offre un set di metodi di estensione per la creazione di route, ad esempio:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Alcuni di questi metodi, ad esempio `MapGet`, richiedono che sia specificato un oggetto `RequestDelegate`. `RequestDelegate` verrà usato come *gestore di route* quando la route corrisponde. Altri metodi di questa famiglia consentono di configurare una pipeline middleware che verrà usata come gestore di route. Se il metodo *Map* metodo non accetta un gestore, ad esempio `MapRoute`, userà `DefaultHandler`.

I metodi `Map[Verb]` usano i vincoli per limitare la route al verbo HTTP nel nome del metodo. Ad esempio, vedere [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) e [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Riferimento per il modello di route

I token all'interno di parentesi graffe (`{ }`) definiscono i *parametri di route* che verranno associati se esiste una corrispondenza per la route. È possibile definire più parametri di route in un segmento di route, ma devono essere separati da un valore letterale. Ad esempio, `{controller=Home}{action=Index}` non è una route valida, poiché non è presente un valore letterale tra `{controller}` e `{action}`. Questi parametri di route devono avere un nome e possono avere attributi aggiuntivi.

Il testo letterale diverso dai parametri di route, ad esempio `{id}`, e il separatore di percorso `/` devono corrispondere al testo nell'URL. La corrispondenza del testo non fa distinzione tra maiuscole e minuscole e si basa sulla rappresentazione decodificata del percorso degli URL. Per verificare la corrispondenza del delimitatore letterale dei parametri di route `{` o `}`, eseguirne l'escape ripetendo il carattere (`{{` o `}}`).

I modelli di URL che tentano di acquisire un nome file con un'estensione facoltativa usano considerazioni aggiuntive. Ad esempio, se si usa il modello `files/{filename}.{ext?}` quando esistono sia `filename` che `ext`, verranno popolati entrambi i valori. Se esiste solo `filename` nell'URL, la route corrisponde perché il punto finale `.` è facoltativo. I seguenti URL corrispondono a questa route:

* `/files/myFile.txt`
* `/files/myFile`

È possibile usare il carattere `*` come prefisso per un parametro di route da associare al resto dell'URI, ovvero un parametro *catch-all*. Ad esempio, `blog/{*slug}` corrisponde a qualsiasi URI che inizia con `/blog` seguito da qualsiasi valore (che verrebbe assegnato al valore di route `slug`). I parametri catch-all possono anche corrispondere alla stringa vuota.

I parametri di route possono avere *valori predefiniti*, definiti specificando il valore predefinito dopo il nome del parametro, separato da un segno `=`. Ad esempio, `{controller=Home}` definisce `Home` come valore predefinito per `controller`. Il valore predefinito viene usato se nell'URL non è presente alcun valore per il parametro. Oltre ai valori predefiniti, i parametri di route possono essere facoltativi, specificati aggiungendo `?` alla fine del nome del parametro, come in `id?`. La differenza tra parametri facoltativi e parametri "con valori predefiniti" è che un parametro di route con un valore predefinito produce sempre un valore, mentre un parametro facoltativo ha un valore solo se ne viene specificato uno.

I parametri di route possono anche presentare dei vincoli, che devono corrispondere al valore di route associato dall'URL. Aggiungendo due punti `:` e il nome del vincolo dopo il nome del parametro di route, si specifica un *vincolo inline* per un parametro di route. Se il vincolo richiede argomenti, vengono specificati tra parentesi `( )` dopo il nome del vincolo. È possibile specificare più vincoli inline aggiungendo di nuovo i due punti `:` e il nome del vincolo. Il nome del vincolo viene passato al servizio `IInlineConstraintResolver` per creare un'istanza di `IRouteConstraint` da usare nell'elaborazione dell'URL. Ad esempio, il modello di route `blog/{article:minlength(10)}` specifica il vincolo `minlength` con l'argomento `10`. Per altre descrizioni dei vincoli di route e un elenco dei vincoli specificati dal framework, vedere [Riferimento per i vincoli di route](#route-constraint-reference).

La tabella seguente illustra alcuni modelli di route con il relativo comportamento.

| Modello di route | Esempio di URL corrispondente | Note |
| -------- | -------- | ------- |
| hello  | /hello  | Verifica la corrispondenza solo del singolo percorso `/hello` |
| {Page=Home} | / | Verifica la corrispondenza e imposta `Page` su `Home` |
| {Page=Home}  | /Contact  | Verifica la corrispondenza e imposta `Page` su `Contact` |
| {controller}/{action}/{id?} | /Products/List | Esegue il mapping al controller `Products` e all'azione `List` |
| {controller}/{action}/{id?} | /Products/Details/123  |  Esegue il mapping al controller `Products` e all'azione `Details`.  `id` impostato su 123 |
| {controller=Home}/{action=Index}/{id?} | /  |  Esegue il mapping al controller `Home` e al metodo `Index`; `id` viene ignorato. |

L'uso di un modello è in genere l'approccio più semplice al routing. I vincoli e le impostazioni predefinite possono essere specificati anche all'esterno del modello di route.

Suggerimento: abilitare la [registrazione](xref:fundamentals/logging/index) per vedere in che modo le implementazioni del routing, ad esempio `Route`, corrispondono alle richieste.

## <a name="reserved-routing-names"></a>Nomi riservati di routing

Le parole chiave seguenti sono nomi riservati e non possono essere usate come nomi o parametri di route:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Riferimento per i vincoli di route

I vincoli di route vengono eseguiti quando una `Route` corrisponde alla sintassi dell'URL in ingresso e suddivide il percorso URL in valori di route in formato token. I vincoli di route in genere controllano il valore di route associato usando il modello di route e stabiliscono con una semplice decisione sì/no se il valore è o non è accettabile. Alcuni vincoli di route usano i dati all'esterno del valore di route per stabilire se la richiesta può essere instradata. Ad esempio, `HttpMethodRouteConstraint` può accettare o rifiutare una richiesta in base al relativo verbo HTTP.

>[!WARNING]
> Evitare l'uso di vincoli per la **convalida dell'input**, poiché un input non valido genera un errore 404 (Non trovato) anziché 400 con messaggio di errore appropriato. I vincoli di route devono essere usati per evitare **ambiguità** tra route simili, non per convalidare gli input per una route specifica.

La tabella seguente illustra alcuni vincoli di route con il relativo comportamento.

| vincolo | Esempio | Esempi di corrispondenza | Note |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Corrisponde a qualsiasi numero intero |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Corrisponde a `true` o `false` (senza distinzione tra maiuscole e minuscole) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Corrisponde a un valore `DateTime` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Corrisponde a un valore `decimal` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Corrisponde a un valore `double` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Corrisponde a un valore `float` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Corrisponde a un valore `Guid` valido |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Corrisponde a un valore `long` valido |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | La stringa deve contenere almeno 4 caratteri |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | La stringa non deve contenere più di 8 caratteri |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | La stringa deve contenere esattamente 12 caratteri |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | La stringa deve contenere almeno 8 e non più di 16 caratteri |
| `min(value)` | `{age:min(18)}` | `19` | Il valore intero deve essere almeno 18 |
| `max(value)` | `{age:max(120)}` |  `91` | Il valore intero non deve essere superiore a 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Il valore intero deve essere almeno 18 ma non più di 120 |
| `alpha` | `{name:alpha}` | `Rick` | La stringa deve essere costituita da uno o più caratteri alfabetici (`a`-`z`, senza distinzione tra maiuscole e minuscole) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | La stringa deve corrispondere all'espressione regolare (vedere i suggerimenti per la definizione di un'espressione regolare) |
| `required`  | `{name:required}` | `Rick` |  Usato per imporre che un valore diverso da un parametro sia presente durante la generazione dell'URL |

Più vincoli delimitati da punti possono essere applicati a un singolo parametro. Ad esempio il vincolo seguente limita un parametro a un valore intero maggiore o uguale a 1:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

>[!WARNING]
> I vincoli di route che verificano l'URL possono essere convertiti in un tipo CLR, ad esempio `int` o `DateTime`, usano sempre le impostazioni cultura inglese non dipendenti da paese/area geografica, presupponendo che l'URL sia non localizzabile. I vincoli di route specificati dal framework non modificano i valori archiviati nei valori di route. Tutti i valori di route analizzati dall'URL vengono archiviati come stringhe. Ad esempio, il [vincolo di route Float](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) tenterà di convertire il valore di route in un valore float, ma il valore convertito viene usato solo per verificare che può essere convertito in un valore float.

## <a name="regular-expressions"></a>Espressioni regolari

Il framework di ASP.NET Core aggiunge `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` al costruttore di espressioni regolari. Vedere l'articolo sull'[enumerazione RegexOptions](/dotnet/api/system.text.regularexpressions.regexoptions) per una descrizione di questi membri.

Le espressioni regolari usano delimitatori e token simili a quelli usati dal routing e dal linguaggio C#. Per i token di espressione è necessario aggiungere caratteri di escape. Ad esempio, per usare l'espressione regolare `^\d{3}-\d{2}-\d{4}$` nel routing, è necessario digitare i caratteri `\` come `\\` nel file di origine C# per eseguire l'escape del carattere di escape `\` della stringa, a meno che non si usino [valori letterali stringa verbatim](/dotnet/csharp/language-reference/keywords/string). Raddoppiare i caratteri `{`, `}`, '[' e ']' per eseguire l'escape dei caratteri di delimitazione dei parametri di routing.  Nella tabella che segue sono riportate due espressioni regolari con la relativa versione dopo l'escape.

| Espressione               | Nota |
| ----------------- | ------------ |
| `^\d{3}-\d{2}-\d{4}$` | Espressione regolare |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Con caratteri di escape  |
| `^[a-z]{2}$` | Espressione regolare |
| `^[[a-z]]{{2}}$` | Con caratteri di escape  |

Le espressioni regolari usate nel routing spesso iniziano con il carattere `^` (corrispondenza con la posizione iniziale della stringa) e terminano con il carattere `$` (corrispondenza con la posizione finale della stringa). I caratteri `^` e `$` consentono di verificare che l'espressione regolare corrisponda all'intero valore del parametro di route. Senza i caratteri `^` e `$` l'espressione regolare corrisponderà a qualsiasi sottostringa all'interno della stringa e spesso questo non è il risultato desiderato. La tabella seguente illustra alcuni esempi e spiega perché si verifica o non si verifica la corrispondenza.

| Espressione               | Stringa | Corrispondenza con | Commento |
| ----------------- | ------------ |  ------------ |  ------------ |
| `[a-z]{2}` | hello | sì | corrispondenze di sottostringhe |
| `[a-z]{2}` | 123abc456 | sì | corrispondenze di sottostringhe |
| `[a-z]{2}` | mz | sì | corrisponde all'espressione |
| `[a-z]{2}` | MZ | sì | senza distinzione maiuscole/minuscole |
| `^[a-z]{2}$` |  hello | No | vedere `^` e `$` sopra |
| `^[a-z]{2}$` |  123abc456 | No | vedere `^` e `$` sopra |

Per altre informazioni sulla sintassi delle espressioni regolari, vedere [Espressioni regolari di .NET Framework](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference).

Per limitare un parametro a un set noto di valori possibili, usare un'espressione regolare. Ad esempio, `{action:regex(^(list|get|create)$)}` verifica la corrispondenza del valore di route `action` solo con `list`, `get` o `create`. Se passata nel dizionario di vincoli, la stringa "^(list|get|create)$" sarebbe equivalente. Anche i vincoli passati nel dizionario di vincoli (non inline all'interno di un modello) che non corrispondono a uno dei vincoli noti vengono considerati espressioni regolari.

## <a name="url-generation-reference"></a>Riferimento per la generazione di URL

L'esempio seguente illustra come generare un collegamento a una route usando un dizionario di valori di route e un oggetto `RouteCollection`.

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

L'oggetto `VirtualPath` generato alla fine dell'esempio precedente è `/package/create/123`.

Il secondo parametro per il costruttore `VirtualPathContext` è una raccolta di *valori di ambiente*. I valori di ambiente sono utili poiché limitano il numero di valori che uno sviluppatore deve specificare all'interno di un determinato contesto di richiesta. I valori di route correnti della richiesta corrente sono considerati valori di ambiente per la generazione del collegamento. Ad esempio, in un'app ASP.NET MVC se si è nell'azione `About` di `HomeController`, non è necessario specificare il valore di route del controller per collegarsi all'azione `Index` poiché verrà usato il valore di ambiente di `Home`.

I valori di ambiente vengono ignorati se non corrispondono a un parametro e quando vengono sostituiti da valori specificati in modo esplicito, procedendo da sinistra verso destra nell'URL.

I valori specificati in modo esplicito ma che non corrispondono ad alcun elemento vengono aggiunti alla stringa di query. La tabella seguente illustra il risultato ottenuto quando si usa il modello di route `{controller}/{action}/{id?}`.

| Valori di ambiente | Valori espliciti | Risultato |
| -------------   | -------------- | ------ |
| controller="Home" | action="About" | `/Home/About` |
| controller="Home" | controller="Order",action="About" | `/Order/About` |
| controller="Home",color="Red" | action="About" | `/Home/About` |
| controller="Home" | action="About",color="Red" | `/Home/About?color=Red`

Se una route ha un valore predefinito che non corrisponde a un parametro e tale valore viene specificato in modo esplicito, il valore deve corrispondere al valore predefinito. Ad esempio:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

La funzionalità di generazione del collegamento genera un collegamento per questa route solo se vengono specificati i valori corrispondenti per il controller e l'azione.
