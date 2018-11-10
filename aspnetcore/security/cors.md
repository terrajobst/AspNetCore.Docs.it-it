---
title: Abilitare le richieste Multiorigine (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come condivisione CORS come standard per consentire o rifiutare le richieste multiorigine in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2018
uid: security/cors
ms.openlocfilehash: 8e5056b448d47d75272e9394b03ce8a58b05a0f4
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191321"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="20016-103">Abilitare le richieste Multiorigine (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="20016-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="20016-104">Dal [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="20016-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="20016-105">Protezione del browser impedisce che una pagina web inviando richieste a un dominio diverso da quello che ha gestito la pagina web.</span><span class="sxs-lookup"><span data-stu-id="20016-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="20016-106">Questa restrizione viene chiamata il *criterio della stessa origine*.</span><span class="sxs-lookup"><span data-stu-id="20016-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="20016-107">Il criterio della stessa origine impedisce a un sito dannoso di leggere i dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="20016-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="20016-108">In alcuni casi, si potrebbe voler consentire che ad altri siti effettuare richieste multiorigine per l'app.</span><span class="sxs-lookup"><span data-stu-id="20016-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="20016-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (le origini CORS) è uno standard W3C che consente a un server di ridurre i criteri di corrispondenza dell'origine.</span><span class="sxs-lookup"><span data-stu-id="20016-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="20016-110">Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre.</span><span class="sxs-lookup"><span data-stu-id="20016-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="20016-111">CORS è più sicura e più flessibile di tecniche precedenti, ad esempio [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="20016-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="20016-112">Questo argomento illustra come abilitare CORS in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="20016-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="20016-113">Origine stessa</span><span class="sxs-lookup"><span data-stu-id="20016-113">Same origin</span></span>

<span data-ttu-id="20016-114">Due URL abbia la stessa origine se dispongono di porte, host e gli schemi identici ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="20016-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="20016-115">Questi due URL abbia la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="20016-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="20016-116">Questi URL sono le entità Origin diversi rispetto i due URL precedente:</span><span class="sxs-lookup"><span data-stu-id="20016-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="20016-117">`https://example.net` &ndash; Dominio diverso</span><span class="sxs-lookup"><span data-stu-id="20016-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="20016-118">`https://www.example.com/foo.html` &ndash; Sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="20016-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="20016-119">`http://example.com/foo.html` &ndash; Schema differente</span><span class="sxs-lookup"><span data-stu-id="20016-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="20016-120">`https://example.com:9000/foo.html` &ndash; Porta diversa</span><span class="sxs-lookup"><span data-stu-id="20016-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="20016-121">Internet Explorer non considera la porta quando si confrontano le entità Origin.</span><span class="sxs-lookup"><span data-stu-id="20016-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="20016-122">Registrare i servizi CORS</span><span class="sxs-lookup"><span data-stu-id="20016-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="20016-123">Riferimento di [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto le [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="20016-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="20016-124">Riferimento di [metapacchetto Microsoft. aspnetcore](xref:fundamentals/metapackage) oppure aggiungere un riferimento al pacchetto le [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="20016-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="20016-125">Aggiungere un riferimento al pacchetto di [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="20016-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="20016-126">Chiamare <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` per aggiungere servizi CORS al contenitore del servizio dell'app:</span><span class="sxs-lookup"><span data-stu-id="20016-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="20016-127">Abilitare CORS</span><span class="sxs-lookup"><span data-stu-id="20016-127">Enable CORS</span></span>

<span data-ttu-id="20016-128">Dopo la registrazione di servizi CORS, utilizzare uno degli approcci seguenti per abilitare CORS in un'app ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="20016-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="20016-129">[Middleware CORS](#enable-cors-with-cors-middleware) &ndash; criteri CORS si applicano a livello globale per l'app tramite il middleware.</span><span class="sxs-lookup"><span data-stu-id="20016-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="20016-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; criteri applicare CORS per ogni azione o per ogni controller.</span><span class="sxs-lookup"><span data-stu-id="20016-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="20016-131">Non usare il Middleware CORS.</span><span class="sxs-lookup"><span data-stu-id="20016-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="20016-132">Abilitare CORS con il Middleware CORS</span><span class="sxs-lookup"><span data-stu-id="20016-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="20016-133">Middleware CORS gestisce le richieste multiorigine per l'app.</span><span class="sxs-lookup"><span data-stu-id="20016-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="20016-134">Per abilitare CORS Middleware nella pipeline di elaborazione della richiesta, chiamare il <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metodo di estensione in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="20016-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="20016-135">Middleware CORS devono precedere qualsiasi endpoint definito nell'app in cui si desidera supportare richieste multiorigine (ad esempio, prima della chiamata a `UseMvc` per il Middleware di pagine Razor MVC /).</span><span class="sxs-lookup"><span data-stu-id="20016-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="20016-136">Oggetto *criteri multiorigine* può essere specificato quando si aggiunge il Middleware CORS usando la <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> classe.</span><span class="sxs-lookup"><span data-stu-id="20016-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="20016-137">Esistono due approcci per la definizione di un criterio CORS:</span><span class="sxs-lookup"><span data-stu-id="20016-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="20016-138">Chiamare `UseCors` con un'espressione lambda:</span><span class="sxs-lookup"><span data-stu-id="20016-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="20016-139">L'operatore lambda accetta un <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> oggetto.</span><span class="sxs-lookup"><span data-stu-id="20016-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="20016-140">[Opzioni di configurazione](#cors-policy-options), ad esempio `WithOrigins`, sono descritti più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="20016-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="20016-141">Nell'esempio precedente, il criterio consente le richieste multiorigine dal `https://example.com` e non le entità Origin.</span><span class="sxs-lookup"><span data-stu-id="20016-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="20016-142">È necessario specificare l'URL senza una barra finale (`/`).</span><span class="sxs-lookup"><span data-stu-id="20016-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="20016-143">Se l'URL termina con `/`, il confronto restituisce `false` e non viene restituita alcuna intestazione.</span><span class="sxs-lookup"><span data-stu-id="20016-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="20016-144">`CorsPolicyBuilder` ha un'API fluent, pertanto è possibile concatenare chiamate al metodo:</span><span class="sxs-lookup"><span data-stu-id="20016-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="20016-145">Definire uno o più criteri CORS denominati e selezionare i criteri in base al nome in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="20016-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="20016-146">L'esempio seguente aggiunge un criterio CORS definite dall'utente denominato *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="20016-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="20016-147">Per selezionare i criteri, passare il nome da `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="20016-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="20016-148">Abilitare CORS in MVC</span><span class="sxs-lookup"><span data-stu-id="20016-148">Enable CORS in MVC</span></span>

<span data-ttu-id="20016-149">In alternativa, è possibile utilizzare MVC per applicare i criteri CORS specifici per ogni azione o per ogni controller.</span><span class="sxs-lookup"><span data-stu-id="20016-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="20016-150">Quando si usa MVC per abilitare CORS, vengono usati i servizi registrati CORS.</span><span class="sxs-lookup"><span data-stu-id="20016-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="20016-151">Non viene usato il Middleware CORS.</span><span class="sxs-lookup"><span data-stu-id="20016-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="20016-152">Per ogni azione</span><span class="sxs-lookup"><span data-stu-id="20016-152">Per action</span></span>

<span data-ttu-id="20016-153">Per specificare un criterio CORS per un'azione specifica, aggiungere il [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attributo all'azione.</span><span class="sxs-lookup"><span data-stu-id="20016-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="20016-154">Specificare il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="20016-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="20016-155">Per ogni controller</span><span class="sxs-lookup"><span data-stu-id="20016-155">Per controller</span></span>

<span data-ttu-id="20016-156">Per specificare i criteri CORS per un controller specifico, aggiungere il [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attributo alla classe controller.</span><span class="sxs-lookup"><span data-stu-id="20016-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="20016-157">Specificare il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="20016-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="20016-158">L'ordine di precedenza è:</span><span class="sxs-lookup"><span data-stu-id="20016-158">The precedence order is:</span></span>

1. <span data-ttu-id="20016-159">action</span><span class="sxs-lookup"><span data-stu-id="20016-159">action</span></span>
1. <span data-ttu-id="20016-160">controller</span><span class="sxs-lookup"><span data-stu-id="20016-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="20016-161">Disabilitare la condivisione CORS</span><span class="sxs-lookup"><span data-stu-id="20016-161">Disable CORS</span></span>

<span data-ttu-id="20016-162">Per disabilitare CORS per un controller o azione, usare il [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attributo:</span><span class="sxs-lookup"><span data-stu-id="20016-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="20016-163">Opzioni dei criteri CORS</span><span class="sxs-lookup"><span data-stu-id="20016-163">CORS policy options</span></span>

<span data-ttu-id="20016-164">Questa sezione descrive le varie opzioni che è possibile impostare in un criterio CORS.</span><span class="sxs-lookup"><span data-stu-id="20016-164">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="20016-165">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="20016-165">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="20016-166">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="20016-166">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="20016-167">Impostare le intestazioni di richieste consentite</span><span class="sxs-lookup"><span data-stu-id="20016-167">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="20016-168">Impostare le intestazioni di risposta esposto</span><span class="sxs-lookup"><span data-stu-id="20016-168">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="20016-169">Credenziali in richieste multiorigine</span><span class="sxs-lookup"><span data-stu-id="20016-169">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="20016-170">Impostare l'ora di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="20016-170">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="20016-171">Per alcune opzioni, potrebbe essere utile leggere il [funziona come CORS](#how-cors-works) sezione prima di tutto.</span><span class="sxs-lookup"><span data-stu-id="20016-171">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="20016-172">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="20016-172">Set the allowed origins</span></span>

<span data-ttu-id="20016-173">Il middleware CORS in ASP.NET Core MVC presenta alcuni modi per specificare origini consentite:</span><span class="sxs-lookup"><span data-stu-id="20016-173">The CORS middleware in ASP.NET Core MVC has a few ways to specify allowed origins:</span></span>

* <span data-ttu-id="20016-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Consente di specificare uno o più URL.</span><span class="sxs-lookup"><span data-stu-id="20016-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Allows specifying one or more URLs.</span></span> <span data-ttu-id="20016-175">L'URL può includere la combinazione nome host e porta senza alcuna informazione sul percorso.</span><span class="sxs-lookup"><span data-stu-id="20016-175">The URL may include the scheme, host name, and port without any path information.</span></span> <span data-ttu-id="20016-176">Ad esempio `https://example.com`.</span><span class="sxs-lookup"><span data-stu-id="20016-176">For example, `https://example.com`.</span></span> <span data-ttu-id="20016-177">È necessario specificare l'URL senza una barra finale (`/`).</span><span class="sxs-lookup"><span data-stu-id="20016-177">The URL must be specified without a trailing slash (`/`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

* <span data-ttu-id="20016-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Consente le richieste CORS da tutte le entità Origin con qualsiasi schema (`http` o `https`).</span><span class="sxs-lookup"><span data-stu-id="20016-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

<span data-ttu-id="20016-179">Si consideri con attenzione prima di consentire le richieste provenienti da qualsiasi origine.</span><span class="sxs-lookup"><span data-stu-id="20016-179">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="20016-180">Consentire le richieste provenienti da qualsiasi origine significa che *qualsiasi sito Web* possono eseguire richieste multiorigine per l'app.</span><span class="sxs-lookup"><span data-stu-id="20016-180">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="20016-181">Che specifica `AllowAnyOrigin` e `AllowCredentials` è una configurazione non protetta e può comportare la richiesta intersito falsa.</span><span class="sxs-lookup"><span data-stu-id="20016-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="20016-182">Il servizio CORS restituisce una risposta CORS non valida quando un'app è configurata con i due.</span><span class="sxs-lookup"><span data-stu-id="20016-182">The CORS service returns an invalid CORS response when an app is configured with the two.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="20016-183">Che specifica `AllowAnyOrigin` e `AllowCredentials` è una configurazione non protetta e può comportare la richiesta intersito falsa.</span><span class="sxs-lookup"><span data-stu-id="20016-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="20016-184">Provare a specificare l'elenco esatto di origini, se il client deve per l'autorizzazione per accedere alle risorse di server.</span><span class="sxs-lookup"><span data-stu-id="20016-184">Consider specifying an exact list of origins if your client needs to authorize to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="20016-185">Questa impostazione influisce [preparare le richieste e l'intestazione Access-Control-Allow-Origin](#preflight-requests) (descritto più avanti in questo argomento).</span><span class="sxs-lookup"><span data-stu-id="20016-185">This setting affects [preflight requests and the Access-Control-Allow-Origin header](#preflight-requests) (described later in this topic).</span></span>

* <span data-ttu-id="20016-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> -Consente le richieste CORS da qualsiasi sottodominio di un dominio specifico.</span><span class="sxs-lookup"><span data-stu-id="20016-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> - Allows CORS requests from any sub-domain of a given domain.</span></span> <span data-ttu-id="20016-187">Lo schema non può essere un carattere jolly.</span><span class="sxs-lookup"><span data-stu-id="20016-187">The scheme cannot be a wildcard.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=98-104&highlight=4)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="20016-188">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="20016-188">Set the allowed HTTP methods</span></span>

<span data-ttu-id="20016-189">Per consentire a tutti i metodi HTTP, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="20016-189">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

<span data-ttu-id="20016-190">Questa impostazione influisce [preparare le richieste e l'intestazione Access-Control-Allow-Methods](#preflight-requests) (descritto più avanti in questo argomento).</span><span class="sxs-lookup"><span data-stu-id="20016-190">This setting affects [preflight requests and the Access-Control-Allow-Methods header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="20016-191">Impostare le intestazioni di richieste consentite</span><span class="sxs-lookup"><span data-stu-id="20016-191">Set the allowed request headers</span></span>

<span data-ttu-id="20016-192">Consentire o meno intestazioni specifiche da inviare in una richiesta CORS, chiamato *creare le intestazioni di richiesta*, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e specificare le intestazioni consentite:</span><span class="sxs-lookup"><span data-stu-id="20016-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="20016-193">Per consentire tutti gli autori il intestazioni della richiesta, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="20016-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="20016-194">Questa impostazione influisce [preparare le richieste e l'intestazione Access-Control-Request-Headers](#preflight-requests) (descritto più avanti in questo argomento).</span><span class="sxs-lookup"><span data-stu-id="20016-194">This setting affects [preflight requests and the Access-Control-Request-Headers header](#preflight-requests) (described later in this topic).</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="20016-195">Una corrispondenza di criteri di Middleware CORS a intestazioni specifiche specificato da `WithHeaders` è possibile solo quando le intestazioni inviate `Access-Control-Request-Headers` corrispondere esattamente le intestazioni riportate `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="20016-195">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="20016-196">Ad esempio, si consideri un'applicazione configurata come segue:</span><span class="sxs-lookup"><span data-stu-id="20016-196">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="20016-197">Middleware CORS rifiuta una richiesta preliminare con intestazione di richiesta seguente, in quanto `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) non è elencato `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="20016-197">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="20016-198">L'app restituisce una *200 OK* risposta, ma non invia nuovamente le intestazioni CORS.</span><span class="sxs-lookup"><span data-stu-id="20016-198">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="20016-199">Pertanto, il browser non tenta la richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="20016-199">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="20016-200">Middleware CORS consente sempre le intestazioni di quattro nel `Access-Control-Request-Headers` da inviare indipendentemente dai valori configurati in CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="20016-200">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="20016-201">Questo elenco di intestazioni include:</span><span class="sxs-lookup"><span data-stu-id="20016-201">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="20016-202">Ad esempio, si consideri un'applicazione configurata come segue:</span><span class="sxs-lookup"><span data-stu-id="20016-202">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="20016-203">Middleware CORS risponde correttamente a una richiesta preliminare con intestazione di richiesta seguente poiché `Content-Language` viene sempre incluso nella whitelist:</span><span class="sxs-lookup"><span data-stu-id="20016-203">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="20016-204">Impostare le intestazioni di risposta esposto</span><span class="sxs-lookup"><span data-stu-id="20016-204">Set the exposed response headers</span></span>

<span data-ttu-id="20016-205">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'app.</span><span class="sxs-lookup"><span data-stu-id="20016-205">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="20016-206">Per altre informazioni, vedere [W3C Cross-Origin Resource Sharing (terminologia): intestazione della risposta semplice](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="20016-206">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="20016-207">Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="20016-207">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="20016-208">La specifica CORS chiama queste intestazioni *intestazioni di risposta semplice*.</span><span class="sxs-lookup"><span data-stu-id="20016-208">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="20016-209">Per rendere disponibili app per le altre intestazioni, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="20016-209">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="20016-210">Credenziali in richieste multiorigine</span><span class="sxs-lookup"><span data-stu-id="20016-210">Credentials in cross-origin requests</span></span>

<span data-ttu-id="20016-211">Credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="20016-211">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="20016-212">Per impostazione predefinita, il browser non invia le credenziali con una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="20016-212">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="20016-213">Le credenziali includono i cookie e gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="20016-213">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="20016-214">Per inviare le credenziali con una richiesta multiorigine, il client deve impostare `XMLHttpRequest.withCredentials` a `true`.</span><span class="sxs-lookup"><span data-stu-id="20016-214">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="20016-215">Usando `XMLHttpRequest` direttamente:</span><span class="sxs-lookup"><span data-stu-id="20016-215">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="20016-216">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="20016-216">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="20016-217">Inoltre, il server deve consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="20016-217">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="20016-218">Per consentire le credenziali tra le origini, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="20016-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

<span data-ttu-id="20016-219">La risposta HTTP include un `Access-Control-Allow-Credentials` intestazione, che indica al browser che il server consenta le credenziali per una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="20016-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="20016-220">Se il browser invia le credenziali, ma la risposta non include un valore valido `Access-Control-Allow-Credentials` intestazione, il browser non espone la risposta all'app e la richiesta multiorigine non riesce.</span><span class="sxs-lookup"><span data-stu-id="20016-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="20016-221">Prestare attenzione quando si consente le credenziali di cross-origin.</span><span class="sxs-lookup"><span data-stu-id="20016-221">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="20016-222">Un sito Web all'indirizzo di un altro dominio può inviare le credenziali dell'utente connesso all'app per conto dell'utente senza il consenso dell'utente.</span><span class="sxs-lookup"><span data-stu-id="20016-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="20016-223">La specifica CORS indica anche che l'impostazione origini `"*"` (tutte le origini) non è valido se la `Access-Control-Allow-Credentials` intestazione è presente.</span><span class="sxs-lookup"><span data-stu-id="20016-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="20016-224">Richieste preliminari</span><span class="sxs-lookup"><span data-stu-id="20016-224">Preflight requests</span></span>

<span data-ttu-id="20016-225">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva prima di effettuare la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="20016-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="20016-226">Questa richiesta viene chiamata un *richiesta preliminare*.</span><span class="sxs-lookup"><span data-stu-id="20016-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="20016-227">Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="20016-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="20016-228">Il metodo di richiesta è GET, HEAD o POST.</span><span class="sxs-lookup"><span data-stu-id="20016-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="20016-229">L'app non imposta le intestazioni delle richieste diverso da `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, o `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="20016-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="20016-230">Il `Content-Type` intestazione, se impostata, dispone di uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="20016-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="20016-231">La regola nelle intestazioni di richiesta impostata per la richiesta del client vengono applicati alle intestazioni che l'app imposta chiamando `setRequestHeader` nella `XMLHttpRequest` oggetto.</span><span class="sxs-lookup"><span data-stu-id="20016-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="20016-232">La specifica CORS chiama queste intestazioni *creare le intestazioni di richiesta*.</span><span class="sxs-lookup"><span data-stu-id="20016-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="20016-233">La regola non è valida per il browser è possibile impostare, ad esempio le intestazioni `User-Agent`, `Host`, o `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="20016-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="20016-234">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="20016-234">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="20016-235">La richiesta preliminare viene utilizzato il metodo OPTIONS HTTP.</span><span class="sxs-lookup"><span data-stu-id="20016-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="20016-236">Include due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="20016-236">It includes two special headers:</span></span>

* <span data-ttu-id="20016-237">`Access-Control-Request-Method`: Metodo HTTP che verrà usato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="20016-237">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="20016-238">`Access-Control-Request-Headers`: Elenco di intestazioni di richiesta che l'app imposta nella richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="20016-238">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="20016-239">Come indicato in precedenza, questo non include le intestazioni che imposta il browser, ad esempio `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="20016-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="20016-240">Una richiesta CORS preliminare può includere un `Access-Control-Request-Headers` intestazione, che indica al server le intestazioni che vengono inviate con la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="20016-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="20016-241">Per consentire a intestazioni specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="20016-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="20016-242">Per consentire tutti gli autori il intestazioni della richiesta, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="20016-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="20016-243">I browser non sono completamente coerenti nel modo in cui sono impostati `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="20016-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="20016-244">Se si imposta le intestazioni a qualsiasi elemento diverso da `"*"` (oppure usare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), è necessario includere almeno `Accept`, `Content-Type`, e `Origin`, oltre a eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="20016-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="20016-245">Di seguito è riportato un esempio di risposta alla richiesta preliminare (presupponendo che il server consente la richiesta):</span><span class="sxs-lookup"><span data-stu-id="20016-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="20016-246">La risposta include un' `Access-Control-Allow-Methods` intestazione in cui sono elencati i metodi consentiti e, facoltativamente, un `Access-Control-Allow-Headers` intestazione, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="20016-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="20016-247">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="20016-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="20016-248">Se la richiesta preliminare viene negata, l'app restituisce una *200 OK* risposta, ma non invia nuovamente le intestazioni CORS.</span><span class="sxs-lookup"><span data-stu-id="20016-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="20016-249">Pertanto, il browser non tenta la richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="20016-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="20016-250">Impostare l'ora di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="20016-250">Set the preflight expiration time</span></span>

<span data-ttu-id="20016-251">Il `Access-Control-Max-Age` intestazione specifica quanto tempo può essere memorizzati nella cache la risposta alla richiesta preliminare.</span><span class="sxs-lookup"><span data-stu-id="20016-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="20016-252">Per impostare questa intestazione, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="20016-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="20016-253">Funzionamento della condivisione CORS</span><span class="sxs-lookup"><span data-stu-id="20016-253">How CORS works</span></span>

<span data-ttu-id="20016-254">In questa sezione viene descritto cosa succede in una richiesta CORS a livello di messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="20016-254">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="20016-255">È importante comprendere il funzionamento di CORS in modo che il criterio CORS può essere configurato correttamente e il debug quando si verificano comportamenti imprevisti.</span><span class="sxs-lookup"><span data-stu-id="20016-255">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="20016-256">La specifica CORS introduce diverse nuove intestazioni HTTP che attivare richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="20016-256">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="20016-257">Se un browser supporta la condivisione CORS, imposta le intestazioni delle automaticamente per le richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="20016-257">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="20016-258">Il codice JavaScript personalizzato non è necessario abilitare CORS.</span><span class="sxs-lookup"><span data-stu-id="20016-258">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="20016-259">Di seguito è riportato un esempio di una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="20016-259">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="20016-260">Il `Origin` intestazione fornisce il dominio del sito che effettua la richiesta:</span><span class="sxs-lookup"><span data-stu-id="20016-260">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="20016-261">Se il server consente la richiesta, viene impostato il `Access-Control-Allow-Origin` intestazione nella risposta.</span><span class="sxs-lookup"><span data-stu-id="20016-261">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="20016-262">Il valore di questa intestazione corrisponde a sia la `Origin` intestazione dalla richiesta o è il valore del carattere jolly `"*"`, che significa che è consentita qualsiasi origine:</span><span class="sxs-lookup"><span data-stu-id="20016-262">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="20016-263">Se la risposta non include il `Access-Control-Allow-Origin` ha esito negativo di intestazione, la richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="20016-263">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="20016-264">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="20016-264">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="20016-265">Anche se il server restituisce una risposta con esito positivo, il browser non rende la risposta disponibili per l'app client.</span><span class="sxs-lookup"><span data-stu-id="20016-265">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="20016-266">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="20016-266">Additional resources</span></span>

* [<span data-ttu-id="20016-267">Cross-Origin Resource Sharing (CORS)</span><span class="sxs-lookup"><span data-stu-id="20016-267">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
