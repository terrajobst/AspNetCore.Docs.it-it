---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guida di ASP.NET SignalR hub API - Server (c#) | Documenti Microsoft
author: pfletcher
description: Questo documento viene fornita un'introduzione alla programmazione l'API di ASP.NET SignalR hub sul lato server per SignalR versione 2, con esempi di codice...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c2567d4d39a494daf77a23db5dff83c8fae4925d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a>Guida di ASP.NET SignalR hub API - Server (c#)
====================
da [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Questo documento viene fornita un'introduzione alla programmazione l'API di ASP.NET SignalR hub sul lato server per SignalR versione 2, con esempi di codice che illustrano le opzioni comuni.
> 
> L'API degli hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server. Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano metodi che eseguono il client. Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi eseguiti nel server. SignalR occuparsene di plumbing client-server.
> 
> SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR, hub e le connessioni permanenti, vedere [Introduzione a SignalR 2](../getting-started/introduction-to-signalr.md).
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
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Come registrare il middleware di SignalR](#route)

    - [L'URL /signalr](#signalrurl)
    - [Configurazione delle opzioni di SignalR](#options)
- [Come creare e utilizzare le classi Hub](#hubclass)

    - [Durata dell'oggetto hub](#transience)
    - [Iniziali maiuscole e minuscole dei nomi di Hub nel client JavaScript](#hubnames)
    - [Hub di più](#multiplehubs)
    - [Hub fortemente tipizzati](#stronglytypedhubs)
- [Come definire i metodi nella classe Hub che i client possono chiamare](#hubmethods)

    - [Iniziali maiuscole e minuscole dei nomi di metodo nei client JavaScript](#methodnames)
    - [Quando è necessario eseguire in modo asincrono](#asyncmethods)
    - [Definizione di overload](#overloads)
    - [Report stato di avanzamento da chiamate del metodo dell'hub](#progress)
- [Come chiamare i metodi della classe Hub client](#callfromhub)

    - [Selezionare i client che riceverà il RPC](#selectingclients)
    - [Nessuna convalida in fase di compilazione per i nomi di metodo](#dynamicmethodnames)
    - [Corrispondenza dei nomi tra maiuscole e minuscole (metodo)](#caseinsensitive)
    - [Esecuzione asincrona](#asyncclient)
- [Come gestire l'appartenenza al gruppo dalla classe dell'Hub](#groupsfromhub)

    - [Esecuzione asincrona dei metodi Add e Remove](#asyncgroupmethods)
    - [Persistenza l'appartenenza al gruppo](#grouppersistence)
    - [Gruppi utente singolo](#singleusergroups)
- [Come gestire gli eventi di durata connessione nella classe Hub](#connectionlifetime)

    - [Quando vengono chiamati OnConnected OnDisconnected e OnReconnected](#onreconnected)
    - [Stato del chiamante non popolato](#nocallerstate)
- [Come ottenere informazioni sul client dalla proprietà di contesto](#contextproperty)
- [Come passare lo stato tra client e la classe di Hub](#passstate)
- [Come gestire gli errori nella classe Hub](#handleErrors)
- [Come chiamare metodi di client e gestire gruppi appartenenti all'esterno della classe di Hub](#callfromoutsidehub)

    - [Chiamata di metodi di client](#callingclientsoutsidehub)
    - [L'appartenenza al gruppo di gestione](#managinggroupsoutsidehub)
- [Come attivare la traccia](#tracing)
- [Come personalizzare la pipeline di hub](#hubpipeline)

Per la documentazione su come i client di programma, vedere le risorse seguenti:

- [Guida di API degli hub SignalR - Client JavaScript](hubs-api-guide-javascript-client.md)
- [Guida di API degli hub SignalR - Client .NET](hubs-api-guide-net-client.md)

Sono disponibili in .NET 4.5 solo i componenti server di SignalR 2. Server che eseguono .NET 4.0 è necessario utilizzare v1. x SignalR.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Come registrare il middleware di SignalR

Per definire la route che i client utilizzeranno per connettersi all'Hub di, chiamare il `MapSignalR` metodo all'avvio dell'applicazione. `MapSignalR`è un [metodo di estensione](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) per la `OwinExtensions` classe. Nell'esempio seguente viene illustrato come definire le route degli hub SignalR utilizzando una classe di avvio OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Se si aggiunge una funzionalità di SignalR a un'applicazione ASP.NET MVC, assicurarsi che la route SignalR viene aggiunto prima di altre route. Per ulteriori informazioni, vedere [esercitazione: Introduzione a SignalR 2 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>L'URL /signalr

Per impostazione predefinita, l'URL di route che i client utilizzeranno per connettersi all'Hub di è "/ signalr". (Non confondere questo URL con l'URL "hub signalr /", ovvero per il file JavaScript generato automaticamente. Per ulteriori informazioni su proxy generato, vedere [Guida API per gli hub SignalR - JavaScript Client, il proxy generato ed esegue automaticamente](hubs-api-guide-javascript-client.md#genproxy).)

Potrebbero esserci circostanze straordinarie che rendono l'URL di base non è utilizzabile per SignalR; ad esempio, si dispone di una cartella nel progetto denominato *signalr* e non si desidera modificare il nome. In tal caso, è possibile modificare l'URL di base, come illustrato negli esempi riportati di seguito (sostituire "/ signalr" nel codice di esempio con l'URL desiderato).

**Codice che specifica l'URL del server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Codice client JavaScript che specifica l'URL (con il proxy generato)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Codice client JavaScript che specifica l'URL (senza il proxy generato)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Codice client .NET che specifica l'URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configurazione delle opzioni di SignalR

Esegue l'overload di `MapSignalR` metodo consentono di specificare un URL personalizzato, un resolver di dipendenza personalizzata e le opzioni seguenti:

- Abilitare le chiamate tra domini tramite CORS o JSONP dai client del browser.

    In genere se il browser viene caricata una pagina da `http://contoso.com`, la connessione SignalR nello stesso dominio, si trova `http://contoso.com/signalr`. Se la pagina `http://contoso.com` stabilisce una connessione a `http://fabrikam.com/signalr`, vale a dire una connessione tra domini. Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita. Per ulteriori informazioni, vedere [ASP.NET SignalR hub API Guida - JavaScript Client - come stabilire una connessione tra domini](hubs-api-guide-javascript-client.md#crossdomain).
- Abilitare i messaggi di errore dettagliati.

    Quando si verificano errori, il comportamento predefinito di SignalR è per inviare ai client un messaggio di notifica senza informazioni dettagliate su cosa è successo. L'invio di informazioni dettagliate sull'errore a client non è consigliato in produzione, perché gli utenti malintenzionati potrebbero essere in grado di utilizzare le informazioni per gli attacchi contro l'applicazione. Per risolvere il problema, è possibile utilizzare questa opzione per abilitare temporaneamente la segnalazione di errori più descrittivi.
- Disabilitare i file di proxy JavaScript generati automaticamente.

    Per impostazione predefinita, viene generato un file di JavaScript con proxy per le classi Hub in risposta all'URL di "hub signalr /". Se non si desidera utilizzare il proxy JavaScript o se si desidera generare manualmente questo file e fare riferimento a un file fisico nel client, è possibile utilizzare questa opzione per disabilitare la generazione di proxy. Per ulteriori informazioni, vedere [Guida di API degli hub SignalR - JavaScript Client, come creare un file fisico per SignalR generato proxy](hubs-api-guide-javascript-client.md#manualproxy).

Nell'esempio seguente viene illustrato come specificare l'URL di connessione SignalR e queste opzioni in una chiamata al `MapSignalR` metodo. Per specificare un URL personalizzato, sostituire "/ signalr" nell'esempio con l'URL a cui si desidera utilizzare.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Come creare e utilizzare le classi Hub

Per creare un Hub, creare una classe che deriva da [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). Nell'esempio seguente viene illustrata una classe semplice di Hub per un'applicazione di chat.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

In questo esempio, è possibile chiamare un client connesso il `NewContosoChatMessage` (metodo), e in questo caso, i dati ricevuti trasmissione a tutti i client connessi.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Durata dell'oggetto hub

Non creare un'istanza della classe Hub o chiamare i metodi dal codice personalizzato sul server. tutto ciò che viene eseguita automaticamente dalla pipeline di hub SignalR. SignalR crea una nuova istanza della classe Hub ogni volta che è necessario per gestire un'operazione di Hub, ad esempio quando un client si connette, si disconnette o effettua un chiamata di metodo per il server.

Poiché le istanze della classe Hub sono temporanee, non possono essere utilizzati per mantenere lo stato da una chiamata al metodo successivo. Ogni volta che il server riceve una chiamata al metodo da un client in una nuova istanza dei processi di classe Hub il messaggio. Per mantenere lo stato attraverso più connessioni e chiamate al metodo, utilizzare un altro metodo, ad esempio un database o una variabile statica nella classe dell'Hub o un'altra classe che deriva da `Hub`. Se si utilizzano dati in memoria, usando un metodo, ad esempio una variabile statica della classe di Hub, i dati andranno persi quando viene riciclato il dominio dell'applicazione.

Se si desidera inviare messaggi ai client dal codice eseguito all'esterno della classe di Hub, è possibile eseguire questa operazione creando un'istanza della classe Hub, ma è possibile farlo tramite il recupero di un riferimento all'oggetto di contesto SignalR per la classe dell'Hub. Per ulteriori informazioni, vedere [come chiamare metodi di client e gestire gruppi appartenenti all'esterno della classe Hub](#callfromoutsidehub) più avanti in questo argomento.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Iniziali maiuscole e minuscole dei nomi di Hub nel client JavaScript

Per impostazione predefinita, i client JavaScript per fare riferimento a un hub utilizzando una versione di maiuscole/minuscole camel del nome della classe. SignalR viene automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript. Nell'esempio precedente potrebbe essere indicata come `contosoChatHub` nel codice JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**Client JavaScript mediante il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Se si desidera specificare un nome diverso per i client da utilizzare, aggiungere il `HubName` attributo. Quando si utilizza un `HubName` attributo, non è presente alcuna modifica nome maiuscole-minuscole camel nei client JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**Client JavaScript mediante il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Hub di più

In un'applicazione, è possibile definire più classi di Hub. Quando a tale scopo, la connessione è condivisa, ma i gruppi sono separati:

- Tutti i client utilizzeranno lo stesso URL per stabilire una connessione SignalR con il servizio ("/ signalr" o l'URL personalizzato se è stata specificata una), e viene utilizzata per tutti gli hub di connessione definite dal servizio.

    Non vi è alcuna differenza nelle prestazioni per gli hub di più rispetto alla definizione di tutte le funzionalità di Hub in una singola classe.
- Tutti gli hub di ottenere le stesse informazioni di richiesta HTTP.

    Poiché tutti gli hub condividono la stessa connessione, l'unica informazione richiesta HTTP che ottiene il server è ciò che viene fornito nella richiesta HTTP originale che stabilisce la connessione SignalR. Se si utilizza la richiesta di connessione per passare le informazioni dal client al server, specificando una stringa di query, è possibile fornire le stringhe di query diversi agli hub diverso. Tutti gli hub riceverà le stesse informazioni.
- Il file proxy JavaScript generato conterrà proxy per tutti gli hub in un file.

    Per informazioni sul proxy di JavaScript, vedere [Guida API per gli hub SignalR - JavaScript Client, il proxy generato ed esegue automaticamente](hubs-api-guide-javascript-client.md#genproxy).
- I gruppi sono definiti all'interno di hub.

    In SignalR che è possibile definire gruppi per la trasmissione di sottoinsiemi di client connessi denominati. I gruppi vengono gestiti separatamente per ogni Hub. Ad esempio, un gruppo denominato "Administrators" include un set di client per il `ContosoChatHub` classe e lo stesso nome di gruppo, fare riferimento a un set diverso di client per la `StockTickerHub` classe.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Hub fortemente tipizzati

Per definire un'interfaccia per i metodi dell'hub che il client può essere riferimento e abilitare Intellisense in metodi di hub, derivare l'hub da `Hub<T>` (introdotto in SignalR 2.1) anziché `Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Come definire i metodi nella classe Hub che i client possono chiamare

Per esporre un metodo dell'hub che si desidera essere chiamato dal client, è possibile dichiarare un metodo pubblico, come illustrato negli esempi seguenti.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

È possibile specificare un tipo restituito e parametri, inclusi i tipi complessi e matrici, esattamente come si farebbe con qualsiasi metodo c#. Tutti i dati ricevuti nei parametri o restituire al chiamante viene comunicati tra il client e il server usando JSON e SignalR gestisce automaticamente l'associazione di oggetti complessi e matrici di oggetti.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Iniziali maiuscole e minuscole dei nomi di metodo nei client JavaScript

Per impostazione predefinita, i client JavaScript per fare riferimento ai metodi di Hub utilizzando una versione di maiuscole/minuscole camel del nome del metodo. SignalR viene automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Client JavaScript mediante il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Se si desidera specificare un nome diverso per i client da utilizzare, aggiungere il `HubMethodName` attributo.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Client JavaScript mediante il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Quando è necessario eseguire in modo asincrono

Se verrà essere a esecuzione prolungata o deve utilizzare il metodo che verrebbe implicano in attesa, ad esempio una ricerca nel database o una chiamata al servizio web, il metodo dell'Hub asincrona restituendo un [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (invece di `void` restituire) o [ Attività&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) oggetto (invece di `T` tipo restituito). Quando viene restituito un `Task` oggetto dal metodo, SignalR attende la `Task` per completare, e quindi invia il risultato annullato il wrapping al client, pertanto non c'è alcuna differenza nella modalità in cui la chiamata al metodo nel client codice.

Effettua un metodo dell'Hub asincrona si evita di bloccare la connessione quando utilizza il trasporto WebSocket. Quando un metodo dell'Hub esegue in modo sincrono e il trasporto WebSocket, le successive chiamate dei metodi dell'hub dallo stesso client vengono bloccate fino al completamento del metodo dell'Hub.

L'esempio seguente viene illustrato lo stesso metodo codificate in modo da eseguire in modo sincrono o in modo asincrono, seguito dal codice client JavaScript che funziona per la chiamata a entrambe le versioni.

**Sincrono**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asynchronous**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Client JavaScript mediante il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Per ulteriori informazioni su come usare i metodi asincroni in ASP.NET 4.5, vedere [utilizzando metodi asincroni in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definizione di overload

Se si desidera definire gli overload per un metodo, il numero di parametri in ogni overload deve essere diverso. Se un overload si differenzia solo specificando i tipi di parametri diversi, la classe Hub verrà compilati ma il servizio SignalR genererà un'eccezione in fase di esecuzione quando i client tentano di chiamare uno degli overload.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Report stato di avanzamento da chiamate del metodo dell'hub

2.1 SignalR aggiunge il supporto per il [modello di report di stato](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introdotta in .NET 4.5. Per implementare i report di stato, definire un `IProgress<T>` parametro per il metodo dell'hub che il client può accedere a:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Quando si scrive un metodo di server con esecuzione prolungata, è importante utilizzare un modello di programmazione asincrono come Async / Await anziché bloccare il thread di hub.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Come chiamare i metodi della classe Hub client

Per chiamare i metodi client dal server, utilizzare il `Clients` proprietà in un metodo nella classe di Hub. L'esempio seguente mostra il codice che chiama server `addNewMessageToPage` su tutti i client e il codice client che definisce il metodo in un client JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

**Client JavaScript mediante il proxy generato**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

È possibile ottenere un valore restituito da un metodo client; la sintassi `int x = Clients.All.add(1,1)` non funziona.

È possibile specificare i tipi complessi e matrici per i parametri. Nell'esempio seguente passa un tipo complesso al client in un parametro di metodo.

**Codice del server che chiama un metodo client utilizzando un oggetto complesso**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Codice del server che definisce l'oggetto complesso**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**Client JavaScript mediante il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Selezionare i client che riceverà il RPC

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
- Tutti i client connessi eccetto il client specificati, identificato dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Tutti i client in un gruppo specificato.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Tutti i client connessi in un gruppo specificato eccetto il client specificati, identificato dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Tutti i client connessi in un gruppo specificato eccetto il client chiamante.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Un utente specifico, identificato dall'ID utente.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Per impostazione predefinita, si tratta di `IPrincipal.Identity.Name`, ma può essere modificato da [la registrazione di un'implementazione di IUserIdProvider con l'host globale](mapping-users-to-connections.md#IUserIdProvider).
- Tutti i client e i gruppi in un elenco di ID connessione.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Un elenco di gruppi.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Un utente in base al nome.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Un elenco di nomi utente (introdotto in SignalR 2.1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Nessuna convalida in fase di compilazione per i nomi di metodo

Il nome del metodo specificato viene interpretato come un oggetto dinamico, ovvero che nessuna IntelliSense o la relativa convalida in fase di compilazione. L'espressione viene valutata in fase di esecuzione. Quando viene eseguita la chiamata al metodo, SignalR invia il nome del metodo e i valori dei parametri per il client e se il client dispone di un metodo che corrisponde al nome, la chiamata a metodo e i valori dei parametri vengono passati al metodo. Se viene trovato alcun metodo di corrispondenza nel client, viene generato alcun errore. Per informazioni sul formato dei dati SignalR trasmette al client in background quando si chiama un metodo client, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Corrispondenza dei nomi tra maiuscole e minuscole (metodo)

Nome di metodo corrispondente è tra maiuscole e minuscole. Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, o `addContosoChatMessageToPage` sul client.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Esecuzione asincrona

Il metodo chiamato viene eseguito in modo asincrono. Qualsiasi codice che viene fornito dopo una chiamata al metodo a un client verrà eseguita immediatamente senza attendere SignalR completare la trasmissione dei dati per i client a meno che non si specifica che le successive righe di codice deve attendere il completamento di metodo. Esempio di codice seguente viene illustrato come eseguire i due metodi di client in modo sequenziale.

**Attende (.NET 4.5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Se si utilizza `await` per l'attesa fino al completamento di un metodo client prima che la riga successiva del codice viene eseguito, questo non significa che i client effettivamente riceverà il messaggio prima dell'esecuzione successiva riga di codice. "Completamento" di una chiamata al metodo client significa solo che abbia eseguito tutto il necessario per inviare il messaggio SignalR. Se è necessario che i client ha ricevuto il messaggio di verifica, è necessario programmare manualmente tale meccanismo. Ad esempio, è possibile codificare una `MessageReceived` metodo dell'hub e nel `addContosoChatMessageToPage` metodo sul client è possibile chiamare `MessageReceived` dopo aver eseguito l'elemento di lavoro è necessario eseguire sul client. In `MessageReceived` nell'Hub è possibile eseguire le operazioni dipende dalla ricezione client effettivo e l'elaborazione della chiamata al metodo originale.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Come utilizzare una variabile di stringa come il nome del metodo

Se si desidera richiamare un metodo client utilizzando una variabile di stringa come il nome del metodo, eseguire il cast `Clients.All` (o `Clients.Others`, `Clients.Caller`e così via) a `IClientProxy` e quindi chiamare [Invoke (NomeMetodo, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Come gestire l'appartenenza al gruppo dalla classe dell'Hub

Gruppi di SignalR forniscono un metodo per la trasmissione di messaggi a un subset specificato di client connessi. Un gruppo può avere qualsiasi numero di client e un client può essere un membro di un numero qualsiasi di gruppi.

Per gestire l'appartenenza al gruppo, utilizzare il [Aggiungi](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) e [rimuovere](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metodi forniti dal `Groups` proprietà della classe di Hub. Nell'esempio seguente il `Groups.Add` e `Groups.Remove` metodi usati nei metodi dell'Hub che vengono chiamati dal codice client, seguito dal codice client JavaScript che li chiama.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**Client JavaScript mediante il proxy generato**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Non è necessario creare in modo esplicito gruppi. In effetti un gruppo viene creato automaticamente la prima volta, specificare il nome in una chiamata a `Groups.Add`, e viene eliminata quando si rimuove l'ultima connessione dall'appartenenza in essa contenuti.

Non vi è alcuna API per ottenere un elenco di appartenenze di gruppo oppure un elenco di gruppi. SignalR invia messaggi al client e i gruppi in base a un [modello pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), e il server non gestisce gli elenchi di gruppi o appartenenza al gruppo. Ciò consente di ottimizzare la scalabilità, poiché ogni volta che si aggiunge un nodo a una web farm, qualsiasi stato che gestisce SignalR deve essere propagata nel nuovo nodo.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Esecuzione asincrona dei metodi Add e Remove

Il `Groups.Add` e `Groups.Remove` metodi eseguire in modo asincrono. Se si desidera aggiungere un client a un gruppo e invia immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il `Groups.Add` metodo termina per prima. Esempio di codice seguente viene illustrato come eseguire questa operazione.

**Aggiunta di un client a un gruppo e quindi che il client di messaggistica**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistenza l'appartenenza al gruppo

SignalR tiene traccia delle connessioni, non gli utenti, pertanto, se si desidera che un utente a essere nello stesso gruppo ogni volta che l'utente stabilisce una connessione, è necessario chiamare `Groups.Add` ogni volta che l'utente definisce una nuova connessione.

Dopo una perdita temporanea della connettività, talvolta SignalR possibile ripristinare la connessione automaticamente. In tal caso, SignalR in corso il ripristino la stessa connessione, senza definire una nuova connessione e quindi ripristinato automaticamente l'appartenenza al gruppo del client. Questo vale anche quando l'interruzione temporanea è il risultato di un errore, o il riavvio del server poiché lo stato di connessione per ogni client, inclusa l'appartenenza al gruppo, è sottoposto a round trip al client. Se un server si arresta e viene sostituito da un nuovo server prima del timeout della connessione, un client possa riconnettersi automaticamente al nuovo server e registrare di nuovo in è un membro dei gruppi.

Quando una connessione non può essere ripristinata automaticamente dopo una perdita di connettività, o quando il timeout della connessione o quando il client si disconnette (ad esempio, quando un browser passa a una nuova pagina), l'appartenenza al gruppo viene persi. Alla successiva si connette l'utente sarà una nuova connessione. Per gestire l'appartenenza ai gruppi quando lo stesso utente stabilisce una nuova connessione, l'applicazione deve rilevare le associazioni tra utenti e gruppi e il ripristino di appartenenza al gruppo ogni volta che un utente stabilisce una nuova connessione.

Per ulteriori informazioni sulle connessioni e riconnessione, vedere [come gestire gli eventi di durata connessione nella classe Hub](#connectionlifetime) più avanti in questo argomento.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Gruppi utente singolo

Le applicazioni che utilizzano in genere SignalR sono necessario tenere traccia delle associazioni tra utenti e le connessioni per sapere quale utente ha inviato un messaggio e che gli utenti dovrebbero ricevere un messaggio. Gruppi vengono usati in uno dei due modelli di usati comune per questo scopo.

- Gruppi utente singolo.

    È possibile specificare il nome utente come il nome del gruppo e aggiungere l'ID di connessione corrente al gruppo ogni volta che l'utente si connette o si riconnette. Per inviare messaggi all'utente di inviare il gruppo. Uno svantaggio di questo metodo è che il gruppo non offre un modo per sapere se l'utente è online o offline.
- Rilevare le associazioni tra i nomi utente e ID connessione.

    È possibile archiviare un'associazione tra ogni nome utente e la connessione di uno o più ID in un database o un dizionario e aggiornare i dati archiviati ogni volta che l'utente si connette o disconnette. Per inviare messaggi all'utente specificare l'ID di connessione. Uno svantaggio di questo metodo è che occorre una maggiore quantità di memoria.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Come gestire gli eventi di durata connessione nella classe Hub

Cause comuni di gestione degli eventi di durata connessione sono di tenere traccia se un utente è connesso o meno e di tenere traccia dell'associazione tra i nomi utente e ID connessione. Per eseguire codice quando i client di connettono o disconnessione, eseguire l'override di `OnConnected`, `OnDisconnected`, e `OnReconnected` metodi virtuali dell'Hub di classe, come illustrato nell'esempio seguente.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Quando vengono chiamati OnConnected OnDisconnected e OnReconnected

Ogni volta che una si accede a una nuova pagina, una nuova connessione deve essere elaborato, ovvero SignalR eseguirà il `OnDisconnected` metodo aggiungendo il `OnConnected` (metodo). SignalR crea sempre un nuovo ID di connessione quando viene stabilita una nuova connessione.

Il `OnReconnected` metodo viene chiamato quando si verifica un'interruzione temporanea della connettività che SignalR è possibile recuperare automaticamente da, ad esempio quando un cavo è temporaneamente disconnesso e riconnesso prima del timeout della connessione. Il `OnDisconnected` metodo viene chiamato quando il client è disconnesso e SignalR non è possibile riconnettersi automaticamente, ad esempio quando un si accede a una nuova pagina. Pertanto, una possibile sequenza di eventi per un determinato client è `OnConnected`, `OnReconnected`, `OnDisconnected`; o `OnConnected`, `OnDisconnected`. Non verrà visualizzata la sequenza `OnConnected`, `OnDisconnected`, `OnReconnected` per una determinata connessione.

Il `OnDisconnected` metodo non può essere chiamato in alcuni scenari, ad esempio quando si arresta un server o il dominio dell'applicazione ottiene riciclato. Quando un altro server nella riga o il dominio dell'applicazione viene completato il riciclo, alcuni client potrebbero essere in grado di riconnettersi e generare il `OnReconnected` evento.

Per ulteriori informazioni, vedere [comprensione e gestione degli eventi di durata connessione in SignalR](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Stato del chiamante non popolato

Vengono chiamati i metodi del gestore eventi Durata connessione dal server, il che significa che qualsiasi stato che si inserisce nel `state` oggetto nel client non viene popolata nel `Caller` proprietà sul server. Per informazioni sul `state` oggetto e `Caller` proprietà, vedere [come passare lo stato tra client e la classe Hub](#passstate) più avanti in questo argomento.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Come ottenere informazioni sul client dalla proprietà di contesto

Per ottenere informazioni sul client, utilizzare il `Context` proprietà della classe di Hub. Il `Context` proprietà restituisce un [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) oggetto che fornisce l'accesso alle informazioni seguenti:

- L'ID di connessione del client chiamante.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    L'ID di connessione è un GUID assegnato da SignalR (è possibile specificare il valore nel codice). È un ID di connessione per ogni connessione e la stessa connessione che ID viene utilizzato da tutti gli hub, se si dispone di più hub nell'applicazione.
- Dati dell'intestazione HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    È inoltre possibile ottenere le intestazioni HTTP dalla `Context.Headers`. Il motivo più riferimenti allo stesso elemento è che `Context.Headers` è stato creato prima di tutto, il `Context.Request` proprietà è stata aggiunta in un secondo momento, e `Context.Headers` è stata mantenuta per compatibilità con le versioni precedenti.
- Eseguire query sui dati di stringa.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    È inoltre possibile ottenere dati di stringa di query da `Context.QueryString`.

    La stringa di query che si ottiene in questa proprietà è quella utilizzata con la richiesta HTTP che stabilito la connessione SignalR. È possibile aggiungere parametri di stringa di query nel client mediante la configurazione della connessione, che è un modo pratico per passare i dati sul client dal client al server. Nell'esempio seguente viene illustrato un modo per aggiungere una stringa di query in un client JavaScript, quando si usa il proxy generato.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Per ulteriori informazioni sull'impostazione di parametri di stringa di query, vedere le guide di API per la [JavaScript](hubs-api-guide-javascript-client.md) e [.NET](hubs-api-guide-net-client.md) client.

    È possibile trovare il metodo di trasporto utilizzato per la connessione dei dati di stringa di query, con alcuni altri valori utilizzati internamente da SignalR:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    Il valore di `transportMethod` sarà "WebSocket", "serverSentEvents", "foreverFrame" o "longPolling". Si noti che se si archivia il valore di `OnConnected` metodo del gestore eventi, in alcuni scenari è inizialmente potrebbe ricevere un valore di trasporto che non è il metodo di trasporto negoziata finale per la connessione. In tal caso, il metodo genererà un'eccezione e verrà chiamato nuovamente in un secondo momento quando il metodo di trasporto finale viene stabilito.
- Cookie.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    È inoltre possibile ottenere i cookie dal `Context.RequestCookies`.
- Informazioni utente.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- L'oggetto HttpContext per la richiesta:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Utilizzare questo metodo anziché `HttpContext.Current` per ottenere il `HttpContext` oggetto per la connessione SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Come passare lo stato tra client e la classe di Hub

Il proxy client fornisce un `state` oggetto in cui è possibile archiviare i dati che si desidera essere trasmessi al server con ogni chiamata al metodo. Nel server è possibile accedere a questi dati nel `Clients.Caller` proprietà nei metodi dell'Hub che vengono chiamati dal client. Il `Clients.Caller` proprietà non viene popolata per i metodi del gestore eventi Durata connessione `OnConnected`, `OnDisconnected`, e `OnReconnected`.

La creazione o aggiornamento dei dati nel `state` oggetto e `Clients.Caller` proprietà viene utilizzata in entrambe le direzioni. È possibile aggiornare i valori nel server e vengono passati al client.

Nell'esempio seguente viene illustrato il codice client JavaScript che archivia lo stato per la trasmissione al server con ogni chiamata al metodo.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

Nell'esempio seguente viene illustrato l'equivalente del codice in un client .NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

Nella classe Hub, è possibile accedere a questi dati nel `Clients.Caller` proprietà. Nell'esempio seguente viene illustrato il codice che recupera lo stato in cui all'esempio precedente.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Questo meccanismo di persistenza dello stato non è destinato grandi quantità di dati, perché tutto ciò che si inserisce nella `state` o `Clients.Caller` proprietà è sottoposto a round trip con ogni chiamata al metodo. È utile per gli elementi più piccoli, ad esempio nomi utente o i contatori.


In Visual Basic.NET o in un hub fortemente tipizzato, l'oggetto di stato del chiamante non sono accessibili attraverso `Clients.Caller`, bensì utilizzare `Clients.CallerState` (introdotto in SignalR 2.1):

**Utilizzo di CallerState in c#**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Utilizzo di CallerState in Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Come gestire gli errori nella classe Hub

Per gestire gli errori che si verificano nei metodi di classe di Hub, utilizzare uno o più dei metodi seguenti:

- Eseguire il wrapping del codice del metodo in blocchi try-catch e accedere all'oggetto eccezione. Ai fini del debug è possibile inviare l'eccezione al client, ma per la sicurezza non sono consigliabile motivi l'invio di informazioni dettagliate per i client nell'ambiente di produzione.
- Creare un modulo di pipeline hub che gestisce il [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) metodo. Nell'esempio seguente viene illustrato un modulo di pipeline che registra gli errori, seguiti dal codice in Startup.cs che inserisce il modulo nella pipeline di hub.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Utilizzare la `HubException` classe (introdotto in SignalR 2). Questo errore può essere generato da qualsiasi chiamata dell'hub. Il `HubError` costruttore accetta una stringa di messaggio e un oggetto per l'archiviazione dei dati aggiuntivi errore. SignalR verrà automaticamente a serializzare l'eccezione e inviarlo al client, in cui verrà consente di rifiutare o interrompere la chiamata del metodo hub.

    Gli esempi di codice seguente viene illustrato come generare un `HubException` durante una chiamata dell'Hub e come gestire l'eccezione sul client .NET e JavaScript.

    **Codice lato server per illustrare la classe HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Codice client JavaScript che illustra una risposta per la generazione di un HubException in un hub**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Codice client .NET che illustra una risposta per la generazione di un HubException in un hub**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Per ulteriori informazioni sui moduli della pipeline di Hub, vedere [come personalizzare la pipeline di hub](#hubpipeline) più avanti in questo argomento.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Come attivare la traccia

Per abilitare la traccia sul lato server, aggiungere un elemento System. Diagnostics nel file Web. config, come illustrato in questo esempio:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Quando si esegue l'applicazione in Visual Studio, è possibile visualizzare i log di **Output** finestra.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Come chiamare metodi di client e gestire gruppi appartenenti all'esterno della classe di Hub

Per chiamare metodi di client da una classe diversa rispetto a classe Hub, ottenere un riferimento all'oggetto di contesto SignalR per l'Hub e utilizzarlo per chiamare metodi sul client o gestire i gruppi.

L'esempio seguente `StockTicker` classe ottiene l'oggetto di contesto, viene memorizzato in un'istanza della classe, archivia l'istanza della classe in una proprietà statica e Usa il contesto dell'istanza di classe singleton per chiamare il `updateStockPrice` nei client che sono (metodo) connesso a un Hub denominato `StockTickerHub`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Se è necessario utilizzare i contesto più-volte in un oggetto di lunga durato, ottenere il riferimento a una sola volta e salvare anziché essere nuovamente ogni volta. Ottenere il contesto di una volta assicura che SignalR invia messaggi ai client nella stessa sequenza in cui i metodi dell'Hub rendere client chiamate al metodo. Per un'esercitazione che illustra come utilizzare il contesto di SignalR per un Hub, vedere [Server Broadcast con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Chiamata di metodi di client

È possibile specificare che i client riceveranno il RPC, ma sono disponibili meno opzioni rispetto a quando si chiama da una classe di Hub. Il motivo è che il contesto non è associato a una determinata chiamata da un client, pertanto tutti i metodi che richiedono la conoscenza dell'ID di connessione corrente, ad esempio `Clients.Others`, o `Clients.Caller`, o `Clients.OthersInGroup`, non sono disponibili. Sono disponibili le seguenti opzioni:

- Tutti i client connessi.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Un client specifico identificato dall'ID di connessione.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Tutti i client connessi eccetto il client specificati, identificato dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Tutti i client in un gruppo specificato.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Tutti i client in un gruppo specificato, eccetto il client specificati, identificato dall'ID di connessione.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Se si chiama la classe non Hub dai metodi nella classe di Hub, è possibile passare l'ID di connessione corrente e utilizzarla con `Clients.Client`, `Clients.AllExcept`, o `Clients.Group` per simulare `Clients.Caller`, `Clients.Others`, o `Clients.OthersInGroup`. Nell'esempio seguente, il `MoveShapeHub` classe passa l'ID di connessione per il `Broadcaster` classe in modo che il `Broadcaster` possibile simulare la classe `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>L'appartenenza al gruppo di gestione

Per gestire i gruppi sono disponibili le opzioni stesso come in una classe di Hub.

- Aggiungere un client a un gruppo

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Rimuovere un client da un gruppo

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Come personalizzare la pipeline di hub

SignalR consente di inserire codice personalizzato nella pipeline dell'Hub. Nell'esempio seguente viene illustrato un modulo di pipeline Hub personalizzato che registra ogni chiamata al metodo in ingresso ricevuto dal client e in uscita chiamata al metodo richiamato nel client:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

Nell'esempio di codice il *Startup.cs* file registra il modulo per l'esecuzione della pipeline di Hub:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Esistono diversi metodi che è possibile eseguire l'override. Per un elenco completo, vedere [HubPipelineModule metodi](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
