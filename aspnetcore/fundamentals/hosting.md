---
title: Hosting in ASP.NET Core
author: guardrex
description: "Informazioni sull'host web in ASP.NET Core, che è responsabile per la gestione di app di avvio e durata."
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 7f6712073002b73ca4ddd7586718c81e62cacbc2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="hosting-in-aspnet-core"></a>Hosting in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Le applicazioni ASP.NET Core configurare e avviare un *host*. L'host è responsabile della gestione di avvio e la durata delle app. Come minimo, l'host consente di configurare un server e una pipeline di elaborazione della richiesta.

## <a name="setting-up-a-host"></a>Impostazione di un host

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo. Nei modelli di progetto, `Main` si trova in *Program.cs*. Una tipica *Program.cs* chiamate [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per iniziare a configurare un host:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder`esegue le attività seguenti:

* Configura [Kestrel](servers/kestrel.md) come server web. Per le opzioni predefinite Kestrel, vedere [il Kestrel Opzioni sezione di implementazione del server web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).
* Imposta la radice del contenuto per il percorso restituito da [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Configurazione facoltativa di carichi da:
  * *appsettings.json*.
  * *appsettings.{Environment}.json*.
  * [Informazioni riservate dell'utente](xref:security/app-secrets) quando viene eseguita l'app `Development` ambiente.
  * Variabili di ambiente.
  * Argomenti della riga di comando.
* Configura [registrazione](xref:fundamentals/logging/index) per l'output di console ed eseguire il debug. La registrazione include [filtri log](xref:fundamentals/logging/index#log-filtering) le regole specificate in una sezione di configurazione di registrazione di un *appSettings. JSON* o *appsettings. { Ambiente} JSON* file.
* Quando si esegue dietro IIS, abilita [integrazione IIS](xref:host-and-deploy/iis/index). Consente di configurare il percorso di base e la porta di ascolto il server quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module). Il modulo Crea un proxy inverso tra IIS e Kestrel. Configura inoltre all'app di [acquisire gli errori di avvio](#capture-startup-errors). Per le opzioni predefinite IIS, vedere [IIS Opzioni sezione dell'Host ASP.NET Core in Windows con IIS](xref:host-and-deploy/iis/index#iis-options).

Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC. Quando l'app viene avviata dalla cartella radice del progetto, cartella radice del progetto viene utilizzato come radice del contenuto. Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).

Per ulteriori informazioni sulla configurazione delle app, vedere [configurazione in ASP.NET Core](xref:fundamentals/configuration/index).

> [!NOTE]
> Come alternativa all'utilizzo statica `CreateDefaultBuilder` (metodo), creazione di un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) è un approccio supportato con ASP.NET Core 2. x. Per ulteriori informazioni, vedere la scheda di 1. x di ASP.NET Core.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Creare un host utilizzando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Creazione di un host è in genere eseguita nel punto di ingresso dell'applicazione, il `Main` metodo. Nei modelli di progetto, `Main` si trova in *Program.cs*:

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder`richiede un [server che implementa IServer](servers/index.md). Server incorporati sono [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (prima del rilascio di base di ASP.NET 2.0, è stato chiamato HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)). In questo esempio, il [il metodo di estensione UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifica il server Kestrel.

Il *contenuto radice* determina in cui l'host esegue ricerche nei file di contenuto, ad esempio i file di visualizzazione MVC. La radice di contenuto predefinito viene ottenuta per `UseContentRoot` da [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Quando l'app viene avviata dalla cartella radice del progetto, cartella radice del progetto viene utilizzato come radice del contenuto. Questo è il valore predefinito utilizzato [Visual Studio](https://www.visualstudio.com/) e [dotnet nuovi modelli](/dotnet/core/tools/dotnet-new).

Per utilizzare IIS come un proxy inverso, chiamare [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) come parte della creazione dell'host. `UseIISIntegration`non configurare un *server*, ad esempio [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does. `UseIISIntegration`Consente di configurare il percorso di base e la porta di ascolto il server quando si utilizza il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) per creare un proxy inverso tra Kestrel e IIS. Per utilizzare IIS con ASP.NET Core, `UseKestrel` e `UseIISIntegration` deve essere specificato. `UseIISIntegration`attiva solo quando si esegue IIS o IIS Express. Per ulteriori informazioni, vedere [Introduzione a ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) e [riferimento di configurazione di ASP.NET Core modulo](xref:host-and-deploy/aspnet-core-module).

Un'implementazione minima che consente di configurare un host (e un'applicazione ASP.NET Core) prevede la specificazione di un server e la configurazione della pipeline di richieste dell'applicazione:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

Quando si configura un host, [configura](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metodi possono essere forniti. Se un `Startup` classe è specificata, deve definire un `Configure` metodo. Per ulteriori informazioni, vedere [avvio dell'applicazione in ASP.NET Core](startup.md). Più chiamate a `ConfigureServices` aggiungere uno a altro. Più chiamate al metodo `Configure` o `UseStartup` sul `WebHostBuilder` sostituire le impostazioni precedenti.

## <a name="host-configuration-values"></a>Valori di configurazione di host

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) si basa su approcci seguenti per impostare l'host di valori di configurazione:

* Configurazione di generatore di host, che include variabili di ambiente con il formato `ASPNETCORE_{configurationKey}`. Ad esempio `ASPNETCORE_URLS`.
* Metodi espliciti, ad esempio `CaptureStartupErrors`.
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e la chiave associata. Quando si imposta un valore con `UseSetting`, il valore è impostato come stringa, indipendentemente dal tipo.

L'host usa indipendentemente dall'opzione imposta un valore dell'ultima. Per ulteriori informazioni, vedere [configurazione verrà ignorato](#overriding-configuration) nella sezione successiva.

### <a name="capture-startup-errors"></a>Acquisire gli errori di avvio

Questa impostazione Controlla l'acquisizione degli errori di avvio.

**Chiave**: captureStartupErrors  
**Tipo**: *bool* (`true` o `1`)  
**Predefinito**: per impostazione predefinita `false` a meno che non viene eseguita l'app con Kestrel dietro IIS, in cui il valore predefinito è `true`.  
**Impostare utilizzando**:`CaptureStartupErrors`  
**Variabile di ambiente**:`ASPNETCORE_CAPTURESTARTUPERRORS`

Quando `false`, errori durante il risultato di avvio nell'host di chiusura. Quando `true`, l'host consente di acquisire le eccezioni durante l'avvio e tenta di avviare il server.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>Radice del contenuto

Questa impostazione determina in cui inizia la ricerca di file di contenuto, ad esempio le visualizzazioni MVC ASP.NET Core. 

**Chiave**: contentRoot  
**Tipo**: *stringa*  
**Predefinito**: per impostazione predefinita la cartella in cui si trova l'assembly di app.  
**Impostare utilizzando**:`UseContentRoot`  
**Variabile di ambiente**:`ASPNETCORE_CONTENTROOT`

La radice del contenuto viene usata anche come percorso di base per il [impostazione radice Web](#web-root). Se il percorso non esiste, l'host non verrà avviato.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>Errori dettagliati

Determina se gli errori dettagliati devono essere acquisiti gli errori.

**Chiave**: detailedErrors  
**Tipo**: *bool* (`true` o `1`)  
**Predefinito**: false  
**Impostare utilizzando**:`UseSetting`  
**Variabile di ambiente**:`ASPNETCORE_DETAILEDERRORS`

Quando è abilitata (o quando il <a href="#environment">ambiente</a> è impostato su `Development`), l'app consente di acquisire le eccezioni dettagliate.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>Ambiente

Imposta l'ambiente dell'app.

**Chiave**: ambiente  
**Tipo**: *stringa*  
**Predefinito**: produzione  
**Impostare utilizzando**:`UseEnvironment`  
**Variabile di ambiente**:`ASPNETCORE_ENVIRONMENT`

Qualsiasi valore, è possibile impostare l'ambiente. I valori definiti dal framework includono `Development`, `Staging`, e `Production`. I valori non sono tra maiuscole e minuscole. Per impostazione predefinita, il *ambiente* viene letto dal `ASPNETCORE_ENVIRONMENT` variabile di ambiente. Quando si utilizza [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate *launchSettings.json* file. Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>Hosting di assembly di avvio

Imposta gli assembly di avvio hosting dell'applicazione.

**Chiave**: hostingStartupAssemblies  
**Tipo**: *stringa*  
**Predefinito**: una stringa vuota  
**Impostare utilizzando**:`UseSetting`  
**Variabile di ambiente**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

Stringa delimitata da punto e virgola di assembly di avvio da caricare all'avvio di hosting. Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.

Anche se il valore di configurazione predefinito è una stringa vuota, gli assembly di avvio hosting sempre includono assembly dell'applicazione. Se vengono specificati gli assembly di avvio hosting, vengono aggiunte all'assembly dell'applicazione per il caricamento quando l'app compila i servizi comuni durante l'avvio.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Questa funzionalità è disponibile in ASP.NET Core 1. x.

---

### <a name="prefer-hosting-urls"></a>Preferiscono gli URL di Hosting

Indica se l'host deve eseguire l'ascolto gli URL configurati con il `WebHostBuilder` invece di quelle configurate con la `IServer` implementazione.

**Chiave**: preferHostingUrls  
**Tipo**: *bool* (`true` o `1`)  
**Predefinito**: true  
**Impostare utilizzando**:`PreferHostingUrls`  
**Variabile di ambiente**:`ASPNETCORE_PREFERHOSTINGURLS`

Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Questa funzionalità è disponibile in ASP.NET Core 1. x.

---

### <a name="prevent-hosting-startup"></a>Impedire l'avvio di Hosting

Impedisce il caricamento automatico di hosting agli assembly di avvio, inclusi gli assembly di avvio configurati dall'assembly dell'applicazione di hosting. Vedere [aggiungere le funzionalità dell'app da un assembly esterno utilizzando IHostingStartup](xref:host-and-deploy/ihostingstartup) per ulteriori informazioni.

**Chiave**: preventHostingStartup  
**Tipo**: *bool* (`true` o `1`)  
**Predefinito**: false  
**Impostare utilizzando**:`UseSetting`  
**Variabile di ambiente**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`

Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Questa funzionalità è disponibile in ASP.NET Core 1. x.

---

### <a name="server-urls"></a>URL del server

Indica il indirizzi IP o gli indirizzi host con le porte e protocolli che il server deve rimanere in attesa per le richieste.

**Chiave**: URL  
**Tipo**: *stringa*  
**Predefinito**: indirizzo http://localhost:5000/  
**Impostare utilizzando**:`UseUrls`  
**Variabile di ambiente**:`ASPNETCORE_URLS`

Impostare in separati da punto e virgola (;) prefissi di elenco di URL per cui il server deve rispondere. Ad esempio `http://localhost:123`. Utilizzare "\*" per indicare che il server deve attendere le richieste su qualsiasi indirizzo IP o nome host utilizzando la porta specificata e il protocollo (ad esempio, `http://*:5000`). Il protocollo (`http://` o `https://`) deve essere incluso in ogni URL. I formati supportati variano tra i server.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel ha la propria configurazione di endpoint API. Per altre informazioni, vedere l'introduzione all'[implementazione del server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>Timeout di chiusura

Specifica la quantità di tempo di attesa per l'host web per l'arresto.

**Chiave**: shutdownTimeoutSeconds  
**Tipo**: *int*  
**Predefinito**: 5  
**Impostare utilizzando**:`UseShutdownTimeout`  
**Variabile di ambiente**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

Anche se la chiave accetta un *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il `UseShutdownTimeout` il metodo di estensione accetta una `TimeSpan`. Questa funzionalità è stato introdotta in ASP.NET 2.0 Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Questa funzionalità è disponibile in ASP.NET Core 1. x.

---

### <a name="startup-assembly"></a>Assembly di avvio

Determina l'assembly per la ricerca di `Startup` classe.

**Chiave**: startupAssembly  
**Tipo**: *stringa*  
**Predefinito**: assembly dell'applicazione  
**Impostare utilizzando**:`UseStartup`  
**Variabile di ambiente**:`ASPNETCORE_STARTUPASSEMBLY`

L'assembly in base al nome (`string`) o un tipo (`TStartup`) è possibile fare riferimento. Se più `UseStartup` metodi vengono chiamati, quella più recente ha la precedenza.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a>Radice Web

Imposta il percorso relativo alle risorse statiche dell'app.

**Chiave**: webroot  
**Tipo**: *stringa*  
**Predefinito**: se non specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esista. Se il percorso non esiste, viene utilizzato un provider di file viene eseguita alcuna operazione.  
**Impostare utilizzando**:`UseWebRoot`  
**Variabile di ambiente**:`ASPNETCORE_WEBROOT`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>Si esegue l'override di configurazione

Utilizzare [configurazione](xref:fundamentals/configuration/index) per configurare l'host. Nell'esempio seguente, configurazione dell'host è specificato, facoltativamente, un *hosting.json* file. Qualsiasi configurazione caricata dal *hosting.json* file può essere sottoposto a override dagli argomenti della riga di comando. La configurazione generata (in `config`) viene usato per configurare l'host con `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Si esegue l'override della configurazione fornita dal `UseUrls` con *hosting.json* configurazione primo argomento della riga di comando config secondo:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> Il [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita dal metodo di estensione `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`. Il `GetSection` metodo filtra le chiavi di configurazione per la sezione richiesta ma non il nome della sezione alle chiavi (ad esempio, `section:urls`, `section:environment`). Il `UseConfiguration` metodo prevede che le chiavi in modo che corrisponda il `WebHostBuilder` chiavi (ad esempio, `urls`, `environment`). La presenza del nome della sezione alle chiavi impedisce i valori della sezione di configurazione dell'host. Questo problema verrà risolto in una delle prossime versioni. Per ulteriori informazioni e soluzioni alternative, vedere [passando una sezione di configurazione in WebHostBuilder.UseConfiguration utilizza chiavi complete](https://github.com/aspnet/Hosting/issues/839).

Per specificare l'host di eseguire in un URL specifico, il valore desiderato può essere passato da un prompt dei comandi durante l'esecuzione di `dotnet run`. L'argomento della riga di comando esegue l'override di `urls` valore il *hosting.json* file e il server è in ascolto sulla porta 8080:

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a>Avvio dell'host

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**Run**

Il `Run` metodo avvia l'app web e blocca il thread chiamante fino alla chiusura:

```csharp
host.Run();
```

**Start**

Eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Se viene passato un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

L'app può inizializzare e avviare un nuovo host utilizzando i valori predefiniti configurati in precedenza di `CreateDefaultBuilder` utilizzando un metodo statico. Questi metodi di avviano il server con e senza l'output di console [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) attendere un'interruzione (Ctrl-C/SIGINT o SIGTERM):

**Start (RequestDelegate app)**

Iniziare con un `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!" `WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM). L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.

**Start (url di stringa, RequestDelegate app)**

Iniziare con un URL e `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produce lo stesso risultato **inizio (RequestDelegate app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.

**Start (azione<IRouteBuilder> routeBuilder)**

Utilizzare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) da utilizzare il middleware di routing:

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Utilizzare le seguenti richieste del browser con l'esempio:

| Richiesta                                    | Risposta                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | Genera un'eccezione con stringa "ooops!" |
| `http://localhost:5000/throw`              | Genera un'eccezione con stringa "Uh oh!" |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Hello World!                             |

`WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM). L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.

**Start (string url, azione<IRouteBuilder> routeBuilder)**

Utilizzare un URL e un'istanza di `IRouteBuilder`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produce lo stesso risultato **Start (azione<IRouteBuilder> routeBuilder)**, ad eccezione dell'applicazione risponde al `http://localhost:8080`.

**StartWith (azione<IApplicationBuilder> app)**

Fornire un delegato per configurare un `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Effettuare una richiesta nel browser per `http://localhost:5000` per ricevere la risposta "Hello World!" `WaitForShutdown`si blocca fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM). L'app viene visualizzata la `Console.WriteLine` messaggio e attende un keypress uscire dall'installazione.

**StartWith (string url, azione<IApplicationBuilder> app)**

Fornire un URL e un delegato per configurare un `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produce lo stesso risultato **StartWith (azione<IApplicationBuilder> app)**, ad eccezione dell'applicazione risponde alla `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**Run**

Il `Run` metodo avvia l'app web e blocca il thread chiamante finché non viene chiuso l'host:

```csharp
host.Run();
```

**Start**

Eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Se viene passato un elenco di URL per il `Start` (metodo), è in ascolto l'URL specificato:


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a>Interfaccia IHostingEnvironment

Il [IHostingEnvironment interfaccia](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting web dell'app. Utilizzare [inserimento costruttore](xref:fundamentals/dependency-injection) per ottenere il `IHostingEnvironment` per utilizzare le proprietà e metodi di estensione:

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

Oggetto [approccio basato sulle convenzioni](xref:fundamentals/environments#startup-conventions) può essere usato per configurare l'app all'avvio in base all'ambiente. In alternativa, inserire il `IHostingEnvironment` nel `Startup` costruttore per l'utilizzo in `ConfigureServices`:

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> Oltre al `IsDevelopment` metodo di estensione, `IHostingEnvironment` offre `IsStaging`, `IsProduction`, e `IsEnvironment(string environmentName)` metodi. Vedere [utilizzo di più ambienti](xref:fundamentals/environments) per informazioni dettagliate.

Il `IHostingEnvironment` servizio può anche essere inserito direttamente nella `Configure` metodo per la configurazione di pipeline di elaborazione:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

`IHostingEnvironment`possono essere inserite nella `Invoke` metodo durante la creazione personalizzata [middleware](xref:fundamentals/middleware#writing-middleware):

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a>Interfaccia IApplicationLifetime

[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente attività di post-avvio e arresto. Tre proprietà nell'interfaccia vengono utilizzati per registrare i token di annullamento `Action` metodi che definiscono gli eventi di avvio e arresto. È inoltre disponibile un `StopApplication` metodo.

| Token di annullamento    | Generato quando &#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | L'host sia stato avviato. |
| `ApplicationStopping` | L'host è in esecuzione un arresto normale. Stia ancora elaborando le richieste. Blocchi di arresto fino al completamento di questo evento. |
| `ApplicationStopped`  | L'host di completamento in corso un arresto normale. Tutte le richieste devono essere elaborate. Blocchi di arresto fino al completamento di questo evento. |

| Metodo            | Operazione                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | Chiusura di richieste dell'applicazione corrente. |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a>Risoluzione dei problemi relativi a System. ArgumentException

**Si applica a ASP.NET Core 2.0**

Un host può essere compilato inserendo `IStartup` direttamente nel contenitore dell'inserimento di dipendenza anziché chiamare `UseStartup` o `Configure`:

```csharp
services.AddSingleton<IStartup, Startup>();
```

Se l'host viene compilato in questo modo, potrebbe verificarsi l'errore seguente:

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

Questo errore si verifica perché il [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (l'assembly corrente) è necessario cercare `HostingStartupAttributes`. Se l'app inserisce manualmente `IStartup` nel contenitore dell'inserimento di dipendenza, aggiungere la chiamata seguente a `WebHostBuilder` con il nome di assembly specificato:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

In alternativa, aggiungere un dummy `Configure` per il `WebHostBuilder`, che imposta il `applicationName`(`ApplicationKey`) automaticamente:

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**Nota**: questo è solo necessario con la versione di ASP.NET 2.0 Core e solo quando l'applicazione non chiama `UseStartup` o `Configure`.

Per ulteriori informazioni, vedere [annunci: Microsoft.Extensions.PlatformAbstractions è stato rimosso (commento)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e [StartupInjection esempio](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Host in Windows con IIS](xref:host-and-deploy/iis/index)
* [Host in Linux con Nginx](xref:host-and-deploy/linux-nginx)
* [Host in Linux con Apache](xref:host-and-deploy/linux-apache)
* [Host in un servizio Windows](xref:host-and-deploy/windows-service)
