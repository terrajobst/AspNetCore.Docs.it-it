---
title: Considerazioni sulla sicurezza in ASP.NET Core SignalR
author: bradygaster
description: Informazioni su come usare l'autenticazione e l'autorizzazione in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/security
ms.openlocfilehash: 4b27d9abb36938ed8161ff0d3535204e3fa68765
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294709"
---
# <a name="security-considerations-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="2ffbf-103">Considerazioni sulla sicurezza in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="2ffbf-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="2ffbf-104">Di [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="2ffbf-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="2ffbf-105">Questo articolo fornisce informazioni sulla sicurezza SignalR.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="2ffbf-106">Condivisione di risorse tra le origini</span><span class="sxs-lookup"><span data-stu-id="2ffbf-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="2ffbf-107">È possibile usare la [condivisione di risorse tra le origini (CORS)](https://www.w3.org/TR/cors/) per consentire le connessioni SignalR tra le origini nel browser.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="2ffbf-108">Se il codice JavaScript è ospitato in un dominio diverso dall'app SignalR, è necessario abilitare il [middleware CORS](xref:security/cors) per consentire a JavaScript di connettersi all'app SignalR.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="2ffbf-109">Consenti richieste tra le origini solo da domini che consideri attendibili o controlli.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="2ffbf-110">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2ffbf-110">For example:</span></span>

* <span data-ttu-id="2ffbf-111">Il sito è ospitato in `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="2ffbf-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="2ffbf-112">L'app SignalR è ospitata in `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="2ffbf-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="2ffbf-113">CORS deve essere configurato nell'app SignalR per consentire solo il `www.example.com`di origine.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="2ffbf-114">Per altre informazioni sulla configurazione di CORS, vedere [abilitare le richieste tra le origini (CORS)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="2ffbf-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> SignalR<span data-ttu-id="2ffbf-115"> **richiede** i seguenti criteri CORS:</span><span class="sxs-lookup"><span data-stu-id="2ffbf-115"> **requires** the following CORS policies:</span></span>

* <span data-ttu-id="2ffbf-116">Consente le origini previste specifiche.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-116">Allow the specific expected origins.</span></span> <span data-ttu-id="2ffbf-117">Consentire qualsiasi origine è possibile, ma **non** è sicura o consigliata.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="2ffbf-118">I metodi HTTP `GET` e `POST` devono essere consentiti.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="2ffbf-119">Le credenziali devono essere consentite affinché le sessioni permanenti basate su cookie funzionino correttamente.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-119">Credentials must be allowed in order for cookie-based sticky sessions to work correctly.</span></span> <span data-ttu-id="2ffbf-120">Devono essere abilitati anche quando non viene utilizzata l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-120">They must be enabled even when authentication isn't used.</span></span>

<!--
::: moniker range=">= aspnetcore-5.0"  // Moniker here just to make sure this doesn't get missed in the 5.0 version update.
However, in 5.0 we have provided an option in the TypeScript client to not use credentials.
The not to use credentials option should only be used when you know 100% that credentials like Cookies are not needed in your app (cookies are used by azure app service when using multiple servers)

For more info, see https://github.com/aspnet/AspNetCore.Docs/issues/16003
.-->

<span data-ttu-id="2ffbf-121">Ad esempio, il criterio CORS seguente consente a un client SignalR browser ospitato su `https://example.com` di accedere all'app SignalR ospitata in `https://signalr.example.com`:</span><span class="sxs-lookup"><span data-stu-id="2ffbf-121">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder =>
    {
        builder.WithOrigins("https://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chatHub");
    });

    // ... other middleware ...
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

::: moniker-end

> [!NOTE]
> SignalR<span data-ttu-id="2ffbf-122"> non è compatibile con la funzionalità CORS incorporata nel servizio app Azure.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-122"> is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="2ffbf-123">Restrizione origine WebSocket</span><span class="sxs-lookup"><span data-stu-id="2ffbf-123">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2ffbf-124">La protezione fornita da CORS non si applica agli oggetti WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-124">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="2ffbf-125">Per la restrizione Origin su WebSocket, leggere la [restrizione Origin di WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).</span><span class="sxs-lookup"><span data-stu-id="2ffbf-125">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2ffbf-126">La protezione fornita da CORS non si applica agli oggetti WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-126">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="2ffbf-127">I browser **non**:</span><span class="sxs-lookup"><span data-stu-id="2ffbf-127">Browsers do **not**:</span></span>

