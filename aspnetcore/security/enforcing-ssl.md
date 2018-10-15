---
title: Imporre HTTPS in ASP.NET Core
author: rick-anderson
description: Informazioni su come richiedere HTTPS/TLS in un'app web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325601"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="ea0e7-103">Imporre HTTPS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea0e7-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="ea0e7-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ea0e7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ea0e7-105">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-105">This document shows how to:</span></span>

* <span data-ttu-id="ea0e7-106">La richiesta di HTTPS per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="ea0e7-107">Reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="ea0e7-108">Nessuna API può impedire a un client di inviare i dati sensibili alla prima richiesta.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="ea0e7-109">Effettuare **non** utilizzare [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) nelle API Web che ricevono le informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="ea0e7-110">`RequireHttpsAttribute` Usa i codici di stato HTTP per reindirizzare i browser da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="ea0e7-111">I client dell'API non possono comprendere o rispettare effettua il reindirizzamento da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="ea0e7-112">Tali client possono inviare le informazioni su HTTP.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="ea0e7-113">Le API Web devono essere:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="ea0e7-114">È in ascolto su HTTP.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="ea0e7-115">Chiudere la connessione con il codice di stato 400 (Bad Request) e non rispondere alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>

## <a name="require-https"></a><span data-ttu-id="ea0e7-116">La richiesta di HTTPS</span><span class="sxs-lookup"><span data-stu-id="ea0e7-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ea0e7-117">Si consiglia di tutta la produzione di ASP.NET Core web App chiamata:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="ea0e7-118">Il Middleware di reindirizzamento HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) per reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="ea0e7-119">[UseHsts](#hsts), protocollo HTTP Strict Transport Security (HSTS).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="ea0e7-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="ea0e7-120">UseHttpsRedirection</span></span>

