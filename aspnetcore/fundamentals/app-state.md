---
title: Stato di sessione e dell'applicazione in ASP.NET Core
author: rick-anderson
description: Approcci per la conservazione di applicazione e lo stato utente (sessione) tra le richieste.
keywords: Registrare ASP.NET Core, lo stato dell'applicazione, lo stato della sessione, stringa di query,
ms.author: riande
manager: wpickett
ms.date: 10/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f9c1d10101d23e105c4a8af41d851f69b1b6a175
ms.sourcegitcommit: 9c27fa0f0c57ad611aa43f63afb9b9c9571d4a94
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>Introduzione allo stato sessione e dell'applicazione ASP.NET di base

Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), e [Diana LaRose](https://github.com/DianaLaRose)

HTTP è un protocollo senza stato. Un server web considera ogni richiesta HTTP come indipendente e non mantiene i valori utente delle richieste precedenti. In questo articolo vengono illustrati diversi modi per mantenere l'applicazione e lo stato della sessione tra le richieste. 

## <a name="session-state"></a>Stato della sessione

Lo stato della sessione è una funzionalità di ASP.NET Core che è possibile usare per salvare e archiviare i dati utente mentre l'utente visualizza l'app Web. È costituito da una tabella hash o dizionario sul server, lo stato della sessione mantiene i dati in tutte le richieste da un browser. I dati della sessione sono supportati da una cache.

ASP.NET Core mantiene lo stato della sessione, assegnando al client un cookie che contiene l'ID di sessione, che viene inviato al server con ogni richiesta. Il server utilizza l'ID di sessione per recuperare i dati di sessione. Poiché il cookie di sessione specifico del browser, è possibile condividere le sessioni tra browser. I cookie di sessione vengono eliminati solo al termine della sessione del browser. Se un cookie viene ricevuto per una sessione scaduta, viene creata una nuova sessione che usa lo stesso cookie di sessione. 

Il server mantiene una sessione per un periodo di tempo limitato dopo l'ultima richiesta. È possibile impostare il timeout della sessione o usare il valore predefinito di 20 minuti. Lo stato della sessione è ideale per archiviare i dati utente che sono specifico per una determinata sessione, ma non devono essere mantenute in modo permanente. Dati vengono eliminati dall'archivio di backup, sia quando si chiama `Session.Clear` o scadenza della sessione nell'archivio dati. Il server non riconosce quando il browser viene chiuso o quando viene eliminato il cookie di sessione.

> [!WARNING]
> Non archiviare i dati sensibili nella sessione. Il client potrebbe non chiudere il browser e deselezionare il cookie di sessione (e alcuni browser mantenere attivi i cookie di sessione in windows). Inoltre, una sessione potrebbe non essere limitata a un utente singolo; l'utente successivo potrebbe continuare con la stessa sessione.

Il provider della sessione in memoria archivia i dati di sessione nel server locale. Se si prevede di eseguire l'app web in una server farm, è necessario utilizzare le sessioni permanenti per associare ogni sessione in un server specifico. La piattaforma di siti Web di Azure per impostazione predefinita le sessioni permanenti (Application Request Routing o ARR). Tuttavia, le sessioni permanenti possono influire sulla scalabilità e complicare gli aggiornamenti delle app web. Un'opzione migliore consiste nell'usare Redis o distribuite di SQL Server memorizza nella cache, che non richiede le sessioni permanenti. Per ulteriori informazioni, vedere [utilizzano una Cache distribuita](xref:performance/caching/distributed). Per informazioni dettagliate sulla configurazione di provider di servizi, vedere [la configurazione di sessione](#configuring-session) più avanti in questo articolo.


<a name="temp"></a>
## <a name="tempdata"></a>TempData

ASP.NET MVC di base espone il [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) proprietà in un [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0). Questa proprietà archivia i dati finché non viene letta. I metodi `Keep` e `Peek` possono essere usati per esaminare i dati senza eliminazione. `TempData`è particolarmente utile per il reindirizzamento, quando sono necessari dati per più di una singola richiesta. `TempData`è implementato dal provider TempData, ad esempio, utilizza i cookie o lo stato della sessione.

### <a name="tempdata-providers"></a>TempData provider

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

In ASP.NET Core 2.0 e versioni successive, per impostazione predefinita viene utilizzato il provider di TempData basato su cookie per memorizzare TempData nei cookie.

I dati del cookie sono codificati con la [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0). Perché il cookie viene crittografato e in blocchi, il cookie single dimensione limite di ASP.NET Core 1. x non è applicabile. I dati del cookie non viene compresso perché la compressione dei dati encryped può comportare problemi di sicurezza, ad esempio il [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violazione](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacchi. Per ulteriori informazioni sul provider TempData basato su cookie, vedere [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

In ASP.NET Core 1.0 e 1.1, il provider di TempData dello stato sessione è il valore predefinito.

--------------

### <a name="choosing-a-tempdata-provider"></a>Scelta di un provider TempData

Scelta di un provider TempData prevede alcune considerazioni, ad esempio:

1. L'applicazione ha già utilizza lo stato della sessione per altri scopi? In questo caso, tramite il provider di TempData dello stato sessione non è senza alcun costo aggiuntivo per l'applicazione (a parte la dimensione dei dati).
2. L'applicazione utilizza TempData solo quando strettamente necessario, per relativamente piccole quantità di dati (fino a 500 byte)? Se in tal caso, il provider di TempData cookie aggiungerà un costo di piccole dimensioni a ogni richiesta che trasmette TempData. In caso contrario, il provider di TempData dello stato di sessione può essere utile evitare round trip una grande quantità di dati in ogni richiesta fino a quando non viene utilizzato il TempData.
3. L'applicazione viene eseguita in una web farm (più server)? In questo caso, non vi è alcuna configurazione aggiuntiva per utilizzare il provider di TempData cookie.

> [!NOTE]
> La maggior parte dei client web (ad esempio i browser web) imporre limiti alla dimensione massima di ciascun cookie, il numero totale di cookie o entrambi. Pertanto, quando si utilizza il provider di TempData cookie, verificare che l'app non supera questi limiti. Prendere in considerazione le dimensioni totali dei dati, tenendo conto dei sovraccarichi di crittografia e la suddivisione in blocchi.

Per configurare il provider TempData per un'applicazione, registrare un'implementazione del provider TempData in `ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddMvc()
        .AddSessionStateTempDataProvider();

    // The Session State TempData Provider requires adding the session state service
    services.AddSession();
}
```

## <a name="query-strings"></a>Stringhe di query

È possibile passare una quantità limitata di dati da una richiesta a un altro, aggiungerlo alla stringa di query della nuova richiesta. Ciò è utile per l'acquisizione dello stato in modo persistente che consente i collegamenti con stato incorporato da condividere tramite posta elettronica o social network. Tuttavia, per questo motivo, non utilizzare le stringhe di query per i dati sensibili. Oltre a facilmente condiviso, inclusi i dati nelle stringhe di query possono creare le opportunità di [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacchi, che possono indurre gli utenti a siti dannosi mentre autenticato. Gli utenti malintenzionati possono quindi intercettare i dati utente dall'app o richiedere azioni dannose per conto dell'utente. Qualsiasi stato mantenuto application o session necessario proteggere da attacchi CSRF. Per ulteriori informazioni sugli attacchi CSRF, vedere [attacchi di prevenzione Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core](../security/anti-request-forgery.md).

## <a name="post-data-and-hidden-fields"></a>Dati post e campi nascosti

Dati possono essere salvati nei campi del form nascosto e inviati nuovamente la richiesta successiva. Questo è comune in un form con più pagine. Tuttavia, poiché il client può potenzialmente manomettere i dati, il server deve sempre sottoporlo. 

## <a name="cookies"></a>Cookie

I cookie consentono di archiviare dati specifici dell'utente nelle applicazioni web. Poiché i cookie vengono inviati con ogni richiesta, la dimensione deve essere mantenuta al minimo. Idealmente, solo un identificatore deve essere archiviato in un cookie con i dati effettivi archiviati nel server. La maggior parte dei browser limitare i cookie a 4096 byte. Inoltre, solo un numero limitato di cookie è disponibile per ogni dominio.  

Poiché i cookie sono soggette alla manomissione, devono essere convalidati nel server. Sebbene la durata del cookie in un client è soggetto a scadenza e l'intervento dell'utente, sono in genere la forma più durevole di persistenza dei dati nel client.

I cookie vengono spesso utilizzati per la personalizzazione, in cui il contenuto viene personalizzato per un utente noto. Poiché l'utente viene identificato solo e non è stato autenticato nella maggior parte dei casi, è possibile proteggere in genere un cookie archiviando il nome utente, nome dell'account o un ID utente univoco (ad esempio un GUID) nel cookie. È quindi possibile utilizzare il cookie di accesso all'infrastruttura di personalizzazione utente di un sito.

## <a name="httpcontextitems"></a>HttpContext. Items

Il `Items` raccolta è una buona posizione in cui archiviare i dati necessari solo durante l'elaborazione una particolare richiesta. Contenuto della raccolta viene eliminato dopo ogni richiesta. Il `Items` insieme è particolarmente utile come un modo per componenti o middleware per comunicare quando operano in momenti diversi durante una richiesta e in alcun modo diretto per passare i parametri. Per ulteriori informazioni, vedere [utilizzo HttpContext. Items](#working-with-httpcontextitems), più avanti in questo articolo.

## <a name="cache"></a>Cache

La memorizzazione nella cache è un modo efficiente per archiviare e recuperare dati. È possibile controllare la durata di elementi memorizzati nella cache in base al tempo e altre considerazioni. Altre informazioni, vedere [la memorizzazione nella cache](../performance/caching/index.md).

<a name=session></a>
## <a name="working-with-session-state"></a>Utilizzo con lo stato della sessione

### <a name="configuring-session"></a>Configurazione della sessione

Il `Microsoft.AspNetCore.Session` pacchetto fornisce il middleware per la gestione dello stato sessione. Per abilitare il middleware di sessione, `Startup`deve contenere:

- Uno del [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) cache in memoria. Il `IDistributedCache` implementazione viene utilizzata come archivio di backup per sessione.
- [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) chiama, che richiede il pacchetto NuGet "Microsoft.AspNetCore.Session".
- [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) chiamare.

Il codice seguente viene illustrato come configurare il provider della sessione in memoria.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

È possibile fare riferimento a una sessione da `HttpContext` una volta installato e configurato.

Se si tenta di accedere a `Session` prima `UseSession` è stato chiamato, l'eccezione `InvalidOperationException: Session has not been configured for this application or request` viene generata un'eccezione.

Se si tenta di creare un nuovo `Session` (vale a dire alcun cookie di sessione non creato) dopo che si è già iniziato la scrittura di `Response` flusso, l'eccezione `InvalidOperationException: The session cannot be established after the response has started` viene generata un'eccezione. L'eccezione è reperibile nel log di server web. non verrà visualizzata nel browser.

### <a name="loading-session-asynchronously"></a>Il caricamento asincrono di sessione 

Il provider di sessione predefinito in ASP.NET Core carica il record di sessione sottostante [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store in modo asincrono solo se il [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) metodo viene chiamato in modo esplicito prima  il `TryGetValue`, `Set`, o `Remove` metodi. Se `LoadAsync` non viene chiamato prima di tutto, sottostante il record di sessione viene caricato in modo sincrono, che possono incidere sul possibilità dell'app di scala.

Per applicare questo modello applicazioni, eseguire il wrapping di [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) e [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementazioni con le versioni che generano un'eccezione se il `LoadAsync` metodo non è chiamato prima `TryGetValue`, `Set`, o `Remove`. Registrare le versioni di cui effettua il wrapping nel contenitore di servizi.

### <a name="implementation-details"></a>Dettagli di implementazione

Sessione utilizza un cookie per tenere traccia e identificare le richieste provenienti da un solo browser. Per impostazione predefinita, questo cookie denominato ". AspNet.Session"e utilizza un percorso di"/". Poiché il valore predefinito di cookie non viene specificato un dominio, non renderlo disponibile per lo script sul lato client nella pagina (perché `CookieHttpOnly` per impostazione predefinita `true`).

Per eseguire l'override delle impostazioni predefinite di sessione, utilizzare `SessionOptions`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

Il server utilizza il `IdleTimeout` proprietà per determinare quanto tempo una sessione può rimanere inattiva prima che il relativo contenuto viene abbandonato. Questa proprietà è indipendente dalla scadenza del cookie. Ogni richiesta che passa attraverso il middleware di sessione (leggere o scrivere) Reimposta il timeout.

Poiché `Session` è *non blocca il thread*, se due richieste tentano di modificare il contenuto della sessione, l'ultima prevale sulla prima. `Session`viene implementato come un *sessione coerente*, il che significa che tutto il contenuto viene archiviato insieme. Due richieste che modificano parti diverse della sessione (chiavi diverse) potrebbero comunque avere un'influenza reciproca.

### <a name="setting-and-getting-session-values"></a>Impostazione e recupero di valori di sessione

Sessione avviene tramite il `Session` proprietà `HttpContext`. Questa proprietà è un [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementazione.

L'esempio seguente mostra l'impostazione e recupero di un tipo int e una stringa:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]

Se si aggiungono i seguenti metodi di estensione, è possibile impostare e ottenere gli oggetti serializzabili alla sessione:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

Nell'esempio seguente viene illustrato come impostare e ottenere un oggetto serializzabile:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>Utilizzo di HttpContext. Items

Il `HttpContext` astrazione fornisce supporto per una raccolta di dizionario di tipo `IDictionary<object, object>`, denominato `Items`. Questa raccolta è disponibile dall'inizio di un *HttpRequest* e vengono eliminati alla fine di ogni richiesta. È possibile accedervi tramite l'assegnazione di un valore a una voce con chiave, o richiedendo il valore per una chiave specifica.

Nell'esempio seguente, [Middleware](middleware.md) aggiunge `isVerified` per il `Items` insieme.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

In un secondo momento nella pipeline, un altro middleware Impossibile accedervi:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

Per middleware che verrà utilizzato solo da una singola app `string` chiavi sono accettabili. Tuttavia, middleware che verrà condivisi tra le applicazioni debba utilizzare chiavi dell'oggetto univoco per evitare qualsiasi rischio di conflitti di chiave. Se si sta sviluppando middleware che devono essere utilizzati in più applicazioni, utilizzare una chiave oggetto univoco definita nella classe middleware come illustrato di seguito:

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

Altro codice può accedere al valore archiviato in `HttpContext.Items` utilizzando la chiave esposta dalla classe middleware:

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

Questo approccio presenta inoltre il vantaggio di eliminare la ripetizione di stringhe"magiche" in più posizioni nel codice.

<a name=appstate-errors></a>

## <a name="application-state-data"></a>Dati di stato dell'applicazione

Utilizzare [Dependency Injection](xref:fundamentals/dependency-injection) per rendere i dati disponibili a tutti gli utenti:

1. Definire un servizio che contiene i dati (ad esempio, una classe denominata `MyAppData`).

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. Aggiungere la classe di servizio per `ConfigureServices` (ad esempio `services.AddSingleton<MyAppData>();`).
3. Utilizzare la classe di servizio dati in ogni controller:

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

### <a name="common-errors-when-working-with-session"></a>Errori comuni quando si lavora con sessione

* "Impossibile risolvere il servizio per il tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' durante il tentativo di attivazione 'Microsoft.AspNetCore.Session.DistributedSessionStore'".

  Questo è generalmente causato dal mancato configurare almeno un `IDistributedCache` implementazione. Per ulteriori informazioni, vedere [utilizzano una Cache distribuita](xref:performance/caching/distributed) e [In memoria la memorizzazione nella cache](xref:performance/caching/memory).

### <a name="additional-resources"></a>Risorse aggiuntive


* [ASP.NET Core 1. x: esempio di codice usato in questo documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2. x: esempio di codice usato in questo documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
