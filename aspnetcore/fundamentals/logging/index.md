---
title: Registrazione in ASP.NET Core
author: ardalis
description: Informazioni sul framework di registrazione di ASP.NET Core. Informazioni sui provider di registrazione predefiniti e sui provider di terze parti più diffusi.
ms.author: tdykstra
ms.date: 07/24/2018
uid: fundamentals/logging/index
ms.openlocfilehash: f629b062afb5c17cd05040a9ef0281aa7121aabc
ms.sourcegitcommit: 516d0645c35ea784a3ae807be087ae70446a46ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320752"
---
# <a name="logging-in-aspnet-core"></a>Registrazione in ASP.NET Core

Di [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione. I provider predefiniti consentono di inviare i log a una o più destinazioni. È possibile collegare un framework di registrazione di terze parti. Questo articolo illustra come usare l'API e i provider di registrazione incorporati nel codice.

::: moniker range=">= aspnetcore-2.0"

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

Per informazioni sulla registrazione stdout per l'hosting con IIS, vedere <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>. Per informazioni sulla registrazione stdout con Servizio app di Azure, vedere <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

## <a name="how-to-create-logs"></a>Come creare log

Per creare i log, implementare un oggetto [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) dal contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection):

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Quindi chiamare i metodi di registrazione su tale oggetto logger:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Questo esempio crea log con la classe `TodoController` come *categoria*. Le categorie sono descritte [più avanti in questo articolo](#log-category).

ASP.NET Core non fornisce metodi di logger asincroni perché la registrazione deve essere così rapida che non vale la pena di usare un metodo asincrono. Se la situazione richiede l'uso di metodi asincroni, considerare l'ipotesi di cambiare modalità di registrazione. Se l'archivio dati è lento, scrivere i messaggi di log prima in un archivio veloce, quindi spostarli in un archivio lento in un secondo momento. Registrare ad esempio in una coda di messaggi che viene letta e salvata in modo permanente in un archivio lento da un altro processo.

## <a name="how-to-add-providers"></a>Come aggiungere provider

::: moniker range=">= aspnetcore-2.0"

Un provider di registrazione recupera i messaggi creati dall'utente con un oggetto `ILogger` e li visualizza o li archivia. Il provider Console, ad esempio, visualizza i messaggi nella console e il provider del Servizio app di Azure può archiviarli nell'archivio BLOB di Azure.

Per usare un provider, chiamare il metodo di estensione `Add<ProviderName>` del provider in *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Il modello di progetto predefinito abilita i provider di registrazione Console e Debug con una chiamata al metodo di estensione [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) in *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Un provider di registrazione recupera i messaggi creati dall'utente con un oggetto `ILogger` e li visualizza o li archivia. Il provider Console, ad esempio, visualizza i messaggi nella console e il provider del Servizio app di Azure può archiviarli nell'archivio BLOB di Azure.

Per usare un provider, installare il relativo pacchetto NuGet e chiamare il metodo di estensione del provider in un'istanza di [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), come illustrato nell'esempio seguente:

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

L'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) di ASP.NET Core fornisce l'istanza di `ILoggerFactory`. I metodi di estensione `AddConsole` e `AddDebug` sono definiti nei pacchetti [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/). Ogni metodo di estensione chiama il metodo `ILoggerFactory.AddProvider` passando un'istanza del provider.

> [!NOTE]
> L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) aggiunge provider di log nel metodo `Startup.Configure`. Per ottenere l'output del log dal codice eseguito in precedenza, aggiungere i provider di log nel costruttore della classe `Startup`.

::: moniker-end

