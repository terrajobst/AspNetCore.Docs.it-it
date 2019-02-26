---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Informazioni sui concetti fondamentali per la compilazione di app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/index
---
# <a name="aspnet-core-fundamentals"></a>Nozioni fondamentali su ASP.NET Core

Questo articolo contiene una panoramica degli argomenti chiave necessari per comprendere come sviluppare app ASP.NET Core.

## <a name="the-startup-class"></a>Classe Startup

All'interno della classe `Startup`:

* Viene eseguita la configurazione di tutti i servizi necessari per l'app.
* Viene definita la pipeline di gestione delle richieste.

* Il codice per configurare o *registrare* i servizi viene aggiunto al metodo `Startup.ConfigureServices`. I *servizi* sono componenti usati dall'app. Un oggetto di contesto Entity Framework Core, ad esempio, è un servizio.
* Il codice per configurare la pipeline di gestione delle richieste viene aggiunto al metodo `Startup.Configure`. La pipeline è strutturata come una serie di componenti *middleware*. Un componente middleware, ad esempio, potrebbe gestire le richieste per i file statici o reindirizzare le richieste HTTP a HTTPS. Ogni componente middleware esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo della pipeline oppure termina la richiesta.

::: moniker range=">= aspnetcore-2.0"

Ecco un esempio di classe `Startup`:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

::: moniker-end

