---
title: Stato di sessione e applicazione in ASP.NET Core
author: rick-anderson
description: Approcci per il mantenimento dello stato dell'applicazione e dell'utente (sessione) tra le richieste.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 7aa200d3612f766ab633ccab807421b9c5393975
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>Introduzione allo stato della sessione e dell'applicazione in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/) e [Diana LaRose](https://github.com/DianaLaRose)

HTTP è un protocollo senza stato. Un server Web considera ogni richiesta HTTP come richiesta indipendente e non mantiene i valori utente delle richieste precedenti. In questo articolo vengono illustrati diversi modi per mantenere lo stato applicazione e lo stato sessione tra le richieste. 

## <a name="session-state"></a>Stato sessione

Lo stato della sessione è una funzionalità di ASP.NET Core che è possibile usare per salvare e archiviare i dati utente mentre l'utente visualizza l'app Web. Lo stato sessione è costituito da un dizionario o una tabella hash sul server e mantiene i dati tra le diverse richieste effettuate da un browser. I dati della sessione sono supportati da una cache.

ASP.NET Core gestisce lo stato della sessione assegnando al client un cookie che contiene l'ID sessione. Tale ID viene inviato al server con ogni richiesta. Il server usa l'ID sessione per recuperare i dati della sessione. Dato che il cookie di sessione è specifico per il browser, non è possibile condividere le sessioni tra browser diversi. I cookie di sessione vengono eliminati solo al termine della sessione del browser. Se viene ricevuto un cookie per una sessione scaduta, viene creata una nuova sessione che usa lo stesso cookie di sessione. 

Il server mantiene una sessione per un periodo di tempo limitato dopo l'ultima richiesta. È possibile impostare il timeout della sessione o usare il valore predefinito, pari a 20 minuti. Lo stato sessione è ideale per archiviare dati utente che sono specifici per una determinata sessione, ma non vanno salvati in modo permanente. I dati vengono eliminati dall'archivio di backup quando viene chiamata `Session.Clear` o alla scadenza della sessione nell'archivio dati. Il server non rileva la chiusura del browser o l'eliminazione del cookie di sessione.

> [!WARNING]
> Evitare di archiviare dati sensibili nella sessione. Il client potrebbe non chiudere il browser ed eliminare il cookie di sessione (alcuni browser salvano anche i cookie di sessione tra finestre diverse). Una sessione può anche non essere limitata a un utente singolo; in tal caso l'utente successivo potrebbe continuare con la stessa sessione.

Il provider di sessione in memoria archivia i dati della sessione nel server locale. Se si prevede di eseguire l'app Web in una server farm, è necessario usare sessioni permanenti per associare ogni sessione a un server specifico. La piattaforma Siti Web di Windows Azure usa per impostazione predefinita le sessioni permanenti (Application Request Routing o ARR). Tuttavia le sessioni permanenti possono compromettere la scalabilità e complicare gli aggiornamenti delle app Web. Un'opzione migliore è l'uso delle cache distribuite Redis o SQL Server, che non richiedono sessioni permanenti. Per altre informazioni, vedere [Uso di una cache distribuita](xref:performance/caching/distributed). Per informazioni dettagliate sulla configurazione di provider di servizi, vedere [Configurazione della sessione](#configuring-session) più avanti in questo articolo.

<a name="temp"></a>
## <a name="tempdata"></a>TempData

ASP.NET Core MVC espone la proprietà [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) per un [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0). Questa proprietà archivia i dati finché non viene letta. I metodi `Keep` e `Peek` possono essere usati per esaminare i dati senza eliminazione. `TempData` è particolarmente utile per il reindirizzamento quando i dati sono necessari per più di una richiesta singola. Ad esempio la proprietà `TempData` viene implementata dai provider TempData mediante i cookie o lo stato sessione.

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a>Provider TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

In ASP.NET Core 2.0 e versioni successive il provider TempData basato su cookie viene usato per impostazione predefinita per memorizzare TempData nei cookie.

