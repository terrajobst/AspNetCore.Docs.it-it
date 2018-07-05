---
uid: signalr/overview/older-versions/troubleshooting
title: Risoluzione dei problemi di SignalR (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Questo articolo descrive i problemi comuni di sviluppo di applicazioni SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 94b7ec44fbe54b114baef6240f303a1e3b706741
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372426"
---
<a name="signalr-troubleshooting-signalr-1x"></a>Risoluzione dei problemi di SignalR (SignalR 1.x)
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> Questo documento descrive problemi comuni di risoluzione dei problemi di SignalR.


Questo documento contiene le sezioni seguenti.

- [La chiamata di metodi tra il client e il server ha esito negativo in modo invisibile](#connection)
- [Altri problemi di connessione](#other)
- [Errori di compilazione e sul lato server](#server)
- [Problemi di Visual Studio](#vs)
- [Problemi di Internet Information Services](#iis)
- [Problemi di Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>La chiamata di metodi tra il client e il server ha esito negativo in modo invisibile

Questa sezione vengono descritte le cause per una chiamata al metodo tra client e server avrà esito negativo senza un messaggio di errore significativo. In un'applicazione di SignalR, il server non dispone di informazioni sui metodi che il client implementa; Quando il server richiama un metodo client, i dati di nome e il parametro di metodo vengono inviati al client e il metodo viene eseguito solo se esiste in un formato che il server specificato. Se viene trovato alcun metodo corrisponda nel client, rimangono invariate, e non viene generato alcun messaggio di errore nel server.

Per ulteriori indagini non introduzione chiamati i metodi di client, è possibile attivare la registrazione prima di chiamare il metodo start sull'hub per vedere quali le chiamate sono provenienti dal server. Per abilitare la registrazione in un'applicazione JavaScript, vedere [come abilitare la registrazione lato client (versione del client JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Per abilitare la registrazione in un'applicazione client .NET, vedere [come abilitare la registrazione lato client (versione del Client .NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Metodo contenente errori di ortografia, firma del metodo non corretto o nome hub non corretto

Se il nome o firma di un metodo chiamato corrisponde esattamente a un metodo appropriato nel client, la chiamata avrà esito negativo. Verificare che il nome del metodo chiamato dal server corrisponda al nome del metodo sul client. SignalR crea inoltre, il proxy dell'hub utilizzando metodi di maiuscole/minuscole camel, come appropriato in JavaScript, pertanto, un metodo chiamato `SendMessage` nel server deve essere chiamato `sendMessage` nel proxy client. Se si usa il `HubName` attributo nel codice lato server, verificare che il nome utilizzato corrisponda al nome usato per creare l'hub nel client. Se non si usa il `HubName` attributo, verificare che il nome dell'hub in un client JavaScript sia maiuscole/minuscole camel, ad esempio chatHub anziché ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nome del metodo duplicata nel client

Verificare che non è un metodo duplicato nel client che differisce solo per i casi. Se l'applicazione client ha un metodo denominato `sendMessage`, verificare che non vi è anche un metodo denominato `SendMessage` anche.

### <a name="missing-json-parser-on-the-client"></a>Parser JSON mancante nel client

SignalR è necessario un parser JSON deve essere presente per serializzare le chiamate tra il server e client. Se il client non è un parser JSON incorporato (ad esempio Internet Explorer 7), è necessario includere una nell'applicazione. È possibile scaricare il parser JSON [qui](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Combinare la sintassi dell'Hub e PersistentConnection

SignalR utilizza i due modelli di comunicazione: hub e PersistentConnections. La sintassi per la chiamata di questi modelli di due comunicazione è diversa nel codice client. Se è stato aggiunto un hub nel codice del server, verificare che tutto il codice client usa la sintassi appropriata dell'hub.

**Codice client JavaScript che crea un PersistentConnection in un client JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Codice client JavaScript che crea un Proxy di Hub in un client Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**Codice c# server che esegue il mapping di una route a una PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**Codice c# server che esegue il mapping di una route a un Hub o in più hub se sono presenti più applicazioni**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Connessione avviata prima dell'aggiunta di sottoscrizioni

Se la connessione dell'Hub è stata avviata prima dell'aggiunta di metodi che possono essere chiamati dal server al proxy, non verranno ricevuti i messaggi. Il codice JavaScript seguente non verrà avviato correttamente l'hub:

**Codice client JavaScript non corretto che non consente i messaggi di hub per la ricezione**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Al contrario, aggiungere le sottoscrizioni di metodo prima di chiamare Start:

**Codice client JavaScript che aggiunga correttamente le sottoscrizioni a un hub**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Nome metodo mancante per il proxy dell'hub

Verificare che il metodo definito nel server è sottoscritto a sul client. Anche se il server definisce il metodo, è necessario aggiungerlo comunque al proxy client. Metodi possono essere aggiunti al proxy client nei modi seguenti (si noti che il metodo viene aggiunto per il `client` membro dell'hub, non l'hub direttamente):

**Codice client JavaScript che consente di aggiungere metodi a un proxy di hub**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Hub o metodi dell'hub non è dichiarati come pubblico

Per essere visibile nel client, l'implementazione dell'hub e i metodi devono essere dichiarati come `public`.

### <a name="accessing-hub-from-a-different-application"></a>L'accesso a hub da un'altra applicazione

Hub SignalR è accessibile solo tramite applicazioni che implementano i client SignalR. SignalR non è possibile interagire con altre librerie di comunicazione (ad esempio SOAP o WCF servizi web.) Se nessun client SignalR è disponibile per la piattaforma di destinazione, è possibile accedere direttamente l'endpoint del server.

### <a name="manually-serializing-data"></a>Manualmente la serializzazione dei dati

SignalR non utilizzeranno automaticamente JSON per il metodo serialize alcuna necessità parametri-non esiste di eseguire tale operazione manualmente.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Remoto metodo dell'Hub non eseguita nel client in OnDisconnected (funzione)

Questo comportamento è previsto dalla progettazione. Quando `OnDisconnected` viene chiamato, l'hub ha già acceduto il `Disconnected` lo stato, che non consente inoltre di chiamare i metodi dell'hub.

**Codice c# server che esegue correttamente il codice nell'evento OnDisconnected**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Raggiunto il limite di connessione

Quando si usa la versione completa di IIS in un sistema operativo client, ad esempio Windows 7, viene imposto un limite di 10 connessioni. Quando si usa un sistema operativo client, utilizzare IIS Express per evitare questo limite.

### <a name="cross-domain-connection-not-set-up-properly"></a>Connessione tra domini non configurato correttamente

Se una connessione cross-domain (una connessione per il quale l'URL di SignalR non è nello stesso dominio della pagina di hosting) non è configurata correttamente, la connessione può non riuscire senza un messaggio di errore. Per informazioni su come abilitare la comunicazione tra domini, vedere [come stabilire una connessione cross-domain](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Connessione tramite NTLM (Active Directory) non funziona nel client .NET

Una connessione bism in un'applicazione client .NET che utilizza la sicurezza del dominio potrebbe non riuscire se la connessione non è configurata correttamente. Per usare SignalR in un ambiente di dominio, impostare la proprietà di connessione necessarie come indicato di seguito:

**Codice c# client che implementa le credenziali di connessione**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Altri problemi di connessione

In questa sezione vengono descritte le cause e soluzioni per sintomi specifici o messaggi di errore che si verificano durante una connessione.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Errore di "Avvio deve essere chiamato prima i dati possono essere inviati"

Questo errore si verifica comunemente se codice fa riferimento a oggetti di SignalR prima che venga avviata la connessione. L'accade per i gestori e simili che verrà chiamata ai metodi definiti nel server deve essere aggiunta dopo il completamento della connessione. Si noti che la chiamata a `Start` è asincrona, in modo da codice dopo la chiamata può essere eseguita prima che venga completato. Il modo migliore per aggiungere gestori dopo l'avvio di una connessione completamente è inserirle in una funzione di callback che viene passata come parametro al metodo di avvio:

**Codice client JavaScript che aggiunga correttamente i gestori di eventi che fanno riferimento a oggetti di SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Verrà visualizzato questo errore se una connessione viene arrestato mentre sono ancora presenti riferimenti oggetti SignalR.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>"301 spostato in modo permanente" o "temporaneamente spostato 302" errore

Questo errore può verificarsi se il progetto contiene una cartella denominata SignalR, onde evitare di interferire con il proxy creato automaticamente. Per evitare questo errore, non utilizzare una cartella denominata `SignalR` nell'applicazione, o generazione proxy automatica turn off. Visualizzare [il Proxy generato e che cosa fa per te](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) per altri dettagli.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>Errore "403 accesso negato" nel client .NET e Silverlight

Questo errore può verificarsi in ambienti tra domini in cui la comunicazione tra domini non è abilitata correttamente. Per informazioni su come abilitare la comunicazione tra domini, vedere [come stabilire una connessione cross-domain](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Per stabilire una connessione tra domini in un client Silverlight, vedere [le connessioni tra domini da client Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Errore "404 non trovato"

Esistono diverse cause di questo problema. Verificare che tutti gli elementi seguenti:

- **Riferimento di indirizzo proxy dell'hub non è formattato correttamente:** questo errore si verifica in genere se il riferimento per l'indirizzo del proxy dell'hub generato non è formattato correttamente. Verificare che il riferimento all'indirizzo hub viene effettuato correttamente. Visualizzare [fanno riferimento al proxy generato dinamicamente come](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) per informazioni dettagliate.
- **Aggiunta di cicli di applicazione prima di aggiungere la route dell'hub:** se l'applicazione usa altre route, verificare che la prima route aggiunta sia la chiamata a `MapHubs`.

### <a name="500-internal-server-error"></a>"Errore interno 500 del Server"

Si tratta di un errore molto generico che potrebbe avere un'ampia gamma di cause. I dettagli dell'errore dovrebbe essere visualizzato nel registro eventi del server oppure possono essere trovati tramite il server di debug. Informazioni più dettagliate sugli errori possono essere ottenute attivando errori dettagliati nel server. Per altre informazioni, vedere [come gestire gli errori nella classe Hub](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt; è definito" errore

Verrà generato questo errore se la chiamata a `MapHubs` non viene eseguita correttamente. Visualizzare [come registrare le route di SignalR e configurare le opzioni di SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) per altre informazioni.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException non è stata gestita dal codice utente

Verificare che i parametri inviati ai metodi non includono i tipi non serializzabili (ad esempio gli handle di file o le connessioni di database). Se è necessario usare i membri in un oggetto sul lato server che non si vuole essere inviato al client, perché per la sicurezza o per motivi di serializzazione, utilizzare il `JSONIgnore` attributo.

### <a name="protocol-error-unknown-transport-error"></a>"Errore di protocollo: trasporto sconosciuto" errore

Questo errore può verificarsi se il client non supporta i trasporti che usa SignalR. Visualizzare [trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports) per informazioni su quali browser può essere usato con SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Generazione di proxy dell'Hub di JavaScript è stata disabilitata".

Questo errore si verifica se `DisableJavaScriptProxies` viene impostato quando si includono anche un riferimento al proxy generato dinamicamente alla `signalr/hubs`. Per altre informazioni su come creare manualmente il proxy, vedere [proxy generato e che cosa fa per te](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"L'ID di connessione è in formato non corretto" o "non è possibile modificare l'identità dell'utente durante una connessione SignalR attiva" errore

Questo errore può verificarsi se viene utilizzata l'autenticazione e il client viene disconnesso prima che venga interrotta la connessione. La soluzione consiste nell'interrompere la connessione SignalR prima della disconnessione del client.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Errore di non rilevate: SignalR: jQuery non trovato. Assicurarsi prima di file SignalR.js cui viene fatto riferimento jQuery"errore

Il client SignalR JavaScript richiede jQuery per l'esecuzione. Verificare che il riferimento jQuery è corretto, che il percorso usato sia valido e che il riferimento a jQuery è prima di riferimento per SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Non rilevate TypeError: non è in grado di leggere proprietà '&lt;proprietà&gt;' non definito" errore

Questo errore si verifica dall'assenza di jQuery o il proxy di hub fa in modo corretto. Verificare che sia corretto, che il percorso usato sia valido e che il riferimento a jQuery sia prima del riferimento al proxy di hub di riferimento per jQuery e il proxy di hub. Il riferimento al proxy di hub predefinito dovrebbe essere simile al seguente:

**Codice sul lato client HTML che fa riferimento in modo corretto il proxy di hub**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Errore "RuntimeBinderException non è stata gestita dal codice utente"

Questo errore può verificarsi quando l'overload non corretto di `Hub.On` viene usato. Se il metodo ha un valore restituito, il tipo restituito deve essere specificato come parametro di tipo generico:

**Metodo definito nel client (senza proxy generato)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>ID connessione non è coerente o connessione si interrompe tra i caricamenti pagina

Questo comportamento è previsto dalla progettazione. Poiché l'oggetto hub è ospitato nell'oggetto di pagina, l'hub viene eliminato quando la pagina viene aggiornata. Un'applicazione a più pagine deve mantenere l'associazione tra utenti e ID di connessione in modo che siano coerenti tra i caricamenti di pagina. L'ID di connessione possono essere archiviati nel server in uno una `ConcurrentDictionary` oggetto o un database.

### <a name="value-cannot-be-null-error"></a>Errore "Valore non può essere null"

Metodi sul lato server con i parametri facoltativi non sono attualmente supportati. Se il parametro facoltativo viene omesso, il metodo avrà esito negativo. Per altre informazioni, vedere [parametri facoltativi](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Non riesce a stabilire una connessione al server in Firefox &lt;indirizzo&gt;" errore nonché di apportare modifiche

Questo messaggio di errore può essere visualizzato nonché di apportare modifiche se la negoziazione del trasporto WebSocket non riesce e viene invece usato l'altro tipo di trasporto. Questo comportamento è previsto dalla progettazione.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Errore "il certificato remoto non è valido in base alla procedura di convalida" nell'applicazione client .NET

Se il server richiede i certificati client personalizzato, è possibile aggiungere un x509certificate per la connessione prima che venga effettuata la richiesta. Aggiungere il certificato per la connessione usando `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Connessione Elimina dopo l'autenticazione di timeout

Questo comportamento è previsto dalla progettazione. Le credenziali di autenticazione non possono essere modificate mentre è attiva una connessione; Per aggiornare le credenziali, la connessione deve essere arrestata e riavviata.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected viene chiamato due volte quando si usa jQuery Mobile

del jQuery Mobile `initializePage` funzione impone gli script in ogni pagina per essere eseguita nuovamente, creando così una seconda connessione. Soluzioni per risolvere questo problema includono:

- Includere il riferimento a jQuery Mobile prima il file JavaScript.
- Disabilitare il `initializePage` funzione impostando `$.mobile.autoInitializePage = false`.
- Attendere che la pagina completare l'inizializzazione prima di avviare la connessione.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Messaggi vengono posticipati nelle applicazioni Silverlight mediante gli eventi inviati Server

Messaggi vengono posticipati quando Usa server inviato gli eventi con Silverlight. Per forzare il tempo di polling da utilizzare in alternativa, usare quanto segue quando si avvia la connessione:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>Utilizzando "Negato l'autorizzazione" Forever Frame protocollo

Si tratta di un problema noto, descritto [qui](https://github.com/SignalR/SignalR/issues/1963). Questo sintomo può essere visualizzato usando la libreria JQuery più recente; la soluzione alternativa consiste nell'effettuare il downgrade all'applicazione di JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Errori di compilazione e sul lato server

 La sezione seguente contiene le possibili soluzioni al compilatore e degli errori di runtime lato server. 

### <a name="reference-to-hub-instance-is-null"></a>Riferimento all'istanza dell'Hub è null

Poiché per ogni connessione viene creata un'istanza di hub, è possibile creare un'istanza di un hub nel codice manualmente. Per chiamare metodi su un hub dall'esterno per lo stesso hub, vedere [come client di chiamare metodi e gestire i gruppi dall'esterno della classe dell'Hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) per informazioni su come ottenere un riferimento al contesto dell'hub.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session è null

Questo comportamento è previsto dalla progettazione. SignalR non supporta lo stato della sessione ASP.NET, poiché l'abilitazione dello stato della sessione causa l'interruzione della messaggistica duplex.

### <a name="no-suitable-method-to-override"></a>Alcun metodo adatto per eseguire l'override

Questo errore si può verificarsi se si usa code dalla documentazione precedente o nei blog. Verificare che siano non fa riferimento a nomi di metodi che sono stati modificati o deprecati (ad esempio `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl è null

Questo comportamento è previsto dalla progettazione. Questo membro è deprecato e non deve essere utilizzato.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Errore "una route denominata 'signalr.hubs' è già nella raccolta di route"

Questo errore verrà visualizzato se `MapHubs` viene chiamato due volte dall'applicazione. Alcune applicazioni di esempio chiamata `MapHubs` direttamente nel file di applicazione globale; altri effettuare la chiamata in una classe wrapper. Assicurarsi che l'applicazione non esegue entrambe.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemi di Visual Studio

In questa sezione vengono descritti i problemi rilevati in Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Nodo documenti script non viene visualizzato in Esplora soluzioni

Alcune delle esercitazioni consentiranno di accedere al nodo "Documenti Script" in Esplora soluzioni durante il debug. Questo nodo viene generato dal debugger JavaScript e verrà visualizzata solo durante il debug di client browser in Internet Explorer. il nodo non viene visualizzata se si utilizza Chrome o Firefox. Il debugger JavaScript non verrà inoltre eseguita se un altro debugger client è in esecuzione, ad esempio il debugger di Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR non funziona in Visual Studio 2008 o versioni precedenti

Questo comportamento è previsto dalla progettazione. SignalR richiede .NET Framework 4 o versione successiva; Ciò richiede che le applicazioni SignalR essere sviluppate in Visual Studio 2010 o versioni successive.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemi relativi a IIS

Questa sezione contiene i problemi con Internet Information Services.

### <a name="web-site-crashes-after-maphubs-call"></a>Sito Web si blocca una volta MapHubs chiamare

Questo problema è stato risolto nella versione più recente di SignalR. Verificare che si usa l'ultima versione rilasciata di SignalR aggiornando l'installazione tramite NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Problemi di Azure

Questa sezione contiene i problemi con Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>I messaggi non vengono ricevuti tramite il backplane di Azure dopo la modifica di nomi di argomento

Gli argomenti usati da Azure backplane non dovranno essere configurabile dall'utente.
