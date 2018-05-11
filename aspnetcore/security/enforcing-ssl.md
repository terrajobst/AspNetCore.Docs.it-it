---
title: Applicare HTTPS in una base di ASP.NET
author: rick-anderson
description: Viene illustrato come richiedere HTTPS/TLS in un Core di ASP.NET web app.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: b324dbcd6d28c1a8505f96da333874728e2e6a18
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="03278-103">Applicare HTTPS in una base di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="03278-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="03278-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="03278-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="03278-105">Questo documento illustra come:</span><span class="sxs-lookup"><span data-stu-id="03278-105">This document shows how to:</span></span>

- <span data-ttu-id="03278-106">Utilizzare HTTPS per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="03278-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="03278-107">Reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="03278-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="03278-108">Eseguire **non** utilizzare `RequireHttpsAttribute` sulle API Web che ricevono informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="03278-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="03278-109">`RequireHttpsAttribute` Usa i codici di stato HTTP per il reindirizzamento dei browser da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="03278-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="03278-110">Client dell'API non possono comprendere o rispettare i reindirizzamenti da HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="03278-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="03278-111">Tali client possono inviare informazioni su HTTP.</span><span class="sxs-lookup"><span data-stu-id="03278-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="03278-112">API Web dovrebbero:</span><span class="sxs-lookup"><span data-stu-id="03278-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="03278-113">È in ascolto su HTTP.</span><span class="sxs-lookup"><span data-stu-id="03278-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="03278-114">Chiudere la connessione con il codice di stato 400 (Bad Request) e non rispondere alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="03278-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="03278-115">Utilizzo di HTTPS</span><span class="sxs-lookup"><span data-stu-id="03278-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="03278-116">È consigliabile tutti ASP.NET Core chiamata di App web `UseHttpsRedirection` per reindirizzare tutte le richieste HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="03278-116">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="03278-117">Se `UseHsts` viene chiamato nell'app, deve essere chiamato prima `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="03278-117">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="03278-118">Il codice seguente chiama `UseHttpsRedirection` nella `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="03278-118">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="03278-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="03278-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="03278-120">Il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="03278-120">The following code:</span></span>

