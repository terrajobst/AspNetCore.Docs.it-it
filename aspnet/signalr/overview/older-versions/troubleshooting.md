---
uid: signalr/overview/older-versions/troubleshooting
title: Risoluzione dei problemi di SignalR (SignalR 1. x) | Documenti Microsoft
author: pfletcher
description: Questo articolo descrive i problemi comuni con lo sviluppo di applicazioni SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: eb739f806eb57d09008fa0bf04b5a40721dab73d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="signalr-troubleshooting-signalr-1x"></a>Risoluzione dei problemi di SignalR (SignalR 1. x)
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> Questo documento descrive problemi di risoluzione dei problemi comuni relativi a SignalR.


Questo documento contiene le sezioni seguenti.

- [Chiamata di metodi tra il client e server automatica ha esito negativo](#connection)
- [Altri problemi di connessione](#other)
- [Errori di compilazione e sul lato server](#server)
- [Problemi di Visual Studio](#vs)
- [Problemi di Internet Information Services](#iis)
- [Problemi di Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Chiamata di metodi tra il client e server automatica ha esito negativo

Questa sezione vengono descritte le cause possibili per una chiamata al metodo tra client e server avrà esito negativo senza un messaggio di errore significativo. In un'applicazione di SignalR, il server non contiene informazioni sui metodi che il client implementa; Quando il server richiama un metodo client, i dati di nome e il parametro di metodo vengono inviati al client e il metodo viene eseguito solo se è presente nel server specificato. Se non viene trovato alcun metodo corrispondente sul client, non accade nulla e nessun messaggio di errore viene generato sul server.

Per ulteriori informazioni su metodi client non chiamati, è possibile attivare la registrazione prima di chiamare il metodo start sull'hub di visualizzare le chiamate provenienti dal server. Per abilitare la registrazione in un'applicazione JavaScript, vedere [come abilitare la registrazione lato client (versione di client JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Per abilitare la registrazione in un'applicazione client .NET, vedere [come abilitare la registrazione lato client (versione del Client .NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Metodo errata, firma del metodo non corretto o il nome dell'hub non corretto

Se il nome o firma di un metodo chiamato corrisponde esattamente a un metodo appropriato nel client, la chiamata avrà esito negativo. Verificare che il nome del metodo chiamato dal server corrisponda al nome del metodo nel client. Inoltre, SignalR crea il proxy dell'hub utilizzando i metodi maiuscole/minuscole camel, come appropriato in JavaScript, pertanto, un metodo chiamato `SendMessage` sul server deve essere chiamato `sendMessage` nel proxy client. Se si utilizza il `HubName` attributo nel codice sul lato server, verificare che il nome utilizzato corrisponda al nome utilizzato per creare l'hub nel client. Se non si utilizza il `HubName` attributo, verificare che il nome dell'hub in un client JavaScript sia maiuscole/minuscole camel, ad esempio chatHub anziché ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nome del metodo duplicato nel client

Verificare che non è un metodo duplicato nel client che differisce solo dalle maiuscole o minuscole. Se l'applicazione client dispone di un metodo denominato `sendMessage`, verificare che non vi è anche un metodo denominato `SendMessage` anche.

### <a name="missing-json-parser-on-the-client"></a>Parser JSON mancante nel client

SignalR richiede un parser JSON deve essere presente per la serializzazione delle chiamate tra il server e client. Se il client non dispone di un parser JSON incorporato (ad esempio Internet Explorer 7), è necessario includere una nell'applicazione. È possibile scaricare il parser JSON [qui](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Combinazione di sintassi di Hub e PersistentConnection

SignalR utilizza i due modelli di comunicazione: hub e PersistentConnections. La sintassi per la chiamata di questi modelli di due comunicazione è diversa nel codice client. Se è stato aggiunto un hub nel codice server, verificare che tutto il codice client viene utilizzata la sintassi di hub appropriato.

**Codice client JavaScript che crea un PersistentConnection in un client JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Codice client JavaScript che consente di creare un Proxy dell'Hub in un client Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**Codice c# server che esegue il mapping di una route per un PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**Codice c# server che esegue il mapping di una route a un Hub o a un hub di più se si dispone di più applicazioni**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Connessione avviato prima dell'aggiunta di sottoscrizioni

Se la connessione dell'Hub è stata avviata prima dell'aggiunta di metodi che possono essere chiamati dal server al proxy, non verranno ricevuti i messaggi. Il codice JavaScript seguente non verrà avviato correttamente l'hub:

**Codice client JavaScript non corretto che non consente messaggi hub da ricevere**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

In alternativa, è possibile aggiungere le sottoscrizioni di metodo prima di chiamare Start:

**Codice client JavaScript che aggiunge in modo corretto le sottoscrizioni a un hub**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Nome di metodo mancante per il proxy dell'hub

Verificare che il metodo definito nel server viene eseguito nel client. Anche se il server definisce il metodo, deve ancora essere aggiunti al proxy client. Metodi possono essere aggiunti al proxy client nel modo seguente (si noti che il metodo viene aggiunto per il `client` membro dell'hub, non l'hub direttamente):

**Codice client JavaScript che consente di aggiungere metodi a un proxy dell'hub**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Metodi dell'hub non è dichiarati come pubblico o l'hub

Per essere visibile nel client, i metodi e l'implementazione dell'hub devono essere dichiarati come `public`.

### <a name="accessing-hub-from-a-different-application"></a>L'accesso a hub da un'altra applicazione

Gli hub SignalR è accessibile solo tramite applicazioni che implementano i client SignalR. SignalR non può interagire con altre librerie di comunicazione (ad esempio SOAP o WCF servizi web.) Se nessun client SignalR è disponibile per la piattaforma di destinazione, è possibile accedere direttamente l'endpoint del server.

### <a name="manually-serializing-data"></a>Manualmente la serializzazione dei dati

SignalR non utilizzeranno automaticamente JSON per il metodo di serializzare alcuna necessità i parametri-non esiste di eseguire tale operazione manualmente.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Metodo dell'Hub remoto non eseguita sul client in OnDisconnected (funzione)

Questo comportamento è previsto dalla progettazione. Quando `OnDisconnected` viene chiamato, l'hub è già immesso il `Disconnected` stato che non consente ulteriori metodi dell'hub da chiamare.

**Codice c# server che esegue correttamente il codice nell'evento OnDisconnected**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Raggiunto il limite di connessione

Quando si utilizza la versione completa di IIS in un sistema operativo client come in Windows 7, viene imposto un limite di 10 connessioni. Quando si utilizza un sistema operativo client, utilizzare IIS Express ma per evitare questo limite.

### <a name="cross-domain-connection-not-set-up-properly"></a>Connessione tra domini non configurato correttamente

Se una connessione tra domini, una connessione per cui l'URL di SignalR non è nello stesso dominio della pagina di hosting, non è configurata correttamente, la connessione può non riuscire senza un messaggio di errore. Per informazioni su come abilitare la comunicazione tra domini, vedere [per stabilire una connessione tra domini](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Connessione tramite NTLM (Active Directory) non funziona nel client .NET

Una connessione in un'applicazione client .NET che utilizza la sicurezza del dominio potrebbe non riuscire se la connessione non è configurata correttamente. Per utilizzare SignalR in un ambiente di dominio, impostare la proprietà di connessione necessarie come segue:

**In c# il codice client che implementa le credenziali di connessione**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Altri problemi di connessione

In questa sezione vengono descritte le cause e soluzioni per i sintomi specifici o messaggi di errore che si verificano durante una connessione.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Errore di "Inizio deve essere chiamato prima di possono inviare i dati"

Questo errore si verifica solitamente se il codice fa riferimento a oggetti di SignalR prima dell'avvio della connessione. L'accade per gestori eventi e così via che verrà chiamata ai metodi definiti nel server è necessario aggiungere al termine della connessione. Si noti che la chiamata a `Start` è asincrona, in modo codice dopo la chiamata può essere eseguita prima che venga completato. Il modo migliore per aggiungere gestori dopo l'avvio di una connessione completamente è per inserirli in una funzione di callback che viene passata come parametro al metodo di avvio:

**Codice client JavaScript che aggiunge correttamente i gestori di eventi che fanno riferimento a oggetti di SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Verrà visualizzato questo errore se una connessione viene arrestato mentre sono ancora presenti riferimenti oggetti SignalR.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>"301 spostato permanentemente" o "302 Spostato temporaneamente" errore

Questo errore può verificarsi se il progetto contiene una cartella denominata SignalR, interferisce con il proxy creato automaticamente. Per evitare questo errore, non utilizzare una cartella denominata `SignalR` nell'applicazione, o la generazione automatica del proxy turn off. Vedere [il Proxy generato ed esegue automaticamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) per altri dettagli.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>Errore "403 accesso negato" nel client .NET o Silverlight

Questo errore può verificarsi in ambienti di domini in cui la comunicazione tra domini non è abilitata in modo corretto. Per informazioni su come abilitare la comunicazione tra domini, vedere [per stabilire una connessione tra domini](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Per stabilire una connessione tra domini in un client Silverlight, vedere [le connessioni tra domini da client Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Errore "404 non trovato"

Esistono diverse cause per questo problema. Verificare che tutti gli elementi seguenti:

- **Riferimento di indirizzo proxy hub non è formattato correttamente:** questo errore si verifica solitamente se il riferimento all'indirizzo proxy generato hub non è formattato correttamente. Verificare che il riferimento all'indirizzo hub viene effettuato correttamente. Vedere [come fanno riferimento al proxy generato dinamicamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) per informazioni dettagliate.
- **Aggiunta di route per l'applicazione prima di aggiungere la route dell'hub:** se l'applicazione utilizza altre route, verificare che la route prima di aggiungere la chiamata a `MapHubs`.

### <a name="500-internal-server-error"></a>"500 Errore interno del Server"

Si tratta di un errore molto generico che potrebbe avere un'ampia gamma di cause. I dettagli dell'errore dovrebbero essere visualizzato nel registro eventi del server o possono essere individuati tramite il debug del server. Per ulteriori informazioni sull'errore possono essere ottenute facendo accensione errori dettagliati nel server. Per ulteriori informazioni, vedere [come gestire gli errori nella classe Hub](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt; è definito" errore

Si verificherà questo errore se la chiamata a `MapHubs` non viene eseguita correttamente. Vedere [come registrare le route SignalR e configurare le opzioni di SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) per ulteriori informazioni.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>Non è stata gestita dal codice utente JsonSerializationException

Verificare che i parametri inviati ai metodi di non includano i tipi non serializzabili (come gli handle di file o le connessioni di database). Se è necessario utilizzare i membri su un oggetto sul lato server che non si desidera essere inviato al client (o per la sicurezza per motivi di serializzazione), utilizzare il `JSONIgnore` attributo.

### <a name="protocol-error-unknown-transport-error"></a>"Errore di protocollo: trasporto sconosciuto" errore

Questo errore può verificarsi se il client non supporta i trasporti che utilizza SignalR. Vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports) per informazioni su cui browser può essere utilizzato con SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Generazione del proxy JavaScript Hub è stata disabilitata".

Questo errore si verifica se `DisableJavaScriptProxies` set di risultati durante include un riferimento al proxy generato dinamicamente nel `signalr/hubs`. Per ulteriori informazioni su come creare manualmente il proxy, vedere [il proxy generato ed esegue automaticamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"L'ID di connessione è in formato non corretto" o "non è possibile modificare l'identità dell'utente durante una connessione SignalR attiva" errore

Questo errore può verificarsi se viene utilizzata l'autenticazione e il client è disconnesso prima che venga interrotta la connessione. La soluzione consiste nell'interrompere la connessione SignalR prima della disconnessione del client.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Errore di non rilevate: SignalR: jQuery non trovato. Verificare che fa riferimento jQuery prima il file SignalR.js"errore

Il client SignalR JavaScript richiede jQuery per l'esecuzione. Verificare che il riferimento jQuery è corretto, che il percorso utilizzato sia valido e che il riferimento jQuery è prima del riferimento a SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Non rilevate TypeError: Impossibile leggere la proprietà '&lt;proprietà&gt;" undefined "errore

Questo errore risulta dal non jQuery o del proxy hub a cui fa riferimento in modo corretto. Verificare che il riferimento jQuery e del proxy hub è corretto, che il percorso utilizzato sia valido e che il riferimento jQuery è prima del riferimento al proxy di hub. Il riferimento predefinito per il proxy dell'hub dovrebbe essere simile al seguente:

**Codice HTML sul lato client che correttamente fa riferimento il proxy dell'hub**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Errore "RuntimeBinderException non è stata gestita dal codice utente"

Questo errore può verificarsi quando l'overload corretto di `Hub.On` viene utilizzato. Se il metodo ha un valore restituito, il tipo restituito deve essere specificato come parametro di tipo generico:

**Metodo definito nel client (senza il proxy generato)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>ID di connessione è incoerente o interruzioni di connessione tra i caricamenti di pagina

Questo comportamento è previsto dalla progettazione. Poiché l'oggetto hub è ospitato nell'oggetto pagina, l'hub viene distrutta quando l'aggiornamento della pagina. Un'applicazione a più pagine deve gestire l'associazione tra utenti e l'ID connessione in modo che siano coerenti tra caricamento pagina. L'ID di connessione possono essere archiviati nel server in entrambi un `ConcurrentDictionary` oggetto o un database.

### <a name="value-cannot-be-null-error"></a>Errore "Valore non può essere null"

Sul lato server metodi con parametri facoltativi non sono attualmente supportati. Se il parametro facoltativo viene omesso, il metodo avrà esito negativo. Per ulteriori informazioni, vedere [parametri facoltativi](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox non è possibile stabilire una connessione al server a &lt;indirizzo&gt;" errore apportare modifiche

Questo messaggio di errore può essere visualizzato in Firebug se la negoziazione del trasporto WebSocket non riesce e viene invece utilizzato il trasporto di un altro. Questo comportamento è previsto dalla progettazione.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Errore "il certificato remoto non è valido in base alla procedura di convalida" nell'applicazione client .NET

Se il server richiede i certificati client personalizzato, è possibile aggiungere un x509certificate per la connessione prima che venga effettuata la richiesta. Aggiungere il certificato per la connessione utilizzando `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Connessione Elimina dopo il timeout di autenticazione

Questo comportamento è previsto dalla progettazione. Le credenziali di autenticazione non possono essere modificate mentre è attiva una connessione; Per aggiornare le credenziali, è necessario arrestare e riavviare la connessione.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected venga chiamato due volte tramite jQuery Mobile

del jQuery Mobile `initializePage` funzione forza gli script in ogni pagina può essere eseguita nuovamente, creando così una seconda connessione. Soluzioni per questo problema includono:

- Includere il riferimento a jQuery Mobile prima di file JavaScript.
- Disabilitare il `initializePage` funzione impostando `$.mobile.autoInitializePage = false`.
- Attendere che la pagina completare l'inizializzazione prima di avviare la connessione.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>I messaggi vengono posticipati nelle applicazioni Silverlight mediante gli eventi inviati Server

I messaggi vengono posticipati quando si utilizza server di inviate eventi su Silverlight. Per forzare lungo il polling da utilizzare in alternativa, utilizzare la seguente quando si avvia la connessione:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>Usando "Autorizzazione negata" Forever Frame protocollo

Si tratta di un problema noto, descritto [qui](https://github.com/SignalR/SignalR/issues/1963). Questo problema può verificarsi utilizzando la libreria JQuery più recente. la soluzione consiste nell'effettuare il downgrade all'applicazione di JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Errori di compilazione e sul lato server

 La sezione seguente contiene le possibili soluzioni a errori di runtime sul lato server e del compilatore. 

### <a name="reference-to-hub-instance-is-null"></a>Riferimento all'istanza dell'Hub è null

Poiché viene creata un'istanza di hub per ogni connessione, è possibile creare un'istanza di un hub nel codice manualmente. Per chiamare metodi su un hub all'esterno di hub di se stesso, vedere [come chiamare metodi di client e gestire gruppi appartenenti all'esterno della classe Hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) per informazioni su come ottenere un riferimento al contesto dell'hub.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session è null

Questo comportamento è previsto dalla progettazione. SignalR non supporta lo stato della sessione ASP.NET, perché lo stato della sessione di abilitazione causa l'interruzione della messaggistica duplex.

### <a name="no-suitable-method-to-override"></a>Alcun metodo adatto per eseguire l'override

Questo errore viene visualizzato se si utilizza un codice di documentazione precedente o blog. Verificare che si fa riferimento non i nomi dei metodi che sono stati modificati o deprecati (ad esempio `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl è null

Questo comportamento è previsto dalla progettazione. Questo membro è obsoleto e non deve essere utilizzato.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Una route denominata 'signalr.hubs' già nella raccolta di route "errore"

Verrà visualizzato questo errore se `MapHubs` viene chiamato due volte dall'applicazione. Alcune applicazioni di esempio chiamata `MapHubs` direttamente nel file di applicazione globale; altri effettuare la chiamata in una classe wrapper. Verificare che l'applicazione non entrambi.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemi di Visual Studio

In questa sezione vengono descritti i problemi rilevati in Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Nodo documenti di script non viene visualizzato in Esplora soluzioni

Alcune delle esercitazioni si direttamente al nodo "Documenti Script" in Esplora soluzioni durante il debug. Questo nodo viene generato dal debugger JavaScript e verrà visualizzata solo durante il debug di client browser in Internet Explorer. il nodo non verrà visualizzati se si utilizza Chrome o Firefox. Il debugger di JavaScript non verrà inoltre eseguito se un altro debugger client è in esecuzione, ad esempio il debugger di Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR non funziona in Visual Studio 2008 o versioni precedenti

Questo comportamento è previsto dalla progettazione. SignalR richiede .NET Framework 4 o versioni successive; è necessario che le applicazioni SignalR essere sviluppate in Visual Studio 2010 o versione successiva.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemi relativi a IIS

In questa sezione contiene i problemi con Internet Information Services.

### <a name="web-site-crashes-after-maphubs-call"></a>Sito Web si blocca dopo la chiamata MapHubs

Questo problema è stato risolto nella versione più recente di SignalR. Verificare che si sta utilizzando la versione più recente di SignalR aggiornando l'installazione tramite NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Problemi di Azure

In questa sezione contiene i problemi relativi a Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>I messaggi vengono ricevuti tramite Azure backplane dopo la modifica di nomi di argomento

Gli argomenti utilizzati da Azure backplane non deve essere configurabili dall'utente.
