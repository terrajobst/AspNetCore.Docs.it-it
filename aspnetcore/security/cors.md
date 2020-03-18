---
title: Abilitare le richieste tra le origini (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come CORS come standard per consentire o rifiutare le richieste tra origini in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 09cc296ebf605907371619124cac00883beb6abb
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511574"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="a03b6-103">Abilitare le richieste tra le origini (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a03b6-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a03b6-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a03b6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a03b6-105">Questo articolo illustra come abilitare CORS in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a03b6-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="a03b6-106">La sicurezza del browser impedisce a una pagina Web di eseguire richieste a un dominio diverso da quello che ha gestito la pagina Web.</span><span class="sxs-lookup"><span data-stu-id="a03b6-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="a03b6-107">Questa restrizione è detta *criterio della stessa origine*.</span><span class="sxs-lookup"><span data-stu-id="a03b6-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="a03b6-108">Il criterio della stessa origine impedisce a un sito dannoso di leggere dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="a03b6-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="a03b6-109">In alcuni casi potrebbe essere necessario consentire ad altri siti di effettuare richieste tra le origini all'app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="a03b6-110">Per altre informazioni, vedere l' [articolo relativo a Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="a03b6-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="a03b6-111">[Condivisione risorse tra le origini](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="a03b6-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="a03b6-112">È uno standard W3C che consente a un server di ridurre i criteri della stessa origine.</span><span class="sxs-lookup"><span data-stu-id="a03b6-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="a03b6-113">**Non** è una funzionalità di sicurezza, CORS rilassa la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a03b6-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="a03b6-114">Un'API non è più sicura consentendo CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="a03b6-115">Per ulteriori informazioni, vedere [funzionamento di CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="a03b6-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="a03b6-116">Consente a un server di consentire in modo esplicito alcune richieste tra origini rifiutando altre.</span><span class="sxs-lookup"><span data-stu-id="a03b6-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="a03b6-117">È più sicuro e flessibile rispetto alle tecniche precedenti, ad esempio [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="a03b6-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="a03b6-118">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a03b6-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="a03b6-119">Stessa origine</span><span class="sxs-lookup"><span data-stu-id="a03b6-119">Same origin</span></span>

<span data-ttu-id="a03b6-120">Due URL hanno la stessa origine se hanno schemi, host e porte identici ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="a03b6-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="a03b6-121">Questi due URL hanno la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="a03b6-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="a03b6-122">Questi URL hanno origini diverse da quelle dei due URL precedenti:</span><span class="sxs-lookup"><span data-stu-id="a03b6-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="a03b6-123">`https://example.net` &ndash; dominio diverso</span><span class="sxs-lookup"><span data-stu-id="a03b6-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="a03b6-124">`https://www.example.com/foo.html` &ndash; sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="a03b6-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="a03b6-125">`http://example.com/foo.html` &ndash; schema diverso</span><span class="sxs-lookup"><span data-stu-id="a03b6-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="a03b6-126">`https://example.com:9000/foo.html` &ndash; porta diversa</span><span class="sxs-lookup"><span data-stu-id="a03b6-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="a03b6-127">Internet Explorer non considera la porta durante il confronto delle origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="a03b6-128">CORS con criteri e middleware denominati</span><span class="sxs-lookup"><span data-stu-id="a03b6-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="a03b6-129">Il middleware CORS gestisce le richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="a03b6-130">Il codice seguente Abilita CORS per l'intera app con l'origine specificata:</span><span class="sxs-lookup"><span data-stu-id="a03b6-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="a03b6-131">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="a03b6-131">The preceding code:</span></span>

* <span data-ttu-id="a03b6-132">Imposta il nome del criterio su "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="a03b6-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="a03b6-133">Il nome del criterio è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="a03b6-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="a03b6-134">Chiama il metodo di estensione <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, che Abilita CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="a03b6-135">Chiama <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con un' [espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="a03b6-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="a03b6-136">L'espressione lambda accetta un oggetto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a03b6-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="a03b6-137">Le [Opzioni di configurazione](#cors-policy-options), ad esempio `WithOrigins`, sono descritte più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="a03b6-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="a03b6-138">La chiamata al metodo <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> aggiunge i servizi CORS al contenitore del servizio dell'app:</span><span class="sxs-lookup"><span data-stu-id="a03b6-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="a03b6-139">Per ulteriori informazioni, vedere [Opzioni dei criteri CORS](#cpo) in questo documento.</span><span class="sxs-lookup"><span data-stu-id="a03b6-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="a03b6-140">Il metodo <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> può concatenare i metodi, come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a03b6-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="a03b6-141">Nota: l'URL **non** deve contenere una barra finale (`/`).</span><span class="sxs-lookup"><span data-stu-id="a03b6-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="a03b6-142">Se l'URL termina con `/`, il confronto restituisce `false` e non viene restituita alcuna intestazione.</span><span class="sxs-lookup"><span data-stu-id="a03b6-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="a03b6-143">Applicare i criteri di CORS a tutti gli endpoint</span><span class="sxs-lookup"><span data-stu-id="a03b6-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="a03b6-144">Il codice seguente applica i criteri di CORS a tutti gli endpoint di app tramite il middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="a03b6-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code omitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code omitted.
}
```

> [!WARNING]
> <span data-ttu-id="a03b6-145">Con il routing dell'endpoint è necessario configurare il middleware CORS per l'esecuzione tra le chiamate a `UseRouting` e `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="a03b6-146">Una configurazione non corretta causerà il corretto funzionamento del middleware.</span><span class="sxs-lookup"><span data-stu-id="a03b6-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

<span data-ttu-id="a03b6-147">Per applicare i criteri di CORS a livello di pagina/controller/azione [, vedere Enable CORS in Razor Pages, Controllers e Action Methods](#ecors) .</span><span class="sxs-lookup"><span data-stu-id="a03b6-147">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="a03b6-148">Vedere [test CORS](#test) per istruzioni sul test del codice precedente.</span><span class="sxs-lookup"><span data-stu-id="a03b6-148">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="a03b6-149">Abilitare cors con routing degli endpoint</span><span class="sxs-lookup"><span data-stu-id="a03b6-149">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="a03b6-150">Con il routing degli endpoint, è possibile abilitare CORS in base all'endpoint usando il set `RequireCors` di metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="a03b6-150">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="a03b6-151">Analogamente, è anche possibile abilitare CORS per tutti i controller:</span><span class="sxs-lookup"><span data-stu-id="a03b6-151">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="a03b6-152">Abilita CORS con attributi</span><span class="sxs-lookup"><span data-stu-id="a03b6-152">Enable CORS with attributes</span></span>

<span data-ttu-id="a03b6-153">L'attributo [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fornisce un'alternativa all'applicazione di CORS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="a03b6-153">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="a03b6-154">L'attributo `[EnableCors]` Abilita CORS per i punti finali selezionati, anziché tutti i punti finali.</span><span class="sxs-lookup"><span data-stu-id="a03b6-154">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="a03b6-155">Utilizzare `[EnableCors]` per specificare i criteri predefiniti e `[EnableCors("{Policy String}")]` per specificare un criterio.</span><span class="sxs-lookup"><span data-stu-id="a03b6-155">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="a03b6-156">L'attributo `[EnableCors]` può essere applicato a:</span><span class="sxs-lookup"><span data-stu-id="a03b6-156">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="a03b6-157">`PageModel` pagina Razor</span><span class="sxs-lookup"><span data-stu-id="a03b6-157">Razor Page `PageModel`</span></span>
* <span data-ttu-id="a03b6-158">Controller</span><span class="sxs-lookup"><span data-stu-id="a03b6-158">Controller</span></span>
* <span data-ttu-id="a03b6-159">Metodo di azione del controller</span><span class="sxs-lookup"><span data-stu-id="a03b6-159">Controller action method</span></span>

<span data-ttu-id="a03b6-160">È possibile applicare criteri diversi a controller/pagina-modello/azione con l'attributo `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-160">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="a03b6-161">Quando l'attributo `[EnableCors]` viene applicato a controller/modello di pagina/Metodo di azione e CORS è abilitato nel middleware, vengono applicati entrambi i criteri.</span><span class="sxs-lookup"><span data-stu-id="a03b6-161">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="a03b6-162">È consigliabile evitare di combinare i criteri.</span><span class="sxs-lookup"><span data-stu-id="a03b6-162">We recommend against combining policies.</span></span> <span data-ttu-id="a03b6-163">Usare il middleware o l'attributo `[EnableCors]`, non entrambi nella stessa app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-163">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="a03b6-164">Il codice seguente applica un criterio diverso a ogni metodo:</span><span class="sxs-lookup"><span data-stu-id="a03b6-164">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="a03b6-165">Il codice seguente crea un criterio predefinito CORS e un criterio denominato `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="a03b6-165">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="a03b6-166">Disabilitare CORS</span><span class="sxs-lookup"><span data-stu-id="a03b6-166">Disable CORS</span></span>

<span data-ttu-id="a03b6-167">L'attributo [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) Disabilita CORS per il controller, il modello di pagina o l'azione.</span><span class="sxs-lookup"><span data-stu-id="a03b6-167">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="a03b6-168">Opzioni dei criteri di CORS</span><span class="sxs-lookup"><span data-stu-id="a03b6-168">CORS policy options</span></span>

<span data-ttu-id="a03b6-169">In questa sezione vengono descritte le varie opzioni che è possibile impostare in un criterio CORS:</span><span class="sxs-lookup"><span data-stu-id="a03b6-169">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="a03b6-170">Imposta le origini consentite</span><span class="sxs-lookup"><span data-stu-id="a03b6-170">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="a03b6-171">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="a03b6-171">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="a03b6-172">Impostare le intestazioni della richiesta consentita</span><span class="sxs-lookup"><span data-stu-id="a03b6-172">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="a03b6-173">Impostare le intestazioni di risposta esposte</span><span class="sxs-lookup"><span data-stu-id="a03b6-173">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="a03b6-174">Credenziali nelle richieste tra le origini</span><span class="sxs-lookup"><span data-stu-id="a03b6-174">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="a03b6-175">Imposta la data di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="a03b6-175">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="a03b6-176"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> viene chiamato `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-176"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a03b6-177">Per alcune opzioni, può essere utile leggere prima la sezione [come funziona CORS](#how-cors) .</span><span class="sxs-lookup"><span data-stu-id="a03b6-177">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="a03b6-178">Imposta le origini consentite</span><span class="sxs-lookup"><span data-stu-id="a03b6-178">Set the allowed origins</span></span>

<span data-ttu-id="a03b6-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; consente richieste CORS da tutte le origini con qualsiasi schema (`http` o `https`).</span><span class="sxs-lookup"><span data-stu-id="a03b6-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="a03b6-180">`AllowAnyOrigin` non è sicuro perché *qualsiasi sito Web* può effettuare richieste tra origini per l'app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-180">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="a03b6-181">La specifica di `AllowAnyOrigin` e `AllowCredentials` è una configurazione non sicura e può comportare la falsa richiesta tra siti.</span><span class="sxs-lookup"><span data-stu-id="a03b6-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="a03b6-182">Il servizio CORS restituisce una risposta CORS non valida quando un'app è configurata con entrambi i metodi.</span><span class="sxs-lookup"><span data-stu-id="a03b6-182">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

<span data-ttu-id="a03b6-183">`AllowAnyOrigin` influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-183">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="a03b6-184">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a03b6-184">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="a03b6-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; imposta la proprietà <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> del criterio in modo che sia una funzione che consente alle origini di corrispondere a un dominio con caratteri jolly configurato durante la valutazione se l'origine è consentita.</span><span class="sxs-lookup"><span data-stu-id="a03b6-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="a03b6-186">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="a03b6-186">Set the allowed HTTP methods</span></span>

<span data-ttu-id="a03b6-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="a03b6-188">Consente qualsiasi metodo HTTP:</span><span class="sxs-lookup"><span data-stu-id="a03b6-188">Allows any HTTP method:</span></span>
* <span data-ttu-id="a03b6-189">Influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-189">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="a03b6-190">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a03b6-190">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="a03b6-191">Impostare le intestazioni della richiesta consentita</span><span class="sxs-lookup"><span data-stu-id="a03b6-191">Set the allowed request headers</span></span>

<span data-ttu-id="a03b6-192">Per consentire l'invio di intestazioni specifiche in una richiesta CORS, denominate *intestazioni richiesta autore*, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e specificare le intestazioni consentite:</span><span class="sxs-lookup"><span data-stu-id="a03b6-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="a03b6-193">Per consentire tutte le intestazioni di richiesta di autore, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="a03b6-194">Questa impostazione influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-194">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="a03b6-195">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a03b6-195">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>


<span data-ttu-id="a03b6-196">Un criterio middleware CORS corrisponde a intestazioni specifiche specificate da `WithHeaders` è possibile solo quando le intestazioni inviate in `Access-Control-Request-Headers` corrispondono esattamente alle intestazioni indicate in `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-196">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="a03b6-197">Si consideri, ad esempio, un'app configurata come segue:</span><span class="sxs-lookup"><span data-stu-id="a03b6-197">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="a03b6-198">Il middleware CORS rifiuta una richiesta preliminare con l'intestazione della richiesta seguente, perché `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) non è elencato in `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="a03b6-198">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="a03b6-199">L'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-199">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="a03b6-200">Pertanto, il browser non tenta di eseguire la richiesta tra origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-200">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="a03b6-201">Impostare le intestazioni di risposta esposte</span><span class="sxs-lookup"><span data-stu-id="a03b6-201">Set the exposed response headers</span></span>

<span data-ttu-id="a03b6-202">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-202">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="a03b6-203">Per altre informazioni, vedere la pagina relativa alla [condivisione delle risorse tra le origini W3C (terminologia): intestazione della risposta semplice](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="a03b6-203">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="a03b6-204">Le intestazioni di risposta disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="a03b6-204">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="a03b6-205">La specifica CORS chiama queste intestazioni di *risposta semplici*.</span><span class="sxs-lookup"><span data-stu-id="a03b6-205">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="a03b6-206">Per rendere disponibili altre intestazioni per l'app, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-206">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="a03b6-207">Credenziali nelle richieste tra le origini</span><span class="sxs-lookup"><span data-stu-id="a03b6-207">Credentials in cross-origin requests</span></span>

<span data-ttu-id="a03b6-208">Le credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-208">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="a03b6-209">Per impostazione predefinita, il browser non invia le credenziali con una richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-209">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="a03b6-210">Le credenziali includono i cookie e gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="a03b6-210">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="a03b6-211">Per inviare le credenziali con una richiesta tra le origini, il client deve impostare `XMLHttpRequest.withCredentials` su `true`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-211">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="a03b6-212">Utilizzando direttamente `XMLHttpRequest`:</span><span class="sxs-lookup"><span data-stu-id="a03b6-212">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="a03b6-213">Uso di jQuery:</span><span class="sxs-lookup"><span data-stu-id="a03b6-213">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="a03b6-214">Uso dell' [API fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="a03b6-214">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="a03b6-215">Il server deve consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="a03b6-215">The server must allow the credentials.</span></span> <span data-ttu-id="a03b6-216">Per consentire le credenziali tra le origini, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-216">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="a03b6-217">La risposta HTTP include un'intestazione `Access-Control-Allow-Credentials`, che indica al browser che il server consente le credenziali per una richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-217">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="a03b6-218">Se il browser invia le credenziali, ma la risposta non include un'intestazione `Access-Control-Allow-Credentials` valida, il browser non espone la risposta all'app e la richiesta tra le origini ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="a03b6-218">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="a03b6-219">Consentire le credenziali tra le origini costituisce un rischio per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a03b6-219">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="a03b6-220">Un sito Web in un altro dominio può inviare le credenziali dell'utente che ha eseguito l'accesso all'app per conto dell'utente senza la conoscenza dell'utente.</span><span class="sxs-lookup"><span data-stu-id="a03b6-220">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="a03b6-221">La specifica CORS indica inoltre che l'impostazione delle origini per `"*"` (tutte le origini) non è valida se è presente l'intestazione del `Access-Control-Allow-Credentials`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-221">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="a03b6-222">Richieste preliminari</span><span class="sxs-lookup"><span data-stu-id="a03b6-222">Preflight requests</span></span>

<span data-ttu-id="a03b6-223">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva prima di effettuare la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a03b6-223">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="a03b6-224">Questa richiesta è detta *richiesta preliminare*.</span><span class="sxs-lookup"><span data-stu-id="a03b6-224">This request is called a *preflight request*.</span></span> <span data-ttu-id="a03b6-225">Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a03b6-225">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="a03b6-226">Il metodo di richiesta è GET, HEAD o POST.</span><span class="sxs-lookup"><span data-stu-id="a03b6-226">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="a03b6-227">L'app non imposta intestazioni di richiesta diverse da `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`o `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-227">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="a03b6-228">L'intestazione `Content-Type`, se impostata, presenta uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="a03b6-228">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="a03b6-229">La regola sulle intestazioni di richiesta impostate per la richiesta del client si applica alle intestazioni impostate dall'app chiamando `setRequestHeader` sull'oggetto `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-229">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="a03b6-230">La specifica CORS chiama queste intestazioni di *richiesta autore*intestazioni.</span><span class="sxs-lookup"><span data-stu-id="a03b6-230">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="a03b6-231">La regola non si applica alle intestazioni che il browser può impostare, ad esempio `User-Agent`, `Host`o `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-231">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="a03b6-232">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="a03b6-232">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="a03b6-233">La richiesta pre-flight usa il metodo delle opzioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="a03b6-233">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="a03b6-234">Sono incluse due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="a03b6-234">It includes two special headers:</span></span>

* <span data-ttu-id="a03b6-235">`Access-Control-Request-Method`: il metodo HTTP che verrà usato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a03b6-235">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="a03b6-236">`Access-Control-Request-Headers`: elenco di intestazioni di richiesta impostate dall'app sulla richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a03b6-236">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="a03b6-237">Come indicato in precedenza, non include le intestazioni impostate dal browser, ad esempio `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-237">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="a03b6-238">Una richiesta preliminare CORS può includere un'intestazione `Access-Control-Request-Headers`, che indica al server le intestazioni inviate con la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a03b6-238">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="a03b6-239">Per consentire intestazioni specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-239">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="a03b6-240">Per consentire tutte le intestazioni di richiesta di autore, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-240">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="a03b6-241">I browser non sono completamente coerenti nell'impostazione `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-241">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="a03b6-242">Se si impostano le intestazioni su un valore diverso da `"*"` (o si utilizza <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), è necessario includere almeno `Accept`, `Content-Type`e `Origin`, oltre a eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="a03b6-242">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="a03b6-243">Di seguito è riportato un esempio di risposta alla richiesta preliminare (presupponendo che il server consenta la richiesta):</span><span class="sxs-lookup"><span data-stu-id="a03b6-243">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="a03b6-244">La risposta include un'intestazione `Access-Control-Allow-Methods` che elenca i metodi consentiti e, facoltativamente, un'intestazione `Access-Control-Allow-Headers`, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="a03b6-244">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="a03b6-245">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a03b6-245">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="a03b6-246">Se la richiesta preliminare viene negata, l'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-246">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="a03b6-247">Pertanto, il browser non tenta di eseguire la richiesta tra origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-247">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="a03b6-248">Imposta la data di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="a03b6-248">Set the preflight expiration time</span></span>

<span data-ttu-id="a03b6-249">L'intestazione `Access-Control-Max-Age` specifica per quanto tempo la risposta alla richiesta preliminare può essere memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="a03b6-249">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="a03b6-250">Per impostare questa intestazione, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-250">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="a03b6-251">Funzionamento di CORS</span><span class="sxs-lookup"><span data-stu-id="a03b6-251">How CORS works</span></span>

<span data-ttu-id="a03b6-252">In questa sezione viene descritto cosa accade in una richiesta [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) al livello dei messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="a03b6-252">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="a03b6-253">CORS **non** è una funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a03b6-253">CORS is **not** a security feature.</span></span> <span data-ttu-id="a03b6-254">CORS è uno standard W3C che consente a un server di ridurre i criteri della stessa origine.</span><span class="sxs-lookup"><span data-stu-id="a03b6-254">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="a03b6-255">Ad esempio, un attore malintenzionato potrebbe usare [Impedisci lo scripting tra siti (XSS)](xref:security/cross-site-scripting) per il sito ed eseguire una richiesta tra siti al sito abilitato per CORS per rubare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="a03b6-255">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="a03b6-256">L'API non è più sicura consentendo CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-256">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="a03b6-257">Il client (browser) deve applicare CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-257">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="a03b6-258">Il server esegue la richiesta e restituisce la risposta, ovvero il client che restituisce un errore e blocca la risposta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-258">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="a03b6-259">Uno degli strumenti seguenti, ad esempio, visualizzerà la risposta del server:</span><span class="sxs-lookup"><span data-stu-id="a03b6-259">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="a03b6-260">Fiddler</span><span class="sxs-lookup"><span data-stu-id="a03b6-260">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="a03b6-261">Postman</span><span class="sxs-lookup"><span data-stu-id="a03b6-261">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="a03b6-262">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="a03b6-262">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="a03b6-263">Un Web browser immettendo l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="a03b6-263">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="a03b6-264">Si tratta di un modo per consentire a un server di consentire ai browser di eseguire una richiesta dell' [API di recupero](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) o di [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) tra le origini, che altrimenti sarebbe proibita.</span><span class="sxs-lookup"><span data-stu-id="a03b6-264">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="a03b6-265">I browser (senza CORS) non possono eseguire richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-265">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="a03b6-266">Prima di CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) è stato usato per aggirare questa restrizione.</span><span class="sxs-lookup"><span data-stu-id="a03b6-266">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="a03b6-267">JSONP non usa XHR, usa il tag `<script>` per ricevere la risposta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-267">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="a03b6-268">Gli script possono essere caricati tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-268">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="a03b6-269">La [specifica CORS](https://www.w3.org/TR/cors/) ha introdotto diverse nuove intestazioni HTTP che consentono richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-269">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="a03b6-270">Se un browser supporta CORS, queste intestazioni vengono impostate automaticamente per le richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-270">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="a03b6-271">Il codice JavaScript personalizzato non è necessario per abilitare CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-271">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="a03b6-272">Di seguito è riportato un esempio di richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-272">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="a03b6-273">L'intestazione `Origin` fornisce il dominio del sito che sta effettuando la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-273">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="a03b6-274">L'intestazione `Origin` è obbligatoria e deve essere diversa dall'host.</span><span class="sxs-lookup"><span data-stu-id="a03b6-274">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="a03b6-275">Se il server consente la richiesta, imposta l'intestazione `Access-Control-Allow-Origin` nella risposta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-275">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="a03b6-276">Il valore di questa intestazione corrisponde all'intestazione `Origin` dalla richiesta o è il valore del carattere jolly `"*"`, che indica che è consentita un'origine:</span><span class="sxs-lookup"><span data-stu-id="a03b6-276">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="a03b6-277">Se la risposta non include l'intestazione `Access-Control-Allow-Origin`, la richiesta tra le origini ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="a03b6-277">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="a03b6-278">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-278">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="a03b6-279">Anche se il server restituisce una risposta con esito positivo, il browser non rende disponibile la risposta all'app client.</span><span class="sxs-lookup"><span data-stu-id="a03b6-279">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="a03b6-280">Testare CORS</span><span class="sxs-lookup"><span data-stu-id="a03b6-280">Test CORS</span></span>

<span data-ttu-id="a03b6-281">Per testare CORS:</span><span class="sxs-lookup"><span data-stu-id="a03b6-281">To test CORS:</span></span>

1. <span data-ttu-id="a03b6-282">[Creare un progetto API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="a03b6-282">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="a03b6-283">In alternativa, è possibile [scaricare l'esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="a03b6-283">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="a03b6-284">Abilitare CORS usando uno degli approcci descritti in questo documento.</span><span class="sxs-lookup"><span data-stu-id="a03b6-284">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="a03b6-285">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="a03b6-285">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="a03b6-286">`WithOrigins("https://localhost:<port>");` deve essere usato solo per il test di un'app di esempio simile al [codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)per il download.</span><span class="sxs-lookup"><span data-stu-id="a03b6-286">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="a03b6-287">Creare un progetto di app Web (Razor Pages o MVC).</span><span class="sxs-lookup"><span data-stu-id="a03b6-287">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="a03b6-288">Nell'esempio viene utilizzato Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a03b6-288">The sample uses Razor Pages.</span></span> <span data-ttu-id="a03b6-289">È possibile creare l'app Web nella stessa soluzione del progetto API.</span><span class="sxs-lookup"><span data-stu-id="a03b6-289">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="a03b6-290">Aggiungere il codice evidenziato seguente al file *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a03b6-290">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="a03b6-291">Nel codice precedente sostituire `url: 'https://<web app>.azurewebsites.net/api/values/1',` con l'URL dell'app distribuita.</span><span class="sxs-lookup"><span data-stu-id="a03b6-291">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="a03b6-292">Distribuire il progetto API.</span><span class="sxs-lookup"><span data-stu-id="a03b6-292">Deploy the API project.</span></span> <span data-ttu-id="a03b6-293">Ad esempio, [eseguire la distribuzione in Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="a03b6-293">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="a03b6-294">Eseguire l'app Razor Pages o MVC dal desktop e fare clic sul pulsante **test** .</span><span class="sxs-lookup"><span data-stu-id="a03b6-294">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="a03b6-295">Usare gli strumenti F12 per esaminare i messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="a03b6-295">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="a03b6-296">Rimuovere l'origine localhost da `WithOrigins` e distribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-296">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="a03b6-297">In alternativa, eseguire l'app client con una porta diversa.</span><span class="sxs-lookup"><span data-stu-id="a03b6-297">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="a03b6-298">Ad esempio, eseguire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a03b6-298">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="a03b6-299">Eseguire il test con l'app client.</span><span class="sxs-lookup"><span data-stu-id="a03b6-299">Test with the client app.</span></span> <span data-ttu-id="a03b6-300">Gli errori CORS restituiscono un errore, ma il messaggio di errore non è disponibile per JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a03b6-300">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="a03b6-301">Usare la scheda console negli strumenti F12 per visualizzare l'errore.</span><span class="sxs-lookup"><span data-stu-id="a03b6-301">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="a03b6-302">A seconda del browser, viene ricevuto un errore (nella console strumenti F12) simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a03b6-302">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="a03b6-303">Uso di Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="a03b6-303">Using Microsoft Edge:</span></span>

     <span data-ttu-id="a03b6-304">**SEC7120: [CORS] la `https://localhost:44375` di origine non ha trovato `https://localhost:44375` nell'intestazione della risposta Access-Control-Allow-Origin per la risorsa tra le origini in `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="a03b6-304">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="a03b6-305">Uso di Chrome:</span><span class="sxs-lookup"><span data-stu-id="a03b6-305">Using Chrome:</span></span>

     <span data-ttu-id="a03b6-306">**L'accesso a XMLHttpRequest in `https://webapi.azurewebsites.net/api/values/1` dalla `https://localhost:44375` di origine è stato bloccato dal criterio CORS: non è presente alcuna intestazione ' Access-Control-Allow-Origin ' nella risorsa richiesta.**</span><span class="sxs-lookup"><span data-stu-id="a03b6-306">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="a03b6-307">Gli endpoint abilitati per CORS possono essere testati con uno strumento, ad esempio [Fiddler](https://www.telerik.com/fiddler) o [postazione](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="a03b6-307">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="a03b6-308">Quando si utilizza uno strumento, l'origine della richiesta specificata dall'intestazione `Origin` deve essere diversa dall'host che riceve la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-308">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="a03b6-309">Se la richiesta non è *tra origini* basate sul valore dell'intestazione `Origin`:</span><span class="sxs-lookup"><span data-stu-id="a03b6-309">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="a03b6-310">Non è necessario che il middleware CORS elabori la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-310">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="a03b6-311">Le intestazioni CORS non vengono restituite nella risposta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-311">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="a03b6-312">CORS in IIS</span><span class="sxs-lookup"><span data-stu-id="a03b6-312">CORS in IIS</span></span>

<span data-ttu-id="a03b6-313">Quando si esegue la distribuzione in IIS, CORS deve essere eseguito prima dell'autenticazione di Windows se il server non è configurato per consentire l'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="a03b6-313">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="a03b6-314">Per supportare questo scenario, è necessario installare e configurare il [modulo cors di IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) per l'app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-314">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a03b6-315">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a03b6-315">Additional resources</span></span>

* [<span data-ttu-id="a03b6-316">Condivisione di risorse tra le origini (CORS)</span><span class="sxs-lookup"><span data-stu-id="a03b6-316">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="a03b6-317">Introduzione al modulo CORS di IIS</span><span class="sxs-lookup"><span data-stu-id="a03b6-317">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a03b6-318">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a03b6-318">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a03b6-319">Questo articolo illustra come abilitare CORS in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a03b6-319">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="a03b6-320">La sicurezza del browser impedisce a una pagina Web di eseguire richieste a un dominio diverso da quello che ha gestito la pagina Web.</span><span class="sxs-lookup"><span data-stu-id="a03b6-320">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="a03b6-321">Questa restrizione è detta *criterio della stessa origine*.</span><span class="sxs-lookup"><span data-stu-id="a03b6-321">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="a03b6-322">Il criterio della stessa origine impedisce a un sito dannoso di leggere dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="a03b6-322">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="a03b6-323">In alcuni casi potrebbe essere necessario consentire ad altri siti di effettuare richieste tra le origini all'app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-323">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="a03b6-324">Per altre informazioni, vedere l' [articolo relativo a Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="a03b6-324">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="a03b6-325">[Condivisione risorse tra le origini](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="a03b6-325">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="a03b6-326">È uno standard W3C che consente a un server di ridurre i criteri della stessa origine.</span><span class="sxs-lookup"><span data-stu-id="a03b6-326">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="a03b6-327">**Non** è una funzionalità di sicurezza, CORS rilassa la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a03b6-327">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="a03b6-328">Un'API non è più sicura consentendo CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-328">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="a03b6-329">Per ulteriori informazioni, vedere [funzionamento di CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="a03b6-329">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="a03b6-330">Consente a un server di consentire in modo esplicito alcune richieste tra origini rifiutando altre.</span><span class="sxs-lookup"><span data-stu-id="a03b6-330">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="a03b6-331">È più sicuro e flessibile rispetto alle tecniche precedenti, ad esempio [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="a03b6-331">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="a03b6-332">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a03b6-332">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="a03b6-333">Stessa origine</span><span class="sxs-lookup"><span data-stu-id="a03b6-333">Same origin</span></span>

<span data-ttu-id="a03b6-334">Due URL hanno la stessa origine se hanno schemi, host e porte identici ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="a03b6-334">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="a03b6-335">Questi due URL hanno la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="a03b6-335">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="a03b6-336">Questi URL hanno origini diverse da quelle dei due URL precedenti:</span><span class="sxs-lookup"><span data-stu-id="a03b6-336">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="a03b6-337">`https://example.net` &ndash; dominio diverso</span><span class="sxs-lookup"><span data-stu-id="a03b6-337">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="a03b6-338">`https://www.example.com/foo.html` &ndash; sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="a03b6-338">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="a03b6-339">`http://example.com/foo.html` &ndash; schema diverso</span><span class="sxs-lookup"><span data-stu-id="a03b6-339">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="a03b6-340">`https://example.com:9000/foo.html` &ndash; porta diversa</span><span class="sxs-lookup"><span data-stu-id="a03b6-340">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="a03b6-341">Internet Explorer non considera la porta durante il confronto delle origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-341">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="a03b6-342">CORS con criteri e middleware denominati</span><span class="sxs-lookup"><span data-stu-id="a03b6-342">CORS with named policy and middleware</span></span>

<span data-ttu-id="a03b6-343">Il middleware CORS gestisce le richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-343">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="a03b6-344">Il codice seguente Abilita CORS per l'intera app con l'origine specificata:</span><span class="sxs-lookup"><span data-stu-id="a03b6-344">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="a03b6-345">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="a03b6-345">The preceding code:</span></span>

* <span data-ttu-id="a03b6-346">Imposta il nome del criterio su "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="a03b6-346">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="a03b6-347">Il nome del criterio è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="a03b6-347">The policy name is arbitrary.</span></span>
* <span data-ttu-id="a03b6-348">Chiama il metodo di estensione <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, che Abilita CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-348">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="a03b6-349">Chiama <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con un' [espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="a03b6-349">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="a03b6-350">L'espressione lambda accetta un oggetto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a03b6-350">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="a03b6-351">Le [Opzioni di configurazione](#cors-policy-options), ad esempio `WithOrigins`, sono descritte più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="a03b6-351">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="a03b6-352">La chiamata al metodo <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> aggiunge i servizi CORS al contenitore del servizio dell'app:</span><span class="sxs-lookup"><span data-stu-id="a03b6-352">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="a03b6-353">Per ulteriori informazioni, vedere [Opzioni dei criteri CORS](#cpo) in questo documento.</span><span class="sxs-lookup"><span data-stu-id="a03b6-353">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="a03b6-354">Il metodo <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> può concatenare i metodi, come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a03b6-354">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="a03b6-355">Nota: l'URL **non** deve contenere una barra finale (`/`).</span><span class="sxs-lookup"><span data-stu-id="a03b6-355">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="a03b6-356">Se l'URL termina con `/`, il confronto restituisce `false` e non viene restituita alcuna intestazione.</span><span class="sxs-lookup"><span data-stu-id="a03b6-356">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="a03b6-357">Il codice seguente applica i criteri di CORS a tutti gli endpoint di app tramite il middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="a03b6-357">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="a03b6-358">Nota: è necessario chiamare `UseCors` prima di `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-358">Note: `UseCors` must be called before `UseMvc`.</span></span>

<span data-ttu-id="a03b6-359">Per applicare i criteri di CORS a livello di pagina/controller/azione [, vedere Enable CORS in Razor Pages, Controllers e Action Methods](#ecors) .</span><span class="sxs-lookup"><span data-stu-id="a03b6-359">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="a03b6-360">Vedere [test CORS](#test) per istruzioni sul test del codice precedente.</span><span class="sxs-lookup"><span data-stu-id="a03b6-360">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="a03b6-361">Abilita CORS con attributi</span><span class="sxs-lookup"><span data-stu-id="a03b6-361">Enable CORS with attributes</span></span>

<span data-ttu-id="a03b6-362">L'attributo [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fornisce un'alternativa all'applicazione di CORS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="a03b6-362">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="a03b6-363">L'attributo `[EnableCors]` Abilita CORS per i punti finali selezionati, anziché tutti i punti finali.</span><span class="sxs-lookup"><span data-stu-id="a03b6-363">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="a03b6-364">Utilizzare `[EnableCors]` per specificare i criteri predefiniti e `[EnableCors("{Policy String}")]` per specificare un criterio.</span><span class="sxs-lookup"><span data-stu-id="a03b6-364">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="a03b6-365">L'attributo `[EnableCors]` può essere applicato a:</span><span class="sxs-lookup"><span data-stu-id="a03b6-365">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="a03b6-366">`PageModel` pagina Razor</span><span class="sxs-lookup"><span data-stu-id="a03b6-366">Razor Page `PageModel`</span></span>
* <span data-ttu-id="a03b6-367">Controller</span><span class="sxs-lookup"><span data-stu-id="a03b6-367">Controller</span></span>
* <span data-ttu-id="a03b6-368">Metodo di azione del controller</span><span class="sxs-lookup"><span data-stu-id="a03b6-368">Controller action method</span></span>

<span data-ttu-id="a03b6-369">È possibile applicare criteri diversi a controller/pagina-modello/azione con l'attributo `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-369">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="a03b6-370">Quando l'attributo `[EnableCors]` viene applicato a controller/modello di pagina/Metodo di azione e CORS è abilitato nel middleware, vengono applicati entrambi i criteri.</span><span class="sxs-lookup"><span data-stu-id="a03b6-370">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="a03b6-371">È consigliabile evitare di combinare i criteri.</span><span class="sxs-lookup"><span data-stu-id="a03b6-371">We recommend against combining policies.</span></span> <span data-ttu-id="a03b6-372">Usare il middleware o l'attributo `[EnableCors]`, non entrambi nella stessa app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-372">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="a03b6-373">Il codice seguente applica un criterio diverso a ogni metodo:</span><span class="sxs-lookup"><span data-stu-id="a03b6-373">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="a03b6-374">Il codice seguente crea un criterio predefinito CORS e un criterio denominato `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="a03b6-374">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="a03b6-375">Disabilitare CORS</span><span class="sxs-lookup"><span data-stu-id="a03b6-375">Disable CORS</span></span>

<span data-ttu-id="a03b6-376">L'attributo [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) Disabilita CORS per il controller, il modello di pagina o l'azione.</span><span class="sxs-lookup"><span data-stu-id="a03b6-376">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="a03b6-377">Opzioni dei criteri di CORS</span><span class="sxs-lookup"><span data-stu-id="a03b6-377">CORS policy options</span></span>

<span data-ttu-id="a03b6-378">In questa sezione vengono descritte le varie opzioni che è possibile impostare in un criterio CORS:</span><span class="sxs-lookup"><span data-stu-id="a03b6-378">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="a03b6-379">Imposta le origini consentite</span><span class="sxs-lookup"><span data-stu-id="a03b6-379">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="a03b6-380">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="a03b6-380">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="a03b6-381">Impostare le intestazioni della richiesta consentita</span><span class="sxs-lookup"><span data-stu-id="a03b6-381">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="a03b6-382">Impostare le intestazioni di risposta esposte</span><span class="sxs-lookup"><span data-stu-id="a03b6-382">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="a03b6-383">Credenziali nelle richieste tra le origini</span><span class="sxs-lookup"><span data-stu-id="a03b6-383">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="a03b6-384">Imposta la data di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="a03b6-384">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="a03b6-385"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> viene chiamato `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-385"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a03b6-386">Per alcune opzioni, può essere utile leggere prima la sezione [come funziona CORS](#how-cors) .</span><span class="sxs-lookup"><span data-stu-id="a03b6-386">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="a03b6-387">Imposta le origini consentite</span><span class="sxs-lookup"><span data-stu-id="a03b6-387">Set the allowed origins</span></span>

<span data-ttu-id="a03b6-388"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; consente richieste CORS da tutte le origini con qualsiasi schema (`http` o `https`).</span><span class="sxs-lookup"><span data-stu-id="a03b6-388"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="a03b6-389">`AllowAnyOrigin` non è sicuro perché *qualsiasi sito Web* può effettuare richieste tra origini per l'app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-389">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="a03b6-390">La specifica di `AllowAnyOrigin` e `AllowCredentials` è una configurazione non sicura e può comportare la falsa richiesta tra siti.</span><span class="sxs-lookup"><span data-stu-id="a03b6-390">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="a03b6-391">Per un'app sicura, specificare un elenco esatto di origini se il client deve autorizzare se stesso ad accedere alle risorse del server.</span><span class="sxs-lookup"><span data-stu-id="a03b6-391">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

<span data-ttu-id="a03b6-392">`AllowAnyOrigin` influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-392">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="a03b6-393">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a03b6-393">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="a03b6-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; imposta la proprietà <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> del criterio in modo che sia una funzione che consente alle origini di corrispondere a un dominio con caratteri jolly configurato durante la valutazione se l'origine è consentita.</span><span class="sxs-lookup"><span data-stu-id="a03b6-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="a03b6-395">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="a03b6-395">Set the allowed HTTP methods</span></span>

<span data-ttu-id="a03b6-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="a03b6-397">Consente qualsiasi metodo HTTP:</span><span class="sxs-lookup"><span data-stu-id="a03b6-397">Allows any HTTP method:</span></span>
* <span data-ttu-id="a03b6-398">Influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-398">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="a03b6-399">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a03b6-399">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="a03b6-400">Impostare le intestazioni della richiesta consentita</span><span class="sxs-lookup"><span data-stu-id="a03b6-400">Set the allowed request headers</span></span>

<span data-ttu-id="a03b6-401">Per consentire l'invio di intestazioni specifiche in una richiesta CORS, denominate *intestazioni richiesta autore*, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e specificare le intestazioni consentite:</span><span class="sxs-lookup"><span data-stu-id="a03b6-401">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="a03b6-402">Per consentire tutte le intestazioni di richiesta di autore, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-402">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="a03b6-403">Questa impostazione influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-403">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="a03b6-404">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a03b6-404">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="a03b6-405">Il middleware CORS consente sempre di inviare quattro intestazioni nel `Access-Control-Request-Headers` indipendentemente dai valori configurati in CorsPolicy. Headers.</span><span class="sxs-lookup"><span data-stu-id="a03b6-405">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="a03b6-406">Questo elenco di intestazioni include:</span><span class="sxs-lookup"><span data-stu-id="a03b6-406">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="a03b6-407">Si consideri, ad esempio, un'app configurata come segue:</span><span class="sxs-lookup"><span data-stu-id="a03b6-407">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="a03b6-408">Il middleware CORS risponde correttamente a una richiesta preliminare con la seguente intestazione di richiesta perché `Content-Language` è sempre consentita:</span><span class="sxs-lookup"><span data-stu-id="a03b6-408">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="a03b6-409">Impostare le intestazioni di risposta esposte</span><span class="sxs-lookup"><span data-stu-id="a03b6-409">Set the exposed response headers</span></span>

<span data-ttu-id="a03b6-410">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-410">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="a03b6-411">Per altre informazioni, vedere la pagina relativa alla [condivisione delle risorse tra le origini W3C (terminologia): intestazione della risposta semplice](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="a03b6-411">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="a03b6-412">Le intestazioni di risposta disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="a03b6-412">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="a03b6-413">La specifica CORS chiama queste intestazioni di *risposta semplici*.</span><span class="sxs-lookup"><span data-stu-id="a03b6-413">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="a03b6-414">Per rendere disponibili altre intestazioni per l'app, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-414">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="a03b6-415">Credenziali nelle richieste tra le origini</span><span class="sxs-lookup"><span data-stu-id="a03b6-415">Credentials in cross-origin requests</span></span>

<span data-ttu-id="a03b6-416">Le credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-416">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="a03b6-417">Per impostazione predefinita, il browser non invia le credenziali con una richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-417">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="a03b6-418">Le credenziali includono i cookie e gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="a03b6-418">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="a03b6-419">Per inviare le credenziali con una richiesta tra le origini, il client deve impostare `XMLHttpRequest.withCredentials` su `true`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-419">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="a03b6-420">Utilizzando direttamente `XMLHttpRequest`:</span><span class="sxs-lookup"><span data-stu-id="a03b6-420">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="a03b6-421">Uso di jQuery:</span><span class="sxs-lookup"><span data-stu-id="a03b6-421">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="a03b6-422">Uso dell' [API fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="a03b6-422">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="a03b6-423">Il server deve consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="a03b6-423">The server must allow the credentials.</span></span> <span data-ttu-id="a03b6-424">Per consentire le credenziali tra le origini, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-424">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="a03b6-425">La risposta HTTP include un'intestazione `Access-Control-Allow-Credentials`, che indica al browser che il server consente le credenziali per una richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-425">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="a03b6-426">Se il browser invia le credenziali, ma la risposta non include un'intestazione `Access-Control-Allow-Credentials` valida, il browser non espone la risposta all'app e la richiesta tra le origini ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="a03b6-426">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="a03b6-427">Consentire le credenziali tra le origini costituisce un rischio per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a03b6-427">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="a03b6-428">Un sito Web in un altro dominio può inviare le credenziali dell'utente che ha eseguito l'accesso all'app per conto dell'utente senza la conoscenza dell'utente.</span><span class="sxs-lookup"><span data-stu-id="a03b6-428">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="a03b6-429">La specifica CORS indica inoltre che l'impostazione delle origini per `"*"` (tutte le origini) non è valida se è presente l'intestazione del `Access-Control-Allow-Credentials`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-429">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="a03b6-430">Richieste preliminari</span><span class="sxs-lookup"><span data-stu-id="a03b6-430">Preflight requests</span></span>

<span data-ttu-id="a03b6-431">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva prima di effettuare la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a03b6-431">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="a03b6-432">Questa richiesta è detta *richiesta preliminare*.</span><span class="sxs-lookup"><span data-stu-id="a03b6-432">This request is called a *preflight request*.</span></span> <span data-ttu-id="a03b6-433">Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a03b6-433">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="a03b6-434">Il metodo di richiesta è GET, HEAD o POST.</span><span class="sxs-lookup"><span data-stu-id="a03b6-434">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="a03b6-435">L'app non imposta intestazioni di richiesta diverse da `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`o `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-435">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="a03b6-436">L'intestazione `Content-Type`, se impostata, presenta uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="a03b6-436">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="a03b6-437">La regola sulle intestazioni di richiesta impostate per la richiesta del client si applica alle intestazioni impostate dall'app chiamando `setRequestHeader` sull'oggetto `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-437">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="a03b6-438">La specifica CORS chiama queste intestazioni di *richiesta autore*intestazioni.</span><span class="sxs-lookup"><span data-stu-id="a03b6-438">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="a03b6-439">La regola non si applica alle intestazioni che il browser può impostare, ad esempio `User-Agent`, `Host`o `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-439">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="a03b6-440">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="a03b6-440">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="a03b6-441">La richiesta pre-flight usa il metodo delle opzioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="a03b6-441">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="a03b6-442">Sono incluse due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="a03b6-442">It includes two special headers:</span></span>

* <span data-ttu-id="a03b6-443">`Access-Control-Request-Method`: il metodo HTTP che verrà usato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a03b6-443">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="a03b6-444">`Access-Control-Request-Headers`: elenco di intestazioni di richiesta impostate dall'app sulla richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a03b6-444">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="a03b6-445">Come indicato in precedenza, non include le intestazioni impostate dal browser, ad esempio `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-445">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="a03b6-446">Una richiesta preliminare CORS può includere un'intestazione `Access-Control-Request-Headers`, che indica al server le intestazioni inviate con la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a03b6-446">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="a03b6-447">Per consentire intestazioni specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-447">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="a03b6-448">Per consentire tutte le intestazioni di richiesta di autore, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-448">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="a03b6-449">I browser non sono completamente coerenti nell'impostazione `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="a03b6-449">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="a03b6-450">Se si impostano le intestazioni su un valore diverso da `"*"` (o si utilizza <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), è necessario includere almeno `Accept`, `Content-Type`e `Origin`, oltre a eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="a03b6-450">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="a03b6-451">Di seguito è riportato un esempio di risposta alla richiesta preliminare (presupponendo che il server consenta la richiesta):</span><span class="sxs-lookup"><span data-stu-id="a03b6-451">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="a03b6-452">La risposta include un'intestazione `Access-Control-Allow-Methods` che elenca i metodi consentiti e, facoltativamente, un'intestazione `Access-Control-Allow-Headers`, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="a03b6-452">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="a03b6-453">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a03b6-453">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="a03b6-454">Se la richiesta preliminare viene negata, l'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-454">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="a03b6-455">Pertanto, il browser non tenta di eseguire la richiesta tra origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-455">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="a03b6-456">Imposta la data di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="a03b6-456">Set the preflight expiration time</span></span>

<span data-ttu-id="a03b6-457">L'intestazione `Access-Control-Max-Age` specifica per quanto tempo la risposta alla richiesta preliminare può essere memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="a03b6-457">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="a03b6-458">Per impostare questa intestazione, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="a03b6-458">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="a03b6-459">Funzionamento di CORS</span><span class="sxs-lookup"><span data-stu-id="a03b6-459">How CORS works</span></span>

<span data-ttu-id="a03b6-460">In questa sezione viene descritto cosa accade in una richiesta [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) al livello dei messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="a03b6-460">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="a03b6-461">CORS **non** è una funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a03b6-461">CORS is **not** a security feature.</span></span> <span data-ttu-id="a03b6-462">CORS è uno standard W3C che consente a un server di ridurre i criteri della stessa origine.</span><span class="sxs-lookup"><span data-stu-id="a03b6-462">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="a03b6-463">Ad esempio, un attore malintenzionato potrebbe usare [Impedisci lo scripting tra siti (XSS)](xref:security/cross-site-scripting) per il sito ed eseguire una richiesta tra siti al sito abilitato per CORS per rubare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="a03b6-463">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="a03b6-464">L'API non è più sicura consentendo CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-464">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="a03b6-465">Il client (browser) deve applicare CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-465">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="a03b6-466">Il server esegue la richiesta e restituisce la risposta, ovvero il client che restituisce un errore e blocca la risposta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-466">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="a03b6-467">Uno degli strumenti seguenti, ad esempio, visualizzerà la risposta del server:</span><span class="sxs-lookup"><span data-stu-id="a03b6-467">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="a03b6-468">Fiddler</span><span class="sxs-lookup"><span data-stu-id="a03b6-468">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="a03b6-469">Postman</span><span class="sxs-lookup"><span data-stu-id="a03b6-469">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="a03b6-470">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="a03b6-470">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="a03b6-471">Un Web browser immettendo l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="a03b6-471">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="a03b6-472">Si tratta di un modo per consentire a un server di consentire ai browser di eseguire una richiesta dell' [API di recupero](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) o di [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) tra le origini, che altrimenti sarebbe proibita.</span><span class="sxs-lookup"><span data-stu-id="a03b6-472">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="a03b6-473">I browser (senza CORS) non possono eseguire richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-473">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="a03b6-474">Prima di CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) è stato usato per aggirare questa restrizione.</span><span class="sxs-lookup"><span data-stu-id="a03b6-474">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="a03b6-475">JSONP non usa XHR, usa il tag `<script>` per ricevere la risposta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-475">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="a03b6-476">Gli script possono essere caricati tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-476">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="a03b6-477">La [specifica CORS](https://www.w3.org/TR/cors/) ha introdotto diverse nuove intestazioni HTTP che consentono richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-477">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="a03b6-478">Se un browser supporta CORS, queste intestazioni vengono impostate automaticamente per le richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-478">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="a03b6-479">Il codice JavaScript personalizzato non è necessario per abilitare CORS.</span><span class="sxs-lookup"><span data-stu-id="a03b6-479">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="a03b6-480">Di seguito è riportato un esempio di richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a03b6-480">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="a03b6-481">L'intestazione `Origin` fornisce il dominio del sito che sta effettuando la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-481">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="a03b6-482">L'intestazione `Origin` è obbligatoria e deve essere diversa dall'host.</span><span class="sxs-lookup"><span data-stu-id="a03b6-482">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="a03b6-483">Se il server consente la richiesta, imposta l'intestazione `Access-Control-Allow-Origin` nella risposta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-483">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="a03b6-484">Il valore di questa intestazione corrisponde all'intestazione `Origin` dalla richiesta o è il valore del carattere jolly `"*"`, che indica che è consentita un'origine:</span><span class="sxs-lookup"><span data-stu-id="a03b6-484">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="a03b6-485">Se la risposta non include l'intestazione `Access-Control-Allow-Origin`, la richiesta tra le origini ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="a03b6-485">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="a03b6-486">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-486">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="a03b6-487">Anche se il server restituisce una risposta con esito positivo, il browser non rende disponibile la risposta all'app client.</span><span class="sxs-lookup"><span data-stu-id="a03b6-487">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="a03b6-488">Testare CORS</span><span class="sxs-lookup"><span data-stu-id="a03b6-488">Test CORS</span></span>

<span data-ttu-id="a03b6-489">Per testare CORS:</span><span class="sxs-lookup"><span data-stu-id="a03b6-489">To test CORS:</span></span>

1. <span data-ttu-id="a03b6-490">[Creare un progetto API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="a03b6-490">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="a03b6-491">In alternativa, è possibile [scaricare l'esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="a03b6-491">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="a03b6-492">Abilitare CORS usando uno degli approcci descritti in questo documento.</span><span class="sxs-lookup"><span data-stu-id="a03b6-492">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="a03b6-493">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="a03b6-493">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="a03b6-494">`WithOrigins("https://localhost:<port>");` deve essere usato solo per il test di un'app di esempio simile al [codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)per il download.</span><span class="sxs-lookup"><span data-stu-id="a03b6-494">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="a03b6-495">Creare un progetto di app Web (Razor Pages o MVC).</span><span class="sxs-lookup"><span data-stu-id="a03b6-495">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="a03b6-496">Nell'esempio viene utilizzato Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a03b6-496">The sample uses Razor Pages.</span></span> <span data-ttu-id="a03b6-497">È possibile creare l'app Web nella stessa soluzione del progetto API.</span><span class="sxs-lookup"><span data-stu-id="a03b6-497">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="a03b6-498">Aggiungere il codice evidenziato seguente al file *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a03b6-498">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="a03b6-499">Nel codice precedente sostituire `url: 'https://<web app>.azurewebsites.net/api/values/1',` con l'URL dell'app distribuita.</span><span class="sxs-lookup"><span data-stu-id="a03b6-499">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="a03b6-500">Distribuire il progetto API.</span><span class="sxs-lookup"><span data-stu-id="a03b6-500">Deploy the API project.</span></span> <span data-ttu-id="a03b6-501">Ad esempio, [eseguire la distribuzione in Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="a03b6-501">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="a03b6-502">Eseguire l'app Razor Pages o MVC dal desktop e fare clic sul pulsante **test** .</span><span class="sxs-lookup"><span data-stu-id="a03b6-502">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="a03b6-503">Usare gli strumenti F12 per esaminare i messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="a03b6-503">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="a03b6-504">Rimuovere l'origine localhost da `WithOrigins` e distribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-504">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="a03b6-505">In alternativa, eseguire l'app client con una porta diversa.</span><span class="sxs-lookup"><span data-stu-id="a03b6-505">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="a03b6-506">Ad esempio, eseguire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a03b6-506">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="a03b6-507">Eseguire il test con l'app client.</span><span class="sxs-lookup"><span data-stu-id="a03b6-507">Test with the client app.</span></span> <span data-ttu-id="a03b6-508">Gli errori CORS restituiscono un errore, ma il messaggio di errore non è disponibile per JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a03b6-508">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="a03b6-509">Usare la scheda console negli strumenti F12 per visualizzare l'errore.</span><span class="sxs-lookup"><span data-stu-id="a03b6-509">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="a03b6-510">A seconda del browser, viene ricevuto un errore (nella console strumenti F12) simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a03b6-510">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="a03b6-511">Uso di Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="a03b6-511">Using Microsoft Edge:</span></span>

     <span data-ttu-id="a03b6-512">**SEC7120: [CORS] la `https://localhost:44375` di origine non ha trovato `https://localhost:44375` nell'intestazione della risposta Access-Control-Allow-Origin per la risorsa tra le origini in `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="a03b6-512">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="a03b6-513">Uso di Chrome:</span><span class="sxs-lookup"><span data-stu-id="a03b6-513">Using Chrome:</span></span>

     <span data-ttu-id="a03b6-514">**L'accesso a XMLHttpRequest in `https://webapi.azurewebsites.net/api/values/1` dalla `https://localhost:44375` di origine è stato bloccato dal criterio CORS: non è presente alcuna intestazione ' Access-Control-Allow-Origin ' nella risorsa richiesta.**</span><span class="sxs-lookup"><span data-stu-id="a03b6-514">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="a03b6-515">Gli endpoint abilitati per CORS possono essere testati con uno strumento, ad esempio [Fiddler](https://www.telerik.com/fiddler) o [postazione](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="a03b6-515">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="a03b6-516">Quando si utilizza uno strumento, l'origine della richiesta specificata dall'intestazione `Origin` deve essere diversa dall'host che riceve la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-516">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="a03b6-517">Se la richiesta non è *tra origini* basate sul valore dell'intestazione `Origin`:</span><span class="sxs-lookup"><span data-stu-id="a03b6-517">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="a03b6-518">Non è necessario che il middleware CORS elabori la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-518">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="a03b6-519">Le intestazioni CORS non vengono restituite nella risposta.</span><span class="sxs-lookup"><span data-stu-id="a03b6-519">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="a03b6-520">CORS in IIS</span><span class="sxs-lookup"><span data-stu-id="a03b6-520">CORS in IIS</span></span>

<span data-ttu-id="a03b6-521">Quando si esegue la distribuzione in IIS, CORS deve essere eseguito prima dell'autenticazione di Windows se il server non è configurato per consentire l'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="a03b6-521">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="a03b6-522">Per supportare questo scenario, è necessario installare e configurare il [modulo cors di IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) per l'app.</span><span class="sxs-lookup"><span data-stu-id="a03b6-522">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a03b6-523">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a03b6-523">Additional resources</span></span>

* [<span data-ttu-id="a03b6-524">Condivisione di risorse tra le origini (CORS)</span><span class="sxs-lookup"><span data-stu-id="a03b6-524">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="a03b6-525">Introduzione al modulo CORS di IIS</span><span class="sxs-lookup"><span data-stu-id="a03b6-525">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end