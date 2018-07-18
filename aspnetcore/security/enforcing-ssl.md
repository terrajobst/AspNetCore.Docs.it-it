---
title: Imporre HTTPS in ASP.NET Core
author: rick-anderson
description: Illustra come richiedere HTTPS/TLS in un ASP.NET Core web app.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 331c17de33b5c13221385ffb4282bc16bde32289
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095717"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="9443c-103">Imporre HTTPS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9443c-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="9443c-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9443c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9443c-105">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="9443c-105">This document shows how to:</span></span>

* <span data-ttu-id="9443c-106">La richiesta di HTTPS per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="9443c-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="9443c-107">Reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9443c-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="9443c-108">Effettuare **non** utilizzare [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) nelle API Web che ricevono le informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="9443c-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="9443c-109">`RequireHttpsAttribute` Usa i codici di stato HTTP per reindirizzare i browser da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9443c-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="9443c-110">I client dell'API non possono comprendere o rispettare effettua il reindirizzamento da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9443c-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="9443c-111">Tali client possono inviare le informazioni su HTTP.</span><span class="sxs-lookup"><span data-stu-id="9443c-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="9443c-112">Le API Web devono essere:</span><span class="sxs-lookup"><span data-stu-id="9443c-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="9443c-113">È in ascolto su HTTP.</span><span class="sxs-lookup"><span data-stu-id="9443c-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="9443c-114">Chiudere la connessione con il codice di stato 400 (Bad Request) e non rispondere alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="9443c-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="9443c-115">La richiesta di HTTPS</span><span class="sxs-lookup"><span data-stu-id="9443c-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9443c-116">Si consiglia di tutte le app web ASP.NET Core chiamerà il Middleware di reindirizzamento HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) per reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9443c-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="9443c-117">Il codice seguente chiama `UseHttpsRedirection` nella `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="9443c-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="9443c-118">Il codice seguente chiama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) per configurare le opzioni del middleware:</span><span class="sxs-lookup"><span data-stu-id="9443c-118">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="9443c-119">Il codice evidenziato sopra:</span><span class="sxs-lookup"><span data-stu-id="9443c-119">The preceding highlighted code:</span></span>

