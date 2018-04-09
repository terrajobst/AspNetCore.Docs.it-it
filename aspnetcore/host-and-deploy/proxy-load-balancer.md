---
title: Configurare ASP.NET Core per essere usati con i server proxy e bilanciamento del carico
author: guardrex
description: Informazioni sulla configurazione per le app ospitate dietro server proxy e servizi di bilanciamento del carico, che spesso oscurare importante richiedere informazioni.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="9578f-103">Configurare ASP.NET Core per essere usati con i server proxy e bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="9578f-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="9578f-104">Dal [Luke Latham](https://github.com/guardrex) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="9578f-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="9578f-105">Nella configurazione consigliata per ASP.NET Core, l'app viene ospitato mediante Apache, Nginx o modulo Core accoppiato.</span><span class="sxs-lookup"><span data-stu-id="9578f-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="9578f-106">Server proxy, servizi di bilanciamento del carico e altri dispositivi di rete spesso nascondano informazioni sulla richiesta prima di raggiungere l'app:</span><span class="sxs-lookup"><span data-stu-id="9578f-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="9578f-107">Quando le richieste HTTPS vengono elaborate tramite HTTP, lo schema originale (HTTPS) viene perso e deve essere inoltrato in un'intestazione.</span><span class="sxs-lookup"><span data-stu-id="9578f-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="9578f-108">Poiché un'app riceve una richiesta dal proxy e non la relativa origine true su Internet o rete aziendale, indirizzo IP di origine client deve essere inoltrato anche in un'intestazione.</span><span class="sxs-lookup"><span data-stu-id="9578f-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="9578f-109">Queste informazioni potrebbero essere importante nell'elaborazione delle richieste, ad esempio nel reindirizzamenti, l'autenticazione, la generazione di collegamento, la valutazione dei criteri e geoloation client.</span><span class="sxs-lookup"><span data-stu-id="9578f-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geoloation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="9578f-110">Intestazioni inoltrate</span><span class="sxs-lookup"><span data-stu-id="9578f-110">Forwarded headers</span></span>

<span data-ttu-id="9578f-111">Per convenzione, i proxy inoltrano le informazioni nelle intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="9578f-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="9578f-112">Header</span><span class="sxs-lookup"><span data-stu-id="9578f-112">Header</span></span> | <span data-ttu-id="9578f-113">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9578f-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="9578f-114">X-inoltrati-per</span><span class="sxs-lookup"><span data-stu-id="9578f-114">X-Forwarded-For</span></span> | <span data-ttu-id="9578f-115">Contiene informazioni sul client che ha avviato la richiesta e i proxy successivi in una catena di proxy.</span><span class="sxs-lookup"><span data-stu-id="9578f-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="9578f-116">Questo parametro può contenere IP indirizzi (e, facoltativamente, i numeri di porta).</span><span class="sxs-lookup"><span data-stu-id="9578f-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="9578f-117">In una catena di server proxy, il primo parametro indica il client in cui è stata innanzitutto eseguita la richiesta.</span><span class="sxs-lookup"><span data-stu-id="9578f-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="9578f-118">Gli identificatori di proxy successive seguire.</span><span class="sxs-lookup"><span data-stu-id="9578f-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="9578f-119">Il proxy ultimo della catena non è nell'elenco dei parametri.</span><span class="sxs-lookup"><span data-stu-id="9578f-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="9578f-120">Indirizzo IP del proxy ultima e, facoltativamente, un numero di porta, sono disponibili come l'indirizzo IP remoto a livello di trasporto.</span><span class="sxs-lookup"><span data-stu-id="9578f-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="9578f-121">X-Forwarded-Proto</span><span class="sxs-lookup"><span data-stu-id="9578f-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="9578f-122">Il valore dello schema di origine (HTTP/HTTPS).</span><span class="sxs-lookup"><span data-stu-id="9578f-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="9578f-123">Il valore può essere anche un elenco di schemi se la richiesta è attraversato più proxy.</span><span class="sxs-lookup"><span data-stu-id="9578f-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="9578f-124">X-Forwarded-Host</span><span class="sxs-lookup"><span data-stu-id="9578f-124">X-Forwarded-Host</span></span> | <span data-ttu-id="9578f-125">Il valore originale del campo dell'intestazione Host.</span><span class="sxs-lookup"><span data-stu-id="9578f-125">The original value of the Host header field.</span></span> <span data-ttu-id="9578f-126">I proxy in genere, non modificare l'intestazione Host.</span><span class="sxs-lookup"><span data-stu-id="9578f-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="9578f-127">Vedere [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) per informazioni su una vulnerabilità di elevazione dei privilegi che interessa i sistemi in cui il proxy non vengono convalidate o restict intestazioni Host per valori buona noti.</span><span class="sxs-lookup"><span data-stu-id="9578f-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restict Host headers to known good values.</span></span> |

