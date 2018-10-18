---
title: Considerazioni sulla sicurezza in ASP.NET Core SignalR
author: tdykstra
description: Informazioni su come usare l'autenticazione e autorizzazione in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: 98b5eb7be87920aacf7a941f76ff652ae7905303
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391258"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Considerazioni sulla sicurezza in ASP.NET Core SignalR

Da [Andrew Stanton-Nurse](https://twitter.com/anurse)

## <a name="overview"></a>Panoramica

SignalR fornisce una serie di meccanismi di protezione per impostazione predefinita. È importante comprendere come configurare queste protezioni.

### <a name="cross-origin-resource-sharing"></a>Cross-origin resource Sharing, condivisione

[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) può essere utilizzato per consentire le connessioni SignalR multiorigine nel browser. Se il codice JavaScript è ospitato su un nome di dominio diverso dall'app SignalR, è necessario abilitare la [middleware CORS di ASP.NET Core](xref:security/cors) per la connessione. In generale, consentire le richieste multiorigine soltanto da domini controllati dall'utente. Ad esempio, se il sito è ospitato in `http://www.example.com` e l'app di SignalR è ospitata in `http://signalr.example.com`, è necessario configurare CORS nell'app SignalR per consentire solo l'origine `www.example.com`.

Per altre informazioni sulla configurazione di CORS, vedere [la documentazione di ASP.NET Core CORS](xref:security/cors). SignalR richiede i seguenti criteri CORS per poter funzionare correttamente:

* I criteri devono consentire le origini specifiche sbagliato o, consentire qualsiasi origine (non consigliato).
* Metodi HTTP `GET` e `POST` devono essere consentiti.
* Anche quando non si usa l'autenticazione, è necessario abilitare le credenziali.

Ad esempio, il seguente criterio CORS consente a un client di browser SignalR ospitato su `http://example.com` per accedere all'app di SignalR:

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> SignalR non è compatibile con la funzionalità incorporata di CORS nel servizio App di Azure.

### <a name="websocket-origin-restriction"></a>Restrizione di origine di WebSocket

La protezione fornita dalle CORS non si applicano agli oggetti WebSocket. I browser non eseguono richieste di pre-flight CORS, né rispettano le restrizioni specificate in `Access-Control` intestazioni quando si effettuano le richieste WebSocket. Tuttavia, i browser inviano il `Origin` intestazione quando si inviano richieste WebSocket. È necessario configurare l'applicazione per convalidare le intestazioni per garantire che solo i WebSockets provenienti da origini che previsti sono consentiti.

In ASP.NET Core 2.1, ciò può essere ottenuto tramite un middleware personalizzato è possibile posizionare **sopra `UseSignalR`e qualsiasi middleware di autenticazione** nel `Configure` metodo:

```csharp
// In your Startup class, add a static field listing the allowed Origin values:
private static readonly HashSet<string> _allowedOrigins = new HashSet<string>()
{
    // Add allowed origins here. For example:
    "http://www.mysite.com",
    "http://mysite.com",
};

// In your Configure method:
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Validate Origin header on WebSocket requests to prevent unexpected cross-site WebSocket requests
    app.Use((context, next) =>
    {
        // Check for a WebSocket request.
        if(string.Equals(context.Request.Headers["Upgrade"], "websocket"))
        {
            var origin = context.Request.Headers["Origin"];

            // If there is no origin header, or if the origin header doesn't match an allowed value:
            if(string.IsNullOrEmpty(origin) && !_allowedOrigins.Contains(origin))
            {
                // The origin is not allowed, reject the request
                context.Response.StatusCode = StatusCodes.Status400BadRequest;
                return Task.CompletedTask;
            }
        }

        // The request is not a WebSocket request or is a valid Origin, so let it continue
        return next();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> Il `Origin` intestazione viene completamente controllata dal client e, come il `Referer` intestazione, possono essere falsificati. Queste intestazioni non devono mai essere utilizzate come meccanismo di autenticazione.

### <a name="access-token-logging"></a>Registrazione di token di accesso

Quando si usa WebSocket o Server-Sent eventi, il browser client invia il token di accesso nella stringa di query. Si tratta in genere sicuro quanto lo standard `Authorization` intestazione, tuttavia, l'URL per ogni richiesta di log di molti server web, tra cui la stringa di query. Ciò significa che il token di accesso possa essere inclusi nei log. È consigliabile rivedere le impostazioni di registrazione del server web per evitare che queste informazioni.

### <a name="exceptions"></a>Eccezioni

I messaggi di eccezione sono in genere considerati dati sensibili che non devono essere rivelati al client. Per impostazione predefinita, SignalR non invia i dettagli di un'eccezione generata da un metodo dell'hub al client. Al contrario, il client riceve un messaggio generico che indica che un errore. È possibile eseguire l'override di questo comportamento impostando il [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) impostazione.

### <a name="buffer-management"></a>Gestione del buffer

SignalR utilizza i buffer per ogni connessione per gestire i messaggi in ingresso e in uscita. Per impostazione predefinita, SignalR limita i buffer su 32KB. Questo significa che il messaggio più grande possibile che un client o server può inviare è 32KB. Questo significa anche la quantità massima di memoria utilizzata da una connessione per i messaggi è 32KB. Se si conosce che i messaggi vengono sempre inferiori a questo limite, è possibile ridurre questa dimensione per impedire che un client sia in grado di inviare un messaggio più grande e forzare il server di allocare memoria per accettare il documento. Analogamente, se si conoscono i messaggi di dimensioni superiori a questo limite, è possibile aumentarla. Tuttavia, tenere presente che l'aumento del limite significa che il client è in grado di impedire al server di allocare memoria aggiuntiva e può ridurre il numero di connessioni simultanee che può gestire l'app.

Sono previsti limiti separati per i messaggi in ingresso e in uscita, entrambi possono essere configurate nel [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) configurato nell'oggetto `MapHub`:

* `ApplicationMaxBufferSize` rappresenta il numero massimo di byte dal client che il buffer del server. Se il client tenta di inviare un messaggio di dimensioni superiori a questo limite, la connessione verrà chiusa.
* `TransportMaxBufferSize` rappresenta il numero massimo di byte che il server può inviare. Se il server tenta di inviare un messaggio (include i valori restituiti dai metodi dell'hub) superano questo limite, verrà generata un'eccezione.

L'impostazione del limite `0` disattiva completamente il limite. Tuttavia, questa operazione deve essere eseguita con estrema cautela. Rimozione del limite consente a un client inviare un messaggio di qualsiasi dimensione. Questo può essere utilizzato da un client dannoso per fare in modo eccessivo della memoria da allocare, in grado di ridurre notevolmente il numero di connessioni simultanee che può supportare l'app.
