---
title: Routing di ASP.NET Core
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bbbcf9e4-3c4c-4f50-b91e-175fe9cae4e2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/routing
ms.openlocfilehash: 98756e2c5b336aabcf5155d929160b616baaf2ee
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="routing-in-aspnet-core"></a>Routing di ASP.NET Core

Da [Ryan Nowak](https://github.com/rynowak), [Steve Smith](http://ardalis.com), e [Rick Anderson](https://twitter.com/RickAndMSFT)

Funzionalità di routing è responsabile per il mapping di una richiesta in ingresso a un gestore di route. Le route sono definite nell'applicazione di ASP.NET e configurate quando viene avviata l'app. Una route può facoltativamente estrarre valori dall'URL contenuta nella richiesta, e questi valori possono quindi essere utilizzati per l'elaborazione della richiesta. Utilizza informazioni relative all'itinerario da app ASP.NET, la funzionalità di routing è in grado di generare gli URL che eseguono il mapping ai gestori di route. Pertanto, routing, è possibile trovare un gestore di route in base a un URL o l'URL corrispondente a un gestore di route specificato in base alle informazioni del gestore di route.

>[!IMPORTANT]
> Questo documento descrive di basso livello ASP.NET Core routing. Per il routing di ASP.NET MVC di base, vedere [Routing alle azioni del Controller](../mvc/controllers/routing.md)

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample)

## <a name="routing-basics"></a>Nozioni fondamentali di routing

Il routing utilizza *route* (implementazioni di [IRouter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.routing.irouter)) per:

* eseguire il mapping di richieste in ingresso per *indirizzare gestori*

* generare gli URL usati nelle risposte

In genere, un'applicazione ha un unico insieme di route. Quando arriva una richiesta, nell'ordine viene elaborato l'insieme di route. Esegue la ricerca della richiesta in ingresso per una route corrispondente all'URL di richiesta chiamando il `RouteAsync` metodo su ogni route disponibile nell'insieme di route. Al contrario, una risposta può utilizzare il routing per generare URL (ad esempio, per il reindirizzamento o collegamenti) in base alle informazioni sulla route e pertanto evitare di dover URL a livello di codice, che consente di manutenibilità.

Routing è connesso il [middleware](middleware.md) pipeline per la `RouterMiddleware` classe. [ASP.NET MVC](../mvc/overview.md) aggiunge il routing a pipeline middleware come parte della relativa configurazione. Per ulteriori informazioni sull'utilizzo di routing come componente autonomo, vedere [utilizzando routing-middleware](#using-routing-middleware).

<a name=url-matching-ref></a>

### <a name="url-matching"></a>URL corrispondente

URL corrispondente è il processo per il routing invia un fax in ingresso richieste per un *gestore*. Questo processo è in genere in base ai dati nel percorso URL, ma può essere estesa per prendere in considerazione tutti i dati nella richiesta. La possibilità di inviare le richieste ai diversi gestori è fondamentale per il ridimensionamento le dimensioni e la complessità di un'applicazione.

Immettere le richieste in ingresso di `RouterMiddleware`, che chiama il `RouteAsync` metodo su ogni route nella sequenza. Il `IRouter` istanza sceglie se *gestire* richiesta impostando il `RouteContext` `Handler` con un valore non null `RequestDelegate`. Se una route imposta un gestore per la richiesta, verrà richiamato per elaborare la richiesta di elaborazione viene arrestata e il gestore di route. Se tutte le route vengono tentate e viene individuato alcun gestore per la richiesta, il middleware chiama *Avanti* e viene richiamato il middleware successivo nella pipeline delle richieste.

L'input principale `RouteAsync` è il `RouteContext` `HttpContext` associato alla richiesta corrente. Il `RouteContext.Handler` e `RouteContext` `RouteData` sono output che verranno impostati dopo una route corrispondente.

Una corrispondenza durante `RouteAsync` verrà inoltre impostare le proprietà del `RouteContext.RouteData` ai valori appropriati in base l'elaborazione della richiesta fino a questo punto. Se una route corrisponde a una richiesta, il `RouteContext.RouteData` conterrà le informazioni sullo stato importanti circa la *risultato*.

