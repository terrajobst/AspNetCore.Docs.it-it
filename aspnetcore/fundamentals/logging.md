---
title: Registrazione di ASP.NET Core
author: ardalis
description: "Introduce il framework di registrazione di ASP.NET Core. Include una sezione per ogni provider di registrazione predefiniti e collegamenti ad alcuni provider di terze parti più diffusi."
keywords: Gli ambiti di ASP.NET Core, registrazione, il provider di log, Microsoft.Extensions.Logging, ILogger, ILoggerFactory, LogLevel, WithFilter, TraceSource, EventLog, EventSource,
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 15abe93d881aed3b6950a859dc9445ec50ee9bb5
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a>Introduzione alla registrazione di ASP.NET Core

Da [Steve Smith](http://ardalis.com) e [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di log. I provider predefiniti consentono di inviare i log per una o più destinazioni, ed è possibile collegare un framework di registrazione di terze parti. In questo articolo viene illustrato come utilizzare i provider e l'API di registrazione incorporate nel codice.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)

---

## <a name="how-to-create-logs"></a>Come creare i registri

Per creare registri, ottenere un `ILogger` dall'oggetto di [inserimento di dipendenze](dependency-injection.md) contenitore:

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

L'oggetto logger quindi chiamare i metodi di registrazione:

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Questo esempio viene creata con i registri di `TodoController` di classi di *categoria*.  Vengono descritte le categorie [più avanti in questo articolo](#log-category).

ASP.NET Core non forniscono async logger metodi perché la registrazione deve essere così rapida non è importante il costo dell'utilizzo di async. Se si trova in una situazione in cui non è true, provare a modificare la modalità di accesso.  Se l'archivio dati è lenta, scrivere i messaggi di log a un archivio veloce prima, quindi spostarli in un archivio lenta in un secondo momento. Ad esempio, accedere a una coda di messaggi che viene letta e resa persistente nell'archiviazione lento da un altro processo.

## <a name="how-to-add-providers"></a>Come aggiungere i provider

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Un provider di log preleva i messaggi creati con un `ILogger` dell'oggetto e consente di visualizzare o archiviarli. Ad esempio, il provider di Console visualizza i messaggi nella console e il provider di servizio App di Azure può archiviare nell'archiviazione blob di Azure.

Per utilizzare un provider, chiamare il provider `Add<ProviderName>` il metodo di estensione in *Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Il modello di progetto predefinito imposta livello di registrazione il modo in cui è visualizzato nel codice precedente, ma la `ConfigureLogging` chiamata viene eseguita `CreateDefaultBuilder` (metodo). Ecco il codice *Program.cs* create da modelli di progetto:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Un provider di log preleva i messaggi creati con un `ILogger` dell'oggetto e consente di visualizzare o archiviarli. Ad esempio, il provider di Console visualizza i messaggi nella console e il provider di servizio App di Azure può archiviare nell'archiviazione blob di Azure.

Per utilizzare un provider, installare il pacchetto NuGet e chiamare il metodo di estensione del provider in un'istanza di `ILoggerFactory`, come illustrato nell'esempio seguente.

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [inserimento di dipendenze](dependency-injection.md) (DI) fornisce il `ILoggerFactory` istanza. Il `AddConsole` e `AddDebug` metodi di estensione sono definiti nel [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pacchetti. Chiama ogni metodo di estensione di `ILoggerFactory.AddProvider` , passando in un'istanza del provider. 

> [!NOTE]
> L'applicazione di esempio per l'articolo aggiunge dei provider di registrazione nel `Configure` metodo la `Startup` classe. Se si desidera ottenere l'output di log dal codice eseguito in precedenza, aggiungere il provider di log nel `Startup` invece costruttore di classe. 

---

Vengono fornite informazioni su ogni [provider di log incorporato](#built-in-logging-providers) e i collegamenti agli [provider di log di terze parti](#third-party-logging-providers) più avanti in questo articolo.

## <a name="sample-logging-output"></a>Esempio di output di registrazione

Con il codice di esempio illustrato nella sezione precedente, si noterà registri nella console durante l'esecuzione dalla riga di comando. Di seguito è riportato un esempio di output della console:

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
 
Questi log sono stati creati, passare a `http://localhost:5000/api/todo/0`, che attiva l'esecuzione di entrambi `ILogger` chiamate illustrate nella sezione precedente.

Ecco un esempio degli stessi log come appaiono nella finestra di Debug quando si esegue l'applicazione di esempio in Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

I log sono stati creati con la `ILogger` chiamate illustrate nella sezione precedente iniziano con "TodoApi.Controllers.TodoController". I registri che iniziano con le categorie di "Microsoft" sono da ASP.NET Core. ASP.NET Core stesso e il codice dell'applicazione utilizza la stessa API di registrazione e gli stessi provider di registrazione.

Il resto di questo articolo illustra alcuni dettagli e le opzioni per la registrazione.

## <a name="nuget-packages"></a>Pacchetti NuGet

Il `ILogger` e `ILoggerFactory` presenti interfacce [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), e le implementazioni predefinite per essi sono [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).

## <a name="log-category"></a>Categoria di log

Oggetto *categoria* è incluso in ogni log creati.  Specificare la categoria quando si crea un `ILogger` oggetto. La categoria può essere qualsiasi stringa, ma una convenzione consiste nell'utilizzare il nome completo della classe da cui vengono scritti i log.  Ad esempio: "TodoApi.Controllers.TodoController".

È possibile specificare la categoria sotto forma di stringa o utilizzare un metodo di estensione da cui deriva il tipo di categoria. Per specificare la categoria sotto forma di stringa, chiamare `CreateLogger` su un `ILoggerFactory` dell'istanza, come illustrato di seguito.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

La maggior parte dei casi, sarà più facile da utilizzare `ILogger<T>`, come nell'esempio seguente.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Ciò equivale a chiamare il metodo `CreateLogger` con il nome completo del tipo di `T`.

## <a name="log-level"></a>Livello di registrazione

Ogni volta che si scrive un log, specificare il relativo [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel). Il livello di log indica il livello di gravità o priorità.  Ad esempio, è possibile scrivere un `Information` log al termine di un metodo in genere, un `Warning` quando un metodo viene restituito un errore 404 codice restituito e un log `Error` log quando viene rilevata un'eccezione imprevista.

Nell'esempio di codice seguente, i nomi dei metodi (ad esempio, `LogWarning`) specificare il livello di registrazione.  Il primo parametro è il [Log ID evento](#log-event-id) (descritta più avanti in questo articolo).  I parametri rimanenti costruire una stringa di messaggio di log.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Metodi di log che includono il livello nel nome del metodo sono [i metodi di estensione ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions). Dietro le quinte, chiamano questi metodi un `Log` metodo che accetta un `LogLevel` parametro. È possibile chiamare il `Log` metodo direttamente, anziché uno di questi metodi di estensione, ma la sintassi è relativamente complesso. Per ulteriori informazioni, vedere il [interfaccia ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) e [codice sorgente di estensioni del logger](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core definisce i seguenti [livelli di log](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordinato seguito da almeno a più alto livello di gravità.

* Traccia = 0

  Per informazioni che sono utile solo per uno sviluppatore di debug di un problema. Questi messaggi possono contenere dati sensibili dell'applicazione e pertanto non devono essere abilitati in un ambiente di produzione. *Disattivata per impostazione predefinita.* Esempio: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Eseguire il debug = 1

  Per informazioni a breve termine utilità durante lo sviluppo e debug. Esempio: `Entering method Configure with flag set to true.` è in genere non consentirebbe `Debug` livello vengono registrati nell'ambiente di produzione, a meno che la risoluzione, a causa dell'elevato volume di log.

* Informazioni = 2

  Per gestire il flusso generale dell'applicazione. Questi log sono in genere un valore a lungo termine. Esempio: `Request received for path /api/todo`

* Avviso = 3

  Per gli eventi imprevisti o anomali nel flusso dell'applicazione. Potrebbero essere presenti errori o altre condizioni che non provocano l'applicazione di interrompere, ma che potrebbe essere necessario analizzare. Eccezioni gestite sono un punto comune per utilizzare il `Warning` livello di registrazione. Esempio: `FileNotFoundException for file quotes.txt.`

* Errore = 4

  Per errori ed eccezioni che non possono essere gestiti. Questi messaggi indicano un errore nell'operazione (ad esempio la richiesta HTTP corrente) o l'attività corrente, non un errore a livello di applicazione. Messaggio di log di esempio:`Cannot insert record due to duplicate key violation.`

* Critico = 5

  Per gli errori che richiedono attenzione immediata. Esempi: scenari di perdita dati, spazio su disco insufficiente.

È possibile utilizzare il livello di registrazione per controllare la quantità del log output viene scritto un supporto di archiviazione specifico o visualizzare una finestra. Ad esempio, nell'ambiente di produzione è consigliabile tutti i log di `Information` livello e inferiore per passare a un archivio dati di volume e tutti i log di `Warning` livello e versioni successive per passare a un archivio dati di valore. Durante lo sviluppo, in genere è possibile inviare i log di `Warning` o gravità superiore nella console. Quando è necessario risolvere i problemi, è possibile aggiungere `Debug` livello. Il [filtri Log](#log-filtering) sezione più avanti in questo articolo viene descritto come controllare quali livelli di registrazione gestisce un provider.

Scrive il framework di ASP.NET Core `Debug` framework eventi nei registri di livello. Gli esempi di log in precedenza in questo articolo esclusi log seguenti `Information` livello, quindi nessun `Debug` venivano visualizzati vengono registrati. Di seguito è riportato un esempio di registri della console se si esegue l'applicazione di esempio configurato per mostrare `Debug` e i registri superiore per il provider di console.

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

## <a name="log-event-id"></a>ID del registro eventi

Ogni volta che si scrive un log, specificare un *ID evento*. L'app di esempio esegue questa funzione utilizzando un oggetto definito localmente `LoggingEvents` classe:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

ID evento è un valore intero che è possibile utilizzare per associare un set di eventi registrati tra loro. Ad esempio, un log per l'aggiunta di un elemento al carrello acquisti potrebbe essere l'ID evento 1000 e un log per il completamento di un fornitore potrebbe essere l'ID evento 1001.

Nell'output di registrazione, l'ID evento può essere archiviato in un campo o inclusi nel testo del messaggio, a seconda del provider.  Il provider di Debug non vengono visualizzati gli ID degli eventi, ma il provider di console Mostra le parentesi quadre dopo la categoria:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a>Stringa di formato di messaggio di log

Ogni volta che si scrive un log, fornire un messaggio di testo. La stringa di messaggio può contenere segnaposto denominati in quale argomento vengono inseriti i valori, come nell'esempio seguente:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

L'ordine dei segnaposto, non i relativi nomi determina quali parametri vengono utilizzati per essi. Si supponga, ad esempio, di avere il codice seguente:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Il messaggio di log risultante sarebbe simile al seguente:

```
Parameter values: parm1, parm2
```

Il framework di registrazione dei messaggi di formattazione in questo modo per rendere possibile per i provider di registrazione implementare [registrazione semantica, nota anche come registrazione strutturata](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Poiché gli argomenti stessi vengono passati al sistema di registrazione, non solo la stringa di messaggio formattato, i provider di registrazione è possono archiviare i valori dei parametri come campi oltre la stringa di messaggio. Ad esempio, se indirizza il log di output per l'archiviazione tabelle di Azure e la chiamata al metodo logger è simile al seguente:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Ogni entità di tabelle di Azure potrebbe avere `ID` e `RequestTime` proprietà, che dovrebbe semplificare le query sui dati di log. È possibile trovare tutti i log all'interno di un particolare `RequestTime` intervallo, senza la necessità di analizzare il valore di timeout del messaggio di testo.

## <a name="logging-exceptions"></a>Registrazione delle eccezioni

I metodi di logger dispongono di overload che consentono di passare un'eccezione, come nell'esempio seguente:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Provider diversi gestire le informazioni sull'eccezione in modi diversi. Di seguito è riportato un esempio di output di Debug di provider di codice sopra riportato.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Il filtraggio di log

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

È possibile specificare un livello di log minimo per un determinato provider e la categoria o per tutti i provider o tutte le categorie.  Tutti i log sotto il livello minimo non sono passati a tale provider, in modo non ottenere visualizzati o archiviati. 

Se si desidera eliminare tutti i log, è possibile specificare `LogLevel.None` come il livello di registrazione minima. Il valore integer `LogLevel.None` è 6, ovvero superiore `LogLevel.Critical` (5).

**Creare regole di filtro nella configurazione**

I modelli di progetto creano codice che chiama `CreateDefaultBuilder` per impostare la registrazione per i provider Console ed eseguire il Debug. Il `CreateDefaultBuilder` metodo imposta inoltre il livello di registrazione per la ricerca per la configurazione di un `Logging` sezione utilizzando codice simile al seguente:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

I dati di configurazione specificano i livelli di log minimo dal provider e categoria, come nell'esempio seguente:

[!code-json[](logging/sample2/appsettings.json)]

Questo codice JSON crea sei regole di filtro, uno per il provider di Debug, quattro per il provider della Console e quello che si applica a tutti i provider. Verrà visualizzato in un secondo momento la modalità di una di queste regole viene scelto per ogni provider quando un `ILogger` viene creato l'oggetto.

**Regole di filtro nel codice**

È possibile registrare le regole di filtro nel codice, come illustrato nell'esempio seguente:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Il secondo `AddFilter` specifica il provider di Debug tramite il nome del tipo. Il primo `AddFilter` si applica a tutti i provider di quanto non consente di specificare un tipo di provider.

**Funzionamento delle regole di filtro vengono applicati.**

I dati di configurazione e `AddFilter` codice illustrato negli esempi precedenti creare le regole indicate nella tabella seguente. I primi sei provengono da esempio di configurazione e gli ultimi due provengono dall'esempio di codice.

Number|Provider|Categorie che iniziano con|Livello di registrazione minima|
------|--------|--------------------------|-----------------|
1|Debug|Tutte le categorie|Informazioni|
2|Console|Microsoft.AspNetCore.Mvc.Razor.Internal|Avviso|
3|Console|Microsoft.AspNetCore.Mvc.Razor.Razor|Debug|
4|Console|Microsoft.AspNetCore.Mvc.Razor|Errore|
5|Console|Tutte le categorie|Informazioni|
6|Tutti i provider|Tutte le categorie|Debug
7|Tutti i provider|System|Debug
8|Debug|Microsoft|Traccia

Quando si crea un `ILogger` oggetto da scrivere i log, con la `ILoggerFactory` oggetto seleziona una singola regola per ogni provider da applicare al logger. Tutti i messaggi scritti da tale `ILogger` oggetto sono filtrati in base alle regole selezionate. La regola più specifica possibile per ogni coppia di categoria e il provider viene selezionata dalle regole disponibili.

Viene utilizzato l'algoritmo seguente per ogni provider quando un `ILogger` viene creato per una determinata categoria:

* Selezionare tutte le regole che soddisfano il provider o il relativo alias.  Se non vengono rilevati, selezionare tutte le regole con un provider vuoto.
* Dal risultato del passaggio precedente, selezionare le regole con più lunga corrispondente prefisso di categoria. Se non vengono rilevati, selezionare tutte le regole che non specifica una categoria.
* Se sono selezionate più regole di richiedere la **ultimo** uno.
* Se è selezionata alcuna regola, utilizzare `MinimumLevel`.
 
Ad esempio, si supponga di avere l'elenco di regole precedente e si crea un `ILogger` oggetto per la categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* Per il provider di Debug, si applicano le regole 1, 6 e 8. Regola 8 è più specifico, in modo che sia quello selezionato.
* Per il provider di Console, si applicano le regole 3, 4, 5 e 6. La regola 3 è più specifica.

Quando si creano log con un `ILogger` per categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", registri di `Trace` livello e sopra verrà inviata al provider di Debug e i registri di `Debug` livello e versioni successive, verrà inviata al provider di Console.

**Alias di provider**

È possibile utilizzare il nome del tipo per specificare un provider di configurazione, ma ogni provider definisce un più breve *alias* che è più facile da utilizzare. Per i provider predefiniti, utilizzare l'alias seguente:

- Console
- Debug
- Registro eventi
- AzureAppServices
- TraceSource
- EventSource

**Livello minimo predefinito**

È un'impostazione del livello minima che diventa effettivo solo se si applica alcuna regola di configurazione o codice per un determinato provider e la categoria. Nell'esempio seguente viene illustrato come impostare il livello minimo:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Se si non imposta in modo esplicito il livello minimo, il valore predefinito è `Information`, vale a dire che `Trace` e `Debug` registri vengono ignorati.

**Funzioni di filtro**

È possibile scrivere codice in una funzione di filtro da applicare le regole di filtro. Una funzione di filtro viene richiamata per tutti i provider e le categorie che non dispongono di regole assegnate tramite configurazione o codice. Codice della funzione dispone di accesso per il tipo di provider, categoria e livello di registrazione per stabilire o meno un messaggio deve essere registrato. Ad esempio:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Alcuni provider di registrazione consente di specificare quando devono essere scritti in un supporto di archiviazione oppure ignorati registri in base a categoria e livello di registrazione.

Il `AddConsole` e `AddDebug` metodi di estensione forniscono gli overload che consentono di passare in Criteri di filtro. Esempio di codice seguente fa sì che il provider di console ignorare i log seguenti `Warning` livello, mentre il provider di Debug ignora i registri che il framework crea.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

Il `AddEventLog` metodo presenta un overload che accetta un `EventLogSettings` istanza, che può contenere una funzione di filtro relative `Filter` proprietà. Il provider TraceSource non è disponibile alcuna di tali overload, poiché il relativo livello di registrazione e gli altri parametri sono basati sul `SourceSwitch` e `TraceListener` Usa.

È possibile impostare le regole di filtro per tutti i provider registrati con un `ILoggerFactory` istanza utilizzando il `WithFilter` metodo di estensione. Nell'esempio seguente consente di limitare framework accede (categoria inizia con "Microsoft" o "System") per gli avvisi accettandone nel registro applicazioni a livello di debug.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Se si desidera utilizzare i filtri per impedire che tutti i log scritti per una determinata categoria, è possibile specificare `LogLevel.None` come il livello di log minimo per tale categoria. Il valore integer `LogLevel.None` è 6, ovvero superiore `LogLevel.Critical` (5).

Il `WithFilter` viene fornito il metodo di estensione per il [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pacchetto NuGet. Il metodo restituisce un nuovo `ILoggerFactory` istanza che consenta di filtrare i messaggi del registro passati a tutti i provider di logger registrati con esso. Non si applica agli altri `ILoggerFactory` istanze, tra cui originale `ILoggerFactory` istanza.

---

## <a name="log-scopes"></a>Ambiti di log

È possibile raggruppare un set di operazioni logiche all'interno di un *ambito* per collegare gli stessi dati per ogni log da cui viene creato come parte di tale set.  Ad esempio, è possibile ogni log creato come parte dell'elaborazione di una transazione per includere l'ID transazione.

Un ambito è un `IDisposable` tipo restituito dal `ILogger.BeginScope<TState>` metodo e viene mantenuta fino a quando non viene eliminato. Utilizzare un ambito mediante il wrapping del logger chiama in un `using` blocco, come illustrato di seguito:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Il codice seguente consente gli ambiti per il provider di console:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

In *Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

In *Startup.cs*:

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

Ogni messaggio del log include le informazioni con ambite:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Provider di registrazione predefiniti

ASP.NET Core sono disponibili i provider seguenti:

* [Console](#console)
* [Debug](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Servizio app di Azure](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>Il provider di console

Il [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package provider invia l'output di log nella console. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

[AddConsole overload](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) consentono passare un valore booleano che indica se gli ambiti sono supportati, una funzione di filtro e un livello di log minimo.  Un'altra opzione consiste nel passare un `IConfiguration` oggetto, che è possibile specificare livelli di registrazione e il supporto di ambiti. 

Se si intende il provider di console per l'utilizzo nell'ambiente di produzione, tenere presente che ha un impatto significativo sulle prestazioni.

Quando si crea un nuovo progetto in Visual Studio, il `AddConsole` metodo è simile al seguente:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Questo codice si intende il `Logging` sezione il *appSettings. JSON* file:

[!code-json[](logging/sample//appsettings.json)]

Le impostazioni visualizzate limite framework accede agli avvisi, consentendo l'app per accedere a livello di debug, come illustrato nel [filtri Log](#log-filtering) sezione. Per ulteriori informazioni, vedere [configurazione](configuration.md).

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>Il provider di Debug

Il [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package provider scrive l'output del log usando il [debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) classe (`Debug.WriteLine` chiamate).

In Linux, il provider scrive i log per */var/log/message*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[Gli overload AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) consentono passare un livello di registrazione minima o una funzione di filtro.

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>Il provider di EventSource

Per le app destinate a ASP.NET Core 1.1.0 o versione successiva, il [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) package provider può implementare la traccia eventi. In Windows, viene utilizzato [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Il provider è multipiattaforma, ma sono non disponibili gli strumenti di raccolta e la visualizzazione ancora per Linux o Mac OS alcun evento. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

Un buon metodo per raccogliere e visualizzare i log consiste nell'utilizzare il [PerfView utilità](https://www.microsoft.com/download/details.aspx?id=28567). Sono disponibili altri strumenti per la visualizzazione dei log ETW, ma PerfView fornisce un'esperienza ottimale per l'utilizzo con gli eventi ETW generati da ASP.NET. 

Per configurare PerfView per raccogliere gli eventi registrati da questo provider, aggiungere la stringa `*Microsoft-Extensions-Logging` per il **altri provider** elenco. (Non perdete l'asterisco all'inizio della stringa).

![Altri provider di Perfview](logging/_static/perfview-additional-providers.png)

Acquisizione di eventi in Nano Server richiede operazioni di configurazione aggiuntive:

* Connettersi Nano Server di comunicazione remota di PowerShell:

  ```powershell
  Enter-PSSession [name]
  ```

* Creare una sessione ETW:

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* Aggiungere i provider ETW per [CLR](https://msdn.microsoft.com/library/ff357718), ASP.NET Core e altri in base alle esigenze. Il provider ASP.NET Core GUID è `3ac73b97-af73-50e9-0822-5da4367920d0`. 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* Esecuzione del sito e tutte le azioni che si desidera che le informazioni relative.

* Arrestare la sessione di traccia è terminato:

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

Il valore risultante *C:\trace.etl* file può essere analizzati con PerfView come in altre edizioni di Windows.

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Il provider del registro eventi di Windows

Il [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) package provider invia l'output di log nel registro eventi di Windows.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[Overload di metodo AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) consentono di passare in `EventLogSettings` o un livello di log minimo.

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>Il provider TraceSource

Il [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider pacchetto utilizza il [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) librerie e i provider.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[Gli overload AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) consentono di passare un'opzione di origine e un listener di traccia.

Per utilizzare questo provider, un'applicazione deve essere eseguito su di .NET Framework (anziché .NET Core). I provider consente di instradare i messaggi a una varietà di [listener](https://msdn.microsoft.com/library/4y5y10s7), ad esempio il [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener) utilizzato nell'applicazione di esempio.

Nell'esempio seguente viene configurata una `TraceSource` provider che registra `Warning` e messaggi superiore alla finestra della console.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Il provider di servizio App di Azure

Il [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) package provider scrive i log in file di testo nel file system di un'applicazione servizio App di Azure e a [nell'archiviazione blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in un account di archiviazione di Azure. Il provider è disponibile solo per le app destinate a ASP.NET Core 1.1.0 o versione successiva. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

> [!NOTE]
> Componenti di base di ASP.NET 2.0 è disponibile in anteprima.  Le app create con la versione di anteprima più recente potrebbero non essere eseguita quando distribuito in Azure App Service. Quando viene rilasciata Core ASP.NET 2.0, Azure App Service eseguirà 2.0 App e il servizio App di Azure provider funzionerà come indicato di seguito.

Non è necessario installare il pacchetto del provider o una chiamata di `AddAzureWebAppDiagnostics` metodo di estensione.  Il provider è automaticamente disponibile per l'app quando si distribuisce l'applicazione di servizio App di Azure.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

Un `AddAzureWebAppDiagnostics` overload consente di passare in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), con cui è possibile eseguire l'override delle impostazioni predefinite, ad esempio il modello di output di registrazione, il nome di blob e il limite di dimensioni di file. (*Modello output* è una stringa di formato di messaggio che viene applicata a tutti i registri, nella parte superiore a quella specificata dall'utente quando si chiama un `ILogger` metodo.)  

---

Quando si distribuisce un'applicazione di servizio App, l'applicazione rispetta le impostazioni di [i log di diagnostica](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) sezione del **servizio App** pagina del portale di Azure. Quando si modificano queste impostazioni, le modifiche avranno effetto immediatamente senza la necessità di riavviare l'app oppure ridistribuire codice. 

![Impostazioni di registrazione di Azure](logging/_static/azure-logging-settings.png)

Il percorso predefinito per i file di log è il *unità d:\\home\\LogFiles\\applicazione* cartella e il nome file predefinito è *diagnostica yyyymmdd.txt*. Il limite di dimensioni di file predefinito è 10 MB e il numero massimo predefinito dei file mantenuti è 2. Il nome di blob predefinito è *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Per ulteriori informazioni sul comportamento predefinito, vedere [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).

Il provider funziona solo quando il progetto viene eseguito nell'ambiente Azure.  Non ha alcun effetto quando si esegue localmente &mdash; non vengono scritti in file locali o di archiviazione di sviluppo locale per i BLOB.

## <a name="third-party-logging-providers"></a>Provider di log di terze parti

Ecco alcuni Framework di registrazione di terze parti che utilizzano ASP.NET Core:

* [elmah.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -provider per il servizio Elmah.Io

* [JSNLog](http://jsnlog.com) -registra eccezioni JavaScript e altri eventi lato client nel registro sul lato server.

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -provider per il servizio Loggr

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) -provider per la libreria NLog

* [Serilog](https://github.com/serilog/serilog-framework-logging) -provider per la libreria Serilog

Alcuni framework di terze parti è possibile eseguire [registrazione semantica, nota anche come registrazione strutturata](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Utilizzare un framework di terze parti è simile all'utilizzo di uno dei provider predefiniti: aggiungere un pacchetto NuGet al progetto e chiamare un metodo di estensione su `ILoggerFactory`. Per ulteriori informazioni, vedere la documentazione del framework ogni.

È possibile creare i propri provider personalizzati, nonché per supportare i propri requisiti di registrazione o di altri framework di registrazione.
