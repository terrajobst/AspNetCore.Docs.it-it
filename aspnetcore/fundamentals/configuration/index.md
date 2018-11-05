---
title: Configurazione in ASP.NET Core
author: guardrex
description: Informazioni su come usare l'API di configurazione per configurare un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 2af66c0f35109dc1de954bf501f33ad61ddef4db
ms.sourcegitcommit: 85f2939af7a167b9694e1d2093277ffc9a741b23
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50968371"
---
# <a name="configuration-in-aspnet-core"></a>Configurazione in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

La configurazione delle app in ASP.NET Core si basa su coppie chiave-valore stabilite dai *provider di configurazione*. I provider di configurazione leggono i dati di configurazione in coppie chiave-valore da un'ampia gamma di origini di configurazione:

::: moniker range=">= aspnetcore-2.1"

* Azure Key Vault
* Argomenti della riga di comando
* Provider personalizzati (installati o creati)
* File della directory
* Variabili di ambiente
* Oggetti .NET in memoria
* File di impostazioni

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* Azure Key Vault
* Argomenti della riga di comando
* Provider personalizzati (installati o creati)
* Variabili di ambiente
* Oggetti .NET in memoria
* File di impostazioni

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* Argomenti della riga di comando
* Provider personalizzati (installati o creati)
* Variabili di ambiente
* Oggetti .NET in memoria
* File di impostazioni

::: moniker-end

Il *modello di opzioni* è un'estensione dei concetti di configurazione descritti in questo argomento. Le opzioni usano le classi per rappresentare i gruppi di impostazioni correlate. Per altre informazioni sull'uso del modello di opzioni, vedere <xref:fundamentals/configuration/options>.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

Gli esempi in questo argomento si basano su:

* Impostazione del percorso base dell'app con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` viene resa disponibile per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).
* Risoluzione di sezioni dei file di configurazione con <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>. `GetSection` viene resa disponibile per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).
* Associazione della configurazione a classi .NET con <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> e [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*). `Bind` e `Get<T>` vengono rese disponibili per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/). `Get<T>` è disponibile in ASP.NET Core 1.1 o versioni successive.

::: moniker range=">= aspnetcore-2.1"

Questi tre pacchetti sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Questi tre pacchetti sono inclusi nel [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

::: moniker-end

## <a name="host-vs-app-configuration"></a>Host e configurazione delle app

Prima che l'app venga configurata e avviata, viene configurato e avviato un *host*. L'host è responsabile della gestione dell'avvio e della durata delle app. Sia l'app che l'host vengono configurati tramite i provider di configurazione descritti in questo argomento. Le coppie chiave-valore di configurazione dell'host diventano parte della configurazione globale dell'app. Per altre informazioni su come vengono usati i provider di configurazione quando viene creato l'host e sugli effetti delle origini di configurazione sull'host di configurazione, vedere <xref:fundamentals/host/index>.

## <a name="security"></a>Sicurezza

Adottare le procedure consigliate seguenti:

* Non archiviare mai la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.
* Non usare i segreti di produzione in ambienti di sviluppo o di test.
* Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.

Altre informazioni su [come usare più ambienti](xref:fundamentals/environments) e la gestione dell'[archiviazione sicura dei segreti delle app durante lo sviluppo con Secret Manager](xref:security/app-secrets) (include consigli sull'uso delle variabili di ambiente per l'archiviazione di dati sensibili). Secret Manager usa il provider di configurazione dei file per archiviare i segreti utente in un file JSON nel sistema locale. Il provider di configurazione dei file è descritto più avanti in questo argomento.

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) è una delle opzioni per l'archiviazione sicura dei segreti delle app. Per ulteriori informazioni, vedere <xref:security/key-vault-configuration>.

## <a name="hierarchical-configuration-data"></a>Dati di configurazione gerarchici

L'API di configurazione è in grado di mantenere i dati di configurazione gerarchici rendendoli flat grazie all'uso di un delimitatore nelle chiavi di configurazione.

Nel file JSON seguente, esistono quattro chiavi in una gerarchia strutturata di due sezioni:

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

Quando il file viene letto nella configurazione, vengono create chiavi univoche per mantenere la struttura di dati gerarchici originale dell'origine di configurazione. Le sezioni e le chiavi vengono rese flat usando due punti (`:`) per mantenere la struttura originale:

* section0:key0
* section0:key1
* section1:key0
* section1:key1

