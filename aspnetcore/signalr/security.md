---
title: Considerazioni sulla sicurezza in ASP.NET Core SignalR
author: tdykstra
description: Informazioni su come usare l'autenticazione e autorizzazione in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: 1adf762cd6de4f0cf62e31c0ec6e595a32ed56f8
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477540"
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

Ad esempio, il seguente criterio CORS consente a un client di browser SignalR ospitato in `http://example.com` per accedere all'app di SignalR ospitato su `http://signalr.example.com`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> SignalR non è compatibile con la funzionalità incorporata di CORS nel servizio App di Azure.

## <a name="websocket-origin-restriction"></a>Restrizione di origine di WebSocket

La protezione fornita dalle CORS non si applicano agli oggetti WebSocket. Browser **non**:

* Eseguire richieste preliminari CORS.
* Rispettare le restrizioni specificate in `Access-Control` intestazioni quando si effettuano le richieste WebSocket.

Tuttavia, i browser inviano il `Origin` intestazione quando si inviano richieste WebSocket. Le applicazioni devono essere configurate per convalidare le intestazioni per assicurarsi che siano consentite solo WebSockets provenienti da origini previsto.

In ASP.NET Core 2.1 e versioni successive, è possibile ottenere la convalida delle intestazioni utilizzando un middleware personalizzato inserito **prima `UseSignalR`e middleware di autenticazione** in `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> Il `Origin` intestazione è controllata dal client e, come il `Referer` intestazione, possono essere falsificati. Queste intestazioni devono **non** utilizzabile come un meccanismo di autenticazione.

## <a name="access-token-logging"></a>Registrazione di token di accesso

Quando si usa WebSocket o Server-Sent eventi, il browser client invia il token di accesso nella stringa di query. Ricevere il token di accesso tramite la stringa di query è in genere sicuro quanto lo standard `Authorization` intestazione. Tuttavia, molti server web registrare l'URL per ogni richiesta, tra cui la stringa di query. Gli URL di registrazione può accedere il token di accesso. Una procedura consigliata consiste nell'impostare le impostazioni di registrazione del server per evitare che i token di accesso di registrazione web.

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
