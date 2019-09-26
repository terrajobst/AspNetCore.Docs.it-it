---
title: Routing in ASP.NET Core
author: rick-anderson
description: Informazioni su come il routing di ASP.NET Core è responsabile del mapping degli URI delle richieste nei selettori degli endpoint e dell'invio delle richieste agli endpoint.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/24/2019
uid: fundamentals/routing
ms.openlocfilehash: 14fe4bf9266ec6b2d588fa26d7a460531e33aa30
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/26/2019
ms.locfileid: "71295502"
---
# <a name="routing-in-aspnet-core"></a>Routing in ASP.NET Core

Di [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/) e [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Il routing è responsabile del mapping degli URI di richiesta agli endpoint e dell'invio di richieste in ingresso a tali endpoint. Le route sono definite nell'app e vengono configurate all'avvio dell'app. Una route può facoltativamente estrarre valori dall'URL contenuto nella richiesta e questi valori possono quindi essere usati per l'elaborazione della richiesta. Usando le informazioni sulla route dall'app, il routing è anche in grado di generare URL con mapping agli endpoint.

> [!IMPORTANT]
> Questo documento descrive il routing di basso livello di ASP.NET Core. Per informazioni sul routing di ASP.NET Core MVC, vedere <xref:mvc/controllers/routing>. Per informazioni sulle convenzioni di routing in Razor Pages, vedere <xref:razor-pages/razor-pages-conventions>.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Nozioni fondamentali sul routing

La maggior parte delle app dovrebbe scegliere uno schema di routing semplice e descrittivo in modo che gli URL siano leggibili e significativi. La route convenzionale predefinita `{controller=Home}/{action=Index}/{id?}`:

* Supporta uno schema di routing semplice e descrittivo.
* È un punto iniziale utile per le app basate su interfaccia utente.

Gli sviluppatori solitamente aggiungono ulteriori route brevi nelle aree a traffico elevato dell'app in situazioni specifiche (ad esempio, endpoint di blog e e-commerce) usando il [routing degli attributi](xref:mvc/controllers/routing#attribute-routing) o route convenzionali dedicate.

Le API Web devono usare il routing degli attributi per modellare le funzionalità dell'app come set di risorse in cui le operazioni sono rappresentate da verbi HTTP. Questo significa che molte operazioni (ad esempio, GET, POST) sulla stessa risorsa logica useranno lo stesso URL. Il routing degli attributi offre un livello di controllo necessario per progettare con attenzione il layout dell'endpoint pubblico di un'API.

Le app Razor Pages usano il routing convenzionale predefinito per servire le risorse denominate nella cartella *Pagine* dell'app. Sono disponibili convenzioni aggiuntive che consentono di personalizzare il comportamento di routing di Razor Pages. Per altre informazioni, vedere <xref:razor-pages/index> e <xref:razor-pages/razor-pages-conventions>.

Il supporto della generazione di URL consente di sviluppare l'app senza URL hardcoded per collegare l'app. Il supporto consente di iniziare con una configurazione di routing di base e di modificare le route dopo aver determinato il layout delle risorse dell'app.

Il routing usa gli *endpoint* (`Endpoint`) per rappresentare gli endpoint logici in un'app.

Un endpoint definisce un delegato per l'elaborazione delle richieste e una raccolta di metadati arbitrari. I metadati vengono usati per implementare gli elementi trasversali in base ai criteri e la configurazione associata a ogni endpoint.

Il sistema di routing ha le caratteristiche seguenti:

* La sintassi del modello di route viene usata per definire le route con parametri di route in formato token.
* La configurazione degli endpoint basata su convenzioni e attributi è consentita.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> consente di determinare se un parametro URL contiene un valore valido per il vincolo di un endpoint specifico.
* I modelli di app, ad esempio MVC o Razor Pages, registrano tutti i relativi endpoint, che hanno un'implementazione stimabile degli scenari di routing.
* L'implementazione del routing determina le decisioni relative al routing nel punto desiderato della pipeline del middleware.
* Il middleware successivo a un middleware di routing può esaminare il risultato della decisione dell'endpoint del middleware di routing per un URI di richiesta specifico.
* È possibile enumerare tutti gli endpoint nell'app in un punto qualsiasi della pipeline del middleware.
* Un'app può usare il routing per generare URL, ad esempio per il reindirizzamento o i collegamenti, in base alle informazioni degli endpoint. In questo modo si evita di impostare gli URL come hardcoded ottimizzando la manutenibilità.
* La generazione degli URL è basata sugli indirizzi, che supportano l'estensibilità arbitraria:

  * L'API del generatore di collegamenti (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) può essere risolta ovunque tramite l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) per generare gli URL.
  * Se l'API del generatore di collegamenti non è disponibile tramite l'inserimento delle dipendenze, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offre metodi per la generazione degli URL.

> [!NOTE]
> Il collegamento di endpoint è limitato alle azioni e alle pagine MVC/Razor Pages. Le espansioni delle funzionalità di collegamento degli endpoint sono previste per le versioni future.

Il routing è connesso alla pipeline [middleware](xref:fundamentals/middleware/index) per mezzo della classe <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>. [ASP.NET Core MVC](xref:mvc/overview) aggiunge il routing alla pipeline del middleware come parte della configurazione e gestisce il routing nelle app MVC e Razor Pages . Per informazioni sull'uso del routing come componente autonomo, vedere la sezione [Usare il middleware di routing](#use-routing-middleware).

### <a name="url-matching"></a>Corrispondenza URL

La corrispondenza dell'URL è il processo con cui il routing invia una richiesta in ingresso a un *endpoint*. Questo processo si basa sui dati presenti nel percorso URL, ma può essere esteso in modo da prendere in considerazione tutti i dati della richiesta. La possibilità di inviare le richieste a gestori separati è fondamentale per ridurre le dimensioni e la complessità di un'app.

Quando viene eseguito un middleware di routing, viene impostato un endpoint (`Endpoint`) e i valori di route vengono impostati su una funzionalità in <xref:Microsoft.AspNetCore.Http.HttpContext>. Per la richiesta corrente:

* La chiamata di `HttpContext.GetEndpoint` ottiene l'endpoint.
* `HttpRequest.RouteValues` ottiene la raccolta dei valori di route.

Il middleware in esecuzione dopo il middleware di routing può visualizzare l'endpoint e agire. Ad esempio, un middleware di autorizzazione può interrogare la raccolta di metadati dell'endpoint per ottenere un criterio di autorizzazione. Dopo l'esecuzione di tutto il middleware nella pipeline di elaborazione della richiesta, viene richiamato il delegato dell'endpoint selezionato.

Il sistema di routing nell'endpoint di routing è responsabile di tutte le decisioni relative all'invio. Poiché il middleware applica i criteri in base all'endpoint selezionato, è importante che qualsiasi decisione che può influire sull'invio o l'applicazione dei criteri di sicurezza venga effettuata all'interno del sistema di routing.

Quando viene eseguito il delegato dell'endpoint, le proprietà di [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) vengono impostate sui valori appropriati in base all'elaborazione della richiesta eseguita fino a quel punto.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) è un dizionario di *valori di route* prodotti dalla route. Questi valori in genere sono determinati dalla suddivisione in token dell'URL e possono essere usati per accettare l'input dell'utente o per prendere ulteriori decisioni riguardo all'invio all'interno dell'app.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) è un elenco proprietà dei dati aggiuntivi correlati alla route corrispondente. Gli elementi <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> vengono forniti per supportare l'associazione dei dati sullo stato con ogni route in modo che l'app sia in grado di prendere decisioni in base alla route corrispondente. Questi valori sono definiti dallo sviluppatore e **non** influiscono sul comportamento del routing in alcun modo. Inoltre, i valori accantonati in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) possono essere di qualsiasi tipo, a differenza dei valori [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) che devono essere convertibili in e da stringhe.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) è un elenco delle route che hanno preso parte alla corrispondenza corretta della richiesta. Le route possono essere annidate l'una nell'altra. La proprietà <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> rispecchia il percorso tramite l'albero logico di route che hanno prodotto una corrispondenza. In genere il primo elemento in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> è la raccolta di route e deve essere usato per la generazione di URL. L'ultimo elemento in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> è il gestore di route corrispondente.

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>Generazione di URL con LinkGenerator

La generazione di URL è il processo con cui il routing crea un percorso URL basato su un set di valori di route. Questo consente di avere una separazione logica tra gli endpoint e gli URL che vi accedono.

Il routing di endpoint include l'API del generatore di collegamenti (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>). <xref:Microsoft.AspNetCore.Routing.LinkGenerator> è un servizio singleton che può essere recuperato dall'inserimento delle dipendenze. L'API può essere usata all'esterno del contesto di una richiesta in esecuzione. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> e gli scenari di MVC basati su <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, ad esempio [helper tag](xref:mvc/views/tag-helpers/intro), helper HTML e [risultati delle azioni](xref:mvc/controllers/actions), usano il generatore di collegamenti per offrire le funzionalità di generazione di collegamenti.

Il generatore di collegamenti si basa sui concetti di *indirizzo* e di *schemi di indirizzi*. Lo schema di indirizzi consente di determinare gli endpoint che devono essere considerati per la generazione dei collegamenti. Ad esempio, gli scenari di nome route e valori di route noti a numerosi utenti in MVC o Razor Pages vengono implementati come schema di indirizzi.

Il generatore di collegamenti può collegarsi alle azioni e alle pagine MVC o Razor Pages tramite i metodi di estensione seguenti:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

Un overload di questi metodi accetta gli argomenti che includono `HttpContext`. Questi metodi sono funzionalmente equivalenti a `Url.Action` e `Url.Page` ma offrono una maggiore flessibilità e altre opzioni.

I metodi `GetPath*` sono più simili a `Url.Action` e `Url.Page` poiché generano un URI che contiene un percorso assoluto. I metodi `GetUri*` generano sempre un URI assoluto contenente uno schema e un host. I metodi che accettano `HttpContext` generano un URI nel contesto della richiesta in esecuzione. Se non vengono sovrascritti, vengono usati i valori di route di ambiente, il percorso base dell'URL, lo schema e l'host dalla richiesta in esecuzione.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> viene chiamato con un indirizzo. La generazione di un URI viene eseguita in due passaggi:

1. Un indirizzo viene associato a un elenco di endpoint che corrispondono all'indirizzo.
1. `RoutePattern` di ogni endpoint viene valutato fino a quando non viene individuato un formato di route che corrisponde ai valori specificati. L'output risultante viene unito alle altre parti dell'URI specificate nel generatore di collegamenti e restituito.

I metodi forniti da <xref:Microsoft.AspNetCore.Routing.LinkGenerator> supportano le funzionalità di generazione di collegamenti standard per tutti i tipi di indirizzi. È consigliabile usare il generatore di collegamenti tramite i metodi di estensione che eseguono le operazioni per un tipo di indirizzo specifico.

| Metodo di estensione | Descrizione |
| ---------------- | ----------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Genera un URI con un percorso assoluto in base ai valori specificati. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Genera un URI assoluto in base ai valori specificati.             |

> [!WARNING]
> Prestare attenzione alle implicazioni seguenti della chiamata ai metodi <xref:Microsoft.AspNetCore.Routing.LinkGenerator>:
>
> * Usare i metodi di estensione `GetUri*` con cautela in una configurazione di app che non convalida l'intestazione `Host` delle richieste in ingresso. Se l'intestazione `Host` delle richieste in ingresso non viene convalidata, l'input delle richieste non attendibili può essere inviato nuovamente al client negli URI di una visualizzazione o pagina. È consigliabile che in tutte le app di produzione il server sia configurato per la convalida dell'intestazione `Host` rispetto ai valori validi noti.
>
> * Usare <xref:Microsoft.AspNetCore.Routing.LinkGenerator> con cautela nel middleware in associazione a `Map` o `MapWhen`. `Map*` modifica il percorso di base della richiesta in esecuzione, che ha effetto sull'output della generazione di collegamenti. Tutte le API <xref:Microsoft.AspNetCore.Routing.LinkGenerator> consentono di specificare un percorso di base. Specificare sempre un percorso di base vuoto per annullare l'effetto di `Map*` sulla generazione di collegamenti.

## <a name="differences-from-earlier-versions-of-routing"></a>Differenze rispetto alle versioni precedenti del routing

Esistono alcune differenze tra il routing degli endpoint e le versioni di routing precedenti a ASP.NET Core 2,2:

* Il sistema di routing di endpoint non supporta l'estensibilità basata su <xref:Microsoft.AspNetCore.Routing.IRouter>, inclusa l'eredità da <xref:Microsoft.AspNetCore.Routing.Route>.

* Il routing di endpoint non supporta [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim). Usare la [versione di compatibilità](xref:mvc/compatibility-version) 2.1 (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) per continuare a usare lo shim per la compatibilità.

