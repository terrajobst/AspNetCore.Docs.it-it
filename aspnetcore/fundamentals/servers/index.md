---
title: Implementazioni di server Web in ASP.NET Core
author: tdykstra
description: Introduce i server Web Kestrel e WebListener per ASP.NET Core. Offre indicazioni su come scegliere uno dei server e quando usarlo con un server proxy inverso.
manager: wpickett
ms.author: tdykstra
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: 9e2bea396e50615bd02affad93f0ee55255d299f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="38950-104">Implementazioni di server Web in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="38950-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="38950-105">Di [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="38950-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="38950-106">Un'applicazione ASP.NET Core viene eseguita con un'implementazione del server HTTP in-process.</span><span class="sxs-lookup"><span data-stu-id="38950-106">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="38950-107">L'implementazione del server attende le richieste HTTP e le rende visibili all'applicazione come set di [funzionalità di richiesta](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) combinate in un oggetto `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="38950-107">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="38950-108">Ad ASP.NET Core sono accluse due implementazioni di server:</span><span class="sxs-lookup"><span data-stu-id="38950-108">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="38950-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="38950-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="38950-110">[Kestrel](kestrel.md) è un server HTTP multipiattaforma basato su [libuv](https://github.com/libuv/libuv), una libreria di I/O asincrona multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="38950-110">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="38950-111">[HTTP.sys](httpsys.md) è un server HTTP solo per Windows basato sul [driver del kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="38950-111">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="38950-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="38950-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="38950-113">[Kestrel](kestrel.md) è un server HTTP multipiattaforma basato su [libuv](https://github.com/libuv/libuv), una libreria di I/O asincrona multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="38950-113">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="38950-114">[WebListener](weblistener.md) è un server HTTP solo per Windows basato sul [driver del kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="38950-114">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="38950-115">Kestrel</span><span class="sxs-lookup"><span data-stu-id="38950-115">Kestrel</span></span>

<span data-ttu-id="38950-116">Kestrel è il server Web incluso per impostazione predefinita nei modelli di nuovo progetto di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="38950-116">Kestrel is the web server that's included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="38950-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="38950-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="38950-118">È possibile usare Kestrel da solo o con un *server proxy inverso*, ad esempio IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="38950-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="38950-119">Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.</span><span class="sxs-lookup"><span data-stu-id="38950-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="38950-122">È possibile usare una delle due configurazioni &mdash; con o senza un server proxy inverso &mdash; anche se Kestrel viene esposto solo a una rete interna.</span><span class="sxs-lookup"><span data-stu-id="38950-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="38950-123">Per informazioni su quando usare Kestrel con un proxy inverso, vedere l'[introduzione a Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="38950-123">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="38950-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="38950-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="38950-125">Se l'applicazione accetta le richieste solo da una rete interna, è possibile usare Kestrel da solo.</span><span class="sxs-lookup"><span data-stu-id="38950-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel comunica direttamente con la rete interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="38950-127">Se si espone l'applicazione a Internet, è necessario usare IIS, Nginx o Apache come *server proxy inverso*.</span><span class="sxs-lookup"><span data-stu-id="38950-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="38950-128">Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari, come illustrato nel diagramma che segue.</span><span class="sxs-lookup"><span data-stu-id="38950-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="38950-130">Il motivo principale per cui si usa un proxy inverso per le distribuzioni perimetrali, ovvero esposte al traffico da Internet, è la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="38950-130">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="38950-131">Nelle versioni 1. x di Kestrel non è disponibile una serie completa di difese contro gli attacchi.</span><span class="sxs-lookup"><span data-stu-id="38950-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="38950-132">Questo include, senza limitazioni, timeout appropriati, limiti delle dimensioni e limiti delle connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="38950-132">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="38950-133">Per informazioni su quando usare Kestrel con un proxy inverso, vedere l'[introduzione a Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="38950-133">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="38950-134">Non è possibile usare IIS, Nginx o Apache senza Kestrel o un'[implementazione del server personalizzata](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="38950-134">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="38950-135">ASP.NET Core è stato progettato per essere eseguito nel proprio processo in modo che il comportamento sia coerente tra le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="38950-135">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="38950-136">IIS, Nginx e Apache impongono il proprio processo di avvio e il proprio ambiente: per usarli direttamente, ASP.NET Core dovrebbe adattarsi alle esigenze di ognuno di essi.</span><span class="sxs-lookup"><span data-stu-id="38950-136">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="38950-137">L'uso di un'implementazione del server Web come Kestrel consente ad ASP.NET Core di controllare il processo di avvio e l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="38950-137">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="38950-138">Quindi, anziché tentare di adattare ASP.NET Core a IIS, Nginx o Apache, è sufficiente impostare i server Web in modo che inoltrino le richieste a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="38950-138">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="38950-139">In questo modo le classi `Program.Main` e `Startup` possono essere essenzialmente le stesse indipendentemente da dove si esegue la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="38950-139">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="38950-140">IIS con Kestrel</span><span class="sxs-lookup"><span data-stu-id="38950-140">IIS with Kestrel</span></span>

<span data-ttu-id="38950-141">Quando si usa IIS o IIS Express come proxy inverso per ASP.NET Core, l'applicazione ASP.NET Core viene eseguita in un processo separato dal processo di lavoro di IIS.</span><span class="sxs-lookup"><span data-stu-id="38950-141">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="38950-142">Nel processo di IIS viene eseguito un modulo IIS speciale per coordinare la relazione di proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="38950-142">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="38950-143">Si tratta del *modulo ASP.NET Core*.</span><span class="sxs-lookup"><span data-stu-id="38950-143">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="38950-144">Le funzioni principali del modulo ASP.NET Core sono l'avvio dell'applicazione ASP.NET Core, il riavvio dell'applicazione quando si blocca e l'inoltro del traffico HTTP.</span><span class="sxs-lookup"><span data-stu-id="38950-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="38950-145">Per altre informazioni, vedere l'[introduzione al modulo ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="38950-145">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="38950-146">Nginx con Kestrel</span><span class="sxs-lookup"><span data-stu-id="38950-146">Nginx with Kestrel</span></span>

<span data-ttu-id="38950-147">Per informazioni sull'uso di Nginx in Linux come server proxy inverso per Kestrel, vedere [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx) (Hosting in Linux con Nginx).</span><span class="sxs-lookup"><span data-stu-id="38950-147">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="38950-148">Apache con Kestrel</span><span class="sxs-lookup"><span data-stu-id="38950-148">Apache with Kestrel</span></span>

<span data-ttu-id="38950-149">Per informazioni sull'uso di Apache in Linux come server proxy inverso per Kestrel, vedere [Host on Linux with Apache](xref:host-and-deploy/linux-apache) (Hosting in Linux con Apache).</span><span class="sxs-lookup"><span data-stu-id="38950-149">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="38950-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="38950-150">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="38950-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="38950-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="38950-152">Se si esegue l'app ASP.NET Core in Windows, HTTP.sys è un'alternativa a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="38950-152">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="38950-153">È possibile usare HTTP.sys per scenari in cui si espone l'app a Internet e sono necessarie funzionalità di HTTP.sys che Kestrel non supporta.</span><span class="sxs-lookup"><span data-stu-id="38950-153">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![HTTP.sys comunica direttamente con Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="38950-155">HTTP.sys può essere usato anche per le applicazioni che vengono esposte solo a una rete interna.</span><span class="sxs-lookup"><span data-stu-id="38950-155">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![HTTP.sys comunica direttamente con la rete interna](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="38950-157">Negli scenari con rete interna, in genere si consiglia Kestrel perché offre prestazioni ottimali, ma in alcuni scenari può essere preferibile una funzionalità offerta solo da HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="38950-157">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="38950-158">Per informazioni sulle funzionalità di HTTP.sys, vedere [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="38950-158">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="38950-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="38950-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="38950-160">HTTP.sys è denominato WebListener in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="38950-160">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="38950-161">Se si esegue l'app ASP.NET Core in Windows, WebListener è un'alternativa che è possibile usare in scenari in cui si vuole esporre l'app a Internet ma non si può usare IIS.</span><span class="sxs-lookup"><span data-stu-id="38950-161">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![WebListener comunica direttamente con Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="38950-163">WebListener può essere usato anche al posto di Kestrel per le applicazioni che vengono esposte solo a una rete interna, se sono necessarie funzionalità di WebListener non supportate da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="38950-163">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![Weblistener comunica direttamente con la rete interna](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="38950-165">Negli scenari con rete interna, in genere si consiglia Kestrel perché offre prestazioni ottimali, ma in alcuni scenari può essere preferibile una funzionalità offerta solo da WebListener.</span><span class="sxs-lookup"><span data-stu-id="38950-165">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="38950-166">Per informazioni sulle funzionalità di WebListener, vedere [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="38950-166">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="38950-167">Note sull'infrastruttura del server ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="38950-167">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="38950-168">L'oggetto [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder), disponibile nella classe `Startup` all'interno del metodo `Configure`, espone la proprietà `ServerFeatures` del tipo [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="38950-168">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="38950-169">Kestrel e WebListener espongono entrambi solo una singola funzionalità, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ma le diverse implementazioni del server possono esporre funzionalità aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="38950-169">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="38950-170">L'oggetto `IServerAddressesFeature` può essere usato per individuare la porta a cui è associata l'implementazione del server in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="38950-170">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="38950-171">Server personalizzati</span><span class="sxs-lookup"><span data-stu-id="38950-171">Custom servers</span></span>

<span data-ttu-id="38950-172">Se i server predefiniti non soddisfano le proprie esigenze, è possibile creare un'implementazione del server personalizzata.</span><span class="sxs-lookup"><span data-stu-id="38950-172">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="38950-173">La [guida all'interfaccia OWIN (Open Web Interface for .NET)](../owin.md) spiega come scrivere un'implementazione [Nowin](https://github.com/Bobris/Nowin) di [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver).</span><span class="sxs-lookup"><span data-stu-id="38950-173">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="38950-174">Si ha la possibilità di implementare solo le interfacce di funzionalità richieste dall'applicazione, anche se è necessario il supporto almeno per [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="38950-174">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="38950-175">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="38950-175">Next steps</span></span>

<span data-ttu-id="38950-176">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="38950-176">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="38950-177">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="38950-177">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="38950-178">Kestrel</span><span class="sxs-lookup"><span data-stu-id="38950-178">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="38950-179">Kestrel con IIS</span><span class="sxs-lookup"><span data-stu-id="38950-179">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="38950-180">Host in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="38950-180">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="38950-181">Host in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="38950-181">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="38950-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="38950-182">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="38950-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="38950-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="38950-184">Kestrel</span><span class="sxs-lookup"><span data-stu-id="38950-184">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="38950-185">Kestrel con IIS</span><span class="sxs-lookup"><span data-stu-id="38950-185">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="38950-186">Host in Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="38950-186">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="38950-187">Host in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="38950-187">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="38950-188">WebListener</span><span class="sxs-lookup"><span data-stu-id="38950-188">WebListener</span></span>](weblistener.md)

---
