---
title: Gestione degli errori nelle app ASP.NET Core Blazer
author: guardrex
description: Scopri in che modo ASP.NET Core Blazer gestisce le eccezioni non gestite e come sviluppare app che rilevano e gestiscono gli errori.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: blazor/handle-errors
ms.openlocfilehash: afcaa4d926c3e5f0a018897ce4b67b54574dae77
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426987"
---
# <a name="handle-errors-in-aspnet-core-blazor-apps"></a>Gestione degli errori nelle app ASP.NET Core Blazer

Di [Steve Sanderson](https://github.com/SteveSandersonMS)

Questo articolo descrive come blazer gestisce le eccezioni non gestite e come sviluppare app che rilevano e gestiscono gli errori.

## <a name="how-the-blazor-framework-reacts-to-unhandled-exceptions"></a>Reazione del Framework Blazer alle eccezioni non gestite

Il server blazer è un Framework con stato. Mentre gli utenti interagiscono con un'app, mantengono una connessione al server noto come *circuito*. Il circuito include istanze di componenti attive, oltre a molti altri aspetti dello stato, ad esempio:

* Output più recente sottoposto a rendering dei componenti.
* Set corrente di delegati di gestione degli eventi che possono essere attivati da eventi lato client.

Se un utente apre l'app in più schede del browser, avrà più circuiti indipendenti.

Blazer considera la maggior parte delle eccezioni non gestite come fatali per il circuito in cui si verificano. Se un circuito viene terminato a causa di un'eccezione non gestita, l'utente può continuare a interagire con l'app ricaricando la pagina per creare un nuovo circuito. I circuiti al di fuori di quello terminato, ovvero circuiti per altri utenti o altre schede del browser, non sono interessati. Questo scenario è simile a un'app desktop che si arresta in modo anomalo&mdash;l'app arrestata in modo anomalo deve essere riavviata, ma non sono interessate altre app.

Un circuito viene terminato quando si verifica un'eccezione non gestita per i motivi seguenti:

* Un'eccezione non gestita spesso lascia il circuito in uno stato non definito.
* L'operazione normale dell'app non può essere garantita dopo un'eccezione non gestita.
* Le vulnerabilità di sicurezza possono essere visualizzate nell'app se il circuito continua.

## <a name="manage-unhandled-exceptions-in-developer-code"></a>Gestire le eccezioni non gestite nel codice dello sviluppatore

Per continuare l'applicazione dopo un errore, l'app deve avere la logica di gestione degli errori. Le sezioni successive di questo articolo descrivono le potenziali fonti di eccezioni non gestite.

In produzione, non eseguire il rendering dei messaggi di eccezione del Framework o delle analisi dello stack nell'interfaccia utente. Il rendering dei messaggi di eccezione o delle analisi dello stack potrebbe:

* Divulgare informazioni riservate agli utenti finali.
* Aiutare un utente malintenzionato a individuare i punti deboli in un'app che può compromettere la sicurezza dell'app, del server o della rete.

## <a name="log-errors-with-a-persistent-provider"></a>Registrare gli errori con un provider persistente

Se si verifica un'eccezione non gestita, l'eccezione viene registrata in <xref:Microsoft.Extensions.Logging.ILogger> istanze configurate nel contenitore dei servizi. Per impostazione predefinita, le app Blaze registrano nell'output della console con il provider di registrazione della console. Prendere in considerazione la registrazione a una posizione più permanente con un provider che gestisce le dimensioni del log e la rotazione del log. Per ulteriori informazioni, vedere <xref:fundamentals/logging/index>.

Durante lo sviluppo, blazer invia in genere i dettagli completi delle eccezioni alla console del browser per facilitare il debug. In produzione, gli errori dettagliati nella console del browser sono disabilitati per impostazione predefinita, il che significa che gli errori non vengono inviati ai client, ma i dettagli completi dell'eccezione sono ancora registrati sul lato server. Per ulteriori informazioni, vedere <xref:fundamentals/error-handling>.

È necessario decidere quali eventi imprevisti registrare e il livello di gravità degli eventi imprevisti registrati. Gli utenti ostili potrebbero essere in grado di attivare intenzionalmente gli errori. Ad esempio, non registrare un evento imprevisto da un errore in cui viene fornito un `ProductId` sconosciuto nell'URL di un componente che Visualizza i dettagli del prodotto. Non tutti gli errori devono essere considerati come eventi imprevisti con gravità elevata per la registrazione.

## <a name="places-where-errors-may-occur"></a>Posizioni in cui possono verificarsi errori

Il Framework e il codice dell'app possono attivare eccezioni non gestite in uno dei percorsi seguenti:

* [Creazione di istanze di componenti](#component-instantiation)
* [Metodi del ciclo di vita](#lifecycle-methods)
* [Logica di rendering](#rendering-logic)
* [Gestori eventi](#event-handlers)
* [Eliminazione componenti](#component-disposal)
* [Interoperabilità JavaScript](#javascript-interop)
* [Gestori del circuito](#circuit-handlers)
* [Eliminazione del circuito](#circuit-disposal)
* [Tempistiche](#prerendering)

Le eccezioni non gestite precedenti sono descritte nelle sezioni seguenti di questo articolo.

### <a name="component-instantiation"></a>Creazione di istanze di componenti

Quando Blazer crea un'istanza di un componente:

* Il costruttore del componente viene richiamato.
* I costruttori di tutti i servizi non singleton forniti al costruttore del componente tramite la direttiva [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) o l'attributo [[Inject]](xref:blazor/dependency-injection#request-a-service-in-a-component) vengono richiamati. 

Un circuito ha esito negativo quando un costruttore eseguito o un setter per qualsiasi proprietà `[Inject]` genera un'eccezione non gestita. L'eccezione è irreversibile perché il Framework non è in grado di creare un'istanza del componente. Se la logica del costruttore può generare eccezioni, l'app deve intercettare le eccezioni usando un'istruzione [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con gestione e registrazione degli errori.

### <a name="lifecycle-methods"></a>Metodi del ciclo di vita

Durante la durata di un componente, blazer richiama i metodi del ciclo di vita:

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

Se un metodo del ciclo di vita genera un'eccezione, in modo sincrono o asincrono, l'eccezione è fatale per il circuito. Per i componenti che gestiscono gli errori nei metodi del ciclo di vita, aggiungere la logica di gestione degli errori.

Nell'esempio seguente `OnParametersSetAsync` chiama un metodo per ottenere un prodotto:

* Un'eccezione generata nel metodo `ProductRepository.GetProductByIdAsync` viene gestita da un'istruzione `try-catch`.
* Quando viene eseguito il blocco `catch`:
  * `loadFailed` è impostato su `true`, che consente di visualizzare un messaggio di errore all'utente.
  * L'errore viene registrato.

[!code-cshtml[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a>Logica di rendering

Il markup dichiarativo in un file componente `.razor` viene compilato C# in un metodo denominato `BuildRenderTree`. Quando viene eseguito il rendering di un componente, `BuildRenderTree` esegue e compila una struttura di dati che descrive gli elementi, il testo e i componenti figlio del componente di cui è stato eseguito il rendering.

La logica di rendering può generare un'eccezione. Un esempio di questo scenario si verifica quando viene valutato `@someObject.PropertyName` ma viene `null``@someObject`. Un'eccezione non gestita generata dalla logica di rendering è irreversibile per il circuito.

Per evitare un'eccezione di riferimento null nella logica di rendering, verificare la presenza di un oggetto `null` prima di accedere ai relativi membri. Nell'esempio seguente, non è possibile accedere alle proprietà `person.Address` se `person.Address` è `null`:

[!code-cshtml[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

Il codice precedente presuppone che `person` non sia `null`. Spesso, la struttura del codice garantisce la presenza di un oggetto nel momento in cui viene eseguito il rendering del componente. In questi casi, non è necessario verificare la presenza di `null` nella logica di rendering. Nell'esempio precedente `person` possibile che esista una garanzia perché `person` viene creato quando viene creata un'istanza del componente.

### <a name="event-handlers"></a>Gestori eventi

Il codice lato client attiva le chiamate del C# codice quando i gestori eventi vengono creati utilizzando:

* `@onclick`
* `@onchange`
* Altri attributi di `@on...`
* `@bind`

Il codice del gestore eventi potrebbe generare un'eccezione non gestita in questi scenari.

Se un gestore eventi genera un'eccezione non gestita, ad esempio una query di database ha esito negativo, l'eccezione è fatale per il circuito. Se l'app chiama il codice che potrebbe non riuscire per motivi esterni, intercettare le eccezioni usando un'istruzione [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con gestione e registrazione degli errori.

Se il codice utente non intercetta e gestisce l'eccezione, il Framework registra l'eccezione e termina il circuito.

### <a name="component-disposal"></a>Eliminazione componenti

Un componente può essere rimosso dall'interfaccia utente, ad esempio perché l'utente ha esplorato un'altra pagina. Quando un componente che implementa <xref:System.IDisposable?displayProperty=fullName> viene rimosso dall'interfaccia utente, il Framework chiama il metodo di <xref:System.IDisposable.Dispose*> del componente. 

Se il metodo `Dispose` del componente genera un'eccezione non gestita, l'eccezione è fatale per il circuito. Se la logica di eliminazione può generare eccezioni, l'app deve intercettare le eccezioni usando un'istruzione [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con gestione e registrazione degli errori.

Per ulteriori informazioni sull'eliminazione dei componenti, vedere <xref:blazor/components#component-disposal-with-idisposable>.

### <a name="javascript-interop"></a>Interoperabilità JavaScript

`IJSRuntime.InvokeAsync<T>` consente al codice .NET di effettuare chiamate asincrone al runtime JavaScript nel browser dell'utente.

Le condizioni seguenti si applicano alla gestione degli errori con `InvokeAsync<T>`:

* Se una chiamata a `InvokeAsync<T>` ha esito negativo in modo sincrono, si verifica un'eccezione .NET. Una chiamata a `InvokeAsync<T>` potrebbe non riuscire, ad esempio perché gli argomenti forniti non possono essere serializzati. Il codice dello sviluppatore deve intercettare l'eccezione. Se il codice dell'app in un gestore eventi o in un metodo del ciclo di vita dei componenti non gestisce un'eccezione, l'eccezione risultante è fatale per il circuito.
* Se una chiamata a `InvokeAsync<T>` ha esito negativo in modo asincrono, l'<xref:System.Threading.Tasks.Task> .NET non riesce. Una chiamata a `InvokeAsync<T>` potrebbe non riuscire, ad esempio perché il codice sul lato JavaScript genera un'eccezione o restituisce una `Promise` completata come `rejected`. Il codice dello sviluppatore deve intercettare l'eccezione. Se si usa l'operatore [await](/dotnet/csharp/language-reference/keywords/await) , è consigliabile eseguire il wrapping della chiamata al metodo in un'istruzione [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con gestione e registrazione degli errori. In caso contrario, il codice in errore genera un'eccezione non gestita che è irreversibile per il circuito.
* Per impostazione predefinita, le chiamate a `InvokeAsync<T>` devono essere completate entro un determinato periodo oppure si verifica il timeout della chiamata. Il periodo di timeout predefinito è di un minuto. Il timeout protegge il codice da una perdita di connettività di rete o codice JavaScript che non restituisce mai un messaggio di completamento. Se si verifica il timeout della chiamata, l'`Task` risultante ha esito negativo con un <xref:System.OperationCanceledException>. Intercettare ed elaborare l'eccezione con la registrazione.

Analogamente, il codice JavaScript può avviare chiamate a metodi .NET indicati dall' [attributo [JSInvokable]](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions). Se questi metodi .NET generano un'eccezione non gestita:

* L'eccezione non viene trattata come irreversibile per il circuito.
* Il `Promise` lato JavaScript viene rifiutato.

È possibile scegliere di usare il codice di gestione degli errori sul lato .NET o sul lato JavaScript della chiamata al metodo.

Per ulteriori informazioni, vedere <xref:blazor/javascript-interop>.

### <a name="circuit-handlers"></a>Gestori del circuito

Blazer consente al codice di definire un *gestore di circuito*, che riceve le notifiche quando cambia lo stato del circuito di un utente. Vengono utilizzati gli Stati seguenti:

* `initialized`
* `connected`
* `disconnected`
* `disposed`

Le notifiche vengono gestite registrando un servizio DI che eredita dalla classe di base astratta `CircuitHandler`.

Se i metodi di un gestore di circuito personalizzato generano un'eccezione non gestita, l'eccezione è fatale per il circuito. Per tollerare le eccezioni nel codice di un gestore o i metodi chiamati, eseguire il wrapping del codice in una o più istruzioni [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con gestione e registrazione degli errori.

### <a name="circuit-disposal"></a>Eliminazione del circuito

Quando un circuito termina perché un utente si è disconnesso e il Framework pulisce lo stato del circuito, il Framework Elimina l'ambito di. Con l'eliminazione dell'ambito vengono eliminati tutti i servizi con ambito DI circuito che implementano <xref:System.IDisposable?displayProperty=fullName>. Se un servizio DI INSERIMENTO DI dipendenze genera un'eccezione non gestita durante l'eliminazione, il Framework registra l'eccezione.

### <a name="prerendering"></a>Tempistiche

È possibile eseguire il prerendering dei componenti di Blazer usando `Html.RenderComponentAsync` in modo che il markup HTML sottoposto a rendering venga restituito come parte della richiesta HTTP iniziale dell'utente. Funziona per:

* Creazione di un nuovo circuito per tutti i componenti di cui è stato eseguito il rendering che fanno parte della stessa pagina.
* Generazione del codice HTML iniziale.
* Trattare il circuito come `disconnected` fino a quando il browser dell'utente non stabilisce una connessione SignalR allo stesso server. Quando viene stabilita la connessione, interattività sul circuito viene ripresa e il markup HTML dei componenti viene aggiornato.

Se un componente genera un'eccezione non gestita durante il prerendering, ad esempio durante un metodo del ciclo di vita o nella logica di rendering:

* L'eccezione è fatale per il circuito.
* L'eccezione viene generata dallo stack di chiamate dalla chiamata `Html.RenderComponentAsync`. Pertanto, l'intera richiesta HTTP ha esito negativo a meno che l'eccezione non venga intercettata in modo esplicito dal codice dello sviluppatore.

In circostanze normali, quando si verifica un errore di prerendering, continuare a compilare ed eseguire il rendering del componente non ha senso perché non è possibile eseguire il rendering di un componente funzionante.

Per tollerare gli errori che possono verificarsi durante il prerendering, la logica di gestione degli errori deve essere inserita all'interno di un componente che può generare eccezioni. Usare le istruzioni [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con la gestione e la registrazione degli errori. Anziché eseguire il wrapping della chiamata a `RenderComponentAsync` in un'istruzione `try-catch`, inserire la logica di gestione degli errori nel componente sottoposto a rendering da `RenderComponentAsync`.

## <a name="advanced-scenarios"></a>Scenari avanzati

### <a name="recursive-rendering"></a>Rendering ricorsivo

I componenti possono essere annidati in modo ricorsivo. Questa operazione è utile per la rappresentazione di strutture di dati ricorsive. Un componente `TreeNode`, ad esempio, può eseguire il rendering di più componenti `TreeNode` per ognuno dei figli del nodo.

Quando si esegue il rendering in modo ricorsivo, evitare i modelli di codifica che generano una ricorsione

* Non esegue il rendering in modo ricorsivo di una struttura di dati che contiene un ciclo. Ad esempio, non eseguire il rendering di un nodo dell'albero i cui elementi figlio includono se stesso.
* Non creare una catena di layout che contiene un ciclo. Non creare, ad esempio, un layout il cui layout è a sua volta.
* Non consentire a un utente finale di violare le invarianti di ricorsione (regole) tramite immissione di dati dannosi o chiamate di interoperabilità JavaScript.

Cicli infiniti durante il rendering:

* Causa la continuazione del processo di rendering.
* Equivale alla creazione di un ciclo senza terminazione.

In questi scenari, il circuito interessato si blocca e il thread in genere tenta di:

* Utilizzare la quantità di tempo della CPU consentita dal sistema operativo, per un periodo illimitato.
* Utilizzare una quantità illimitata di memoria del server. L'utilizzo di memoria illimitata equivale allo scenario in cui un ciclo senza terminazione aggiunge voci a una raccolta in ogni iterazione.

Per evitare modelli di ricorsione infinita, verificare che il codice di rendering ricorsivo contenga condizioni di arresto appropriate.

### <a name="custom-render-tree-logic"></a>Logica dell'albero di rendering personalizzata

La maggior parte dei componenti Blazer viene implementata come file con *estensione Razor* e viene compilata per produrre la logica che opera su un `RenderTreeBuilder` per eseguire il rendering dell'output. Uno sviluppatore può implementare manualmente `RenderTreeBuilder` logica usando il C# codice procedurale. Per ulteriori informazioni, vedere <xref:blazor/components#manual-rendertreebuilder-logic>.

> [!WARNING]
> L'uso della logica del generatore di albero di rendering manuale è considerato uno scenario avanzato e non sicuro, non consigliato per lo sviluppo di componenti generali.

Se `RenderTreeBuilder` codice viene scritto, lo sviluppatore deve garantire la correttezza del codice. Ad esempio, lo sviluppatore deve garantire quanto segue:

* Le chiamate a `OpenElement` e `CloseElement` sono bilanciate correttamente.
* Gli attributi vengono aggiunti solo nei punti corretti.

La logica del generatore di albero di rendering manuale non corretta può causare un comportamento arbitrario non definito, tra cui arresti anomali, blocchi del server e vulnerabilità della sicurezza.

Prendere in considerazione la logica del generatore di albero di rendering manuale sullo stesso livello di complessità e con lo stesso livello di *rischio* di scrivere manualmente codice assembly o istruzioni MSIL.
