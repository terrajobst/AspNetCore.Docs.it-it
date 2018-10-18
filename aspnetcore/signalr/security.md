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
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="bc9d8-103">Considerazioni sulla sicurezza in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="bc9d8-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="bc9d8-104">Da [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="bc9d8-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="bc9d8-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="bc9d8-105">Overview</span></span>

<span data-ttu-id="bc9d8-106">SignalR fornisce una serie di meccanismi di protezione per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="bc9d8-107">È importante comprendere come configurare queste protezioni.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="bc9d8-108">Cross-origin resource Sharing, condivisione</span><span class="sxs-lookup"><span data-stu-id="bc9d8-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="bc9d8-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) può essere utilizzato per consentire le connessioni SignalR multiorigine nel browser.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="bc9d8-110">Se il codice JavaScript è ospitato su un nome di dominio diverso dall'app SignalR, è necessario abilitare la [middleware CORS di ASP.NET Core](xref:security/cors) per la connessione.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="bc9d8-111">In generale, consentire le richieste multiorigine soltanto da domini controllati dall'utente.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="bc9d8-112">Ad esempio, se il sito è ospitato in `http://www.example.com` e l'app di SignalR è ospitata in `http://signalr.example.com`, è necessario configurare CORS nell'app SignalR per consentire solo l'origine `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="bc9d8-113">Per altre informazioni sulla configurazione di CORS, vedere [la documentazione di ASP.NET Core CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="bc9d8-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="bc9d8-114">SignalR richiede i seguenti criteri CORS per poter funzionare correttamente:</span><span class="sxs-lookup"><span data-stu-id="bc9d8-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="bc9d8-115">I criteri devono consentire le origini specifiche sbagliato o, consentire qualsiasi origine (non consigliato).</span><span class="sxs-lookup"><span data-stu-id="bc9d8-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="bc9d8-116">Metodi HTTP `GET` e `POST` devono essere consentiti.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="bc9d8-117">Anche quando non si usa l'autenticazione, è necessario abilitare le credenziali.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="bc9d8-118">Ad esempio, il seguente criterio CORS consente a un client di browser SignalR ospitato su `http://example.com` per accedere all'app di SignalR:</span><span class="sxs-lookup"><span data-stu-id="bc9d8-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

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
> <span data-ttu-id="bc9d8-119">SignalR non è compatibile con la funzionalità incorporata di CORS nel servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="bc9d8-120">Restrizione di origine di WebSocket</span><span class="sxs-lookup"><span data-stu-id="bc9d8-120">WebSocket Origin Restriction</span></span>

<span data-ttu-id="bc9d8-121">La protezione fornita dalle CORS non si applicano agli oggetti WebSocket.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-121">The protections provided by CORS do not apply to WebSockets.</span></span> <span data-ttu-id="bc9d8-122">I browser non eseguono richieste di pre-flight CORS, né rispettano le restrizioni specificate in `Access-Control` intestazioni quando si effettuano le richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-122">Browsers do not perform CORS pre-flight requests, nor do they respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span> <span data-ttu-id="bc9d8-123">Tuttavia, i browser inviano il `Origin` intestazione quando si inviano richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-123">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="bc9d8-124">È necessario configurare l'applicazione per convalidare le intestazioni per garantire che solo i WebSockets provenienti da origini che previsti sono consentiti.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-124">You should configure your application to validate these headers in order to ensure that only WebSockets coming from the origins you expect are allowed.</span></span>