* <span data-ttu-id="2ffbf-128">Eseguono richieste CORS preventive.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-128">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="2ffbf-129">Rispettano le restrizioni specificate nelle intestazioni `Access-Control` quando eseguono richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-129">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="2ffbf-130">I browser, tuttavia, inviano l'intestazione `Origin` quando rilasciano richieste WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-130">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="2ffbf-131">Le applicazioni devono essere configurate per la convalida di queste intestazioni per assicurarsi che siano consentiti solo WebSocket provenienti dalle origini previste.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-131">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="2ffbf-132">In ASP.NET Core 2,1 e versioni successive, è possibile ottenere la convalida delle intestazioni usando un middleware personalizzato inserito **prima `UseSignalR`e il middleware di autenticazione** in `Configure`:</span><span class="sxs-lookup"><span data-stu-id="2ffbf-132">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="2ffbf-133">L'intestazione `Origin` viene controllata dal client e, come l'intestazione `Referer`, può essere falsificata.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-133">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="2ffbf-134">Queste intestazioni **non** devono essere usate come meccanismo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-134">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="connectionid"></a><span data-ttu-id="2ffbf-135">ConnectionId</span><span class="sxs-lookup"><span data-stu-id="2ffbf-135">ConnectionId</span></span>

<span data-ttu-id="2ffbf-136">L'esposizione di `ConnectionId` può causare la rappresentazione dannosa se la versione del server o del client di SignalR è ASP.NET Core 2,2 o versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-136">Exposing `ConnectionId` can lead to malicious impersonation if the SignalR server or client version is ASP.NET Core 2.2 or earlier.</span></span> <span data-ttu-id="2ffbf-137">Se il server SignalR e la versione del client sono ASP.NET Core 3,0 o versioni successive, il `ConnectionToken` anziché il `ConnectionId` deve essere mantenuto segreto.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-137">If the SignalR server and client version are ASP.NET Core 3.0 or later, the `ConnectionToken` rather than the `ConnectionId` must be kept secret.</span></span> <span data-ttu-id="2ffbf-138">Il `ConnectionToken` non viene esposto intenzionalmente in alcuna API.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-138">The `ConnectionToken` is purposely not exposed in any API.</span></span>  <span data-ttu-id="2ffbf-139">Può essere difficile assicurarsi che i client di SignalR meno recenti non si connettano al server, quindi anche se la versione del server SignalR è ASP.NET Core 3,0 o successiva, non è necessario esporre il `ConnectionId`.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-139">It can be difficult to ensure that older SignalR clients aren't connecting to the server, so even if your SignalR server version is ASP.NET Core 3.0 or later, the `ConnectionId` shouldn't be exposed.</span></span>

## <a name="access-token-logging"></a><span data-ttu-id="2ffbf-140">Registrazione del token di accesso</span><span class="sxs-lookup"><span data-stu-id="2ffbf-140">Access token logging</span></span>

