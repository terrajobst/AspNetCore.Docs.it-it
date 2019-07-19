---
title: Registrazione in ASP.NET Core
author: tdykstra
description: Informazioni sul framework di registrazione di ASP.NET Core. Informazioni sui provider di registrazione predefiniti e sui provider di terze parti più diffusi.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/11/2019
uid: fundamentals/logging/index
ms.openlocfilehash: cdb5cad2302cc8ec02107021005b8590efce7533
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308211"
---
# <a name="logging-in-aspnet-core"></a>Registrazione in ASP.NET Core

Di [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione predefiniti e di terze parti. Questo articolo illustra come usare l'API di registrazione con i provider predefiniti.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="add-providers"></a>Aggiungere provider

Un provider di registrazione visualizza o archivia i log. Il provider Console, ad esempio, visualizza i log nella console, mentre il provider Azure Application Insights li archivia in Azure Application Insights. I log possono essere inviati a più destinazioni aggiungendo più provider.

::: moniker range=">= aspnetcore-2.0"

Per aggiungere un provider, chiamare il metodo di estensione `Add{provider name}` del provider in *Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

Il codice precedente richiede riferimenti a `Microsoft.Extensions.Logging` e `Microsoft.Extensions.Configuration`.

Il modello di progetto predefinito chiama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, che aggiunge i provider di registrazione seguenti:

* Console
* Debug
* EventSource (a partire da ASP.NET Core 2.2)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

Se si usa `CreateDefaultBuilder`, è possibile sostituire i provider predefiniti con quelli di propria scelta. Chiamare <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> e aggiungere i provider desiderati.

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Per usare un provider, installare il relativo pacchetto NuGet e chiamare il metodo di estensione del provider in un'istanza di <xref:Microsoft.Extensions.Logging.ILoggerFactory>:

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

L'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) di ASP.NET Core fornisce l'istanza di `ILoggerFactory`. I metodi di estensione `AddConsole` e `AddDebug` sono definiti nei pacchetti [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/). Ogni metodo di estensione chiama il metodo `ILoggerFactory.AddProvider` passando un'istanza del provider.

> [!NOTE]
> L'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) aggiunge provider di log nel metodo `Startup.Configure`. Per ottenere l'output di log dal codice eseguito in precedenza, aggiungere i provider di registrazione nel costruttore della classe `Startup`.

::: moniker-end

Altre informazioni sui [provider di registrazione predefiniti](#built-in-logging-providers) e sui [provider di registrazione di terze parti](#third-party-logging-providers) vengono fornite più avanti in questo articolo.

## <a name="create-logs"></a>Creare log

Ottenere un oggetto <xref:Microsoft.Extensions.Logging.ILogger%601> dall'inserimento delle dipendenze.

::: moniker range=">= aspnetcore-2.0"

Il controller di esempio seguente crea i log `Information` e `Warning`. La *categoria* è `TodoApiSample.Controllers.TodoController` (nome completo della classe `TodoController` nell'app di esempio):

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

L'esempio di Razor Pages seguente crea log con `Information` come *livello* e `TodoApiSample.Pages.AboutModel` come *categoria*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

L'esempio precedente crea log con `Information` e `Warning` come *livello* e la classe `TodoController` come *categoria*. 

::: moniker-end

Il *livello* del log indica la gravità dell'evento registrato. La *categoria* del log è una stringa associata a ogni log. L'istanza di `ILogger<T>` crea log con nome completo di tipo `T` come categoria. [Livelli](#log-level) e [categorie](#log-category) vengono illustrati in modo dettagliato più avanti in questo articolo. 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a>Creare log nella classe Startup

Per scrivere log nella classe `Startup`, includere un parametro `ILogger` nella firma del costruttore:

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a>Creare log nella classe Program

Per scrivere log nella classe `Program`, ottenere un'istanza di `ILogger` dall'inserimento delle dipendenze:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a>Evitare l'uso di metodi logger asincroni

La registrazione deve essere così rapida da non giustificare l'impatto sulle prestazioni del codice asincrono. Se l'archivio dati di registrazione è lento, non scrivere direttamente al suo interno. Scrivere invece i messaggi di log prima in un archivio veloce e quindi spostarli nell'archivio lento in un secondo momento. Ad esempio, se la registrazione viene eseguita in SQL Server, è preferibile non farlo direttamente in un metodo `Log`, poiché i metodi `Log` sono sincroni. Al contrario, aggiungere i messaggi di log in modo sincrono a una coda in memoria e usare un ruolo di lavoro in background per eseguire il pull dei messaggi dalla coda per eseguire le operazioni asincrone di push dei dati in SQL Server.

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
      "IncludeScopes": true
    }
  }
}
```

La proprietà `Logging` può avere le proprietà `LogLevel` e quella del provider di log (nell'esempio, Console).

La proprietà `LogLevel` in `Logging` specifica il [livello](#log-level) minimo per la registrazione per determinate categorie. Nell'esempio, le categorie `System` e `Microsoft` eseguono la registrazione al livello `Information`, mentre tutte le altre la eseguono al livello `Debug`.

Altre proprietà in `Logging` specificano i provider di registrazione. L'esempio si riferisce al provider Console. Se un provider supporta gli [ambiti di log](#log-scopes), `IncludeScopes` indica se tali ambiti sono abilitati. Una proprietà del provider (ad esempio `Console` nell'esempio) può specificare anche una proprietà `LogLevel`. `LogLevel` in un provider specifica i livelli di registrazione per tale provider.

Se i livelli sono specificati in `Logging.{providername}.LogLevel`, eseguono l'override di eventuali valori impostati in `Logging.LogLevel`.

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

Con il codice di esempio illustrato nella sezione precedente i log vengono visualizzati nella console quando l'app viene eseguita dalla riga di comando. Ecco un esempio di output della console:

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

I log precedenti sono stati generati inviando una richiesta HTTP Get all'app di esempio all'indirizzo `http://localhost:5000/api/todo/0`.

Ecco un esempio degli stessi log visualizzati nella finestra Debug quando si esegue l'app di esempio in Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

I log che sono stati creati dalle chiamate a `ILogger` illustrate nella sezione precedente iniziano con "TodoApi.Controllers.TodoController". I log che iniziano con le categorie "Microsoft" provengono dal codice del framework ASP.NET Core. ASP.NET Core e il codice dell'applicazione usano la stessa API di registrazione e gli stessi provider.

Il resto di questo articolo spiega alcuni dettagli e le opzioni per la registrazione.

## <a name="nuget-packages"></a>Pacchetti NuGet

Le interfacce `ILogger` e `ILoggerFactory` si trovano in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) e le loro implementazioni predefinite si trovano in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Categoria dei log

