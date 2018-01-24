---
title: Configurazione in ASP.NET Core
author: rick-anderson
description: "È possibile usare l'API di configurazione per configurare un'app ASP.NET Core in diversi modi."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: c4f57d1e02ad5f4e235039999af9df9d236756a7
ms.sourcegitcommit: 3d512ea991ac36dfd4c800b7d1f8a27bfc50635e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2018
---
# <a name="configure-an-aspnet-core-app"></a>Configurare un'app ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

L'API di configurazione fornisce un modo per configurare un'app Web ASP.NET Core in base a un elenco di coppie nome-valore. La configurazione viene letta in fase di esecuzione da più origini. È possibile raggruppare queste coppie nome-valore in una gerarchia a più livelli.

Esistono provider di configurazione per:

* Formati di file (INI, JSON e XML)
* Argomenti della riga di comando
* Variabili di ambiente
* Oggetti .NET in memoria
* Un archivio utente crittografato
* [Insieme di credenziali delle chiavi di Azure](xref:security/key-vault-configuration)
* Provider personalizzati (installati o creati)

Ogni valore di configurazione è associato a una chiave di stringa. È disponibile il supporto di associazione incorporato per deserializzare le impostazioni in un oggetto personalizzato [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) (una classe .NET semplice con proprietà).

Il modello di opzioni usa le classi di opzioni per rappresentare i gruppi di impostazioni correlate. Per altre informazioni sull'uso del modello di opzioni, vedere l'argomento [Opzioni](xref:fundamentals/configuration/options).

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="json-configuration"></a>Configurazione JSON

La seguente app di console usa il provider di configurazione JSON:

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

L'app legge e visualizza le impostazioni di configurazione seguenti:

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

La configurazione è costituita da un elenco gerarchico di coppie nome-valore in cui i nodi sono separati dai due punti. Per recuperare un valore, accedere all'indicizzatore `Configuration` con la chiave dell'elemento corrispondente:

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=21-22)]

Per lavorare con le matrici nelle origini di configurazione in formato JSON, usare un indice di matrice come parte della stringa separata da due punti. L'esempio seguente recupera il nome del primo elemento della matrice precedente `wizards`:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