<span data-ttu-id="2ffbf-141">Quando si usano WebSocket o eventi inviati dal server, il client browser invia il token di accesso nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-141">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="2ffbf-142">La ricezione del token di accesso tramite la stringa di query è generalmente sicura come l'utilizzo dell'intestazione `Authorization` standard.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-142">Receiving the access token via query string is generally secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="2ffbf-143">Usare sempre HTTPS per garantire una connessione end-to-end sicura tra il client e il server.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-143">Always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="2ffbf-144">Molti server Web registrano l'URL per ogni richiesta, inclusa la stringa di query.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-144">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="2ffbf-145">La registrazione degli URL può registrare il token di accesso.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-145">Logging the URLs may log the access token.</span></span> <span data-ttu-id="2ffbf-146">Per impostazione predefinita, ASP.NET Core registra l'URL per ogni richiesta, che include la stringa di query.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-146">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="2ffbf-147">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2ffbf-147">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="2ffbf-148">In caso di dubbi sulla registrazione di questi dati con i log del server, è possibile disabilitare completamente questa registrazione configurando il logger di `Microsoft.AspNetCore.Hosting` al livello di `Warning` o superiore (questi messaggi vengono scritti a livello di `Info`).</span><span class="sxs-lookup"><span data-stu-id="2ffbf-148">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="2ffbf-149">Per ulteriori informazioni, vedere [filtro dei log](xref:fundamentals/logging/index#log-filtering) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-149">For more information, see [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="2ffbf-150">Se si vuole comunque registrare determinate informazioni sulle richieste, è possibile [scrivere un middleware](xref:fundamentals/middleware/write) per registrare i dati necessari e filtrare il valore della stringa di query `access_token` (se presente).</span><span class="sxs-lookup"><span data-stu-id="2ffbf-150">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="2ffbf-151">Eccezioni</span><span class="sxs-lookup"><span data-stu-id="2ffbf-151">Exceptions</span></span>

<span data-ttu-id="2ffbf-152">I messaggi di eccezione vengono in genere considerati dati sensibili che non devono essere rivelati a un client.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-152">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="2ffbf-153">Per impostazione predefinita, SignalR non invia al client i dettagli di un'eccezione generata da un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-153">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="2ffbf-154">Il client riceve invece un messaggio generico che indica che si è verificato un errore.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-154">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="2ffbf-155">Il recapito dei messaggi di eccezione al client può essere sottoposto a override (ad esempio in fase di sviluppo o test) con [EnableDetailedErrors](xref:signalr/configuration#configure-server-options).</span><span class="sxs-lookup"><span data-stu-id="2ffbf-155">Exception message delivery to the client can be overridden (for example in development or test) with [EnableDetailedErrors](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="2ffbf-156">I messaggi di eccezione non devono essere esposti al client nelle app di produzione.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-156">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="2ffbf-157">Gestione del buffer</span><span class="sxs-lookup"><span data-stu-id="2ffbf-157">Buffer management</span></span>

SignalR<span data-ttu-id="2ffbf-158"> utilizza i buffer per connessione per gestire i messaggi in ingresso e in uscita.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-158"> uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="2ffbf-159">Per impostazione predefinita, SignalR limita questi buffer a 32 KB.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-159">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="2ffbf-160">Il messaggio più grande che può essere inviato da un client o da un server è 32 KB.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-160">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="2ffbf-161">La quantità massima di memoria utilizzata da una connessione per i messaggi è 32 KB.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-161">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="2ffbf-162">Se i messaggi sono sempre minori di 32 KB, è possibile ridurre il limite, che:</span><span class="sxs-lookup"><span data-stu-id="2ffbf-162">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="2ffbf-163">Impedisce a un client di inviare un messaggio di dimensioni maggiori.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-163">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="2ffbf-164">Il server non dovrà mai allocare buffer di grandi dimensioni per accettare messaggi.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-164">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="2ffbf-165">Se i messaggi sono maggiori di 32 KB, è possibile aumentare il limite.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-165">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="2ffbf-166">L'aumento di questo limite significa:</span><span class="sxs-lookup"><span data-stu-id="2ffbf-166">Increasing this limit means:</span></span>

* <span data-ttu-id="2ffbf-167">Il client può causare l'allocazione di buffer di memoria di grandi dimensioni da parte del server.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-167">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="2ffbf-168">L'allocazione dei server di buffer di grandi dimensioni può ridurre il numero di connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-168">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="2ffbf-169">Sono previsti limiti per i messaggi in ingresso e in uscita. entrambi possono essere configurati nell'oggetto [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) configurato in `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="2ffbf-169">There are limits for incoming and outgoing messages, both can be configured on the [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="2ffbf-170">`ApplicationMaxBufferSize` rappresenta il numero massimo di byte dal client che il server memorizza nel buffer.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-170">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="2ffbf-171">Se il client tenta di inviare un messaggio di dimensioni superiori a questo limite, è possibile che la connessione venga chiusa.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-171">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="2ffbf-172">`TransportMaxBufferSize` rappresenta il numero massimo di byte che il server può inviare.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-172">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="2ffbf-173">Se il server tenta di inviare un messaggio (compresi i valori restituiti dai metodi dell'hub) maggiore di questo limite, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-173">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="2ffbf-174">Impostando il limite su `0` viene disabilitato il limite.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-174">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="2ffbf-175">La rimozione del limite consente a un client di inviare un messaggio di qualsiasi dimensione.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-175">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="2ffbf-176">I client malintenzionati che inviano messaggi di grandi dimensioni possono causare l'allocazione di memoria.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-176">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="2ffbf-177">Un utilizzo eccessivo della memoria può ridurre significativamente il numero di connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="2ffbf-177">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