<span data-ttu-id="ea0e7-121">Il codice seguente chiama `UseHttpsRedirection` nella `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="ea0e7-122">Il codice evidenziato sopra:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="ea0e7-123">Usa il valore predefinito [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="ea0e7-124">Usa il valore predefinito [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) a meno che non viene sottoposto a override per il `ASPNETCORE_HTTPS_PORT` variabile di ambiente oppure [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="ea0e7-125">È consigliabile usare reindirizzamenti temporanei anziché di reindirizzamenti permanenti, come la memorizzazione nella cache di collegamento può causare un comportamento instabile in ambienti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-125">We recommend using temporary redirects rather than permanent redirects, as link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="ea0e7-126">È consigliabile usare [HSTS](#hsts) per segnalare ai client solo di proteggere risorse richieste devono essere inviate all'app (solo nell'ambiente di produzione).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-126">We recommend using [HSTS](#hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

> [!WARNING]
> <span data-ttu-id="ea0e7-127">Una porta deve essere disponibile per il middleware per reindirizzare a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-127">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="ea0e7-128">Se non è disponibile alcuna porta, non verrà eseguito il reindirizzamento a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-128">If no port is available, redirection to HTTPS doesn't occur.</span></span> <span data-ttu-id="ea0e7-129">La porta HTTPS può essere specificata usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-129">The HTTPS port can be specified using any of the following approaches:</span></span>
>
> * <span data-ttu-id="ea0e7-130">Impostare `HttpsRedirectionOptions.HttpsPort`.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-130">Set `HttpsRedirectionOptions.HttpsPort`.</span></span>
> * <span data-ttu-id="ea0e7-131">Impostare la variabile di ambiente `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-131">Set the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
> * <span data-ttu-id="ea0e7-132">In fase di sviluppo, impostare un URL HTTPS *launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-132">In development, set an HTTPS URL in *launchsettings.json*.</span></span>
> * <span data-ttu-id="ea0e7-133">Configurare un endpoint di URL HTTPS per [Kestrel](xref:fundamentals/servers/kestrel) oppure [HTTP. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-133">Configure an HTTPS URL endpoint for [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>
>
> <span data-ttu-id="ea0e7-134">Se Kestrel o HTTP. sys è utilizzato come un server perimetrale rivolte al pubblico, Kestrel o HTTP. sys deve essere configurato per l'ascolto su entrambi:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-134">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>
>
> * <span data-ttu-id="ea0e7-135">La porta sicura in cui viene reindirizzato il client (in genere, 443 in 5001 in fase di sviluppo e produzione).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-135">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
> * <span data-ttu-id="ea0e7-136">La porta non protetta (in genere, 80 nell'ambiente di produzione) e 5000 in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-136">The insecure port (typically, 80 in production and 5000 in development).</span></span>
>
> <span data-ttu-id="ea0e7-137">La porta non protetta deve essere accessibile dal client affinché l'app per ricevere una richiesta non sicura e reindirizzarla alla porta sicura.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-137">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect it to the secure port.</span></span>
>
> <span data-ttu-id="ea0e7-138">Eventuali firewall tra il client e il server deve inoltre disporre le porte aprire per il traffico.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-138">Any firewall between the client and server must also have the ports open for traffic.</span></span>
>
> <span data-ttu-id="ea0e7-139">Per altre informazioni, vedere [configurazione dell'endpoint di Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) o <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-139">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

<span data-ttu-id="ea0e7-140">L'evidenziato seguente codice chiama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) per configurare le opzioni del middleware:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-140">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="ea0e7-141">La chiamata `AddHttpsRedirection` è necessario modificare i valori di solo `HttpsPort` o `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-141">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="ea0e7-142">Il codice evidenziato sopra:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-142">The preceding highlighted code:</span></span>

* <span data-ttu-id="ea0e7-143">Set [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) al [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), ovvero il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-143">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), which is the default value.</span></span> <span data-ttu-id="ea0e7-144">Usare i campi del [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) classe per le assegnazioni ai `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-144">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="ea0e7-145">Nastavuje port HTTPS in 5001.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-145">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="ea0e7-146">Il valore predefinito è 443.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-146">The default value is 443.</span></span>

<span data-ttu-id="ea0e7-147">I meccanismi seguenti impostate automaticamente la porta:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-147">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="ea0e7-148">Il middleware può individuare le porte tramite [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando vengono applicate le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-148">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

  * <span data-ttu-id="ea0e7-149">Kestrel o HTTP. sys è usate direttamente con gli endpoint HTTPS (si applica anche all'esecuzione dell'app con il debugger di Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-149">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  * <span data-ttu-id="ea0e7-150">Solo **una sola porta HTTPS** viene usata dall'app.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-150">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="ea0e7-151">Viene usato Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-151">Visual Studio is used:</span></span>

  * <span data-ttu-id="ea0e7-152">IIS Express è abilitato per HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-152">IIS Express has HTTPS enabled.</span></span>
  * <span data-ttu-id="ea0e7-153">*launchsettings. JSON* imposta il `sslPort` per IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-153">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="ea0e7-154">Quando un'app viene eseguita dietro un proxy inverso (ad esempio, IIS, IIS Express), `IServerAddressesFeature` non è disponibile.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-154">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="ea0e7-155">La porta deve essere configurata manualmente.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-155">The port must be manually configured.</span></span> <span data-ttu-id="ea0e7-156">Quando la porta non è impostata, le richieste non vengono reindirizzate.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-156">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="ea0e7-157">La porta può essere configurata impostando il [impostazione di configurazione Host Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="ea0e7-157">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="ea0e7-158">**Chiave**: https_port</span><span class="sxs-lookup"><span data-stu-id="ea0e7-158">**Key**: https_port</span></span>  
<span data-ttu-id="ea0e7-159">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="ea0e7-159">**Type**: *string*</span></span>  
<span data-ttu-id="ea0e7-160">**Predefinito**: non è impostato un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-160">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="ea0e7-161">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="ea0e7-161">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="ea0e7-162">**Variabile di ambiente**: `<PREFIX_>HTTPS_PORT` (il prefisso è `ASPNETCORE_` quando si usa l'Host Web.)</span><span class="sxs-lookup"><span data-stu-id="ea0e7-162">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="ea0e7-163">La porta può essere configurata indirettamente impostando l'URL con il `ASPNETCORE_URLS` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-163">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="ea0e7-164">La variabile di ambiente consente di configurare il server e quindi il middleware indirettamente consente di individuare la porta HTTPS tramite `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-164">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="ea0e7-165">Se non è impostata alcuna porta:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-165">If no port is set:</span></span>

* <span data-ttu-id="ea0e7-166">Le richieste non vengono reindirizzate.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-166">Requests aren't redirected.</span></span>
* <span data-ttu-id="ea0e7-167">Il middleware registra l'avviso "Impossibile determinare la porta https per reindirizzamento".</span><span class="sxs-lookup"><span data-stu-id="ea0e7-167">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="ea0e7-168">Un'alternativa all'uso del Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) consiste nell'usare Middleware riscrittura URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-168">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="ea0e7-169">`AddRedirectToHttps` può anche impostare il codice di stato e la porta quando viene eseguito il reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-169">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="ea0e7-170">Per altre informazioni, vedere [Middleware riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-170">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="ea0e7-171">Quando si reindirizzano a HTTPS senza la necessità per le regole di reindirizzamento supplementare, è consigliabile usare il Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-171">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="ea0e7-172">Il [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) viene utilizzato per richiedere HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-172">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="ea0e7-173">`[RequireHttpsAttribute]` può decorare i controller o metodi o possono essere applicati a livello globale.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-173">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="ea0e7-174">Per applicare l'attributo a livello globale, aggiungere il codice seguente al `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-174">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="ea0e7-175">Il codice evidenziato sopra è necessario utilizzano tutte le richieste `HTTPS`; di conseguenza, le richieste HTTP vengono ignorate.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-175">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="ea0e7-176">Il codice evidenziato seguente reindirizza tutte le richieste HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-176">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="ea0e7-177">Per altre informazioni, vedere [Middleware riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-177">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="ea0e7-178">Il middleware consente anche all'app per impostare il codice di stato o il codice di stato e la porta quando viene eseguito il reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-178">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="ea0e7-179">Richiesta di HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una protezione ottimale.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-179">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="ea0e7-180">Applicando la `[RequireHttps]` attributo a tutte le pagine Razor/controller non è considerata sicura come richiesta di HTTPS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-180">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="ea0e7-181">Non è possibile garantire la `[RequireHttps]` attributo viene applicato quando vengono aggiunti nuovi controller e le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-181">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="ea0e7-182">Protocollo HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="ea0e7-182">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="ea0e7-183">Per ogni [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) è un miglioramento della sicurezza con consenso esplicito specificato da un'app web tramite l'uso di un'intestazione di risposta.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-183">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="ea0e7-184">Quando un [browser che supporti HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) riceve questa intestazione:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-184">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="ea0e7-185">Il browser memorizza la configurazione per il dominio che impedisce l'invio di tutte le comunicazioni su HTTP.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-185">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="ea0e7-186">Il browser forza tutte le comunicazioni tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-186">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="ea0e7-187">Il browser impedisce all'utente di utilizzo di certificati non attendibili o non validi.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-187">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="ea0e7-188">Il browser Disabilita prompt che permettono all'utente di considerare temporaneamente attendibile questo certificato.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-188">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="ea0e7-189">Poiché HSTS viene applicato per il client lo presenta alcune limitazioni:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-189">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="ea0e7-190">Il client deve supportare HSTS.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-190">The client must support HSTS.</span></span>
* <span data-ttu-id="ea0e7-191">HSTS richiede almeno una richiesta HTTPS con esito positivo per stabilire i criteri HSTS.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-191">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="ea0e7-192">L'applicazione deve controllare ogni richiesta HTTP e reindirizzare o rifiutare la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-192">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="ea0e7-193">ASP.NET Core 2.1 o versioni successive implementa HSTS con il `UseHsts` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-193">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="ea0e7-194">Il codice seguente chiama `UseHsts` quando l'app non è presente nel [modalità di sviluppo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="ea0e7-194">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="ea0e7-195">`UseHsts` non è consigliato in fase di sviluppo perché le impostazioni HSTS sono altamente memorizzabile nella cache dal browser.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-195">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="ea0e7-196">Per impostazione predefinita, `UseHsts` esclude l'indirizzo di loopback locale.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-196">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="ea0e7-197">Per gli ambienti di produzione l'implementazione di HTTPS per la prima volta, impostare il valore HSTS iniziale su un valore ridotto.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-197">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="ea0e7-198">Impostare il valore delle ore a non più di un singolo giorno nel caso in cui sia necessario ripristinare l'infrastruttura HTTPS a HTTP.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-198">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="ea0e7-199">Dopo che si è ritenuto affidabile la sostenibilità della configurazione di HTTPS, aumentare il valore di max-age HSTS; un valore di uso comune è un anno.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-199">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="ea0e7-200">Il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-200">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="ea0e7-201">Imposta il parametro precaricamento dell'intestazione Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-201">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="ea0e7-202">Precaricamento non fa parte del [specifica RFC HSTS](https://tools.ietf.org/html/rfc6797), ma è supportato dal web browser per precaricare siti HSTS nella nuova installazione.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-202">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="ea0e7-203">Per altre informazioni, vedere [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-203">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="ea0e7-204">Abilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), cui viene applicato il criterio HSTS per ospitare sottodomini.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-204">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="ea0e7-205">Imposta in modo esplicito il parametro di validità massima dell'intestazione Strict-Transport-Security per 60 giorni.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-205">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="ea0e7-206">Se non impostato, il valore predefinito è 30 giorni.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-206">If not set, defaults to 30 days.</span></span> <span data-ttu-id="ea0e7-207">Vedere le [direttiva max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-207">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="ea0e7-208">Aggiunge `example.com` all'elenco degli host da escludere.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-208">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="ea0e7-209">`UseHsts` esclude gli host di loopback seguenti:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-209">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="ea0e7-210">`localhost` : L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-210">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="ea0e7-211">`127.0.0.1` : L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-211">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="ea0e7-212">`[::1]` : L'indirizzo di loopback IPv6.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-212">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="ea0e7-213">Nell'esempio precedente viene illustrato come aggiungere altri host.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-213">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="ea0e7-214">Rifiutare esplicitamente di HTTPS/HSTS sulla creazione di progetti</span><span class="sxs-lookup"><span data-stu-id="ea0e7-214">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="ea0e7-215">In alcuni scenari di servizio back-end in cui la protezione della connessione viene gestita nei dispositivi perimetrali pubbliche della rete, la configurazione di sicurezza della connessione in ogni nodo non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-215">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="ea0e7-216">Generata dai modelli in Visual Studio o da app Web di [dotnet nuove](/dotnet/core/tools/dotnet-new) comando enable [il reindirizzamento HTTPS](#require) e [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-216">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="ea0e7-217">Per le distribuzioni che non richiedono questi scenari, è possibile rifiutare esplicitamente di HTTPS/HSTS quando l'app viene creata dal modello.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-217">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="ea0e7-218">Per rifiutare esplicitamente di HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-218">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ea0e7-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea0e7-219">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="ea0e7-220">Deselezionare i **Configura per HTTPS** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-220">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramma dell'entità](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ea0e7-222">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="ea0e7-222">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="ea0e7-223">Usare l'opzione `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="ea0e7-223">Use the `--no-https` option.</span></span> <span data-ttu-id="ea0e7-224">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ea0e7-224">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="ea0e7-225">Come configurare un certificato dello sviluppatore per Docker</span><span class="sxs-lookup"><span data-stu-id="ea0e7-225">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="ea0e7-226">Visualizzare [questo problema su GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="ea0e7-226">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="ea0e7-227">Informazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ea0e7-227">Additional information</span></span>

* [<span data-ttu-id="ea0e7-228">Supporto browser di OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="ea0e7-228">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