`RouteData``Values` è un dizionario di *i valori di route* prodotta dalla route. Questi valori in genere sono determinati dalla suddivisione in token l'URL e possono essere utilizzati per accettare l'input dell'utente o per prendere decisioni ulteriormente invio all'interno dell'applicazione.

`RouteData``DataTokens` è un contenitore delle proprietà di dati aggiuntivi relativi a una route corrispondente. `DataTokens`vengono forniti per supportare lo stato di associazione dati con ogni route in modo l'applicazione può prendere decisioni in un secondo momento in base la route corrispondono. Questi valori sono definite dallo sviluppatore e si **non** influiscono sul comportamento di routing in alcun modo. Inoltre, i valori accantonati nei token di dati possono essere di qualsiasi tipo, a differenza di valori di route, che deve essere facilmente convertibili da e verso le stringhe.

`RouteData``Routers` è riportato un elenco di route che ha partecipato alla corrispondenza correttamente la richiesta. Le route possono essere annidate all'interno di un altro e `Routers` proprietà riflette il percorso nell'albero logico di route che ha comportato una corrispondenza. In genere il primo elemento `Routers` è la raccolta di route e deve essere utilizzato per la generazione di URL. L'ultimo elemento `Routers` è il gestore di route corrispondente.

### <a name="url-generation"></a>Generazione di URL

Generazione di URL è il processo da cui il routing può creare un percorso URL basato su un set di valori di route. In questo modo una separazione logica tra i gestori e gli URL che vi accedono.

Generazione URL segue un processo iterativo simile, ma ha inizio con il codice utente o il framework chiama la `GetVirtualPath` dell'insieme di route. Ogni *route* avranno relativo `GetVirtualPath` metodo chiamato nella sequenza finché non null `VirtualPathData` viene restituito.

Il database primario è input `GetVirtualPath` sono:

* `VirtualPathContext` `HttpContext`

* `VirtualPathContext` `Values`

* `VirtualPathContext` `AmbientValues`

Le route usano principalmente i valori della route forniti dal `Values` e `AmbientValues` decidere dove è possibile generare un URL e valori da includere. Il `AmbientValues` sono il set di valori di route che sono state prodotte dalla corrispondenza la richiesta corrente con il sistema di routing. Al contrario, `Values` sono i valori della route che specificano la modalità generare l'URL desiderato per l'operazione corrente. Il `HttpContext` viene fornita nel caso in cui una route deve ottenere servizi o dati aggiuntivi associati al contesto corrente.

Suggerimento: Considerare `Values` come un set di override per il `AmbientValues`. Generazione URL tenta di riutilizzare i valori di route della richiesta corrente per semplificarne l'inserimento generare URL per i collegamenti utilizzando la stessa route o valori di route.

L'output di `GetVirtualPath` è un `VirtualPathData`. `VirtualPathData`è un'operazione parallela del `RouteData`; contiene il `VirtualPath` per l'URL di output, nonché alcune proprietà aggiuntive che deve essere impostata per la route.

Il `VirtualPathData` `VirtualPath` proprietà contiene il *percorso virtuale* prodotta dalla route. A seconda delle esigenze potrebbe essere necessario elaborare ulteriormente il percorso. Ad esempio, se si desidera eseguire il rendering in HTML URL generato è necessario anteporre il percorso di base dell'applicazione.

Il `VirtualPathData` `Router` è un riferimento alla route che è stato generato correttamente l'URL.

Il `VirtualPathData` `DataTokens` proprietà è un dizionario di dati aggiuntivi relativi alla route che ha generato l'URL. Si tratta di parallelo di `RouteData.DataTokens`.

### <a name="creating-routes"></a>Creazione di route

Fornisce il routing di `Route` classe dell'implementazione standard di `IRouter`. `Route`Usa il *modello di route* sintassi per definire i modelli che verranno trovata corrispondenza con il percorso dell'URL quando `RouteAsync` viene chiamato. `Route`verrà utilizzato lo stesso modello di route per generare un URL quando `GetVirtualPath` viene chiamato.

La maggior parte delle applicazioni creerà le route chiamando `MapRoute` o uno dei metodi di estensione simile definiti in `IRouteBuilder`. Tutti questi metodi verrà creata un'istanza di `Route` e aggiungerlo all'insieme di route.

