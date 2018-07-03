---
title: Host generico .NET
author: guardrex
description: Informazioni sull'host generico in .NET, che gestisce l'avvio e la durata delle app.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 33e5829ce4a09e132743b4174a588cf232a44775
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276260"
---
# <a name="net-generic-host"></a>Host generico .NET

Di [Luke Latham](https://github.com/guardrex)

Le app .NET configurano e avviano un *host*. L'host è responsabile della gestione dell'avvio e della durata delle app. Questo argomento illustra l'host generico di ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), utile per l'hosting di app che non elaborano le richieste HTTP. Per informazioni sull'host Web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), vedere l'argomento [Host Web](xref:fundamentals/host/web-host).

L'obiettivo dell'host generico è separare la pipeline HTTP dall'API dell'host Web, per abilitare una gamma più ampia di scenari host. La messaggistica, le attività in background e altri carichi di lavoro non HTTP basati sull'host generico traggono vantaggio dalle funzionalità a montaggio incrociato come la configurazione, l'inserimento di dipendenze (DI) e la registrazione.

L'host generico è una nuova funzionalità introdotta in ASP.NET Core 2.1 e non è adatto agli scenari di hosting Web. Per gli scenari di hosting Web usare l'[host Web](xref:fundamentals/host/web-host). L'host generico è in fase di sviluppo e sostituirà l'host Web in una versione futura, funzionando come API host principale negli scenari sia HTTP che non HTTP.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

Quando si esegue l'app di esempio in [Visual Studio Code](https://code.visualstudio.com/), usare un *terminale esterno o integrato*. Non eseguire l'esempio in un ambiente `internalConsole`.

Per impostare la console in Visual Studio Code:

1. Aprire il file *.vscode/launch.json*.
1. Nella configurazione **.NET Core Launch (console)** trovare la voce **console**. Impostare il valore su `externalTerminal` o `integratedTerminal`.

## <a name="introduction"></a>Introduzione

La libreria host generico è disponibile nello [spazio dei nomi Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) e viene specificata dal pacchetto [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/). Il pacchetto `Microsoft.Extensions.Hosting` è incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o versioni successive).

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) è il punto di ingresso per l'esecuzione del codice. Ogni implementazione `IHostedService` viene eseguita nell'ordine di [registrazione del servizio in ConfigureServices](#configureservices). [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) viene chiamato su ogni `IHostedService` quando viene avviato l'host e [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) viene chiamato in ordine di registrazione inverso quando l'host viene chiuso normalmente.

## <a name="set-up-a-host"></a>Configurare un host

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) è il componente principale che le librerie e le app usano per inizializzare, compilare ed eseguire l'host:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>Configurazione dell'host

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) si basa sugli approcci seguenti per impostare i valori di configurazione dell'host:

* Generatore di configurazioni
* Configurazione del metodo di estensione

### <a name="configuration-builder"></a>Generatore di configurazioni

La configurazione del generatore host viene creata chiamando [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) sull'implementazione [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder). `ConfigureHostConfiguration` usa un'interfaccia [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) per creare una [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) per l'host. Il generatore di configurazioni inizializza [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) per l'uso nel processo di compilazione dell'app.

La configurazione della variabile di ambiente non viene aggiunta per impostazione predefinita. Chiamare [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) nel generatore di host per configurare l'host dalle variabili di ambiente. `AddEnvironmentVariables` accetta un prefisso facoltativo definito dall'utente. L'app di esempio usa un prefisso di `PREFIX_`. Il prefisso viene rimosso quando vengono lette le variabili di ambiente. Quando viene configurato l'host dell'app di esempio, il valore della variabile di ambiente per `PREFIX_ENVIRONMENT` diventa il valore di configurazione dell'host per la chiave `environment`.