I metodi <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sono disponibili per isolare le sezioni e gli elementi figlio di una sezione nei dati di configurazione. Questi metodi sono descritti più avanti in [GetSection, GetChildren ed Exists](#getsection-getchildren-and-exists).

## <a name="conventions"></a>Convenzioni

All'avvio dell'app, le origini di configurazione vengono lette nell'ordine con cui sono specificati i rispettivi provider di configurazione.

I provider di configurazione dei file supportano il ricaricamento della configurazione quando un file di impostazioni sottostante viene modificato dopo l'avvio dell'app. Il provider di configurazione dei file è descritto più avanti in questo argomento.

<xref:Microsoft.Extensions.Configuration.IConfiguration> è disponibile nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app. I provider di configurazione non possono usare l'inserimento delle dipendenze, perché non è disponibile quando vengono configurati dall'host.

Le chiavi di configurazione adottano le convenzioni seguenti:

* Per le chiavi non viene fatta distinzione tra maiuscole e minuscole. Ad esempio, `ConnectionString` e `connectionstring` vengono considerate chiavi equivalenti.
* Se un valore per la stessa chiave viene impostato dallo stesso provider di configurazione o da provider diversi, viene usato l'ultimo valore impostato per la chiave.
* Chiavi gerarchiche
  * Nell'ambito dell'API di configurazione, il separatore due punti (`:`) funziona in tutte le piattaforme.
  * Nelle variabili di ambiente, un separatore due punti potrebbe non funzionare in tutte le piattaforme. Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene convertito in due punti.
  * In Azure Key Vault, le chiavi gerarchiche usano `--` (due trattini) come separatore. È necessario fornire il codice per sostituire i trattini con due punti quando i segreti vengono caricati nella configurazione dell'app.
* Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione. L'associazione di matrici è descritta nella sezione [Associare una matrice a una classe](#bind-an-array-to-a-class).

I valori di configurazione adottano le convenzioni seguenti:

* I valori sono stringhe.
* I valori null non possono essere archiviati nella configurazione o associati a oggetti.

## <a name="providers"></a>Provider

La tabella seguente mostra i provider di configurazione disponibili per le app ASP.NET Core.

::: moniker range=">= aspnetcore-2.1"

| Provider | Fornisce la configurazione da&hellip; |
| -------- | ----------------------------------- |
| [Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*) | Azure Key Vault |
| [Provider di configurazione della riga di comando](#command-line-configuration-provider) | Parametri della riga di comando |
| [Provider di configurazione personalizzato](#custom-configuration-provider) | Origine personalizzata |
| [Provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider) | Variabili di ambiente |
| [Provider di configurazione dei file](#file-configuration-provider) | File (INI, JSON, XML) |
| [Provider di configurazione chiave-per-file](#key-per-file-configuration-provider) | File della directory |
| [Provider di configurazione della memoria](#memory-configuration-provider) | Raccolte in memoria |
| [Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*) | File nella directory dei profili utente |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| Provider | Fornisce la configurazione da&hellip; |
| -------- | ----------------------------------- |
| [Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*) | Azure Key Vault |
| [Provider di configurazione della riga di comando](#command-line-configuration-provider) | Parametri della riga di comando |
| [Provider di configurazione personalizzato](#custom-configuration-provider) | Origine personalizzata |
| [Provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider) | Variabili di ambiente |
| [Provider di configurazione dei file](#file-configuration-provider) | File (INI, JSON, XML) |
| [Provider di configurazione della memoria](#memory-configuration-provider) | Raccolte in memoria |
| [Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*) | File nella directory dei profili utente |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| Provider | Fornisce la configurazione da&hellip; |
| -------- | ----------------------------------- |
| [Provider di configurazione della riga di comando](#command-line-configuration-provider) | Parametri della riga di comando |
| [Provider di configurazione personalizzato](#custom-configuration-provider) | Origine personalizzata |
| [Provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider) | Variabili di ambiente |
| [Provider di configurazione dei file](#file-configuration-provider) | File (INI, JSON, XML) |
| [Provider di configurazione della memoria](#memory-configuration-provider) | Raccolte in memoria |
| [Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*) | File nella directory dei profili utente |

::: moniker-end

Le origini di configurazione vengono lette nell'ordine in cui vengono specificati i rispetti provider di configurazione all'avvio. I provider di configurazione descritti in questo argomento vengono presentati in ordine alfabetico e non nell'ordine in cui potrebbe disporli il codice. Ordinare i provider di configurazione nel codice in base alle specifiche priorità per le origini di configurazione sottostanti.

Una sequenza tipica di provider di configurazione è:

1. File (*appsettings.json*, *appsettings.{Environment}.json*, dove `{Environment}` è l'ambiente di hosting corrente dell'app)
1. [Insieme di credenziali delle chiavi di Azure](xref:security/key-vault-configuration)
1. [Segreti utente (Secret Manager)](xref:security/app-secrets) (sono nell'ambiente Development)
1. Variabili di ambiente
1. Argomenti della riga di comando

È pratica comune posizionare il provider di configurazione della riga di comando per ultimo in una serie di provider per consentire agli argomenti della riga di comando di sostituire la configurazione impostata da altri provider.

::: moniker range=">= aspnetcore-2.0"

Questa sequenza di provider viene applicata quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

È possibile creare questa sequenza di provider per l'app (non l'host) con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> e una chiamata al relativo metodo <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> in `Startup`:

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

Nell'esempio precedente, il nome dell'ambiente (`env.EnvironmentName`) e il nome di assembly dell'app (`env.ApplicationName`) vengono forniti da <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>. Per ulteriori informazioni, vedere <xref:fundamentals/environments>.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host Web per specificare i provider di configurazione dell'app, oltre a quelli aggiunti automaticamente da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a>Provider di configurazione della riga di comando

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carica la configurazione da coppie chiave-valore di argomenti della riga di comando in fase di esecuzione.

Per attivare la configurazione della riga di comando, metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> viene chiamato su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

::: moniker range=">= aspnetcore-2.0"

`AddCommandLine` viene chiamato automaticamente quando si Inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` carica anche:

* Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.
* [Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).
* Variabili di ambiente.

`CreateDefaultBuilder` aggiunge il provider di configurazione della riga di comando per ultimo. Gli argomenti della riga di comando passati in fase di esecuzione sostituiscono la configurazione impostata dagli altri provider.

`CreateDefaultBuilder` opera durante la creazione dell'host. Pertanto, la configurazione della riga di comando attivata da `CreateDefaultBuilder` può influire sul modo in cui l'host viene configurato.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.

`AddCommandLine` è già stato chiamato da `CreateDefaultBuilder`. Se è necessario specificare la configurazione dell'app ed è ancora possibile eseguire l'override della configurazione con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare infine `AddCommandLine`.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.

`AddCommandLine` è già stato chiamato da `CreateDefaultBuilder` quando è stato chiamato `UseConfiguration`. Se è necessario specificare la configurazione dell'app ed è ancora possibile eseguire l'override della configurazione con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in un `ConfigurationBuilder` e chiamare infine `AddCommandLine`.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Per attivare la configurazione della riga di comando, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Chiamare il provider per ultimo per consentire agli argomenti della riga di comando passati in fase di esecuzione di sostituire la configurazione impostata da altri provider di configurazione.

Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**Esempio**

::: moniker range=">= aspnetcore-2.0"

L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

L'app di esempio 1.x chiama <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> per un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

::: moniker-end

1. Aprire un prompt dei comandi nella directory del progetto.
1. Fornire un argomento della riga di comando per il comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.
1. Dopo che l'app è in esecuzione, aprire un browser per l'app all'indirizzo `http://localhost:5000`.
1. Notare che l'output contiene la coppia chiave-valore per l'argomento della riga di comando di configurazione fornito per `dotnet run`.

### <a name="arguments"></a>Argomenti

Il valore deve seguire un segno di uguale (`=`) o la chiave deve avere un prefisso (`--` o `/`) quando il valore segue uno spazio. Il valore può essere null se viene usato un segno di uguale (ad esempio, `CommandLineKey=`).

| Prefisso chiave               | Esempio                                                |
| ------------------------ | ------------------------------------------------------ |
| Nessun prefisso                | `CommandLineKey1=value1`                               |
| Due trattini (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Barra (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

Nello stesso comando, non mischiare coppie chiave-valore di argomenti della riga di comando che usano un segno di uguale con coppie chiave-valore che usano uno spazio.

Comandi di esempio:

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>Mapping di sostituzione

I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave. Quando si crea manualmente la configurazione con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, è possibile specificare un dizionario di mapping di sostituzione per il metodo <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando. Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la coppia chiave-valore nella configurazione dell'app. Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.

Regole principali del dizionario dei mapping di sostituzione:

* Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).
* Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.

::: moniker range=">= aspnetcore-2.1"

Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

Come illustrato nell'esempio precedente, la chiamata a `CreateDefaultBuilder` non deve passare argomenti quando vengono usati i mapping di sostituzione. La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`. Se gli argomenti includono una sostituzione mappata e vengono passati a `CreateDefaultBuilder`, l'inizializzazione del rispettivo provider `AddCommandLine` non riesce con <xref:System.FormatException>. La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Come illustrato nell'esempio precedente, la chiamata a `CreateDefaultBuilder` non deve passare argomenti quando vengono usati i mapping di sostituzione. La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`. Se gli argomenti includono una sostituzione mappata e vengono passati a `CreateDefaultBuilder`, l'inizializzazione del rispettivo provider `AddCommandLine` non riesce con <xref:System.FormatException>. La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.

| Chiave       | Valore             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

Se le chiavi con mapping di sostituzione vengono usate all'avvio dell'app, la configurazione riceve il valore di configurazione per la chiave fornita dal dizionario:

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

Dopo aver eseguito il comando precedente, la configurazione contiene i valori mostrati nella tabella seguente.

| Chiave               | Valore    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a>Provider di configurazione delle variabili di ambiente

<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carica la configurazione da coppie chiave-valore di variabili di ambiente in fase di esecuzione.

Per attivare la configurazione delle variabili di ambiente, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Quando si utilizzano chiavi gerarchiche in variabili di ambiente, il separatore due punti (`:`) potrebbe non funzionare in tutte le piattaforme. Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene sostituito con due punti.

[Servizio App di Azure](https://azure.microsoft.com/services/app-service/) consente di impostare variabili di ambiente nel portale di Azure che possono sostituire la configurazione delle app tramite il provider di configurazione delle variabili di ambiente. Per altre informazioni, vedere [App di Azure: Eseguire l'override della configurazione delle app usando il portale di Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).

::: moniker range=">= aspnetcore-2.0"

`AddEnvironmentVariables` viene chiamato automaticamente quando si Inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` carica anche:

* Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.
* [Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).
* Argomenti della riga di comando.

Il provider di configurazione delle variabili di ambiente viene chiamato dopo aver stabilito la configurazione dai segreti utente e dai file *appsettings*. La chiamata del provider in questa posizione consente alle variabili di ambiente lette in fase di esecuzione di sostituire la configurazione impostata dai segreti utente e dai file *appsettings*.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.

`AddEnvironmentVariables` per variabili di ambiente con prefisso `ASPNETCORE_` è già stato chiamato da `CreateDefaultBuilder`. Se è necessario specificare la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare `AddEnvironmentVariables` con il prefisso.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Chiamare il metodo di estensione `AddEnvironmentVariables` su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>. Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.

`AddEnvironmentVariables` per variabili di ambiente con prefisso `ASPNETCORE_` è già stato chiamato da `CreateDefaultBuilder`. Se è necessario specificare la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare `AddEnvironmentVariables` con il prefisso.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**Esempio**

::: moniker range=">= aspnetcore-2.0"

L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a `AddEnvironmentVariables`.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

L'app di esempio 1.x chiama `AddEnvironmentVariables` per un `ConfigurationBuilder`.

::: moniker-end

1. Eseguire l'app di esempio. Aprire un browser per l'app all'indirizzo `http://localhost:5000`.
1. Notare che l'output contiene la coppia chiave-valore per la variabile di ambiente `ENVIRONMENT`. Il valore riflette l'ambiente in cui l'app è in esecuzione, in genere `Development` durante l'esecuzione locale.

Per mantenere breve l'elenco delle variabili di ambiente restituito dall'app, l'app filtra le variabili di ambiente includendo quelle che iniziano come segue:

* ASPNETCORE_
* urls
* Registrazione
* ENVIRONMENT
* contentRoot
* AllowedHosts
* applicationName
* CommandLine

::: moniker range=">= aspnetcore-2.0"

Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Pages/Index.cshtml.cs* come segue:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Controllers/HomeController.cs* come segue:

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>Prefissi

Le variabili di ambiente caricate nella configurazione dell'app vengono filtrate quando si specifica un prefisso per il metodo `AddEnvironmentVariables`. Ad esempio, per filtrare le variabili di ambiente in base al prefisso `CUSTOM_`, fornire il prefisso al provider di configurazione:

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

Il prefisso viene rimosso quando vengono create le coppie chiave-valore della configurazione.

::: moniker range=">= aspnetcore-2.0"

Il metodo di servizio statico `CreateDefaultBuilder` crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> per stabilire l'host dell'app. Quando viene creato `WebHostBuilder`, la configurazione dell'host corrispondente viene individuata nelle variabili di ambiente con il prefisso `ASPNETCORE_`.

::: moniker-end

**Prefissi della stringa di connessione**

L'API di configurazione prevede regole di elaborazione speciali per quattro variabili di ambiente di stringa di connessione coinvolte nella configurazione di stringhe di connessione di Azure per l'ambiente dell'app. Le variabili di ambiente con i prefissi indicati nella tabella vengono caricate nell'app se non viene fornito alcun prefisso a `AddEnvironmentVariables`.

| Prefisso della stringa di connessione | Provider |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | Provider personalizzato |
| `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Database SQL di Azure](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |

Quando una variabile di ambiente viene individuata e caricata nella configurazione con uno qualsiasi dei quattro prefissi indicati nella tabella:

* La chiave di configurazione viene creata rimuovendo il prefisso della variabile di ambiente e aggiungendo una sezione per la chiave di configurazione (`ConnectionStrings`).
* Viene creata una nuova coppia chiave-valore della configurazione che rappresenta il provider di connessione al database (ad eccezione di `CUSTOMCONNSTR_`, che non ha un provider dichiarato).

| Chiave della variabile di ambiente | Chiave di configurazione convertita | Voce di configurazione del provider                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | Voce di configurazione non creata.                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | Chiave: `ConnectionStrings:<KEY>_ProviderName`:<br>Valore: `MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | Chiave: `ConnectionStrings:<KEY>_ProviderName`:<br>Valore: `System.Data.SqlClient`  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | Chiave: `ConnectionStrings:<KEY>_ProviderName`:<br>Valore: `System.Data.SqlClient`  |

## <a name="file-configuration-provider"></a>Provider di configurazione dei file

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> è la classe base per il caricamento della configurazione dal file system. I provider di configurazione seguenti sono dedicati per specifici tipi di file:

* [Provider di configurazione INI](#ini-configuration-provider)
* [Provider di configurazione JSON](#json-configuration-provider)
* [Provider di configurazione XML](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>Provider di configurazione INI

<xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carica la configurazione da coppie chiave-valore di file INI in fase di esecuzione.

Per attivare la configurazione di file INI, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

I due punti possono essere usati come delimitatore di sezione nella configurazione di file INI.

Gli overload consentono di specificare:

* Se il file è facoltativo.
* Se la configurazione viene ricaricata se viene modificato il file.
* Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.

::: moniker range=">= aspnetcore-2.1"

Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

Un esempio generico di un file di configurazione INI:

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

Il file di configurazione precedente carica le chiavi seguenti con `value`:

* section0:key0
* section0:key1
* section1:subsection:key
* section2:subsection0:key
* section2:subsection1:key

### <a name="json-configuration-provider"></a>Provider di configurazione JSON

Il <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carica la configurazione da coppie chiave-valore di file JSON in fase di esecuzione.

Per attivare la configurazione di file JSON, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Gli overload consentono di specificare:

* Se il file è facoltativo.
* Se la configurazione viene ricaricata se viene modificato il file.
* Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.

::: moniker range=">= aspnetcore-2.0"

`AddJsonFile` viene chiamato automaticamente due volte quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Il metodo viene chiamato per caricare la configurazione da:

* *appsettings.json* &ndash; Questo file viene letto per primo. La versione dell'ambiente del file può sostituire i valori forniti dal file *appsettings.json*.
* *appsettings.{Environment}.json* &ndash; La versione dell'ambiente del file viene caricata in base a [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).

Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` carica anche:

* Variabili di ambiente.
* [Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).
* Argomenti della riga di comando.

Il provider di configurazione JSON viene stabilito per primo. I segreti utente, le variabili di ambiente e gli argomenti della riga di comando sostituiscono quindi la configurazione impostata dai file *appsettings*.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app per file diversi da *appsettings.json* e *appsettings.{Environment}.json*:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

È possibile chiamare direttamente il metodo di estensione `AddJsonFile` su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**Esempio**

::: moniker range=">= aspnetcore-2.0"

L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include due chiamate a `AddJsonFile`. La configurazione viene caricata da *appsettings.json* e *appsettings.{Environment}.json*.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

L'app di esempio 1.x chiama `AddJsonFile` due volte su un `ConfigurationBuilder`. La configurazione viene caricata da *appsettings.json* e *appsettings.{Environment}.json*.

::: moniker-end

1. Eseguire l'app di esempio. Aprire un browser per l'app all'indirizzo `http://localhost:5000`.
1. Notare che l'output contiene coppie chiave-valore per la configurazione mostrata nella tabella in base all'ambiente. Le chiavi di configurazione di registrazione usano i due punti (`:`) come separatore gerarchico.

| Chiave                        | Valore Development | Valore Production |
| -------------------------- | :---------------: | :--------------: |
| Logging:LogLevel:System    | Informazioni       | Informazioni      |
| Logging:LogLevel:Microsoft | Informazioni       | Informazioni      |
| Logging:LogLevel:Default   | Debug             | Error            |
| AllowedHosts               | *                 | *                |

### <a name="xml-configuration-provider"></a>Provider di configurazione XML

Il <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carica la configurazione da coppie chiave-valore di file XML in fase di esecuzione.

Per attivare la configurazione di file XML, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Gli overload consentono di specificare:

* Se il file è facoltativo.
* Se la configurazione viene ricaricata se viene modificato il file.
* Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.

Il nodo radice del file di configurazione viene ignorato quando vengono create le coppie chiave-valore della configurazione. Non specificare una definizione DTD (Document Type Definition) o uno spazio dei nomi nel file.

::: moniker range=">= aspnetcore-2.1"

Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

I file di configurazione XML possono usare nomi di elementi distinti per le sezioni ripetute:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

Il file di configurazione precedente carica le chiavi seguenti con `value`:

* section0:key0
* section0:key1
* section1:key0
* section1:key1

La ripetizione di elementi che usano lo stesso nome di elemento funziona se si usa l'attributo `name` per distinguere gli elementi:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

Il file di configurazione precedente carica le chiavi seguenti con `value`:

* section:section0:key:key0
* section:section0:key:key1
* section:section1:key:key0
* section:section1:key:key1

È possibile usare attributi per fornire valori:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

Il file di configurazione precedente carica le chiavi seguenti con `value`:

* key:attribute
* section:key:attribute

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a>Provider di configurazione KeyPerFile

Il <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa i file di una directory come coppie chiave-valore della configurazione. La chiave è il nome del file. Il valore contiene il contenuto del file. Il provider di configurazione KeyPerFile viene usato in scenari di hosting Docker.

Per attivare la configurazione chiave-per-file, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>. Il `directoryPath` per i file deve essere un percorso assoluto.

Gli overload consentono di specificare:

* Un delegato `Action<KeyPerFileConfigurationSource>` che configura l'origine.
* Se la directory è facoltativa e il percorso della directory.

Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a>Provider di configurazione della memoria

Il <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una raccolta in memoria come coppie chiave-valore della configurazione.

Per attivare la configurazione della raccolta in memoria, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Il provider di configurazione può essere inizializzato con un `IEnumerable<KeyValuePair<String,String>>`.

::: moniker range=">= aspnetcore-2.1"

Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

        var config = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) estrae un valore dalla configurazione con una chiave specificata e lo converte nel tipo specificato. Un overload consente di specificare un valore predefinito se non viene trovata la chiave.

L'esempio seguente estrae il valore della stringa dalla configurazione con la chiave `NumberKey`, assegna al valore il tipo `int` e archivia il valore nella variabile `intValue`. Se `NumberKey` non viene trovato nelle chiavi di configurazione `intValue` riceve il valore predefinito `99`:

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a>GetSection, GetChildren ed Exists

Per gli esempi che seguono, prendere in considerazione il file JSON seguente. Vengono trovate quattro chiavi in due sezioni, una delle quali include una coppia di sottosezioni:

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

Quando il file viene letto nella configurazione, vengono create le chiavi gerarchiche univoche seguenti per contenere i valori di configurazione:

* section0:key0
* section0:key1
* section1:key0
* section1:key1
* section2:subsection0:key0
* section2:subsection0:key1
* section2:subsection1:key0
* section2:subsection1:key1

### <a name="getsection"></a>GetSection

[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) estrae una sottosezione della configurazione con la chiave di sottosezione specificata.

Per restituire una <xref:Microsoft.Extensions.Configuration.IConfigurationSection> che contiene solo le coppie chiave-valore in `section1`, chiamare `GetSection` e fornire il nome della sezione:

```csharp
var configSection = _config.GetSection("section1");
```

In modo analogo, per ottenere i valori per le chiavi in `section2:subsection0`, chiamare `GetSection` e fornire il percorso della sezione:

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

`GetSection` non restituisce mai `null`. Se non viene trovata una sezione corrispondente, viene restituita una `IConfigurationSection` vuota.

### <a name="getchildren"></a>GetChildren

Una chiamata a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) per `section2` ottiene una `IEnumerable<IConfigurationSection>` che include:

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a>Esistente

Usare [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) per determinare se esiste una sezione di configurazione:

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

Considerati i dati di esempio, `sectionExists` è `false` perché non esiste una sezione `section2:subsection2` nei dati di configurazione.

::: moniker-end

## <a name="bind-to-a-class"></a>Eseguire l'associazione a una classe

La configurazione può essere associata alle classi che rappresentano gruppi di impostazioni correlate usando il *modello di opzioni*. Per ulteriori informazioni, vedere <xref:fundamentals/configuration/options>.

I valori di configurazione vengono restituiti come stringhe, ma la chiamata di <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> consente la costruzione di oggetti [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).

L'app di esempio contiene un modello `Starship` (*Models/Starship.cs*):

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

La sezione `starship` del file *starship.json* crea la configurazione quando l'app di esempio usa il provider di configurazione JSON per caricare la configurazione:

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

Vengono create le coppie chiave-valore della configurazione seguenti:

| Chiave                   | Valore                                             |
| --------------------- | ------------------------------------------------- |
| starship:name         | USS Enterprise                                    |
| starship:registry     | NCC-1701                                          |
| starship:class        | Constitution                                      |
| starship:length       | 304.8                                             |
| starship:commissioned | False                                             |
| trademark             | Paramount Pictures Corp. http://www.paramount.com |

L'app di esempio chiama `GetSection` con la chiave `starship`. Le coppie chiave-valore `starship` sono isolate. Il metodo `Bind` viene chiamato sulla sottosezione passando un'istanza della classe `Starship`. Dopo aver associato i valori dell'istanza, l'istanza viene assegnata a una proprietà per il rendering:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a>Associazione a un oggetto grafico

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> è in grado di associare un intero oggetto grafico POCO.

L'esempio contiene un modello `TvShow` il cui oggetto grafico include `Metadata` e le classi `Actors` (*Models/TvShow.cs*):

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

L'app di esempio ha un file *tvshow.xml* che contiene i dati di configurazione:

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

La configurazione viene associata all'intero oggetto grafico `TvShow` con il metodo `Bind`. L'istanza associata viene assegnata a una proprietà per il rendering:

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e restituisce il tipo specificato. `Get<T>` può essere più comodo che usare `Bind`. Il codice seguente mostra come usare `Get<T>` con l'esempio precedente, che consente di assegnare l'istanza associata direttamente alla proprietà usata per il rendering:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a>Associare una matrice a una classe

*L'app di esempio dimostra i concetti spiegati in questa sezione.*

Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione. Qualsiasi formato di matrice che espone un segmento chiave numerico (`:0:`, `:1:`, &hellip; `:{n}:`) supporta l'associazione di matrici a una matrice di classi POCO.

> [!NOTE]
> L'associazione viene fornita per convenzione. I provider di configurazione personalizzati non devono implementare l'associazione di matrici.

**Elaborazione di matrici in memoria**

Prendere in considerazione le chiavi di configurazione e i valori indicati nella tabella seguente.

| Chiave     | Valore  |
| :-----: | :----: |
| array:0 | value0 |
| array:1 | value1 |
| array:2 | value2 |
| array:4 | value4 |
| array:5 | value5 |

Queste chiavi e i valori vengono caricati nell'app di esempio usando il provider di configurazione della memoria:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

La matrice ignora un valore per l'indice &num;3. Lo strumento di associazione di configurazione non è in grado di associare valori null o di creare voci null negli oggetti associati, situazione che diventerà chiara tra poco quando viene illustrato il risultato dell'associazione di questa matrice a un oggetto.

Nell'app di esempio, è disponibile una classe POCO per i dati di configurazione associati:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

I dati di configurazione sono associati all'oggetto:

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

È possibile usare anche la sintassi [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), che produce codice più compatto:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

L'oggetto associato, un'istanza di `ArrayExample`, riceve i dati di matrice dalla configurazione.

| Indice di `ArrayExamples.Entries` | Valore di `ArrayExamples.Entries` |
| :---------------------------: | :---------------------------: |
| 0                             | value0                        |
| 1                             | value1                        |
| 2                             | value2                        |
| 3                             | value4                        |
| 4                             | value5                        |

L'indice &num;3 nell'oggetto associato contiene i dati di configurazione per la chiave di configurazione `array:4` e il relativo valore `value4`. Quando i dati di configurazione contenenti una matrice vengono associati, gli indici di matrice nelle chiavi di configurazione vengono usati semplicemente per scorrere i dati di configurazione quando si crea l'oggetto. Un valore null non può essere mantenuto nei dati di configurazione e una voce con valore null non viene creata in un oggetto associato quando una matrice nelle chiavi di configurazione ignora uno o più indici.

L'elemento di configurazione mancante per l'indice &num;3 può essere fornito prima dell'associazione all'istanza `ArrayExamples` da qualsiasi provider di configurazione che produce la coppia chiave-valore corretta nella configurazione. Se il codice di esempio include un provider di configurazione JSON aggiuntivo con la coppia chiave-valore mancante, `ArrayExamples.Entries` corrisponde alla matrice di configurazione completa:

*missing_value.json*:

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Nel costruttore `Startup`:

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

La coppia chiave-valore mostrata nella tabella viene caricata nella configurazione.

| Chiave             | Valore  |
| :-------------: | :----: |
| array:entries:3 | value3 |

Se l'istanza della classe `ArrayExamples` viene associata dopo che il provider di configurazione JSON include la voce per l'indice &num;3, la matrice `ArrayExamples.Entries` include il valore.

| Indice di `ArrayExamples.Entries` | Valore di `ArrayExamples.Entries` |
| :---------------------------: | :---------------------------: |
| 0                             | value0                        |
| 1                             | value1                        |
| 2                             | value2                        |
| 3                             | value3                        |
| 4                             | value4                        |
| 5                             | value5                        |

**Elaborazione di matrice JSON**

Se un file JSON contiene una matrice, vengono create chiavi di configurazione per gli elementi della matrice con un indice di sezione in base zero. Nel file di configurazione seguente, `subsection` è una matrice:

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

Il provider di configurazione JSON legge i dati di configurazione nelle coppie chiave-valore seguenti:

| Chiave                     | Valore  |
| ----------------------- | :----: |
| json_array:key          | valueA |
| json_array:subsection:0 | valueB |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | valueD |

Nell'app di esempio, la classe POCO seguente è disponibile per associare le coppie chiave-valore della configurazione:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

Dopo l'associazione, `JsonArrayExample.Key` contiene il valore `valueA`. I valori di sottosezione vengono archiviati nella proprietà matrice POCO `Subsection`.

| Indice di `JsonArrayExample.Subsection` | Valore di `JsonArrayExample.Subsection` |
| :---------------------------------: | :---------------------------------: |
| 0                                   | valueB                              |
| 1                                   | valueC                              |
| 2                                   | valueD                              |

## <a name="custom-configuration-provider"></a>Provider di configurazione personalizzato

L'app di esempio dimostra come creare un provider di configurazione di base che legge le coppie chiave-valore di configurazione da un database usando [Entity Framework (EF)](/ef/core/).

Il provider ha le caratteristiche seguenti:

* Il database in memoria di Entity Framework viene usato a scopo dimostrativo. Per usare un database che richiede una stringa di connessione, implementare un `ConfigurationBuilder` secondario per fornire la stringa di connessione da un altro provider di configurazione.
* Il provider legge una tabella di database in una configurazione all'avvio. Il provider non esegue una query sul database per ogni chiave.
* Il ricaricamento in caso di modifica non è implementato, quindi l'aggiornamento del database dopo l'avvio dell'app non ha alcun effetto sulla configurazione dell'app.

Definire un'entità `EFConfigurationValue` per l'archiviazione dei valori di configurazione nel database.

*Models/EFConfigurationValue.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.

*EFConfigurationProvider/EFConfigurationContext.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.

*EFConfigurationProvider/EFConfigurationSource.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>. Il provider di configurazione inizializza il database quando è vuoto.

*EFConfigurationProvider/EFConfigurationProvider.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.

*Extensions/EntityFrameworkExtensions.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a>Accedere alla configurazione durante l'avvio

Inserire `IConfiguration` nel costruttore `Startup` per accedere ai valori di configurazione in `Startup.ConfigureServices`. Per accedere alla configurazione in `Startup.Configure`, inserire `IConfiguration` direttamente nel metodo o usare l'istanza dal costruttore:

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

Per un esempio di accesso alla configurazione usando metodi di servizio di avvio, vedere [Avvio dell'applicazione: Metodi pratici](xref:fundamentals/startup#convenience-methods).

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a>Accedere alla configurazione in una pagina Razor Pages o in una visualizzazione MVC

Per accedere alle impostazioni di configurazione in una pagina Razor Pages o in una visualizzazione MVC, aggiungere un [direttiva using](xref:mvc/views/razor#using) ([Informazioni di riferimento su C#: direttiva using](/dotnet/csharp/language-reference/keywords/using-directive)) per lo [spazio dei nomi Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inserire <xref:Microsoft.Extensions.Configuration.IConfiguration> nella pagina o nella visualizzazione.

In una pagina Razor Pages:

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

In una visualizzazione MVC:

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a>Aggiungere la configurazione da un assembly esterno

Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app. Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/configuration/options>
* [Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Configurazione Microsoft in dettaglio)
