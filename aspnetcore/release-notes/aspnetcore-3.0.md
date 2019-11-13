---
title: Novità di ASP.NET Core 3,0
author: rick-anderson
description: Informazioni sulle nuove funzionalità di ASP.NET Core 3,0.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.0
ms.openlocfilehash: c3dde383507ec919f82b5268ddbf23911c3d24f8
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963115"
---
# <a name="whats-new-in-aspnet-core-30"></a>Novità di ASP.NET Core 3,0

In questo articolo vengono evidenziate le modifiche più significative in ASP.NET Core 3,0 con i collegamenti alla documentazione pertinente.

## Blazor

Blazor è un nuovo Framework in ASP.NET Core per la creazione di un'interfaccia utente Web interattiva sul lato client con .NET:

* Creare interfacce utente interattive avanzate con C# invece di JavaScript.
* Condividere la logica dell'app scritta in .NET sul lato client e sul lato server.
* Eseguire il rendering dell'interfaccia utente come HTML e CSS per un ampio supporto dei browser, inclusi i browser per dispositivi mobili.

scenari supportati di Blazor Framework:

* Componenti dell'interfaccia utente riutilizzabili (componenti Razor)
* Routing lato client
* Layout componenti
* Supporto per l'inserimento di dipendenze
* Moduli e convalida
* Compilare librerie di componenti con le librerie di classi Razor
* Interoperabilità JavaScript

Per ulteriori informazioni, vedere <xref:blazor/index>.

### <a name="opno-locblazor-server"></a>Server Blazor

Blazor separa la logica di rendering dei componenti dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente. Blazor Server fornisce il supporto per l'hosting di componenti Razor sul server in un'app ASP.NET Core. Gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR. il server Blazor è supportato ASP.NET Core 3,0.

### <a name="opno-locblazor-webassembly-preview"></a>Blazor webassembly (anteprima)

le app Blazor possono anche essere eseguite direttamente nel browser usando un Runtime .NET basato su webassembly. Blazor webassembly è in anteprima e *non* è supportato in ASP.NET Core 3,0. Blazor webassembly sarà supportato in una versione futura di ASP.NET Core.

### <a name="razor-components"></a>Componenti Razor

Blazor app sono compilate da componenti. I componenti sono blocchi autonomi dell'interfaccia utente (UI), ad esempio una pagina, una finestra di dialogo o un form. I componenti sono classi .NET normali che definiscono la logica di rendering dell'interfaccia utente e i gestori eventi sul lato client. È possibile creare app Web interattive avanzate senza JavaScript.

I componenti di Blazor vengono in genere creati usando sintassi Razor, una combinazione naturale di HTML C#e. I componenti Razor sono simili alle visualizzazioni Razor Pages e MVC in quanto entrambi utilizzano Razor. Diversamente dalle pagine e dalle viste, basate su un modello di richiesta-risposta, i componenti vengono usati in modo specifico per la gestione della composizione dell'interfaccia utente.

## <a name="grpc"></a>gRPC