I dati del cookie sono codificati con [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0). Dato che il cookie è crittografato e suddiviso in blocchi, il limite di dimensione per un singolo cookie di ASP.NET Core 1.x non è applicabile. I dati del cookie non vengono compressi perché la compressione di dati crittografati può comportare problemi di sicurezza, ad esempio con attacchi [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)). Per altre informazioni sul provider TempData basato sui cookie, vedere [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

In ASP.NET Core 1.0 e 1.1 il provider TempData con stato sessione è il valore predefinito.

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a>Scelta di un provider TempData

La scelta di un provider TempData implica diverse considerazioni, tra cui:

1. L'applicazione usa già lo stato sessione per altri scopi? Se sì, l'uso del provider TempData con stato sessione non comporta un carico aggiuntivo per l'applicazione (indipendentemente dalla dimensione dei dati).
2. L'applicazione usa TempData solo quando necessario e per quantità di dati relativamente piccole (fino a 500 byte)? Se sì, il provider TempData con cookie aggiungerà un piccolo carico di lavoro a ogni richiesta che include TempData. Se no, il provider TempData con stato sessione può essere utile per evitare sequenze di andata e ritorno di grandi quantità di dati in ogni richiesta fino a quando il contenuto TempData non viene consumato.
3. L'applicazione viene eseguita in una Web farm (costituita da più server)? Se sì, non sono necessari elementi di configurazione aggiuntivi per l'uso del provider TempData con cookie.

> [!NOTE]
> La maggior parte dei client Web (ad esempio i Web browser) impone limiti per la dimensione massima di ogni cookie, il numero totale di cookie o entrambe le impostazioni. Pertanto se si usa il provider TempData con cookie, verificare che l'app non superi tali limiti. Considerare le dimensioni totali dei dati, tenendo conto dei sovraccarichi rappresentati dalla crittografia e dalla divisione in blocchi.

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a>Configurare il provider TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il provider TempData basato su cookie è abilitato per impostazione predefinita. Il codice classe `Startup` seguente configura il provider TempData basato sulla sessione:

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Il codice classe `Startup` seguente configura il provider TempData basato sulla sessione:

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

L'ordine è essenziale per i componenti middleware. Nell'esempio precedente si verifica un'eccezione di tipo `InvalidOperationException` se `UseSession` viene chiamata dopo `UseMvcWithDefaultRoute`. Per altri dettagli, vedere [Middleware Ordering](xref:fundamentals/middleware#ordering) (Ordine del middleware).

> [!IMPORTANT]
> Se la destinazione è .NET Framework e si usa il provider basato sulla sessione, aggiungere il pacchetto NuGet [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) al progetto.

## <a name="query-strings"></a>Stringhe di query

È possibile passare una quantità limitata di dati da una richiesta a un'altra aggiungendo i dati alla stringa di query della nuova richiesta. Questo è utile per l'acquisizione dello stato con una modalità persistente, che consente la condivisione dei collegamenti con stato incorporato tramite posta elettronica o social network. Tuttavia, per lo stesso motivo, le stringhe di query non vanno mai usate per i dati sensibili. OItre a essere facilmente condivisibili, i dati presenti nelle stringhe di query possono essere oggetto di attacchi di [falsificazione della richiesta tra siti (CSRF, Cross-Site Request Forgery)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), che ingannano gli utenti autenticati invitandoli a visitare siti pericolosi. Gli utenti malintenzionati possono quindi sottrarre dati utente dall'app o eseguire azioni dannose per conto dell'utente. Qualsiasi stato dell'applicazione o della sessione mantenuto deve garantire la protezione dagli attacchi CSRF. Per altre informazioni sugli attacchi CSRF, vedere [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md) (Prevenzione degli attacchi di falsificazione della richiesta tra siti (XSRF/CSRF) in ASP.NET Core).

## <a name="post-data-and-hidden-fields"></a>Pubblicazione di dati e campi nascosti

I dati possono essere salvati in campi modulo nascosti e pubblicati di nuovo nella richiesta successiva. Questo si verifica spesso nei moduli a più pagine. Tuttavia, dato che il client potrebbe alterare i dati, il server deve sempre ripeterne la convalida. 

## <a name="cookies"></a>Cookie

I cookie consentono di archiviare dati specifici dell'utente nelle applicazioni Web. Poiché i cookie vengono inviati con ogni richiesta, la loro dimensione deve essere mantenuta al minimo. Idealmente, in un cookie deve essere archiviato solo un identificatore con i dati effettivi archiviati nel server. La maggior parte dei browser limita la dimensione dei cookie a 4096 byte. Tenere anche presente che per ogni dominio è disponibile solo un numero di cookie limitato.  

Dato che i cookie possono essere manomessi, devono essere convalidati nel server. Anche se la durabilità del cookie in un client è soggetta all'intervento dell'utente e provvista di scadenza, a livello generale i cookie sono la forma più durevole di persistenza dei dati nel client.

I cookie vengono spesso usati per la personalizzazione, ovvero il contenuto viene personalizzato per un utente noto. Dato che nella maggior parte dei casi l'utente è solo identificato ma non autenticato, generalmente è possibile proteggere un cookie archiviando al suo interno il nome utente, il nome account o un ID utente univoco, come un identificatore univoco globale (GUID). È quindi possibile usare il cookie per accedere all'infrastruttura di personalizzazione utente di un sito.

## <a name="httpcontextitems"></a>HttpContext.Items

La raccolta `Items` è una posizione ideale per archiviare dati che sono necessari solo durante l'elaborazione di una determinata richiesta. Il contenuto della raccolta viene rimosso dopo ogni richiesta. La raccolta `Items` è particolarmente utile come strumento di comunicazione per componenti o middleware che operano in momenti diversi durante una richiesta e non dispongono di un metodo diretto per passare i parametri. Per altre informazioni, vedere [Uso di HttpContext.Items](#working-with-httpcontextitems) più avanti in questo articolo.

## <a name="cache"></a>Cache

La memorizzazione nella cache è un modo efficiente per archiviare e recuperare dati. È possibile controllare la durata degli elementi memorizzati nella cache in base al tempo e ad altre considerazioni. Altre informazioni sulla [memorizzazione nella cache](../performance/caching/index.md).

<a name="session"></a>
## <a name="working-with-session-state"></a>Uso dello stato sessione

### <a name="configuring-session"></a>Configurazione della sessione

Il pacchetto `Microsoft.AspNetCore.Session` offre il middleware per la gestione dello stato sessione. Per abilitare il middleware della sessione, `Startup` deve contenere:

- Una delle cache di memoria [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache). L'implementazione `IDistributedCache` viene usata come archivio di backup per la sessione.
- Una chiamata [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_), che richiede il pacchetto NuGet "Microsoft.AspNetCore.Session".
- Una chiamata [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_).

Il codice seguente visualizza come configurare il provider della sessione in memoria.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

È possibile fare riferimento a Session da `HttpContext` una volta installato e configurato.

Se si prova ad accedere a `Session` prima della chiamata di `UseSession` viene generata un'eccezione `InvalidOperationException: Session has not been configured for this application or request`.

Se si prova a creare un nuovo elemento `Session` (ovvero se non è stato creato nessun cookie di sessione) dopo che è già iniziata la scrittura nel flusso `Response`, viene generata l'eccezione `InvalidOperationException: The session cannot be established after the response has started`. L'eccezione è disponibile nel log del server Web ma non viene visualizzata nel browser.

### <a name="loading-session-asynchronously"></a>Caricamento di Session in modalità asincrona 

Il provider di sessione predefinito in ASP.NET Core carica in modo asincrono il record di sessione dall'archivio [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) sottostante solo se il metodo [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) viene chiamato in modo esplicito prima dei metodi `TryGetValue`, `Set` o `Remove`. Se `LoadAsync` non viene chiamato per primo, il record di sessione sottostante viene caricato in modo sincrono e ciò può compromettere la capacità di ridimensionamento dell'app.

Per fare in modo che le applicazioni implementino questo criterio, eseguire il wrapping delle implementazioni [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) e [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) con versioni che generano un'eccezione se il metodo `LoadAsync` non viene chiamato prima di `TryGetValue`, `Set` o `Remove`. Registrare le versioni con wrapping nel contenitore di servizi.

### <a name="implementation-details"></a>Dettagli sull'implementazione

Session usa un cookie per tenere traccia e identificare le richieste provenienti da un solo browser. Per impostazione predefinita, questo cookie è denominato ".AspNet.Session"e usa il percorso "/". Dato che il cookie predefinito non specifica un dominio, non viene reso disponibile allo script lato client nella pagina (perché `CookieHttpOnly` assume il valore `true`).

Per eseguire l'override delle impostazioni predefinite di sessione, usare `SessionOptions`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

Il server usa la proprietà `IdleTimeout` per determinare quanto tempo una sessione può rimanere inattiva prima che il relativo contenuto venga abbandonato. Questa proprietà è indipendente dalla scadenza del cookie. Ogni richiesta che passa attraverso il middleware Session (come richiesta letta o scritta) reimposta il timeout.

Dato che `Session` *non applica un blocco*, se due richieste provano a modificare il contenuto della sessione, l'ultima esegue l'override della prima. `Session` viene implementata come *sessione coerente*, ovvero tutto il contenuto viene archiviato insieme. Due richieste che modificano parti diverse della sessione (chiavi diverse) potrebbero comunque influire l'una sull'altra.

### <a name="setting-and-getting-session-values"></a>Impostazione e recupero di valori di Session

È possibile accedere a Session attraverso la proprietà `Session` in `HttpContext`. Questa proprietà è un'implementazione di [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession).

L'esempio seguente visualizza l'impostazione e il recupero di un valore int e una stringa:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

Se si aggiungono i seguenti metodi di estensione è possibile impostare e ottenere oggetti serializzabili in Session:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

L'esempio seguente illustra come impostare e ottenere un oggetto serializzabile:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>Uso di HttpContext.Items

L'astrazione `HttpContext` offre supporto per una raccolta dizionario di tipo `IDictionary<object, object>` denominata `Items`. Questa raccolta è disponibile dall'inizio di una richiesta *HttpRequest* e viene rimossa alla fine di ogni richiesta. È possibile accedervi assegnando un valore a una voce con chiave o richiedendo il valore per una chiave specifica.

Nell'esempio seguente [Middleware](middleware.md) aggiunge `isVerified` all'insieme `Items`.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

In punto successivo della pipeline, un altro middleware potrà accedere all'elemento:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

Per il middleware che viene usato da un'unica app sono accettabili le chiavi `string`. Il middleware condiviso tra più applicazioni deve invece usare chiavi oggetto univoche, per evitare qualsiasi rischio di conflitti di chiavi. Se si sta sviluppando middleware che dovrà funzionare con più applicazioni, usare una chiave oggetto univoca definita nella classe middleware come illustrato di seguito:

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

Altri elementi di codice possono accedere al valore archiviato in `HttpContext.Items` usando la chiave esposta dalla classe middleware:

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

Questo approccio ha anche il vantaggio di eliminare la ripetizione di "stringhe magiche" in più posizioni nel codice.

<a name="appstate-errors"></a>

## <a name="application-state-data"></a>Dati dello stato applicazione

Usare [Dependency Injection](xref:fundamentals/dependency-injection) (Inserimento di dipendenze) per rendere i dati disponibili a tutti gli utenti:

1. Definire un servizio contenente i dati (ad esempio una classe denominata `MyAppData`).

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. Aggiungere la classe del servizio a `ConfigureServices` (ad esempio `services.AddSingleton<MyAppData>();`).
3. Usare la classe del servizio dati in ogni controller:

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

## <a name="common-errors-when-working-with-session"></a>Errori comuni quando si lavora con Session

* "Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'." (Risoluzione servizio non riuscita per il tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' durante il tentativo di attivazione di 'Microsoft.AspNetCore.Session.DistributedSessionStore').

  Questo errore dipende in genere dal fatto che almeno un'implementazione `IDistributedCache` non è stata configurata. Per altre informazioni, vedere [Uso di una cache distribuita](xref:performance/caching/distributed) e [Cache In memory](xref:performance/caching/memory).

* Se il middleware Session non riesce a mantenere una sessione (ad esempio se il database non è disponibile), registra l'eccezione e la assorbe. La richiesta continua quindi normalmente, determinando un comportamento di difficile previsione.

Ecco un esempio tipico:

Un utente archivia un carrello acquisti nella sessione. L'utente aggiunge un elemento, ma il commit ha esito negativo. L'app non riconosce l'errore e restituisce il messaggio "L'elemento è stato aggiunto", ma ciò non è vero.

Il metodo consigliato per verificare la presenza di tali errori è la chiamata di `await feature.Session.CommitAsync();` dal codice dell'app dopo il completamento della scrittura nella sessione. Quindi sarà possibile gestire l'errore nel modo desiderato. Il funzionamento è identico quando si chiama `LoadAsync`.

### <a name="additional-resources"></a>Risorse aggiuntive

* [ASP.NET Core 1.x: Codice di esempio usato in questo documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x: Codice di esempio usato in questo documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
