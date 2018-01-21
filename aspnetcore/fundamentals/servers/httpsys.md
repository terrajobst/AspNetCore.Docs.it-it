---
title: Implementazione del server web HTTP.sys in ASP.NET Core
author: rick-anderson
description: "Introduce HTTP.sys, un server web per ASP.NET Core in Windows. Basato sul driver Http.Sys in modalità kernel, HTTP.sys è un'alternativa a Kestrel che può essere utilizzato per la connessione diretta a Internet senza IIS."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 60301e1e3eb96f51e86ef9f8be61f5fd8a4c009c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="6fdce-104">Implementazione del server web HTTP.sys in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6fdce-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="6fdce-105">Da [Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="6fdce-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="6fdce-106">Questo argomento si applica solo ai componenti di base di ASP.NET 2.0 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="6fdce-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="6fdce-107">Nelle versioni precedenti di ASP.NET Core, denominato HTTP.sys [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="6fdce-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="6fdce-108">HTTP.sys è un [server web per ASP.NET Core](index.md) che viene eseguita solo su Windows.</span><span class="sxs-lookup"><span data-stu-id="6fdce-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="6fdce-109">Si basa sul [driver in modalità kernel HTTP. sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fdce-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="6fdce-110">HTTP.sys è un'alternativa a [Kestrel](kestrel.md) che offre alcune funzionalità che non Kestel.</span><span class="sxs-lookup"><span data-stu-id="6fdce-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="6fdce-111">**HTTP.sys non può essere utilizzato con IIS o IIS Express, che non è compatibile con il [ASP.NET Core modulo](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="6fdce-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="6fdce-112">HTTP.sys supporta le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="6fdce-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="6fdce-113">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="6fdce-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="6fdce-114">Condivisione delle porte</span><span class="sxs-lookup"><span data-stu-id="6fdce-114">Port sharing</span></span>
- <span data-ttu-id="6fdce-115">Utilizzo di HTTPS con SNI</span><span class="sxs-lookup"><span data-stu-id="6fdce-115">HTTPS with SNI</span></span>
- <span data-ttu-id="6fdce-116">HTTP/2 tramite TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="6fdce-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="6fdce-117">Trasmissione diretta del file</span><span class="sxs-lookup"><span data-stu-id="6fdce-117">Direct file transmission</span></span>
- <span data-ttu-id="6fdce-118">La memorizzazione nella cache di risposta</span><span class="sxs-lookup"><span data-stu-id="6fdce-118">Response caching</span></span>
- <span data-ttu-id="6fdce-119">WebSocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="6fdce-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="6fdce-120">Versioni supportate di Windows:</span><span class="sxs-lookup"><span data-stu-id="6fdce-120">Supported Windows versions:</span></span>

- <span data-ttu-id="6fdce-121">Windows 7 e Windows Server 2008 R2 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="6fdce-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="6fdce-122">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6fdce-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="6fdce-123">Quando utilizzare HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6fdce-123">When to use HTTP.sys</span></span>

<span data-ttu-id="6fdce-124">HTTP.sys è utile per le distribuzioni in cui è necessario esporre il server direttamente a Internet senza l'utilizzo di IIS.</span><span class="sxs-lookup"><span data-stu-id="6fdce-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys comunica direttamente con Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="6fdce-126">Poiché è stato progettato su Http.Sys, HTTP.sys non richiede un server proxy inverso per la protezione contro gli attacchi.</span><span class="sxs-lookup"><span data-stu-id="6fdce-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="6fdce-127">Http.Sys è una tecnologia avanzata che consente di proteggere molti tipi di attacchi e fornisce l'affidabilità, sicurezza e la scalabilità di un server web con caratteristiche complete.</span><span class="sxs-lookup"><span data-stu-id="6fdce-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="6fdce-128">IIS viene eseguito come un listener HTTP in Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="6fdce-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="6fdce-129">HTTP.sys è una scelta ottimale per le distribuzioni interne quando occorre una funzionalità non disponibile in Kestrel, ad esempio l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="6fdce-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys comunica direttamente con la rete interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="6fdce-131">Come usare HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6fdce-131">How to use HTTP.sys</span></span>

<span data-ttu-id="6fdce-132">Di seguito viene fornita una panoramica delle attività di configurazione per il sistema operativo host e l'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6fdce-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="6fdce-133">Configurare Windows Server</span><span class="sxs-lookup"><span data-stu-id="6fdce-133">Configure Windows Server</span></span>

* <span data-ttu-id="6fdce-134">Installare la versione di .NET che richiede l'applicazione, ad esempio [.NET Core](https://www.microsoft.com/net/download/core) o [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="6fdce-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="6fdce-135">Pre-registrare prefissi URL per associare a HTTP.sys e configurare i certificati SSL</span><span class="sxs-lookup"><span data-stu-id="6fdce-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="6fdce-136">Se si non pre-registrare prefissi URL in Windows, è necessario eseguire l'applicazione con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="6fdce-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="6fdce-137">L'unica eccezione è se si associa a localhost tramite HTTP (non HTTPS) con un numero di porta maggiore di 1024; In tal caso, i privilegi di amministratore non sono necessari.</span><span class="sxs-lookup"><span data-stu-id="6fdce-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="6fdce-138">Per informazioni dettagliate, vedere [come pre-registrare prefissi e configurare SSL](#preregister-url-prefixes-and-configure-ssl) più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="6fdce-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="6fdce-139">Aprire le porte del firewall per consentire il traffico raggiungere HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="6fdce-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="6fdce-140">È possibile utilizzare *netsh.exe* o [i cmdlet di PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="6fdce-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="6fdce-141">Sono inoltre disponibili [impostazioni del Registro di sistema Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="6fdce-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="6fdce-142">Configurare l'applicazione ASP.NET Core per utilizzare HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="6fdce-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="6fdce-143">Installazione del pacchetto non è necessaria se si usa il [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="6fdce-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="6fdce-144">Il [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) pacchetto è incluso nel metapackage.</span><span class="sxs-lookup"><span data-stu-id="6fdce-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="6fdce-145">Chiamare il `UseHttpSys` metodo di estensione su `WebHostBuilder` nel `Main` metodo, specificando uno [HTTP.sys opzioni](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) che è necessario, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="6fdce-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="6fdce-146">Configurare le opzioni di HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6fdce-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="6fdce-147">Di seguito sono riportate alcune impostazioni HTTP.sys e i limiti che è possibile configurare.</span><span class="sxs-lookup"><span data-stu-id="6fdce-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="6fdce-148">**Connessioni client massima**</span><span class="sxs-lookup"><span data-stu-id="6fdce-148">**Maximum client connections**</span></span>

<span data-ttu-id="6fdce-149">Impostare il numero massimo di connessioni simultanee aperte TCP per l'intera applicazione con il codice seguente in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="6fdce-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="6fdce-150">Il numero massimo di connessioni è illimitato (null) per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6fdce-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="6fdce-151">**Dimensione massima del corpo della richiesta**</span><span class="sxs-lookup"><span data-stu-id="6fdce-151">**Maximum request body size**</span></span>

<span data-ttu-id="6fdce-152">Dimensioni predefinite del corpo massima della richiesta sono 30,000,000 byte, ovvero circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="6fdce-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="6fdce-153">Il metodo consigliato per ignorare il limite in un'applicazione ASP.NET MVC di base consiste nell'utilizzare il [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attributo in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="6fdce-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="6fdce-154">Di seguito è riportato un esempio che illustra come configurare il vincolo per l'intera applicazione, ogni richiesta:</span><span class="sxs-lookup"><span data-stu-id="6fdce-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="6fdce-155">È possibile ignorare l'impostazione di una richiesta specifica in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6fdce-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="6fdce-156">Se si tenta di configurare il limite per una richiesta dopo l'applicazione ha avviato la richiesta di lettura, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="6fdce-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="6fdce-157">È presente un `IsReadOnly` proprietà che indica se il `MaxRequestBodySize` proprietà è in stato di sola lettura, pertanto è troppo tardi per configurare il limite.</span><span class="sxs-lookup"><span data-stu-id="6fdce-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="6fdce-158">Per informazioni sulle altre opzioni di HTTP.sys, vedere [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="6fdce-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="6fdce-159">Configurare gli URL e le porte per l'attesa su</span><span class="sxs-lookup"><span data-stu-id="6fdce-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="6fdce-160">Per impostazione predefinita ASP.NET Core associa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6fdce-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="6fdce-161">Per configurare le porte e i prefissi URL, è possibile utilizzare il `UseUrls` metodo di estensione, il `urls` argomento della riga di comando, la variabile di ambiente ASPNETCORE_URLS o `UrlPrefixes` proprietà [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="6fdce-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="6fdce-162">Nell'esempio di codice viene illustrato come utilizzare `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="6fdce-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="6fdce-163">Un vantaggio offerto da `UrlPrefixes` ottengono immediatamente un messaggio di errore se si tenta di aggiungere un prefisso che viene formattato errato.</span><span class="sxs-lookup"><span data-stu-id="6fdce-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="6fdce-164">Un vantaggio offerto da `UseUrls` (condiviso con `urls` e ASPNETCORE_URLS) è che è possibile passare più facilmente tra Kestrel e HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="6fdce-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="6fdce-165">Se si utilizzano entrambi `UseUrls` (o `urls` o ASPNETCORE_URLS) e `UrlPrefixes`, le impostazioni di `UrlPrefixes` eseguire l'override di quelli nei `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6fdce-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="6fdce-166">Per altre informazioni, vedere [Hosting](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="6fdce-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="6fdce-167">HTTP.sys Usa il [formati stringa HTTP Server API UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fdce-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="6fdce-168">Assicurarsi di specificare le stesse stringhe di prefisso in `UseUrls` o `UrlPrefixes` che pre-registrare il server.</span><span class="sxs-lookup"><span data-stu-id="6fdce-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="6fdce-169">Non utilizzare IIS</span><span class="sxs-lookup"><span data-stu-id="6fdce-169">Don't use IIS</span></span>

<span data-ttu-id="6fdce-170">Verificare che l'applicazione non è configurato per l'esecuzione di IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6fdce-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="6fdce-171">In Visual Studio, il profilo di avvio predefinito è per IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6fdce-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="6fdce-172">Per eseguire il progetto come applicazione console, modificare manualmente il profilo selezionato, come illustrato nella schermata seguente.</span><span class="sxs-lookup"><span data-stu-id="6fdce-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Selezionare il profilo di applicazione console](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="6fdce-174">Pre-registrare prefissi URL e configurare SSL</span><span class="sxs-lookup"><span data-stu-id="6fdce-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="6fdce-175">Sia IIS sia HTTP.sys si basano su driver sottostante in modalità kernel HTTP. sys in ascolto delle richieste e di elaborazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="6fdce-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="6fdce-176">In IIS, la gestione dell'interfaccia utente fornisce un modo semplice per configurare tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="6fdce-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="6fdce-177">Tuttavia, è necessario configurare manualmente Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="6fdce-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="6fdce-178">Lo strumento incorporato al riguardo sono *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="6fdce-178">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="6fdce-179">Con *netsh.exe* è possibile riservare i prefissi URL e assegnare i certificati SSL.</span><span class="sxs-lookup"><span data-stu-id="6fdce-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="6fdce-180">Lo strumento richiede privilegi amministrativi.</span><span class="sxs-lookup"><span data-stu-id="6fdce-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="6fdce-181">Nell'esempio seguente viene illustrato il requisito minimo necessario per riservare i prefissi URL per le porte 80 e 443:</span><span class="sxs-lookup"><span data-stu-id="6fdce-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="6fdce-182">Nell'esempio seguente viene illustrato come assegnare un certificato SSL:</span><span class="sxs-lookup"><span data-stu-id="6fdce-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="6fdce-183">Ecco la documentazione di riferimento per *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="6fdce-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="6fdce-184">Comandi Netsh per Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="6fdce-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="6fdce-185">UrlPrefix stringhe</span><span class="sxs-lookup"><span data-stu-id="6fdce-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="6fdce-186">Le risorse seguenti forniscono istruzioni dettagliate per scenari diversi.</span><span class="sxs-lookup"><span data-stu-id="6fdce-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="6fdce-187">Gli articoli che fanno riferimento a HttpListener applicano ugualmente a HTTP.sys, come entrambe basate su HTTP. sys.</span><span class="sxs-lookup"><span data-stu-id="6fdce-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="6fdce-188">Procedura: Configurare una porta con un certificato SSL</span><span class="sxs-lookup"><span data-stu-id="6fdce-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="6fdce-189">[Le comunicazioni HTTPS - HttpListener basato su host e la certificazione Client](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) questo è un blog di terze parti e si è piuttosto precedente ma si dispone ancora di informazioni utili.</span><span class="sxs-lookup"><span data-stu-id="6fdce-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="6fdce-190">[Procedura: HttpListener utilizzando questa procedura dettagliata o un Http Server codice non gestito (C++) come Server semplice SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) anche questo va un blog precedente con informazioni utili.</span><span class="sxs-lookup"><span data-stu-id="6fdce-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="6fdce-191">Ecco alcuni strumenti di terze parti che possono essere più facili da utilizzare più di *netsh.exe* riga di comando.</span><span class="sxs-lookup"><span data-stu-id="6fdce-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="6fdce-192">Questi non vengono forniti da o approvati da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6fdce-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="6fdce-193">Gli strumenti di Esegui come amministratore per impostazione predefinita, poiché *netsh.exe* stesso richiede privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="6fdce-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="6fdce-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) fornisce un'interfaccia utente per l'elenco e la configurazione di opzioni e i certificati SSL, le prenotazioni del prefisso ed elenchi di certificati attendibili.</span><span class="sxs-lookup"><span data-stu-id="6fdce-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="6fdce-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) consente di elencare o configurare i certificati SSL e i prefissi URL.</span><span class="sxs-lookup"><span data-stu-id="6fdce-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="6fdce-196">L'interfaccia utente è più precise di http.sys Manager ed espone alcuni ulteriori opzioni di configurazione, ma in caso contrario fornisce funzionalità simili.</span><span class="sxs-lookup"><span data-stu-id="6fdce-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="6fdce-197">Non è possibile creare un nuovo elenco di certificati attendibili (CTL), ma può assegnare a quelli esistenti.</span><span class="sxs-lookup"><span data-stu-id="6fdce-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="6fdce-198">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6fdce-198">Next steps</span></span>

<span data-ttu-id="6fdce-199">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="6fdce-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="6fdce-200">Applicazione di esempio per questo articolo</span><span class="sxs-lookup"><span data-stu-id="6fdce-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="6fdce-201">Codice sorgente HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6fdce-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="6fdce-202">Hosting</span><span class="sxs-lookup"><span data-stu-id="6fdce-202">Hosting</span></span>](xref:fundamentals/hosting)
