---
title: Implementazioni di server Web in ASP.NET Core
author: guardrex
description: Individuare i server Web Kestrel e HTTP.sys per ASP.NET Core. Informazioni su come scegliere un server e quando usare un server proxy inverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 06d4bf09b07fc70a10b3e260e78c29fe189486c5
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505726"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="2dd56-104">Implementazioni di server Web in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2dd56-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="2dd56-105">Di [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="2dd56-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="2dd56-106">Un'app ASP.NET Core viene eseguita con un'implementazione del server HTTP in-process.</span><span class="sxs-lookup"><span data-stu-id="2dd56-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="2dd56-107">L'implementazione del server attende le richieste HTTP e le rende visibili all'app come set di [funzionalità di richiesta](xref:fundamentals/request-features) combinate in un <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="2dd56-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="2dd56-108">ASP.NET Core viene fornito con le implementazioni server seguenti:</span><span class="sxs-lookup"><span data-stu-id="2dd56-108">ASP.NET Core ships with the following server implementations:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="2dd56-109">[Kestrel](xref:fundamentals/servers/kestrel) è il server HTTP multipiattaforma predefinito per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2dd56-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="2dd56-110">`IISHttpServer` viene usato con il [modello di hosting in-process](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) e il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) in Windows.</span><span class="sxs-lookup"><span data-stu-id="2dd56-110">`IISHttpServer` is used with the [in-process hosting model](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) on Windows.</span></span>
* <span data-ttu-id="2dd56-111">[HTTP.sys](xref:fundamentals/servers/httpsys) è un server HTTP solo per Windows basato sul [driver del kernel HTTP.Sys e l'API HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="2dd56-111">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="2dd56-112">(HTTP.sys è denominato [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span><span class="sxs-lookup"><span data-stu-id="2dd56-112">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="2dd56-113">[Kestrel](xref:fundamentals/servers/kestrel) è il server HTTP multipiattaforma predefinito per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2dd56-113">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="2dd56-114">[HTTP.sys](xref:fundamentals/servers/httpsys) è un server HTTP solo per Windows basato sul [driver del kernel HTTP.Sys e l'API HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="2dd56-114">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="2dd56-115">(HTTP.sys è denominato [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span><span class="sxs-lookup"><span data-stu-id="2dd56-115">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="2dd56-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="2dd56-116">Kestrel</span></span>

<span data-ttu-id="2dd56-117">Kestrel è il server Web predefinito incluso nei modelli di progetto di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2dd56-117">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2dd56-118">È possibile usare Kestrel da solo o con un *server proxy inverso*, ad esempio IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="2dd56-118">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="2dd56-119">Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.</span><span class="sxs-lookup"><span data-stu-id="2dd56-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="2dd56-122">Entrambe le configurazioni (con o senza un server proxy inverso) sono configurazioni di hosting valide e supportate per le app ASP.NET Core 2.0 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="2dd56-122">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="2dd56-123">Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="2dd56-123">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2dd56-124">Se l'app accetta solo le richieste provenienti da una rete interna, Kestrel è utilizzabile in modo autonomo.</span><span class="sxs-lookup"><span data-stu-id="2dd56-124">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel comunica direttamente con la rete interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="2dd56-126">Se si espone l'app a Internet, Kestrel deve usare IIS, Nginx o Apache come *server proxy inverso*.</span><span class="sxs-lookup"><span data-stu-id="2dd56-126">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="2dd56-127">Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari, come illustrato nel diagramma che segue:</span><span class="sxs-lookup"><span data-stu-id="2dd56-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="2dd56-129">Il motivo principale per cui si usa un proxy inverso per le distribuzioni server perimetrali pubbliche, esposte direttamente a Internet, è la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="2dd56-129">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="2dd56-130">Le versioni 1.x di Kestrel non includono funzionalità di sicurezza importanti per difendersi da attacchi da Internet,</span><span class="sxs-lookup"><span data-stu-id="2dd56-130">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="2dd56-131">ad esempio timeout appropriati, limiti delle dimensioni delle richieste e limiti delle connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="2dd56-131">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="2dd56-132">Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="2dd56-132">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

<span data-ttu-id="2dd56-133">Non è possibile usare IIS, Nginx e Apache senza Kestrel o un'[implementazione del server personalizzata](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="2dd56-133">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="2dd56-134">ASP.NET Core è stato progettato per essere eseguito nel proprio processo in modo che il comportamento sia coerente tra le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="2dd56-134">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="2dd56-135">IIS, Nginx e Apache stabiliscono procedure di avvio e ambiente propri.</span><span class="sxs-lookup"><span data-stu-id="2dd56-135">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="2dd56-136">Per usare queste tecnologie server direttamente, ASP.NET Core dovrà adattarsi ai requisiti di ogni server.</span><span class="sxs-lookup"><span data-stu-id="2dd56-136">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="2dd56-137">L'uso di un'implementazione del server Web come Kestrel consente ad ASP.NET Core di controllare il processo di avvio e l'ambiente quando viene ospitato in tecnologie server diverse.</span><span class="sxs-lookup"><span data-stu-id="2dd56-137">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="2dd56-138">IIS con Kestrel</span><span class="sxs-lookup"><span data-stu-id="2dd56-138">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2dd56-139">Se si usa [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), l'app ASP.NET Core può essere eseguita nello stesso processo del processo di lavoro IIS (modello di hosting *in-process*) o in un processo separato dal processo di lavoro IIS (modello di hosting *out-of-process*).</span><span class="sxs-lookup"><span data-stu-id="2dd56-139">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="2dd56-140">Il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) è un modulo IIS nativo che gestisce le richieste IIS native con il server HTTP dell'app IIS in-process o il server Kestrel out-of-process.</span><span class="sxs-lookup"><span data-stu-id="2dd56-140">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS Http Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="2dd56-141">Per ulteriori informazioni, vedere <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="2dd56-141">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2dd56-142">Quando si usa [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) come proxy inverso per ASP.NET Core, l'app ASP.NET Core viene eseguita in un processo separato dal processo di lavoro di IIS.</span><span class="sxs-lookup"><span data-stu-id="2dd56-142">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="2dd56-143">Nel processo di IIS, il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordina la relazione di proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="2dd56-143">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="2dd56-144">Le funzioni principali del modulo ASP.NET Core sono l'avvio dell'app ASP.NET Core, il riavvio dell'app quando si blocca e l'inoltro del traffico HTTP all'app.</span><span class="sxs-lookup"><span data-stu-id="2dd56-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="2dd56-145">Per ulteriori informazioni, vedere <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="2dd56-145">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="2dd56-146">Nginx con Kestrel</span><span class="sxs-lookup"><span data-stu-id="2dd56-146">Nginx with Kestrel</span></span>

<span data-ttu-id="2dd56-147">Per informazioni su come usare Nginx in Linux come server proxy inverso per Kestrel, vedere [Host in Linux con Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="2dd56-147">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="2dd56-148">Apache con Kestrel</span><span class="sxs-lookup"><span data-stu-id="2dd56-148">Apache with Kestrel</span></span>

<span data-ttu-id="2dd56-149">Per informazioni su come usare Apache in Linux come server proxy inverso per Kestrel, vedere [Host in Linux con Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="2dd56-149">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="2dd56-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="2dd56-150">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2dd56-151">Se si eseguono app ASP.NET Core in Windows, HTTP.sys è un'alternativa a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2dd56-151">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="2dd56-152">Kestrel è in genere consigliato per ottenere prestazioni ottimali.</span><span class="sxs-lookup"><span data-stu-id="2dd56-152">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="2dd56-153">HTTP.sys è utilizzabile negli scenari in cui l'app viene esposta a Internet e le funzionalità necessarie sono supportate da HTTP.sys, ma non da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2dd56-153">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="2dd56-154">Per informazioni su HTTP.sys, vedere [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="2dd56-154">For information on HTTP.sys, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![HTTP.sys comunica direttamente con Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="2dd56-156">HTTP.sys può essere usato anche per le app che vengono esposte solo a una rete interna.</span><span class="sxs-lookup"><span data-stu-id="2dd56-156">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys comunica direttamente con la rete interna](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2dd56-158">HTTP.sys è denominato [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="2dd56-158">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="2dd56-159">Se le app ASP.NET Core vengono eseguite in Windows, WebListener rappresenta un'alternativa per gli scenari in cui IIS non è disponibile per l'hosting delle app.</span><span class="sxs-lookup"><span data-stu-id="2dd56-159">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![WebListener comunica direttamente con Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="2dd56-161">WebListener può essere usato anche al posto di Kestrel per le app che vengono esposte solo a una rete interna, se le funzionalità necessarie sono supportate da WebListener ma non da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2dd56-161">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="2dd56-162">Per informazioni su WebListener, vedere [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="2dd56-162">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![Weblistener comunica direttamente con la rete interna](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="2dd56-164">Infrastruttura del server ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2dd56-164">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="2dd56-165">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder), disponibile nel metodo `Startup.Configure`, espone la proprietà [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) di tipo [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="2dd56-165">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="2dd56-166">Kestrel e HTTP. sys (WebListener in ASP.NET Core 1.x) espongono solo una singola funzionalità ognuno, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ma implementazioni server diverse potrebbero esporre funzionalità aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="2dd56-166">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="2dd56-167">È possibile usare `IServerAddressesFeature` per individuare la porta a cui è associata l'implementazione server in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2dd56-167">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="2dd56-168">Server personalizzati</span><span class="sxs-lookup"><span data-stu-id="2dd56-168">Custom servers</span></span>

<span data-ttu-id="2dd56-169">Se i server predefiniti non soddisfano i requisiti dell'app, è possibile creare un'implementazione server personalizzata.</span><span class="sxs-lookup"><span data-stu-id="2dd56-169">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="2dd56-170">La [guida all'interfaccia OWIN (Open Web Interface for .NET)](xref:fundamentals/owin) spiega come scrivere un'implementazione [Nowin](https://github.com/Bobris/Nowin) di [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver).</span><span class="sxs-lookup"><span data-stu-id="2dd56-170">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="2dd56-171">Solo le interfacce delle funzionalità usate dall'app devono essere implementate, anche se è richiesto come minimo il supporto di [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="2dd56-171">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="2dd56-172">Avvio del server</span><span class="sxs-lookup"><span data-stu-id="2dd56-172">Server startup</span></span>

<span data-ttu-id="2dd56-173">Quando si usa [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio per Mac](https://www.visualstudio.com/vs/mac/) o [Visual Studio Code](https://code.visualstudio.com/), il server viene avviato all'avvio dell'app dall'ambiente di sviluppo integrato (IDE).</span><span class="sxs-lookup"><span data-stu-id="2dd56-173">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="2dd56-174">In Visual Studio in Windows, è possibile usare i profili di avvio per avviare l'app e il server con [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) o la console.</span><span class="sxs-lookup"><span data-stu-id="2dd56-174">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="2dd56-175">In Visual Studio Code, l'app e il server vengono avviati da [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), che attiva il debugger CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="2dd56-175">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="2dd56-176">In Visual Studio per Mac, l'app e il server vengono avviati mediante il [debugger in modalità Mono Soft](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="2dd56-176">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="2dd56-177">All'avvio di un'app dal prompt dei comandi nella cartella del progetto, [dotnet run](/dotnet/core/tools/dotnet-run) avvia l'app e il server (solo Kestrel e HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="2dd56-177">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="2dd56-178">La configurazione viene specificata dall'opzione `-c|--configuration`, impostata su `Debug` (valore predefinito) o su `Release`.</span><span class="sxs-lookup"><span data-stu-id="2dd56-178">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="2dd56-179">Se sono presenti profili di avvio in un file *launchSettings.json*, usare l'opzione `--launch-profile <NAME>` per impostare il profilo di avvio (ad esempio, `Development` o `Production`).</span><span class="sxs-lookup"><span data-stu-id="2dd56-179">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="2dd56-180">Per altre informazioni, vedere gli argomenti [dotnet run](/dotnet/core/tools/dotnet-run) e [Creazione di pacchetti di distribuzione di .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="2dd56-180">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="2dd56-181">Supporto per HTTP/2</span><span class="sxs-lookup"><span data-stu-id="2dd56-181">HTTP/2 support</span></span>

<span data-ttu-id="2dd56-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) è supportato con ASP.NET Core negli scenari di distribuzione seguenti:</span><span class="sxs-lookup"><span data-stu-id="2dd56-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="2dd56-183">Kestrel</span><span class="sxs-lookup"><span data-stu-id="2dd56-183">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="2dd56-184">Sistema operativo</span><span class="sxs-lookup"><span data-stu-id="2dd56-184">Operating system</span></span>
    * <span data-ttu-id="2dd56-185">Windows Server 2016/Windows 10 o versioni successive&dagger;</span><span class="sxs-lookup"><span data-stu-id="2dd56-185">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="2dd56-186">Linux con OpenSSL 1.0.2 o versioni successive (ad esempio, Ubuntu 16.04 o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="2dd56-186">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="2dd56-187">HTTP/2 verrà supportato in macOS in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="2dd56-187">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="2dd56-188">Framework di destinazione: .NET Core 2.2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="2dd56-188">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="2dd56-189">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="2dd56-189">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="2dd56-190">Windows Server 2016/Windows 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="2dd56-190">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="2dd56-191">Framework di destinazione: non applicabile alle distribuzioni HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="2dd56-191">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="2dd56-192">IIS (in-process)</span><span class="sxs-lookup"><span data-stu-id="2dd56-192">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="2dd56-193">Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="2dd56-193">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="2dd56-194">Framework di destinazione: .NET Core 2.2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="2dd56-194">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="2dd56-195">IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="2dd56-195">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="2dd56-196">Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="2dd56-196">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="2dd56-197">Le connessioni server perimetrali pubbliche usano HTTP/2, mentre la connessione con proxy inverso a Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="2dd56-197">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="2dd56-198">Framework di destinazione: non applicabile alle distribuzioni IIS out-of-process.</span><span class="sxs-lookup"><span data-stu-id="2dd56-198">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="2dd56-199">&dagger;Kestrel ha un supporto limitato per HTTP/2 in Windows Server 2012 R2 e Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="2dd56-199">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="2dd56-200">Il supporto è limitato perché l'elenco di suite di crittografia TLS supportate disponibili in questi sistemi operativi è limitato.</span><span class="sxs-lookup"><span data-stu-id="2dd56-200">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="2dd56-201">Un certificato generato con un algoritmo ECDSA potrebbe essere necessario per proteggere le connessioni TLS.</span><span class="sxs-lookup"><span data-stu-id="2dd56-201">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="2dd56-202">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="2dd56-202">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="2dd56-203">Windows Server 2016/Windows 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="2dd56-203">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="2dd56-204">Framework di destinazione: non applicabile alle distribuzioni HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="2dd56-204">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="2dd56-205">IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="2dd56-205">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="2dd56-206">Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="2dd56-206">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="2dd56-207">Le connessioni server perimetrali pubbliche usano HTTP/2, mentre la connessione con proxy inverso a Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="2dd56-207">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="2dd56-208">Framework di destinazione: non applicabile alle distribuzioni IIS out-of-process.</span><span class="sxs-lookup"><span data-stu-id="2dd56-208">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="2dd56-209">Una connessione HTTP/2 deve usare [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3) e TLS 1.2 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="2dd56-209">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="2dd56-210">Per altre informazioni, vedere gli argomenti relativi agli scenari di distribuzione server.</span><span class="sxs-lookup"><span data-stu-id="2dd56-210">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2dd56-211">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2dd56-211">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="2dd56-212"><xref:fundamentals/servers/httpsys> (per ASP.NET Core 1.x, vedere <xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="2dd56-212"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
