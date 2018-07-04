---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guida all'API di ASP.NET SignalR Hubs - Server (c#) | Microsoft Docs
author: pfletcher
description: Questo documento viene fornita un'introduzione alla programmazione sul lato server dell'API di hub SignalR ASP.NET per SignalR versione 2, con esempi di codice...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c8814236495c3680ad648234f2d2507730f4f775
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382095"
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a>Guida all'API di ASP.NET SignalR Hubs - Server (c#)
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Questo documento fornisce un'introduzione alla programmazione sul lato server dell'API di hub SignalR ASP.NET per SignalR versione 2, con esempi di codice che illustrano le opzioni comuni.
> 
> L'API di hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server. Nel codice server, si definiscono i metodi che possono essere chiamati da parte dei client e si chiamano i metodi eseguiti nel client. Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi che eseguono sul server. SignalR si occupa di tutte le attività client-server.
> 
> SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR hub e connessioni permanenti, vedere [Introduzione a SignalR 2](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="topic-versions"></a>Versioni di argomento
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Come registrare il middleware di SignalR](#route)

    - [L'URL /signalr](#signalrurl)
    - [Configurazione delle opzioni di SignalR](#options)
- [Come creare e usare classi di Hub](#hubclass)

    - [Durata dell'oggetto hub](#transience)
    - [Notazione camel le maiuscole/minuscole dei nomi dell'Hub nel client JavaScript](#hubnames)
    - [Più hub](#multiplehubs)
    - [Hub fortemente tipizzati](#stronglytypedhubs)
- [Come definire i metodi nella classe Hub che i client possono chiamare](#hubmethods)

    - [Notazione camel le maiuscole/minuscole dei nomi di metodo nel client JavaScript](#methodnames)
    - [Quando è necessario eseguire in modo asincrono](#asyncmethods)
    - [La definizione di overload](#overloads)
    - [Creazione di report stato di avanzamento di chiamate del metodo dell'hub](#progress)
- [Come chiamare i metodi client dalla classe Hub](#callfromhub)

    - [Selezionando i client che riceverà la chiamata RPC](#selectingclients)
    - [Nessuna convalida in fase di compilazione per i nomi di metodo](#dynamicmethodnames)
    - [La corrispondenza dei nomi di metodo tra maiuscole e minuscole](#caseinsensitive)
    - [Esecuzione asincrona](#asyncclient)
- [Come gestire l'appartenenza al gruppo dalla classe dell'Hub](#groupsfromhub)

    - [Esecuzione asincrona dei metodi Add e Remove](#asyncgroupmethods)
    - [Persistenza di appartenenza al gruppo](#grouppersistence)
    - [Gruppi utente singolo](#singleusergroups)
- [Come gestire gli eventi di durata connessione nella classe Hub](#connectionlifetime)

    - [Quando vengono chiamati OnConnected OnDisconnected e OnReconnected](#onreconnected)
    - [Stato del chiamante non popolato](#nocallerstate)
- [Come ottenere informazioni relative al client dalla proprietà di contesto](#contextproperty)
- [Come passare lo stato tra i client e la classe dell'Hub](#passstate)
- [Come gestire gli errori nella classe Hub](#handleErrors)
- [Come chiamare i metodi di client e gestire i gruppi dall'esterno della classe dell'Hub](#callfromoutsidehub)

    - [Chiamata dei metodi client](#callingclientsoutsidehub)
    - [Gestire l'appartenenza al gruppo](#managinggroupsoutsidehub)
- [Come abilitare la traccia](#tracing)
- [Come personalizzare la pipeline di hub](#hubpipeline)

Per informazioni su come i client di programma, vedere le risorse seguenti:

- [Guida all'API di SignalR Hubs - Client JavaScript](hubs-api-guide-javascript-client.md)
- [Guida all'API di SignalR Hubs - Client .NET](hubs-api-guide-net-client.md)

Sono disponibili in .NET 4.5 solo i componenti server per SignalR 2. Server che eseguono .NET 4.0 è necessario usare v1.x SignalR.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Come registrare il middleware di SignalR

Per definire le route che i client useranno per connettersi all'hub, chiamare il `MapSignalR` metodo all'avvio dell'applicazione. `MapSignalR` è un' [metodo di estensione](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) per il `OwinExtensions` classe. Nell'esempio seguente viene illustrato come definire le route degli hub SignalR tramite una classe di avvio OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Se si aggiunge una funzionalità SignalR a un'applicazione ASP.NET MVC, assicurarsi che prima di altre route viene aggiunta la route di SignalR. Per altre informazioni, vedere [esercitazione: Introduzione a SignalR 2 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>L'URL /signalr

Per impostazione predefinita, l'URL di route che i client useranno per connettersi all'hub è "/ signalr". (Non confondere questo URL con l'URL "hub signalr /", ovvero per il file JavaScript generato automaticamente. Per altre informazioni sul proxy generato, vedere [Guida all'API di SignalR Hubs - JavaScript Client - proxy generato e che cosa fa per te](hubs-api-guide-javascript-client.md#genproxy).)

Potrebbero esserci casi straordinari che rendono questo URL di base non è utilizzabile per SignalR; ad esempio, è presente una cartella nel progetto denominato *signalr* e non si desidera modificare il nome. In tal caso, è possibile modificare l'URL di base, come illustrato negli esempi seguenti (sostituire "/ signalr" nel codice di esempio con il proprio URL desiderato).

**Codice del server che specifica l'URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Codice JavaScript client che specifica l'URL (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Codice JavaScript client che specifica l'URL (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Codice client .NET che specifica l'URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configurazione delle opzioni di SignalR

Esegue l'overload del `MapSignalR` metodo consentono di specificare un URL personalizzato, un resolver di dipendenza personalizzate e le opzioni seguenti:

- Abilitare le chiamate tra domini con JSONP o CORS da browser client.

    In genere se il browser ha caricato una pagina dalla `http://contoso.com`, la connessione SignalR è nello stesso dominio, in `http://contoso.com/signalr`. Se la pagina dalla `http://contoso.com` stabilisce una connessione a `http://fabrikam.com/signalr`, vale a dire una connessione tra domini. Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita. Per altre informazioni, vedere [Guida all'API di hub di ASP.NET SignalR - JavaScript Client - come stabilire una connessione cross-domain](hubs-api-guide-javascript-client.md#crossdomain).
- Abilitare i messaggi di errore dettagliato.

    Quando si verificano errori, il comportamento predefinito di SignalR consiste nell'inviare ai client un messaggio di notifica senza informazioni dettagliate su cosa è successo. L'invio di informazioni dettagliate sull'errore per i client non è consigliabile nell'ambiente di produzione, perché gli utenti malintenzionati potrebbero essere in grado di usare le informazioni per gli attacchi contro la tua applicazione. Per risolvere il problema, è possibile utilizzare questa opzione per abilitare temporaneamente la segnalazione di errori maggiori.
- Disabilita la funzionalità file proxy JavaScript generati automaticamente.

    Per impostazione predefinita, viene generato un file JavaScript con i proxy per le classi dell'Hub in risposta all'URL "/ signalr/hub". Se non si vuole usare i proxy di JavaScript, o se si desidera generare manualmente questo file e fare riferimento a un file fisico nel client, è possibile utilizzare questa opzione per disabilitare la generazione del proxy. Per altre informazioni, vedere [Guida all'API di SignalR Hubs - JavaScript Client - come creare un file fisico per SignalR generato proxy](hubs-api-guide-javascript-client.md#manualproxy).

Nell'esempio seguente viene illustrato come specificare l'URL di connessione SignalR e queste opzioni in una chiamata al `MapSignalR` (metodo). Per specificare un URL personalizzato, sostituire "/ signalr" nell'esempio con l'URL a cui si desidera utilizzare.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Come creare e usare classi di Hub

Per creare un Hub, creare una classe che deriva da [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). Nell'esempio seguente viene illustrata una semplice classe di Hub per un'applicazione di chat.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

In questo esempio, è possibile chiamare un client connesso di `NewContosoChatMessage` (metodo), e in questo caso, i dati ricevuti sono sono trasmesse a tutti i client connessi.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Durata dell'oggetto hub

Non si crea un'istanza della classe Hub o chiamarne i metodi dal codice del server. tutto ciò che avviene automaticamente tramite la pipeline degli hub SignalR. SignalR crea una nuova istanza della classe Hub ogni volta che deve gestire un'operazione dell'Hub, ad esempio quando un client si connette, si disconnette o effettua un chiamata di metodo nel server.

Poiché le istanze della classe Hub sono temporanee, non possono essere utilizzati per mantenere lo stato da una chiamata al metodo a quella successiva. Ogni volta che il server riceve una chiamata al metodo da un client in una nuova istanza dei processi di classe dell'Hub del messaggio. Per mantenere lo stato attraverso più connessioni e chiamate al metodo, usare un altro metodo, ad esempio un database o una variabile statica nella classe dell'Hub o un'altra classe non deriva da `Hub`. Se è rendere persistenti i dati in memoria, usando un metodo, ad esempio una variabile statica della classe dell'Hub, i dati andranno perse quando viene riciclato il dominio dell'applicazione.

Se si desidera inviare messaggi ai client dal codice che viene eseguito all'esterno della classe dell'Hub, non è possibile farlo creando un'istanza della classe dell'Hub, ma è possibile farlo tramite il recupero di un riferimento all'oggetto di contesto SignalR per la classe dell'Hub. Per altre informazioni, vedere [come client di chiamare metodi e gestire i gruppi dall'esterno della classe dell'Hub](#callfromoutsidehub) più avanti in questo argomento.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Notazione camel le maiuscole/minuscole dei nomi dell'Hub nel client JavaScript

Per impostazione predefinita, i client JavaScript fare riferimento a un hub usando una versione con notazione camel del nome della classe. SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript. Nell'esempio precedente potrebbe essere indicata come `contosoChatHub` nel codice JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**Client JavaScript tramite proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Se si desidera specificare un nome diverso per i client da usare, aggiungere il `HubName` attributo. Quando si usa un `HubName` attributo, non vi è alcuna modifica del nome in notazione camel nel client JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**Client JavaScript tramite proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Più hub

È possibile definire più classi Hub in un'applicazione. Quando si esegue questa operazione, la connessione è condivisa ma i gruppi sono separati:

- Tutti i client utilizzeranno lo stesso URL per stabilire una connessione di SignalR con il servizio ("/ signalr" o l'URL personalizzato se è stata specificata una), e viene utilizzata per tutti gli hub di connessione definite dal servizio.

    Non vi è alcuna differenza nelle prestazioni per più hub rispetto alla definizione di tutte le funzionalità dell'Hub in una singola classe.
- Tutti gli hub di ottenere le stesse informazioni di richiesta HTTP.

    Poiché tutti gli hub condividono la stessa connessione, l'unica informazione richiesta HTTP che il server ottiene è ciò che viene ricevuta la richiesta HTTP originale che stabilisce la connessione di SignalR. Se si usa la richiesta di connessione per passare le informazioni dal client al server, specificando una stringa di query, non è possibile fornire le stringhe di query diversi hub diversi. Tutti gli hub riceverà le stesse informazioni.
- Il file proxy JavaScript generato conterrà i proxy per tutti gli hub in un unico file.

    Per informazioni sui proxy JavaScript, vedere [Guida all'API di SignalR Hubs - JavaScript Client - proxy generato e che cosa fa per te](hubs-api-guide-javascript-client.md#genproxy).
- I gruppi vengono definiti all'interno di hub.

    In SignalR che è possibile definire gruppi per la trasmissione a subset di client connessi con nome. I gruppi vengono mantenuti separatamente per ogni Hub. Ad esempio, un gruppo denominato "Administrators" sono un set di client per il `ContosoChatHub` classi e lo stesso nome di gruppo per fare riferimento a un set diverso di client per il `StockTickerHub` classe.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Hub fortemente tipizzati

Per definire un'interfaccia per i metodi dell'hub che il client può essere riferimento e Abilita Intellisense sui metodi dell'hub, derivare hub indicato nel `Hub<T>` (introdotta in SignalR 2.1) anziché `Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Come definire i metodi nella classe Hub che i client possono chiamare

Per esporre un metodo dell'hub che si desidera essere richiamabile dal client, dichiarare un metodo pubblico, come illustrato negli esempi seguenti.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

È possibile specificare un tipo restituito e parametri, compresi i tipi complessi e le matrici, come si farebbe in qualsiasi metodo c#. Tutti i dati ricevuti nei parametri o restituire al chiamante vengono comunicati tra il client e server mediante JSON e SignalR gestisce automaticamente l'associazione di oggetti complessi e le matrici di oggetti.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Notazione camel le maiuscole/minuscole dei nomi di metodo nel client JavaScript

Per impostazione predefinita, i client JavaScript fare riferimento ai metodi dell'Hub usando una versione con notazione camel del nome del metodo. SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Client JavaScript tramite proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Se si desidera specificare un nome diverso per i client da usare, aggiungere il `HubMethodName` attributo.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Client JavaScript tramite proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Quando è necessario eseguire in modo asincrono

Se il metodo verrà essere a esecuzione prolungata o è necessario eseguire le operazioni che verrebbe comportano l'attesa, ad esempio una ricerca nel database o una chiamata al servizio web, rendere asincrono il metodo dell'Hub, restituendo un [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (invece di `void` restituire) o [ Task&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) oggetto (invece di `T` tipo restituito). Quando si torna una `Task` oggetto dal metodo, SignalR attende la `Task` completamento, e quindi invia il risultato annullato il wrapping al client, pertanto non presenta alcuna differenza nel modo in cui è codice la chiamata al metodo nel client.

Rendere un metodo dell'Hub asincrona consente di evitare blocchi la connessione quando utilizza il trasporto WebSocket. Quando un metodo dell'Hub esegue in modo sincrono e il trasporto WebSocket, le successive chiamate dei metodi dell'hub dallo stesso client vengono bloccate fino al completamento del metodo dell'Hub.

L'esempio seguente illustra il metodo di stesso codificate in modo da eseguire in modo sincrono o asincrono, seguito da codice JavaScript client che funziona per la chiamata a entrambe le versioni.

**Sincrono**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asincrono**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Client JavaScript tramite proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Per altre informazioni su come usare i metodi asincroni in ASP.NET 4.5, vedere [usando i metodi asincroni in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>La definizione di overload

Se si desidera definire gli overload per un metodo, il numero di parametri in ogni overload deve essere diverso. Se un overload si differenzia solo specificando i tipi di parametro diversi, la classe dell'Hub verrà compilate, ma il servizio SignalR genererà un'eccezione in fase di esecuzione quando i client di provano a chiamare uno degli overload.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Creazione di report stato di avanzamento di chiamate del metodo dell'hub

2.1 SignalR aggiunge il supporto per la [modello di report di stato](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introdotta in .NET 4.5. Per implementare i report di stato di avanzamento, definire un `IProgress<T>` parametro per il metodo dell'hub che il client può accedere a:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Quando si scrive un metodo di server con esecuzione prolungata, è importante usare un modello di programmazione asincrona, ad esempio Async / Await anziché bloccare il thread dell'hub.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Come chiamare i metodi client dalla classe Hub

Per chiamare i metodi di client dal server, usare il `Clients` proprietà in un metodo nella classe dell'Hub. L'esempio seguente illustra codice lato server che chiama `addNewMessageToPage` sui client connesse tutte le soluzioni e codice client che definisce il metodo in un client JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

**Client JavaScript tramite proxy generato**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

È possibile ottenere un valore restituito da un metodo client. la sintassi, ad esempio `int x = Clients.All.add(1,1)` non funziona.

È possibile specificare i tipi complessi e matrici per i parametri. Nell'esempio seguente passa un tipo complesso al client in un parametro del metodo.

**Codice del server che chiama un metodo client usando un oggetto complesso**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Codice del server che definisce l'oggetto complesso**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**Client JavaScript tramite proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Selezionando i client che riceverà la chiamata RPC

La proprietà restituisce ai client un [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) oggetto che fornisce diverse opzioni per specificare che i client riceveranno il RPC:

- Tutti i client connessi.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Solo il client chiamante.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Tutti i client, eccetto il client chiamante.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Un client specifico identificato dall'ID di connessione.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    Questo esempio viene chiamato `addContosoChatMessageToPage` sul client chiamante e ha lo stesso effetto dell'utilizzo `Clients.Caller`.
- Tutti i client connessi eccetto il client specificato, identificato dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Tutti i client in un gruppo specificato connessi.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Tutti i client connessi in un gruppo specificato eccetto il client specificato, identificato dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Tutti i client connessi in un gruppo specificato eccetto il client chiamante.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Un utente specifico, identificato dall'ID utente.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Per impostazione predefinita, si tratta `IPrincipal.Identity.Name`, ma può essere modificato dal [la registrazione di un'implementazione di IUserIdProvider con l'host globale](mapping-users-to-connections.md#IUserIdProvider).
- Tutti i client e i gruppi in un elenco di ID connessione.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Un elenco di gruppi.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Un utente in base al nome.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Un elenco di nomi utente (introdotta in SignalR 2.1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Nessuna convalida in fase di compilazione per i nomi di metodo

Il nome del metodo specificato viene interpretato come un oggetto dinamico, ovvero che non IntelliSense o convalida in fase di compilazione appositamente. L'espressione viene valutata in fase di esecuzione. Quando viene eseguita la chiamata al metodo, SignalR invia il nome del metodo e i valori dei parametri per il client, e se il client dispone di un metodo che corrisponde al nome, la chiamata a metodo e i valori dei parametri vengono passati a esso. Se non viene trovato alcun metodo corrispondente nel client, viene generato alcun errore. Per informazioni sul formato dei dati che SignalR trasmette al client dietro le quinte quando si chiama un metodo client, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>La corrispondenza dei nomi di metodo tra maiuscole e minuscole

Metodo nome viene applicata alcuna distinzione tra maiuscole e minuscole. Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, o `addContosoChatMessageToPage` sul client.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Esecuzione asincrona

Il metodo chiamato viene eseguito in modo asincrono. Qualsiasi codice che segue una chiamata al metodo a un client verrà eseguita immediatamente senza attendere SignalR completare la trasmissione dei dati per i client a meno che non si specifica che le successive righe di codice deve attendere il completamento di metodo. Esempio di codice seguente viene illustrato come eseguire i due metodi di client in modo sequenziale.

**Si attende (.NET 4.5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Se si usa `await` per attendere fino al completamento di un metodo client prima dell'esecuzione riga di codice successiva, ciò non significa che i client effettivamente riceverà il messaggio prima dell'esecuzione riga di codice successiva. "Completamento" di una chiamata al metodo client significa solo che ha eseguito tutto il necessario per inviare il messaggio SignalR. Se è necessario che i client ha ricevuto il messaggio di verifica, è necessario programmare tale meccanismo manualmente. Ad esempio, è possibile codificare una `MessageReceived` metodo dell'hub e nel `addContosoChatMessageToPage` metodo sul client è possibile chiamare `MessageReceived` al termine dell'operazione è qualsiasi elemento di lavoro da eseguire sul client. In `MessageReceived` nell'Hub è possibile eseguire le operazioni dipende da client effettiva ricezione e l'elaborazione della chiamata al metodo originale.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Come usare una variabile di stringa come il nome del metodo

Se si vuole richiamare un metodo client utilizzando una variabile di stringa come il nome del metodo, eseguire il cast `Clients.All` (o `Clients.Others`, `Clients.Caller`e così via) per `IClientProxy` e quindi chiamare [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Come gestire l'appartenenza al gruppo dalla classe dell'Hub

Gruppi in SignalR forniscono un metodo per la trasmissione di messaggi per sottoinsiemi specifici di client connessi. Un gruppo può avere qualsiasi numero di client e un client può essere un membro di un numero qualsiasi di gruppi.

Per gestire l'appartenenza al gruppo, usare il [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) e [rimuovere](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metodi forniti dal `Groups` proprietà della classe dell'Hub. L'esempio seguente mostra le `Groups.Add` e `Groups.Remove` metodi usati nei metodi dell'Hub che vengono chiamati dal codice client, seguito da codice JavaScript client che li chiama.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**Client JavaScript tramite proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Non è necessario creare in modo esplicito gruppi. In effetti un gruppo viene creato automaticamente la prima volta specificarne il nome in una chiamata a `Groups.Add`, e viene eliminata quando si rimuove l'ultima connessione dall'appartenenza al suo interno.

Non è disponibile alcuna API per ottenere un elenco di appartenenza al gruppo o un elenco di gruppi. SignalR invia messaggi al client e i gruppi basati su un [modello pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), e il server non venga mantenuto un elenco di gruppi o appartenenze ai gruppi. Ciò consente di ottimizzare la scalabilità, perché ogni volta che si aggiunge un nodo a una web farm, qualsiasi stato che gestisce SignalR deve essere propagata nel nuovo nodo.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Esecuzione asincrona dei metodi Add e Remove

Il `Groups.Add` e `Groups.Remove` metodi eseguiti in modo asincrono. Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il `Groups.Add` metodo termina per prima. Esempio di codice seguente viene illustrato come eseguire questa operazione.

**Aggiunta di un client a un gruppo e quindi il client di messaggistica**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistenza di appartenenza al gruppo

SignalR tiene traccia delle connessioni, non per gli utenti, pertanto se si desidera che un utente sia nello stesso gruppo ogni volta che l'utente definisce una connessione, è necessario chiamare `Groups.Add` ogni volta che l'utente definisce una nuova connessione.

Dopo una perdita temporanea della connettività, in alcuni casi SignalR possibile ripristinare la connessione automaticamente. In tal caso, SignalR sta ripristinando la stessa connessione, non stabilire una nuova connessione e quindi, vengono ripristinato automaticamente l'appartenenza al gruppo del client. Questo è possibile anche quando l'interruzione temporanea è il risultato di un riavvio del server o di errore, perché lo stato della connessione per ogni client, incluse le appartenenze di gruppo, viene sottoposto a round trip al client. Se un server si arresta e viene sostituito da un nuovo server prima del timeout della connessione, un client possa riconnettersi automaticamente al nuovo server e nuovamente la registrazione è un membro dei gruppi.

Quando una connessione non può essere ripristinata automaticamente dopo una perdita di connettività, o quando si verifica il timeout della connessione o quando il client si disconnette (ad esempio, quando un browser consente di passare a una nuova pagina), le appartenenze di gruppo vengono perse. Al successivo si connette l'utente sarà una nuova connessione. Per mantenere appartenenze quando lo stesso utente stabilisce una nuova connessione, l'applicazione deve rilevare le associazioni tra utenti e gruppi e ripristinare le appartenenze di gruppo ogni volta che un utente stabilisce una nuova connessione.

Per altre informazioni sulle connessioni e le riconnessioni, vedere [come gestire gli eventi di durata connessione nella classe Hub](#connectionlifetime) più avanti in questo argomento.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Gruppi utente singolo

Le applicazioni che utilizzano in genere SignalR devono tenere traccia delle associazioni tra utenti e connessioni per sapere quale utente ha inviato un messaggio e che gli utenti dovrebbero ricevere un messaggio. Gruppi vengono usati in uno dei due modelli di usati comune per questo scopo.

- Gruppi utente singolo.

    È possibile specificare il nome utente come nome del gruppo e aggiungere l'ID di connessione corrente al gruppo ogni volta che l'utente si connette o si riconnette. Per inviare messaggi all'utente di trasmissione al gruppo. Uno svantaggio di questo metodo è che il gruppo non offre un modo per verificare se l'utente è online o offline.
- Rilevare le associazioni tra i nomi utente e ID di connessione.

    È possibile archiviare un'associazione tra ogni nome utente e ID di connessione una o più in un dizionario o un database e aggiornare i dati archiviati ogni volta che l'utente si connette o disconnette. Per inviare messaggi all'utente specificare l'ID di connessione. Uno svantaggio di questo metodo è che sono necessari più memoria.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Come gestire gli eventi di durata connessione nella classe Hub

Motivi tipici per la gestione degli eventi di durata connessione sono per consentire di controllare se un utente è connesso o meno e di tenere traccia dell'associazione tra i nomi utente e ID di connessione. Per eseguire codice personalizzato quando i client di connettere o disconnettere, eseguire l'override di `OnConnected`, `OnDisconnected`, e `OnReconnected` classe i metodi virtuali dell'Hub, come illustrato nell'esempio seguente.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Quando vengono chiamati OnConnected OnDisconnected e OnReconnected

Ogni volta che un browser consente di passare a una nuova pagina, una nuova connessione deve essere stabilita, ovvero SignalR eseguirà il `OnDisconnected` metodo aggiungendo il `OnConnected` (metodo). SignalR crea sempre un nuovo ID di connessione quando viene stabilita una nuova connessione.

Il `OnReconnected` metodo viene chiamato quando si verifica un'interruzione temporanea della connettività che SignalR può automaticamente ripristinare, ad esempio quando un cavo è temporaneamente disconnesso e riconnesso prima del timeout della connessione. Il `OnDisconnected` metodo viene chiamato quando il client viene disconnesso e SignalR non è possibile riconnettersi automaticamente, ad esempio quando un browser passa a una nuova pagina. Pertanto, è una sequenza di eventi per un determinato client possibili `OnConnected`, `OnReconnected`, `OnDisconnected`; oppure `OnConnected`, `OnDisconnected`. Non verrà visualizzata la sequenza `OnConnected`, `OnDisconnected`, `OnReconnected` per una determinata connessione.

Il `OnDisconnected` il dominio dell'applicazione ottiene riciclato o metodo non può essere chiamato in alcuni scenari, ad esempio quando un server si arresta. Quando un altro server è online o il dominio dell'applicazione completato il riciclo, alcuni client potrebbero essere in grado di ristabilire la connessione e vengono attivati i `OnReconnected` evento.

Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione in SignalR](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Stato del chiamante non popolato

Vengono chiamati i metodi del gestore eventi Durata connessione dal server, il che significa che qualsiasi stato che si inserisce nel `state` oggetti nel client non verranno popolati nel `Caller` proprietà sul server. Per informazioni sul `state` oggetto e il `Caller` proprietà, vedere [come passare lo stato tra i client e la classe Hub](#passstate) più avanti in questo argomento.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Come ottenere informazioni relative al client dalla proprietà di contesto

Per ottenere informazioni sul client, usare il `Context` proprietà della classe dell'Hub. Il `Context` proprietà restituisce un [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) oggetto che fornisce l'accesso alle informazioni seguenti:

- L'ID di connessione del client chiamante.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    L'ID di connessione è un GUID assegnato da SignalR (è possibile specificare il valore nel codice). È un ID di connessione per ogni connessione e la stessa connessione che ID viene utilizzato da tutti gli hub, se si dispone di più hub nell'applicazione.
- Dati dell'intestazione HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    È anche possibile ottenere le intestazioni HTTP dalla `Context.Headers`. Il motivo più riferimenti allo stesso concetto è che `Context.Headers` è stato creato prima di tutto la `Context.Request` è stata aggiunta in un secondo momento, proprietà e `Context.Headers` è stata mantenuta per compatibilità con le versioni precedenti.
- Eseguire query sui dati di tipo stringa.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    È anche possibile ottenere i dati di stringa di query da `Context.QueryString`.

    La stringa di query che si ottiene questa proprietà è quello che è stato usato con la richiesta HTTP che la connessione SignalR stabilita. È possibile aggiungere parametri della stringa di query nel client tramite la configurazione della connessione, che è un modo pratico per passare i dati relativi ai client dal client al server. Nell'esempio seguente viene illustrato un modo per aggiungere una stringa di query in un client JavaScript quando si usa il proxy generato.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Per altre informazioni sull'impostazione di parametri della stringa di query, vedere le guide di API per il [JavaScript](hubs-api-guide-javascript-client.md) e [.NET](hubs-api-guide-net-client.md) client.

    È possibile trovare il metodo di trasporto utilizzato per la connessione dei dati di stringa di query, con alcuni altri valori utilizzati internamente da SignalR:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    Il valore di `transportMethod` sarà "WebSocket", "serverSentEvents", "foreverFrame" o "longPolling". Si noti che se si archivia il valore di `OnConnected` metodo del gestore eventi, in alcuni scenari è inizialmente potrebbe ottenere un valore di trasporto che non sia il metodo di trasporto negoziata finale per la connessione. In tal caso il metodo genererà un'eccezione e verrà chiamato nuovamente in un secondo momento quando il metodo di trasporto finale viene stabilito.
- Cookie.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    È anche possibile ottenere i cookie provenienti da `Context.RequestCookies`.
- Informazioni sull'utente.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- L'oggetto HttpContext per la richiesta:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Usare questo metodo anziché ottenere `HttpContext.Current` per ottenere il `HttpContext` oggetto per la connessione di SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Come passare lo stato tra i client e la classe dell'Hub

Il proxy client fornisce un `state` oggetto in cui è possibile archiviare i dati che si desidera essere trasmessi al server con ogni chiamata al metodo. Nel server è possibile accedere a questi dati nel `Clients.Caller` proprietà nei metodi dell'Hub che vengono chiamati dal client. Il `Clients.Caller` proprietà non viene popolata per i metodi del gestore eventi Durata connessione `OnConnected`, `OnDisconnected`, e `OnReconnected`.

La creazione o aggiornamento dei dati nel `state` oggetto e il `Clients.Caller` proprietà funziona in entrambe le direzioni. È possibile aggiornare i valori nel server e vengono passati al client.

Nell'esempio seguente viene illustrato il codice client JavaScript che archivia lo stato per la trasmissione al server con ogni chiamata al metodo.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

Nell'esempio seguente mostra il codice equivalente in un client .NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

Nella classe Hub, è possibile accedere a questi dati nel `Clients.Caller` proprietà. Nell'esempio seguente viene illustrato il codice che recupera lo stato definito nell'esempio precedente.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Questo meccanismo di persistenza dello stato non è progettato per grandi quantità di dati, poiché tutto ciò che si inserisce nel `state` o `Clients.Caller` proprietà è sottoposto a round trip con ogni chiamata al metodo. È utile per gli elementi più piccoli, ad esempio nomi utente o i contatori.


In Visual Basic.NET o in un hub fortemente tipizzato, l'oggetto di stato del chiamante non sono accessibili attraverso `Clients.Caller`; in alternativa, usare `Clients.CallerState` (introdotta in SignalR 2.1):

**Utilizzo CallerState in c#**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Utilizzo CallerState in Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Come gestire gli errori nella classe Hub

Per gestire gli errori che si verificano nei metodi di classe dell'Hub, usare uno o più dei metodi seguenti:

- Eseguire il wrapping del codice del metodo in blocchi try-catch e accedere all'oggetto eccezione. A scopo di debug è possibile inviare l'eccezione al client, ma per la sicurezza non sono consigliabile motivi l'invio di informazioni dettagliate per i client nell'ambiente di produzione.
- Creare un modulo di hub della pipeline che gestisce il [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) (metodo). Nell'esempio seguente mostra un modulo di pipeline che registra gli errori, seguiti dal codice nel file Startup.cs che inserisce il modulo nella pipeline di hub.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Usare il `HubException` classe (introdotta in SignalR 2). Questo errore può essere generato da qualsiasi chiamata dell'hub. Il `HubError` costruttore accetta una stringa di messaggio e un oggetto per l'archiviazione dei dati aggiuntivi errore. SignalR verrà auto-serializzare l'eccezione e inviarlo al client, in cui utilizzarlo per rifiutare o interrompere la chiamata al metodo dell'hub.

    Gli esempi di codice seguenti illustrano come generare un `HubException` durante una chiamata dell'Hub e su come gestire l'eccezione nel client .NET e JavaScript.

    **Codice del server per illustrare la classe HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Codice JavaScript client dimostrazione della risposta alla generazione di un HubException in un hub**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Codice client .NET dimostrazione della risposta alla generazione di un HubException in un hub**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Per altre informazioni sui moduli di Hub della pipeline, vedere [come personalizzare la pipeline hub](#hubpipeline) più avanti in questo argomento.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Come abilitare la traccia

Per abilitare la traccia sul lato server, aggiungere un elemento System. Diagnostics al file Web. config, come illustrato in questo esempio:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Quando si esegue l'applicazione in Visual Studio, è possibile visualizzare i log nel **Output** finestra.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Come chiamare i metodi di client e gestire i gruppi dall'esterno della classe dell'Hub

Per chiamare i metodi di client da una classe diversa rispetto a classe Hub, ottenere un riferimento all'oggetto di contesto SignalR per l'Hub e usarlo per chiamare metodi sul client o gestire gruppi.

L'esempio seguente `StockTicker` classe ottiene l'oggetto di contesto, archiviarli in un'istanza della classe, archivia l'istanza della classe in una proprietà statica e Usa il contesto dall'istanza della classe singleton per chiamare il `updateStockPrice` sul client che si trovano (metodo) connesso a un Hub denominato `StockTickerHub`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Se è necessario usare i tempi di più rapida in un oggetto di lunga duraturo, ottenere il riferimento a una sola volta e salvare, anziché ottenerlo nuovamente ogni volta. Recupero del contesto di una volta garantisce che SignalR invia messaggi ai client nella stessa sequenza in cui i metodi dell'Hub sul rendono client chiamate al metodo. Per un'esercitazione che illustra come usare il contesto di SignalR per un Hub, vedere [trasmissione Server con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Chiamata dei metodi client

È possibile specificare che i client riceveranno il RPC, ma sono disponibili meno opzioni rispetto a quando si chiama da una classe dell'Hub. Il motivo è che il contesto non è associato a una particolare chiamata da un client, in modo che tutti i metodi che richiedono la conoscenza di ID connessione corrente, ad esempio `Clients.Others`, oppure `Clients.Caller`, o `Clients.OthersInGroup`, non sono disponibili. Sono disponibili le seguenti opzioni:

- Tutti i client connessi.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Un client specifico identificato dall'ID di connessione.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Tutti i client connessi eccetto il client specificato, identificato dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Tutti i client in un gruppo specificato connessi.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Client connessi tutti in un gruppo specificato, eccetto i client specificati, identificato dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Se si chiama nella classe non-Hub dai metodi nella classe Hub, è possibile passare l'ID di connessione corrente e usarlo con `Clients.Client`, `Clients.AllExcept`, o `Clients.Group` simulare `Clients.Caller`, `Clients.Others`, o `Clients.OthersInGroup`. Nell'esempio seguente, il `MoveShapeHub` classe passa l'ID connessione per il `Broadcaster` classe in modo che le `Broadcaster` classe consente di simulare `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Gestire l'appartenenza al gruppo

Per gestire i gruppi sono disponibili le opzioni stesso come in una classe dell'Hub.

- Aggiungere un client a un gruppo

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Rimuovere un client da un gruppo

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Come personalizzare la pipeline di hub

SignalR consente di inserire codice personalizzato nella pipeline dell'Hub. Nell'esempio seguente mostra un modulo di pipeline Hub personalizzato che registra ogni chiamata al metodo in ingresso ricevuto dal client e in uscita chiamata al metodo richiamato nel client:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

Il codice seguente il *Startup.cs* file registra il modulo per l'esecuzione della pipeline dell'Hub:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Esistono diversi metodi che è possibile eseguire l'override. Per un elenco completo, vedere [HubPipelineModule metodi](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