Per altre informazioni, vedere [Avvio dell'app](xref:fundamentals/startup).

## <a name="dependency-injection-services"></a>Iniezione di dipendenze (servizi)

ASP.NET Core include un framework di inserimento delle dipendenze predefinito che rende disponibili i servizi configurati per le classi di un'app. Un modo per inserire un'istanza di un servizio in una classe consiste nel creare un costruttore con un parametro del tipo richiesto. Il parametro può essere il tipo di servizio o un'interfaccia. Il sistema di inserimento delle dipendenze fornisce il servizio in runtime.

::: moniker range=">= aspnetcore-2.0"

Di seguito viene proposto un esempio di classe che usa l'inserimento delle dipendenze per ottenere un oggetto di contesto Entity Framework Core. La riga evidenziata è un esempio di inserimento costruttore:

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

::: moniker-end

Nonostante l'inserimento delle dipendenze sia predefinito, è progettato per consentire il collegamento di un contenitore di inversione del controllo (IoC) di terze parti, qualora lo si preferisse.

Per altre informazioni, vedere [Dependency injection](xref:fundamentals/dependency-injection) (Inserimento delle dipendenze).

## <a name="middleware"></a>Middleware

La pipeline di gestione delle richieste è strutturata come una serie di componenti middleware. Ogni componente esegue operazioni asincrone su un `HttpContext`, quindi richiama il componente middleware successivo nella pipeline oppure termina la richiesta.

Per convenzione viene aggiunto un componente middleware alla pipeline richiamando il relativo metodo di estensione `Use...` nel metodo `Startup.Configure`. Per abilitare il rendering dei file statici, ad esempio, chiamare `UseStaticFiles`.

::: moniker range=">= aspnetcore-2.0"

Il codice evidenziato nell'esempio seguente configura la pipeline di gestione delle richieste:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

::: moniker-end

ASP.NET Core include un ampio set di middleware predefiniti, ma consente anche di scrivere middleware personalizzati.

Per altre informazioni, vedere [Middleware](xref:fundamentals/middleware/index).

<a id="host"/>

## <a name="the-host"></a>L'host

Un'app ASP.NET Core crea un *host* all'avvio. L'host è un oggetto che incapsula tutte le risorse dell'app, ad esempio:

* Un'implementazione del server HTTP
* I componenti middleware
* Registrazione
* Inserimento delle dipendenze
* Configurazione

Il motivo principale per cui tutte le risorse interdipendenti dell'app sono incluse in un unico oggetto è la gestione del ciclo di vita, vale a dire il controllo sull'avvio dell'app e sull'arresto normale.

Il codice usato per creare un host si trova in `Program.Main` e segue il pattern [Builder](https://wikipedia.org/wiki/Builder_pattern). Per configurare ogni risorsa che fa parte dell'host vengono chiamati dei metodi. Un metodo builder viene chiamato per combinare il tutto e creare un'istanza dell'oggetto host.

::: moniker range="<= aspnetcore-2.2"

ASP.NET Core 2.x usa un host Web (classe `WebHost`) per le app Web. Il framework offre i metodi di estensione `CreateDefaultBuilder` che consentono di configurare un host con le opzioni usate comunemente, ad esempio:

* Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.
* Caricare la configurazione da *appsettings.json*, variabili di ambiente, argomenti della riga di comando e altre origini.
* Inviare l'output di registrazione alla console e ai provider di debug.

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Ecco un esempio di codice per creare un host:

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

Per altre informazioni, vedere [Host Web](xref:fundamentals/host/web-host).

::: moniker-end

::: moniker range="> aspnetcore-2.2"

Nelle app Web in ASP.NET Core 3.0 è possibile usare un host Web (classe `WebHost`) o un host generico (classe `Host`). È consigliato l'uso di un host generico, mentre l'host Web è disponibile per garantire la compatibilità con le versioni precedenti.

Il framework offre i metodi di estensione `CreateDefaultBuilder` e `ConfigureWebHostDefaults` che consentono di configurare un host con le opzioni usate comunemente, ad esempio:

* Usare [Kestrel](#servers) come server Web e abilitare l'integrazione IIS.
* Caricare la configurazione da *appsettings.json*, *appsettings.[EnvironmentName].json*, variabili di ambiente e argomenti della riga di comando.
* Inviare l'output di registrazione alla console e ai provider di debug.

Ecco un esempio di codice per creare un host. I metodi di estensione che configurano l'host con le opzioni usate comunemente sono evidenziati.

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

Per altre informazioni, vedere [Host generico](xref:fundamentals/host/generic-host) e [Host Web](xref:fundamentals/host/web-host)

::: moniker-end

### <a name="advanced-host-scenarios"></a>Scenari host avanzati

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

L'host Web è progettato per includere un'implementazione del server HTTP che non è necessaria per altri tipi di app .NET. A partire dalla versione 2.1, l'host generico (classe `Host`) è disponibile per qualsiasi app .NET Core, non solo per le app ASP.NET Core. L'host generico consente di usare funzionalità trasversali quali la registrazione, l'inserimento delle dipendenze, la configurazione e la gestione del ciclo di vita delle app in altri tipi di app. Per altre informazioni, vedere [Host generico](xref:fundamentals/host/generic-host).

::: moniker-end

::: moniker range="> aspnetcore-2.2"

L'host generico è disponibile per qualsiasi app .NET Core, non solo per le app ASP.NET Core. L'host generico consente di usare funzionalità trasversali quali la registrazione, l'inserimento delle dipendenze, la configurazione e la gestione del ciclo di vita delle app in altri tipi di app. Per altre informazioni, vedere [Host generico](xref:fundamentals/host/generic-host).

::: moniker-end

È anche possibile usare l'host per eseguire attività in background. Per altre informazioni, vedere [Attività in background](xref:fundamentals/host/hosted-services).

## <a name="servers"></a>Server

Un'app ASP.NET Core usa un'implementazione del server HTTP per l'ascolto delle richieste HTTP. Il server espone le richieste all'app sotto forma di insieme di [funzionalità di richiesta](xref:fundamentals/request-features) strutturate in un `HttpContext`.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core include le implementazioni server seguenti:

* *Kestrel* è un server Web multipiattaforma. Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/). In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.
* *Server HTTP IIS* è un server per Windows che usa IIS. Con questo server l'app ASP.NET Core e IIS sono eseguiti nello stesso processo.
* *HTTP.sys* è un server per Windows che non viene usato con IIS.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*. In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet. Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*. In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet. Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core include le implementazioni server seguenti:

* *Kestrel* è un server Web multipiattaforma. Kestrel viene spesso eseguito in una configurazione con proxy inverso tramite [IIS](https://www.iis.net/). In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet.
* *HTTP.sys* è un server per Windows che non viene usato con IIS.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*. In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet. Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core fornisce l'implementazione del server multipiattaforma *Kestrel*. In ASP.NET Core 2.0 o versione successiva Kestrel può essere eseguito come server perimetrale pubblico esposto direttamente a Internet. Kestrel viene spesso eseguito in una configurazione con proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).

---

::: moniker-end

Per altre informazioni, vedere [Server](xref:fundamentals/servers/index).

## <a name="configuration"></a>Configurazione

ASP.NET Core offre un framework di configurazione che ottiene le impostazioni, ad esempio coppie nome-valore, da un set ordinato di provider di configurazione. Esistono provider di configurazione predefiniti per un'ampia gamma di origini, ad esempio file con estensione *JSON*, file con estensione *XML*, variabili di ambiente e argomenti della riga di comando. È anche possibile scrivere provider di configurazione personalizzati.

Si potrebbe specificare, ad esempio, che la configurazione proviene da *appsettings.json* e da variabili di ambiente. Quindi, quando viene richiesto il valore di *ConnectionString*, il framework esaminerebbe per primo il file *appsettings.json*. Se il valore venisse trovato qui ma anche in una variabile di ambiente, il valore della variabile di ambiente avrebbe la precedenza.

Per la gestione dei dati di configurazione riservati, ad esempio le password, ASP.NET Core offre uno [strumento Secret Manager](xref:security/app-secrets). Per i segreti di produzione, si consiglia di usare [Azure Key Vault](/aspnet/core/security/key-vault-configuration).

Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).

## <a name="options"></a>Opzioni

Ove possibile, ASP.NET Core segue il *modello di opzioni* per archiviare e recuperare i valori di configurazione. Il modello di opzioni usa le classi per rappresentare i gruppi di impostazioni correlate.

Il codice seguente, ad esempio, imposta le opzioni WebSockets:

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

Per altre informazioni, vedere [Opzioni](xref:fundamentals/configuration/options).

## <a name="environments"></a>Ambienti

Gli ambienti di esecuzione, ad esempio *Sviluppo*, *Staging* e *Produzione*, sono un concetto fondamentale in ASP.NET Core. È possibile specificare l'ambiente in cui viene eseguita un'app impostando la variabile di ambiente `ASPNETCORE_ENVIRONMENT`. ASP.NET Core legge tale variabile di ambiente all'avvio dell'app e ne archivia il valore in un'implementazione `IHostingEnvironment`. L'oggetto ambiente è disponibile in un punto qualsiasi dell'app tramite l'inserimento delle dipendenze.

::: moniker range=">= aspnetcore-2.0"

Il seguente codice di esempio della classe `Startup` configura l'app in modo che fornisca informazioni dettagliate sull'errore solo quando viene eseguita nell'ambiente di sviluppo:

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

::: moniker-end

Per altre informazioni, vedere [Ambienti](xref:fundamentals/environments).

## <a name="logging"></a>Registrazione

ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione predefiniti e di terze parti. Tra i provider disponibili sono inclusi i seguenti:

* Console
* Debug
* Event Tracing for Windows
* Registro eventi di Windows
* TraceSource
* Servizio app di Azure
* Azure Application Insights

Scrivere i log da un punto qualsiasi del codice di un'app recuperando un oggetto `ILogger` dall'inserimento delle dipendenze e chiamando i metodi di registrazione.

::: moniker range=">= aspnetcore-2.0"

Ecco un esempio di codice che usa un oggetto `ILogger` con inserimento costruttore e chiamate al metodo di registrazione evidenziati.

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

::: moniker-end

L'interfaccia `ILogger` consente di passare qualsiasi numero di campi al provider di registrazione. I campi vengono usati comunemente per costruire una stringa di messaggio, ma il provider può anche inviarli come campi separati a un archivio dati. Questa funzionalità consente ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Per altre informazioni, vedere [Registrazione](xref:fundamentals/logging/index).

## <a name="routing"></a>Routing

Una *route* è un modello URL di cui è stato eseguito il mapping su un gestore. Il gestore è in genere una pagina Razor, un metodo di azione in un controller MVC o un tipo di middleware. Il routing di ASP.NET Core consente di controllare gli URL usati dall'app.

Per altre informazioni, vedere [Routing](xref:fundamentals/routing).

## <a name="error-handling"></a>Gestione degli errori

ASP.NET Core dispone di funzionalità predefinite per la gestione degli errori, ad esempio:

* Una pagina delle eccezioni per gli sviluppatori
* Pagine degli errori personalizzate
* Pagine dei codici di stato statiche
* Gestione delle eccezioni durante l'avvio

Per altre informazioni, vedere [Gestione degli errori](xref:fundamentals/error-handling).

::: moniker range=">= aspnetcore-2.1"

## <a name="make-http-requests"></a>Creare richieste HTTP

Un'implementazione di `IHttpClientFactory` è disponibile per la creazione di istanze `HttpClient`. Il factory:

* Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche. Ad esempio, è possibile registrare e configurare un client *github* per accedere a GitHub. È possibile registrare un client predefinito per altri scopi.
* Supporta la registrazione e il concatenamento di più gestori di delega per creare una pipeline di middleware per le richieste in uscita. Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core. Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.
* Si integra con *Polly*, una famosa libreria di terze parti per la gestione degli errori temporanei.
* Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.
* Aggiunge un'esperienza di registrazione configurabile, tramite *ILogger*, per tutte le richieste inviate attraverso i client creati dalla factory.

Per altre informazioni, vedere [Creare richieste HTTP](xref:fundamentals/http-requests).

::: moniker-end

## <a name="content-root"></a>Radice del contenuto

La radice del contenuto è il percorso di base per i contenuti privati usati dall'app, ad esempio i relativi file Razor. Per impostazione predefinita, la radice del contenuto è il percorso di base per il file eseguibile che ospita l'app. È possibile specificare un percorso alternativo in fase di [creazione dell'host](#host).

::: moniker range="<= aspnetcore-2.2"

Per altre informazioni, vedere [Radice del contenuto](xref:fundamentals/host/web-host#content-root).

::: moniker-end

::: moniker range="> aspnetcore-2.2"

Per altre informazioni, vedere [Radice del contenuto](xref:fundamentals/host/generic-host#content-root).

::: moniker-end

## <a name="web-root"></a>Radice Web

La radice Web, nota anche come *webroot*, è il percorso di base per le risorse statiche pubbliche, ad esempio CSS, JavaScript e i file di immagine. Per impostazione predefinita, il middleware dei file statici verrà usato solo dai file della radice Web e dalle relative sottodirectory. Il percorso della radice Web è, per impostazione predefinita, *\<radice del contenuto >/wwwroot*, ma è possibile impostare un percorso diverso in fase di [creazione dell'host](#host).

Per i file Razor (file con estensione  *CSHTML*), il carattere tilde-barra `~/` punta alla radice Web. I percorsi che iniziano con `~/` sono indicati come percorsi virtuali.

Per altre informazioni, vedere [File statici](xref:fundamentals/static-files).
