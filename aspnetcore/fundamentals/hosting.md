---
title: Hosting in ASP.NET Core | Documenti Microsoft
author: ardalis
description: Introduzione agli host web in ASP.NET Core.
keywords: ASP.NET Core, host web, IWebHost
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a>Introduzione all'hosting in ASP.NET Core

Da [Steve Smith](http://ardalis.com)

Per eseguire un'applicazione ASP.NET di base, è necessario configurare e avviare un host utilizzando `WebHostBuilder`.

## <a name="what-is-a-host"></a>Che cos'è un Host?

Le applicazioni ASP.NET Core richiedono un *host* in cui si desidera eseguire. Un host deve implementare il `IWebHost` interfaccia, che espone le raccolte di funzionalità e i servizi, e un `Start` metodo. L'host è in genere creato utilizzando un'istanza di un `WebHostBuilder`, che crea e restituisce un `WebHost` istanza. Il `WebHost` fa riferimento al server che gestirà le richieste. Altre informazioni, vedere [server](servers/index.md).

### <a name="what-is-the-difference-between-a-host-and-a-server"></a>Che cos'è la differenza tra un host e un server?

L'host è responsabile della gestione di avvio e la durata dell'applicazione. Il server è responsabile per l'accettazione di richieste HTTP. Parte di responsabilità dell'host include la verifica che i servizi dell'applicazione e il server è disponibile e configurato correttamente. È possibile considerare l'host come un wrapper per il server. L'host è configurato per utilizzare un server specifico. il server non è a conoscenza del relativo host.

## <a name="setting-up-a-host"></a>Impostazione di un Host

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Creare un host utilizzando un'istanza di `WebHostBuilder`. Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione: `public static void Main` (che nei modelli di progetto si trova un *Program.cs* file). Una tipica *Program.cs*, come illustrato di seguito viene illustrato come utilizzare un `WebHostBuilder` per compilare un host.

[!code-csharp[Principale](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]

Il `WebHostBuilder` è responsabile della creazione dell'host che verrà bootstrap del server per l'app. `WebHostBuilder`è necessario fornire un server che implementa `IServer` (`UseKestrel` nel codice precedente). `UseKestrel`Specifica che il server Kestrel verrà usato dall'app.

Il server *contenuto radice* determina in cui cercare i file di contenuto, ad esempio i file di visualizzazione MVC. La radice di contenuto predefinito è la cartella da cui viene eseguita l'applicazione.

> [!NOTE]
> Specifica di `Directory.GetCurrentDirectory` come radice del contenuto utilizzeranno cartella radice del progetto web come radice del contenuto dell'app quando l'applicazione viene avviata da questa cartella (ad esempio, la chiamata `dotnet run` dalla cartella del progetto web). Questa è l'impostazione predefinita utilizzata in Visual Studio e `dotnet new` modelli.

Per utilizzare IIS come un proxy inverso, chiamare `UseIISIntegration` come parte della creazione dell'host. 

Si noti che `UseIISIntegration` non configurare un *server*, ad esempio `UseKestrel` does. Per utilizzare IIS con ASP.NET Core, è necessario specificare sia `UseKestrel` e `UseIISIntegration`. `UseKestrel`Crea il server web e ospita l'app. `UseIISIntegration`esamina le variabili di ambiente utilizzate da IIS/IIS Express e configura le impostazioni, ad esempio la porta per l'ascolto e le intestazioni da usare.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Creare un host utilizzando un'istanza di `WebHostBuilder`. Questa operazione viene in genere eseguita nel punto di ingresso dell'applicazione: `public static void Main` (che nei modelli di progetto si trova un *Program.cs* file). Una tipica *Program.cs*, come illustrato di seguito, le chiamate `CreateDefaultbuilder` per compilare un host:

[!code-csharp[Principale](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

`CreateDefaultbuilder`Crea un'istanza di `WebHostBuilder` per compilare l'host che avvia il server per l'app. L'host richiede un [server che implementa IServer](servers/index.md). Server incorporati sono [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` utilizzare Kestrel per impostazione predefinita.

`CreateDefaultbuilder`esegue attività di configurazione oltre alla configurazione Kestrel come server web:

* Imposta la radice del contenuto `Directory.GetCurrentDirectory`.
* Configurazione di caricamento da:
  * *appSettings. JSON*
  * *appSettings. \<EnvironmentName > JSON*.
  * informazioni riservate dell'utente quando viene eseguita l'app nell'ambiente di sviluppo
  * variabili di ambiente
  * argomenti della riga di comando fornito
* Configura la registrazione per l'output di console ed eseguire il debug, con le regole specificate in una sezione di configurazione della registrazione di filtro.
* Consente l'integrazione di IIS.
* Aggiunge la pagina di eccezione sviluppatore quando viene eseguita l'app nell'ambiente di sviluppo.

Il server *contenuto radice* determina in cui cercare i file di contenuto, ad esempio i file di visualizzazione MVC. La radice di contenuto predefinito è la cartella da cui viene eseguita l'applicazione.

> [!NOTE]
> Specifica di `Directory.GetCurrentDirectory` come radice del contenuto utilizzeranno cartella radice del progetto web come radice del contenuto dell'app quando l'applicazione viene avviata da questa cartella (ad esempio, la chiamata `dotnet run` dalla cartella del progetto web). Questa è l'impostazione predefinita utilizzata in Visual Studio e `dotnet new` modelli.

Quando si utilizza IIS come un proxy inverso, ASP.NET Core chiama automaticamente `UseIISIntegration` come parte della creazione dell'host. Per ulteriori informazioni, vedere [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module).

Si noti che `UseIISIntegration` non configurare un *server*, ad esempio `UseKestrel` does. `UseKestrel`Crea il server web e ospita l'app. `UseIISIntegration`esamina le variabili di ambiente utilizzate da IIS/IIS Express e configura le impostazioni, ad esempio la porta per l'ascolto e le intestazioni da usare.

---

Un'implementazione minima di configurazione di un host (e un'applicazione ASP.NET Core) include solo un server e una configurazione della pipeline di richieste dell'applicazione:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> Quando si configura un host, è possibile fornire `Configure` e `ConfigureServices` metodi, anziché o oltre a specificare un `Startup` classe (che deve anche definire questi metodi, vedere [avvio dell'applicazione](startup.md)). Più chiamate al metodo `ConfigureServices` aggiungerà uno a altro; le chiamate a `Configure` o `UseStartup` sostituiranno le impostazioni precedenti.

## <a name="configuring-a-host"></a>Configurazione di un Host

Il `WebHostBuilder` fornisce metodi per impostare la maggior parte dei valori di configurazione disponibili per l'host, che può essere impostata anche utilizzando direttamente `UseSetting` e la chiave associata.

### <a name="host-configuration-values"></a>Valori di configurazione di host

**Acquisire gli errori di avvio**`bool`

Chiave: `captureStartupErrors`. Il valore predefinito è `false`. Quando `false`, errori durante il risultato di avvio nell'host di chiusura. Quando `true`, l'host consente di acquisire tutte le eccezioni dalla `Startup` classe e si tenta di avviare il server. Verrà visualizzata una pagina di errore (generico o di dettaglio, in base all'impostazione, gli errori dettagliati seguito) per ogni richiesta. Impostare l'utilizzo di `CaptureStartupErrors` metodo.

Nota: Quando l'app viene eseguita con Kestrel e IIS, il comportamento predefinito è per acquisire gli errori di avvio. 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

**Contenuto radice**`string`

Chiave: `contentRoot`. Valore predefinito è la cartella in cui si trova l'assembly dell'applicazione (per Kestrel; IIS utilizzerà la radice del progetto web per impostazione predefinita). Questa impostazione determina in ASP.NET Core verrà avviata la ricerca di file di contenuto, ad esempio le visualizzazioni MVC. Utilizzato anche come percorso di base per il [impostazione radice Web](#web-root-setting). Impostare l'utilizzo di `UseContentRoot` metodo. Percorso deve essere presente o non sarà possibile avviare host.

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

**Errori dettagliati**`bool`

Chiave: `detailedErrors`. Il valore predefinito è `false`. Quando `true` (o quando cui ambiente è impostato su "Development"), l'app sarà visualizzati i dettagli delle eccezioni di avvio, anziché una pagina di errore generico. Impostare utilizzando `UseSetting`.

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

Quando gli errori dettagliati è impostato su `false` e acquisire gli errori di avvio è `true`, viene visualizzata una pagina di errore generico in risposta a ogni richiesta al server.

![Pagina di errore generico](hosting/_static/generic-error-page.png)

Quando gli errori dettagliati è impostato su `true` e acquisire gli errori di avvio è `true`, viene visualizzata una pagina di errore dettagliati in risposta a ogni richiesta al server.

![Pagina di errore dettagliato](hosting/_static/detailed-error-page.png)

**Ambiente**`string`

Chiave: `environment`. Il valore predefinito "Produzione". Può essere impostata su qualsiasi valore. I valori definiti dal framework includono "Sviluppo", "Gestione temporanea" e "Produzione". Valori non fanno distinzione tra maiuscole e minuscole. Vedere [utilizzo di più ambienti](environments.md). Impostare l'utilizzo di `UseEnvironment` metodo.

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> Per impostazione predefinita, l'ambiente viene letto dal `ASPNETCORE_ENVIRONMENT` variabile di ambiente. Quando si utilizza Visual Studio, le variabili di ambiente possono essere impostate *launchSettings.json* file.

<a id="server-urls"></a>

**Gli URL del server**`string`

Chiave: `urls`. Impostare un punto e virgola (;) separati prefissi di elenco di URL per cui il server deve rispondere. Ad esempio `http://localhost:123`. Il nome di dominio o l'host può essere sostituito con "\*" per indicare il server deve restare in attesa delle richieste su qualsiasi indirizzo IP o host utilizzando la porta specificata e il protocollo (ad esempio, `http://*:5000` o `https://*:5001`). Il protocollo (`http://` o `https://`) deve essere incluso in ogni URL. I prefissi sono interpretati dal server configurato. i formati supportati variano tra i server.

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

In ASP.NET 2.0 Core Kestrel ha la propria configurazione di endpoint API e non supporta `https://` nel `urls` stringa. Per ulteriori informazioni, vedere [Introduzione a Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

**Assembly di avvio**`string`

Chiave: `startupAssembly`. Determina l'assembly per la ricerca di `Startup` classe. Impostare l'utilizzo di `UseStartup` metodo. Può invece fare riferimento a un tipo specifico usando `WebHostBuilder.UseStartup<StartupType>`. Se più `UseStartup` metodi vengono chiamati, quella più recente ha la precedenza.

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

**Web radice**`string`

Chiave: `webroot`. Se non è specificato il valore predefinito è `(Content Root Path)\wwwroot`, se presente. Se questo percorso non esiste, viene utilizzato un provider di file viene eseguita alcuna operazione. Impostare utilizzando `UseWebRoot`.

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a>Si esegue l'override di configurazione

Utilizzare [configurazione](configuration.md) per impostare i valori di configurazione da utilizzare dall'host. Questi valori possono essere successivamente sottoposto a override. Viene specificato utilizzando `UseConfiguration`.

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

Nell'esempio precedente, possono essere passati gli argomenti della riga di comando per configurare l'host o, facoltativamente, è possibile specificare le impostazioni di configurazione in un *hosting.json* file. Per specificare l'host di eseguire in un URL specifico, è possibile passare il valore desiderato da un prompt dei comandi:

```console
dotnet run --urls "http://*:5000"
```

Il `Run` metodo avvia l'app web e blocca il thread chiamante finché l'host viene arrestato.

```csharp
host.Run();
```

È possibile eseguire l'host in modo non bloccante chiamando il relativo `Start` metodo:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Passare un elenco di URL per il `Start` (metodo) e sarà in ascolto l'URL specificato:

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

I formati di URL validi qui dipendono dal server in uso. Per ulteriori informazioni, vedere [gli URL del Server](#server-urls) più indietro in questo articolo.

> [!NOTE]
> Il `UseConfiguration` non è attualmente in grado di analizzare una sezione di configurazione restituita dal metodo di estensione `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`. Il `GetSection` metodo filtra le chiavi di configurazione per la sezione richiesta ma non il nome della sezione alle chiavi (ad esempio, `section:urls`, `section:environment`). Il `UseConfiguration` metodo prevede che le chiavi in modo che corrisponda il `WebHostBuilder` chiavi (ad esempio, `urls`, `environment`). La presenza del nome della sezione alle chiavi impedisce i valori della sezione di configurazione dell'host. Questo problema verrà risolto in una delle prossime versioni. Per ulteriori informazioni e soluzioni alternative, vedere [passando una sezione di configurazione in WebHostBuilder.UseConfiguration utilizza chiavi complete](https://github.com/aspnet/Hosting/issues/839).

### <a name="ordering-importance"></a>Ordine di priorità

`WebHostBuilder`le impostazioni vengono lette prima da alcune variabili di ambiente, se impostata. Queste variabili di ambiente è necessario utilizzare il formato `ASPNETCORE_{configurationKey}`, ad esempio per impostare gli URL di server resterà in ascolto su per impostazione predefinita, è necessario impostare `ASPNETCORE_URLS`.

È possibile sostituire qualsiasi di questi valori di variabili di ambiente specificando una configurazione (con `UseConfiguration`) o impostando il valore in modo esplicito (mediante `UseUrls` per l'istanza). L'host utilizzerà indipendentemente dall'opzione imposta il valore dell'ultima. Per questo motivo, `UseIISIntegration` devono essere visualizzate dopo `UseUrls`, perché l'URL che sostituisce con una condizione in modo dinamico da IIS. Se si desidera impostare l'URL predefinito a un valore a livello di codice, ma consentono di eseguire l'override con la configurazione, è possibile configurare l'host come segue:

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Pubblicare in Windows tramite IIS](../publishing/iis.md)
* [Pubblicare in Linux con Nginx](../publishing/linuxproduction.md)
* [Pubblicare in Linux con Apache](../publishing/apache-proxy.md)
* [Host in un servizio Windows](xref:hosting/windows-service)

