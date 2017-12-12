---
title: Routing di azioni del Controller
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 26250a4d-bf62-4d45-8549-26801cf956e9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: d6e230351eb2f4c8549b54d75fd8e345718e6109
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2017
---
# <a name="routing-to-controller-actions"></a>Routing di azioni del Controller

Da [Ryan Nowak](https://github.com/rynowak) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Componenti di base di ASP.NET MVC utilizza il Routing [middleware](../../fundamentals/middleware.md) per associare gli URL delle richieste in ingresso e di eseguirne il mapping alle azioni. Le route sono definite nel codice di avvio o attributi. Le route viene descritto come i percorsi URL dovrebbero corrispondere alle azioni. Le route consentono inoltre di generare gli URL (per i collegamenti) inviati nelle risposte. 

Azioni di vengono tradizionalmente indirizzate o attributo indirizzato. Inserimento di una route del controller o l'azione rende attributo indirizzato. Vedere [mista routing](#routing-mixed-ref-label) per ulteriori informazioni.

Questo documento illustrerà le interazioni tra MVC e routing e come tipico assicurarsi di applicazioni MVC Usa le funzionalità di routine. Vedere [Routing](xref:fundamentals/routing) per ulteriori informazioni sul routing avanzata.

## <a name="setting-up-routing-middleware"></a>Configurazione di Routing Middleware

Nel *configura* metodo potrebbe apparire simile al codice:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Nella chiamata a `UseMvc`, `MapRoute` viene utilizzato per creare una singola route, si farà riferimento a come il `default` route. La maggior parte delle applicazioni MVC utilizzerà una route con un modello simile al `default` route.

Il modello di route `"{controller=Home}/{action=Index}/{id?}"` può corrispondere a un percorso URL come `/Products/Details/5` che consentirà di estrarre i valori della route `{ controller = Products, action = Details, id = 5 }` per il percorso di suddivisione in token. MVC tenterà di individuare un controller denominato `ProductsController` ed eseguire l'azione `Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Si noti che in questo esempio, l'associazione di modelli utilizzerà il valore di `id = 5` per impostare il `id` parametro `5` quando si richiama l'azione. Vedere il [associazione di modelli](../models/model-binding.md) per altri dettagli.

Utilizzo di `default` route:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Il modello di route:

* `{controller=Home}`definisce `Home` come impostazione predefinita`controller`

* `{action=Index}`definisce `Index` come impostazione predefinita`action`

* `{id?}`definisce `id` come facoltativi

Impostazione predefinita e i parametri di route facoltativo non è necessario trovarsi nel percorso URL per trovare una corrispondenza. Vedere [riferimento del modello di Route](../../fundamentals/routing.md#route-template-reference) per una descrizione dettagliata della sintassi di modello di route.

`"{controller=Home}/{action=Index}/{id?}"`può corrispondere al percorso di URL `/` e restituirà i valori della route `{ controller = Home, action = Index }`. I valori per `controller` e `action` rendere utilizzare i valori predefiniti, `id` non produce un valore perché è presente alcun segmento corrispondente nel percorso URL. MVC utilizzare questi valori di route per selezionare il `HomeController` e `Index` azione:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Usare questa definizione di controller e il modello di route, il `HomeController.Index` azione verrà eseguita per uno o più percorsi URL seguenti:

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

Il metodo praticità `UseMvcWithDefaultRoute`:

```csharp
app.UseMvcWithDefaultRoute();
```

Può essere utilizzato per sostituire:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc`e `UseMvcWithDefaultRoute` aggiungere un'istanza di `RouterMiddleware` la pipeline middleware. MVC non interagisce direttamente con middleware e utilizza il routing per gestire le richieste. MVC è connesso a tutte le route tramite un'istanza di `MvcRouteHandler`. Il codice all'interno di `UseMvc` è simile al seguente:

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc`non definire direttamente le route, viene aggiunto un segnaposto per la raccolta di route per il `attribute` route. L'overload `UseMvc(Action<IRouteBuilder>)` consente di aggiungere route personalizzati e supporta anche l'attributo di routing.  `UseMvc`e le sue varianti viene aggiunto un segnaposto per la route di attributo: routing degli attributi è sempre disponibile indipendentemente dalla modalità di configurazione `UseMvc`. `UseMvcWithDefaultRoute`definisce una route predefinita e supporta il routing di attributo. Il [attributo Routing](#attribute-routing-ref-label) sono incluse ulteriori informazioni sul routing degli attributi.

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>Il routing convenzionale

Il `default` route:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

è un esempio di un *routing convenzionale*. Viene chiamato questo stile *routing convenzionale* poiché stabilisce un *convenzione* per i percorsi URL:

* il primo segmento del percorso viene eseguito il mapping al nome del controller

* il secondo esegue il mapping per il nome dell'azione.

* il terzo segmento viene utilizzato per un parametro facoltativo `id` utilizzato per eseguire il mapping a un'entità del modello

Usando questa `default` route, il percorso URL `/Products/List` esegue il mapping al `ProductsController.List` action e `/Blog/Article/17` esegue il mapping a `BlogController.Article`. Questo mapping si basa sui nomi di azione e del controller **solo** e non si basa su spazi dei nomi, i percorsi dei file di origine o i parametri del metodo.

> [!TIP]
> Utilizzo convenzionale di routing con la route predefinita consente di compilare rapidamente l'applicazione senza che sia necessario ideare un nuovo modello di URL per ogni azione che si definisce. Per un'applicazione con azioni di stile CRUD, con verifica coerenza per gli URL tra i controller consente di semplificare il codice e rendere l'interfaccia utente più prevedibile.

> [!WARNING]
> Il `id` definiti come facoltativi per il modello di route, vale a dire che le azioni possono essere eseguiti senza l'ID fornito come parte dell'URL. In genere cosa succede se `id` viene omesso dall'URL che verrà impostato su `0` dall'associazione del modello e di conseguenza verrà rilevata alcuna entità nei database di ricerca `id == 0`. Routing degli attributi consente di visualizzare un controllo accurato per rendere l'ID obbligatorio per alcune azioni e non per altri utenti. Per convenzione includerà la documentazione di parametri facoltativi come `id` quando vengono maggiore probabilità di comparire in uso corretto.

## <a name="multiple-routes"></a>Più route

È possibile aggiungere più route all'interno di `UseMvc` aggiungendo ulteriori chiamate a `MapRoute`. In questo modo consente di definire più convenzioni, o aggiungere le route convenzionale dedicati a un'azione specifica, ad esempio:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Il `blog` route qui è un *dedicato route convenzionale*, vale a dire che utilizza il sistema di routing tradizionale, ma è dedicato a un'azione specifica. Poiché `controller` e `action` non vengono visualizzati nel modello di route come parametri, possono avere solo i valori predefiniti e pertanto questa route verrà sempre eseguito il mapping all'azione `BlogController.Article`.

Le route nella raccolta di route vengono ordinate e verranno elaborate nell'ordine in che cui vengono aggiunti. In tal caso, in questo esempio, il `blog` route verrà tentate prima il `default` route.

> [!NOTE]
> *Dedicato route convenzionale* utilizzano spesso parametri di route catch-all come `{*article}` per acquisire la parte rimanente del percorso dell'URL. In questo modo una route 'troppo greedy' vale a dire che corrisponda agli URL che si deve essere accompagnato da altre route. Inserire le route "greedy" più avanti in questa tabella di route per risolvere il problema.

### <a name="fallback"></a>Fallback

Come parte dell'elaborazione della richiesta, MVC verificherà i valori della route possono essere utilizzati per trovare un controller e l'azione nell'applicazione. Se i valori di route non corrispondano a un'azione quindi la route non viene considerata una corrispondenza e la successiva route verrà tentata. Si tratta di *fallback*, e è progettato per semplificare i casi in cui si sovrappongono route convenzionale.

### <a name="disambiguating-actions"></a>Per evitare ambiguità tra azioni

Quando due azioni corrispondono dal routing, è necessario evitare ambiguità MVC per scegliere il candidato 'migliore' o in caso contrario, genera un'eccezione. Ad esempio:

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Il controller definisce due azioni corrispondenti il percorso URL `/Products/Edit/17` e l'invio di dati `{ controller = Products, action = Edit, id = 17 }`. Si tratta di un modello tipico per i controller MVC in `Edit(int)` viene illustrato un form per modificare un prodotto, e `Edit(int, Product)` elabora il modulo registrato. A tale scopo sarà necessario scegliere MVC `Edit(int, Product)` se la richiesta è HTTP `POST` e `Edit(int)` quando il verbo HTTP è diversa.

Il `HttpPostAttribute` ( `[HttpPost]` ) è un'implementazione di `IActionConstraint` che consentirà solo l'azione che deve essere selezionato quando il verbo HTTP è `POST`. La presenza di un `IActionConstraint` rende il `Edit(int, Product)` 'meglio' corrisponde a `Edit(int)`, pertanto `Edit(int, Product)` verranno provate prima.

È necessario scrivere personalizzato `IActionConstraint` implementazioni specializzati scenari, ma di importanti per comprendere il ruolo di attributi quali `HttpPostAttribute` -definiti attributi simili per altri verbi HTTP. Nel routing convenzionale è comune per le azioni da utilizzare lo stesso nome di azione quando sono parte di un `show form -> submit form` flusso di lavoro. Il vantaggio di questo modello verrà diventano più evidente dopo aver esaminato il [IActionConstraint comprensione](#understanding-iactionconstraint) sezione.

Se più route corrispondano e MVC non è possibile trovare una route 'le', verrà generata una `AmbiguousActionException`.

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>Nomi di route

Le stringhe `"blog"` e `"default"` negli esempi seguenti sono i nomi di route:


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

I nomi delle route dare la route un nome logico, in modo che la route denominata può essere utilizzata per la generazione di URL. Questo semplifica notevolmente la creazione di URL durante l'ordinamento delle route può rendere complessa la generazione di URL. I nomi di route devono essere univoci a livello di applicazione.

I nomi di route non avere un impatto sull'URL corrispondente o la gestione di richieste. vengono utilizzati solo per la generazione di URL. [Routing](xref:fundamentals/routing) maggiori informazioni sulla generazione di URL incluso generazione URL helper MVC specifico.

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>Routing degli attributi

Attributo routing utilizza un set di attributi per eseguire il mapping di azioni direttamente ai modelli di route. Nell'esempio seguente, `app.UseMvc();` viene utilizzata la `Configure` (metodo) e nessuna route viene passata. Il `HomeController` corrisponderà a un set di URL simile al quale la route predefinita `{controller=Home}/{action=Index}/{id?}` corrisponderebbe:

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

Il `HomeController.Index()` azione verrà eseguita per tutti i percorsi URL `/`, `/Home`, o `/Home/Index`.

> [!NOTE]
> In questo esempio evidenzia una chiave programmazione differenza routing degli attributi e il routing convenzionale. Routing degli attributi richiede più input per specificare una route; la route predefinita convenzionale gestisce le route più conciso. Tuttavia, routing degli attributi consente e richiede un controllo preciso dei quali modelli route si applicano a ogni azione.

Con il nome del controller e i nomi di azione di routing attributo riprodurre **non** ruolo in cui è selezionata l'azione. In questo esempio corrisponderà gli stessi URL dell'esempio precedente.

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
> I modelli di route precedenti non definiscono i parametri di route per `action`, `area`, e `controller`. In realtà, questi parametri di route non sono consentiti in route di attributi. Poiché il modello di route è già associato a un'azione, il risulterebbero utili per analizzare il nome dell'azione dall'URL.

## <a name="attribute-routing-with-httpverb-attributes"></a>Attributo di routing con attributi Http [verbo]

Routing degli attributi può inoltre effettuare usano il `Http[Verb]` attributi quali `HttpPostAttribute`. Tutti questi attributi possono accettare un modello di route. Questo esempio mostra due azioni che corrispondono al modello di route stesso:

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

Per un percorso URL come `/products` il `ProductsApi.ListProducts` azione verrà eseguita quando il verbo HTTP è `GET` e `ProductsApi.CreateProduct` verrà eseguito quando il verbo HTTP è `POST`. Attributo di routing corrisponde all'URL rispetto al set di modelli di route definite dagli attributi di route. Una volta corrisponde a un modello di route, `IActionConstraint` i vincoli vengono applicati per determinare le azioni che possono essere eseguite.

> [!TIP]
> Quando si compila un'API REST, è raro che è consigliabile usare `[Route(...)]` su un metodo di azione. Si consiglia di utilizzare più specifica `Http*Verb*Attributes` precisa su quelle supportate dall'API. I client di API REST devono sapere quali verbi HTTP e percorsi di eseguire il mapping a specifiche operazioni logiche.

Poiché una route di attributo si applica a un'azione specifica, è ideale per ottenere i parametri richiesti come parte della definizione del modello di route. In questo esempio, `id` è necessario come parte del percorso dell'URL.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Il `ProductsApi.GetProduct(int)` azione verrà eseguita per un percorso URL come `/products/3` ma non per un percorso URL come `/products`. Vedere [Routing](../../fundamentals/routing.md) per una descrizione completa dei modelli di route e le opzioni correlate.

## <a name="route-name"></a>Nome di route

Il codice seguente definisce un *nome di route* di `Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

I nomi di route è utilizzabile per generare un URL basato su un percorso specifico. I nomi di route non avere un impatto sull'URL di comportamento di routing di corrispondenza e vengono usati solo per la generazione di URL. I nomi di route devono essere univoci a livello di applicazione.

> [!NOTE]
> Ciò si differenzia convenzionali *route predefinita*, che definisce il `id` parametro come facoltativi (`{id?}`). La possibilità di specificare in modo preciso le API presenta vantaggi, ad esempio per consentire `/products` e `/products/5` deve essere inviato a diverse azioni.

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>Route di combinazione

Affinché l'attributo routing meno ricorrenti, attributi delle route sul controller vengono combinati con attributi di route per le singole azioni. I modelli di route definiti nel controller vengono anteposti ai modelli di route alle azioni. Inserendo un attributo della route nel controller di **tutti** azioni nel controller di utilizzano il routing di attributo.

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

In questo esempio il percorso URL `/products` può corrispondere a `ProductsApi.ListProducts`e il percorso URL `/products/5` può corrispondere a `ProductsApi.GetProduct(int)`. Entrambe le azioni corrisponde solo HTTP `GET` perché sono contrassegnati con il `HttpGetAttribute`.

Distribuire modelli applicati a un'azione che iniziano con un `/` non combinare con i modelli di route applicati al controller. In questo esempio corrisponde a un set di percorsi URL simili a quelli di *route predefinita*.

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Does not combine, defines the route template ""
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

### <a name="ordering-attribute-routes"></a>Route di attributi di ordinamento

A differenza delle route convenzionale che vengono eseguiti in un ordine definito, routing degli attributi viene compilato un albero e corrisponde a tutte le route contemporaneamente. Si comporta come-se le voci della route inserite in un ordinamento ideale; le route più specifiche hanno la possibilità di eseguire prima le route più generale.

Ad esempio, una route come `blog/search/{topic}` è più specifico di una route come `blog/{*article}`. In termini logici di `blog/search/{topic}` route 'esegue' in primo luogo, per impostazione predefinita, poiché questo è l'ordinamento solo ragionevoli. Utilizza il routing convenzionale, lo sviluppatore è responsabile dell'immissione delle route nell'ordine desiderato.

Route di attributi è possono configurare un ordine, utilizzando il `Order` proprietà di tutti gli attributi di route framework fornito. Le route vengono elaborate in base a un ordinamento crescente del `Order` proprietà. L'ordine predefinito è `0`. Impostazione di una route utilizzando `Order = -1` verrà eseguito prima che le route che non impostano un ordine. Impostazione di una route utilizzando `Order = 1` verrà eseguito dopo l'ordinamento predefinito route.

> [!TIP]
> Evitare in base alle `Order`. Se lo spazio di URL richiede valori di ordine espliciti per indirizzare correttamente, è probabilmente confusione anche ai client. In generale attributo routing selezionerà corretta route con URL corrispondente. Se l'ordine predefinito utilizzato per la generazione di URL non funziona, utilizzando il nome della route come una sostituzione è in genere più semplice rispetto all'applicazione il `Order` proprietà.

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Token di sostituzione nei modelli di route ([controller], [azione], [area])

Per praticità, route di attributi supportano *sostituzione del token* da un token di inclusione tra parentesi graffe di quadrati (`[`, `]`). I token `[action]`, `[area]`, e `[controller]` verrà sostituito con i valori del nome dell'azione, nome dell'area e nome del controller dall'azione in cui è definita la route. In questo esempio le azioni possono corrispondere i percorsi URL come descritto nei commenti:

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

Sostituzione dei token si verifica come ultimo passaggio della creazione di route di attributi. L'esempio precedente si comporterà come il codice seguente:

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Route di attributi possono anche essere combinate con ereditarietà. Questo è particolarmente efficace combinato con la sostituzione dei token.

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

Sostituzione dei token si applica anche ai nomi di route definiti per una route di attributi. `[Route("[controller]/[action]", Name="[controller]_[action]")]`verrà generato un nome di route univoca per ogni azione.

In modo che corrisponda al delimitatore valore letterale per la sostituzione dei token `[` o `]`, ripetendo il carattere di escape (`[[` o `]]`).

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>Più route

Attributo di routing supporta la definizione di più route che raggiungono la stessa azione. L'utilizzo più comune di questo oggetto è per simulare il comportamento del *route convenzionale predefinita* come illustrato nell'esempio seguente:

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

Inserimento di più attributi delle route nel controller significa che ognuno di essi verrà combinati con gli attributi delle route in metodi di azione.

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

Quando più attributi delle route (che implementano `IActionConstraint`) si trovano in un'azione, quindi ogni vincolo azione combina con il modello di route dall'attributo che lo definisce.

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
> Utilizzo di più route in azioni può sembrare potente, è preferibile mantenere spazio URL dell'applicazione semplice e ben definito. Utilizzare più route azioni solo in caso di necessità, ad esempio per supportare i client esistenti.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Specifica di parametri facoltativi route di attributo, i valori predefiniti e vincoli

Route di attributi supportano la stessa sintassi inline come route convenzionale per specificare i parametri facoltativi, i valori predefiniti e vincoli.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

Vedere [riferimento del modello di Route](../../fundamentals/routing.md#route-template-reference) per una descrizione dettagliata della sintassi di modello di route.

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Attributi di route personalizzati utilizzando`IRouteTemplateProvider`

Tutti gli attributi di route disponibili in framework ( `[Route(...)]`, `[HttpGet(...)]` e così via) implementare il `IRouteTemplateProvider` interfaccia. MVC esegue la ricerca per gli attributi nelle classi controller e metodi di azione quando l'app viene avviata e utilizza quelle che implementano `IRouteTemplateProvider` per compilare il set iniziale di route.

È possibile implementare `IRouteTemplateProvider` per definire gli attributi della route. Ogni `IRouteTemplateProvider` consente di definire una singola route con un modello di route personalizzati, ordinamento e assegnare un nome:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

Imposta automaticamente l'attributo dell'esempio precedente il `Template` a `"api/[controller]"` quando `[MyApiController]` viene applicato.

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Utilizzando il modello di applicazione per personalizzare le route di attributo

Il *modello applicativo* è un modello a oggetti creato all'avvio con tutti i metadati utilizzati da MVC per distribuire ed eseguire le azioni. Il *modello applicativo* include tutti i dati raccolti da attributi delle route (tramite `IRouteTemplateProvider`). È possibile scrivere *convenzioni* per modificare il modello di applicazione in fase di avvio per personalizzare il comportamento di routing. In questa sezione illustra un semplice esempio di personalizzazione di routing tramite il modello di applicazione.

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Mista routing: attributo vs routing il routing convenzionale

Le applicazioni MVC è possono combinare l'utilizzo di routing convenzionale e routing degli attributi. È in genere utilizzare convenzionale route per i controller di servire le pagine HTML per i browser e routing per controller serve le API REST dell'attributo.

Azioni di vengono tradizionalmente indirizzate o attributo indirizzato. Inserimento di una route del controller o l'azione rende attributo indirizzato. Le azioni che definiscono le route di attributo non è raggiungibile tramite la route convenzionale e viceversa. **Qualsiasi** degli attributi delle route nel controller rende tutte le azioni nell'attributo controller indirizzato.

> [!NOTE]
> Ciò che distingue i due tipi di sistemi di routing è il processo applicato dopo un URL corrisponde a un modello di route. Nel routing convenzionale, vengono utilizzati i valori della route da quella per scegliere l'azione e il controller da una tabella di ricerca di tutte le azioni indirizzate convenzionale. Routing degli attributi, ogni modello è già associata a un'azione, in non è necessaria alcuna ulteriore ricerca.

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>Generazione di URL

Le applicazioni MVC è possono utilizzare funzionalità di generazione di URL del routing per generare URL i collegamenti alle azioni. Generazione degli URL Elimina URL hardcoded, rendere il codice più affidabile e gestibile. In questa sezione vengono illustrate le funzionalità di generazione di URL fornite da MVC e riguarderà solo nozioni di base del funzionamento della generazione di URL. Vedere [Routing](../../fundamentals/routing.md) per una descrizione dettagliata della generazione di URL.

Il `IUrlHelper` interfaccia è l'elemento sottostante dell'infrastruttura tra MVC e routing per la generazione di URL. È possibile trovare un'istanza di `IUrlHelper` disponibili tramite il `Url` proprietà nel controller, visualizzazioni e i componenti di visualizzazione.

In questo esempio, il `IUrlHelper` viene utilizzata l'interfaccia tramite la `Controller.Url` proprietà per generare un URL a un'altra azione.

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Se l'applicazione usa il valore predefinito di convenzionale route, il valore di `url` variabile corrisponderà alla stringa di percorso URL `/UrlGeneration/Destination`. Il percorso dell'URL viene creato dal routing combinando i valori della route della richiesta corrente (valori di ambiente), con i valori passati a `Url.Action` e sostituendo i valori nel modello di route:

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Ogni parametro di route nel modello di route ha valore sostituito da nomi corrispondenti con i valori e ambiente. Un parametro di route che non dispone di un valore è possibile utilizzare un valore predefinito se dispone di uno o essere ignorato se è facoltativo (come nel caso di `id` in questo esempio). Generazione URL avrà esito negativo se qualsiasi parametro di route richiesto non dispone di un valore corrispondente. Se la generazione di URL non riesce per una route, viene tentata la successiva route fino a quando non sono stati provati tutti i cicli o viene trovata una corrispondenza.

Nell'esempio di `Url.Action` sopra presuppone il routing convenzionale, ma URL generazione funziona allo stesso modo con il routing di attributo, sebbene i concetti sono diversi. Con il routing convenzionale, i valori della route vengono utilizzati per estendere un modello e i valori di route per `controller` e `action` in genere vengono incluse nel modello - questo procedimento funziona perché l'URL corrispondente al routing rispettare un *convenzione*. Nel routing degli attributi, i valori della route per `controller` e `action` non potranno essere visualizzati nel modello, vengono invece utilizzati per cercare il modello da utilizzare.

Questo esempio viene utilizzato l'attributo di routing:

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC compila una tabella di ricerca di tutte le azioni attributo indirizzato e corrisponderà il `controller` e `action` i valori per selezionare il modello di route da utilizzare per la generazione di URL. Nell'esempio precedente, `custom/url/to/destination` viene generato.

### <a name="generating-urls-by-action-name"></a>Generazione degli URL in base al nome di azione

`Url.Action` (`IUrlHelper` . `Action`) e tutti i relativi overload tutti sono basati su tale concetto che si desidera specificare ciò che sta collegando a specificando un nome del controller e il nome dell'azione.

> [!NOTE]
> Quando si utilizza `Url.Action`, i valori della route corrente per `controller` e `action` specificati per il valore è - `controller` e `action` fanno parte di entrambi *ambiente valori* **e** *valori*. Il metodo `Url.Action`, Usa sempre i valori correnti delle `action` e `controller` e genererà un percorso di URL che esegue il routing per l'azione corrente.

Tentativo di utilizzare i valori nei valori di ambiente per ottenere informazioni che non sono stati forniti durante la generazione di un URL di routing. Utilizzo di una route come `{a}/{b}/{c}/{d}` e i valori di ambiente `{ a = Alice, b = Bob, c = Carol, d = David }`, routing disponga di informazioni sufficienti per generare un URL senza valori aggiuntivi: poiché indirizzare tutti i parametri sono un valore. Se è stato aggiunto il valore `{ d = Donovan }`, il valore `{ d = David }` verrà ignorato e il percorso URL generato sarà `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> I percorsi URL sono gerarchici. Nell'esempio precedente, se è stato aggiunto il valore `{ c = Cheryl }`, entrambi i valori `{ c = Carol, d = David }` verrà ignorato. In questo caso non è più disponibile un valore per `d` e generazione di URL avrà esito negativo. È necessario specificare il valore desiderato di `c` e `d`.  Si potrebbe aspettare di passaggi di questo problema con la route predefinita (`{controller}/{action}/{id?}`)- ma si verifica raramente ciò in pratica come `Url.Action` specificherà sempre in modo esplicito un `controller` e `action` valore.

Più overload di `Url.Action` accettano anche un ulteriore *i valori di route* oggetto per fornire valori per parametri di route diverso da `controller` e `action`. In genere visualizzato utilizzata con `id` come `Url.Action("Buy", "Products", new { id = 17 })`. Per convenzione il *i valori di route* è in genere un oggetto di tipo anonimo, ma può anche essere un `IDictionary<>` o *normale vecchio oggetto .NET*. I valori di route aggiuntive che non corrispondono a parametri di route vengono inseriti nella stringa di query.

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> Per creare un URL assoluto, usare un overload che accetta un `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>Generazione degli URL route

Il codice precedente è stato illustrato generazione di un URL passando il nome di azione e del controller. `IUrlHelper`fornisce inoltre il `Url.RouteUrl` famiglia di metodi. Questi metodi sono simili a `Url.Action`, ma non copiano i valori correnti delle `action` e `controller` per i valori della route. L'utilizzo più comune consiste nello specificare un nome di route da utilizzare un percorso specifico per generare l'URL, in genere *senza* specificando un nome di azione o controller.

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>Generazione degli URL in formato HTML

`IHtmlHelper`fornisce il `HtmlHelper` metodi `Html.BeginForm` e `Html.ActionLink` per generare `<form>` e `<a>` elementi rispettivamente. Utilizzano questi metodi di `Url.Action` metodo per generare un URL e accettino argomenti simili. Il `Url.RouteUrl` se utilizzati insieme per `HtmlHelper` sono `Html.BeginRouteForm` e `Html.RouteLink` che hanno una funzionalità simile.

TagHelpers generare URL tramite il `form` helper tag e `<a>` helper tag. Entrambi questi utilizzare `IUrlHelper` per la relativa implementazione. Vedere [utilizzo dei moduli](../views/working-with-forms.md) per ulteriori informazioni.

All'interno delle viste, le `IUrlHelper` è disponibile tramite il `Url` proprietà per tutte le generazioni di URL ad hoc non coperto da sopra indicate.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>Generazione degli URL nei risultati di azione

Gli esempi precedenti sono state illustrate utilizzando `IUrlHelper` in un controller, mentre l'utilizzo più comune in un controller consiste nel generare un URL come parte del risultato di un'azione.

Il `ControllerBase` e `Controller` classi di base forniscono metodi pratici per i risultati dell'azione che fanno riferimento a un'altra azione. Un utilizzo tipico consiste nel reindirizzamento dopo aver accettato l'input dell'utente.

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

I metodi di azione risultati factory seguono un modello simile ai metodi `IUrlHelper`.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Caso speciale per le route convenzionale dedicate

Il routing convenzionale è possibile utilizzare un tipo speciale di definizione di una route denominata un *route convenzionale dedicato*. Nell'esempio seguente, la route denominata `blog` sia una route convenzionale dedicata.

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Utilizzo di queste definizioni di route, `Url.Action("Index", "Home")` genererà il percorso URL `/` con il `default` route, ma il motivo? Si potrebbero immaginare i valori della route `{ controller = Home, action = Index }` sarebbero sufficienti per generare un URL utilizzando `blog`, e il risultato sarebbe `/blog?action=Index&controller=Home`.

Le route convenzionale dedicate si basano su un comportamento speciale dei valori predefiniti che non dispongono di un parametro di route corrispondenti che impedisce la route "troppo greedy" con la generazione di URL. In questo caso i valori predefiniti sono `{ controller = Blog, action = Article }`e né `controller` né `action` viene visualizzato come un parametro di route. Quando il routing esegue la generazione di URL, i valori forniti devono corrispondere i valori predefiniti. Generazione di URL utilizzando `blog` avrà esito negativo perché i valori `{ controller = Home, action = Index }` non corrispondono `{ controller = Blog, action = Article }`. Routing quindi esegue il fallback per provare `default`, che ha esito positivo.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Aree

[Aree](areas.md) sono una funzionalità MVC consentono di organizzare la funzionalità correlata in un gruppo come una struttura di cartelle (per le viste) e il routing-spazio dei nomi separato (per le azioni del controller). Utilizzo di aree consente a più controller con lo stesso nome, un'applicazione purché hanno diversi *aree*. Utilizzo di aree consente di creare una gerarchia allo scopo di routing mediante l'aggiunta di un altro parametro di route, `area` a `controller` e `action`. In questa sezione verrà illustrate le modalità routing interagisce con aree - vedere [aree](areas.md) per informazioni dettagliate sull'utilizzo delle aree con le visualizzazioni.

Nell'esempio seguente configura MVC per l'utilizzo convenzionale route predefinita e un *route area* per un'area denominata `Blog`:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

In caso di corrispondenza di un percorso URL come `/Manage/Users/AddUser`, la prima route produrrà i valori della route `{ area = Blog, controller = Users, action = AddUser }`. Il `area` valore route è generato da un valore predefinito per `area`, in realtà la route creata `MapAreaRoute` equivale alla seguente:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute`Crea una route che utilizza un valore predefinito e vincolo per `area` utilizzando il nome dell'area specificata, in questo caso `Blog`. Il valore predefinito assicura che la route produce sempre `{ area = Blog, ... }`, che richiede il valore `{ area = Blog, ... }` per la generazione di URL.

> [!TIP]
> Il routing convenzionale è dipendente dall'ordinamento. In generale, le route delle aree devono trovarsi in precedenza nella tabella di routing come se fossero più specifiche rispetto alle route senza un'area.

Utilizzando l'esempio precedente, i valori della route corrisponderebbe l'azione seguente:

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

Il `AreaAttribute` è ciò che denota un controller come parte di un'area, si supponga che il controller è nel `Blog` area. Controller senza un `[Area]` attributo non sono membri di qualsiasi area e verrà **non** corrisponde a quando il `area` viene fornito il valore di route dal routing. Nell'esempio seguente, solo il primo controller elencati può corrispondere i valori della route `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> Lo spazio dei nomi di ogni controller è illustrato di seguito per motivi di completezza, in caso contrario i controller siano presenti una denominazione in conflitto e generare un errore del compilatore. Spazi dei nomi di classe non hanno effetto sul routing di MVC.

I primi due controller sono membri di aree e corrispondenza solo quando viene fornito il proprio nome di ciascuna area per il `area` indirizzare valore. Il terzo controller non è un membro di qualsiasi area e può essere la corrispondenza solo quando nessun valore per `area` viene fornito dal routing.

> [!NOTE]
> In termini di ricerca *alcun valore*, l'assenza del `area` valore è lo stesso come se il valore per `area` sono null o una stringa vuota.

Quando si esegue un'azione all'interno di un'area, il valore per la route `area` saranno disponibili come un *valore ambiente* per il routing per l'utilizzo per la generazione di URL. Ciò significa che per impostazione predefinita le aree di agire *sticky* per la generazione di URL, come illustrato nell'esempio seguente.

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>Informazioni sui IActionConstraint

> [!NOTE]
> In questa sezione è un'analisi approfondita su elementi interni di framework e come MVC sceglie un'azione da eseguire. Una tipica applicazione non è necessario un oggetto personalizzato`IActionConstraint`

Probabilmente è già utilizzato `IActionConstraint` anche se non si ha familiarità con l'interfaccia. Il `[HttpGet]` attributo e così via `[Http-VERB]` implementano `IActionConstraint` per limitare l'esecuzione di un metodo di azione.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Presupponendo che la route predefinita convenzionale, il percorso URL `/Products/Edit` produrrebbe i valori `{ controller = Products, action = Edit }`, quale corrisponderebbe **entrambi** delle azioni illustrate di seguito. In `IActionConstraint` terminologia si direbbe che entrambe le azioni sono considerati candidati - come corrispondano i dati della route.

Quando il `HttpGetAttribute` esegue, indicherà che *Edit()* è una corrispondenza per *ottenere* e non è una corrispondenza per qualsiasi altro verbo HTTP. Il `Edit(...)` azione non dispone di tutti i vincoli definiti e pertanto corrisponderà a qualsiasi verbo HTTP. Pertanto, presupponendo che un `POST` - solo `Edit(...)` corrispondente. Tuttavia, per un `GET` entrambe le azioni possono tuttavia corrispondere -, tuttavia, un'azione con un `IActionConstraint` viene sempre considerata *migliori* rispetto a un'azione senza. Pertanto, perché `Edit()` è `[HttpGet]` è considerato più specifica e verrà selezionato se possono corrispondere a entrambe le azioni.

Concettualmente, `IActionConstraint` è una forma di *overload*, ma anziché l'overload dei metodi con lo stesso nome, è l'overload tra le azioni che corrispondono all'URL stesso. Attributo di routing utilizza inoltre `IActionConstraint` e possono causare azioni dai diversi controller sia presa in considerazione candidati.

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>Implementazione IActionConstraint

Il modo più semplice per implementare un `IActionConstraint` consiste nel creare una classe derivata da `System.Attribute` e posizionarlo sul controller e azioni. MVC rileverà automaticamente qualsiasi `IActionConstraint` che vengono applicati come attributi. È possibile utilizzare il modello di applicazione per applicare vincoli e questo problema è probabilmente l'approccio più flessibile, in quanto consente a metaprogram come vengono applicati.

Nell'esempio seguente un vincolo sceglie un'azione in base a un *codice paese* dai dati di route. Il [esempio completo su GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

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

Si è responsabili dell'implementazione di `Accept` (metodo) e scegliendo un 'Order' per il vincolo da eseguire. In questo caso, il `Accept` restituisce `true` per indicare l'azione è una corrispondenza quando il `country` valore corrispondenze di route. Questo comportamento è diverso da un `RouteValueAttribute` nel senso che consente il fallback a un'azione senza attributi. L'esempio mostra che se si definisce un `en-US` azione quindi un codice paese, ad esempio `fr-FR` eseguirà il fallback a un controller più generico che non dispone di `[CountrySpecific(...)]` applicato.

Il `Order` proprietà decide quali *fase* il vincolo fa parte di. Vincoli relativi alle azioni eseguite in gruppi in base il `Order`. Ad esempio, tutti i framework forniti gli attributi del metodo HTTP utilizzano la stessa `Order` valore in modo che eseguano la stessa fase. È possibile avere altre fasi necessarie implementare i criteri desiderati.

> [!TIP]
> Per scegliere un valore per `Order` considerare o meno il vincolo deve essere applicato prima di metodi HTTP. I numeri più bassi eseguiti per primi.