Scoprire di più sui [provider di registrazione predefiniti](#built-in-logging-providers) e vedere i collegamenti ai [provider di registrazione di terze parti](#third-party-logging-providers) più avanti in questo articolo.

## <a name="configuration"></a>Configurazione

La configurazione dei provider di registrazione viene fornita da uno o più provider di configurazione:

* Formati di file (INI, JSON e XML).
* Argomenti della riga di comando.
* Variabili di ambiente.
* Oggetti .NET in memoria.
* Archiviazione con [Secret Manager](xref:security/app-secrets) senza crittografia.
* Un archivio utente non crittografato, come [Azure Key Vault](xref:security/key-vault-configuration).
* Provider personalizzati (installati o creati).

Ad esempio, la configurazione di registrazione viene comunemente fornita dalla sezione `Logging` del file di impostazioni dell'app. L'esempio seguente mostra il contenuto di un file *appsettings.Development.json* tipico:

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": "true"
    }
  }
}
```

Le chiavi `LogLevel` rappresentano i nomi dei log. La chiave `Default` si applica ai log non esplicitamente elencati. Il valore rappresenta il [livello del log](#log-level) applicato al log specificato. Le chiavi dei log che impostano `IncludeScopes` (`Console` nell'esempio) specificano se gli [ambiti dei log](#log-scopes) sono abilitati per il log indicato.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

Le chiavi `LogLevel` rappresentano i nomi dei log. La chiave `Default` si applica ai log non esplicitamente elencati. Il valore rappresenta il [livello del log](#log-level) applicato al log specificato.

::: moniker-end

Per informazioni sull'implementazione dei provider di configurazione, vedere <xref:fundamentals/configuration/index>.

## <a name="sample-logging-output"></a>Esempio di output di registrazione

Con il codice di esempio illustrato nella sezione precedente si visualizzeranno i log nella console durante l'esecuzione dalla riga di comando. Ecco un esempio di output della console:

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

Questi log sono stati creati in `http://localhost:5000/api/todo/0`, attivando l'esecuzione di entrambe le chiamate a `ILogger` illustrate nella sezione precedente.

Ecco un esempio degli stessi log nella finestra Debug quando si esegue l'applicazione di esempio in Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

I log che sono stati creati con le chiamate a `ILogger` illustrate nella sezione precedente iniziano con "TodoApi.Controllers.TodoController". I log che iniziano con le categorie "Microsoft" sono di ASP.NET Core. ASP.NET Core stesso e il codice dell'applicazione usano la stessa API di registrazione e gli stessi provider di registrazione.

Il resto di questo articolo spiega alcuni dettagli e le opzioni per la registrazione.

## <a name="nuget-packages"></a>Pacchetti NuGet

Le interfacce `ILogger` e `ILoggerFactory` si trovano in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) e le loro implementazioni predefinite si trovano in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Categoria dei log

Ogni log creato include una *categoria*. La categoria viene specificata quando si crea un oggetto `ILogger`. La categoria può essere qualsiasi stringa, ma una convenzione prevede l'uso del nome completo della classe da cui vengono scritti i log. Ad esempio: "TodoApi.Controllers.TodoController".

È possibile specificare la categoria sotto forma di stringa o usare un metodo di estensione che deriva la categoria dal tipo. Per specificare la categoria sotto forma di stringa, chiamare `CreateLogger` su un'istanza di `ILoggerFactory`, come illustrato di seguito.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

In genere è più semplice usare `ILogger<T>`, come nell'esempio seguente.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Ciò equivale a chiamare `CreateLogger` con il nome completo del tipo `T`.

## <a name="log-level"></a>Livello di registrazione

Ogni volta che si scrive un log, si specifica il relativo [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel). Il livello di registrazione indica il livello di gravità o priorità. È ad esempio possibile scrivere un log `Information` quando un metodo termina normalmente, un log `Warning` quando un metodo restituisce un codice di errore 404 e un log `Error` quando viene rilevata un'eccezione imprevista.