<span data-ttu-id="bc9d8-125">In ASP.NET Core 2.1, ciò può essere ottenuto tramite un middleware personalizzato è possibile posizionare **sopra `UseSignalR`e qualsiasi middleware di autenticazione** nel `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="bc9d8-125">In ASP.NET Core 2.1, this can be achieved using a custom middleware you can place **above `UseSignalR`, and any authentication middleware** in your `Configure` method:</span></span>

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
> <span data-ttu-id="bc9d8-126">Il `Origin` intestazione viene completamente controllata dal client e, come il `Referer` intestazione, possono essere falsificati.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-126">The `Origin` header is completely controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="bc9d8-127">Queste intestazioni non devono mai essere utilizzate come meccanismo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-127">These headers should never be used as an authentication mechanism.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="bc9d8-128">Registrazione di token di accesso</span><span class="sxs-lookup"><span data-stu-id="bc9d8-128">Access token logging</span></span>

<span data-ttu-id="bc9d8-129">Quando si usa WebSocket o Server-Sent eventi, il browser client invia il token di accesso nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-129">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="bc9d8-130">Si tratta in genere sicuro quanto lo standard `Authorization` intestazione, tuttavia, l'URL per ogni richiesta di log di molti server web, tra cui la stringa di query.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-130">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="bc9d8-131">Ciò significa che il token di accesso possa essere inclusi nei log.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-131">This means the access token may be included in logs.</span></span> <span data-ttu-id="bc9d8-132">È consigliabile rivedere le impostazioni di registrazione del server web per evitare che queste informazioni.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-132">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="bc9d8-133">Eccezioni</span><span class="sxs-lookup"><span data-stu-id="bc9d8-133">Exceptions</span></span>

<span data-ttu-id="bc9d8-134">I messaggi di eccezione sono in genere considerati dati sensibili che non devono essere rivelati al client.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-134">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="bc9d8-135">Per impostazione predefinita, SignalR non invia i dettagli di un'eccezione generata da un metodo dell'hub al client.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-135">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="bc9d8-136">Al contrario, il client riceve un messaggio generico che indica che un errore.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-136">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="bc9d8-137">È possibile eseguire l'override di questo comportamento impostando il [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) impostazione.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-137">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="bc9d8-138">Gestione del buffer</span><span class="sxs-lookup"><span data-stu-id="bc9d8-138">Buffer management</span></span>

<span data-ttu-id="bc9d8-139">SignalR utilizza i buffer per ogni connessione per gestire i messaggi in ingresso e in uscita.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-139">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="bc9d8-140">Per impostazione predefinita, SignalR limita i buffer su 32KB.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-140">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="bc9d8-141">Questo significa che il messaggio più grande possibile che un client o server può inviare è 32KB.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-141">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="bc9d8-142">Questo significa anche la quantità massima di memoria utilizzata da una connessione per i messaggi è 32KB.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-142">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="bc9d8-143">Se si conosce che i messaggi vengono sempre inferiori a questo limite, è possibile ridurre questa dimensione per impedire che un client sia in grado di inviare un messaggio più grande e forzare il server di allocare memoria per accettare il documento.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-143">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="bc9d8-144">Analogamente, se si conoscono i messaggi di dimensioni superiori a questo limite, è possibile aumentarla.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-144">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="bc9d8-145">Tuttavia, tenere presente che l'aumento del limite significa che il client è in grado di impedire al server di allocare memoria aggiuntiva e può ridurre il numero di connessioni simultanee che può gestire l'app.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-145">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="bc9d8-146">Sono previsti limiti separati per i messaggi in ingresso e in uscita, entrambi possono essere configurate nel [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) configurato nell'oggetto `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="bc9d8-146">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="bc9d8-147">`ApplicationMaxBufferSize` rappresenta il numero massimo di byte dal client che il buffer del server.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-147">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="bc9d8-148">Se il client tenta di inviare un messaggio di dimensioni superiori a questo limite, la connessione verrà chiusa.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-148">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="bc9d8-149">`TransportMaxBufferSize` rappresenta il numero massimo di byte che il server può inviare.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-149">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="bc9d8-150">Se il server tenta di inviare un messaggio (include i valori restituiti dai metodi dell'hub) superano questo limite, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-150">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="bc9d8-151">L'impostazione del limite `0` disattiva completamente il limite.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-151">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="bc9d8-152">Tuttavia, questa operazione deve essere eseguita con estrema cautela.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-152">However, this should be done with extreme caution.</span></span> <span data-ttu-id="bc9d8-153">Rimozione del limite consente a un client inviare un messaggio di qualsiasi dimensione.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-153">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="bc9d8-154">Questo può essere utilizzato da un client dannoso per fare in modo eccessivo della memoria da allocare, in grado di ridurre notevolmente il numero di connessioni simultanee che può supportare l'app.</span><span class="sxs-lookup"><span data-stu-id="bc9d8-154">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