Durante lo sviluppo quando si usa [Visual Studio](https://www.visualstudio.com/) o si esegue un'app con `dotnet run`, le variabili di ambiente possono essere impostate nel file *Properties/launchSettings.json*. Durante lo sviluppo in [Visual Studio Code](https://code.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *.vscode/launch.json*. Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments).

`ConfigureHostConfiguration` può essere chiamato più volte e i risultati si sommano. L'host usa l'ultima opzione che imposta un valore.

*hostsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Configurazione di esempio di `HostBuilder` che usa `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> Il metodo di estensione [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (ad esempio `.AddConfiguration(Configuration.GetSection("section"))`). Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio `section:environment`). Il metodo `AddConfiguration` prevede che le chiavi corrispondano alle chiavi `HostBuilder` (ad esempio `environment`). La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'host. Questo problema verrà risolto in una delle prossime versioni. Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).

### <a name="extension-method-configuration"></a>Configurazione del metodo di estensione

I metodi di estensione vengono chiamati nell'implementazione `IHostBuilder` per configurare la radice del contenuto e l'ambiente.

#### <a name="content-root"></a>Radice del contenuto

Questa impostazione determina la posizione da cui l'host inizia la ricerca dei file di contenuto.

**Chiave**: contentRoot  
**Tipo**: *string*  
**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.  
**Impostare usando**: `UseContentRoot`  
**Variabile di ambiente**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configuration-builder))

Se il percorso non esiste, l'host non verrà avviato.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Ambiente

Imposta l'[ambiente](xref:fundamentals/environments) per l'app.

**Chiave**: environment  
**Tipo**: *string*  
**Impostazione predefinita**: Production  
**Impostare usando**: `UseEnvironment`  
**Variabile di ambiente**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` è [facoltativo e definito dall'utente](#configuration-builder))

L'ambiente può essere impostato su qualsiasi valore. I valori definiti dal framework includono `Development`, `Staging` e `Production`. Nei valori non viene fatta distinzione tra maiuscole e minuscole.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

La configurazione del generatore app viene creata chiamando [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) sull'implementazione [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder). `ConfigureAppConfiguration` usa un'interfaccia [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) per creare una [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) per l'app. `ConfigureAppConfiguration` può essere chiamato più volte e i risultati si sommano. L'app usa l'ultima opzione che imposta un valore. La configurazione creata da `ConfigureAppConfiguration` è disponibile in [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) per le operazioni successive e in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).

Configurazione app di esempio che usa `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> Il metodo di estensione [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (ad esempio `.AddConfiguration(Configuration.GetSection("section"))`). Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio `section:Logging:LogLevel:Default`). Il metodo `AddConfiguration` prevede una corrispondenza esatta per le chiavi di configurazione (ad esempio `Logging:LogLevel:Default`). La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'app. Questo problema verrà risolto in una delle prossime versioni. Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).

## <a name="configureservices"></a>ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) aggiunge servizi al contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app. `ConfigureServices` può essere chiamato più volte e i risultati si sommano.

Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). Per altre informazioni, vedere l'argomento [Attività in background con servizi ospitati](xref:fundamentals/host/hosted-services).

L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa il metodo di estensione `AddHostedService` per aggiungere all'app un servizio per gli eventi di durata (`LifetimeEventsHostedService`) e un'attività in background con timeout (`TimedHostedService`):

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) aggiunge un delegato per la configurazione dell'interfaccia [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) specificata. `ConfigureLogging` può essere chiamato più volte e i risultati si sommano.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) resta in ascolto di `Ctrl+C`/SIGINT o SIGTERM e chiama [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) per avviare il processo di arresto. `UseConsoleLifetime` sblocca estensioni come [RunAsync](#runasync) e [WaitForShutdownAsync](#waitforshutdownasync). [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) è preregistrata come implementazione di durata predefinita. Viene usata l'ultima durata registrata.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Configurazione contenitore

Per supportare l'inserimento di altri contenitori, l'host può accettare un elemento [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1). La disponibilità di una factory non fa parte della registrazione del contenitore DI, ma è una funzionalità intrinseca dell'host usata per creare il contenitore DI concreto. [UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) esegue l'override della factory predefinita usata per la creazione del provider di servizi dell'app.

La configurazione del contenitore personalizzato è gestita dal metodo [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer). `ConfigureContainer` offre un'esperienza fortemente tipizzata per la configurazione del contenitore sulla base dell'API host sottostante. `ConfigureContainer` può essere chiamato più volte e i risultati si sommano.

Creare un contenitore di servizi per l'app:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Specificare una factory per il contenitore di servizi:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Usare la factory e configurare il contenitore di servizi personalizzato per l'app:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Estendibilità

L'estensibilità host viene eseguita con i metodi di estensione su `IHostBuilder`. L'esempio seguente illustra come un metodo di estensione estende un'implementazione `IHostBuilder` con [RabbitMQ](https://www.rabbitmq.com/). Il metodo di estensione (altrove nell'app) registra un `IHostedService` di RabbitMQ:

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>Gestire l'host

L'implementazione [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) è responsabile dell'avvio e arresto delle implementazioni `IHostedService` che sono registrate nel contenitore di servizi.

### <a name="run"></a>Esegui

[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) avvia l'app e blocca il thread di chiamata fino all'arresto dell'host:

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a>RunAsync

[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) esegue l'app e restituisce un elemento `Task` che viene completato quando viene attivato il token di annullamento o l'arresto:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a>RunConsoleAsync

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) abilita il supporto della console, compila e avvia l'host e resta in ascolto di `Ctrl+C`/SIGINT o SIGTERM per eseguire l'arresto.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a>Start e StopAsync

[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) avvia l'host in modo sincrono.

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) prova ad arrestare l'host entro il timeout specificato.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a>StartAsync e StopAsync

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) avvia l'app.

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) arresta l'app.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a>WaitForShutdown

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) viene attivato tramite [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), ad esempio [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (in ascolto di `Ctrl+C`/SIGINT o SIGTERM). `WaitForShutdown` chiama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) restituisce un elemento `Task` che viene completato quando viene attivato l'arresto tramite il token specificato, quindi chiama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a>Controllo esterno

Il controllo esterno dell'host può essere ottenuto usando metodi che possono essere chiamati dall'esterno:

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) viene chiamato all'inizio di [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), che attende il completamento prima di continuare. Questo è utile per ritardare l'avvio fino al segnale trasmesso da un evento esterno.

## <a name="ihostingenvironment-interface"></a>Interfaccia IHostingEnvironment

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) include informazioni sull'ambiente di hosting dell'app. Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere `IHostingEnvironment` per poterne usare le proprietà e i metodi di estensione:

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments).

## <a name="iapplicationlifetime-interface"></a>Interfaccia IApplicationLifetime

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) consente attività post-avvio e di arresto, incluse le richieste di arresto normale. Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi `Action` che definiscono gli eventi di avvio e arresto.

| Token di annullamento | Attivato quando&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | L'host è stato completamente avviato. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | L'host sta completando un arresto normale. Tutte le richieste devono essere elaborate. L'arresto è bloccato fino al completamento di questo evento. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | L'host sta eseguendo un arresto normale. È possibile che le richieste siano ancora in esecuzione. L'arresto è bloccato fino al completamento di questo evento. |

Inserire tramite costruttore il servizio `IApplicationLifetime` in qualsiasi classe. L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa l'inserimento del costruttore in una classe `LifetimeEventsHostedService` (un'implementazione `IHostedService`) per registrare gli eventi.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) richiede la chiusura dell'applicazione corrente. La classe seguente usa `StopApplication` per arrestare normalmente un'app quando viene chiamato il metodo `Shutdown` della classe:

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Attività in background con servizi ospitati](xref:fundamentals/host/hosted-services)
* [Esempi di repository Hosting in GitHub](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