Nell'esempio di codice seguente i nomi dei metodi (ad esempio `LogWarning`) specificano il livello di registrazione. Il primo parametro è l'[ID dell'evento di registrazione](#log-event-id). Il secondo parametro è un [modello di messaggio](#log-message-template) con segnaposto per i valori degli argomenti forniti dai rimanenti parametri del metodo. I parametri del metodo sono descritti in maggiore dettaglio in questo articolo.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

I metodi di registrazione che includono il livello nel nome del metodo sono i [metodi di estensione di ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions). Questi metodi chiamano in background un metodo `Log` che accetta un parametro `LogLevel`. È possibile chiamare il metodo `Log` direttamente anziché chiamare uno di questi metodi di estensione, ma la sintassi è relativamente complessa. Per altre informazioni, vedere l'[interfaccia ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) e il [codice sorgente delle estensioni del logger](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core definisce i seguenti [livelli di registrazione](/dotnet/api/microsoft.extensions.logging.loglevel), ordinati di seguito dal meno grave al più grave.

* Trace = 0

  Per informazioni utili solo per uno sviluppatore che esegue il debug di un problema. Questi messaggi possono contenere dati sensibili dell'applicazione e pertanto non devono essere abilitati in un ambiente di produzione. *Disattivato per impostazione predefinita.* Esempio: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Debug = 1

  Per informazioni con utilità a breve termine durante lo sviluppo e il debug. Esempio: `Entering method Configure with flag set to true.` In genere non si abilitano i livelli di registrazione `Debug` nell'ambiente di produzione, a meno che non occorra risolvere problemi, per l'elevato volume delle registrazioni.

* Information = 2

  Per tenere traccia del flusso generale dell'applicazione. Queste registrazioni hanno in genere un valore a lungo termine. Esempio: `Request received for path /api/todo`

* Warning = 3

  Per gli eventi imprevisti o anomali del flusso dell'applicazione. Potrebbero includere errori o altre condizioni che non provocano l'arresto dell'applicazione, ma che potrebbe essere necessario analizzare. Le eccezioni gestite sono una situazione comune in cui usare il livello di registrazione `Warning`. Esempio: `FileNotFoundException for file quotes.txt.`

* Error = 4

  Per errori ed eccezioni che non possono essere gestiti. Questi messaggi indicano un errore nell'operazione (ad esempio la richiesta HTTP corrente) o nell'attività corrente, non un errore a livello di applicazione. Messaggio di registrazione di esempio:`Cannot insert record due to duplicate key violation.`

* Critical = 5

  Per gli errori che richiedono attenzione immediata. Esempi: scenari di perdita di dati, spazio su disco insufficiente.

È possibile usare il livello di registrazione per controllare la quantità di output scritto su un supporto di archiviazione specifico o in una finestra. Ad esempio, nell'ambiente di produzione è consigliabile che tutti i log di livello `Information` e inferiore passino a un archivio dati di volume e che tutti i log di livello `Warning` e superiore passino a un archivio dati di valori. Durante lo sviluppo, in genere è possibile inviare i log di livello `Warning` o con gravità superiore alla console. Per la risoluzione dei problemi, è possibile aggiungere il livello `Debug`. La sezione [Filtro dei log](#log-filtering) più avanti in questo articolo descrive come controllare quali livelli di registrazione gestisce un provider.

Il framework di ASP.NET Core scrive log di livello `Debug` per gli eventi del framework. Gli esempi visti in precedenza in questo articolo escludono i log al di sotto del livello `Information`, quindi non compare alcun log di livello `Debug`. Ecco un esempio di log della console se si esegue l'applicazione di esempio configurata per visualizzare i log di livello `Debug` e superiore per il provider della console.

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>ID evento di registrazione

Ogni volta che si scrive un log, è possibile specificare il relativo *ID evento*. L'app di esempio esegue questa operazione tramite una classe `LoggingEvents` definita a livello locale:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Un ID evento è un valore intero che può essere usato per associare tra loro un set di eventi registrati. Ad esempio, la registrazione dell'aggiunta di un elemento al carrello potrebbe avere ID evento 1000 e la registrazione per il completamento di un acquisto potrebbe avere ID evento 1001.

Nella registrazione dell'output, l'ID evento può essere archiviato in un campo o incluso nel messaggio di testo, a seconda del provider. Il provider Debug non visualizza gli ID evento, mentre il provider Console li visualizza tra parentesi quadre dopo la categoria:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Modello di messaggio di registrazione

Ogni volta che si scrive un messaggio di registrazione, si fornisce un modello di messaggio. Il modello di messaggio può essere una stringa o può contenere segnaposto denominati in cui vengono inseriti i valori degli argomenti. Il modello non è una stringa formattata e i segnaposto devono essere denominati, non numerati.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

L'ordine dei segnaposto, non il loro nome, determina i parametri da usare per fornire i valori corrispondenti. Se si ha il codice seguente:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Il messaggio di registrazione risultante è simile a:

```
Parameter values: parm1, parm2
```

Il framework di registrazione formatta i messaggi in questo modo per consentire ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Poiché gli argomenti stessi vengono passati al sistema di registrazione, non solo il modello di messaggio formattato, i provider di registrazione possono archiviare i valori dei parametri come campi oltre al modello di messaggio. Se si indirizza l'output del log all'archiviazione tabelle di Azure e il metodo logger ha l'aspetto seguente:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Ogni entità della tabella di Azure può avere le proprietà `ID` e `RequestTime`, il che semplifica le query sui dati del log. È possibile trovare tutti i log entro un determinato intervallo `RequestTime` senza che sia necessario analizzare il tempo fuori dal messaggio di testo.

## <a name="logging-exceptions"></a>Registrazione delle eccezioni

I metodi logger dispongono di overload che consentono di passare un'eccezione, come nell'esempio seguente:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Provider diversi gestiscono le informazioni sulle eccezioni in modi diversi. Ecco un esempio di output del provider Debug dal codice sopra riportato.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtro dei log

::: moniker range=">= aspnetcore-2.0"

È possibile specificare un livello di registrazione minimo per un provider e una categoria specifici oppure per tutti i provider o tutte le categorie. Tutti i log sotto il livello minimo non sono passati al provider, quindi non vengono visualizzati o archiviati.

Se si vuole eliminare tutti i log, è possibile specificare `LogLevel.None` come livello di registrazione minimo. Il valore intero di `LogLevel.None` è 6, maggiore di `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Creare regole di filtro nella configurazione

I modelli di progetto creano codice che chiama `CreateDefaultBuilder` per impostare la registrazione per i provider Console e Debug. Il metodo `CreateDefaultBuilder` imposta inoltre la registrazione per la ricerca della configurazione di una sezione `Logging` tramite codice simile al seguente:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

I dati di configurazione specificano i livelli di registrazione minimi in base al provider e alla categoria, come nell'esempio seguente:

[!code-json[](index/sample2/appsettings.json)]

Questo codice JSON crea sei regole di filtro, una per il provider Debug, quattro per il provider Console e una che si applica a tutti i provider. Più avanti si vedrà come si sceglie una sola di queste regole per ogni provider quando si crea un oggetto `ILogger`.

### <a name="filter-rules-in-code"></a>Regole di filtro nel codice

È possibile registrare le regole di filtro nel codice, come illustrato nell'esempio seguente:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Il secondo `AddFilter` specifica il provider Debug tramite il nome del tipo. Il primo `AddFilter` si applica a tutti i provider in quanto non consente di specificare un tipo di provider.

### <a name="how-filtering-rules-are-applied"></a>Applicazione delle regole di filtro

I dati di configurazione e il codice `AddFilter` illustrato negli esempi precedenti creano le regole indicate nella tabella seguente. I primi sei provengono dall'esempio di configurazione e gli ultimi due dall'esempio di codice.

| Number | Provider      | Categorie che iniziano con...          | Livello di registrazione minimo |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Debug         | Tutte le categorie                          | Informazioni       |
| 2      | Console       | Microsoft.AspNetCore.Mvc.Razor.Internal | Avviso           |
| 3      | Console       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Debug             |
| 4      | Console       | Microsoft.AspNetCore.Mvc.Razor          | Error             |
| 5      | Console       | Tutte le categorie                          | Informazioni       |
| 6      | Tutti i provider | Tutte le categorie                          | Debug             |
| 7      | Tutti i provider | System                                  | Debug             |
| 8      | Debug         | Microsoft                               | Traccia             |

Quando si crea un oggetto `ILogger` con cui scrivere i log, l'oggetto `ILoggerFactory` seleziona una singola regola per ogni provider da applicare al logger. Tutti i messaggi scritti da tale oggetto `ILogger` sono filtrati in base alle regole selezionate. Tra le regole disponibili viene selezionata la regola più specifica possibile per ogni coppia di categoria e provider.

L'algoritmo seguente viene usato per ogni provider quando viene creato un `ILogger` per una determinata categoria:

* Selezionare tutte le regole corrispondenti al provider o al relativo alias. Se non ne vengono rilevate, selezionare tutte le regole con un provider vuoto.
* Dal risultato del passaggio precedente, selezionare le regole con il prefisso di categoria corrispondente più lungo. Se non vengono rilevate, selezionare tutte le regole che non specificano una categoria.
* Se sono selezionate più regole, scegliere l'**ultima**.
* Se non è selezionata alcuna regola, usare `MinimumLevel`.

Si supponga ad esempio di avere l'elenco di regole precedente e di creare un oggetto `ILogger` per la categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* Per il provider Debug si applicano le regole 1, 6 e 8. La regola 8 è più specifica, così sarà quella selezionata.
* Per il provider Console si applicano le regole 3, 4, 5 e 6. La regola 3 è la più specifica.

Quando si creano log con un `ILogger` per la categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", i log di livello `Trace` e superiore andranno al provider Debug e i log di livello `Debug` e superiore andranno al provider Console.

### <a name="provider-aliases"></a>Alias dei provider

È possibile usare il nome del tipo per specificare un provider nella configurazione, ma ogni provider definisce un *alias* più breve che è più facile da usare. Per i provider predefiniti, usare gli alias seguenti:

* Console
* Debug
* EventLog
* AzureAppServices
* TraceSource
* EventSource

### <a name="default-minimum-level"></a>Livello minimo predefinito

Esiste un'impostazione del livello minimo che diventa effettiva solo se non si applica alcuna regola di configurazione o codice per un provider e una categoria specifici. L'esempio seguente illustra come impostare il livello minimo:

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Se non si imposta in modo esplicito il livello minimo, il valore predefinito è `Information`, che indica che i log `Trace` e `Debug` vengono ignorati.

### <a name="filter-functions"></a>Funzioni di filtro

È possibile scrivere codice in una funzione di filtro per applicare le regole di filtro. Una funzione di filtro viene richiamata per tutti i provider e le categorie a cui non sono assegnate regole tramite la configurazione o il codice. Il codice della funzione ha accesso al tipo di provider, alla categoria e al livello di registrazione per stabilire se un messaggio deve essere registrato. Ad esempio:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Alcuni provider di registrazione consentono di specificare quando i log devono essere scritti in un supporto di archiviazione oppure ignorati in base al livello di registrazione e alla categoria.

I metodi di estensione `AddConsole` e `AddDebug` forniscono overload che consentono di passare criteri di filtro. L'esempio di codice seguente fa sì che il provider Console ignori i log di livello inferiore a `Warning`, mentre il provider Debug ignora i log creati dal framework.

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

Il metodo `AddEventLog` ha un overload che accetta un'istanza di `EventLogSettings`, che può contenere una funzione di filtro nella proprietà `Filter`. Il provider TraceSource non fornisce alcun overload di questo tipo, in quanto il suo livello di registrazione e altri parametri si basano sugli oggetti `SourceSwitch` e `TraceListener` in uso.

È possibile impostare le regole di filtro per tutti i provider registrati con un'istanza di `ILoggerFactory` tramite il metodo di estensione `WithFilter`. L'esempio seguente limita i log del framework (la categoria inizia con "Microsoft" o "System") agli avvisi consentendo all'app di registrare a livello debug.

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Per usare i filtri per impedire la scrittura di tutti i log per una determinata categoria, è possibile specificare `LogLevel.None` come il livello di registrazione minimo per tale categoria. Il valore intero di `LogLevel.None` è 6, maggiore di `LogLevel.Critical` (5).

Il metodo di estensione `WithFilter` viene fornito dal pacchetto NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter). Il metodo restituisce una nuova istanza di `ILoggerFactory` che filtra i messaggi di registrazione passati a tutti i provider di logger registrati con esso. Non si applica ad altre istanze di `ILoggerFactory`, inclusa l'istanza originale di `ILoggerFactory`.

::: moniker-end

## <a name="log-scopes"></a>Ambiti dei log

È possibile raggruppare un set di operazioni logiche all'interno di un *ambito* per collegare gli stessi dati a ogni log creato come parte del set. È ad esempio possibile fare in modo che ogni log creato come parte dell'elaborazione di una transazione includa l'ID della transazione.

Un ambito è un tipo `IDisposable` restituito dal metodo [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) e viene mantenuto fino all'eliminazione. Un ambito si usa mediante il wrapping delle chiamate del logger in un blocco `using`, come illustrato di seguito:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Il codice seguente abilita gli ambiti per il provider Console:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> La configurazione dell'opzione del logger della console `IncludeScopes` è necessaria per abilitare la registrazione basata sull'ambito.
>
> Per informazioni sulla configurazione, vedere la sezione [Configurazione](#Configuration).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> La configurazione dell'opzione del logger della console `IncludeScopes` è necessaria per abilitare la registrazione basata sull'ambito.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

Ogni messaggio di registrazione include le informazioni sull'ambito:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Provider di registrazione predefiniti

ASP.NET Core include i provider seguenti:

* [Console](#console-provider)
* [Debug](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [Servizio app di Azure](#azure-app-service-provider)

### <a name="console-provider"></a>Provider Console

Il pacchetto del provider [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) invia l'output della registrazione alla console.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

[Gli overload di AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) consentono di passare un livello di registrazione minimo, una funzione di filtro e un valore booleano che indica se gli ambiti sono supportati. Un'altra opzione consiste nel passare un oggetto `IConfiguration`, che può specificare livelli di registrazione e il supporto di ambiti.

Se si intende usare il provider di console nell'ambiente di produzione, tenere presente che ha un impatto significativo sulle prestazioni.

Quando si crea un nuovo progetto in Visual Studio, il metodo `AddConsole` è simile al seguente:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Questo codice fa riferimento alla sezione `Logging` del file *appSettings.json*:

[!code-json[](index/sample//appsettings.json)]

Le impostazioni visualizzate limitano i log del framework agli avvisi, consentendo all'app di registrare a livello di debug, come descritto nella sezione [Filtri dei log](#log-filtering). Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).

::: moniker-end

### <a name="debug-provider"></a>Provider Debug

Il pacchetto del provider [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) scrive l'output della registrazione usando la classe [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (chiamate del metodo `Debug.WriteLine`).

In Linux, questo provider scrive i log in */var/log/message*.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

Gli [overload di AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) consentono passare un livello di registrazione minimo o una funzione di filtro.

::: moniker-end

### <a name="eventsource-provider"></a>Provider EventSource

Per le app destinate a ASP.NET Core 1.1.0 o versione successiva, il pacchetto di provider [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) può implementare la traccia degli eventi. In Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Il provider è multipiattaforma, ma non sono ancora disponibili gli strumenti di raccolta e visualizzazione degli eventi per Linux o macOS.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

Un buon metodo per raccogliere e visualizzare i log consiste nell'usare l'[utilità PerfView](https://github.com/Microsoft/perfview). Sono disponibili altri strumenti per la visualizzazione dei log ETW, ma PerfView fornisce un'esperienza ottimale per l'uso con gli eventi ETW generati da ASP.NET.

Per configurare PerfView per la raccolta degli eventi registrati da questo provider, aggiungere la stringa `*Microsoft-Extensions-Logging` nell'elenco **Provider aggiuntivi** (non dimenticare l'asterisco all'inizio della stringa).

![Provider aggiuntivi di PerfView](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Provider EventLog di Windows

Il pacchetto di provider [Microsoft.Extensions.Logging.AventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) invia l'output del log al registro eventi di Windows.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

Gli [overload di AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) consentono di passare `EventLogSettings` o un livello di registrazione minimo.

::: moniker-end

### <a name="tracesource-provider"></a>Provider TraceSource

Il pacchetto di provider [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa le librerie e i provider di [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

Gli [overload di AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) consentono di passare un cambio di origine e un listener di traccia.

Per usare questo provider, un'applicazione deve essere eseguita su di .NET Framework (anziché su .NET Core). Il provider consente di instradare i messaggi a vari [listener](/dotnet/framework/debug-trace-profile/trace-listeners), come il listener [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) usato nell'applicazione di esempio.

L'esempio seguente configura un provider `TraceSource` che registra messaggi `Warning` e di livello superiore nella finestra della console.

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a>Provider del Servizio app di Azure

Il pacchetto di provider [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) scrive i log in file di testo nel file system di un'app del Servizio app di Azure e nell'[archivio di BLOB](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in un account di archiviazione di Azure. Il pacchetto di provider è disponibile per le app destinate a .NET Core 1.1 o versione successiva.

::: moniker range=">= aspnetcore-2.0"

Se la destinazione è .NET Core, tenere presente quanto segue:

* Il pacchetto del provider è incluso nel [metapacchetto Microsoft.AspNetCore.All ](xref:fundamentals/metapackage) ASP.NET Core ma non nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
* Non chiamare in modo esplicito [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics). Il provider diventa automaticamente disponibile per l'app quando l'app viene distribuita nel Servizio app di Azure.

Se la destinazione è .NET Framework o il riferimento è il metapacchetto `Microsoft.AspNetCore.App`, aggiungere il pacchetto di provider al progetto. Richiamare `AddAzureWebAppDiagnostics` in un'istanza [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory):

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

Un overload di [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) consente di passare [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) con cui è possibile eseguire l'override delle impostazioni predefinite, ad esempio il modello di output del log, il nome di BLOB e il limite delle dimensioni del file. (Il *modello di output* è un modello di messaggio che viene applicato a tutti i log oltre quello specificato dall'utente quando si chiama un metodo `ILogger`.)

Quando si distribuisce un'app del Servizio app, l'app rispetta le impostazioni della sezione dei [log di diagnostica](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) dalla pagina del **Servizio app** del portale di Azure. Quando queste impostazioni vengono aggiornate, le modifiche hanno effetto immediato senza richiedere un riavvio o la ridistribuzione dell'app.

![Impostazioni di registrazione di Azure](index/_static/azure-logging-settings.png)

Il percorso predefinito per i file di log è nella cartella *D:\\home\\LogFiles\\Application* e il nome di file predefinito è *diagnostics-aaaammgg.txt*. Il limite predefinito per le dimensioni del file è 10 MB e il numero massimo predefinito di file conservati è 2. Il nome BLOB predefinito è *{nome-app}{timestamp}/aaaa/mm/gg/hh/{guid}-applicationLog.txt*. Per altre informazioni sul comportamento predefinito, vedere [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).

Il provider funziona solo quando il progetto viene eseguito nell'ambiente di Azure. Non ha alcun effetto quando il progetto viene eseguito localmente, in quanto non scrive nulla in file locali o in archivi di sviluppo locali per i BLOB.

## <a name="third-party-logging-providers"></a>Provider di registrazione di terze parti

Framework di registrazione di terze parti che usano ASP.NET Core:

* [elmah.io](https://elmah.io/) ([repository GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repository GitHub](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([repository GitHub](https://github.com/mperdeck/jsnlog))
* [Loggr](http://loggr.net/) ([repository GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](http://nlog-project.org/) ([repository GitHub](https://github.com/NLog/NLog.Extensions.Logging))
* [Serilog](https://serilog.net/) ([repository GitHub](https://github.com/serilog/serilog-extensions-logging))

Alcuni framework di terze parti possono eseguire la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

L'uso di un framework di terze parti è simile a quello di uno dei provider predefiniti:

1. Aggiungere un pacchetto NuGet al progetto.
1. Chiamare un metodo di estensione in `ILoggerFactory`.

Per altre informazioni, vedere la documentazione di ciascun framework.

## <a name="azure-log-streaming"></a>Flusso di registrazione di Azure

Il flusso di registrazione di Azure consente di visualizzare l'attività di registrazione in tempo reale da:

* Server applicazioni
* Server Web
* Traccia delle richieste non riuscite

Per configurare il flusso di registrazione di Azure:

* Passare alla pagina **Log di diagnostica** dalla pagina del portale dell'applicazione
* Attivare **Registrazione applicazioni (file system)**.

![Pagina dei log di diagnostica del portale di Azure](index/_static/azure-diagnostic-logs.png)

Passare alla pagina **Flusso di registrazione** per visualizzare i messaggi dell'applicazione. Vengono registrati dall'applicazione tramite l'interfaccia `ILogger`.

![Flusso di registrazione dell'applicazione nel portale di Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a>Registrazione di traccia di Azure Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) SDK raccoglie la telemetria di traccia dei registri generati con l'infrastruttura di registrazione di ASP.NET Core. Per altre informazioni, vedere [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging) (Wiki di Microsoft/ApplicationInsights-aspnetcore: registrazione).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/logging/loggermessage>