Nota: `MapRoute` non accetta un parametro del gestore di route - aggiunge solo le route che verranno gestite dal `DefaultHandler`. Poiché il gestore predefinito è un `IRouter`, potrebbe decidere di non gestire la richiesta. Ad esempio, ASP.NET MVC viene in genere configurato come un gestore predefinito che gestisce solo le richieste che corrispondono a un'azione e del controller disponibili. Per ulteriori informazioni sul routing per MVC, vedere [Routing alle azioni del Controller](../mvc/controllers/routing.md).

Questo è un esempio di un `MapRoute` chiamata utilizzata da una definizione di route ASP.NET MVC tipico:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Questo modello corrisponderanno a un percorso URL come `/Products/Details/17` ed estrarre i valori della route `{ controller = Products, action = Details, id = 17 }`. I valori della route sono determinati da suddividere il percorso dell'URL in segmenti una e la corrispondenza di ogni segmento con il *parametro di route* nel modello di route. Sono denominati parametri di route. Vengono definiti da racchiudere il nome del parametro tra parentesi graffe `{ }`.

Il modello può anche corrispondere il percorso URL `/` e potrebbe creare i valori `{ controller = Home, action = Index }`. Ciò accade perché il `{controller}` e `{action}` i parametri di route hanno valori predefiniti e `id` parametro di route è facoltativo. Uguale `=` sign seguita da un valore dopo il nome del parametro di route definisce un valore predefinito per il parametro. Un punto interrogativo `?` dopo il nome del parametro di route definisce il parametro come facoltativi. Parametri con un valore predefinito della route *sempre* producono un valore di route, se la route corrisponde, i parametri facoltativi non produrrà un valore di route se si è verificato alcun segmento di percorso URL corrispondente.

