---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: Guida di ASP.NET SignalR hub API - Client .NET (SignalR 1. x) | Documenti Microsoft
author: pfletcher
description: Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client di .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e gli svantaggi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9a61bd255a217876aa2fdbeb6389539483b9f013
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28037480"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a>Guida di ASP.NET SignalR hub API - Client .NET (SignalR 1. x)
====================
da [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client di .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e applicazioni console.
> 
> L'API degli hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server. Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano metodi che eseguono il client. Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi eseguiti nel server. SignalR occuparsene di plumbing client-server.
> 
> SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR, hub e le connessioni persistenti o per un'esercitazione che illustra come compilare un'applicazione SignalR completa, vedere [SignalR - Introduzione](../getting-started/index.md).


## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Installazione del client](#clientsetup)
- [Come stabilire una connessione](#establishconnection)

    - [Connessioni tra domini da client Silverlight](#slcrossdomain)
- [Come configurare la connessione](#configureconnection)

    - [Come impostare il numero massimo di connessioni simultanee in client WPF](#maxconnections)
    - [Come specificare i parametri di stringa di query](#querystring)
    - [Come specificare il metodo di trasporto](#transport)
    - [Come specificare le intestazioni HTTP](#httpheaders)
    - [Come specificare i certificati client](#clientcertificate)
- [Come creare il proxy dell'Hub](#proxy)
- [Come definire i metodi nel client che il server può chiamare](#callclient)

    - [Metodi senza parametri](#clientmethodswithoutparms)
    - [Metodi con parametri, che specificano i tipi di parametro](#clientmethodswithparmtypes)
    - [Metodi con parametri, specificare gli oggetti dinamici per i parametri](#clientmethodswithdynamparms)
    - [Come rimuovere un gestore](#removehandler)
- [Come chiamare i metodi di server dal client](#callserver)
- [Come gestire gli eventi di durata connessione](#connectionlifetime)
- [Come gestire gli errori](#handleerrors)
- [Come abilitare la registrazione lato client](#logging)
- [Esempi di codice di applicazione console per i metodi di client che il server può chiamare WPF e Silverlight](#wpfsl)

Per un progetto client di esempio .NET, vedere le risorse seguenti:

- [Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) su GitHub.com (esempi di app console WinRT, Silverlight,).
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) su GitHub.com (ad esempio WPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) su GitHub.com (ad esempio app Console).

Per la documentazione su come programmare il server o client JavaScript, vedere le risorse seguenti:

- [Guida di API degli hub SignalR - Server](../guide-to-the-api/hubs-api-guide-server.md)
- [Guida di API degli hub SignalR - Client JavaScript](../guide-to-the-api/hubs-api-guide-javascript-client.md)

I collegamenti agli argomenti di riferimento API sono alla versione dell'API di .NET 4.5. Se si utilizza .NET 4, vedere [la versione di .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Installazione del client

Installare il [italiano](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pacchetto NuGet (non il [Microsoft. ASPNET](http://nuget.org/packages/microsoft.aspnet.signalr) pacchetto). Questo pacchetto supporta WinRT, Silverlight, WPF, applicazione console e i client di Windows Phone, per .NET 4 e .NET 4.5.

Se la versione di SignalR presenti nel client è diversa dalla versione di cui è installato il server, SignalR è spesso in grado di adattarsi alla differenza. Ad esempio, quando viene rilasciato SignalR versione 2.0 e si installa che sul server, il server verrà supporta i client che dispongono di 1.1.x installata sia client che dispongono di 2.0 installato. Se la differenza tra la versione sul server e la versione del client è eccessiva, SignalR genera un `InvalidOperationException` eccezione quando il client tenta di stabilire una connessione. Messaggio di errore "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Come stabilire una connessione

Prima di stabilire una connessione, è necessario creare un `HubConnection` dell'oggetto e creare un proxy. Per stabilire la connessione, chiamare il `Start` metodo il `HubConnection` oggetto.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> Per i client JavaScript è necessario registrare almeno un gestore dell'evento prima di chiamare il `Start` metodo per stabilire la connessione. Questa operazione è necessaria per i client .NET. Per i client JavaScript, il codice proxy generato automaticamente viene creato proxy per tutti gli hub che esistano nel server e la registrazione di un gestore come indicare quali hub il client intende utilizzare. Ma per un client .NET è creare proxy Hub manualmente, in modo SignalR si presuppone che verrà utilizzato alcun Hub che si crea un proxy per.


Il codice di esempio viene utilizzato il valore predefinito "/ signalr" URL per la connessione al servizio SignalR. Per informazioni su come specificare un URL di base diversi, vedere [ASP.NET SignalR hub API Guida - Server - URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

Il `Start` metodo viene eseguito in modo asincrono. Per assicurarsi che le successive righe di codice non vengono eseguite fino al dopo aver stabilita la connessione, utilizzare `await` in un metodo asincrono ASP.NET 4.5 o `.Wait()` in un metodo sincrono. Non utilizzare `.Wait()` in un client di WinRT.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

La classe `HubConnection` è thread-safe.

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Connessioni tra domini da client Silverlight

Per informazioni su come abilitare le connessioni tra domini da client Silverlight, vedere [effettua un servizio disponibile tra confini di domini](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Come configurare la connessione

Prima stabilire una connessione, è possibile specificare le opzioni seguenti:

- Limite di connessioni simultanee.
- Parametri di stringa di query.
- Il metodo di trasporto.
- Intestazioni HTTP.
- Certificati client.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Come impostare il numero massimo di connessioni simultanee in client WPF

Nei client WPF, potrebbe essere necessario aumentare il numero massimo di connessioni simultanee rispetto al valore predefinito di 2. Il valore consigliato è 10.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Per ulteriori informazioni, vedere [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Come specificare i parametri di stringa di query

Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri di stringa di query all'oggetto connessione. Nell'esempio seguente viene illustrato come impostare un parametro di stringa di query nel codice client.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Come specificare il metodo di trasporto

Come parte del processo di connessione, un client SignalR normalmente negozia con il server per determinare il migliore trasporto supportato dal server e client. Se si conosce già il trasporto a cui si desidera utilizzare, è possibile ignorare questo processo di negoziazione. Per specificare il metodo di trasporto, passare un oggetto di trasporto per il metodo di avvio. Nell'esempio seguente viene illustrato come specificare il metodo di trasporto nel codice client.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

Il [Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) spazio dei nomi include le classi seguenti che è possibile utilizzare per specificare il trasporto.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponibile solo quando i server e client utilizzano .NET 4.5).
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (sceglie automaticamente il migliore trasporto supportato dal client sia sul server. Questo è il trasporto predefinito. Il passaggio di questa funzione è presente per il `Start` metodo ha lo stesso effetto del passaggio non qualcosa.)

Il trasporto ForeverFrame non è incluso nell'elenco perché è utilizzato solo da alcuni browser.

Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [ASP.NET SignalR hub API Guida - Server - come ottenere informazioni sul client dalla proprietà di contesto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Per ulteriori informazioni sui trasporti e di fallback, vedere [Introduzione a SignalR - trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Come specificare le intestazioni HTTP

Per impostare le intestazioni HTTP, utilizzare il `Headers` proprietà sull'oggetto connessione. Nell'esempio seguente viene illustrato come aggiungere un'intestazione HTTP.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Come specificare i certificati client

Per aggiungere i certificati client, utilizzare il `AddClientCertificate` metodo sull'oggetto connessione.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Come creare il proxy dell'Hub

Per definire i metodi nel client che è possibile chiamare un Hub dal server e per richiamare metodi su un Hub sul server, è possibile creare un proxy per l'Hub chiamando `CreateHubProxy` sull'oggetto connessione. La stringa viene passato a `CreateHubProxy` è il nome della classe Hub o il nome specificato per il `HubName` attributo se è stata utilizzata nel server. Nome non è rilevante.

**Classe hub sul server**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Creare il proxy client per la classe di Hub**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Se si decorata la classe Hub con un `HubName` attributo, utilizzare tale nome.

**Classe hub sul server**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

**Creare il proxy client per la classe di Hub**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

L'oggetto proxy è thread-safe. Infatti, se si chiama `HubConnection.CreateHubProxy` più volte con lo stesso `hubName`, si ottiene lo stesso nella cache `IHubProxy` oggetto.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Come definire i metodi nel client che il server può chiamare

Per definire un metodo che è possibile chiamare il server, utilizzare il proxy `On` metodo per registrare un gestore eventi.

Nome di metodo corrispondente è tra maiuscole e minuscole. Ad esempio, `Clients.All.UpdateStockPrice` sul server eseguirà `updateStockPrice`, `updatestockprice`, o `UpdateStockPrice` sul client.

Piattaforme client diverse hanno requisiti diversi per la scrittura di codice di metodo per aggiornare l'interfaccia utente. Gli esempi illustrati sono per i client WinRT (Windows Store .NET). Vengono forniti esempi di applicazione console, WPF e Silverlight in [una sezione separata più avanti in questo argomento](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Metodi senza parametri

Se il metodo che si sta gestendo non dispone di parametri, utilizzare il metodo di overload non generico di `On` metodo:

**Chiamata del metodo client senza parametri codice server**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Codice WinRT Client per il metodo chiamato dal server senza parametri ([WPF e Silverlight esempi più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metodi con parametri, che specificano i tipi di parametro

Se il metodo per gestire include parametri, specificare i tipi dei parametri come i tipi generici del `On` metodo. Esistono overload generico del `On` metodo che consente di specificare i parametri fino a 8 (4 in Windows Phone 7). Nell'esempio seguente, viene inviato un parametro per il `UpdateStockPrice` metodo.

**Metodo client con un parametro di codice server**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**La classe Stock utilizzata per il parametro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

**Codice WinRT Client per un metodo chiamato dal server con un parametro ([WPF e Silverlight esempi più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metodi con parametri, specificare gli oggetti dinamici per i parametri

In alternativa, per specificare parametri come tipi generici del `On` (metodo), è possibile specificare i parametri come oggetti dinamici:

**Metodo client con un parametro di codice server**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**La classe Stock utilizzata per il parametro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

**Codice WinRT Client per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro ([WPF e Silverlight esempi più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Come rimuovere un gestore

Per rimuovere un gestore, chiamare il relativo `Dispose` metodo.

**Codice client per un metodo chiamato dal server**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Codice client per rimuovere il gestore**

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Come chiamare i metodi di server dal client

Per chiamare un metodo nel server, utilizzare il `Invoke` metodo sul proxy di Hub.

Se il metodo server non ha restituito alcun valore, utilizzare il metodo di overload non generico di `Invoke` metodo.

**Codice lato server per un metodo che non restituisce alcun valore**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Codice client chiama un metodo che non restituisce alcun valore**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Se il metodo server ha un valore restituito, specificare il tipo restituito del tipo generico del `Invoke` metodo.

**Codice lato server per un metodo che restituisce un valore e accetta un parametro di tipo complesso**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**La classe Stock utilizzato per il parametro e valore restituito**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

**Codice client chiama un metodo che presenta un valore restituito e accetta un parametro di tipo complesso, in un metodo asincrono ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Codice client chiama un metodo che presenta un valore restituito e accetta un parametro di tipo complesso, in un metodo sincrono**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

Il `Invoke` metodo viene eseguito in modo asincrono e restituisce un `Task` oggetto. Se non si specifica `await` o `.Wait()`, la riga successiva del codice verrà eseguito prima il metodo che si richiama ha terminato l'esecuzione.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Come gestire gli eventi di durata connessione

SignalR fornisce la seguente connessione di eventi di durata che è possibile gestire:

- `Received`: Generato quando vengono ricevuti dati per la connessione. Fornisce i dati ricevuti.
- `ConnectionSlow`: Generato quando il client rileva una connessione lenta o l'eliminazione di frequente.
- `Reconnecting`: Generato quando il trasporto sottostante inizia la riconnessione.
- `Reconnected`: Generato quando il trasporto sottostante è stata riconnessa.
- `StateChanged`: Generato quando cambia lo stato di connessione. Fornisce lo stato precedente e il nuovo stato. Per informazioni sulla connessione vedere i valori dello stato [enumerazione ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Generato quando la connessione è interrotta.

Ad esempio, se si desidera visualizzare i messaggi di avviso per gli errori non irreversibili ma causano problemi di connessione intermittenti, ad esempio di come ciò o frequenti eliminazione della connessione, gestiscono il `ConnectionSlow` evento.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

Per ulteriori informazioni, vedere [comprensione e gestione degli eventi di durata connessione in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Come gestire gli errori

Se si non abilita in modo esplicito i messaggi di errore dettagliato nel server, l'oggetto eccezione che restituisce SignalR dopo un errore contiene informazioni minime sull'errore. Ad esempio, se una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati per i client nell'ambiente di produzione non è consigliata per motivi di sicurezza, ma se si desidera abilitare per i messaggi di errore dettagliati risoluzione dei problemi, utilizzare il codice seguente nel server.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Per gestire gli errori che genera SignalR, è possibile aggiungere un gestore per il `Error` eventi sull'oggetto connessione.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

Per gestire gli errori da chiamate al metodo, inserire il codice in un blocco try-catch.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Come abilitare la registrazione lato client

Per abilitare la registrazione lato client, impostare il `TraceLevel` e `TraceWriter` le proprietà dell'oggetto connessione.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>Esempi di codice di applicazione console per i metodi di client che il server può chiamare WPF e Silverlight

Gli esempi di codice illustrati in precedenza per la definizione di metodi di client che può chiamare il server si applicano ai client di WinRT. Negli esempi seguenti mostrano l'equivalente del codice per client di applicazioni console WPF e Silverlight.

### <a name="methods-without-parameters"></a>Metodi senza parametri

**Codice client WPF per il metodo chiamato dal server senza parametri**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Codice client Silverlight per il metodo chiamato dal server senza parametri**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Codice client dell'applicazione console per il metodo chiamato dal server senza parametri**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metodi con parametri, che specificano i tipi di parametro

**Codice client WPF per un metodo chiamato dal server con un parametro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Codice client Silverlight per un metodo chiamato dal server con un parametro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Codice dell'applicazione client per un metodo chiamato dal server con un parametro di console**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metodi con parametri, specificare gli oggetti dinamici per i parametri

**Codice client WPF per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Codice client Silverlight per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Console di codice dell'applicazione client per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
