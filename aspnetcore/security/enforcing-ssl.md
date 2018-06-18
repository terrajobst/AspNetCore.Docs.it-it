---
title: Applicare HTTPS in ASP.NET Core
author: rick-anderson
description: Viene illustrato come richiedere HTTPS/TLS in un Core di ASP.NET web app.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: f49a7846149385125390285e2f1332d8e40642c0
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725936"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="cc76b-103">Applicare HTTPS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc76b-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="cc76b-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cc76b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cc76b-105">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="cc76b-105">This document shows how to:</span></span>

* <span data-ttu-id="cc76b-106">Utilizzare HTTPS per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="cc76b-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="cc76b-107">Reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc76b-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="cc76b-108">Effettuare **non** utilizzare [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) sulle API Web che ricevono informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="cc76b-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="cc76b-109">`RequireHttpsAttribute` Usa i codici di stato HTTP per il reindirizzamento dei browser da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc76b-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="cc76b-110">Client dell'API non possono comprendere o rispettare i reindirizzamenti da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc76b-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="cc76b-111">Tali client possono inviare informazioni su HTTP.</span><span class="sxs-lookup"><span data-stu-id="cc76b-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="cc76b-112">API Web dovrebbero:</span><span class="sxs-lookup"><span data-stu-id="cc76b-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="cc76b-113">È in ascolto su HTTP.</span><span class="sxs-lookup"><span data-stu-id="cc76b-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="cc76b-114">Chiudere la connessione con il codice di stato 400 (Bad Request) e non rispondere alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc76b-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="cc76b-115">Utilizzo di HTTPS</span><span class="sxs-lookup"><span data-stu-id="cc76b-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="cc76b-116">Si consiglia di tutte le app web ASP.NET Core chiamerà il Middleware di reindirizzamento HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) per reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc76b-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="cc76b-117">Il codice seguente chiama `UseHttpsRedirection` nella `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="cc76b-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="cc76b-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="cc76b-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="cc76b-119">Il codice seguente chiama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) per configurare le opzioni del middleware:</span><span class="sxs-lookup"><span data-stu-id="cc76b-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="cc76b-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="cc76b-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="cc76b-121">Il codice evidenziato precedente:</span><span class="sxs-lookup"><span data-stu-id="cc76b-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="cc76b-122">Set [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) a `Status307TemporaryRedirect`, ovvero il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="cc76b-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="cc76b-123">Le applicazioni di produzione devono chiamare [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="cc76b-123">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="cc76b-124">Imposta la porta HTTPS 5001.</span><span class="sxs-lookup"><span data-stu-id="cc76b-124">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="cc76b-125">Il valore predefinito è 443.</span><span class="sxs-lookup"><span data-stu-id="cc76b-125">The default value is 443.</span></span>