Vedere [al riferimento di modello di route](#route-template-reference) per una descrizione completa della sintassi e le caratteristiche del modello di route.

Questo esempio è inclusa una *indirizzare vincolo*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Questo modello corrisponderanno a un percorso URL come `/Products/Details/17`, ma non `/Products/Details/Apples`. La definizione di parametro di route `{id:int}` definisce un *indirizzare vincolo* per il `id` parametro di route. Implementano vincoli della route `IRouteConstraint` ed esaminare i valori di route per la verifica. In questo esempio il valore di route `id` deve essere convertibile in un numero intero. Vedere [riferimento di vincolo di route](#route-constraint-reference) per una spiegazione più dettagliata di vincoli della route che vengono forniti dal framework.

Altri overload di `MapRoute` accettare i valori per `constraints`, `dataTokens`, e `defaults`. Questi parametri aggiuntivi di `MapRoute` sono definite come tipo `object`. L'utilizzo tipico di questi parametri consiste nel passare un oggetto tipizzato in modo anonimo, in cui i nomi delle proprietà della corrispondenza di tipo anonimo nomi di parametro di route.

I due esempi seguenti creano route equivalente:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Suggerimento: La sintassi inline per definire i vincoli e le impostazioni predefinite può essere più utile per le route semplice. Tuttavia, esistono delle funzionalità, ad esempio i token dei dati che non sono supportati dalla sintassi inline.

In questo esempio illustra alcune altre funzionalità:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Questo modello corrisponderanno a un percorso URL come `/Blog/All-About-Routing/Introduction` che consentirà di estrarre i valori `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. I valori per la route predefinita `controller` e `action` vengono generati dalla route anche se sono non presenti parametri di route corrispondente nel modello. Nel modello di route, è possono specificare i valori predefiniti. Il `article` parametro di route è definito come un *catch-all* dall'aspetto di un asterisco `*` prima del nome di parametro di route. I parametri di route catch-all acquisire il resto del percorso dell'URL e possono anche corrispondere a una stringa vuota.

Questo esempio vengono aggiunti i token di dati e i vincoli della route:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Questo modello corrisponderanno a un percorso URL come `/Products/5` che consentirà di estrarre i valori `{ controller = Products, action = Details, id = 5 }` e token di dati `{ locale = en-US }`.

![Token di Windows di variabili locali](routing/_static/tokens.png)

<a name=id1></a>

### <a name="url-generation"></a>Generazione di URL

La `Route` classe può inoltre eseguire generazione URL combinando un set di valori di route con il relativo modello di route. Questo è logicamente il processo inverso di corrispondente al percorso URL.

Suggerimento: Per comprendere meglio la generazione di URL, si supponga l'URL a cui si desidera generare e quindi considerare come un modello di route corrisponderebbe tale URL. I valori che deve essere prodotta? È l'equivalente del funzionamento di generazione di URL in approssimativa di `Route` classe.

In questo esempio utilizza una route di stile di ASP.NET MVC base:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Con i valori della route `{ controller = Products, action = List }`, questa route genererà l'URL `/Products/List`. I valori della route vengono sostituiti dai parametri di route corrispondenti formare il percorso URL. Poiché `id` è un parametro di route, non rappresenta alcun problema che non ha un valore.

Con i valori della route `{ controller = Home, action = Index }`, questa route genererà l'URL `/`. I valori della route che sono stati specificati corrispondano ai valori predefiniti in modo i segmenti corrispondenti a tali valori possono essere omesso in modo sicuro. Si noti che entrambi gli URL generati verrebbero andata e ritorno con la definizione route e producono gli stessi valori di route che sono stati utilizzati per generare l'URL.

Suggerimento: È necessario utilizzare un'applicazione mediante ASP.NET MVC `UrlHelper` per generare URL anziché chiamare direttamente il routing.

Per ulteriori informazioni sul processo di generazione di URL, vedere [url-generazione-riferimento](#url-generation-reference).

## <a name="using-routing-middleware"></a>Utilizzando Routing Middleware

Aggiungere il pacchetto NuGet "Microsoft.AspNetCore.Routing".

Aggiungere il routing al contenitore del servizio in *Startup.cs*:

[!code-csharp[Principale](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

Le route devono essere configurate nel `Configure` metodo la `Startup` classe. Nell'esempio seguente Usa queste API:

* `RouteBuilder`
* `Build`
* `MapGet`Corrisponde solo richieste GET HTTP
* `UseRouter`

<!-- literal_block {"xml:space": "preserve", "source": "fundamentals/routing/sample/RoutingSample/Startup.cs", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->

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

La tabella seguente mostra le risposte con l'URI specificato.

| URI | Risposta  |
| ------- | -------- |
| /package/create/3  | Hello! I valori di route: [operazione, creare], [id, 3] |
| pacchetto/traccia/3  | Hello! I valori di route: [operazione, traccia], [id, -3] |
| / pacchetto/traccia/3 / | Hello! I valori di route: [operazione, traccia], [id, -3]  |
| /package traccia / | \<Passaggio alcuna corrispondenza > |
| OTTENERE /hello/Joe | Salve, Joe! |
| POST /hello/Joe | \<Passaggio a, HTTP GET solo corrispondenze > |
| OTTENERE /hello/Joe/Smith | \<Passaggio alcuna corrispondenza > |

Se si sta configurando una singola route, chiamare `app.UseRouter` passando un `IRouter` istanza. Non è necessario chiamare `RouteBuilder`.

Il framework fornisce un set di metodi di estensione per la creazione di route, ad esempio:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Alcuni di questi metodi, ad esempio `MapGet` richiedono un `RequestDelegate` deve essere fornito. Il `RequestDelegate` verrà utilizzato come il *gestore di route* quando la route corrisponde. Altri metodi di questa famiglia consentono di configurare una pipeline middleware che verrà utilizzata come gestore di route. Se il *mappa* metodo non accetta un gestore, ad esempio `MapRoute`, viene utilizzata la `DefaultHandler`.

Il `Map[Verb]` metodi utilizzano i vincoli per limitare la route per il verbo HTTP nel nome del metodo. Ad esempio, vedere [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) e [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Riferimento del modello di route

Token all'interno delle parentesi graffe (`{ }`) definire *parametri della route* che verrà associato se esiste una corrispondenza per la route. È possibile definire più di un parametro di route in un segmento di route, ma devono essere separati da un valore letterale. Ad esempio `{controller=Home}{action=Index}` non sarebbe una route valida, poiché è presente alcun valore letterale tra `{controller}` e `{action}`. Questi parametri di route devono avere un nome e possono avere attributi aggiuntivi specificato.

Testo letterale diverso da parametri di route (ad esempio, `{id}`) e il separatore di percorso `/` deve corrispondere il testo nell'URL. Criteri di testo è in base alla rappresentazione del percorso URL decodificato con distinzione tra maiuscole e minuscole. In modo che corrisponda il delimitatore di parametro di route letterale `{` o `}`, ripetendo il carattere di escape (`{{` o `}}`).

I modelli di URL che tentano di acquisire un nome file con un'estensione di file facoltativo sono considerazioni aggiuntive. Ad esempio, utilizzando il modello `files/{filename}.{ext?}` : se entrambi `filename` e `ext` esiste, verranno popolati entrambi i valori. Se solo `filename` esiste nell'URL, le corrispondenze di route, perché il punto finale `.` è facoltativo. I seguenti URL corrisponderebbe questa route:

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

È possibile utilizzare il `*` carattere come prefisso per un parametro di route da associare al resto dell'URI - si tratta di un *catch-all* parametro. Ad esempio, `blog/{*slug}` corrisponderà a qualsiasi URI avviata con `/blog` e qualsiasi valore riportata di seguito (che verrebbe assegnato al `slug` indirizzare valore). I parametri di catch-all possono inoltre corrispondere la stringa vuota.

Possono avere parametri di route *i valori predefiniti*, designato specificando il valore predefinito dopo il nome del parametro, separato da un `=`. Ad esempio, `{controller=Home}` definirebbe `Home` come valore predefinito per `controller`. Se è presente nell'URL per il parametro alcun valore, viene utilizzato il valore predefinito. Oltre ai valori predefiniti, i parametri di route possono essere facoltativi (specificati mediante l'aggiunta di un `?` alla fine del nome del parametro, come in `id?`). La differenza tra facoltativo e "con valore predefinito" è che un parametro di route con un valore predefinito produce sempre un valore. parametro facoltativo ha un valore solo se è disponibile.

I parametri di route possono inoltre avere vincoli, che devono corrispondere al valore di route associato dall'URL. Aggiunta di due punti `:` e il nome del vincolo dopo il nome del parametro di route specifica un *vincolo inline* su un parametro di route. Se il vincolo richiede argomenti quelle racchiuso tra parentesi `( )` dopo il nome del vincolo. È possibile specificare più vincoli inline aggiungendo un'altra virgola `:` e il nome del vincolo. Il nome del vincolo viene passato per il `IInlineConstraintResolver` servizio per creare un'istanza di `IRouteConstraint` da utilizzare nell'elaborazione di URL. Ad esempio, il modello di route `blog/{article:minlength(10)}` specifica il `minlength` vincolo con l'argomento `10`. Per ulteriori vincoli della route descrizione e un elenco dei vincoli fornita dal framework, vedere [riferimento di vincolo di route](#route-constraint-reference).

Nella tabella seguente illustra alcuni modelli di route e il relativo comportamento.

| Modello di route | Esempio di URL corrispondenti | Note |
| -------- | -------- | ------- |
| hello  | /Hello  | Solo corrisponda al percorso del singolo`/hello` |
| {Pagina Home =} | / | Corrispondente e imposta `Page` per`Home` |
| {Pagina Home =}  | / Contatto  | Corrispondente e imposta `Page` per`Contact` |
| {controller} / {action} / {id}? | / / Elenco dei prodotti | Esegue il mapping a `Products` controller e `List` azione |
| {controller} / {action} / {id}? | / Prodotti/dettagli/123  |  Esegue il mapping a `Products` controller e `Details` azione.  `id`Impostare su 123 |
| {controller Home =} / {azione = indice} / {id}? | /  |  Esegue il mapping a `Home` controller e `Index` ; (metodo) `id` viene ignorato. |

Utilizzo di un modello è in genere l'approccio più semplice per il routing. I vincoli e le impostazioni predefinite possono essere specificate anche all'esterno del modello di route.

Suggerimento: Abilitare [registrazione](logging.md) per vedere come il basate nelle implementazioni di routine, ad esempio `Route`, alle richieste.

## <a name="route-constraint-reference"></a>Riferimento di vincolo di route

Vincoli della route eseguire quando un `Route` ha corrisponde la sintassi dell'URL in ingresso e in formato token percorso dell'URL in valori di route. Vincoli della route in genere di controllare il valore di route associato tramite il modello di route ed eseguire una semplice sì/no decisione sulla o meno il valore è accettabile. Alcuni vincoli della route usare dati all'esterno del valore di route per valutare se la richiesta può essere instradata. Ad esempio, il `HttpMethodRouteConstraint` può accettare o rifiutare una richiesta basata sul relativo verbo HTTP.

>[!WARNING]
> Evitare l'utilizzo di vincoli per **convalida dell'input**, poiché ciò significa che tale valore non valido genererà un errore 404 (non trovato) anziché un 400 con un messaggio di errore appropriato. Vincoli della route devono essere utilizzati per **ambiguità** tra route simili, non si desidera convalidare l'input per una route specifica.

Nella tabella seguente illustra alcuni vincoli della route e il relativo comportamento previsto.

| vincolo | Esempio | Corrispondenze di esempio | Note |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Corrisponde a qualsiasi numero intero |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Corrispondenze `true` o `false` (tra maiuscole e minuscole) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Corrisponde a un oggetto valido `DateTime` valore (in lingua inglese - vedere avviso) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Corrisponde a un oggetto valido `decimal` valore (in lingua inglese - vedere avviso) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Corrisponde a un oggetto valido `double` valore (in lingua inglese - vedere avviso) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Corrisponde a un oggetto valido `float` valore (in lingua inglese - vedere avviso) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Corrisponde a un oggetto valido `Guid` valore |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Corrisponde a un oggetto valido `long` valore |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Stringa deve contenere almeno 4 caratteri |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Stringa deve essere non più di 8 caratteri |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Stringa deve essere esattamente di 12 caratteri |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Stringa deve contenere almeno 8 e non più di 16 caratteri |
| `min(value)` | `{age:min(18)}` | `19` | Valore intero deve essere almeno 18 |
| `max(value)` | `{age:max(120)}` |  `91` | Valore intero deve essere non più di 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Valore intero deve essere almeno 18 ma non più di 120 |
| `alpha` | `{name:alpha}` | `Rick` | Stringa deve essere costituito da uno o più caratteri alfabetici (`a`-`z`tra maiuscole e minuscole) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Stringa deve corrispondere all'espressione regolare (vedere i suggerimenti per la definizione di un'espressione regolare) |
| `required`  | `{name:required}` | `Rick` |  Utilizzato per imporre che un valore di parametro non è presente durante la generazione di URL |

>[!WARNING]
> Vincoli della route per verificare l'URL possono essere convertiti in un tipo CLR (ad esempio `int` o `DateTime`) utilizzare sempre le impostazioni cultura invarianti, si presuppone l'URL è non localizzabile. I vincoli della route forniti dal framework non modificare i valori archiviati nei valori di route. Tutti i valori di route analizzati dall'URL vengono archiviati come stringhe. Ad esempio, il [il vincolo di route Float](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) tenterà di convertire il valore di route per un valore float, ma il valore convertito viene utilizzato solo per verificare che può essere convertito in un valore float.

## <a name="regular-expressions"></a>Espressioni regolari 

Aggiunge il framework di ASP.NET Core `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` al costruttore di espressione regolare. Vedere [RegexOptions Enumeration](https://msdn.microsoft.com/library/system.text.regularexpressions.regexoptions(v=vs.110).aspx) per una descrizione di questi membri.

Espressioni regolari utilizzano delimitatori e i token simili a quelli utilizzati dal servizio Routing e il linguaggio c#. I token di espressione regolare devono essere codificati. Ad esempio, per utilizzare l'espressione regolare `^\d{3}-\d{2}-\d{4}$` nel servizio di Routing, è necessario disporre di `\` caratteri digitati come `\\` nel file di origine c# di escape per il `\` stringa di caratteri di escape (non usa [verbatim valori letterali stringa](https://msdn.microsoft.com/library/aa691090(v=vs.71).aspx)). Il `{` , `}` , ' [' e ']' necessario caratteri di escape raddoppiando in modo che i caratteri di delimitazione di parametro di Routing di escape.  Nella tabella seguente viene illustrata un'espressione regolare e la versione sottoposta a escape.

| Espressione               | Nota |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | Espressione regolare |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Caratteri di escape  |
| `^[a-z]{2}$` | Espressione regolare |
| `^[[a-z]]{{2}}$` | Caratteri di escape  |

Le espressioni regolari usate nel routing spesso inizierà con il `^` carattere (corrispondenza della posizione della stringa iniziale) e terminare con il `$` carattere (corrispondenza della posizione della stringa finale). Il `^` e `$` caratteri assicurarsi che la corrispondenza di espressione regolare il valore del parametro intera route. Senza il `^` e `$` caratteri dell'espressione regolare corrisponderà qualsiasi sottostringa all'interno della stringa, che è spesso non si desidera. Nella tabella seguente vengono illustrati alcuni esempi e viene spiegato perché corrispondono o non corrispondono.

| Espressione               | Stringa | Corrispondenza con | Commento |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | hello | sì | sottostringa corrispondenze |
| `[a-z]{2}` | 123abc456 | sì | sottostringa corrispondenze |
| `[a-z]{2}` | MZ | sì | corrisponde all'espressione |
| `[a-z]{2}` | MZ | sì | prevedono la distinzione tra maiuscole e minuscole non |
| `^[a-z]{2}$` |  hello | No | vedere `^` e `$` sopra |
| `^[a-z]{2}$` |  123abc456 | No | vedere `^` e `$` sopra |

Fare riferimento a [espressioni regolari di .NET Framework](https://msdn.microsoft.com/library/hs600312(v=vs.110).aspx) per ulteriori informazioni sulla sintassi delle espressioni regolari.

Per limitare un parametro con un set noto di valori possibili, utilizzare un'espressione regolare. Ad esempio `{action:regex(^(list|get|create)$)}` Cerca solo corrispondenze di `action` indirizzare valore `list`, `get`, o `create`. Se passato nel dizionario di vincoli, la stringa "^ (elenco | get | creare) $" sarebbe equivalente. I vincoli che vengono passati nel dizionario di vincoli (non in linea all'interno di un modello) che non corrispondono a uno dei vincoli noti vengono inoltre considerati espressioni regolari.

## <a name="url-generation-reference"></a>Riferimento di generazione di URL

Nell'esempio seguente viene illustrato come generare un collegamento a una route specificata di un dizionario dei valori di route e un `RouteCollection`.

[!code-csharp[Principale](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

Il `VirtualPath` generato alla fine dell'esempio precedente è `/package/create/123`.

Il secondo parametro per il `VirtualPathContext` costruttore è una raccolta di *ambiente valori*. I valori di ambiente epoca limitando il numero di valori, che uno sviluppatore deve specificare all'interno di un determinato contesto di richiesta. I valori della route corrente della richiesta corrente vengono considerati valori di ambiente per la generazione del collegamento. Ad esempio, in un'applicazione MVC ASP.NET se è il `About` azione del `HomeController`, non è necessario specificare il valore di route controller a cui collegarsi il `Index` azione (il valore di ambiente di `Home` verrà utilizzato).

I valori di ambiente che non corrispondano a un parametro vengono ignorati e i valori di ambiente vengono ignorati anche quando un valore fornito in modo esplicito ne esegue l'override, da sinistra verso destra nell'URL.

I valori che vengono forniti in modo esplicito, ma che non corrisponde a nessun elemento vengono aggiunti alla stringa di query. Nella tabella seguente mostra il risultato quando si usa il modello di route `{controller}/{action}/{id?}`.

| Valori di ambiente | Valori espliciti | Risultato |
| -------------   | -------------- | ------ |
| controller = "Home" | azione = "About" | `/Home/About` |
| controller = "Home" | controller = "Order", azione = "About" | `/Order/About` |
| controller = "Home", color = "Red" | azione = "About" | `/Home/About` |
| controller = "Home" | azione = colore "About" = "Red" | `/Home/About?color=Red`

Se una route ha un valore predefinito che non corrisponde a un parametro e tale valore viene fornito in modo esplicito, il valore predefinito deve corrispondere. Ad esempio:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

Generazione di collegamento genererebbero solo un collegamento per la route quando vengono specificati i valori corrispondenti per azione e del controller.
