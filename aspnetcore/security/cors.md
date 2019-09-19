---
title: Abilitare le richieste tra le origini (CORS) in ASP.NET Core
author: rick-anderson
description: Informazioni su come CORS come standard per consentire o rifiutare le richieste tra origini in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a34b77ad799a00707048c923b82b48774ce91682
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108076"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="a0586-103">Abilitare le richieste tra le origini (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0586-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="a0586-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a0586-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a0586-105">Questo articolo illustra come abilitare CORS in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0586-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="a0586-106">La sicurezza del browser impedisce a una pagina Web di eseguire richieste a un dominio diverso da quello che ha gestito la pagina Web.</span><span class="sxs-lookup"><span data-stu-id="a0586-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="a0586-107">Questa restrizione è detta *criterio della stessa origine*.</span><span class="sxs-lookup"><span data-stu-id="a0586-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="a0586-108">Il criterio della stessa origine impedisce a un sito dannoso di leggere dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="a0586-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="a0586-109">In alcuni casi potrebbe essere necessario consentire ad altri siti di effettuare richieste tra le origini all'app.</span><span class="sxs-lookup"><span data-stu-id="a0586-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="a0586-110">Per altre informazioni, vedere l' [articolo relativo a Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="a0586-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="a0586-111">[Condivisione di risorse tra le origini](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="a0586-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="a0586-112">È uno standard W3C che consente a un server di ridurre i criteri della stessa origine.</span><span class="sxs-lookup"><span data-stu-id="a0586-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="a0586-113">**Non** è una funzionalità di sicurezza, CORS rilassa la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a0586-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="a0586-114">Un'API non è più sicura consentendo CORS.</span><span class="sxs-lookup"><span data-stu-id="a0586-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="a0586-115">Per ulteriori informazioni, vedere [funzionamento di CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="a0586-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="a0586-116">Consente a un server di consentire in modo esplicito alcune richieste tra origini rifiutando altre.</span><span class="sxs-lookup"><span data-stu-id="a0586-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="a0586-117">È più sicuro e flessibile rispetto alle tecniche precedenti, ad esempio [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="a0586-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="a0586-118">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a0586-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="a0586-119">Stessa origine</span><span class="sxs-lookup"><span data-stu-id="a0586-119">Same origin</span></span>

<span data-ttu-id="a0586-120">Due URL hanno la stessa origine se hanno schemi, host e porte identici ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="a0586-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="a0586-121">Questi due URL hanno la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="a0586-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="a0586-122">Questi URL hanno origini diverse da quelle dei due URL precedenti:</span><span class="sxs-lookup"><span data-stu-id="a0586-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="a0586-123">`https://example.net`&ndash; Dominio diverso</span><span class="sxs-lookup"><span data-stu-id="a0586-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="a0586-124">`https://www.example.com/foo.html`&ndash; Sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="a0586-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="a0586-125">`http://example.com/foo.html`&ndash; Schema diverso</span><span class="sxs-lookup"><span data-stu-id="a0586-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="a0586-126">`https://example.com:9000/foo.html`&ndash; Porta diversa</span><span class="sxs-lookup"><span data-stu-id="a0586-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="a0586-127">Internet Explorer non considera la porta durante il confronto delle origini.</span><span class="sxs-lookup"><span data-stu-id="a0586-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="a0586-128">CORS con criteri e middleware denominati</span><span class="sxs-lookup"><span data-stu-id="a0586-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="a0586-129">Il middleware CORS gestisce le richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a0586-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="a0586-130">Il codice seguente Abilita CORS per l'intera app con l'origine specificata:</span><span class="sxs-lookup"><span data-stu-id="a0586-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="a0586-131">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="a0586-131">The preceding code:</span></span>

* <span data-ttu-id="a0586-132">Imposta il nome del criterio su\_"myAllowSpecificOrigins".</span><span class="sxs-lookup"><span data-stu-id="a0586-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="a0586-133">Il nome del criterio è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="a0586-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="a0586-134">Chiama il <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> metodo di estensione, che Abilita CORS.</span><span class="sxs-lookup"><span data-stu-id="a0586-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="a0586-135">Chiama <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con un' [espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="a0586-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="a0586-136">L'espressione lambda accetta <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> un oggetto.</span><span class="sxs-lookup"><span data-stu-id="a0586-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="a0586-137">Le [Opzioni di configurazione](#cors-policy-options), `WithOrigins`ad esempio, sono descritte più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="a0586-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="a0586-138">La <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> chiamata al metodo aggiunge i servizi CORS al contenitore del servizio dell'app:</span><span class="sxs-lookup"><span data-stu-id="a0586-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="a0586-139">Per ulteriori informazioni, vedere [Opzioni dei criteri CORS](#cpo) in questo documento.</span><span class="sxs-lookup"><span data-stu-id="a0586-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="a0586-140">Il <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> metodo può concatenare i metodi, come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a0586-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="a0586-141">Nota: L'URL **non** deve contenere una barra finale (`/`).</span><span class="sxs-lookup"><span data-stu-id="a0586-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="a0586-142">Se l'URL termina con `/`, il confronto restituisce `false` e non viene restituita alcuna intestazione.</span><span class="sxs-lookup"><span data-stu-id="a0586-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a0586-143">Il codice seguente applica i criteri di CORS a tutti gli endpoint di app tramite il middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="a0586-143">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
> <span data-ttu-id="a0586-144">Con il routing dell'endpoint è necessario configurare il middleware CORS per l'esecuzione tra le `UseRouting` chiamate `UseEndpoints`a e.</span><span class="sxs-lookup"><span data-stu-id="a0586-144">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="a0586-145">Una configurazione non corretta causerà il corretto funzionamento del middleware.</span><span class="sxs-lookup"><span data-stu-id="a0586-145">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="a0586-146">Il codice seguente applica i criteri di CORS a tutti gli endpoint di app tramite il middleware CORS:</span><span class="sxs-lookup"><span data-stu-id="a0586-146">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="a0586-147">Nota: `UseCors` è necessario chiamare prima `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="a0586-147">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="a0586-148">Per applicare i criteri di CORS a livello di pagina/controller/azione [, vedere Enable CORS in Razor Pages, Controllers e Action Methods](#ecors) .</span><span class="sxs-lookup"><span data-stu-id="a0586-148">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="a0586-149">Vedere [test CORS](#test) per istruzioni sul test del codice precedente.</span><span class="sxs-lookup"><span data-stu-id="a0586-149">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="a0586-150">Abilitare cors con routing degli endpoint</span><span class="sxs-lookup"><span data-stu-id="a0586-150">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="a0586-151">Con il routing degli endpoint, è possibile abilitare CORS in base all'endpoint usando il `RequireCors` set di metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="a0586-151">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="a0586-152">Analogamente, è anche possibile abilitare CORS per tutti i controller:</span><span class="sxs-lookup"><span data-stu-id="a0586-152">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="a0586-153">Abilita CORS con attributi</span><span class="sxs-lookup"><span data-stu-id="a0586-153">Enable CORS with attributes</span></span>

<span data-ttu-id="a0586-154">[ L'&lbrack;attributoEnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fornisce un'alternativa all'applicazione di CORS a livello globale.</span><span class="sxs-lookup"><span data-stu-id="a0586-154">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="a0586-155">L' `[EnableCors]` attributo Abilita CORS per i punti finali selezionati, anziché tutti i punti finali.</span><span class="sxs-lookup"><span data-stu-id="a0586-155">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="a0586-156">Usare `[EnableCors]` per specificare i criteri predefiniti e `[EnableCors("{Policy String}")]` per specificare un criterio.</span><span class="sxs-lookup"><span data-stu-id="a0586-156">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="a0586-157">L' `[EnableCors]` attributo può essere applicato a:</span><span class="sxs-lookup"><span data-stu-id="a0586-157">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="a0586-158">Pagina Razor`PageModel`</span><span class="sxs-lookup"><span data-stu-id="a0586-158">Razor Page `PageModel`</span></span>
* <span data-ttu-id="a0586-159">Controller</span><span class="sxs-lookup"><span data-stu-id="a0586-159">Controller</span></span>
* <span data-ttu-id="a0586-160">Metodo di azione del controller</span><span class="sxs-lookup"><span data-stu-id="a0586-160">Controller action method</span></span>

<span data-ttu-id="a0586-161">È possibile applicare criteri diversi a controller/pagina-modello/azione con l' `[EnableCors]` attributo.</span><span class="sxs-lookup"><span data-stu-id="a0586-161">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="a0586-162">Quando l' `[EnableCors]` attributo viene applicato a controller/modello di pagina/Metodo di azione e CORS è abilitato nel middleware, vengono applicati entrambi i criteri.</span><span class="sxs-lookup"><span data-stu-id="a0586-162">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="a0586-163">È consigliabile evitare di combinare i criteri.</span><span class="sxs-lookup"><span data-stu-id="a0586-163">We recommend against combining policies.</span></span> <span data-ttu-id="a0586-164">Usare l' `[EnableCors]` attributo o il middleware, non entrambi nella stessa app.</span><span class="sxs-lookup"><span data-stu-id="a0586-164">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="a0586-165">Il codice seguente applica un criterio diverso a ogni metodo:</span><span class="sxs-lookup"><span data-stu-id="a0586-165">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="a0586-166">Il codice seguente crea un criterio predefinito CORS e un criterio denominato `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="a0586-166">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="a0586-167">Disabilitare CORS</span><span class="sxs-lookup"><span data-stu-id="a0586-167">Disable CORS</span></span>

<span data-ttu-id="a0586-168">[ L'&lbrack;attributoDisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) Disabilita CORS per il controller, il modello di pagina o l'azione.</span><span class="sxs-lookup"><span data-stu-id="a0586-168">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="a0586-169">Opzioni dei criteri di CORS</span><span class="sxs-lookup"><span data-stu-id="a0586-169">CORS policy options</span></span>

<span data-ttu-id="a0586-170">In questa sezione vengono descritte le varie opzioni che è possibile impostare in un criterio CORS:</span><span class="sxs-lookup"><span data-stu-id="a0586-170">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="a0586-171">Imposta le origini consentite</span><span class="sxs-lookup"><span data-stu-id="a0586-171">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="a0586-172">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="a0586-172">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="a0586-173">Impostare le intestazioni della richiesta consentita</span><span class="sxs-lookup"><span data-stu-id="a0586-173">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="a0586-174">Impostare le intestazioni di risposta esposte</span><span class="sxs-lookup"><span data-stu-id="a0586-174">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="a0586-175">Credenziali nelle richieste tra le origini</span><span class="sxs-lookup"><span data-stu-id="a0586-175">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="a0586-176">Imposta la data di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="a0586-176">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="a0586-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>viene chiamato in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a0586-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a0586-178">Per alcune opzioni, può essere utile leggere prima la sezione [come funziona CORS](#how-cors) .</span><span class="sxs-lookup"><span data-stu-id="a0586-178">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="a0586-179">Imposta le origini consentite</span><span class="sxs-lookup"><span data-stu-id="a0586-179">Set the allowed origins</span></span>

<span data-ttu-id="a0586-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>Consente le richieste CORS da tutte le origini con qualsiasi`http` schema `https`(o). &ndash;</span><span class="sxs-lookup"><span data-stu-id="a0586-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="a0586-181">`AllowAnyOrigin`non è sicuro perché *qualsiasi sito Web* può effettuare richieste tra origini per l'app.</span><span class="sxs-lookup"><span data-stu-id="a0586-181">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="a0586-182">La `AllowAnyOrigin` definizione `AllowCredentials` di e è una configurazione non sicura e può comportare la falsificazione della richiesta tra siti.</span><span class="sxs-lookup"><span data-stu-id="a0586-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="a0586-183">Il servizio CORS restituisce una risposta CORS non valida quando un'app è configurata con entrambi i metodi.</span><span class="sxs-lookup"><span data-stu-id="a0586-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="a0586-184">La `AllowAnyOrigin` definizione `AllowCredentials` di e è una configurazione non sicura e può comportare la falsificazione della richiesta tra siti.</span><span class="sxs-lookup"><span data-stu-id="a0586-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="a0586-185">Per un'app sicura, specificare un elenco esatto di origini se il client deve autorizzare se stesso ad accedere alle risorse del server.</span><span class="sxs-lookup"><span data-stu-id="a0586-185">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="a0586-186">`AllowAnyOrigin`influiscono sulle richieste preliminari `Access-Control-Allow-Origin` e sull'intestazione.</span><span class="sxs-lookup"><span data-stu-id="a0586-186">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="a0586-187">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a0586-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a0586-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash; Imposta la<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> proprietà del criterio come funzione che consente alle origini di corrispondere a un dominio con caratteri jolly configurato durante la valutazione se l'origine è consentita.</span><span class="sxs-lookup"><span data-stu-id="a0586-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="a0586-189">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="a0586-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="a0586-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="a0586-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="a0586-191">Consente qualsiasi metodo HTTP:</span><span class="sxs-lookup"><span data-stu-id="a0586-191">Allows any HTTP method:</span></span>
* <span data-ttu-id="a0586-192">Influiscono sulle richieste preliminari `Access-Control-Allow-Methods` e sull'intestazione.</span><span class="sxs-lookup"><span data-stu-id="a0586-192">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="a0586-193">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a0586-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="a0586-194">Impostare le intestazioni della richiesta consentita</span><span class="sxs-lookup"><span data-stu-id="a0586-194">Set the allowed request headers</span></span>

<span data-ttu-id="a0586-195">Per consentire l'invio di intestazioni specifiche in una richiesta CORS, denominate *intestazioni richiesta autore*, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e specificare le intestazioni consentite:</span><span class="sxs-lookup"><span data-stu-id="a0586-195">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="a0586-196">Per consentire tutte le intestazioni di richiesta di <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>autore, chiamare:</span><span class="sxs-lookup"><span data-stu-id="a0586-196">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="a0586-197">Questa impostazione influiscono sulle richieste preliminari `Access-Control-Request-Headers` e sull'intestazione.</span><span class="sxs-lookup"><span data-stu-id="a0586-197">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="a0586-198">Per ulteriori informazioni, vedere la sezione [preflight requests](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="a0586-198">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a0586-199">Un criterio middleware CORS corrisponde a intestazioni specifiche specificate da `WithHeaders` è possibile solo quando le intestazioni `Access-Control-Request-Headers` inviate corrispondono esattamente alle intestazioni indicate in `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="a0586-199">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="a0586-200">Si consideri, ad esempio, un'app configurata come segue:</span><span class="sxs-lookup"><span data-stu-id="a0586-200">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="a0586-201">Il middleware CORS rifiuta una richiesta preliminare con la seguente intestazione di richiesta `Content-Language` perché ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) non è `WithHeaders`elencato in:</span><span class="sxs-lookup"><span data-stu-id="a0586-201">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="a0586-202">L'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS.</span><span class="sxs-lookup"><span data-stu-id="a0586-202">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="a0586-203">Pertanto, il browser non tenta di eseguire la richiesta tra origini.</span><span class="sxs-lookup"><span data-stu-id="a0586-203">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a0586-204">Il `Access-Control-Request-Headers` middleware CORS consente sempre l'invio di quattro intestazioni in, indipendentemente dai valori configurati in CorsPolicy. Headers.</span><span class="sxs-lookup"><span data-stu-id="a0586-204">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="a0586-205">Questo elenco di intestazioni include:</span><span class="sxs-lookup"><span data-stu-id="a0586-205">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="a0586-206">Si consideri, ad esempio, un'app configurata come segue:</span><span class="sxs-lookup"><span data-stu-id="a0586-206">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="a0586-207">Il middleware CORS risponde correttamente a una richiesta preliminare con la seguente intestazione di richiesta `Content-Language` perché è sempre consentita:</span><span class="sxs-lookup"><span data-stu-id="a0586-207">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="a0586-208">Impostare le intestazioni di risposta esposte</span><span class="sxs-lookup"><span data-stu-id="a0586-208">Set the exposed response headers</span></span>

<span data-ttu-id="a0586-209">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'app.</span><span class="sxs-lookup"><span data-stu-id="a0586-209">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="a0586-210">Per ulteriori informazioni, vedere [la pagina relativa alla condivisione delle risorse tra le origini W3C (terminologia): Intestazione](https://www.w3.org/TR/cors/#simple-response-header)della risposta semplice.</span><span class="sxs-lookup"><span data-stu-id="a0586-210">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="a0586-211">Le intestazioni di risposta disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="a0586-211">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="a0586-212">La specifica CORS chiama queste intestazioni di *risposta semplici*.</span><span class="sxs-lookup"><span data-stu-id="a0586-212">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="a0586-213">Per rendere disponibili altre intestazioni per l'app, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="a0586-213">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="a0586-214">Credenziali nelle richieste tra le origini</span><span class="sxs-lookup"><span data-stu-id="a0586-214">Credentials in cross-origin requests</span></span>

<span data-ttu-id="a0586-215">Le credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="a0586-215">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="a0586-216">Per impostazione predefinita, il browser non invia le credenziali con una richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a0586-216">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="a0586-217">Le credenziali includono i cookie e gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0586-217">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="a0586-218">Per inviare le credenziali con una richiesta tra le origini, il client deve `XMLHttpRequest.withCredentials` impostare `true`su.</span><span class="sxs-lookup"><span data-stu-id="a0586-218">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="a0586-219">Utilizzando `XMLHttpRequest` direttamente:</span><span class="sxs-lookup"><span data-stu-id="a0586-219">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="a0586-220">Uso di jQuery:</span><span class="sxs-lookup"><span data-stu-id="a0586-220">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="a0586-221">Uso dell' [API fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="a0586-221">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="a0586-222">Il server deve consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="a0586-222">The server must allow the credentials.</span></span> <span data-ttu-id="a0586-223">Per consentire le credenziali tra le origini, <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>chiamare:</span><span class="sxs-lookup"><span data-stu-id="a0586-223">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="a0586-224">La risposta HTTP include un' `Access-Control-Allow-Credentials` intestazione che indica al browser che il server consente le credenziali per una richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a0586-224">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="a0586-225">Se il browser invia le credenziali, ma la risposta non include `Access-Control-Allow-Credentials` un'intestazione valida, il browser non espone la risposta all'app e la richiesta tra le origini ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="a0586-225">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="a0586-226">Consentire le credenziali tra le origini costituisce un rischio per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a0586-226">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="a0586-227">Un sito Web in un altro dominio può inviare le credenziali dell'utente che ha eseguito l'accesso all'app per conto dell'utente senza la conoscenza dell'utente.</span><span class="sxs-lookup"><span data-stu-id="a0586-227">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="a0586-228">La specifica CORS indica inoltre che l'impostazione delle `"*"` origini su (tutte le origini) non `Access-Control-Allow-Credentials` è valida se l'intestazione è presente.</span><span class="sxs-lookup"><span data-stu-id="a0586-228">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="a0586-229">Richieste preliminari</span><span class="sxs-lookup"><span data-stu-id="a0586-229">Preflight requests</span></span>

<span data-ttu-id="a0586-230">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva prima di effettuare la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a0586-230">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="a0586-231">Questa richiesta è detta *richiesta preliminare*.</span><span class="sxs-lookup"><span data-stu-id="a0586-231">This request is called a *preflight request*.</span></span> <span data-ttu-id="a0586-232">Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a0586-232">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="a0586-233">Il metodo di richiesta è GET, HEAD o POST.</span><span class="sxs-lookup"><span data-stu-id="a0586-233">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="a0586-234">L'app non imposta intestazioni di richiesta diverse `Accept`da `Accept-Language`, `Content-Language`, `Content-Type`, o `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="a0586-234">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="a0586-235">L' `Content-Type` intestazione, se impostata, presenta uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="a0586-235">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="a0586-236">La regola sulle intestazioni di richiesta impostate per la richiesta del client si applica alle intestazioni impostate dall'app `setRequestHeader` chiamando `XMLHttpRequest` sull'oggetto.</span><span class="sxs-lookup"><span data-stu-id="a0586-236">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="a0586-237">La specifica CORS chiama queste intestazioni di *richiesta autore*intestazioni.</span><span class="sxs-lookup"><span data-stu-id="a0586-237">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="a0586-238">La regola non si applica alle intestazioni che il browser può impostare, `User-Agent`ad `Host`esempio, `Content-Length`o.</span><span class="sxs-lookup"><span data-stu-id="a0586-238">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="a0586-239">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="a0586-239">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="a0586-240">La richiesta pre-flight usa il metodo delle opzioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0586-240">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="a0586-241">Sono incluse due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="a0586-241">It includes two special headers:</span></span>

* <span data-ttu-id="a0586-242">`Access-Control-Request-Method`: Metodo HTTP che verrà utilizzato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a0586-242">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="a0586-243">`Access-Control-Request-Headers`: Elenco di intestazioni di richiesta impostate dall'app sulla richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a0586-243">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="a0586-244">Come indicato in precedenza, non include le intestazioni impostate dal browser, ad esempio `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="a0586-244">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="a0586-245">Una richiesta preliminare CORS può includere un' `Access-Control-Request-Headers` intestazione, che indica al server le intestazioni inviate con la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a0586-245">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="a0586-246">Per consentire intestazioni specifiche, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="a0586-246">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="a0586-247">Per consentire tutte le intestazioni di richiesta di <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>autore, chiamare:</span><span class="sxs-lookup"><span data-stu-id="a0586-247">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="a0586-248">I browser non sono completamente coerenti nel modo `Access-Control-Request-Headers`in cui vengono impostati.</span><span class="sxs-lookup"><span data-stu-id="a0586-248">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="a0586-249">Se si impostano le intestazioni su `"*"` un valore diverso <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>da (o si utilizza), `Accept`è `Content-Type`necessario includere `Origin`almeno, e, più eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="a0586-249">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="a0586-250">Di seguito è riportato un esempio di risposta alla richiesta preliminare (presupponendo che il server consenta la richiesta):</span><span class="sxs-lookup"><span data-stu-id="a0586-250">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="a0586-251">La risposta include un' `Access-Control-Allow-Methods` intestazione che elenca i metodi consentiti e, `Access-Control-Allow-Headers` facoltativamente, un'intestazione, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="a0586-251">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="a0586-252">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="a0586-252">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="a0586-253">Se la richiesta preliminare viene negata, l'app restituisce una risposta *200 OK* , ma non invia le intestazioni CORS.</span><span class="sxs-lookup"><span data-stu-id="a0586-253">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="a0586-254">Pertanto, il browser non tenta di eseguire la richiesta tra origini.</span><span class="sxs-lookup"><span data-stu-id="a0586-254">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="a0586-255">Imposta la data di scadenza preliminare</span><span class="sxs-lookup"><span data-stu-id="a0586-255">Set the preflight expiration time</span></span>

<span data-ttu-id="a0586-256">L' `Access-Control-Max-Age` intestazione specifica per quanto tempo la risposta alla richiesta preliminare può essere memorizzata nella cache.</span><span class="sxs-lookup"><span data-stu-id="a0586-256">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="a0586-257">Per impostare questa intestazione, chiamare <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="a0586-257">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="a0586-258">Funzionamento di CORS</span><span class="sxs-lookup"><span data-stu-id="a0586-258">How CORS works</span></span>

<span data-ttu-id="a0586-259">In questa sezione viene descritto cosa accade in una richiesta [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) al livello dei messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0586-259">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="a0586-260">CORS **non** è una funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a0586-260">CORS is **not** a security feature.</span></span> <span data-ttu-id="a0586-261">CORS è uno standard W3C che consente a un server di ridurre i criteri della stessa origine.</span><span class="sxs-lookup"><span data-stu-id="a0586-261">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="a0586-262">Ad esempio, un attore malintenzionato potrebbe usare [Impedisci lo scripting tra siti (XSS)](xref:security/cross-site-scripting) per il sito ed eseguire una richiesta tra siti al sito abilitato per CORS per rubare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="a0586-262">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="a0586-263">L'API non è più sicura consentendo CORS.</span><span class="sxs-lookup"><span data-stu-id="a0586-263">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="a0586-264">Il client (browser) deve applicare CORS.</span><span class="sxs-lookup"><span data-stu-id="a0586-264">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="a0586-265">Il server esegue la richiesta e restituisce la risposta, ovvero il client che restituisce un errore e blocca la risposta.</span><span class="sxs-lookup"><span data-stu-id="a0586-265">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="a0586-266">Uno degli strumenti seguenti, ad esempio, visualizzerà la risposta del server:</span><span class="sxs-lookup"><span data-stu-id="a0586-266">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="a0586-267">Fiddler</span><span class="sxs-lookup"><span data-stu-id="a0586-267">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="a0586-268">Postman</span><span class="sxs-lookup"><span data-stu-id="a0586-268">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="a0586-269">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="a0586-269">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="a0586-270">Un Web browser immettendo l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="a0586-270">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="a0586-271">Si tratta di un modo per consentire a un server di consentire ai browser di eseguire una richiesta dell' [API di recupero](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) o di [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) tra le origini, che altrimenti sarebbe proibita.</span><span class="sxs-lookup"><span data-stu-id="a0586-271">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="a0586-272">I browser (senza CORS) non possono eseguire richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a0586-272">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="a0586-273">Prima di CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) è stato usato per aggirare questa restrizione.</span><span class="sxs-lookup"><span data-stu-id="a0586-273">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="a0586-274">JSONP non usa XHR, usa il `<script>` tag per ricevere la risposta.</span><span class="sxs-lookup"><span data-stu-id="a0586-274">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="a0586-275">Gli script possono essere caricati tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a0586-275">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="a0586-276">La [specifica CORS](https://www.w3.org/TR/cors/) ha introdotto diverse nuove intestazioni HTTP che consentono richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a0586-276">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="a0586-277">Se un browser supporta CORS, queste intestazioni vengono impostate automaticamente per le richieste tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a0586-277">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="a0586-278">Il codice JavaScript personalizzato non è necessario per abilitare CORS.</span><span class="sxs-lookup"><span data-stu-id="a0586-278">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="a0586-279">Di seguito è riportato un esempio di richiesta tra le origini.</span><span class="sxs-lookup"><span data-stu-id="a0586-279">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="a0586-280">L' `Origin` intestazione fornisce il dominio del sito che sta effettuando la richiesta:</span><span class="sxs-lookup"><span data-stu-id="a0586-280">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="a0586-281">Se il server consente la richiesta, imposta l' `Access-Control-Allow-Origin` intestazione nella risposta.</span><span class="sxs-lookup"><span data-stu-id="a0586-281">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="a0586-282">Il valore di questa intestazione corrisponde all' `Origin` intestazione della richiesta o è il valore `"*"`del carattere jolly, ovvero qualsiasi origine è consentita:</span><span class="sxs-lookup"><span data-stu-id="a0586-282">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="a0586-283">Se la risposta non include l' `Access-Control-Allow-Origin` intestazione, la richiesta tra le origini ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="a0586-283">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="a0586-284">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="a0586-284">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="a0586-285">Anche se il server restituisce una risposta con esito positivo, il browser non rende disponibile la risposta all'app client.</span><span class="sxs-lookup"><span data-stu-id="a0586-285">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="a0586-286">Test CORS</span><span class="sxs-lookup"><span data-stu-id="a0586-286">Test CORS</span></span>

<span data-ttu-id="a0586-287">Per testare CORS:</span><span class="sxs-lookup"><span data-stu-id="a0586-287">To test CORS:</span></span>

1. <span data-ttu-id="a0586-288">[Creare un progetto API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="a0586-288">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="a0586-289">In alternativa, è possibile [scaricare l'esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="a0586-289">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="a0586-290">Abilitare CORS usando uno degli approcci descritti in questo documento.</span><span class="sxs-lookup"><span data-stu-id="a0586-290">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="a0586-291">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a0586-291">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="a0586-292">`WithOrigins("https://localhost:<port>");`deve essere usato solo per il test di un'app di esempio simile al [codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)per il download.</span><span class="sxs-lookup"><span data-stu-id="a0586-292">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="a0586-293">Creare un progetto di app Web (Razor Pages o MVC).</span><span class="sxs-lookup"><span data-stu-id="a0586-293">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="a0586-294">Nell'esempio viene utilizzato Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a0586-294">The sample uses Razor Pages.</span></span> <span data-ttu-id="a0586-295">È possibile creare l'app Web nella stessa soluzione del progetto API.</span><span class="sxs-lookup"><span data-stu-id="a0586-295">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="a0586-296">Aggiungere il codice evidenziato seguente al file *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a0586-296">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="a0586-297">Nel codice precedente sostituire `url: 'https://<web app>.azurewebsites.net/api/values/1',` con l'URL dell'app distribuita.</span><span class="sxs-lookup"><span data-stu-id="a0586-297">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="a0586-298">Distribuire il progetto API.</span><span class="sxs-lookup"><span data-stu-id="a0586-298">Deploy the API project.</span></span> <span data-ttu-id="a0586-299">Ad esempio, [eseguire la distribuzione in Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="a0586-299">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="a0586-300">Eseguire l'app Razor Pages o MVC dal desktop e fare clic sul pulsante **test** .</span><span class="sxs-lookup"><span data-stu-id="a0586-300">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="a0586-301">Usare gli strumenti F12 per esaminare i messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="a0586-301">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="a0586-302">Rimuovere l'origine localhost da `WithOrigins` e distribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="a0586-302">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="a0586-303">In alternativa, eseguire l'app client con una porta diversa.</span><span class="sxs-lookup"><span data-stu-id="a0586-303">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="a0586-304">Ad esempio, eseguire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0586-304">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="a0586-305">Eseguire il test con l'app client.</span><span class="sxs-lookup"><span data-stu-id="a0586-305">Test with the client app.</span></span> <span data-ttu-id="a0586-306">Gli errori CORS restituiscono un errore, ma il messaggio di errore non è disponibile per JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a0586-306">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="a0586-307">Usare la scheda console negli strumenti F12 per visualizzare l'errore.</span><span class="sxs-lookup"><span data-stu-id="a0586-307">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="a0586-308">A seconda del browser, viene ricevuto un errore (nella console strumenti F12) simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a0586-308">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="a0586-309">Uso di Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="a0586-309">Using Microsoft Edge:</span></span>

     <span data-ttu-id="a0586-310">**SEC7120: [CORS] l'origine `https://localhost:44375` non è stata `https://localhost:44375` trovata nell'intestazione della risposta Access-Control-Allow-Origin per la risorsa tra le origini in`https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="a0586-310">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="a0586-311">Uso di Chrome:</span><span class="sxs-lookup"><span data-stu-id="a0586-311">Using Chrome:</span></span>

     <span data-ttu-id="a0586-312">**L'accesso a XMLHttpRequest `https://webapi.azurewebsites.net/api/values/1` da Origin `https://localhost:44375` è stato bloccato dal criterio CORS: Non è presente alcuna intestazione ' Access-Control-Allow-Origin ' nella risorsa richiesta.**</span><span class="sxs-lookup"><span data-stu-id="a0586-312">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a0586-313">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a0586-313">Additional resources</span></span>

* [<span data-ttu-id="a0586-314">Condivisione di risorse tra le origini (CORS)</span><span class="sxs-lookup"><span data-stu-id="a0586-314">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
