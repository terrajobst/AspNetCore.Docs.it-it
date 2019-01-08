---
title: Imporre HTTPS in ASP.NET Core
author: rick-anderson
description: Informazioni su come richiedere HTTPS/TLS in un'app web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 0c3add9c8860a47932cda3a8b07c83dc774bf1f1
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098974"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="025bd-103">Imporre HTTPS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="025bd-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="025bd-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="025bd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="025bd-105">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="025bd-105">This document shows how to:</span></span>

* <span data-ttu-id="025bd-106">La richiesta di HTTPS per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="025bd-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="025bd-107">Reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="025bd-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="025bd-108">Nessuna API può impedire a un client di inviare i dati sensibili alla prima richiesta.</span><span class="sxs-lookup"><span data-stu-id="025bd-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="025bd-109">Effettuare **non** utilizzare [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) nelle API Web che ricevono le informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="025bd-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="025bd-110">`RequireHttpsAttribute` Usa i codici di stato HTTP per reindirizzare i browser da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="025bd-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="025bd-111">I client dell'API non possono comprendere o rispettare effettua il reindirizzamento da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="025bd-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="025bd-112">Tali client possono inviare le informazioni su HTTP.</span><span class="sxs-lookup"><span data-stu-id="025bd-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="025bd-113">Le API Web devono essere:</span><span class="sxs-lookup"><span data-stu-id="025bd-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="025bd-114">È in ascolto su HTTP.</span><span class="sxs-lookup"><span data-stu-id="025bd-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="025bd-115">Chiudere la connessione con il codice di stato 400 (Bad Request) e non rispondere alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="025bd-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="025bd-116">La richiesta di HTTPS</span><span class="sxs-lookup"><span data-stu-id="025bd-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="025bd-117">È consigliabile che la produzione di ASP.NET Core web App chiamata:</span><span class="sxs-lookup"><span data-stu-id="025bd-117">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="025bd-118">Middleware di reindirizzamento HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) per reindirizzare le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="025bd-118">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="025bd-119">Middleware HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) per inviare le intestazioni HTTP Strict Transport Security protocollo (HSTS) ai client.</span><span class="sxs-lookup"><span data-stu-id="025bd-119">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="025bd-120">Le app distribuite in una configurazione proxy inverso consentono il proxy per la gestione di sicurezza della connessione (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="025bd-120">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="025bd-121">Se il proxy gestisce anche il reindirizzamento HTTPS, non è necessario usare il Middleware di reindirizzamento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="025bd-121">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="025bd-122">Se il server proxy gestisce inoltre la scrittura delle intestazioni HSTS (ad esempio, [HSTS native supportano in IIS 10.0 (1709) o versione successiva](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), Middleware HSTS non è richiesto dall'app.</span><span class="sxs-lookup"><span data-stu-id="025bd-122">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="025bd-123">Per altre informazioni, vedere [Opt-out di HTTPS/HSTS al momento della creazione progetto](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="025bd-123">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="025bd-124">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="025bd-124">UseHttpsRedirection</span></span>