Quando viene creato un oggetto `ILogger`, viene specificata la relativa *categoria*. La categoria è inclusa in ogni messaggio di log creato da tale istanza di `ILogger`. La categoria può essere qualsiasi stringa, ma per convenzione si usa il nome della classe, ad esempio "TodoApi.Controllers.TodoController".

Usare `ILogger<T>` per ottenere un'istanza di `ILogger` che usa il nome completo di tipo `T` come categoria:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

Per specificare in modo esplicito la categoria, chiamare `ILoggerFactory.CreateLogger`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

L'uso di `ILogger<T>` equivale a chiamare `CreateLogger` con il nome completo di tipo `T`.

## <a name="log-level"></a>Livello di registrazione

Ogni log specifica un valore <xref:Microsoft.Extensions.Logging.LogLevel>. Il livello di registrazione indica la gravità o l'importanza. È ad esempio possibile scrivere un log `Information` quando un metodo termina normalmente e un log `Warning` quando un metodo restituisce un codice di stato *404 Non trovato*.

Il codice seguente crea i log `Information` e `Warning`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Nel codice precedente il primo parametro è l'[ID dell'evento di log](#log-event-id). Il secondo parametro è un modello di messaggio con segnaposto per i valori degli argomenti forniti dai parametri dei metodi rimanenti. I parametri dei metodi sono descritti nella [sezione relativa al modello di messaggio](#log-message-template) più avanti in questo articolo.

I metodi di registrazione che includono il livello nel nome del metodo (ad esempio `LogInformation` e `LogWarning`) sono [metodi di estensione per ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions). Questi metodi chiamano un metodo `Log` che accetta un parametro `LogLevel`. È possibile chiamare il metodo `Log` direttamente anziché chiamare uno di questi metodi di estensione, ma la sintassi è relativamente complessa. Per altre informazioni, vedere <xref:Microsoft.Extensions.Logging.ILogger> e il [codice sorgente delle estensioni del logger](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).

ASP.NET Core definisce i livelli di registrazione seguenti, ordinati dal meno grave al più grave.

* Trace = 0

  Per informazioni in genere utili solo per il debug. Questi messaggi possono contenere dati sensibili dell'applicazione e pertanto non devono essere abilitati in un ambiente di produzione. *Disattivato per impostazione predefinita.*

* Debug = 1

  Per informazioni che possono essere utili per lo sviluppo e il debug. Esempio: `Entering method Configure with flag set to true.` Abilitare i livelli di registrazione `Debug` nell'ambiente di produzione solo in fase di risoluzione dei problemi, per non fare aumentare eccessivamente il volume dei log.

* Information = 2

  Per tenere traccia del flusso generale dell'app. Queste registrazioni hanno in genere un valore a lungo termine. Esempio: `Request received for path /api/todo`

* Warning = 3

  Per gli eventi imprevisti o anomali nel flusso dell'app. Tali eventi potrebbero includere errori o altre condizioni che non provocano l'arresto dell'app, ma che potrebbe essere necessario analizzare. Le eccezioni gestite sono una situazione comune in cui usare il livello di registrazione `Warning`. Esempio: `FileNotFoundException for file quotes.txt.`

* Error = 4

  Per errori ed eccezioni che non possono essere gestiti. Questi messaggi indicano un errore nell'operazione o nell'attività in corso (ad esempio la richiesta HTTP corrente), non un errore a livello di app. Messaggio di registrazione di esempio:`Cannot insert record due to duplicate key violation.`

* Critical = 5

  Per gli errori che richiedono attenzione immediata. Esempi: scenari di perdita di dati, spazio su disco insufficiente.

Usare il livello di registrazione per controllare la quantità di output di log scritto in un supporto di archiviazione specifico o in una finestra. Ad esempio:

* Nell'ambiente di produzione, inviare i messaggi con livello da `Trace` a `Information` a un archivio dati per grandi volumi. Inviare i messaggi con livello da `Warning` a `Critical` a un archivio dati di valore.
* Durante lo sviluppo, inviare i messaggi con livello da `Warning` a `Critical` alla console e aggiungere il livello da `Trace` a `Information` quando si svolgono attività di risoluzione dei problemi.

La sezione [Filtro dei log](#log-filtering) più avanti in questo articolo descrive come controllare quali livelli di registrazione gestisce un provider.

ASP.NET Core scrive log per gli eventi del framework. Gli esempi di log illustrati in precedenza in questo articolo escludono i log al di sotto del livello `Information`, quindi non vengono creati log di livello `Debug` o `Trace`. Ecco un esempio di log della console prodotti eseguendo l'app di esempio configurata in modo da mostrare i log di livello `Debug`:

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

Ogni log può specificare un *ID evento*. A tale scopo, l'app di esempio usa una classe `LoggingEvents` definita a livello locale:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

Un ID evento associa un set di eventi. Ad esempio, tutti i log correlati alla visualizzazione di un elenco di elementi in una pagina potrebbero essere indicati con 1001.

Il provider di log può archiviare l'ID evento in un campo ID o nel messaggio di registrazione oppure non archiviarlo. Il provider Debug non visualizza gli ID evento. Il provider Console visualizza gli ID evento tra parentesi quadre dopo la categoria:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Modello di messaggio di registrazione

Ogni log specifica un modello di messaggio. Il modello di messaggio può contenere segnaposto per i quali vengono forniti argomenti. Usare nomi per i segnaposto, non numeri.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

L'ordine dei segnaposto, non il loro nome, determina i parametri da usare per fornire i valori corrispondenti. Nel codice seguente, si noti che i nomi dei parametri non sono in sequenza nel modello di messaggio:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Questo codice crea un messaggio di log con i valori dei parametri in sequenza:

```
Parameter values: parm1, parm2
```

Il framework di registrazione funziona in questo modo per consentire ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Al sistema di registrazione vengono passati gli argomenti e non solo il modello di messaggio formattato. Queste informazioni consentono ai provider di registrazione di archiviare i valori dei parametri come campi. Si supponga, ad esempio, che il metodo logger esegua chiamate simili alla seguente:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Se si inviano i log all'archiviazione tabelle di Azure, ogni entità tabella di Azure può avere le proprietà `ID` e `RequestTime`, il che semplifica le query sui dati dei log. Una query può trovare tutti i log entro un determinato intervallo `RequestTime` senza che sia necessario analizzare il messaggio di testo per determinare l'intervallo di tempo.

## <a name="logging-exceptions"></a>Registrazione delle eccezioni

I metodi logger dispongono di overload che consentono di passare un'eccezione, come nell'esempio seguente:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

Provider diversi gestiscono le informazioni sulle eccezioni in modi diversi. Ecco un esempio di output del provider Debug dal codice sopra riportato.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtro dei log

::: moniker range=">= aspnetcore-2.0"

È possibile specificare un livello di registrazione minimo per un provider e una categoria specifici oppure per tutti i provider o tutte le categorie. Tutti i log sotto il livello minimo non sono passati al provider, quindi non vengono visualizzati o archiviati.

Per eliminare tutti i log, specificare `LogLevel.None` come livello di registrazione minimo. Il valore intero di `LogLevel.None` è 6, maggiore di `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Creare regole di filtro nella configurazione

Il codice del modello di progetto chiama `CreateDefaultBuilder` per configurare la registrazione per i provider Console e Debug. Il metodo `CreateDefaultBuilder` imposta inoltre la registrazione per la ricerca della configurazione di una sezione `Logging` tramite codice simile al seguente:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17)]

I dati di configurazione specificano i livelli di registrazione minimi in base al provider e alla categoria, come nell'esempio seguente:

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

Questo codice JSON crea sei regole di filtro, una per il provider Debug, quattro per il provider Console e una per tutti i provider. Quando viene creato un oggetto `ILogger`, viene scelta una singola regola per ogni provider.

### <a name="filter-rules-in-code"></a>Regole di filtro nel codice

L'esempio seguente illustra come registrare le regole di filtro nel codice:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

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

Quando viene creato un oggetto `ILogger`, l'oggetto `ILoggerFactory` seleziona una singola regola per ogni provider da applicare al logger. Tutti i messaggi scritti da un'istanza di `ILogger` vengono filtrati in base alle regole selezionate. Tra le regole disponibili viene selezionata la regola più specifica possibile per ogni coppia di categoria e provider.

L'algoritmo seguente viene usato per ogni provider quando viene creato un `ILogger` per una determinata categoria:

* Selezionare tutte le regole corrispondenti al provider o al relativo alias. Se non viene trovata alcuna corrispondenza, selezionare tutte le regole con un provider vuoto.
* Dal risultato del passaggio precedente, selezionare le regole con il prefisso di categoria corrispondente più lungo. Se non viene trovata alcuna corrispondenza, selezionare tutte le regole che non specificano una categoria.
* Se sono selezionate più regole, scegliere l'**ultima**.
* Se non è selezionata alcuna regola, usare `MinimumLevel`.

Con l'elenco di regole precedente, si supponga di creare un oggetto `ILogger` per la categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* Per il provider Debug si applicano le regole 1, 6 e 8. La regola 8 è più specifica, così sarà quella selezionata.
* Per il provider Console si applicano le regole 3, 4, 5 e 6. La regola 3 è la più specifica.

L'istanza di `ILogger` risultante invia i log di livello `Trace` o superiore al provider Debug. I log di livello `Debug` o superiore vengono inviati al provider Console.

### <a name="provider-aliases"></a>Alias dei provider

Ogni provider definisce un *alias* che può essere utilizzato nella configurazione al posto del nome completo di tipo.  Per i provider predefiniti, usare gli alias seguenti:

* Console
* Debug
* EventSource
* EventLog
* TraceSource
* AzureAppServicesFile
* AzureAppServicesBlob
* ApplicationInsights

### <a name="default-minimum-level"></a>Livello minimo predefinito

Esiste un'impostazione del livello minimo che diventa effettiva solo se non si applica alcuna regola di configurazione o codice per un provider e una categoria specifici. L'esempio seguente illustra come impostare il livello minimo:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

Se non si imposta in modo esplicito il livello minimo, il valore predefinito è `Information`, che indica che i log `Trace` e `Debug` vengono ignorati.

### <a name="filter-functions"></a>Funzioni di filtro

Una funzione di filtro viene richiamata per tutti i provider e le categorie a cui non sono assegnate regole tramite la configurazione o il codice. Il codice della funzione ha accesso al tipo di provider, alla categoria e al livello di registrazione. Ad esempio:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Alcuni provider di registrazione consentono di specificare quando i log devono essere scritti in un supporto di archiviazione oppure ignorati in base al livello di registrazione e alla categoria.

I metodi di estensione `AddConsole` e `AddDebug` forniscono overload che accettano criteri di filtro. L'esempio di codice seguente fa sì che il provider Console ignori i log di livello inferiore a `Warning`, mentre il provider Debug ignora i log creati dal framework.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

Il metodo `AddEventLog` ha un overload che accetta un'istanza di `EventLogSettings`, che può contenere una funzione di filtro nella proprietà `Filter`. Il provider TraceSource non fornisce alcun overload di questo tipo, in quanto il suo livello di registrazione e altri parametri si basano sugli oggetti `SourceSwitch` e `TraceListener` in uso.

Per impostare regole di filtro per tutti i provider registrati con un'istanza di `ILoggerFactory`, usare il metodo di estensione `WithFilter`. L'esempio seguente limita i log del framework (la categoria inizia con "Microsoft" o "System") agli avvisi consentendo la registrazione a livello debug per i log creati dal codice dell'applicazione.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Per impedire la scrittura di log, specificare `LogLevel.None` come livello di registrazione minimo. Il valore intero di `LogLevel.None` è 6, maggiore di `LogLevel.Critical` (5).

Il metodo di estensione `WithFilter` viene fornito dal pacchetto NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter). Il metodo restituisce una nuova istanza di `ILoggerFactory` che filtra i messaggi di registrazione passati a tutti i provider di logger registrati con esso. Non si applica ad altre istanze di `ILoggerFactory`, inclusa l'istanza originale di `ILoggerFactory`.

::: moniker-end

## <a name="system-categories-and-levels"></a>Livelli e categorie di sistema

Di seguito sono elencate alcune categorie usate da ASP.NET Core ed Entity Framework Core, con note sui log previsti per ognuna:

| Category                            | Note |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | Diagnostica generale di ASP.NET Core. |
| Microsoft.AspNetCore.DataProtection | Chiavi considerate, trovate e usate. |
| Microsoft.AspNetCore.HostFiltering  | Host consentiti. |
| Microsoft.AspNetCore.Hosting        | Tempo impiegato per il completamento delle richieste HTTP e ora di inizio delle richieste. Assembly di avvio dell'hosting caricati. |
| Microsoft.AspNetCore.Mvc            | Diagnostica di MVC e Razor. Associazione di modelli, esecuzione di filtri, compilazione di viste, selezione di azioni. |
| Microsoft.AspNetCore.Routing        | Informazioni sulla corrispondenza di route. |
| Microsoft.AspNetCore.Server         | Risposte di avvio, arresto e keep-alive per la connessione. Informazioni sui certificati HTTPS. |
| Microsoft.AspNetCore.StaticFiles    | File forniti. |
| Microsoft.EntityFrameworkCore       | Diagnostica generale di Entity Framework Core. Attività e configurazione del database, rilevamento delle modifiche, migrazioni. |

## <a name="log-scopes"></a>Ambiti dei log

 Un *ambito* può raggruppare un set di operazioni logiche. Questo raggruppamento può essere usato per collegare gli stessi dati a ogni log creato come parte di un set. Ad esempio, ogni log creato come parte dell'elaborazione di una transazione può includere l'ID transazione.

Un ambito è un tipo `IDisposable` restituito dal metodo <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> e viene mantenuto fino all'eliminazione. Un ambito si usa mediante il wrapping delle chiamate del logger in un blocco `using`:

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Il codice seguente abilita gli ambiti per il provider Console:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> La configurazione dell'opzione del logger della console `IncludeScopes` è necessaria per abilitare la registrazione basata sull'ambito.
>
> Per informazioni sulla configurazione, vedere la sezione [Configurazione](#configuration).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> La configurazione dell'opzione del logger della console `IncludeScopes` è necessaria per abilitare la registrazione basata sull'ambito.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

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
* [AzureAppServicesFile](#azure-app-service-provider)
* [AzureAppServicesBlob](#azure-app-service-provider)
* [ApplicationInsights](#azure-application-insights-trace-logging)

Per informazioni su StdOut e la registrazione debug con il modulo ASP.NET Core, vedere <xref:test/troubleshoot-azure-iis> e <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.

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

Gli [overload di AddConsole](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) consentono di passare un livello di registrazione minimo, una funzione di filtro e un valore booleano che indica se gli ambiti sono supportati. Un'altra opzione consiste nel passare un oggetto `IConfiguration`, che può specificare livelli di registrazione e il supporto di ambiti.

Per le opzioni del provider della console, vedere <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions>.

Il provider Console ha un impatto significativo sulle prestazioni e non è in genere appropriato per l'uso in un ambiente di produzione.

Quando si crea un nuovo progetto in Visual Studio, il metodo `AddConsole` è simile al seguente:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Questo codice fa riferimento alla sezione `Logging` del file *appSettings.json*:

[!code-json[](index/samples/1.x/TodoApiSample/appsettings.json)]

Le impostazioni visualizzate limitano i log del framework agli avvisi, consentendo all'app di registrare a livello di debug, come descritto nella sezione [Filtri dei log](#log-filtering). Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).

::: moniker-end

Per visualizzare l'output di registrazione del provider Console, aprire un prompt dei comandi nella cartella del progetto ed eseguire il comando seguente:

```console
dotnet run
```

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

Gli [overload di AddDebug](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) consentono passare un livello di registrazione minimo o una funzione di filtro.

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

Gli [overload di AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) consentono di passare `EventLogSettings` o un livello di registrazione minimo.

::: moniker-end

### <a name="tracesource-provider"></a>Provider TraceSource

Il pacchetto di provider [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa le librerie e i provider <xref:System.Diagnostics.TraceSource>.

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

Gli [overload di AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) consentono di passare un cambio di origine e un listener di traccia.

Per usare questo provider, un'app deve essere in esecuzione in .NET Framework (invece che in .NET Core). Il provider può instradare i messaggi a diversi [listener](/dotnet/framework/debug-trace-profile/trace-listeners), come <xref:System.Diagnostics.TextWriterTraceListener>, usato nell'app di esempio.

::: moniker range="< aspnetcore-2.0"

L'esempio seguente configura un provider `TraceSource` che registra messaggi `Warning` e di livello superiore nella finestra della console.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a>Provider del Servizio app di Azure

Il pacchetto di provider [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) scrive i log in file di testo nel file system di un'app del Servizio app di Azure e nell'[archivio di BLOB](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in un account di archiviazione di Azure. Il pacchetto di provider è disponibile per le app destinate a .NET Core 1.1 o versione successiva.

::: moniker range=">= aspnetcore-2.0"

Se la destinazione è .NET Core, tenere presente quanto segue:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Il pacchetto del provider è incluso nel [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ASP.NET Core.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Il pacchetto del provider non è incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Per usare il provider, installare il pacchetto.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Se la destinazione è .NET Framework o il riferimento è il metapacchetto `Microsoft.AspNetCore.App`, aggiungere il pacchetto di provider al progetto. Richiamare `AddAzureWebAppDiagnostics`:

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

Un overload di <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> consente di passare <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>. L'oggetto impostazioni può eseguire l'override delle impostazioni predefinite, ad esempio il modello di output di registrazione, il nome di BLOB e il limite di dimensioni di file. Il *modello di output* è un modello di messaggio che viene applicato a tutti i log in aggiunta a quanto specificato quando si chiama un metodo `ILogger`.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Per configurare le impostazioni del provider, usare <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> e <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, come illustrato nell'esempio seguente:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

Quando si distribuisce un'app del servizio app, l'applicazione applica le impostazioni della sezione [Log del servizio app](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) della pagina **Servizio app** del portale di Azure. Quando le impostazioni seguenti vengono aggiornate, le modifiche hanno effetto immediatamente senza richiedere un riavvio o la ridistribuzione dell'app.

* **Registrazione applicazioni (file system)**
* **Registrazione applicazione (BLOB)**

Il percorso predefinito per i file di log è nella cartella *D:\\home\\LogFiles\\Application* e il nome di file predefinito è *diagnostics-aaaammgg.txt*. Il limite predefinito per le dimensioni del file è 10 MB e il numero massimo predefinito di file conservati è 2. Il nome BLOB predefinito è *{nome-app}{timestamp}/aaaa/mm/gg/hh/{guid}-applicationLog.txt*.

Il provider funziona solo quando il progetto viene eseguito nell'ambiente di Azure. Non ha alcun effetto quando il progetto viene eseguito localmente, in quanto non scrive nulla in file locali o in archivi di sviluppo locali per i BLOB.

#### <a name="azure-log-streaming"></a>Flusso di registrazione di Azure

Il flusso di registrazione di Azure consente di visualizzare l'attività di registrazione in tempo reale da:

* Server applicazioni
* Server Web
* Traccia delle richieste non riuscite

Per configurare il flusso di registrazione di Azure:

* Passare alla pagina **Log del servizio app** dalla pagina del portale dell'app.
* Impostare **Registrazione applicazioni (file system)** su **Sì**.
* Scegliere il livello di registrazione in **Livello**.

Passare alla pagina **Flusso di registrazione** per visualizzare i messaggi dell'app. I messaggi vengono registrati dall'app tramite l'interfaccia `ILogger`.

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a>Registrazione di traccia di Azure Application Insights

Il pacchetto di provider [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) scrive log in Azure Application Insights. Application Insights è un servizio che monitora un'app Web e fornisce gli strumenti per l'esecuzione di query sui dati di telemetria e la loro analisi. Se si usa questo provider, è possibile eseguire query sui log e analizzarli usando gli strumenti di Application Insights.

Il provider di registrazione è incluso come dipendenza di [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), ovvero il pacchetto che fornisce tutti i dati di telemetria disponibili per ASP.NET Core. Se si usa questo pacchetto, non è necessario installare il pacchetto di provider.

Non usare il pacchetto [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web), che è per ASP.NET 4.x.

Per altre informazioni, vedere le seguenti risorse:

* [Panoramica di Application Insights](/azure/application-insights/app-insights-overview)
* [Application Insights per applicazioni ASP.NET Core](/azure/azure-monitor/app/asp-net-core-no-visualstudio): iniziare da qui se si vuole implementare l'intervallo completo dei dati di telemetria di Application Insights insieme alla registrazione.
* [ApplicationInsightsLoggerProvider per log ILogger .NET Core](/azure/azure-monitor/app/ilogger): iniziare da qui se si vuole implementare il provider di registrazione senza gli altri dati di telemetria di Application Insights.
* [Adattatori di registrazione di Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).
* [Installare, configurare e inizializzare Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights): esercitazione interattiva nel sito Microsoft Learn.
::: moniker-end

> [!NOTE]
> Dal 1° maggio 2019 l'articolo intitolato [Application Insights per ASP.NET Core](/azure/azure-monitor/app/asp-net-core) non è aggiornato e i passaggi dell'esercitazione non funzionano. Vedere invece [Application Insights per applicazioni ASP.NET Core](/azure/azure-monitor/app/asp-net-core-no-visualstudio). Microsoft è a conoscenza del problema e sta lavorando per risolverlo.

## <a name="third-party-logging-providers"></a>Provider di registrazione di terze parti

Framework di registrazione di terze parti che usano ASP.NET Core:

* [elmah.io](https://elmah.io/) ([repository GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([repository GitHub](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](https://jsnlog.com/) ([repository GitHub](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([repository GitHub](https://github.com/catalingavan/KissLog-net))
* [Loggr](https://loggr.net/) ([repository GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](https://nlog-project.org/) ([repository GitHub](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([repository GitHub](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([repository GitHub](https://github.com/serilog/serilog-aspnetcore))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([repository Github](https://github.com/googleapis/google-cloud-dotnet))

Alcuni framework di terze parti possono eseguire la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

L'uso di un framework di terze parti è simile a quello di uno dei provider predefiniti:

1. Aggiungere un pacchetto NuGet al progetto.
1. Chiamare un oggetto `ILoggerFactory`.

Per altre informazioni, vedere la documentazione di ogni provider. I provider di registrazione di terze parti non sono supportati da Microsoft.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/logging/loggermessage>
