---
title: Imporre HTTPS in ASP.NET Core
author: rick-anderson
description: Illustra come richiedere HTTPS/TLS in un ASP.NET Core web app.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 6e16191b1a4627e683fd2281e5556b2a6e84c082
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523144"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="1a634-103">Imporre HTTPS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a634-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="1a634-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1a634-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1a634-105">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="1a634-105">This document shows how to:</span></span>

* <span data-ttu-id="1a634-106">La richiesta di HTTPS per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="1a634-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="1a634-107">Reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1a634-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="1a634-108">Nessuna API può impedire a un client di inviare i dati sensibili alla prima richiesta.</span><span class="sxs-lookup"><span data-stu-id="1a634-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="1a634-109">Effettuare **non** utilizzare [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) nelle API Web che ricevono le informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="1a634-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="1a634-110">`RequireHttpsAttribute` Usa i codici di stato HTTP per reindirizzare i browser da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1a634-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="1a634-111">I client dell'API non possono comprendere o rispettare effettua il reindirizzamento da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1a634-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="1a634-112">Tali client possono inviare le informazioni su HTTP.</span><span class="sxs-lookup"><span data-stu-id="1a634-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="1a634-113">Le API Web devono essere:</span><span class="sxs-lookup"><span data-stu-id="1a634-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="1a634-114">È in ascolto su HTTP.</span><span class="sxs-lookup"><span data-stu-id="1a634-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="1a634-115">Chiudere la connessione con il codice di stato 400 (Bad Request) e non rispondere alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="1a634-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="1a634-116">La richiesta di HTTPS</span><span class="sxs-lookup"><span data-stu-id="1a634-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1a634-117">Si consiglia di tutta la produzione di ASP.NET Core web App chiamata:</span><span class="sxs-lookup"><span data-stu-id="1a634-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="1a634-118">Il Middleware di reindirizzamento HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) per reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1a634-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="1a634-119">[UseHsts](#hsts), protocollo HTTP Strict Transport Security (HSTS).</span><span class="sxs-lookup"><span data-stu-id="1a634-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="1a634-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="1a634-120">UseHttpsRedirection</span></span>

<span data-ttu-id="1a634-121">Il codice seguente chiama `UseHttpsRedirection` nella `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="1a634-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="1a634-122">Il codice evidenziato sopra:</span><span class="sxs-lookup"><span data-stu-id="1a634-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="1a634-123">Usa il valore predefinito [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="1a634-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span>
* <span data-ttu-id="1a634-124">Usa il valore predefinito [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) a meno che non viene sottoposto a override per il `ASPNETCORE_HTTPS_PORT` variabile di ambiente oppure [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="1a634-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

> [!WARNING] 
><span data-ttu-id="1a634-125">Una porta deve essere disponibile per il middleware per reindirizzare a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1a634-125">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="1a634-126">Se non è disponibile alcuna porta, non viene eseguito il reindirizzamento a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1a634-126">If no port is available, redirection to HTTPS does not occur.</span></span> <span data-ttu-id="1a634-127">È possibile specificare la porta HTTPS da uno qualsiasi dell'impostazione seguente:</span><span class="sxs-lookup"><span data-stu-id="1a634-127">The HTTPS port can be specified by any of the following setting:</span></span>
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* <span data-ttu-id="1a634-128">Il `ASPNETCORE_HTTPS_PORT` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1a634-128">The `ASPNETCORE_HTTPS_PORT` environment variable.</span></span> 
>* <span data-ttu-id="1a634-129">In fase di sviluppo, un url HTTPS nel *launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="1a634-129">In development, an HTTPS url in *launchsettings.json*.</span></span> 
>* <span data-ttu-id="1a634-130">Un url HTTPS configurato direttamente in Kestrel o HttpSys.</span><span class="sxs-lookup"><span data-stu-id="1a634-130">An HTTPS url configured directly on Kestrel or HttpSys.</span></span> 

<span data-ttu-id="1a634-131">L'evidenziato seguente codice chiama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) per configurare le opzioni del middleware:</span><span class="sxs-lookup"><span data-stu-id="1a634-131">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="1a634-132">La chiamata `AddHttpsRedirection` è necessario modificare i valori di solo ` HttpsPort` o ` RedirectStatusCode`;</span><span class="sxs-lookup"><span data-stu-id="1a634-132">Calling `AddHttpsRedirection` is only necessary to change the values of ` HttpsPort` or ` RedirectStatusCode`;</span></span>

<span data-ttu-id="1a634-133">Il codice evidenziato sopra:</span><span class="sxs-lookup"><span data-stu-id="1a634-133">The preceding highlighted code:</span></span>

* <span data-ttu-id="1a634-134">Set [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) a `Status307TemporaryRedirect`, ovvero il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="1a634-134">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span>
* <span data-ttu-id="1a634-135">Nastavuje port HTTPS in 5001.</span><span class="sxs-lookup"><span data-stu-id="1a634-135">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="1a634-136">Il valore predefinito è 443.</span><span class="sxs-lookup"><span data-stu-id="1a634-136">The default value is 443.</span></span>

<span data-ttu-id="1a634-137">I meccanismi seguenti impostate automaticamente la porta:</span><span class="sxs-lookup"><span data-stu-id="1a634-137">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="1a634-138">Il middleware può individuare le porte tramite [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando vengono applicate le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1a634-138">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

   * <span data-ttu-id="1a634-139">Kestrel o HTTP. sys è usate direttamente con gli endpoint HTTPS (si applica anche all'esecuzione dell'app con il debugger di Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="1a634-139">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
   * <span data-ttu-id="1a634-140">Solo **una sola porta HTTPS** viene usata dall'app.</span><span class="sxs-lookup"><span data-stu-id="1a634-140">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="1a634-141">Viene usato Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="1a634-141">Visual Studio is used:</span></span>
   * <span data-ttu-id="1a634-142">IIS Express è abilitato per HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1a634-142">IIS Express has HTTPS enabled.</span></span>
   * <span data-ttu-id="1a634-143">*launchsettings. JSON* imposta il `sslPort` per IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1a634-143">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="1a634-144">Quando un'app viene eseguita dietro un proxy inverso (ad esempio, IIS, IIS Express), `IServerAddressesFeature` non è disponibile.</span><span class="sxs-lookup"><span data-stu-id="1a634-144">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="1a634-145">La porta deve essere configurata manualmente.</span><span class="sxs-lookup"><span data-stu-id="1a634-145">The port must be manually configured.</span></span> <span data-ttu-id="1a634-146">Quando la porta non è impostata, le richieste non vengono reindirizzate.</span><span class="sxs-lookup"><span data-stu-id="1a634-146">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="1a634-147">La porta può essere configurata impostando il [impostazione di configurazione Host Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="1a634-147">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="1a634-148">**Chiave**: https_port</span><span class="sxs-lookup"><span data-stu-id="1a634-148">**Key**: https_port</span></span>  
<span data-ttu-id="1a634-149">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="1a634-149">**Type**: *string*</span></span>  
<span data-ttu-id="1a634-150">**Predefinito**: non è impostato un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="1a634-150">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="1a634-151">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1a634-151">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1a634-152">**Variabile di ambiente**: `<PREFIX_>HTTPS_PORT` (il prefisso è `ASPNETCORE_` quando si usa l'Host Web.)</span><span class="sxs-lookup"><span data-stu-id="1a634-152">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="1a634-153">La porta può essere configurata indirettamente impostando l'URL con il `ASPNETCORE_URLS` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1a634-153">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="1a634-154">La variabile di ambiente consente di configurare il server e quindi il middleware indirettamente consente di individuare la porta HTTPS tramite `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="1a634-154">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="1a634-155">Se non è impostata alcuna porta:</span><span class="sxs-lookup"><span data-stu-id="1a634-155">If no port is set:</span></span>

* <span data-ttu-id="1a634-156">Le richieste non vengono reindirizzate.</span><span class="sxs-lookup"><span data-stu-id="1a634-156">Requests aren't redirected.</span></span>
* <span data-ttu-id="1a634-157">Il middleware registra l'avviso "Impossibile determinare la porta https per reindirizzamento".</span><span class="sxs-lookup"><span data-stu-id="1a634-157">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="1a634-158">Un'alternativa all'uso del Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) consiste nell'usare Middleware riscrittura URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="1a634-158">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="1a634-159">`AddRedirectToHttps` può anche impostare il codice di stato e la porta quando viene eseguito il reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="1a634-159">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="1a634-160">Per altre informazioni, vedere [Middleware riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="1a634-160">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="1a634-161">Quando si reindirizzano a HTTPS senza la necessità per le regole di reindirizzamento supplementare, è consigliabile usare il Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="1a634-161">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="1a634-162">Il [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) viene utilizzato per richiedere HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1a634-162">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="1a634-163">`[RequireHttpsAttribute]` può decorare i controller o metodi o possono essere applicati a livello globale.</span><span class="sxs-lookup"><span data-stu-id="1a634-163">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="1a634-164">Per applicare l'attributo a livello globale, aggiungere il codice seguente al `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="1a634-164">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="1a634-165">Il codice evidenziato sopra è necessario utilizzano tutte le richieste `HTTPS`; di conseguenza, le richieste HTTP vengono ignorate.</span><span class="sxs-lookup"><span data-stu-id="1a634-165">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="1a634-166">Il codice evidenziato seguente reindirizza tutte le richieste HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="1a634-166">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="1a634-167">Per altre informazioni, vedere [Middleware riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="1a634-167">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="1a634-168">Il middleware consente anche all'app per impostare il codice di stato o il codice di stato e la porta quando viene eseguito il reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="1a634-168">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="1a634-169">Richiesta di HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una protezione ottimale.</span><span class="sxs-lookup"><span data-stu-id="1a634-169">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="1a634-170">Applicando la `[RequireHttps]` attributo a tutte le pagine Razor/controller non è considerata sicura come richiesta di HTTPS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="1a634-170">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="1a634-171">Non è possibile garantire la `[RequireHttps]` attributo viene applicato quando vengono aggiunti nuovi controller e le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="1a634-171">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="1a634-172">Protocollo HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="1a634-172">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="1a634-173">Per ogni [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) è un miglioramento della sicurezza con consenso esplicito specificato da un'app web tramite l'uso di un'intestazione di risposta.</span><span class="sxs-lookup"><span data-stu-id="1a634-173">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="1a634-174">Quando un [browser che supporti HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) riceve questa intestazione:</span><span class="sxs-lookup"><span data-stu-id="1a634-174">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="1a634-175">Il browser memorizza la configurazione per il dominio che impedisce l'invio di tutte le comunicazioni su HTTP.</span><span class="sxs-lookup"><span data-stu-id="1a634-175">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="1a634-176">Il browser forza tutte le comunicazioni tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1a634-176">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="1a634-177">Il browser impedisce all'utente di utilizzo di certificati non attendibili o non validi.</span><span class="sxs-lookup"><span data-stu-id="1a634-177">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="1a634-178">Il browser Disabilita prompt che permettono all'utente di considerare temporaneamente attendibile questo certificato.</span><span class="sxs-lookup"><span data-stu-id="1a634-178">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="1a634-179">Poiché HSTS viene applicato per il client lo presenta alcune limitazioni:</span><span class="sxs-lookup"><span data-stu-id="1a634-179">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="1a634-180">Il client deve supportare HSTS.</span><span class="sxs-lookup"><span data-stu-id="1a634-180">The client must support HSTS.</span></span>
* <span data-ttu-id="1a634-181">HSTS richiede almeno una richiesta HTTPS con esito positivo per stabilire i criteri HSTS.</span><span class="sxs-lookup"><span data-stu-id="1a634-181">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="1a634-182">L'applicazione deve controllare ogni richiesta HTTP e reindirizzare o rifiutare la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="1a634-182">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="1a634-183">ASP.NET Core 2.1 o versioni successive implementa HSTS con il `UseHsts` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="1a634-183">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="1a634-184">Il codice seguente chiama `UseHsts` quando l'app non è presente nel [modalità di sviluppo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="1a634-184">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="1a634-185">`UseHsts` non è consigliato in fase di sviluppo perché le impostazioni HSTS sono altamente memorizzabile nella cache dal browser.</span><span class="sxs-lookup"><span data-stu-id="1a634-185">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="1a634-186">Per impostazione predefinita, `UseHsts` esclude l'indirizzo di loopback locale.</span><span class="sxs-lookup"><span data-stu-id="1a634-186">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="1a634-187">Per gli ambienti di produzione l'implementazione di HTTPS per la prima volta, impostare il valore HSTS iniziale su un valore ridotto.</span><span class="sxs-lookup"><span data-stu-id="1a634-187">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="1a634-188">Impostare il valore delle ore a non più di un singolo giorno nel caso in cui sia necessario ripristinare l'infrastruttura HTTPS a HTTP.</span><span class="sxs-lookup"><span data-stu-id="1a634-188">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="1a634-189">Dopo che si è ritenuto affidabile la sostenibilità della configurazione di HTTPS, aumentare il valore di max-age HSTS; un valore di uso comune è un anno.</span><span class="sxs-lookup"><span data-stu-id="1a634-189">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="1a634-190">Il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1a634-190">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="1a634-191">Imposta il parametro precaricamento dell'intestazione Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="1a634-191">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="1a634-192">Precaricamento non fa parte del [specifica RFC HSTS](https://tools.ietf.org/html/rfc6797), ma è supportato dal web browser per precaricare siti HSTS nella nuova installazione.</span><span class="sxs-lookup"><span data-stu-id="1a634-192">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="1a634-193">Per altre informazioni, vedere [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="1a634-193">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="1a634-194">Abilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), cui viene applicato il criterio HSTS per ospitare sottodomini.</span><span class="sxs-lookup"><span data-stu-id="1a634-194">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="1a634-195">Imposta in modo esplicito il parametro di validità massima dell'intestazione Strict-Transport-Security per 60 giorni.</span><span class="sxs-lookup"><span data-stu-id="1a634-195">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="1a634-196">Se non impostato, il valore predefinito è 30 giorni.</span><span class="sxs-lookup"><span data-stu-id="1a634-196">If not set, defaults to 30 days.</span></span> <span data-ttu-id="1a634-197">Vedere le [direttiva max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="1a634-197">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="1a634-198">Aggiunge `example.com` all'elenco degli host da escludere.</span><span class="sxs-lookup"><span data-stu-id="1a634-198">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="1a634-199">`UseHsts` esclude gli host di loopback seguenti:</span><span class="sxs-lookup"><span data-stu-id="1a634-199">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="1a634-200">`localhost` : L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="1a634-200">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="1a634-201">`127.0.0.1` : L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="1a634-201">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="1a634-202">`[::1]` : L'indirizzo di loopback IPv6.</span><span class="sxs-lookup"><span data-stu-id="1a634-202">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="1a634-203">Nell'esempio precedente viene illustrato come aggiungere altri host.</span><span class="sxs-lookup"><span data-stu-id="1a634-203">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="1a634-204">Rifiutare esplicitamente di HTTPS nella creazione del progetto</span><span class="sxs-lookup"><span data-stu-id="1a634-204">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="1a634-205">I modelli di applicazione ASP.NET Core web 2.1 o versioni successive (da Visual Studio o la riga di comando di dotnet) abilitano [il reindirizzamento HTTPS](#require) e [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="1a634-205">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="1a634-206">Per le distribuzioni che non richiedono HTTPS, è possibile rifiutare esplicitamente di HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1a634-206">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="1a634-207">Ad esempio, alcuni servizi di back-end in cui HTTPS viene gestito esternamente sulla rete perimetrale, tramite HTTPS in ogni nodo non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="1a634-207">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node isn't needed.</span></span>

<span data-ttu-id="1a634-208">Per rifiutare esplicitamente di HTTPS:</span><span class="sxs-lookup"><span data-stu-id="1a634-208">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a634-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a634-209">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="1a634-210">Deselezionare i **Configura per HTTPS** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="1a634-210">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramma dell'entità](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a634-212">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="1a634-212">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="1a634-213">Usare l'opzione `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="1a634-213">Use the `--no-https` option.</span></span> <span data-ttu-id="1a634-214">Esempio:</span><span class="sxs-lookup"><span data-stu-id="1a634-214">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="1a634-215">Come configurare un certificato dello sviluppatore per Docker</span><span class="sxs-lookup"><span data-stu-id="1a634-215">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="1a634-216">Visualizzare [questo problema su GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="1a634-216">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="1a634-217">Informazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1a634-217">Additional information</span></span>

* [<span data-ttu-id="1a634-218">Supporto browser di OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="1a634-218">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
