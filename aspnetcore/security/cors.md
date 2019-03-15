---
title: Abilitare le richieste Multiorigine (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come condivisione CORS come standard per consentire o rifiutare le richieste multiorigine in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/27/2019
uid: security/cors
ms.openlocfilehash: 6be8b4da1642a9eff021371c229a17071d6e9bfb
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978471"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="e2701-103">Abilitare le richieste Multiorigine (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2701-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="e2701-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e2701-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e2701-105">Questo articolo illustra come abilitare CORS in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2701-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="e2701-106">Protezione del browser impedisce che una pagina web inviando richieste a un dominio diverso da quello che ha gestito la pagina web.</span><span class="sxs-lookup"><span data-stu-id="e2701-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="e2701-107">Questa restrizione viene chiamata il *criterio della stessa origine*.</span><span class="sxs-lookup"><span data-stu-id="e2701-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="e2701-108">Il criterio della stessa origine impedisce a un sito dannoso di leggere i dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="e2701-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="e2701-109">In alcuni casi, si potrebbe voler consentire che ad altri siti effettuare richieste multiorigine per l'app.</span><span class="sxs-lookup"><span data-stu-id="e2701-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="e2701-110">Per altre informazioni, vedere la [articolo Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="e2701-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="e2701-111">[Cross Origin Resource Sharing, condivisione](https://www.w3.org/TR/cors/) (le origini CORS):</span><span class="sxs-lookup"><span data-stu-id="e2701-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="e2701-112">È un W3C standard che consente a un server di ridurre i criteri di corrispondenza dell'origine.</span><span class="sxs-lookup"><span data-stu-id="e2701-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="e2701-113">Viene **non** una funzionalità di sicurezza, CORS rilassa sicurezza.</span><span class="sxs-lookup"><span data-stu-id="e2701-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="e2701-114">Un'API non è più sicura, consentendo a CORS.</span><span class="sxs-lookup"><span data-stu-id="e2701-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="e2701-115">Per altre informazioni, vedere [funziona come CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="e2701-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="e2701-116">Consente a un server consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre.</span><span class="sxs-lookup"><span data-stu-id="e2701-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="e2701-117">È più sicura e più flessibile di tecniche precedenti, ad esempio [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="e2701-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="e2701-118">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e2701-118">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="e2701-119">Origine stessa</span><span class="sxs-lookup"><span data-stu-id="e2701-119">Same origin</span></span>

<span data-ttu-id="e2701-120">Due URL abbia la stessa origine se dispongono di porte, host e gli schemi identici ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="e2701-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="e2701-121">Questi due URL abbia la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="e2701-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="e2701-122">Questi URL sono le entità Origin diversi rispetto i due URL precedente:</span><span class="sxs-lookup"><span data-stu-id="e2701-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="e2701-123">`https://example.net` &ndash; Dominio diverso</span><span class="sxs-lookup"><span data-stu-id="e2701-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="e2701-124">`https://www.example.com/foo.html` &ndash; Sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="e2701-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="e2701-125">`http://example.com/foo.html` &ndash; Schema differente</span><span class="sxs-lookup"><span data-stu-id="e2701-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="e2701-126">`https://example.com:9000/foo.html` &ndash; Porta diversa</span><span class="sxs-lookup"><span data-stu-id="e2701-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="e2701-127">Internet Explorer non considera la porta quando si confrontano le entità Origin.</span><span class="sxs-lookup"><span data-stu-id="e2701-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="e2701-128">CORS con criteri denominati e middleware</span><span class="sxs-lookup"><span data-stu-id="e2701-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="e2701-129">Middleware CORS gestisce le richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="e2701-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="e2701-130">Il codice seguente Abilita CORS per l'intera app con l'entità origin specificata:</span><span class="sxs-lookup"><span data-stu-id="e2701-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="e2701-131">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="e2701-131">The preceding code:</span></span>

* <span data-ttu-id="e2701-132">Imposta il nome del criterio "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="e2701-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="e2701-133">Il nome del criterio è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="e2701-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="e2701-134">Le chiamate di <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metodo di estensione, che consente di core.</span><span class="sxs-lookup"><span data-stu-id="e2701-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables cores.</span></span>
* <span data-ttu-id="e2701-135">Le chiamate <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con un [espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="e2701-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="e2701-136">L'operatore lambda accetta un <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> oggetto.</span><span class="sxs-lookup"><span data-stu-id="e2701-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="e2701-137">[Opzioni di configurazione](#cors-policy-options), ad esempio `WithOrigins`, sono descritti più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="e2701-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="e2701-138">Il <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> chiamata al metodo aggiunge servizi CORS al contenitore del servizio dell'app:</span><span class="sxs-lookup"><span data-stu-id="e2701-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="e2701-139">Per altre informazioni, vedere [le opzioni dei criteri CORS](#cpo) in questo documento.</span><span class="sxs-lookup"><span data-stu-id="e2701-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="e2701-140">Il <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> metodo possibile concatenare i metodi, come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e2701-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="e2701-141">Il codice evidenziato seguente applica i criteri CORS per tutti gli endpoint di App tramite il Middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="e2701-141">The following highlighted code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors(); 

    app.UseHttpsRedirection();
    app.UseMvc();
}
```

<span data-ttu-id="e2701-142">Visualizzare [abilitare CORS in Razor Pages, controller e metodi di azione](#ecors) per applicare i criteri CORS a livello di pagina/controller/azione.</span><span class="sxs-lookup"><span data-stu-id="e2701-142">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="e2701-143">Nota:</span><span class="sxs-lookup"><span data-stu-id="e2701-143">Note:</span></span>

* <span data-ttu-id="e2701-144">`UseCors` deve essere chiamato prima `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="e2701-144">`UseCors` must be called before `UseMvc`.</span></span>
* <span data-ttu-id="e2701-145">L'URL deve **non** contengono una barra finale (`/`).</span><span class="sxs-lookup"><span data-stu-id="e2701-145">The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="e2701-146">Se l'URL termina con `/`, il confronto restituisce `false` e non viene restituita alcuna intestazione.</span><span class="sxs-lookup"><span data-stu-id="e2701-146">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="e2701-147">Visualizzare [Test CORS](#test) per istruzioni su come testare il codice precedente.</span><span class="sxs-lookup"><span data-stu-id="e2701-147">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="e2701-148">Abilitare CORS con attributi</span><span class="sxs-lookup"><span data-stu-id="e2701-148">Enable CORS with attributes</span></span>

<span data-ttu-id="e2701-149">Il [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attributo fornisce un'alternativa all'applicazione di condivisione CORS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="e2701-149">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="e2701-150">Il `[EnableCors]` attributo Abilita CORS per punti di fine selezionati anziché tutti i punti di fine.</span><span class="sxs-lookup"><span data-stu-id="e2701-150">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="e2701-151">Uso `[EnableCors]` per specificare i criteri predefiniti e `[EnableCors("{Policy String}")]` per specificare un criterio.</span><span class="sxs-lookup"><span data-stu-id="e2701-151">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="e2701-152">Il `[EnableCors]` attributo può essere applicato a:</span><span class="sxs-lookup"><span data-stu-id="e2701-152">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="e2701-153">Pagina Razor `PageModel`</span><span class="sxs-lookup"><span data-stu-id="e2701-153">Razor Page `PageModel`</span></span>
* <span data-ttu-id="e2701-154">Controller</span><span class="sxs-lookup"><span data-stu-id="e2701-154">Controller</span></span>
* <span data-ttu-id="e2701-155">Metodo di azione del controller</span><span class="sxs-lookup"><span data-stu-id="e2701-155">Controller action method</span></span>

<span data-ttu-id="e2701-156">È possibile applicare criteri diversi per controller/pagina-modello/azione con il `[EnableCors]` attributo.</span><span class="sxs-lookup"><span data-stu-id="e2701-156">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="e2701-157">Quando il `[EnableCors]` attributo viene applicato a un metodo di azione/controller/pagina-modello e condivisione CORS è abilitata nel middleware, entrambi i criteri vengono applicati.</span><span class="sxs-lookup"><span data-stu-id="e2701-157">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="e2701-158">È consigliabile evitare di combinazione di criteri.</span><span class="sxs-lookup"><span data-stu-id="e2701-158">We recommend against combining policies.</span></span> <span data-ttu-id="e2701-159">Usare il `[EnableCors]` attributo o un middleware, non entrambi nella stessa app.</span><span class="sxs-lookup"><span data-stu-id="e2701-159">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="e2701-160">Il codice seguente si applica un criterio diverso per ogni metodo:</span><span class="sxs-lookup"><span data-stu-id="e2701-160">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="e2701-161">Il codice seguente crea un criterio predefinito CORS e un criterio denominato `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="e2701-161">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="e2701-162">Disabilitare la condivisione CORS</span><span class="sxs-lookup"><span data-stu-id="e2701-162">Disable CORS</span></span>

<span data-ttu-id="e2701-163">Il [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attributo disabilita CORS per il controller/pagina-modello/azione.</span><span class="sxs-lookup"><span data-stu-id="e2701-163">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="e2701-164">Opzioni dei criteri CORS</span><span class="sxs-lookup"><span data-stu-id="e2701-164">CORS policy options</span></span>

<span data-ttu-id="e2701-165">Questa sezione descrive le varie opzioni che possono essere impostate in un criterio CORS:</span><span class="sxs-lookup"><span data-stu-id="e2701-165">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="e2701-166">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="e2701-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="e2701-167">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="e2701-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="e2701-168">Impostare le intestazioni di richieste consentite</span><span class="sxs-lookup"><span data-stu-id="e2701-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="e2701-169">Impostare le intestazioni di risposta esposto</span><span class="sxs-lookup"><span data-stu-id="e2701-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="e2701-170">Credenziali in richieste multiorigine</span><span class="sxs-lookup"><span data-stu-id="e2701-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="e2701-171">Impostare l'ora di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="e2701-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

 <span data-ttu-id="e2701-172"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> viene chiamato `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e2701-172"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e2701-173">Per alcune opzioni, potrebbe essere utile leggere il [funziona come CORS](#how-cors) sezione prima di tutto.</span><span class="sxs-lookup"><span data-stu-id="e2701-173">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="e2701-174">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="e2701-174">Set the allowed origins</span></span>

<span data-ttu-id="e2701-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Consente le richieste CORS da tutte le entità Origin con qualsiasi schema (`http` o `https`).</span><span class="sxs-lookup"><span data-stu-id="e2701-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="e2701-176">`AllowAnyOrigin` non è sicura perché *qualsiasi sito Web* possono eseguire richieste multiorigine per l'app.</span><span class="sxs-lookup"><span data-stu-id="e2701-176">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="e2701-177">Che specifica `AllowAnyOrigin` e `AllowCredentials` è una configurazione non protetta e può comportare la richiesta intersito falsa.</span><span class="sxs-lookup"><span data-stu-id="e2701-177">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="e2701-178">Il servizio CORS restituisce una risposta CORS non valida quando un'app è configurata con entrambi i metodi.</span><span class="sxs-lookup"><span data-stu-id="e2701-178">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="e2701-179">Che specifica `AllowAnyOrigin` e `AllowCredentials` è una configurazione non protetta e può comportare la richiesta intersito falsa.</span><span class="sxs-lookup"><span data-stu-id="e2701-179">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="e2701-180">Per un'app protetta, specificare l'elenco esatto di origini, se è necessario autorizzare il client per accedere alle risorse di server.</span><span class="sxs-lookup"><span data-stu-id="e2701-180">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

  ::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**. 
-->

  <span data-ttu-id="e2701-181">`AllowAnyOrigin` le richieste di verifica preliminare interessa e `Access-Control-Allow-Origin` intestazione.</span><span class="sxs-lookup"><span data-stu-id="e2701-181">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="e2701-182">Per altre informazioni, vedere la [richieste di verifica preliminare](#preflight-requests) sezione.</span><span class="sxs-lookup"><span data-stu-id="e2701-182">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e2701-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Imposta il <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> proprietà dei criteri è una funzione che consente le origini corrispondere a un dominio configurato con caratteri jolly quando si valuta se l'origine è consentita.</span><span class="sxs-lookup"><span data-stu-id="e2701-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="e2701-184">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="e2701-184">Set the allowed HTTP methods</span></span>

<span data-ttu-id="e2701-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="e2701-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="e2701-186">Consente a qualsiasi metodo HTTP:</span><span class="sxs-lookup"><span data-stu-id="e2701-186">Allows any HTTP method:</span></span>
* <span data-ttu-id="e2701-187">Le richieste di verifica preliminare interessa e `Access-Control-Allow-Methods` intestazione.</span><span class="sxs-lookup"><span data-stu-id="e2701-187">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="e2701-188">Per altre informazioni, vedere la [richieste di verifica preliminare](#preflight-requests) sezione.</span><span class="sxs-lookup"><span data-stu-id="e2701-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="e2701-189">Impostare le intestazioni di richieste consentite</span><span class="sxs-lookup"><span data-stu-id="e2701-189">Set the allowed request headers</span></span>

<span data-ttu-id="e2701-190">Consentire o meno intestazioni specifiche da inviare in una richiesta CORS, chiamato *creare le intestazioni di richiesta*, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e specificare le intestazioni consentite:</span><span class="sxs-lookup"><span data-stu-id="e2701-190">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="e2701-191">Per consentire tutti gli autori il intestazioni della richiesta, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="e2701-191">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="e2701-192">Questa impostazione interessa le richieste preliminari e `Access-Control-Request-Headers` intestazione.</span><span class="sxs-lookup"><span data-stu-id="e2701-192">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="e2701-193">Per altre informazioni, vedere la [richieste di verifica preliminare](#preflight-requests) sezione.</span><span class="sxs-lookup"><span data-stu-id="e2701-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e2701-194">Una corrispondenza di criteri di Middleware CORS a intestazioni specifiche specificato da `WithHeaders` è possibile solo quando le intestazioni inviate `Access-Control-Request-Headers` corrispondere esattamente le intestazioni riportate `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="e2701-194">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="e2701-195">Ad esempio, si consideri un'applicazione configurata come segue:</span><span class="sxs-lookup"><span data-stu-id="e2701-195">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="e2701-196">Middleware CORS rifiuta una richiesta preliminare con intestazione di richiesta seguente, in quanto `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) non è elencato `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="e2701-196">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="e2701-197">L'app restituisce una *200 OK* risposta, ma non invia nuovamente le intestazioni CORS.</span><span class="sxs-lookup"><span data-stu-id="e2701-197">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="e2701-198">Pertanto, il browser non tenta la richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="e2701-198">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e2701-199">Middleware CORS consente sempre le intestazioni di quattro nel `Access-Control-Request-Headers` da inviare indipendentemente dai valori configurati in CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="e2701-199">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="e2701-200">Questo elenco di intestazioni include:</span><span class="sxs-lookup"><span data-stu-id="e2701-200">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="e2701-201">Ad esempio, si consideri un'applicazione configurata come segue:</span><span class="sxs-lookup"><span data-stu-id="e2701-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="e2701-202">Middleware CORS risponde correttamente a una richiesta preliminare con intestazione di richiesta seguente poiché `Content-Language` viene sempre incluso nella whitelist:</span><span class="sxs-lookup"><span data-stu-id="e2701-202">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="e2701-203">Impostare le intestazioni di risposta esposto</span><span class="sxs-lookup"><span data-stu-id="e2701-203">Set the exposed response headers</span></span>

<span data-ttu-id="e2701-204">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'app.</span><span class="sxs-lookup"><span data-stu-id="e2701-204">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="e2701-205">Per altre informazioni, vedere [W3C Cross-Origin Resource Sharing (terminologia): Intestazione della risposta semplice](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="e2701-205">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="e2701-206">Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="e2701-206">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="e2701-207">La specifica CORS chiama queste intestazioni *intestazioni di risposta semplice*.</span><span class="sxs-lookup"><span data-stu-id="e2701-207">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="e2701-208">Per rendere disponibili app per le altre intestazioni, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="e2701-208">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="e2701-209">Credenziali in richieste multiorigine</span><span class="sxs-lookup"><span data-stu-id="e2701-209">Credentials in cross-origin requests</span></span>

<span data-ttu-id="e2701-210">Credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="e2701-210">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="e2701-211">Per impostazione predefinita, il browser non invia le credenziali con una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="e2701-211">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="e2701-212">Le credenziali includono i cookie e gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2701-212">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="e2701-213">Per inviare le credenziali con una richiesta multiorigine, il client deve impostare `XMLHttpRequest.withCredentials` a `true`.</span><span class="sxs-lookup"><span data-stu-id="e2701-213">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="e2701-214">Usando `XMLHttpRequest` direttamente:</span><span class="sxs-lookup"><span data-stu-id="e2701-214">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="e2701-215">Uso di jQuery:</span><span class="sxs-lookup"><span data-stu-id="e2701-215">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="e2701-216">Usando il [recuperare API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="e2701-216">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="e2701-217">Il server deve consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="e2701-217">The server must allow the credentials.</span></span> <span data-ttu-id="e2701-218">Per consentire le credenziali tra le origini, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="e2701-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="e2701-219">La risposta HTTP include un `Access-Control-Allow-Credentials` intestazione, che indica al browser che il server consenta le credenziali per una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="e2701-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="e2701-220">Se il browser invia le credenziali, ma la risposta non include un valore valido `Access-Control-Allow-Credentials` intestazione, il browser non espone la risposta all'app e la richiesta multiorigine non riesce.</span><span class="sxs-lookup"><span data-stu-id="e2701-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="e2701-221">Consentire le credenziali di cross-origin è un rischio per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="e2701-221">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="e2701-222">Un sito Web all'indirizzo di un altro dominio può inviare le credenziali dell'utente connesso all'app per conto dell'utente senza il consenso dell'utente.</span><span class="sxs-lookup"><span data-stu-id="e2701-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="e2701-223">La specifica CORS indica anche che l'impostazione origini `"*"` (tutte le origini) non è valido se la `Access-Control-Allow-Credentials` intestazione è presente.</span><span class="sxs-lookup"><span data-stu-id="e2701-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="e2701-224">Richieste preliminari</span><span class="sxs-lookup"><span data-stu-id="e2701-224">Preflight requests</span></span>

<span data-ttu-id="e2701-225">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva prima di effettuare la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="e2701-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="e2701-226">Questa richiesta viene chiamata un *richiesta preliminare*.</span><span class="sxs-lookup"><span data-stu-id="e2701-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="e2701-227">Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="e2701-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="e2701-228">Il metodo di richiesta è GET, HEAD o POST.</span><span class="sxs-lookup"><span data-stu-id="e2701-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="e2701-229">L'app non imposta le intestazioni delle richieste diverso da `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, o `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="e2701-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="e2701-230">Il `Content-Type` intestazione, se impostata, dispone di uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="e2701-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="e2701-231">La regola nelle intestazioni di richiesta impostata per la richiesta del client vengono applicati alle intestazioni che l'app imposta chiamando `setRequestHeader` nella `XMLHttpRequest` oggetto.</span><span class="sxs-lookup"><span data-stu-id="e2701-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="e2701-232">La specifica CORS chiama queste intestazioni *creare le intestazioni di richiesta*.</span><span class="sxs-lookup"><span data-stu-id="e2701-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="e2701-233">La regola non è valida per il browser è possibile impostare, ad esempio le intestazioni `User-Agent`, `Host`, o `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="e2701-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="e2701-234">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="e2701-234">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="e2701-235">La richiesta preliminare viene utilizzato il metodo OPTIONS HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2701-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="e2701-236">Include due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="e2701-236">It includes two special headers:</span></span>

* <span data-ttu-id="e2701-237">`Access-Control-Request-Method`: Il metodo HTTP che verrà usato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="e2701-237">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="e2701-238">`Access-Control-Request-Headers`: Elenco di intestazioni di richiesta che l'app imposta nella richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="e2701-238">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="e2701-239">Come indicato in precedenza, questo non include le intestazioni che imposta il browser, ad esempio `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="e2701-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="e2701-240">Una richiesta CORS preliminare può includere un `Access-Control-Request-Headers` intestazione, che indica al server le intestazioni che vengono inviate con la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="e2701-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="e2701-241">Per consentire a intestazioni specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="e2701-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="e2701-242">Per consentire tutti gli autori il intestazioni della richiesta, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="e2701-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="e2701-243">I browser non sono completamente coerenti nel modo in cui sono impostati `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="e2701-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="e2701-244">Se si imposta le intestazioni a qualsiasi elemento diverso da `"*"` (oppure usare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), è necessario includere almeno `Accept`, `Content-Type`, e `Origin`, oltre a eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="e2701-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="e2701-245">Di seguito è riportato un esempio di risposta alla richiesta preliminare (presupponendo che il server consente la richiesta):</span><span class="sxs-lookup"><span data-stu-id="e2701-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="e2701-246">La risposta include un' `Access-Control-Allow-Methods` intestazione in cui sono elencati i metodi consentiti e, facoltativamente, un `Access-Control-Allow-Headers` intestazione, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="e2701-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="e2701-247">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="e2701-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="e2701-248">Se la richiesta preliminare viene negata, l'app restituisce una *200 OK* risposta, ma non invia nuovamente le intestazioni CORS.</span><span class="sxs-lookup"><span data-stu-id="e2701-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="e2701-249">Pertanto, il browser non tenta la richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="e2701-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="e2701-250">Impostare l'ora di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="e2701-250">Set the preflight expiration time</span></span>

<span data-ttu-id="e2701-251">Il `Access-Control-Max-Age` intestazione specifica quanto tempo può essere memorizzati nella cache la risposta alla richiesta preliminare.</span><span class="sxs-lookup"><span data-stu-id="e2701-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="e2701-252">Per impostare questa intestazione, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="e2701-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="e2701-253">Funzionamento della condivisione CORS</span><span class="sxs-lookup"><span data-stu-id="e2701-253">How CORS works</span></span>

<span data-ttu-id="e2701-254">In questa sezione viene descritto cosa succede un [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) richiesta a livello di messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2701-254">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="e2701-255">CORS è **non** una funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="e2701-255">CORS is **not** a security feature.</span></span> <span data-ttu-id="e2701-256">CORS è un standard W3C che consente a un server di ridurre i criteri di corrispondenza dell'origine.</span><span class="sxs-lookup"><span data-stu-id="e2701-256">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="e2701-257">Ad esempio, è possibile usare un attore malintenzionato [impedire Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) contro il sito ed eseguire una richiesta tra siti con il relativo sito abilitato per CORS di sottrarre informazioni.</span><span class="sxs-lookup"><span data-stu-id="e2701-257">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="e2701-258">L'API non è più sicuro, consentendo a CORS.</span><span class="sxs-lookup"><span data-stu-id="e2701-258">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="e2701-259">Spetta al client (browser) per applicare CORS.</span><span class="sxs-lookup"><span data-stu-id="e2701-259">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="e2701-260">Il server esegue la richiesta e restituisce la risposta, ma il client che restituisce un errore e i blocchi la risposta.</span><span class="sxs-lookup"><span data-stu-id="e2701-260">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="e2701-261">Ad esempio, uno qualsiasi dei seguenti strumenti verrà visualizzata la risposta del server:</span><span class="sxs-lookup"><span data-stu-id="e2701-261">For example, any of the following tools will display the server response:</span></span>
     * [<span data-ttu-id="e2701-262">Fiddler</span><span class="sxs-lookup"><span data-stu-id="e2701-262">Fiddler</span></span>](https://www.telerik.com/fiddler)
     * [<span data-ttu-id="e2701-263">Postman</span><span class="sxs-lookup"><span data-stu-id="e2701-263">Postman</span></span>](https://www.getpostman.com/)
     * [<span data-ttu-id="e2701-264">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="e2701-264">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
     * <span data-ttu-id="e2701-265">Un web browser immettendo l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="e2701-265">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="e2701-266">È un modo per un server per consentire i browser per l'esecuzione di una cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) oppure [recuperare API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) richiesta che in caso contrario, potrebbe essere non è consentito.</span><span class="sxs-lookup"><span data-stu-id="e2701-266">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="e2701-267">Browser (senza CORS) non è possibile eseguire l'operazione di richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="e2701-267">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="e2701-268">Prima di CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) usata per aggirare questa limitazione.</span><span class="sxs-lookup"><span data-stu-id="e2701-268">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="e2701-269">JSONP non usa XHR, Usa il `<script>` tag per ricevere la risposta.</span><span class="sxs-lookup"><span data-stu-id="e2701-269">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="e2701-270">Gli script possono essere caricati cross-origin.</span><span class="sxs-lookup"><span data-stu-id="e2701-270">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="e2701-271">Il [specifica CORS]() introdotte diverse nuove intestazioni HTTP che attivare richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="e2701-271">The [CORS specification]() introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="e2701-272">Se un browser supporta la condivisione CORS, imposta le intestazioni delle automaticamente per le richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="e2701-272">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="e2701-273">Il codice JavaScript personalizzato non è necessario abilitare CORS.</span><span class="sxs-lookup"><span data-stu-id="e2701-273">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="e2701-274">Di seguito è riportato un esempio di una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="e2701-274">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="e2701-275">Il `Origin` intestazione fornisce il dominio del sito che effettua la richiesta:</span><span class="sxs-lookup"><span data-stu-id="e2701-275">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="e2701-276">Se il server consente la richiesta, viene impostato il `Access-Control-Allow-Origin` intestazione nella risposta.</span><span class="sxs-lookup"><span data-stu-id="e2701-276">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="e2701-277">Il valore di questa intestazione corrisponde a sia la `Origin` intestazione dalla richiesta o è il valore del carattere jolly `"*"`, che significa che è consentita qualsiasi origine:</span><span class="sxs-lookup"><span data-stu-id="e2701-277">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="e2701-278">Se la risposta non include il `Access-Control-Allow-Origin` ha esito negativo di intestazione, la richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="e2701-278">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="e2701-279">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="e2701-279">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="e2701-280">Anche se il server restituisce una risposta con esito positivo, il browser non rende la risposta disponibili per l'app client.</span><span class="sxs-lookup"><span data-stu-id="e2701-280">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="e2701-281">Testare CORS</span><span class="sxs-lookup"><span data-stu-id="e2701-281">Test CORS</span></span>

<span data-ttu-id="e2701-282">Testare CORS:</span><span class="sxs-lookup"><span data-stu-id="e2701-282">To test CORS:</span></span>

1. <span data-ttu-id="e2701-283">[Creare un progetto API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="e2701-283">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="e2701-284">In alternativa, è possibile [scaricare l'esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="e2701-284">Alternatively, you can [download the sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="e2701-285">Abilitare la condivisione CORS tramite uno degli approcci in questo documento.</span><span class="sxs-lookup"><span data-stu-id="e2701-285">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="e2701-286">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e2701-286">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]
  
  > [!WARNING]
  > <span data-ttu-id="e2701-287">`WithOrigins("https://localhost:<port>");` deve essere utilizzato solo per i test di un'app di esempio simile al [scaricare codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="e2701-287">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="e2701-288">Creare un progetto di app web (Razor Pages o MVC).</span><span class="sxs-lookup"><span data-stu-id="e2701-288">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="e2701-289">L'esempio Usa le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="e2701-289">The sample uses Razor Pages.</span></span> <span data-ttu-id="e2701-290">È possibile creare l'app web nella stessa soluzione come progetto di API.</span><span class="sxs-lookup"><span data-stu-id="e2701-290">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="e2701-291">Aggiungere il codice evidenziato seguente per il *index. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="e2701-291">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="e2701-292">Nel codice precedente sostituire `url: 'https://<web app>.azurewebsites.net/api/values/1',` con l'URL per l'app distribuita.</span><span class="sxs-lookup"><span data-stu-id="e2701-292">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="e2701-293">Distribuire il progetto di API.</span><span class="sxs-lookup"><span data-stu-id="e2701-293">Deploy the API project.</span></span> <span data-ttu-id="e2701-294">Ad esempio, [Distribuisci in Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="e2701-294">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="e2701-295">Eseguire l'app per Razor Pages o MVC dal desktop e fare clic sui **Test** pulsante.</span><span class="sxs-lookup"><span data-stu-id="e2701-295">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="e2701-296">Usare gli strumenti F12 per esaminare i messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="e2701-296">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="e2701-297">Rimuovere l'origine di localhost da `WithOrigins` e distribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="e2701-297">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="e2701-298">In alternativa, eseguire l'app client con una porta diversa.</span><span class="sxs-lookup"><span data-stu-id="e2701-298">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="e2701-299">Ad esempio, eseguire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2701-299">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="e2701-300">Il test con l'app client.</span><span class="sxs-lookup"><span data-stu-id="e2701-300">Test with the client app.</span></span> <span data-ttu-id="e2701-301">Gli errori CORS restituiscono un errore, ma il messaggio di errore non è disponibile a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e2701-301">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="e2701-302">Utilizzare la scheda della console negli strumenti F12 per visualizzare l'errore.</span><span class="sxs-lookup"><span data-stu-id="e2701-302">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="e2701-303">A seconda del browser, viene visualizzato un errore (nella console di strumenti F12) simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e2701-303">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

  * <span data-ttu-id="e2701-304">Utilizzo di Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="e2701-304">Using Microsoft Edge:</span></span>

    <span data-ttu-id="e2701-305">**SEC7120: [CORS] origine `https://localhost:44375` non ha trovato `https://localhost:44375` nell'intestazione della risposta Access-Control-Allow-Origin per la risorsa cross-origin in `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="e2701-305">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

  * <span data-ttu-id="e2701-306">Usa Chrome:</span><span class="sxs-lookup"><span data-stu-id="e2701-306">Using Chrome:</span></span>

    <span data-ttu-id="e2701-307">**Accesso a XMLHttpRequest al `https://webapi.azurewebsites.net/api/values/1` dall'origine `https://localhost:44375` è stata bloccata dal criterio CORS: Nessuna intestazione 'Access-Control-Allow-Origin' è presente nella risorsa richiesta.**</span><span class="sxs-lookup"><span data-stu-id="e2701-307">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2701-308">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e2701-308">Additional resources</span></span>

* [<span data-ttu-id="e2701-309">Cross-Origin Resource Sharing (CORS)</span><span class="sxs-lookup"><span data-stu-id="e2701-309">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