<span data-ttu-id="cc76b-126">I seguenti meccanismi di impostare automaticamente la porta:</span><span class="sxs-lookup"><span data-stu-id="cc76b-126">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="cc76b-127">Il middleware è in grado di individuare le porte tramite [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando si applicano le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="cc76b-127">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="cc76b-128">Kestrel o HTTP. sys è utilizzata direttamente con gli endpoint HTTPS (si applica anche a esecuzione l'app con debugger del codice di Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="cc76b-128">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="cc76b-129">Solo **una porta HTTPS** usato dall'app.</span><span class="sxs-lookup"><span data-stu-id="cc76b-129">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="cc76b-130">Viene utilizzato Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="cc76b-130">Visual Studio is used:</span></span>
  - <span data-ttu-id="cc76b-131">IIS Express è attivati per HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc76b-131">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="cc76b-132">*launchSettings.json* imposta il `sslPort` per IIS Express.</span><span class="sxs-lookup"><span data-stu-id="cc76b-132">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="cc76b-133">Quando si esegue un'app dietro un proxy inverso (ad esempio IIS, IIS Express), `IServerAddressesFeature` non è disponibile.</span><span class="sxs-lookup"><span data-stu-id="cc76b-133">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="cc76b-134">La porta deve essere configurata manualmente.</span><span class="sxs-lookup"><span data-stu-id="cc76b-134">The port must be manually configured.</span></span> <span data-ttu-id="cc76b-135">Quando la porta non è impostata, non sono reindirizzare le richieste.</span><span class="sxs-lookup"><span data-stu-id="cc76b-135">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="cc76b-136">La porta può essere configurata impostando il:</span><span class="sxs-lookup"><span data-stu-id="cc76b-136">The port can be configured by setting the:</span></span>

* <span data-ttu-id="cc76b-137">La variabile di ambiente `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="cc76b-137">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="cc76b-138">`http_port` chiave di configurazione host (ad esempio, tramite *hostsettings.json* o un argomento della riga di comando).</span><span class="sxs-lookup"><span data-stu-id="cc76b-138">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="cc76b-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="cc76b-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="cc76b-140">Vedere l'esempio precedente in cui viene illustrato come impostare la porta 5001.</span><span class="sxs-lookup"><span data-stu-id="cc76b-140">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="cc76b-141">La porta può essere configurata indirettamente impostando l'URL con il `ASPNETCORE_URLS` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="cc76b-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="cc76b-142">La variabile di ambiente consente di configurare il server e quindi il middleware indirettamente consente di individuare la porta HTTPS tramite `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="cc76b-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="cc76b-143">Se non è impostata alcuna porta:</span><span class="sxs-lookup"><span data-stu-id="cc76b-143">If no port is set:</span></span>

* <span data-ttu-id="cc76b-144">Non sono reindirizzare le richieste.</span><span class="sxs-lookup"><span data-stu-id="cc76b-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="cc76b-145">Il middleware registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="cc76b-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="cc76b-146">Un'alternativa all'utilizzo di HTTPS reindirizzamento Middleware (`UseHttpsRedirection`) consiste nell'utilizzare il Middleware di riscrittura URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="cc76b-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="cc76b-147">`AddRedirectToHttps` inoltre possibile impostare il codice di stato e la porta quando viene eseguito il reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="cc76b-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="cc76b-148">Per ulteriori informazioni, vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="cc76b-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="cc76b-149">Durante il reindirizzamento a HTTPS senza la necessità per le regole di reindirizzamento aggiuntivi, è consigliabile usare HTTPS reindirizzamento Middleware (`UseHttpsRedirection`) descritto in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="cc76b-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="cc76b-150">Il [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) viene utilizzato per richiedere HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc76b-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="cc76b-151">`[RequireHttpsAttribute]` può decorare controller o i metodi, oppure può essere applicata a livello globale.</span><span class="sxs-lookup"><span data-stu-id="cc76b-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="cc76b-152">Per applicare l'attributo a livello globale, aggiungere il seguente codice al `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="cc76b-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="cc76b-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="cc76b-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="cc76b-154">Il codice evidenziato precedente richiede l'utilizzano di tutte le richieste `HTTPS`; pertanto, le richieste HTTP vengono ignorate.</span><span class="sxs-lookup"><span data-stu-id="cc76b-154">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="cc76b-155">Il seguente codice evidenziato reindirizza tutte le richieste HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="cc76b-155">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="cc76b-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="cc76b-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="cc76b-157">Per ulteriori informazioni, vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="cc76b-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="cc76b-158">Il middleware consente inoltre all'app di impostare il codice di stato o il codice di stato e la porta quando viene eseguito il reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="cc76b-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="cc76b-159">Che richiedono HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una procedura consigliata.</span><span class="sxs-lookup"><span data-stu-id="cc76b-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="cc76b-160">L'applicazione di `[RequireHttps]` attributo a tutte le pagine controller/Razor non è considerata sicura come che richiedono HTTPS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="cc76b-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="cc76b-161">Non è possibile garantire il `[RequireHttps]` attributo viene applicato quando vengono aggiunti nuovi controller e le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="cc76b-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="cc76b-162">Protocollo di sicurezza trasporto Strict HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="cc76b-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="cc76b-163">Per ogni [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict trasporto sicurezza (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) è un miglioramento della sicurezza opt-in specificato da un'applicazione web tramite l'utilizzo di un'intestazione di risposta speciali.</span><span class="sxs-lookup"><span data-stu-id="cc76b-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="cc76b-164">Una volta un browser supportato riceve questa intestazione tale browser impedirà tutte le comunicazioni da inviati tramite HTTP per il dominio specificato e verrà invece inviato tutte le comunicazioni tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc76b-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="cc76b-165">Impedisce inoltre fare clic su HTTPS tramite richieste su browser.</span><span class="sxs-lookup"><span data-stu-id="cc76b-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="cc76b-166">ASP.NET Core 2.1 o versioni successive implementa HSTS con il `UseHsts` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="cc76b-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="cc76b-167">Il codice seguente chiama `UseHsts` quando l'app non si trova in [modalità di sviluppo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="cc76b-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="cc76b-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="cc76b-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="cc76b-169">`UseHsts` non è consigliato in fase di sviluppo perché l'intestazione HSTS è altamente inseribili nella cache dal browser.</span><span class="sxs-lookup"><span data-stu-id="cc76b-169">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="cc76b-170">Per impostazione predefinita, `UseHsts` esclude l'indirizzo di loopback locale.</span><span class="sxs-lookup"><span data-stu-id="cc76b-170">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="cc76b-171">Il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cc76b-171">The following code:</span></span>

<span data-ttu-id="cc76b-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="cc76b-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="cc76b-173">Imposta il parametro precaricamento dell'intestazione di sicurezza di trasporto Strict.</span><span class="sxs-lookup"><span data-stu-id="cc76b-173">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="cc76b-174">Precaricamento non è in parte il [specifica RFC HSTS](https://tools.ietf.org/html/rfc6797), ma è supportato dal web browser per precaricare siti HSTS nella nuova installazione.</span><span class="sxs-lookup"><span data-stu-id="cc76b-174">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="cc76b-175">Per altre informazioni, vedere [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="cc76b-175">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="cc76b-176">Abilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), che applica i criteri HSTS sottodomini Host.</span><span class="sxs-lookup"><span data-stu-id="cc76b-176">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="cc76b-177">Imposta in modo esplicito il parametro max-age dell'intestazione Strict-trasporto-Security per 60 giorni.</span><span class="sxs-lookup"><span data-stu-id="cc76b-177">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="cc76b-178">Se non impostato, i valori predefiniti per 30 giorni.</span><span class="sxs-lookup"><span data-stu-id="cc76b-178">If not set, defaults to 30 days.</span></span> <span data-ttu-id="cc76b-179">Vedere la [direttiva max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="cc76b-179">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="cc76b-180">Aggiunge `example.com` all'elenco degli host da escludere.</span><span class="sxs-lookup"><span data-stu-id="cc76b-180">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="cc76b-181">`UseHsts` esclude gli host di loopback seguenti:</span><span class="sxs-lookup"><span data-stu-id="cc76b-181">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="cc76b-182">`localhost` : L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="cc76b-182">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="cc76b-183">`127.0.0.1` : L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="cc76b-183">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="cc76b-184">`[::1]` : L'indirizzo di loopback IPv6.</span><span class="sxs-lookup"><span data-stu-id="cc76b-184">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="cc76b-185">Nell'esempio precedente viene illustrato come aggiungere altri host.</span><span class="sxs-lookup"><span data-stu-id="cc76b-185">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="cc76b-186">Rifiuto HTTPS della creazione del progetto</span><span class="sxs-lookup"><span data-stu-id="cc76b-186">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="cc76b-187">I modelli di applicazione web 2.1 o versione successiva di ASP.NET Core (in Visual Studio o la riga di comando dotnet) abilitare [il reindirizzamento HTTPS](#require) e [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="cc76b-187">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="cc76b-188">Per le distribuzioni che non richiedono HTTPS, è possibile rifiutare esplicitamente di HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc76b-188">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="cc76b-189">Ad esempio, alcuni servizi back-end in HTTPS viene gestita esternamente alla periferia, utilizzando il protocollo HTTPS in corrispondenza di ogni nodo non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="cc76b-189">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="cc76b-190">Per rifiutare esplicitamente di HTTPS:</span><span class="sxs-lookup"><span data-stu-id="cc76b-190">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cc76b-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc76b-191">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="cc76b-192">Deselezionare la **Configura per HTTPS** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="cc76b-192">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramma dell'entità](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cc76b-194">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc76b-194">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="cc76b-195">Usare l'opzione `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="cc76b-195">Use the `--no-https` option.</span></span> <span data-ttu-id="cc76b-196">Esempio:</span><span class="sxs-lookup"><span data-stu-id="cc76b-196">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="cc76b-198">Come configurare un certificato dello sviluppatore per Docker</span><span class="sxs-lookup"><span data-stu-id="cc76b-198">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="cc76b-199">Vedere [questo problema GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="cc76b-199">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
