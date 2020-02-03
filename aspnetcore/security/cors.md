---
title: Abilitare le richieste tra le origini (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come CORS come standard per consentire o rifiutare le richieste tra origini in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 57098be73164c71d1b0d1fe2f3aee7ec41a32346
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727320"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="a9c62-103">Abilitare le richieste tra le origini (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9c62-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="a9c62-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a9c62-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a9c62-105">Questo articolo illustra come abilitare CORS in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a9c62-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="a9c62-106">La sicurezza del browser impedisce a una pagina Web di eseguire richieste a un dominio diverso da quello che ha gestito la pagina Web.</span><span class="sxs-lookup"><span data-stu-id="a9c62-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="a9c62-107">Questa restrizione è detta *criterio della stessa origine*.</span><span class="sxs-lookup"><span data-stu-id="a9c62-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="a9c62-108">Il criterio della stessa origine impedisce a un sito dannoso di leggere dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="a9c62-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="a9c62-109">In alcuni casi potrebbe essere necessario consentire ad altri siti di effettuare richieste tra le origini all'app.</span><span class="sxs-lookup"><span data-stu-id="a9c62-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="a9c62-110">Per altre informazioni, vedere l' [articolo relativo a Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="a9c62-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="a9c62-111">[Condivisione risorse tra le origini](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="a9c62-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="a9c62-112">È uno standard W3C che consente a un server di ridurre i criteri della stessa origine.</span><span class="sxs-lookup"><span data-stu-id="a9c62-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="a9c62-113">**Non** è una funzionalità di sicurezza, CORS rilassa la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a9c62-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="a9c62-114">Un'API non è più sicura consentendo CORS.</span><span class="sxs-lookup"><span data-stu-id="a9c62-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="a9c62-115">Per ulteriori informazioni, vedere [funzionamento di CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="a9c62-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="a9c62-116">Consente a un server di consentire in modo esplicito alcune richieste tra origini rifiutando altre.</span><span class="sxs-lookup"><span data-stu-id="a9c62-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="a9c62-117">È più sicuro e flessibile rispetto alle tecniche precedenti, ad esempio [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="a9c62-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="a9c62-118">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9c62-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="a9c62-119">Stessa origine</span><span class="sxs-lookup"><span data-stu-id="a9c62-119">Same origin</span></span>

<span data-ttu-id="a9c62-120">Due URL hanno la stessa origine se hanno schemi, host e porte identici ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="a9c62-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="a9c62-121">Questi due URL hanno la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="a9c62-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="a9c62-122">Questi URL hanno origini diverse da quelle dei due URL precedenti:</span><span class="sxs-lookup"><span data-stu-id="a9c62-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="a9c62-123">`https://example.net` &ndash; dominio diverso</span><span class="sxs-lookup"><span data-stu-id="a9c62-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="a9c62-124">`https://www.example.com/foo.html` &ndash; sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="a9c62-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="a9c62-125">`http://example.com/foo.html` &ndash; schema diverso</span><span class="sxs-lookup"><span data-stu-id="a9c62-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="a9c62-126">`https://example.com:9000/foo.html` &ndash; porta diversa</span><span class="sxs-lookup"><span data-stu-id="a9c62-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="a9c62-127">Internet Explorer non considera la porta durante il confronto delle origini.</span><span class="sxs-lookup"><span data-stu-id="a9c62-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="a9c62-128">CORS con criteri e middleware denominati</span><span class="sxs-lookup"><span data-stu-id="a9c62-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="a9c62-129">Il middleware CORS gestisce le richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a9c62-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="a9c62-130">Il codice seguente Abilita CORS per l'intera app con l'origine specificata:</span><span class="sxs-lookup"><span data-stu-id="a9c62-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="a9c62-131">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="a9c62-131">The preceding code:</span></span>

* <span data-ttu-id="a9c62-132">Imposta il nome del criterio su "\_myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="a9c62-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="a9c62-133">Il nome del criterio è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="a9c62-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="a9c62-134">Chiama il metodo di estensione <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, che Abilita CORS.</span><span class="sxs-lookup"><span data-stu-id="a9c62-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="a9c62-135">Chiama <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con un' [espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="a9c62-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="a9c62-136">L'espressione lambda accetta un oggetto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a9c62-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="a9c62-137">Le [Opzioni di configurazione](#cors-policy-options), ad esempio `WithOrigins`, sono descritte più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="a9c62-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="a9c62-138">La chiamata al metodo <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> aggiunge i servizi CORS al contenitore del servizio dell'app:</span><span class="sxs-lookup"><span data-stu-id="a9c62-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="a9c62-139">Per ulteriori informazioni, vedere [Opzioni dei criteri CORS](#cpo) in questo documento.</span><span class="sxs-lookup"><span data-stu-id="a9c62-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="a9c62-140">Il metodo <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> può concatenare i metodi, come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a9c62-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="a9c62-141">Nota: l'URL **non** deve contenere una barra finale (`/`).</span><span class="sxs-lookup"><span data-stu-id="a9c62-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="a9c62-142">Se l'URL termina con `/`, il confronto restituisce `false` e non viene restituita alcuna intestazione.</span><span class="sxs-lookup"><span data-stu-id="a9c62-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="a9c62-143">Applicare i criteri di CORS a tutti gli endpoint</span><span class="sxs-lookup"><span data-stu-id="a9c62-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="a9c62-144">Il codice seguente applica i criteri di CORS a tutti gli endpoint di app tramite il middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="a9c62-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> <span data-ttu-id="a9c62-145">Con il routing dell'endpoint è necessario configurare il middleware CORS per l'esecuzione tra le chiamate a `UseRouting` e `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="a9c62-146">Una configurazione non corretta causerà il corretto funzionamento del middleware.</span><span class="sxs-lookup"><span data-stu-id="a9c62-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="a9c62-147">Il codice seguente applica i criteri di CORS a tutti gli endpoint di app tramite il middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="a9c62-147">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="a9c62-148">Nota: è necessario chiamare `UseCors` prima di `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-148">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="a9c62-149">Per applicare i criteri di CORS a livello di pagina/controller/azione [, vedere Enable CORS in Razor Pages, Controllers e Action Methods](#ecors) .</span><span class="sxs-lookup"><span data-stu-id="a9c62-149">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="a9c62-150">Vedere [test CORS](#test) per istruzioni sul test del codice precedente.</span><span class="sxs-lookup"><span data-stu-id="a9c62-150">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="a9c62-151">Abilitare cors con routing degli endpoint</span><span class="sxs-lookup"><span data-stu-id="a9c62-151">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="a9c62-152">Con il routing degli endpoint, è possibile abilitare CORS in base all'endpoint usando il set `RequireCors` di metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="a9c62-152">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="a9c62-153">Analogamente, è anche possibile abilitare CORS per tutti i controller:</span><span class="sxs-lookup"><span data-stu-id="a9c62-153">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="a9c62-154">Abilita CORS con attributi</span><span class="sxs-lookup"><span data-stu-id="a9c62-154">Enable CORS with attributes</span></span>

<span data-ttu-id="a9c62-155">L'attributo [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fornisce un'alternativa all'applicazione di CORS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="a9c62-155">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="a9c62-156">L'attributo `[EnableCors]` Abilita CORS per i punti finali selezionati, anziché tutti i punti finali.</span><span class="sxs-lookup"><span data-stu-id="a9c62-156">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="a9c62-157">Utilizzare `[EnableCors]` per specificare i criteri predefiniti e `[EnableCors("{Policy String}")]` per specificare un criterio.</span><span class="sxs-lookup"><span data-stu-id="a9c62-157">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="a9c62-158">L'attributo `[EnableCors]` può essere applicato a:</span><span class="sxs-lookup"><span data-stu-id="a9c62-158">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="a9c62-159">`PageModel` pagina Razor</span><span class="sxs-lookup"><span data-stu-id="a9c62-159">Razor Page `PageModel`</span></span>
* <span data-ttu-id="a9c62-160">Controller</span><span class="sxs-lookup"><span data-stu-id="a9c62-160">Controller</span></span>
* <span data-ttu-id="a9c62-161">Metodo di azione del controller</span><span class="sxs-lookup"><span data-stu-id="a9c62-161">Controller action method</span></span>

<span data-ttu-id="a9c62-162">È possibile applicare criteri diversi a controller/pagina-modello/azione con l'attributo `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-162">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="a9c62-163">Quando l'attributo `[EnableCors]` viene applicato a controller/modello di pagina/Metodo di azione e CORS è abilitato nel middleware, vengono applicati entrambi i criteri.</span><span class="sxs-lookup"><span data-stu-id="a9c62-163">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="a9c62-164">È consigliabile evitare di combinare i criteri.</span><span class="sxs-lookup"><span data-stu-id="a9c62-164">We recommend against combining policies.</span></span> <span data-ttu-id="a9c62-165">Usare il middleware o l'attributo `[EnableCors]`, non entrambi nella stessa app.</span><span class="sxs-lookup"><span data-stu-id="a9c62-165">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="a9c62-166">Il codice seguente applica un criterio diverso a ogni metodo:</span><span class="sxs-lookup"><span data-stu-id="a9c62-166">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="a9c62-167">Il codice seguente crea un criterio predefinito CORS e un criterio denominato `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="a9c62-167">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="a9c62-168">Disabilitare CORS</span><span class="sxs-lookup"><span data-stu-id="a9c62-168">Disable CORS</span></span>

<span data-ttu-id="a9c62-169">L'attributo [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) Disabilita CORS per il controller, il modello di pagina o l'azione.</span><span class="sxs-lookup"><span data-stu-id="a9c62-169">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="a9c62-170">Opzioni dei criteri di CORS</span><span class="sxs-lookup"><span data-stu-id="a9c62-170">CORS policy options</span></span>

<span data-ttu-id="a9c62-171">In questa sezione vengono descritte le varie opzioni che è possibile impostare in un criterio CORS:</span><span class="sxs-lookup"><span data-stu-id="a9c62-171">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="a9c62-172">Imposta le origini consentite</span><span class="sxs-lookup"><span data-stu-id="a9c62-172">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="a9c62-173">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="a9c62-173">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="a9c62-174">Impostare le intestazioni della richiesta consentita</span><span class="sxs-lookup"><span data-stu-id="a9c62-174">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="a9c62-175">Impostare le intestazioni di risposta esposte</span><span class="sxs-lookup"><span data-stu-id="a9c62-175">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="a9c62-176">Credenziali nelle richieste tra le origini</span><span class="sxs-lookup"><span data-stu-id="a9c62-176">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="a9c62-177">Imposta la data di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="a9c62-177">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="a9c62-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> viene chiamato `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a9c62-179">Per alcune opzioni, può essere utile leggere prima la sezione [come funziona CORS](#how-cors) .</span><span class="sxs-lookup"><span data-stu-id="a9c62-179">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="a9c62-180">Imposta le origini consentite</span><span class="sxs-lookup"><span data-stu-id="a9c62-180">Set the allowed origins</span></span>

<span data-ttu-id="a9c62-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; consente richieste CORS da tutte le origini con qualsiasi schema (`http` o `https`).</span><span class="sxs-lookup"><span data-stu-id="a9c62-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="a9c62-182">`AllowAnyOrigin` non è sicuro perché *qualsiasi sito Web* può effettuare richieste tra origini per l'app.</span><span class="sxs-lookup"><span data-stu-id="a9c62-182">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="a9c62-183">La specifica di `AllowAnyOrigin` e `AllowCredentials` è una configurazione non sicura e può comportare la falsa richiesta tra siti.</span><span class="sxs-lookup"><span data-stu-id="a9c62-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="a9c62-184">Il servizio CORS restituisce una risposta CORS non valida quando un'app è configurata con entrambi i metodi.</span><span class="sxs-lookup"><span data-stu-id="a9c62-184">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="a9c62-185">La specifica di `AllowAnyOrigin` e `AllowCredentials` è una configurazione non sicura e può comportare la falsa richiesta tra siti.</span><span class="sxs-lookup"><span data-stu-id="a9c62-185">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="a9c62-186">Per un'app sicura, specificare un elenco esatto di origini se il client deve autorizzare se stesso ad accedere alle risorse del server.</span><span class="sxs-lookup"><span data-stu-id="a9c62-186">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="a9c62-187">`AllowAnyOrigin` influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-187">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="a9c62-188">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a9c62-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a9c62-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; imposta la proprietà <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> del criterio in modo che sia una funzione che consente alle origini di corrispondere a un dominio con caratteri jolly configurato durante la valutazione se l'origine è consentita.</span><span class="sxs-lookup"><span data-stu-id="a9c62-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="a9c62-190">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="a9c62-190">Set the allowed HTTP methods</span></span>

<span data-ttu-id="a9c62-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="a9c62-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="a9c62-192">Consente qualsiasi metodo HTTP:</span><span class="sxs-lookup"><span data-stu-id="a9c62-192">Allows any HTTP method:</span></span>
* <span data-ttu-id="a9c62-193">Influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-193">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="a9c62-194">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a9c62-194">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="a9c62-195">Impostare le intestazioni della richiesta consentita</span><span class="sxs-lookup"><span data-stu-id="a9c62-195">Set the allowed request headers</span></span>

<span data-ttu-id="a9c62-196">Per consentire l'invio di intestazioni specifiche in una richiesta CORS, denominate *intestazioni richiesta autore*, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e specificare le intestazioni consentite:</span><span class="sxs-lookup"><span data-stu-id="a9c62-196">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="a9c62-197">Per consentire tutte le intestazioni di richiesta di autore, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="a9c62-197">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="a9c62-198">Questa impostazione influiscono sulle richieste preliminari e sull'intestazione `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-198">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="a9c62-199">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a9c62-199">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a9c62-200">Un criterio middleware CORS corrisponde a intestazioni specifiche specificate da `WithHeaders` è possibile solo quando le intestazioni inviate in `Access-Control-Request-Headers` corrispondono esattamente alle intestazioni indicate in `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-200">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="a9c62-201">Si consideri, ad esempio, un'app configurata come segue:</span><span class="sxs-lookup"><span data-stu-id="a9c62-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="a9c62-202">Il middleware CORS rifiuta una richiesta preliminare con l'intestazione della richiesta seguente, perché `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) non è elencato in `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="a9c62-202">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="a9c62-203">L'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS.</span><span class="sxs-lookup"><span data-stu-id="a9c62-203">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="a9c62-204">Pertanto, il browser non tenta di eseguire la richiesta tra origini.</span><span class="sxs-lookup"><span data-stu-id="a9c62-204">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a9c62-205">Il middleware CORS consente sempre di inviare quattro intestazioni nel `Access-Control-Request-Headers` indipendentemente dai valori configurati in CorsPolicy. Headers.</span><span class="sxs-lookup"><span data-stu-id="a9c62-205">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="a9c62-206">Questo elenco di intestazioni include:</span><span class="sxs-lookup"><span data-stu-id="a9c62-206">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="a9c62-207">Si consideri, ad esempio, un'app configurata come segue:</span><span class="sxs-lookup"><span data-stu-id="a9c62-207">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="a9c62-208">Il middleware CORS risponde correttamente a una richiesta preliminare con la seguente intestazione di richiesta perché `Content-Language` è sempre consentita:</span><span class="sxs-lookup"><span data-stu-id="a9c62-208">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="a9c62-209">Impostare le intestazioni di risposta esposte</span><span class="sxs-lookup"><span data-stu-id="a9c62-209">Set the exposed response headers</span></span>

<span data-ttu-id="a9c62-210">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'app.</span><span class="sxs-lookup"><span data-stu-id="a9c62-210">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="a9c62-211">Per altre informazioni, vedere la pagina relativa alla [condivisione delle risorse tra le origini W3C (terminologia): intestazione della risposta semplice](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="a9c62-211">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="a9c62-212">Le intestazioni di risposta disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="a9c62-212">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="a9c62-213">La specifica CORS chiama queste intestazioni di *risposta semplici*.</span><span class="sxs-lookup"><span data-stu-id="a9c62-213">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="a9c62-214">Per rendere disponibili altre intestazioni per l'app, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="a9c62-214">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="a9c62-215">Credenziali nelle richieste tra le origini</span><span class="sxs-lookup"><span data-stu-id="a9c62-215">Credentials in cross-origin requests</span></span>

<span data-ttu-id="a9c62-216">Le credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="a9c62-216">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="a9c62-217">Per impostazione predefinita, il browser non invia le credenziali con una richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a9c62-217">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="a9c62-218">Le credenziali includono i cookie e gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="a9c62-218">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="a9c62-219">Per inviare le credenziali con una richiesta tra le origini, il client deve impostare `XMLHttpRequest.withCredentials` su `true`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-219">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="a9c62-220">Utilizzando direttamente `XMLHttpRequest`:</span><span class="sxs-lookup"><span data-stu-id="a9c62-220">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="a9c62-221">Uso di jQuery:</span><span class="sxs-lookup"><span data-stu-id="a9c62-221">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="a9c62-222">Uso dell' [API fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="a9c62-222">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="a9c62-223">Il server deve consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="a9c62-223">The server must allow the credentials.</span></span> <span data-ttu-id="a9c62-224">Per consentire le credenziali tra le origini, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="a9c62-224">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="a9c62-225">La risposta HTTP include un'intestazione `Access-Control-Allow-Credentials`, che indica al browser che il server consente le credenziali per una richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a9c62-225">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="a9c62-226">Se il browser invia le credenziali, ma la risposta non include un'intestazione `Access-Control-Allow-Credentials` valida, il browser non espone la risposta all'app e la richiesta tra le origini ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="a9c62-226">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="a9c62-227">Consentire le credenziali tra le origini costituisce un rischio per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a9c62-227">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="a9c62-228">Un sito Web in un altro dominio può inviare le credenziali dell'utente che ha eseguito l'accesso all'app per conto dell'utente senza la conoscenza dell'utente.</span><span class="sxs-lookup"><span data-stu-id="a9c62-228">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="a9c62-229">La specifica CORS indica inoltre che l'impostazione delle origini per `"*"` (tutte le origini) non è valida se è presente l'intestazione del `Access-Control-Allow-Credentials`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-229">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="a9c62-230">Richieste preliminari</span><span class="sxs-lookup"><span data-stu-id="a9c62-230">Preflight requests</span></span>

<span data-ttu-id="a9c62-231">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva prima di effettuare la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a9c62-231">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="a9c62-232">Questa richiesta è detta *richiesta preliminare*.</span><span class="sxs-lookup"><span data-stu-id="a9c62-232">This request is called a *preflight request*.</span></span> <span data-ttu-id="a9c62-233">Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a9c62-233">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="a9c62-234">Il metodo di richiesta è GET, HEAD o POST.</span><span class="sxs-lookup"><span data-stu-id="a9c62-234">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="a9c62-235">L'app non imposta intestazioni di richiesta diverse da `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`o `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-235">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="a9c62-236">L'intestazione `Content-Type`, se impostata, presenta uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="a9c62-236">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="a9c62-237">La regola sulle intestazioni di richiesta impostate per la richiesta del client si applica alle intestazioni impostate dall'app chiamando `setRequestHeader` sull'oggetto `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-237">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="a9c62-238">La specifica CORS chiama queste intestazioni di *richiesta autore*intestazioni.</span><span class="sxs-lookup"><span data-stu-id="a9c62-238">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="a9c62-239">La regola non si applica alle intestazioni che il browser può impostare, ad esempio `User-Agent`, `Host`o `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-239">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="a9c62-240">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="a9c62-240">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="a9c62-241">La richiesta pre-flight usa il metodo delle opzioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="a9c62-241">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="a9c62-242">Sono incluse due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="a9c62-242">It includes two special headers:</span></span>

* <span data-ttu-id="a9c62-243">`Access-Control-Request-Method`: il metodo HTTP che verrà usato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a9c62-243">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="a9c62-244">`Access-Control-Request-Headers`: elenco di intestazioni di richiesta impostate dall'app sulla richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a9c62-244">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="a9c62-245">Come indicato in precedenza, non include le intestazioni impostate dal browser, ad esempio `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-245">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="a9c62-246">Una richiesta preliminare CORS può includere un'intestazione `Access-Control-Request-Headers`, che indica al server le intestazioni inviate con la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a9c62-246">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="a9c62-247">Per consentire intestazioni specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="a9c62-247">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="a9c62-248">Per consentire tutte le intestazioni di richiesta di autore, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="a9c62-248">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="a9c62-249">I browser non sono completamente coerenti nell'impostazione `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="a9c62-249">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="a9c62-250">Se si impostano le intestazioni su un valore diverso da `"*"` (o si utilizza <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), è necessario includere almeno `Accept`, `Content-Type`e `Origin`, oltre a eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="a9c62-250">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="a9c62-251">Di seguito è riportato un esempio di risposta alla richiesta preliminare (presupponendo che il server consenta la richiesta):</span><span class="sxs-lookup"><span data-stu-id="a9c62-251">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="a9c62-252">La risposta include un'intestazione `Access-Control-Allow-Methods` che elenca i metodi consentiti e, facoltativamente, un'intestazione `Access-Control-Allow-Headers`, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="a9c62-252">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="a9c62-253">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a9c62-253">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="a9c62-254">Se la richiesta preliminare viene negata, l'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS.</span><span class="sxs-lookup"><span data-stu-id="a9c62-254">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="a9c62-255">Pertanto, il browser non tenta di eseguire la richiesta tra origini.</span><span class="sxs-lookup"><span data-stu-id="a9c62-255">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="a9c62-256">Imposta la data di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="a9c62-256">Set the preflight expiration time</span></span>

<span data-ttu-id="a9c62-257">L'intestazione `Access-Control-Max-Age` specifica per quanto tempo la risposta alla richiesta preliminare può essere memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="a9c62-257">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="a9c62-258">Per impostare questa intestazione, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="a9c62-258">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="a9c62-259">Funzionamento di CORS</span><span class="sxs-lookup"><span data-stu-id="a9c62-259">How CORS works</span></span>

<span data-ttu-id="a9c62-260">In questa sezione viene descritto cosa accade in una richiesta [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) al livello dei messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="a9c62-260">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="a9c62-261">CORS **non** è una funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a9c62-261">CORS is **not** a security feature.</span></span> <span data-ttu-id="a9c62-262">CORS è uno standard W3C che consente a un server di ridurre i criteri della stessa origine.</span><span class="sxs-lookup"><span data-stu-id="a9c62-262">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="a9c62-263">Ad esempio, un attore malintenzionato potrebbe usare [Impedisci lo scripting tra siti (XSS)](xref:security/cross-site-scripting) per il sito ed eseguire una richiesta tra siti al sito abilitato per CORS per rubare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="a9c62-263">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="a9c62-264">L'API non è più sicura consentendo CORS.</span><span class="sxs-lookup"><span data-stu-id="a9c62-264">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="a9c62-265">Il client (browser) deve applicare CORS.</span><span class="sxs-lookup"><span data-stu-id="a9c62-265">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="a9c62-266">Il server esegue la richiesta e restituisce la risposta, ovvero il client che restituisce un errore e blocca la risposta.</span><span class="sxs-lookup"><span data-stu-id="a9c62-266">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="a9c62-267">Uno degli strumenti seguenti, ad esempio, visualizzerà la risposta del server:</span><span class="sxs-lookup"><span data-stu-id="a9c62-267">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="a9c62-268">Fiddler</span><span class="sxs-lookup"><span data-stu-id="a9c62-268">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="a9c62-269">Postman</span><span class="sxs-lookup"><span data-stu-id="a9c62-269">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="a9c62-270">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="a9c62-270">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="a9c62-271">Un Web browser immettendo l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="a9c62-271">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="a9c62-272">Si tratta di un modo per consentire a un server di consentire ai browser di eseguire una richiesta dell' [API di recupero](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) o di [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) tra le origini, che altrimenti sarebbe proibita.</span><span class="sxs-lookup"><span data-stu-id="a9c62-272">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="a9c62-273">I browser (senza CORS) non possono eseguire richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a9c62-273">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="a9c62-274">Prima di CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) è stato usato per aggirare questa restrizione.</span><span class="sxs-lookup"><span data-stu-id="a9c62-274">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="a9c62-275">JSONP non usa XHR, usa il tag `<script>` per ricevere la risposta.</span><span class="sxs-lookup"><span data-stu-id="a9c62-275">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="a9c62-276">Gli script possono essere caricati tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a9c62-276">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="a9c62-277">La [specifica CORS](https://www.w3.org/TR/cors/) ha introdotto diverse nuove intestazioni HTTP che consentono richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a9c62-277">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="a9c62-278">Se un browser supporta CORS, queste intestazioni vengono impostate automaticamente per le richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a9c62-278">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="a9c62-279">Il codice JavaScript personalizzato non è necessario per abilitare CORS.</span><span class="sxs-lookup"><span data-stu-id="a9c62-279">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="a9c62-280">Di seguito è riportato un esempio di richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a9c62-280">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="a9c62-281">L'intestazione `Origin` fornisce il dominio del sito che sta effettuando la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a9c62-281">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="a9c62-282">L'intestazione `Origin` è obbligatoria e deve essere diversa dall'host.</span><span class="sxs-lookup"><span data-stu-id="a9c62-282">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="a9c62-283">Se il server consente la richiesta, imposta l'intestazione `Access-Control-Allow-Origin` nella risposta.</span><span class="sxs-lookup"><span data-stu-id="a9c62-283">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="a9c62-284">Il valore di questa intestazione corrisponde all'intestazione `Origin` dalla richiesta o è il valore del carattere jolly `"*"`, che indica che è consentita un'origine:</span><span class="sxs-lookup"><span data-stu-id="a9c62-284">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="a9c62-285">Se la risposta non include l'intestazione `Access-Control-Allow-Origin`, la richiesta tra le origini ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="a9c62-285">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="a9c62-286">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a9c62-286">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="a9c62-287">Anche se il server restituisce una risposta con esito positivo, il browser non rende disponibile la risposta all'app client.</span><span class="sxs-lookup"><span data-stu-id="a9c62-287">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="a9c62-288">Testare CORS</span><span class="sxs-lookup"><span data-stu-id="a9c62-288">Test CORS</span></span>

<span data-ttu-id="a9c62-289">Per testare CORS:</span><span class="sxs-lookup"><span data-stu-id="a9c62-289">To test CORS:</span></span>

1. <span data-ttu-id="a9c62-290">[Creare un progetto API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="a9c62-290">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="a9c62-291">In alternativa, è possibile [scaricare l'esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="a9c62-291">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="a9c62-292">Abilitare CORS usando uno degli approcci descritti in questo documento.</span><span class="sxs-lookup"><span data-stu-id="a9c62-292">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="a9c62-293">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="a9c62-293">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="a9c62-294">`WithOrigins("https://localhost:<port>");` deve essere usato solo per il test di un'app di esempio simile al [codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)per il download.</span><span class="sxs-lookup"><span data-stu-id="a9c62-294">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="a9c62-295">Creare un progetto di app Web (Razor Pages o MVC).</span><span class="sxs-lookup"><span data-stu-id="a9c62-295">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="a9c62-296">Nell'esempio viene utilizzato Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a9c62-296">The sample uses Razor Pages.</span></span> <span data-ttu-id="a9c62-297">È possibile creare l'app Web nella stessa soluzione del progetto API.</span><span class="sxs-lookup"><span data-stu-id="a9c62-297">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="a9c62-298">Aggiungere il codice evidenziato seguente al file *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a9c62-298">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="a9c62-299">Nel codice precedente sostituire `url: 'https://<web app>.azurewebsites.net/api/values/1',` con l'URL dell'app distribuita.</span><span class="sxs-lookup"><span data-stu-id="a9c62-299">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="a9c62-300">Distribuire il progetto API.</span><span class="sxs-lookup"><span data-stu-id="a9c62-300">Deploy the API project.</span></span> <span data-ttu-id="a9c62-301">Ad esempio, [eseguire la distribuzione in Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="a9c62-301">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="a9c62-302">Eseguire l'app Razor Pages o MVC dal desktop e fare clic sul pulsante **test** .</span><span class="sxs-lookup"><span data-stu-id="a9c62-302">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="a9c62-303">Usare gli strumenti F12 per esaminare i messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="a9c62-303">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="a9c62-304">Rimuovere l'origine localhost da `WithOrigins` e distribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="a9c62-304">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="a9c62-305">In alternativa, eseguire l'app client con una porta diversa.</span><span class="sxs-lookup"><span data-stu-id="a9c62-305">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="a9c62-306">Ad esempio, eseguire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9c62-306">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="a9c62-307">Eseguire il test con l'app client.</span><span class="sxs-lookup"><span data-stu-id="a9c62-307">Test with the client app.</span></span> <span data-ttu-id="a9c62-308">Gli errori CORS restituiscono un errore, ma il messaggio di errore non è disponibile per JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a9c62-308">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="a9c62-309">Usare la scheda console negli strumenti F12 per visualizzare l'errore.</span><span class="sxs-lookup"><span data-stu-id="a9c62-309">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="a9c62-310">A seconda del browser, viene ricevuto un errore (nella console strumenti F12) simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a9c62-310">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="a9c62-311">Uso di Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="a9c62-311">Using Microsoft Edge:</span></span>

     <span data-ttu-id="a9c62-312">**SEC7120: [CORS] la `https://localhost:44375` di origine non ha trovato `https://localhost:44375` nell'intestazione della risposta Access-Control-Allow-Origin per la risorsa tra le origini in `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="a9c62-312">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="a9c62-313">Uso di Chrome:</span><span class="sxs-lookup"><span data-stu-id="a9c62-313">Using Chrome:</span></span>

     <span data-ttu-id="a9c62-314">**L'accesso a XMLHttpRequest in `https://webapi.azurewebsites.net/api/values/1` dalla `https://localhost:44375` di origine è stato bloccato dal criterio CORS: non è presente alcuna intestazione ' Access-Control-Allow-Origin ' nella risorsa richiesta.**</span><span class="sxs-lookup"><span data-stu-id="a9c62-314">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="a9c62-315">Gli endpoint abilitati per CORS possono essere testati con uno strumento, ad esempio [Fiddler](https://www.telerik.com/fiddler) o [postazione](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="a9c62-315">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="a9c62-316">Quando si utilizza uno strumento, l'origine della richiesta specificata dall'intestazione `Origin` deve essere diversa dall'host che riceve la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a9c62-316">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="a9c62-317">Se la richiesta non è *tra origini* basate sul valore dell'intestazione `Origin`:</span><span class="sxs-lookup"><span data-stu-id="a9c62-317">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="a9c62-318">Non è necessario che il middleware CORS elabori la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a9c62-318">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="a9c62-319">Le intestazioni CORS non vengono restituite nella risposta.</span><span class="sxs-lookup"><span data-stu-id="a9c62-319">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="a9c62-320">CORS in IIS</span><span class="sxs-lookup"><span data-stu-id="a9c62-320">CORS in IIS</span></span>

<span data-ttu-id="a9c62-321">Quando si esegue la distribuzione in IIS, CORS deve essere eseguito prima dell'autenticazione di Windows se il server non è configurato per consentire l'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="a9c62-321">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="a9c62-322">Per supportare questo scenario, è necessario installare e configurare il [modulo cors di IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) per l'app.</span><span class="sxs-lookup"><span data-stu-id="a9c62-322">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a9c62-323">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a9c62-323">Additional resources</span></span>

* [<span data-ttu-id="a9c62-324">Condivisione di risorse tra le origini (CORS)</span><span class="sxs-lookup"><span data-stu-id="a9c62-324">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="a9c62-325">Introduzione al modulo CORS di IIS</span><span class="sxs-lookup"><span data-stu-id="a9c62-325">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)
