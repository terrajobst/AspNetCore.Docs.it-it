---
title: Routing ad azioni del controller in ASP.NET Core
author: rick-anderson
description: Informazioni su come ASP.NET Core MVC usa middleware di routing per verificare la corrispondenza degli URL delle richieste in ingresso ed eseguirne il mapping alle azioni.
ms.author: riande
ms.date: 01/24/2019
uid: mvc/controllers/routing
ms.openlocfilehash: b4d5cd3add3fda6b70873eb5cce1dcee651f9185
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087511"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a>Routing ad azioni del controller in ASP.NET Core

Di [Ryan Nowak](https://github.com/rynowak) e [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC usa [middleware](xref:fundamentals/middleware/index) di routing per verificare la corrispondenza degli URL delle richieste in ingresso ed eseguirne il mapping alle azioni. Le route sono definite nel codice di avvio o negli attributi. Le route descrivono il modo in cui i percorsi URL devono corrispondere alle azioni. Vengono usate anche per generare gli URL (per i collegamenti) inviati nelle risposte.

Le azioni vengono indirizzate in modo convenzionale o con attributi. Se una route viene inserita nel controller o nell'azione, viene indirizzata con attributi. Per altre informazioni, vedere [Routing misto](#routing-mixed-ref-label).

Questo documento illustra le interazioni tra MVC e il routing e il modo in cui le tipiche app MVC usano le funzionalità di routing. Vedere [Routing](xref:fundamentals/routing) per informazioni dettagliate sul routing avanzato.

## <a name="setting-up-routing-middleware"></a>Impostazione del middleware di routing

Nel metodo *Configure* può apparire codice simile al seguente:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Nella chiamata a `UseMvc`, si usa `MapRoute` per creare una singola route, a cui si farà riferimento come alla route predefinita (`default`). La maggior parte delle app MVC userà una route con un modello simile alla route `default`.

Il modello di route `"{controller=Home}/{action=Index}/{id?}"` può corrispondere a un percorso URL come `/Products/Details/5` ed estrae i valori di route `{ controller = Products, action = Details, id = 5 }` suddividendo il percorso in token. MVC tenterà di individuare un controller denominato `ProductsController` e di eseguire l'azione `Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Si noti che in questo esempio l'associazione di modelli usa il valore di `id = 5` per impostare il parametro `id` su `5` quando si richiama l'azione. Per maggiori dettagli, vedere [Associazione di modelli](../models/model-binding.md).

Usando la route `default`:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Il modello di route:

* `{controller=Home}` definisce `Home` come oggetto `controller` predefinito

* `{action=Index}` definisce `Index` come oggetto `action` predefinito

* `{id?}` definisce `id` come facoltativo

I parametri di route predefiniti e facoltativi non devono necessariamente essere presenti nel percorso URL per trovare una corrispondenza. Vedere [Riferimento per i modelli di route](../../fundamentals/routing.md#route-template-reference) per una descrizione dettagliata della sintassi del modello di route.

`"{controller=Home}/{action=Index}/{id?}"` può corrispondere al percorso URL `/` e restituirà i valori di route `{ controller = Home, action = Index }`. I valori per `controller` e `action` usano i valori predefiniti, `id` non genera un valore perché non esiste un segmento corrispondente nel percorso URL. MVC usa questi valori di route per selezionare l'azione `HomeController` e `Index`:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Usando questa definizione del controller e il modello di route, l'azione `HomeController.Index` viene eseguita per tutti i percorsi URL seguenti:

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

Il metodo pratico `UseMvcWithDefaultRoute`:

```csharp
app.UseMvcWithDefaultRoute();
```

Può essere usato per sostituire:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc` e `UseMvcWithDefaultRoute` aggiungono un'istanza di `RouterMiddleware` alla pipeline middleware. MVC non interagisce direttamente con il middleware e usa il routing per gestire le richieste. MVC è connesso alle route attraverso un'istanza di `MvcRouteHandler`. Il codice all'interno di `UseMvc` è simile al seguente:

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc` non definisce direttamente le route, aggiunge un segnaposto alla raccolta di route per la route `attribute`. L'overload `UseMvc(Action<IRouteBuilder>)` consente di aggiungere le proprie route e supporta anche il routing con attributi.  `UseMvc` e tutte le sue varianti aggiungono un segnaposto per la route con attributi: il routing con attributi è sempre disponibile indipendentemente dal modo in cui si configura `UseMvc`. `UseMvcWithDefaultRoute` definisce una route predefinita e supporta il routing con attributi. La sezione [Routing con attributi](#attribute-routing-ref-label) include maggiori dettagli sul routing con attributi.

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>Routing convenzionale

La route predefinita (`default`):

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

è un esempio di *routing convenzionale*. Questo stile è detto *routing convenzionale* poiché stabilisce una *convenzione* per i percorsi URL:

* il primo segmento del percorso esegue il mapping al nome del controller

* il secondo esegue il mapping al nome dell'azione.

* il terzo segmento viene usato per un `id` facoltativo usato per eseguire il mapping a un'entità del modello

Usando la route `default`, il percorso URL `/Products/List` esegue il mapping all'azione `ProductsController.List` e `/Blog/Article/17` lo esegue a `BlogController.Article`. Questo mapping si basa **solo** sui nomi dell'azione e del controller e non su spazi dei nomi, percorsi dei file di origine o parametri del metodo.

> [!TIP]
> L'uso del routing convenzionale con la route predefinita consente di compilare rapidamente l'applicazione senza dover creare un nuovo modello di URL per ogni azione che si definisce. Per un'applicazione con azioni di stile CRUD, la coerenza degli URL tra i controller può semplificare il codice e rendere l'interfaccia utente più prevedibile.

> [!WARNING]
> L'oggetto `id` è definito come facoltativo nel modello di route, ovvero le azioni possono essere eseguite senza l'ID specificato come parte dell'URL. In genere, se si omette `id` dall'URL, il valore viene impostato su `0` dall'associazione del modello e di conseguenza non viene rilevata alcuna entità nel database corrispondente a `id == 0`. Il routing con attributi offre un controllo con granularità fine che consente di rendere l'ID obbligatorio per alcune azioni e non per altre. Per convenzione la documentazione include i parametri facoltativi come `id` quando è probabile che appaiano nell'uso corretto.

## <a name="multiple-routes"></a>Route multiple

È possibile aggiungere più route all'interno di `UseMvc` aggiungendo altre chiamate a `MapRoute`. Questa operazione consente di definire più convenzioni o di aggiungere route convenzionali dedicate a un'azione specifica, ad esempio:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

La route `blog` è una *route convenzionale dedicata*, vale a dire che usa il sistema di routing convenzionale, ma è dedicata a un'azione specifica. Poiché `controller` e `action` non appaiono nel modello di route come parametri, possono avere solo i valori predefiniti e quindi questa route eseguirà sempre il mapping all'azione `BlogController.Article`.

Le route sono ordinate nella raccolta di route e verranno elaborate nell'ordine in cui vengono aggiunte. Quindi in questo esempio la route `blog` viene tentata prima della route `default`.

> [!NOTE]
> Le *route convenzionali dedicate* usano spesso parametri di route catch-all come `{*article}` per acquisire la parte rimanente del percorso URL. In questo modo una route può diventare troppo "greedy", ovvero corrispondere agli URL che dovrebbero corrispondere ad altre route. Posizionare le route "greedy" più avanti nella tabella delle route per risolvere il problema.

### <a name="fallback"></a>Fallback

Come parte dell'elaborazione della richiesta, MVC verifica che i valori di route possano essere usati per trovare un controller e un'azione nell'applicazione. Se i valori di route non corrispondano a un'azione, la route non viene considerata una corrispondenza e viene tentata la route successiva. Questa operazione si chiama *fallback* e viene eseguita allo scopo di semplificare i casi in cui le route convenzionali si sovrappongono.

### <a name="disambiguating-actions"></a>Rimozione dell'ambiguità delle azioni

Quando due azioni corrispondono in base al routing, MVC deve rimuovere l'ambiguità per scegliere il candidato migliore, altrimenti genera un'eccezione. Ad esempio:

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Questo controller definisce due azioni corrispondenti al percorso URL `/Products/Edit/17` e indirizzano i dati `{ controller = Products, action = Edit, id = 17 }`. Si tratta di un modello tipico per i controller MVC in cui `Edit(int)` visualizza un modulo per modificare un prodotto e `Edit(int, Product)` elabora il modulo inviato. Per poter eseguire l'operazione, MVC deve scegliere `Edit(int, Product)` se la richiesta è un `POST` HTTP e `Edit(int)` quando il verbo HTTP è un altro elemento.

`HttpPostAttribute` (`[HttpPost]`) è un'implementazione di `IActionConstraint` che consente di selezionare l'azione solo quando il verbo HTTP è `POST`. La presenza di un oggetto `IActionConstraint` fa sì che `Edit(int, Product)` sia una corrispondenza migliore di `Edit(int)`, quindi `Edit(int, Product)` viene tentata per prima.

È necessario scrivere implementazioni personalizzate di `IActionConstraint` solo in scenari specializzati, ma è importante comprendere il ruolo degli attributi, ad esempio `HttpPostAttribute`. Attributi simili vengono definiti per altri verbi HTTP. Nel routing convenzionale è normale che le azioni usino lo stesso nome di azione quando fanno parte di un flusso di lavoro `show form -> submit form`. La comodità offerta da questo modello sarà più evidente dopo aver esaminato la sezione relativa a [IActionConstraint](#understanding-iactionconstraint).

Se vi sono più route corrispondenti e MVC non è in grado di trovare la route migliore, viene generata un'eccezione `AmbiguousActionException`.

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>Nomi di route

Le stringhe `"blog"` e `"default"` degli esempi seguenti sono nomi di route:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

I nomi di route consentono di assegnare a una route un nome logico, in modo che la route denominata possa essere usata per la generazione di URL. Questo semplifica notevolmente la creazione degli URL quando l'ordinamento delle route può rendere complessa la generazione di URL. I nomi delle route devono essere univoci a livello di applicazione.

I nomi di route non influiscono sulla corrispondenza degli URL o sulla gestione delle richieste, vengono usati solo per la generazione di URL. L'articolo sul [routing](xref:fundamentals/routing) contiene informazioni dettagliate sulla generazione di URL, inclusa la generazione negli helper specifici di MVC.

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>Routing con attributi

Il routing con attributi usa un set di attributi per eseguire il mapping delle azioni direttamente ai modelli di route. Nell'esempio seguente viene usato l'oggetto `app.UseMvc();` nel metodo `Configure` e non viene passata alcuna route. `HomeController` corrisponde a un set di URL simile a quello a cui corrisponderebbe la route predefinita `{controller=Home}/{action=Index}/{id?}`:

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

L'azione `HomeController.Index()` verrà eseguita per tutti i percorsi URL, `/`, `/Home` o `/Home/Index`.

> [!NOTE]
> In questo esempio viene evidenziata un'importante differenza a livello di programmazione tra il routing con attributi e il routing convenzionale. Il routing con attributi richiede più input per specificare una route, la route convenzionale predefinita gestisce le route in modo più conciso. Tuttavia, il routing con attributi consente, e richiede, un controllo preciso dei modelli route da applicare a ogni azione.

Con il routing con attributi i nomi del controller e delle azioni **non** hanno alcun ruolo nella selezione dell'azione. In questo esempio viene verificata la corrispondenza degli stessi URL dell'esempio precedente.

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> I modelli di route precedenti non definiscono i parametri di route per `action`, `area` e `controller`. In realtà, questi parametri di route non sono consentiti nelle route con attributi. Poiché il modello di route è già associato a un'azione, non avrebbe senso analizzare il nome dell'azione dall'URL.

## <a name="attribute-routing-with-httpverb-attributes"></a>Routing con attributi Http[Verb]

Il routing con attributi può usare anche gli attributi `Http[Verb]`, ad esempio `HttpPostAttribute`. Tutti questi attributi possono accettare un modello di route. Questo esempio illustra due azioni che corrispondono allo stesso modello di route:

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

Per un percorso URL come `/products` l'azione `ProductsApi.ListProducts` verrà eseguita quando il verbo HTTP è `GET` e `ProductsApi.CreateProduct` verrà eseguita quando il verbo HTTP è `POST`. Nel routing con attributi l'URL viene prima confrontato con il set di modelli di route definiti dagli attributi della route. Quando un modello di route corrisponde, vengono applicati i vincoli `IActionConstraint` per determinare quali azioni possono essere eseguite.

> [!TIP]
> Quando si compila un'API REST, è raro che sia opportuno usare `[Route(...)]` per un metodo di azione. Si consiglia di usare l'oggetto `Http*Verb*Attributes`, più specifico, per essere precisi circa gli elementi supportati dall'API. I client delle API REST devono sapere quali percorsi e verbi HTTP devono essere mappati a operazioni logiche specifiche.

Poiché una route con attributi si applica a un'azione specifica, è facile fare in modo che i parametri siano richiesti come parte della definizione del modello di route. In questo esempio `id` è richiesto come parte del percorso URL.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

L'azione `ProductsApi.GetProduct(int)` verrà eseguita per un percorso URL come `/products/3`, ma non per un percorso URL come `/products`. Vedere [Routing](../../fundamentals/routing.md) per una descrizione completa dei modelli di route e delle opzioni correlate.

## <a name="route-name"></a>Nome di route

Nell’esempio di codice riportato di seguito viene definito un *nome di route* di `Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

I nomi di route possono essere usati per generare un URL in base a un percorso specifico. I nomi di route non influiscono sul comportamento del routing riguardo alla corrispondenza degli URL e vengono usati solo per la generazione di URL. I nomi di route devono essere univoci a livello di applicazione.

> [!NOTE]
> Confrontare questa opzione con la *route predefinita* convenzionale, che definisce il parametro `id` come facoltativo (`{id?}`). La possibilità di specificare in modo preciso le API presenta dei vantaggi, ad esempio consente di inviare `/products` e `/products/5` ad azioni  differenti.

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>Combinazione di route

Per rendere il routing con attributi meno ripetitivo, gli attributi di route del controller vengono combinati con gli attributi di route delle singole azioni. I modelli di route definiti per il controller vengono anteposti ai modelli di route delle azioni. Inserendo un attributo di route nel controller, **tutte** le azioni presenti nel controller useranno il routing con attributi.

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

In questo esempio il percorso URL `/products` può corrispondere a `ProductsApi.ListProducts` e il percorso URL `/products/5` può corrispondere a `ProductsApi.GetProduct(int)`. Entrambe le azioni corrispondono solo a HTTP `GET` poiché sono contrassegnate con `HttpGetAttribute`.

I modelli di route applicati a un'azione che iniziano con `/` o `~/` non vengono combinati con i modelli di route applicati al controller. Questo esempio illustra la corrispondenza di un set di percorsi URL simile alla *route predefinita*.

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a>Ordinamento delle route con attributi

A differenza delle route convenzionali che vengono eseguite in un ordine definito, il routing con attributi crea una struttura ad albero e confronta tutte le route contemporaneamente. Si comporta come se le voci delle route seguissero un ordine ideale in cui le route più specifiche vengono probabilmente eseguite prima delle route più generali.

Ad esempio, una route come `blog/search/{topic}` è più specifica di una route come `blog/{*article}`. In termini logici la route `blog/search/{topic}` viene eseguita per prima, per impostazione predefinita, perché questo è l'ordine più plausibile. Usando il routing convenzionale, lo sviluppatore ordina le route in base alle proprie esigenze.

Nelle route con attributi si può configurare un ordine usando la proprietà `Order` di tutti gli attributi di route specificati dal framework. Le route vengono elaborate in base a un ordinamento crescente della proprietà `Order`. L'ordine predefinito è `0`. L'impostazione di una route usando `Order = -1` viene eseguita prima delle route per cui non è impostato un ordine. L'impostazione di una route usando `Order = 1` viene eseguito dopo l'ordinamento predefinito delle route.

> [!TIP]
> Evitare la dipendenza da `Order`. Se lo spazio URL richiede i valori in un ordine esplicito per indirizzare correttamente i dati, si può probabilmente creare confusione anche per i client. In genere il routing con attributi seleziona la route corretta con la corrispondenza di URL. Se l'ordine predefinito usato per la generazione di URL non funziona, usare il nome della route come override è in genere più semplice che applicare la proprietà `Order`.

Il routing di Razor Pages e il routing del controller MVC condividono un'implementazione. Per informazioni sull'ordine delle route negli argomenti di Razor Pages, vedere [Convenzioni di route e app per Razor Pages: Ordine delle route](xref:razor-pages/razor-pages-conventions#route-order).

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Sostituzione dei token nei modelli di route ([controller], [action], [area])

Per praticità, le route con attributi supportano la *sostituzione dei token* eseguita racchiudendo i token tra parentesi quadre (`[`, `]`). I token `[action]`, `[area]` e `[controller]` vengono sostituiti con i valori di nome dell'azione, nome dell'area e nome del controller dall'azione in cui è definita la route. Nell'esempio seguente le azioni corrispondono ai percorsi URL come descritto nei commenti:

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

La sostituzione dei token avviene come ultimo passaggio della creazione delle route con attributi. L'esempio precedente si comporterà come il codice seguente:

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Le route con attributi possono anche essere combinate con l'ereditarietà. Ciò risulta particolarmente efficace se combinato con la sostituzione dei token.

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPut("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

La sostituzione dei token si applica anche ai nomi di route definiti dalle route con attributi. `[Route("[controller]/[action]", Name="[controller]_[action]")]` genera un nome di route univoco per ogni azione.

Per verificare la corrispondenza del delimitatore letterale della sostituzione di token `[` o `]`, eseguirne l'escape ripetendo il carattere (`[[` o `]]`).

::: moniker range=">= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a>Usare un trasformatore di parametri per personalizzare la sostituzione dei token

La sostituzione dei token può essere personalizzata usando un trasformatore di parametri. Un trasformatore di parametri implementa `IOutboundParameterTransformer` e trasforma il valore dei parametri. Ad esempio, un trasformatore di parametri `SlugifyParameterTransformer` personalizzato cambia il valore di route `SubscriptionManagement` in `subscription-management`.

`RouteTokenTransformerConvention` è una convenzione del modello di applicazione che:

* Applica un trasformatore di parametri a tutte le route di attributi in un'applicazione.
* Personalizza i valori dei token delle route di attributi quando vengono sostituiti.

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

La convenzione `RouteTokenTransformerConvention` è registrata come un'opzione in `ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Conventions.Add(new RouteTokenTransformerConvention(
                                     new SlugifyParameterTransformer()));
    });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>Route multiple

Il routing con attributi supporta la definizione di più route che raggiungono la stessa azione. L'uso più comune è simulare il comportamento della *route convenzionale predefinita* come illustrato nell'esempio seguente:

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

Inserire più attributi di route nel controller significa che verranno tutti combinati con tutti gli attributi di route nei metodi dell'azione.

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

Quando più attributi di route (che implementano `IActionConstraint`) sono inseriti in un'azione, ogni vincolo dell'azione viene combinato con il modello di route dall'attributo che lo definisce.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> Benché l'uso di più route per le azioni possa sembrare efficace, è preferibile mantenere lo spazio URL dell'applicazione semplice e ben definito. Usare più route per le azioni solo se necessario, ad esempio per supportare i client esistenti.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Definizione di parametri facoltativi, valori predefiniti e vincoli della route con attributi

Le route con attributi supportano la stessa sintassi inline delle route convenzionali per specificare i parametri facoltativi, i valori predefiniti e i vincoli.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

Vedere [Riferimento per i modelli di route](../../fundamentals/routing.md#route-template-reference) per una descrizione dettagliata della sintassi del modello di route.

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Attributi di route personalizzati che usano `IRouteTemplateProvider`

Tutti gli attributi di route disponibili nel framework (`[Route(...)]`, `[HttpGet(...)]` e così via) implementano l'interfaccia `IRouteTemplateProvider`. MVC cerca gli attributi per le classi del controller e i metodi delle azioni all'avvio dell'app e usa quelli che implementano `IRouteTemplateProvider` per compilare il set iniziale di route.

È possibile implementare `IRouteTemplateProvider` per definire i propri attributi di route. Ogni `IRouteTemplateProvider` consente di definire una singola route con un modello di route, un ordinamento e un nome personalizzati:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

L'attributo dell'esempio precedente imposta automaticamente `Template` su `"api/[controller]"` quando si applica `[MyApiController]`.

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Uso del modello applicativo per personalizzare le route con attributi

Il *modello applicativo* è un modello a oggetti creato all'avvio con tutti i metadati usati da MVC per indirizzare ed eseguire le azioni. Il *modello applicativo* include tutti i dati raccolti da attributi di route (attraverso `IRouteTemplateProvider`). È possibile scrivere *convenzioni* per modificare il modello applicativo in fase di avvio per personalizzare il comportamento del routing. Questa sezione illustra un semplice esempio di personalizzazione del routing con il modello applicativo.

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Routing misto: routing con attributi e routing convenzionale

Le applicazioni MVC sono in grado di usare una combinazione di routing convenzionale e routing con attributi. In genere le route convenzionali vengono usate per i controller che gestiscono le pagine HTML per i browser e le route con attributi per i controller che gestiscono le API REST.

Le azioni vengono indirizzate in modo convenzionale o con attributi. Se una route viene inserita nel controller o nell'azione, viene indirizzata con attributi. Le azioni che definiscono le route con attributi non possono essere raggiunte usando le route convenzionali e viceversa. **Qualsiasi** attributo di route del controller rende tutte le azioni presenti nel controller indirizzate con attributi.

> [!NOTE]
> Ciò che distingue i due tipi di sistema di routing è il processo applicato dopo che si è verificata la corrispondenza di un URL con un modello di route. Nel routing convenzionale i valori di route della corrispondenza vengono usati per scegliere l'azione e il controller da una tabella di ricerca di tutte le azioni indirizzate in modo convenzionale. Nel routing con attributi ogni modello è già associato a un'azione e non è necessaria alcuna ulteriore ricerca.

## <a name="complex-segments"></a>Segmenti complessi

I segmenti complessi (ad esempio, `[Route("/dog{token}cat")]`), vengono elaborati individuando corrispondenze per i valori letterali da destra a sinistra in modalità non-greedy. Vedere [il codice sorgente](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) per una descrizione. Per altre informazioni, vedere [questo problema](https://github.com/aspnet/AspNetCore.Docs/issues/8197).

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>Generazione di URL

Le applicazioni MVC sono in grado di usare le funzionalità di generazione di URL del routing per generare i collegamenti URL alle azioni. La generazione di URL evita la codifica degli URL e consente di rendere il codice più affidabile e gestibile. Questa sezione illustra le funzionalità di generazione di URL disponibili in MVC e tratta solo le nozioni di base sul funzionamento della generazione di URL. Vedere [Routing](../../fundamentals/routing.md) per una descrizione dettagliata della generazione di URL.

L'interfaccia `IUrlHelper` è l'elemento sottostante dell'infrastruttura tra MVC e il routing per la generazione di URL. È possibile trovare un'istanza di `IUrlHelper` disponibile usando la proprietà `Url` in controller, visualizzazioni e componenti di visualizzazione.

In questo esempio l'interfaccia `IUrlHelper` viene usata attraverso la proprietà `Controller.Url` per generare un URL a un'altra azione.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Se l'applicazione usa la route convenzionale predefinita, il valore della variabile `url` sarà la stringa del percorso URL `/UrlGeneration/Destination`. Il percorso URL viene creato dal routing combinando i valori di route della richiesta corrente (valori di ambiente) con i valori passati a `Url.Action` e sostituendo tali valori nel modello di route:

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Il valore di ogni parametro di route del modello di route viene sostituito attraverso la corrispondenza dei nomi con i valori e i valori di ambiente. Per un parametro di route senza un valore si può usare un valore predefinito, se ne esiste uno, oppure ignorare il parametro se è facoltativo (come nel caso di `id` in questo esempio). La generazione di URL avrà esito negativo se un parametro di route obbligatorio non ha un valore corrispondente. Se la generazione di URL non riesce per una route, viene tentata la route successiva finché non vengono tentate tutte le route o viene trovata una corrispondenza.

Nell'esempio di `Url.Action` precedente si presuppone che il routing sia convenzionale, ma la generazione di URL funziona in modo analogo con il routing con attributi, sebbene i concetti siano differenti. Con il routing convenzionale i valori di route vengono usati per espandere un modello e i valori di route per `controller` e `action` in genere appaiono in quel modello. Questo procedimento funziona perché gli URL corrispondenti in base al routing rispettano una *convenzione*. Nel routing con attributi i valori di route per `controller` e `action` non possono essere visualizzati nel modello ma vengono usati per individuare il modello da usare.

Questo esempio usa il routing con attributi:

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC compila una tabella di ricerca di tutte le azioni indirizzate con attributi e stabilisce la corrispondenza dei valori `controller` e `action` per selezionare il modello di route da usare per la generazione di URL. Nell'esempio precedente viene generato `custom/url/to/destination`.

### <a name="generating-urls-by-action-name"></a>Generazione di URL in base al nome dell'azione

`Url.Action` (`IUrlHelper` . `Action`) e tutti gli overload correlati sono basati sull'idea che si voglia definire l'oggetto a cui ci si sta collegando specificando un nome di controller e un nome di azione.

> [!NOTE]
> Quando si usa `Url.Action`, i valori di route correnti per `controller` e `action` vengono specificati automaticamente: i valori di `controller` e `action` fanno parte sia dei *valori di ambiente* **sia** dei *valori*. Il metodo `Url.Action`, usa sempre i valori correnti di `action` e `controller` e genererà un percorso URL che indirizza all'azione corrente.

Il routing tenta di usare i valori nei valori di ambiente per ottenere le informazioni non specificate durante la generazione di un URL. Usando una route come `{a}/{b}/{c}/{d}` e i valori di ambiente `{ a = Alice, b = Bob, c = Carol, d = David }`, il routing ha informazioni sufficienti per generare un URL senza valori aggiuntivi, poiché tutti i parametri di route hanno un valore. Se è stato aggiunto il valore `{ d = Donovan }`, il valore `{ d = David }` viene ignorato e il percorso URL generato è `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> I percorsi URL sono gerarchici. Nell'esempio precedente, se è stato aggiunto il valore `{ c = Cheryl }`, entrambi i valori `{ c = Carol, d = David }` vengono ignorati. In questo caso non è più disponibile un valore per `d` e la generazione dell'URL avrà esito negativo. Sarà necessario specificare un valore per `c` e `d`.  In apparenza questo problema si può risolvere con la route predefinita (`{controller}/{action}/{id?}`), ma in realtà questo comportamento si verifica raramente perché `Url.Action` specifica sempre in modo esplicito un valore `controller` e `action`.

Anche gli overload di `Url.Action` di maggiore durata accettano anche un oggetto *valori di route* aggiuntivo per specificare i valori per i parametri di route diversi da `controller` e `action`. In genere viene usato con `id` come `Url.Action("Buy", "Products", new { id = 17 })`. Per convenzione l'oggetto *valori di route* è in genere un oggetto di tipo anonimo, ma può anche essere un `IDictionary<>` o un *normale oggetto .NET*. I valori di route aggiuntivi che non corrispondono a parametri di route vengono inseriti nella stringa di query.

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> Per creare un URL assoluto, usare un overload che accetta un `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>Generazione di URL in base alla route

Il codice precedente illustra come generare un URL passando il nome del controller e dell'azione. `IUrlHelper` specifica anche la famiglia di metodi `Url.RouteUrl`. Questi metodi sono simili a `Url.Action`, ma non copiano i valori correnti di `action` e `controller` nei valori di route. L'uso più comune consiste nello specificare un nome di route per usare un percorso specifico per generare l'URL, in genere *senza* specificare un nome di controller o azione.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>Generazione di URL in HTML

`IHtmlHelper` specifica i metodi `HtmlHelper` `Html.BeginForm` e `Html.ActionLink` per generare rispettivamente gli elementi `<form>` e `<a>`. Questi metodi usano il metodo `Url.Action` per generare un URL e accettano argomenti simili. Gli oggetti `Url.RouteUrl` complementi di `HtmlHelper` sono `Html.BeginRouteForm` e `Html.RouteLink` e hanno una funzionalità simile.

Gli helper tag generano gli URL attraverso l'helper tag `form` e l'helper tag `<a>`. Entrambi usano `IUrlHelper` per la propria implementazione. Per altre informazioni, vedere [Uso dei moduli](../views/working-with-forms.md).

All'interno delle visualizzazioni `IUrlHelper` è reso disponibile dalla proprietà `Url` per tutte le generazioni di URL ad hoc che non rientrano nelle situazioni descritte in precedenza.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>Generazione di URL nei risultati delle azioni

Negli esempi precedenti è stato usato `IUrlHelper` in un controller, ma l'uso più comune in un controller è generare un URL come parte del risultato di un'azione.

Le classi di base `ControllerBase` e `Controller` offrono metodi pratici per i risultati delle azioni che fanno riferimento a un'altra azione. Un uso tipico è eseguire il reindirizzamento dopo aver accettato l'input dell'utente.

```csharp
public IActionResult Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
    return View(customer);
}
```

I metodi factory dei risultati delle azioni seguono un modello simile ai metodi per `IUrlHelper`.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Caso speciale per le route convenzionali dedicate

Nel routing convenzionale è possibile usare un tipo speciale di definizione di route denominata *route convenzionale dedicata*. Nell'esempio seguente la route denominata `blog` è una route convenzionale dedicata.

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Usando queste definizioni di route, `Url.Action("Index", "Home")` genera il percorso URL `/` con la route `default` per un motivo ben preciso. Si potrebbe pensare che i valori di route `{ controller = Home, action = Index }` siano sufficienti per generare un URL usando `blog` e che il risultato sia `/blog?action=Index&controller=Home`.

Le route convenzionali dedicate si basano su un comportamento speciale dei valori predefiniti per i quali non esiste un parametro di route corrispondente che impedisce alla route di essere troppo "greedy" con la generazione degli URL. In questo caso i valori predefiniti sono `{ controller = Blog, action = Article }` e né `controller` né `action` vengono visualizzati come parametri di route. Quando il routing esegue la generazione di URL, i valori specificati devono corrispondere ai valori predefiniti. La generazione di URL che usa `blog` avrà esito negativo perché i valori `{ controller = Home, action = Index }` non corrispondono a `{ controller = Blog, action = Article }`. Il routing quindi esegue il fallback per provare `default`, che ha esito positivo.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Aree

Le [aree](areas.md) sono una funzionalità di MVC che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi di routing separato (per le azioni del controller) e struttura di cartelle (per le visualizzazioni). L'uso delle aree consente a un'applicazione di usare più controller con lo stesso nome, purché abbiano *aree* diverse. Usando le aree si crea una gerarchia per il routing aggiungendo un altro parametro di route, `area` a `controller` e `action`. Questa sezione illustra il modo in cui il routing interagisce con le aree. Vedere [Aree](areas.md) per informazioni dettagliate sull'uso delle aree con le visualizzazioni.

Nell'esempio seguente MVC viene configurato in modo da usare la route convenzionale predefinita e una *route di area* per un'area denominata `Blog`:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

Nella corrispondenza di un percorso URL come `/Manage/Users/AddUser` la prima route produrrà i valori di route `{ area = Blog, controller = Users, action = AddUser }`. Il valore di route `area` è generato da un valore predefinito per `area`, infatti la route creata da `MapAreaRoute` equivale alla seguente:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute` crea una route che usa sia un valore predefinito che un vincolo per `area` usando il nome di area specificato, in questo caso `Blog`. Il valore predefinito assicura che la route generi sempre `{ area = Blog, ... }`, il vincolo richiede il valore `{ area = Blog, ... }` per la generazione di URL.

> [!TIP]
> Il routing convenzionale dipende dall'ordinamento. In generale, le route con aree devono essere posizionate prima delle altre nella tabella di route poiché sono più specifiche rispetto alle route senza un'area.

Se si usa l'esempio precedente, i valori di route corrispondono all'azione seguente:

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute` è ciò che denota un controller come parte di un'area, questo controller in particolare si trova nell'area `Blog`. I controller senza un attributo `[Area]` non sono membri di alcuna area e **non** corrispondono quando il valore di route `area` viene specificato dal routing. Nell'esempio seguente solo il primo controller indicato può corrispondere ai valori di route `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> Lo spazio dei nomi di ogni controller viene indicato qui per motivi di completezza, per evitare conflitti di denominazione nei controller e la generazione di errori del compilatore. Gli spazi dei nomi delle classi non hanno effetto sul routing di MVC.

I primi due controller sono membri di aree e corrispondono solo quando viene specificato il relativo nome di area dal valore di route `area`. Il terzo controller non è un membro di un'area e può corrispondere solo quando non vengono specificati valori per `area` dal routing.

> [!NOTE]
> In termini di corrispondenza con *nessun valore*, l'assenza del valore `area` è come se il valore per `area` fosse Null o la stringa vuota.

Quando si esegue un'azione all'interno di un'area, il valore di route per `area` sarà disponibile come *valore di ambiente* per il routing da usare per la generazione di URL. Ciò significa che per impostazione predefinita le aree funzionano *temporaneamente* per la generazione di URL, come illustrato nell'esempio seguente.

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>Informazioni su IActionConstraint

> [!NOTE]
> Questa sezione offre un'analisi approfondita degli elementi interni del framework e del modo in cui MVC sceglie un'azione da eseguire. Un'applicazione tipica non richiede un oggetto `IActionConstraint` personalizzato

Probabilmente l'utente ha già usato `IActionConstraint` anche se non ha familiarità con l'interfaccia. L'attributo `[HttpGet]` e gli attributi `[Http-VERB]` dello stesso tipo implementano `IActionConstraint` per limitare l'esecuzione di un metodo di azione.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Se si usa la route convenzionale predefinita, il percorso URL `/Products/Edit` produce i valori `{ controller = Products, action = Edit }`, che corrispondono a **entrambe** le azioni illustrate qui. Nella terminologia di `IActionConstraint` si dice che entrambe le azioni sono considerate candidati poiché corrispondono entrambe ai dati della route.

Quando viene eseguito `HttpGetAttribute` indica che *Edit()* è una corrispondenza per *GET* e non è una corrispondenza per qualsiasi altro verbo HTTP. L'azione `Edit(...)` non ha tutti i vincoli definiti e quindi corrisponde a qualsiasi verbo HTTP. Presupponendo l'uso di un oggetto `POST`, corrisponde solo `Edit(...)`. Per un oggetto `GET` possono invece corrispondere entrambe le azioni, tuttavia un'azione con `IActionConstraint` viene sempre considerata *migliore* rispetto a un'azione senza tale oggetto. Quindi, dal momento che `Edit()` ha `[HttpGet]` è considerata più specifica e verrà selezionata se entrambe le azioni possono corrispondere.

Concettualmente, `IActionConstraint` è una forma di *overload*, ma anziché sostituire i metodi con lo stesso nome, supporta l'overload tra le azioni che corrispondono allo stesso URL. Anche il routing con attributi usa `IActionConstraint` e può accadere che azioni di controller diversi vengano entrambe considerate candidati.

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>Implementazione di IActionConstraint

Il modo più semplice di implementare un oggetto `IActionConstraint` è creare una classe derivata da `System.Attribute` e inserirla nelle azioni e nel controller. MVC rileverà automaticamente tutti gli oggetti `IActionConstraint` applicati come attributi. È possibile usare il modello di applicazione per applicare i vincoli e questo è probabilmente l'approccio più flessibile, poiché consente di metaprogrammare le modalità di applicazione.

Nell'esempio seguente un vincolo sceglie un'azione in base a un *codice paese* dai dati di route. Vedere l'[esempio completo su GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

L'utente è responsabile dell'implementazione del metodo `Accept` e della scelta di un "ordine" per il vincolo da eseguire. In questo caso il metodo `Accept` restituisce `true` per indicare che l'azione è una corrispondenza quando il valore di route `country` corrisponde. Questo comportamento è diverso da quello di `RouteValueAttribute` poiché viene consentito il fallback a un'azione senza attributi. L'esempio indica che se si definisce un'azione `en-US`, per il codice paese, ad esempio `fr-FR`, viene eseguito il fallback a un controller più generico a cui non è applicato `[CountrySpecific(...)]`.

La proprietà `Order` decide a quale *fase* appartiene il vincolo. I vincoli relativi alle azioni vengono eseguiti in gruppi basati su `Order`. Ad esempio, tutti gli attributi del metodo HTTP specificati dal framework usano lo stesso valore `Order` in modo da essere eseguiti nella stessa fase. È possibile usare un numero illimitato di fasi per implementare i criteri necessari.

> [!TIP]
> Quando si sceglie un valore per `Order` considerare se il vincolo dovrà essere applicato o meno prima dei metodi HTTP. I numeri più bassi vengono eseguiti per primi.
