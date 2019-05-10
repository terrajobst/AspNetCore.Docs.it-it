---
title: Considerazioni sulla sicurezza in ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come usare l'autenticazione e autorizzazione in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: 6e9f849ed856cf1cbf989b8b16cab5209c465471
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892048"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Considerazioni sulla sicurezza in ASP.NET Core SignalR

Da [Andrew Stanton-Nurse](https://twitter.com/anurse)

Questo articolo fornisce informazioni sulla protezione di SignalR.

## <a name="cross-origin-resource-sharing"></a>Cross-origin resource Sharing, condivisione

[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) può essere utilizzato per consentire le connessioni SignalR multiorigine nel browser. Se il codice JavaScript è ospitato in un dominio diverso dall'app, SignalR [middleware CORS](xref:security/cors) deve essere abilitata per consentire il codice JavaScript per la connessione all'app di SignalR. Consentire richieste multiorigine solo dal controllo o di domini che attendibili. Ad esempio:

* Il sito è ospitato in `http://www.example.com`
* L'app di SignalR è ospitato in `http://signalr.example.com`

CORS deve essere configurato nell'app SignalR per consentire solo l'origine `www.example.com`.

Per altre informazioni sulla configurazione di CORS, vedere [abilita la richieste (CORS)](xref:security/cors). SignalR **richiede** i criteri CORS seguenti:

* Consenti le specifiche origini previsto. Consentire qualsiasi origine, è possibile ma viene **non** sicura o consigliati.
* Metodi HTTP `GET` e `POST` devono essere consentiti.
* Anche quando non viene utilizzata l'autenticazione, è necessario abilitare le credenziali.

Ad esempio, il seguente criterio CORS consente a un client di browser SignalR ospitato in `https://example.com` per accedere all'app di SignalR ospitato su `https://signalr.example.com`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> SignalR non è compatibile con la funzionalità incorporata di CORS nel servizio App di Azure.

## <a name="websocket-origin-restriction"></a>Restrizione di origine di WebSocket

::: moniker range=">= aspnetcore-2.2"

La protezione fornita da CORS non si applica agli oggetti WebSocket. Per la restrizione di origine su WebSockets, leggere [restrizione di origine di WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

La protezione fornita da CORS non si applica agli oggetti WebSocket. I browser **non**:

* Eseguono richieste CORS preventive.
* Rispettano le restrizioni specificate nelle intestazioni `Access-Control` quando eseguono richieste WebSocket.

I browser, tuttavia, inviano l'intestazione `Origin` quando rilasciano richieste WebSocket. Le applicazioni devono essere configurate per la convalida di queste intestazioni per assicurarsi che siano consentiti solo WebSocket provenienti dalle origini previste.

In ASP.NET Core 2.1 e versioni successive, è possibile ottenere la convalida delle intestazioni utilizzando un middleware personalizzato inserito **prima `UseSignalR`e middleware di autenticazione** in `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> L'intestazione `Origin` viene controllata dal client e, come l'intestazione `Referer`, può essere falsificata. Queste intestazioni devono **non** utilizzabile come un meccanismo di autenticazione.

::: moniker-end

## <a name="access-token-logging"></a>Registrazione di token di accesso

Quando si usa WebSocket o Server-Sent eventi, il browser client invia il token di accesso nella stringa di query. Ricevere il token di accesso tramite la stringa di query è in genere sicuro quanto lo standard `Authorization` intestazione. È consigliabile usare sempre HTTPS per garantire una connessione end-to-end sicura tra il client e il server. L'URL per ogni richiesta, tra cui la stringa di query di log di molti server web. Gli URL di registrazione può accedere il token di accesso. ASP.NET Core registra l'URL per ogni richiesta per impostazione predefinita, che include la stringa di query. Ad esempio:

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

Se si hanno dubbi sulla registrazione di questi dati con i log di server, è possibile disabilitare questa registrazione interamente configurando il `Microsoft.AspNetCore.Hosting` logger per il `Warning` a livello di o versione successiva (questi messaggi vengono scritti in `Info` a livello di). Vedere la documentazione sul [filtro dei Log](xref:fundamentals/logging/index#log-filtering) per altre informazioni. Se vuoi ancora alcune informazioni sulla richiesta di log, è possibile [scrivere un middleware](xref:fundamentals/middleware/write) per registrare i dati è richiesta e filtrare il `access_token` valore di stringa di query (se presente).

## <a name="exceptions"></a>Eccezioni

I messaggi di eccezione sono in genere considerati dati sensibili che non devono essere rivelati al client. Per impostazione predefinita, SignalR non invia i dettagli di un'eccezione generata da un metodo dell'hub al client. Al contrario, il client riceve un messaggio generico che indica che un errore. Recapito dei messaggi di eccezione al client può essere sostituito (ad esempio in sviluppo o test) da [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options). I messaggi di eccezione non devono essere esposti al client in app di produzione.

## <a name="buffer-management"></a>Gestione del buffer

SignalR utilizza i buffer per ogni connessione per gestire i messaggi in ingresso e in uscita. Per impostazione predefinita, SignalR limita i buffer su 32 KB. Il messaggio più grande di che un client o server può inviare è 32 KB. La memoria massima usata da una connessione per i messaggi è 32 KB. Se i messaggi sono sempre minori di 32 KB, è possibile ridurre il limite, quali:

* Impedisce la possibilità di inviare un messaggio di dimensioni maggiori di un client.
* Il server non sarà mai necessario allocare buffer di grandi dimensioni per accettare i messaggi.

Se i messaggi sono superiori a 32 KB, è possibile aumentare il limite. Aumentare questo limite si intende:

* Il client può causare il server di allocare i buffer di memoria di grandi dimensioni.
* Allocazione di server di buffer di grandi dimensioni può ridurre il numero di connessioni simultanee.

Sono previsti limiti per i messaggi in ingresso e in uscita, entrambi possono essere configurate nel [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) configurato nell'oggetto `MapHub`:

* `ApplicationMaxBufferSize` rappresenta il numero massimo di byte dal client che il buffer del server. Se il client tenta di inviare un messaggio di dimensioni superiori a questo limite, la connessione verrà chiusa.
* `TransportMaxBufferSize` rappresenta il numero massimo di byte che il server può inviare. Se il server tenta di inviare un messaggio (tra cui i valori restituiti dai metodi dell'hub) superano questo limite, verrà generata un'eccezione.

L'impostazione del limite `0` disattiva il limite. Rimozione del limite consente a un client inviare un messaggio di qualsiasi dimensione. Client non autorizzati l'invio di messaggi di grandi dimensioni può causare in eccesso della memoria da allocare. Utilizzo della memoria in eccesso possa ridurre notevolmente il numero di connessioni simultanee.