<span data-ttu-id="025bd-125">Il codice seguente chiama `UseHttpsRedirection` nella `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="025bd-125">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="025bd-126">Il codice evidenziato sopra:</span><span class="sxs-lookup"><span data-stu-id="025bd-126">The preceding highlighted code:</span></span>

* <span data-ttu-id="025bd-127">Usa il valore predefinito [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="025bd-127">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="025bd-128">Usa il valore predefinito [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) a meno che non viene sottoposto a override per il `ASPNETCORE_HTTPS_PORT` variabile di ambiente oppure [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="025bd-128">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="025bd-129">È consigliabile utilizzare reindirizzamenti temporanei anziché di reindirizzamenti permanenti.</span><span class="sxs-lookup"><span data-stu-id="025bd-129">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="025bd-130">La memorizzazione nella cache di collegamento, rendendo instabile il comportamento può causare in ambienti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="025bd-130">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="025bd-131">Se si preferisce inviare un codice di stato come reindirizzamento permanente quando l'app si trova in un ambiente non di sviluppo, vedere la [configurare i reindirizzamenti permanenti nell'ambiente di produzione](#configure-permanent-redirects-in-production) sezione.</span><span class="sxs-lookup"><span data-stu-id="025bd-131">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="025bd-132">È consigliabile usare [HSTS](#http-strict-transport-security-protocol-hsts) per segnalare ai client solo di proteggere risorse richieste devono essere inviate all'app (solo nell'ambiente di produzione).</span><span class="sxs-lookup"><span data-stu-id="025bd-132">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="025bd-133">Configurazione della porta</span><span class="sxs-lookup"><span data-stu-id="025bd-133">Port configuration</span></span>

<span data-ttu-id="025bd-134">Una porta deve essere disponibile per il middleware per reindirizzare una richiesta non sicura a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="025bd-134">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="025bd-135">Se non è disponibile alcuna porta:</span><span class="sxs-lookup"><span data-stu-id="025bd-135">If no port is available:</span></span>

* <span data-ttu-id="025bd-136">Non si verifica il reindirizzamento a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="025bd-136">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="025bd-137">Il middleware registra l'avviso "Impossibile determinare la porta https per reindirizzamento".</span><span class="sxs-lookup"><span data-stu-id="025bd-137">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="025bd-138">Specificare la porta HTTPS usando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="025bd-138">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="025bd-139">Impostare [HttpsRedirectionOptions.HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="025bd-139">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="025bd-140">Impostare il `ASPNETCORE_HTTPS_PORT` variabile di ambiente oppure [impostazione di configurazione Host Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="025bd-140">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="025bd-141">**Chiave**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="025bd-141">**Key**: `https_port`</span></span>  
  <span data-ttu-id="025bd-142">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="025bd-142">**Type**: *string*</span></span>  
  <span data-ttu-id="025bd-143">**Default**: Non è impostato un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="025bd-143">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="025bd-144">**Impostare usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="025bd-144">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="025bd-145">**Variabile di ambiente**: `<PREFIX_>HTTPS_PORT` (È il prefisso `ASPNETCORE_` quando si usano i [Host Web](xref:fundamentals/host/web-host).)</span><span class="sxs-lookup"><span data-stu-id="025bd-145">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="025bd-146">Quando si configura un <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span><span class="sxs-lookup"><span data-stu-id="025bd-146">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="025bd-147">Indicare una porta con lo schema sicuro usando la `ASPNETCORE_URLS` variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="025bd-147">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="025bd-148">La variabile di ambiente consente di configurare il server.</span><span class="sxs-lookup"><span data-stu-id="025bd-148">The environment variable configures the server.</span></span> <span data-ttu-id="025bd-149">Il middleware indirettamente individua la porta HTTPS tramite <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="025bd-149">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="025bd-150">Questo approccio non funziona nelle distribuzioni di proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="025bd-150">This approach doesn't work in reverse proxy deployments.</span></span>
* <span data-ttu-id="025bd-151">In fase di sviluppo, impostare un URL HTTPS *launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="025bd-151">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="025bd-152">Abilitare HTTPS quando si usa IIS Express.</span><span class="sxs-lookup"><span data-stu-id="025bd-152">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="025bd-153">Configurare un endpoint di URL HTTPS per una distribuzione edge rivolte al pubblico di [Kestrel](xref:fundamentals/servers/kestrel) server oppure [HTTP. sys](xref:fundamentals/servers/httpsys) server.</span><span class="sxs-lookup"><span data-stu-id="025bd-153">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="025bd-154">Solo **una sola porta HTTPS** viene usata dall'app.</span><span class="sxs-lookup"><span data-stu-id="025bd-154">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="025bd-155">Il middleware consente di individuare la porta tramite <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="025bd-155">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="025bd-156">Quando un'app viene eseguita in una configurazione proxy inverso, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> non è disponibile.</span><span class="sxs-lookup"><span data-stu-id="025bd-156">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="025bd-157">Impostare la porta usando uno degli altri approcci descritti in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="025bd-157">Set the port using one of the other approaches described in this section.</span></span>

<span data-ttu-id="025bd-158">Se Kestrel o HTTP. sys è utilizzato come un server perimetrale rivolte al pubblico, Kestrel o HTTP. sys deve essere configurato per l'ascolto su entrambi:</span><span class="sxs-lookup"><span data-stu-id="025bd-158">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="025bd-159">La porta sicura in cui viene reindirizzato il client (in genere, 443 in 5001 in fase di sviluppo e produzione).</span><span class="sxs-lookup"><span data-stu-id="025bd-159">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="025bd-160">La porta non protetta (in genere, 80 nell'ambiente di produzione) e 5000 in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="025bd-160">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="025bd-161">La porta non protetta deve essere accessibile dal client affinché l'app per ricevere una richiesta non sicura e reindirizzare il client alla porta sicura.</span><span class="sxs-lookup"><span data-stu-id="025bd-161">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="025bd-162">Per altre informazioni, vedere [configurazione dell'endpoint di Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) o <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="025bd-162">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="025bd-163">Scenari di distribuzione</span><span class="sxs-lookup"><span data-stu-id="025bd-163">Deployment scenarios</span></span>

<span data-ttu-id="025bd-164">Eventuali firewall tra il client e il server deve inoltre essere le porte di comunicazione aperto per il traffico.</span><span class="sxs-lookup"><span data-stu-id="025bd-164">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="025bd-165">Se le richieste vengono inoltrate in una configurazione proxy inverso, usare [Middleware delle intestazioni inoltrate](xref:host-and-deploy/proxy-load-balancer) prima di chiamare il Middleware di reindirizzamento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="025bd-165">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="025bd-166">Gli aggiornamenti di Middleware delle intestazioni inoltrate le `Request.Scheme`, usando il `X-Forwarded-Proto` intestazione.</span><span class="sxs-lookup"><span data-stu-id="025bd-166">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="025bd-167">I permessi di middleware redirect URI e altri criteri di sicurezza per funzionare correttamente.</span><span class="sxs-lookup"><span data-stu-id="025bd-167">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="025bd-168">Quando non viene usato Middleware delle intestazioni inoltrate, l'app back-end potrebbe non ricevere lo schema appropriato e finire in un ciclo di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="025bd-168">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="025bd-169">Un messaggio di errore comuni di utente finale è che si sono verificati troppi reindirizzamenti.</span><span class="sxs-lookup"><span data-stu-id="025bd-169">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="025bd-170">Durante la distribuzione in servizio App di Azure, seguire le indicazioni fornite in [esercitazione: Associare un certificato SSL personalizzato esistente ad App Web di Azure](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="025bd-170">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="025bd-171">Opzioni</span><span class="sxs-lookup"><span data-stu-id="025bd-171">Options</span></span>

<span data-ttu-id="025bd-172">L'evidenziato seguente codice chiama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) per configurare le opzioni del middleware:</span><span class="sxs-lookup"><span data-stu-id="025bd-172">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="025bd-173">La chiamata `AddHttpsRedirection` è necessario modificare i valori di solo `HttpsPort` o `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="025bd-173">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="025bd-174">Il codice evidenziato sopra:</span><span class="sxs-lookup"><span data-stu-id="025bd-174">The preceding highlighted code:</span></span>