Le coppie nome-valore scritte nei provider predefiniti [Configurazione](/dotnet/api/microsoft.extensions.configuration) **non** sono salvate in modo permanente. Tuttavia è possibile creare un provider personalizzato che salva i valori. Vedere [provider di configurazione personalizzati](xref:fundamentals/configuration/index#custom-config-providers).

L'esempio precedente usa l'indicizzatore di configurazione per leggere i valori. Per accedere alla configurazione fuori da `Startup`, usare il *modello di opzioni*. Per altre informazioni, vedere l'argomento [Opzioni](xref:fundamentals/configuration/options).


## <a name="configuration-by-environment"></a>Configurazione per ambiente

Spesso si hanno diverse impostazioni di configurazione per ambienti diversi, ad esempio per sviluppo, test e produzione. Il metodo di estensione `CreateDefaultBuilder` in un'app ASP.NET Core 2.x (o l'uso di `AddJsonFile` e `AddEnvironmentVariables` direttamente in un'app ASP.NET Core 1.x) aggiunge dei provider di configurazione per la lettura dei file JSON e le origini configurazione del sistema:

* *appsettings.json*
* *appsettings.\<EnvironmentName>.json*
* Variabili di ambiente

Le app di ASP.NET Core 1.x devono chiamare `AddJsonFile` e [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).

Vedere [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) per una spiegazione dei parametri. `reloadOnChange` è supportato solo in ASP.NET Core 1.1 e versioni successive.

Le origini di configurazione vengono lette nell'ordine in cui sono specificate. Nel codice precedente, le variabili di ambiente vengono lette per ultime. I valori di configurazione impostati tramite l'ambiente sostituiscono quelli impostati nei due provider precedenti.

Considerare il file seguente *appsettings. Staging.JSON*:

[!code-json[Main](index/sample/appsettings.Staging.json)]

Quando l'ambiente è impostato su `Staging`, il metodo `Configure` seguente legge il valore di `MyConfig`:

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


L'ambiente è in genere impostato su `Development`, `Staging` o `Production`. Per altre informazioni, vedere [Working with multiple environments](xref:fundamentals/environments) (Utilizzo con più ambienti).

Considerazioni sulla configurazione:

* `IOptionsSnapshot` può ricaricare i dati di configurazione in caso di modifiche. Per altre informazioni, vedere [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).
* Le chiavi di configurazione **non** rispettano la distinzione tra maiuscole e minuscole.
* Non archiviare **mai** la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale. Non usare i segreti di produzione nell'ambiente di sviluppo o di test. Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati al repository. Altre informazioni sull'[uso di più ambienti](xref:fundamentals/environments) e sulla gestione dell'[archiviazione sicura dei segreti delle app durante lo sviluppo](xref:security/app-secrets).
* Se non è possibile usare i due punti (`:`) nelle variabili di ambiente nel sistema, sostituire i due punti (`:`) con un doppio carattere di sottolineatura (`__`).

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Provider in memoria e associazione a una classe POCO

L'esempio seguente illustra come usare il provider in memoria e associarlo a una classe:

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

I valori di configurazione vengono restituiti come stringhe, ma l'associazione consente la costruzione di oggetti. L'associazione consente di recuperare gli oggetti POCO o addirittura interi oggetti grafici.

### <a name="getvalue"></a>GetValue

Nell'esempio seguente viene illustrato il metodo di estensione [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

Il metodo `GetValue<T>` di ConfigurationBinder consente di specificare un valore predefinito (80 nell'esempio). `GetValue<T>` è per scenari semplici e non viene associato a intere sezioni. `GetValue<T>` ottiene valori scalari da `GetSection(key).Value` convertiti in un tipo specifico.

## <a name="bind-to-an-object-graph"></a>Associazione a un oggetto grafico

È possibile impostare associazioni ricorsive a ogni oggetto di una classe. Si consideri la classe `AppSettings` seguente:

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

L'esempio seguente è associato alla classe `AppSettings`:

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** e le versioni successive possono usare `Get<T>`, che funziona con intere sezioni. `Get<T>` può essere più comodo che usare `Bind`. L'esempio di codice seguente illustra come usare il metodo `Get<T>` con l'esempio precedente:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Tramite il file *appSettings.json* seguente:

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

Il programma visualizza `Height 11`.

Il codice seguente consente di seguire un unit test della configurazione:

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a>Creare un provider personalizzato di Entity Framework

In questa sezione viene creato un provider di configurazione di base che legge le coppie nome-valore da un database tramite Entity Framework.

Definire un'entità `ConfigurationValue` per l'archiviazione dei valori di configurazione nel database:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Aggiungere `ConfigurationContext` per archiviare e accedere ai valori configurati:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Creare una classe che implementi [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Creare il provider di configurazione personalizzato ereditando da [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider). Il provider di configurazione inizializza il database quando è vuoto:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Quando l'esempio viene eseguito, vengono visualizzati i valori evidenziati del database ("value_from_ef_1" e "value_from_ef_2").

È possibile aggiungere un metodo di estensione `EFConfigSource` per aggiungere l'origine di configurazione:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

L'esempio di codice seguente illustra come usare il metodo `EFConfigProvider` personalizzato:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Notare che l'esempio aggiunge l'oggetto personalizzato `EFConfigProvider` dopo il provider JSON, pertanto le impostazioni del database sostituiranno le impostazioni del file *appSettings.json*.

Tramite il file *appSettings.json* seguente:

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

Verrà visualizzato l'output seguente:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Provider di configurazione CommandLine

Il [provider di configurazione CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) riceve coppie chiave-valore di argomenti della riga di comando per la configurazione in fase di esecuzione.

[Visualizzare o scaricare l'esempio di configurazione CommandLine](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Impostazione e uso del provider di configurazione CommandLine

# <a name="basic-configurationtabbasicconfiguration"></a>[Configurazione di base](#tab/basicconfiguration)

Per attivare la configurazione della riga di comando, chiamare il metodo di estensione `AddCommandLine` su un'istanza di [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

Eseguendo il codice, verrà visualizzato l'output seguente:

```console
MachineName: MairaPC
Left: 1980
```

Passando coppie chiave-valore degli argomenti nella riga di comando si modificano i valori di `Profile:MachineName` e `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

Nella finestra della console viene visualizzato:

```console
MachineName: BartPC
Left: 1979
```

Per eseguire l'override della configurazione fornita da altri provider di configurazione con la configurazione della riga di comando, chiamare `AddCommandLine` per ultimo su `ConfigurationBuilder`:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Le tipiche app ASP.NET Core 2.x usano il metodo statico `CreateDefaultBuilder` per compilare l'host:

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` carica la configurazione facoltativa da *appSettings.json*, *appsettings.{Environment}.json*, [i segreti utente](xref:security/app-secrets) (nell'ambiente `Development`), le variabili di ambiente e gli argomenti della riga di comando. Il provider di configurazione CommandLine è chiamato per ultimo. Chiamando il provider per ultimo, gli argomenti della riga di comando passati in fase di esecuzione possono eseguire l'override della configurazione impostata da altri provider di configurazione chiamati in precedenza.

Per i file *appsettings* in cui:

* `reloadOnChange` è abilitato.
* Contiene la stessa impostazione negli argomenti della riga di comando e un file *appsettings*.
* Il file *appsettings* contenente l'argomento della riga di comando corrispondente è stato modificato dopo l'avvio dell'applicazione.

Se tutte le condizioni precedenti sono vere, gli argomenti della riga di comando vengono sottoposti a override.

L'app di ASP.NET Core 2. x può usare WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) invece di "CreateDefaultBuilder`. When using `WebHostBuilder", impostare manualmente la configurazione con [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder). Per altre informazioni, vedere la scheda ASP.NET Core 1.x.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Creare un oggetto [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) e chiamare il metodo `AddCommandLine` per usare il provider di configurazione CommandLine. Chiamando il provider per ultimo, gli argomenti della riga di comando passati in fase di esecuzione possono eseguire l'override della configurazione impostata da altri provider di configurazione chiamati in precedenza. Applicare la configurazione a [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) con il metodo `UseConfiguration`:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Argomenti

Gli argomenti passati alla riga di comando devono essere conformi a uno dei due formati indicati nella tabella seguente:

| Formato dell'argomento                                                     | Esempio        |
| ------------------------------------------------------------------- | :------------: |
| Singolo argomento: una coppia chiave-valore separata da un segno di uguale (`=`) | `key1=value`   |
| Sequenza di due argomenti: una coppia chiave-valore separata da uno spazio    | `/key1 value1` |

**Singolo argomento**

Il valore deve seguire un segno di uguale (`=`). Il valore può essere null (ad esempio `mykey=`).

La chiave può avere un prefisso.

| Prefisso chiave               | Esempio         |
| ------------------------ | :-------------: |
| Nessun prefisso                | `key1=value1`   |
| Trattino singolo (`-`)&#8224; | `-key2=value2`  |
| Due trattini (`--`)        | `--key3=value3` |
| Barra (`/`)      | `/key4=value4`  |

&#8224;Una chiave con un prefisso con trattino singolo (`-`) deve essere fornita nei [mapping di sostituzione](#switch-mappings), come descritto di seguito.

Comando di esempio:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Nota: se `-key1` non è presente nei [mapping di sostituzione](#switch-mappings) forniti al provider di configurazione, viene generata un'eccezione `FormatException`.

**Sequenza di due argomenti**

Il valore non può essere null e deve seguire la chiave separata da uno spazio.

La chiave deve avere un prefisso.

| Prefisso chiave               | Esempio         |
| ------------------------ | :-------------: |
| Trattino singolo (`-`)&#8224; | `-key1 value1`  |
| Due trattini (`--`)        | `--key2 value2` |
| Barra (`/`)      | `/key3 value3`  |

&#8224;Una chiave con un prefisso con trattino singolo (`-`) deve essere fornita nei [mapping di sostituzione](#switch-mappings), come descritto di seguito.

Comando di esempio:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Nota: se `-key1` non è presente nei [mapping di sostituzione](#switch-mappings) forniti al provider di configurazione, viene generata un'eccezione `FormatException`.

### <a name="duplicate-keys"></a>Chiavi duplicate

Se vengono fornite chiavi duplicate, viene usata l'ultima coppia chiave-valore.

### <a name="switch-mappings"></a>Mapping di sostituzione

Quando si compila manualmente la configurazione con `ConfigurationBuilder`, è possibile fornire facoltativamente un dizionario di mapping di sostituzione per il metodo `AddCommandLine`. I mapping di sostituzione consentono di fornire la logica di sostituzione del nome della chiave.

Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando. Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la configurazione. Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.

Regole principali del dizionario dei mapping di sostituzione:

* Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).
* Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.

Nell'esempio seguente, il metodo `GetSwitchMappings` consente agli argomenti della riga di comando di usare un prefisso di chiave con trattino singolo (`-`) ed evitare i prefissi iniziali nelle sottochiavi.

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

Senza fornire argomenti della riga di comando, il dizionario per `AddInMemoryCollection` imposta i valori di configurazione. Eseguire l'app con il comando seguente:

```console
dotnet run
```

Nella finestra della console viene visualizzato:

```console
MachineName: RickPC
Left: 1980
```

Per passare le impostazioni di configurazione, usare il codice seguente:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

Nella finestra della console viene visualizzato:

```console
MachineName: DahliaPC
Left: 1984
```

Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente:

| Chiave            | Valore                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Per illustrare la sostituzione della chiave tramite il dizionario, eseguire il comando seguente:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Le chiavi della riga di comando vengono scambiate. Nella finestra della console vengono visualizzati i valori di configurazione per `Profile:MachineName` e `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>File web.config

Un file *web.config* è obbligatorio per l'hosting di app in IIS o IIS Express. Le impostazioni in *web.config* consentono al [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) di avviare l'app e configurare altre impostazioni e moduli di IIS. Se il file *web.config* file non è presente e il file di progetto include `<Project Sdk="Microsoft.NET.Sdk.Web">`, la pubblicazione del progetto crea un file *web.config* nell'output pubblicato (cartella *publish*). Per altre informazioni, vedere [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig) (Host ASP.NET Core in Windows con IIS).

## <a name="additional-notes"></a>Note aggiuntive

* L'inserimento delle dipendenze non viene impostato fino quando non viene richiamato `ConfigureServices`.
* Il sistema di configurazione non riconosce l'inserimento delle dipendenze.
* `IConfiguration` ha due specializzazioni:
  * `IConfigurationRoot` Usata per il nodo radice. Può attivare un ricaricamento.
  * `IConfigurationSection` Rappresenta una sezione dei valori di configurazione. I metodi `GetSection` e `GetChildren` restituiscono un oggetto `IConfigurationSection`.
  * Usare [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) durante il ricaricamento di configurazione o accedere a ogni provider. Non si tratta di situazioni comuni.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Opzioni](xref:fundamentals/configuration/options)
* [Uso di più ambienti](xref:fundamentals/environments)
* [Archiviazione sicura di segreti dell'app durante lo sviluppo](xref:security/app-secrets)
* [Hosting in ASP.NET Core](xref:fundamentals/hosting)
* [Inserimento dipendenze](xref:fundamentals/dependency-injection)
* [Azure Key Vault configuration provider](xref:security/key-vault-configuration) (Provider di configurazione di Azure Key Vault)