<span data-ttu-id="9578f-128">Il Middleware di intestazioni inoltrati dal [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) del pacchetto, legge le intestazioni e compilati i campi associati nella [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="9578f-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span> 

<span data-ttu-id="9578f-129">Gli aggiornamenti di middleware:</span><span class="sxs-lookup"><span data-stu-id="9578f-129">The middleware updates:</span></span>

* <span data-ttu-id="9578f-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; impostate utilizzando il `X-Forwarded-For` valore dell'intestazione.</span><span class="sxs-lookup"><span data-stu-id="9578f-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="9578f-131">Impostazioni aggiuntive influire sul modo in cui il middleware imposta `RemoteIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="9578f-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="9578f-132">Per informazioni dettagliate, vedere la [opzioni del Middleware intestazioni inoltrati](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="9578f-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="9578f-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; impostate utilizzando il `X-Forwarded-Proto` valore dell'intestazione.</span><span class="sxs-lookup"><span data-stu-id="9578f-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="9578f-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; impostate utilizzando il `X-Forwarded-Host` valore dell'intestazione.</span><span class="sxs-lookup"><span data-stu-id="9578f-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="9578f-135">Si noti che non tutti i dispositivi di rete aggiungere la `X-Forwarded-For` e `X-Forwarded-Proto` intestazioni senza alcuna configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="9578f-135">Note that not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="9578f-136">Se le richieste elaborate non contengono le intestazioni quando raggiungono l'app, consultare la Guida del produttore del dispositivo.</span><span class="sxs-lookup"><span data-stu-id="9578f-136">Consult your appliance manufacturer's guidance if the proxied requests don't contain these headers when they reach the app.</span></span>

<span data-ttu-id="9578f-137">Inoltrati intestazioni Middleware [le impostazioni predefinite](#forwarded-headers-middleware-options) possono essere configurate.</span><span class="sxs-lookup"><span data-stu-id="9578f-137">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="9578f-138">Le impostazioni predefinite sono:</span><span class="sxs-lookup"><span data-stu-id="9578f-138">The default settings are:</span></span>

* <span data-ttu-id="9578f-139">Solo *un proxy* tra l'app e l'origine delle richieste.</span><span class="sxs-lookup"><span data-stu-id="9578f-139">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="9578f-140">Solo gli indirizzi di loopback configurati per i proxy noti e noti reti.</span><span class="sxs-lookup"><span data-stu-id="9578f-140">Only loopback addresses are configured for known proxies and known networks.</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="9578f-141">Modulo Core ASP.NET e IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="9578f-141">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="9578f-142">Inoltrato Middleware intestazioni è abilitata per impostazione predefinita dal Middleware di integrazione IIS quando si esegue l'app associata a IIS e il modulo di base di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9578f-142">Forwarded Headers Middleware is enabled by default by IIS Integration Middleware when the app is run behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="9578f-143">Inoltrato Middleware intestazioni viene attivato prima l'esecuzione della pipeline middleware con una configurazione con restrizioni specifica per il modulo di base di ASP.NET per motivi di trust con le intestazioni inoltrate (ad esempio [lo spoofing degli indirizzi IP](https://www.iplocation.net/ip-spoofing)).</span><span class="sxs-lookup"><span data-stu-id="9578f-143">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="9578f-144">Il middleware è configurato per inoltrare il `X-Forwarded-For` e `X-Forwarded-Proto` intestazioni ed è limitato a un proxy localhost singolo.</span><span class="sxs-lookup"><span data-stu-id="9578f-144">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="9578f-145">Se è richiesta una configurazione aggiuntiva, vedere la [opzioni del Middleware intestazioni inoltrati](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="9578f-145">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="9578f-146">Altri scenari del servizio di bilanciamento carico e il server proxy</span><span class="sxs-lookup"><span data-stu-id="9578f-146">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="9578f-147">Se non si utilizza IIS Integration Middleware, inoltrati Middleware intestazioni non è abilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9578f-147">Outside of using IIS Integration Middleware, Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="9578f-148">Middleware intestazioni inoltrato deve essere abilitato per un'app per le intestazioni di processo inoltrato con [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="9578f-148">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span></span> <span data-ttu-id="9578f-149">Dopo aver abilitato il middleware se nessun [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) specificato per il middleware, il valore predefinito [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) sono [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="9578f-149">After enabling the middleware if no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span>

<span data-ttu-id="9578f-150">Configurare il middleware con [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) per l'inoltro di `X-Forwarded-For` e `X-Forwarded-Proto` intestazioni in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9578f-150">Configure the middleware with [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9578f-151">Richiamare il [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metodo `Startup.Configure` prima di chiamare altro middleware:</span><span class="sxs-lookup"><span data-stu-id="9578f-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling other middleware:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> <span data-ttu-id="9578f-152">Se nessun [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) specificato `Startup.ConfigureServices` o direttamente al metodo di estensione con [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), il valore predefinito le intestazioni per l'inoltro sono [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="9578f-152">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified in `Startup.ConfigureServices` or directly to the extension method with [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), the default headers to forward are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> <span data-ttu-id="9578f-153">Il [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) proprietà deve essere configurata con le intestazioni per l'inoltro.</span><span class="sxs-lookup"><span data-stu-id="9578f-153">The [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) property must be configured with the headers to forward.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="9578f-154">Opzioni del Middleware intestazioni inoltrate</span><span class="sxs-lookup"><span data-stu-id="9578f-154">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="9578f-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) controllano il comportamento del Middleware di intestazioni inoltrati:</span><span class="sxs-lookup"><span data-stu-id="9578f-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) control the behavior of the Forwarded Headers Middleware:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| <span data-ttu-id="9578f-156">Opzione</span><span class="sxs-lookup"><span data-stu-id="9578f-156">Option</span></span> | <span data-ttu-id="9578f-157">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9578f-157">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="9578f-158">ForwardedForHeaderName</span><span class="sxs-lookup"><span data-stu-id="9578f-158">ForwardedForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | <span data-ttu-id="9578f-159">Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span><span class="sxs-lookup"><span data-stu-id="9578f-159">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span></span><br><br><span data-ttu-id="9578f-160">Il valore predefinito è `X-Forwarded-For`.</span><span class="sxs-lookup"><span data-stu-id="9578f-160">The default is `X-Forwarded-For`.</span></span> |
| [<span data-ttu-id="9578f-161">ForwardedHeaders</span><span class="sxs-lookup"><span data-stu-id="9578f-161">ForwardedHeaders</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | <span data-ttu-id="9578f-162">Identifica i server d'inoltro devono essere elaborati.</span><span class="sxs-lookup"><span data-stu-id="9578f-162">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="9578f-163">Vedere la [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) per l'elenco dei campi che si applicano.</span><span class="sxs-lookup"><span data-stu-id="9578f-163">See the [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) for the list of fields that apply.</span></span> <span data-ttu-id="9578f-164">I valori tipici assegnati a questa proprietà sono <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span><span class="sxs-lookup"><span data-stu-id="9578f-164">Typical values assigned to this property are <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span></span><br><br><span data-ttu-id="9578f-165">Il valore predefinito è [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="9578f-165">The default value is [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> |
| [<span data-ttu-id="9578f-166">ForwardedHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="9578f-166">ForwardedHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | <span data-ttu-id="9578f-167">Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span><span class="sxs-lookup"><span data-stu-id="9578f-167">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span></span><br><br><span data-ttu-id="9578f-168">Il valore predefinito è `X-Forwarded-Host`.</span><span class="sxs-lookup"><span data-stu-id="9578f-168">The default is `X-Forwarded-Host`.</span></span> |
| [<span data-ttu-id="9578f-169">ForwardedProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="9578f-169">ForwardedProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | <span data-ttu-id="9578f-170">Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="9578f-170">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span></span><br><br><span data-ttu-id="9578f-171">Il valore predefinito è `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="9578f-171">The default is `X-Forwarded-Proto`.</span></span> |
| [<span data-ttu-id="9578f-172">ForwardLimit</span><span class="sxs-lookup"><span data-stu-id="9578f-172">ForwardLimit</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | <span data-ttu-id="9578f-173">Limita il numero di voci nelle intestazioni che vengono elaborate.</span><span class="sxs-lookup"><span data-stu-id="9578f-173">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="9578f-174">Impostato su `null` per disabilitare il limite, ma questo deve essere eseguita solo se `KnownProxies` o `KnownNetworks` configurati.</span><span class="sxs-lookup"><span data-stu-id="9578f-174">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span><br><br><span data-ttu-id="9578f-175">Il valore predefinito è 1.</span><span class="sxs-lookup"><span data-stu-id="9578f-175">The default is 1.</span></span> |
| [<span data-ttu-id="9578f-176">KnownNetworks</span><span class="sxs-lookup"><span data-stu-id="9578f-176">KnownNetworks</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | <span data-ttu-id="9578f-177">Intervalli dei proxy noti per accettare le intestazioni inoltrate di indirizzi.</span><span class="sxs-lookup"><span data-stu-id="9578f-177">Address ranges of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="9578f-178">Specificare gli intervalli IP usando la notazione Classless Interdomain Routing (CIDR).</span><span class="sxs-lookup"><span data-stu-id="9578f-178">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="9578f-179">Il valore predefinito è un [IList](/dotnet/api/system.collections.generic.ilist-1)\<[rete IP](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> contenente un'unica voce per `IPAddress.Loopback`.</span><span class="sxs-lookup"><span data-stu-id="9578f-179">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> containing a single entry for `IPAddress.Loopback`.</span></span> |
| [<span data-ttu-id="9578f-180">KnownProxies</span><span class="sxs-lookup"><span data-stu-id="9578f-180">KnownProxies</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | <span data-ttu-id="9578f-181">Indirizzi dei proxy noti per accettare le intestazioni inoltrate dal.</span><span class="sxs-lookup"><span data-stu-id="9578f-181">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="9578f-182">Utilizzare `KnownProxies` per specificare l'indirizzo IP esatto corrispondenze.</span><span class="sxs-lookup"><span data-stu-id="9578f-182">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="9578f-183">Il valore predefinito è un [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> contenente un'unica voce per `IPAddress.IPv6Loopback`.</span><span class="sxs-lookup"><span data-stu-id="9578f-183">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| [<span data-ttu-id="9578f-184">OriginalForHeaderName</span><span class="sxs-lookup"><span data-stu-id="9578f-184">OriginalForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | <span data-ttu-id="9578f-185">Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span><span class="sxs-lookup"><span data-stu-id="9578f-185">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span></span><br><br><span data-ttu-id="9578f-186">Il valore predefinito è `X-Original-For`.</span><span class="sxs-lookup"><span data-stu-id="9578f-186">The default is `X-Original-For`.</span></span> |
| [<span data-ttu-id="9578f-187">OriginalHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="9578f-187">OriginalHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | <span data-ttu-id="9578f-188">Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span><span class="sxs-lookup"><span data-stu-id="9578f-188">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span></span><br><br><span data-ttu-id="9578f-189">Il valore predefinito è `X-Original-Host`.</span><span class="sxs-lookup"><span data-stu-id="9578f-189">The default is `X-Original-Host`.</span></span> |
| [<span data-ttu-id="9578f-190">OriginalProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="9578f-190">OriginalProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | <span data-ttu-id="9578f-191">Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="9578f-191">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span></span><br><br><span data-ttu-id="9578f-192">Il valore predefinito è `X-Original-Proto`.</span><span class="sxs-lookup"><span data-stu-id="9578f-192">The default is `X-Original-Proto`.</span></span> |
| [<span data-ttu-id="9578f-193">RequireHeaderSymmetry</span><span class="sxs-lookup"><span data-stu-id="9578f-193">RequireHeaderSymmetry</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | <span data-ttu-id="9578f-194">Richiedere il numero di valori di intestazione alla sincronizzazione tra il [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) in fase di elaborazione.</span><span class="sxs-lookup"><span data-stu-id="9578f-194">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) being processed.</span></span><br><br><span data-ttu-id="9578f-195">Il valore predefinito in ASP.NET Core 1.x è `true`.</span><span class="sxs-lookup"><span data-stu-id="9578f-195">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="9578f-196">Il valore predefinito di ASP.NET Core 2.0 o versione successiva è `false`.</span><span class="sxs-lookup"><span data-stu-id="9578f-196">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="9578f-197">Scenari e casi d'uso</span><span class="sxs-lookup"><span data-stu-id="9578f-197">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="9578f-198">Quando non è possibile aggiungere inoltrato intestazioni e tutte le richieste sono sicure</span><span class="sxs-lookup"><span data-stu-id="9578f-198">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="9578f-199">In alcuni casi, potrebbe non essere possibile aggiungere intestazioni inoltrate alle richieste elaborate all'app.</span><span class="sxs-lookup"><span data-stu-id="9578f-199">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="9578f-200">Se il proxy consiste nell'imporre che tutte le richieste esterne pubbliche siano HTTPS, lo schema può essere impostato manualmente `Startup.Configure` prima di utilizzare qualsiasi tipo di middleware:</span><span class="sxs-lookup"><span data-stu-id="9578f-200">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="9578f-201">Questo codice può essere disabilitato tramite una variabile di ambiente o altre impostazioni di configurazione in un ambiente di sviluppo o gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="9578f-201">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="9578f-202">Gestire con percorso di basa e proxy che modificano il percorso della richiesta</span><span class="sxs-lookup"><span data-stu-id="9578f-202">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="9578f-203">Alcuni proxy passare il percorso intatto, ma con un'app di percorso di base che deve essere rimossi in modo che il routing funziona correttamente.</span><span class="sxs-lookup"><span data-stu-id="9578f-203">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="9578f-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware suddivide il percorso nella [HttpRequest. Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) e il percorso di base in app [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span><span class="sxs-lookup"><span data-stu-id="9578f-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware splits the path into [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) and the app base path into [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span></span>

<span data-ttu-id="9578f-205">Se `/foo` è il percorso dell'app di base passato come un percorso di proxy `/foo/api/1`, i set di middleware `Request.PathBase` a `/foo` e `Request.Path` a `/api/1` con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9578f-205">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="9578f-206">Il percorso e il percorso di basa originale vengono riapplicate quando viene chiamato nuovamente il middleware in ordine inverso.</span><span class="sxs-lookup"><span data-stu-id="9578f-206">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="9578f-207">Per ulteriori informazioni sull'elaborazione degli ordini middleware, vedere [Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="9578f-207">For more information on middleware order processing, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<span data-ttu-id="9578f-208">Se il proxy Elimina il percorso (ad esempio, l'inoltro `/foo/api/1` al `/api/1`), correzione reindirizza e collega impostando la richiesta [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) proprietà:</span><span class="sxs-lookup"><span data-stu-id="9578f-208">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="9578f-209">Se il proxy aggiunge i dati del percorso, eliminare parte del percorso di correggere i reindirizzamenti e i collegamenti utilizzando [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) e l'assegnazione per il [percorso](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) proprietà:</span><span class="sxs-lookup"><span data-stu-id="9578f-209">If the proxy is adding path data, discard part of the path to fix redirects and links by using [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) and assigning to the [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) property:</span></span>

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

## <a name="troubleshoot"></a><span data-ttu-id="9578f-210">Risolvere i problemi</span><span class="sxs-lookup"><span data-stu-id="9578f-210">Troubleshoot</span></span>

<span data-ttu-id="9578f-211">Quando le intestazioni non vengono inoltrate come previsto, attivare [registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="9578f-211">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="9578f-212">Se i registri non forniscono informazioni sufficienti per risolvere il problema, enumerare le intestazioni delle richieste ricevute dal server.</span><span class="sxs-lookup"><span data-stu-id="9578f-212">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="9578f-213">Le intestazioni possono essere scritti una risposta di app utilizzando middleware inline:</span><span class="sxs-lookup"><span data-stu-id="9578f-213">The headers can be written to an app response using inline middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    }
}
```

<span data-ttu-id="9578f-214">Verificare che la X - inoltrata-\* intestazioni vengono ricevute dal server con i valori previsti.</span><span class="sxs-lookup"><span data-stu-id="9578f-214">Ensure that the X-Forwarded-\* headers are received by the server with the expected values.</span></span> <span data-ttu-id="9578f-215">Se sono presenti più valori in una determinata intestazione, tenere presente le intestazioni di processi inoltrati Middleware intestazioni in ordine inverso da destra a sinistra.</span><span class="sxs-lookup"><span data-stu-id="9578f-215">If there are multiple values in a given header, note Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span>

<span data-ttu-id="9578f-216">IP remoto originale della richiesta deve corrispondere una voce nella `KnownProxies` o `KnownNetworks` elenca prima dell'elaborazione X-inoltrati-per.</span><span class="sxs-lookup"><span data-stu-id="9578f-216">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before X-Forwarded-For is processed.</span></span> <span data-ttu-id="9578f-217">Questo riduce lo spoofing degli indirizzi di intestazione non accettando server d'inoltro da proxy non attendibili.</span><span class="sxs-lookup"><span data-stu-id="9578f-217">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9578f-218">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9578f-218">Additional resources</span></span>

* [<span data-ttu-id="9578f-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core elevazione dei privilegi</span><span class="sxs-lookup"><span data-stu-id="9578f-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability</span></span>](https://github.com/aspnet/Announcements/issues/295)