<span data-ttu-id="03278-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="03278-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="03278-122">Set `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="03278-122">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="03278-123">Imposta la porta HTTPS 5001.</span><span class="sxs-lookup"><span data-stu-id="03278-123">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="03278-124">Il [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) viene utilizzato per richiedere HTTPS.</span><span class="sxs-lookup"><span data-stu-id="03278-124">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="03278-125">`[RequireHttpsAttribute]` può decorare controller o i metodi, oppure può essere applicata a livello globale.</span><span class="sxs-lookup"><span data-stu-id="03278-125">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="03278-126">Per applicare l'attributo a livello globale, aggiungere il seguente codice al `ConfigureServices` in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="03278-126">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="03278-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="03278-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="03278-128">Il codice evidenziato precedente richiede l'utilizzano di tutte le richieste `HTTPS`; pertanto, le richieste HTTP vengono ignorate.</span><span class="sxs-lookup"><span data-stu-id="03278-128">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="03278-129">Il seguente codice evidenziato reindirizza tutte le richieste HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="03278-129">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="03278-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="03278-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="03278-131">Per ulteriori informazioni, vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="03278-131">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="03278-132">Che richiedono HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una procedura consigliata.</span><span class="sxs-lookup"><span data-stu-id="03278-132">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="03278-133">L'applicazione di `[RequireHttps]` attributo a tutte le pagine controller/Razor non è considerata sicura come che richiedono HTTPS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="03278-133">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="03278-134">Non è possibile garantire il `[RequireHttps]` attributo viene applicato quando vengono aggiunti nuovi controller e le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="03278-134">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="03278-135">Protocollo di sicurezza trasporto Strict HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="03278-135">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="03278-136">Per ogni [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict trasporto sicurezza (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) è un miglioramento della sicurezza opt-in specificato da un'applicazione web tramite l'utilizzo di un'intestazione di risposta speciali.</span><span class="sxs-lookup"><span data-stu-id="03278-136">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="03278-137">Una volta un browser supportato riceve questa intestazione tale browser impedirà tutte le comunicazioni da inviati tramite HTTP per il dominio specificato e verrà invece inviato tutte le comunicazioni tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="03278-137">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="03278-138">Impedisce inoltre fare clic su HTTPS tramite richieste su browser.</span><span class="sxs-lookup"><span data-stu-id="03278-138">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="03278-139">ASP.NET Core preview1 2.1 o versioni successive implementa HSTS con il `UseHsts` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="03278-139">ASP.NET Core 2.1 preview1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="03278-140">Il codice seguente chiama `UseHsts` quando l'app non si trova in [modalità di sviluppo](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="03278-140">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="03278-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="03278-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="03278-142">`UseHsts` non è consigliare nello sviluppo perché l'intestazione HSTS è altamente memorizzabile nella cache dal browser.</span><span class="sxs-lookup"><span data-stu-id="03278-142">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="03278-143">Per impostazione predefinita, UseHsts esclude l'indirizzo di loopback locale.</span><span class="sxs-lookup"><span data-stu-id="03278-143">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="03278-144">Il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="03278-144">The following code:</span></span>

<span data-ttu-id="03278-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="03278-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="03278-146">Imposta il parametro precaricamento dell'intestazione di sicurezza di trasporto Strict.</span><span class="sxs-lookup"><span data-stu-id="03278-146">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="03278-147">Precaricamento non è in parte il [specifica RFC HSTS](https://tools.ietf.org/html/rfc6797), ma è supportato dal web browser per precaricare siti HSTS nella nuova installazione.</span><span class="sxs-lookup"><span data-stu-id="03278-147">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="03278-148">Per altre informazioni, vedere [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="03278-148">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="03278-149">Abilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), che applica i criteri HSTS sottodomini Host.</span><span class="sxs-lookup"><span data-stu-id="03278-149">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="03278-150">Imposta in modo esplicito il parametro max-age dell'intestazione Strict-trasporto-Security per 60 giorni.</span><span class="sxs-lookup"><span data-stu-id="03278-150">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="03278-151">Se non impostato, i valori predefiniti per 30 giorni.</span><span class="sxs-lookup"><span data-stu-id="03278-151">If not set, defaults to 30 days.</span></span> <span data-ttu-id="03278-152">Vedere la [direttiva max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="03278-152">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="03278-153">Aggiunge `example.com` all'elenco degli host da escludere.</span><span class="sxs-lookup"><span data-stu-id="03278-153">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="03278-154">`UseHsts` esclude gli host di loopback seguenti:</span><span class="sxs-lookup"><span data-stu-id="03278-154">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="03278-155">`localhost` : L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="03278-155">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="03278-156">`127.0.0.1` : L'indirizzo di loopback IPv4.</span><span class="sxs-lookup"><span data-stu-id="03278-156">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="03278-157">`[::1]` : L'indirizzo di loopback IPv6.</span><span class="sxs-lookup"><span data-stu-id="03278-157">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="03278-158">Nell'esempio precedente viene illustrato come aggiungere altri host.</span><span class="sxs-lookup"><span data-stu-id="03278-158">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="03278-159">Rifiuto HTTPS della creazione del progetto</span><span class="sxs-lookup"><span data-stu-id="03278-159">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="03278-160">Le ASP.NET Core 2.1 e versioni successive modelli di applicazione web (in Visual Studio o la riga di comando dotnet) abilitano [il reindirizzamento HTTPS](#require) e [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="03278-160">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="03278-161">Per le distribuzioni che non richiedono HTTPS, è possibile rifiutare esplicitamente di HTTPS.</span><span class="sxs-lookup"><span data-stu-id="03278-161">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="03278-162">Ad esempio, alcuni servizi back-end in HTTPS viene gestita esternamente alla periferia, utilizzando il protocollo HTTPS in corrispondenza di ogni nodo non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="03278-162">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="03278-163">Per rifiutare esplicitamente di HTTPS:</span><span class="sxs-lookup"><span data-stu-id="03278-163">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03278-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03278-164">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="03278-165">Deselezionare la **Configura per HTTPS** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="03278-165">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagramma dell'entità](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="03278-167">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="03278-167">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="03278-168">Usare l'opzione `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="03278-168">Use the `--no-https` option.</span></span> <span data-ttu-id="03278-169">Esempio:</span><span class="sxs-lookup"><span data-stu-id="03278-169">For example</span></span>

```cli
dotnet new razor --no-https
```

------

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="03278-170">Come configurare un certificato dello sviluppatore per Docker</span><span class="sxs-lookup"><span data-stu-id="03278-170">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="03278-171">Vedere [questo problema GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="03278-171">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
