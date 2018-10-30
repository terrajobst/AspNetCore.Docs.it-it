---
title: Implementazione del server Web Kestrel in ASP.NET Core
author: rick-anderson
description: Informazioni su Kestrel, il server Web multipiattaforma per ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 7e47bc3df90a72f15a6fdde080aae6fbf7494384
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207459"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>Implementazione del server Web Kestrel in ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Stephen Halter](https://twitter.com/halter73)

Kestrel è un [server Web per ASP.NET Core](xref:fundamentals/servers/index) multipiattaforma. Kestrel è il server Web incluso per impostazione predefinita nei modelli di progetto di ASP.NET Core.

Kestrel supporta le funzionalità seguenti:

::: moniker range=">= aspnetcore-2.2"

* HTTPS
* Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)
* Socket Unix ad alte prestazioni dietro Nginx
* HTTP/2 (tranne che in macOS&dagger;)

&dagger;HTTP/2 verrà supportato in macOS in una versione futura.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* HTTPS
* Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)
* Socket Unix ad alte prestazioni dietro Nginx

::: moniker-end

Kestrel è supportato in tutte le piattaforme e le versioni supportate da .NET Core.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a>Supporto per HTTP/2

[HTTP/2](https://httpwg.org/specs/rfc7540.html) è disponibile per le app ASP.NET Core se vengono soddisfatti i requisiti di base seguenti:

* Sistema operativo&dagger;
  * Windows Server 2012 R2/Windows 8.1 o versioni successive
  * Linux con OpenSSL 1.0.2 o versioni successive (ad esempio, Ubuntu 16.04 o versioni successive)
* Framework di destinazione: .NET Core 2.2 o versioni successive
* Connessione [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3)
* Connessione TLS 1.2 o successiva

&dagger;HTTP/2 verrà supportato in macOS in una versione futura.

Se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/2`.

HTTP/2 è disabilitato per impostazione predefinita. Per altre informazioni sulla configurazione, vedere le sezioni [Opzioni Kestrel](#kestrel-options) e [Configurazione dell'endpoint](#endpoint-configuration).

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Quando usare Kestrel con un proxy inverso

::: moniker range=">= aspnetcore-2.0"

È possibile usare Kestrel da solo o con un *server proxy inverso*, ad esempio IIS, Nginx o Apache. Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

Entrambe le configurazioni (con o senza un server proxy inverso) sono configurazioni di hosting valide e supportate per le app ASP.NET Core 2.0 o versioni successive.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Se un'app accetta solo le richieste provenienti da una rete interna, è possibile usare Kestrel direttamente come server dell'app.

![Kestrel comunica direttamente con la rete interna](kestrel/_static/kestrel-to-internal.png)

Se si espone l'app a Internet, è necessario usare IIS, Nginx o Apache come *server proxy inverso*. Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

Per motivi di sicurezza, è richiesto un proxy inverso per le distribuzioni di server perimetrali rivolte al pubblico, ovvero le distribuzioni esposte al traffico Internet. Le versioni 1.x di Kestrel non hanno una struttura completa di difesa contro gli attacchi, ad esempio timeout appropriati, limiti di dimensioni e limiti di connessioni simultanee.

::: moniker-end

Esiste uno scenario con proxy inverso quando diverse app condividono lo stesso IP e la stessa porta in un unico server. Kestrel non supporta questo scenario perché non supporta la condivisione dello stesso IP e della stessa porta tra più processi. Quando Kestrel è configurato per restare in ascolto su una porta, gestisce tutto il traffico per tale porta indipendentemente dall'intestazione host delle richieste. Un proxy inverso che può condividere le porte può eseguire l'inoltro delle richieste a Kestrel su un unico IP e un'unica porta.

Anche se un server proxy inverso non è necessario, può risultare utile per i motivi seguenti:

* Limita l'area di superficie di attacco pubblica esposta delle app ospitate.
* Offre un livello aggiuntivo di configurazione e protezione.
* Può offrire un'integrazione migliore con l'infrastruttura esistente.
* Semplifica il bilanciamento del carico e la configurazione SSL. Solo il server proxy inverso richiede un certificato SSL e tale server può comunicare con i server delle applicazioni sulla rete interna mediante protocollo HTTP semplice.

> [!WARNING]
> Se non si usa un proxy inverso con filtro host abilitato, è necessario abilitate il [filtro host](#host-filtering).

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Come usare Kestrel nelle app ASP.NET Core

::: moniker range=">= aspnetcore-2.0"

Il pacchetto [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) è incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versione successiva).

I modelli di progetto ASP.NET Core usano Kestrel per impostazione predefinita. In *Program.cs* il codice modello chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), che a sua volta chiama [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) in background.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Per fornire una configurazione aggiuntiva dopo la chiamata di `CreateDefaultBuilder`, usare `ConfigureKestrel`:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

Per fornire una configurazione aggiuntiva dopo la chiamata di `CreateDefaultBuilder`, chiamare [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        })
        .Build();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Installare il pacchetto NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Chiamare il metodo di estensione [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) su [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) nel metodo `Main`, specificando le [opzioni Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) richieste, come visualizzato nella sezione seguente.

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

::: moniker-end

## <a name="kestrel-options"></a>Opzioni Kestrel

::: moniker range=">= aspnetcore-2.0"

Il server Web Kestrel dispone di opzioni di configurazione dei vincoli che risultano particolarmente utili nelle distribuzioni con connessione Internet. Alcuni limiti importanti che possono essere personalizzati:

* Numero massimo di connessioni client
* Dimensione massima del corpo della richiesta
* Velocità minima dei dati del corpo della richiesta

Impostare questi e altri vincoli nella proprietà [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) della classe [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions). La proprietà `Limits` contiene un'istanza della classe [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).

### <a name="maximum-client-connections"></a>Numero massimo di connessioni client

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

::: moniker-end

È possibile impostare il numero massimo di connessioni TCP aperte simultaneamente per l'intera app con il codice seguente:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

È previsto un limite separato per le connessioni che sono state aggiornate da HTTP o HTTPS a un altro protocollo (ad esempio su una richiesta WebSocket). Dopo l'aggiornamento di una connessione, questa non viene conteggiata per il limite `MaxConcurrentConnections`.

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Per impostazione predefinita, il numero massimo di connessioni è illimitato (null).

### <a name="maximum-request-body-size"></a>Dimensione massima del corpo della richiesta

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

La dimensione massima predefinita del corpo della richiesta è pari a 30.000.000 di byte, equivalenti a circa 28,6 MB.

Il metodo consigliato per ignorare il limite in un'app ASP.NET Core MVC è l'uso dell'attributo [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) in un metodo di azione:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

::: moniker-end

L'esempio seguente illustra come configurare il vincolo per l'app in ogni richiesta:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

È possibile eseguire l'override dell'impostazione per una richiesta specifica nel middleware:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Se si tenta di configurare il limite per una richiesta dopo che l'app ha avviato la lettura della richiesta stessa, viene generata un'eccezione. Una proprietà `IsReadOnly` indica se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.

### <a name="minimum-request-body-data-rate"></a>Velocità minima dei dati del corpo della richiesta

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

Kestrel controlla ogni secondo se i dati arrivano alla velocità in byte al secondo specificata. Se la velocità scende sotto il valore minimo, la connessione raggiunge il timeout. Il periodo di tolleranza è la quantità di tempo che Kestrel concede al client per aumentare la velocità di trasmissione fino al valore minimo. Durante tale periodo la velocità non viene rilevata. Il periodo di prova consente di evitare l'interruzione di connessioni che inizialmente inviano i dati a velocità ridotta a causa dell'avvio lento del protocollo TCP.

La velocità minima predefinita è di 240 byte al secondo, con un periodo di tolleranza di 5 secondi.

Anche per la risposta è prevista una velocità minima. Il codice per impostare il limite della richiesta e della risposta è identico e varia solo per `RequestBody` o `Response` nei nomi di proprietà e di interfaccia.

L'esempio seguente visualizza come configurare la velocità minima dei dati in *Program.cs*:

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

È possibile configurare la velocità per ogni singola richiesta nel middleware:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a>Numero massimo di flussi per connessione

`Http2.MaxStreamsPerConnection` limita il numero di flussi di richieste simultanee per ogni connessione HTTP/2. I flussi in eccesso vengono rifiutati.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

Il valore predefinito è 100.

### <a name="header-table-size"></a>Dimensioni della tabella delle intestazioni

Il decodificatore HPACK decomprime le intestazioni HTTP per le connessioni HTTP/2. `Http2.HeaderTableSize` limita le dimensioni della tabella di compressione delle intestazioni usata dal decodificatore HPACK. Il valore viene specificato in ottetti e deve essere maggiore di zero (0).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

Il valore predefinito è 4096.

### <a name="maximum-frame-size"></a>Dimensione massima del frame

`Http2.MaxFrameSize` indica le dimensioni massime del payload del frame di connessione HTTP/2 da ricevere. Il valore viene specificato in ottetti e deve essere compreso tra 2^14 (16.384) e 2^24-1 (16.777.215).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

Il valore predefinito è 2^14 (16.384).

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Per informazioni su altre opzioni e limiti di Kestrel, vedere:

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Per informazioni su opzioni e limiti di Kestrel, vedere:

* [KestrelServerOptions class](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) (Classe KestrelServerOptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

::: moniker-end

## <a name="endpoint-configuration"></a>Configurazione dell'endpoint

::: moniker range="= aspnetcore-2.0"

Per impostazione predefinita, ASP.NET Core è associato a `http://localhost:5000`. Chiamare i metodi [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) oppure [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) su [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) per configurare i prefissi URL e le porte per Kestrel. Anche `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` e la variabile di ambiente `ASPNETCORE_URLS` funzionano, ma con le limitazioni indicate più avanti nella sezione.

La chiave di configurazione dell'host `urls` deve provenire dalla configurazione dell'host, non dalla configurazione dell'app. L'aggiunta di una chiave `urls` e di un valore a *appsettings.json* non influisce sulla configurazione dell'host perché l'host viene completamente inizializzato prima che la configurazione venga letta dal file di configurazione. Tuttavia, una chiave `urls` in *appsettings.json* può essere usata con [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) nel generatore di host per configurare l'host:

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Per impostazione predefinita, ASP.NET Core è associato a:

* `http://localhost:5000`
* `https://localhost:5001` (quando è presente un certificato di sviluppo locale)

Viene creato un certificato di sviluppo nei casi seguenti:

* Quando viene installato [.NET Core SDK](/dotnet/core/sdk).
* Quando per creare un certificato viene usato [lo strumento dev-certs](xref:aspnetcore-2.1#https).

Alcuni browser richiedono un'autorizzazione esplicita per considerare attendibile il certificato di sviluppo locale.

ASP.NET Core 2.1 e le versioni successive dei modelli di progetto configurano le app da eseguire su HTTPS per impostazione predefinita e includono [il reindirizzamento a HTTPS e il supporto di HSTS](xref:security/enforcing-ssl).

Chiamare i metodi [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) oppure [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) su [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) per configurare i prefissi URL e le porte per Kestrel.

Anche `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` e la variabile di ambiente `ASPNETCORE_URLS` funzionano, ma con le limitazioni indicate più avanti nella sezione (deve essere disponibile un certificato predefinito per la configurazione dell'endopoint HTTPS).

Configurazione di `KestrelServerOptions` in ASP.NET Core 2.1:

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a>ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)

Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint specificato. Se si chiama `ConfigureEndpointDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a>ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)

Specifica un elemento di configurazione `Action` da eseguire per ogni endpoint HTTPS. Se si chiama `ConfigureHttpsDefaults` più volte, gli elementi `Action` precedenti vengono sostituiti con l'ultimo elemento `Action` specificato.

### <a name="configureiconfiguration"></a>Configure(IConfiguration)

Crea un loader di configurazione per la configurazione di Kestrel che accetta un elemento [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) come input. L'ambito della configurazione deve essere la sezione di configurazione per Kestrel.

### <a name="listenoptionsusehttps"></a>ListenOptions.UseHttps

Configura Kestrel per l'uso di HTTPS.

Estensioni `ListenOptions.UseHttps`:

* `UseHttps` - Configura Kestrel per l'uso di HTTPS con il certificato predefinito. Genera un'eccezione se non è stato configurato alcun certificato predefinito.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

Parametri `ListenOptions.UseHttps`:

* `filename` è il percorso e il nome file di un file di certificato, relativo alla directory che contiene i file di contenuto dell'app.
* `password` è la password richiesta per accedere ai dati del certificato X.509.
* `configureOptions` è un elemento `Action` per configurare `HttpsConnectionAdapterOptions`. Restituisce `ListenOptions`.
* `storeName` è l'archivio certificati da cui caricare il certificato.
* `subject` è il nome dell'oggetto del certificato.
* `allowInvalid` indica se devono essere considerati i certificati non validi, come ad esempio i certificati autofirmati.
* `location` è il percorso dell'archivio da cui caricare il certificato.
* `serverCertificate` è il certificato X.509.

Nell'ambiente di produzione, HTTPS deve essere configurato in modo esplicito. Come minimo, è necessario specificare un certificato predefinito.

Configurazioni supportate descritte in seguito:

* Nessuna configurazione
* Sostituire il certificato predefinito della configurazione
* Modificare le impostazioni predefinite nel codice

*Nessuna configurazione*

Kestrel è in ascolto su `http://localhost:5000` e `https://localhost:5001` (se è disponibile un certificato predefinito).

Specificare gli URL usando gli elementi seguenti:

* La variabile di ambiente `ASPNETCORE_URLS`.
* L'argomento della riga di comando `--urls`.
* La chiave di configurazione dell'host `urls`.
* Il metodo di estensione `UseUrls`.

Per altre informazioni, vedere [URL del server](xref:fundamentals/host/web-host#server-urls) e [Override della configurazione](xref:fundamentals/host/web-host#override-configuration).

Il valore specificato usando i metodi seguenti può essere uno o più endpoint HTTP e HTTPS (HTTPS se è disponibile un certificato predefinito). Configurare il valore come un elenco delimitato da punto e virgola (ad esempio, `"Urls": "http://localhost:8000; http://localhost:8001"`).

*Sostituire il certificato predefinito della configurazione*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) chiama `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` per impostazione predefinita per caricare la configurazione Kestrel. È disponibile per Kestrel uno schema di configurazione delle impostazioni delle app HTTPS predefinito. Configurare più endpoint, inclusi gli URL e i certificati da usare, da un file su disco o da un archivio certificati.

Nel file *appsettings.json* di esempio seguente:

* Impostare **AllowInvalid** su `true` per consentire l'uso di certificati non validi, come ad esempio i certificati autofirmati.
* Tutti gli endpoint HTTPS che non specificano un certificato (**HttpsDefaultCert** nell'esempio che segue) usano il certificato definito in **Certificates** > **Default**  o il certificato di sviluppo.

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Un'alternativa all'uso di **Path** e **Password** per qualsiasi nodo del certificato consiste nello specificare il certificato usando i campi dell'archivio certificati. Ad esempio, il certificato specificato mediante **Certificates** > **Default** può essere specificato come:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Note di schema:

* I nomi degli endpoint non applicano la distinzione tra maiuscole e minuscole. Ad esempio, sono validi sia `HTTPS` che `Https`.
* Il parametro `Url` è obbligatorio per ogni endpoint. Il formato per questo parametro è uguale a quello del parametro di configurazione di primo livello `Urls`, con la differenza che si limita a un singolo valore.
* Questi endpoint sostituiscono quelli definiti nella configurazione di primo livello `Urls`, non vi si aggiungono. Gli endpoint definiti nel codice tramite `Listen` si aggiungono agli endpoint definiti nella sezione di configurazione.
* La sezione `Certificate` è facoltativa. Se la sezione `Certificate` non è specificata, vengono usati i valori predefiniti definiti negli scenari precedenti. Se non è disponibile alcun valore predefinito, il server genera un'eccezione e non viene avviato.
* La sezione `Certificate` supporta sia i certificati **Path**&ndash;**Password** che i certificati **Subject**&ndash;**Store**.
* In questo modo è possibile definire un numero qualsiasi di endpoint, purché non provochino conflitti di porte.
* `options.Configure(context.Configuration.GetSection("Kestrel"))` restituisce un oggetto `KestrelConfigurationLoader` con un metodo `.Endpoint(string name, options => { })` che può essere usato per integrare le impostazioni dell'endpoint configurato:

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  È anche possibile accedere direttamente a `KestrelServerOptions.ConfigurationLoader` per mantenere l'iterazione nel caricatore esistente, ad esempio quello specificato da [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

* La sezione di configurazione per ogni endpoint è disponibile sulle opzioni nel metodo `Endpoint` in modo che le impostazioni personalizzate possano essere lette.
* È possibile caricare più configurazioni chiamando ancora `options.Configure(context.Configuration.GetSection("Kestrel"))` con un'altra sezione. Viene usata solo l'ultima configurazione, a meno che `Load` sia stato chiamato esplicitamente in istanze precedenti. Il metapacchetto non chiama `Load` in modo che la sezione di configurazione predefinita venga sostituita.
* `KestrelConfigurationLoader` riflette la famiglia di API `Listen` da `KestrelServerOptions` all'overload di `Endpoint`, in modo che gli endpoint di codice e di configurazione possano essere configurati nella stessa posizione. Questi overload non usano nomi e usano solo le impostazioni predefinite della configurazione.

*Modificare le impostazioni predefinite nel codice*

`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` possono essere usati per modificare le impostazioni predefinite per `ListenOptions` e `HttpsConnectionAdapterOptions`, inclusa l'esecuzione dell'override del certificato predefinito specificato nello scenario precedente. `ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` devono essere chiamati prima che venga configurato qualsiasi endpoint.

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*Supporto kestrel per SNI*

[L'Indicazione nome server (SNI)](https://tools.ietf.org/html/rfc6066#section-3) può essere usata per ospitare più domini sulla stessa porta e indirizzo IP. Perché SNI funzioni è necessario che il client invii al server il nome host per la sessione protetta durante l'handshake TLS in modo che il server possa specificare il certificato corretto. Il client usa il certificato specificato per la comunicazione crittografata con il server durante la sessione protetta che segue l'handshake TLS.

Kestrel supporta SNI tramite il callback `ServerCertificateSelector`. Il callback viene richiamato una volta per ogni connessione per consentire all'app di controllare il nome host e selezionare il certificato appropriato.

Il supporto SNI richiede:

* Esecuzione in `netcoreapp2.1` del framework di destinazione. In `netcoreapp2.0` e `net461` il callback viene richiamato, ma l'elemento `name` è sempre `null`. L'elemento `name` è `null` anche se il client non specifica il parametro del nome host nell'handshake TLS.
* Esecuzione di tutti i siti Web nella stessa istanza di Kestrel. Kestrel supporta la condivisione di un indirizzo IP e di una porta tra più istanze solo con un proxy inverso.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        })
        .Build();
```

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a>Associazione a un socket TCP

Il metodo [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) esegue l'associazione a un socket TCP. Un'espressione lambda per le opzioni consente di configurare un certificato SSL:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

L'esempio configura SSL per un endpoint con [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions). Usare la stessa API per configurare altre impostazioni di Kestrel per endpoint specifici.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a>Associazione a un socket Unix

Impostare l'ascolto su un socket Unix con [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) per migliorare le prestazioni con Nginx, come visualizzato nell'esempio seguente:

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="port-0"></a>Porta 0

Quando si specifica il numero di porta `0`, Kestrel esegue l'associazione dinamica a una porta disponibile. L'esempio seguente indica come determinare la porta alla quale Kestrel ha eseguito l'associazione in fase di esecuzione:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Quando si esegue l'app, l'output della finestra della console indica la porta dinamica in cui l'app può essere raggiunta:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a>Limitazioni

Configurare gli endpoint con i metodi seguenti:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* L'argomento della riga di comando `--urls`
* La chiave di configurazione dell'host `urls`
* La variabile di ambiente `ASPNETCORE_URLS`

Questi metodi sono utili se si vuole che il codice funzioni con server diversi da Kestrel. Tenere presenti, tuttavia, le limitazioni seguenti:

* SSL non può essere usato con questi approcci, a meno che non venga specificato un certificato predefinito nella configurazione dell'endpoint HTTPS (ad esempio, usando la configurazione `KestrelServerOptions` o un file di configurazione come illustrato in precedenza in questo argomento).
* Quando sia l'approccio `Listen` che l'approccio `UseUrls` vengono usati contemporaneamente, gli endpoint `Listen` eseguono l'override degli endpoint `UseUrls`.

### <a name="iis-endpoint-configuration"></a>Configurazione dell'endpoint IIS

Quando si usa IIS, le associazioni di URL per le associazioni di override di IIS vengono impostate mediante `Listen` o `UseUrls`. Per altre informazioni, vedere l'articolo [Introduzione al modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Per impostazione predefinita, ASP.NET Core è associato a `http://localhost:5000`. Configurare i prefissi URL e le porte per l'uso di Kestrel:

* Il metodo di estensione [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)
* L'argomento della riga di comando `--urls`
* La chiave di configurazione dell'host `urls`
* Il sistema di configurazione di ASP.NET Core, inclusa la variabile di ambiente `ASPNETCORE_URLS`

Per altre informazioni su questi metodi, vedere [Hosting](xref:fundamentals/host/index).

### <a name="iis-endpoint-configuration"></a>Configurazione dell'endpoint IIS

Quando si usa IIS, le associazioni di URL per le associazioni di override di IIS vengono impostate mediante `UseUrls`. Per altre informazioni, vedere l'articolo [Introduzione al modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a>ListenOptions.Protocols

La proprietà `Protocols` stabilisce i protocolli HTTP (`HttpProtocols`) abilitati in un endpoint di connessione o per il server. Assegnare un valore alla proprietà `Protocols` dall'enumerazione `HttpProtocols`.

| Valore di enumerazione `HttpProtocols` | Protocollo di connessione consentito |
| -------------------------- | ----------------------------- |
| `Http1`                    | Solo HTTP/1.1. Può essere usato con o senza TLS. |
| `Http2`                    | Solo HTTP/2. Viene essenzialmente usato con TLS. Può essere usato senza TLS solo se il client supporta una [modalità di conoscenza pregressa](https://tools.ietf.org/html/rfc7540#section-3.4). |
| `Http1AndHttp2`            | HTTP/1.1 e HTTP/2. È necessaria una connessione TLS e [ALPN (Application Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3) per negoziare HTTP/2; in caso contrario, la connessione viene automaticamente impostata su HTTP/1.1. |

Il protocollo predefinito è HTTP/1.1.

Restrizioni relative a TLS per HTTP/2:

* TLS versione 1.2 o successiva
* Rinegoziazione disabilitata
* Compressione disabilitata
* Dimensioni minime per lo scambio di chiavi temporanee:
  * Diffie-Hellman a curva ellittica (ECDH) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit (minimo)
  * Diffie-Hellman a campo finito (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bit (minimo)
* Pacchetto di crittografia consentito

`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; con curva ellittica P-256 &lbrack;`FIPS186`&rbrack; è supportato per impostazione predefinita.

Nell'esempio seguente sono consentite connessioni HTTP/1.1 e HTTP/2 sulla porta 8000. Le connessioni sono protette da TLS con un certificato incluso:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        }
```

Facoltativamente, creare un'implementazione `IConnectionAdapter` per filtrare gli handshake TLS in base alla connessione in caso di crittografie specifiche:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
                listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
            });
        }
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that you don't 
        // wish to support. Alternatively, define and compare 
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher 
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null 
        // indicates that no cipher algorithm supported by Kestrel matches the 
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

*Impostare il protocollo dalla configurazione*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) chiama `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` per impostazione predefinita per caricare la configurazione Kestrel.

Nell'esempio *appsettings.json* seguente viene stabilito un protocollo di connessione predefinito (HTTP/1.1 e HTTP/2) per tutti gli endpoint Kestrel:

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

Il file di configurazione di esempio seguente stabilisce un protocollo di connessione per un endpoint specifico:

```json
{
  "Kestrel": {
    "EndPoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

I protocolli specificati nei valori di override del codice sono impostati dalla configurazione.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a>Configurazione del trasporto

Con la versione ASP.NET Core 2.1, il trasporto predefinito di Kestrel non si basa più su Libuv, ma su socket gestiti. Si tratta di una modifica importante per le app ASP.NET 2.0 Core che vengono aggiornate alla versione 2.1, che chiamano [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) e dipendono da uno dei pacchetti seguenti:

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (riferimento diretto al pacchetto)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

Per i progetti ASP.NET Core 2.1 o versioni successive che usano il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) e richiedono l'uso di Libuv:

* Aggiungere una dipendenza per il pacchetto [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al file di progetto dell'app:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* Chiamare [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a>Prefissi URL

Se si usa `UseUrls`, l'argomento della riga di comando `--urls`, la chiave di configurazione dell'host `urls` o la variabile di ambiente `ASPNETCORE_URLS`, i prefissi URL possono avere uno dei formati seguenti.

::: moniker range=">= aspnetcore-2.0"

Sono validi solo i prefissi URL HTTP. Kestrel non supporta SSL quando la configurazione delle associazioni URL viene eseguita con `UseUrls`.

* Indirizzo IPv4 con numero di porta

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.

* Indirizzo IPv6 con numero di porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` è l'equivalente IPv6 di `0.0.0.0` per IPv4.

* Nome host con numero di porta

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  I nomi host, `*` e `+` non sono casi particolari. Tutto ciò che non è riconosciuto come un indirizzo IP o un elemento `localhost` valido esegue l'associazione a tutti gli IP IPv4 e IPv6. Per associare nomi host diversi ad app ASP.NET Core diverse sulla stessa porta, usare [HTTP.sys](xref:fundamentals/servers/httpsys) o un server proxy inverso come IIS, Nginx o Apache.

  > [!WARNING]
  > Se non si usa un proxy inverso con filtro host abilitato, abilitare il [filtro host](#host-filtering).

* Nome host `localhost` con numero di porta o IP di loopback con numero di porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Se è specificato `localhost`, Kestrel tenta l'associazione alle interfacce di loopback sia IPv4 che IPv6. Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato. Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* Indirizzo IPv4 con numero di porta

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  `0.0.0.0` è un caso speciale che esegue l'associazione a tutti gli indirizzi IPv4.

* Indirizzo IPv6 con numero di porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  `[::]` è l'equivalente IPv6 di `0.0.0.0` per IPv4.

* Nome host con numero di porta

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  I nomi host, `*` e `+` non sono casi particolari. Tutto ciò che non è un indirizzo IP o un elemento `localhost` riconosciuto esegue l'associazione a tutti gli IP IPv4 e IPv6. Per associare nomi host diversi ad app ASP.NET Core diverse sulla stessa porta, usare [WebListener](xref:fundamentals/servers/weblistener) o un server proxy inverso come IIS, Nginx o Apache.

* Nome host `localhost` con numero di porta o IP di loopback con numero di porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Se è specificato `localhost`, Kestrel tenta l'associazione alle interfacce di loopback sia IPv4 che IPv6. Se la porta richiesta è in uso da parte di un altro servizio in una delle due interfacce di loopback, Kestrel non viene avviato. Se una delle due interfacce di loopback non è disponibile per qualsiasi altro motivo (in genere perché IPv6 non è supportato), Kestrel registra un avviso.

* Socket UNIX

  ```
  http://unix:/run/dan-live.sock
  ```

**Porta 0**

Quando si specifica il numero di porta `0`, Kestrel esegue l'associazione dinamica a una porta disponibile. L'associazione alla porta `0` è consentita per qualsiasi nome host o IP ad eccezione di `localhost`.

Quando si esegue l'app, l'output della finestra della console indica la porta dinamica in cui l'app può essere raggiunta:

```console
Now listening on: http://127.0.0.1:48508
```

**Prefissi URL per SSL**

Se si chiama il metodo di estensione `UseHttps`, assicurarsi di includere i prefissi URL con `https:`:

```csharp
var host = new WebHostBuilder()
    .UseKestrel(options =>
    {
        options.UseHttps("testCert.pfx", "testPassword");
    })
   .UseUrls("http://localhost:5000", "https://localhost:5001")
   .UseContentRoot(Directory.GetCurrentDirectory())
   .UseStartup<Startup>()
   .Build();
```

> [!NOTE]
> HTTPS e HTTP non possono essere ospitati sulla stessa porta.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

::: moniker-end

## <a name="host-filtering"></a>Filtro host

Mentre supporta la configurazione in base ai prefissi, ad esempio `http://example.com:5000`, Kestrel ignora quasi sempre il nome host. L'host `localhost` è un caso speciale usato per l'associazione agli indirizzi di loopback. Qualsiasi host che non sia un indirizzo IP esplicito esegue l'associazione a tutti gli indirizzi IP pubblici. Nessuna di queste informazioni viene usata per convalidare le intestazioni `Host` della richiesta.

::: moniker range="< aspnetcore-2.0"

Come soluzione alternativa, eseguire l'host in un proxy inverso con filtro delle intestazioni host. Questo è l'unico scenario supportato per Kestrel in ASP.NET Core 1.x.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Come soluzione alternativa, usare un middleware per filtrare le richieste in base all'intestazione `Host`:

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

Registrare l'elemento `HostFilteringMiddleware` precedente in `Startup.Configure`. Si noti che l'[ordinamento della registrazione del middleware](xref:fundamentals/middleware/index#order) è importante. La registrazione deve verificarsi immediatamente dopo la registrazione del middleware di diagnostica (ad esempio, `app.UseExceptionHandler`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

Per il middleware è prevista una chiave `AllowedHosts` in *appsettings.json*/*appsettings.\<NomeAmbiente>.json*. Il valore è un elenco con valori delimitati da punto e virgola di nomi host senza numeri di porta:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Come soluzione alternativa, usare il middleware di filtro host. Il middleware di filtro host è disponibile nel pacchetto [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive). Il middleware viene aggiunto da [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), che chiama [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Per impostazione predefinita, il middleware di filtro host è disabilitato per impostazione predefinita. Per abilitare il middleware, definire una chiave `AllowedHosts` in *appsettings.json*/*appsettings.\<NomeAmbiente>.json*. Il valore è un elenco con valori delimitati da punto e virgola di nomi host senza numeri di porta:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

*appsettings.json*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> Anche il [middleware di intestazioni inoltrate](xref:host-and-deploy/proxy-load-balancer) ha un'opzione [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts). Il middleware di intestazioni inoltrate e il middleware di filtro host hanno funzionalità simili per diversi scenari. L'impostazione di `AllowedHosts` con il middleware di intestazioni inoltrate è appropriato se l'intestazione host non viene mantenuta durante l'inoltro delle richieste con un server proxy inverso o il bilanciamento del carico. L'impostazione di `AllowedHosts` con il middleware di filtro host è appropriata se si usa Kestrel come server perimetrale rivolto al pubblico o se l'intestazione host viene inoltrata direttamente.
>
> Per altre informazioni sul middleware di intestazioni inoltrate, vedere [Configurare ASP.NET Core per l'uso di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* [Imporre HTTPS](xref:security/enforcing-ssl)
* [Codice sorgente di Kestrel](https://github.com/aspnet/KestrelHttpServer)
* [RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4) (RFC 7230: Sintassi e routing dei messaggi (sezione 5.4: Host))
* [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer)
