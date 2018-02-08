---
title: Implementazione del server Web HTTP.sys in ASP.NET Core
author: rick-anderson
description: "Presentazione di HTTP.sys, un server Web per ASP.NET Core in Windows. Basato sul driver in modalità kernel Http.Sys, HTTP.sys è un'alternativa a Kestrel che consente la connessione diretta a Internet senza IIS."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="ac9b8-104">Implementazione del server Web HTTP.sys in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac9b8-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="ac9b8-105">[Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="ac9b8-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="ac9b8-106">Questo argomento si applica solo ad ASP.NET Core 2.0 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="ac9b8-107">Nelle versioni precedenti di ASP.NET Core, HTTP.sys è denominato [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="ac9b8-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="ac9b8-108">HTTP.sys è un [server Web per ASP.NET Core](index.md) che può essere eseguito solo in Windows.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="ac9b8-109">È basato sul [driver in modalità kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac9b8-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="ac9b8-110">HTTP.sys è un'alternativa a [Kestrel](kestrel.md) e offre alcune funzionalità non presenti in Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="ac9b8-111">**HTTP.sys non può essere usato con IIS o IIS Express, che non è compatibile con il [modulo ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="ac9b8-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="ac9b8-112">HTTP.sys supporta le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="ac9b8-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="ac9b8-113">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="ac9b8-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="ac9b8-114">Condivisione delle porte</span><span class="sxs-lookup"><span data-stu-id="ac9b8-114">Port sharing</span></span>
- <span data-ttu-id="ac9b8-115">HTTPS con SNI</span><span class="sxs-lookup"><span data-stu-id="ac9b8-115">HTTPS with SNI</span></span>
- <span data-ttu-id="ac9b8-116">HTTP/2 su TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="ac9b8-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="ac9b8-117">Trasmissione diretta dei file</span><span class="sxs-lookup"><span data-stu-id="ac9b8-117">Direct file transmission</span></span>
- <span data-ttu-id="ac9b8-118">Memorizzazione nella cache delle risposte</span><span class="sxs-lookup"><span data-stu-id="ac9b8-118">Response caching</span></span>
- <span data-ttu-id="ac9b8-119">WebSocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="ac9b8-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="ac9b8-120">Versioni di Windows supportate:</span><span class="sxs-lookup"><span data-stu-id="ac9b8-120">Supported Windows versions:</span></span>

- <span data-ttu-id="ac9b8-121">Windows 7 e Windows Server 2008 R2 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="ac9b8-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="ac9b8-122">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ac9b8-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="ac9b8-123">Quando usare HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ac9b8-123">When to use HTTP.sys</span></span>

<span data-ttu-id="ac9b8-124">HTTP.sys è utile per le distribuzioni in cui è necessario esporre il server direttamente a Internet senza l'utilizzo di IIS.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys comunica direttamente con Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="ac9b8-126">Poiché è basato su Http.Sys, HTTP.sys non richiede un server proxy inverso per la protezione dagli attacchi.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="ac9b8-127">Http.Sys è una tecnologia avanzata che protegge da molti tipi di attacchi e offre l'affidabilità, la sicurezza e la scalabilità di un server Web con funzionalità complete.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="ac9b8-128">IIS viene eseguito come listener HTTP in Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="ac9b8-129">HTTP.sys è una scelta ottimale per le distribuzioni interne quando è necessario usare una funzionalità non disponibile in Kestrel, ad esempio l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys comunica direttamente con la rete interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="ac9b8-131">Come usare HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ac9b8-131">How to use HTTP.sys</span></span>

<span data-ttu-id="ac9b8-132">Di seguito sono descritte le attività di configurazione per il sistema operativo host e l'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="ac9b8-133">Configurare Windows Server</span><span class="sxs-lookup"><span data-stu-id="ac9b8-133">Configure Windows Server</span></span>

* <span data-ttu-id="ac9b8-134">Installare la versione di .NET richiesta dall'applicazione, ad esempio [.NET Core](https://www.microsoft.com/net/download/core) o [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="ac9b8-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="ac9b8-135">Pre-registrare i prefissi URL per l'associazione a HTTP.sys e impostare i certificati SSL</span><span class="sxs-lookup"><span data-stu-id="ac9b8-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="ac9b8-136">Se non si pre-registrano i prefissi URL in Windows, è necessario eseguire l'applicazione con i privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="ac9b8-137">L'unica eccezione si verifica quando si esegue l'associazione a localhost usando HTTP (non HTTPS) con un numero di porta maggiore di 1024; in tal caso, i privilegi di amministratore non sono necessari.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="ac9b8-138">Per informazioni dettagliate, vedere [Pre-registrare i prefissi URL e configurare SSL](#preregister-url-prefixes-and-configure-ssl) più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="ac9b8-139">Aprire le porte del firewall per consentire al traffico di raggiungere HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="ac9b8-140">È possibile usare *netsh.exe* o i [cmdlet di PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="ac9b8-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="ac9b8-141">Sono anche disponibili le [Impostazioni del Registro di sistema HTTP. sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="ac9b8-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="ac9b8-142">Configurare l'applicazione ASP.NET Core per l'utilizzo di HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="ac9b8-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="ac9b8-143">Se si usa il metapacchetto [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) non è necessario installare alcun pacchetto.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="ac9b8-144">Il pacchetto [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) è incluso nel metapacchetto.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="ac9b8-145">Chiamare il metodo di estensione `UseHttpSys` in `WebHostBuilder` nel metodo `Main`, specificando le [opzioni di HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) necessarie, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="ac9b8-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="ac9b8-146">Configurare le opzioni di HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ac9b8-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="ac9b8-147">Di seguito sono riportate alcune impostazioni di HTTP.sys e i limiti che è possibile configurare.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="ac9b8-148">**Numero massimo di connessioni client**</span><span class="sxs-lookup"><span data-stu-id="ac9b8-148">**Maximum client connections**</span></span>

<span data-ttu-id="ac9b8-149">È possibile impostare il numero massimo di connessioni TCP aperte simultaneamente per l'intera applicazione con il seguente codice in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ac9b8-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="ac9b8-150">Per impostazione predefinita, il numero massimo di connessioni è illimitato (null).</span><span class="sxs-lookup"><span data-stu-id="ac9b8-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="ac9b8-151">**Dimensione massima del corpo della richiesta**</span><span class="sxs-lookup"><span data-stu-id="ac9b8-151">**Maximum request body size**</span></span>

<span data-ttu-id="ac9b8-152">La dimensione massima predefinita del corpo della richiesta è pari a 30.000.000 byte, equivalenti a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="ac9b8-153">Il metodo consigliato per ignorare il limite in un'applicazione ASP.NET Core MVC è l'uso dell'attributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="ac9b8-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="ac9b8-154">L'esempio seguente mostra come configurare il vincolo per l'intera applicazione e ogni richiesta:</span><span class="sxs-lookup"><span data-stu-id="ac9b8-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="ac9b8-155">È possibile eseguire l'override dell'impostazione per una richiesta specifica in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ac9b8-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="ac9b8-156">Se si tenta di configurare il limite per una richiesta dopo che l'applicazione ha avviato la lettura della richiesta, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="ac9b8-157">Una proprietà `IsReadOnly` indica se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="ac9b8-158">Per informazioni sulle altre opzioni di HTTP.sys, vedere [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="ac9b8-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="ac9b8-159">Configurare gli URL e le porte su cui eseguire l'ascolto</span><span class="sxs-lookup"><span data-stu-id="ac9b8-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="ac9b8-160">Per impostazione predefinita ASP.NET Core è associato a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="ac9b8-161">Per configurare i prefissi URL e le porte, è possibile usare il metodo di estensione `UseUrls`, l'argomento della riga di comando `urls`, la variabile di ambiente ASPNETCORE_URLS o la proprietà `UrlPrefixes` in [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="ac9b8-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="ac9b8-162">L'esempio di codice seguente usa `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="ac9b8-163">Un vantaggio offerto da `UrlPrefixes` è la ricezione immediata di un messaggio di errore se si tenta di aggiungere un prefisso con formato errato.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that's formatted wrong.</span></span> <span data-ttu-id="ac9b8-164">Un vantaggio offerto da `UseUrls` (condiviso con `urls` e ASPNETCORE_URLS) è la possibilità di passare più facilmente da Kestrel a HTTP.sys e viceversa.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="ac9b8-165">Se si usa sia `UseUrls` (oppure `urls` o ASPNETCORE_URLS) che `UrlPrefixes`, le impostazioni in `UrlPrefixes` eseguono l'override delle impostazioni in `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="ac9b8-166">Per altre informazioni, vedere [Hosting](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="ac9b8-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="ac9b8-167">HTTP.sys usa i [formati di stringa UrlPrefix dell'API del server HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac9b8-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="ac9b8-168">Assicurarsi di specificare le stesse stringhe di prefisso in `UseUrls` o `UrlPrefixes` che vengono pre-registrate nel server.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="ac9b8-169">Non usare IIS</span><span class="sxs-lookup"><span data-stu-id="ac9b8-169">Don't use IIS</span></span>

<span data-ttu-id="ac9b8-170">Verificare che l'applicazione non sia configurata per l'esecuzione di IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="ac9b8-171">In Visual Studio il profilo di avvio predefinito è per IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="ac9b8-172">Per eseguire il progetto come applicazione console, modificare manualmente il profilo selezionato, come illustrato nello screenshot seguente.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Selezionare il profilo dell'applicazione console](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="ac9b8-174">Pre-registrare i prefissi URL e configurare SSL</span><span class="sxs-lookup"><span data-stu-id="ac9b8-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="ac9b8-175">Sia IIS sia HTTP.sys si basano sul driver in modalità kernel Http. sys sottostante per l'ascolto delle richieste e l'elaborazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="ac9b8-176">In IIS l'interfaccia utente di gestione consente di configurare tutti gli elementi in modo relativamente semplice.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="ac9b8-177">Http.Sys deve essere tuttavia configurato manualmente.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="ac9b8-178">Lo strumento predefinito per questo scopo è *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-178">The built-in tool for doing that's *netsh.exe*.</span></span> 

<span data-ttu-id="ac9b8-179">Con *netsh.exe* è possibile riservare i prefissi URL e assegnare i certificati SSL.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="ac9b8-180">Per usare lo strumento sono necessari i privilegi amministrativi.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="ac9b8-181">L'esempio seguente illustra i prefissi URL minimi da riservare per le porte 80 e 443:</span><span class="sxs-lookup"><span data-stu-id="ac9b8-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="ac9b8-182">L'esempio seguente illustra come assegnare un certificato SSL:</span><span class="sxs-lookup"><span data-stu-id="ac9b8-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="ac9b8-183">La documentazione di riferimento per *netsh.exe* è la seguente:</span><span class="sxs-lookup"><span data-stu-id="ac9b8-183">Here is the reference documentation for *netsh.exe*:</span></span>

* <span data-ttu-id="ac9b8-184">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (Comandi di Netsh per il protocollo HTTP)</span><span class="sxs-lookup"><span data-stu-id="ac9b8-184">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)</span></span>
* <span data-ttu-id="ac9b8-185">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (Stringhe UrlPrefix)</span><span class="sxs-lookup"><span data-stu-id="ac9b8-185">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)</span></span>

<span data-ttu-id="ac9b8-186">Le risorse seguenti offrono istruzioni dettagliate per scenari diversi.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="ac9b8-187">Gli articoli che fanno riferimento a HttpListener si applicano anche a HTTP.sys poiché sono entrambi basati su Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="ac9b8-188">Procedura: Configurare una porta con un certificato SSL</span><span class="sxs-lookup"><span data-stu-id="ac9b8-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="ac9b8-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicazione HTTPS - Hosting e certificazione client basati su HttpListener) Blog di terze parti non recente ma che contiene informazioni utili.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="ac9b8-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (Procedura: Procedura dettagliata per l'uso di HttpListener o di codice non gestito del server HTTP (C++) come SSL Simple Server) Blog non recente contenente informazioni utili.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="ac9b8-191">Di seguito sono elencati alcuni strumenti di terze parti che possono risultare più semplici da usare rispetto alla riga di comando *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="ac9b8-192">Questi strumenti non sono forniti o promossi da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="ac9b8-193">Gli strumenti vengono eseguiti come amministratore per impostazione predefinita, poiché anche *netsh.exe* richiede i privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="ac9b8-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) offre l'interfaccia utente per visualizzare e configurare i certificati e le opzioni SSL, le prenotazioni dei prefissi e gli elenchi scopi consentiti ai certificati.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="ac9b8-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) consente di visualizzare o configurare i certificati SSL e i prefissi URL.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="ac9b8-196">L'interfaccia utente è più avanzata di http.sys Manager ed espone un numero maggiore di opzioni di configurazione ma offre comunque una funzionalità simile.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="ac9b8-197">Non consente di creare un nuovo elenco scopi consentiti ai certificati (CTL), ma consente di assegnare elenchi esistenti.</span><span class="sxs-lookup"><span data-stu-id="ac9b8-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="ac9b8-198">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="ac9b8-198">Next steps</span></span>

<span data-ttu-id="ac9b8-199">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="ac9b8-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="ac9b8-200">App di esempio per questo articolo</span><span class="sxs-lookup"><span data-stu-id="ac9b8-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="ac9b8-201">Codice sorgente di HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ac9b8-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="ac9b8-202">Hosting</span><span class="sxs-lookup"><span data-stu-id="ac9b8-202">Hosting</span></span>](xref:fundamentals/hosting)
