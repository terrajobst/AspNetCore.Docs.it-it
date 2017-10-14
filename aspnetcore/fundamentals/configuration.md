---
title: Configurazione in ASP.NET Core
author: rick-anderson
description: "Informazioni su come usare l'API di configurazione per configurare un'app di ASP.NET Core da più origini."
keywords: ASP.NET Core, configurazione, JSON, configurazione
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: d626768fe1a485705e104a5c758cbdb0b46685a3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="configuration-in-aspnet-core"></a>Configurazione in ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT), [feed di Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), e [Luke Latham](https://github.com/guardrex)

L'API di configurazione fornisce un modo per configurare un'app in base a un elenco di coppie nome-valore. Configurazione è di lettura in fase di esecuzione da più origini. Le coppie nome-valore possono essere raggruppate in una gerarchia a più livello. Sono disponibili provider di configurazione per:

* Formati di file (INI, JSON e XML)
* Argomenti della riga di comando
* Variabili di ambiente
* Oggetti .NET in memoria
* Un archivio utente crittografato
* [Insieme di credenziali chiave di Azure](xref:security/key-vault-configuration)
* Provider personalizzati, cui installa o crea

Ogni valore di configurazione esegue il mapping a una chiave di stringa. Supporto di associazione predefinita per deserializzare le impostazioni in un oggetto personalizzato [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) oggetto (una classe .NET semplice con proprietà).

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="simple-configuration"></a>Configurazione semplice

La seguente applicazione console utilizza il provider di configurazione JSON:

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

L'app legge e visualizza le impostazioni di configurazione seguenti:

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

Configurazione è costituita da un elenco gerarchico di coppie nome-valore in cui i nodi sono separati dai due punti. Per recuperare un valore, accedere il `Configuration` un indicizzatore con la corrispondente chiave dell'elemento:

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

Per lavorare con le matrici nelle origini configurazione in formato JSON, utilizzare un indice della matrice come parte della stringa separate da virgola. Nell'esempio seguente ottiene il nome del primo elemento nel passaggio precedente `wizards` matrice:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

Coppie nome-valore scritte incorporato in `Configuration` provider sono **non** persistente, tuttavia, è possibile creare un provider personalizzato che salva i valori. Vedere [provider di configurazione personalizzato](xref:fundamentals/configuration#custom-config-providers).

L'esempio precedente Usa l'indicizzatore di configurazione per leggere i valori. Per la configurazione di accesso di fuori di `Startup`, utilizzare il [modello opzioni](xref:fundamentals/configuration#options-config-objects). Il *modello opzioni* è illustrato più avanti in questo articolo.

È in genere hanno diverse impostazioni di configurazione per gli ambienti diversi, ad esempio sviluppo, test e produzione. Il `CreateDefaultBuilder` il metodo di estensione in un'app di ASP.NET Core 2. x (o tramite `AddJsonFile` e `AddEnvironmentVariables` direttamente in un'app di ASP.NET Core 1. x) aggiunge dei provider di configurazione per la lettura dei file JSON e sistema origini configurazione:

* *appsettings.json*
* *appSettings. \<EnvironmentName > JSON*
* variabili di ambiente

Vedere [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) per una spiegazione dei parametri. `reloadOnChange`è supportato solo in ASP.NET Core 1.1 e versioni successive. 

Origini di configurazione vengono letti nell'ordine in che cui vengono specificati. Nel codice precedente, le variabili di ambiente vengono lette ultima. Tutti i valori di configurazione impostati tramite l'ambiente di sostituire quelli impostati nei due provider precedente.

L'ambiente è in genere impostato su uno dei `Development`, `Staging`, o `Production`. Vedere [utilizzo di più ambienti](environments.md) per ulteriori informazioni.

Considerazioni sulla configurazione di:

* `IOptionsSnapshot`possibile ricaricare i dati di configurazione in caso di modifiche. Utilizzare `IOptionsSnapshot` se è necessario ricaricare i dati di configurazione.  Vedere [IOptionsSnapshot](#ioptionssnapshot) per ulteriori informazioni.
* Le chiavi di configurazione vengono fatta distinzione tra maiuscole e minuscole.
* Una procedura consigliata è specificare le variabili di ambiente per ultimo, in modo che l'ambiente locale può eseguire l'override di qualsiasi elemento impostati nei file di configurazione distribuita.
* **Mai** archiviare password o altri dati sensibili nel codice di provider di configurazione o nel file di configurazione di testo normale. Non è possibile utilizzare i segreti di produzione nell'ambiente di sviluppo o ambienti di test. Specificare invece i segreti di fuori della struttura del progetto, in modo che non possono accidentalmente salvate nel repository. Altre informazioni, vedere [utilizzo di più ambienti](environments.md) e la gestione di [archiviazione sicura di segreti dell'app durante lo sviluppo](../security/app-secrets.md).
* Se `:` non può essere utilizzato in variabili di ambiente del sistema, sostituire `:` con `__` (doppio carattere di sottolineatura).

<a name="options-config-objects"></a>

## <a name="using-options-and-configuration-objects"></a>Utilizzando le opzioni e gli oggetti di configurazione

Il modello di opzioni Usa le classi di opzioni personalizzate per rappresentare un gruppo di impostazioni correlate. Si consiglia di creare classi separate per ogni funzionalità all'interno dell'app. Classi disaccoppiate seguono:

* Il [interfaccia separazione principio (ISP)](http://deviq.com/interface-segregation-principle/) : classi dipendono solo usano le impostazioni di configurazione.
* [La separazione dei compiti](http://deviq.com/separation-of-concerns/) : le impostazioni per le diverse parti dell'app non sono dipendenti o a uno di loro.

La classe di opzioni deve essere non astratto con costruttore pubblico senza parametri. Ad esempio:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name="options-example"></a>

Nel codice seguente, il provider di configurazione JSON è abilitato. La `MyOptions` classe viene aggiunta al contenitore del servizio e l'associazione alla configurazione.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

Nell'esempio [controller](../mvc/controllers/index.md) utilizza [costruttore Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) su [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) per accedere alle impostazioni:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

Con i seguenti *appSettings. JSON* file:

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

Il `HomeController.Index` restituisce `option1 = value1_from_json, option2 = 2`.

App tipica non associare l'intera configurazione in un file con le opzioni single. In seguito verrà illustrato come utilizzare `GetSection` da associare a una sezione.

Nel codice seguente, una seconda `IConfigureOptions<TOptions>` servizio viene aggiunto al contenitore del servizio. Usa un delegato per configurare l'associazione con `MyOptions`.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

È possibile aggiungere più provider di configurazione. Provider di configurazione sono disponibili in pacchetti NuGet. Vengono applicate nell'ordine in cui sono registrati.

Ogni chiamata a `Configure<TOptions>` aggiunge un `IConfigureOptions<TOptions>` servizio al contenitore del servizio. Nell'esempio precedente, i valori di `Option1` e `Option2` sono entrambi specificati nel *appSettings. JSON* , ma il valore del `Option1` viene sottoposto a override dal delegato configurato. 

Quando più di un servizio di configurazione è abilitato, l'ultima origine di configurazione specificato "wins" (imposta il valore di configurazione). Nel codice precedente, il `HomeController.Index` restituisce `option1 = value1_from_action, option2 = 2`.

Quando si associa opzioni a configurazione, ogni proprietà nel tipo di opzioni è associata a una chiave di configurazione del modulo `property[:sub-property:]`. Ad esempio, il `MyOptions.Option1` proprietà è associata alla chiave `Option1`, che viene letto dal `option1` proprietà *appSettings. JSON*. Un esempio delle sottoproprietà è illustrato più avanti in questo articolo.

Nel codice seguente, una terza `IConfigureOptions<TOptions>` servizio viene aggiunto al contenitore del servizio. Associa `MySubOptions` alla sezione `subsection` del *appSettings. JSON* file:

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

Nota: Questo metodo di estensione richiede il `Microsoft.Extensions.Options.ConfigurationExtensions` pacchetto NuGet.

Con i seguenti *appSettings. JSON* file:

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

La `MySubOptions` classe:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

Con i seguenti `Controller`:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

`subOption1 = subvalue1_from_json, subOption2 = 200`viene restituito.

È inoltre possibile visualizzare le opzioni disponibili in un modello di visualizzazione o inserire `IOptions<TOptions>` direttamente in una vista:

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name="in-memory-provider"></a>

## <a name="ioptionssnapshot"></a>IOptionsSnapshot

*Richiede ASP.NET Core 1.1 o versione successiva.*

`IOptionsSnapshot`supporta il ricaricamento di dati di configurazione quando viene modificato il file di configurazione. Ha un overhead minimo. Utilizzando `IOptionsSnapshot` con `reloadOnChange: true`, le opzioni sono associate a `IConfiguration` e ricaricati quando modificato.

Nell'esempio seguente viene illustrato come un nuovo `IOptionsSnapshot` viene creato dopo *config. JSON* le modifiche. Le richieste per il server restituirà lo stesso ora *config. JSON* è **non** modificato. La prima richiesta dopo *config. JSON* le modifiche verranno visualizzate una nuova ora.

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

La figura seguente mostra l'output del server:

![visualizzazione di immagini browser "ultimo aggiornamento: 22/11/2016 4:43 PM"](configuration/_static/first.png)

Aggiornamento del browser non modifica il valore del messaggio o l'ora visualizzata (quando *config. JSON* non è stato modificato).

Modificare e salvare il *config. JSON* e quindi aggiornare il browser:

![visualizzazione di immagini browser "ultimo aggiornamento a, e: 22/11/2016 4:53 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Provider in memoria e l'associazione a una classe POCO

L'esempio seguente viene illustrato come utilizzare il provider in memoria e associare a una classe:

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

I valori di configurazione vengono restituiti come stringhe, ma l'associazione consente la costruzione di oggetti. Associazione consente di recuperare gli oggetti POCO o addirittura interi oggetti grafici. Nell'esempio seguente viene illustrato come associare a `MyWindow` e utilizzare il modello di opzioni con un'applicazione ASP.NET MVC di base:

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

Associare la classe personalizzata in `ConfigureServices` durante la compilazione dell'host:

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

Visualizzare le impostazioni dal `HomeController`:

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a>GetValue

Nell'esempio seguente viene illustrato il [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) metodo di estensione:

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

Il ConfigurationBinder `GetValue<T>` metodo consente di specificare un valore predefinito (80 nell'esempio). `GetValue<T>`per scenari semplici e non viene associato a intere sezioni. `GetValue<T>`Ottiene i valori scalari `GetSection(key).Value` convertito in un tipo specifico.

## <a name="binding-to-an-object-graph"></a>Associazione a un oggetto grafico

È possibile associare in modo ricorsivo a ogni oggetto in una classe. Tenere presente quanto segue `AppOptions` classe:

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

Associa l'esempio seguente la `AppOptions` classe:

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** e versioni successive è possibile utilizzare `Get<T>`, che funziona con intere sezioni. `Get<T>`può essere più convienent rispetto all'utilizzo di `Bind`. Il codice seguente viene illustrato come utilizzare `Get<T>` con l'esempio precedente:

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

Con i seguenti *appSettings. JSON* file:

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

Il programma visualizza `Height 11`.

Il codice seguente consente di unit test della configurazione:

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

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a>Esempio di base di provider di Entity Framework

In questa sezione viene creato un provider di configurazione di base che legge le coppie nome-valore da un database tramite Entity Framework. 

Definire un `ConfigurationValue` entità per l'archiviazione dei valori di configurazione nel database:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Aggiungere un `ConfigurationContext` archiviare e accedere ai valori configurati:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Creare una classe che implementa [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Creare il provider di configurazione personalizzato ereditando dalla [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).  Il provider di configurazione consente di inizializzare il database quando è vuoto:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Quando si esegue l'esempio, vengono visualizzati i valori evidenziati dal database ("value_from_ef_1" e "value_from_ef_2").

È possibile aggiungere un `EFConfigSource` metodo di estensione per l'aggiunta dell'origine di configurazione:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Il codice seguente viene illustrato come utilizzare l'oggetto personalizzato `EFConfigProvider`:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Nota L'esempio aggiunge l'oggetto personalizzato `EFConfigProvider` quando il provider JSON, pertanto le impostazioni dal database sostituiranno le impostazioni dal *appSettings. JSON* file.

Con i seguenti *appSettings. JSON* file:

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

Verrà visualizzato quanto segue:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Provider di configurazione di riga di comando

Il [il provider di configurazione di riga di comando](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) riceve coppie chiave-valore di argomento della riga di comando per la configurazione in fase di esecuzione.

[Consente di visualizzare o scaricare l'esempio di configurazione della riga di comando](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a>Il provider di configurazione

# <a name="basic-configurationtabbasicconfiguration"></a>[Configurazione di base](#tab/basicconfiguration)

Per attivare la configurazione della riga di comando, chiamare il `AddCommandLine` il metodo di estensione in un'istanza di [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

Esecuzione del codice, viene visualizzato il seguente output:

```console
MachineName: MairaPC
Left: 1980
```

Il passaggio di coppie chiave-valore argomento nella riga di comando di modifica i valori di `Profile:MachineName` e `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

Consente di visualizzare la finestra della console:

```console
MachineName: BartPC
Left: 1979
```

Per eseguire l'override di configurazione fornita da altri provider di configurazione con la configurazione della riga di comando, chiamare `AddCommandLine` l'ultimo `ConfigurationBuilder`:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

App di 2. x di ASP.NET Core tipico utilizzare il metodo pratici statici `CreateDefaultBuilder` per compilare l'host:

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

`CreateDefaultBuilder`Carica la configurazione facoltativa da *appSettings. JSON*, *appsettings. { Ambiente} JSON*, [informazioni riservate dell'utente](xref:security/app-secrets) (nel `Development` ambiente), le variabili di ambiente e gli argomenti della riga di comando. Il provider di configurazione della riga di comando viene chiamato per ultimo. Chiamare il provider di ultima consente gli argomenti della riga di comando passati in fase di esecuzione per eseguire l'override di configurazione impostata da altri provider di configurazione è stato chiamato in precedenza.

Si noti che per *appsettings* file `reloadOnChange` è abilitata. Argomenti della riga di comando vengono ignorati se un valore di configurazione corrispondente in un *appsettings* file è stato modificato dopo l'avvio dell'applicazione.

> [!NOTE]
> Come alternativa all'utilizzo di `CreateDefaultBuilder` (metodo), la creazione di un host utilizzando [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) e creare manualmente la configurazione con [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) è supportato in ASP.NET Core 2. x. Vedere la scheda di 1. x di ASP.NET Core per altre informazioni.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Creare un [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) e chiamare il `AddCommandLine` metodo da utilizzare il provider di configurazione della riga di comando. Chiamare il provider di ultima consente gli argomenti della riga di comando passati in fase di esecuzione per eseguire l'override di configurazione impostata da altri provider di configurazione è stato chiamato in precedenza. Applicare la configurazione per [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) con il `UseConfiguration` metodo:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Argomenti

Gli argomenti passati alla riga di comando devono essere conforme a uno dei due formati indicati nella tabella seguente.

| Formato dell'argomento                                                     | Esempio        |
| ------------------------------------------------------------------- | :------------: |
| Singolo argomento: una coppia chiave-valore separate da un segno di uguale (`=`) | `key1=value`   |
| Sequenza di due argomenti: una coppia chiave-valore separate da uno spazio    | `/key1 value1` |

**Singolo argomento**

Il valore deve seguire un segno di uguale (`=`). Il valore può essere null (ad esempio, `mykey=`).

La chiave potrebbe essere un prefisso.

| Prefisso chiave               | Esempio         |
| ------------------------ | :-------------: |
| Nessun prefisso                | `key1=value1`   |
| Singolo dash (`-`) &#8224; | `-key2=value2`  |
| Due trattini (`--`)        | `--key3=value3` |
| Barra (`/`)      | `/key4=value4`  |

&#8224; Una chiave con un prefisso singolo dash (`-`) deve essere specificato in [passare mapping](#switch-mappings), descritto di seguito.

Comando di esempio:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Nota: Se `-key1` non è presente nel [passare mapping](#switch-mappings) fornito al provider di configurazione, un `FormatException` viene generata un'eccezione.

**Sequenza di due argomenti**

Il valore non può essere null e deve seguire la chiave separata da uno spazio.

La chiave deve essere un prefisso.

| Prefisso chiave               | Esempio         |
| ------------------------ | :-------------: |
| Singolo dash (`-`) &#8224; | `-key1 value1`  |
| Due trattini (`--`)        | `--key2 value2` |
| Barra (`/`)      | `/key3 value3`  |

&#8224; Una chiave con un prefisso singolo dash (`-`) deve essere specificato in [passare mapping](#switch-mappings), descritto di seguito.

Comando di esempio:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Nota: Se `-key1` non è presente nel [passare mapping](#switch-mappings) fornito al provider di configurazione, un `FormatException` viene generata un'eccezione.

### <a name="duplicate-keys"></a>Chiavi duplicate

Se le chiavi duplicate, viene utilizzata l'ultima coppia chiave-valore.

### <a name="switch-mappings"></a>Mapping di commutatore

Quando si compila manualmente configurazione con `ConfigurationBuilder`, è possibile fornire facoltativamente un dizionario di mapping del commutatore per il `AddCommandLine` metodo. Mapping di commutatore consentono di fornire la logica di sostituzione nome della chiave.

Quando viene utilizzato il dizionario di mapping di parametro, il dizionario viene controllato per una chiave corrispondente alla chiave fornita da un argomento della riga di comando. Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la configurazione. Un mapping del commutatore è necessario per una chiave della riga di comando, preceduta da un trattino singolo (`-`).

L'opzione regole di chiave del dizionario di mapping:

* Switch devono iniziare con un trattino (`-`) o doppio-trattino (`--`).
* Il dizionario di mapping commutatore non deve contenere chiavi duplicate.

Nell'esempio seguente, il `GetSwitchMappings` metodo consente gli argomenti della riga di comando usare un singolo trattino (`-`) chiave prefisso ed evitare i prefissi sottochiavi iniziali.

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

Senza fornire argomenti della riga di comando, il dizionario fornito per `AddInMemoryCollection` imposta i valori di configurazione. Eseguire l'app con il comando seguente:

```console
dotnet run
```

Consente di visualizzare la finestra della console:

```console
MachineName: RickPC
Left: 1980
```

Per passare le impostazioni di configurazione, usare la seguente:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

Consente di visualizzare la finestra della console:

```console
MachineName: DahliaPC
Left: 1984
```

Dopo aver creato il dizionario di mapping di commutatore, contiene i dati visualizzati nella tabella seguente.

| Chiave            | Valore                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Per dimostrare la commutazione chiave utilizzando il dizionario, eseguire il comando seguente:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Le chiavi della riga di comando vengono scambiate. Nella finestra della console vengono visualizzati i valori di configurazione per `Profile:MachineName` e `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>Il file Web. config

Oggetto *Web. config* file è obbligatorio quando si ospita l'applicazione in IIS o IIS Express. *Web. config* attiva AspNetCoreModule in IIS per avviare l'app. Le impostazioni in *Web. config* abilitare AspNetCoreModule in IIS per avviare l'app e configurare altre impostazioni di IIS e i moduli. Se si utilizza Visual Studio ed Elimina *Web. config*, Visual Studio creerà uno nuovo.

## <a name="additional-notes"></a>Note aggiuntive

* Dependency Injection (DI) non viene impostato fino a dopo `ConfigureServices` viene richiamato.
* Il sistema di configurazione non è in grado di riconoscere SEN.
* `IConfiguration`presenta due specializzazioni:
  * `IConfigurationRoot`Utilizzato per il nodo radice. Può attivare un ricaricamento.
  * `IConfigurationSection`Rappresenta una sezione dei valori di configurazione. Il `GetSection` e `GetChildren` i metodi restituiscono un `IConfigurationSection`.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Uso di più ambienti](environments.md)
* [Archiviazione sicura di segreti dell'app durante lo sviluppo](../security/app-secrets.md)
* [Hosting in ASP.NET Core](xref:fundamentals/hosting)
* [Inserimento dipendenze](dependency-injection.md)
* [Azure Key Vault configuration provider](xref:security/key-vault-configuration) (Provider di configurazione di Azure Key Vault)