* <span data-ttu-id="025bd-175">Set [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) a <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, ovvero il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="025bd-175">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="025bd-176">Usare i campi del <xref:Microsoft.AspNetCore.Http.StatusCodes> classe per le assegnazioni ai `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="025bd-176">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="025bd-177">Nastavuje port HTTPS in 5001.</span><span class="sxs-lookup"><span data-stu-id="025bd-177">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="025bd-178">Il valore predefinito è 443.</span><span class="sxs-lookup"><span data-stu-id="025bd-178">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="025bd-179">Configurare i reindirizzamenti permanenti nell'ambiente di produzione</span><span class="sxs-lookup"><span data-stu-id="025bd-179">Configure permanent redirects in production</span></span>

<span data-ttu-id="025bd-180">Valore predefinito è il middleware per l'invio di un [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) con tutti i reindirizzamenti.</span><span class="sxs-lookup"><span data-stu-id="025bd-180">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="025bd-181">Se si preferisce inviare un codice di stato come reindirizzamento permanente quando l'app si trova in un ambiente non di sviluppo, eseguire il wrapping le opzioni di configurazione del middleware in un controllo condizionale per un ambiente non di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="025bd-181">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="025bd-182">Quando si configura un `IWebHostBuilder` nelle *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="025bd-182">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="025bd-183">Approccio alternativo Middleware reindirizzamento HTTPS</span><span class="sxs-lookup"><span data-stu-id="025bd-183">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="025bd-184">Un'alternativa all'uso del Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) consiste nell'usare Middleware riscrittura URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="025bd-184">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="025bd-185">`AddRedirectToHttps` può anche impostare il codice di stato e la porta quando viene eseguito il reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="025bd-185">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="025bd-186">Per altre informazioni, vedere [Middleware riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="025bd-186">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="025bd-187">Quando si reindirizzano a HTTPS senza la necessità per le regole di reindirizzamento supplementare, è consigliabile usare il Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="025bd-187">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="025bd-188">Il [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) viene utilizzato per richiedere HTTPS.</span><span class="sxs-lookup"><span data-stu-id="025bd-188">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="025bd-189">`[RequireHttpsAttribute]` può decorare i controller o metodi o possono essere applicati a livello globale.</span><span class="sxs-lookup"><span data-stu-id="025bd-189">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="025bd-190">Per applicare l'attributo a livello globale, aggiungere il codice seguente al `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="025bd-190">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="025bd-191">Il codice evidenziato sopra è necessario utilizzano tutte le richieste `HTTPS`; di conseguenza, le richieste HTTP vengono ignorate.</span><span class="sxs-lookup"><span data-stu-id="025bd-191">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="025bd-192">Il codice evidenziato seguente reindirizza tutte le richieste HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="025bd-192">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="025bd-193">Per altre informazioni, vedere [Middleware riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="025bd-193">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="025bd-194">Il middleware consente anche all'app per impostare il codice di stato o il codice di stato e la porta quando viene eseguito il reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="025bd-194">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="025bd-195">Richiesta di HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una protezione ottimale.</span><span class="sxs-lookup"><span data-stu-id="025bd-195">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="025bd-196">Applicando la `[RequireHttps]` attributo a tutte le pagine Razor/controller non è considerata sicura come richiesta di HTTPS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="025bd-196">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="025bd-197">Non è possibile garantire la `[RequireHttps]` attributo viene applicato quando vengono aggiunti nuovi controller e le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="025bd-197">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="025bd-198">Protocollo HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="025bd-198">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="025bd-199">Per ogni [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) è un miglioramento della sicurezza con consenso esplicito specificato da un'app web tramite l'uso di un'intestazione di risposta.</span><span class="sxs-lookup"><span data-stu-id="025bd-199">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="025bd-200">Quando un [browser che supporti HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) riceve questa intestazione:</span><span class="sxs-lookup"><span data-stu-id="025bd-200">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="025bd-201">Il browser memorizza la configurazione per il dominio che impedisce l'invio di tutte le comunicazioni su HTTP.</span><span class="sxs-lookup"><span data-stu-id="025bd-201">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="025bd-202">Il browser forza tutte le comunicazioni tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="025bd-202">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="025bd-203">Il browser impedisce all'utente di utilizzo di certificati non attendibili o non validi.</span><span class="sxs-lookup"><span data-stu-id="025bd-203">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="025bd-204">Il browser Disabilita prompt che permettono all'utente di considerare temporaneamente attendibile questo certificato.</span><span class="sxs-lookup"><span data-stu-id="025bd-204">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="025bd-205">Poiché HSTS viene applicato per il client lo presenta alcune limitazioni:</span><span class="sxs-lookup"><span data-stu-id="025bd-205">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="025bd-206">Il client deve supportare HSTS.</span><span class="sxs-lookup"><span data-stu-id="025bd-206">The client must support HSTS.</span></span>
* <span data-ttu-id="025bd-207">HSTS richiede almeno una richiesta HTTPS con esito positivo per stabilire i criteri HSTS.</span><span class="sxs-lookup"><span data-stu-id="025bd-207">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="025bd-208">L'applicazione deve controllare ogni richiesta HTTP e reindirizzare o rifiutare la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="025bd-208">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="025bd-209">ASP.NET Core 2.1 o versioni successive implementa HSTS con il `UseHsts` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="025bd-209">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="025bd-210">Il codice seguente chiama `UseHsts` quando l'app non è presente nel [modalità di sviluppo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="025bd-210">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="025bd-211">`UseHsts` non è consigliato in fase di sviluppo perché le impostazioni HSTS sono altamente memorizzabile nella cache dal browser.</span><span class="sxs-lookup"><span data-stu-id="025bd-211">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="025bd-212">Per impostazione predefinita, `UseHsts` esclude l'indirizzo di loopback locale.</span><span class="sxs-lookup"><span data-stu-id="025bd-212">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="025bd-213">Per gli ambienti di produzione, l'implementazione di HTTPS per la prima volta, impostare iniziale [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) su un valore ridotto usando uno del <xref:System.TimeSpan> metodi.</span><span class="sxs-lookup"><span data-stu-id="025bd-213">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="025bd-214">Impostare il valore delle ore a non più di un singolo giorno nel caso in cui sia necessario ripristinare l'infrastruttura HTTPS a HTTP.</span><span class="sxs-lookup"><span data-stu-id="025bd-214">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="025bd-215">Dopo che si è ritenuto affidabile la sostenibilità della configurazione di HTTPS, aumentare il valore di max-age HSTS; un valore di uso comune è un anno.</span><span class="sxs-lookup"><span data-stu-id="025bd-215">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="025bd-216">Il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="025bd-216">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="025bd-217">Imposta il parametro precaricamento dell'intestazione Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="025bd-217">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="025bd-218">Precaricamento non fa parte del [specifica RFC HSTS](https://tools.ietf.org/html/rfc6797), ma è supportato dal web browser per precaricare siti HSTS nella nuova installazione.</span><span class="sxs-lookup"><span data-stu-id="025bd-218">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="025bd-219">Per altre informazioni, vedere [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="025bd-219">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="025bd-220">Abilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), cui viene applicato il criterio HSTS per ospitare sottodomini.</span><span class="sxs-lookup"><span data-stu-id="025bd-220">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="025bd-221">Imposta in modo esplicito il parametro di validità massima dell'intestazione Strict-Transport-Security per 60 giorni.</span><span class="sxs-lookup"><span data-stu-id="025bd-221">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="025bd-222">Se non impostato, il valore predefinito è 30 giorni.</span><span class="sxs-lookup"><span data-stu-id="025bd-222">If not set, defaults to 30 days.</span></span> <span data-ttu-id="025bd-223">Vedere le [direttiva max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="025bd-223">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="025bd-224">Aggiunge `example.com` all'elenco degli host da escludere.</span><span class="sxs-lookup"><span data-stu-id="025bd-224">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="025bd-225">`UseHsts` esclude gli host di loopback seguenti:</span><span class="sxs-lookup"><span data-stu-id="025bd-225">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="025bd-226">`localhost`: L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="025bd-226">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="025bd-227">`127.0.0.1`: L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="025bd-227">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="025bd-228">`[::1]`: L'indirizzo di loopback IPv6.</span><span class="sxs-lookup"><span data-stu-id="025bd-228">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="025bd-229">Rifiutare esplicitamente di HTTPS/HSTS sulla creazione di progetti</span><span class="sxs-lookup"><span data-stu-id="025bd-229">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="025bd-230">In alcuni scenari di servizio back-end in cui la protezione della connessione viene gestita nei dispositivi perimetrali pubbliche della rete, la configurazione di sicurezza della connessione in ogni nodo non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="025bd-230">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="025bd-231">Generata dai modelli in Visual Studio o da app Web di [dotnet nuove](/dotnet/core/tools/dotnet-new) comando enable [il reindirizzamento HTTPS](#require-https) e [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="025bd-231">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="025bd-232">Per le distribuzioni che non richiedono questi scenari, è possibile rifiutare esplicitamente di HTTPS/HSTS quando l'app viene creata dal modello.</span><span class="sxs-lookup"><span data-stu-id="025bd-232">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="025bd-233">Per rifiutare esplicitamente di HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="025bd-233">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="025bd-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="025bd-234">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="025bd-235">Deselezionare i **Configura per HTTPS** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="025bd-235">Uncheck the **Configure for HTTPS** check box.</span></span>

![Nuova applicazione Web ASP.NET Core finestra di dialogo che mostra la configura per HTTPS casella di controllo deselezionata.](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="025bd-237">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="025bd-237">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="025bd-238">Usare l'opzione `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="025bd-238">Use the `--no-https` option.</span></span> <span data-ttu-id="025bd-239">Esempio:</span><span class="sxs-lookup"><span data-stu-id="025bd-239">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="025bd-240">Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS in Windows e macOS</span><span class="sxs-lookup"><span data-stu-id="025bd-240">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="025bd-241">.NET core SDK include un certificato di sviluppo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="025bd-241">.NET Core SDK includes a HTTPS development certificate.</span></span> <span data-ttu-id="025bd-242">Il certificato viene installato come parte dell'esperienza della prima esecuzione.</span><span class="sxs-lookup"><span data-stu-id="025bd-242">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="025bd-243">Ad esempio, `dotnet --info` genera un output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="025bd-243">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="025bd-244">Installazione di .NET Core SDK consente di installare il certificato di sviluppo di ASP.NET Core HTTPS nell'archivio certificati utente locale.</span><span class="sxs-lookup"><span data-stu-id="025bd-244">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="025bd-245">Il certificato è stato installato, ma non è attendibile.</span><span class="sxs-lookup"><span data-stu-id="025bd-245">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="025bd-246">Per considerare attendibile il certificato di eseguire l'operazione una tantum per l'esecuzione di dotnet `dev-certs` strumento:</span><span class="sxs-lookup"><span data-stu-id="025bd-246">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="025bd-247">Il comando riportato di seguito vengono fornite informazioni sul `dev-certs` strumento:</span><span class="sxs-lookup"><span data-stu-id="025bd-247">The following command provides help on the `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="025bd-248">Come configurare un certificato dello sviluppatore per Docker</span><span class="sxs-lookup"><span data-stu-id="025bd-248">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="025bd-249">Visualizzare [questo problema su GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="025bd-249">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="025bd-250">Informazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="025bd-250">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="025bd-251">Host ASP.NET Core in Linux con Apache: Configurazione di HTTPS</span><span class="sxs-lookup"><span data-stu-id="025bd-251">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="025bd-252">Host ASP.NET Core in Linux con Nginx: Configurazione di HTTPS</span><span class="sxs-lookup"><span data-stu-id="025bd-252">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="025bd-253">Come configurare SSL in IIS</span><span class="sxs-lookup"><span data-stu-id="025bd-253">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="025bd-254">Supporto browser di OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="025bd-254">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