[gRPC](https://grpc.io/):

* È un noto Framework RPC (Remote Procedure Call) a prestazioni elevate.
* Offre un approccio di primo contratto per lo sviluppo di API.
* Usa tecnologie moderne, ad esempio:

  * HTTP/2 per il trasporto.
  * Buffer del protocollo come linguaggio di descrizione dell'interfaccia.
  * Formato di serializzazione binario.
* Fornisce funzionalità come:

  * Authentication
  * Flusso bidirezionale e controllo di flusso.
  * Annullamento e timeout.

la funzionalità gRPC in ASP.NET Core 3,0 include:

* [Grpc. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; un framework ASP.NET Core per l'hosting di servizi Grpc. gRPC su ASP.NET Core si integra con funzionalità di ASP.NET Core standard come la registrazione, l'inserimento DI dipendenze, l'autenticazione e l'autorizzazione.
* [Grpc .NET. client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; un client Grpc per .NET Core che si basa sulle `HttpClient`familiari.
* [Grpc .NET. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; l'integrazione client Grpc con `HttpClientFactory`.

Per ulteriori informazioni, vedere <xref:grpc/index>.

## SignalR

Per istruzioni sulla migrazione, vedere [aggiornare il codice SignalR](xref:migration/22-to-30#signalr) . SignalR USA ora `System.Text.Json` per serializzare/deserializzare i messaggi JSON. Per istruzioni su come ripristinare il serializzatore basato su `Newtonsoft.Json`, vedere [passare a Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson) .

Nei client JavaScript e .NET per SignalRè stato aggiunto il supporto per la riconnessione automatica. Per impostazione predefinita, il client tenta di riconnettersi immediatamente e riprovare dopo 2, 10 e 30 secondi, se necessario. Se il client si riconnette correttamente, riceve un nuovo ID connessione. La riconnessione automatica è esplicita:

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

È possibile specificare gli intervalli di riconnessione passando una matrice di durate basate su millisecondi:

```javascript
.withAutomaticReconnect([0, 3000, 5000, 10000, 15000, 30000])
//.withAutomaticReconnect([0, 2000, 10000, 30000]) The default intervals.
```

È possibile passare un'implementazione personalizzata per il controllo completo degli intervalli di riconnessione.

Se la riconnessione non riesce dopo l'ultimo intervallo di riconnessione:

* Il client considera la connessione offline.
* Il client smette di tentare di riconnettersi.

Durante i tentativi di riconnessione, aggiornare l'interfaccia utente dell'app per notificare all'utente che è in corso il tentativo di riconnessione.

Per fornire commenti e suggerimenti dell'interfaccia utente quando la connessione viene interrotta, l'API client SignalR è stata espansa per includere i gestori eventi seguenti:

* `onreconnecting`: offre agli sviluppatori la possibilità di disabilitare l'interfaccia utente o di informare gli utenti che l'app è offline.
* `onreconnected`: offre agli sviluppatori la possibilità di aggiornare l'interfaccia utente una volta ristabilita la connessione.

Il codice seguente usa `onreconnecting` per aggiornare l'interfaccia utente durante il tentativo di connessione:

```javascript
connection.onreconnecting((error) => {
    const status = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messageInput").disabled = true;
    document.getElementById("sendButton").disabled = true;
    document.getElementById("connectionStatus").innerText = status;
});
```

Il codice seguente usa `onreconnected` per aggiornare l'interfaccia utente alla connessione:

```javascript
connection.onreconnected((connectionId) => {
    const status = `Connection reestablished. Connected.`;
    document.getElementById("messageInput").disabled = false;
    document.getElementById("sendButton").disabled = false;
    document.getElementById("connectionStatus").innerText = status;
});
```

SignalR 3,0 e versioni successive fornisce una risorsa personalizzata ai gestori di autorizzazione quando un metodo Hub richiede l'autorizzazione. La risorsa è un'istanza di `HubInvocationContext`. Il `HubInvocationContext` include:

* `HubCallerContext`
* Nome del metodo Hub richiamato.
* Argomenti per il metodo dell'hub.

Si consideri l'esempio seguente di un'app Chat Room che consente l'accesso a più organizzazioni tramite Azure Active Directory. Chiunque disponga di un account Microsoft può accedere a chat, ma solo i membri dell'organizzazione proprietaria possono vietare gli utenti o visualizzare le cronologie della chat degli utenti. L'app potrebbe limitare determinate funzionalità di utenti specifici.

```csharp
public class DomainRestrictedRequirement :
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>,
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement,
        HubInvocationContext resource)
    {
        if (context.User?.Identity?.Name == null)
        {
            return Task.CompletedTask;
        }

        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name))
        {
            context.Succeed(requirement);
        }

        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName, string currentUsername)
    {
        if (hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase))
        {
            return currentUsername.Equals("bob42@jabbr.net", StringComparison.OrdinalIgnoreCase);
        }

        return currentUsername.EndsWith("@jabbr.net", StringComparison.OrdinalIgnoreCase));
    }
}
```

Nel codice precedente `DomainRestrictedRequirement` funge da `IAuthorizationRequirement`personalizzato. Poiché il parametro della risorsa `HubInvocationContext` viene passato, la logica interna può:

* Esaminare il contesto in cui viene chiamato l'hub.
* Prendere decisioni per consentire all'utente di eseguire singoli metodi dell'hub.

I singoli metodi dell'hub possono essere decorati con il nome dei criteri controllati dal codice in fase di esecuzione. Poiché i client tentano di chiamare i singoli metodi dell'hub, il gestore `DomainRestrictedRequirement` esegue e controlla l'accesso ai metodi. In base al modo in cui il `DomainRestrictedRequirement` controlla l'accesso:

* Tutti gli utenti connessi possono chiamare il metodo `SendMessage`.
* Solo gli utenti che hanno eseguito l'accesso con un indirizzo di posta elettronica `@jabbr.net` possono visualizzare le cronologie degli utenti.
* Solo `bob42@jabbr.net` possibile escludere gli utenti dalla chat room.

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}
```

La creazione dei criteri di `DomainRestricted` può implicare:

* In *Startup.cs*aggiungere il nuovo criterio.
* Fornire il requisito di `DomainRestrictedRequirement` personalizzato come parametro.
* Registrazione di `DomainRestricted` con il middleware di autorizzazione.

```csharp
services
    .AddAuthorization(options =>
    {
        options.AddPolicy("DomainRestricted", policy =>
        {
            policy.Requirements.Add(new DomainRestrictedRequirement());
        });
    });
```

gli hub SignalR usano il [routing degli endpoint](xref:fundamentals/routing). la connessione all'hub SignalR è stata eseguita in precedenza in modo esplicito:

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});
```

Nella versione precedente, gli sviluppatori dovevano collegare i controller, le pagine Razor e gli hub in un'ampia gamma di posizioni. La connessione esplicita genera una serie di segmenti di routing quasi identici:

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});

app.UseRouting(routes =>
{
    routes.MapRazorPages();
});
```

SignalR 3,0 hub possono essere indirizzati tramite il routing degli endpoint. Con il routing degli endpoint, in genere è possibile configurare tutti i routing in `UseRouting`:

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapHub<ChatHub>("hubs/chat");
});
```

ASP.NET Core 3,0 SignalR aggiunto:

Streaming da client a server. Con il flusso da client a server, i metodi lato server possono prendere le istanze di un `IAsyncEnumerable<T>` o `ChannelReader<T>`. Nell'esempio seguente C# , il metodo `UploadStream` sull'hub riceverà un flusso di stringhe dal client:

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        // process content
    }
}
```

Le app client .NET possono passare un'istanza di `IAsyncEnumerable<T>` o `ChannelReader<T>` come argomento `stream` del metodo dell'hub `UploadStream`.

Al termine del ciclo `for` e alla chiusura della funzione locale, viene inviato il completamento del flusso:

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
}

await connection.SendAsync("UploadStream", clientStreamData());
```

