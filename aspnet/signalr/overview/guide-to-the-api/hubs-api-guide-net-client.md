---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Guida all'API di ASP.NET SignalR Hubs - Client .NET (c#) | Microsoft Docs
author: pfletcher
description: Questo documento viene fornita un'introduzione all'uso dell'API di hub per la versione 2 nel client .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e gli svantaggi di SignalR...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 0a2b24039259ef90579a7f215bb9e35ebef7b9b9
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288041"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>Guida all'API di ASP.NET SignalR Hubs - Client .NET (c#)
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo documento fornisce un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client .NET, ad esempio applicazioni console, WPF, Silverlight e Windows Store (WinRT).
>
> L'API di hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server. Nel codice server, si definiscono i metodi che possono essere chiamati da parte dei client e si chiamano i metodi eseguiti nel client. Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi che eseguono sul server. SignalR si occupa di tutte le attività client-server.
>
> SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti. Per un'introduzione a SignalR hub e connessioni permanenti o per un'esercitazione che illustra come compilare un'applicazione di SignalR completa, vedere [SignalR - Guida introduttiva](../getting-started/index.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Programma di installazione client](#clientsetup)
- [Come stabilire una connessione](#establishconnection)

    - [Connessioni tra domini da client Silverlight](#slcrossdomain)
- [Come configurare la connessione](#configureconnection)

    - [Come impostare il numero massimo di connessioni simultanee nei client WPF](#maxconnections)
    - [Come specificare i parametri di stringa di query](#querystring)
    - [Come specificare il metodo di trasporto](#transport)
    - [Come specificare le intestazioni HTTP](#httpheaders)
    - [Come specificare i certificati client](#clientcertificate)
- [Come creare il proxy dell'Hub](#proxy)
- [Come definire i metodi sul client che il server può chiamare](#callclient)

    - [Metodi senza parametri](#clientmethodswithoutparms)
    - [Metodi con parametri, che specificano i tipi di parametro](#clientmethodswithparmtypes)
    - [Metodi con parametri, che specificano gli oggetti dinamici per i parametri](#clientmethodswithdynamparms)
    - [Come rimuovere un gestore](#removehandler)
- [Come chiamare i metodi del server dal client](#callserver)
- [Come gestire gli eventi di durata connessione](#connectionlifetime)
- [Come gestire gli errori](#handleerrors)
- [Come abilitare la registrazione lato client](#logging)
- [Esempi di codice di applicazione console per i metodi di client che il server può chiamare, Silverlight e WPF](#wpfsl)

Per un progetto di client .NET di esempio, vedere le risorse seguenti:

- [Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) in GitHub.com (esempi di app console di WinRT, Silverlight,).
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) in GitHub.com (ad esempio WPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) in GitHub.com (esempio di app Console).

Per informazioni su come programmare il server o client JavaScript, vedere le risorse seguenti:

- [Guida all'API di SignalR Hubs - Server](hubs-api-guide-server.md)
- [Guida all'API di SignalR Hubs - Client JavaScript](hubs-api-guide-javascript-client.md)

I collegamenti agli argomenti di riferimento all'API sono alla versione dell'API .NET 4.5. Se si usa .NET 4, vedere [la versione di .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Programma di installazione client

Installare il [ASPNET](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pacchetto NuGet (non il [ASPNET](http://nuget.org/packages/microsoft.aspnet.signalr) pacchetto). Questo pacchetto supporta WinRT, Silverlight, WPF, applicazione console e i client di Windows Phone, per .NET 4 e .NET 4.5.

Se la versione di SignalR in uso sul client è diversa da quella che si dispone sul server, SignalR è spesso in grado di adattarsi alla differenza. Ad esempio, un server che esegue la versione 2 di SignalR supporterà i client che hanno installato 1.1.x, nonché i client con la versione 2 installato. Se la differenza tra la versione sul server e la versione del client è troppo elevata o se il client è più recente rispetto al server, SignalR genera un `InvalidOperationException` eccezione quando il client prova a stabilire una connessione. Messaggio di errore "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Come stabilire una connessione

Prima di stabilire una connessione, è necessario creare un `HubConnection` dell'oggetto e creare un proxy. Per stabilire la connessione, chiamare il `Start` metodo di `HubConnection` oggetto.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> Per i client JavaScript è necessario registrare almeno un gestore di eventi prima di chiamare il `Start` metodo per stabilire la connessione. Non è necessario per i client .NET. Per i client JavaScript, il codice proxy generato automaticamente Crea proxy per tutti gli hub che esiste nel server e la registrazione di un gestore di è consente di indicare quali hub il client intende utilizzare. Ma per un client .NET è creare i proxy di Hub manualmente, in modo da SignalR presuppone che si userà qualsiasi Hub che si crea un proxy per.


Il codice di esempio Usa il valore predefinito "/ signalr" URL per la connessione al servizio SignalR. Per informazioni su come specificare un URL di base diversi, vedere [Guida all'API di hub di ASP.NET SignalR - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).

Il `Start` metodo viene eseguito in modo asincrono. Per assicurarsi che le successive righe di codice non eseguire fino a dopo aver stabilita la connessione, usare `await` in un metodo asincrono di ASP.NET 4.5 o `.Wait()` in un metodo sincrono. Non usare `.Wait()` in un client di WinRT.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Connessioni tra domini da client Silverlight

Per informazioni su come abilitare le connessioni tra domini da client Silverlight, vedere [rendendo un servizio disponibile attraverso i limiti del dominio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Come configurare la connessione

Prima stabilire una connessione, è possibile specificare una delle opzioni seguenti:

- Limite di connessioni simultanee.
- I parametri della stringa di query.
- Il metodo di trasporto.
- Intestazioni HTTP.
- Certificati client.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Come impostare il numero massimo di connessioni simultanee nei client WPF

Nei client WPF, potrebbe essere necessario aumentare il numero massimo di connessioni simultanee rispetto al valore predefinito di 2. Il valore consigliato è 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Per altre informazioni, vedere [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Come specificare i parametri di stringa di query

Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri della stringa di query all'oggetto connessione. Nell'esempio seguente viene illustrato come impostare un parametro di stringa di query nel codice client.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Come specificare il metodo di trasporto

Durante il processo di connessione, un client SignalR normalmente negozia con il server per determinare il migliore trasporto supportato dal server e client. Se si conosce già il trasporto da usare, è possibile ignorare questo processo di negoziazione. Per specificare il metodo di trasporto, passare un oggetto di trasporto per il metodo di avvio. Nell'esempio seguente viene illustrato come specificare il metodo di trasporto nel codice client.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

Il [Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) dello spazio dei nomi include le classi seguenti che è possibile usare per specificare il trasporto.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponibile solo quando i server e client usare .NET 4.5).
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (sceglie automaticamente il migliore trasporto supportato dal client sia sul server. Questo è il trasporto predefinito. Il passaggio di questa opzione in per il `Start` metodo ha lo stesso effetto come non conformi in un elemento.)

Il trasporto ForeverFrame non è incluso nell'elenco perché è utilizzato solo dai browser.

Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [- Server - Guida all'API Hubs SignalR di ASP.NET come ottenere informazioni relative al client dalla proprietà di contesto](hubs-api-guide-server.md#contextproperty). Per altre informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR - trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Come specificare le intestazioni HTTP

Per impostare le intestazioni HTTP, usare il `Headers` proprietà sull'oggetto connessione. Nell'esempio seguente viene illustrato come aggiungere un'intestazione HTTP.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Come specificare i certificati client

Per aggiungere i certificati client, usare il `AddClientCertificate` metodo sull'oggetto connessione.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Come creare il proxy dell'Hub

Per definire i metodi sul client che un Hub è possibile chiamare dal server e per richiamare metodi su un Hub sul server, creare un proxy per l'Hub chiamando `CreateHubProxy` sull'oggetto connessione. La stringa è passare al `CreateHubProxy` è il nome della classe Hub o il nome specificato da di `HubName` attributo se è stata utilizzata nel server. Nome viene applicata alcuna distinzione tra maiuscole e minuscole.

**Classe dell'hub sul server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Creare il proxy client per la classe dell'Hub**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Se si decora la classe di Hub con un `HubName` attributo, usare il nome.

**Classe dell'hub sul server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Creare il proxy client per la classe dell'Hub**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

Se si chiama `HubConnection.CreateHubProxy` più volte con lo stesso `hubName`, si ottiene lo stesso memorizzato nella cache `IHubProxy` oggetto.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Come definire i metodi sul client che il server può chiamare

Per definire un metodo che può chiamare il server, usare il proxy `On` metodo per registrare un gestore eventi.

Metodo nome viene applicata alcuna distinzione tra maiuscole e minuscole. Ad esempio, `Clients.All.UpdateStockPrice` sul server eseguirà `updateStockPrice`, `updatestockprice`, o `UpdateStockPrice` sul client.

Piattaforme client diverse hanno requisiti diversi per la modalità di scrittura codice del metodo per aggiornare l'interfaccia utente. Gli esempi illustrati sono per i client di WinRT (Windows Store .NET). WPF, Silverlight ed esempi di applicazione console vengono forniti [una sezione separata più avanti in questo argomento](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Metodi senza parametri

Se il metodo in fase di gestione non dispone di parametri, usare l'overload del metodo non generico di `On` metodo:

**Codice lato server chiamata client metodo senza parametri**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Codice di WinRT Client per il metodo chiamato dal server senza parametri ([vedere gli esempi WPF e Silverlight più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metodi con parametri, che specificano i tipi di parametro

Se il metodo in fase di gestione dispone di parametri, specificare i tipi dei parametri come i tipi generici del `On` (metodo). Esistono overload generici del `On` metodo per consentire di specificare i parametri fino a 8 (4 in Windows Phone 7). Nell'esempio seguente, viene inviato un solo parametro per il `UpdateStockPrice` (metodo).

**Metodo client con un parametro di chiamata del codice server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**La classe Stock utilizzata per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**Il codice WinRT Client per un metodo chiamato dal server con un parametro ([vedere gli esempi WPF e Silverlight più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metodi con parametri, che specificano gli oggetti dinamici per i parametri

Come alternativa per specificare parametri come tipi generici del `On` metodo, è possibile specificare parametri come oggetti dinamici:

**Metodo client con un parametro di chiamata del codice server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**La classe Stock utilizzata per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**Il codice WinRT Client per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro ([vedere gli esempi WPF e Silverlight più avanti in questo argomento](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Come rimuovere un gestore

Per rimuovere un gestore, chiamare il `Dispose` (metodo).

**Codice client per un metodo chiamato dal server**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Codice client per rimuovere il gestore**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Come chiamare i metodi del server dal client

Per chiamare un metodo nel server, usare il `Invoke` metodo sul proxy dell'Hub.

Se il metodo del server non restituisce alcun valore, usare l'overload del metodo non generico di `Invoke` (metodo).

**Codice del server per un metodo che non restituisce alcun valore**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Codice client chiama un metodo che non restituisce alcun valore**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Se il metodo del server ha un valore restituito, specificare il tipo restituito come tipo generico del `Invoke` (metodo).

**Codice server per un metodo che presenta un valore restituito e accetta un parametro di tipo complesso**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**La classe Stock utilizzato per il parametro e valore restituito**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**Codice client chiama un metodo che presenta un valore restituito e accetta un parametro di tipo complesso, in un metodo asincrono di ASP.NET 4.5**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Codice client chiama un metodo che presenta un valore restituito e accetta un parametro di tipo complesso, in un metodo sincrono**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

Il `Invoke` metodo viene eseguito in modo asincrono e restituisce un `Task` oggetto. Se non si specifica `await` o `.Wait()`, la riga successiva del codice verrà eseguito prima il metodo che si richiama ha terminato l'esecuzione.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Come gestire gli eventi di durata connessione

SignalR fornisce il collegamento seguente gli eventi di durata che è possibile gestire:

- `Received`: Generato quando vengono ricevuti tutti i dati nella connessione. Fornisce i dati ricevuti.
- `ConnectionSlow`: Generato quando il client rileva una connessione lenta o frequentemente eliminazione.
- `Reconnecting`: Generato quando il trasporto sottostante inizia la riconnessione.
- `Reconnected`: Generato quando il trasporto sottostante è stata riconnessa.
- `StateChanged`: Generato quando cambia lo stato della connessione. Fornisce lo stato precedente e il nuovo stato. Per informazioni sulla connessione di vedere i valori dello stato [enumerazione ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Generato quando la connessione è disconnesso.

Ad esempio, se si desidera visualizzare i messaggi di avviso per gli errori non irreversibili ma causano problemi di connessione intermittente, ad esempio come lentezza o di frequenza eliminazione della connessione, gestire il `ConnectionSlow` evento.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione in SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Come gestire gli errori

Se si non abilita in modo esplicito i messaggi di errore dettagliato nel server, l'oggetto eccezione SignalR restituito dopo un errore contiene informazioni minime sull'errore. Ad esempio, se una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati per i client nell'ambiente di produzione non è consigliato per motivi di sicurezza, ma se si vuole abilitare i messaggi di errore dettagliato per risoluzione dei problemi, usare il codice seguente nel server.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Per gestire gli errori che genera SignalR, è possibile aggiungere un gestore per il `Error` evento sull'oggetto connessione.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Per gestire gli errori di chiamate al metodo, eseguire il wrapping di codice in un blocco try-catch.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Come abilitare la registrazione lato client

Per abilitare la registrazione lato client, impostare il `TraceLevel` e `TraceWriter` le proprietà dell'oggetto connessione.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>Esempi di codice di applicazione console per i metodi di client che il server può chiamare, Silverlight e WPF

Gli esempi di codice illustrati in precedenza per la definizione di metodi di client può chiamare il server si applicano ai client di WinRT. Gli esempi seguenti mostrano il codice equivalente per WPF, Silverlight e i client di applicazione console.

### <a name="methods-without-parameters"></a>Metodi senza parametri

**Codice client WPF per il metodo chiamato dal server senza parametri**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Codice client Silverlight per il metodo chiamato dal server senza parametri**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Il codice client dell'applicazione console per il metodo chiamato dal server senza parametri**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metodi con parametri, che specificano i tipi di parametro

**Codice client WPF per un metodo chiamato dal server con un parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Codice client Silverlight per un metodo chiamato dal server con un parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Il codice client dell'applicazione console per un metodo chiamato dal server con un parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metodi con parametri, che specificano gli oggetti dinamici per i parametri

**Codice client WPF per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Codice client Silverlight per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Il codice client dell'applicazione console per un metodo chiamato dal server con un parametro, usando un oggetto dinamico per il parametro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