* Il routing di endpoint ha un comportamento diverso per l'utilizzo delle maiuscole e delle minuscole degli URI generati quando si usano le route convenzionali.

  Esaminare il modello di route predefinito seguente:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Si supponga di generare un collegamento a un'azione usando la route seguente:

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  Con il routing basato su <xref:Microsoft.AspNetCore.Routing.IRouter>, questo codice genera un URI di `/blog/ReadPost/17`, che rispetta le maiuscole e le minuscole del valore di route specificato. Il routing di endpoint in ASP.NET Core 2.2 o versioni successive produce `/Blog/ReadPost/17` ("Blog" viene convertita in lettere maiuscole). Il routing di endpoint offre l'interfaccia `IOutboundParameterTransformer` che può essere usata per personalizzare questo comportamento globalmente o per applicare convenzioni diverse per il mapping degli URL.

  Per altre informazioni, vedere la sezione [Riferimento ai trasformatori di parametro](#parameter-transformer-reference).

* La generazione di collegamenti usata da MVC o Razor Pages con le route convenzionali ha un comportamento diverso quando si tenta il collegamento a un controller/azione o a una pagina non esistente.

  Esaminare il modello di route predefinito seguente:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Si supponga di generare un collegamento a un'azione usando il modello predefinito con il codice seguente:

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  Con il routing basato su `IRouter`, il risultato è sempre `/Blog/ReadPost/17`, anche se `BlogController` non è presente o non ha un metodo di azione `ReadPost`. Come previsto, il routing di endpoint in ASP.NET Core 2.2 o versioni successive produce `/Blog/ReadPost/17` se è presente il metodo di azione. *Tuttavia, il routing di endpoint produce una stringa vuota se l'azione non è presente.* Concettualmente, il routing di endpoint non presume che l'endpoint sia presente se l'azione non è presente.

* L'*algoritmo di invalidamento dei valori di ambiente* della generazione di collegamenti ha un comportamento diverso quando viene usato con il routing di endpoint.

  L'*invalidamento dei valori di ambiente* è l'algoritmo che decide quali valori di route della richiesta in esecuzione (valori di ambiente) possono essere usati nelle operazioni di generazione di collegamenti. Il routing convenzionale ha sempre invalidato i valori di route aggiuntivi durante il collegamento a un'azione diversa. Prima del rilascio di ASP.NET Core 2.2, il routing degli attributi non aveva questo comportamento. Nelle versioni precedenti di ASP.NET Core i collegamenti a un'altra azione con gli stessi nomi di parametro di route causavano errori nella generazione dei collegamenti. In ASP.NET Core 2.2 o versioni successive entrambi i tipi di routing invalidano i valori in caso di collegamento a un'altra azione.

  Esaminare l'esempio seguente in ASP.NET Core 2.1 o versioni precedenti. In caso di collegamento a un'altra azione o a un'altra pagina, i valori di route possono essere riutilizzati in modi indesiderati.

  In */Pages/Store/Product.cshtml*:

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  In */Pages/Login.cshtml*:

  ```cshtml
  @page "{id?}"
  ```

  Se l'URI è `/Store/Product/18` in ASP.NET Core 2.1 o versioni precedenti, il collegamento generato nella pagina Store/Info da `@Url.Page("/Login")` è `/Login/18`. Il valore `id` di 18 viene riutilizzato, anche se la destinazione del collegamento è una parte diversa dell'app. Il valore di route `id` nel contesto della pagina `/Login` è probabilmente un valore di ID utente, non un valore di ID prodotto del punto vendita.

  Nel routing di endpoint con ASP.NET Core 2.2 o versioni successive, il risultato è `/Login`. I valori di ambiente non vengono riutilizzati quando la destinazione collegata è costituita da un'azione o una pagina diversa.

* Sintassi dei parametri di route di round trip: le barre non vengono codificate quando viene usata una sintassi del parametro catch-all con doppio asterisco (`**`).

  Durante la generazione di collegamenti, il sistema di routing codifica il valore acquisito in un parametro catch-all con doppio asterisco (`**`) (ad esempio, `{**myparametername}`) escludendo le barre. Il parametro catch-all con doppio asterisco è supportato con il routing basato su `IRouter` in ASP.NET Core 2.2 o versioni successive.

  La sintassi del parametro catch-all con asterisco singolo nelle versioni precedenti di ASP.NET Core (`{*myparametername}`) resta supportata e le barre vengono codificate.

  | Route              | Collegamento generato con<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts` (la barra viene codificata)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Esempio di middleware

Nell'esempio seguente un middleware usa l'API <xref:Microsoft.AspNetCore.Routing.LinkGenerator> per creare il collegamento a un metodo di azione che elenca i prodotti del punto vendita. L'uso del generatore di collegamenti tramite l'inserimento in una classe e la chiamata a `GenerateLink` è possibile in tutte le classi di un'app.

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

### <a name="create-routes"></a>Creare le route

La maggior parte delle app crea le route chiamando <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> o uno dei metodi di estensione simili definiti per <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Tutti i metodi di estensione <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> creano un'istanza di <xref:Microsoft.AspNetCore.Routing.Route> e la aggiungono alla raccolta di route.

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> non accetta alcun parametro del gestore di route. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> aggiunge solo le route che sono gestite da <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Per altre informazioni sul routing in MVC, vedere <xref:mvc/controllers/routing>.

L'esempio di codice seguente si riferisce a una chiamata <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> usata da una tipica definizione di route di ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Questo modello confronta un percorso URL ed estrae i valori di route. Ad esempio, il percorso `/Products/Details/17` genera i valori di route seguenti: `{ controller = Products, action = Details, id = 17 }`.

I valori di route vengono determinati suddividendo il percorso URL in segmenti e confrontando ogni segmento con il nome del *parametro di route* nel modello di route. I parametri di route sono denominati. Parametri definiti racchiudendo il nome del parametro tra parentesi graffe `{ ... }`.

Il modello precedente può anche confrontare il percorso URL `/` e generare i valori `{ controller = Home, action = Index }`. Ciò accade perché i parametri di route `{controller}` e `{action}` hanno valori predefiniti e il parametro di route `id` è facoltativo. Un segno di uguale (`=`) seguito da un valore dopo il nome del parametro di route definisce un valore predefinito per il parametro. Un punto interrogativo (`?`) dopo il nome del parametro di route definisce un parametro facoltativo.

I parametri di route con un valore predefinito producono *sempre* un valore di route quando la route corrisponde. I parametri facoltativi non producono alcun valore di route se non esiste un segmento di percorso URL corrispondente. Vedere la sezione [Riferimento per i modelli di route](#route-template-reference) per una descrizione completa degli scenari e della sintassi del modello di route.

Nell'esempio seguente la definizione del parametro di route `{id:int}` definisce un [vincolo di route](#route-constraint-reference) per il parametro di route `id`:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Questo modello confronta un percorso URL come `/Products/Details/17`, ma non `/Products/Details/Apples`. I vincoli di route implementano <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> ed esaminano i valori di route per verificarli. In questo esempio il valore di route `id` deve poter essere convertito in un numero intero. Vedere [Riferimento per i vincoli di route](#route-constraint-reference) per una descrizione dei vincoli di route specificati dal framework.

Altri overload di <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accettano i valori per `constraints`, `dataTokens` e `defaults`. L'uso tipico di questi parametri è passare un oggetto tipizzato in modo anonimo, in cui i nomi di proprietà di tipo anonimo corrispondono ai nomi dei parametri di route.

Gli esempi <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> seguenti creano route equivalenti:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> La sintassi inline per definire i vincoli e i valori predefiniti può essere utile per le route semplici. Esistono tuttavia scenari, come i token di dati, che non sono supportati dalla sintassi inline.

L'esempio seguente illustra alcuni scenari aggiuntivi:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Il modello precedente confronta un percorso URL come `/Blog/All-About-Routing/Introduction` ed estrae i valori `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. I valori di route predefiniti per `controller` e `action` vengono generati dalla route anche se sono non presenti parametri di route corrispondenti nel modello. I valori predefiniti possono essere specificati nel modello di route. Il parametro di route `article` è definito come *catch-all* in base alla presenza di un doppio asterisco (`**`) prima del nome del parametro di route. I parametri di route catch-all acquisiscono il resto del percorso URL e possono anche corrispondere alla stringa vuota.

L'esempio seguente aggiunge i vincoli di route e i token di dati:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Il modello precedente confronta un percorso URL come `/en-US/Products/5` ed estrae i valori `{ controller = Products, action = Details, id = 5 }` e i token di dati `{ locale = en-US }`.

![Token della finestra Variabili locali](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Generazione di URL della classe di route

La classe <xref:Microsoft.AspNetCore.Routing.Route> è inoltre in grado di eseguire la generazione di URL combinando un set di valori di route con il relativo modello di route. Questo è logicamente il processo inverso di corrispondenza del percorso URL.

> [!TIP]
> Per comprendere meglio la generazione di URL, si pensi a quale URL si vuole generare e al modo in cui un modello di route deve corrispondere a tale URL. Quali valori devono essere prodotti? Questo equivale approssimativamente al funzionamento della generazione di URL nella classe <xref:Microsoft.AspNetCore.Routing.Route>.

L'esempio seguente usa una route predefinita di ASP.NET Core MVC generale:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Con i valori di route `{ controller = Products, action = List }`, viene generato l'URL `/Products/List`. I valori di route vengono sostituiti dai parametri di route corrispondenti per formare il percorso URL. Poiché `id` è un parametro di route facoltativo, l'URL è stato generato senza un valore per `id`.

Con i valori di route `{ controller = Home, action = Index }`, viene generato l'URL `/`. Poiché i valori di route specificati corrispondono ai valori predefiniti, i segmenti corrispondenti ai valori predefiniti possono essere omessi.

Per entrambi gli URL generati viene eseguito il round trip con la definizione di route seguente (`/Home/Index` e `/`) e vengono prodotti gli stessi valori di route usati per generare l'URL.

> [!NOTE]
> Un'app che usa ASP.NET Core MVC deve usare <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> per generare gli URL invece di eseguire la chiamata direttamente nel routing.

Per altre informazioni sulla generazione di URL, vedere la sezione [Riferimento per la generazione di URL](#url-generation-reference).

## <a name="use-routing-middleware"></a>Usare il middleware di routing

Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) nel file di progetto dell'app.

Aggiungere il routing al contenitore del servizio in `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Le route devono essere configurate nel metodo `Startup.Configure`. L'app di esempio usa le API seguenti:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Confronta solo le richieste HTTP GET.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

Nella tabella seguente sono elencate le risposte con gli URI specificati.

| URI                    | Risposta                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Hello! Route values: [operation, create], [id, 3] |
| `/package/track/-3`    | Hello! Route values: [operation, track], [id, -3] |
| `/package/track/-3/`   | Hello! Route values: [operation, track], [id, -3] |
| `/package/track/`      | La richiesta non viene eseguita, nessuna corrispondenza.              |
| `GET /hello/Joe`       | Hi, Joe!                                          |
| `POST /hello/Joe`      | La richiesta non viene eseguita, corrispondenza solo con HTTP GET. |
| `GET /hello/Joe/Smith` | La richiesta non viene eseguita, nessuna corrispondenza.              |

Il framework offre un set di metodi di estensione per la creazione di route (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

I metodi `Map[Verb]` usano i vincoli per limitare la route al verbo HTTP nel nome del metodo. Vedere ad esempio <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> e <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Riferimento per il modello di route

I token all'interno di parentesi graffe (`{ ... }`) definiscono i *parametri di route* che vengono associati se esiste una corrispondenza per la route. È possibile definire più parametri di route in un segmento di route, ma devono essere separati da un valore letterale. Ad esempio, `{controller=Home}{action=Index}` non è una route valida perché non è presente un valore letterale tra `{controller}` e `{action}`. Questi parametri di route devono avere un nome e possono avere attributi aggiuntivi.

Il testo letterale diverso dai parametri di route, ad esempio `{id}`, e il separatore di percorso `/` devono corrispondere al testo nell'URL. La corrispondenza del testo non fa distinzione tra maiuscole e minuscole e si basa sulla rappresentazione decodificata del percorso degli URL. Per verificare la corrispondenza di un delimitatore letterale dei parametri di route (`{` o `}`), eseguire l'escape del delimitatore ripetendo il carattere (`{{` o `}}`).

I modelli di URL che tentano di acquisire un nome file con un'estensione facoltativa hanno considerazioni aggiuntive. Considerare ad esempio il modello `files/{filename}.{ext?}`. Se esistono i valori per `filename` e `ext`, vengono popolati entrambi i valori. Se esiste solo un valore per `filename` nell'URL, la route corrisponde perché il punto finale (`.`) è facoltativo. Gli URL seguenti corrispondono a questa route:

* `/files/myFile.txt`
* `/files/myFile`

È possibile usare un asterisco (`*`) o un doppio asterisco (`**`) come prefisso per un parametro di route da associare alla parte rimanente dell'URI. Si tratta di parametri chiamati parametri *catch-all*. Ad esempio, `blog/{**slug}` corrisponde a qualsiasi URI che inizia con `/blog` seguito da qualsiasi valore, che viene assegnato al valore di route `slug`. I parametri catch-all possono anche corrispondere alla stringa vuota.

Il parametro catch-all esegue l'escape dei caratteri appropriati, incluso il separatore di percorso (`/`), quando la route viene usata per generare un URL. Ad esempio, la route `foo/{*path}` con i valori di route `{ path = "my/path" }` genera `foo/my%2Fpath`. Si noti la barra di escape. Per eseguire il round trip dei caratteri di separatore di percorso, usare il prefisso del parametro di route `**`. La route `foo/{**path}` con `{ path = "my/path" }` genera `foo/my/path`.

I parametri di route possono avere *valori predefiniti*, definiti specificando il valore predefinito dopo il nome del parametro, separato da un segno di uguale (`=`). Ad esempio, `{controller=Home}` definisce `Home` come valore predefinito per `controller`. Il valore predefinito viene usato se nell'URL non è presente alcun valore per il parametro. I parametri di route vengono resi facoltativi aggiungendo un punto interrogativo (`?`) alla fine del nome del parametro, come in `id?`. La differenza tra parametri facoltativi e parametri di route predefiniti è che un parametro di route con un valore predefinito produce sempre un valore, mentre un parametro facoltativo ha un valore solo se ne viene specificato uno dall'URL della richiesta.

I parametri di route possono presentare dei vincoli che devono corrispondere al valore di route associato dall'URL. Aggiungendo due punti (`:`) e il nome del vincolo dopo il nome del parametro di route, si specifica un *vincolo inline* per un parametro di route. Se il vincolo richiede argomenti, vengono racchiusi tra parentesi (`(...)`) dopo il nome del vincolo. È possibile specificare più vincoli inline aggiungendo di nuovo i due punti (`:`) e il nome del vincolo.

Il nome del vincolo e gli argomenti vengono passati al servizio <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> per creare un'istanza di <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> da usare nell'elaborazione dell'URL. Il modello di route `blog/{article:minlength(10)}` specifica ad esempio un vincolo `minlength` con l'argomento `10`. Per altre informazioni sui vincoli di route e per un elenco dei vincoli specificati dal framework, vedere la sezione [Riferimento per i vincoli di route](#route-constraint-reference).

I parametri di route possono avere anche trasformatori di parametro che trasformano un valore di parametro durante la generazione dei collegamenti e l'abbinamento delle azioni e delle pagine agli URI. Come i vincoli, i trasformatori di parametro possono essere aggiunti inline a un parametro di route inserendo due punti (`:`) e il nome del trasformatore dopo il nome del parametro di route. Ad esempio, il modello di route `blog/{article:slugify}` specifica un trasformatore `slugify`. Per altre informazioni sui trasformatori di parametro, vedere la sezione [Riferimento ai trasformatori di parametro](#parameter-transformer-reference).

La tabella seguente illustra i modelli di route di esempio e il relativo comportamento.

| Modello di route                           | URI corrispondente di esempio    | L'URI della richiesta&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Verifica la corrispondenza solo del singolo percorso `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Verifica la corrispondenza e imposta `Page` su `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Verifica la corrispondenza e imposta `Page` su `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Esegue il mapping al controller `Products` e all'azione `List`.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Esegue il mapping al controller `Products` e all'azione `Details` (`id` impostato su 123). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Esegue il mapping al controller `Home` e al metodo `Index` (`id` viene ignorato).        |

L'uso di un modello è in genere l'approccio più semplice al routing. I vincoli e le impostazioni predefinite possono essere specificati anche all'esterno del modello di route.

> [!TIP]
> Abilitare la [registrazione](xref:fundamentals/logging/index) per verificare in che modo le implementazioni del routing predefinite, ad esempio <xref:Microsoft.AspNetCore.Routing.Route>, corrispondono alle richieste.

## <a name="reserved-routing-names"></a>Nomi riservati di routing

Le parole chiave seguenti sono nomi riservati e non possono essere usate come nomi o parametri di route:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Riferimento per i vincoli di route

I vincoli di route vengono eseguiti quando si verifica una corrispondenza nell'URL in ingresso e il percorso URL viene suddiviso in valori di route in formato token. I vincoli di route in genere controllano il valore di route associato usando il modello di route e stabiliscono con una decisione Sì/No se il valore è accettabile o meno. Alcuni vincoli di route usano i dati all'esterno del valore di route per stabilire se la richiesta può essere instradata. Ad esempio, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> può accettare o rifiutare una richiesta in base al relativo verbo HTTP. I vincoli vengono usati nelle richieste del routing e nella generazione di collegamenti.

> [!WARNING]
> Non usare i vincoli per la **convalida dell'input**. Se vengono usati vincoli per la **convalida dell'input**, un input non valido causa la visualizzazione di un errore di tipo *404 (Non trovato)* invece di un errore di tipo *400 - Richiesta non valida* con un messaggio di errore appropriato. I vincoli di route vengono usati per evitare **ambiguità** tra route simili, non per convalidare gli input per una route specifica.

La tabella seguente illustra i vincoli di route di esempio e il relativo comportamento.

| vincolo | Esempio | Esempi di corrispondenza | Note |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Corrisponde a qualsiasi numero intero |
| `bool` | `{active:bool}` | `true`, `FALSE` | Corrisponde a `true` o `false` (senza distinzione tra maiuscole e minuscole) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Corrisponde a un valore `DateTime` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Corrisponde a un valore `decimal` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Corrisponde a un valore `double` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Corrisponde a un valore `float` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Corrisponde a un valore `Guid` valido |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Corrisponde a un valore `long` valido |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | La stringa deve contenere almeno 4 caratteri |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | La stringa non deve contenere più di 8 caratteri |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | La stringa deve contenere esattamente 12 caratteri |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | La stringa deve contenere almeno 8 e non più di 16 caratteri |
| `min(value)` | `{age:min(18)}` | `19` | Il valore intero deve essere almeno 18 |
| `max(value)` | `{age:max(120)}` | `91` | Il valore intero non deve essere superiore a 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Il valore intero deve essere almeno 18 ma non più di 120 |
| `alpha` | `{name:alpha}` | `Rick` | La stringa deve essere costituita da uno o più caratteri alfabetici (`a`-`z`, senza distinzione tra maiuscole e minuscole) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | La stringa deve corrispondere all'espressione regolare (vedere i suggerimenti per la definizione di un'espressione regolare) |
| `required` | `{name:required}` | `Rick` | Usato per imporre che un valore diverso da un parametro sia presente durante la generazione dell'URL |

Più vincoli delimitati da punti possono essere applicati a un singolo parametro. Ad esempio il vincolo seguente limita un parametro a un valore intero maggiore o uguale a 1:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> I vincoli di route che verificano l'URL e vengono convertiti in un tipo CLR, ad esempio `int` o `DateTime`, usano sempre le impostazioni cultura inglese non dipendenti da paese/area geografica, presupponendo che l'URL sia non localizzabile. I vincoli di route specificati dal framework non modificano i valori archiviati nei valori di route. Tutti i valori di route analizzati dall'URL vengono archiviati come stringhe. Ad esempio, il vincolo `float` prova a convertire il valore di route in un valore float, ma il valore convertito viene usato solo per verificare che può essere convertito in un valore float.

## <a name="regular-expressions"></a>Espressioni regolari

Il framework di ASP.NET Core aggiunge `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` al costruttore di espressioni regolari. Per una descrizione di questi membri, vedere <xref:System.Text.RegularExpressions.RegexOptions>.

Le espressioni regolari usano delimitatori e token simili a quelli usati dal routing e dal linguaggio C#. Per i token di espressione è necessario aggiungere caratteri di escape. Per usare l'espressione regolare `^\d{3}-\d{2}-\d{4}$` nel routing, l'espressione deve includere i caratteri `\` (barra rovesciata singola) specificati nella stringa come caratteri `\\` (barra rovesciata doppia) nel file di origine C# per eseguire l'escape del carattere di escape della stringa `\`, a meno che non si usino [valori letterali di stringa verbatim](/dotnet/csharp/language-reference/keywords/string). Per eseguire l'escape dii caratteri delimitatori dei parametri di routing (`{`, `}`, `[`, `]`), raddoppiare i caratteri nell'espressione (`{{`, `}`, `[[`, `]]`). La tabella seguente mostra un'espressione regolare e la versione dopo l'escape.

| Espressione regolare    | Espressione regolare con caratteri di escape     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Le espressioni regolari usate nel routing spesso iniziano con l'accento circonflesso (`^`) e corrispondono alla posizione iniziale della stringa. Le espressioni spesso terminano con il segno di dollaro (`$`) e corrispondono alla fine della stringa. I caratteri `^` e `$` consentono di verificare che l'espressione regolare corrisponda all'intero valore del parametro di route. Senza i caratteri `^` e `$` l'espressione regolare corrisponde a qualsiasi sottostringa all'interno della stringa e spesso questo non è il risultato desiderato. La tabella seguente include alcuni esempi e descrive perché si verifica o non si verifica la corrispondenza.

| Espressione   | Stringa    | Corrispondenza con | Commento               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | hello     | Yes   | Corrispondenze di sottostringhe     |
| `[a-z]{2}`   | 123abc456 | Yes   | Corrispondenze di sottostringhe     |
| `[a-z]{2}`   | mz        | Yes   | Corrisponde all'espressione    |
| `[a-z]{2}`   | MZ        | Yes   | Senza distinzione maiuscole/minuscole    |
| `^[a-z]{2}$` | hello     | No    | Vedere `^` e `$` sopra |
| `^[a-z]{2}$` | 123abc456 | No    | Vedere `^` e `$` sopra |

Per altre informazioni sulla sintassi delle espressioni regolari, vedere [Espressioni regolari di .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Per limitare un parametro a un set noto di valori possibili, usare un'espressione regolare. Ad esempio, `{action:regex(^(list|get|create)$)}` verifica la corrispondenza del valore di route `action` solo con `list`, `get` o `create`. Se viene passata nel dizionario di vincoli, la stringa `^(list|get|create)$` è equivalente. Anche i vincoli passati nel dizionario di vincoli (non inline all'interno di un modello) che non corrispondono a uno dei vincoli noti vengono considerati espressioni regolari.

## <a name="custom-route-constraints"></a>Vincoli di route personalizzati

Oltre ai vincoli di route predefiniti, è possibile creare vincoli di route personalizzati implementando l'interfaccia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>. L'interfaccia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> contiene un singolo metodo, `Match`, che restituisce `true` se il vincolo viene soddisfatto e `false` in caso contrario.

Per usare un'interfaccia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> personalizzata, il tipo di vincolo di route deve essere registrato con la proprietà <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> dell'app nel contenitore di servizi dell'app. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> è un dizionario che esegue il mapping delle chiavi dei vincoli di route alle implementazioni di <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> che convalidano tali vincoli. La proprietà <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> di un'app può essere aggiornata in `Startup.ConfigureServices` come parte di una chiamata [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) o configurando <xref:Microsoft.AspNetCore.Routing.RouteOptions> direttamente con `services.Configure<RouteOptions>`. Ad esempio:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

Il vincolo può quindi essere applicato alle route nel modo consueto, usando il nome specificato al momento della registrazione del tipo di vincolo. Ad esempio:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a>Riferimento ai trasformatori di parametro

I trasformatori di parametro:

* Vengono eseguiti quando si genera un collegamento per <xref:Microsoft.AspNetCore.Routing.Route>.
* Implementare `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Vengono configurati tramite <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Acquisire il valore della route del parametro e trasformarlo in un nuovo valore stringa.
* Il valore trasformato viene usato nel collegamento generato.

Ad esempio, un trasformatore di parametro `slugify` personalizzato nel modello di route `blog\{article:slugify}` con `Url.Action(new { article = "MyTestArticle" })` genera `blog\my-test-article`.

Per usare un trasformatore di parametro in un modello di route, configurarlo prima usando <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

I trasformatori di parametro sono usati dai framework per trasformare l'URI in cui viene risolto un endpoint. Ad esempio, ASP.NET Core MVC usa i trasformatori di parametro per trasformare il valore di route usato per abbinare `area`, `controller`, `action` e `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

Con la route precedente, l'azione `SubscriptionManagementController.GetAll()` viene abbinata all'URI `/subscription-management/get-all`. Un trasformatore di parametro non modifica i valori della route usati per generare un collegamento. Ad esempio, `Url.Action("GetAll", "SubscriptionManagement")` restituisce `/subscription-management/get-all`.

ASP.NET Core offre convenzioni API per l'uso di trasformatori di parametro con le route generate:

* ASP.NET Core MVC include la convenzione API `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention`. Questa convenzione applica un trasformatore di parametro specifico a tutte le route di attributi nell'app. Il trasformatore di parametro trasforma i token di route di attributi man mano che vengono sostituiti. Per altre informazioni vedere [Use a parameter transformer to customize token replacement](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement) (Usare un trasformatore di parametro per personalizzare la sostituzione di token).
* Razor Pages include la convenzione API `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention`. Questa convenzione applica un trasformatore di parametro specificato a tutte le istanze di Razor Pages individuate automaticamente. Il trasformatore di parametro trasforma la cartella e i segmenti di nome file delle route di Razor Pages. Per altre informazioni, vedere [Use a parameter transformer to customize page routes](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes) (Usare un trasformatore di parametro per personalizzare route di pagine).

## <a name="url-generation-reference"></a>Riferimento per la generazione di URL

L'esempio seguente mostra come generare un collegamento a una route usando un dizionario di valori di route e un elemento <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

L'elemento <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generato alla fine dell'esempio precedente è `/package/create/123`. Il dizionario fornisce i valori di route `operation` e `id` del modello "Track Package Route", `package/{operation}/{id}`. Per informazioni dettagliate, vedere il codice di esempio nella sezione [Usare il middleware di routing](#use-routing-middleware) oppure l'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Il secondo parametro per il costruttore <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> è una raccolta di *valori di ambiente*. I valori di ambiente sono utili poiché limitano il numero di valori che uno sviluppatore deve specificare all'interno del contesto di una richiesta. I valori di route correnti della richiesta corrente sono considerati valori di ambiente per la generazione del collegamento. Nell'azione `About` di un'app ASP.NET Core MVC di `HomeController`, non è necessario specificare il valore di route del controller per collegarsi all'azione `Index`. Viene usato il valore di ambiente `Home`.

I valori di ambiente che non corrispondono a un parametro vengono ignorati. I valori di ambiente vengono ignorati anche quando un valore specificato in modo esplicito esegue l'override del valore di ambiente. La corrispondenza si verifica da sinistra a destra nell'URL.

I valori specificati in modo esplicito ma che non corrispondono a un segmento della route vengono aggiunti alla stringa di query. La tabella seguente illustra il risultato ottenuto quando si usa il modello di route `{controller}/{action}/{id?}`.

| Valori di ambiente                     | Valori espliciti                        | Risultato                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |

Se una route ha un valore predefinito che non corrisponde a un parametro e tale valore viene specificato in modo esplicito, deve corrispondere al valore predefinito:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

La generazione dei collegamenti genera un collegamento per questa route quando vengono specificati i valori corrispondenti per `controller` e `action`.

## <a name="complex-segments"></a>Segmenti complessi

I segmenti complessi (ad esempio, `[Route("/x{token}y")]`), vengono elaborati individuando corrispondenze per i valori letterali da destra a sinistra in modalità non-greedy. Vedere [questo codice](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) per una spiegazione dettagliata di come vengono confrontati i segmenti complessi. L'[esempio di codice](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) non viene usato da ASP.NET Core, ma offre una spiegazione esauriente dei segmenti complessi.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Il routing è responsabile del mapping degli URI di richiesta agli endpoint e dell'invio di richieste in ingresso a tali endpoint. Le route sono definite nell'app e vengono configurate all'avvio dell'app. Una route può facoltativamente estrarre valori dall'URL contenuto nella richiesta e questi valori possono quindi essere usati per l'elaborazione della richiesta. Usando le informazioni sulla route dall'app, il routing è anche in grado di generare URL con mapping agli endpoint.

Per usare gli scenari di routing più recenti in ASP.NET Core 2.2, specificare la [versione di compatibilità](xref:mvc/compatibility-version) nella registrazione dei servizi MVC in `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

L'opzione <xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> determina se il routing deve usare internamente la logica basata sugli endpoint o la logica basata su <xref:Microsoft.AspNetCore.Routing.IRouter> di ASP.NET Core 2.1 o versioni precedenti. Quando la versione di compatibilità è impostata su 2.2 o versioni successive, il valore predefinito è `true`. Impostare il valore su `false` per usare la logica di routing precedente:

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

Per altre informazioni sul routing basato su <xref:Microsoft.AspNetCore.Routing.IRouter>, vedere la [versione per ASP.NET Core 2.1 di questo argomento](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).

> [!IMPORTANT]
> Questo documento descrive il routing di basso livello di ASP.NET Core. Per informazioni sul routing di ASP.NET Core MVC, vedere <xref:mvc/controllers/routing>. Per informazioni sulle convenzioni di routing in Razor Pages, vedere <xref:razor-pages/razor-pages-conventions>.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Nozioni fondamentali sul routing

La maggior parte delle app dovrebbe scegliere uno schema di routing semplice e descrittivo in modo che gli URL siano leggibili e significativi. La route convenzionale predefinita `{controller=Home}/{action=Index}/{id?}`:

* Supporta uno schema di routing semplice e descrittivo.
* È un punto iniziale utile per le app basate su interfaccia utente.

Gli sviluppatori solitamente aggiungono ulteriori route brevi nelle aree a traffico elevato dell'app in situazioni specifiche (ad esempio, endpoint di blog e e-commerce) usando il [routing degli attributi](xref:mvc/controllers/routing#attribute-routing) o route convenzionali dedicate.

Le API Web devono usare il routing degli attributi per modellare le funzionalità dell'app come set di risorse in cui le operazioni sono rappresentate da verbi HTTP. Questo significa che molte operazioni (ad esempio, GET, POST) sulla stessa risorsa logica useranno lo stesso URL. Il routing degli attributi offre un livello di controllo necessario per progettare con attenzione il layout dell'endpoint pubblico di un'API.

Le app Razor Pages usano il routing convenzionale predefinito per servire le risorse denominate nella cartella *Pagine* dell'app. Sono disponibili convenzioni aggiuntive che consentono di personalizzare il comportamento di routing di Razor Pages. Per altre informazioni, vedere <xref:razor-pages/index> e <xref:razor-pages/razor-pages-conventions>.

Il supporto della generazione di URL consente di sviluppare l'app senza URL hardcoded per collegare l'app. Il supporto consente di iniziare con una configurazione di routing di base e di modificare le route dopo aver determinato il layout delle risorse dell'app.

Il routing usa gli *endpoint* (`Endpoint`) per rappresentare gli endpoint logici in un'app.

Un endpoint definisce un delegato per l'elaborazione delle richieste e una raccolta di metadati arbitrari. I metadati vengono usati per implementare gli elementi trasversali in base ai criteri e la configurazione associata a ogni endpoint.

Il sistema di routing ha le caratteristiche seguenti:

* La sintassi del modello di route viene usata per definire le route con parametri di route in formato token.
* La configurazione degli endpoint basata su convenzioni e attributi è consentita.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> consente di determinare se un parametro URL contiene un valore valido per il vincolo di un endpoint specifico.
* I modelli di app, ad esempio MVC o Razor Pages, registrano tutti i relativi endpoint, che hanno un'implementazione stimabile degli scenari di routing.
* L'implementazione del routing determina le decisioni relative al routing nel punto desiderato della pipeline del middleware.
* Il middleware successivo a un middleware di routing può esaminare il risultato della decisione dell'endpoint del middleware di routing per un URI di richiesta specifico.
* È possibile enumerare tutti gli endpoint nell'app in un punto qualsiasi della pipeline del middleware.
* Un'app può usare il routing per generare URL, ad esempio per il reindirizzamento o i collegamenti, in base alle informazioni degli endpoint. In questo modo si evita di impostare gli URL come hardcoded ottimizzando la manutenibilità.
* La generazione degli URL è basata sugli indirizzi, che supportano l'estensibilità arbitraria:

  * L'API del generatore di collegamenti (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) può essere risolta ovunque tramite l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) per generare gli URL.
  * Se l'API del generatore di collegamenti non è disponibile tramite l'inserimento delle dipendenze, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offre metodi per la generazione degli URL.

> [!NOTE]
> Con il rilascio del routing degli endpoint in ASP.NET Core 2.2, il collegamento degli endpoint è limitato alle azioni o alle pagine di MVC o Razor Pages. Le espansioni delle funzionalità di collegamento degli endpoint sono previste per le versioni future.

Il routing è connesso alla pipeline [middleware](xref:fundamentals/middleware/index) per mezzo della classe <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>. [ASP.NET Core MVC](xref:mvc/overview) aggiunge il routing alla pipeline del middleware come parte della configurazione e gestisce il routing nelle app MVC e Razor Pages . Per informazioni sull'uso del routing come componente autonomo, vedere la sezione [Usare il middleware di routing](#use-routing-middleware).

### <a name="url-matching"></a>Corrispondenza URL

La corrispondenza dell'URL è il processo con cui il routing invia una richiesta in ingresso a un *endpoint*. Questo processo si basa sui dati presenti nel percorso URL, ma può essere esteso in modo da prendere in considerazione tutti i dati della richiesta. La possibilità di inviare le richieste a gestori separati è fondamentale per ridurre le dimensioni e la complessità di un'app.

Il sistema di routing nell'endpoint di routing è responsabile di tutte le decisioni relative all'invio. Poiché il middleware applica i criteri in base all'endpoint selezionato, è importante che qualsiasi decisione che può influire sull'invio o l'applicazione dei criteri di sicurezza venga effettuata all'interno del sistema di routing.

Quando viene eseguito il delegato dell'endpoint, le proprietà di [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) vengono impostate sui valori appropriati in base all'elaborazione della richiesta eseguita fino a quel punto.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) è un dizionario di *valori di route* prodotti dalla route. Questi valori in genere sono determinati dalla suddivisione in token dell'URL e possono essere usati per accettare l'input dell'utente o per prendere ulteriori decisioni riguardo all'invio all'interno dell'app.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) è un elenco proprietà dei dati aggiuntivi correlati alla route corrispondente. Gli elementi <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> vengono forniti per supportare l'associazione dei dati sullo stato con ogni route in modo che l'app sia in grado di prendere decisioni in base alla route corrispondente. Questi valori sono definiti dallo sviluppatore e **non** influiscono sul comportamento del routing in alcun modo. Inoltre, i valori accantonati in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) possono essere di qualsiasi tipo, a differenza dei valori [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) che devono essere convertibili in e da stringhe.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) è un elenco delle route che hanno preso parte alla corrispondenza corretta della richiesta. Le route possono essere annidate l'una nell'altra. La proprietà <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> rispecchia il percorso tramite l'albero logico di route che hanno prodotto una corrispondenza. In genere il primo elemento in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> è la raccolta di route e deve essere usato per la generazione di URL. L'ultimo elemento in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> è il gestore di route corrispondente.

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>Generazione di URL con LinkGenerator

La generazione di URL è il processo con cui il routing crea un percorso URL basato su un set di valori di route. Questo consente di avere una separazione logica tra gli endpoint e gli URL che vi accedono.

Il routing di endpoint include l'API del generatore di collegamenti (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>). <xref:Microsoft.AspNetCore.Routing.LinkGenerator> è un servizio singleton che può essere recuperato dall'inserimento delle dipendenze. L'API può essere usata all'esterno del contesto di una richiesta in esecuzione. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> e gli scenari di MVC basati su <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, ad esempio [helper tag](xref:mvc/views/tag-helpers/intro), helper HTML e [risultati delle azioni](xref:mvc/controllers/actions), usano il generatore di collegamenti per offrire le funzionalità di generazione di collegamenti.

Il generatore di collegamenti si basa sui concetti di *indirizzo* e di *schemi di indirizzi*. Lo schema di indirizzi consente di determinare gli endpoint che devono essere considerati per la generazione dei collegamenti. Ad esempio, gli scenari di nome route e valori di route noti a numerosi utenti in MVC o Razor Pages vengono implementati come schema di indirizzi.

Il generatore di collegamenti può collegarsi alle azioni e alle pagine MVC o Razor Pages tramite i metodi di estensione seguenti:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

Un overload di questi metodi accetta gli argomenti che includono `HttpContext`. Questi metodi sono funzionalmente equivalenti a `Url.Action` e `Url.Page` ma offrono una maggiore flessibilità e altre opzioni.

I metodi `GetPath*` sono più simili a `Url.Action` e `Url.Page` poiché generano un URI che contiene un percorso assoluto. I metodi `GetUri*` generano sempre un URI assoluto contenente uno schema e un host. I metodi che accettano `HttpContext` generano un URI nel contesto della richiesta in esecuzione. Se non vengono sovrascritti, vengono usati i valori di route di ambiente, il percorso base dell'URL, lo schema e l'host dalla richiesta in esecuzione.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> viene chiamato con un indirizzo. La generazione di un URI viene eseguita in due passaggi:

1. Un indirizzo viene associato a un elenco di endpoint che corrispondono all'indirizzo.
1. `RoutePattern` di ogni endpoint viene valutato fino a quando non viene individuato un formato di route che corrisponde ai valori specificati. L'output risultante viene unito alle altre parti dell'URI specificate nel generatore di collegamenti e restituito.

I metodi forniti da <xref:Microsoft.AspNetCore.Routing.LinkGenerator> supportano le funzionalità di generazione di collegamenti standard per tutti i tipi di indirizzi. È consigliabile usare il generatore di collegamenti tramite i metodi di estensione che eseguono le operazioni per un tipo di indirizzo specifico.

| Metodo di estensione   | Descrizione                                                         |
| ------------------ | ------------------------------------------------------------------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Genera un URI con un percorso assoluto in base ai valori specificati. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Genera un URI assoluto in base ai valori specificati.             |

> [!WARNING]
> Prestare attenzione alle implicazioni seguenti della chiamata ai metodi <xref:Microsoft.AspNetCore.Routing.LinkGenerator>:
>
> * Usare i metodi di estensione `GetUri*` con cautela in una configurazione di app che non convalida l'intestazione `Host` delle richieste in ingresso. Se l'intestazione `Host` delle richieste in ingresso non viene convalidata, l'input delle richieste non attendibili può essere inviato nuovamente al client negli URI di una visualizzazione o pagina. È consigliabile che in tutte le app di produzione il server sia configurato per la convalida dell'intestazione `Host` rispetto ai valori validi noti.
>
> * Usare <xref:Microsoft.AspNetCore.Routing.LinkGenerator> con cautela nel middleware in associazione a `Map` o `MapWhen`. `Map*` modifica il percorso di base della richiesta in esecuzione, che ha effetto sull'output della generazione di collegamenti. Tutte le API <xref:Microsoft.AspNetCore.Routing.LinkGenerator> consentono di specificare un percorso di base. Specificare sempre un percorso di base vuoto per annullare l'effetto di `Map*` sulla generazione di collegamenti.

## <a name="differences-from-earlier-versions-of-routing"></a>Differenze rispetto alle versioni precedenti del routing

Tra il routing di endpoint in ASP.NET Core 2.2 o versioni successive e le versioni precedenti del routing in ASP.NET Core esistono alcune differenze:

* Il sistema di routing di endpoint non supporta l'estensibilità basata su <xref:Microsoft.AspNetCore.Routing.IRouter>, inclusa l'eredità da <xref:Microsoft.AspNetCore.Routing.Route>.

* Il routing di endpoint non supporta [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim). Usare la [versione di compatibilità](xref:mvc/compatibility-version) 2.1 (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) per continuare a usare lo shim per la compatibilità.

* Il routing di endpoint ha un comportamento diverso per l'utilizzo delle maiuscole e delle minuscole degli URI generati quando si usano le route convenzionali.

  Esaminare il modello di route predefinito seguente:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Si supponga di generare un collegamento a un'azione usando la route seguente:

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  Con il routing basato su <xref:Microsoft.AspNetCore.Routing.IRouter>, questo codice genera un URI di `/blog/ReadPost/17`, che rispetta le maiuscole e le minuscole del valore di route specificato. Il routing di endpoint in ASP.NET Core 2.2 o versioni successive produce `/Blog/ReadPost/17` ("Blog" viene convertita in lettere maiuscole). Il routing di endpoint offre l'interfaccia `IOutboundParameterTransformer` che può essere usata per personalizzare questo comportamento globalmente o per applicare convenzioni diverse per il mapping degli URL.

  Per altre informazioni, vedere la sezione [Riferimento ai trasformatori di parametro](#parameter-transformer-reference).

* La generazione di collegamenti usata da MVC o Razor Pages con le route convenzionali ha un comportamento diverso quando si tenta il collegamento a un controller/azione o a una pagina non esistente.

  Esaminare il modello di route predefinito seguente:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Si supponga di generare un collegamento a un'azione usando il modello predefinito con il codice seguente:

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  Con il routing basato su `IRouter`, il risultato è sempre `/Blog/ReadPost/17`, anche se `BlogController` non è presente o non ha un metodo di azione `ReadPost`. Come previsto, il routing di endpoint in ASP.NET Core 2.2 o versioni successive produce `/Blog/ReadPost/17` se è presente il metodo di azione. *Tuttavia, il routing di endpoint produce una stringa vuota se l'azione non è presente.* Concettualmente, il routing di endpoint non presume che l'endpoint sia presente se l'azione non è presente.

* L'*algoritmo di invalidamento dei valori di ambiente* della generazione di collegamenti ha un comportamento diverso quando viene usato con il routing di endpoint.

  L'*invalidamento dei valori di ambiente* è l'algoritmo che decide quali valori di route della richiesta in esecuzione (valori di ambiente) possono essere usati nelle operazioni di generazione di collegamenti. Il routing convenzionale ha sempre invalidato i valori di route aggiuntivi durante il collegamento a un'azione diversa. Prima del rilascio di ASP.NET Core 2.2, il routing degli attributi non aveva questo comportamento. Nelle versioni precedenti di ASP.NET Core i collegamenti a un'altra azione con gli stessi nomi di parametro di route causavano errori nella generazione dei collegamenti. In ASP.NET Core 2.2 o versioni successive entrambi i tipi di routing invalidano i valori in caso di collegamento a un'altra azione.

  Esaminare l'esempio seguente in ASP.NET Core 2.1 o versioni precedenti. In caso di collegamento a un'altra azione o a un'altra pagina, i valori di route possono essere riutilizzati in modi indesiderati.

  In */Pages/Store/Product.cshtml*:

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  In */Pages/Login.cshtml*:

  ```cshtml
  @page "{id?}"
  ```

  Se l'URI è `/Store/Product/18` in ASP.NET Core 2.1 o versioni precedenti, il collegamento generato nella pagina Store/Info da `@Url.Page("/Login")` è `/Login/18`. Il valore `id` di 18 viene riutilizzato, anche se la destinazione del collegamento è una parte diversa dell'app. Il valore di route `id` nel contesto della pagina `/Login` è probabilmente un valore di ID utente, non un valore di ID prodotto del punto vendita.

  Nel routing di endpoint con ASP.NET Core 2.2 o versioni successive, il risultato è `/Login`. I valori di ambiente non vengono riutilizzati quando la destinazione collegata è costituita da un'azione o una pagina diversa.

* Sintassi dei parametri di route di round trip: le barre non vengono codificate quando viene usata una sintassi del parametro catch-all con doppio asterisco (`**`).

  Durante la generazione di collegamenti, il sistema di routing codifica il valore acquisito in un parametro catch-all con doppio asterisco (`**`) (ad esempio, `{**myparametername}`) escludendo le barre. Il parametro catch-all con doppio asterisco è supportato con il routing basato su `IRouter` in ASP.NET Core 2.2 o versioni successive.

  La sintassi del parametro catch-all con asterisco singolo nelle versioni precedenti di ASP.NET Core (`{*myparametername}`) resta supportata e le barre vengono codificate.

  | Route              | Collegamento generato con<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts` (la barra viene codificata)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Esempio di middleware

Nell'esempio seguente un middleware usa l'API <xref:Microsoft.AspNetCore.Routing.LinkGenerator> per creare il collegamento a un metodo di azione che elenca i prodotti del punto vendita. L'uso del generatore di collegamenti tramite l'inserimento in una classe e la chiamata a `GenerateLink` è possibile in tutte le classi di un'app.

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

### <a name="create-routes"></a>Creare le route

La maggior parte delle app crea le route chiamando <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> o uno dei metodi di estensione simili definiti per <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Tutti i metodi di estensione <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> creano un'istanza di <xref:Microsoft.AspNetCore.Routing.Route> e la aggiungono alla raccolta di route.

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> non accetta alcun parametro del gestore di route. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> aggiunge solo le route che sono gestite da <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Per altre informazioni sul routing in MVC, vedere <xref:mvc/controllers/routing>.

L'esempio di codice seguente si riferisce a una chiamata <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> usata da una tipica definizione di route di ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Questo modello confronta un percorso URL ed estrae i valori di route. Ad esempio, il percorso `/Products/Details/17` genera i valori di route seguenti: `{ controller = Products, action = Details, id = 17 }`.

I valori di route vengono determinati suddividendo il percorso URL in segmenti e confrontando ogni segmento con il nome del *parametro di route* nel modello di route. I parametri di route sono denominati. Parametri definiti racchiudendo il nome del parametro tra parentesi graffe `{ ... }`.

Il modello precedente può anche confrontare il percorso URL `/` e generare i valori `{ controller = Home, action = Index }`. Ciò accade perché i parametri di route `{controller}` e `{action}` hanno valori predefiniti e il parametro di route `id` è facoltativo. Un segno di uguale (`=`) seguito da un valore dopo il nome del parametro di route definisce un valore predefinito per il parametro. Un punto interrogativo (`?`) dopo il nome del parametro di route definisce un parametro facoltativo.

I parametri di route con un valore predefinito producono *sempre* un valore di route quando la route corrisponde. I parametri facoltativi non producono alcun valore di route se non esiste un segmento di percorso URL corrispondente. Vedere la sezione [Riferimento per i modelli di route](#route-template-reference) per una descrizione completa degli scenari e della sintassi del modello di route.

Nell'esempio seguente la definizione del parametro di route `{id:int}` definisce un [vincolo di route](#route-constraint-reference) per il parametro di route `id`:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Questo modello confronta un percorso URL come `/Products/Details/17`, ma non `/Products/Details/Apples`. I vincoli di route implementano <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> ed esaminano i valori di route per verificarli. In questo esempio il valore di route `id` deve poter essere convertito in un numero intero. Vedere [Riferimento per i vincoli di route](#route-constraint-reference) per una descrizione dei vincoli di route specificati dal framework.

Altri overload di <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accettano i valori per `constraints`, `dataTokens` e `defaults`. L'uso tipico di questi parametri è passare un oggetto tipizzato in modo anonimo, in cui i nomi di proprietà di tipo anonimo corrispondono ai nomi dei parametri di route.

Gli esempi <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> seguenti creano route equivalenti:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> La sintassi inline per definire i vincoli e i valori predefiniti può essere utile per le route semplici. Esistono tuttavia scenari, come i token di dati, che non sono supportati dalla sintassi inline.

L'esempio seguente illustra alcuni scenari aggiuntivi:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Il modello precedente confronta un percorso URL come `/Blog/All-About-Routing/Introduction` ed estrae i valori `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. I valori di route predefiniti per `controller` e `action` vengono generati dalla route anche se sono non presenti parametri di route corrispondenti nel modello. I valori predefiniti possono essere specificati nel modello di route. Il parametro di route `article` è definito come *catch-all* in base alla presenza di un doppio asterisco (`**`) prima del nome del parametro di route. I parametri di route catch-all acquisiscono il resto del percorso URL e possono anche corrispondere alla stringa vuota.

L'esempio seguente aggiunge i vincoli di route e i token di dati:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Il modello precedente confronta un percorso URL come `/en-US/Products/5` ed estrae i valori `{ controller = Products, action = Details, id = 5 }` e i token di dati `{ locale = en-US }`.

![Token della finestra Variabili locali](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Generazione di URL della classe di route

La classe <xref:Microsoft.AspNetCore.Routing.Route> è inoltre in grado di eseguire la generazione di URL combinando un set di valori di route con il relativo modello di route. Questo è logicamente il processo inverso di corrispondenza del percorso URL.

> [!TIP]
> Per comprendere meglio la generazione di URL, si pensi a quale URL si vuole generare e al modo in cui un modello di route deve corrispondere a tale URL. Quali valori devono essere prodotti? Questo equivale approssimativamente al funzionamento della generazione di URL nella classe <xref:Microsoft.AspNetCore.Routing.Route>.

L'esempio seguente usa una route predefinita di ASP.NET Core MVC generale:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Con i valori di route `{ controller = Products, action = List }`, viene generato l'URL `/Products/List`. I valori di route vengono sostituiti dai parametri di route corrispondenti per formare il percorso URL. Poiché `id` è un parametro di route facoltativo, l'URL è stato generato senza un valore per `id`.

Con i valori di route `{ controller = Home, action = Index }`, viene generato l'URL `/`. Poiché i valori di route specificati corrispondono ai valori predefiniti, i segmenti corrispondenti ai valori predefiniti possono essere omessi.

Per entrambi gli URL generati viene eseguito il round trip con la definizione di route seguente (`/Home/Index` e `/`) e vengono prodotti gli stessi valori di route usati per generare l'URL.

> [!NOTE]
> Un'app che usa ASP.NET Core MVC deve usare <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> per generare gli URL invece di eseguire la chiamata direttamente nel routing.

Per altre informazioni sulla generazione di URL, vedere la sezione [Riferimento per la generazione di URL](#url-generation-reference).

## <a name="use-routing-middleware"></a>Usare il middleware di routing

Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) nel file di progetto dell'app.

Aggiungere il routing al contenitore del servizio in `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Le route devono essere configurate nel metodo `Startup.Configure`. L'app di esempio usa le API seguenti:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Confronta solo le richieste HTTP GET.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

Nella tabella seguente sono elencate le risposte con gli URI specificati.

| URI                    | Risposta                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Hello! Route values: [operation, create], [id, 3] |
| `/package/track/-3`    | Hello! Route values: [operation, track], [id, -3] |
| `/package/track/-3/`   | Hello! Route values: [operation, track], [id, -3] |
| `/package/track/`      | La richiesta non viene eseguita, nessuna corrispondenza.              |
| `GET /hello/Joe`       | Hi, Joe!                                          |
| `POST /hello/Joe`      | La richiesta non viene eseguita, corrispondenza solo con HTTP GET. |
| `GET /hello/Joe/Smith` | La richiesta non viene eseguita, nessuna corrispondenza.              |

Il framework offre un set di metodi di estensione per la creazione di route (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

I metodi `Map[Verb]` usano i vincoli per limitare la route al verbo HTTP nel nome del metodo. Vedere ad esempio <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> e <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Riferimento per il modello di route

I token all'interno di parentesi graffe (`{ ... }`) definiscono i *parametri di route* che vengono associati se esiste una corrispondenza per la route. È possibile definire più parametri di route in un segmento di route, ma devono essere separati da un valore letterale. Ad esempio, `{controller=Home}{action=Index}` non è una route valida perché non è presente un valore letterale tra `{controller}` e `{action}`. Questi parametri di route devono avere un nome e possono avere attributi aggiuntivi.

Il testo letterale diverso dai parametri di route, ad esempio `{id}`, e il separatore di percorso `/` devono corrispondere al testo nell'URL. La corrispondenza del testo non fa distinzione tra maiuscole e minuscole e si basa sulla rappresentazione decodificata del percorso degli URL. Per verificare la corrispondenza di un delimitatore letterale dei parametri di route (`{` o `}`), eseguire l'escape del delimitatore ripetendo il carattere (`{{` o `}}`).

I modelli di URL che tentano di acquisire un nome file con un'estensione facoltativa hanno considerazioni aggiuntive. Considerare ad esempio il modello `files/{filename}.{ext?}`. Se esistono i valori per `filename` e `ext`, vengono popolati entrambi i valori. Se esiste solo un valore per `filename` nell'URL, la route corrisponde perché il punto finale (`.`) è facoltativo. Gli URL seguenti corrispondono a questa route:

* `/files/myFile.txt`
* `/files/myFile`

È possibile usare un asterisco (`*`) o un doppio asterisco (`**`) come prefisso per un parametro di route da associare alla parte rimanente dell'URI. Si tratta di parametri chiamati parametri *catch-all*. Ad esempio, `blog/{**slug}` corrisponde a qualsiasi URI che inizia con `/blog` seguito da qualsiasi valore, che viene assegnato al valore di route `slug`. I parametri catch-all possono anche corrispondere alla stringa vuota.

Il parametro catch-all esegue l'escape dei caratteri appropriati, incluso il separatore di percorso (`/`), quando la route viene usata per generare un URL. Ad esempio, la route `foo/{*path}` con i valori di route `{ path = "my/path" }` genera `foo/my%2Fpath`. Si noti la barra di escape. Per eseguire il round trip dei caratteri di separatore di percorso, usare il prefisso del parametro di route `**`. La route `foo/{**path}` con `{ path = "my/path" }` genera `foo/my/path`.

I parametri di route possono avere *valori predefiniti*, definiti specificando il valore predefinito dopo il nome del parametro, separato da un segno di uguale (`=`). Ad esempio, `{controller=Home}` definisce `Home` come valore predefinito per `controller`. Il valore predefinito viene usato se nell'URL non è presente alcun valore per il parametro. I parametri di route vengono resi facoltativi aggiungendo un punto interrogativo (`?`) alla fine del nome del parametro, come in `id?`. La differenza tra parametri facoltativi e parametri di route predefiniti è che un parametro di route con un valore predefinito produce sempre un valore, mentre un parametro facoltativo ha un valore solo se ne viene specificato uno dall'URL della richiesta.

I parametri di route possono presentare dei vincoli che devono corrispondere al valore di route associato dall'URL. Aggiungendo due punti (`:`) e il nome del vincolo dopo il nome del parametro di route, si specifica un *vincolo inline* per un parametro di route. Se il vincolo richiede argomenti, vengono racchiusi tra parentesi (`(...)`) dopo il nome del vincolo. È possibile specificare più vincoli inline aggiungendo di nuovo i due punti (`:`) e il nome del vincolo.

Il nome del vincolo e gli argomenti vengono passati al servizio <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> per creare un'istanza di <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> da usare nell'elaborazione dell'URL. Il modello di route `blog/{article:minlength(10)}` specifica ad esempio un vincolo `minlength` con l'argomento `10`. Per altre informazioni sui vincoli di route e per un elenco dei vincoli specificati dal framework, vedere la sezione [Riferimento per i vincoli di route](#route-constraint-reference).

I parametri di route possono avere anche trasformatori di parametro che trasformano un valore di parametro durante la generazione dei collegamenti e l'abbinamento delle azioni e delle pagine agli URI. Come i vincoli, i trasformatori di parametro possono essere aggiunti inline a un parametro di route inserendo due punti (`:`) e il nome del trasformatore dopo il nome del parametro di route. Ad esempio, il modello di route `blog/{article:slugify}` specifica un trasformatore `slugify`. Per altre informazioni sui trasformatori di parametro, vedere la sezione [Riferimento ai trasformatori di parametro](#parameter-transformer-reference).

La tabella seguente illustra i modelli di route di esempio e il relativo comportamento.

| Modello di route                           | URI corrispondente di esempio    | L'URI della richiesta&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Verifica la corrispondenza solo del singolo percorso `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Verifica la corrispondenza e imposta `Page` su `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Verifica la corrispondenza e imposta `Page` su `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Esegue il mapping al controller `Products` e all'azione `List`.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Esegue il mapping al controller `Products` e all'azione `Details` (`id` impostato su 123). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Esegue il mapping al controller `Home` e al metodo `Index` (`id` viene ignorato).        |

L'uso di un modello è in genere l'approccio più semplice al routing. I vincoli e le impostazioni predefinite possono essere specificati anche all'esterno del modello di route.

> [!TIP]
> Abilitare la [registrazione](xref:fundamentals/logging/index) per verificare in che modo le implementazioni del routing predefinite, ad esempio <xref:Microsoft.AspNetCore.Routing.Route>, corrispondono alle richieste.

## <a name="reserved-routing-names"></a>Nomi riservati di routing

Le parole chiave seguenti sono nomi riservati e non possono essere usate come nomi o parametri di route:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Riferimento per i vincoli di route

I vincoli di route vengono eseguiti quando si verifica una corrispondenza nell'URL in ingresso e il percorso URL viene suddiviso in valori di route in formato token. I vincoli di route in genere controllano il valore di route associato usando il modello di route e stabiliscono con una decisione Sì/No se il valore è accettabile o meno. Alcuni vincoli di route usano i dati all'esterno del valore di route per stabilire se la richiesta può essere instradata. Ad esempio, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> può accettare o rifiutare una richiesta in base al relativo verbo HTTP. I vincoli vengono usati nelle richieste del routing e nella generazione di collegamenti.

> [!WARNING]
> Non usare i vincoli per la **convalida dell'input**. Se vengono usati vincoli per la **convalida dell'input**, un input non valido causa la visualizzazione di un errore di tipo *404 (Non trovato)* invece di un errore di tipo *400 - Richiesta non valida* con un messaggio di errore appropriato. I vincoli di route vengono usati per evitare **ambiguità** tra route simili, non per convalidare gli input per una route specifica.

La tabella seguente illustra i vincoli di route di esempio e il relativo comportamento.

| vincolo | Esempio | Esempi di corrispondenza | Note |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Corrisponde a qualsiasi numero intero |
| `bool` | `{active:bool}` | `true`, `FALSE` | Corrisponde a `true` o `false` (senza distinzione tra maiuscole e minuscole) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Corrisponde a un valore `DateTime` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Corrisponde a un valore `decimal` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Corrisponde a un valore `double` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Corrisponde a un valore `float` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Corrisponde a un valore `Guid` valido |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Corrisponde a un valore `long` valido |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | La stringa deve contenere almeno 4 caratteri |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | La stringa non deve contenere più di 8 caratteri |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | La stringa deve contenere esattamente 12 caratteri |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | La stringa deve contenere almeno 8 e non più di 16 caratteri |
| `min(value)` | `{age:min(18)}` | `19` | Il valore intero deve essere almeno 18 |
| `max(value)` | `{age:max(120)}` | `91` | Il valore intero non deve essere superiore a 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Il valore intero deve essere almeno 18 ma non più di 120 |
| `alpha` | `{name:alpha}` | `Rick` | La stringa deve essere costituita da uno o più caratteri alfabetici (`a`-`z`, senza distinzione tra maiuscole e minuscole) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | La stringa deve corrispondere all'espressione regolare (vedere i suggerimenti per la definizione di un'espressione regolare) |
| `required` | `{name:required}` | `Rick` | Usato per imporre che un valore diverso da un parametro sia presente durante la generazione dell'URL |

Più vincoli delimitati da punti possono essere applicati a un singolo parametro. Ad esempio il vincolo seguente limita un parametro a un valore intero maggiore o uguale a 1:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> I vincoli di route che verificano l'URL e vengono convertiti in un tipo CLR, ad esempio `int` o `DateTime`, usano sempre le impostazioni cultura inglese non dipendenti da paese/area geografica, presupponendo che l'URL sia non localizzabile. I vincoli di route specificati dal framework non modificano i valori archiviati nei valori di route. Tutti i valori di route analizzati dall'URL vengono archiviati come stringhe. Ad esempio, il vincolo `float` prova a convertire il valore di route in un valore float, ma il valore convertito viene usato solo per verificare che può essere convertito in un valore float.

## <a name="regular-expressions"></a>Espressioni regolari

Il framework di ASP.NET Core aggiunge `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` al costruttore di espressioni regolari. Per una descrizione di questi membri, vedere <xref:System.Text.RegularExpressions.RegexOptions>.

Le espressioni regolari usano delimitatori e token simili a quelli usati dal routing e dal linguaggio C#. Per i token di espressione è necessario aggiungere caratteri di escape. Per usare l'espressione regolare `^\d{3}-\d{2}-\d{4}$` nel routing, l'espressione deve includere i caratteri `\` (barra rovesciata singola) specificati nella stringa come caratteri `\\` (barra rovesciata doppia) nel file di origine C# per eseguire l'escape del carattere di escape della stringa `\`, a meno che non si usino [valori letterali di stringa verbatim](/dotnet/csharp/language-reference/keywords/string). Per eseguire l'escape dii caratteri delimitatori dei parametri di routing (`{`, `}`, `[`, `]`), raddoppiare i caratteri nell'espressione (`{{`, `}`, `[[`, `]]`). La tabella seguente mostra un'espressione regolare e la versione dopo l'escape.

| Espressione regolare    | Espressione regolare con caratteri di escape     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Le espressioni regolari usate nel routing spesso iniziano con l'accento circonflesso (`^`) e corrispondono alla posizione iniziale della stringa. Le espressioni spesso terminano con il segno di dollaro (`$`) e corrispondono alla fine della stringa. I caratteri `^` e `$` consentono di verificare che l'espressione regolare corrisponda all'intero valore del parametro di route. Senza i caratteri `^` e `$` l'espressione regolare corrisponde a qualsiasi sottostringa all'interno della stringa e spesso questo non è il risultato desiderato. La tabella seguente include alcuni esempi e descrive perché si verifica o non si verifica la corrispondenza.

| Espressione   | Stringa    | Corrispondenza con | Commento               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | hello     | Yes   | Corrispondenze di sottostringhe     |
| `[a-z]{2}`   | 123abc456 | Yes   | Corrispondenze di sottostringhe     |
| `[a-z]{2}`   | mz        | Yes   | Corrisponde all'espressione    |
| `[a-z]{2}`   | MZ        | Yes   | Senza distinzione maiuscole/minuscole    |
| `^[a-z]{2}$` | hello     | No    | Vedere `^` e `$` sopra |
| `^[a-z]{2}$` | 123abc456 | No    | Vedere `^` e `$` sopra |

Per altre informazioni sulla sintassi delle espressioni regolari, vedere [Espressioni regolari di .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Per limitare un parametro a un set noto di valori possibili, usare un'espressione regolare. Ad esempio, `{action:regex(^(list|get|create)$)}` verifica la corrispondenza del valore di route `action` solo con `list`, `get` o `create`. Se viene passata nel dizionario di vincoli, la stringa `^(list|get|create)$` è equivalente. Anche i vincoli passati nel dizionario di vincoli (non inline all'interno di un modello) che non corrispondono a uno dei vincoli noti vengono considerati espressioni regolari.

## <a name="custom-route-constraints"></a>Vincoli di route personalizzati

Oltre ai vincoli di route predefiniti, è possibile creare vincoli di route personalizzati implementando l'interfaccia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>. L'interfaccia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> contiene un singolo metodo, `Match`, che restituisce `true` se il vincolo viene soddisfatto e `false` in caso contrario.

Per usare un'interfaccia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> personalizzata, il tipo di vincolo di route deve essere registrato con la proprietà <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> dell'app nel contenitore di servizi dell'app. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> è un dizionario che esegue il mapping delle chiavi dei vincoli di route alle implementazioni di <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> che convalidano tali vincoli. La proprietà <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> di un'app può essere aggiornata in `Startup.ConfigureServices` come parte di una chiamata [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) o configurando <xref:Microsoft.AspNetCore.Routing.RouteOptions> direttamente con `services.Configure<RouteOptions>`. Ad esempio:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

Il vincolo può quindi essere applicato alle route nel modo consueto, usando il nome specificato al momento della registrazione del tipo di vincolo. Ad esempio:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a>Riferimento ai trasformatori di parametro

I trasformatori di parametro:

* Vengono eseguiti quando si genera un collegamento per <xref:Microsoft.AspNetCore.Routing.Route>.
* Implementare `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Vengono configurati tramite <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Acquisire il valore della route del parametro e trasformarlo in un nuovo valore stringa.
* Il valore trasformato viene usato nel collegamento generato.

Ad esempio, un trasformatore di parametro `slugify` personalizzato nel modello di route `blog\{article:slugify}` con `Url.Action(new { article = "MyTestArticle" })` genera `blog\my-test-article`.

Per usare un trasformatore di parametro in un modello di route, configurarlo prima usando <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

I trasformatori di parametro sono usati dai framework per trasformare l'URI in cui viene risolto un endpoint. Ad esempio, ASP.NET Core MVC usa i trasformatori di parametro per trasformare il valore di route usato per abbinare `area`, `controller`, `action` e `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

Con la route precedente, l'azione `SubscriptionManagementController.GetAll()` viene abbinata all'URI `/subscription-management/get-all`. Un trasformatore di parametro non modifica i valori della route usati per generare un collegamento. Ad esempio, `Url.Action("GetAll", "SubscriptionManagement")` restituisce `/subscription-management/get-all`.

ASP.NET Core offre convenzioni API per l'uso di trasformatori di parametro con le route generate:

* ASP.NET Core MVC include la convenzione API `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention`. Questa convenzione applica un trasformatore di parametro specifico a tutte le route di attributi nell'app. Il trasformatore di parametro trasforma i token di route di attributi man mano che vengono sostituiti. Per altre informazioni vedere [Use a parameter transformer to customize token replacement](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement) (Usare un trasformatore di parametro per personalizzare la sostituzione di token).
* Razor Pages include la convenzione API `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention`. Questa convenzione applica un trasformatore di parametro specificato a tutte le istanze di Razor Pages individuate automaticamente. Il trasformatore di parametro trasforma la cartella e i segmenti di nome file delle route di Razor Pages. Per altre informazioni, vedere [Use a parameter transformer to customize page routes](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes) (Usare un trasformatore di parametro per personalizzare route di pagine).

## <a name="url-generation-reference"></a>Riferimento per la generazione di URL

L'esempio seguente mostra come generare un collegamento a una route usando un dizionario di valori di route e un elemento <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

L'elemento <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generato alla fine dell'esempio precedente è `/package/create/123`. Il dizionario fornisce i valori di route `operation` e `id` del modello "Track Package Route", `package/{operation}/{id}`. Per informazioni dettagliate, vedere il codice di esempio nella sezione [Usare il middleware di routing](#use-routing-middleware) oppure l'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Il secondo parametro per il costruttore <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> è una raccolta di *valori di ambiente*. I valori di ambiente sono utili poiché limitano il numero di valori che uno sviluppatore deve specificare all'interno del contesto di una richiesta. I valori di route correnti della richiesta corrente sono considerati valori di ambiente per la generazione del collegamento. Nell'azione `About` di un'app ASP.NET Core MVC di `HomeController`, non è necessario specificare il valore di route del controller per collegarsi all'azione `Index`. Viene usato il valore di ambiente `Home`.

I valori di ambiente che non corrispondono a un parametro vengono ignorati. I valori di ambiente vengono ignorati anche quando un valore specificato in modo esplicito esegue l'override del valore di ambiente. La corrispondenza si verifica da sinistra a destra nell'URL.

I valori specificati in modo esplicito ma che non corrispondono a un segmento della route vengono aggiunti alla stringa di query. La tabella seguente illustra il risultato ottenuto quando si usa il modello di route `{controller}/{action}/{id?}`.

| Valori di ambiente                     | Valori espliciti                        | Risultato                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |

Se una route ha un valore predefinito che non corrisponde a un parametro e tale valore viene specificato in modo esplicito, deve corrispondere al valore predefinito:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

La generazione dei collegamenti genera un collegamento per questa route quando vengono specificati i valori corrispondenti per `controller` e `action`.

## <a name="complex-segments"></a>Segmenti complessi

I segmenti complessi (ad esempio, `[Route("/x{token}y")]`), vengono elaborati individuando corrispondenze per i valori letterali da destra a sinistra in modalità non-greedy. Vedere [questo codice](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) per una spiegazione dettagliata di come vengono confrontati i segmenti complessi. L'[esempio di codice](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) non viene usato da ASP.NET Core, ma offre una spiegazione esauriente dei segmenti complessi.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Il routing è responsabile del mapping degli URI delle richieste ai gestori di route e dell'invio delle richieste in ingresso. Le route sono definite nell'app e vengono configurate all'avvio dell'app. Una route può facoltativamente estrarre valori dall'URL contenuto nella richiesta e questi valori possono quindi essere usati per l'elaborazione della richiesta. Con l'uso di route configurate dall'app, il routing è in grado di generare gli URL che eseguono il mapping ai gestori di route.

Per usare gli scenari di routing più recenti in ASP.NET Core 2.1, specificare la [versione di compatibilità](xref:mvc/compatibility-version) nella registrazione dei servizi MVC in `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

> [!IMPORTANT]
> Questo documento descrive il routing di basso livello di ASP.NET Core. Per informazioni sul routing di ASP.NET Core MVC, vedere <xref:mvc/controllers/routing>. Per informazioni sulle convenzioni di routing in Razor Pages, vedere <xref:razor-pages/razor-pages-conventions>.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Nozioni fondamentali sul routing

La maggior parte delle app dovrebbe scegliere uno schema di routing semplice e descrittivo in modo che gli URL siano leggibili e significativi. La route convenzionale predefinita `{controller=Home}/{action=Index}/{id?}`:

* Supporta uno schema di routing semplice e descrittivo.
* È un punto iniziale utile per le app basate su interfaccia utente.

Gli sviluppatori solitamente aggiungono ulteriori route brevi nelle aree a traffico elevato dell'app in situazioni specifiche (ad esempio, endpoint di blog e e-commerce) usando il [routing degli attributi](xref:mvc/controllers/routing#attribute-routing) o route convenzionali dedicate.

Le API Web devono usare il routing degli attributi per modellare le funzionalità dell'app come set di risorse in cui le operazioni sono rappresentate da verbi HTTP. Questo significa che molte operazioni (ad esempio, GET, POST) sulla stessa risorsa logica useranno lo stesso URL. Il routing degli attributi offre un livello di controllo necessario per progettare con attenzione il layout dell'endpoint pubblico di un'API.

Le app Razor Pages usano il routing convenzionale predefinito per servire le risorse denominate nella cartella *Pagine* dell'app. Sono disponibili convenzioni aggiuntive che consentono di personalizzare il comportamento di routing di Razor Pages. Per altre informazioni, vedere <xref:razor-pages/index> e <xref:razor-pages/razor-pages-conventions>.

Il supporto della generazione di URL consente di sviluppare l'app senza URL hardcoded per collegare l'app. Il supporto consente di iniziare con una configurazione di routing di base e di modificare le route dopo aver determinato il layout delle risorse dell'app.

Il routing usa le *route* (implementazioni di <xref:Microsoft.AspNetCore.Routing.IRouter>) per:

* Eseguire il mapping di richieste in ingresso ai *gestori di route*.
* Generare gli URL usati nelle risposte.

Per impostazione predefinita, un'app ha una sola raccolta di route. Quando arriva una richiesta, le route della raccolta vengono elaborate nell'ordine in cui si trovano nella raccolta. Il framework tenta di associare l'URL di una richiesta in ingresso a una route della raccolta chiamando il metodo <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> in ogni route della raccolta. Una risposta può usare il routing per generare gli URL, ad esempio per il reindirizzamento o i collegamenti, in base alle informazioni delle route. In questo modo si evita di impostare gli URL come hardcoded ottimizzando la manutenibilità.

Il sistema di routing ha le caratteristiche seguenti:

* La sintassi del modello di route viene usata per definire le route con parametri di route in formato token.
* La configurazione degli endpoint basata su convenzioni e attributi è consentita.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> consente di determinare se un parametro URL contiene un valore valido per il vincolo di un endpoint specifico.
* I modelli di app, ad esempio MVC o Razor Pages, registrano tutte le relative route, che hanno un'implementazione stimabile degli scenari di routing.
* Una risposta può usare il routing per generare gli URL, ad esempio per il reindirizzamento o i collegamenti, in base alle informazioni delle route. In questo modo si evita di impostare gli URL come hardcoded ottimizzando la manutenibilità.
* La generazione degli URL è basata sulle route, che supportano l'estensibilità arbitraria. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offre metodi per generare gli URL.

Il routing è connesso alla pipeline [middleware](xref:fundamentals/middleware/index) per mezzo della classe <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>. [ASP.NET Core MVC](xref:mvc/overview) aggiunge il routing alla pipeline del middleware come parte della configurazione e gestisce il routing nelle app MVC e Razor Pages . Per informazioni sull'uso del routing come componente autonomo, vedere la sezione [Usare il middleware di routing](#use-routing-middleware).

### <a name="url-matching"></a>Corrispondenza URL

La corrispondenza dell'URL è il processo con cui il routing invia una richiesta in ingresso a un *gestore*. Questo processo si basa sui dati presenti nel percorso URL, ma può essere esteso in modo da prendere in considerazione tutti i dati della richiesta. La possibilità di inviare le richieste a gestori separati è fondamentale per ridurre le dimensioni e la complessità di un'app.

Le richieste in ingresso immettono <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>, che chiama il metodo <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> per ogni route della sequenza. L'istanza di <xref:Microsoft.AspNetCore.Routing.IRouter> sceglie se *gestire* la richiesta impostando [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) su un valore <xref:Microsoft.AspNetCore.Http.RequestDelegate> non Null. Se una route imposta un gestore per la richiesta, l'elaborazione della route si interrompe e viene richiamato il gestore per elaborare la richiesta. Se non viene individuato alcun gestore di route per elaborare la richiesta, il middleware passa la richiesta al middleware successivo nella pipeline delle richieste.

L'input principale per <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> è l'elemento [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) associato alla richiesta corrente. [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) e [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) sono gli output che vengono impostati quando una route corrisponde.

Una corrispondenza che chiama <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> imposta anche le proprietà di [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) sui valori appropriati in base all'elaborazione della richiesta eseguita fino a quel punto.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) è un dizionario di *valori di route* prodotti dalla route. Questi valori in genere sono determinati dalla suddivisione in token dell'URL e possono essere usati per accettare l'input dell'utente o per prendere ulteriori decisioni riguardo all'invio all'interno dell'app.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) è un elenco proprietà dei dati aggiuntivi correlati alla route corrispondente. Gli elementi <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> vengono forniti per supportare l'associazione dei dati sullo stato con ogni route in modo che l'app sia in grado di prendere decisioni in base alla route corrispondente. Questi valori sono definiti dallo sviluppatore e **non** influiscono sul comportamento del routing in alcun modo. Inoltre, i valori accantonati in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) possono essere di qualsiasi tipo, a differenza dei valori [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) che devono essere convertibili in e da stringhe.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) è un elenco delle route che hanno preso parte alla corrispondenza corretta della richiesta. Le route possono essere annidate l'una nell'altra. La proprietà <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> rispecchia il percorso tramite l'albero logico di route che hanno prodotto una corrispondenza. In genere il primo elemento in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> è la raccolta di route e deve essere usato per la generazione di URL. L'ultimo elemento in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> è il gestore di route corrispondente.

<a name="lg"></a>

### <a name="url-generation"></a>Generazione di URL

La generazione di URL è il processo con cui il routing crea un percorso URL basato su un set di valori di route. Questo consente di avere una separazione logica tra i gestori di route e gli URL che vi accedono.

La generazione di URL segue un processo iterativo simile, ma inizia con la chiamata del codice dell'utente o del framework nel metodo <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> della raccolta di route. Per ogni *route* viene chiamato in sequenza il metodo <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> finché non viene restituito un valore <xref:Microsoft.AspNetCore.Routing.VirtualPathData> non Null.

Gli input primari per <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> sono:

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

Le route usano principalmente i valori di route specificati da <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> e <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> per decidere se è possibile generare un URL e quali valori includere. Gli elementi <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> sono il set di valori di route prodotti dalla corrispondenza della richiesta corrente. Al contrario, gli elementi <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> sono i valori di route che specificano in che modo viene generato l'URL per l'operazione corrente. <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext> viene specificato nel caso in cui una route ottenga servizi o dati aggiuntivi associati al contesto corrente.

> [!TIP]
> Considerare [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) come un set di override per [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). La generazione di URL prova a usare nuovamente i valori di route della richiesta corrente per generare URL per i collegamenti che usano la stessa route o gli stessi valori di route.

L'output di <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> è un oggetto <xref:Microsoft.AspNetCore.Routing.VirtualPathData>. <xref:Microsoft.AspNetCore.Routing.VirtualPathData> è un elemento parallelo di <xref:Microsoft.AspNetCore.Routing.RouteData>. <xref:Microsoft.AspNetCore.Routing.VirtualPathData> contiene l'elemento <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> per l'URL di output e alcune proprietà aggiuntive che devono essere impostate dalla route.

La proprietà [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) contiene il *percorso virtuale* prodotto dalla route. In base alle esigenze specifiche, può essere necessario elaborare ulteriormente il percorso. Se si vuole eseguire il rendering in HTML dell'URL generato, anteporre il percorso di base dell'app.

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) è un riferimento alla route che ha generato correttamente l'URL.

[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) è un dizionario di dati aggiuntivi correlati alla route che ha generato l'URL. È l'elemento parallelo di [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

### <a name="create-routes"></a>Creare le route

Il routing specifica la classe <xref:Microsoft.AspNetCore.Routing.Route> come implementazione standard di <xref:Microsoft.AspNetCore.Routing.IRouter>. <xref:Microsoft.AspNetCore.Routing.Route> usa la sintassi del *modello di route* per definire i criteri in base ai quali confrontare il percorso URL quando si chiama <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*>. <xref:Microsoft.AspNetCore.Routing.Route> usa lo stesso modello di route per generare un URL quando si chiama <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*>.

La maggior parte delle app crea le route chiamando <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> o uno dei metodi di estensione simili definiti per <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Tutti i metodi di estensione <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> creano un'istanza di <xref:Microsoft.AspNetCore.Routing.Route> e la aggiungono alla raccolta di route.

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> non accetta alcun parametro del gestore di route. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> aggiunge solo le route che sono gestite da <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Il gestore predefinito è `IRouter` e il gestore potrebbe non gestire la richiesta. Ad esempio, ASP.NET Core MVC viene in genere configurato come un gestore predefinito che gestisce solo le richieste che corrispondono a un'azione e un controller disponibili. Per altre informazioni sul routing in MVC, vedere <xref:mvc/controllers/routing>.

L'esempio di codice seguente si riferisce a una chiamata <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> usata da una tipica definizione di route di ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Questo modello confronta un percorso URL ed estrae i valori di route. Ad esempio, il percorso `/Products/Details/17` genera i valori di route seguenti: `{ controller = Products, action = Details, id = 17 }`.

I valori di route vengono determinati suddividendo il percorso URL in segmenti e confrontando ogni segmento con il nome del *parametro di route* nel modello di route. I parametri di route sono denominati. Parametri definiti racchiudendo il nome del parametro tra parentesi graffe `{ ... }`.

Il modello precedente può anche confrontare il percorso URL `/` e generare i valori `{ controller = Home, action = Index }`. Ciò accade perché i parametri di route `{controller}` e `{action}` hanno valori predefiniti e il parametro di route `id` è facoltativo. Un segno di uguale (`=`) seguito da un valore dopo il nome del parametro di route definisce un valore predefinito per il parametro. Un punto interrogativo (`?`) dopo il nome del parametro di route definisce un parametro facoltativo.

I parametri di route con un valore predefinito producono *sempre* un valore di route quando la route corrisponde. I parametri facoltativi non producono alcun valore di route se non esiste un segmento di percorso URL corrispondente. Vedere la sezione [Riferimento per i modelli di route](#route-template-reference) per una descrizione completa degli scenari e della sintassi del modello di route.

Nell'esempio seguente la definizione del parametro di route `{id:int}` definisce un [vincolo di route](#route-constraint-reference) per il parametro di route `id`:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Questo modello confronta un percorso URL come `/Products/Details/17`, ma non `/Products/Details/Apples`. I vincoli di route implementano <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> ed esaminano i valori di route per verificarli. In questo esempio il valore di route `id` deve poter essere convertito in un numero intero. Vedere [Riferimento per i vincoli di route](#route-constraint-reference) per una descrizione dei vincoli di route specificati dal framework.

Altri overload di <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accettano i valori per `constraints`, `dataTokens` e `defaults`. L'uso tipico di questi parametri è passare un oggetto tipizzato in modo anonimo, in cui i nomi di proprietà di tipo anonimo corrispondono ai nomi dei parametri di route.

Gli esempi <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> seguenti creano route equivalenti:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> La sintassi inline per definire i vincoli e i valori predefiniti può essere utile per le route semplici. Esistono tuttavia scenari, come i token di dati, che non sono supportati dalla sintassi inline.

L'esempio seguente illustra alcuni scenari aggiuntivi:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Il modello precedente confronta un percorso URL come `/Blog/All-About-Routing/Introduction` ed estrae i valori `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. I valori di route predefiniti per `controller` e `action` vengono generati dalla route anche se sono non presenti parametri di route corrispondenti nel modello. I valori predefiniti possono essere specificati nel modello di route. Il parametro di route `article` è definito come *catch-all* in base alla presenza di un asterisco (`*`) prima del nome del parametro di route. I parametri di route catch-all acquisiscono il resto del percorso URL e possono anche corrispondere alla stringa vuota.

L'esempio seguente aggiunge i vincoli di route e i token di dati:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Il modello precedente confronta un percorso URL come `/en-US/Products/5` ed estrae i valori `{ controller = Products, action = Details, id = 5 }` e i token di dati `{ locale = en-US }`.

![Token della finestra Variabili locali](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Generazione di URL della classe di route

La classe <xref:Microsoft.AspNetCore.Routing.Route> è inoltre in grado di eseguire la generazione di URL combinando un set di valori di route con il relativo modello di route. Questo è logicamente il processo inverso di corrispondenza del percorso URL.

> [!TIP]
> Per comprendere meglio la generazione di URL, si pensi a quale URL si vuole generare e al modo in cui un modello di route deve corrispondere a tale URL. Quali valori devono essere prodotti? Questo equivale approssimativamente al funzionamento della generazione di URL nella classe <xref:Microsoft.AspNetCore.Routing.Route>.

L'esempio seguente usa una route predefinita di ASP.NET Core MVC generale:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Con i valori di route `{ controller = Products, action = List }`, viene generato l'URL `/Products/List`. I valori di route vengono sostituiti dai parametri di route corrispondenti per formare il percorso URL. Poiché `id` è un parametro di route facoltativo, l'URL è stato generato senza un valore per `id`.

Con i valori di route `{ controller = Home, action = Index }`, viene generato l'URL `/`. Poiché i valori di route specificati corrispondono ai valori predefiniti, i segmenti corrispondenti ai valori predefiniti possono essere omessi.

Per entrambi gli URL generati viene eseguito il round trip con la definizione di route seguente (`/Home/Index` e `/`) e vengono prodotti gli stessi valori di route usati per generare l'URL.

> [!NOTE]
> Un'app che usa ASP.NET Core MVC deve usare <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> per generare gli URL invece di eseguire la chiamata direttamente nel routing.

Per altre informazioni sulla generazione di URL, vedere la sezione [Riferimento per la generazione di URL](#url-generation-reference).

## <a name="use-routing-middleware"></a>Usare il middleware di routing

Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) nel file di progetto dell'app.

Aggiungere il routing al contenitore del servizio in `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Le route devono essere configurate nel metodo `Startup.Configure`. L'app di esempio usa le API seguenti:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Confronta solo le richieste HTTP GET.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

Nella tabella seguente sono elencate le risposte con gli URI specificati.

| URI                    | Risposta                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Hello! Route values: [operation, create], [id, 3] |
| `/package/track/-3`    | Hello! Route values: [operation, track], [id, -3] |
| `/package/track/-3/`   | Hello! Route values: [operation, track], [id, -3] |
| `/package/track/`      | La richiesta non viene eseguita, nessuna corrispondenza.              |
| `GET /hello/Joe`       | Hi, Joe!                                          |
| `POST /hello/Joe`      | La richiesta non viene eseguita, corrispondenza solo con HTTP GET. |
| `GET /hello/Joe/Smith` | La richiesta non viene eseguita, nessuna corrispondenza.              |

Se si sta configurando una singola route, chiamare <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> passando un'istanza di `IRouter`. Non è necessario usare <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

Il framework offre un set di metodi di estensione per la creazione di route (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

Alcuni dei metodi elencati, ad esempio <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>, richiedono <xref:Microsoft.AspNetCore.Http.RequestDelegate>. <xref:Microsoft.AspNetCore.Http.RequestDelegate> viene usato come *gestore di route* quando la route corrisponde. Altri metodi di questa famiglia consentono di configurare una pipeline middleware che verrà usata come gestore di route. Se il metodo `Map*` non accetta un gestore, ad esempio <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>, il metodo usa <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

I metodi `Map[Verb]` usano i vincoli per limitare la route al verbo HTTP nel nome del metodo. Vedere ad esempio <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> e <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Riferimento per il modello di route

I token all'interno di parentesi graffe (`{ ... }`) definiscono i *parametri di route* che vengono associati se esiste una corrispondenza per la route. È possibile definire più parametri di route in un segmento di route, ma devono essere separati da un valore letterale. Ad esempio, `{controller=Home}{action=Index}` non è una route valida perché non è presente un valore letterale tra `{controller}` e `{action}`. Questi parametri di route devono avere un nome e possono avere attributi aggiuntivi.

Il testo letterale diverso dai parametri di route, ad esempio `{id}`, e il separatore di percorso `/` devono corrispondere al testo nell'URL. La corrispondenza del testo non fa distinzione tra maiuscole e minuscole e si basa sulla rappresentazione decodificata del percorso degli URL. Per verificare la corrispondenza di un delimitatore letterale dei parametri di route (`{` o `}`), eseguire l'escape del delimitatore ripetendo il carattere (`{{` o `}}`).

I modelli di URL che tentano di acquisire un nome file con un'estensione facoltativa hanno considerazioni aggiuntive. Considerare ad esempio il modello `files/{filename}.{ext?}`. Se esistono i valori per `filename` e `ext`, vengono popolati entrambi i valori. Se esiste solo un valore per `filename` nell'URL, la route corrisponde perché il punto finale (`.`) è facoltativo. Gli URL seguenti corrispondono a questa route:

* `/files/myFile.txt`
* `/files/myFile`

È possibile usare l'asterisco (`*`) come prefisso per un parametro di route da associare alla parte rimanente dell'URI, ovvero un parametro *catch-all*. Ad esempio, `blog/{*slug}` corrisponde a qualsiasi URI che inizia con `/blog` seguito da qualsiasi valore, che viene assegnato al valore di route `slug`. I parametri catch-all possono anche corrispondere alla stringa vuota.

Il parametro catch-all esegue l'escape dei caratteri appropriati, incluso il separatore di percorso (`/`), quando la route viene usata per generare un URL. Ad esempio, la route `foo/{*path}` con i valori di route `{ path = "my/path" }` genera `foo/my%2Fpath`. Si noti la barra di escape.

I parametri di route possono avere *valori predefiniti*, definiti specificando il valore predefinito dopo il nome del parametro, separato da un segno di uguale (`=`). Ad esempio, `{controller=Home}` definisce `Home` come valore predefinito per `controller`. Il valore predefinito viene usato se nell'URL non è presente alcun valore per il parametro. I parametri di route vengono resi facoltativi aggiungendo un punto interrogativo (`?`) alla fine del nome del parametro, come in `id?`. La differenza tra parametri facoltativi e parametri di route predefiniti è che un parametro di route con un valore predefinito produce sempre un valore, mentre un parametro facoltativo ha un valore solo se ne viene specificato uno dall'URL della richiesta.

I parametri di route possono presentare dei vincoli che devono corrispondere al valore di route associato dall'URL. Aggiungendo due punti (`:`) e il nome del vincolo dopo il nome del parametro di route, si specifica un *vincolo inline* per un parametro di route. Se il vincolo richiede argomenti, vengono racchiusi tra parentesi (`(...)`) dopo il nome del vincolo. È possibile specificare più vincoli inline aggiungendo di nuovo i due punti (`:`) e il nome del vincolo.

Il nome del vincolo e gli argomenti vengono passati al servizio <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> per creare un'istanza di <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> da usare nell'elaborazione dell'URL. Il modello di route `blog/{article:minlength(10)}` specifica ad esempio un vincolo `minlength` con l'argomento `10`. Per altre informazioni sui vincoli di route e per un elenco dei vincoli specificati dal framework, vedere la sezione [Riferimento per i vincoli di route](#route-constraint-reference).

La tabella seguente illustra i modelli di route di esempio e il relativo comportamento.

| Modello di route                           | URI corrispondente di esempio    | L'URI della richiesta&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Verifica la corrispondenza solo del singolo percorso `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Verifica la corrispondenza e imposta `Page` su `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Verifica la corrispondenza e imposta `Page` su `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Esegue il mapping al controller `Products` e all'azione `List`.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Esegue il mapping al controller `Products` e all'azione `Details` (`id` impostato su 123). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Esegue il mapping al controller `Home` e al metodo `Index` (`id` viene ignorato).        |

L'uso di un modello è in genere l'approccio più semplice al routing. I vincoli e le impostazioni predefinite possono essere specificati anche all'esterno del modello di route.

> [!TIP]
> Abilitare la [registrazione](xref:fundamentals/logging/index) per verificare in che modo le implementazioni del routing predefinite, ad esempio <xref:Microsoft.AspNetCore.Routing.Route>, corrispondono alle richieste.

## <a name="reserved-routing-names"></a>Nomi riservati di routing

Le parole chiave seguenti sono nomi riservati e non possono essere usate come nomi o parametri di route:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Riferimento per i vincoli di route

I vincoli di route vengono eseguiti quando si verifica una corrispondenza nell'URL in ingresso e il percorso URL viene suddiviso in valori di route in formato token. I vincoli di route in genere controllano il valore di route associato usando il modello di route e stabiliscono con una decisione Sì/No se il valore è accettabile o meno. Alcuni vincoli di route usano i dati all'esterno del valore di route per stabilire se la richiesta può essere instradata. Ad esempio, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> può accettare o rifiutare una richiesta in base al relativo verbo HTTP. I vincoli vengono usati nelle richieste del routing e nella generazione di collegamenti.

> [!WARNING]
> Non usare i vincoli per la **convalida dell'input**. Se vengono usati vincoli per la **convalida dell'input**, un input non valido causa la visualizzazione di un errore di tipo *404 (Non trovato)* invece di un errore di tipo *400 - Richiesta non valida* con un messaggio di errore appropriato. I vincoli di route vengono usati per evitare **ambiguità** tra route simili, non per convalidare gli input per una route specifica.

La tabella seguente illustra i vincoli di route di esempio e il relativo comportamento.

| vincolo | Esempio | Esempi di corrispondenza | Note |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Corrisponde a qualsiasi numero intero |
| `bool` | `{active:bool}` | `true`, `FALSE` | Corrisponde a `true` o `false` (senza distinzione tra maiuscole e minuscole) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Corrisponde a un valore `DateTime` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Corrisponde a un valore `decimal` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Corrisponde a un valore `double` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Corrisponde a un valore `float` valido (impostazioni cultura inglese non dipendenti da paese/area geografica, vedere avviso) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Corrisponde a un valore `Guid` valido |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Corrisponde a un valore `long` valido |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | La stringa deve contenere almeno 4 caratteri |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | La stringa non deve contenere più di 8 caratteri |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | La stringa deve contenere esattamente 12 caratteri |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | La stringa deve contenere almeno 8 e non più di 16 caratteri |
| `min(value)` | `{age:min(18)}` | `19` | Il valore intero deve essere almeno 18 |
| `max(value)` | `{age:max(120)}` | `91` | Il valore intero non deve essere superiore a 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Il valore intero deve essere almeno 18 ma non più di 120 |
| `alpha` | `{name:alpha}` | `Rick` | La stringa deve essere costituita da uno o più caratteri alfabetici (`a`-`z`, senza distinzione tra maiuscole e minuscole) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | La stringa deve corrispondere all'espressione regolare (vedere i suggerimenti per la definizione di un'espressione regolare) |
| `required` | `{name:required}` | `Rick` | Usato per imporre che un valore diverso da un parametro sia presente durante la generazione dell'URL |

Più vincoli delimitati da punti possono essere applicati a un singolo parametro. Ad esempio il vincolo seguente limita un parametro a un valore intero maggiore o uguale a 1:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> I vincoli di route che verificano l'URL e vengono convertiti in un tipo CLR, ad esempio `int` o `DateTime`, usano sempre le impostazioni cultura inglese non dipendenti da paese/area geografica, presupponendo che l'URL sia non localizzabile. I vincoli di route specificati dal framework non modificano i valori archiviati nei valori di route. Tutti i valori di route analizzati dall'URL vengono archiviati come stringhe. Ad esempio, il vincolo `float` prova a convertire il valore di route in un valore float, ma il valore convertito viene usato solo per verificare che può essere convertito in un valore float.

## <a name="regular-expressions"></a>Espressioni regolari

Il framework di ASP.NET Core aggiunge `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` al costruttore di espressioni regolari. Per una descrizione di questi membri, vedere <xref:System.Text.RegularExpressions.RegexOptions>.

Le espressioni regolari usano delimitatori e token simili a quelli usati dal routing e dal linguaggio C#. Per i token di espressione è necessario aggiungere caratteri di escape. Per usare l'espressione regolare `^\d{3}-\d{2}-\d{4}$` nel routing, l'espressione deve includere i caratteri `\` (barra rovesciata singola) specificati nella stringa come caratteri `\\` (barra rovesciata doppia) nel file di origine C# per eseguire l'escape del carattere di escape della stringa `\`, a meno che non si usino [valori letterali di stringa verbatim](/dotnet/csharp/language-reference/keywords/string). Per eseguire l'escape dii caratteri delimitatori dei parametri di routing (`{`, `}`, `[`, `]`), raddoppiare i caratteri nell'espressione (`{{`, `}`, `[[`, `]]`). La tabella seguente mostra un'espressione regolare e la versione dopo l'escape.

| Espressione regolare    | Espressione regolare con caratteri di escape     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Le espressioni regolari usate nel routing spesso iniziano con l'accento circonflesso (`^`) e corrispondono alla posizione iniziale della stringa. Le espressioni spesso terminano con il segno di dollaro (`$`) e corrispondono alla fine della stringa. I caratteri `^` e `$` consentono di verificare che l'espressione regolare corrisponda all'intero valore del parametro di route. Senza i caratteri `^` e `$` l'espressione regolare corrisponde a qualsiasi sottostringa all'interno della stringa e spesso questo non è il risultato desiderato. La tabella seguente include alcuni esempi e descrive perché si verifica o non si verifica la corrispondenza.

| Espressione   | Stringa    | Corrispondenza con | Commento               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | hello     | Yes   | Corrispondenze di sottostringhe     |
| `[a-z]{2}`   | 123abc456 | Yes   | Corrispondenze di sottostringhe     |
| `[a-z]{2}`   | mz        | Yes   | Corrisponde all'espressione    |
| `[a-z]{2}`   | MZ        | Yes   | Senza distinzione maiuscole/minuscole    |
| `^[a-z]{2}$` | hello     | No    | Vedere `^` e `$` sopra |
| `^[a-z]{2}$` | 123abc456 | No    | Vedere `^` e `$` sopra |

Per altre informazioni sulla sintassi delle espressioni regolari, vedere [Espressioni regolari di .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Per limitare un parametro a un set noto di valori possibili, usare un'espressione regolare. Ad esempio, `{action:regex(^(list|get|create)$)}` verifica la corrispondenza del valore di route `action` solo con `list`, `get` o `create`. Se viene passata nel dizionario di vincoli, la stringa `^(list|get|create)$` è equivalente. Anche i vincoli passati nel dizionario di vincoli (non inline all'interno di un modello) che non corrispondono a uno dei vincoli noti vengono considerati espressioni regolari.

## <a name="custom-route-constraints"></a>Vincoli di route personalizzati

Oltre ai vincoli di route predefiniti, è possibile creare vincoli di route personalizzati implementando l'interfaccia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>. L'interfaccia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> contiene un singolo metodo, `Match`, che restituisce `true` se il vincolo viene soddisfatto e `false` in caso contrario.

Per usare un'interfaccia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> personalizzata, il tipo di vincolo di route deve essere registrato con la proprietà <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> dell'app nel contenitore di servizi dell'app. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> è un dizionario che esegue il mapping delle chiavi dei vincoli di route alle implementazioni di <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> che convalidano tali vincoli. La proprietà <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> di un'app può essere aggiornata in `Startup.ConfigureServices` come parte di una chiamata [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) o configurando <xref:Microsoft.AspNetCore.Routing.RouteOptions> direttamente con `services.Configure<RouteOptions>`. Ad esempio:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

Il vincolo può quindi essere applicato alle route nel modo consueto, usando il nome specificato al momento della registrazione del tipo di vincolo. Ad esempio:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="url-generation-reference"></a>Riferimento per la generazione di URL

L'esempio seguente mostra come generare un collegamento a una route usando un dizionario di valori di route e un elemento <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

L'elemento <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generato alla fine dell'esempio precedente è `/package/create/123`. Il dizionario fornisce i valori di route `operation` e `id` del modello "Track Package Route", `package/{operation}/{id}`. Per informazioni dettagliate, vedere il codice di esempio nella sezione [Usare il middleware di routing](#use-routing-middleware) oppure l'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Il secondo parametro per il costruttore <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> è una raccolta di *valori di ambiente*. I valori di ambiente sono utili poiché limitano il numero di valori che uno sviluppatore deve specificare all'interno del contesto di una richiesta. I valori di route correnti della richiesta corrente sono considerati valori di ambiente per la generazione del collegamento. Nell'azione `About` di un'app ASP.NET Core MVC di `HomeController`, non è necessario specificare il valore di route del controller per collegarsi all'azione `Index`. Viene usato il valore di ambiente `Home`.

I valori di ambiente che non corrispondono a un parametro vengono ignorati. I valori di ambiente vengono ignorati anche quando un valore specificato in modo esplicito esegue l'override del valore di ambiente. La corrispondenza si verifica da sinistra a destra nell'URL.

I valori specificati in modo esplicito ma che non corrispondono a un segmento della route vengono aggiunti alla stringa di query. La tabella seguente illustra il risultato ottenuto quando si usa il modello di route `{controller}/{action}/{id?}`.

| Valori di ambiente                     | Valori espliciti                        | Risultato                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |

Se una route ha un valore predefinito che non corrisponde a un parametro e tale valore viene specificato in modo esplicito, deve corrispondere al valore predefinito:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

La generazione dei collegamenti genera un collegamento per questa route quando vengono specificati i valori corrispondenti per `controller` e `action`.

## <a name="complex-segments"></a>Segmenti complessi

I segmenti complessi (ad esempio, `[Route("/x{token}y")]`), vengono elaborati individuando corrispondenze per i valori letterali da destra a sinistra in modalità non-greedy. Vedere [questo codice](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) per una spiegazione dettagliata di come vengono confrontati i segmenti complessi. L'[esempio di codice](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) non viene usato da ASP.NET Core, ma offre una spiegazione esauriente dei segmenti complessi.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end