Le app client JavaScript usano il SignalR `Subject` (o un [oggetto RxJS](https://rxjs.dev/api/index/class/Subject)) per l'argomento `stream` del metodo dell'hub `UploadStream` sopra.

```javascript
let subject = new signalR.Subject();
await connection.send("StartStream", "MyAsciiArtStream", subject);
```

Il codice JavaScript può usare il metodo `subject.next` per gestire le stringhe Man mano che vengono acquisite e pronte per l'invio al server.

```javascript
subject.next("example");
subject.complete();
```

Utilizzando codice come i due frammenti precedenti, è possibile creare esperienze di streaming in tempo reale.

## <a name="new-json-serialization"></a>Nuova serializzazione JSON

ASP.NET Core 3,0 USA ora <xref:System.Text.Json> per impostazione predefinita per la serializzazione JSON:

* Legge e scrive JSON in modo asincrono.
* È ottimizzato per il testo UTF-8.
* Prestazioni in genere superiori rispetto a `Newtonsoft.Json`.

Per aggiungere Json.NET a ASP.NET Core 3,0, vedere [aggiungere il supporto per il formato JSON basato su Newtonsoft. JSON](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).

## <a name="new-razor-directives"></a>Nuove direttive Razor

L'elenco seguente contiene le nuove direttive Razor:

* [@attribute](xref:mvc/views/razor#attribute) &ndash; la direttiva `@attribute` applica l'attributo specificato alla classe della pagina o della visualizzazione generata. Ad esempio `@attribute [Authorize]`.
* [@implements](xref:mvc/views/razor#implements) &ndash; la direttiva `@implements` implementa un'interfaccia per la classe generata. Ad esempio `@implements IDisposable`.

## <a name="identityserver4-supports-authentication-and-authorization-for-web-apis-and-spas"></a>IdentityServer4 supporta l'autenticazione e l'autorizzazione per le API Web e le Spa

ASP.NET Core 3,0 offre l'autenticazione in app a pagina singola (Spa) usando il supporto per l'autorizzazione dell'API Web. ASP.NET Core identità per l'autenticazione e l'archiviazione degli utenti viene combinata con [IdentityServer4](https://identityserver.io/) per l'implementazione di Open ID Connect.

IdentityServer4 è un Framework di OpenID Connect e OAuth 2,0 per ASP.NET Core 3,0. Consente le seguenti funzionalità di sicurezza:

* Autenticazione come servizio (AaaS)
* Single Sign-on/off (SSO) su più tipi di applicazione
* Controllo di accesso per le API
* Gateway federativo

Per ulteriori informazioni, vedere [la documentazione di IdentityServer4](http://docs.identityserver.io/en/latest/index.html) o [l'autenticazione e l'autorizzazione per le Spa](xref:security/authentication/identity/spa).

## <a name="certificate-and-kerberos-authentication"></a>Certificato e autenticazione Kerberos

L'autenticazione del certificato richiede:

* Configurazione del server per l'accettazione dei certificati.
* Aggiunta del middleware di autenticazione in `Startup.Configure`.
* Aggiunta del servizio di autenticazione del certificato in `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

Le opzioni per l'autenticazione dei certificati includono la possibilità di:

* Accettare certificati autofirmati.
* Controllare la revoca del certificato.
* Verificare che nel certificato fornito siano presenti i flag di utilizzo corretti.

Un'entità utente predefinita viene costruita dalle proprietà del certificato. L'entità utente contiene un evento che consente l'aggiunta o la sostituzione dell'entità. Per ulteriori informazioni, vedere <xref:security/authentication/certauth>.

[L'autenticazione di Windows](/windows-server/security/windows-authentication/windows-authentication-overview) è stata estesa in Linux e MacOS. Nelle versioni precedenti, l'autenticazione di Windows era limitata a [IIS](xref:host-and-deploy/iis/index) e a [HttpSys](xref:fundamentals/servers/httpsys). In ASP.NET Core 3,0, [gheppio](xref:fundamentals/servers/kestrel) è in grado di usare Negotiate, [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview)e [NTLM in Windows](/windows-server/security/kerberos/ntlm-overview), Linux e MacOS per gli host aggiunti a un dominio Windows. Il supporto di gheppio di questi schemi di autenticazione viene fornito dal pacchetto [NuGet Microsoft. AspNetCore. Authentication. Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) . Come per gli altri servizi di autenticazione, configurare la larghezza dell'app di autenticazione, quindi configurare il servizio:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
        .AddNegotiate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

Requisiti host:

* Gli host Windows devono disporre di [nomi di entità servizio](/windows/win32/ad/service-principal-names) (SPN) aggiunti all'account utente che ospita l'app.
* I computer Linux e macOS devono essere aggiunti al dominio.
  * È necessario creare nomi SPN per il processo Web.
  * [I file keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) devono essere generati e configurati nel computer host.

Per ulteriori informazioni, vedere <xref:security/authentication/windowsauth>.

## <a name="template-changes"></a>Modifiche ai modelli

I modelli dell'interfaccia utente Web (Razor Pages, MVC con controller e visualizzazioni) hanno rimosso quanto segue:

* L'interfaccia utente di consenso del cookie non è più inclusa. Per abilitare la funzionalità di consenso dei cookie in un ASP.NET Core app generata da un modello 3,0, vedere <xref:security/gdpr>.
* Agli script e agli asset statici correlati viene ora fatto riferimento come file locali invece di usare CDNs. Per altre informazioni, vedere [gli script e gli asset statici correlati a cui viene ora fatto riferimento come file locali invece di usare CDNs in base all'ambiente corrente (ASPNET/AspNetCore. Docs #14350)](https://github.com/aspnet/AspNetCore.Docs/issues/14350).

Modello angolare aggiornato per l'utilizzo di 8 angolare.

Per impostazione predefinita, il modello RCL (Razor Class Library) per impostazione predefinita è lo sviluppo di componenti Razor. Una nuova opzione di modello in Visual Studio fornisce il supporto del modello per le pagine e le visualizzazioni. Quando si crea un RCL dal modello in una shell dei comandi, passare l'opzione `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`).

## <a name="generic-host"></a>Host generico

I modelli ASP.NET Core 3,0 usano <xref:fundamentals/host/generic-host>. Versioni precedenti utilizzate <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>. L'uso dell'host generico .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) offre una migliore integrazione delle app ASP.NET Core con altri scenari server che non sono specifici del Web. Per ulteriori informazioni, vedere [HostBuilder sostituisce WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder).

### <a name="host-configuration"></a>Configurazione dell'host

Prima del rilascio di ASP.NET Core 3,0, le variabili di ambiente con prefisso `ASPNETCORE_` sono state caricate per la configurazione host dell'host Web. In 3,0 `AddEnvironmentVariables` viene usato per caricare le variabili di ambiente con il prefisso `DOTNET_` per la configurazione host con `CreateDefaultBuilder`.

### <a name="changes-to-startup-constructor-injection"></a>Modifiche all'inserimento del costruttore di avvio

L'host generico supporta solo i tipi seguenti per `Startup` injection del costruttore:

* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

Tutti i servizi possono comunque essere inseriti direttamente come argomenti per il metodo `Startup.Configure`. Per ulteriori informazioni, vedere l' [host generico limita l'inserimento del costruttore di avvio (ASPNET/annunci #353)](https://github.com/aspnet/Announcements/issues/353).

## <a name="kestrel"></a>Kestrel

* La configurazione di Gheppio è stata aggiornata per la migrazione all'host generico. In 3,0, Gheppio viene configurato nel generatore host Web fornito da `ConfigureWebHostDefaults`.
* Le schede di connessione sono state rimosse da gheppio e sostituite con il middleware di connessione, che è simile al middleware HTTP nella pipeline ASP.NET Core ma per le connessioni di livello inferiore.
* Il livello trasporto gheppio è stato esposto come interfaccia pubblica in `Connections.Abstractions`.
* L'ambiguità tra intestazioni e trailer è stata risolta spostando le intestazioni finali in una nuova raccolta.
* Le API di i/o sincrone, ad esempio `HttpRequest.Body.Read`, rappresentano una fonte comune di inedia dei thread che causa arresti anomali dell'app. In 3,0, `AllowSynchronousIO` è disabilitato per impostazione predefinita.

Per ulteriori informazioni, vedere <xref:migration/22-to-30#kestrel>.

## <a name="http2-enabled-by-default"></a>HTTP/2 abilitato per impostazione predefinita

HTTP/2 è abilitato per impostazione predefinita in gheppio per gli endpoint HTTPS. Il supporto HTTP/2 per IIS o HTTP. sys è abilitato quando supportato dal sistema operativo.

## <a name="eventcounters-on-request"></a>EventCounters su richiesta

Il EventSource di hosting, `Microsoft.AspNetCore.Hosting`, emette i nuovi tipi di <xref:System.Diagnostics.Tracing.EventCounter> seguenti correlati alle richieste in ingresso:

* `requests-per-second`
* `total-requests`
* `current-requests`
* `failed-requests`

## <a name="endpoint-routing"></a>Routing degli endpoint

Il routing degli endpoint, che consente ai Framework, ad esempio MVC, di funzionare correttamente con il middleware, viene migliorato:

* L'ordine di middleware ed endpoint è configurabile nella pipeline di elaborazione delle richieste di `Startup.Configure`.
* Gli endpoint e il middleware si compongono bene con altre tecnologie basate su ASP.NET Core, ad esempio i controlli di integrità.
* Gli endpoint possono implementare un criterio, ad esempio CORS o Authorization, sia in middleware che in MVC.
* I filtri e gli attributi possono essere inseriti nei metodi dei controller.

Per ulteriori informazioni, vedere <xref:fundamentals/routing#routing-basics>.

## <a name="health-checks"></a>Controlli di integrità

I controlli di integrità usano il routing degli endpoint con l'host generico. In `Startup.Configure` chiamare `MapHealthChecks` nel generatore di endpoint con l'URL dell'endpoint o il percorso relativo:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

Gli endpoint di controllo integrità possono:

* Specificare uno o più host o porte consentiti.
* Richiedere l'autorizzazione.
* Richiede CORS.

Per altre informazioni, vedere i seguenti articoli:

* <xref:migration/22-to-30#health-checks>
* <xref:host-and-deploy/health-checks>

## <a name="pipes-on-httpcontext"></a>Pipe in HttpContext

È ora possibile leggere il corpo della richiesta e scrivere il corpo della risposta usando l'API <xref:System.IO.Pipelines>. Alla classe <!-- <xref:Microsoft.AspNetCore.Http.HttpRequest.BodyReader> --> `HttpRequest.BodyReader` proprietà fornisce un <xref:System.IO.Pipelines.PipeReader> che può essere utilizzato per leggere il corpo della richiesta. Alla classe <!-- <xref:Microsoft.AspNetCore.Http.> --> `HttpResponse.BodyWriter` proprietà fornisce un <xref:System.IO.Pipelines.PipeWriter> che può essere utilizzato per scrivere il corpo della risposta. `HttpRequest.BodyReader` è un analogo del flusso di `HttpRequest.Body`. `HttpResponse.BodyWriter` è un analogo del flusso di `HttpResponse.Body`.

<!-- indirectly related, https://github.com/dotnet/docs/pull/14414 won't be published by 9/23  -->

## <a name="improved-error-reporting-in-iis"></a>Segnalazione degli errori migliorata in IIS

Gli errori di avvio quando si ospitano app ASP.NET Core in IIS generano ora dati di diagnostica più completi. Questi errori vengono segnalati al registro eventi di Windows con le tracce dello stack laddove applicabile. Inoltre, tutti gli avvisi, gli errori e le eccezioni non gestite vengono registrati nel registro eventi di Windows.

## <a name="worker-service-and-worker-sdk"></a>Worker Service e worker SDK

.NET Core 3,0 introduce il nuovo modello di app del servizio Worker. Questo modello offre un punto di partenza per la scrittura di servizi con esecuzione prolungata in .NET Core.

Per altre informazioni, vedere:

* [Ruoli di lavoro di .NET Core come servizi Windows](https://devblogs.microsoft.com/aspnet/net-core-workers-as-windows-services/)
* <xref:fundamentals/host/hosted-services>
* <xref:host-and-deploy/windows-service>

## <a name="forwarded-headers-middleware-improvements"></a>Miglioramenti del middleware per intestazioni con inoltri

Nelle versioni precedenti di ASP.NET Core la chiamata di <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> e <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> è problematica quando è stata distribuita in Azure Linux o dietro qualsiasi proxy inverso diverso da IIS. La correzione per le versioni precedenti è documentata in [inoltro dello schema per i proxy inversi di Linux e non IIS](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies).

Questo scenario è stato risolto in ASP.NET Core 3,0. L'host Abilita il [middleware delle intestazioni con inoltri](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options) quando la variabile di ambiente `ASPNETCORE_FORWARDEDHEADERS_ENABLED` è impostata su `true`. `ASPNETCORE_FORWARDEDHEADERS_ENABLED` è impostato su `true` nelle immagini del contenitore.

## <a name="performance-improvements"></a>Miglioramenti delle prestazioni

ASP.NET Core 3,0 include numerosi miglioramenti che consentono di ridurre l'utilizzo della memoria e migliorare la velocità effettiva:

* Riduzione dell'utilizzo della memoria quando si utilizza il contenitore incorporato di inserimento delle dipendenze per i servizi con ambito.
* Riduzione delle allocazioni nel Framework, inclusi gli scenari middleware e il routing.
* Riduzione dell'utilizzo della memoria per le connessioni WebSocket.
* Miglioramenti della velocità effettiva e della riduzione della memoria per le connessioni HTTPS.
* Nuovo serializzatore JSON ottimizzato e completamente asincrono.
* Riduzione dell'utilizzo della memoria e miglioramento della velocità effettiva nell'analisi dei moduli.

## <a name="aspnet-core-30-only-runs-on-net-core-30"></a>ASP.NET Core 3,0 viene eseguito solo in .NET Core 3,0

A partire da ASP.NET Core 3,0, .NET Framework non è più un Framework di destinazione supportato. I progetti destinati a .NET Framework possono continuare in modo completamente supportato con la [versione di .NET Core 2,1 LTS](https://www.microsoft.com/net/download/dotnet-core/2.1). La maggior parte dei pacchetti di ASP.NET Core 2.1. x saranno supportati per un tempo illimitato, oltre il periodo LTS di tre anni per .NET Core 2,1.

Per informazioni sulla migrazione, vedere [trasferire il codice da .NET Framework a .NET Core](/dotnet/core/porting/).

## <a name="use-the-aspnet-core-shared-framework"></a>Usare il Framework condiviso ASP.NET Core

Il Framework condiviso ASP.NET Core 3,0, contenuto nel [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), non richiede più un elemento `<PackageReference />` esplicito nel file di progetto. Al Framework condiviso viene fatto automaticamente riferimento quando si usa l'SDK `Microsoft.NET.Sdk.Web` nel file di progetto:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

## <a name="assemblies-removed-from-the-aspnet-core-shared-framework"></a>Assembly rimossi dal Framework condiviso ASP.NET Core

Gli assembly più rilevanti rimossi dal Framework condiviso ASP.NET Core 3,0 sono:

* [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/) (JSON.NET). Per aggiungere Json.NET a ASP.NET Core 3,0, vedere [aggiungere il supporto per il formato JSON basato su Newtonsoft. JSON](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support). ASP.NET Core 3,0 introduce `System.Text.Json` per la lettura e la scrittura di JSON. Per ulteriori informazioni, vedere la pagina relativa alla [nuova serializzazione JSON](#new-json-serialization) in questo documento.
* [Entity Framework Core](/ef/core/)

Per un elenco completo degli assembly rimossi dal Framework condiviso, vedere [assembly rimossi da Microsoft. AspNetCore. App 3,0](https://github.com/aspnet/AspNetCore/issues/3755). Per altre informazioni sulla motivazione di questa modifica, vedere [modifiche di rilievo a Microsoft. AspNetCore. app in 3,0](https://github.com/aspnet/Announcements/issues/325) e [una prima occhiata alle modifiche apportate in ASP.NET Core 3,0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).

<!-- 
## Additional information
For the complete list of changes, see the [ASP.NET Core 3.0 Release Notes](WHERE IS THIS????).
-->