* <span data-ttu-id="9443c-120">Set [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) a `Status307TemporaryRedirect`, ovvero il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="9443c-120">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="9443c-121">Le app di produzione devono chiamare [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="9443c-121">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="9443c-122">Nastavuje port HTTPS in 5001.</span><span class="sxs-lookup"><span data-stu-id="9443c-122">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="9443c-123">Il valore predefinito è 443.</span><span class="sxs-lookup"><span data-stu-id="9443c-123">The default value is 443.</span></span>

<span data-ttu-id="9443c-124">I meccanismi seguenti impostate automaticamente la porta:</span><span class="sxs-lookup"><span data-stu-id="9443c-124">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="9443c-125">Il middleware può individuare le porte tramite [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando vengono applicate le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9443c-125">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="9443c-126">Kestrel o HTTP. sys è usate direttamente con gli endpoint HTTPS (si applica anche all'esecuzione dell'app con il debugger di Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="9443c-126">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="9443c-127">Solo **una sola porta HTTPS** viene usata dall'app.</span><span class="sxs-lookup"><span data-stu-id="9443c-127">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="9443c-128">Viene usato Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9443c-128">Visual Studio is used:</span></span>
  - <span data-ttu-id="9443c-129">IIS Express è abilitato per HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9443c-129">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="9443c-130">*launchsettings. JSON* imposta il `sslPort` per IIS Express.</span><span class="sxs-lookup"><span data-stu-id="9443c-130">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="9443c-131">Quando un'app viene eseguita dietro un proxy inverso (ad esempio, IIS, IIS Express), `IServerAddressesFeature` non è disponibile.</span><span class="sxs-lookup"><span data-stu-id="9443c-131">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="9443c-132">La porta deve essere configurata manualmente.</span><span class="sxs-lookup"><span data-stu-id="9443c-132">The port must be manually configured.</span></span> <span data-ttu-id="9443c-133">Quando la porta non è impostata, le richieste non vengono reindirizzate.</span><span class="sxs-lookup"><span data-stu-id="9443c-133">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="9443c-134">La porta può essere configurata impostando il:</span><span class="sxs-lookup"><span data-stu-id="9443c-134">The port can be configured by setting the:</span></span>

* <span data-ttu-id="9443c-135">La variabile di ambiente `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="9443c-135">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="9443c-136">`http_port` chiave di configurazione host (ad esempio, tramite *hostsettings.json* o un argomento della riga di comando).</span><span class="sxs-lookup"><span data-stu-id="9443c-136">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="9443c-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="9443c-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="9443c-138">Vedere l'esempio precedente viene illustrato come impostare la porta in 5001.</span><span class="sxs-lookup"><span data-stu-id="9443c-138">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="9443c-139">La porta può essere configurata indirettamente impostando l'URL con il `ASPNETCORE_URLS` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="9443c-139">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="9443c-140">La variabile di ambiente consente di configurare il server e quindi il middleware indirettamente consente di individuare la porta HTTPS tramite `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="9443c-140">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="9443c-141">Se non è impostata alcuna porta:</span><span class="sxs-lookup"><span data-stu-id="9443c-141">If no port is set:</span></span>

* <span data-ttu-id="9443c-142">Le richieste non vengono reindirizzate.</span><span class="sxs-lookup"><span data-stu-id="9443c-142">Requests aren't redirected.</span></span>
* <span data-ttu-id="9443c-143">Il middleware registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="9443c-143">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="9443c-144">Un'alternativa all'uso del Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) consiste nell'usare Middleware riscrittura URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="9443c-144">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="9443c-145">`AddRedirectToHttps` può anche impostare il codice di stato e la porta quando viene eseguito il reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="9443c-145">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="9443c-146">Per altre informazioni, vedere [Middleware riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="9443c-146">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="9443c-147">Quando si reindirizzano a HTTPS senza la necessità per le regole di reindirizzamento supplementare, è consigliabile usare il Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="9443c-147">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="9443c-148">Il [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) viene utilizzato per richiedere HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9443c-148">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="9443c-149">`[RequireHttpsAttribute]` può decorare i controller o metodi o possono essere applicati a livello globale.</span><span class="sxs-lookup"><span data-stu-id="9443c-149">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="9443c-150">Per applicare l'attributo a livello globale, aggiungere il codice seguente al `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="9443c-150">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="9443c-151">Il codice evidenziato sopra è necessario utilizzano tutte le richieste `HTTPS`; di conseguenza, le richieste HTTP vengono ignorate.</span><span class="sxs-lookup"><span data-stu-id="9443c-151">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="9443c-152">Il codice evidenziato seguente reindirizza tutte le richieste HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="9443c-152">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="9443c-153">Per altre informazioni, vedere [Middleware riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="9443c-153">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="9443c-154">Il middleware consente anche all'app per impostare il codice di stato o il codice di stato e la porta quando viene eseguito il reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="9443c-154">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="9443c-155">Richiesta di HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una protezione ottimale.</span><span class="sxs-lookup"><span data-stu-id="9443c-155">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="9443c-156">Applicando la `[RequireHttps]` attributo a tutte le pagine Razor/controller non è considerata sicura come richiesta di HTTPS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="9443c-156">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="9443c-157">Non è possibile garantire la `[RequireHttps]` attributo viene applicato quando vengono aggiunti nuovi controller e le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="9443c-157">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="9443c-158">Protocollo HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="9443c-158">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="9443c-159">Per ogni [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) è un miglioramento della sicurezza con consenso esplicito specificato da un'applicazione web usando una speciale intestazione della risposta.</span><span class="sxs-lookup"><span data-stu-id="9443c-159">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="9443c-160">Una volta che un browser supportato riceve questa intestazione browser impedirà tutte le comunicazioni che vengano inviati tramite HTTP per il dominio specificato e invia tutte le comunicazioni tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9443c-160">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="9443c-161">Impedisce anche click-through richieste nei browser HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9443c-161">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="9443c-162">ASP.NET Core 2.1 o versioni successive implementa HSTS con il `UseHsts` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="9443c-162">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="9443c-163">Il codice seguente chiama `UseHsts` quando l'app non è presente nel [modalità di sviluppo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="9443c-163">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="9443c-164">`UseHsts` non è consigliato in fase di sviluppo poiché l'intestazione HSTS sia altamente memorizzabile nella cache dal browser.</span><span class="sxs-lookup"><span data-stu-id="9443c-164">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="9443c-165">Per impostazione predefinita, `UseHsts` esclude l'indirizzo di loopback locale.</span><span class="sxs-lookup"><span data-stu-id="9443c-165">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="9443c-166">Il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9443c-166">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="9443c-167">Imposta il parametro precaricamento dell'intestazione Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="9443c-167">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="9443c-168">Precaricamento non è in parte i [specifica RFC HSTS](https://tools.ietf.org/html/rfc6797), ma è supportato dal web browser per precaricare siti HSTS nella nuova installazione.</span><span class="sxs-lookup"><span data-stu-id="9443c-168">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="9443c-169">Per altre informazioni, vedere [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="9443c-169">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="9443c-170">Abilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), cui viene applicato il criterio HSTS per ospitare sottodomini.</span><span class="sxs-lookup"><span data-stu-id="9443c-170">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="9443c-171">Imposta in modo esplicito il parametro di durata massima dell'intestazione Strict-Transport-Security per 60 giorni.</span><span class="sxs-lookup"><span data-stu-id="9443c-171">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="9443c-172">Se non impostato, il valore predefinito è 30 giorni.</span><span class="sxs-lookup"><span data-stu-id="9443c-172">If not set, defaults to 30 days.</span></span> <span data-ttu-id="9443c-173">Vedere le [direttiva max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="9443c-173">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="9443c-174">Aggiunge `example.com` all'elenco degli host da escludere.</span><span class="sxs-lookup"><span data-stu-id="9443c-174">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="9443c-175">`UseHsts` esclude gli host di loopback seguenti:</span><span class="sxs-lookup"><span data-stu-id="9443c-175">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="9443c-176">`localhost` : L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="9443c-176">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="9443c-177">`127.0.0.1` : L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="9443c-177">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="9443c-178">`[::1]` : L'indirizzo di loopback IPv6.</span><span class="sxs-lookup"><span data-stu-id="9443c-178">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="9443c-179">Nell'esempio precedente viene illustrato come aggiungere altri host.</span><span class="sxs-lookup"><span data-stu-id="9443c-179">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="9443c-180">Rifiutare esplicitamente di HTTPS nella creazione del progetto</span><span class="sxs-lookup"><span data-stu-id="9443c-180">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="9443c-181">I modelli di applicazione ASP.NET Core web 2.1 o versioni successive (da Visual Studio o la riga di comando di dotnet) abilitano [il reindirizzamento HTTPS](#require) e [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="9443c-181">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="9443c-182">Per le distribuzioni che non richiedono HTTPS, è possibile rifiutare esplicitamente di HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9443c-182">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="9443c-183">Ad esempio, alcuni servizi di back-end in cui HTTPS viene gestito esternamente sulla rete perimetrale, tramite HTTPS in ogni nodo non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="9443c-183">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="9443c-184">Per rifiutare esplicitamente di HTTPS:</span><span class="sxs-lookup"><span data-stu-id="9443c-184">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9443c-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9443c-185">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="9443c-186">Deselezionare i **Configura per HTTPS** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="9443c-186">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramma dell'entità](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9443c-188">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="9443c-188">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="9443c-189">Usare l'opzione `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="9443c-189">Use the `--no-https` option.</span></span> <span data-ttu-id="9443c-190">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9443c-190">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="9443c-191">Come configurare un certificato dello sviluppatore per Docker</span><span class="sxs-lookup"><span data-stu-id="9443c-191">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="9443c-192">Visualizzare [questo problema su GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="9443c-192">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
